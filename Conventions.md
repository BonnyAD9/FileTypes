# Conventions

- `LE` stands for Little-Endian byte order
- `BE` stands for Big-Endian byte order

## Values
- exact string value is shown in doublequotes `"` and is in ASCII encoding, unless otherwise specified
- numbers are in the base 10
- base 16 numbers have the `0x` prefix
- base 2 numbers have the `0b` prefix
- base 16 and base 2 numbers separated by spaces are shown in the same byte order as in file
- `signed` specifies signed integer
- `unsigned` specifies unsigned integer
- `fixed a.b` specifies fixed point value where `a` is number of integer bits in 2s compliment and `b` is decimal value NOT in 2s compliment
- `a << b` specifies that `a` is shifted left by `b` bits
- `a >> b` specifies that `a` is shifted right by `b` bits
- `a | b` specifies flags with values `a` and `b`

## Fields
- Index is specified in bytes
- Index specifies the index relative to the section of the file
- Size is specified in bytes
- Size of `0` specifies that it is optional
- Size of `?` specifies that it is variable but not 0
- Values separated by dash `-` specify inclusive range
- Values separated by comma `,` specify possible options (only one of them is the actial size)
- Values separated by semicolon `;` apply all
- Something else than space directly folowed by questionmark `?` is assumed (unverified) information