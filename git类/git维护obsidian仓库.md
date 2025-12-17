



## **2) 初始化 Git 仓库**


在 Vault 根目录执行：

```
cd ~/Vaults/KnowledgeBase
git init
git branch -M main
```

---

## **3) 写好** 

## **.gitignore**

## **（关键，避免无意义文件和冲突）**


在 Vault 根目录创建 .gitignore（推荐模板如下）：

```
# Obsidian cache / workspace（强烈建议不跟踪）
.obsidian/workspace*
.obsidian/cache
.obsidian/graph.json
.obsidian/hotkeys.json
.obsidian/app.json

# 插件数据（默认不跟踪，减少跨设备冲突）
.obsidian/plugins/
.obsidian/themes/

# 可选：如果你想同步插件/主题，把上面两行删掉，改成忽略插件里的缓存
# .obsidian/plugins/**/data.json
# .obsidian/plugins/**/cache/

# 系统垃圾
.DS_Store
Thumbs.db

# 临时/导出
*.tmp
*.bak
```

原则：

- **笔记内容要跟踪**（你的 .md、图片、附件）
    
- **Obsidian 的 workspace/cache 不要跟踪**（最容易冲突）
    

---

## **4) 首次提交**

```
git add .
git commit -m "init vault"
```

---

## **5) 推到 GitHub 私有仓库（SSH）**

  

先在 GitHub 新建一个 **Private** 空仓库（不要勾 README）。

  

绑定远程并推送：

```
git remote add origin git@github.com:USERNAME/REPO.git
git push -u origin main
```

以后就不需要输入密码（前提是你 SSH key 已配置好）。

---

## **6) 日常使用工作流（最重要）**


### **每次开始写笔记前（任何设备）**

```
git pull --rebase
```

### **写完一段，准备收工时**

```
git status
git add -A
git commit -m "notes: update"
git push
```

推荐你坚持一个规则：

- **打开 Obsidian 前 pull**
    
- **关 Obsidian 前 commit+push**
    

---

## **7) 多设备同步的冲突管理（避免翻车）**

  
### **最关键规则**

1. **不要两台设备同时改同一个文件**
    
2. 一台设备 push 完再换另一台 pull
    

  
### **如果真的冲突了**



先看哪些文件冲突：

```
git status
```

处理后：

```
git add -A
git commit -m "resolve conflict"
git push
```

（通常冲突发生在 .obsidian/workspace*，所以我们才要 ignore 它。）

---

## **8) 建议加一个“自动化快捷命令”（可选但强烈建议）**

  

在 Vault 根目录建两个脚本：

  
### **pull.sh**

```
#!/usr/bin/env bash
set -e
git pull --rebase
```

### **push.sh**

```
#!/usr/bin/env bash
set -e
git add -A
git commit -m "notes: update" || true
git push
```

赋权：

```
chmod +x pull.sh push.sh
```

以后你只要：

```
./pull.sh
# 写笔记
./push.sh
```

---

## **9) 附件（图片/PDF）怎么管（实用建议）**

- 附件可以直接跟踪（你个人知识库一般问题不大）
    
- 如果附件很多、仓库变大，建议：
    
    - 用 Git LFS（适合大文件）
        
    - 或把大附件放云盘，仓库只存链接/索引
        
    

---

## **10) 推荐的仓库结构（更好找）**

```
KnowledgeBase/
├─ Inbox/              # 临时收集，定期整理
├─ Notes/              # 归档后的正文
├─ Index/              # 各领域索引页
├─ Attachments/        # 图片/PDF等
└─ .obsidian/
```


