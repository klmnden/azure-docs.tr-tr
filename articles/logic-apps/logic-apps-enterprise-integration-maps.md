---
title: XSLT eşlemeleri - Azure Logic Apps ile XML dönüştürme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile XML dönüştürme XSLT eşlemeleri ekleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.date: 07/08/2016
ms.openlocfilehash: c5e5e0a0a3f8bd5feedc00d5bbfb76a1453ccc84
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123565"
---
# <a name="add-maps-for-xml-transformation-in-azure-logic-apps-with-enterprise-integration-pack"></a>Azure Logic Apps ile Enterprise Integration Pack, XML dönüştürme için eşlemeler ekleme

Kurumsal tümleştirme, XML veri biçimleri arasında dönüştürmek için haritalar'ı kullanır. Başka bir biçime dönüştürülmesi bir belgedeki verileri tanımlayan bir XML belgesi haritasıdır. 

## <a name="why-use-maps"></a>Haritalar neden kullanmalısınız?

Düzenli olarak B2B sipariş veya fatura tarihleri için YYYMMDD biçimi kullanan bir müşterinin aldığınız varsayalım. Ancak, kuruluşunuzda tarihleri MMDDYYY biçimde depolayın. Bir eşleme için kullanabileceğiniz *dönüştürme* müşteri etkinliği veritabanınızda sırası veya faturayı Ayrıntılar depolanmadan önce MMDDYYY YYYMMDD tarih biçimine.


## <a name="how-do-i-create-a-map"></a>Bir harita nasıl oluşturulur?

İle BizTalk tümleştirme projeleri oluşturabilirsiniz [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "enterprise Integration pack hakkında bilgi edinin") Visual Studio 2015 için. Ardından, görsel öğeleri iki XML şema dosyaları arasında eşleme sağlayan bir tümleştirme Haritası oluşturabilirsiniz. Bu projeyi derledikten sonra XSLT belge olacaktır.

Dış bir derlemeye bir başvuru içeriyor, sonra hem tümleştirme hesabı için karşıya yüklenmelidir. Olması gereken belirli bir sırayla ilk derleme ve derlemeye başvuran harita karşıya yüklendi.


## <a name="how-do-i-add-a-map"></a>Bir harita nasıl ekleyebilirim?

1. Azure portalında **Gözat**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Filtre Arama kutusuna **tümleştirme**, ardından **tümleştirme hesapları** sonuçları listesinde.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Harita eklemek istediğiniz tümleştirme hesabı'nı seçin.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Seçin **haritalar** Döşe.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Eşlemeler sayfası sonra seçin **Ekle**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Girin bir **adı** haritanızı için. Eşleme dosyası karşıya yüklemek için sağ tarafındaki klasör simgesini seçin. **harita** metin kutusu. Karşıya yükleme işlemi tamamlandıktan sonra seçin **Tamam**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Azure harita tümleştirme hesabınıza ekledikten sonra harita dosyanızı veya eklendikten sonra gösteren bir ekran iletisi alırsınız. Bu iletiyi aldıktan sonra seçin **haritalar** yeni eklenen haritada görüntülemek için kutucuk.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Bir derlemenin nasıl ekleyebilirim?
Derlemeyi yüklemek istediğiniz tümleştirme hesabı'nı açın.

1. Seçin **derlemeleri** Döşe.

    ![integrationaccount derleme kutucuğu](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Derlemeleri sayfası sonra seçin **Ekle**. Girin bir **adı** derlemenizi için. Derleme dosyasını karşıya yüklemek için sağ tarafındaki klasör simgesini seçin. **derleme** metin kutusu. Karşıya yükleme işlemi tamamlandıktan sonra seçin **Tamam**.

    ![derleme Ekle](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Bir harita nasıl düzenleyebilirim?

İstediğiniz değişiklikleri içeren yeni bir eşleme dosyası yüklemeniz gerekir. Harita düzenlemek için öncelikle indirebilirsiniz.

Var olan eşleme yerini alan yeni bir harita karşıya yüklemek için aşağıdaki adımları izleyin.

1. Seçin **haritalar** Döşe.

2. Eşlemeler sayfası açıldıktan sonra düzenlemek istediğiniz harita seçin.

3. Üzerinde **haritalar** sayfasında **güncelleştirme**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. Dosya Seçici karşıya yükleyin ve ardından istediğiniz eşleme dosyası seçin **açık**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Bir harita silme

1. Seçin **haritalar** Döşe.

2. Eşlemeler sayfası açıldıktan sonra silmek istediğiniz harita seçin.

3. Seçin **Sil**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Eşlemeyi silmek istediğinizi onaylayın.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Sonraki Adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Sözleşmeleri hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-agreements.md "Kurumsal tümleştirme anlaşmalar hakkında bilgi edinin")  
* [Dönüşümler hakkında daha fazla bilgi](logic-apps-enterprise-integration-transform.md "enterprise integration dönüşümleri hakkında bilgi edinin")  

