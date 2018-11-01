---
title: Avere vFXT önkoşulları - Azure
description: Azure için Avere vFXT için Önkoşullar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 823bf50a54ff43fa95f7136c137e3d8f3303c3e0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634380"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Avere vFXT oluşturmak için hazırlık yapma

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
* Kaynak grupları ve sanal ağlar'ı yönetmek küme denetleyicisi düğümü izin ver 
* Küme denetleyicisi oluşturmak ve küme düğümlerini değiştirmek izin ver 

VFXT oluşturan kullanıcılar için sahip erişimi vermek istemiyorsanız iki geçici çözüm vardır:

* Bu koşullar karşılanıyorsa, bir kaynak grubu sahibi bir küme oluşturabilirsiniz:

  * Abonelik sahibi gerekir [Avere vFXT yazılım koşullarını kabul](#accept-software-terms-in-advance) ve [küme düğümü erişim rolünü oluşturma](avere-vfxt-deploy.md#create-the-cluster-node-access-role).
  * Kaynak grubu içinde dağıtılmış olması gereken tüm Avere vFXT kaynaklar dahil olmak üzere:
    * Küme Denetleyicisi
    * Küme düğümleri
    * Blob depolama
    * Ağ öğelerini
 
* Bir ek erişim rolü oluşturulur ve kullanıcıya önceden atanmış hiçbir sahip ayrıcalıklarına sahip bir kullanıcı vFXT kümeleri oluşturabilirsiniz. Ancak, bu rol, bu kullanıcılara önemli izinleri verir. [Bu makalede](avere-vfxt-non-owner.md) sahipleri olmayan kümeleri oluşturmak için yetkilendirme açıklanmaktadır.

## <a name="quota-for-the-vfxt-cluster"></a>Kota vFXT kümesi için

Aşağıdaki Azure bileşenleri için yeterli kotası olması gerekir. Gerekirse, [bir kota artırım talebinde](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

> [!NOTE]
> Burada listelenen SSD bileşenleri ve sanal makineler vFXT kümenin kendisi için var. VM'ler ve işlem grubunuz için kullanmayı düşündüğünüz SSD için ek kota gerekir.  Kota burada iş akışını çalıştırmak istediğiniz bölgeyi etkin olduğundan emin olun.

|Azure bileşeni|Kota|
|----------|-----------|
|Sanal makineler|3 veya daha fazla D16s_v3 veya E32s_v3|
|Premium SSD depolaması|200 GB işletim sistemi boşluk ve düğüm başına 4 TB önbellek alanı 1 TB |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama alanı (isteğe bağlı) |Yeni bir LRS Blob kapsayıcısı |

## <a name="accept-software-terms-in-advance"></a>Yazılım önceden koşulları kabul edin.

> [!NOTE] 
> Abonelik sahibi Avere vFXT kümeyi oluşturur, bu adım gerekli değildir.

Bir küme oluşturmadan önce Avere vFXT yazılım için hizmet koşullarını kabul etmeniz gerekir. Abonelik sahibi değilseniz, abonelik sahibi önceden koşullarını kabul sahip. Bu adım yalnızca abonelik başına bir kez gerçekleştirilmesi gerekir.

Yazılımı önceden koşulları kabul etmek için: 

1. Azure portalında veya atarak bir cloud Shell'i açmak <https://shell.azure.com>. Abonelik kimliğinizi bilgilerinizle oturum açın

   ```azurecli
    az login
    az account set --subscription abc123de-f456-abc7-89de-f01234567890
   ```

1. Hizmet koşullarını kabul edin ve Azure yazılım görüntüleri Avere vFXT için programlı erişimi etkinleştirmek için şu komutu yürütün: 

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-node:latest
   ```

## <a name="next-step-create-the-vfxt-cluster"></a>Sonraki adım: vFXT kümesi oluşturma

Bu Önkoşullar tamamlandıktan sonra küme oluşturmaya atlayabilirsiniz. Okuma [vFXT kümeyi dağıtmak](avere-vfxt-deploy.md) yönergeler için.