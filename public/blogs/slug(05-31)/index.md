https://github.com/dromara/free-fs?tab=readme-ov-file


## 特性
## 核心亮点
大文件上传 - 分片上传、断点续传、秒传功能，支持 TB 级文件
实时上传进度 - 实时推送上传进度，精确到分片级别
秒传功能 - 基于 MD5 双重校验，相同文件秒级完成
插件化存储 - SPI 机制热插拔，5 分钟接入一个新存储平台
工作空间 - 多工作空间支持，团队协作更高效
国际化支持 - 中英文双语支持，轻松扩展更多语言
模块化架构 - 清晰的分层设计，易于维护和扩展
在线预览 - 支持多种文件格式的在线预览，预览防盗链功能
安全可靠 - JWT 认证、权限控制、文件完整性校验
功能特性
文件管理

文件上传（分片上传、断点续传、秒传）
文件预览
文件下载
文件夹创建与管理
文件/文件夹重命名、移动
文件分享/授权码分享
文件删除
工作空间 🆕

多工作空间管理
工作空间成员管理
角色权限控制
成员邀请（邮件邀请）
工作空间切换
团队协作 🆕

成员邀请与管理
角色权限分配
工作空间隔离
成员权限控制
国际化 🆕

中文简体
英文
支持扩展更多语言
认证与授权

用户名密码登录
邮箱验证码登录 🆕
JWT Token 认证
基于角色的权限控制（RBAC）
回收站

文件还原（支持批量操作）
彻底删除（支持批量操作）
一键清空回收站
自动清理机制
存储平台

支持多存储平台（本地、MinIO、阿里云 OSS、七牛云 Kodo、S3 体系等）
一键切换存储平台
平台配置管理
存储空间统计
预览支持
系统默认支持以下多种文件类型的预览：

图片: jpg, jpeg, png, gif, bmp, webp, svg, tif, tiff
文档: pdf, doc, docx, xls, xlsx, csv, ppt, pptx
文本/代码: txt, log, ini, properties, yaml, yml, conf, java, js, jsx, ts, tsx, py, c, cpp, h, hpp, cc, cxx, html, css, scss, sass, less, vue, php, go, rs, rb, swift, kt, scala, json, xml, sql, sh, bash, bat, ps1, cs, toml Markdown: md, markdown
音视频: mp4, avi, mkv, mov, wmv, flv, webm, mp3, wav, flac, aac, ogg, m4a, wma
压缩包: zip, rar, 7z, tar (支持查看目录结构，支持预览压缩包中的文件)