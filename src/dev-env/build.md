# 编译

进入项目根目录
```bash
mkdir build && cd build
cmake .. -DWITH_BOOST=../boost -DWITH_DEBUG=ON
make -j`nproc`
```
