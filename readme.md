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
    CloudFlare Tunnel管理页 https://one.dash.cloudflare.com/ 加1个Public hostname 指向 `http://nginx:80`
6. (可选)关掉 自动程序攻击模式  
    由于探针上报日志频繁，可能会被CF误拦截导致无法正常工作。可以添加绕过规则（路径 security/waf/custom-rules 安全性-WAF-自定义规则）  
   规则内容，编辑表达式后粘贴以下： 
   ```
   (starts_with(http.request.uri.path, "/proto.NezhaService/") and starts_with(http.user_agent, "grpc-go/") and http.host eq "探针域名")
   ```
   采取措施：跳过  
   要跳过的 WAF 组件：全选
   
   部署即可。
7. (可选)将配置文件中的 TLS设置为True  
    编辑 data/config.yaml，找到 tls:这项，修改为 `tls: true` 后 保存。然后
   ```shell
   docker compose down && docker compose up -d
   ```
   重启服务生效
## Dashboard配置
/dashboard/settings  里面设置一下 
1. Agent对接地址【域名/IP:端口】  
上面的 Public hostname:443
2. 真实IP请求头  
可以写`nz-realip`或者`CF-Connecting-IP`

## AGENT
dashboard右上角复制安装命令，注意手动修改参数中的8008端口为443（如果你没有修改Agent对接地址），TLS改为True（如果你没有将配置文件中的 TLS设置为True）。
## DOCKER 安装 AGENT(可选)
...晚点写
## 其他  
后台地址 /dashboard  
默认密码 admin/admin
