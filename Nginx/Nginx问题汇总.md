##### 负载均衡

1. 轮询（默认）

2. Weight

   ```bash
   upstream bakend {    
   server 192.168.0.14 weight=10;    
   server 192.168.0.15 weight=10;    
   }
   ```

3. IP Hash

   ```bash
   upstream bakend {    
   ip_hash;    
   server 192.168.0.14:88;    
   server 192.168.0.15:80;    
   } 
   ```

4. URL Hash

   ```bash
   upstream backend {    
   server squid1:3128;    
   server squid2:3128;    
   hash $request_uri;    
   hash_method crc32;    
   }  
   ```

   

5. Fair 根据响应时间分法请求，响应时间越短分发的请求越多。

   ```bash
   upstream backend {    
   server server1;    
   server server2;    
   fair;    
   } 
   ```

   

