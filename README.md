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

<pre><code>php artisan voyager:install --with-dummy</code></pre>

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

> 注：以下修改内容为非可选内容，只有当你更改了默认的User模型时才需要调整。本项目将使用默认的App\User.php。

在.gitignore中增加以下内容

<pre><code>/public/vendor</code></pre>

如果调整了默认的数据模型位置（例：将默认的模型存放位置为app\改为app\Models\，则还应当修改以下配置：


* vendor\tcg\voyager\src\Commands\InstallCommand.php

<pre><code>
        $this->info('Attempting to set Voyager User model as parent to App\User');
        if (file_exists(app_path('User.php'))) {
            $str = file_get_contents(app_path('User.php'));

            if ($str !== false) {
                $str = str_replace('extends Authenticatable', "extends \TCG\Voyager\Models\User", $str);

                file_put_contents(app_path('User.php'), $str);
            }
</code></pre>

改为

<pre><code>
        $userPath = config('voyager.user.default_path', app_path('User.php'));
        $this->info('Attempting to set Voyager User model as parent to ' . $userPath);        if (file_exists($userPath)) {
            $str = file_get_contents($userPath);

            if ($str !== false) {
                $str = str_replace('extends Authenticatable', "extends \TCG\Voyager\Models\User", $str);

                file_put_contents($userPath, $str);
            }
</code></pre>

并在config\voyager.php中user数组中增<code>'default_path'                 => app_path('Models/User.php'),</code>

<pre><code>
    'user' => [
        'add_default_role_on_register' => true,
        'default_role'                 => 'user',
        'namespace'                    => App\Models\User::class,       //Change
        'default_avatar'               => 'users/default.png',       
        'default_path'                 => app_path('Models/User.php'),  // Add
    ],
</code></pre>
            
* app\User.php

<pre><code>namespace App;</code></pre>

改为

<pre><code>namespace App\Models;</code></pre>


* config\auth.php

<pre><code>'model' => App\User::class,</code></pre>

改为

<pre><code>'model' => App\Models\User::class,</code></pre>

最后执行加载，并重新迁移数据

<pre><code>
composer dump-autoload
php artisan voyager:install --with-dummy
</code></pre>


