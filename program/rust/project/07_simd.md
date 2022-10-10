# SIMD

## 1. 查看本地 支持 情况

注意：打印出来的 target_feature

``` cmd
rustc --print cfg
```

## 2. 查看 支持的 目标 CPU

``` cmd
rustc --print target-cpus
```

## 3. 本地 配置

当运行 cargo bench 时候，只有如下 会 起作用：

项目目录/.cargo/config


``` toml
[build]
rustflags = ["-Ctarget-cpu=native", "-Ctarget-feature=+sse2,+fma"]
```

