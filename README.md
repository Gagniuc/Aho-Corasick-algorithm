# Aho-Corasick algorithm

In this example, single-letter class and function names are used for brevity. The class T represents nodes in the trie structure built by the algorithm. Each node contains the transitions to child nodes (c), the output patterns associated with the node (o), and the failure state pointer (f) for traversal. The function b constructs the trie structure from a list of patterns, iteratively adding each pattern character by character. The function f builds the failure transitions for nodes, assigning the correct failure links across the trie. The main algorithmic work of searching for matching patterns is performed by the function a, which scans the input text using the trie and returns a list of matched patterns along with their positions. In the provided example, a list of patterns p = ["s1", "s2", "s3", "s4"] is defined, and the text t = "IAs3mAfile" is searched. The trie is built with b, failure transitions are established with f, and pattern searching is accomplished with a. Matched patterns and their positions in the input text are then printed, showcasing the efficient multi-pattern string matching capability of the Aho-Corasick algorithm implemented in a highly condensed form.

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
