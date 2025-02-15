以下是一份简明实用的 Git 系统教程，包含核心原理与高频命令，适合开发人员日常使用：

---

### **一、Git 核心原理（理解这些将不再死记硬背命令）**
#### 1. 数据模型
- **快照存储**：每次提交都是项目完整目录结构的快照（而非差异）
- **哈希寻址**：每个对象（提交/文件/目录）都有唯一的 SHA-1 哈希值
- **三棵重要树**：
  - **工作目录**：本地可见的文件
  - **暂存区（Index）**：准备提交的修改
  - **版本库（HEAD）**：已确认的历史记录

#### 2. 分支本质
- **分支指针**：分支本质是指向某个提交的可移动指针
- **HEAD 指针**：指向当前所在分支的最新提交
- **合并原理**：通过三方合并（共同祖先 + 双方修改）生成新提交

#### 3. 底层文件结构（.git 目录）
```
.git/
├── objects/    # 存储所有数据内容（Blob、Tree、Commit）
├── refs/       # 存储分支和标签的指针
├── HEAD        # 指向当前分支
└── config      # 仓库特定配置
```

---

### **二、Git 配置（先做好基础设置）**
```bash
# 全局用户配置
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 开启颜色显示
git config --global color.ui auto

# 设置默认编辑器（VSCode示例）
git config --global core.editor "code --wait"

# 查看所有配置
git config --list
```

---

### **三、高频命令手册**

#### **基础操作**
| 命令                        | 说明             |
| --------------------------- | ---------------- |
| `git init`                  | 初始化新仓库     |
| `git clone <url>`           | 克隆远程仓库     |
| `git status`                | 查看工作区状态   |
| `git add <file>`            | 添加文件到暂存区 |
| `git commit -m "msg"`       | 提交更改         |
| `git log --oneline --graph` | 查看简洁提交历史 |

#### **分支管理**
| 命令                       | 说明                     |
| -------------------------- | ------------------------ |
| `git branch`               | 查看本地分支             |
| `git checkout -b <branch>` | 创建并切换分支           |
| `git merge <branch>`       | 合并指定分支到当前分支   |
| `git rebase <branch>`      | 变基操作（整理提交历史） |
| `git branch -d <branch>`   | 删除已合并分支           |

#### **撤销操作**
| 命令                          | 说明                   |
| ----------------------------- | ---------------------- |
| `git restore <file>`          | 丢弃工作区修改         |
| `git restore --staged <file>` | 取消暂存               |
| `git commit --amend`          | 修改最后一次提交       |
| `git reset <commit>`          | 回退到指定提交（慎用） |
| `git revert <commit>`         | 创建撤销提交           |

#### **远程协作**
| 命令                          | 说明                   |
| ----------------------------- | ---------------------- |
| `git remote -v`               | 查看远程仓库           |
| `git fetch`                   | 下载远程更新（不合并） |
| `git pull`                    | 拉取并合并远程更改     |
| `git push -u origin <branch>` | 推送并关联远程分支     |
| `git push --force-with-lease` | 安全强制推送           |

---

### **四、典型工作流**
#### 1. 日常开发流程
```bash
git checkout -b feature/new-login  # 创建特性分支
# 编写代码...
git add .                         # 添加修改
git commit -m "实现登录验证功能"    # 提交
git push origin feature/new-login # 推送到远程
# 创建 Pull Request 进行代码审查
```

#### 2. 紧急修复线上问题
```bash
git checkout main                 # 切换到主分支
git pull                         # 获取最新代码
git checkout -b hotfix/404-error # 创建热修复分支
# 修复问题...
git commit -m "修复404页面错误"
git checkout main                # 回到主分支
git merge hotfix/404-error       # 合并修复
git push origin main             # 部署
git branch -d hotfix/404-error   # 清理分支
```

---

### **五、实用技巧**
#### 1. 清理历史记录
```bash
# 交互式重写提交历史（最近3次）
git rebase -i HEAD~3
```

#### 2. 找回丢失的提交
```bash
# 查看所有操作记录
git reflog
# 重置到指定操作
git reset --hard <commit-hash>
```

#### 3. 忽略文件配置（.gitignore）
```text
# 通用模板示例
*.log
node_modules/
.DS_Store
.idea/
```

#### 4. 设置命令别名
```bash
git config --global alias.st status
git config --global alias.br branch
git config --global alias.ci commit
```

---

### **六、推荐学习路径**
1. 掌握基础命令 → 2. 理解分支原理 → 3. 练习解决冲突 → 4. 学习 rebase 高级用法 → 5. 研究 Git 内部机制

**延伸学习**：
- 官方文档：[https://git-scm.com/doc](https://git-scm.com/doc)
- 可视化学习工具：[Learn Git Branching](https://learngitbranching.js.org/)

实际开发中遇到问题时，多使用 `git status` 查看状态，用 `git log --graph` 分析提交历史，大多数问题都能快速定位解决。