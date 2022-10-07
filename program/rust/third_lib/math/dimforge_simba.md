# [simba 抽象代数库](https://docs.rs/simba/latest/simba/)


一组数学相关的 Trait，便于使用基于 SIMD 的 AoSoA（Array of Struct of Array）存储模式。

## 1. Scalar

|trait|约束|说明|
|--|--|--|
|SubsetOf|Sized|子集|
|SupersetOf|Sized|超集|
|ClosedNeg|Sized + Neg|封闭 运算|
|ClosedAdd|Sized + Add + AddAssign|封闭 运算|
|ClosedSub|Sized + Sub + SubAssign|封闭 运算|
|ClosedMul|Sized + Mul + MulAssign|封闭 运算|
|ClosedDiv|Sized + Div + DivAssign|封闭 运算|
|Field|[Num](./num_traits.md) + SimdValue + ClosedNeg|数域|
|ComplexField|[FromPrimitive](./num_traits.md) + Field + SubsetOf + SupersetOf + Clone + Send + Sync + Any|复数域|
|`RealField`|ComplexField + PartialOrd + [Signed](./num_traits.md) + [RelativeEq](./approx.md) + [UlpsEq](./approx.md)|实数域|

## 2. SimdValue

|trait|约束|说明|
|--|--|--|
|SimdValue|Sized|
|SimdBool|Copy + 与 或 非|
|SimdComplexField|Num + Field + SubsetOf + SupersetOf + Clone + Send + Sync + Any|
|SimdPartialOrd|SimdValue||
|SimdSigned|SimdValue||
|SimdRealField|SimdComplexField + SimdPartialOrd + SimdSigned|
|PrimitiveSimdValue|Copy + SimdValue|