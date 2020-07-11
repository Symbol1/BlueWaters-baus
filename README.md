# Blue Waters computing ReedMullerCode(2,6).TuttePolynomial()

**[Blue Waters]** is a petascale supercomputer
at the National Center for Supercomputing Applications (NCSA)
at the University of Illinois at Urbana--Champaign (UIUC).
It was the fastest supercomputer at a university anywhere in the world
until *TACC Frontera* came online in 2019.

The [64, 22, 16]-*[Reed-Muller code]*,
also known as RM(2, 6), is the linear code over GF(2) generated by this matrix expressed in python syntax:

```python
[
  [ c&r == 0 for c in range(64) ]
             for r in range(64) if bin(r).count('1') < 3
]
```

More precisely, by this matrix

```text
1111111111111111111111111111111111111111111111111111111111111111
1010101010101010101010101010101010101010101010101010101010101010
1100110011001100110011001100110011001100110011001100110011001100
1000100010001000100010001000100010001000100010001000100010001000
1111000011110000111100001111000011110000111100001111000011110000
1010000010100000101000001010000010100000101000001010000010100000
1100000011000000110000001100000011000000110000001100000011000000
1111111100000000111111110000000011111111000000001111111100000000
1010101000000000101010100000000010101010000000001010101000000000
1100110000000000110011000000000011001100000000001100110000000000
1111000000000000111100000000000011110000000000001111000000000000
1111111111111111000000000000000011111111111111110000000000000000
1010101010101010000000000000000010101010101010100000000000000000
1100110011001100000000000000000011001100110011000000000000000000
1111000011110000000000000000000011110000111100000000000000000000
1111111100000000000000000000000011111111000000000000000000000000
1111111111111111111111111111111100000000000000000000000000000000
1010101010101010101010101010101000000000000000000000000000000000
1100110011001100110011001100110000000000000000000000000000000000
1111000011110000111100001111000000000000000000000000000000000000
1111111100000000111111110000000000000000000000000000000000000000
1111111111111111000000000000000000000000000000000000000000000000
```

We compute its *[Tutte--Whitney polynomial]*
and more general invariants on Blue Waters.
See [the resulting polynomial](rm64tutte.txt) in matrix form.

The full computation costs 2,000 node-hours.
We deliberately divide the computation into 48,520 subjobs so that
anyone can challenge our computation by rerunning only a subset of subjobs.
This makes our computation (economically) **[falsifiable]**.

The full data set is uploaded to my
[Google Drive .edu account].
See [how to challenge the computation](challenge.md) for more information.

[Blue Waters]: https://en.wikipedia.org/wiki/Blue_Waters
[Reed-Muller code]: https://en.wikipedia.org/wiki/Reed%E2%80%93Muller_code
[Tutte--Whitney polynomial]: https://en.wikipedia.org/wiki/Tutte_polynomial
[falsifiable]: https://en.wikipedia.org/wiki/Falsifiability
[Google Drive .edu account]: https://drive.google.com/drive/folders/1zYv2R-oqepX1vJ_Fr5JBmrVNdle0mi9M
