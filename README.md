# eCMF简介
laravel 5.5 voyager

## 一、 安装指南

### 1.1 安装 Laravel 5.5

使用 Laravel 安装器

<pre><code>laravel new blog</code></pre>

或通过 Composer Create-Project

<pre><code>composer create-project --prefer-dist laravel/laravel blog 5.5.*</code></pre>

### 1.2 安装扩展包

#### A. tcg/voyager

> 详见：[https://github.com/the-control-group/voyager](https://github.com/the-control-group/voyager) 

##### (1) 增加Voyager软件包

<pre><code>composer require tcg/voyager</code></pre>

##### (2) 修改.env配置文件

注：如果使用的环境是laradock，则内容应当如下： 

<pre>
APP_URL=http://blog.app

DB_HOST=mysql
DB_DATABASE=blog
DB_USERNAME=root
DB_PASSWORD=root
</pre>

##### (3) 运行安装程序(如不需要演示数据，请删除参数<code> --with-dummy </code>)

<pre><code>php artisan voyager:install --with-dummyg</code></pre>

> **email:** admin@admin.com  **password:** password

##### (4) 修改.env配置文件

注：当数据库中没有当前用户时，才会创建虚拟用户。

如果您没有使用虚拟用户，则可能希望为现有用户分配管理员权限。

这可以通过运行以下命令轻松完成：

<pre><code>php artisan voyager:admin your@email.com</code></pre>

如果你想创建一个新的管理员用户，你可以通过这样的--create标志：

<pre><code>php artisan voyager:admin your@email.com --create</code></pre>

并提示您输入用户名和密码。

##### (5) 其它修改

在.gitignore中增加以下内容

<pre><code>/public/vendor</code></pre>





