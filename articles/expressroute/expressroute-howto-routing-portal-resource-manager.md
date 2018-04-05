---
title: 'Yönlendirme (bir ExpressRoute için eşliği) hattı yapılandırma: Resource Manager: Azure | Microsoft Docs'
description: Bu makalede, bir ExpressRoute bağlantı hattı için özel, ortak ve Microsoft eşlemesinin nasıl oluşturulduğu ve sağlandığı adım adım anlatılmaktadır. Bu makalede ayrıca bağlantı hattınızın durumunu denetleme, bağlantı hattını güncelleştirme veya silme işlemlerinin nasıl yapıldığı da anlatılmaktadır.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2018
ms.author: cherylmc
ms.openlocfilehash: f0f0a31abc4e2d3114d71729c6c447c569295290
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Oluşturma ve bir ExpressRoute bağlantı hattı için eşlemesini değiştirme

Bu makalede, Azure portalını kullanarak Resource Manager dağıtım modelinde bir expressroute bağlantı hattı için yönlendirme yapılandırması oluşturma ve yönetme yardımcı olur. Ayrıca, durumu, güncelleştirme veya silme denetleyin ve bir expressroute bağlantı hattı için eşlemeler sağlamayı sonlandırın. Bağlantı hattınız ile çalışmak için farklı bir yöntem kullanmak istiyorsanız, bir makale aşağıdaki listeden seçin:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - özel eşliği](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - ortak eşleme](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft eşlemesi](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 


## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Expressroute bağlantı hattı, sonraki bölümlerde cmdlet'leri çalıştırılabilmesi sağlanmış ve etkin durumda olması gerekir.
* Paylaşılan bir anahtar/MD5 karma değeri kullanmayı planlıyorsanız, bu tünel her iki tarafında kullanın ve en fazla 25 karakter sayısını sınırlamak emin olun.

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen sunan bir hizmet sağlayıcısı kullanıyorsanız, Katman 3 Hizmetleri (genellikle gibi bir IPVPN MPLS), bağlantı sağlayıcınız yönlendirmeyi sizin için yönetir ve yapılandırır. 

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşlemelerini yapılandırmadan önce hizmet sağlayıcınıza başvurun.
> 
> 

Bir ExpressRoute bağlantı hattı için bir, iki veya üç eşlemenin tamamını (Azure özel, Azure ortak ve Microsoft) yapılandırabilirsiniz. Eşlemeleri seçtiğiniz herhangi bir sırayla yapılandırabilirsiniz. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeleri hakkında daha fazla bilgi için bkz: [ExpressRoute Yönlendirme etki alanları](expressroute-circuit-peerings.md).

## <a name="msft"></a>Microsoft eşlemesi

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Microsoft eşleme yapılandırmasını silme yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 önce yapılandırılmış olan ExpressRoute bağlantı hatları, Microsoft eşlemesi yol filtreleri tanımlanmamış olsa bile eşleme, Microsoft tarafından tanıtılan tüm hizmet ön eklerin sahip olur. Ve 1 Ağustos 2017 sonrasında yapılandırılmış ExpressRoute bağlantı hatları, Microsoft eşlemesi sahip olmaz tüm ön eklerin rota filtre devresine bağlanana kadar tanıtılan. Daha fazla bilgi için bkz: [Microsoft eşliği için rota filtresini Yapılandır](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Daha ileriye devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için eşlemeyi Microsoft etkinleştirmek isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin.

  ![Microsoft eşlemesi listesi](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

  * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
  * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır.
  * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
  * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir ön ek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu ön ekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
  * **İsteğe bağlı -** müşteri ASN'si: eşleme AS numarasına kayıtlı olmayan önekler Tanıtıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.
  * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
  * **İsteğe bağlı -** kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki örnekte gösterildiği gibi yapılandırmak için istediğiniz eşlemeyi seçebilirsiniz. Microsoft eşleme satırını seçin.

  ![Microsoft eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Microsoft eşlemesini yapılandırın. Aşağıdaki resimde bir yapılandırma örneği gösterilir:

  ![Microsoft eşlemesini yapılandırın](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin.

  Bağlantı hattınız 'gerekli bir doğrulama' alır, (görüntüde gösterildiği gibi) durumunda, kanıtı destek ekibimize önekleri sahipliğini göstermek için bir destek bileti açmanız gerekir.

  ![Microsoft eşleme yapılandırmasını kaydedin](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Aşağıdaki örnekte gösterildiği gibi bir destek bileti doğrudan portalından açabilirsiniz:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki görüntüye benzer bir şey görürsünüz:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure ortak eşleme özelliklerini görüntüleyebilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="updatemsft"></a>Microsoft eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="private"></a>Azure özel eşleme

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Azure özel eşleme yapılandırmasını silme yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan özel sizin için Azure eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin.

  ![liste](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

  * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
  * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır.
  * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. Bu eşleme için özel bir AS numarası kullanabilirsiniz. 65515’i kullanmadığınızdan emin olun.
  * **İsteğe bağlı -** kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki örnekte gösterildiği gibi Azure Private eşleme satırını seçin:

  ![Özel](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Özel eşleme oluşturun. Aşağıdaki resimde bir yapılandırma örneği gösterilir:

  ![Özel eşleme oluşturun](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey görürsünüz:

  ![Özel eşleme Kaydet](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="getprivate"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure özel eşleme özelliklerini görüntüleyebilirsiniz.

![Özel eşleme görüntüleyin](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz.

![Özel eşleme güncelleştir](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

![Özel eşleme Sil](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="public"></a>Azure ortak eşleme

Bu bölümde, oluşturma, alma, güncelleştirme ve bir expressroute bağlantı hattı için Azure ortak eşleme yapılandırmasını silme yardımcı olur.

### <a name="to-create-azure-public-peering"></a>Azure ortak eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Daha ileriye devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. Bağlantı sağlayıcınız yönetilen Katman 3 Hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Azure ortak sizin için eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için hattınızı oluşturduktan sonra yönetmiyorsa yapılandırmanızı sonraki adımlarla devam edin.

  ![Ortak eşleme listesi](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Bağlantı hattı için Azure ortak eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

  * Birincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
  * İkincil bağlantı için bir /30 alt ağı. Bu geçerli bir ortak IPv4 öneki olmalıdır.
  * Bu eşlemenin kurulacak geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun.
  * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
  * **İsteğe bağlı -** kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki görüntüde gösterildiği gibi Azure public eşleme satırını seçin:

  ![Ortak eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Ortak eşlemeyi yapılandırın. Aşağıdaki resimde bir yapılandırma örneği gösterilir:

  ![Ortak eşlemeyi yapılandırın](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Tüm parametreleri belirledikten sonra yapılandırmayı kaydedin. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey görürsünüz:

  ![Ortak eşleme yapılandırmasını kaydedin](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="getpublic"></a>Azure ortak eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure ortak eşleme özelliklerini görüntüleyebilirsiniz.

![Ortak eşleme özelliklerini görüntüle](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="updatepublic"></a>Azure ortak eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz.

![Ortak eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="deletepublic"></a>Azure ortak eşlemesini silmek için

Aşağıdaki örnekte gösterildiği gibi silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

![Ortak eşleme Sil](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [expressroute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
