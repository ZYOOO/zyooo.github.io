# Blog搭建和我的信息


# blog搭建和我的信息

现在已经安装好了hugo  在cmd中输入 hugo version可以查询

## 创建启动

hugo new site [path] [flags] 创建

这里是新创建了一个hugo new site F:\myblog blog

到 themes.gohugo.io里面去下载主题直接在cmd中clone

cd [path]

git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c

然后启动    	hugo server -t m10c --buildDrafts

Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)

通过本地链接就可以访问了

## 写一篇文章

hugo new post/hello.md

git 令牌

## 生成public文件夹

```go
hugo --theme=m10c --baseUrl="https://zyooo.github.io/"  --buildDrafts
```

每一次添加东西先生成然后commit   push

## git操作

到public目录下

git add .

git commit -m" "

git push origin main


