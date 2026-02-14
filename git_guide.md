# Git 分支操作指南

## 分支操作

| 操作       | 命令                          | 说明                          |
|------------|-------------------------------|-------------------------------|
| 创建分支   | `git branch <name>`          | 创建新分支，但不会自动切换    |
| 切换分支   | `git checkout <name>` 或 `git switch <name>` | 切换到指定分支                |
| 创建并切换 | `git checkout -b <name>`     | 一步完成创建与切换            |
| 查看分支   | `git branch`                 | 列出本地所有分支，当前分支前有 * |
| 合并分支   | `git merge <name>`           | 将指定分支合并到当前分支      |
| 删除分支   | `git branch -d <name>`       | 安全删除已合并的分支          |

## 基本工作流程

1. **切换回主分支**：`git checkout main`。
2. **创建功能分支**：`git checkout -b feature-login`。
3. **提交更改**：在功能分支上进行开发并执行 `git commit`。
4. **合并回主线**：先切回主分支，再执行 `git merge feature-login`。
5. **推送远程**：`git push origin main` 将本地合并后的结果同步到服务器。 
