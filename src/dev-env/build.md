# 编译

进入项目根目录
```bash
mkdir build && cd build
cmake .. -DWITH_BOOST=../boost -DWITH_DEBUG=ON
make -j`nproc`
```

这一步仅用于验证环境配置是否正确，若能正常编译再进入下一步。
