# git及GitHub基础操作

> 安装git    https://git-scm.com/downloads
>

## 本地仓库与远程仓库操作

### git clone远程仓库到本地

1. GitHub上找到仓库或者新建仓库，复制clone地址

2. 本地 git bash 输入 clone地址 
   - 如 git clone https://github.com/your-username/MyWebServer.git

3. 本地添加或修改代码

   - git add test.cpp


   - git commit -m "Update test file"

4. 推送到远程仓库

   -  git push


   - 如果是第一次推送，没有自动关联 git push -u origin main

5. 拉取远程最新代码
   - git pull

### 本地新建仓库 —— 再关联远程仓库remote（GitHub - repository）

1. 本地创建文件夹，作为本地仓库

2. 在该文件夹中打开git bash，初始化本地仓库
   -   git init

3. 本地仓库可添加文件或代码（比如test.cpp）

   -   git add test.cpp添加文件到暂存区
     （可以先用 git status 查看状态，会提示哪些文件未添加到仓库）


   -   git commit -m "commit cpp file"，暂存区提交到本地仓库
     （" "里是提交信息，描述本次提交）

4. 在GitHub新建repository，获取仓库clone地址（Code - HTTPS/SSH），如  https://github.com/LionZhY/MyNote-ZZZZH.git

5. 本地仓库关联远程仓库，

   - git remote add origin https://github.com/LionZhY/MyNote-ZZZZH.git
     git remote <远程仓库名称> <远程仓库URL>        远程仓库名称默认是origin


   - git remote -v验证是否关联成功

6. 拉取远程仓库同步到本地
   -  git pull origin master（现在可能默认是main）

7. 添加或者修改代码，比如修改了test.cpp

   - git add test.cpp


   - git commit -m "Update test file"


   - git push  本地代码推送到GitHub远程仓库

     - 需指明分支
       - git push origin master
     - 不需要指明分支
         - git branch -M main 重命名当前分支为main
           也可以不改（更符合新的标准，可以查一下，现在好像都是main了）
         
         - git push -u origin main     -u 会建立本地分支和远程分支的关联，以后只需要  git push      git pull 不用指明分支
     
         - git push
     



