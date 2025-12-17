
## 先配置身份
```shell
git config --global user.name "YakutsukuriYuu"
git config --global user.email "yakutsukuriyuu@gmail.com"

# 看看配置
git config --global --list

# 配置默认分支
git config --global init.defaultBranch main
```

## 生成ssh连接github
```shell
ssh-keygen -t ed25519 -C "yakutsukuriyuu@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub

# 测试
ssh -T git@github.com
```


## 常用命令
```shell
git init
git add .
git commit -m "init vault"
git remote add origin git@github.com:USERNAME/REPO.git
git push -u origin main
```

## 有了仓库之后，常用
```shell
# 写之前
git pull --rebase
#**先把远程的最新提交拉下来，然后把你本地还没推上去的提交“挪到最上面”重新接上**，让历史保持一条直线，尽量减少“合并提交”。
#- 先把 A 的更新拉下来 再把 B 的提交按顺序接上去 结果就是：**历史干净**，更像“你一直在同一条线写笔记”。 
git add -A
git commit -m "notes: update"
git push
```



## 杂项
```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```


这两行的作用是：**启动 ssh-agent（一个临时“钥匙串”进程）并把你的私钥加载进去**，这样后续 git push/pull 等 SSH 连接时可以自动用密钥完成认证。

  

如果你“不做这一步”，会出现的情况取决于你当前环境：

1. **最常见：仍然能用，但会更麻烦**
    
    - 你每次 git push/pull 时，SSH 可能会提示你输入私钥的 passphrase（如果你给私钥设置过）。
        
    - 如果你的私钥没设 passphrase，很多情况下也能直接用，只是每次都由 ssh 进程自己去读 ~/.ssh/id_ed25519。
        
    
2. **在一些场景：会直接连不上（Permission denied (publickey)）**
    
    - 典型是你用了**非默认文件名**的私钥（不叫 id_ed25519/id_rsa），或者你依赖 ssh-agent 来决定用哪把钥匙。
        
    - 或者你的环境/配置（~/.ssh/config）要求必须从 agent 里取 key。
        
    
3. **在 macOS 上：你可能更应该用 Keychain，而不是每次手动 ssh-add**
    
    - 你不做 ssh-add，GitHub 连接仍可能成功（看你的配置和是否有 passphrase）。
        
    - 但如果你想“开机后/重开终端后不再重复输入”，通常做法是把 key 存到系统钥匙串（Keychain），让它自动加载。
        
    

  

快速判断你现在属于哪种情况（可直接运行）：

```
ssh -T git@github.com
```

- 如果提示成功（例如 “Hi xxx! …”），说明即使不做 ssh-add 也能连上（至少当前终端会话可以）。
    
- 如果报 Permission denied (publickey)，通常就需要 ssh-add 或检查 ~/.ssh/config、公钥是否已添加到 GitHub。
    

  

补充：如果你只是“偶尔用”或者你的 key **没有 passphrase**，不做这一步通常没什么；如果你想实现“不要每次输密码/口令”，那就建议使用 ssh-agent/ssh-add（或 macOS Keychain 持久化）。