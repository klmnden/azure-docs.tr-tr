---
title: "Mantıksal uygulamaları 2015-08-01-önizleme şema sürümüne geçirme | Microsoft Belgeleri"
description: "Mantıksal uygulamalarınızı en son şema sürümüne kolayca geçirebilirsiniz. Şu adımları izlemeniz yeterlidir."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ab22e0369781f9f9afb9b3df9e7fdd54660a959d


---
# <a name="how-to-migrate-logic-apps-to-schema-version-20150801preview"></a>Mantıksal uygulamaları şema sürümü 2015-08-01-önizlemeye geçirme
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
* [Mantıksal uygulamalarınızı el ile geçirmeyi öğrenme](../app-service-logic/app-service-logic-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Nov16_HO2-->


