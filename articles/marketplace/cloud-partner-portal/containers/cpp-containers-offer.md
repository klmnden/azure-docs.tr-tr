---
title: Azure kapsayıcı görüntüsü teklifi | Azure Market
description: Bir kapsayıcı teklifin Azure Marketi'nde yayımlama işlemine genel bakış.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: pabutler
ms.openlocfilehash: 9617380822f5814d1cbd2bd2b69eac72722fe813
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942745"
---
# <a name="containers"></a>Kapsayıcılar

<table> <tr> <td>Bu bölümde bir kapsayıcı görüntüsüne yayımlama açıklanmaktadır <a href="https://azuremarketplace.microsoft.com">Azure Marketi</a>.  
Docker kapsayıcı görüntüleri sağlanan olarak kapsayıcı teklif türünü destekler <a href="https://docs.microsoft.com/azure/aks/index">Azure Kubernetes hizmeti</a> örnekleri veya <a href="https://docs.microsoft.com/azure/container-instances/container-instances-overview">Azure Container Instances</a> ve barındırılan bir <a href="https://docs.microsoft.com/azure/container-registry">Azure Container Registry </a> depo. </td> <td><img src="./media/container-icon.png"  alt="Azure container icon" /></td> </tr> </table>

## <a name="offer-components"></a>Teklif bileşenleri

Bu bölümde, bir kapsayıcı yayımlama öğeleri özetler ve Azure Marketi yayımcı için bir kılavuz olarak tasarlanmıştır. Yayımlama'nın aşağıdaki ana bölümlere:

- [Önkoşullar](./cpp-prerequisites.md) -oluşturma veya bir kapsayıcı teklifini yayımlamanın önce teknik ve iş gereksinimleri listelenir.
- [Teklif oluşturma](./cpp-create-offer.md) -bulut iş ortağı portalını kullanarak yeni bir kapsayıcı Teklif girişi oluşturmak için gerekli olan adımları listeler.
- [Teknik varlıkları hazırlama](./cpp-create-technical-assets.md) -Azure marketi'ndeki teklif olarak bir kapsayıcı çözümü için teknik varlıkları oluşturma.
- [Teklifi yayımlama](./cpp-publish-offer.md) -teklifi Azure Marketi yayımlama için gönderme.

## <a name="container-publishing-process"></a>Kapsayıcı yayımlama işlemi

Aşağıdaki diyagramda bir VM teklifi yayımlama üst düzey adımları gösterilmektedir.
![Bir teklifi yayımlama adımları](./media/containers-offer-process.png)

Bir kapsayıcı teklif yayımlamak için üst düzey adımlar şunlardır:

1. Teklif oluşturma - teklifi hakkında ayrıntılı bilgi sağlar. Bu bilgiler içerir: pazarlama malzemeleri, destek bilgileri ve varlık belirtimleri teklif açıklaması.
2. İş ve teknik varlıkları oluşturma - iş varlıkları (yasal belgeler ve pazarlama malzemeleri) ve ilişkili çözümü için teknik varlıkları oluşturma (kapsayıcı görüntülerini bir Azure Container Registry'de barındırılan.
3. SKU Oluştur - teklifle ilgili fiyatlarını oluşturun. Yayımlama planlıyorsanız her görüntü için benzersiz bir SKU gereklidir.
4. Onayla ve - teklifi yayımlama teklif ve teknik varlıkları tamamlandıktan sonra teklifi gönderebilirsiniz. Bu gönderi, yayımlama işlemi başlar. Bu işlem sırasında çözümü, doğrulanmış sertifikalı, ardından "Azure Marketi'nde etkin hale gelir" test edilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu adımları düşünmeden önce karşılamanız gereken [teknik ve işle ilgili gereksinimleriniz](./cpp-prerequisites.md) kapsayıcı Microsoft Azure Marketi'nde yayımlama.