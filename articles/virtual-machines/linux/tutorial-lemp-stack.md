---
title: Öğretici - Azure’daki bir Linux sanal makinesinde LEMP dağıtma | Microsoft Docs
description: Bu öğreticide, Azure’daki bir Linux sanal makinesinde LEMP yığını yüklemeyi öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: 15d88e082f9ab0838f4a560d89801edd9d46d682
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703534"
---
# <a name="tutorial-install-a-lemp-web-server-on-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure'da bir Linux sanal makinesine LEMP web sunucusu yükleme

Bu makalede, Azure’daki bir Ubuntu sanal makinesine NGINX web sunucusunun, MySQL ve PHP’nin (LEMP yığını) nasıl dağıtılacağı gösterilmektedir. LEMP yığını, Azure’a da yükleyebileceğiniz popüler [LAMP yığınının](tutorial-lamp-stack.md) bir alternatifidir. LEMP sunucusunu çalışır halde görmek için, isteğe bağlı olarak bir WordPress sitesi yükleyip yapılandırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma (LEMP yığınındaki 'L')
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * NGINX, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LEMP sunucusuna WordPress yükleme

Bu kurulum, hızlı testler veya kavram kanıtı içindir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>NGINX, MySQL ve PHP yükleme

Ubuntu paket kaynaklarını güncelleştirmek ve NGINX, MySQL ve PHP yüklemek için aşağıdaki komutu çalıştırın. 

```bash
sudo apt update && sudo apt install nginx && sudo apt install mysql-server php-mysql php-fpm
```

Paketleri ve diğer bağımlılıkları yüklemeniz istenir. Bu işlem, MySQL ile PHP kullanmak için gereken en düşük PHP uzantılarını yükler.  

## <a name="verify-installation-and-configuration"></a>Yükleme ve yapılandırmayı doğrulama


### <a name="verify-nginx"></a>Verify NGINX

Aşağıdaki komutla NGINX sürümünü denetleyin:
```bash
nginx -v
```

NGINX yüklüyken ve sanal makinenizde 80 numaralı bağlantı noktası açıkken, web sunucusuna İnternet üzerinden erişilebilir. NGINX hoş geldiniz sayfasını görüntülemek için bir web tarayıcısı açın ve sanal makinenin genel IP adresini girin. Sanal makineye SSH oturumu açmak için kullandığınız genel IP adresini kullanın:

![NGINX varsayılan sayfası][3]


### <a name="verify-and-secure-mysql"></a>Doğrulayın ve MySQL güvenliğini sağlama

Aşağıdaki komutla MySQL sürümünü denetleyin (ana `V` parametresini not edin):

```bash
mysql -V
```

Bir kök parola ayarlama dahil olmak üzere, MySQL yüklemesini güvenli hale getirmek için çalıştırma `mysql_secure_installation` betiği. 

```bash
sudo mysql_secure_installation
```

İsteğe bağlı olarak, doğrulama parola (önerilen) eklentisi ayarlayabilirsiniz. Ardından, MySQL kök kullanıcı için bir parola ayarlamanız ve ortamınız için kalan güvenlik ayarlarını yapılandırın. "Y" (Evet) tüm soruları yanıtlamak olmasını öneririz.

MySQL özelliklerini (MySQL veritabanı oluşturma, kullanıcı ekleme veya yapılandırma ayarlarını değiştirme) denemek istiyorsanız MySQL’de oturum açın. Bu öğreticiyi tamamlamak için bu adım gerekli değildir. 


```bash
sudo mysql -u root -p
```

İşiniz bittiğinde, `\q` yazarak mysql isteminden çıkın.

### <a name="verify-php"></a>PHP doğrulayın

Aşağıdaki komutla PHP sürümünü denetleyin:

```bash
php -v
```

PHP FastCGI Process Manager’ı (PHP-FPM) kullanacak şekilde NGINX’i yapılandırın. Aşağıdaki komutları çalıştırarak özgün NGINX sunucu bloğu yapılandırma dosyasını yedekleyin ve sonra seçtiğiniz bir düzenleyicide özgün dosyayı düzenleyin:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

Düzenleyicide, `/etc/nginx/sites-available/default` içeriklerini aşağıdakilerle değiştirin. Ayarların açıklaması için yorumlara bakın. Sanal makinenizin genel IP adresini değiştirin *Yourpublicıpaddress*, PHP sürümü onaylayın `fastcgi_pass`ve kalan ayarları değiştirmeden bırakın. Ardından dosyayı kaydedin.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.2-fpm.sock;
    }
}
```

NGINX yapılandırmasında sözdizimi hataları olup olmadığını denetleyin:

```bash
sudo nginx -t
```

Sözdizimi doğruysa, aşağıdaki komutla NGINX’i yeniden başlatın:

```bash
sudo service nginx restart
```

Daha fazla test etmek istiyorsanız, tarayıcıda görüntülenecek bir hızlı PHP bilgi sayfası oluşturun. Aşağıdaki komut, PHP bilgi sayfasını oluşturur:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Artık oluşturduğunuz PHP bilgi sayfasını denetleyebilirsiniz. Bir tarayıcı açın ve `http://yourPublicIPAddress/info.php` adresine gidin. Sanal makinenizin genel IP adresini değiştirin. Şu görüntüye benzemelidir.

![PHP bilgi sayfası][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure’da bir LEMP sunucusu dağıttınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * NGINX, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LEMP yığınına WordPress yükleme

SSL sertifikalarını kullanarak güvenli web sunucularının güvenliğini nasıl sağlayabileceğinizi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [SSL ile web sunucusunun güvenliğini sağlama](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
