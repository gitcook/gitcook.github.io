# gitcook.github.io
hexo backup

到了新的电脑上时，我们需要将项目先下载到本地，然后再进行hexo初始化。

###git clone https://github.com/gitcook/gitcook.github.io.git
###cd gitcook.github.io
###npm install -g hexo-cli
###npm install
###npm install hexo-deployer-git --save

##安装的插件(直接执行npm instal会安装package.json里面的包)
sitemap
###npm install hexo-generator-sitemap --save
###npm install hexo-generator-baidu-sitemap --save

gulp压缩
###npm install gulp -g
###npm install gulp-clean-css gulp-uglify gulp-htmlmin gulp-imagemin gulp-htmlclean gulp --save

###之后开始写博客，写好部署好之后，别忘记 git add , ….git push origin hexo…推上去。。。
