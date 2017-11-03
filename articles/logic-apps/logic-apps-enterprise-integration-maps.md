---
title: "XSLT Eşlemleriyle - Azure Logic Apps XML dönüştürme | Microsoft Docs"
description: "Azure mantıksal uygulamaları ve kurumsal tümleştirme paketi ile XML verileri dönüştürmek için XSLT eşlemeleri ekleme"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 4445a84a6c6425110e7d705019a28b5cc5447046
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-maps-for-xml-data-transform"></a>XML veri dönüştürme için eşlemeleri ekleme

Enterprise Integration eşlemeleri XML veri biçimleri arasında dönüştürme için kullanır. Bir harita başka bir biçime dönüştürülmesi gereken bir belgedeki verileri tanımlayan bir XML dosyasıdır. 

## <a name="why-use-maps"></a>Maps neden kullanılır?

Düzenli olarak B2B sipariş veya fatura tarihlerini YYYMMDD biçimi kullanan bir müşterinin aldığınız varsayalım. Ancak, kuruluşunuzda MMDDYYY biçimindeki tarihler depolar. Bir harita kullanabilirsiniz *dönüştürme* sipariş veya fatura ayrıntıları müşteri etkinlik veritabanınızda depolamak önce MMDDYYY YYYMMDD tarih biçimine.

## <a name="how-do-i-create-a-map"></a>Bir harita nasıl oluşturulur?

BizTalk tümleştirme projelerle oluşturabilirsiniz [Kurumsal tümleştirme paketi](logic-apps-enterprise-integration-overview.md "enterprise Integration pack hakkında bilgi edinin") Visual Studio 2015 için. Daha sonra görsel öğeleri iki XML şema dosyaları arasında eşlemenize olanak sağlayan bir tümleştirme haritası dosyası oluşturabilirsiniz. Bu projeyi derledikten sonra XSLT belge sahip olur.

## <a name="how-do-i-add-a-map"></a>Bir harita nasıl eklenir?

1. Azure portalında seçin **Gözat**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Filtre Arama kutusuna **tümleştirme**seçeneğini belirleyip **tümleştirme hesapları** sonuçları listesinden.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Harita eklemek istediğiniz tümleştirme hesabı seçin.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **eşlemeleri** döşeme.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Maps dikey penceresi açıldıktan sonra Seç **Ekle**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Girin bir **adı** haritanızı için. Eşleme dosyasını karşıya yüklemek için klasör simgesine sağ tarafında seçin **harita** metin kutusu. Karşıya yükleme işlemi tamamlandıktan sonra Seç **Tamam**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Azure harita tümleştirme hesabınıza ekledikten sonra harita dosya veya eklenmiş olan gösteren bir ekranda iletisi alırsınız. Bu iletiyi aldıktan sonra Seç **eşlemeleri** yeni eklenen harita görüntüleyebilmeniz döşeme.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Bir harita düzenleme nasıl?

Yeni bir eşleme dosyası, istediğiniz değişiklikleri yüklemeniz gerekir. İlk düzenlemek için harita indirebilirsiniz.

Varolan eşlemeyi yerini alan yeni bir harita karşıya yüklemek için aşağıdaki adımları izleyin.

1. Seçin **eşlemeleri** döşeme.

2. Maps dikey penceresi açıldıktan sonra düzenlemek istediğiniz harita seçin.

3. Üzerinde **eşlemeleri** dikey penceresinde, seçin **güncelleştirme**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Dosya seçiciyi karşıya yükleyin ve ardından istediğiniz eşleme dosyasını seçin **açık**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Bir harita silmek nasıl?

1. Seçin **eşlemeleri** döşeme.

2. Maps dikey penceresi açıldıktan sonra silmek istediğiniz harita seçin.

3. Seçin **silmek**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Harita silmek istediğinizi onaylayın.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Sonraki Adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  
* [Dönüşümler hakkında daha fazla bilgi](logic-apps-enterprise-integration-transform.md "Kurumsal tümleştirme dönüşümler hakkında bilgi edinin")  

