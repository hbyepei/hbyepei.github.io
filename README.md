# hbyepei.github.io
Personal Blog

hexo-git使用说明:
    目前在githug中存在两个仓库:master(发布网站)和hexo，其中hexo是默认仓库(存放源文件)
    
    在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
    依次执行git add .、git commit -m ""、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
    然后才执行hexo d -g 发布网站到master分支上。
    如果提示:ERROR Deployer not found: git，则先执行一下："npm install hexo-deployer-git --save"
        
        
    
    
