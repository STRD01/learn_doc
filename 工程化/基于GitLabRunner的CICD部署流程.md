# 基于GitLab Runner的CICD部署流程

## 工程化概述

前端工程化是一种软件开发实践，旨在提高前端开发的效率、可维护性和协作性。它涵盖了一系列工具、流程和最佳实践，用于优化前端开发工作流程，以满足现代 Web 应用程序的需求。下面是前端工程化的一些关键方面：
1. 版本控制：使用版本控制系统（如Git）来跟踪代码更改，协助团队合作和确保代码的版本管理。
2. 自动化构建：前端工程化包括使用构建工具（如Vite、Webpack、Grunt、Gulp）来自动化任务，如文件合并、压缩、代码转换（例如，将ES6转换为ES5）、资源优化等。
3. 包管理：使用包管理工具（如npm或Yarn）来管理项目依赖，使开发人员可以轻松地安装、更新和分享第三方库和工具。
4. 代码规范和风格指南：制定和遵循代码规范和风格指南有助于确保代码的一致性和可读性。工具如ESLint和Prettier可以帮助检查和自动修复代码问题。
5. 单元测试和集成测试：编写测试用例以确保代码质量和可维护性。使用测试工具（如Jest、Mocha、Cypress）来执行单元测试和集成测试。
6. 持续集成/持续交付（CI/CD）：自动化测试和部署流程，确保代码更改经过测试后能够快速、稳定地交付到生产环境。
7. 模块化开发：将前端代码分解为小的、可重用的模块，以促进团队合作、减少重复编码，并提高可维护性。
8. 性能优化：前端工程化也包括优化网站性能的实践，如减小页面加载时间、减小资源文件大小、使用浏览器缓存等。
9. 代码分离：使用动态导入或按需加载技术，以确保只加载必需的代码，提高性能和用户体验。
10. 代码分支管理：采用适当的分支管理策略，以支持同时进行多个功能开发、热修复和版本发布。
11. 文档生成：创建文档以记录代码、API、组件和项目结构，以协助开发人员和团队成员理解和使用代码。
12. 错误监控和日志记录：使用工具（如Sentry、LogRocket）来监控和记录前端应用程序的错误和日志，以便及时识别和解决问题。
13. 模拟数据和后端协作：在前端工程化中，可以使用模拟数据或与后端API的契约来加速前端开发，即使后端服务尚未准备就绪。
14. 环境配置：管理开发、测试和生产环境的配置变量，以确保应用程序能够在不同环境中正常运行。

前端工程化的目标是提高开发效率、代码质量和可维护性，同时降低错误风险。它有助于前端开发人员更好地应对快速发展的Web技术，并支持多人协作，以构建复杂的Web应用程序。  

## CI/CD概述

CI/CD（Continuous Integration / Continuous Deployment）是一种软件开发实践，旨在通过自动化流程来改进软件交付的速度、质量和可靠性。它分为两个主要部分：持续集成（CI）和持续部署/交付（CD）。
1. 持续集成（Continuous Integration - CI）：
持续集成是一种开发实践，要求开发者将代码频繁地（通常是每日或多次）集成到一个共享的版本控制仓库中，以确保代码的一致性和质量。主要特点包括：
- 自动化构建：每当代码变更时，CI服务器会自动构建应用程序，包括编译、测试和其他必要的构建任务。
- 自动化测试：各种测试，包括单元测试、集成测试和端到端测试，会自动运行以检测代码中的错误。
- 代码质量检查：通过代码规范检查和静态代码分析工具，确保代码符合规范并且质量高。
- 自动化部署至测试环境：将构建后的应用程序部署到测试环境，使开发团队和其他利益相关者可以测试新功能和改进。

2. 持续部署/交付（Continuous Deployment / Continuous Delivery - CD）：
持续部署和交付构建在持续集成的基础上，它们的主要目标是自动将应用程序部署到生产环境。主要特点包括：
- 持续交付（Continuous Delivery - CD）：在持续交付中，应用程序的构建版本经过自动化测试和部署到预生产环境，但最终的发布需要手动批准。这意味着每次构建都是潜在可发布的版本。
- 持续部署（Continuous Deployment - CD）：与持续交付不同，持续部署中的每次构建都自动部署到生产环境，没有人工干预。这需要极高的自动化和信任代码质量的过程。

CI/CD 的优势包括：
- 加速交付：CI/CD自动化流程允许更快速、频繁地交付新功能和改进，缩短了上线周期。
- 提高质量：自动化测试和代码质量检查确保了应用程序的稳定性和可靠性，减少了错误进入生产环境的可能性。
- 减少手动工作：CI/CD工具减少了手动构建、测试和部署的工作，降低了人为错误的风险。
- 提高协作：CI/CD要求团队成员频繁集成代码，有助于解决集成问题并促进团队协作。
- 可重复性：每次构建都是自动化的，可以完全重复，从而降低了环境相关的问题。
- 快速回滚：在出现问题或错误的情况下，快速回滚可以迅速还原到先前稳定的版本，以减少对用户的不利影响。

一些流行的CI/CD工具包括Jenkins、Travis CI、CircleCI、GitLab CI/CD、GitHub Actions等。CI/CD已成为现代软件开发的核心实践，有助于提高团队的生产力，减少错误，加速交付，提高客户满意度。

## GitLab CI/CD

### 概述

GitLab CI/CD 是 GitLab 提供的持续集成和持续部署工具，它集成在 GitLab 开发平台中，旨在帮助团队实现自动化构建、测试和部署，以提高开发流程的效率和质量。以下是关于 GitLab CI/CD 的主要概念：
1. Runner：Runner 是运行CI/CD作业的代理，可以是GitLab Runner（GitLab提供的Runner）或者用户自己托管的Runner。Runner负责接收CI/CD作业请求，并在指定环境中执行它们。
2. Pipeline：Pipeline是CI/CD的核心概念，它代表了一系列的CI/CD作业，通常包括构建、测试、部署等任务。Pipeline可以自动触发，也可以手动创建。
3. Job：Job是Pipeline的组成部分，它表示一个具体的任务，如编译应用程序、运行测试、部署到生产环境等。每个Job可以在不同的Runner上执行。
4. .gitlab-ci.yml文件：CI/CD配置存储在项目中的.gitlab-ci.yml文件中，该文件定义了Pipeline中包括哪些Job，以及它们的执行顺序、条件和变量。

### 为什么在Docker容器内创建Runner

随着项目的增多和技术的迭代，GitLab仓库内存在大量打包环境不同的代码，主要在于node环境的不同，而最初的runner所在服务器node版本为10.0.0，导致需求较高node版本的项目无法使用runner，每次上线都需要手动打包。而升级node版本又影响到大量老项目，基于此，经过技术调研决定在docker内运行Runner，来创建多个node环境的临时容器打包部署。

在Docker容器内运行Runner有以下好处：
- 隔离和一致性：Docker容器提供隔离的运行环境，确保作业不会与主机系统或其他作业产生干扰。这有助于维持一致性，因为容器内的运行环境在不同主机上是相同的。
- 灵活性和可移植性：容器化的Runner非常灵活，可以轻松在不同主机之间移植，而不受主机操作系统的限制。这使得在不同环境中运行CI/CD作业变得更容易。
- 资源隔离：容器化Runner允许你为每个容器分配特定的资源，如CPU、内存和存储。这有助于有效管理资源并防止资源争用问题。
- 简化部署和升级：容器化的Runner可以轻松升级到新版本，而不需要干扰主机系统。这使得版本管理和升级变得更加简单。
- 自包含性：容器中包括了Runner所需的依赖项和运行时环境，这意味着你不必担心主机上的依赖问题。容器是自包含的，便于部署和管理。
- 多种操作系统支持：容器化的Runner可以在同一主机上运行多个容器，每个容器可以使用不同版本的操作系统。这提供了更大的灵活性，以满足不同项目的需求。
- 易于管理：使用Docker Compose或容器编排工具（如Kubernetes），你可以轻松管理多个容器化的Runner实例，同时维护一致性和可扩展性。

### Docker内创建Runner流程

1. 安装 Docker：

    ```bash
    curl -sSL https://get.daocloud.io/docker | sh
    ```
    参考 [https://yeasy.gitbook.io/docker_practice/install](https://yeasy.gitbook.io/docker_practice/install)
    建议安装好后切换国内镜像源，参考 [https://yeasy.gitbook.io/docker_practice/install/mirror](https://yeasy.gitbook.io/docker_practice/install/mirror)

2. 拉取 gitlab-runner image(镜像)

    ```bash
    docker pull gitlab/gitlab-runner:latest
    ```

3. 添加 gitlab-runner container(容器)

   ```bash
   docker run -d --name gitlab-runner --restart always \
    -v /srv/gitlab-runner/config:/etc/gitlab-runner \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gitlab/gitlab-runner:latest
   ```
    我们来解读下这段指令：
    - `docker run`: 这是 Docker 命令，用于启动一个新的容器。
    - `-d`: 这是一个选项，表示在后台（即以守护进程方式）运行容器。
    - `--name gitlab-runner`: 这个选项为容器指定了一个名称，即 "gitlab-runner"，这有助于后续管理容器。
    - `--restart always`: 这个选项指定容器应该在退出时总是重新启动，以确保 GitLab Runner 始终运行。
    - `-v /srv/gitlab-runner/config:/etc/gitlab-runner`: 这个选项用于挂载卷（Volume），将本地文件系统中的 /srv/gitlab-runner/config 目录与容器内的 /etc/gitlab-runner 目录进行关联。这允许容器访问本地配置文件。
    - `-v /var/run/docker.sock:/var/run/docker.sock`: 这个选项也是用于挂载卷，将 Docker 守护进程的 UNIX 套接字（socket）与容器内的 /var/run/docker.sock 进行关联。这是为了使容器能够与宿主机上的 Docker 守护进程进行通信，从而可以在容器内执行 Docker 命令。
    - `gitlab/gitlab-runner:latest`: 这是要运行的容器的镜像名称和标签。在这种情况下，它使用了 GitLab Runner 的官方 Docker 镜像，版本标签为 "latest"，表示使用最新版本。

    综合起来，这个命令启动一个名为 "gitlab-runner" 的 Docker 容器，该容器运行 GitLab Runner 以用于 CI/CD 工作。容器与本地文件系统上的配置文件和 Docker 守护进程进行关联，以实现必要的功能。容器将在退出时自动重新启动，以确保 GitLab Runner 始终可用。

4. 编写 Dockerfile
   
   我们公司前端这边跑 Runner 的需求不只有依赖下载、项目打包和部署，还需要将打包好的项目文件上传到七牛云进行CDN内容分发，所以仅使用 node 官方提供的基础镜像是不够的，需要自己编写 Dockerfile 来定制镜像。

   node14+ 环境 Dockerfile 示例：
   ```bash
    # 基于node14镜像
    FROM node:14
    # 更新包管理器，安装 git imagemagick openssh-client python2.7，构建 python 环境的原因是部分老项目的依赖下载需要 python 环境
    RUN apt-get update && apt-get install -y git imagemagick openssh-client python2.7
    # 设置 python2.7 为默认 Python 版本
    RUN update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
    # 清理之前的 apt-get 命令生成的缓存和临时文件，以减小镜像大小
    RUN apt-get clean
    # 下载并配置七牛云的 qshell 工具，包括下载、解压缩、设置环境变量和配置 qshell 账户信息，并删除压缩包减小镜像大小
    RUN cd /home/ && \
        mkdir qshell && \
        cd qshell/ && \
        wget https://devtools.qiniu.com/qshell-v2.10.0-linux-386.tar.gz && \
        gunzip qshell-v2.10.0-linux-386.tar.gz && \
        tar -xf qshell-v2.10.0-linux-386.tar && \
        export PATH=$PATH:/home/qshell && \
        qshell account v******P 9*****G qiniu@***.com && \
        rm -rf qshell-v2.10.0-linux-386.tar
    # 定义参数 SSH_PRIVATE_KEY，传递 SSH 私钥
    ARG SSH_PRIVATE_KEY
    # 创建 SSH 目录、设置 SSH 私钥、配置已知主机和设置适当的权限
    RUN mkdir -p ~/.ssh \
        && chmod 700 ~/.ssh \
        && echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa \
        && chmod 600 ~/.ssh/id_rsa \
        && ssh-keyscan 1.1.1.1 >> ~/.ssh/known_hosts \
        && ssh-keyscan 1.1.1.1 >> ~/.ssh/known_hosts \
        && chmod 644 ~/.ssh/known_hosts

    # 设置环境变量 PATH，将 qshell 工具的路径添加到容器的环境变量中，以便可以直接运行 qshell 命令
    `ENV PATH=$PATH:/home/qshell`
   ```
   node14 以下环境 Dockerfile 示例，与 14+ 版本的区别仅限于使用的包管理器不同，apt-get 包管理器在较低 node 版本镜像内更新失败：
   ```bash
    FROM node:12-alpine
    # 使用 apk 包管理器
    RUN apk update && apk add bash git imagemagick openssh-client python2
    RUN rm -rf /var/cache/apk/*
    RUN cd /home/ && \
        mkdir qshell && \
        cd qshell/ && \
        wget https://devtools.qiniu.com/qshell-v2.10.0-linux-386.tar.gz && \
        gunzip qshell-v2.10.0-linux-386.tar.gz && \
        tar -xf qshell-v2.10.0-linux-386.tar && \
        export PATH=$PATH:/home/qshell && \
        qshell account v******P 9*****G qiniu@***.com && \
        rm -rf qshell-v2.10.0-linux-386.tar
    ARG SSH_PRIVATE_KEY
    RUN mkdir -p ~/.ssh \
        && chmod 700 ~/.ssh \
        && echo "$SSH_PRIVATE_KEY" | tr -d '\r' > ~/.ssh/id_rsa \
        && chmod 600 ~/.ssh/id_rsa \
        && ssh-keyscan 1.1.1.1 >> ~/.ssh/known_hosts \
        && ssh-keyscan 1.1.1.1 >> ~/.ssh/known_hosts \
        && chmod 644 ~/.ssh/known_hosts
    ENV PATH=$PATH:/home/qshell
   ```
   自定义 Dockerfile 的好处：
    - 自定义镜像：Dockerfile 允许你创建一个完全符合你的应用程序或服务需求的自定义容器镜像。你可以在其中包含特定的软件包、配置文件、环境变量等，以满足应用程序的要求。
    - 可重复性：使用 Dockerfile 可以确保容器的构建过程是可重复的。这样，你可以在不同的环境中（开发、测试、生产）使用相同的 Dockerfile 构建镜像，确保一致性。
    - 版本控制：Dockerfile 可以与版本控制系统（如Git）一起使用，以便将容器镜像的构建过程与应用程序代码一起进行版本控制。这有助于跟踪镜像构建的历史记录，以及在需要时回滚到特定版本。
    - 安全性：自己编写 Dockerfile 可以更好地控制容器中包含的软件和组件，从而提高容器的安全性。你可以选择仅包含必需的组件，减少潜在的安全漏洞。
    - 轻量化：自定义 Dockerfile 可以创建更轻量的容器镜像，因为你可以仅包含应用程序所需的最小依赖项。这有助于减小镜像的大小，提高性能和资源利用率。
    - 自动化构建：Dockerfile 可以与持续集成/持续交付（CI/CD）工作流程集成，自动化容器镜像的构建和部署过程。
    - 社区支持：Docker社区和其他容器技术社区提供了丰富的Dockerfile库和最佳实践，以帮助你开始编写高质量的Dockerfile。

5. 构建 Docker Image

    在编写好 Dockerfile 之后，我们需要将其构建成 docker 镜像。
    在我们的 Dockerfile 中，使用到了一个变量 `SSH_PRIVATE_KEY`，这个变量的值是我们的 SSH 私钥，在构建镜像时，需要将私钥注入到镜像中。
    - 首先 Dockerfile 文件所在目录新建一个名为 `id_rsa` 的 SSH 私钥文件，然后将私钥内容复制到这个文件中。
    - 然后新建一个名为 `.dockerignore` 的文件，将以下内容添加到其中，以确保在构建镜像时 SSH 密钥文件不会被包括在 Docker 构建上下文中：
        ```bash
        .git
        .ssh
        id_rsa
        ```
    - 最后，使用以下命令构建镜像：`docker build -t image-name --build-arg SSH_PRIVATE_KEY="$(cat id_rsa)" .`，将镜像命名为 image-name，并将 `id_rsa` 文件中的私钥内容作为参数`SSH_PRIVATE_KEY`的值构建镜像。

6. 注册 Runner
   
   使用以下命令注册 Runner：`docker exec -it gitlab-runner gitlab-ci-multi-runner register`
     - `exec`: 这是 Docker 命令的子命令，用于在正在运行的容器内执行命令。
     - `-it`: 这是选项，用于指定一个交互式终端（TTY）以及与之关联的标准输入（stdin）和标准输出（stdout）。这使您能够与容器内的命令进行交互。
     - `gitlab-runner`: 这是容器的名称或 ID，表示要在哪个容器中执行命令。
     - `gitlab-ci-multi-runner register`: 这是要在容器内执行的命令。具体来说，这是 GitLab Runner 工具的 register 子命令，它用于注册 GitLab Runner 到 GitLab CI/CD 服务。

    注册时程序会要求你填写相关的信息，这些信息可以从 Gitlab 项目的 管理员 -> CI/CD 页面中找到：

    ![image.png](./images/基于GitLabRunner的CICD部署流程/1.jpg)

     ```bash
        Please enter the gitlab-ci coordinator URL:
        # https://gitlab.***.com/

        Please enter the gitlab-ci token for this runner:
        # token 注册令牌

        Please enter the gitlab-ci description for this runner:
        # Runner 的 description

        Please enter the gitlab-ci tags for this runner (comma separated):
        # Runner 的 tag，该 tag 用于指定项目运行于哪个 Runner 上

        Whether to run untagged builds [true/false]:
        # true

        Please enter the executor: docker, parallels, shell, kubernetes, docker-ssh, ssh, virtualbox, docker+machine, docker-ssh+machine:
        # docker 我们选择docker

        Please enter the default Docker image (e.g. ruby:2.7):
        # image-name 填入构建 Docker image 时填写的 image 名称，即我们通过 Dockerfile 构建的镜像
     ```

    注册成功后，就可以在 GitLab 项目 -> CI/CD -> Runners 页面中看到注册成功的 Runner 了。但是在我们使用刚注册的 Runner 进行项目构建时，会发现执行失败并报错：` ERROR: Job failed: API error (404): repository xxx not found: does not exist or no pull access`，这是因为 Gitlab Runner 会默认从远程拉取 image，而我们的 image 是在本地构建的，所以需要对 gitlab-runner 进行配置，把 pull_policy 设置为 if-not-present 或 never。
     ```bash
        # 进入 gitlab-runner 的 bash 环境
        docker exec -it gitlab-runner bash

        # 编辑 config.toml
        vim /etc/gitlab-runner/config.toml

        # 如果没有安装 vim 或 vi 或 nano，则先进行安装
        apt-get update && apt-get install -y vim

        # 添加下面代码
        pull_policy = "if-not-present"

        # 编辑保存完成后输入 exit 敲回车退出
        exit
     ```

     ![image.png](./images/基于GitLabRunner的CICD部署流程/2.jpg)
    
    这时再使用注册的 Runner 进行项目构建，就发现可以正常执行了。

### 编写部署时需要的 shell 脚本

#### ips.txt 和 ips-t.txt 文件参考

注意文件最后一行要保留一行空行

```bash
web0   1.1.1.1
web1  1.1.1.1
# 空行
```

#### 开发环境部署脚本参考
```bash
#!/bin/bash -

home=$(
  cd $(dirname $0)
  pwd
)
# cd $home

## 项目部署路径
root_path=$(cat ./vite.config.js | grep "testRootPath:" | cut -d "'" -f2 | cut -d "'" -f1)
if [[ "__ONLINE_ROOT_PATH__" == "${root_path}" || "" == "${root_path}" ]]; then
  echo " ************************ "
  echo " 请配置 vite.config.ts 中的 testRootPath "
  echo " ************************ "
  exit
fi
## 遍历文件
function walk(){
    if [ -f $1 ]; then
        dep $1
        return
    fi
    mkdir_dir ${1##*dist/}
    for item in `ls $1`
    do
        local file=$1"/"$item
        if [ -d $file ]
        then
            walk $file
        else
            ## 遍历时不同步入口index.html
            if [ "$file" == "dist/index.html" ];
            then
                continue
            fi
            dep ${file}
        fi
    done
}

## 创建目录
function mkdir_dir(){
    root_dir=${root_path}/$1
    arr=(${root_dir//\// }) ##表示'/'替换为' '空格
    local dir=""
    for item in ${arr[@]}
    do
    if [ "$item" == "dist" ];
    then
        continue
    fi
        dir=$dir/$item
        while read path ip ;
        do
            # echo "ssh root@$ip '[ -d ${dir} ] && echo ok || mkdir -p ${dir}'"
            ssh root@$ip "[ -d ${dir} ] && echo ok || mkdir -p ${dir}" < /dev/null
        done < ./scripts/deploy/ips-t.txt
    done
}

## 同步文件
function dep(){
    file=$1
    while read path ip ;
    do
        echo "scp -p -r  $file   root@$ip:$root_path/${file##*dist/}"
        scp -p -r  $file   root@${ip}:${root_path}/${file##*dist/} < /dev/null
    done < ./scripts/deploy/ips-t.txt
}

echo "=================开始发布================"

dir=${1%*/}
walk $dir
## 最后同步入口文件
dep ${dir}/index.html

echo "=================完成发布================"
```
需要部署的服务器 ip 保存在同目录下的 `ips.txt` 文件中，执行指令 `bash ./scripts/deploy/deploy-t.sh dist`，需要将 `dist` 目录作为参数传递给脚本。

#### 生产环境部署脚本参考
```bash
    #!/bin/bash -
    home=$(
    cd $(dirname $0)
    pwd
    )
    # cd $home

    # 项目部署路径
    root_path=$(cat ./vite.config.js | grep "onlineRootPath:" | cut -d "'" -f2 | cut -d "'" -f1) # 注意这里被 "" 包裹的引号是单引号还是双引号要和 onlineRootPath 的 value 对应
    if [[ "__ONLINE_ROOT_PATH__" == "${root_path}" || "" == "${root_path}" ]]; then
    echo " ************************ "
    echo " 请配置 vite.config.js 中的 onlineRootPath "
    echo " ************************ "
    exit
    fi
    echo ${root_path}
    ## 遍历文件
    function walk(){
        if [ -f $1 ]; then
            dep $1
            return
        fi
        mkdir_dir ${1##*dist/}
        for item in `ls $1`
        do
            local file=$1"/"$item
            if [ -d $file ]
            then
                walk $file
            else
                ## 遍历时不同步入口index.html，入口文件在最后同步
                if [ "$file" == "dist/index.html" ];
                then
                    continue
                fi
                dep ${file}
            fi
        done
    }

    ## 创建目录
    function mkdir_dir(){
        root_dir=${root_path}/$1
        arr=(${root_dir//\// }) ##表示'/'替换为' '空格
        local dir=""
        for item in ${arr[@]}
        do
        if [ "$item" == "dist" ];
        then
            continue
        fi
            dir=$dir/$item
            while read path ip ;
            do
                # echo "ssh root@$ip '[ -d ${dir} ] && echo ok || mkdir -p ${dir}'"
                ssh root@$ip "[ -d ${dir} ] && echo ok || mkdir -p ${dir}" < /dev/null
            done < ./scripts/deploy/ips.txt
        done
    }

    ## 同步文件
    function dep(){
        file=$1
        while read path ip ;
        do
            echo "scp -p -r  $file   root@$ip:$root_path/${file##*dist/}"
            scp -p -r  $file   root@${ip}:${root_path}/${file##*dist/} < /dev/null
        done < ./scripts/deploy/ips.txt
    }

    echo "=================开始发布================"

    dir=${1%*/}
    walk $dir
    ## 最后同步入口文件
    dep ${dir}/index.html

    echo "=================完成发布================"
```
需要部署的服务器 ip 保存在同目录下的 `ips.txt` 文件中，执行指令 `bash ./scripts/deploy/deploy.sh dist`，需要将 `dist` 目录作为参数传递给脚本。

#### 同步七牛云CDN脚本参考
```bash
#! /bin/bash

home=$(cd $(dirname $0);pwd)
count=0

function read_dir(){
    for file in `ls $1`
    do
        if [ -d $1"/"$file ]
        then
            read_dir $1"/"$file
        else
            if [ "$1/$file" == "dist/index.html" ];
            then
                continue
            fi
            count=`expr $count + 1`
            ## 剔除 dsit
            short=${1##*dist/}
            qshell rput yd-common $short"/"$file $1"/"$file --overwrite
            echo "https://***.***.com/$short/$file"
        fi
    done
}

echo "============ 开始同步CDN ==========="

name=${1%*/}
read_dir $name
echo "count ===== $count"

echo "============ 完成同步CDN ==========="
```
执行指令 `bash ./scripts/cdn/qiniu.sh dist`，同样需要将 `dist` 目录作为参数传递给脚本。

### 编写 package.json 的 "scripts" 配置项
以 vite 脚手架构建的 vue3 项目举例
```bash
"build:t": "vite build --mode development", # 开发环境打包
"build:p": "vite build --mode production", # 生产环境打包
"qiniu": "bash ./scripts/cdn/qiniu.sh dist", # 生产环境部署时执行七牛云 CDN 上传脚本
"deploy": "bash ./scripts/deploy/deploy.sh dist", # 生产环境部署时执行的生产环境部署脚本
"deploy:t": "bash ./scripts/deploy/deploy-t.sh dist", # 开发环境部署时执行的开发环境部署脚本，使用两个脚本文件而不是一个脚本文件内进行判断是为了之后更好的扩展
"deploy_index": "bash ./scripts/deploy/deploy.sh dist/index.html", # 生产环境快速回滚，该逻辑的实现基于 dist 内文件上传到七牛CDN和后台后都会保留的逻辑，根据CDN链接或引用文件名获取对应的文件，只需重新部署 index.html 文件即可
```

### 编写 .gitlab-ci.yml 文件

`.gitlab-ci.yml` 文件是GitLab CI/CD的配置文件，用于定义CI/CD流水线的构建、测试和部署过程。该文件必须位于GitLab项目的根目录，并包含有关如何构建和交付项目的信息。
编写参考：[https://docs.gitlab.com/ee/ci/yaml/?query=gitlab-ci.yml](https://docs.gitlab.com/ee/ci/yaml/?query=gitlab-ci.yml)

举例，该配置将master分支代码打包部署到生产环境，将release开头分支代码打包部署到测试地址，具体部署到哪里由根目录 *.config.* 文件内配置的路径决定(onlineRootPath、testRootPath)：
```bash
# 定义 stages
stages:
    - install
    - build
    - deploy
    - deploy_index

# 每个job之前运行的命令
before_script:
    - whoami
    - pwd
    - node -v
    - npm -v

# 在不同分支可以使用共同的缓存需要关闭受保护分支缓存：在 GitLab中找到项目，左下角设置 - CI/CD - 流水线通用设置 - 清除"为受保护的分支使用单独的缓存"复选框 - 保存修改
cache:
    key: ${CI_PROJECT_NAME} # 使用项目名称做为缓存名称，方便在不同分支使用该缓存
    paths:
        - node_modules/ # 为node_modules增加缓存

install:
    stage: install
    # 使用的 runner，根据项目所需不同 node 版本去选择，目前可选 node16 node14 node12 node10
    # 使用 node16 和 node14 需开启共享Runner：在 GitLab中找到项目，左下角设置 - CI/CD - Runner - 打开为该项目启用共享Runner
    # 不配置 tags 随机使用 node12 node10
    tags:
        - d-node16
    script:
        - echo "======= 开始 安装依赖 ======="
        - echo "${CI_PROJECT_NAME} ${CI_COMMIT_BRANCH}" # 打印核对下项目名称和分支名称
        - npm install
        - echo "======= 结束 安装依赖 ======="
    only: # 只有在 master 分支和 release 开头分支才会展示可执行
        - master
        - /^release/
    when: manual # 手动执行

build:
    stage: build
    tags:
        - d-node16
    script:
        - echo "======= 开始 构建 ======="
        - | # 根据不同分支执行不同逻辑
        if [[ "${CI_COMMIT_BRANCH}" == "master" ]]; then
            npm run build:p
        fi
        if [[ "${CI_COMMIT_BRANCH}" =~ ^release ]]; then
            npm run build:t
        fi
        - echo "======= 结束 构建 ======="
    artifacts:
        expire_in: 1 week # 生成文件保存周期
        name: '${CI_JOB_NAME}_${CI_COMMIT_REF_NAME}'
        paths:
        - dist # 编译后生成的文件夹名
    only:
        - master
        - /^release/

deploy:
    stage: deploy
    tags:
        - d-node16
    script:
        - echo "======= 开始 部署 ======="
        - |
        if [[ "${CI_COMMIT_BRANCH}" == "master" ]]; then
            npm run qiniu
            npm run deploy
        fi
        if [[ "${CI_COMMIT_BRANCH}" =~ ^release ]]; then
            npm run deploy:t
        fi
        - echo "======= 完成 部署 ======="
    only:
        - master
        - /^release/

deploy_index:
    stage: deploy_index
    script:
        - echo "======= 开始 部署 index.html ======="
        - npm run deploy_index
        - echo "======= 完成 部署 index.html ======="
    when: manual
    only:
        - master
```

### 更新 Runner 使用的镜像

我们在需要新增部署的服务器时需要更新镜像，以便在镜像生成的容器内有访问新服务器的权限，这时候我们需要：

1. 删除或重命名要修改的镜像
2. 更新对应的 Dockerfile 文件
3. 镜像名称保持不变并重新构建镜像

在之后对应 Runner 执行时就会根据我们新生成的镜像文件创建临时容器了。
