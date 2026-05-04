# PulseField™

[![PulseField™](https://repository-images.githubusercontent.com/1141499400/0d3ae64a-3add-467b-9471-bc1a8ddea02b)](https://github.com/ghostproxies/pulsefield)

## Table of Contents

- [Introduction](README.md?tab=readme-ov-file#introduction)
- [Author](README.md?tab=readme-ov-file#author)
- [License](README.md?tab=readme-ov-file#license)
- [Reference](README.md?tab=readme-ov-file#reference)

## Introduction

PulseField™ is the hyper-efficient divider of parallel PRNG sequences.

In further detail, PulseField™ enables non-colliding parallel PRNG states (without unnecessary jump functions) for hyper-efficient PRNGs (such as [BlastCircuit™](https://github.com/ghostproxies/blastcircuit) and [RecoilFuse™](https://github.com/ghostproxies/recoilfuse)).

## Author

PulseField™ was created by [William Stafford Parsons](https://github.com/williamstaffordparsons) as a product of [GhostProxies](https://ghostproxies.com).

## License

PulseField™ is licensed with the [BSD-3-Clause](LICENSE) license.

## Reference

The following algorithm demonstrates an iterative PulseField™ procedure in C.

``` c
a = ROTATE_LEFT(a, CIRCULAR_SHIFT) ^ b;
b += ODD_INCREASE;
```

Both `a` and `b` must have a matching bits length of `BITS_LENGTH` (or `ⁿ`) as a power of 2 that's a supported data type length greater than or equal to `8`.

`b` must wrap around `BITS_LENGTH` bits.

Each instance within a set of parallel PRNG instances that use PulseField™ must adhere to the following 4 seeding requirements before generating parallel PRNG sequences.

- `CIRCULAR_SHIFT` must be an integer (greater than `0` and less than `BITS_LENGTH`) that's consistent among the set of parallel instances.
- `ODD_INCREASE` must be an odd-numbered integer (greater than `0` and less than `2ⁿ`) that's consistent among the set of parallel instances.
- `a` must be an integer (greater than or equal to `0` and less than or equal to `2ⁿ`) that's unique among the set of parallel instances.
- `b` must be an integer (greater than or equal to `0` and less than or equal to `2ⁿ`) that's consistent among the set of parallel instances.

PulseField™ can generate up to 2ⁿ parallel instances that each avoid full state collisions (for at least 2ⁿ output results) among the set of parallel instances.

For example, the following PulseField™ seed values are valid for a set of 3 parallel PRNG instances.

```
a: 0
b: 0

a: 1
b: 0

a: 2
b: 0
```

After seeding a set of parallel PRNG instances that use PulseField™, assigning a value (outside of an iterative PulseField™ procedure) to `a`, `b`, `CIRCULAR_SHIFT` or `ODD_INCREASE` may void the aforementioned parallelism properties.

After each iterative PulseField™ procedure, `a` must be mixed into additional state (excluding `b`) that affects the PRNG output (for example, [BlastCircuit™](https://github.com/ghostproxies/blastcircuit) mixes `a` with `c = ((c << 41) | (c >> 23)) + a`).

[pulsefield.c](pulsefield.c) is a PulseField™ validation (when `BITS_LENGTH` is `8` and the `stdint.h` header defines an 8-bit, unsigned integral type for `uint8_t`).
