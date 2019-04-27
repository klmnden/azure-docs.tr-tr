---
title: Teklif ayarları için bir Power BI uygulaması - Azure Marketi'nde teklif | Microsoft Docs
description: Bir Power BI uygulaması teklif Microsoft AppSource Market teklifi ayarlarını yapılandırın.
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
ms.openlocfilehash: 49bbe5dcf17a9aa20cb47f477c59e7115d29dacc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60727956"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Power BI uygulamaları sunar Ayarlar sekmesi

Açtığınızda **yeni teklif** sayfasında hizmet uygulamaları için ilk gördüğünüz **teklif ayarları** sekmesi. Bu sekmede birincil tanımlayıcıları ve teklifiniz için bir ad sağlayın. Bir yıldız işareti (*) gerekli bir alanı belirtir.

![Teklif Ayarları sekmesi](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Teklif ayarları alanları 

Üzerinde **teklif ayarları** sekmesinde gereken aşağıdaki gerekli alanlara bu bilgileri girin:

|  Alan        |  Açıklama                                                               |
|---------------|----------------------------------------------------------------------------|
| **Teklif kimliği**  | Teklif için benzersiz bir tanımlayıcı (profilinde yayımcı). Bu tanımlayıcı ürün URL'leri, Azure Resource Manager şablonları, görünür ve faturalandırma raporlar. En fazla 50 karakterdir. Yalnızca küçük harf alfasayısal karakterler ve tire (-) içerebilir. Bu, kısa çizgi ile bitemez. Bu tanımlayıcı, bir teklif Canlı geçtikten sonra değiştirilemez. Contoso bir teklifle yayımlarsa Teklif kimliği `sample-SvcApp`, teklif AppSource URL atanır `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Yayımcı** | Kuruluşunuzun içindeki benzersiz tanımlayıcısı [AppSource](https://appsource.microsoft.com). Tüm teklifleri, yayımcı kimliği ile ilişkili olmalıdır Teklif kaydedildikten sonra bu değer değiştirilemez.                         |
| **Ad**      | Teklifiniz için bir görünen ad. Bu ad, bulut iş ortağı portalı ve AppSource üzerinde görünür. En fazla 50 karakterdir. Ürününüzün tanınabilir bir marka adı kullanın. Uygulama, bu ada sahip pazarlanmadığından sürece, kuruluşunuzun adını buraya dahil değildir. Bu teklif, diğer Web siteleri ve yayınlar sağlıyorsanız tüm yayınlarında aynı adı kullanın.    <br/>Önizleme dönemi boyunca bir teklif için Power BI uygulamaları sürüm dizesi ekleyin `(Preview)` bunun gibi uygulamanızın adının sonunda: `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Sonraki adımlar

Sonraki sekmede, belirtirsiniz [teknik bilgi](./cpp-technical-info-tab.md) teklifiniz için.
