#### PHP 手册
- [https://www.runoob.com/php/php-tutorial.html](https://www.runoob.com/php/php-tutorial.html)
## laravel框架
- laravel框架一键安装包，直接下载源码，方便实用
[https://xueyuanjun.com/post/2](https://xueyuanjun.com/post/2)
- github仓库下载地址 
1. 应用仓库
[https://github.com/laravel/laravel](https://github.com/laravel/laravel)
2. 框架仓库
[https://github.com/laravel/framework](https://github.com/laravel/framework)
- 学院君
[https://xueyuanjun.com/](https://xueyuanjun.com/)
- laravel社区
[https://learnku.com/](https://learnku.com/)
- Laravel学院
[https://www.golaravel.com/](https://www.golaravel.com/)

## Composer
- Composer镜像加速
[https://learnku.com/articles/15977/composer-accelerate-and-modify-mirror-source-in-china](https://learnku.com/articles/15977/composer-accelerate-and-modify-mirror-source-in-china)

- Composer 安装问题
执行`composer install`会导致版本检查问题，可以变成：`composer install/update --ignore-platform-reqs`<br />
参考：
[https://blog.csdn.net/weixin_34283445/article/details/91735405](https://blog.csdn.net/weixin_34283445/article/details/91735405)

- laravel框架下执行`composer intall` 导致一些问题:
因为没有添加相应的版本依赖,更新导致版本缺失的问题（问题及解决方案：[https://www.e-learn.cn/topic/3332506](https://www.e-learn.cn/topic/3332506)）
添加依赖：`"symfony/translation": "4.3.8"`即可