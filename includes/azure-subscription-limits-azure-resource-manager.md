---
title: include dosyası
description: include dosyası
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 10/19/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: a1645a8471f75f1d56f3e61c0adfc3fadee14560
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554022"
---
| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına VM |25.000<sup>1</sup> bölge başına. |Bölge başına 25.000. |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına toplam VM çekirdeği sayısı |20<sup>1</sup> bölge başına. | Desteğe başvurun. |
| VM başına gibi Dv2 ve F serisi, çekirdek başına [abonelik](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> bölge başına. | Desteğe başvurun. |
| [Diğer yöneticiler](../articles/billing-add-change-azure-subscription-administrator.md) abonelik başına |Sınırsız. |Sınırsız. |
| [Depolama hesapları](../articles/storage/common/storage-quickstart-create-account.md) her Abonelikteki bölge başına |200 |200<sup>2</sup> |
| [Kaynak grupları](../articles/azure-resource-manager/resource-group-overview.md) abonelik başına |980 |980 |
| [Kullanılabilirlik kümeleri](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) abonelik başına |Bölge başına 2.000. |Bölge başına 2.000. |
| Azure Resource Manager API'si istek boyutu |4,194,304 bayt. |4,194,304 bayt. |
| Abonelik başına etiket<sup>3</sup> |Sınırsız. |Sınırsız. |
| Abonelik başına benzersiz etiket hesaplaması<sup>3</sup> | 10,000 | 10,000 |
| Abonelik başına [bulut hizmeti](../articles/cloud-services/cloud-services-choose-me.md) sayısı |YOK<sup>4</sup> |YOK<sup>4</sup> |
| Abonelik başına [benzeşim grubu](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) sayısı |YOK<sup>4</sup> |YOK<sup>4</sup> |
| [Abonelik düzeyinde dağıtımlar](../articles/azure-resource-manager/deploy-to-subscription.md) konum başına | 800 | 800 |

<sup>1</sup>varsayılan limitler ücretsiz deneme sürümü ve Kullandıkça Öde, gibi teklif kategori türüne ve Dv2, F ve g serisi göre değişir

<sup>2</sup>hem standart ve Premium depolama hesapları dahil edilir. 200'den fazla depolama hesabı gerekiyorsa, bir istekte aracılığıyla [Azure Destek](https://azure.microsoft.com/support/faq/). Azure depolama ekibi, işinizin durumunu inceler ve en fazla 250 depolama hesapları onaylama.

<sup>3</sup>Abonelik başına sınırsız sayıda etiket uygulayabilirsiniz. Kaynak ya da kaynak grubu başına etiket sayısı 15 ile sınırlıdır. Resource Manager döndürür bir [benzersiz etiket adı ve değerlerinin listesini](/rest/api/resources/tags) aboneliği yalnızca gerektiğinde etiket sayısı 10.000 veya daha az. 10.000 aştığında etikete göre bir kaynak yine de bulabilirsiniz.  

<sup>4</sup>bu özellikler artık Azure kaynak grupları ve Resource Manager ile gerekli değildir.

> [!NOTE]
> Sanal makine çekirdeklerinin bölgesel bir toplam limiti vardır. Ayrıca sahip oldukları için bölgesel boyutu serisi, Dv2 ve F. gibi bir sınır Bu sınırlar ayrı ayrı uygulanır. Örneğin, Doğu ABD toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 adet A1 sanal veya 30 adet D1 sanal makinesi veya ikisinin birleşimini toplamda 30 çekirdeği geçmeyecek dağıtabilirsiniz. Bir birleşim 10 adet A1 sanal ve 20 adet D1 sanal örneğidir.  
> <!-- -->
> 
> 

