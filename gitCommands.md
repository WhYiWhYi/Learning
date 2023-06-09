### 普通命令
```git
// 1.初始化git仓库
git init

// 2.查看git仓库状态
git status

// 3.将文件添加到git仓库的临时缓冲区（还未提交到仓库）
// 未添加至缓冲区时，将提示"untracked files"
git add <file>

// 4.将文件提交到git仓库
// 提交完成侯，提升"nothing to commit, working tree clean"
git commit -m "text commit"  // "text commit"类似于备注

// 5.查看git仓库提交日志
git log

// 6.分支相关
git branch  // 查看分支；查询结果中，*代表当前所在分支
git branch a  // 创建分支a
git checkout a  //切换至分支a
git merge a  // 合并分支的时候，要考虑到两个分支是否有冲突，如果有冲突，则不能直接合并，需要先解决冲突
git branch -d a  // 删除分支a
git branch -D a  // 强制删除分支a
git tag <tag>  // 为当前所在分支添加tag
```

###  代码提交
1. 将远程（github）仓库clone到本地（git）仓库（clone到本地的仓库本身就是git仓库，无需再进行init初始化，且其自动关联远程仓库）
```
// 在github进入要clone的项目，点击"clone or download"，复制地址（示例：https://github.com/WhYiWhYi/Learning.git）
// 在gitBash中进入GitRepo目录

git clone https://github.com/WhYiWhYi/Learning.git
```
2. 将本地（git）仓库新增的内容同步到远程（github）仓库
```
// 本体新增的仓库需要先进行初始化步骤

// 首先对新增的内容进行add和commit
git add <file>
git commit -m "text commit"

// 提交至远程仓库
git push

// 将本地仓库指定分支推送到远程仓库的指定分支
git push <远程仓库的别名> <本地分支名>:<远程分支名>
```