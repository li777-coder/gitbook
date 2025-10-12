# 为什么要学习GIT

Git，作为一款分布式版本控制系统（DVCS），其核心价值在于它能够**详尽地存储并管理项目代码的每一次版本演变**。这使得项目开发过程中的版本状态始终处于可控范围之内，为开发者提供了坚实的代码安全保障。具体而言：

*   **版本回溯与问题修复：** 当代码在某一修改后不幸引入了新的错误（BUG），我们可以通过Git强大的历史回溯能力，迅速定位并恢复到功能正常时的任一历史版本。这极大地降低了开发风险，确保了项目的快速故障恢复能力。
*   **有序的版本进化：** 通过遵循良好的提交（commits）规范，即在每次代码变更后清晰地记录修改内容和目的，我们可以构建起一个逻辑清晰、易于追踪的版本进化路径。这不仅有助于未来问题的诊断，更能规范化项目的长期迭代与演进。


同时，Git的另一大特色在于其**卓越的团队协作能力**。在现代软件开发，特别是像车载电控系统这样复杂的嵌入式项目中，代码工作往往不是由一个人单兵作战，而是由多名电控工程师（例如2-3人甚至更多）并行协作完成。在这种多方参与的模式下，传统的代码管理方式极易引发一系列挑战：

*   **代码冲突难题：** 当多位工程师不约而同地修改了同一处代码，并在尝试合并各自工作成果时，极易发生代码冲突。这会中断开发流程，需要耗费额外的时间和精力去协调和解决。
*   **版本差异与历史保留：** 随着项目进展，不同开发者的本地代码库版本可能出现显著差异。如何在不丢失任何一方修改历史记录的前提下，实现高效、有序且准确的代码合并，是另一个亟待解决的复杂问题。

Git针对这些痛点提供了成熟而高效的解决方案。它允许每位开发者基于主线代码（通常是称为`main`或`master`的分支）创建独立的**开发分支（`branch`）**。在个人分支上的所有修改，都只影响该分支，而不会直接干扰到其他分支或主线代码的稳定性。开发者可以在个人分支上自由开发、实验和测试，待功能完善并经过充分验证后，再将代码合并回主线。Git会在合并过程中自动进行冲突检测，并提供强大的工具协助解决这些冲突，从而有效地保障了团队协作的顺畅与代码集成的高效性。

# 在苍穹内部的Git使用

在苍穹内部的项目开发中，我们对Git的学习和掌握持务实高效的态度。我们的目标不要求成员全面精通Git的所有高级特性和复杂操作，而是强调其核心功能的应用，确保能够有效解决上文所提及的，关于**代码版本控制和团队协作**的关键问题。

为了在苍穹内部更好地利用Git，以下是一些基本的使用原则和工作流程建议：

1.  **理解分支策略：** 掌握如何创建、切换和合并分支。通常我们会采用简洁的分支模型，例如`main`（或`master`）作为稳定发布分支，以及以**个人名字命名**的开发分支（例如`zhang_san`、`li_si`）来承载个人开发工作。每个**个人分支**承载了该开发者进行的各种功能开发或bug修复。
2.  **规范提交信息（Commit Messages）：** 每一次提交都应包含清晰、简洁且有意义的提交信息。这不仅便于他人理解你的修改意图，也是未来追溯问题、理解代码演进的关键。建议遵循一定的提交信息格式（例如，包含类型、范围、描述等）。
3.  **频繁提交与同步：** 鼓励开发者频繁地提交（`git commit`）个人工作进度，并定期将本地修改推送到远程仓库（`git push`），同时从远程拉取最新代码（`git pull`），保持与团队的同步。这有助于及时发现和解决潜在冲突，避免代码差异过大。
4.  **熟练掌握合并（Merge）与变基（Rebase）：** 理解`git merge`和`git rebase`的区别和适用场景，特别是在保持提交历史整洁方面。在分支合并时，尤其是在将个人分支合并到`main`分支之前，应进行彻底的冲突检查和解决。善用图形化工具（如GitKraken、VS Code内置Git工具或Sourcetree）来辅助冲突解决。
5.  **代码审查（Code Review）：** 将Git与代码审查流程相结合，通过Pull Request（或Merge Request）的机制，让团队成员对即将合并到主线的代码进行审查，确保代码质量、发现潜在问题并分享知识。

## Git常用命令教程

为帮助大家快速掌握Git的实践操作，我们整理了一系列核心命令及其使用场景。你可以在Ubuntu 24.04的zsh终端中直接运行这些命令。

### 1. 初始化与克隆仓库

*   **`git init`**：**在当前目录初始化一个新的Git仓库。**当你有一个全新的项目，想要开始用Git管理时使用。
    ```zsh
    # 进入你的项目目录
    cd my_new_vehicle_project
    # 初始化Git仓库
    git init
    ```
*   **`git clone <url>`**：**克隆一个远程Git仓库到本地。**当你开始一个新项目或加入一个现有团队时，通常通过克隆远程仓库来获取所有代码和历史记录。
    ```zsh
    # 克隆一个远程仓库到当前目录下的一个新文件夹
    git clone https://gitserver.example.com/vehicle-software/ecu_firmware.git
    ```

### 2. 查看状态与添加文件

*   **`git status`**：**查看工作区和暂存区的状态。**这个命令会告诉你哪些文件被修改了、哪些文件已暂存、哪些是新文件但未被Git追踪。这是你最常用的命令之一，用来了解当前仓库的“健康状况”。
    ```zsh
    git status
    ```    输出示例：
    ```
    On branch main
    Your branch is up to date with 'origin/main'.

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
        modified:   src/main.c

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
        docs/requirements.txt
    ```
    这表示`src/main.c`被修改但未暂存，`docs/requirements.txt`是新文件未被追踪。

*   **`git add <file_name>`**：**将文件添加到暂存区。**暂存区是提交前的“缓冲区”。只有添加到暂存区的文件，才会被包含在下一次提交中。
    ```zsh
    # 添加单个文件到暂存区
    git add src/main.c
    # 添加docs目录下的所有文件
    git add docs/
    # 添加所有修改过的或新创建的文件到暂存区 (慎用，确保你清楚添加了哪些文件)
    git add .
    ```

### 3. 提交更改

*   **`git commit -m "Your commit message"`**：**将暂存区的更改提交到本地仓库。**每次提交都是一次“快照”，代表了项目的一个稳定状态。`-m`参数后面是本次提交的简短描述信息，非常重要！
    ```zsh
    # 提交所有暂存区的更改
    git commit -m "feat: 添加发动机转速控制逻辑"
    ```
    一个好的提交信息应该清晰说明：
    *   **类型：** (feat: 新功能, fix: 修复bug, docs: 文档, style: 格式, chore: 构建/工具变更等)
    *   **描述：** 详细说明本次提交做了什么。

### 4. 分支管理

*   **`git branch`**：**列出所有本地分支。**当前分支会有一个星号`*`标记。
    ```zsh
    git branch
    ```
*   **`git branch <your_name_branch>`**：**创建一个以你的名字命名的新分支。**
    ```zsh
    # 例如，你的名字是张三，创建一个名为zhang_san的分支
    git branch zhang_san
    ```
*   **`git checkout <your_name_branch>`**：**切换到指定分支。**切换后，你的工作区会更新为该分支的代码版本。
    ```zsh
    # 切换到zhang_san分支
    git checkout zhang_san
    ```
*   **`git checkout -b <your_name_branch>`**：**创建并立即切换到新分支。**这是上面两个命令的快捷方式。
    ```zsh
    # 创建并切换到li_si分支
    git checkout -b li_si
    ```*   **`git branch -d <your_name_branch>`**：**删除一个本地分支。**在合并了分支后，通常会删除不再需要的分支。
    ```zsh
    # 删除名为zhang_san的分支 (前提是该分支已合并到其他分支)
    git branch -d zhang_san
    # 如果分支未合并，但确定要删除，可以使用 -D 强制删除
    # git branch -D zhang_san
    ```

### 5. 远程操作

*   **`git remote -v`**：**查看已配置的远程仓库。**通常你会看到`origin`，它指向你的主远程仓库。
    ```zsh
    git remote -v
    ```
    输出示例可能如下：
    ```
    origin  https://gitserver.example.com/vehicle-software/ecu_firmware.git (fetch)
    origin  https://gitserver.example.com/vehicle-software/ecu_firmware.git (push)
    ```

*   **`git push origin <your_name_branch>`**：**将本地分支的提交推送到远程仓库。**这是与团队成员分享你的代码变更的方式。
    ```zsh
    # 将当前本地分支的更改推送到远程同名分支
    git push origin zhang_san
    # 首次推送新分支时，可能需要设置上游分支，方便后续直接git push
    git push -u origin zhang_san
    ```
*   **`git pull origin <branch_name>`**：**从远程仓库拉取（获取并合并）最新代码。**在开始工作前或定期拉取，以确保你的本地仓库与远程仓库保持同步。
    ```zsh
    # 拉取远程main分支的最新代码并合并到当前本地分支 (例如，当你在zhang_san分支工作，需要获取main的更新)
    git pull origin main
    ```

*   **修改 `origin` 远程仓库的 URL**

    有时候，远程仓库的地址会发生变化（例如，项目迁移到新的Git服务器，或者URL拼写错误）。这时，你需要更新本地仓库中 `origin` 对应的URL。

    **`git remote set-url origin <new_url>`**：**更新名为 `origin` 的远程仓库的URL。**

    **操作步骤：**
    1.  **首先，查看当前的 `origin` URL**，确认其旧地址。
        ```zsh
        git remote -v
        ```
    2.  **使用 `set-url` 命令修改 `origin` 的URL。** 将 `https://new-gitserver.example.com/vehicle-software/ecu_firmware.git` 替换为你的新远程仓库地址。
        ```zsh
        git remote set-url origin https://new-gitserver.example.com/vehicle-software/ecu_firmware.git
        ```
    3.  **再次查看 `origin` URL**，确认修改已生效。
        ```zsh
        git remote -v
        ```
        输出应该显示新的URL：
        ```
        origin  https://new-gitserver.example.com/vehicle-software/ecu_firmware.git (fetch)
        origin  https://new-gitserver.example.com/vehicle-software/ecu_firmware.git (push)
        ```
        现在，你所有的 `git push` 和 `git pull` 操作都会指向新的远程仓库地址了。

### 6. 合并分支

*   **`git merge <branch_to_merge>`**：**将指定分支的更改合并到当前所在分支。**
    ```zsh
    # 假设你当前在main分支（已通过git checkout main切换），你想把zhang_san分支的更改合并进来
    git checkout main
    git merge zhang_san
    ```
    如果发生冲突，Git会提示你手动解决冲突。解决后，需要`git add`冲突文件，然后`git commit`完成合并。

### 7. 更新个人分支（Rebase）

在开发过程中，`main`分支可能会有新的提交。为了让你的个人工作分支（如`zhang_san`）保持最新，并且拥有更线性、整洁的提交历史，我们可以使用 `git rebase`。

*   **`git rebase <base_branch>`**：**将当前分支的更改“变基”到指定的基础分支上。**
    这会将你的个人分支上的所有提交“剪切”下来，然后将它们重新“粘贴”到`base_branch`的最新提交之后。结果是，你的个人分支历史看起来就像是从`base_branch`的最新点开始的，没有分叉合并的痕迹，历史更清晰。

    **使用场景：** 当你在`zhang_san`分支上工作时，`main`分支有了新的更新，你想把这些更新整合到你的`zhang_san`分支，并且希望保持提交历史的简洁线性。

    **操作步骤：**
    1.  **保存你当前分支的本地修改**（如果有的话）。确保工作区干净，或者先`git add .`和`git commit`。
        ```zsh
        git status # 确认工作区干净
        ```
    2.  **切换到你的个人分支**（例如`zhang_san`）。
        ```zsh
        git checkout zhang_san
        ```
    3.  **拉取远程 `main` 分支的最新代码**，确保本地的 `main` 分支是最新的。
        ```zsh
        git pull origin main:main # 拉取origin/main到本地main分支
        ```
        或者先切换到 `main`，拉取，再切换回你的分支：
        ```zsh
        git checkout main
        git pull origin main
        git checkout zhang_san
        ```
    4.  **执行 `rebase` 操作**，将 `zhang_san` 分支变基到 `main` 分支上。
        ```zsh
        git rebase main
        ```
        此时，Git会将`zhang_san`分支上的所有提交逐个应用到`main`分支的最新状态之上。
    5.  **解决可能发生的冲突**：如果`zhang_san`分支的修改与`main`分支的更新有冲突，Git会在每个冲突点暂停，你需要手动解决。
        *   编辑冲突文件，解决冲突。
        *   `git add <冲突文件>`
        *   `git rebase --continue` (继续rebase过程)
        *   如果需要中止rebase，可以使用 `git rebase --abort`。
    6.  **Rebase完成后，如果之前曾将此分支推送到远程（`git push origin zhang_san`），那么你现在需要强制推送**，因为rebase改变了分支历史。**请务必小心强制推送，只在你确信当前分支不是共享分支，或已与团队成员沟通好时才使用。**
        ```zsh
        git push --force-with-lease origin zhang_san
        ```
        `--force-with-lease` 相比 `--force` 更安全，它会检查远程分支在你上次拉取后是否被其他人更新过，避免覆盖他人的工作。

    **何时慎用或避免 `rebase`：**
    *   **不要对已经推送到远程的、且有其他人在其之上工作的共享分支执行 `rebase`。** `rebase` 会改写历史，强行推送会给团队成员带来回滚和同步的巨大麻烦。个人开发分支在未合并到`main`并通过Code Review之前，通常是安全的。

### 8. 查看历史记录

*   **`git log`**：**查看提交历史。**按时间顺序显示所有提交记录，包括提交者、日期、提交信息等。
    ```zsh
    git log
    # 以更简洁的单行格式显示，配合图形化工具理解分支走向
    git log --oneline --graph --decorate
    ```

---

通过有效掌握和实践这些Git命令，苍穹的团队成员将能够更高效、更协作地进行代码开发，保障车载电控项目的质量和进度。记住，熟能生巧，多使用、多练习是掌握Git的关键。遇到问题时，不要犹豫查阅Git官方文档或向团队中的Git专家寻求帮助。
