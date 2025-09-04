# TeslaMateMobile-Visited

功能：
将 TeslaMate 的 "visited" 
功能移动化，通过 App 在高德地图上进行展示。

*   **跨平台支持：** iOS 和 Android。
*   **多样化地图类型：**
    *   普通地图 🗺️
    *   卫星地图 🛰️
    *   导航视图 🧭
    *   公交视图 🚌
    *   黑夜视图 🌃
*   **自定义轨迹：** 支持修改行车轨迹的颜色。

---

## 部署方法

### 前提条件
您需要已经通过 Docker 成功部署了 [TeslaMate](https://github.com/teslamate-org/teslamate)。本服务的数据库将连接到您现有的 
TeslaMate 数据库。

---

### 服务端安装

您可以通过以下任一方式安装 `teslamate-visited` 服务端：

#### 1. 使用 Docker `run` 命令快速部署
普通vps、nas：

```
docker run -d \
--name teslamate-visited \
-p 9999:8080 \
--restart unless-stopped \
-e DB_HOST=database \
-e DB_PORT=5432 \
-e DB_NAME=teslamate \
-e DB_USER=teslamate \
-e DB_PASS=your_teslamate_db_password \
-e API_KEY=your_secret_api_key_here \
gdzhujun933/teslamate-visited:v1.2
```
openwrt：

```
docker run -d \
--name teslamate-visited \
--network docker_default \
-p 9999:8080 \
--restart unless-stopped \
-e DB_HOST=database \
-e DB_PORT=5432 \
-e DB_NAME=teslamate \
-e DB_USER=teslamate \
-e DB_PASS=your_teslamate_db_password \
-e API_KEY=your_secret_api_key_here \
gdzhujun933/teslamate-visited:v1.2
```


**重要环境变量说明：**

*   `DB_PASS`: **必须**与您部署 TeslaMate 时为数据库设置的 `POSTGRES_PASSWORD` 
或 `DATABASE_PASS` 完全相同。
*   `API_KEY`: 您自定义的 API 
密钥（Token）。后续在 App 
的设置中需要填入此值以进行验证。




2. 使用 Docker Compose (docker-compose.yml)将以下服务配置添加到您现有的 docker-compose.yml 文件中的 services 部分：

```
services:
  #您现有的服务配置

  # 添加 TeslaMatevisited 服务
  teslamate-visited:
    image: gdzhujun933/teslamate-visited:v1.2
    container_name: teslamate-visited
    restart: unless-stopped
    environment:
      - DB_HOST=database
      - DB_PORT=5432        # 与teslamate相同，默认5432
      - DB_NAME=teslamate        # 与teslamate相同
      - DB_USER=teslamate        # 与teslamate相同
      - DB_PASS=your_teslamate_db_password    # 必须与teslamate设置的完全相同
      - API_KEY=your_secret_api_key_here             # 自行设置，后续app中需要填入
    ports:
      - "9999:8080"  # 可修改为其他端口
````

**注意：**

*   `--network docker_default`: 确保此服务与您的 TeslaMate `database` 
容器在同一个 Docker 
网络中，以便能够通过服务名 `database` 访问数据库。如果您的 TeslaMate 
使用了不同的网络名称，请替换 `docker_default`。
*   `-p 9999:8080`: 将宿主机的 `9999` 端口映射到容器的 `8080` 
端口。您可以根据需要修改宿主机端口 `9999`。
*   将 `your_teslamate_db_password` 替换为您的 TeslaMate 数据库密码。
*   将 `your_secret_api_key_here` 替换为您自定义的 API 密钥。

---
## App 配置

安装完 App 后，您需要在 App 的**设置**中填写以下信息才能正常使用：

1.  **服务器地址 (Server Address):**
    *   格式为：`http://<您的服务器IP或域名>:<端口号>/api`
    *   例如，如果 `teslamate-visited` 服务部署在 IP 地址为 `10.0.0.234` 的服务器上，并且您在 Docker 
部署时将宿主机端口设置为 `9999`，则服务器地址应填写为：`http://10.0.0.234:9999/api`
2.  **Token:**
    *   填写您在部署服务端时设置的 `API_KEY` 的值。

---



注意：•确保 DB_HOST 指向的是您 Docker Compose 文件中定义的 TeslaMate 数据库服务的名称（通常是 database）。•将 your_teslamate_db_password 替换为您的 TeslaMate 数据库密码。•将 your_secret_api_key_here 替换为您自定义的 API 密钥。•如果您的 TeslaMate 服务使用了自定义的 Docker 网络，请确保 teslamate-visited 服务也通过 networks 指令连接到相同的网络。

---
## 截图
<img width="500" height="1084" alt="Simulator Screenshot - iPhone 16 Plus - 2025-09-04 at 00 19 21" src="https://github.com/user-attachments/assets/25e97cae-d7d0-4d05-a93b-ca04d93f1ae3" />
<img width="500" height="1084" alt="Simulator Screenshot - iPhone 16 Plus - 2025-09-04 at 00 19 29" src="https://github.com/user-attachments/assets/0b8ac2a9-9e2a-4bad-a1cb-5395ea733e23" />
<img width="500" height="1084" alt="Simulator Screenshot - iPhone 16 Plus - 2025-09-04 at 00 19 37" src="https://github.com/user-attachments/assets/e5d02942-e47d-4146-978e-26aefc71befd" />
<img width="500" height="1084" alt="Simulator Screenshot - iPhone 16 Plus - 2025-09-04 at 00 20 00" src="https://github.com/user-attachments/assets/31eb377e-b7ba-499c-b097-0000e6e4db5a" />
<img width="500" height="1084" alt="Simulator Screenshot - iPhone 16 Plus - 2025-09-04 at 00 19 50" src="https://github.com/user-attachments/assets/57ae01ef-3ae9-4c46-a70c-7414717b39a8" />

