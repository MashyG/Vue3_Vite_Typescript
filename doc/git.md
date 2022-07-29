## 配置多个GitHub账号于本机

1. 查看 `git` 已配置列表
   ```
   git config --list
   ```
2. 若已存在全局的 `git` 配置</br>
   可删除：
   ```
   git config --global --unset user.name
   git config --global --unset user.email
   ```
   可新增另外的 `ssh-key`:
   ```
   // 一直回车即可
   ssh-keygen -t rsa -C "github账号"
   // 默认生成～/.ssh/id_rsa（密钥）和.ssh/id_rsa.pub（公钥）

   ssh-keygen -t rsa -C "github账号" -f ～/.ssh/另外的文件名
   // 生成～/.ssh/另外的文件名（密钥）和.ssh/另外的文件名.pub（公钥）
   ...
   ```
3. 将刚才新增的两个或多个 `ssh-key` 放入 `ssh-agent` 信任列表中
   ```
   ssh-add ~/.ssh/id_rsa
   ssh-add ~/.ssh/另外的文件名
   ...
   ```
4. 复制两个或多个公钥（.pub文件）,分别添加到不同的git账号下
5. 需要添加一个config文件到.ssh文件夹下（没有则新建一个），去配置多个github账号的推送操作
   ```md
   # default
   Host github.com   // 用于区分不同github账号的
   Hostname github.com  // github地址或其他仓库地址
   IdentityFile ~/.ssh/id_rsa // 本地密钥地址
   User default   // 自定义用户名
    
   # 另外的文件名
   Host 另外的文件名.github.com
   Hostname github.com
   IdentityFile ~/.ssh/另外的文件名
   User 另外的文件名
   ```
6. 测试连接
   ```
   ssh -T git@github.com
   // 连接默认，成功会提示：Hi，default！...

   ssh -T git@另外的文件名.github.com
   // 连接默认，成功会提示：Hi，另外的文件名！...
   ```
7. clone
   ```
   // default
   git clone git@github.com:xxx/xxx.git
   // 另外的文件名
   git clone git@另外的文件名github.com:xxx/xxx.git
   ```

## 本地项目上传到 git 远程仓库

1. 进入项目目录中
2. 添加git用户和邮箱
   ```
   git config --global user.name 'github的名字'
   git config --global user.email 'github绑定的邮箱'

   git config --local user.email 'github绑定的邮箱'
   git config --local user.email 'github绑定的邮箱'
   ``` 
3. 生成秘钥，找到`id_rsa.pub文件`，复制秘钥添加到github上
   ```
   ssh-keygen -t rsa -C 'github绑定的邮箱'
   ```
4. 初始化git
   ```
   git init
   ```
6. 提交项目内容
   ```
   git add .
   ```
7. 撤销已提交内容
   ```
   git reset .
   ```
8. 可查看git状态
   ```
   git status
   ```
9. 上传提交的内容到本地仓库
   ```
   git commit -m '注释'
   ```
10. 连接github远程仓库
   ```
   git remote add origin '你的仓库地址'
   // 若有多个github
   git remote add origin 'git@另外的文件名.github.com/xxx'
   ```
11. 推送本地仓库到远程仓库(新的远程仓库无分支，则会新建一个`master`分支)
   ```
   git push -u origin master
   ```