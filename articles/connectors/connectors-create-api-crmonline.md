---
title: "Azure mantıksal uygulamaları (çevrimiçi) Dynamics 365 bağlanma | Microsoft Docs"
description: "Dynamics 365 (çevrimiçi) varlıklar Dynamics 365 Bağlayıcısı tarafından sağlanan API aracılığıyla yönetmek uygulama iş mantığı oluşturun"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: d35647921ff540167a3a591fb489d3bab031a5c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-dynamics-365-from-logic-app-workflows"></a>Mantıksal uygulama akışlarından Dynamics 365'e bağlanın

Logic Apps ile (çevrimiçi) Dynamics 365 bağlama ve kayıtları oluşturma, öğeleri güncelleştirmek veya kayıtlar listesi döndüren kullanışlı iş akışları oluşturma. Dynamics 365 Bağlayıcısı ile şunları yapabilirsiniz:

* İş akışınız Dynamics 365'ten (çevrimiçi) alma verileri temel alan oluşturun.
* Bir yanıt ve daha sonra çıktı diğer eylemler için kullanılabilir hale eylemlerini kullanın. Örneğin, bir öğe Dynamics 365'te (çevrimiçi) güncelleştirildiğinde, Office 365 kullanılarak bir e-posta gönderebilirsiniz.

Bu konuda yeni bir sağlama Dynamics 365'te oluşturulduğunda Dynamics 365'te bir görev oluşturan bir mantıksal uygulama oluşturulacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar
* Bir Azure hesabı.
* Dynamics 365 (çevrimiçi) hesabı.

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Yeni bir sağlama Dynamics 365'te oluşturduğunuzda bir görev oluşturma

1.  [Azure'da oturum aç](https://portal.azure.com).

2.  Azure arama kutusuna `Logic apps`, ve ENTER tuşuna basın.

      ![Logic Apps Bul](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  Altında **Logic apps**, tıklatın **Ekle**.

      ![LogicApp Ekle](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  Mantıksal uygulama oluşturmak için tamamlamak **adı**, **abonelik**, **kaynak grubu**, ve **konumu** alanları ve ardından**Oluşturma**.

5.  Yeni mantıksal uygulamaya seçin. Aldığınızda **dağıtımı başarılı oldu** bildirimi tıklatın **yenileme**.

6.  Altında **geliştirme araçları**, tıklatın **mantığı Uygulama Tasarımcısı**. Şablon listesinde tıklayın **boş mantıksal uygulama**.

7.  Arama kutusuna `Dynamics 365`. Dynamics 365 Tetikleyiciler listesinden **Dynamics bir kayıt oluşturulduğunda 365 –**.

8.  Dynamics 365'e oturum açmak için istenirse, bunu şimdi yapın.

9.  Tetikleyici Ayrıntılar aşağıdaki bilgileri girin:

  * **Kuruluş adı**. Dinlemek için mantıksal uygulama istediğiniz Dynamics 365 örneği seçin.

  * **Varlık adı**. Dinlemek istediğiniz varlığı seçin. Bu olay mantıksal uygulamayı başlatmak için bir tetikleyici olarak davranır. 
  Bu kılavuzda **müşteri adayları** seçilir.

  * **Ne sıklıkta öğeleri denetlemek istiyor musunuz?** Mantıksal uygulama tetikleyici ile ilgili güncelleştirmeler için ne sıklıkta denetleyeceğini bu değerleri ayarlayın. Her üç dakikada güncelleştirmeleri denetlemek için varsayılan ayardır.

    * **Sıklık**. Saniye, dakika, saat veya gün seçin.

    * **Aralığı**. Saniye, dakika, saat veya önce sonraki onay geçirmek istediğiniz gün sayısını girin.

      ![Mantıksal uygulama tetikleyici ayrıntıları](./media/connectors-create-api-crmonline/trigger-details.png)

10. Tıklatın **yeni adım**ve ardından **Eylem Ekle**.

11. Arama kutusuna `Dynamics 365`. Eylemler listesinden **Dynamics 365 – yeni bir kayıt oluşturmak**.

12. Aşağıdaki bilgileri girin:

    * **Kuruluş adı**. Akış kaydı oluşturmak için istediğiniz Dynamics 365 örneği seçin. 
    Bu örneği burada gelen olay tetiklenir örnekle aynı olmak zorunda değildir dikkat edin.

    * **Varlık adı**. Olay tetiklendiğinde bir kayıt oluşturmak istediğiniz varlığı seçin. 
    Bu kılavuzda **görevleri** seçilir.

13. ' I tıklatın **konu** görüntülenen kutusunda. Görüntülenen dinamik içerik listeden, bu alanlardan birini seçebilirsiniz:

    * **Soyadı**. Görev kaydı oluşturulduğunda, bu alanın seçilmesi Soyadı sağlama için konu alanı görev ekler.
    * **Konu**. Görev kaydı oluşturulduğunda, bu alanın seçilmesi görev için konu alanına sağlama konu alanı ekler. 
    Tıklatın **konu** olarak eklemek için **konu** kutusu.

      ![Mantıksal uygulama oluşturma yeni kayıt ayrıntıları](./media/connectors-create-api-crmonline/create-record-details.png)

14. Mantıksal Uygulama Tasarımcısı araç çubuğunda tıklatın **kaydetmek**.

    ![Mantıksal Uygulama Tasarımcısı araç Kaydet](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. Mantıksal uygulama başlatmak için tıklatın **çalıştırmak**.

    ![Mantıksal Uygulama Tasarımcısı araç Kaydet](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Artık Dynamics 365 satış sağlama kaydı oluşturun ve akışınız eylem bakın!

## <a name="set-advanced-options-for-a-logic-app-step"></a>Bir mantıksal uygulama adımı için Gelişmiş seçenekleri ayarlama

Bir mantıksal uygulama adımda veri filtreleme belirtmek için tıklatın **Gelişmiş Seçenekleri Göster** sorgu tarafından filtre veya düzeni sonra bu adımda, ekleyin.

Örneğin, yalnızca active hesapları ve sırası tarafından hesap adını almak için bir filtre sorgusu kullanabilirsiniz. Bu görevi gerçekleştirmek için OData filtre sorgusu girin `statuscode eq 1`seçip **hesap adı** dinamik içerik listesinden. Daha fazla bilgi: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) ve [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Gelişmiş Seçenekleri mantıksal uygulama](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Gelişmiş seçenekleri kullanırken en iyi uygulamalar

Bir değeri bir alan eklediğinizde, bir değer yazın ya da dinamik içerik listeden bir değer seçin alan türü eşleşmesi gerekir.

Alan türü  |Nasıl kullanılır  |Nerede bulunacağını  |Ad  |Veri türü  
---------|---------|---------|---------|---------
Metin alanları|Metin alanları tek satırlık bir metin veya metin türü alan dinamik içerik gerektirir. Örnekler kategori ve alt kategori alanlarını içerir.|Ayarlar > özelleştirmeleri > Sistem Özelleştirme > varlıklar > Görev > alanları |category |Tek satırlık metin        
Tamsayı alanları | Bazı alanlar, tamsayı veya tamsayı türünde bir alan dinamik içerik gerektirir. Örnek tamamlanma yüzdesi ve süre verilebilir. |Ayarlar > özelleştirmeleri > Sistem Özelleştirme > varlıklar > Görev > alanları |TamamlanmaYüzdesi |Tam sayı         
Tarih alanları | Bazı alanlar, tarih gg/aa/yyyy biçiminde veya bir tarih alanı dinamik içerik girilmesini gerektirir. Oluşturma tarihi, başlangıç tarihi, gerçek başlatma, son zaman tutun, gerçek bitiş ve son tarih örnekler. | Ayarlar > özelleştirmeleri > Sistem Özelleştirme > varlıklar > Görev > alanları |createdon |Tarih ve saat
Bir kayıt kimliği hem arama gerektiren alanlar yazın |Başka bir varlık kaydı başvuru bazı alanları kayıt kimliği ve arama türü gerektirir. |Ayarlar > özelleştirmeleri > Sistem Özelleştirme > varlıklar > hesabı > alanları  | Hesap Kimliği  | Birincil anahtar

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Daha fazla örnekleri bir kayıt kimliği ve arama gerektiren alanlar yazın
Önceki tabloda genişleterek, daha fazla dinamik içerik listeden seçilen değerlerle çalışmıyor alanları örnekleri aşağıda verilmiştir. Bunun yerine, bu alanların PowerApps alanlara girilen her iki kayıt kimliği ve arama türü gerektirir.  
* Sahibi ve sahip türü. Sahip alanı geçerli bir kullanıcı veya takım kayıt kimliği olmalıdır Sahibi türü ya da olmalıdır **systemusers** veya **takımlar**.
* Müşteri ve müşteri türü. Müşteri alanın geçerli bir hesap olması veya kayıt kimliği başvurun Sahibi türü ya da olmalıdır **hesapları** veya **kişiler**.
* İlgili ve ilgili türü. İlgili alanı bir hesap gibi bir geçerli kayıt kimliği olması veya kayıt kimliği başvurun İlgili kaydı için arama türü gibi türünden olmalıdır **hesapları** veya **kişiler**.

Aşağıdaki görev oluşturma eylem örneği görev ilgi alanına ekleme kayıt kimliği karşılık gelen bir hesap kaydı ekler.

![Akış RecordID ve türü hesabı](./media/connectors-create-api-crmonline/recordid-type-account.png)

Bu örnek ayrıca kullanıcının kaydı kimliğine göre belirli bir kullanıcı bir görev atar.

![Akış RecordID ve türü hesabı](./media/connectors-create-api-crmonline/recordid-type-user.png)

Bir kaydın Kimliğini bulmak için aşağıdaki bölüme bakın: *kayıt kimliği bulunamıyor*

## <a name="find-the-record-id"></a>Kayıt Kimliği bulunamıyor

1. Hesap kaydı gibi bir kaydı açın.

2. Eylemler araç çubuğunda tıklatın **öne çıkar** ![açılan kayıt](./media/connectors-create-api-crmonline/popout-record.png).
Alternatif olarak, varsayılan e-posta programınıza tam URL'yi kopyalamak için Eylemler araç çubuğunda tıklatın **e-posta A bağlantı**.

   Kayıt Kimliği URL'nin karakter kodlama % 7b ve %7 d Between görüntülenir.

   ![Akış RecordID ve türü hesabı](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Sorun giderme
Bir mantıksal uygulama başarısız bir adımda gidermek için olay durum ayrıntılarını görüntüleyin.

1. Altında **Logic Apps**, mantıksal uygulamanızı seçin ve ardından **genel bakış**. 

   Özet alanı gösterilir ve mantıksal uygulama için çalışma durumunu sağlar. 

   ![Mantıksal uygulama çalışma durumu](./media/connectors-create-api-crmonline/tshoot1.png)

2. Tüm başarısız çalışmaları hakkında daha fazla bilgi görüntülemek için başarısız olay'ı tıklatın. Başarısız olan bir adım genişletmek için bu adımı'ı tıklatın.

   ![Başarısız olan adımda genişletin](./media/connectors-create-api-crmonline/tshoot2.png)

   Adım ayrıntıları görünür ve hatanın nedenini gidermenize yardımcı olacak.

   ![Adım ayrıntıları başarısız oldu](./media/connectors-create-api-crmonline/tshoot3.png)

Logic apps sorunlarını giderme hakkında daha fazla bilgi için bkz: [mantığı uygulama hatalarını tanılama](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/crm/). 

## <a name="next-steps"></a>Sonraki adımlar
Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API'leri listesi](apis-list.md).
