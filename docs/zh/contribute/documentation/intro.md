# 维基文档维护

如果你想参与文档的维护，那么你可以按照以下步骤进行：

1. **复刻项目仓库到本地**
    - 访问项目的 [GitHub 仓库页面](https://github.com/CatIsNotFound/CContainerKit-Wiki)。
    - 点击页面右上角的 `Fork` 按钮，将项目复刻到你自己的 GitHub 账户下。
    - 复刻完成后，在你自己账户下的项目仓库页面，点击 `Code` 按钮，复制仓库的 HTTPS 或 SSH 链接。
    - 打开本地终端，使用 `git clone` 命令将仓库克隆到本地，示例命令如下：
      ```bash
      git clone https://github.com/<你的用户名>/CContainerKit-Wiki
      ```

2. **创建一个新的分支，用于修改文档**
    - 进入克隆到本地的项目目录，使用以下命令切换到主分支（通常是 `main` 或 `master`）：
      ```bash
      git checkout main
      ```
    - 拉取最新的代码：
      ```bash
      git pull origin main
      ```
    - 创建并切换到一个新的分支，分支名建议使用有描述性的名称，例如 `doc/update-readme`，示例命令如下：
      ```bash
      git checkout -b <新分支名>
      ```

3. **进行文档编写和修改**
    - 使用你喜欢的编辑器打开项目文档，根据需求进行编写或修改。
    - 编写时请确保文档内容准确、清晰，格式统一。
    - 修改完成后，检查文档是否存在拼写错误或语法错误。

4. **提交代码并创建一个 Pull Request**
    - 在终端中，使用以下命令将修改的文件添加到暂存区：
      ```bash
      git add .
      ```
    - 使用 `git commit` 命令提交代码，提交信息应简洁明了地描述本次文档修改的内容，示例命令如下：
      ```bash
      git commit -m "更新 README 文档"
      ```
    - 使用 `git push` 命令将本地分支推送到你自己账户下的 GitHub 仓库，示例命令如下：
      ```bash
      git push origin <新分支名>
      ```
    - 回到你自己账户下的 GitHub 仓库页面，点击页面上的 `Compare & pull request` 按钮，填写 Pull Request 的标题和描述，然后点击 `Create pull request` 按钮。

5. **等待项目维护者进行代码审查和合并**
    - 项目维护者会收到你的 Pull Request 请求，并对你的文档修改进行审查。
    - 审查过程中，维护者可能会提出一些修改意见，你需要根据意见对文档进行修改，然后再次提交。
    - 当修改通过审查后，维护者会将你的修改合并到主项目中。