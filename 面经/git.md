```js
target - 4 工具

git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并。

git pull 会将远程仓库的变化下载下来，并和当前分支合并。

如果你想要一个干净的，没有merge commit的线性历史树，那么你应该选择git rebase
如果你想保留完整的历史记录，并且想要避免重写commit history的风险，你应该选择使用git merge 【merge的效果，简单来说就合并两个分支并生成一个新的提交。】

可以看出merge结果能够体现出时间线，但是rebase会打乱时间线。
而rebase看起来简洁，但是merge看起来不太简洁。


在项目中经常使用git pull来拉取代码，
git pull相当于是git fetch + git merge
运行git pull -r，也就是git pull --rebase，相当于git fetch + git rebase


开发流程
git clone 

基于某个分支创建新的分支
# 获取主干最新代码
$ git checkout dev
$ git pull

# 新建一个开发分支myfeature
$ git checkout -b myfeature


git add .
git commit -m 'fix(app): fix xyxyxy'
git push origin feat-zt

提交 commit 时，必须给出完整扼要的提交信息。按照 ops: xxx 的格式填写，ops分为以下几类：
build 修改项目构建系统(如 vite 的配置等)
ci 修改项目持续集成流程(如 Jenkins，GitLab CI等)
perf 优化/性能提升
feat 新增功能
fix bug 修复
refactor 重构代码(既没有新增功能，也没有修复bug)
docs docs 文档和注释相关
chore 依赖更新及不属于其他类型的提交
style 代码风格相关不影响程序逻辑的修改
merge 分支合并

test 测试相关

merge 之前
git rebase -i HEAD~3 合并最近三次的commit。【squash】
git push origin feat-zt -f

拉取目标分支最新代码
$ git fetch origin
$ git rebase origin/dev 将commit 排在最新的dev 之后；

如果有冲突，解决之后，执行
git rebase --continue

git push origin feat-zt -f
```