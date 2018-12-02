#### git checkout 进阶

- `git checkout -- [filename]`
  - 将<font color="red">工作区</font>的内容丢弃，与最后一次commit保持一致。
  - 对 test.txt 文件进行编辑，不add进 暂存区。随后进行 git checkout -- test.txt 操作。会发现刚刚编辑的内容，被丢弃了。

与之类似的命令：`git reest HEAD [filename]`,作用是将添加到暂存区（stage,index）的内容从暂存区挪动到工作区。

- `git checkout [commit_id]`

  - 切换某一个commit_id上（注意：不是回退）

    查看log

    ```bash
    $ git log
    commit a29300d36bb28e3c268b60c96e2273c12073094e (HEAD -> master, dev)
    Author: zhangyuqing <coderzyq@163.com>
    Date:   Mon Nov 26 23:25:24 2018 +0800
    
        test55.txt
    
    commit 37553ae440210d7c52c2058f34fad695e9b3f794
    Author: zhangyuqing <coderzyq@163.com>
    Date:   Mon Nov 26 23:14:05 2018 +0800
    
        test5.tx
    
    commit 7ea85edfc1bf15b6a0205c0588c83d762898fe80
    Merge: 3d123f6 8dc752e
    Author: zhangyuqing <coderzyq@163.com>
    Date:   Mon Nov 26 23:09:25 2018 +0800
    
        Merge branch 'dev'
    ```

    执行`git checkout 7ea8`，切换到7ea8提交上

    ```bash
    $ git checkout 7ea8
    Note: checking out '7ea8'.
    
    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.
    
    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:
    
      git checkout -b <new-branch-name>
    
    HEAD is now at 7ea85ed... Merge branch 'dev'
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit ((7ea85ed...))
    ```

    可以看出，已切换到 `7ea8`上了，并且shell终端已提示处于该commit_id上。

    提示信息说，目前处于`游离状态`。

    如果此时对当前commit_id下的 test.txt 文件进行修改，并进行commit：

    ```bash
    $ git status
    HEAD detached at 7ea85ed
    nothing to commit, working tree clean
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit ((7ea85ed...))
    $ ls
    b3.txt  test.txt  test2.txt  test3.txt  test4.txt
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit ((7ea85ed...))
    $ vi test.txt
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit ((7ea85ed...))
    $ cat test.txt
    hello 456
    789
    $ git add .
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit ((7ea85ed...))
    $ git commit -m "add a new line "
    [detached HEAD 0d24d8d] add a new line
     1 file changed, 1 insertion(+)
    ```

    如果此时进行尝试，切换到 master 分支：

    ```bash
    $ git checkout master
    Warning: you are leaving 1 commit behind, not connected to
    any of your branches:
    
      0d24d8d add a new line
    
    If you want to keep it by creating a new branch, this may be a good time
    to do so with:
    
     git branch <new-branch-name> 0d24d8d
    
    Switched to branch 'master'
    ```

    git 会提示你，刚刚你修改的test.txt文件，进行了提交，但是没有在任何分支上。如果在此时将刚刚的提交（0d24d8d）与一个分支进行绑定，将是非常的时间。此时，已切换回master分支。

    我们按照提示进行创建新的分支与`0d24d8d`提交绑定：

    ```bash
    $ git branch tmpBr 0d24d8d
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit (master)
    $ cat test.txt
    hello 456
    ```

    可以看出，刚刚对 test.txt 作出的修改（添加了一行：789）并没有被带回到master分支。

    查看当前所有的分支：

    ```bash
    $ git branch
      b3
      dev
    * master
      tmpBr
    ```

    会发现多出为0d24d8d创建的新分支：tmpBr，我们切换过去：

    ```bash
    $ git checkout tmpBr
    Switched to branch 'tmpBr'
    
    Yangle@DESKTOP-2L22GDH MINGW64 /e/Learn/shell/mygit (tmpBr)
    $ cat test.txt
    hello 456
    789
    ```

    可以发现，刚刚对 test.txt 作出的修改，被保存起来了。并且当时的commit也有了自己的分支。

### stash

