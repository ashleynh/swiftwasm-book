# Getting Started (with Nightly Toolchain)


## Installation

You can install latest toolchain of SwiftWasm from [Release Page](https://github.com/swiftwasm/swift/releases)

The toolchains can be installed via [`swiftenv`](https://github.com/kylef/swiftenv) like official nightly toolchain.

e.g.
```sh

$ swiftenv install \
  https://github.com/swiftwasm/swift/releases/download/swift-wasm-DEVELOPMENT-SNAPSHOT-2020-06-07-a/swift-wasm-DEVELOPMENT-SNAPSHOT-2020-06-07-a-osx.tar.gz
$ swift --version
Swift version 5.3-dev (LLVM 7fc8796bc1, Swift 5be35e7aee)
Target: x86_64-apple-darwin19.3.0
```


## Hello, World!

```sh
$ echo 'print("Hello, world!")' > hello.swift
# Compile Swift code into WebAssembly with WASI
$ TOOLCHAIN_PATH=$(dirname $(swiftenv which swiftc))/../
$ swiftc \
    -target wasm32-unknown-wasi \
    -sdk $TOOLCHAIN_PATH/share/wasi-sysroot \
    hello.swift -o hello.wasm
```

You can the run the produced binary with [wasmer](https://wasmer.io/) (or other WebAssembly runtime):

```sh
$ wasmer hello.wasm
```

The produced binary depends on WASI which is an interface of system call for WebAssembly.
So you need to use WASI supported runtime and when you run the binary on browser, you need WASI polyfill library like [@wasmer/wasi](https://github.com/wasmerio/wasmer-js/tree/master/packages/wasi).


## Swift Package Manager

You can also use SwiftPM for managing packages in the same way as other platforms.

```sh
$ swiftenv local wasm-DEVELOPMENT-SNAPSHOT-2020-06-07-a
$ swift package init --type executable
$ swift build --triple wasm32-unknown-wasi
$ wasmer ./.build/debug/hello-swiftwasm
Hello, world!
```


## JavaScript Binding

[JavaScriptKit](https://github.com/kateinoigakukun/JavaScriptKit) is Swift framework to interact with JavaScript through WebAssembly.

You can use any JavaScript API from Swift using this library.