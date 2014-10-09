MongoDB
=======

If you're afraid of running out of memory by using Redis, or want your forum to be more easily scalable, you can install NodeBB with MongoDB. This tutorial assumes you know how to SSH into your server and have root access.

**These instructions are for Ubuntu. Adjust them accordingly for your distro.**

**Note:** If you have to add ``sudo`` to any command, do so. No one is going to hold it against you ;)

第 1 步：安装 MongoDB
-------------------------

需要最新版的 MongoDB (或者至少高于包管理器的版本)。安装步骤可以参阅 `MongoDB 手册 <http://docs.mongodb.org/manual/administration/install-on-linux/>`_)。

第 2 步：安装 node.js
-------------------------

和 MongoDB 一样，需要, the latest and greatest node.js is required (or at least greater than the package manager), so I'm leaving this to the official wiki. The instructions to install can be found on `Joyent <https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager>`_.

**Note: NPM is installed along with node.js, so there is no need to install it separately**

第 3 步: 安装基础软件
-------------------------

Enter the following into the terminal to install the base software required to run NodeBB:

.. code:: bash

    # apt-get install git build-essential imagemagick

第 4 步: 克隆代码库
-------------------------

Enter the following into the terminal, replacing `/path/to/nodebb/install/location` to where you would like NodeBB to be installed.

.. code:: bash

    $ cd /path/to/nodebb/install/location
    $ git clone git://github.com/NodeBB/NodeBB.git nodebb

第 5 步: 安装 NodeBB 依赖的软件
-------------------------

Go into the newly created `nodebb` directory and install the required dependencies by entering the following.

.. code:: bash

    $ cd nodebb
    $ npm install

第 6 步: 添加新数据库
-------------------------

进入 MongoDB 命令行，输入：

.. code:: bash

    $ mongo

添加数据库，命名为 `nodebb`，输入：

.. code::

    > use nodebb

添加访问 `nodebb` 数据库的用户，输入：

.. code::

    > db.createUser( { user: "nodebb", pwd: "<输入安全密码>", roles: [ "readWrite" ] } )

**提示:** 角色 ``readWrite`` 将制定数据库的读或写任何集合(collection)的权限付给用户。

第 7 步: 配置 MongoDB
-------------------------

MongoDB 需要启用文本搜索。修改 ``/etc/mongodb.conf``。

.. code::

    # nano /etc/mongodb.conf

在末尾添加 ``setParameter=textSearchEnabled=true``。并且启用认证，取消注释 ``auth = true``。然后重启 MongoDB。

.. code::

    # service mongodb restart

第 8 步: 配置 NodeBB
-------------------------

确认你在 NodeBB 的根目录下。如果不在，输入：

.. code::

    $ cd /path/to/nodebb

安装应用，输入：

.. code::

    $ node app --setup

* 更改 hostname 为你的域名。
* 敲回车接受默认设置。只到询问吧你使用什么数据库，在此输入 ``mongo``。
* Accept the default port, unless you changed it in the previous steps.
* Change your username to ``nodebb``, unless you set it to another username.
* Enter in the password you made in step 5.
* Change the database to ``nodebb``, unless you named it something else.

Continue with the installation, following the instructions the installer provides you.

第 9 步: 启动应用
-------------------------

启用应用，运行：

.. code::

    $ node app

现在访问 ``yourdomainorip.com:4567`` 然后你安装的 NodeBB 已经在运行了。

NodeBB 可以由其他的辅助程序启动，例如 :doc:`supervisor 或 forever <../../running/index>`。你同样可以用 ``nginx`` 做 :doc:`反向代理 <../../configuring/proxies>`)。
