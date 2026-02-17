# CNB 起步
** 基于云原生构建的远程开发解决方案，支持通过 WebIDE、VSCode 客户端、Cursor 客户端连接远程开发环境进行远程开发。
云原生开发具有以下特点：**

- 声明式：基于 Docker 生态，Dockerfile 声明开发环境，与代码同源管理
- 快速启动：即使是超大仓库，也可以数秒准备好代码和环境
- 按需使用：按需获取开发资源，闲时快速回收，避免资源浪费



## 1. .cnb.yml 文件配置
```yml
# 这里是一个示例 .cnb.yml
  
$:
  vscode:
    - docker:
        # image: ubuntu:25.04
        # build: .ide/Dockerfile
        # image: docker.cnb.cool/ittalksite/ubuntu:u24-dev
        # image: docker.cnb.cool/ittalksite/ubuntu:u24-idea
        image: docker.cnb.cool/ittalksite/ubuntu:u24-idea2532
      services:
        - vscode
        - docker
      
      env:
        CNB_WELCOME_CMD: |
          echo "Welcome..."
          # docker compose up -d

      # 销毁之前保存文件
      endStages:
        - name: "save files"
          script: |
            # docker compose down -v
            git add . 
            git commit -m "自动代码同步-sync code $(date "+%Y-%m-%d %H:%M:%S")"
            git push
        
# 匹配main分支
main:
  # 触发事件为push事件
  push:
    # 这里是阶段任务示例，可配置任意多个阶段，这里以一个阶段为例
    - name: "your first CNB building"
      stages:
        # 使用流水线输出你的构建配置数据
        - name: echo your build data
          script: echo ${CNB_BUILD_USER} 通过 ${CNB_BRANCH} 分支触发了 ${CNB_EVENT} 事件
  
  # 更多资料见 https://docs.cnb.cool/build/quick-start.html
```