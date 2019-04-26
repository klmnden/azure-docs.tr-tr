---
title: En son şema - Azure Logic Apps uygulama geçirme | Microsoft Docs
description: Mantıksal uygulamalarınızı en son şema sürümüne geçirme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: bf27739bd42106550c18e3bbc27a1ff8b3770747
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60447171"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Mantıksal uygulamaları en son şema sürümüne geçirme

Mevcut mantıksal uygulamalarınızı yeni şemaya taşımak için şu adımları izleyin: 

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

2. Mantıksal uygulama menüsünde, **genel bakış**. Araç çubuğunda **Şemayı Güncelleştir**.

   > [!NOTE]
   > Seçeneğini belirlediğinizde **Şemayı Güncelleştir**, Azure Logic Apps, otomatik olarak geçiş adımlarını çalışır ve size kod çıkışı sağlar. Bu çıkış, mantıksal uygulama tanımınızı güncelleştirmek için kullanabilirsiniz. Ancak, aşağıda açıklandığı gibi en iyi yöntemleri izlediğinizden emin olun **en iyi uygulamalar** bölümü.

   ![Şemayı Güncelleştir](./media/connectors-schema-migration/update-schema.png)

   Şemayı Güncelleştir sayfası görünür ve yeni şemadaki geliştirmelerin açıklayan bir belge için bir bağlantı gösterir.

## <a name="best-practices"></a>En iyi uygulamalar

Mantıksal uygulamalarınızı en son şema sürümüne geçirme için bazı en iyi yöntemler şunlardır:

* Geçirilen betiği yeni bir mantıksal uygulamaya kopyalayın. Testinizi tamamlayın ve geçirilen uygulamanız beklendiği gibi çalıştığını onaylayın kadar eskisinin üzerine yazmayın.

* Mantıksal uygulamanızı test **önce** üretime geçmeden.

* Geçişi tamamladıktan sonra kullanmak için mantıksal uygulamalarınızı güncelleştirmeye başlayın [yönetilen API'ler](../connectors/apis-list.md) mümkün olduğunda. Örneğin, Dropbox v2 DropBox v1 kullandığınız her yerde kullanmaya başlayın.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [mantıksal uygulamalarınızı el ile geçirme](../logic-apps/logic-apps-schema-2015-08-01.md)
