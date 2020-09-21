#### 合并历史提交

我们在开发一个需求时，通常为了防止代码丢失，会进行多次提交，但是在最后push时这些提交记录就没什么用了，所以需要将历史提交进行合并。`rebase`命令派上了用场：

````bash
# 查看提交历史，找到需要变基的提交
git log 
PS D:\xp\doc> git log
commit 9e9cbebb97a7bff19cb592c63c91149b36ea82d3 (HEAD -> master)
Author: xupan <xp545945@163.com>
Date:   Mon Sep 14 23:45:55 2020 +0800

    add rebase

commit d7f52526bbd6567111411ad07b03a539f845b854 (origin/master, origin/HEAD)
Author: panxu18 <xp545945@163.com>
Date:   Mon Sep 14 21:36:50 2020 +0800

    add pollchunk

commit 07f7806d8c3f78f2d24b0c94cbafe614422a1c5b
Author: panxu18 <xp545945@163.com>
Date:   Sun Sep 13 21:56:18 2020 +0800

    modify netty byteBUf

commit bb4e20b921e25ff61ce67c1674479dbf54505bcf
Author: panxu18 <xp545945@163.com>
Date:   Sun Sep 13 21:55:29 2020 +0800

    add netty bytebuf

# 选择一个commit进行变基，这里选择了checkout分支时的最新提交
git rebase -i d7f52526bbd6567111411ad07b03a539f845b854
# 根据提示选择需要合并的分支
pick：表示保留该提交
squash：表示将该commit和之前的commit合并

````

#### 优化提交历史

当多个人需要在共同开发时，例如我在master分支上checkout一个新分支dev，当我愉快的开发完之后发现master分支已经有人提交新修改了，于是有以下一系列操作：

```bash
git switch master
git pull
git switch -
git merge master
git push
#然后提交合并，经过这些操作之后无论是在mater分支还是在本地分支上查看都会有如下合并记录
*   commit a8c81e4be17888567a1b8c342d9568b921b2c681 (HEAD -> dev)
|\  Merge: dbf9c3b 7678aee
| | Author: panxu18 <xp545945@163.com>
| | Date:   Tue Sep 15 11:12:15 2020 +0800
| | 
| |     Merge branch 'master' into dev
| | 
| * commit 7678aeed957523fad5f7a1001cdd290f2826cb35 (origin/master, master)
| | Author: xupan <xp545945@163.com>
| | Date:   Mon Sep 14 23:45:55 2020 +0800
| | 
| |     add rebase.md
| | 
* | commit dbf9c3be609e87df1b97b5a2ddbcbea438a088df
|/  Author: panxu18 <xp545945@163.com>
|   Date:   Tue Sep 15 10:56:15 2020 +0800
|   
|       add PoolChunk内存分配
| 
* commit d7f52526bbd6567111411ad07b03a539f845b854

```

之所以出现这个分叉是因为，我的dev分支是从提交d7f5252签出的，提交d7f5252也就是基点，而现在因为有人将master分支修改了，所以我想基于基点d7f5252进行合并时无法快速向前合并，所以merge命令就将mater最新的提交7678aee和dev最新的提交dbf9c3b进行合并，形成一个新的提交a8c81e4。

rebase操作此时就可以将dev的基点进行修改，也就是说先本地将已提交的dbf9c3b删除，然后将master上的提交全部合并到dev上，这时dev的基点就相当于更新为7678aee了，在这个基础上再将本地提交重新应用就可以了。

```bash
git switch dev
git rebase master
# 解决冲突
git rebase --continue
# 提交合并后，提交记录就是一条直线，可以看到最后一个提交的hash变了，说明这是一个新的提交。
* commit 88741223058c8ca34a2512a889d40d5b90016932 (HEAD -> dev, origin/master,
 master)
| Author: panxu18 <xp545945@163.com>
| Date:   Tue Sep 15 10:56:15 2020 +0800
| 
|     add PoolChunk内存分配
| 
* commit 7678aeed957523fad5f7a1001cdd290f2826cb35
| Author: xupan <xp545945@163.com>
| Date:   Mon Sep 14 23:45:55 2020 +0800
| 
|     add rebase.md
| 
* commit d7f52526bbd6567111411ad07b03a539f845b854
| Author: panxu18 <xp545945@163.com>
| Date:   Mon Sep 14 21:36:50 2020 +0800
| 
|     add pollchunk
| 

```

#### 注意事项

rebase不能应用与共享分支，从上面的操作中可以看出，rebase会删除历史提交记录，形成一个结果一样的新的提交记录，如果我们将rebase的分支强制提交到remote。那么其他人提交时肯定无法fast-forward合并，当然也可以通过rebase进行合并，但是这样就浪费了大家的时间。所以rebase只能用于自己的分支，美化自己的分支的提交记录。





