1. 安装 Hexo

   - 打开 Command Prompt

   - 输入 `npm install -g hexo-cli`

   - 回车开始安装

   - 输入 `hexo -v`

   - 得到 hexo-cli: 1.0.4 等一串数据

     安装成功

2. 创建本地博客

   - 在D盘下创建文件夹 blog

   - 鼠标右键 blog，选择 Git Bash Here。 如果没有安装 Git，就不会有这个选项。

   - Git Bash 打开之后，所在的位置就是 blog 这个文件夹的位置。（/d/blog）

   - 输入 `hexo init` 将 blog 文件夹初始化成一个博客文件夹。

   - 输入 `npm install` 安装依赖包。

   - 输入 `hexo g` 生成（generate）网页。 由于我们还没创建任何博客，生成的网页会展示 Hexo 里面自带了一个 Hello World 的博客。

   - 输入 `hexo s` 将生成的网页放在了本地服务器（server）。

   - 浏览器里输入 [http://localhost:4000/](http://localhost:4000/) 。 就可以看到刚才的成果了。

   - 回到 Git Bash，按 Ctrl+C 结束。

     此时再看 [http://localhost:4000/](http://localhost:4000/) 就是无法访问了。

3. 发布一篇博客

   - 继续在 Git Bash 里，所在路径还是 /d/blog。输入 `hexo new "My First Post"`
   - 在 D:\blog\source_posts 路径下，会有一个 My-First-Post.md 的文件。 编辑这个文件，然后保存。
   - 回到 Git Bash，输入 `hexo g`
   - 输入 `hexo s`
   - 前往 [http://localhost:4000/](http://localhost:4000/) 查看成果。
   - 回到 Git Bash，按 Ctrl+C 结束。

## 将本地 Hexo 博客部署在 Github 上

前面两个部分，我们已经有了本地博客，和一个能托管这些资料的线上仓库。只要把本地博客部署（deploy）在我们的 Github 对应的 Repository 就可以了。

**操作如下：**

1. 获取 Github 对应的 Repository 的链接。

   - 登陆 Github，进入到 ryanluoxu.github.io

   - 点击 Clone or download

   - 复制 URL 待用

     我的是 `https://github.com/Ryanluoxu/ryanluoxu.github.io.git`

2. 修改博客的配置文件

   - 打开配置文件 /d/blog/_config.yml （使用 bash 里的 vi 或者 notepad++）

   - 找到

      

     ```
     #Deployment
     ```

     ，填入以下内容：

     ```
     deploy:  
     	  type: git  
     	  repository: https://github.com/Ryanluoxu/ryanluoxu.github.io.git  
     	  branch: master
     ```

3. 部署

   - 回到 Git Bash

   - 输入 `npm install hexo-deployer-git --save` 安装 hexo-deployer-git 此步骤只需要做一次。

   - 输入 `hexo d`

   - 得到 `INFO Deploy done: git` 即为部署成功

     之前我们创建的 ReadMe.md 会被自动覆盖掉。

4. 查看成果

   前往 ryanluoxu.github.io 即可。

## 使用 Next 主题

[更多 Hexo 的主题看这里](https://hexo.io/themes/)

这里以 Next 为例。 大概思路就是把整个主题的文件克隆到我们的主题文件夹中。在配置文件中注明使用该主题。

**操作如下：**

1. 还是回到 Git Bash。 输入 `git clone https://github.com/iissnan/hexo-theme-next themes/next`

   这样，该主题的文件就全部克隆到 D:\blog\themes\next 下面。

2. 修改博客配置文件

   - 打开 D:\blog_config.yml

   - 找到 `theme:`

   - 把 Hexo 默认的 lanscape 修改成 next。 即 `theme: next`

   - 找到 `# Site`，添加博客名称，作者名字等。

   - 在 `language` 后面填入 en 或者 zh-Hans，选择英文或者中文。

   - 找到 `# URL`, 填入 url。比如 `url: https://ryanluoxu.github.io`

     填入名字后会有很风骚的 © 2017 Ryan Luo Xu 的字样出现在博客底部。

3. 重新生成部署即可

   - 回到 Git Bash。输入 `hexo g -d`就可以了。

     先把修改的内容生成网页，再部署。

4. 查看成果

   前往 ryanluoxu.github.io 即可。