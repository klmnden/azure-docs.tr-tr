---
title: Teklif ayarları için bir Power BI uygulaması - Azure Marketi'nde teklif | Microsoft Docs
description: Microsoft AppSource Marketplace için Power BI uygulaması teklif için teklif ayarlarını yapılandırın.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: fef2455129d94913c5b51a08c04403834861bb48
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666789"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Power BI uygulamaları sunar Ayarlar sekmesi

**Yeni teklif** hizmet uygulamaları sayfası adlı birinci sekmede açılır **teklif ayarları**.  Bu sekmede, teklif için ad ve birincil tanımlayıcıları sağlayacaktır.  Alan adı eklenmiş bir yıldız (*) gerekli olduğunu gösterir.

![Teklif ayarları sekmesi](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Teklif ayarları alanları 

İçinde **teklif ayarları** sekmesinde, aşağıdaki gerekli alanları sağlamanız gerekir.

|  Alan        |  Açıklama                                                               |
|---------------|----------------------------------------------------------------------------|
| **Teklif kimliği**  | Teklif için benzersiz bir tanımlayıcı (profilinde yayımcı). Bu tanımlayıcı ürün URL'ler, Resource Manager şablonları, görünür ve faturalandırma raporlar. En fazla 50 karakter uzunluğunda, yalnızca küçük harf alfasayısal karakterler ve tire (-) oluşturulmuş olabilir ancak tire bitemez. Bu alan, teklif Canlı geçtikten sonra değiştirilemez. Bir teklif olan contoso yayımlar, örneğin, Teklif kimliği `sample-SvcApp`, AppSource URL atanır `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Yayımcı** | Kuruluşunuzun içindeki benzersiz tanımlayıcısı [AppSource](https://appsource.microsoft.com). Tüm teklifleri, yayımcı kimliği ile ilişkili olmalıdır Teklif kaydedildikten sonra bu değer değiştirilemez.                         |
| **Ad**      | Teklifiniz için görünen ad. Bu ad, appsource'ta ve bulut iş ortağı Portalı'nda görüntülenir. En fazla 50 karakter olabilir. Bir kılavuz ürününüzün tanınabilir bir marka adı eklemektir. Uygulamayı nasıl pazarlanmadığından olmadığı sürece, kuruluşunuzun adını buraya dahil değildir. Bu teklif diğer Web sitelerinde ve yayın'nu pazarlama, adı aynı tüm yayınları arasında olduğundan emin olun.    <br/>Power BI uygulamaları, Önizleme modunu, Önizleme süresince teklif serbest bırakırsanız dizesini `(Preview)` Örneğin, uygulamanızın adı için `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Sonraki adımlar

Sonraki sekmede, belirteceğinizi [teknik bilgi](./cpp-technical-info-tab.md) teklifiniz için.
