---
title: Erişim Avere vFXT Denetim Masası - Azure
description: VFXT küme ve tarayıcı tabanlı Avere Avere vFXT yapılandırmak için Denetim Masası bağlanma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f989f4d103efecf2b6e206287dd8b7b300a1796d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60794313"
---
# <a name="access-the-vfxt-cluster"></a>Erişim vFXT küme

Ayarları değiştirmek ve Avere vFXT kümesini izlemek için Avere Denetim Masası'nı kullanın. Tarayıcı tabanlı bir grafik arabirim kümeye Avere denetim masasıdır.

VFXT kümesi özel bir sanal ağ içinde yer alan olduğundan, bir SSH tüneli oluşturma veya kümenin yönetim IP adresi ulaşmak için başka bir yöntem kullanın. İki temel adım vardır: 

1. İş istasyonunuzu ve özel sanal ağ arasında bağlantı oluşturma 
1. Kümenin Denetim Masası'ndaki bir web tarayıcısı yükleyin 

> [!NOTE] 
> Bu makalede, küme denetleyicisinde veya başka bir VM, kümenin sanal ağ içindeki bir genel IP adresi belirlediğinizi varsayar. Bu makalede, kümeye erişmek için bir konak olarak bu VM'ye kullanmayı açıklar. Sanal ağ erişimi için bir VPN veya ExpressRoute kullanıyorsanız, atlamak [Avere Denetim Masası'na Bağlan](#connect-to-the-avere-control-panel-in-a-browser).

Bağlamadan önce küme denetleyicisi oluştururken kullanılan SSH ortak/özel anahtar çifti yerel makinenizde yüklü olduğundan emin olun. SSH anahtarları belgelerini okuyun [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) yardıma ihtiyacınız varsa. (Bir parola yerine bir ortak anahtar kullandıysanız, bağlandığınızda girmeniz istenir.) 

## <a name="ssh-tunnel-with-a-linux-host"></a>Bir Linux ana bilgisayarla SSH tüneli

Linux tabanlı bir istemci kullanıyorsanız, bir SSH tünel oluşturma komutu ile bu formu kullanın: 

SSH -L *local_port*:*cluster_mgmt_ip*: 443 *controller_username*\@*controller_public_IP*

Bu komut kümenin yönetim IP adresi küme denetleyicisinin IP adresi üzerinden bağlanır.

Örnek:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

SSH ortak anahtarınızı kümeyi oluşturmak için kullanılan ve eşleşen anahtarı istemci sisteminde yüklüyse kimlik doğrulaması otomatiktir. Kullandıysanız, parolayı sistem girmek isteyip istemediğinizi sorar.

## <a name="ssh-tunnel-with-a-windows-host"></a>Bir Windows ana bilgisayarla SSH tüneli

Bu örnek genel Windows tabanlı terminal yardımcı programını PuTTY kullanır.

PuTTY içinde dolgu **ana bilgisayar adı** küme denetleyicisi kullanıcı adı ve IP adresi alanı: *your_username*\@*controller_public_IP*.

Örnek: ``azureuser@203.0.113.51``

İçinde **yapılandırma** paneli:

1. Genişletin **bağlantı** > **SSH** soldaki. 
1. Tıklayın **tüneller**. 
1. Bir kaynak bağlantı noktası 8443 gibi girin. 
1. Hedef için vFXT kümenin yönetim IP adresi ve bağlantı noktası 443'ü girin. 
   Örnek: ``203.0.113.51:443``
1. **Ekle**'ye tıklayın.
1. **Aç**'a tıklayın.

![Tünel eklemek için tıklatın nerede gösteren ekran görüntüsü, Putty uygulama](media/avere-vfxt-ptty-numbered.png)

SSH ortak anahtarınızı kümeyi oluşturmak için kullanılan ve eşleşen anahtarı istemci sisteminde yüklüyse kimlik doğrulaması otomatiktir. Kullandıysanız, parolayı sistem girmek isteyip istemediğinizi sorar.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Bir tarayıcıda Avere Denetim Masası'na bağlanma

Bu adım, bir web tarayıcısı vFXT küme üzerinde çalışan yapılandırma yardımcı programı bağlanmak için kullanır.

* Bir SSH tüneli bağlantısı için web tarayıcınızı açın ve gidin `https://127.0.0.1:8443`. 

  Tarayıcıda localhost IP adresini kullanmanız gerekmez, tüneli oluştururken IP adresi kümeye bağlı. Yerel bağlantı noktası 8443 dışında kullandıysanız, bağlantı noktası numaranızı kullanın.

* Kümeye erişmek için bir VPN veya ExpressRoute kullanıyorsanız, tarayıcınızda küme yönetimi IP adresine gidin. Örnek: ``https://203.0.113.51``

Tarayıcınıza bağlı olarak tıklamanız gerekebilir **Gelişmiş** ve sayfasına gitmek güvenli olduğundan emin olun.

Kullanıcı adı girin `admin` ve ne zaman sağladığınız yönetici parolasını kümesi oluşturma.

![Oturum açma ekran görüntüsü Avere, 'admin' kullanıcı adı ve parola ile doldurulmuş sayfasındaki](media/avere-vfxt-gui-login.png)

Tıklayın **oturum açma** veya klavyenizde enter tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Küme erişebilir, etkinleştirme [Destek](avere-vfxt-enable-support.md).
