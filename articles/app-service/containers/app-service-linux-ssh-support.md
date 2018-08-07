---
title: Linux üzerinde Azure App Service için SSH desteği | Microsoft Docs
description: Linux üzerinde Azure App Service ile SSH kullanma hakkında bilgi edinin.
keywords: Azure app service, web uygulaması, linux, oss
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: 63fdf2cc82438fe55792b12244dd697721adda15
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579587"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service için SSH desteği

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) yönetim komutları uzaktan komut satırı terminalden yürütmek için yaygın olarak kullanılır. Linux'ta App Service, her yeni web uygulaması için çalışma zamanı yığını olarak kullanılan yerleşik Docker görüntüleri uygulama kapsayıcı içine SSH desteği sağlar. 

![Çalışma zamanı yığınları](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

SSH sunucusu özel görüntünüzü yapılandırarak özel Docker görüntüleri için.

Ayrıca, kapsayıcıya SSH ve SFTP kullanarak doğrudan yerel geliştirme makinenize bağlanabilirsiniz.

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

Kapsayıcınızı ile bir SSH istemcisi bağlantı kurmak için uygulamanızı çalıştırıyor olmalıdır.

Aşağıdaki URL'yi değiştirin ve tarayıcı yapıştırın \<app_name > Uygulama adınız:

```
https://<app_name>.scm.azurewebsites.net/webssh/host
```

Önceden doğrulanmış değil, Azure aboneliğinizle bağlanmak için kimlik doğrulaması için gereklidir. Kimlik doğrulandıktan sonra bir tarayıcı içi Kabuk burada kapsayıcınızın komutları çalıştırabilirsiniz görürsünüz.

![SSH bağlantısı](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="use-ssh-support-with-custom-docker-images"></a>SSH desteği ile özel Docker görüntülerinizi kullanın

Azure portalında kapsayıcı ile istemci arasındaki SSH iletişimi desteklemek özel bir Docker görüntüsü için sırada Docker görüntünüzü için aşağıdaki adımları gerçekleştirin.

Azure App Service deposu olarak bu adımları gösterilen [örneği](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Dahil `openssh-server` yüklemesinde [ `RUN` yönerge](https://docs.docker.com/engine/reference/builder/#run) görüntü ve kök parolası hesap için kümesi için bir Dockerfile içinde `"Docker!"`.

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu erişilebilir / yayımlama kimlik bilgilerini kullanarak kimlik doğrulaması SCM sitesine.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Ekleme bir [ `COPY` yönerge](https://docs.docker.com/engine/reference/builder/#copy) Dockerfile kopyalamak için bir [sshd_config](http://man.openbsd.org/sshd_config) dosyasını */etc/ssh/* dizin. Yapılandırma dosyanızı sshd_config dosyasında Azure App Service GitHub deposunda dayanmalıdır [burada](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > *Sshd_config* bağlantı başarısız veya dosyası şunları içermelidir: 
    > * `Ciphers` aşağıdakilerden en az birini içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` aşağıdakilerden en az birini içermelidir: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Bağlantı noktasına 2222 Et [ `EXPOSE` yönerge](https://docs.docker.com/engine/reference/builder/#expose) Dockerfile için. Kök parolası biliniyor olsa da, 2222 numaralı bağlantı noktasına İnternet üzerinden erişilemez. Bir iç yalnızca bağlantı noktası erişilebilir yalnızca özel bir sanal ağın köprü ağı içinde kapsayıcılara göre var.

    ```docker
    EXPOSE 2222 80
    ```

1. Bir kabuk betiği kullanarak SSH hizmetini başlatmak emin olun (bkz örneğe [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

Dockerfile'ı kullanan [ `ENTRYPOINT` yönerge](https://docs.docker.com/engine/reference/builder/#entrypoint) betiği çalıştırmak için.

    ```docker
    COPY startup /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="open-ssh-session-from-remote-shell"></a>Uzak kabuğundan SSH oturumu açın

> [!NOTE]
> Bu özellik şu anda Önizleme aşamasındadır.
>

TCP, tünel kullanarak bir geliştirme makineniz ve kapsayıcılar için Web uygulaması arasında ağ bağlantısı üzerinden kimliği doğrulanmış bir WebSocket bağlantısı oluşturabilirsiniz. Tercih ettiğiniz istemciden App Service'te çalışan kapsayıcınıza bir SSH oturumu açmak sağlar.

Başlamak için yüklemeniz gerekir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLI'yı yüklemeden nasıl çalıştığını görmek için [Azure Cloud Shell](../../cloud-shell/overview.md). 

Çalıştırarak en son uygulama hizmeti uzantısı Ekle [az uzantısı ekleme](/cli/azure/extension?view=azure-cli-latest#az-extension-add):

```azurecli-interactive
az extension add --name webapp
```

Zaten çalıştırırsanız, `az extension add` önce çalıştırmak [az uzantı güncelleştirmesinin](/cli/azure/extension?view=azure-cli-latest#az-extension-update) bunun yerine:

```azurecli-interactive
az extension update --name webapp
```

Uygulamayı kullanarak uzak bir bağlantı açmak [az webapp uzaktan bağlantı oluşturma](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) komutu. Belirtin  _\<abonelik\_kimliği >_,  _\<grubu\_adı >_ ve \_< app\_adı > _, uygulamanız için değiştirin \<bağlantı noktası > yerel bağlantı noktası numarası.

```azurecli-interactive
az webapp remote-connection create --subscription <subscription_id> --resource-group <group_name> -n <app_name> -p <port> &
```

> [!TIP]
> `&` Cloud Shell kullanıyorsanız komutu sonunda yalnızca kolaylık sağlamaya yöneliktir. Sonraki komut aynı shell'de çalıştırabilmeniz için işlem arka planda çalışır.

Komut çıktısı, bir SSH oturumu açmak gereken bilgileri sağlar.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Yerel bağlantı noktasını kullanarak istemci tercih ettiğiniz ile kapsayıcınız ile bir SSH oturumu açın. Aşağıdaki örnekte varsayılan [ssh](https://ss64.com/bash/ssh.html) komutu:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

İstenen zaman yazın `yes` bağlanmaya devam etmek için. Ardından, parola istenir. Kullanım `Docker!`, hangi için başında gösterilen.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Kimlik doğrulaması yaptıkları sonra oturumu Hoş Geldiniz ekranını görmeniz gerekir.

```
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  >
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
```

Şimdi, Bağlayıcınızı bağlanır. 

Çalıştırmayı deneyin [üst](https://ss64.com/bash/top.html) komutu. İşlem, uygulamanızın işlem görebilmeniz gerekir. Aşağıdaki örnek çıktıda, sahip olduğu `PID 263`.

```
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
```

## <a name="next-steps"></a>Sonraki adımlar

Hakkında sorularınızı ve çekincelerinizi gönderebilir [Azure Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Kapsayıcılar için Web Apps hakkında daha fazla bilgi için bkz:

* [VS Code'den Azure App Service'te Node.js uygulamaları uzaktan hata ayıklama ile tanışın](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)
