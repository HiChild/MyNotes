

# Mac git常用命令总结



```


git config --global user.email "you@example.com" //设置用户标识

git config --global user.name "Your Name" //设置用户标识

which git //查看git的安装目录

git clone git@git.xxx.com:XXX.git //拉取项目

git status //查看缓冲区状态

git add xxx文件 //刷新 xxx文件至缓冲区

git commit -m "信息" //将缓冲区的数据提交至仓库

git push xxx //推送到远程仓库

git checkout xxx//切换到xxx分支

git pull -r //推荐使用 拉取远端的分支 （包扩git fetch)

git fetch //只更新远端的库，不会更新本地的工作区

git remotes origin/master //可以免pull



```

git cherry-pick学习：https://zhuanlan.zhihu.com/p/58962086

