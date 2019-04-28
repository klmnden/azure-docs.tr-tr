---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/23/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: ee5faedd4f59aa791424a1f178f0462922f21d28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485239"
---
Her bir uç noktası olan bir *genel bağlantı noktası* ve *özel bağlantı noktası*:

* Genel bağlantı noktası için gelen trafiği internet'ten sanal makineye dinlemek için Azure yük dengeleyici tarafından kullanılır.
* Özel bağlantı noktası genellikle bir uygulama veya hizmetin sanal makine üzerinde çalışan hedefleyen gelen trafiği dinleyecek şekilde sanal makine tarafından kullanılır.

İyi bilinen ağ protokolleri, uç noktaları Azure portalı ile oluşturduğunuzda sağlanan TCP veya UDP bağlantı noktalarını ve IP protokolü için varsayılan değerler. Özel uç noktalar için doğru IP Protokolü (TCP veya UDP) ve ortak ve özel bağlantı noktalarını belirtin. Gelen trafiği birden çok sanal makine arasında rastgele dağıtmak için birden çok uç oluşan yük dengeli bir küme oluşturun.

Bir uç nokta oluşturduktan sonra erişim denetimi listesi (ACL) izin veren veya kaynak IP adresine göre uç noktasının genel bağlantı noktasına gelen trafiği reddeden kuralları tanımlamak için kullanabilirsiniz. Sanal makine bir Azure sanal ağında ise, ancak bunun yerine ağ güvenlik grupları kullanın. Daha fazla bilgi için [ağ güvenlik grupları hakkında](../articles/virtual-network/security-overview.md).

> [!NOTE]
> Azure sanal makineleri için güvenlik duvarı yapılandırması, Azure otomatik olarak ayarlar uzak bağlantı uç noktaları ile ilişkili bağlantı noktaları otomatik olarak gerçekleştirilir. İçin diğer uç noktalardan belirtilen bağlantı noktaları için yapılandırma güvenlik duvarı sanal makinenin otomatik olarak gerçekleştirilir. Sanal makine için bir uç noktası oluşturduğunuzda, sanal makinenin bir güvenlik duvarı trafiği uç nokta yapılandırması için karşılık gelen özel bağlantı noktası ve protokolü için de izin verdiğinden emin olun. Güvenlik Duvarı'nı yapılandırmak için belge veya sanal makinede çalışan işletim sistemi için çevrimiçi yardıma bakın.
>
>

## <a name="create-an-endpoint"></a>Uç nokta oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **sanal makineler**ve ardından yapılandırmak istediğiniz sanal makineyi seçin.

3. Seçin **uç noktaları** içinde **ayarları** grubu. **Uç noktaları** sayfası görüntülenirse, tüm geçerli uç noktaları sanal makine için listelenir. (Bu örnekte, bir Windows VM için klasörüdür. Bir Linux VM varsayılan olarak bir uç nokta için SSH gösterir.)

   <!-- ![Endpoints](./media/virtual-machines-common-classic-setup-endpoints/endpointswindows.png) -->
   ![Uç noktaları](./media/virtual-machines-common-classic-setup-endpoints/endpointsblade.png)


4. Uç noktası girişleri yukarıdaki komut çubuğunda **Ekle**. **Uç noktası ekleme** sayfası görüntülenir.

5. İçin **adı**, uç nokta için bir ad girin.

6. İçin **Protokolü**, seçin ya da **TCP** veya **UDP**.

7. İçin **genel bağlantı noktası**, bağlantı noktası numarası için gelen trafiği internet'ten girin. 

8. İçin **özel bağlantı noktası**, sanal makine dinlediği bağlantı noktası numarasını girin. Genel ve özel bağlantı noktası numaralarını farklı olabilir. Güvenlik Duvarı sanal makinede özel bağlantı noktası ve protokol karşılık gelen trafiğe izin verecek şekilde yapılandırıldığından emin olun.

9. **Tamam**’ı seçin.

Yeni uç nokta listelenir **uç noktaları** sayfası.

![Uç nokta oluşturma başarılı](./media/virtual-machines-common-classic-setup-endpoints/endpointcreated.png)

## <a name="manage-the-acl-on-an-endpoint"></a>Bir uç nokta ACL'sini Yönet
Trafik gönderebilir bilgisayarların kümesini tanımlamak için kaynak IP adresini alarak trafiği bir uç nokta ACL'sini kısıtlayabilirsiniz. Ekleme, değiştirme veya bir uç noktada bir ACL'yi kaldırmak için aşağıdaki adımları izleyin.

> [!NOTE]
> Uç nokta yük dengeli bir kümenin parçasıysa, kümedeki tüm uç noktalar için bir uç nokta ACL'sini yaptığınız tüm değişiklikler uygulanır.
>
>

Bir Azure sanal ağında sanal makine ise ACL'leri yerine ağ güvenlik grupları kullanın. Daha fazla bilgi için [ağ güvenlik grupları hakkında](../articles/virtual-network/security-overview.md).

1. Azure Portal’da oturum açın.

2. Seçin **sanal makineler**ve ardından yapılandırmak istediğiniz sanal makinenin adını seçin.

3. Seçin **uç noktaları**. Uç noktalar listesinden uygun uç noktayı seçin. Sayfanın alt kısmında ACL listesidir.

   ![ACL ayrıntılarını belirtin](./media/virtual-machines-common-classic-setup-endpoints/aclpreentry.png)

4. Satır eklemek, silmek veya bir ACL için kuralları düzenlemek ve sırayı değiştirmek için listede kullanın. **Uzak alt** izin vermek veya kaynak IP adresine göre trafiği reddetmeye yönelik Azure load balancer kullandığı internet'ten gelen trafiği için bir IP adresi aralığı değerdir. Classless Inter-Domain yönlendirme (CIDR) biçiminde adres ön eki biçimi olarak da bilinen IP adresi aralığı belirttiğinizden emin olun. Örneğin, `10.1.0.0/8`.

   ![Yeni ACL girişi](./media/virtual-machines-common-classic-setup-endpoints/newaclentry.png)


İnternet üzerindeki bilgisayarlara karşılık gelen belirli bilgisayarlardan gelen yalnızca trafiğine izin verecek şekilde veya belirli, bilinen adres aralıklarını trafiği reddetmeye yönelik kurallar kullanabilirsiniz.

Kurallar, ilk kural ile başlayıp son kuralla sırayla değerlendirilir. Bu nedenle, kuralları en az kısıtlayıcı en kısıtlayıcı sıralanmalıdır. Daha fazla bilgi için [ağ erişim denetimi listesi nedir](../articles/virtual-network/virtual-networks-acl.md).
