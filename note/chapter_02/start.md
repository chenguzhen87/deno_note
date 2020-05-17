# 快速开始

## helloworld

### 准备代码

- [mod.ts](https://github.com/chenshenhai/deno_note/blob/master/demo/hello/mod.ts);

```js
// deno run --allow-net

/**
 * helloworld HTTP服务
 * @param opts {Deno.ListenOptions}
 */
async function simpleServer(opts: Deno.ListenOptions): Promise<void> {
  // 创建TCP服务
  const listener: Deno.Listener = Deno.listen(opts) as Deno.Listener;
  console.log("listening on", `${opts.hostname}:${opts.port}`);
  // 死循环监听TCP请求
  while (true) {
    // 等待TCP连接
    const conn: Deno.Conn = await listener.accept();
    // 执行响应
    const CRLF = "\r\n";
    const bodyStr = "hello world!";
    const res = [
      `HTTP/1.1 200`,
      `content-length: ${bodyStr.length}`,
      ``,
      `${bodyStr}`
    ].join(CRLF);
    // 将HTTP报文字符串转成 二进制数据流
    const encoder = new TextEncoder();
    // 将HTTP二进制数据流写入TCP连接
    await conn.write(encoder.encode(res));
    conn.close();
  }
}
simpleServer({
  hostname: "127.0.0.1",
  port: 3001
});
```

### 执行代码

```js
deno run --allow-net mod.ts
```

### 浏览器访问

- 访问: [http://127.0.0.1:3001/](http://127.0.0.1:3001/)
- 访问结果

![chapter_0_02](./../image/hello_server.jpg)
