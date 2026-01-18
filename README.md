# ABSESpy-examples
Examples for using ABSESpy

## 项目结构

本项目使用 Git Submodule 来管理示例代码。当前包含以下 submodule：

- `abses_forestfire`: 森林火灾模拟示例

## Submodule 使用指南

### 初始化 Submodule

首次克隆仓库时，需要初始化并更新 submodule：

```bash
# 克隆仓库时自动初始化 submodule
git clone --recurse-submodules <repository-url>

# 或者克隆后手动初始化
git submodule init
git submodule update

# 或者使用一条命令完成初始化和更新
git submodule update --init --recursive
```

### 更新 Submodule

#### 1. 在 Submodule 中进行开发

```bash
# 进入 submodule 目录
cd abses_forestfire

# 切换到开发分支（或其他分支）
git checkout dev

# 进行代码修改后，在 submodule 内部提交
git add .
git commit -m "Your commit message"
git push origin dev
```

#### 2. 同步 Submodule 更改到主仓库

当 submodule 有新的提交后，需要更新主仓库对 submodule 的引用：

```bash
# 返回主仓库根目录
cd ..

# 方法一：使用 git submodule update --remote（推荐）
# 注意：需要在 .gitmodules 中指定 branch = dev
git submodule update --remote abses_forestfire

# 方法二：手动更新到特定分支的最新提交
cd abses_forestfire
git pull examples dev  # 如果远程仓库名为 examples
# 或者
git pull origin dev    # 如果远程仓库名为 origin
cd ..

# 查看 submodule 引用变化
git status

# 提交主仓库的 submodule 引用更改
git add abses_forestfire .gitmodules
git commit -m "Update submodule reference"
git push origin main
```

**故障排除：如果遇到 `Unable to find refs/remotes/origin/HEAD` 错误**

这通常是因为 submodule 的远程仓库名不是 `origin`，或者缺少 HEAD 引用。解决方法：

1. 确保 `.gitmodules` 中指定了分支（如 `branch = dev`）
2. 使用手动更新的方法（方法二）
3. 或者在 submodule 中添加 `origin` 远程：
   ```bash
   cd abses_forestfire
   git remote add origin <remote-url>
   # 或者重命名现有远程
   git remote rename examples origin
   cd ..
   ```

#### 3. 拉取主仓库的 Submodule 更新

当其他协作者更新了 submodule 引用后：

```bash
# 更新主仓库
git pull

# 同步 submodule 到主仓库引用的提交
git submodule update --init --recursive
```

### 常用命令

```bash
# 查看所有 submodule 状态
git submodule status

# 更新所有 submodule 到远程最新提交
git submodule update --remote

# 更新所有 submodule 到主仓库引用的提交
git submodule update

# 删除 submodule（如果不再需要）
git submodule deinit -f abses_forestfire
git rm -f abses_forestfire
rm -rf .git/modules/abses_forestfire
```

## 注意事项

- Submodule 是一个独立的 Git 仓库，需要在 submodule 目录内单独进行 Git 操作
- 主仓库只保存 submodule 的提交引用（commit hash），不保存 submodule 的具体代码
- 修改 submodule 后，需要在 submodule 内提交并推送，然后再在主仓库中更新引用并提交
