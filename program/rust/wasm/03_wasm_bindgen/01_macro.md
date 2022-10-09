- [宏展开：[wasm-bindgen]](#宏展开wasm-bindgen)
  - [1. 不用 [wasm-bindgen] 宏，直接用工具链构建](#1-不用-wasm-bindgen-宏直接用工具链构建)
  - [2. [wasm-bindgen] 初步](#2-wasm-bindgen-初步)
    - [2.1. 用 cargo-expand 命令查看](#21-用-cargo-expand-命令查看)
    - [2.2. 如果是切片](#22-如果是切片)
    - [2.3. 宏展开后](#23-宏展开后)
    - [2.4. 底层 对 返回值 的 设置](#24-底层-对-返回值-的-设置)
    - [2.5. js 代码](#25-js-代码)
    - [2.6. 分配参数](#26-分配参数)
    - [2.7. 返回值获取](#27-返回值获取)
  - [3. 导入 符号](#3-导入-符号)
  - [4. js-sys 和 web-sys](#4-js-sys-和-web-sys)
  - [5. 疑问](#5-疑问)
  - [6. 参考](#6-参考)

# 宏展开：[wasm-bindgen]

## 1. 不用 [wasm-bindgen] 宏，直接用工具链构建

+ Rust 会生成 wasm 代码，下面函数会导出成：_add_i32
+ 不会生成js封装代码
+ 对参数和返回值不会做任何转换
	- 默认只接受：i32，i64，f32，f64

``` rs

#[no_mangle]
pub fn add_i32(a: i32, b: i32) -> i32 {
	a + b
}

```

## 2. [wasm-bindgen] 初步

``` rs

#[wasm_bindgen]
pub fn add_i32(a: i32, b: i32) -> i32 {
	a + b
}

```

### 2.1. 用 cargo-expand 命令查看

+ wasm导出的add_i32，对应wasm-bindgen生成的代码：__wasm_bindgen_generated_add_i32
+ 重点是，每个参数做一次 argi = wasm_bindgen::convert::FromWasmAbi>::from_abi(argi)
+ 返回值做一次：wasm_bindgen::convert::ReturnWasmAbi>::return_abi(_ret)

``` rs

pub fn add_i32(a: i32, b: i32) -> i32 {
    a + b
}

#[allow(non_snake_case)]
#[export_name = "add_i32"]
#[allow(clippy::all)]
pub extern "C" fn __wasm_bindgen_generated_add_i32(
    arg0: <i32 as wasm_bindgen::convert::FromWasmAbi>::Abi,
    arg1: <i32 as wasm_bindgen::convert::FromWasmAbi>::Abi,
) -> <i32 as wasm_bindgen::convert::ReturnWasmAbi>::Abi {
    let _ret = {
        let arg0 = unsafe { <i32 as wasm_bindgen::convert::FromWasmAbi>::from_abi(arg0) };
        let arg1 = unsafe { <i32 as wasm_bindgen::convert::FromWasmAbi>::from_abi(arg1) };
        add_i32(arg0, arg1)
    };
    <i32 as wasm_bindgen::convert::ReturnWasmAbi>::return_abi(_ret)
}

```

### 2.2. 如果是切片

``` rs
#[wasm_bindgen]
pub fn sub_slice(a: &[u8], b: &[u8]) -> Uint8Array {
	// ...

	Uint8Array::from(r.as_slice())
}
```

### 2.3. 宏展开后

+ arg0: 对应js的两个参数：ptr0, len0
	- ptr0指向wasm堆；
+ arg1: 对应js的两个参数：ptr1, len1
	- ptr1指向wasm堆；
+ return_abi 回调用 导入 对象，分配js堆，将wasm堆的数据拷贝过去
+ 下面函数结束后，会释放掉wasm堆的对象 _ret，释放 ptr0，ptr1代表的slice；

``` rs

#[allow(non_snake_case)]
#[export_name = "sub_slice"]
#[allow(clippy::all)]
pub extern "C" fn __wasm_bindgen_generated_sub_slice(
    arg0: <[u8] as wasm_bindgen::convert::RefFromWasmAbi>::Abi,
    arg1: <[u8] as wasm_bindgen::convert::RefFromWasmAbi>::Abi,
) -> <Uint8Array as wasm_bindgen::convert::ReturnWasmAbi>::Abi {
    let _ret = {
        let arg0 = unsafe { <[u8] as wasm_bindgen::convert::RefFromWasmAbi>::ref_from_abi(arg0) };
        let arg0 = &*arg0;
        let arg1 = unsafe { <[u8] as wasm_bindgen::convert::RefFromWasmAbi>::ref_from_abi(arg1) };
        let arg1 = &*arg1;
        sub_slice(arg0, arg1)
    };

	<Uint8Array as wasm_bindgen::convert::ReturnWasmAbi>::return_abi(_ret)

	// _ret 在这里会释放
}

```

### 2.4. 底层 对 返回值 的 设置

+ js会生成一系列import 函数，用于完成底层wasm到高层js堆的拷贝；
+ TODO：关于底层如何拷贝两个堆的数据，暂时没看懂

``` js

imports.wbg.__wbg_newwithbyteoffsetandlength_ca3d3d8811ecb569 = logError(function(arg0, arg1, arg2) {
	var ret = new Uint8Array(getObject(arg0), arg1 >>> 0, arg2 >>> 0);
	return addHeapObject(ret);
});

```

### 2.5. js 代码

+ 给高层用的js，需要对wasm的函数再做一次包装，分配和拷贝arg0，arg1的数据
+ 当需要分配内存时候，wasm-bindgen会导出：__wbindgen_malloc
	- let offset: number = __wbindgen_malloc(size: number)
+ wasm32的返回值 的 释放由底层完成，参数的释放也有底层完成

``` js

__exports.sub_slice = function(a, b) {
    var ptr0 = passArray8ToWasm0(a, wasm.__wbindgen_malloc);
    var len0 = WASM_VECTOR_LEN;

    var ptr1 = passArray8ToWasm0(b, wasm.__wbindgen_malloc);
    var len1 = WASM_VECTOR_LEN;

	// 对应底层的：__wasm_bindgen_generated_sub_slice
	var ret = wasm.sub_slice(ptr0, len0, ptr1, len1);
    return takeObject(ret);
};

```

### 2.6. 分配参数

``` js

let WASM_VECTOR_LEN = 0;

function passArray8ToWasm0(arg, malloc) {
    const ptr = malloc(arg.length + 1);

	getUint8Memory0().set(arg, ptr / 1);

	WASM_VECTOR_LEN = arg.length;
    return ptr;
}

```

### 2.7. 返回值获取

``` js

const heap = new Array(32).fill(undefined);
heap.push(undefined, null, true, false);

function getObject(idx) {
	return heap[idx];
}

function takeObject(idx) {
    const ret = getObject(idx);
    dropObject(idx);
    return ret;
}

// heap_next: 当前空闲的槽位
function dropObject(idx) {
    if (idx < 36) return;
    heap[idx] = heap_next;
    heap_next = idx;
}

```

## 3. 导入 符号

``` js

let wbg = {};

// 填到 WebAssembly.instantiate 的导出对象
let imports = {wbg};

// __wbindgen 框架使用
wbg.__wbindgen_**+ = function () {

}

// __wbg_ 项目代码使用
wbg.__wbg_**+ = function () {

}

```

## 4. js-sys 和 web-sys

+ js_sys：JS 的一些函数的封装和宏展开
+ web-sys：需要指定feature打开对应的功能 和 代码 生成片段；
+ wasm-bindgen：js-wasm交互的思路，通过 生成 import 和 export 代码 完成；
	- wasm 对 js 的调用：通过 #[wasm-bindgen(module = "路径")] 指定对应的js文件；
	- 不支持 es5，必须有模块系统，nodejs 或者 esm；
	- 如果参数需要转换，则 会在 js 导入有相关的封装；

## 5. 疑问

简单的demo中，是应该要导入 webgl 代码的，但不知道为什么，GUI那边居然不需要导入uniform4i函数也可以运行？

如果属实，那么 在GUI的某种用法中，应该存在一种 除了 import 和 export 之外的 wasm 和 js 交互的机制 ？？？之后需要弄懂。

等小燕的结果出来后，再去查看原因：

``` js

imports.wbg.__wbg_uniform4i_caf8efda9e53f968 = function(arg0, arg1, arg2, arg3, arg4, arg5) {
	getObject(arg0).uniform4i(getObject(arg1), arg2, arg3, arg4, arg5);
};

```

## 6. 参考

cargo-expand 宏展开

+ 安装：cargo install cargo-expand
+ 运行：cargo expand --target=wasm32-unknown-unknown > r.rs