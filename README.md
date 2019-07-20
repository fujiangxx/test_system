本项目是一款基于nodejs的关于考试的webApp,主体分为后台管理系统、教师端、学生端，其前端页面是用vue技术编写，后台是用nodejs的express框架搭建，数据库使 用的是mysql,处理前后台的逻辑使用的是javascript技术，其中搭建后台管理系统可视化页面使用了element-ui搭建。

##后台管理系统可视化页面的功能

*实现了教师、学生的信息的管理:教师、学生的信息的单条添加及多条添加(excel文件上传)、查询数据库中教师和学生的信息(数据繁多时以10条为限做了分页)，在查询出的信息中可通过姓名检索（ps:在mysql中储存性别的数字 男 1 保密 2 女 3 ）

*实现了班级的添加及查询（该部分未做其编辑与删除功能，感觉不合理）

*实现了试题管理 试题管理分为方向的添加及查询、类型的添加及查询、试题的添加

##学生端实现的功能

*学生的个人信息的修改，包括头像的上传和姓名、性别、班级的修改及个人密码的修改(在个人主页的齿轮按钮或主页头像点击进入)
关于头像上传及更改是将选中的图片上传到本地服务器上，并不是上传到其他的云服务器上,且用户获取头像是写了一个关于uploads的代理
注:后台管理系统可视化页面并未对用户的头像进行增删改查的操作，即全由教师端和学生端自己上传或更改

*基本操作:学生考试及查询已考的试题 其中学生的个人信息修改使用vuex

##教师端实现的功能

*教师的个人信息修改与学生端的操作基本一致

*基本操作:教师组题(将后台管理系统添加的试题给与相应的分数 组给某班级) 批阅已考的试题的简答题，并更新分数


#vue打包需要的准备

*build的utils.js   需要添加publicPath:'../../'

    return ExtractTextPlugin.extract({
      use: loaders,
      publicPath: '../../',
      fallback: 'vue-style-loader'
    })

*config的index.js   修改代理   及修改路径

    proxyTable: {
        '/api': {
            target: '119.23.230.65:80', // 接口的域名
            // secure: false,  // 如果是https接口，需要配置这个参数
            changeOrigin: true, // 如果接口跨域，需要进行这个参数配置
            pathRewrite: {
                '^/api': '/api'
            }
        },
        //代理，访问上传的附件
        '/upload': {
            target: '119.23.230.65:80/uploads',
            changeOrigin: true,
            secure: false,  // 如果是https接口，需要配置这个参数
            pathRewrite: {
                '^/upload': ''
            },
        }
    
    },

    assetsPublicPath: '/',改为assetsPublicPath: './',

#部署到阿里云轻量级服务器上的步骤

*1、购买阿里云的轻量级服务器及域名  并下载xshell及xftp

*2、xshell远程连接服务器上使用docker安装mysql 并登录进mysql给普通用户远程连接的权限（授予权限），并使用navicat for mysql 远程连接服务器的mysql数据库(阿里云上使用数据管理DMS的公网自建库)

*3、xshell 上node -v  npm -v 检查node及npm的版本 ，并下载pm2(pm2用于启动服务器 即关闭xshell服务器仍启动)  使用xftp上传本地服务器文件(vue已打包并放置在服务器文件中)  上传完成后，使用PM2启动    pm2 start app   ,最后进入浏览器打开云服务器的ip地址查看项目运行情况

*尚存在的问题

1、在我关闭xshell一段时间(半天或一天),项目的3个端口登录页无法实现登录操作，即进不去主页，使用xshell检查服务器状态（pm2 list)显示运行中，且docker ps显示mysql容器并未关闭（自杀）,最后pm2 restart app 则恢复正常。原因尚且不明

2、在上传或更改头像时，在本地运行是ok的，但云服务器上 数据库存储的路径应该为   uploads/时间戳+图片名.图片后缀  结果却为uploads//时间戳+图片名.图片后缀

部署成功后的地址及相关信息(域名尚未备案)

*后台管理系统  访问地址   http://119.23.230.65/admin/index.html     用户     admin       密码    123456

*学生端    访问地址   http://119.23.230.65/stu/index.html        用户     小江           密码    123456

*教师端   访问地址  http://119.23.230.65/teach/index.html       用户      符一           密码  123456



