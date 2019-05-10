---
title: Azure Container ınstances'da SSL'i etkinleştirin
description: Çalıştıran Azure Container Instances'da bir kapsayıcı grubu için bir SSL veya TLS uç noktası oluşturma
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 12de4ef31084d8ac8586c79ffe3d0a8e891727bf
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411395"
---
# <a name="enable-an-ssl-endpoint-in-a-container-group"></a>Bir kapsayıcı grubundaki bir SSL uç noktayı etkinleştirme

Bu makalede nasıl oluşturulacağını gösterir. bir [kapsayıcı grubu](container-instances-container-groups.md) bir uygulama kapsayıcısı ve bir SSL Sağlayıcısı'nı çalıştıran bir sepet kapsayıcısı. Bir kapsayıcı grubu ayrı bir SSL uç noktası ayarlayarak, SSL bağlantıları, uygulamanız için uygulamanın kodunu değiştirmeden sağlar.

Size iki kapsayıcı içeren bir kapsayıcı grubu ayarlayın:
* Genel Microsoft kullanarak basit bir web uygulaması çalıştıran bir uygulama kapsayıcısı [aci-helloworld](https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld) görüntü. 
* Ortak çalışan bir sepet kapsayıcısı [Ngınx](https://hub.docker.com/_/nginx) görüntü, SSL kullanacak şekilde yapılandırılmış. 

Bu örnekte, kapsayıcı grubunun yalnızca bağlantı noktası 443 Nginx için genel IP adresiyle kullanıma sunar. Ngınx dahili olarak 80 numaralı bağlantı noktasında dinler Yardımcısı web uygulaması için HTTPS istekleri yönlendirir. Örnek kapsayıcı uygulamalar, diğer bağlantı noktalarında dinlemek için uyarlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu makaleyi tamamlamak için Azure Cloud Shell veya Azure CLI'ın yerel bir yüklemesi'ni kullanabilirsiniz. Yerel olarak 2.0.55 sürümü kullanmak istiyorsanız veya üzeri önerilir olur. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Ngınx SSL sağlayıcısı olarak ayarlamak için bir SSL sertifikası gerekir. Bu makale oluşturma ve otomatik olarak imzalanan SSL sertifika ayarlama. Üretim senaryoları için bir sertifika yetkilisinden bir sertifika edinmeleri gerekir.

Otomatik olarak imzalanan bir SSL sertifikası oluşturmak için kullanın [OpenSSL](https://www.openssl.org/) Azure Cloud Shell ve birçok Linux dağıtımı kullanılabilir aracı veya işletim sisteminizde bir karşılaştırılabilir istemci aracı kullanın.

İlk kez bir sertifika isteği (.csr dosyası) bir yerel çalışma dizininde oluşturun:

```console
openssl req -new -newkey rsa:2048 -nodes -keyout ssl.key -out ssl.csr
```

Kimlik bilgilerini eklemek için yönergeleri izleyin. Ortak ad için sertifikayla ilişkili ana bilgisayar adı girin. Bir parola istendiğinde parola atlamak için yazmadan Enter tuşuna basın.

Sertifika isteğinden (.crt dosyası) otomatik olarak imzalanan sertifika oluşturmak için aşağıdaki komutu çalıştırın. Örneğin:

```console
openssl x509 -req -days 365 -in ssl.csr -signkey ssl.key -out ssl.crt
```

Artık üç dosyalarını şu dizinde görmeniz gerekir: sertifika isteğini (`ssl.csr`), özel anahtarı (`ssl.key`) ve kendinden imzalı bir sertifika (`ssl.crt`). Kullandığınız `ssl.key` ve `ssl.crt` sonraki adımlarda.

## <a name="configure-nginx-to-use-ssl"></a>SSL kullanacak şekilde Ngınx'i yapılandırın

### <a name="create-nginx-configuration-file"></a>Ngınx yapılandırma dosyası oluşturma

Bu bölümde, SSL kullanacak şekilde Ngınx'i için bir yapılandırma dosyası oluşturun. Başlangıç adlı yeni bir dosya aşağıdaki metni kopyalayarak`nginx.conf`. Azure Cloud Shell'de, çalışma dizininizde dosya oluşturma için Visual Studio Code kullanabilirsiniz:

```console
code nginx.conf
```

İçinde `location`, ayarladığınızdan emin olun `proxy_pass` uygulaması için doğru bağlantı noktasına sahip. Bu örnekte, bağlantı noktası 80 için ayarladığımız `aci-helloworld` kapsayıcı.

```console
# nginx Configuration File
# https://wiki.nginx.org/Configuration

# Run as a less privileged user for security reasons.
user nginx;

worker_processes auto;

events {
  worker_connections 1024;
}

pid        /var/run/nginx.pid;

http {

    #Redirect to https, using 307 instead of 301 to preserve post data

    server {
        listen [::]:443 ssl;
        listen 443 ssl;

        server_name localhost;

        # Protect against the BEAST attack by not using SSLv3 at all. If you need to support older browsers (IE6) you may need to add
        # SSLv3 to the list of protocols below.
        ssl_protocols              TLSv1 TLSv1.1 TLSv1.2;

        # Ciphers set to best allow protection from Beast, while providing forwarding secrecy, as defined by Mozilla - https://wiki.mozilla.org/Security/Server_Side_TLS#Nginx
        ssl_ciphers                ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:ECDHE-RSA-RC4-SHA:ECDHE-ECDSA-RC4-SHA:AES128:AES256:RC4-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!3DES:!MD5:!PSK;
        ssl_prefer_server_ciphers  on;

        # Optimize SSL by caching session parameters for 10 minutes. This cuts down on the number of expensive SSL handshakes.
        # The handshake is the most CPU-intensive operation, and by default it is re-negotiated on every new/parallel connection.
        # By enabling a cache (of type "shared between all Nginx workers"), we tell the client to re-use the already negotiated state.
        # Further optimization can be achieved by raising keepalive_timeout, but that shouldn't be done unless you serve primarily HTTPS.
        ssl_session_cache    shared:SSL:10m; # a 1mb cache can hold about 4000 sessions, so we can hold 40000 sessions
        ssl_session_timeout  24h;


        # Use a higher keepalive timeout to reduce the need for repeated handshakes
        keepalive_timeout 300; # up from 75 secs default

        # remember the certificate for a year and automatically connect to HTTPS
        add_header Strict-Transport-Security 'max-age=31536000; includeSubDomains';

        ssl_certificate      /etc/nginx/ssl.crt;
        ssl_certificate_key  /etc/nginx/ssl.key;

        location / {
            proxy_pass http://localhost:80; # TODO: replace port if app listens on port other than 80
            
            proxy_set_header Connection "";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $remote_addr;
        }
    }
}
```

### <a name="base64-encode-secrets-and-configuration-file"></a>Base64 kodlama gizli dizileri ve yapılandırma dosyası

Base64 ile kodlamanız Ngınx yapılandırma dosyasını, SSL sertifikası ve SSL anahtarı. Sonraki bölümde, kapsayıcı grubu dağıtmak için kullanılan bir YAML dosyası kodlanmış içeriğini girin.

```console
cat nginx.conf | base64 -w 0 > base64-nginx.conf
cat ssl.crt | base64 -w 0 > base64-ssl.crt
cat ssl.key | base64 -w 0 > base64-ssl.key
```

## <a name="deploy-container-group"></a>Kapsayıcı grubu dağıtma

Artık kapsayıcı yapılandırmalarında belirterek kapsayıcı grubu dağıtma bir [YAML dosyası](container-instances-multi-container-yaml.md).

### <a name="create-yaml-file"></a>YAML dosyası oluşturma

Adlı yeni bir dosyaya aşağıdaki YAML'ye kopyalayın `deploy-aci.yaml`. Azure Cloud Shell'de, çalışma dizininizde dosya oluşturma için Visual Studio Code kullanabilirsiniz:

```console
code deploy-aci.yaml
```

Base64 ile kodlanmış içeriği girin altında belirtilen yerlerde dosyaları `secret`. Örneğin, `cat` içeriğini görmek için base64 kodlamalı dosyaların her biri. Bu dosyalar eklenir, dağıtım sırasında bir [gizli birimi](container-instances-volume-secret.md) kapsayıcı grubunda. Bu örnekte, gizli birimi için Nginx kapsayıcısını bağlanmıştır.

```YAML
api-version: 2018-10-01
location: westus
name: app-with-ssl
properties:
  containers:
  - name: nginx-with-ssl
    properties:
      image: nginx
      ports:
      - port: 443
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - name: nginx-config
        mountPath: /etc/nginx
  - name: my-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  volumes:
  - secret:
      ssl.crt: <Enter contents of base64-ssl.crt here>
      ssl.key: <Enter contents of base64-ssl.key here>
      nginx.conf: <Enter contents of base64-nginx.conf here>
    name: nginx-config
  ipAddress:
    ports:
    - port: 443
      protocol: TCP
    type: Public
  osType: Linux
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

### <a name="deploy-the-container-group"></a>Kapsayıcı grubu dağıtma

Bir kaynak grubu oluşturun [az grubu oluşturma](/cli/azure/group#az-group-create) komutu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kapsayıcı grubu dağıtma [az kapsayıcı oluşturma](/cli/azure/container#az-container-create) YAML dosyası bağımsız değişken geçirme komutu.

```azurecli
az container create --resource-group <myResourceGroup> --file deploy-aci.yaml
```

### <a name="view-deployment-state"></a>Görünüm dağıtım durumu

Dağıtım durumunu görüntülemek için aşağıdakileri kullanın [az container show](/cli/azure/container#az-container-show) komutu:

```azurecli
az container show --resource-group <myResourceGroup> --name app-with-ssl --output table
```

Başarılı bir dağıtım için çıktı aşağıdakine benzer.

```console
Name          ResourceGroup    Status    Image                                                    IP:ports             Network    CPU/Memory       OsType    Location
------------  ---------------  --------  -------------------------------------------------------  -------------------  ---------  ---------------  --------  ----------
app-with-ssl  myresourcegroup  Running   mcr.microsoft.com/azuredocs/nginx, aci-helloworld        52.157.22.76:443     Public     1.0 core/1.5 gb  Linux     westus
```

## <a name="verify-ssl-connection"></a>SSL bağlantısını doğrulama

Çalışan uygulamayı görüntülemek için tarayıcınızda IP adresine gidin. Örneğin, bu örnekte gösterilen IP adresidir `52.157.22.76`. Kullanmalısınız `https://<IP-ADDRESS>` Ngınx sunucu yapılandırması nedeniyle, çalışan uygulamayı görmek için. İle bağlantı kurmayı dener `http://<IP-ADDRESS>` başarısız.

![Bir Azure kapsayıcı örneğinde çalışan uygulamayı gösteren tarayıcı ekran görüntüsü](./media/container-instances-container-group-ssl/aci-app-ssl-browser.png)

> [!NOTE]
> Bu örnekte otomatik olarak imzalanan bir sertifika ve sertifika yetkilisinden bir kullandığından, tarayıcı siteye HTTPS üzerinden bağlanırken bir güvenlik uyarısı görüntüler. Bu davranış beklenmektedir.
>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kapsayıcı grubu içinde çalışan bir web uygulaması için SSL bağlantılarını etkinleştirmeye yönelik Ngınx kapsayıcısı yapma gösterdi. Bu örnekte 80 numaralı bağlantı noktası dışındaki bağlantı noktaları üzerinde dinleme uygulamaları uyarlayabilirsiniz. Ngınx yapılandırma dosyasını otomatik olarak bağlantı noktası 80 (HTTP) HTTPS kullanmak üzere sunucu bağlantılarını yeniden yönlendirmek için de güncelleştirebilirsiniz.

Bu makalede sepet Ngınx kullanırken, başka bir SSL sağlayıcısı gibi kullanabileceğiniz [Caddy](https://caddyserver.com/).

Grubunda dağıtmak için bir kapsayıcı grubunda SSL etkinleştirmek için başka bir yaklaşım olan bir [Azure sanal ağı](container-instances-vnet.md) ile bir [Azure uygulama ağ geçidi](../application-gateway/overview.md). Ağ geçidi SSL uç noktası ayarlanabilir. Bir örnek görmek [dağıtım şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress-vnet) ağ geçidinde SSL sonlandırmayı etkinleştirme uyarlayabilirsiniz.
