1. 初始化`git init`
2. 创建ssh密钥`ssh-keygen -t rsa -C "YourEmail"`
3. 将公钥粘贴到github账户上
4. 关联本地仓库`git remote add origin git@github.com:WinterSweet20/Learn.git`
5. 启动ssh-agent：`ssh-agent bash`
6. 添加私钥`ssh-add ~/.ssh/id_rsa`
7. 拉`git pull origin master`
8. 把文件添加到仓库`git add filename`
9. 把文件提交到仓库`git commit -m '说明'`
10. 提交远程仓库`git push origin master`



修改目标用户名和邮箱

`git config user.name yourName`<br/>`git config user.email yourEmail`

修改全局用户名和邮箱

`git config --global user.name yourName`<br/>`git config --global user.email yourEmail`

