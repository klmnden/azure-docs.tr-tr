---
title: Avere vFXT önkoşulları - Azure
description: Azure için Avere vFXT için Önkoşullar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: v-erkell
ms.openlocfilehash: 04af92f21cecaa832e857a7017b67f815f6ab685
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417981"
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

Abonelik için sahip izinlerine sahip bir kullanıcı vFXT küme oluşturmanız gerekir. Diğerlerinin yanı sıra bu eylemler için abonelik sahibi izinler gerekir:

* Avere vFXT koşulları kabul edin
* Küme düğümü erişim rolü oluşturma 

VFXT oluşturan kullanıcılar için sahip erişimi vermek istemiyorsanız iki geçici çözüm vardır:

* Bu koşullar karşılanıyorsa, bir kaynak grubu sahibi bir küme oluşturabilirsiniz:

  * Abonelik sahibi gerekir [Avere vFXT yazılım koşullarını kabul](#accept-software-terms) ve [küme düğümü erişim rolünü oluşturma](#create-the-cluster-node-access-role). 
  * Kaynak grubu içinde dağıtılmış olması gereken tüm Avere vFXT kaynaklar dahil olmak üzere:
    * Küme Denetleyicisi
    * Küme düğümleri
    * Blob depolama
    * Ağ öğelerini
 
* Hiçbir sahip ayrıcalıklarına sahip bir kullanıcı ayrıcalıkları kullanıcıya atamak için rol tabanlı erişim denetimi (RBAC) önceden kullanarak vFXT kümeleri oluşturabilirsiniz. Bu yöntem, bu kullanıcılara önemli izinleri verir. [Bu makalede](avere-vfxt-non-owner.md) sahipleri olmayan kümeleri oluşturma yetkisi vermek için bir erişim rolünün nasıl oluşturulacağını açıklar.

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

## <a name="create-access-roles"></a>Erişim rolleri oluşturma 

[Rol tabanlı erişim denetimi](../role-based-access-control/index.yml) (RBAC) vFXT küme denetleyici ve küme düğümlerini gerekli görevler gerçekleştirme yetkisi verir.

* Küme denetleyicisi oluşturmak ve kümeyi oluşturmak için Vm'leri değiştirmek için izniniz gerekiyor. 

* Azure kaynak özelliklerini tek tek vFXT düğümleri gibi şeyleri yapmanıza gerek okuma, depolama alanını yönet ve diğer düğümlere ağ arabirimi ayarları normal küme işlemi bir parçası olarak denetim.

Avere vFXT kümenizi oluşturmadan önce küme düğümleri ile kullanılacak özel bir rol tanımlamanız gerekir. 

Küme denetleyici için varsayılan rol şablondan kabul edebilir. Varsayılan Küme Denetleyicisi kaynak grubu sahibi ayrıcalıklar verir. Denetleyici için özel bir rol oluşturmak isterseniz, bkz. [özelleştirilmiş denetleyicisi erişim rolünü](avere-vfxt-controller-role.md).

> [!NOTE] 
> Yalnızca abonelik sahibi veya sahibi veya kullanıcı erişimi Yöneticisi rolüne sahip bir kullanıcı rolleri oluşturabilir. Rolleri önceden oluşturulabilir.  

### <a name="create-the-cluster-node-access-role"></a>Küme düğümü erişim rolü oluşturma

<!-- caution - this header is linked to in the template so don't change it unless you can change that -->

Azure kümesine için Avere vFXT oluşturabilmeniz için önce küme düğümü rolünü oluşturmanız gerekir.

> [!TIP] 
> Microsoft iç kullanıcılar yerine çalışırken "Avere küme çalışma zamanını operatörü" adlı varolan bir rolünü oluşturmak için kullanmanız gerekir. 

1. Bu dosya kopyalayın. Abonelik Kimliğinizi AssignableScopes satır ekleyin.

   (Bu dosyanın geçerli sürümü github.com/Azure/Avere deposu olarak depolanan [AvereOperator.txt](https://github.com/Azure/Avere/blob/master/src/vfxt/src/roles/AvereOperator.txt).)  

   ```json
   {
      "AssignableScopes": [
          "/subscriptions/PUT_YOUR_SUBSCRIPTION_ID_HERE"
      ],
      "Name": "Avere Operator",
      "IsCustom": "true",
      "Description": "Used by the Avere vFXT cluster to manage the cluster",
      "NotActions": [],
      "Actions": [
          "Microsoft.Compute/virtualMachines/read",
          "Microsoft.Network/networkInterfaces/read",
          "Microsoft.Network/networkInterfaces/write",
          "Microsoft.Network/virtualNetworks/read",
          "Microsoft.Network/virtualNetworks/subnets/read",
          "Microsoft.Network/virtualNetworks/subnets/join/action",
          "Microsoft.Network/networkSecurityGroups/join/action",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "DataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ]
   }
   ```

1. Dosyayı Farklı Kaydet ``avere-operator.json`` veya benzer bir akılda kalıcı bir dosya adı. 


1. Bir Azure Cloud shell ve abonelik Kimliğiniz ile oturum açın (açıklanan [bu belgenin daha öncesinde](#accept-software-terms)). Rolü oluşturmak için bu komutu kullanın:

   ```bash
   az role definition create --role-definition /avere-operator.json
   ```

Rol adı, kümeyi oluştururken kullanılır. Bu örnekte, addır ``avere-operator``.

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