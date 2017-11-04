---
title: "Azure'da bir Linux sanal makinede AMPUL dağıtma | Microsoft Docs"
description: "Öğretici - yükleme azure'da bir Linux VM AMPUL yığında"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: c00e6a190633348411f47490808739d570cafd69
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Azure VM temelinde bir AMPUL web sunucusunu yükleme
Bu makalede bir Apache web sunucusu, MySQL ve azure'da bir Ubuntu VM üzerinde PHP (AMPUL yığını) dağıtma konusunda size yol göstermektedir. NGINX web sunucusu tercih ederseniz, bkz. [LEMP yığın](tutorial-lemp-stack.md) Öğreticisi. Eylem AMPUL Server'da görmek için isteğe bağlı olarak yükleyebilir ve bir WordPress sitesi yapılandırın. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir Ubuntu VM (' L' AMPUL yığınında) oluşturma
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmasını doğrulayın
> * AMPUL sunucu üzerinde WordPress yükleme


AMPUL yığında öneriler bir üretim ortamı için de dahil olmak üzere daha fazla bilgi için bkz: [Ubuntu belgelerine](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, MySQL ve PHP yükleme

Ubuntu paket kaynaklarını güncelleştirmek ve Apache, MySQL ve PHP yüklemek için aşağıdaki komutu çalıştırın. Komut sonunda şapka (^) unutmayın.


```bash
sudo apt update && sudo apt install lamp-server^
```



Paketler ve diğer bağımlılıklar yüklemeniz istenir. İstendiğinde, MySQL için bir kök parola ayarlayın ve ardından devam etmek için Enter. Kalan istemleri izleyin. Bu işlem, PHP, MySQL ile kullanmak için gerekli en düşük gerekli PHP uzantıları yükler. 

![MySQL kök parola sayfası][1]

## <a name="verify-installation-and-configuration"></a>Yükleme ve yapılandırmasını doğrulayın


### <a name="apache"></a>Apache

Aşağıdaki komutu Apache sürümüyle denetleyin:
```bash
apache2 -v
```

Apache yüklü ve bağlantı noktası 80 VM'nize açık ile web sunucusu artık Internet üzerinden erişilebilir. Varsayılan Apache2 Ubuntu sayfasını görüntülemek için bir web tarayıcısı açın ve VM ortak IP adresini girin. SSH VM için kullanılan ortak IP adresini kullan:

![Apache varsayılan sayfa][3]


### <a name="mysql"></a>MySQL

MySQL sürümü aşağıdaki komutla denetleyin (büyük harf Not `V` parametresi):

```bash
mysql -V
```

MySQL yükleme güvenliğinin sağlanmasına yardımcı olmak için aşağıdaki betiği çalıştıran öneririz:

```bash
mysql_secure_installation
```

MySQL kök parolanızı girin ve ortamınız için güvenlik ayarlarını yapılandırın.

Bir MySQL veritabanı oluşturmak istiyorsanız, kullanıcı ekleme ya da yapılandırma ayarlarını, MySQL oturum açma değiştirin:

```bash
mysql -u root -p
```

İşiniz bittiğinde, mysql istemi yazarak çıkmak `\q`.

### <a name="php"></a>PHP

PHP sürümünü aşağıdaki komutla denetleyin:

```bash
php -v
```

Daha fazla test etmek isterseniz, bir tarayıcıda görüntülemek üzere hızlı bir PHP bilgileri sayfası oluşturun. Aşağıdaki komut, PHP bilgileri sayfası oluşturur:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Şimdi, oluşturduğunuz PHP bilgileri sayfasını kontrol edebilirsiniz. Bir tarayıcı açın ve gidin `http://yourPublicIPAddress/info.php`. VM ortak IP adresini değiştirin. Bu görüntüsüne benzer görünmelidir.

![PHP bilgileri sayfası][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure AMPUL Server'da dağıtıldı. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir Ubuntu VM oluşturma
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmasını doğrulayın
> * AMPUL sunucu üzerinde WordPress yükleme

SSL sertifikaları web sunucularıyla güvenli öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [SSL ile güvenli web sunucusu](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png