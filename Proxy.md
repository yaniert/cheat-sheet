#linux #script #proxy  

# Proxificando linux

### Configuración de proxy para apt-get, apt, aptitude:

```sh  
nano /etc/apt/apt.conf 
```

 ```sh
 Acquire::http::Proxy "http://10.142.7.44:3128";
 Acquire::https::Proxy "http://10.142.7.44:3128";
 Acquire::ftp::Proxy "http://10.142.7.44:3128"; 
 ```


### Proxy para wget:

```sh
nano /etc/wgetrc
```

```sh
https_proxy = http://10.142.7.44:3128/
http_proxy = http://proxyserver:puerto/
ftp_proxy = http://proxyserver:puerto/
```


### Configuración de proxy para nodejs(npm)

```sh
npm config set proxy http://username:password@proxyserver:puerto
npm config set https-proxy http://username:password@proxyserver:puerto
```


### Configurar proxy para git

```bash
git config --global http.proxy http//proxyserver:puerto
git config --global https.proxy https//proxyserver:puerto

En caso de tener conexión directa y necesitemos quitarle la configuración de proxy:

git config --global --unset http.proxy
git config --global --unset https.proxy
``` 
  
  [link](https://www.sysadminsdecuba.com/2017/11/tips-configuracion-de-proxy-para-los-servicios-de-linux/)
