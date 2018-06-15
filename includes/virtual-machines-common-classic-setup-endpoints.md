---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/17/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: cfe675ca269a69c7c2bfa67638acd0afbcd1c8ea
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371295"
---
Her bitiş noktasının bir *genel bağlantı noktası* ve *özel bağlantı noktası*:

* Genel bağlantı noktası, Internet'ten gelen trafiği sanal makineye dinlenecek Azure yük dengeleyici tarafından kullanılır.
* Özel bağlantı noktası, genellikle bir uygulama veya sanal makinede çalışan hizmet giden gelen trafiği dinlemek için sanal makine tarafından kullanılır.

TCP veya UDP bağlantı noktaları için iyi bilinen ağ uç noktalarını Azure portalıyla oluşturduğunuzda protokolleri sağlanır ve IP protokolü için varsayılan değerleri. Özel uç noktaları için doğru IP Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktaları belirtmeniz gerekir. Gelen trafiği rastgele birden fazla sanal makine arasında dağıtmak için birden çok uç noktaları oluşan bir yük dengeli kümesi oluşturmanız gerekir.

Bir uç nokta oluşturduktan sonra izin veya kaynak IP adresine göre uç noktasının ortak bağlantı noktasına gelen trafiği reddeden kuralları tanımlamak için bir erişim denetimi listesi (ACL) kullanabilirsiniz. Ancak, sanal makine bir Azure sanal ağında ise, ağ güvenlik grupları yerine kullanmanız gerekir. Ayrıntılar için bkz [ağ güvenlik grupları hakkında](../articles/virtual-network/security-overview.md).

> [!NOTE]
> Azure sanal makineleri için güvenlik duvarı yapılandırması, Azure otomatik olarak ayarlayan uzak bağlantı uç ile ilişkili bağlantı noktaları için otomatik olarak yapılır. Diğer uç için belirtilen bağlantı noktaları için yapılandırma güvenlik duvarı sanal makinenin otomatik olarak yapılır. Sanal makine için bir uç nokta oluşturduğunuzda, Güvenlik Duvarı'nı sanal makinenin uç nokta yapılandırması karşılık gelen özel bağlantı noktası ve protokol trafiği verdiğinden emin olmak gerekir. Güvenlik Duvarı'nı yapılandırmak için belgelere veya sanal makinede çalışan işletim sistemi için çevrimiçi yardıma bakın.
>
>

## <a name="create-an-endpoint"></a>Uç nokta oluşturma
1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com)da oturum açın
2. Tıklatın **sanal makineleri**ve ardından yapılandırmak istediğiniz sanal makinenin adına tıklayın.
3. Tıklatın **uç noktaları** içinde **ayarları** grubu. **Uç noktaları** sayfası, tüm geçerli uç noktaları sanal makine için listeler. (Bu örnek bir Windows VM'dir. Bir Linux VM varsayılan bir uç nokta için SSH gösterir.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Uç Noktalar](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)

4. Uç noktası girişleri yukarıdaki komut çubuğunda tıklayın **Ekle**.
5. Üzerinde **uç nokta ekleme** sayfasında, uç için bir ad yazın **adı**.
6. İçinde **Protokolü**, ya da seçin **TCP** veya **UDP**.
7. İçinde **genel bağlantı noktası**, Internet'ten gelen trafik için bağlantı noktası numarasını yazın. İçinde **özel bağlantı noktası**, sanal makine dinleme bağlantı noktası numarasını yazın. Bu bağlantı noktası numaralarını farklı olabilir. Sanal makinede Güvenlik Duvarı'nı protokolünde (6. adım) ve özel bağlantı noktası için karşılık gelen trafiğe izin verecek şekilde yapılandırıldığından emin olun.
10. **Tamam**’a tıklayın.

Yeni uç nokta listelenir **uç noktaları** sayfası.

![Uç nokta oluşturma başarılı](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>Bir uç nokta ACL'sini Yönet
Trafik gönderebilen bir bilgisayarlar kümesi tanımlamak için kaynak IP adresine göre trafiği bir uç noktasındaki ACL kısıtlayabilirsiniz. Ekleme, değiştirme ve bir uç noktada bir ACL kaldırmak için aşağıdaki adımları izleyin.

> [!NOTE]
> Uç nokta yük dengeli kümesinin bir parçası ise, kümedeki tüm uç noktaları bir uç noktasındaki ACL yaptığınız tüm değişiklikler uygulanır.
>
>

Sanal makine bir Azure sanal ağında ise, ağ güvenlik grupları, ACL yerine öneririz. Ayrıntılar için bkz [ağ güvenlik grupları hakkında](../articles/virtual-network/security-overview.md).

1. Zaten, Azure portalında oturum açma yapmadıysanız.
2. Tıklatın **sanal makineleri**ve ardından yapılandırmak istediğiniz sanal makinenin adına tıklayın.
3. **Uç Noktalar**’a tıklayın. Listeden uygun uç nokta seçin. Sayfanın alt kısmında ACL listesidir.

   ![ACL ayrıntılarını belirtin](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Satır listede eklemek, silmek veya için bir ACL kuralları düzenlemek ve sıralamalarını değiştirmek için kullanın. **Uzak alt** izin vermek veya kaynak IP adresine göre trafiği engellemek için Azure yük dengeleyici kullanır Internet'ten gelen trafiği için bir IP adresi aralığı bir değerdir. CIDR biçiminde adres öneki biçimi olarak da bilinen IP adresi aralığı belirttiğinizden emin olun. `10.1.0.0/8` bunun bir örneğidir.

 ![Yeni ACL giriş](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


Internet üzerindeki bilgisayarlara karşılık gelen belirli bilgisayarlardan yalnızca trafiğine izin vermek için veya belirli, bilinen adres aralıklarının trafiği reddetmek için kuralları kullanabilirsiniz.

Kuralları ile ilk kural başlangıç ve son kuralla bitiş sırayla değerlendirilir. Bu, kuralları en az kısıtlayıcı en kısıtlayıcı sıralanmalıdır olduğunu anlamına gelir. Örnekler ve daha fazla bilgi için bkz: [bir ağ erişim denetimi listesi nedir](../articles/virtual-network/virtual-networks-acl.md).
