---
title: 'Bir sanal ağı devreye - ExpressRoute bağlantı: Azure portalı | Microsoft Docs'
description: Bir sanal ağ için Azure ExpressRoute bağlantı hattına bağlayın. Nasıl yapılır adımları.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 66c43556e698063b95ce1c3787458e40c9d71504
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60365629"
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Portalı kullanarak bir ExpressRoute bağlantı hattına bir sanal ağı bağlama
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

Bu makale, Azure portalını kullanarak bir Azure ExpressRoute bağlantı hattına bir sanal ağa bağlamak için bir bağlantı oluşturmanıza yardımcı olur. Azure ExpressRoute devreniz bağlandığınız sanal ağlar aynı abonelikte olabilir veya başka bir abonelik parçası olabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Gözden geçirme [önkoşulları](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.

* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.
  * Yönergelerini izleyin [ExpressRoute devresi oluşturma](expressroute-howto-circuit-portal-resource-manager.md) ve bağlantı hattı, bağlantı sağlayıcı tarafından etkin.
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](expressroute-howto-routing-portal-resource-manager.md) makale için yönlendirme yönergeleri.
  * Azure özel eşdüzey hizmet sağlama yapılandırılır ve uçtan uca bağlantıyı etkinleştirmek üzere ağınız ile Microsoft arasında BGP eşliği ayarlama olduğundan emin olun.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan sanal ağ geçidi olduğundan emin olun. Yönergelerini izleyin [ExpressRoute için sanal ağ geçidi oluşturma](expressroute-howto-add-gateway-resource-manager.md). ExpressRoute için sanal ağ geçidi, GatewayType 'ExpressRoute' VPN'değil kullanır.

* Standart bir ExpressRoute bağlantı hattı için en fazla 10 sanal ağlara bağlayabilirsiniz. Tüm sanal ağları, standart bir ExpressRoute bağlantı hattını kullanırken aynı jeopolitik bölgede olması gerekir.

* En fazla dört ExpressRoute bağlantı hatları için tek bir sanal ağa bağlanabilir. Bağlanmakta olduğunuz her bir ExpressRoute bağlantı hattı için yeni bir bağlantı nesnesi oluşturmak için aşağıdaki işlemi kullanın. ExpressRoute bağlantı hatları, aynı abonelik, farklı Aboneliklerde veya her ikisinin bir karışımı olabilir.

* Bir sanal ağ ExpressRoute bağlantı hattının coğrafi bölge dışında bağlama ya da ExpressRoute premium eklentisi etkinleştirildiğinde, çok sayıda sanal ağları ExpressRoute devreniz bağlayın. Denetleme [SSS](expressroute-faqs.md) premium eklenti hakkında daha fazla ayrıntı için.

* Yapabilecekleriniz [video görüntüleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) adımları daha iyi anlamak başlamadan önce.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Devreye - aynı Abonelikteki bir sanal ağa bağlama

> [!NOTE]
> BGP yapılandırma bilgilerini Katman 3 sağlayıcısı, eşlemeler yapılandırılıp yapılandırılmadığını gösterilmez. Bağlantı hattınızın sağlanmış bir durumda ise, bağlantıları oluşturabilirsiniz.
>

### <a name="to-create-a-connection"></a>Bir bağlantı oluşturmak için

1. ExpressRoute bağlantı hattı ve Azure özel eşdüzey hizmet sağlama başarılı bir şekilde yapılandırıldığından emin olun. Bölümündeki yönergeleri [ExpressRoute devresi oluşturma](expressroute-howto-circuit-arm.md) ve [yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md). ExpressRoute devreniz şu resimdeki gibi görünmelidir:

   ![ExpressRoute bağlantı hattı ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
2. ExpressRoute bağlantı hattı için sanal ağ geçidinizin bağlantı için bağlantı sağlama şimdi başlayabilirsiniz. Tıklayın **bağlantı** > **Ekle** açmak için **Bağlantı Ekle** sayfasında ve değerlerini yapılandırın.

   ![Bağlantı ekran görüntüsü Ekle](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
3. Bağlantınızın başarılı bir şekilde yapılandırıldıktan sonra bağlantı nesnesi bağlantı bilgilerini gösterir.

   ![Bağlantı nesnesi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Devreye - farklı bir aboneliğe bir VNet bağlama

Bir ExpressRoute bağlantı hattı birden çok farklı abonelikler arasında paylaşabilirsiniz. Aşağıdaki şekilde basit bir ExpressRoute bağlantı hatları için nasıl paylaşım Works şematik, birden fazla aboneliği analiz gösterilmektedir.

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Her küçük bulutların büyük bulut içinde bir kuruluştaki farklı departmanlara ait abonelikleri temsil etmek için kullanılır.
- Her kuruluş içindeki bölümlerin hizmetlerini dağıtmak için kendi aboneliği kullanabilirsiniz ancak, şirket içi ağınıza bağlanmak için tek bir ExpressRoute bağlantı hattı paylaşabilirler.
- Tek bir bölüm (Bu örnekte: BT) ExpressRoute bağlantı hattına sahip olabilir. Kuruluştaki diğer Aboneliklerdeki ilişkili diğer Azure Active Directory kiracıları ve Kurumsal Anlaşma kayıtları bağlantılı abonelikleri de dahil olmak üzere, bağlantı hattı için yetkilendirme ve ExpressRoute bağlantı hattı kullanabilirsiniz.

  > [!NOTE]
  > ExpressRoute bağlantı hattı sahibinden için adanmış bir bağlantı hattı için bağlantı ve bant genişliği ücretleri uygulanır. Tüm sanal ağları, aynı bant genişliğini paylaşır.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Bağlantı hattı sahipleri ve bağlantı hattı kullanıcılar hakkındaki - yönetim

'Bağlantı hattı sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcıdır. Bağlantı hattı sahibinden 'devre kullanıcıları tarafından' ödenebilecek yetkilendirmeleri oluşturabilirsiniz. ExpressRoute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitleri sahipleri bağlantı hattını kullanıcılardır. Devre kullanıcılarının, yetkilendirmeleri (sanal ağ başına bir yetkilendirme) kullanmak.

Bağlantı hattı sahibinden yetkilendirme dilediğiniz zaman iptal et ve değiştirmek için gücüne sahiptir. Bir yetkilendirme sonuçlarına tüm bağlantı erişimini iptal edildi abonelikten silinmesini iptal ediliyor.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bağlantı Yetkilendirme oluşturmak için**

Bağlantı hattı sahibinden bir yetkilendirme oluşturur. ExpressRoute bağlantı hattına kendi sanal ağ geçitlerine bağlanmak için bir bağlantı hattı kullanıcısı tarafından kullanılan bir yetkilendirme anahtarına oluşturulmasını sonuçlanır. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

> [!NOTE]
> Her bağlantı için ayrı bir yetkilendirme gerektirir.
>

1. ExpressRoute sayfasında tıklatın **yetkilendirmeleri** Anahtar'a tıklayın ve bir **adı** tıklatın ve yetkilendirme için **Kaydet**.

   ![Yetkilendirmeler](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Yapılandırma kaydedildikten sonra kopyalama **kaynak kimliği** ve **yetkilendirme anahtarını**.

   ![Yetkilendirme anahtarı](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Bağlantı Yetkilendirme silmek için**

Bir bağlantıyı seçerek silebilirsiniz **Sil** sayfasında, bağlantınız için bir simge.

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcısı, kaynak Kimliğini ve bağlantı hattı sahibinden yetkilendirme anahtarı gerekir.

**Bağlantı Yetkilendirme kullanmak için**

1. Tıklayın **+ yeni** düğmesi.

   ![Yeni'yi tıklatın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Arama **"Bağlantısı"** Market'te seçin ve tıklayın **Oluştur**.

   ![Bağlantı arayın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. Emin **bağlantı türü** "ExpressRoute" ayarlanır.
4. Ayrıntıları girin ve ardından tıklayın **Tamam** temel bilgileri sayfasında.

   ![Temel bilgileri sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. İçinde **ayarları** seçin sayfasında **sanal ağ geçidi** ve **yetkilendirmeyi kullanma** onay kutusu.
6. Girin **yetkilendirme anahtarını** ve **eş devre URI'si** ve bağlantı bir ad verin. **Tamam** düğmesine tıklayın.

   ![Ayarlar sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. Bilgileri gözden **özeti** sayfasında ve tıklayın **Tamam**.

**Bağlantı Yetkilendirme serbest bırakmak için**

Sanal ağı ExpressRoute bağlantı hattına bağlayan bağlantı silerek bir yetkilendirme serbest bırakabilirsiniz.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Bir sanal ağ bağlantısını için bağlantıyı silme

Bağlantıyı silme ve ExpressRoute bağlantı hattına sanal ağınıza seçerek bağlantısını **Sil** sayfasında, bağlantınız için bir simge.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
