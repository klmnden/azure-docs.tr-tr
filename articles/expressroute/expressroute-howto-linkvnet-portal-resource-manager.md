---
title: "Bir sanal ağ için bir expressroute bağlantı: Azure portal | Microsoft Docs"
description: "Bir VNet Azure expressroute bağlantı hattına bağlayın. Nasıl yapılır adımlar."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2018
ms.author: cherylmc
ms.openlocfilehash: 95b732229f151b8f27dce1dcc3825d9aa2e1d1ed
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit-using-the-portal"></a>Portalı'nı kullanarak bir expressroute bağlantı hattı için bir sanal ağa bağlanma
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-linkvnet-classic.md)
> 

Bu makalede, Azure portalını kullanarak bir Azure expressroute bağlantı hattı için bir sanal ağa bağlamak için bir bağlantı oluşturmanıza yardımcı olur. Başka bir abonelik parçası olabilir veya Azure expressroute bağlantı hattınız bağlandığınız sanal ağlar aynı abonelikte ya da olabilir.

## <a name="before-you-begin"></a>Başlamadan önce

* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md), [yönlendirme gereksinimleri](expressroute-routing.md), ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir.

  * Yönergeleri izleyerek [bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) ve bağlantı sağlayıcınız tarafından etkinleştirilmiş hattı sahip.
  * Bağlantı hattınız için yapılandırılmış Azure özel eşleme olduğundan emin olun. Bkz: [yönlendirmeyi yapılandırma](expressroute-howto-routing-portal-resource-manager.md) yönlendirme yönergeleri için makalenin.
  * Azure özel eşleme yapılandırılır ve uçtan uca bağlantı etkinleştirebilmeniz için ağınız ve Microsoft arasında BGP eşliği yukarı olduğundan emin olun.
  * Bir sanal ağ ve oluşturulan ve tam olarak sağlanan bir sanal ağ geçidi olduğundan emin olun. Yönergeleri izleyerek [ExpressRoute için bir sanal ağ geçidi oluşturmak](expressroute-howto-add-gateway-resource-manager.md). ExpressRoute için bir sanal ağ geçidi GatewayType 'ExpressRoute' değil VPN kullanır.

* Standart bir expressroute bağlantı hattı için en fazla 10 sanal ağlara bağlantı oluşturabilirsiniz. Tüm sanal ağları, standart bir expressroute bağlantı hattını kullanırken aynı coğrafi bölgede olması gerekir. 
* Expressroute bağlantı hattı coğrafi bölge dışında bir sanal ağ bağlantısı veya ExpressRoute premium eklentisi etkinse, expressroute bağlantı hattı için çok sayıda sanal ağlar bağlanabilirsiniz. Denetleme [SSS](expressroute-faqs.md) premium eklentisi hakkında daha fazla ayrıntı için.
* Yapabilecekleriniz [bir video izlemek](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) adımları daha iyi anlamak başlamadan önce.

## <a name="connect-a-vnet-to-a-circuit---same-subscription"></a>Bir VNet bir hattı - aynı abonelik Bağlan

> [!NOTE]
> BGP yapılandırma bilgilerini Katman 3 sağlayıcısı, eşlemeler yapılandırılıp yapılandırılmadığını gösterilmez. Bağlantı hattınız sağlanan durumdaysa bağlantılar oluşturmak mümkün olması gerekir.
>

### <a name="to-create-a-connection"></a>Bir bağlantı oluşturmak için

1. Expressroute bağlantı hattı ve Azure özel eşleme başarılı bir şekilde yapılandırıldığından emin olun. ' Ndaki yönergeleri izleyin [bir expressroute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md) ve [yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md). Expressroute bağlantı hattı aşağıdaki görüntü gibi görünmelidir:

  ![ExpressRoute bağlantı hattı ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
2. Sanal ağ geçidinizi expressroute bağlantı hattı bağlantı için bağlantı sağlama şimdi başlayabilirsiniz. Tıklatın **bağlantı** > **Ekle** açmak için **Bağlantı Ekle** sayfasında ve değerleri yapılandırın.

  ![Bağlantı ekran Ekle](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)
3. Bağlantınızı başarıyla yapılandırıldıktan sonra bağlantı nesnesi bağlantı bilgilerini gösterir.

  ![Bağlantı nesnesi ekran görüntüsü](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

## <a name="connect-a-vnet-to-a-circuit---different-subscription"></a>Bir hattı - farklı bir abonelik bir sanal ağa bağlan

Bir expressroute bağlantı hattı birden çok abonelik paylaşabilirsiniz. Aşağıdaki şekilde arasında birden çok abonelik basit bir ExpressRoute bağlantı hatları için nasıl paylaşım works'ün şematik gösterilmektedir.

![Çapraz abonelik bağlantısı](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Her büyük bulut içinde daha küçük bulut, kuruluş içindeki farklı departmanlara ait abonelikleri temsil etmek için kullanılır.
- Her kuruluş içinde bölümlerin hizmetlerini dağıtmak için kendi abonelik kullanabilirsiniz ancak, şirket içi ağınıza bağlanmak için tek bir expressroute bağlantı hattı paylaşabilirsiniz.
- Tek bir bölüm (Bu örnekte: BT) expressroute bağlantı hattına sahip olabilir. Kuruluştaki diğer abonelikler expressroute bağlantı hattı ve diğer Azure Active Directory kiracılar ve Kurumsal Anlaşma kayıtları bağlı abonelikleri dahil olmak üzere bağlantı hattına ilişkili yetkilerini kullanabilirsiniz.

  > [!NOTE]
  > Bağlantı ve bant genişliği ücretleri ayrılmış bağlantı hattı için ExpressRoute bağlantı hattı sahibine uygulanır. Tüm sanal ağları aynı bant genişliğini paylaşır.
  >
  >

### <a name="administration---about-circuit-owners-and-circuit-users"></a>Yönetim - hattı sahipleri ve hattı kullanıcılar hakkında

'Devre sahibinden' ExpressRoute bağlantı hattı kaynak yetkili bir güç kullanıcısıdır. Devre sahibinden 'hattı kullanıcılar tarafından kullanılan yetkilerini oluşturabilirsiniz. Bağlantı hattı, expressroute bağlantı hattı aynı abonelik içinde olmayan sanal ağ geçitlerini sahipleri kullanıcılardır. Bağlantı hattı kullanıcıların yetkilerini (sanal ağ başına bir yetkilendirme) kullanmak.

Devre sahibinden yetkilerini herhangi bir zamanda iptal etme ve değiştirmek için power sahiptir. Bir yetkilendirme sonuçlarına tüm bağlantı erişimini iptal edildi abonelikten silinmesini iptal ediliyor.

### <a name="circuit-owner-operations"></a>Bağlantı hattı sahibi işlemleri

**Bir bağlantı yetkilendirme oluşturmak için**

Devre sahibinden bir yetkilendirme oluşturur. Bu, kendi sanal ağ geçitlerini expressroute bağlantı hattına bağlanmak için bir bağlantı hattı kullanıcı tarafından kullanılan bir yetkilendirme anahtarı oluşturulmasını sonuçlanır. Bir yetkilendirme yalnızca bir bağlantı için geçerli değil.

1. ExpressRoute sayfasında tıklatın **yetkilerini** ve ardından bir **adı** tıklatın ve yetkilendirme için **kaydetmek**.

  ![Yetkilendirmeler](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)
2. Yapılandırma kaydedildikten sonra kopyalama **kaynak kimliği** ve **yetkilendirme anahtarının**.

  ![Yetkilendirme anahtarı](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Bir bağlantı yetkilendirme silmek için**

Bir bağlantı seçerek silebilirsiniz **silmek** bağlantınızı sayfasında simgesi.

### <a name="circuit-user-operations"></a>Bağlantı hattı kullanıcı işlemleri

Bağlantı hattı kullanıcının kaynak kimliği ile devre sahibinden yetkilendirme anahtarından gerekir.

**Bir bağlantı yetkilendirme kullanmak için**

1. Tıklatın **+ yeni** düğmesi.

  ![Yeni'yi tıklatın](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)
2. Arama **"Bağlantı"** Market seçin ve tıklatın **oluşturma**.

  ![Bağlantı için arama](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)
3. Emin olun **bağlantı türü** "ExpressRoute" ayarlanır.
4. Ayrıntıları doldurun ve ardından **Tamam** temelleri sayfasında.

  ![Temel bilgileri sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)
5. İçinde **ayarları** sayfasında, **sanal ağ geçidi** ve denetleme **yetkilendirme kullanmak** onay kutusu.
6. Girin **yetkilendirme anahtar** ve **hattı URI eş** ve bağlantı bir ad verin. **Tamam**’a tıklayın.

  ![Ayarlar sayfası](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)
7. Bilgileri gözden **Özet** sayfasında ve tıklayın **Tamam**.

**Bir bağlantı yetkilendirme serbest bırakmak için**

Bir yetkilendirme bağlanan sanal ağ expressroute bağlantı hattı bağlantı silerek serbest bırakabilirsiniz.

## <a name="delete-a-connection-to-unlink-a-vnet"></a>Bir sanal ağ bağlantısını kesmek için bağlantıyı silme

Bağlantıyı silme ve ağınızı bir expressroute bağlantı hattı için seçerek bağlantısını **silmek** bağlantınızı sayfasında simgesi.

## <a name="next-steps"></a>Sonraki adımlar
ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).