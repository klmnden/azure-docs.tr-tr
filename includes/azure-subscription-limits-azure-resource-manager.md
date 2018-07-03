---
title: include dosyası
description: include dosyası
services: billing
author: rothja
ms.service: billing
ms.topic: include
ms.date: 07/02/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: f47654d056a26f5a58b1ccc2c200f774bee1f19f
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37348210"
---
| Kaynak | Varsayılan Sınır | Üst Sınır |
| --- | --- | --- |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına VM |Bölge başına 10.000 <sup>1</sup> |Bölge başına 10.000 |
| [Abonelik](../articles/billing-buy-sign-up-azure-subscription.md) başına toplam VM çekirdeği sayısı |Bölge başına 20<sup>1</sup> | Desteğe başvurun |
| Bir [abonelikteki](../articles/billing-buy-sign-up-azure-subscription.md) seri (Dv2, F vb.) çekirdeği başına VM |Bölge başına 20<sup>1</sup> | Desteğe başvurun |
| Abonelik başına [ortak yönetici](../articles/billing-add-change-azure-subscription-administrator.md) sayısı |Sınırsız |Sınırsız |
| [Depolama hesapları](../articles/storage/common/storage-create-storage-account.md) her Abonelikteki bölge başına |200 |200<sup>2</sup> |
| Abonelik başına [Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md) |980 |980 |
| Abonelik başına [Kullanılabilirlik Kümesi](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) |Bölge başına 2.000 |Bölge başına 2.000 |
| Resource Manager API'si Okuma İşlemleri |Saatte 15.000 |Saatte 15.000 |
| Resource Manager API'si Yazma İşlemleri |Saatte 1.200 |Saatte 1.200 |
| Resource Manager API'si istek boyutu |4.194.304 bayt |4.194.304 bayt |
| Abonelik başına etiket<sup>3</sup> |sınırsız |sınırsız |
| Abonelik başına benzersiz etiket hesaplaması<sup>3</sup> | 10,000 | 10,000 |
| Abonelik başına [bulut hizmeti](../articles/cloud-services/cloud-services-choose-me.md) sayısı |Uygulanamaz<sup>4</sup> |Uygulanamaz<sup>4</sup> |
| Abonelik başına [benzeşim grubu](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) sayısı |Uygulanamaz<sup>4</sup> |Uygulanamaz<sup>4</sup> |

<sup>1</sup>Varsayıla limitler Ücretsiz Deneme, Kullandıkça Öde gibi teklif Kategori Türüne ve Dv2, F, G vb. serilere göre farklılık gösterir.

<sup>2</sup>Buna hem Standart hem de Premium depolama hesapları dahildir. 200'den fazla depolama hesabı gerekiyorsa, [Azure Destek](https://azure.microsoft.com/support/faq/) üzerinden bir istek oluşturun. Azure Depolama ekibi, işinizin durumunu inceler ve 250’ye kadar depolama hesabı için onay verebilir.

<sup>3</sup>Abonelik başına sınırsız sayıda etiket uygulayabilirsiniz. Kaynak ya da kaynak grubu başına etiket sayısı 15 ile sınırlıdır. Etiket sayısı 10.000 veya daha az olduğunda Resource Manager yalnızca abonelikteki [benzersiz etiket adı ve değerlerinin listesini](/rest/api/resources/tags#Tags_List) döndürür. Ancak, sayı 10.000’i aştığında bile etikete göre bir kaynak bulabilirsiniz.  

<sup>4</sup>Bu özellikler artık Azure Kaynak Grupları ve Azure Resource Manager ile gerekli değildir.

> [!NOTE]
> Sanal makine çekirdeklerinin ayrı ayrı uygulanan bölgesel bir toplam limiti ve boyut başına bölgesel seri (Dv2, F vb.) limitinin olduğu bilinmelidir.  Örneğin, Doğu ABD toplam VM çekirdek limiti 30, A serisi çekirdek limiti 30 ve D serisi çekirdek limiti 30 olan bir abonelik düşünün.  Bu aboneliğin 30 adet A1 sanal makinesi veya 30 adet D1 sanal makinesi ya da ikisinin toplamda 30 çekirdeği geçmeyecek bir birleşimini (örneğin, 10 adet A1 sanal makinesi ve 20 adet D1 sanal makinesi) dağıtmasına izin verilir.  
> <!-- -->
> 
> 

