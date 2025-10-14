安装一个插件才能生成Go代码
```text
$ go get github.com/golang/protobuf/protoc-gen-go
```



```text
protoc --proto_path=src --go_out=build/gen src/foo.proto src/bar/baz.proto
```
- `--go_out`告诉编译器把Go源代码写到哪里。
- `src/foo.proto src/bar/baz.proto`：两个proto文件
- 生成文件的扩展名是`.pb.go`。比如说`player_record.proto`编译后会得到`player_record.pb.go`。

`protoc --go_out=. *.proto`：在当前目录下生成go文件