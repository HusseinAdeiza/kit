---
'@solana/codecs-numbers': patch
---

Fix `NaN` silently passing the range check in the integer codecs. `assertNumberIsBetweenForCodec` used `value < min || value > max`, and both comparisons are false for `NaN`, so encoding `NaN` was treated as in-range and wrote zero bytes (`getU8Codec().encode(NaN)` produced `00`, `getU32Codec().encode(NaN)` produced `00000000`) instead of throwing `SOLANA_ERROR__CODECS__NUMBER_OUT_OF_RANGE`. The check is now negated, so `NaN` is rejected like any other out-of-range value. This affects `u8`, `u16`, `u32`, `u64`, `u128`, `u256`, `i8`, `i16`, `i32`, `i64`, `i128`, `i256` and `shortU16`. The `f32` and `f64` codecs are unaffected, since they carry no range.
