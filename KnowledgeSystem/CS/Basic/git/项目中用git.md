1. 将项目克隆下来后需要创建一个新的本地分支
`git checkout -b "newbranch"`
2. 在自己添加好新的功能后进行
```bash
git add filename   # 将文件存到暂存区
git commit -s      # 将文件添加注释
```
3. 将暂存区的文件提交到云端分支
```bash
#  推送到云端分支  本地分支名：云端分支名（云端分支名可以随便命名）
git push origin newbranch:newCloudBranchName
```
4. 文件保存到云端分支后，提交PR
5. PR选择将云端分支newCloudBranchName 合并到 cracc-develop
6. 