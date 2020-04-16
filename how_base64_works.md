## how base64 encoding/decoding works

let s look at the example of a 7-byte hex string and its base64 encoding:

```
80 5e 96 e6 60 00 6b
gF6W5mAAaw==
```

step by step
```
7 bytes in hex:
80 5e 96 e6 60 00 6b

in binary:
10000000 01011110 10010110 11100110 01100000 00000000 01101011

base64 rearranges this into groups of 6 bits:
100000	000101	111010	010110	111001	100110	000000	000000	011010	11

which each can be read as decimals from 0 to 64, pointing at chars in the base64 set:
	https://en.wikipedia.org/wiki/Base64#Base64_table
100000	000101	111010	010110	111001	100110	000000	000000	011010	11  <== note the hanging two bits!
32	5	58	22	57	38	0	0	26
g	F	6	W	5	m	A	A	a

what do we do with the last two bits? 
the two "=" (padding chars) told us how to handle this
	https://en.wikipedia.org/wiki/Base64#Decoding_Base64_with_padding

so we pad those bits up to be:
110000
48
w

result:
gF6W5mAAaw

the process backwards then is:

- base64 Chars into dec, into binary groups of 6
- regroup as groups of 8 (= bytes)
- into hex
- done.
```
