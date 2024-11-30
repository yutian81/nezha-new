# 哪吒v1 Docker CloudFlare Tunnel版   
无需公网IP，全程都在CF下，项目优势：
1. 不暴露公网ip 防止被攻击
2. 单栈转双栈 ipv4 ipv6 都能用 纯ipv6🐔 也方便挂探针
3. 除境内网络外 走cf基本都优化
4. 开箱即用 迁移备份方便
## Dashboard安装
1. 安装好docker
1. 申请 CloudFlare Tunnel Token  
https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/
2. CloudFlare开启GRPC流量代理  
https://developers.cloudflare.com/network/grpc-connections/
3. 启动服务    
    ```shell
    git clone https://github.com/yumusb/nezha-new.git  
    ```
    编辑 .env 文件中的 TUNNEL_TOKEN 为自己申请的 
    ```shell
    docker compose up -d 
    ```
5. 服务端映射到CF  
    CloudFlare Tunnel管理页 https://one.dash.cloudflare.com/ 加1个Public hostname 指向 `http://dashboard:8008`
## Dashboard配置
/dashboard/settings  里面设置一下 
1. 仪表板服务器域名/IP（无 CDN）  
上面的 Public hostname
2. 真实IP请求头  
可以写`nz-realip`或者`CF-Connecting-IP`

## AGENT
官方脚本配置即可，注意手动修改8008端口为443。

## DOCKER 安装 AGENT(可选)
...晚点写
## 其他  
后台地址 /dashboard  
默认密码 admin/admin