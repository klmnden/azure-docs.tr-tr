---
title: include dosyası
description: include dosyası
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 04/22/2019
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 712b70960e09a9c2b0e7a998bc0bddbc28c1e112
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66146235"
---
| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına VM |25\.000<sup>1</sup> bölge başına. |Bölge başına 25.000. |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına toplam VM çekirdeği sayısı |20<sup>1</sup> bölge başına. | Desteğe başvurun. |
| VM başına gibi Dv2 ve F serisi, çekirdek başına [abonelik](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> bölge başına. | Desteğe başvurun. |
| [Diğer yöneticiler](../articles/billing-add-change-azure-subscription-administrator.md) abonelik başına |Sınırsız. |Sınırsız. |
| [Depolama hesapları](../articles/storage/common/storage-quickstart-create-account.md) her Abonelikteki bölge başına |250 |250 |
| [Kaynak grupları](../articles/azure-resource-manager/resource-group-overview.md) abonelik başına |980 |980 |
| [Kullanılabilirlik kümeleri](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) abonelik başına |Bölge başına 2.000. |Bölge başına 2.000. |
| Azure Resource Manager API'si istek boyutu |4,194,304 bayt. |4,194,304 bayt. |
| Abonelik başına etiket<sup>2</sup> |Sınırsız. |Sınırsız. |
| Abonelik başına benzersiz etiket hesaplaması<sup>2</sup> | 10,000 | 10,000 |
| Abonelik başına [bulut hizmeti](../articles/cloud-services/cloud-services-choose-me.md) sayısı |YOK<sup>3</sup> |YOK<sup>3</sup> |
| Abonelik başına [benzeşim grubu](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) sayısı |YOK<sup>3</sup> |YOK<sup>3</sup> |
| [Abonelik düzeyinde dağıtımlar](../articles/azure-resource-manager/deploy-to-subscription.md) konum başına | 800<sup>4</sup> | 800 |

<sup>1</sup>varsayılan limitler ücretsiz deneme sürümü ve Kullandıkça Öde, gibi teklif kategori türüne ve Dv2, F ve g serisi göre değişir

<sup>2</sup>sınırsız sayıda abonelik başına etiket uygulayabilirsiniz. Kaynak ya da kaynak grubu başına etiket sayısı 15 ile sınırlıdır. Resource Manager döndürür bir [benzersiz etiket adı ve değerlerinin listesini](/rest/api/resources/tags) aboneliği yalnızca gerektiğinde etiket sayısı 10.000 veya daha az. 10\.000 aştığında etikete göre bir kaynak yine de bulabilirsiniz.  

<sup>3</sup>bu özellikler artık Azure kaynak grupları ve Resource Manager ile gerekli değildir.

<sup>4</sup>800 dağıtımları sınırına ulaşırsanız, artık gerekmeyen geçmişinden dağıtımları silin. Abonelik düzeyi dağıtımları silmek için kullanın [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) veya [az silineceği](/cli/azure/deployment?view=azure-cli-latest#az-deployment-delete).

> [!NOTE]
> Sanal makine çekirdeklerinin bölgesel bir toplam limiti vardır. Ayrıca sahip oldukları için bölgesel boyutu serisi, Dv2 ve F. gibi bir sınır Bu sınırlar ayrı ayrı uygulanır. Örneğin, Doğu ABD toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün. Bu aboneliğin 30 adet A1 sanal veya 30 adet D1 sanal makinesi veya ikisinin birleşimini toplamda 30 çekirdeği geçmeyecek dağıtabilirsiniz. Bir birleşim 10 adet A1 sanal ve 20 adet D1 sanal örneğidir.  
> <!-- -->
> 
> 

