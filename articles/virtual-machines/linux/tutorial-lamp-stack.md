---
title: Öğretici - Azure’daki bir Linux sanal makinesinde LAMP dağıtma | Microsoft Docs
description: Bu öğreticide, Azure’daki bir Linux sanal makinesinde LAMP yığını yüklemeyi öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: 66b7d7692d9143c8db813ad135b0b9c70b8869d2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708580"
---
# <a name="tutorial-install-a-lamp-web-server-on-a-linux-virtual-machine-in-azure"></a>Öğretici: Azure'da bir Linux sanal makinesine LAMP web sunucusu yükleme

Bu makalede, Azure’daki bir Ubuntu sanal makinesine Apache web sunucusunun, MySQL ve PHP’nin (LAMP yığını) nasıl dağıtılacağı gösterilmektedir. NGINX web sunucusunu tercih ederseniz [LEMP yığını](tutorial-lemp-stack.md) öğreticisine bakın. LAMP sunucusunu çalışır halde görmek için, isteğe bağlı olarak bir WordPress sitesi yükleyip yapılandırabilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma (LAMP yığınındaki 'L')
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LAMP sunucusuna WordPress yükleme

Bu kurulum, hızlı testler veya kavram kanıtı içindir. Üretim ortamına yönelik öneriler de dahil olmak üzere, LAMP yığını hakkında daha fazla bilgi için [Ubuntu belgelerine](https://help.ubuntu.com/community/ApacheMySQLPHP) bakın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, MySQL ve PHP yükleme

Ubuntu paket kaynaklarını güncelleştirmek ve Apache, MySQL ve PHP yüklemek için aşağıdaki komutu çalıştırın. `lamp-server^` paket adının parçası olan, komutun sonundaki şapka karakterine (^) dikkat edin. 


```bash
sudo apt update && sudo apt install lamp-server^
```

Paketleri ve diğer bağımlılıkları yüklemeniz istenir. Bu işlem, MySQL ile PHP kullanmak için gereken en düşük PHP uzantılarını yükler.  

## <a name="verify-installation-and-configuration"></a>Yükleme ve yapılandırmayı doğrulama


### <a name="verify-apache"></a>Apache doğrulayın

Aşağıdaki komutla Apache sürümünü denetleyin:
```bash
apache2 -v
```

Apache yüklüyken ve sanal makinenizde 80 numaralı bağlantı noktası açıkken, web sunucusuna İnternet üzerinden erişilebilir. Apache2 Ubuntu Varsayılan Sayfasını görüntülemek için bir web tarayıcısı açın ve sanal makinenin genel IP adresini girin. Sanal makineye SSH oturumu açmak için kullandığınız genel IP adresini kullanın:

![Apache varsayılan sayfası][3]


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

Daha fazla test etmek istiyorsanız, tarayıcıda görüntülenecek bir hızlı PHP bilgi sayfası oluşturun. Aşağıdaki komut, PHP bilgi sayfasını oluşturur:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Artık oluşturduğunuz PHP bilgi sayfasını denetleyebilirsiniz. Bir tarayıcı açın ve `http://yourPublicIPAddress/info.php` adresine gidin. Sanal makinenizin genel IP adresini değiştirin. Şu görüntüye benzemelidir.

![PHP bilgi sayfası][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure’da bir LAMP sunucusu dağıttınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Ubuntu sanal makinesi oluşturma
> * Web trafiği için 80 numaralı bağlantı noktasını açın
> * Apache, MySQL ve PHP yükleme
> * Yükleme ve yapılandırmayı doğrulama
> * LAMP sunucusuna WordPress yükleme

SSL sertifikalarını kullanarak güvenli web sunucularının güvenliğini nasıl sağlayabileceğinizi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [SSL ile web sunucusunun güvenliğini sağlama](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png
