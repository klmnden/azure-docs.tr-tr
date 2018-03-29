---
title: XSLT Eşlemleriyle - Azure Logic Apps XML dönüştürme | Microsoft Docs
description: Azure mantıksal uygulamaları ve kurumsal tümleştirme paketi ile XML verileri dönüştürmek için XSLT eşlemeleri ekleme
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
ms.openlocfilehash: 4b4d626028eed09e9ce6a45fa8fa69859c082da7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="add-maps-for-xml-data-transform"></a>XML veri dönüştürme için eşlemeleri ekleme

Enterprise Integration eşlemeleri XML veri biçimleri arasında dönüştürme için kullanır. Bir harita başka bir biçime dönüştürülmesi gereken bir belgedeki verileri tanımlayan bir XML dosyasıdır. 

## <a name="why-use-maps"></a>Maps neden kullanılır?

Düzenli olarak B2B sipariş veya fatura tarihlerini YYYMMDD biçimi kullanan bir müşterinin aldığınız varsayalım. Ancak, kuruluşunuzda MMDDYYY biçimindeki tarihler depolar. Bir harita kullanabilirsiniz *dönüştürme* sipariş veya fatura ayrıntıları müşteri etkinlik veritabanınızda depolamak önce MMDDYYY YYYMMDD tarih biçimine.


## <a name="how-do-i-create-a-map"></a>Bir harita nasıl oluşturulur?

BizTalk tümleştirme projelerle oluşturabilirsiniz [Kurumsal tümleştirme paketi](logic-apps-enterprise-integration-overview.md "enterprise Integration pack hakkında bilgi edinin") Visual Studio 2015 için. Daha sonra görsel öğeleri iki XML şema dosyaları arasında eşlemenize olanak sağlayan bir tümleştirme haritası dosyası oluşturabilirsiniz. Bu projeyi derledikten sonra XSLT belge sahip olur.

Harita bir dış derlemesine başvuru varsa, ardından her ikisini de tümleştirme hesabına karşıya yüklenmelidir. Olması gereken belirli bir sırada ilk derleme ve derlemeye başvuran harita karşıya.


## <a name="how-do-i-add-a-map"></a>Bir harita nasıl eklenir?

1. Azure portalında seçin **Gözat**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Filtre Arama kutusuna **tümleştirme**seçeneğini belirleyip **tümleştirme hesapları** sonuçları listesinden.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Harita eklemek istediğiniz tümleştirme hesabı seçin.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **eşlemeleri** döşeme.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Maps sayfası sonra tercih **Ekle**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Girin bir **adı** haritanızı için. Eşleme dosyasını karşıya yüklemek için klasör simgesine sağ tarafında seçin **harita** metin kutusu. Karşıya yükleme işlemi tamamlandıktan sonra Seç **Tamam**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Azure harita tümleştirme hesabınıza ekledikten sonra harita dosya veya eklenmiş olan gösteren bir ekranda iletisi alırsınız. Bu iletiyi aldıktan sonra Seç **eşlemeleri** yeni eklenen harita görüntüleyebilmeniz döşeme.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Bir derlemeyi nasıl eklenir?
Derleme karşıya yüklemek istediğiniz tümleştirme hesabını açın.

1. Seçin **derlemeleri** döşeme.

    ![integrationaccount derleme döşeme](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Derlemeleri sayfası sonra tercih **Ekle**. Girin bir **adı** derlemenizi için. Derleme dosyasını karşıya yüklemek için klasör simgesine sağ tarafında seçin **derleme** metin kutusu. Karşıya yükleme işlemi tamamlandıktan sonra Seç **Tamam**.

    ![derleme Ekle](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Bir harita düzenleme nasıl?

Yeni bir eşleme dosyası, istediğiniz değişiklikleri yüklemeniz gerekir. İlk düzenlemek için harita indirebilirsiniz.

Varolan eşlemeyi yerini alan yeni bir harita karşıya yüklemek için aşağıdaki adımları izleyin.

1. Seçin **eşlemeleri** döşeme.

2. Maps sayfa açıldıktan sonra düzenlemek istediğiniz harita seçin.

3. Üzerinde **eşlemeleri** sayfasında, **güncelleştirme**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Dosya seçiciyi karşıya yükleyin ve ardından istediğiniz eşleme dosyasını seçin **açık**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Bir harita silmek nasıl?

1. Seçin **eşlemeleri** döşeme.

2. Maps sayfa açıldıktan sonra silmek istediğiniz harita seçin.

3. Seçin **silmek**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Harita silmek istediğinizi onaylayın.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Sonraki Adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Anlaşmaları hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmaları hakkında bilgi edinin")  
* [Dönüşümler hakkında daha fazla bilgi](logic-apps-enterprise-integration-transform.md "Kurumsal tümleştirme dönüşümler hakkında bilgi edinin")  

