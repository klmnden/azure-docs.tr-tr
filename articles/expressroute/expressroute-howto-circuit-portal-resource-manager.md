---
title: "Oluşturma ve bir expressroute bağlantı hattı değiştirme: Azure portal | Microsoft Docs"
description: "Bu makalede, oluşturmak, sağlamak, doğrulayın, güncelleştirme, silme ve bir expressroute bağlantı hattı yetkisini kaldırma açıklar."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/20/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: a21fdfbc4396f2b7aff50fae4ca796d8ea6a733b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Oluşturma ve bir expressroute bağlantı hattı değiştirme
> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure portalı](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-circuit-classic.md)
>

Bu makalede Azure portalı ve Azure Resource Manager dağıtım modeli kullanarak bir Azure expressroute oluşturmayı açıklar. Aşağıdaki adımlar da nasıl devrenin durumunu denetleyin, güncelleştirme veya silme ve onu yetkisini kaldırma gösterir.


## <a name="before-you-begin"></a>Başlamadan önce
* Gözden geçirme [Önkoşullar](expressroute-prerequisites.md) ve [iş akışları](expressroute-workflows.md) yapılandırmaya başlamadan önce.
* Erişimi olduğundan emin olun [Azure portal](https://portal.azure.com).
* Yeni ağ kaynaklarını oluşturma izni olduğundan emin olun. Doğru izinler yoksa, hesap yöneticinize başvurun.
* Yapabilecekleriniz [bir video izlemek](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) adımları daha iyi anlamak için başlamadan önce.

## <a name="create"></a>Oluşturma ve bir expressroute bağlantı hattı sağlama
### <a name="1-sign-in-to-the-azure-portal"></a>1. Azure portalında oturum açın
Bir tarayıcıdan [Azure portalına](http://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Yeni bir expressroute bağlantı hattı oluşturma
> [!IMPORTANT]
> ExpressRoute bağlantı hattınız bir hizmet anahtarı verilen andan itibaren faturalandırılır. Bağlantı sağlayıcı bağlantı hattı sağlamak hazır olduğunda bu işlemi gerçekleştirmek emin olun.
> 
> 

1. Yeni bir kaynak oluşturma seçeneğini seçerek bir expressroute bağlantı hattı oluşturabilirsiniz. Tıklatın **yeni** > **ağ** > **ExpressRoute**aşağıdaki görüntüde gösterildiği gibi:

  ![ExpressRoute bağlantı hattı oluşturma](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Tıklattıktan sonra **ExpressRoute**, göreceğiniz **oluşturma expressroute bağlantı hattı** sayfası. Bu sayfada değerleri doldurma sırasında doğru SKU Katmanı (standart veya Premium) ve fatura modelini (sınırsız veya Metered) ölçümü verilerini belirttiğinizden emin olun.

  ![SKU katmanı ve ölçüm verilerini Yapılandır](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit.png)

  * **Katman** bir expressroute bağlantı standart ExpressRoute premium Eklentisi etkin olup olmadığını belirler. Belirleyebileceğiniz **standart** standart SKU almak için veya **Premium** premium eklenti için.
  * **Ölçüm verilerini** faturalama türü belirler. Belirleyebileceğiniz **Metered** ölçülen veri planı için ve **sınırsız** sınırsız veri planı için. Fatura türünden değiştirebileceğinizi unutmayın **Metered** için **sınırsız**, ancak türünden değiştiremezsiniz **sınırsız** için **Metered**.
  * **Eşleme konumu** Burada sizin eşlemeyi Microsoft ile fiziksel konumu.

    > [!IMPORTANT]
    > Eşleme konumu belirten [fiziksel konumu](expressroute-locations.md) Burada sizin eşlemeyi Microsoft ile. Bu **değil** Azure ağ kaynak sağlayıcısı bulunduğu Coğrafya başvuruyor "Konum" özelliğine bağlı. Bunlar ilişkili olsa da, bir ağ kaynak sağlayıcısı coğrafi olarak yakın bağlantı hattının eşleme konumu seçmek için iyi bir uygulamadır.
    >
    >

### <a name="3-view-the-circuits-and-properties"></a>3. Bağlantı hatları ve özelliklerini görüntüleyin
**Tüm devreler görüntüleyin**

Seçerek oluşturulan bağlantı hatları görüntüleyebilirsiniz **tüm kaynakları** sol taraftaki menüsünde.

![Görünüm bağlantı hatları](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Özelliklerini görüntülemek**

Bağlantı hattı özelliklerini seçerek görüntüleyebilirsiniz. Üzerinde **genel bakış** sayfasında hattınız için hizmet anahtarı hizmet anahtar alanında görüntülenir. Bağlantı hattınız için hizmet anahtarı kopyalayın ve sağlama işlemini tamamlamak için aşağıya doğru hizmet sağlayıcısının göndermesi gerekir. Devre hizmet anahtarını hattınız için özeldir.

![Özellikleri görüntüle](./media/expressroute-howto-circuit-portal-resource-manager/servicekey1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. Hizmet anahtarı sağlamak için bağlantı sağlayıcınızı Gönder
Bu sayfada **sağlayıcı durumu** hizmet sağlayıcı tarafında sağlama geçerli durumu hakkında bilgi sağlar. **Hattı durum** Microsoft tarafında durumunu sağlar. Bağlantı hattı durumları sağlama hakkında daha fazla bilgi için bkz: [iş akışları](expressroute-workflows.md#expressroute-circuit-provisioning-states) makalesi.

Yeni bir expressroute bağlantı hattı oluşturduğunuzda, bağlantı hattı şu durumda olur:

Sağlayıcı Durumu: sağlanmadı<BR>
Devre, durum: etkin

![Sağlama işlemini başlatın](./media/expressroute-howto-circuit-portal-resource-manager/status.png)

Bağlantı sağlayıcı onu sizin için etkinleştirme sürecinde olduğunda bağlantı hattı için şu durum değiştirir:

Sağlayıcı Durumu: sağlama<BR>
Devre, durum: etkin

Bir expressroute bağlantı hattı kullanabilmek için şu durumda olmalıdır:

Sağlayıcı Durumu: sağlanan<BR>
Devre, durum: etkin

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Durum ve hattı anahtar durumunu düzenli aralıklarla denetleyin
Siz de seçerek ilgileniyor bağlantı hattı özelliklerini görüntüleyebilirsiniz. Denetleme **sağlayıcı durumu** ve bu için taşınmıştır olun **hazırlandı** devam etmeden önce.

![Bağlantı hattı ve sağlayıcı durumu](./media/expressroute-howto-circuit-portal-resource-manager/provisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Yönlendirme yapılandırması oluşturma
Adım adım yönergeler için bkz [expressroute bağlantı hattı yönlendirme yapılandırması](expressroute-howto-routing-portal-resource-manager.md) oluşturup hattı eşlemeler değiştirmek için makale.

> [!IMPORTANT]
> Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle bir IP VPN, MPLS gibi), bağlantı sağlayıcınız yönlendirmeyi sizin için yönetir ve yapılandırır.
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. ExpressRoute bağlantı hattına bir sanal ağı bağlama
Ardından, bir sanal ağ, expressroute bağlantı hattına bağlayın. Kullanım [ExpressRoute bağlantı hatları için sanal ağları bağlama](expressroute-howto-linkvnet-arm.md) makale Resource Manager dağıtım modeliyle çalışırken.

## <a name="status"></a>Bir expressroute bağlantı hattı durumunu alma
Seçip genel bakış sayfasında görüntüleme bir devrenin durumunu görüntüleyebilirsiniz. 

## <a name="modify"></a>Bir expressroute bağlantı hattı değiştirme
Bağlantı etkilemeden belirli bir expressroute bağlantı hattı özelliklerini değiştirebilirsiniz. Bant genişliği, SKU, fatura modelini değiştirin ve klasik işlemleri ver **yapılandırma** sayfası. Sınırlar ve sınırlamalar hakkında daha fazla bilgi için bkz: [ExpressRoute SSS](expressroute-faqs.md). 

Kapalı kalma süresi olmadan aşağıdaki görevleri gerçekleştirebilirsiniz:

* Etkinleştirmek veya expressroute bağlantı hattı için ExpressRoute Premium eklentisi devre dışı.
* Sağlanmış kapasite kullanılabilir bağlantı noktası, expressroute bağlantı hattı bant genişliğini artırır. Bir bağlantı hattının bant genişliğini önceki sürüme indirme desteklenmiyor. 
* Ölçüm planından değiştirme *ölçülen veri* için *sınırsız veri*. Ölçüm plan sınırsız verilerden ölçülen verileri değiştirme desteklenmez.
* Etkinleştirme ve devre dışı *izin Klasik işlemleri*.

> [!IMPORTANT]
> Varolan bir bağlantı üzerinde yetersiz kapasite ise expressroute bağlantı hattı yeniden başlatmanız gerekebilir. Varsa hiçbir ek kapasite kullanılabilir o konumda bağlantı hattı yükseltemezsiniz.
>
> Bir expressroute bağlantı hattı kesintiye uğratmadan bant indiremezsiniz. Bant genişliği eski sürüme düşürmeyi expressroute bağlantı hattı yetkisini kaldırma ve yeni bir expressroute bağlantı hattı yeniden hazırlayana gerektirir.
> 
> Standart bağlantı hattı için izin daha büyük olan kaynaklar kullanıyorsanız, Premium eklenti işlemi devre dışı bırakma başarısız olabilir.
> 
> 

Bir expressroute bağlantı hattı değiştirmek için tıklatın **yapılandırma**.

![Bağlantı hattı değiştirme](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)




## <a name="delete"></a>Sağlamayı kaldırmayı ve bir expressroute bağlantı hattı silme
Expressroute bağlantı hattı seçerek silebilirsiniz **silmek** simgesi. Aşağıdaki bilgileri unutmayın:

* Expressroute bağlantı hattı tüm sanal ağlardan bağlantısını gerekir. Bu işlem başarısız olursa, tüm sanal ağları devresine bağlı olup olmadığını denetleyin.
* Sağlama durumu ExpressRoute bağlantı hattı hizmet sağlayıcı ise **sağlama** veya **hazırlandı** kendi tarafında hattı yetkisini kaldırma için hizmet sağlayıcınıza birlikte çalışmalısınız. Kaynakları ayırabilir ve hizmet sağlayıcısı devre sağlama kaldırma işlemi tamamlandıktan ve bize bildiren kadar sizi faturalandırmak devam ediyoruz.
* Hizmet sağlayıcısı hattı sağlaması kaldırılıyor. sağlaması değilse (sağlama durumu hizmet sağlayıcısı kümesine **sağlanmadı**), bağlantı hattı silebilirsiniz. Bağlantı hattı için fatura durdurur.

## <a name="next-steps"></a>Sonraki adımlar
Bağlantı hattınız oluşturduktan sonra sonraki adımlara devam edin:

* [Oluşturma ve expressroute bağlantı hattı için yönlendirmeyi değiştirme](expressroute-howto-routing-portal-resource-manager.md)
* [Sanal ağ, ExpressRoute devresine bağlama](expressroute-howto-linkvnet-arm.md)