---
title: "XML doğrulama - Azure Logic Apps şemalarda | Microsoft Docs"
description: "XML belgeleri şemalarda Azure Logic Apps ve kurumsal tümleştirme paketi için doğrulama"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: db2d9313e443ebc9dd32fcb905b0ae62219e4bbf
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-the-enterprise-integration-pack"></a>XML şeması olan Azure Logic Apps ve kurumsal tümleştirme paketi için doğrulama

Şemalar aldığınız XML belgelerinin geçerli ve beklenen verileri önceden tanımlanmış bir biçime sahip olduğunu onaylayın. Şemalar, B2B senaryoda alınıp verilen iletileri doğrulamak yardımcı olur.

## <a name="add-a-schema"></a>Bir şema ekleyin

1. Azure portalında seçin **tüm hizmetleri**.

    ![Azure portalı, "Tüm hizmetleri"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Filtre Arama kutusuna **tümleştirme**seçip **tümleştirme hesapları** sonuçları listesinden.

    ![Filtre Arama kutusu](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Seçin **tümleştirme hesabını** şema eklemek istediğiniz.

    ![Tümleştirme hesapları listesi](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Seçin **şemaları** döşeme.

    ![Örnek tümleştirme hesabını, "Şemaları"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>2 MB'den küçük bir şema dosyası ekleme

1. İçinde **şemaları** (yukarıdaki adımların) açıldığını seçin dikey **Ekle**.

    ![Şemalar dikey penceresinde, "Ekle"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Şemanızı için bir ad girin. Şema dosyası yanındaki klasör simgesini seçerek karşıya **şema** kutusu. Karşıya yükleme işlemi tamamlandıktan sonra Seç **Tamam**.

    ![Şemanın"Ekle", "küçük dosyası vurgulanmış" ile ekran görüntüsü](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>2 MB (en fazla 8 MB maksimum) daha büyük bir şema dosyası ekleme

Bu adımları blob kapsayıcı erişimi düzeyi göre farklılık: **ortak** veya **anonim erişim yok**.

**Bu erişim düzeyi belirlemek için**

1.  Açık **Azure Storage Gezgini**. 

2.  Altında **Blob kapsayıcıları**, istediğiniz blob kapsayıcısı seçin. 

3.  Seçin **güvenlik**, **erişim düzeyine**.

Blob güvenlik erişim düzeyini ise **ortak**, şu adımları izleyin.

!["Blob kapsayıcıları", "Güvenlik" ve "Genel" vurgulanmış olan Azure Depolama Gezgini](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Şema depolama hesabınıza yüklemeniz ve URI'yi kopyalayın.

    ![Vurgulanan URI ile depolama hesabı](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. İçinde **Şema Ekle**seçin **büyük dosya**ve URI'de sağlamak **içerik URI** metin kutusu.

    !["Ekle" düğmesi ve "büyük dosyası vurgulanmış" ile şemaları](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Blob güvenlik erişim düzeyini ise **anonim erişim yok**, şu adımları izleyin.

!["Blob kapsayıcıları", "Güvenlik" ve "vurgulanmış anonim erişim yok" ile Azure Depolama Gezgini](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Şema depolama hesabınıza yükleyin.

    ![Depolama hesabı](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Şema için bir paylaşılan erişim imzası oluşturur.

    ![Depolama hesabıyla vurgulanmış paylaşılan erişim İmzalar sekmesi](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. İçinde **Şema Ekle**seçin **büyük dosya**ve paylaşılan erişim imzası URI sağlayın, **içerik URI** metin kutusu.

    !["Ekle" düğmesi ve "büyük dosyası vurgulanmış" ile şemaları](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. İçinde **şemaları** tümleştirme hesabınızın dikey penceresinde, yeni eklenen şemanızı görünmelidir.

    !["Şemaları" ve vurgulanmış yeni şema ile tümleştirme hesabınız](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Şemalar Düzenle

1. Seçin **şemaları** döşeme.

2. Sonra **şemaları** dikey penceresi açılır ve düzenlemek istediğiniz şema seçin.

3. Üzerinde **şemaları** dikey penceresinde, seçin **Düzenle**.

    ![Şemalar dikey penceresi](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Düzenleyin ve ardından istediğiniz şema dosyasını Seç **açık**.

    ![Düzenlemek için açık şema dosyası](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Şema başarıyla karşıya ileti Azure gösterir.

## <a name="delete-schemas"></a>Şemalar Sil

1. Seçin **şemaları** döşeme.

2. Sonra **şemaları** dikey penceresi açılır ve silmek istediğiniz şema seçin.

3. Üzerinde **şemaları** dikey penceresinde, seçin **silmek**.

    ![Şemalar dikey penceresi](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Seçili şeması silmek istediğinizi onaylamak için tercih **Evet**.

    !["Şema delete" onay iletisi](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    İçinde **şemaları** dikey penceresinde, şema listesini yeniler ve artık sildiğiniz şeması içerir.

    ![Tümleştirme hesabıyla "vurgulanmış şemaları"](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "enterprise Integration pack hakkında bilgi edinin").  

