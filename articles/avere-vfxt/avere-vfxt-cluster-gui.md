---
title: Erişim Avere vFXT Denetim Masası - Azure
description: VFXT küme ve tarayıcı tabanlı Avere Avere vFXT yapılandırmak için Denetim Masası bağlanma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: c48f0d8f7ad34db585f4deae566641b6453357e8
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634541"
---
# <a name="access-the-vfxt-cluster"></a>Erişim vFXT küme

Ayarları değiştirmek ve Avere vFXT kümesini izlemek için Avere Denetim Masası'nı kullanın. Tarayıcı tabanlı bir grafik arabirim kümeye Avere denetim masasıdır.

VFXT kümesi özel bir sanal ağ içinde yer alan olduğundan, bir SSH tüneli oluşturma veya kümenin yönetim IP adresi ulaşmak için başka bir yöntem kullanın. İki temel adım vardır: 

1. İş istasyonunuzu ve özel sanal ağ arasında bağlantı oluşturma 
1. Denetim Masası'nda bir web tarayıcısı yüklemek için kümenin yönetim IP adresi kullanın 

> [!NOTE] 
> Bu makalede, küme denetleyicisinde veya başka bir VM, kümenin sanal ağ içindeki bir genel IP adresi belirlediğinizi varsayar. Sanal ağ erişimi için bir VPN veya ExpressRoute kullanıyorsanız, atlamak [Avere Denetim Masası'na Bağlan](#connect-to-the-avere-control-panel-in-a-browser).

Bağlamadan önce küme denetleyicisi oluştururken kullanılan SSH ortak/özel anahtar çifti yerel makinenizde yüklü olduğundan emin olun. SSH anahtarları belgelerini okuyun [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) veya [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) yardıma ihtiyacınız varsa.  

## <a name="access-with-a-linux-host"></a>Bir Linux ana bilgisayar ile erişim

Bu formu ile: 

SSH -L *local_port*:*cluster_mgmt_ip*: 443 *controller_username*@*controller_public_IP* 

Bu komut kümenin yönetim IP adresi küme denetleyicisinin IP adresi üzerinden bağlanır.

Örnek:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

SSH ortak anahtarınızı kümeyi oluşturmak için kullanılan ve eşleşen anahtarı istemci sisteminde yüklüyse kimlik doğrulaması otomatiktir.


## <a name="access-with-a-windows-host"></a>Bir Windows konak ile erişim

PuTTY kullanıyorsanız doldurun **hostname** küme denetleyicisi kullanıcı adı ve IP adresi alanı: *your_username*@*controller_public_IP*.

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

SSH ortak anahtarınızı kümeyi oluşturmak için kullanılan ve eşleşen anahtarı istemci sisteminde yüklüyse kimlik doğrulaması otomatiktir.

## <a name="connect-to-the-avere-control-panel-in-a-browser"></a>Bir tarayıcıda Avere Denetim Masası'na bağlanma

Bu adım, bir web tarayıcısı vFXT küme üzerinde çalışan yapılandırma yardımcı programı bağlanmak için kullanır.

Web tarayıcınızı açın ve gidin https://127.0.0.1:8443. 

Tarayıcınıza bağlı olarak tıklamanız gerekebilir **Gelişmiş** ve sayfasına gitmek güvenli olduğundan emin olun.

Kullanıcı adı girin `admin` ve küme girdiğiniz parola oluşturuluyor.

![Oturum açma ekran görüntüsü Avere, 'admin' kullanıcı adı ve parola ile doldurulmuş sayfasındaki](media/avere-vfxt-gui-login.png)

Tıklayın **oturum açma** veya klavyenizde enter tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Küme erişebilir, etkinleştirme [Destek](avere-vfxt-enable-support.md).
