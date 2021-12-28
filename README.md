
访问地址： [https://bytesfly.github.io/elasticsearch-sql](https://bytesfly.github.io/elasticsearch-sql)

> parse sql into elasticsearch dsl with antlr4, support jdbc.

Wiki： [https://github.com/iamazy/elasticsearch-sql/wiki](https://github.com/iamazy/elasticsearch-sql/wiki)

本文档仓库地址： [https://github.com/bytesfly/elasticsearch-sql/tree/gh-pages](https://github.com/bytesfly/elasticsearch-sql/tree/gh-pages)

> [!WARNING]
> 文档正在完善中，欢迎测试使用时给文档仓库提`PR`。

如何在本地编辑文档并实时预览效果呢？

## 本地部署

- 第一步： 克隆文档项目

```bash
git clone -b gh-pages https://github.com/bytesfly/elasticsearch-sql.git

mv elasticsearch-sql elasticsearch-sql-doc
```

注意： 该文档所在分支是`gh-pages`，如果你`Fork`了该项目，克隆地址换成你自己的仓库地址(比如：`git@github.com:bytesfly/elasticsearch-sql.git`)。

另外为了避免混淆，这里把文档目录直接改成`elasticsearch-sql-doc`。

- 第二步： 安装启动nginx

`Linux`系统：

```bash
# 安装
sudo apt-get install nginx

# 查看状态
sudo systemctl status nginx

# 启动
sudo systemctl start nginx
```

`Windows`系统(待补充)：
```bash
# TODO
```
如果安装启动成功，浏览器打开 [http://localhost/](http://localhost/) ，可见如下界面：

![](https://img2020.cnblogs.com/blog/1546632/202112/1546632-20211219104217494-963491267.png)

- 第三步： 配置nginx

`Linux`系统：

```bash
# 进入nginx配置目录
cd /etc/nginx/conf.d

# 创建新配置
sudo touch elasticsearch-sql-doc.conf
```
然后打开`elasticsearch-sql-doc.conf`，添加如下内容：
```text
server {
  listen 8080;
  root /home/bytesfly/proj/doc/elasticsearch-sql-doc;
  index index.html;
  location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt){
    root /home/bytesfly/proj/doc/elasticsearch-sql-doc;
  }
}
```
其中`root`后面配置的是刚才克隆的`elasticsearch-sql`项目文档绝对路径。

再执行命令让`nginx`重新加载：

```bash
sudo nginx -s reload
```

浏览器打开 [http://localhost:8080/](http://localhost:8080/) 即可预览文档效果。

> [!WARNING]
> 此时，用你喜欢的本地编辑器编写`Markdown`文档并保存，浏览器刷新页面(`Ctrl + F5`)即可实时预览效果。

## Docker部署

当然，如果本地有Docker环境，也可使用Docker部署。下面以`docker-compose`为例。

下面是整体目录结构，当前目录下有`docker-compose.yml`文件和`conf.d`文件夹，`conf.d`文件夹下有`elasticsearch-sql-doc.conf`文件。

```bash
➜  ~ tree
.
├── conf.d
│   └── elasticsearch-sql-doc.conf
└── docker-compose.yml

1 directory, 2 files
```

`docker-compose.yml`文件内容如下：

```yaml
version: '3.9'
services:
  nginx:
    image: nginx:1.20.1
    volumes:
      - ./conf.d:/etc/nginx/conf.d:ro
      - /home/bytesfly/proj/doc/elasticsearch-sql-doc:/var/www
    ports:
      - "8080:8080"
    networks:
      internal:  {}
    restart: always
networks:
  internal: {}
```

同理，其中`/home/bytesfly/proj/doc/elasticsearch-sql-doc`是文档项目所在绝对路径。

`elasticsearch-sql-doc.conf`文件内容如下：

```text
server {
  listen 8080;
  root /var/www;
  index index.html;
  location ~* ^.+\.(jpg|jpeg|gif|png|ico|css|js|pdf|txt){
    root /var/www;
  }
}
```

在`docker-compose.yml`当前目录执行如下命令：

```bash
sudo docker-compose up -d
```

如果没有其他问题的话，浏览器打开 [http://localhost:8080/](http://localhost:8080/) 查看文档。

## GitHub Pages部署

参考： [https://docsify.js.org/#/zh-cn/deploy](https://docsify.js.org/#/zh-cn/deploy)

仓库设置(`Settings`)页面开启`GitHub Pages`功能

![](https://img2020.cnblogs.com/blog/1546632/202112/1546632-20211228150538831-836353350.png)

然后，你就可以打开`https://<yourname>.github.io/elasticsearch-sql`看看效果了。
