# Aho-Corasick algorithm

Ex. (19) – <strong>Aho–Corasick</strong> multi-pattern scan appears here in Python. Although the syntax is Python-specific, the core idea mirrors the usual form of the algorithm. This code sample is one entry in a suite of 127 algorithms set out in <i>[Antivirus Engines: From Methods to Innovations, Design, and Applications](https://github.com/Gagniuc/Antivirus-Engines)</i> (Elsevier Syngress, 2024).

***

The snippet below presents a <i>compact</i> and <i>bare-bones</i> version of the <strong>Aho–Corasick</strong> algorithm for multi-pattern search written in pure Python. Note that <code>T</code> defines a trie node: each node holds a dict of child edges (<code>c</code>) and a list of patterns that end at that node (<code>o</code>). The builder <code>b(p)</code> walks through every pattern in <code>p</code>, inserts each character into the trie, then appends the full pattern to the output list at the final node. Function <code>f(r)</code> augments the trie, rooted at <code>r</code>, with failure links. A breadth-first pass sets for every node an <code>f</code> pointer to the longest proper suffix that is also a valid prefix in the trie. These links allow an <i>O(1)</i> amortized fallback when the next text char lacks a direct edge. Function <code>a(t, r)</code> runs the search on text <code>t</code>. The routine reads chars from left to right, follows edges when possible, else hops on failure links until it reaches the root or a valid edge. Each time the active node has output items, the start index of every matched pattern is stored. <ul> <li><b>Build phase</b>: <i>O(Σ |p|)</i> time and space</li> <li><b>Search phase</b>: <i>O(|t| + k)</i>, where <i>k</i> denotes the count of reported hits</li> </ul>
The <code>main</code> demo supplies the patterns <code>"s1"</code>, <code>"s2"</code>, <code>"s3"</code>, <code>"s4"</code> and reveals <code>"s3"</code> at the correct offset inside <code>"IAs3mAfile"</code>, which confirms the proper result of this minimalist approach.

## Native implementation in Python

```python
class T:
    def __init__(s):
        s.c = {}
        s.o = []

def b(p):
    r = T()
    for x in p:
        n = r
        for y in x:
            if y not in n.c:
                n.c[y] = T()
            n = n.c[y]
        n.o.append(x)
    return r

def a(t, r):
    z = []
    n = r
    for i, y in enumerate(t):
        while y not in n.c and n != r:
            n = n.f
        if y in n.c:
            n = n.c[y]
            for x in n.o:
                z.append((i - len(x) + 1, x))
    return z

def f(r):
    q = []
    for n in r.c.values():
        n.f = r
        q.append(n)
    while q:
        m = q.pop(0)
        for y, k in m.c.items():
            q.append(k)
            s = m.f
            while y not in s.c and s != r:
                s = s.f
            k.f = s.c.get(y, r)

if __name__ == "__main__":
    p = ["s1", "s2", "s3", "s4"]
    t = "IAs3mAfile"

    r = b(p)
    f(r)
    z = a(t, r)

    if z:
        print("Found patterns:")
        for i, x in z:
            print(f"Pattern '{x}' found at position {i}")
    else:
        print("No patterns found.")
``` 

```text
Output:
Found patterns:
Pattern 's3' found at position 2
```

## References

- <i>Paul A. Gagniuc. Antivirus Engines: From Methods to Innovations, Design, and Applications. Cambridge, MA: Elsevier Syngress, 2024. pp. 1-656.</i>

***
