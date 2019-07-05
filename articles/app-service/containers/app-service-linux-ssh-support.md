---
title: Linux'ta - Azure App Service için SSH desteği | Microsoft Docs
description: Linux üzerinde Azure App Service ile SSH kullanma hakkında bilgi edinin.
keywords: Azure app service, web uygulaması, linux, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: b54d5003f67a1bd79e1e52eef87df858bc68ade1
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551904"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service için SSH desteği

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) yönetim komutları uzaktan komut satırı terminalden yürütmek için yaygın olarak kullanılır. Linux'ta App Service, her yeni web uygulaması için çalışma zamanı yığını olarak kullanılan yerleşik Docker görüntüleri uygulama kapsayıcı içine SSH desteği sağlar. 

![Çalışma zamanı yığınları](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

SSH sunucusu özel görüntünüzü yapılandırarak özel Docker görüntüleri için.

Ayrıca, kapsayıcıya SSH ve SFTP kullanarak doğrudan yerel geliştirme makinenize bağlanabilirsiniz.

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturum aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-no-h.md)]

## <a name="use-ssh-support-with-custom-docker-images"></a>SSH desteği ile özel Docker görüntülerinizi kullanın

Bkz: [yapılandırma SSH özel bir kapsayıcıda](configure-custom-container.md#enable-ssh).

## <a name="open-ssh-session-from-remote-shell"></a>Uzak kabuğundan SSH oturumu açın

> [!NOTE]
> Bu özellik şu anda Önizleme aşamasındadır.
>

TCP, tünel kullanarak bir geliştirme makineniz ve kapsayıcılar için Web uygulaması arasında ağ bağlantısı üzerinden kimliği doğrulanmış bir WebSocket bağlantısı oluşturabilirsiniz. Tercih ettiğiniz istemciden App Service'te çalışan kapsayıcınıza bir SSH oturumu açmak sağlar.

Başlamak için yüklemeniz gerekir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLI'yı yüklemeden nasıl çalıştığını görmek için [Azure Cloud Shell](../../cloud-shell/overview.md). 

Uygulamayı kullanarak uzak bir bağlantı açmak [az webapp uzaktan bağlantı oluşturma](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) komutu. Belirtin  _\<subscrıptıon-ID >_ ,  _\<grup-adı >_ ve \_ \<-adı > _ uygulamanız için.

```azurecli-interactive
az webapp create-remote-connection --subscription <subscription-id> --resource-group <resource-group-name> -n <app-name> &
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
