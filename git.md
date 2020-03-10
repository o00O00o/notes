# git&github使用方法

+ 查看git的使用方法：`git`
+ 把当前文件夹变为一个git仓库(创建git仓库)：`git init`
+ 查看当前仓库文件变化情况：`git status`
+ 添加修改：`git add filename`(可使用`git add .`来添加当前仓库所有修改)
+ 查看本次添加但还没有提交的修改：`git diff`(比较工作区与暂存区的区别)
+ 撤销提交操作：`git reset`
+ 向Git提交自己的名字：`git config --global user.name "xxx"`
+ 向Git提交自己邮箱：`git config --global user.email "xxx@xx.com"`
+ 向Git提交内容：`git commit -m “xx”（xx为对提交的内容进行描述）`
+ 让Git不提交某些文件/忽略某些文件：创建文件 .gitignore 并在文件中添加文件名/文件夹名 即可 （若git已经开始追踪某些文件 则需要11）
+ 让Git不再追踪某个/某些文件：`git rm --cached xx （xx为文件名）`
+ Git添加分支：`git branch xx （xx为分支名）`
+ Git切换分支：`git checkout xx （xx为分支名）`
+ 合并分支：`git merge xx（xx为分支名）`
+ 列出本地分支：`git branch`
+ 删除分支:`git branch -d xx (xx为分支名，-D强制删除)`
+ 添加远程仓库：`git remote add origin xxx （xxx为远程地址）`
+ 设置本地分支追踪远程分支：`git push --set-upstream`
+ 克隆仓库：`git clone xxx（xxx为远程地址）`