add == track
关注.gitignore
正则表达式与glob模式

git status 与 git diff

1、当一个文件被修改后，会处于modified&未stages状态。
2、当你再次add这个文件时，会显示这个文件处于等待被
git add命令。这是个多功能命令:可以用它开 始跟踪新文件,或者把已跟踪的文件放到暂存区,还能用于合并时把有冲突的文件标记为已解决状态等。
再add后进行commit，提交的并非add后再次修改的文件，而是git中的staged暂时缓存区

#git
