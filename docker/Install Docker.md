*Instalar dependencias necesarias

```sh
~apt install apt-transport-https ca-certificates gnupg curl software-properties-common apparmor-utils avahi-daemon jq network-manager socat qrencode
```

## Añadir la llave GPG publica al almacén de llaves de APT

- Desde el Repositorio Oficial de Docker

> [!Info]
>Si estas en Cuba y deseas usar la llave GPG del Repositorio Oficial de Docker, obligatoriamente debes usar un VPN, dado que no es accesible directamente desde nuestro país.

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

- Desde un Repositorio de Terceros

> [!Info]
>Si se usa un repositorio de terceros, o mirror, no es necesario el uso de VPN.

```sh
curl -x ip:puerto -U usuario:contraseña -fsSL https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```


## Agregar origen del Repositorio de Docker en el APT

Comencemos editando nuestro ==source.list== . Para no hacer el ajuste a mano usare el comando *sed* para cambiar el repo de archive.ubuntu.com a *repos.uclv.edu.cu*

```sh
~sed -i 's/archive.ubuntu.com/repos.uclv.edu.cu/' /etc/apt/sources.list
~sed -i 's/security.ubuntu.com/repos.uclv.edu.cu/' /etc/apt/sources.list
```


*Actualizamos el sistema:

```sh
~apt update && apt upgrade -y
```


## Instalamos docker y docker-compose:

```sh
~apt install docker.io docker-compose -y
```

comprobamos:

```sh 
~docker version
~systemctl is-active docker
~docker-compose version
```

## Crear el *daemon.json* para usar como mirror el registro de la universidad de las villas.

```sh
~nano /etc/docker/daemon.json
```
 
Agregamos las siguientes lineas:

```sh
{
        "max-concurrent-downloads": 4,
        "registry-mirrors": [
                "https://docker.uclv.cu" ]
}
```

o

```sh
{
    "max-concurrent-downloads": 4,
    "registry-mirrors": [
    "https://docker.uclv.cu",
    "https://rw21enj1.mirror.aliyuncs.com",
    "https://dockerhub.azk8s.cn",
    "https://reg-mirror.qiniu.com",
    "https://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn",
    "https://1nj0zren.mirror.aliyuncs.com",
    "https://quay.io",
    "https://docker.mirrors.ustc.edu.cn",
    "http://f1361db2.m.daocloud.io",
    "https://registry.docker-cn.com"
    ]
}
```

```sh
~service docker restart
```

## Configurar Proxy para Docker

```sh
~mkdir -p /etc/systemd/system/docker.service.d
```

```sh
~nano /etc/systemd/system/docker.service.d/https-proxy.conf
```

adicionar:
```sh
[Service]
Environment="HTTP_PROXY=http://10.142.7.44:3128"
Environment="HTTPS_PROXY=http://10.142.7.44:3128/"
Environment="NO_PROXY="localhost,127.0.0.1,::1"
```

```sh
~systemctl daemon-reload && systemctl restart docker
```


```sh
~docker run docker.uclv.cu/hello-world
```

docker pull docker.uclv.cu/rutaimagen:tag

# Script automático hasta aqui:

```sh
#!/bin/bash
 
# cambiando source.list
echo "Cambiando source.list de archive.ubuntu.com a repos.uclv.edu.cu"
 
sudo sed -i 's/archive.ubuntu.com/repos.uclv.edu.cu/' /etc/apt/sources.list
sudo sed -i 's/security.ubuntu.com/repos.uclv.edu.cu/' /etc/apt/sources.list
 
# actualizando
 
apt update && sudo apt upgrade -y
 
# Instalando docker docker-compose
 
apt install docker.io docker-compose -y
 
# creando daemon.json
 
bash -c 'cat << EOF > /etc/docker/daemon.json

{
        "max-concurrent-downloads": 4,
        "registry-mirrors": [
                "https://docker.uclv.cu"
        ]
}

EOF'
sleep 10
# Reiniciando docker
echo "Reiniciando servicio docker"
sudo service docker restart
```

## Añadir repositorios 

>[!nota]
>Una buena práctica es añadir las líneas de orígenes de Repositorios en el APT dentro de la ubicación /etc/apt/sources.list.d en un archivo independiente .list; no es elegante ubicar todo dentro de /etc/apt/sources.list.

Repositorio de Terceros:

echo "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu versionqueusas stable" |  tee /etc/apt/sources.list.d/docker-ce.list



## Instalación de los paquetes de Docker CE

- Primero actualizamos las listas de paquetes del APT:

```sh
~apt update && sudo apt full-upgrade -y
```

- Y luego instalamos los paquetes correspondientes:

```sh
~apt install docker-ce docker-compose docker-ce-cli containerd.io -y
```


	Nota:  En las distribuciones basadas en DEB el servicio se inicia automáticamente. En las distribuciones basadas en RPM, es necesario iniciarlo manualmente mediante el comando systemctl o service. 

Referencia: https://docs.docker.com/engine/install/ubuntu/
https://www.sysadminsdecuba.com/2021/07/instalando-docker-usando-la-red-cubana/

https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.74c61b11Y0eZoS


Corre una imagen de docker
docker run
Mostrar los container que están corriendo
docker ps
	Mostrar las imágenes descargadas
docker images
Mostar información de la configuración de Docker
docker info
Traer una imagen
docker pull
Etiquetar una imagen
docker tag

docker search imagename
docker image ls



https://github.com/pawelmalak/snippet-box
```yml
version: '3'
services:
  snippet-box:
    image: pawelmalak/snippet-box:latest
    container_name: snippet-box
    volumes:
      - /path/to/host/data:/app/data
    ports:
      - 5000:5000
    restart: unless-stopped
```
