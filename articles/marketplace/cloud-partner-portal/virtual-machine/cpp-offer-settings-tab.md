---
title: Azure Marketi için bulut iş ortağı Portalı'nda sanal makine teklifi Ayarlar sekmesinde | Microsoft Docs
description: Bir Azure Market VM teklifi oluşturmak için kullanılan teklif Ayarlar sekmesinde açıklar.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 43c830bea57a4c6f3b6c56552dbcacaccc75d54e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844355"
---
# <a name="virtual-machine-offer-settings-tab"></a>Sanal makine teklifi Ayarlar sekmesi

**Yeni teklif** sayfası sanal makineler için adlı birinci sekmede açılır **teklif ayarları**.  Alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir. 

![Sanal makineler için yeni teklif sayfası](./media/publishvm_004.png)

İçinde **teklif ayarları** sekmesinde, aşağıdaki gerekli alanları sağlamanız gerekir.

|  **Alan**       |     **Açıklama**                                                          |
|  ---------       |     ---------------                                                          |
| **Teklif kimliği**       | Teklif için benzersiz bir tanımlayıcı (profilinde yayımcı). Bu tanımlayıcı ürün URL'leri, Azure Resource Manager şablonları, görünür ve faturalandırma raporlar. En fazla 50 karakter uzunluğunda, yalnızca küçük harf alfasayısal karakterler ve tire (-) oluşturulmuş olabilir ancak tire bitemez. Bu alan, teklif Canlı geçtikten sonra değiştirilemez. <br> Bir teklif olan contoso yayımlar, örneğin, Teklif kimliği **örnek vm**, Azure Market URL atanır `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` . |
| **Yayımcı**     | Kuruluşunuzun Azure Marketi'nde benzersiz tanımlayıcısı. Tüm teklifleri, yayımcı kimliği ile ilişkili olmalıdır Teklif kaydedildikten sonra bu değer değiştirilemez. |
| **Ad**          | Teklifiniz için görünen ad. Bu ad, Azure Marketi'nde ve bulut iş ortağı Portalı'nda görüntülenir. En fazla 50 karakter olabilir. Bir kılavuz ürününüzün tanınabilir bir marka adı eklemektir. Nasıl pazarlanmadığından olmadığı sürece, kuruluşunuzun adını buraya dahil değildir. Diğer Web siteleri ve yayınlar bu teklifte pazarlama, adın tam olarak aynı tüm yayınları arasında olduğundan emin olun. |
|  |  |
 
Tıklayın **Kaydet** ilerlemenizi kaydetmek için. Sonraki sekmede ekleyeceksiniz [SKU'ları](./cpp-skus-tab.md) kullanıcınıza teklifiniz için.
