---
title: 'Bir bağlantı hattı için - ExpressRoute eşlemesi yapılandırın: Azure | Microsoft Docs'
description: Bu makale oluşturma ve sağlama ExpressRoute özel ve Microsoft eşlemesi için adımları içermektedir. Bu makalede ayrıca durumunu denetlemek nasıl gösterir güncelleştirme veya silme bir bağlantı hattı için eşleme.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 08d8103c4b35148a87d347e31b11c7c8c968598b
ms.sourcegitcommit: dda9fc615db84e6849963b20e1dce74c9fe51821
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622340"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattı için eşlemesi oluşturma ve değiştirme

Bu makalede, Azure portalını kullanarak Azure Resource Manager (ARM) ExpressRoute devresi için yönlendirme yapılandırması oluşturma ve yönetme yardımcı olur. Ayrıca, durum, update veya delete denetleyin ve eşlemeler için ExpressRoute bağlantı hattı sağlamasını kaldırma. Bağlantı hattınız ile çalışmak için farklı bir yöntem kullanmak istiyorsanız, bir makale aşağıdaki listeden seçin:

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - özel eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - genel eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft eşdüzey hizmet sağlama](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

Azure özel ve Microsoft bir ExpressRoute bağlantı hattı için eşleme yapılandırma (Azure genel eşdüzey hizmet sağlama kullanım dışı yeni bağlantı hatları için). Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz. [devreler ve eşlemeleri hakkında](expressroute-circuit-peerings.md).

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Eşleme (s) yapılandırmak için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir. 
* Paylaşılan bir anahtar/MD5 karma değeri kullanmayı planlıyorsanız, bu tünelinin iki tarafı üzerinde kullanın ve en fazla 25 alfasayısal karakter sayısını sınırlamak emin olun. Özel karakterler desteklenmez. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Katman 3 Hizmetleri (genellikle gibi bir IPVPN MPLS) yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yapılandırır ve yönlendirmeyi sizin için yönetir. 

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşlemelerini yapılandırmadan önce hizmet sağlayıcınıza başvurun.
> 
> 

## <a name="msft"></a>Microsoft eşlemesi

Bu bölümde, oluşturma, alma, güncelleştirme ve bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını silme yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017'den önce yapılandırılmış olan ExpressRoute devrelerinin Microsoft eşdüzey hizmet sağlama, tüm hizmet ön eklerin rota filtreleri tanımlanmamış olsa bile, Microsoft eşlemesi tanıtılan sahip olur. 1 Ağustos 2017 veya sonrasında yapılandırılmış ExpressRoute devrelerinin Microsoft eşlemesi tüm ön ekleri olmaz bağlantı hattına bir rota filtresinde bağlanana kadar tanıtılan. Daha fazla bilgi için [Microsoft eşlemesi için rota filtresini Yapılandır](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Denetleme **sağlayıcısı durumu** bağlantı hattının tam olarak bağlantı sağlayıcı tarafından daha ileriye devam etmeden önce sağlandığından emin olmak için.

   Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için eşlemeyi Microsoft etkinleştirmek isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izleyin gerekmez. Bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra yönetmez, ancak bu adımlarla devam edin.

   **Bağlantı hattı - sağlayıcısı durumu: Sağlanmadı**

    [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m.png "Sağlayıcı Durumu: Sağlanmadı")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m-lightbox.png#lightbox)

   **Bağlantı hattı - sağlayıcısı durumu: sağlanan**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m.png "Sağlayıcı Durumu sağlanan =")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m-lightbox.png#lightbox)
2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve ikincil bağlantı için aynı VLAN kimliğini kullanmanız gerekir
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm ön eklerin listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * **İsteğe bağlı -** müşteri ASN'si: Eşleme AS numarasına kayıtlı olmayan önekler tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.
   * Yönlendirme kayıt defteri adı: Belirtebileceğiniz RIR / IRR'yi AS numarası ve öneklerinin kaydedildiği rır.
   * **İsteğe bağlı -** kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki örnekte gösterildiği gibi yapılandırmak için istediğiniz eşlemeyi seçebilirsiniz. Microsoft eşleme satırını seçin.

   [![Microsoft eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m.png "Microsoft eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m-lightbox.png#lightbox)
4. Microsoft eşlemesini yapılandırın. **Kaydet** tüm parametreleri belirledikten sonra yapılandırma. Aşağıdaki resimde örnek bir yapılandırma gösterilmektedir:

   ![Microsoft eşlemesini yapılandırın](./media/expressroute-howto-routing-portal-resource-manager/configuration-m.png)

   Bağlantı hattınız 'gerekli bir doğrulama' alır, durumunu, kavram kanıtı destek ekibimize ön eklerin sahipliğine ilişkin göstermek için bir destek bileti açmak gerekir. Aşağıdaki örnekte gösterildiği gibi doğrudan portalından bir destek bileti açabilirsiniz:

   ![Doğrulama gerekiyor - destek bileti](./media/expressroute-howto-routing-portal-resource-manager/ticket-portal-m.png)

5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki görüntüye benzer bir şey görürsünüz:

   ![Eşleme durumu: Yapılandırılmış](./media/expressroute-howto-routing-portal-resource-manager/configured-m.png "eşdüzey hizmet sağlama durumu: Yapılandırılmış")]

### <a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Microsoft eşleme için satırı seçerek eşleme özelliklerini görüntüleyebilirsiniz.

[![Microsoft eşleme özelliklerini görüntüleme](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png "özelliklerini görüntüleme")](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m-lightbox.png#lightbox)
### <a name="updatemsft"></a>Microsoft eşleme yapılandırmasını güncelleştirmek için

Değiştirin, ardından eşleme özelliklerini değiştirebilirsiniz ve değişikliklerinizi kaydetmek istediğiniz eşleme için satırı seçebilirsiniz.

![Eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/update-peering-m.png)

### <a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi Sil simgesini tıklayarak eşleme yapılandırmanızı kaldırabilirsiniz:

![Eşlemeyi Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-peering-m.png)

## <a name="private"></a>Azure özel eşdüzey hizmet sağlama

Bu bölümde, oluşturma, alma, güncelleştirme ve bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını silme yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. 

   Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Azure özel, eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izleyin gerekmez. Bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmiyorsa, bağlantı hattınızı oluşturduktan sonra yönetmez, ancak sonraki adımlara devam edin.

   **Bağlantı hattı - sağlayıcısı durumu: Sağlanmadı**

   [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p.png "Sağlayıcı Durumu sağlanmadı =")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p-lightbox.png#lightbox)

   **Bağlantı hattı - sağlayıcısı durumu: sağlanan**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p.png "Sağlayıcı Durumu = sağlandı")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p-lightbox.png#lightbox)

2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

   * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.
   * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Microsoft, yönlendirici için ikinci kalmayacak IP kullandığından bu alt ağından yönlendiriciniz için ilk kullanılabilir IP adresi atar.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve ikincil bağlantı için aynı VLAN kimliğini kullanmanız gerekir
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Kullanabileceğiniz özel eşleme için AS bu 65515 numarasından 65520, dışında aralığında numarası.
   * Özel eşleme ayarlama ayarladığınızda, yollarını şirket içi uç yönlendiriciniz BGP aracılığıyla azure'a tanıtmanız gerekir.
   * **İsteğe bağlı -** kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki örnekte gösterildiği gibi Azure private eşleme satırını seçin:

   [![Özel eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p.png "private eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p-lightbox.png#lightbox)
4. Özel eşleme oluşturun. **Kaydet** tüm parametreleri belirledikten sonra yapılandırma.

   ![Özel eşleme oluşturun](./media/expressroute-howto-routing-portal-resource-manager/configuration-p.png)
5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey görürsünüz:

   ![Özel eşleme kaydedildi](./media/expressroute-howto-routing-portal-resource-manager/save-p.png)

### <a name="getprivate"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure özel eşleme özelliklerini görüntüleyebilirsiniz.

[![Özel eşleme özelliklerini görüntüleme](./media/expressroute-howto-routing-portal-resource-manager/view-p.png "özel eşleme özelliklerini görüntüleme")](./media/expressroute-howto-routing-portal-resource-manager/view-p-lightbox.png#lightbox)

### <a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. Güncelleştirdikten sonra yaptığınız değişiklikleri kaydedin.

![Özel eşleme güncelleştir](./media/expressroute-howto-routing-portal-resource-manager/update-peering-p.png)

### <a name="deleteprivate"></a>Azure özel eşlemeyi silmek için

Aşağıdaki görüntüde gösterilen silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağlar ve ExpressRoute Global erişim bağlantıları kaldırıldı emin olmanız gerekir. 
> 
> 

![Özel eşleme Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-p.png)

## <a name="public"></a>Azure genel eşdüzey hizmet sağlama

Bu bölümde, oluşturma, alma, güncelleştirme ve bir ExpressRoute bağlantı hattı için Azure ortak eşleme yapılandırmasını silme yardımcı olur.

> [!Note]
> Azure genel eşdüzey hizmet sağlama, yeni bağlantı hatları için kullanım dışı bırakılmıştır. Daha fazla bilgi için [ExpressRoute eşdüzey hizmet sağlaması](expressroute-circuit-peerings.md).
>

### <a name="getpublic"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure ortak eşleme özelliklerini görüntüleyin.

### <a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçin, sonra eşleme özelliklerini değiştirebilirsiniz.

### <a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Sil simgesini seçerek eşleme yapılandırmanızı kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [bir ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
