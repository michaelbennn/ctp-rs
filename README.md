# ctp-rs
CTP sdk version: `6.3.15_20190220 15:47:00`

`*.hpp` and `*.cpp` are generated by `autobind.py`, then processed by [rust_bindgen](https://rust-lang.github.io/rust-bindgen/requirements.html). Look at [xtp-rs](https://github.com/dovahcrow/xtp-rs) for detail.

__Use at your own risk__. The crate is unsafe, look at [examples/tdapi.rs](./examples/tdapi.rs) and [examples/mdapi.rs](./examples/mdapi.rs) for how to use it.

## How to use autobind.py
If you want to generate wrapp.hpp by yourself or wrapper similar apis. You can put your header files in `shared/include` like I did. Then run

```
python3 -m pip install libclang
python3 autobind.py > src/wrapper_ext
```

Those cpp code are included in `src/wrapper.hpp`, where you can add your custom functions.

Then the rust_bindgen will generate rust `extern "C" func_XXX` function declarations. Since then all api function should be usable.

There are lots of `extern "C" Rust_XXX_Trait_On_XX` function inside wrapper_ext, which are colloceted in `build.rs` to generate nice trait object. Remove it in `autobind.py` if you don't want it.

## where to put my .so files
XXX.so files are not included, please put it like this to satify compiler:

```
$ tree shared

shared
├── data_collect
│   ├── unix.x86_64
│   │   ├── libLinuxDataCollect.so -> LinuxDataCollect.so
│   │   └── LinuxDataCollect.so
│   ├── windows.x86
│   │   ├── WinDataCollect.dll
│   │   └── WinDataCollect.lib
│   └── windows.x86_x64
│       ├── WinDataCollect.dll
│       └── WinDataCollect.lib
├── include
│   ├── DataCollect.h
│   ├── ThostFtdcMdApi.h
│   ├── ThostFtdcTraderApi.h
│   ├── ThostFtdcUserApiDataType.h
│   └── ThostFtdcUserApiStruct.h
├── md
│   ├── unix.x86_64
│   │   ├── libthostmduserapi_se.so -> thostmduserapi_se.so
│   │   └── thostmduserapi_se.so
│   ├── windows.x86
│   │   ├── thostmduserapi_se.dll
│   │   └── thostmduserapi_se.lib
│   └── windows.x86_x64
│       ├── thostmduserapi_se.dll
│       └── thostmduserapi_se.lib
└── td
    ├── unix.x86_64
    │   ├── libthosttraderapi_se.so -> thosttraderapi_se.so
    │   └── thosttraderapi_se.so
    ├── windows.x86
    │   ├── thosttraderapi_se.dll
    │   └── thosttraderapi_se.lib
    └── windows.x86_x64
        ├── thosttraderapi_se.dll
        └── thosttraderapi_se.lib
```
