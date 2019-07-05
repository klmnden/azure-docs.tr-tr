---
title: Erişim Avere vFXT Denetim Masası - Azure
description: VFXT küme ve tarayıcı tabanlı Avere Avere vFXT yapılandırmak için Denetim Masası bağlanma
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: v-erkell
ms.openlocfilehash: 830be92d37f304598cca05c3ac80973158c38a59
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439981"
---
# <a name="access-the-vfxt-cluster"></a>Erişim vFXT küme

Ayarları değiştirmek ve Avere vFXT kümesini izlemek için Avere Denetim Masası'nı kullanın. Tarayıcı tabanlı bir grafik arabirim kümeye Avere denetim masasıdır.

VFXT kümesi özel bir sanal ağ içinde yer alan olduğundan, bir SSH tüneli oluşturma veya kümenin yönetim IP adresi ulaşmak için başka bir yöntem kullanın. İki temel adım vardır: 

1. İş istasyonunuzu ve özel sanal ağ arasında bağlantı oluşturma 
1. Kümenin Denetim Masası'ndaki bir web tarayıcısı yükleyin 

> [!NOTE] 
> Bu makalede, küme denetleyicisinde veya başka bir VM, kümenin sanal ağ içindeki bir genel IP adresi belirlediğinizi varsayar. Bu makalede, kümeye erişmek için bir konak olarak bu VM'ye kullanmayı açıklar. Sanal ağ erişimi için bir VPN veya ExpressRoute kullanıyorsanız, atlamak [Avere Denetim Masası'na Bağlan](#connect-to-the-avere-control-panel-in-a-browser).

Bağlamadan önce küme denetleyicisi oluştururken kullanılan SSH ortak/özel anahtar çifti yerel makinenizde yüklü olduğundan emin olun. SSH anahtarları belgelerini okuyun [Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) yardıma ihtiyacınız varsa. (Bir parola yerine bir ortak anahtar kullandıysanız, bağlandığınızda girmeniz istenir.) 

## <a name="create-an-ssh-tunnel"></a>SSH tüneli oluşturma 

Bir SSH tüneli, Linux tabanlı bir komut satırından veya Windows 10 istemci sistemi oluşturabilirsiniz. 

Bir SSH tünel oluşturma komutu ile bu formu kullanın: 

SSH -L *local_port*:*cluster_mgmt_ip*: 443 *controller_username*\@*controller_public_IP*

Bu komut kümenin yönetim IP adresi küme denetleyicisinin IP adresi üzerinden bağlanır.

Örnek:

```sh
ssh -L 8443:10.0.0.5:443 azureuser@203.0.113.51
```

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
