## Git从入门到精简——高见龙（笔记）

### 1.Git中快照：

Git快照指的是在Git版本控制系统中创建的文件系统状态的副本或记录。当你在Git中进行提交时，Git会创建一个文件系统的快照，并将这个快照保存为一个提交对象。这个快照包含了那个时刻所有文件的状态和位置信息。每次提交操作都会创建一个新的快照，记录了文件系统在那个时间点的状态，而不是记录文件的变化。Git使用这些快照来跟踪文件的变化，使得你可以轻松地查看文件在不同提交之间的变化，并在需要时进行版本回退或合并操作。

### 2.git add . 与 git add -all

 前者从本目录开始加入暂存区，后者不管在哪全部加入。

### 3.git commit：

将暂存区的内容加入到存储库。-m 后面说明这次commit做了什么事情。

### 4.`git commit`

只会提交暂存区中的内容，并且创建一个新的提交记录。如果某些更改没有被`git add`添加到暂存区，这些更改将不会被包含在下一次提交中。

### 5.工作区、暂存区、存储库

**工作区（Working Directory）：** 这是你实际编辑和修改文件的地方。工作区是你电脑上的文件目录，包含了项目的实际文件。在工作区中你可以修改、添加、删除文件。

**暂存区（Staging Area / Index）：** 暂存区是一个介于工作区和存储库之间的区域。在这里，你可以使用 `git add` 命令将工作区中的特定更改添加到暂存区，准备将这些更改作为一个单独的快照提交到存储库。

**存储库（Repository）：** 存储库（也称为仓库或库）是包含项目完整历史记录的地方，它包含了所有提交的快照。这个存储库通常包含了所有的文件、提交历史以及与项目相关的元数据。

### 6.`git commit -a` 

命令是一个便捷的方式来将所有**已经被 Git 追踪**（tracked）的文件的更改一次性提交到存储库。这个命令结合了两个步骤：`git add` 和 `git commit`。

具体来说，`-a` 标志告诉 Git 在提交之前先将所有已经被 Git 追踪并且被修改过的文件自动添加到暂存区，然后执行提交。这个命令不会添加新文件或未被 Git 追踪的文件，只会处理已经在 Git 跟踪下的文件。

### 7.`git checkout` 命令

在 Git 中有多种用途之一就是可以用来“挽救”文件，即恢复误操作或者丢失更改的文件。你可以使用以下命令将工作区中的文件恢复到最近一次提交的状态：这个命令会放弃对指定文件的修改并将其还原为最近一次提交的状态。如果你在修改文件后还没有执行 `git add` 将其加入暂存区，或者文件尚未提交，这个命令会使文件回到最近一次提交的状态，丢弃你在工作区所做的修改。

如果你已经执行了 `git add` 将文件添加到暂存区，但还没有提交，可以使用 `git reset HEAD <file>` 将文件从暂存区撤回到工作区，然后再使用 `git checkout -- <file>` 恢复到最近一次提交的状态。

### 8.`git reset --mixed <commit>` 

会将 暂存区会被重置到指定提交的状态，但工作区的文件并不会自动更改为指定提交中的状态。（工作区不动，动暂存区和git仓库）

```
git checkout .
```

它会将***工作区***中所有已修改但尚未添加到暂存区的文件恢复到最后一次提交的状态。

git reset --soft: 将git提交目录更新为指定状态，工作区和暂存区都不动。 

git reset --hard 工作区、暂存区、git仓库都动。

### 9.reset恢复

如果reset但是想恢复，直接reset到reset之前的那个commit就可以，如果忘了commit id可以用reflog查看。

### 10.merge会commit一次

通常merge的时候会commit一次，记录当前合并的内容。当你试图将一个分支合并到另一个分支时，如果被合并的分支的所有提交都是目标分支的直接祖先，Git 就会使用快速转移模式。在这种情况下，Git 会简单地将目标分支的指针直接移动到被合并分支的最新提交上，而不会创建新的合并提交。

### 11.`git rebase` 和 `git merge` 

是 Git 中两种不同的合并分支的方式，它们的主要区别在于合并后的提交历史展现方式和合并的实际操作。

#### 1. Git Merge（合并）

- **操作方式：** `git merge` 将两个分支的历史记录合并到一起。在合并时，会创建一个新的合并提交，将两个分支的更改合并在一起，这个合并提交包含了被合并分支的所有历史记录。
- **提交历史：** 合并提交的产生会在历史记录中保留被合并分支的完整信息，使得整个合并过程清晰可见。
- **优点：** 合并的历史记录相对简单，合并操作不会改变提交历史，能够保留分支的完整性。

#### 2. Git Rebase（变基）

- **操作方式：** `git rebase` 会将当前分支的提交移动到目标分支的顶部，并且在这个过程中会逐个应用每个提交，形成一系列的新提交。
- **提交历史：** 通过变基，提交历史可以更加线性和清晰。它可以将你的提交应用到目标分支的最新提交上，形成一个干净的、直线式的提交历史。
- **优点：** 产生的提交历史更加整洁和清晰，不会创建额外的合并提交，使得项目历史更易于理解和管理。

### 12.如果现在的工作做到一半，需要切换到另一个分支去完成其他任务。

有两种方案

方案一、先commit未完成的，等其他工作完成后，再reset HEAD^ reset到待完成分支最后一次提交之前。

方案二、stash 先将修改存储起来。

`git stash` 是一个用于临时保存工作目录和暂存区改动的命令。它可以暂时将你的当前工作状态存储起来，让你可以切换到其他分支或者进行其他操作，而不必提交未完成的更改。

#### 使用方式：

1. **存储当前工作目录和暂存区的更改：**

   ```bash
   git stash
   ```

   这个命令会将未提交的工作目录和暂存区的更改保存在一个栈中。

2. **查看存储的工作目录状态：**

   ```bash
   git stash list
   ```

   这个命令用来查看存储在栈中的所有 stash 记录。

3. **恢复 stash 的更改：**

   - **应用最新的 stash：**

     ```bash
     git stash apply
     ```

     这个命令会应用并保留最新的 stash，但不会将其从 stash 栈中删除。

   - **应用特定的 stash：**

     ```bash
     git stash apply stash@{n}
     ```

     这个命令会应用指定编号 `stash@{n}` 的 stash 记录，但不会将其从栈中删除。

4. **删除 stash 记录：**

   - **删除最新的 stash：**

     ```bash
     git stash drop
     ```

     这个命令会永久删除最新的 stash 记录。

   - **删除特定的 stash：**

     ```bash
     git stash drop stash@{n}
     ```

     这个命令会永久删除指定编号 `stash@{n}` 的 stash 记录。

5. **恢复并删除 stash 记录：**

   - **应用并删除最新的 stash：**

     ```bash
     git stash pop
     ```

     这个命令会应用并删除最新的 stash 记录。

   - **应用并删除特定的 stash：**

     ```bash
     git stash pop stash@{n}
     ```

     这个命令会应用并删除指定编号 `stash@{n}` 的 stash 记录。

`git stash` 是一个非常有用的功能，可以在你需要暂时切换分支或者进行其他操作时，暂存当前未完成的更改，以便稍后再继续工作。

### 13.`git push -u` 命令

用于将本地分支推送到远程仓库，并在推送时将本地分支与远程分支进行关联。

- `-u` 或者 `--set-upstream` 参数用来在推送同时设置本地分支与远程分支的关联。这个关联指定了本地分支应该与哪个远程分支进行交互，之后在使用 `git pull` 或 `git push` 时，Git 将会自动识别这个关联。

举个例子，假设你有一个本地分支叫做 `feature`，你想将它推送到名为 `origin` 的远程仓库的 `main` 分支上，并在推送时建立起这两者的关联，你可以执行：

```
bashCopy code
git push -u origin feature:main
```

这会将本地的 `feature` 分支推送到远程的 `main` 分支，并通过 `-u` 参数将本地的 `feature` 分支与远程的 `main` 分支建立关联。

之后，当你在 `feature` 分支上进行修改后，只需执行 `git push` 或 `git pull`，Git 将会自动知道你想要推送到哪个远程分支，或者从哪个远程分支拉取。

### 14.git push origin master == git push origin master:master 

因为要在本地master分支推上去后，在服务器上更新master的分支进度。如果不存在该分支，则会创建master分支。推上去想更改名称，就把后面的名称改掉。

### 15.`git pull` 命令

是一个组合操作，它相当于先执行 `git fetch` 拉取远程仓库的更新，然后再执行 `git merge` 或 `git rebase` 将这些更新合并到当前分支上。

具体来说：

1. `git pull`

   ：它会自动执行两步操作：

   - **`git fetch`**：从远程仓库获取最新的提交和数据到本地。
   - **`git merge`** 或 **`git rebase`**：将获取的内容合并到当前所在的分支上。

- **`git pull` 的用法：**

  ```
  bashCopy code
  git pull <remote-name> <branch-name>
  ```

  这个命令会从指定远程仓库 `<remote-name>` 中拉取指定分支 `<branch-name>` 的更新并合并到当前所在的分支上。

- **默认行为：**

  如果你不指定远程和分支，`git pull` 将会根据当前分支的追踪设置，从默认的远程仓库和默认跟踪的分支中拉取更新。

- **注意事项：**

  使用 `git pull` 时需要小心，因为它会直接修改你当前工作目录下的文件。如果有冲突，你需要解决这些冲突才能完成合并。

通常在协作开发中，如果你想要获取远程仓库的最新更改并将其合并到你的工作分支上，可以使用 `git pull`。但是，如果你更倾向于控制合并过程，也可以使用 `git fetch` 和 `git merge` 或 `git rebase` 分步执行合并操作。

### 16.`git fetch` 命令

会将远程仓库的最新提交和分支信息下载到本地仓库，但它不会直接修改你的工作区文件。它主要更新了你本地的远程跟踪分支（如 `origin/master`），以便你可以查看远程分支的最新状态和对比本地分支和远程分支的差异。

即使你运行了 `git fetch`，你的工作区中的文件并不会直接改变。要将远程分支的更新合并到你的当前工作分支中，你需要额外运行 `git merge` 或 `git rebase` 命令。例如：

```
bashCopy code
git fetch origin
git merge origin/master  # 或者 git merge origin/main （具体分支名称取决于远程仓库的分支名）
```

上述命令将拉取远程仓库的更新，然后将远程 `origin` 的 `master` 分支合并到你当前的分支上。如果有冲突，你需要解决这些冲突后再进行提交。

### 17.如果在线版本（远程仓库）的内容比本地仓库更新

可能是由于本地仓库落后于远程仓库的原因。在这种情况下，你可以尝试以下几个步骤来解决问题：

#### 1. 拉取远程仓库的更新：

```
bashCopy code
git pull origin <branch-name>
```

这个命令会将远程仓库的更新拉取到你的本地仓库，并尝试将这些更新合并到你当前所在的分支上。如果存在冲突，你需要解决这些冲突后再进行提交。

#### 2. 解决冲突：

如果 `git pull` 操作后出现了合并冲突，Git 会提示你解决这些冲突。你需要手动编辑文件，解决冲突后再进行提交。

#### 3. 强制推送到远程仓库（慎用）：

**注意：强制推送会覆盖远程仓库的内容，请谨慎操作。**

如果你确信本地的修改是正确的，并且你愿意覆盖远程仓库的内容，可以使用强制推送。但在进行此操作之前，请确保你了解其风险和后果。

```
bashCopy code
git push origin <branch-name> --force
```

#### 4. 使用强制合并（慎用）：

如果你想强制将远程仓库的内容合并到本地并覆盖本地内容，可以尝试使用强制合并。

```
bashCopy code
git fetch origin
git reset --hard origin/<branch-name>
```

**警告：强制合并和强制推送可能会导致数据丢失或不可逆的更改，请确保在了解其影响并备份重要数据后才执行这些操作。**

### 18.`git clone` 

是用于克隆（复制）远程仓库到本地的命令。它可以从远程仓库创建一个副本到你的本地机器上。

#### 基本用法：

```
bashCopy code
git clone <远程仓库的 URL>
```

- `<远程仓库的 URL>` 是远程仓库的地址，可以是 HTTPS 或 SSH 协议的 URL。

