---
title: Mantıksal uygulamaları 2015-08-01-önizleme şema sürümüne geçirme | Microsoft Belgeleri
description: Mantıksal uygulamalarınızı en son şema sürümüne kolayca geçirebilirsiniz. Şu adımları izlemeniz yeterlidir.
services: logic-apps
documentationcenter: ''
author: MSFTMAN
manager: erikre
editor: ''
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
ms.openlocfilehash: a5a73a9f124e5339b61dbc49021444a208a471f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "22707133"
---
# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Mantıksal uygulamaları şema sürümü 2015-08-01-önizlemeye geçirme
Mevcut mantıksal uygulamalarınızı yeni şemaya taşımak için aşağıdakileri yapın:  

1. Azure portalda mantıksal uygulamanızı açın  
2. Şemayı Güncelleştir’e tıklayın:
   
   ![API Icon][step1]   
   Şemayı Güncelleştir sayfası, yeni şemadaki geliştirmelerin ayrıntılarını sağlayan bir belgeyi görüntüler ve buna ilişkin bir bağlantı sağlar: ![API Icon][step2]

> [!NOTE]
> **Şemayı Güncelleştir**’i seçtiğinizde, otomatik olarak geçiş adımlarını çalıştırırız ve size kod çıkışı sağlarız. Bunu tanımınızı güncelleştirmek için kullanabilirsiniz, ancak aşağıda **En İyi Uygulamalar** bölümünde belirtilenler gibi iyi kodlama uygulamalarını izlediğinizden emin olun.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Mantıksal uygulamalarınızı en son şema sürümüne geçirirken en iyi uygulamalar:
* Geçirilen betiği yeni Mantıksal Uygulamaya kopyalayın; testinizi tamamlayıncaya ve geçirilen uygulamalarının beklendiği gibi çalıştığını onaylayıncaya kadar eskisinin üzerine yazmayın.
* Üretime geçmeden **önce** Mantıksal uygulamanızı test edin
* Geçiş işlemi tamamlandığında, mümkün olduğunda [yönetilen API’leri](apis-list.md) kullanmak için Mantıksal uygulamalarınızı güncelleştirmeye başlayın. Örneğin, DropBox v1 kullandığınız durumda Dropbox v2 kullanmaya başlayabilirsiniz.

## <a name="whats-next"></a>Sırada ne var?
* [Mantıksal uygulamalarınızı el ile geçirmeyi öğrenme](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






