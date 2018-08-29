---
title: XML doğrulama - Azure Logic Apps için şemalar ekleme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile XML belgeleri doğrulamak şemaları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.date: 07/29/2016
ms.openlocfilehash: e03346da1c2b77f885c39d5329f990684979c56e
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123082"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML şemaları Azure Logic Apps Enterprise Integration Pack ile doğrulama

Şemaları aldığınız XML belgelerinin geçerli ve beklenen verileri önceden tanımlanmış bir biçimde sahip onaylayın. Şemalar, B2B senaryoda arasında alınıp verilen iletileri doğrulamak da yardımcı olur.

## <a name="add-a-schema"></a>Bir şema ekleyin

1. Azure portalında **tüm hizmetleri**.

    ![Azure portalı, "Tüm hizmetler"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Filtre Arama kutusuna **tümleştirme**seçip **tümleştirme hesapları** sonuçları listesinde.

    ![Arama kutusuna Filtrele](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Seçin **tümleştirme hesabı** şema eklemek istediğiniz.

    ![Tümleştirme hesapları listesi](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Seçin **şemaları** Döşe.

    ![Örnek tümleştirme hesabı, "Şemaları"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>2 MB'tan daha küçük bir şema dosyası Ekle

1. İçinde **şemaları** (önceki adımlardaki), açılır seçim dikey **Ekle**.

    ![Şemaları dikey penceresinde "Ekle"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Şemanızı için bir ad girin. Şema dosyası yanındaki klasör simgesini seçerek karşıya **şema** kutusu. Karşıya yükleme işlemi tamamlandıktan sonra seçin **Tamam**.

    ![Şemanın"Ekle", "küçük dosya vurgulanmış" ile ekran görüntüsü](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>(En fazla 8 MB maksimum) 2 MB'tan büyük olan bir şema dosyası Ekle

Bu adımlar blob kapsayıcı erişim düzeyine göre farklılık gösterebilir: **genel** veya **anonim erişim yok**.

**Bu erişim düzeyi belirlemek için**

1.  Açık **Azure Depolama Gezgini**. 

2.  Altında **Blob kapsayıcıları**, istediğiniz blob kapsayıcıyı seçin. 

3.  Seçin **güvenlik**, **erişim düzeyi**.

Blob güvenlik erişim düzeyi ise **genel**, şu adımları izleyin.

!["Blob kapsayıcıları", "Güvenlik" ve "Genel" vurgulanmış olan Azure Depolama Gezgini](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Şema, depolama hesabınıza yükleyin ve URI'yi kopyalayın.

    ![Depolama hesabıyla URI'sini vurgulanan](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. İçinde **Şema Ekle**seçin **büyük dosya**ve URI'de **içerik URI** metin kutusu.

    !["Ekle" düğmesi ve "büyük dosya vurgulanmış" şemaları](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Blob güvenlik erişim düzeyi ise **anonim erişim yok**, şu adımları izleyin.

![Azure Depolama Gezgini, "Blob kapsayıcıları", "Güvenlik" ve "vurgulanmış anonim erişim yok"](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Şema, depolama hesabınıza yükleyin.

    ![Depolama hesabı](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Şema için paylaşılan erişim imzası oluşturma.

    ![Paylaşılan erişim imzalar sekmesiyle vurgulanmış depolama hesabı](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. İçinde **Şema Ekle**seçin **büyük dosya**ve paylaşılan erişim imzası URI'si sağlayın, **içerik URI** metin kutusu.

    !["Ekle" düğmesi ve "büyük dosya vurgulanmış" şemaları](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. İçinde **şemaları** , tümleştirme hesabı dikey penceresinde yeni eklenen şemanızı görüntülenmelidir.

    !["Şemaları" ve vurgulanmış yeni şema ile tümleştirme hesabı](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Şemaları Düzenle

1. Seçin **şemaları** Döşe.

2. Sonra **şemaları** dikey penceresi açılır ve düzenlemek istediğiniz şema seçin.

3. Üzerinde **şemaları** dikey penceresinde, seçin **Düzenle**.

    ![Şemaları dikey penceresi](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Şema dosyasını düzenleyin ve ardından istediğiniz seçin **açık**.

    ![Dosyayı düzenlemek için şemayı Aç](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Şema başarıyla karşıya ileti Azure gösterir.

## <a name="delete-schemas"></a>Şemaları Sil

1. Seçin **şemaları** Döşe.

2. Sonra **şemaları** dikey penceresi açıldığında, silmek istediğiniz şema seçin.

3. Üzerinde **şemaları** dikey penceresinde, seçin **Sil**.

    ![Şemaları dikey penceresi](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Seçili şemayı silmek istediğinizi onaylamak için seçin **Evet**.

    ![Onay iletisi "Şema Sil"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    İçinde **şemaları** dikey penceresinde, şema listesini yeniler ve artık sildiğiniz şema içerir.

    ![Tümleştirmenizi hesabıyla "şemaları vurgulanmış"](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "enterprise Integration pack hakkında bilgi edinin").  

