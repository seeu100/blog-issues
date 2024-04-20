报错如下：
Enforce HTTPS — Unavailable for your site because your domain is not properly configured to support HTTPS ([`00002000.xyz`](http://00002000.xyz/)) — [Troubleshooting custom domains](https://docs.github.com/articles/troubleshooting-custom-domains/#https-errors)

HTTPS provides a layer of encryption that prevents others from snooping on or tampering with traffic to your site.
When HTTPS is enforced, your site will only be served over HTTPS. [Learn more about securing your GitHub Pages site with HTTPS](https://docs.github.com/articles/securing-your-github-pages-site-with-https).

原因：用cloudflare解析DNS记录并开启了代理（proxy），先**取消代理**即可。
运行命令：`dig blog.00002000.xyz +noall +answer -t A`

开启代理时：
```
blog.00002000.xyz.      600     IN      A       104.21.82.3
blog.00002000.xyz.      600     IN      A       172.67.167.86
```

关闭代理时：
```
blog.00002000.xyz.      300     IN      CNAME   seeu100.github.io.
seeu100.github.io.      3600    IN      A       185.199.108.153
seeu100.github.io.      3600    IN      A       185.199.109.153
seeu100.github.io.      3600    IN      A       185.199.110.153
seeu100.github.io.      3600    IN      A       185.199.111.153
```