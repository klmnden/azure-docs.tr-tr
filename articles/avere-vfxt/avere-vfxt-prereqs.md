---
title: Avere vFXT önkoşulları - Azure
description: Azure için Avere vFXT için Önkoşullar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 352833b12c00abbefcf7016d27dfb580ee25e450
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786449"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Avere vFXT oluşturmaya hazırlanma

Bu makalede bir Avere vFXT kümesi oluşturmak için önkoşul görevleri açıklar.

## <a name="create-a-new-subscription"></a>Yeni abonelik oluştur

Yeni bir Azure aboneliği oluşturarak başlayın. Kolayca tüm proje kaynaklarını ve izlemek, diğer projeleri olası kaynak sağlama işlemi sırasında azaltma korunmasına ve temizlemeyi kolaylaştırmak için her Avere vFXT proje için ayrı bir abonelik kullanın.  

Azure portalında yeni bir Azure aboneliği oluşturmak için:

* Gidin [abonelikler dikey penceresi](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)
* Tıklayın **+ Ekle** üstünde düğme
* İstenirse oturum açın
* Bir teklif seçin ve yeni bir abonelik oluşturmak için adım adım

## <a name="configure-subscription-owner-permissions"></a>Abonelik sahibi izinlerini yapılandırma

Abonelik için sahip izinlerine sahip bir kullanıcı vFXT küme oluşturmanız gerekir. Yazılım hizmet koşullarını kabul edin ve diğer eylemleri gerçekleştirmek için abonelik sahibi izinleri gereklidir. 

Bir Azure kümesi için bir Avere vFTX oluşturmak sahip olmayan izin bazı geçici çözüm senaryolar vardır. Bu senaryolar, kaynakları kısıtlayarak ve ek roller oluşturucusuna atama içerir. Her iki durumda, abonelik sahibi de gerekir [Avere vFXT yazılım koşullarını kabul](#accept-software-terms) önceden. 

| Senaryo | Kısıtlamalar | Avere vFXT kümeyi oluşturmak için gerekli erişim rolleri | 
|----------|--------|-------|
| Kaynak Grubu Yöneticisi | Sanal ağ, küme denetleyici ve küme düğümleri kaynak grubu içinde oluşturulmalıdır | [Kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) rolleri, hem de kapsamlı için hedef kaynak grubu | 
| Dış sanal ağ | Küme düğümlerini ve küme denetleyicisi kaynak grubu içinde oluşturulur ancak farklı bir kaynak grubu mevcut bir sanal ağda kullanılan | (1) [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [katkıda bulunan](../role-based-access-control/built-in-roles.md#contributor) vFXT kaynak grubu; ve (2) için kapsamlı rolleri [sanal makine Katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [kullanıcı erişimi Yönetici](../role-based-access-control/built-in-roles.md#user-access-administrator), ve [Avere katkıda bulunan](../role-based-access-control/built-in-roles.md#avere-contributor) VNET kaynak grubunun kapsamında rolleri. |
 
İçinde anlatıldığı gibi bir özel rol tabanlı erişim denetimi (RBAC) rolüne önceden oluşturup ayrıcalıkları, kullanıcıya atamak için alternatif olan [bu makalede](avere-vfxt-non-owner.md). Bu yöntem, bu kullanıcılara önemli izinleri verir. 

## <a name="quota-for-the-vfxt-cluster"></a>Kota vFXT kümesi için

Aşağıdaki Azure bileşenleri için yeterli kotası olması gerekir. Gerekirse, [bir kota artırım talebinde](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> Burada listelenen SSD bileşenleri ve sanal makineler vFXT kümenin kendisi için var. VM'ler ve işlem grubunuz için kullanmayı düşündüğünüz SSD için ek kota gerekir.  Kota burada iş akışını çalıştırmak istediğiniz bölgeyi etkin olduğundan emin olun.

|Azure bileşeni|Kota|
|----------|-----------|
|Sanal makineler|3 veya daha fazla E32s_v3|
|Premium SSD depolama alanı|200 GB işletim sistemi alanına ek olarak düğüm başına 1 TB-4 TB önbellek alanı |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama alanı (isteğe bağlı) |Yeni bir LRS Blob kapsayıcısı |

## <a name="accept-software-terms"></a>Yazılım koşullarını kabul edin

> [!NOTE] 
> Abonelik sahibi Avere vFXT kümeyi oluşturur, bu adım gerekli değildir.

Küme oluşturma sırasında Avere vFXT yazılım için hizmet koşullarını kabul etmeniz gerekir. Abonelik sahibi değilseniz, abonelik sahibi önceden koşullarını kabul sahip. Bu adım yalnızca abonelik başına bir kez gerçekleştirilmesi gerekir.

Yazılımı önceden koşulları kabul etmek için: 

1. Azure portalında veya atarak bir cloud Shell'i açmak <https://shell.azure.com>. Abonelik kimliğinizi bilgilerinizle oturum açın

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Hizmet koşullarını kabul edin ve Azure yazılım görüntüsünün Avere vFXT için programlı erişimi etkinleştirmek için şu komutu yürütün: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>(Gerekirse) kullanarak sanal ağınızdaki bir depolama hizmet uç noktası oluşturma

A [hizmet uç noktası](../virtual-network/virtual-network-service-endpoints-overview.md) Azure Blob trafiği sanal ağ dışında yönlendirme yerine yerel tutar getirin. Arka uç veri depolama için Azure Blob kullanan Azure küme için tüm Avere vFXT için önerilir. 

Mevcut bir vnet'i sağlayan ve yeni bir Azure Blob kapsayıcısı için arka uç depolama alanınızın kümesi oluşturmanın bir parçası oluşturma, Microsoft depolama için sanal ağ hizmet uç noktası olmalıdır. Bu uç nokta kümesi oluşturmadan önce mevcut olmalıdır veya oluşturma başarısız olur. 

Daha sonra eklediğiniz depolama bile tüm Avere vFXT Azure Blob Depolama kullanan Azure kümesi için depolama hizmet uç noktası önerilir. 

> [!TIP] 
> * Kümeyi oluşturmanın bir parçası yeni bir sanal ağ oluşturuyorsanız, bu adımı atlayın. 
> * Bu adım, küme oluşturma sırasında Blob Depolama oluşturmuyorsanız isteğe bağlıdır. Bu durumda, Azure Blob kullanmaya karar verirseniz, hizmet uç noktası daha sonra oluşturabilirsiniz.

Azure portalında depolama hizmet uç noktası oluşturun. 

1. Portalda, **sanal ağlar** soldaki.
1. Kümenizin bir sanal ağ seçin. 
1. Tıklayın **hizmet uç noktalarını** soldaki.
1. Tıklayın **Ekle** en üstünde.
1. Hizmet olarak bırakın ``Microsoft.Storage`` ve kümenin bulunduğu alt ağ seçin.
1. Alt kısmında tıklayın **Ekle**.

   ![Hizmet uç noktası oluşturma adımları için ek açıklamalar ile Azure portal ekran görüntüsü](media/avere-vfxt-service-endpoint.png)


## <a name="next-step-create-the-vfxt-cluster"></a>Sonraki adım: VFXT kümesi oluşturma

Bu Önkoşullar tamamlandıktan sonra küme oluşturmaya atlayabilirsiniz. Okuma [vFXT kümeyi dağıtmak](avere-vfxt-deploy.md) yönergeler için.