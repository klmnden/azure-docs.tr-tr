---
title: Azure uygulama hizmeti Linux üzerinde SSH desteği | Microsoft Docs
description: Azure uygulama hizmeti Linux'ta ile SSH kullanma hakkında bilgi edinin.
keywords: Azure uygulama hizmeti, web uygulaması, linux, oss
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
ms.openlocfilehash: c2beb67a27b667d31402b903f38dbf116e9425d0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34301084"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Azure uygulama hizmeti Linux üzerinde SSH desteği

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) yönetim komutları uzaktan komut satırı terminal durumundan yürütmek için yaygın olarak kullanılır. Linux üzerinde App Service, her yeni web uygulamaları için çalışma zamanı yığınını kullanılan yerleşik Docker görüntülerinin uygulama kapsayıcısı içine SSH desteği sağlar. 

![Çalışma zamanı yığınları](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Özel görüntünüzü SSH sunucusu yapılandırarak özel Docker görüntüler için.

SSH ve SFTP kullanarak doğrudan yerel geliştirme makinenizden kapsayıcıya da bağlanabilirsiniz.

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda Aç SSH oturumu

Bir SSH istemci bağlantısı, kapsayıcısı ile yapmak için uygulamanızın çalıştırması gerekir.

Tarayıcı ve Değiştir URL'sini yapıştırın \<app_name > Uygulama adı ile:

```
https://<app_name>.scm.azurewebsites.net/webssh/host
```

Önceden doğrulanmış değil, Azure aboneliğinizle bağlanmak için kimlik doğrulaması için gereklidir. Kimliği doğrulanmış sonra bir tarayıcı içi Kabuk burada kapsayıcısı içindeki komutları çalıştırabilirsiniz bakın.

![SSH bağlantısı](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="use-ssh-support-with-custom-docker-images"></a>SSH desteği özel Docker görüntülerle kullanın

Azure portalında kapsayıcı ve istemci arasındaki SSH iletişimi desteklemek özel bir Docker görüntü için sırayla Docker görüntüsü için aşağıdaki adımları gerçekleştirin.

Azure uygulama hizmeti deposu olarak bu adımları gösterilen [örnek](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Dahil `openssh-server` yüklemesinde [ `RUN` yönerge](https://docs.docker.com/engine/reference/builder/#run) Dockerfile görüntü ve kök parolasını hesap için kümesi içinde `"Docker!"`.

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu erişilebilir / yayımlama kimlik bilgileri kullanılarak kimlik doğrulaması SCM Site.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Ekleme bir [ `COPY` yönerge](https://docs.docker.com/engine/reference/builder/#copy) Dockerfile kopyalamak için bir [sshd_config](http://man.openbsd.org/sshd_config) dosya */vb./ssh/* dizin. Yapılandırma dosyanızı Azure App Service GitHub deposuna sshd_config dosyasında dayanmalıdır [burada](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > *Sshd_config* dosyası şunları içermelidir veya bağlantı başarısız olur: 
    > * `Ciphers` aşağıdakilerden en az birini içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` aşağıdakilerden en az birini içermelidir: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Bağlantı noktası 2222 dahil [ `EXPOSE` yönerge](https://docs.docker.com/engine/reference/builder/#expose) Dockerfile için. Kök parolası biliniyor olsa da, 2222 numaralı bağlantı noktasına İnternet üzerinden erişilemez. Bir iç yalnızca bağlantı noktası erişilebilir yalnızca özel bir sanal ağ köprüsü ağının kapsayıcılara tarafından değil.

    ```docker
    EXPOSE 2222 80
    ```

1. Bir kabuk betiği'ni kullanarak SSH hizmetini başlatmak emin olun (örneğe bakın [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

Dockerfile kullanan [ `ENTRYPOINT` yönerge](https://docs.docker.com/engine/reference/builder/#entrypoint) komut dosyasını çalıştırmak için.

    ```docker
    COPY startup /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="open-ssh-session-from-remote-shell"></a>Uzak Kabuğu'ndan SSH oturumu açma

> [!NOTE]
> Bu özellik şu anda önizlemede değil.
>

TCP, tünel kullanarak kimliği doğrulanmış WebSocket bağlantısı üzerinden geliştirme makine kapsayıcıları için Web uygulaması arasında bir ağ bağlantısı oluşturabilirsiniz. Tercih ettiğiniz istemciden App Service içinde çalışan, kapsayıcısı ile bir SSH oturumu açmanızı sağlar.

Başlamak için yüklemeniz gerekir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLI yüklemeden nasıl çalıştığını görmek için açık [Azure bulut Kabuk](../../cloud-shell/overview.md). 

Çalıştırarak en son uygulama hizmeti uzantısı Ekle [az uzantısı Ekle](/cli/azure/extension?view=azure-cli-latest#az-extension-add):

```azurecli-interactive
az extension add -–name webapp
```

Zaten çalıştırırsanız, `az extension add` çalıştırmadan önce [az uzantısı güncelleştirmesinin](/cli/azure/extension?view=azure-cli-latest#az-extension-update) bunun yerine:

```azurecli-interactive
az extension update -–name webapp
```

Uygulama kullanarak uzak bir bağlantı açmak [az webapp uzaktan bağlantı oluşturma](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) komutu. Belirtin  _\<grup\_adı >_ ve \_< app\_adı > _ uygulama ve değiştirme için \<bağlantı noktası > yerel bağlantı noktası numarası.

```azurecli-interactive
az webapp remote-connection create --resource-group <group_name> -n <app_name> -p <port> &
```

> [!TIP]
> `&` Bulut Kabuk kullanıyorsanız komutu sonunda yalnızca kolaylık sağlamaya yöneliktir. Aynı Kabuğu'nda bir sonraki komut çalıştırabilmeniz için işlemi arka planda çalışır.

Komut çıktısı, bir SSH oturumu açmak gereken bilgileri sağlar.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Yerel bağlantı noktası kullanan istemci tercih ettiğiniz ile kapsayıcısı ile bir SSH oturumu açın. Aşağıdaki örnek, varsayılan kullanır [ssh](https://ss64.com/bash/ssh.html) komutu:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

İstenmesini zaman yazın `yes` bağlanmaya devam etmek. Ardından parolası istenir. Kullanım `Docker!`, hangi için başında gösterilen.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Kimliği doğrulanmış sonra oturumu Hoş Geldiniz ekranında görmeniz gerekir.

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

Şimdi, bağlayıcı bağlanır. 

Try çalışan [üst](https://ss64.com/bash/top.html) komutu. İşlem listesi, uygulamanızın işleminde görebilmeniz gerekir. İsteğe bağlı olarak aşağıdaki örnek çıktıda sahip olduğu `PID 263`.

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

Hakkında sorular ve sorunları nakledebilirsiniz [Azure Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Kapsayıcıları için Web uygulaması hakkında daha fazla bilgi için bkz:

* [VS kodundan Azure App Service Node.js uygulamaları uzaktan hata ayıklama Tanıtımı](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma](quickstart-docker-go.md)
* [Linux üzerinde Azure App Service’te .NET Core Kullanma](quickstart-dotnetcore.md)
* [Linux üzerinde Azure App Service’te Ruby Kullanma](quickstart-ruby.md)
* [Kapsayıcılar için Azure App Service Web App SSS](app-service-linux-faq.md)