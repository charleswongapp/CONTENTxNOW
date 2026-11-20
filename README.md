# CONTENTxNOW 内容工作台

把平时转发给自己的好文章，变成下一期公众号的选题库。在 CONTENTxNOW 里，你可以沿着一篇参考文章开稿，给自己的读者选正文、挑标题、配图，再把排好版的成稿送到要发布的公众号。

[独立部署](#独立部署) · [公众号工作流](#从微信收藏到公众号发布) · [界面截图](#工作台截图) · [微信交流群](#微信交流群)

## CONTENTxNOW 在线版即将开放

CONTENTxNOW 正在准备面向大家开放的 SaaS 在线版。届时打开浏览器就能使用内容工作台。想提前体验，可以先[扫码加入微信交流群，预约体验资格](#微信交流群)。

## 从微信收藏到公众号发布

1. **看到好内容，先转发给自己。** 刷到一个好选题、一篇值得拆解的爆款，顺手转给自己的微信或「文件传输助手」。平时花几秒留下的参考，到做选题时就有了用处。
2. **一次整理，把整批文章收进素材库。** 导出这段聊天记录。如果拿到的是压缩包，先解压，把公众号文章链接整理到 Excel 或 CSV 的第一列。在「爆款素材库 → 添加爆款文章 → 链接导入」上传表格，也可以直接粘贴链接，每行一条。确认识别结果后，工作台会去重，逐篇采集原文和配图。手头已有文章 HTML 时，可直接选择多篇批量导入。
3. **挑一篇参考，定下自己的写作方向。** 在素材库里打开文章，点「发起母稿生成」，选好目标读者，再写下这次的要求。例如：「写给第一次接触这个话题的人。保留原文提出的问题，用我提供的门店案例来讲，结尾留一个读者愿意回答的问题。」
4. **正文、标题、摘要和图片，分别挑最合适的。** 同一选题可以比较多版正文，再从候选标题和摘要里选出最贴合的一组。启用生图模型后，封面和文中配图也有多套候选可挑。喜欢第一版的正文、第二版的标题和另一张封面，就按这个组合定稿。
5. **定稿后，选一种方式发表。** 如果习惯在公众号后台做最后一遍校稿，可以一键复制图文。同时运营多个公众号时，也可以从工作台直接把成稿送到指定账号的草稿箱。

### 手动发布 · 一键复制到公众号编辑器

在「生成审阅」选好正文和配图，点击 **「复制 HTML」**。登录微信公众平台，新建图文，把光标放进**正文编辑区**，按 ⌘V（Mac）或 Ctrl+V（Windows）。段落样式、文字强调和文中配图会随正文一起粘贴。这条路径无需在 CONTENTxNOW 配置公众号 API。[打开微信公众平台](https://mp.weixin.qq.com/)

粘贴后，微信编辑器会尝试自动上传文中配图。等图片显示完整，填好标题、摘要并选好封面，预览后即可发表。发完回到工作台点「标记手动发布」，下次选稿时就知道这篇发过了。

### API 发布 · 把整篇成稿送进指定草稿箱

在工作台点「发布」，选好目标公众号并确认。CONTENTxNOW 通过微信官方 API 填入标题、摘要和排版后的正文，自动上传封面和文中配图。开通草稿及素材接口并完成配置的未认证公众号，也可以在微信草稿箱里预览并手动发表。

具备发布接口权限的账号还可以在工作台立即发表，或设置定时发表，由工作台到点提交。微信对公众号的发布接口要求企业主体已认证，具体以目标账号的接口权限为准。[微信草稿接口说明](https://developers.weixin.qq.com/doc/service/api/draftbox/draftmanage/api_draft_add.html) · [微信发布接口说明](https://developers.weixin.qq.com/doc/service/api/public/api_freepublish_submit.html)

## 工作台截图

以下截图使用匿名示例数据，灰线代替账号、文章标题和正文。

**素材库**按标签筛选文章，可在同一页切换 Markdown 与 HTML 原文。

![素材库的标签筛选、文章列表和原文阅读区](assets/material-library.png)

**母稿生成**可以从选中的素材发起，选择发布平台、生成模型和目标受众。

![母稿生成窗口中的平台、模型和受众选项](assets/generation-setup.png)

**生成审阅**集中展示推荐标题和备选标题，编辑可以复制或采用选中的版本。

![标题审阅页中的生成记录、推荐标题和采用按钮](assets/editorial-review.png)

## 独立部署

GitHub 公开产品文档和版本说明。企业内部自用可通过[微信交流群](#微信交流群)咨询部署授权，取得与版本及机器架构匹配的 **工作台和链接采集镜像、Compose 配置、环境变量模板、工作流目录及权重清单** 后，按以下步骤安装。GitHub Release 自动生成的 Source code 压缩包只含本展示仓的文件。

### 1. 准备运行环境

- **Docker**。使用 Docker Desktop，或 Linux 上的 Docker Engine，配套 Docker Compose **2.30.0 或更高版本**。以下命令适用于 macOS 或 Linux 的终端，另需 OpenSSL 和 curl。可运行 `docker compose version` 查看版本。[Docker Compose 安装说明](https://docs.docker.com/compose/install/)
- **PostgreSQL**。复用现有实例，准备独立业务库 `Workflow` 和可在其中建表的数据库用户。若需新建，假设已有业务用户 `studio_user`，数据库管理员可执行 `CREATE DATABASE "Workflow" OWNER studio_user;`。工作台容器需要能连接该实例。
- **配音权重**。默认部署包含本地配音，启动时会校验 `Fun-CosyVoice3-0.5B-2512` 的 14 个文件，合计约 5.05 GiB。可复用已有权重，或按随交付提供的 `scripts/model-manifest.json` 从各文件对应的 ModelScope 地址下载，保留清单中的相对路径。记下包含 `cosyvoice3.yaml` 的目录的绝对路径。[ModelScope 模型页](https://modelscope.cn/models/FunAudioLLM/Fun-CosyVoice3-0.5B-2512)

### 2. 导入镜像并初始化配置

在部署文件中找到同时包含 `compose.yaml`、`docker/` 和 `platform-workflows/` 的目录，进入该目录后执行后续命令。若以归档形式取得镜像，对每个镜像归档执行一次以下命令，将路径换成实际文件路径。

```sh
docker image load --input /path/to/image.tar
```

首次安装时，以下命令会从模板创建配置，分别生成工作台服务密钥和链接采集密钥。重复执行会保留已有文件。

```sh
(
  set -euC
  umask 077
  mkdir -p runtime/link-archives
  if [ ! -e runtime/docker.env ]; then
    token=$(openssl rand -hex 32)
    sed "s/^STUDIO_SERVICE_TOKEN=$/STUDIO_SERVICE_TOKEN=$token/" docker/docker.env.example > runtime/docker.env
  fi
  test -e runtime/docker.paths.env || cp docker/docker.paths.env.example runtime/docker.paths.env
  test -e runtime/link-capture.secret || openssl rand -hex 32 > runtime/link-capture.secret
)
```

### 3. 配置数据库和模型

编辑下列文件中的对应项。示例中的用户名、密码和路径需换成实际值。

| 配置文件 | 配置项 | 填写示例 |
| --- | --- | --- |
| `runtime/docker.env` | `DATABASE_URL` | `postgresql://studio_user:YOUR_PASSWORD@host.docker.internal:5432/Workflow`。此地址适用于数据库端口已开放给容器的宿主机。独立数据库服务器请填写其内网地址，密码中的特殊字符需按 URL 规则编码。 |
| `runtime/docker.paths.env` | `CONTENT_STUDIO_MODEL_DIR` | `/absolute/path/to/Fun-CosyVoice3-0.5B-2512`，工作台会以只读方式挂载该目录。 |
| `runtime/docker.paths.env` 和 `runtime/docker.env` | `STUDIO_PORT` 和 `STUDIO_PUBLIC_URL` | 默认分别为 `80` 和 `http://localhost`。若 80 已占用，可分别改为 `8080` 和 `http://localhost:8080`。 |

如果使用企业自己的写作模型，在 `runtime/docker.env` 中设置以下两项。示例指向宿主机上的 Ollama，把模型名换成该服务 `GET /v1/models` 返回的实际 ID。需要鉴权时，在同一文件另设 `STUDIO_LOCAL_MODEL_API_KEY`。

```dotenv
STUDIO_LOCAL_MODEL_BASE=http://host.docker.internal:11434/v1
STUDIO_LOCAL_MODEL=qwen3:8b
```

### 4. 启动并首次登录

```sh
docker compose --env-file runtime/docker.paths.env config --quiet
docker compose --env-file runtime/docker.paths.env up -d --no-build --pull never --wait
docker compose --env-file runtime/docker.paths.env ps
curl --fail --show-error http://localhost/health
```

当 `studio` 和 `link-capture` 均显示 `healthy`，且健康接口返回 `"ok":true`，在部署机器上打开 [http://localhost](http://localhost)，首次进入时创建管理员账号，密码至少 12 位。若改用了 8080，访问地址和健康检查地址都要加上 `:8080`。接入本地写作模型后，可导入一篇参考文章，点「发起母稿生成」检查模型是否可选。

### 日常维护

启动失败时，可运行 `docker compose --env-file runtime/docker.paths.env logs --tail 100 studio link-capture` 查看日志。若提示缺少镜像，用 `docker compose --env-file runtime/docker.paths.env config --images` 核对已导入的镜像名称和标签。备份或迁移时，请一并保存 PostgreSQL 的 `Workflow` 库、`runtime/`、`platform-workflows/` 和配音权重。保留原 `STUDIO_SERVICE_TOKEN`，以便还原已加密的凭据。

## 企业本地模型

企业独立部署版可连接本机或内网的 OpenAI 兼容模型服务。管理员指定模型地址和已加载的模型 ID，编辑即可在生成页面选择该模型。模型服务须提供模型列表、聊天补全和 JSON 结构化输出。分析原文配图还需要视觉模型。

本地模式把推理请求直接交给企业配置的服务，连接失败会提示重试。使用企业本地算力的推理不经过平台钱包。使用托管模型的团队可以在统一门户查看模型消费记录。

## 版本与授权

[查看 v1.0.0 发行说明](https://github.com/charleswongapp/CONTENTxNOW/releases/tag/v1.0.0)。本仓库公开产品说明和版本记录。核心软件闭源，企业可联系获取内部自用部署授权。未经另行书面授权，禁止将自有软件打包售卖、转授权或用于对外提供商业服务。

## 接下来的内容形式

**贴图号内容、小红书笔记、短视频的自动生成**已列入开发计划。欢迎[加入交流群](#微信交流群)，告诉我们你最想先做哪一种内容。

## 开发预告 · STREAMxNOW AI 直播平台

STREAMxNOW 正在开发，即将开放。它面向希望用一台电脑开播的主播，开发方向包括防识别、实时互动、语音克隆，以及实景与数字人同屏直播。

在导播台，你可以手动把一段话术关联到对应的视频素材。开播后，两条时间线实时显示话术讲到哪里、视频播到哪里。需要临时讲解某件商品时，可以插播关联的话术和画面。

![STREAMxNOW 导播台，左侧为话术队列，右侧为视频队列，蓝色虚线标示当前播放位置](assets/streamxnow-director.png)

## 微信交流群

用微信扫描下方二维码进群，可预约 CONTENTxNOW 在线版提前体验资格，也可以交流内容工作流和企业部署。

![CONTENTxNOW 微信交流群二维码](assets/community-qr.png)
