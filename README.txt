Maailmavalo™ Mideo Maker — Cloudflare Workers 免费版部署包

本版本针对 Cloudflare Workers Static Assets 单文件 25 MiB 限制进行了适配。
原始 ffmpeg-core.wasm（32,232,419 字节）已拆成两个文件：
- ffmpeg/ffmpeg-core.wasm.part-001（16,777,216 字节）
- ffmpeg/ffmpeg-core.wasm.part-002（15,455,203 字节）
浏览器会从同源地址并行下载两个分片，检查大小后拼成 application/wasm Blob，再传给 FFmpeg。
FFmpeg 核心 JS 仍使用 /ffmpeg/ffmpeg-core.js；Worker、模块和日志功能均保留。

本地预览
1. 在项目根目录运行：python -m http.server 8000
2. 打开：http://localhost:8000
3. 选择 MKV 测试导出。

部署到 Cloudflare Workers 免费版（推荐 Wrangler 静态资源部署）
1. 安装 Node.js。
2. 在项目根目录打开终端。
3. 执行：npx wrangler login
4. 执行：npx wrangler deploy
5. Wrangler 会按 wrangler.jsonc 中的 assets.directory = "." 上传项目静态文件。
   .assetsignore 会排除 README、Wrangler 配置和忽略文件本身。

注意
- 不要把原始 ffmpeg-core.wasm 放回项目目录；本版本使用两个 .part 文件。
- 不要重命名或移动两个 .part 文件，index.html 中使用的是固定路径。
- 两个分片都低于 Cloudflare Workers 静态资源单文件 25 MiB 限制。
- 静态资源由 Cloudflare 自动缓存。
- 本项目只在浏览器端运行 FFmpeg，不会把用户视频上传到 Cloudflare。
- 若更换 FFmpeg WASM，请重新分片，并同步更新 index.html 中的预期字节数和总大小。
