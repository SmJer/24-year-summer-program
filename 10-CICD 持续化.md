# CI/CD 持续集成与交付

在软件开发过程中，CI/CD 通常指的是持续集成和持续交付或持续部署。CI/CD 通过在应用程序的构建、测试和部署中实施自动化，在开发和运营团队之间架起了桥梁。

现代 DevOps 实践涉及软件应用程序在整个开发生命周期内的持续开发、持续测试、持续集成、持续部署和持续监控。CI/CD 构成了现代 DevOps 业务的主干。

> 简单来说，CI/CD 在软件开发的过程中达到了自动化的效果。软件开发完成后通过 CI/CD 可以快速的构建、测试、部署应用。

## 通过 GitHub 实现 CI/CD

参考项目：<https://gitee.com/myhfw003/dev-ops-deployment-project.git>

1. 拉取仓库后，查看目录可见：

   ![20240729150909](https://oss.isiou.cn/images/20240729150909.png)

2. 登录 GitHub 主页后，点击左侧 New 新建仓库：

   ![20240729151033](https://oss.isiou.cn/images/20240729151033.png)

3. 创建如图所示新项目：

   ![20240729151141](https://oss.isiou.cn/images/20240729151141.png)

4. 创建完成后如图所示，并在本机上拉取仓库：

   ![20240729151206](https://oss.isiou.cn/images/20240729151206.png)

5. 将第 1 步拉取仓库下的 .gitignore、package.json、app.js 复制到新拉取的仓库下，并在本机上使用 yarn 拉取依赖：

   ```shell
   npm config set registry https://registry.npmmirror.com
   npm i -g yarn
   yarn
   ```

   > 使用 yarn 或其他包管理器若报错则需使用管理员权限打开 PowerShell 使用 `set-ExecutionPolicy RemoteSigned` 命令。

   ![20240729151640](https://oss.isiou.cn/images/20240729151640.png)

   ![20240729151703](https://oss.isiou.cn/images/20240729151703.png)

   使用 `node app.js` 命令，查看服务运行：

   ![20240729151812](https://oss.isiou.cn/images/20240729151812.png)

6. 配置 Webpack 打包：

   - 安装 Webpack、babel 及其依赖：

     > Webpack 是一个模块打包工具，可以将 JavaScript 文件、样式表、图片等资源打包成一个或多个静态文件。

     ```shell
     npm install --save-dev webpack webpack-cli webpack-node-externals
     ```

   - 配置打包配置文件：

     在根目录下创建 `webpack.config.cjs` 文件，并配置以下内容：

     ```cjs
     const path = require("path");
     module.exports = {
       // 设置为开发模式
       mode: "development",
       // 入口文件
       entry: "./app.js",
       // 环境
       target: "node",
       // 设置输出
       output: {
         // 输出目录
         path: path.resolve(__dirname, "dist"),
         // 输出文件名
         filename: "bundle.js",
       },
       resolve: {
         // 解析 .js
         extensions: [".js"],
       },
     };
     ```

   - 修改 package.json 文件：

     添加以下内容：

     ```json
     "scripts": {
        "build": "webpack --config webpack.config.cjs"
     }
     ```

   - 运行打包命令，测试是否成功：

     ```shell
     npm run build
     ```

     ![20240729155101](https://oss.isiou.cn/images/20240729155101.png)

     根目录下会生成 `dist/bundle.js` 文件。

7. 在项目根目录下嵌套创建 `.github\workflows` 文件夹，并创建 `main.yml` 文件（名称自定义）：

   `main.yml` 文件配置：

   ```yml
   # CI/CD 项目名称
   name: Node.js CI/CD

   on:
     # 监听 push 和 PR 改动
     push:
       # 监听 main 分支
       branches:
         - main
     pull_request:
       branches:
         - main

   jobs:
     # 构建
     build:
       # 运行环境
       runs-on: ubuntu-latest

       # 步骤
       steps:
         # 将代码仓库复制到工作目录，以便后续的步骤可以访问并操作这些文件
         - name: 检出仓库
           uses: actions/checkout@v3

         - name: 安装 Node.js
           uses: actions/setup-node@v3
           with:
             node-version: "20"

         - name: 拉取依赖
           run: npm install

         - name: 打包项目
           run: npm run build

         # 将构建过程中生成的文件上传，以便在后续工作中继续使用
         - name: 上传产品
           uses: actions/upload-artifact@v3
           with:
             name: node-app
             path: "dist/bundle.js"
   ```

8. 将仓库更改同步到 GitHub 上：

   ```shell
   git commit -m "push"
   git branch -M main
   git push -u origin main
   ```

9. 查看 GitHub Action 列表：

   ![20240729161249](https://oss.isiou.cn/images/20240729161249.png)

   ![20240729161319](https://oss.isiou.cn/images/20240729161319.png)

   查看构建过程：

   ![20240729161354](https://oss.isiou.cn/images/20240729161354.png)

   产出：

   ![20240729161443](https://oss.isiou.cn/images/20240729161443.png)

## 通过 Gitee 实现 CI/CD

1. 创建仓库，步骤同上，或者使用 fork 直接复制仓库。

   ![20240729163745](https://oss.isiou.cn/images/20240729163745.png)

2. 在本地上拉取仓库，并进入目录并配置依赖和文件，同上节第 5、6 步。

3. 查看 Gitee 控制台，选择流水线：

   ![20240729164149](https://oss.isiou.cn/images/20240729164149.png)

   点击开通后，选择不创建：

   ![20240729164251](https://oss.isiou.cn/images/20240729164251.png)

   点击新建流水线：

   ![20240729164312](https://oss.isiou.cn/images/20240729164312.png)

   设置流水线名称和标识：

   ![20240729164342](https://oss.isiou.cn/images/20240729164342.png)

   设置监听分支：

   ![20240729164403](https://oss.isiou.cn/images/20240729164403.png)

   任务编排，新建任务：

   ![20240729164544](https://oss.isiou.cn/images/20240729164544.png)

   选择 Node.js 构建：

   ![20240729164619](https://oss.isiou.cn/images/20240729164619.png)

   编辑任务：

   ![20240729164730](https://oss.isiou.cn/images/20240729164730.png)

   完成后保存即可：

   ![20240729164810](https://oss.isiou.cn/images/20240729164810.png)

4. 在本地仓库内：

   ```shell
   # 拉取更新
   git pull
   git add .
   git commit -m "push"
   git push
   ```

5. 提交到远程仓库后，查看流水线：

   ![20240729165437](https://oss.isiou.cn/images/20240729165437.png)

   查看构建过程：

   ![20240729165505](https://oss.isiou.cn/images/20240729165505.png)

   查看产出：

   ![20240729165551](https://oss.isiou.cn/images/20240729165551.png)

## CI/CD 实现上传-打包-部署

上述过程中实现了每次提交代码，都会在远程仓库的 CI/CD 任务下完成打包并输出文件，接下来将以 Gitee 为例，实现打包后文件直接部署在服务器上。

示例项目仍为上节中 Node.js 项目，且前期工作（上传-打包）不在此赘述，接着上一步 Gitee 流水线为例。

1. 查看仓库流水线：

   ![20240729175807](https://oss.isiou.cn/images/20240729175807.png)

2. 编辑流水线：

   ![20240729175838](https://oss.isiou.cn/images/20240729175838.png)

   选择任务编排列表，在上一任务节点右侧选择添加新阶段，设置阶段名称：

   ![20240729180030](https://oss.isiou.cn/images/20240729180030.png)

   点击新的任务后，添加任务，并选择部署任务：

   ![20240729180102](https://oss.isiou.cn/images/20240729180102.png)

   选择主机部署：

   ![20240729180222](https://oss.isiou.cn/images/20240729180222.png)

3. 进行到上一步后，需要在 Gitee 中配置主机组，接下来以阿里云为例：

   - 查看主机组：

     前往 <https://gitee.com/profile/host_groups> 查看当前帐号的主机组：

     ![20240729180406](https://oss.isiou.cn/images/20240729180406.png)

   - 添加主机组：

     若无主机组的话则需添加，此处以阿里云为例：

     - 选择新建主机组：

       ![20240729180512](https://oss.isiou.cn/images/20240729180512.png)

       选择阿里云导入，点击下一步：

       ![20240729180635](https://oss.isiou.cn/images/20240729180635.png)

       自定义主机组名称和标识，点击凭证管理：

       ![20240729180709](https://oss.isiou.cn/images/20240729180709.png)

       选择新建凭证：

       ![20240729180805](https://oss.isiou.cn/images/20240729180805.png)

       选择阿里云，点击下一步后：

       ![20240729180911](https://oss.isiou.cn/images/20240729180911.png)

       前往阿里云控制台，获取自己的 AKID 和 AK 密钥，地址：<https://ram.console.aliyun.com/manage/ak>

       界面如图所示，点击创建 AccessKey 后验证身份，获得 AccessKey：

       > 此步骤弹窗先不要关闭，关闭后无法再获取此 ID 对应密钥！

       ![20240729181200](https://oss.isiou.cn/images/20240729181200.png)

     - 回到 Gitee 凭证管理，将刚获取到的 AKID 和 AK 填写进去：

       > 地域 ID 对应表，服务器在哪个地域就填哪个 ID：
       >
       > ![20240729181453](https://oss.isiou.cn/images/20240729181453.png)

       填写完成后如图所示：

       ![20240729181535](https://oss.isiou.cn/images/20240729181535.png)

       点击保存即可。

     - 回到新建主机组页面，选择刚创建的凭据：

       ![20240729181658](https://oss.isiou.cn/images/20240729181658.png)

       点击下一步，点击开始安装后安装云助手：

       ![20240729181749](https://oss.isiou.cn/images/20240729181749.png)

       点击确认后，查看主机组列表：

       ![20240729181831](https://oss.isiou.cn/images/20240729181831.png)

4. 主机组配置完成后，继续编辑任务：

   执行主机组中，选择刚才添加的主机组：

   ![20240729182213](https://oss.isiou.cn/images/20240729182213.png)

   文件来源选择上游构建产出并选择上传文件：

   ![20240729182419](https://oss.isiou.cn/images/20240729182419.png)

   编写部署脚本：

   ![20240729182958](https://oss.isiou.cn/images/20240729182958.png)

   ![20240729183254](https://oss.isiou.cn/images/20240729183254.png)

   > 参考脚本：
   >
   > ```shell
   > # 安装 npm
   > apt install npm -y
   > # 创建目录
   > mkdir -p /home/admin/app
   > # 解压上游产出文件包
   > tar zxvf ~/gitee_go/deploy/output.tar.gz -C /home/admin/app
   > # 设置 npm 源为阿里源
   > npm config set registry http://registry.npmmirror.com
   > # 安装挂载软件 pm2
   > npm install -g pm2
   > # 将 node 项目挂载在 pm2 上
   > pm2 start /home/admin/app/dist/bundle.js
   > echo "OK"
   > ```

   配置完成后，点击保存并更新：

   ![20240729183430](https://oss.isiou.cn/images/20240729183430.png)

5. 回到流水线页面，查看正在进行的构建：

   ![20240729183522](https://oss.isiou.cn/images/20240729183522.png)

   稍等一会，等待部署完成：

   ![20240729183711](https://oss.isiou.cn/images/20240729183711.png)

   访问服务器的 3000 端口查看是否部署并运行：

   > 服务器需先放行 3000 端口。

   ![20240729183810](https://oss.isiou.cn/images/20240729183810.png)
