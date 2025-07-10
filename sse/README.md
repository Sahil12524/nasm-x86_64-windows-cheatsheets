# SSE Instructions Reference - x64 Windows ABI

A comprehensive reference for SSE (Streaming SIMD Extensions) instructions targeting x64 Windows ABI. This repository covers all major SSE instruction variants from SSE1 through SSE4.2, including syntax, operands, and usage examples.

## Table of Contents

- [SSE Instructions Reference - x64 Windows ABI](#sse-instructions-reference---x64-windows-abi)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [1⃣ XMM Registers](#1⃣-xmm-registers)
  - [2⃣ Data Movement Instructions](#2⃣-data-movement-instructions)
  - [3⃣ Arithmetic Instructions](#3⃣-arithmetic-instructions)
  - [4⃣ Comparison Instructions](#4⃣-comparison-instructions)
  - [5⃣ Logical Instructions](#5⃣-logical-instructions)
  - [6⃣ Integer Comparison Instructions](#6⃣-integer-comparison-instructions)
  - [7⃣ Shuffle and Unpack Instructions](#7⃣-shuffle-and-unpack-instructions)
  - [8⃣ Conversion Instructions](#8⃣-conversion-instructions)
  - [9⃣ Control and State Instructions](#9⃣-control-and-state-instructions)
  - [🔟 Masked Move Instructions](#-masked-move-instructions)
  - [1⃣1⃣ Extract and Insert Instructions](#1⃣1⃣-extract-and-insert-instructions)
  - [1⃣2⃣ Test Instructions](#1⃣2⃣-test-instructions)
  - [1⃣3⃣ Rounding Instructions](#1⃣3⃣-rounding-instructions)
  - [1⃣4⃣ String Comparison Instructions](#1⃣4⃣-string-comparison-instructions)
  - [1⃣5⃣ Bit Manipulation Instructions](#1⃣5⃣-bit-manipulation-instructions)
  - [1⃣6⃣ SSSE3 Instructions](#1⃣6⃣-ssse3-instructions)
  - [Building and Testing](#building-and-testing)
    - [Prerequisites](#prerequisites)
    - [Assembly Example](#assembly-example)
    - [Operand Types Reference](#operand-types-reference)
    - [Common Usage Examples](#common-usage-examples)
    - [Comparison Predicates for CMP Instructions](#comparison-predicates-for-cmp-instructions)
    - [Rounding Control for ROUND Instructions](#rounding-control-for-round-instructions)

## Overview

This repository provides detailed documentation and examples for SSE instructions under the x64 Windows ABI calling convention. All instructions are documented with instruction syntax, operand types, register constraints, and functional descriptions.

## 1⃣ XMM Registers

| Register | Purpose                                    | Preserved? | Size   |
| -------- | ------------------------------------------ | ---------- | ------ |
| XMM0     | Parameter 1 / Return value                 | No         | 128-bit |
| XMM1     | Parameter 2                                | No         | 128-bit |
| XMM2     | Parameter 3                                | No         | 128-bit |
| XMM3     | Parameter 4                                | No         | 128-bit |
| XMM4     | Parameter 5                                | No         | 128-bit |
| XMM5     | Parameter 6                                | No         | 128-bit |
| XMM6     | Volatile / General purpose                 | No         | 128-bit |
| XMM7     | Volatile / General purpose                 | No         | 128-bit |
| XMM8     | Volatile / General purpose                 | No         | 128-bit |
| XMM9     | Volatile / General purpose                 | No         | 128-bit |
| XMM10    | Volatile / General purpose                 | No         | 128-bit |
| XMM11    | Volatile / General purpose                 | No         | 128-bit |
| XMM12    | Volatile / General purpose                 | No         | 128-bit |
| XMM13    | Volatile / General purpose                 | No         | 128-bit |
| XMM14    | Volatile / General purpose                 | No         | 128-bit |
| XMM15    | Volatile / General purpose                 | No         | 128-bit |

## 2⃣ Data Movement Instructions

| Instruction | Syntax                    | Description                                    | SSE Version |
| ----------- | ------------------------- | ---------------------------------------------- | ----------- |
| MOVAPS      | `movaps xmm, xmm/m128`    | Move Aligned Packed Single-Precision          | SSE1        |
| MOVUPS      | `movups xmm, xmm/m128`    | Move Unaligned Packed Single-Precision        | SSE1        |
| MOVSS       | `movss xmm, xmm/m32`      | Move Scalar Single-Precision                   | SSE1        |
| MOVSD       | `movsd xmm, xmm/m64`      | Move Scalar Double-Precision                   | SSE2        |
| MOVLPS      | `movlps xmm, m64`         | Move Low Packed Single-Precision               | SSE1        |
| MOVHPS      | `movhps xmm, m64`         | Move High Packed Single-Precision              | SSE1        |
| MOVLHPS     | `movlhps xmm, xmm`        | Move Low to High Packed Single-Precision       | SSE1        |
| MOVHLPS     | `movhlps xmm, xmm`        | Move High to Low Packed Single-Precision       | SSE1        |
| MOVMSKPS    | `movmskps r32, xmm`       | Extract Packed Single-Precision Sign Mask      | SSE1        |
| MOVNTPS     | `movntps m128, xmm`       | Store Packed Single-Precision Non-Temporal     | SSE1        |
| MOVNTQ      | `movntq m64, mm`          | Store Packed Integers Non-Temporal              | SSE1        |
| MOVNTDQ     | `movntdq m128, xmm`       | Store Double Quadword Non-Temporal              | SSE2        |
| MOVDQA      | `movdqa xmm, xmm/m128`    | Move Aligned Double Quadword                    | SSE2        |
| MOVDQU      | `movdqu xmm, xmm/m128`    | Move Unaligned Double Quadword                  | SSE2        |
| MOVSHDUP    | `movshdup xmm, xmm/m128`  | Move Packed Single-Precision High and Duplicate | SSE3        |
| MOVSLDUP    | `movsldup xmm, xmm/m128`  | Move Packed Single-Precision Low and Duplicate  | SSE3        |
| MOVDDUP     | `movddup xmm, xmm/m64`    | Move One Double-Precision and Duplicate         | SSE3        |
| LDDQU       | `lddqu xmm, m128`         | Load Unaligned Integer 128 Bits                | SSE3        |

## 3⃣ Arithmetic Instructions

| Instruction | Syntax                      | Description                                    | SSE Version |
| ----------- | --------------------------- | ---------------------------------------------- | ----------- |
| ADDPS       | `addps xmm, xmm/m128`       | Add Packed Single-Precision                    | SSE1        |
| ADDSS       | `addss xmm, xmm/m32`        | Add Scalar Single-Precision                    | SSE1        |
| SUBPS       | `subps xmm, xmm/m128`       | Subtract Packed Single-Precision               | SSE1        |
| SUBSS       | `subss xmm, xmm/m32`        | Subtract Scalar Single-Precision               | SSE1        |
| MULPS       | `mulps xmm, xmm/m128`       | Multiply Packed Single-Precision               | SSE1        |
| MULSS       | `mulss xmm, xmm/m32`        | Multiply Scalar Single-Precision               | SSE1        |
| DIVPS       | `divps xmm, xmm/m128`       | Divide Packed Single-Precision                 | SSE1        |
| DIVSS       | `divss xmm, xmm/m32`        | Divide Scalar Single-Precision                 | SSE1        |
| ADDPD       | `addpd xmm, xmm/m128`       | Add Packed Double-Precision                    | SSE2        |
| ADDSD       | `addsd xmm, xmm/m64`        | Add Scalar Double-Precision                    | SSE2        |
| SUBPD       | `subpd xmm, xmm/m128`       | Subtract Packed Double-Precision               | SSE2        |
| SUBSD       | `subsd xmm, xmm/m64`        | Subtract Scalar Double-Precision               | SSE2        |
| MULPD       | `mulpd xmm, xmm/m128`       | Multiply Packed Double-Precision               | SSE2        |
| MULSD       | `mulsd xmm, xmm/m64`        | Multiply Scalar Double-Precision               | SSE2        |
| DIVPD       | `divpd xmm, xmm/m128`       | Divide Packed Double-Precision                 | SSE2        |
| DIVSD       | `divsd xmm, xmm/m64`        | Divide Scalar Double-Precision                 | SSE2        |
| ADDSUBPD    | `addsubpd xmm, xmm/m128`    | Add/Subtract Packed Double-Precision           | SSE3        |
| ADDSUBPS    | `addsubps xmm, xmm/m128`    | Add/Subtract Packed Single-Precision           | SSE3        |
| SQRTPS      | `sqrtps xmm, xmm/m128`      | Square Root Packed Single-Precision            | SSE1        |
| SQRTSS      | `sqrtss xmm, xmm/m32`       | Square Root Scalar Single-Precision            | SSE1        |
| SQRTPD      | `sqrtpd xmm, xmm/m128`      | Square Root Packed Double-Precision            | SSE2        |
| SQRTSD      | `sqrtsd xmm, xmm/m64`       | Square Root Scalar Double-Precision            | SSE2        |
| RSQRTPS     | `rsqrtps xmm, xmm/m128`     | Reciprocal Square Root Packed Single-Precision | SSE1        |
| RSQRTSS     | `rsqrtss xmm, xmm/m32`      | Reciprocal Square Root Scalar Single-Precision | SSE1        |
| RCPPS       | `rcpps xmm, xmm/m128`       | Reciprocal Packed Single-Precision             | SSE1        |
| RCPSS       | `rcpss xmm, xmm/m32`        | Reciprocal Scalar Single-Precision             | SSE1        |
| MAXPS       | `maxps xmm, xmm/m128`       | Maximum Packed Single-Precision                | SSE1        |
| MAXSS       | `maxss xmm, xmm/m32`        | Maximum Scalar Single-Precision                | SSE1        |
| MINPS       | `minps xmm, xmm/m128`       | Minimum Packed Single-Precision                | SSE1        |
| MINSS       | `minss xmm, xmm/m32`        | Minimum Scalar Single-Precision                | SSE1        |
| MAXPD       | `maxpd xmm, xmm/m128`       | Maximum Packed Double-Precision                | SSE2        |
| MAXSD       | `maxsd xmm, xmm/m64`        | Maximum Scalar Double-Precision                | SSE2        |
| MINPD       | `minpd xmm, xmm/m128`       | Minimum Packed Double-Precision                | SSE2        |
| MINSD       | `minsd xmm, xmm/m64`        | Minimum Scalar Double-Precision                | SSE2        |
| HADDPS      | `haddps xmm, xmm/m128`      | Horizontal Add Packed Single-Precision         | SSE3        |
| HSUBPS      | `hsubps xmm, xmm/m128`      | Horizontal Subtract Packed Single-Precision    | SSE3        |
| HADDPD      | `haddpd xmm, xmm/m128`      | Horizontal Add Packed Double-Precision         | SSE3        |
| HSUBPD      | `hsubpd xmm, xmm/m128`      | Horizontal Subtract Packed Double-Precision    | SSE3        |
| DPPS        | `dpps xmm, xmm/m128, imm8`  | Dot Product Packed Single-Precision            | SSE4.1      |
| DPPD        | `dppd xmm, xmm/m128, imm8`  | Dot Product Packed Double-Precision            | SSE4.1      |

## 4⃣ Comparison Instructions

| Instruction | Syntax                      | Description                                  | SSE Version |
| ----------- | --------------------------- | -------------------------------------------- | ----------- |
| CMPPS       | `cmpps xmm, xmm/m128, imm8` | Compare Packed Single-Precision             | SSE1        |
| CMPSS       | `cmpss xmm, xmm/m32, imm8`  | Compare Scalar Single-Precision             | SSE1        |
| CMPPD       | `cmppd xmm, xmm/m128, imm8` | Compare Packed Double-Precision             | SSE2        |
| CMPSD       | `cmpsd xmm, xmm/m64, imm8`  | Compare Scalar Double-Precision             | SSE2        |
| COMISS      | `comiss xmm, xmm/m32`       | Compare Ordered Scalar Single-Precision     | SSE1        |
| COMISD      | `comisd xmm, xmm/m64`       | Compare Ordered Scalar Double-Precision     | SSE2        |
| UCOMISS     | `ucomiss xmm, xmm/m32`      | Compare Unordered Scalar Single-Precision   | SSE1        |
| UCOMISD     | `ucomisd xmm, xmm/m64`      | Compare Unordered Scalar Double-Precision   | SSE2        |

## 5⃣ Logical Instructions

| Instruction | Syntax                    | Description                                | SSE Version |
| ----------- | ------------------------- | ------------------------------------------ | ----------- |
| ANDPS       | `andps xmm, xmm/m128`     | Bitwise AND Packed Single-Precision       | SSE1        |
| ANDNPS      | `andnps xmm, xmm/m128`    | Bitwise AND NOT Packed Single-Precision   | SSE1        |
| ORPS        | `orps xmm, xmm/m128`      | Bitwise OR Packed Single-Precision        | SSE1        |
| XORPS       | `xorps xmm, xmm/m128`     | Bitwise XOR Packed Single-Precision       | SSE1        |
| ANDPD       | `andpd xmm, xmm/m128`     | Bitwise AND Packed Double-Precision       | SSE2        |
| ANDNPD      | `andnpd xmm, xmm/m128`    | Bitwise AND NOT Packed Double-Precision   | SSE2        |
| ORPD        | `orpd xmm, xmm/m128`      | Bitwise OR Packed Double-Precision        | SSE2        |
| XORPD       | `xorpd xmm, xmm/m128`     | Bitwise XOR Packed Double-Precision       | SSE2        |
| PAND        | `pand xmm, xmm/m128`      | Bitwise AND Packed Integers               | SSE2        |
| PANDN       | `pandn xmm, xmm/m128`     | Bitwise AND NOT Packed Integers           | SSE2        |
| POR         | `por xmm, xmm/m128`       | Bitwise OR Packed Integers                | SSE2        |
| PXOR        | `pxor xmm, xmm/m128`      | Bitwise XOR Packed Integers               | SSE2        |

## 6⃣ Integer Comparison Instructions

| Instruction | Syntax                    | Description                                     | SSE Version |
| ----------- | ------------------------- | ----------------------------------------------- | ----------- |
| PCMPEQB     | `pcmpeqb xmm, xmm/m128`   | Compare Packed Bytes for Equality              | SSE2        |
| PCMPEQW     | `pcmpeqw xmm, xmm/m128`   | Compare Packed Words for Equality              | SSE2        |
| PCMPEQD     | `pcmpeqd xmm, xmm/m128`   | Compare Packed Doublewords for Equality        | SSE2        |
| PCMPEQQ     | `pcmpeqq xmm, xmm/m128`   | Compare Packed Quadwords for Equality          | SSE4.1      |
| PCMPGTB     | `pcmpgtb xmm, xmm/m128`   | Compare Packed Signed Bytes for Greater Than   | SSE2        |
| PCMPGTW     | `pcmpgtw xmm, xmm/m128`   | Compare Packed Signed Words for Greater Than   | SSE2        |
| PCMPGTD     | `pcmpgtd xmm, xmm/m128`   | Compare Packed Signed Doublewords for Greater Than | SSE2        |
| PCMPGTQ     | `pcmpgtq xmm, xmm/m128`   | Compare Packed Signed Quadwords for Greater Than | SSE4.2      |

## 7⃣ Shuffle and Unpack Instructions

| Instruction | Syntax                      | Description                                | SSE Version |
| ----------- | --------------------------- | ------------------------------------------ | ----------- |
| SHUFPS      | `shufps xmm, xmm/m128, imm8` | Shuffle Packed Single-Precision          | SSE1        |
| SHUFPD      | `shufpd xmm, xmm/m128, imm8` | Shuffle Packed Double-Precision          | SSE2        |
| UNPCKHPS    | `unpckhps xmm, xmm/m128`    | Unpack High Packed Single-Precision       | SSE1        |
| UNPCKLPS    | `unpcklps xmm, xmm/m128`    | Unpack Low Packed Single-Precision        | SSE1        |
| UNPCKHPD    | `unpckhpd xmm, xmm/m128`    | Unpack High Packed Double-Precision       | SSE2        |
| UNPCKLPD    | `unpcklpd xmm, xmm/m128`    | Unpack Low Packed Double-Precision        | SSE2        |
| PSHUFD      | `pshufd xmm, xmm/m128, imm8` | Shuffle Packed Doublewords               | SSE2        |
| PSHUFHW     | `pshufhw xmm, xmm/m128, imm8` | Shuffle High Packed Words               | SSE2        |
| PSHUFLW     | `pshuflw xmm, xmm/m128, imm8` | Shuffle Low Packed Words                | SSE2        |
| PSHUFB      | `pshufb xmm, xmm/m128`      | Shuffle Packed Bytes                       | SSSE3       |
| PALIGNR     | `palignr xmm, xmm/m128, imm8` | Packed Align Right                       | SSSE3       |

## 8⃣ Conversion Instructions

| Instruction | Syntax                    | Description                                                      | SSE Version |
| ----------- | ------------------------- | ---------------------------------------------------------------- | ----------- |
| CVTPS2PI    | `cvtps2pi mm, xmm/m64`    | Convert Packed Single-Precision to Packed Integers              | SSE1        |
| CVTTPS2PI   | `cvttps2pi mm, xmm/m64`   | Convert with Truncation Packed Single-Precision to Packed Integers | SSE1        |
| CVTPI2PS    | `cvtpi2ps xmm, mm/m64`    | Convert Packed Integers to Packed Single-Precision              | SSE1        |
| CVTSI2SS    | `cvtsi2ss xmm, r32/m32`   | Convert Scalar Integer to Scalar Single-Precision               | SSE1        |
| CVTSS2SI    | `cvtss2si r32, xmm/m32`   | Convert Scalar Single-Precision to Scalar Integer               | SSE1        |
| CVTTSS2SI   | `cvttss2si r32, xmm/m32`  | Convert with Truncation Scalar Single-Precision to Scalar Integer | SSE1        |
| CVTPS2PD    | `cvtps2pd xmm, xmm/m64`   | Convert Packed Single-Precision to Packed Double-Precision      | SSE2        |
| CVTPD2PS    | `cvtpd2ps xmm, xmm/m128`  | Convert Packed Double-Precision to Packed Single-Precision      | SSE2        |
| CVTSD2SS    | `cvtsd2ss xmm, xmm/m64`   | Convert Scalar Double-Precision to Scalar Single-Precision      | SSE2        |
| CVTSS2SD    | `cvtss2sd xmm, xmm/m32`   | Convert Scalar Single-Precision to Scalar Double-Precision      | SSE2        |
| CVTSD2SI    | `cvtsd2si r32, xmm/m64`   | Convert Scalar Double-Precision to Scalar Integer               | SSE2        |
| CVTTSD2SI   | `cvttsd2si r32, xmm/m64`  | Convert with Truncation Scalar Double-Precision to Scalar Integer | SSE2        |
| CVTSI2SD    | `cvtsi2sd xmm, r32/m32`   | Convert Scalar Integer to Scalar Double-Precision               | SSE2        |
| CVTDQ2PS    | `cvtdq2ps xmm, xmm/m128`  | Convert Packed Doubleword Integers to Packed Single-Precision   | SSE2        |
| CVTPS2DQ    | `cvtps2dq xmm, xmm/m128`  | Convert Packed Single-Precision to Packed Doubleword Integers   | SSE2        |
| CVTTPS2DQ   | `cvttps2dq xmm, xmm/m128` | Convert with Truncation Packed Single-Precision to Packed Doubleword Integers | SSE2        |
| CVTDQ2PD    | `cvtdq2pd xmm, xmm/m64`   | Convert Packed Doubleword Integers to Packed Double-Precision   | SSE2        |
| CVTPD2DQ    | `cvtpd2dq xmm, xmm/m128`  | Convert Packed Double-Precision to Packed Doubleword Integers   | SSE2        |
| CVTTPD2DQ   | `cvttpd2dq xmm, xmm/m128` | Convert with Truncation Packed Double-Precision to Packed Doubleword Integers | SSE2        |

## 9⃣ Control and State Instructions

| Instruction | Syntax                | Description                     | SSE Version |
| ----------- | --------------------- | ------------------------------- | ----------- |
| LDMXCSR     | `ldmxcsr m32`         | Load MXCSR Register             | SSE1        |
| STMXCSR     | `stmxcsr m32`         | Store MXCSR Register            | SSE1        |
| CLFLUSH     | `clflush m8`          | Cache Line Flush                | SSE2        |
| LFENCE      | `lfence`              | Load Fence                      | SSE2        |
| MFENCE      | `mfence`              | Memory Fence                    | SSE2        |
| PAUSE       | `pause`               | Spin Loop Hint                  | SSE2        |

## 🔟 Masked Move Instructions

| Instruction | Syntax                    | Description                           | SSE Version |
| ----------- | ------------------------- | ------------------------------------- | ----------- |
| MASKMOVQ    | `maskmovq mm, mm`         | Store Selected Bytes of Quadword      | SSE1        |
| MASKMOVDQU  | `maskmovdqu xmm, xmm`     | Store Selected Bytes of Double Quadword | SSE2        |

## 1⃣1⃣ Extract and Insert Instructions

| Instruction | Syntax                        | Description                  | SSE Version |
| ----------- | ----------------------------- | ---------------------------- | ----------- |
| PEXTRW      | `pextrw r32, mm/xmm, imm8`   | Extract Word                 | SSE1/SSE2   |
| PINSRW      | `pinsrw mm/xmm, r32/m16, imm8` | Insert Word                | SSE1/SSE2   |
| PEXTRD      | `pextrd r32/m32, xmm, imm8`  | Extract Doubleword           | SSE4.1      |
| PEXTRQ      | `pextrq r64/m64, xmm, imm8`  | Extract Quadword             | SSE4.1      |
| PINSRD      | `pinsrd xmm, r32/m32, imm8`  | Insert Doubleword            | SSE4.1      |
| PINSRQ      | `pinsrq xmm, r64/m64, imm8`  | Insert Quadword              | SSE4.1      |

## 1⃣2⃣ Test Instructions

| Instruction | Syntax                    | Description      | SSE Version |
| ----------- | ------------------------- | ---------------- | ----------- |
| PTEST       | `ptest xmm, xmm/m128`     | Logical Compare  | SSE4.1      |

## 1⃣3⃣ Rounding Instructions

| Instruction | Syntax                          | Description                            | SSE Version |
| ----------- | ------------------------------- | -------------------------------------- | ----------- |
| ROUNDPS     | `roundps xmm, xmm/m128, imm8`   | Round Packed Single-Precision Values  | SSE4.1      |
| ROUNDPD     | `roundpd xmm, xmm/m128, imm8`   | Round Packed Double-Precision Values  | SSE4.1      |
| ROUNDSS     | `roundss xmm, xmm/m32, imm8`    | Round Scalar Single-Precision Values  | SSE4.1      |
| ROUNDSD     | `roundsd xmm, xmm/m64, imm8`    | Round Scalar Double-Precision Values  | SSE4.1      |

## 1⃣4⃣ String Comparison Instructions

| Instruction | Syntax                          | Description                                      | SSE Version |
| ----------- | ------------------------------- | ------------------------------------------------ | ----------- |
| PCMPESTRI   | `pcmpestri xmm, xmm/m128, imm8` | Packed Compare Explicit Length Strings, Return Index | SSE4.2      |
| PCMPESTRM   | `pcmpestrm xmm, xmm/m128, imm8` | Packed Compare Explicit Length Strings, Return Mask | SSE4.2      |
| PCMPISTRI   | `pcmpistri xmm, xmm/m128, imm8` | Packed Compare Implicit Length Strings, Return Index | SSE4.2      |
| PCMPISTRM   | `pcmpistrm xmm, xmm/m128, imm8` | Packed Compare Implicit Length Strings, Return Mask | SSE4.2      |

## 1⃣5⃣ Bit Manipulation Instructions

| Instruction | Syntax                    | Description                           | SSE Version |
| ----------- | ------------------------- | ------------------------------------- | ----------- |
| CRC32       | `crc32 r32, r8/r16/r32`   | Accumulate CRC32 Value                | SSE4.2      |
| POPCNT      | `popcnt r16/r32/r64, r/m` | Return the Count of Number of Bits Set to 1 | SSE4.2      |

## 1⃣6⃣ SSSE3 Instructions

| Instruction | Syntax                    | Description                                          | SSE Version |
| ----------- | ------------------------- | ---------------------------------------------------- | ----------- |
| PHADDW      | `phaddw xmm, xmm/m128`    | Packed Horizontal Add Words                          | SSSE3       |
| PHADDD      | `phaddd xmm, xmm/m128`    | Packed Horizontal Add Doublewords                    | SSSE3       |
| PHADDSW     | `phaddsw xmm, xmm/m128`   | Packed Horizontal Add and Saturate Words             | SSSE3       |
| PHSUBW      | `phsubw xmm, xmm/m128`    | Packed Horizontal Subtract Words                     | SSSE3       |
| PHSUBD      | `phsubd xmm, xmm/m128`    | Packed Horizontal Subtract Doublewords              | SSSE3       |
| PHSUBSW     | `phsubsw xmm, xmm/m128`   | Packed Horizontal Subtract and Saturate Words       | SSSE3       |
| PMADDUBSW   | `pmaddubsw xmm, xmm/m128` | Multiply and Add Packed Signed and Unsigned Bytes   | SSSE3       |
| PMULHRSW    | `pmulhrsw xmm, xmm/m128`  | Packed Multiply High with Round and Scale           | SSSE3       |
| PSIGNB      | `psignb xmm, xmm/m128`    | Packed Sign Bytes                                    | SSSE3       |
| PSIGNW      | `psignw xmm, xmm/m128`    | Packed Sign Words                                    | SSSE3       |
| PSIGND      | `psignd xmm, xmm/m128`    | Packed Sign Doublewords                              | SSSE3       |
| PABSB       | `pabsb xmm, xmm/m128`     | Packed Absolute Value Bytes                          | SSSE3       |
| PABSW       | `pabsw xmm, xmm/m128`     | Packed Absolute Value Words                          | SSSE3       |
| PABSD       | `pabsd xmm, xmm/m128`     | Packed Absolute Value Doublewords                    | SSSE3       |

## Building and Testing

### Prerequisites
- MASM64 or NASM (for assembly)
- Visual Studio or Windows SDK (for x64 development)
- Windows 10/11 x64

### Assembly Example
```asm
.code
example_sse_function PROC
    ; Function prologue
    sub rsp, 28h                ; Shadow space + alignment
    
    ; SSE operations
    movaps xmm0, xmm1           ; Move data
    addps xmm0, xmm2            ; Add packed single-precision
    mulps xmm0, xmm3            ; Multiply packed single-precision
    
    ; Function epilogue
    add rsp, 28h
    ret
example_sse_function ENDP
END
```

### Operand Types Reference

| Operand | Description                    | Example        |
| ------- | ------------------------------ | -------------- |
| xmm     | XMM register (xmm0-xmm15)      | `xmm0, xmm1`   |
| m128    | 128-bit memory operand         | `[rax]`        |
| m64     | 64-bit memory operand          | `[rbx+8]`      |
| m32     | 32-bit memory operand          | `[rcx+4]`      |
| r32     | 32-bit general-purpose register | `eax, ebx`     |
| r64     | 64-bit general-purpose register | `rax, rbx`     |
| imm8    | 8-bit immediate value          | `0x1B, 255`    |

### Common Usage Examples

```asm
; Data Movement
movaps xmm0, xmm1               ; Move aligned packed single-precision
movups xmm0, [rbx]              ; Move unaligned packed single-precision
movss xmm0, [rax+4]             ; Move scalar single-precision

; Arithmetic
addps xmm0, xmm1                ; Add packed single-precision
mulss xmm0, xmm1                ; Multiply scalar single-precision
sqrtpd xmm0, xmm1               ; Square root packed double-precision

; Comparison
cmpps xmm0, xmm1, 0             ; Compare packed single-precision (equal)
comiss xmm0, xmm1               ; Compare ordered scalar single-precision

; Logical
andps xmm0, xmm1                ; Bitwise AND packed single-precision
xorpd xmm0, xmm0                ; Zero out XMM register

; Shuffle
shufps xmm0, xmm1, 0x1B         ; Shuffle packed single-precision
pshufd xmm0, xmm1, 0x4E         ; Shuffle packed doublewords

; Conversion
cvtps2pd xmm0, xmm1             ; Convert packed single to double precision
cvtsi2ss xmm0, eax              ; Convert scalar integer to single precision
```

### Comparison Predicates for CMP Instructions

| Predicate | Value | Description                    |
| --------- | ----- | ------------------------------ |
| EQ        | 0     | Equal                          |
| LT        | 1     | Less Than                      |
| LE        | 2     | Less Than or Equal             |
| UNORD     | 3     | Unordered                      |
| NE        | 4     | Not Equal                      |
| NLT       | 5     | Not Less Than                  |
| NLE       | 6     | Not Less Than or Equal         |
| ORD       | 7     | Ordered                        |

### Rounding Control for ROUND Instructions

| Mode | Value | Description                    |
| ---- | ----- | ------------------------------ |
| RN   | 0     | Round to Nearest (Even)        |
| RD   | 1     | Round Down (toward -∞)         |
| RU   | 2     | Round Up (toward +∞)           |
| RZ   | 3     | Round toward Zero (Truncate)   |
