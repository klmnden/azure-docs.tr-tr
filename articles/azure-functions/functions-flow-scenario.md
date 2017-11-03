---
title: "Microsoft Flow Azure bir işlevi çağırmak | Microsoft Docs"
description: "Özel bir bağlayıcı oluşturun ardından bu Bağlayıcısı'nı kullanarak bir işlevini çağırın."
services: functions
keywords: "Bulut uygulamaları, bulut Hizmetleri, Microsoft Flow iş süreçlerini iş uygulaması"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 120f5d69441c5e01ffafbdb8dccb179bf00bdb0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-function-from-microsoft-flow"></a>Microsoft Flow’dan işlev çağırma

[Microsoft Flow](https://flow.microsoft.com/) iş akışları ve sık kullanılan uygulamalar ve hizmetler arasındaki iş süreçlerini otomatikleştirmek daha kolay hale gelir. Profesyonel geliştiricilere akış oluşturucular teknik ayrıntıları koruma sırasında Microsoft Flow yeteneklerini artırmak için Azure işlevleri kullanabilirsiniz.

Bu konuda bakım senaryo Rüzgar turbines için temel akış oluşturun. Bu konu içinde tanımlı bir işlevi çağırmak gösterilmiştir [işlevi için bir OpenAPI tanımı oluşturma](functions-openapi-definition.md). İşlev acil bir onarım Rüzgar Türbin üzerinde uygun maliyetli olup olmadığını belirler. Düşük maliyetli, akış onarım önermek için e-posta gönderir.

PowerApps aynı işlevi çağırma hakkında daha fazla bilgi için bkz: [PowerApps bir işlevi çağırmak](functions-powerapps-scenario.md).

Bu konuda, bilgi nasıl yapılır:

> [!div class="checklist"]
> * SharePoint'te bir liste oluşturur.
> * API tanımı verin.
> * API için bağlantı ekleyin.
> * Onarım uygun maliyetli olması durumunda e-posta göndermek için bir akış oluşturun.
> * Akış çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

+ Etkin bir [Microsoft Flow hesap](https://flow.microsoft.com/documentation/sign-up-sign-in/) kimlik bilgileri Azure hesabınız olarak işaretli. 
+ SharePoint, bu akış için bir veri kaynağı olarak kullanın. Kaydolun [bir Office 365 deneme](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1) SharePoint zaten yoksa.
+ Öğreticiyi tamamlamak [işlevi için bir OpenAPI tanımı oluşturma](functions-openapi-definition.md).

## <a name="create-a-sharepoint-list"></a>Bir SharePoint listesi oluşturma
Akış veri kaynağı olarak kullanmak bir liste oluşturarak başlayın. Listesinde aşağıdaki sütunlar var.

| Liste sütunu     | Veri türü           | Notlar                                    |
|-----------------|---------------------|------------------------------------------|
| **Başlık**           | Tek satırlık metin | Türbin adı                      |
| **LastServiceDate** | Tarih                |                                          |
| **MaxOutput**       | Sayı              | Kwh Türbin çıktısı            |
| **ServiceRequired** | Evet/Hayır              |                                          |
| **EstimatedEffort** | Sayı              | Saat cinsinden onarım için tahmini süre |

1. SharePoint siteniz tıklayın veya dokunun **yeni**, ardından **listesi**.

    ![Yeni SharePoint listesi oluşturma](./media/functions-flow-scenario/new-list.png)

2. Bir ad girin `Turbines`, ardından tıklayın veya dokunun **oluşturma**.

    ![Yeni bir liste için bir ad belirtin](./media/functions-flow-scenario/create-list.png)

    **Turbines** listesi oluşturulur, varsayılan **başlık** alan.

    ![Turbines listesi](./media/functions-flow-scenario/initial-list.png)

3. ' A tıklayın veya dokunun ![yeni öğesi simgesi](./media/functions-flow-scenario/icon-new.png) sonra **tarih**.

    ![Tek satırlı metin alanı Ekle](./media/functions-flow-scenario/add-column.png)

4. Bir ad girin `LastServiceDate`, ardından tıklayın veya dokunun **oluşturma**.

    ![LastServiceDate bir sütun oluşturun](./media/functions-flow-scenario/date-column.png)

5. Diğer üç sütun listesinde için son iki adımı tekrarlayın:

    1. **Sayı** > "MaxOutput"

    2. **Evet/Hayır** > "ServiceRequired"

    3. **Sayı** > "EstimatedEffort"

Şu an için bu kadar - aşağıdaki görüntü gibi görünen boş bir liste olması gerekir. Akış oluşturduktan sonra veri listesine ekleyin.

![Boş liste](media/functions-flow-scenario/empty-list.png)

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>API için bir bağlantı Ekle
Özel API (özel bir bağlayıcı olarak da bilinir) Microsoft Flow kullanılabilir, ancak bir akışı kullanabilmeniz için önce bir API için bağlantısı gerekir.

1. İçinde [flow.microsoft.com](https://flow.microsoft.com), dişli simgesine (sağ üstte) tıklayın ve ardından **bağlantıları**.

    ![Akış bağlantıları](media/functions-flow-scenario/flow-connections.png)

1. ' I tıklatın **bağlantı oluşturma**, doğru aşağı kaydırın **Türbin onarım** bağlayıcısını ve tıklatın.

    ![Bağlantı oluşturma](media/functions-flow-scenario/create-connection.png)

1. API anahtarını girin ve **bağlantı oluşturmak**.

    ![API anahtarını girin ve oluşturma](media/functions-flow-scenario/api-key.png)

> [!NOTE]
> Akışınız başkalarıyla paylaşıyorsanız, çalışır veya akış kullanan her kişi API'sine bağlanmak için API anahtarı da girmeniz gerekir. Bu davranış gelecekte değişebilir ve bu konuda, yansıtacak şekilde güncelleştireceğiz.


## <a name="create-a-flow"></a>Bir akış oluşturma

Artık özel bağlayıcı ve oluşturduğunuz SharePoint listesi kullanan bir akışı oluşturmak hazırsınız.

### <a name="add-a-trigger-and-specify-a-condition"></a>Bir tetikleyici ekleyin ve bir koşul belirtin

İlk (olmadan bir şablon) boş bir akış oluşturun ve ekleme bir *tetikleyici* SharePoint listesindeki bir öğeyi oluşturulduğunda ateşlenir. Ardından eklediğiniz bir *koşulu* ne olacağını belirlemek için.

1. İçinde [flow.microsoft.com](https://flow.microsoft.com), tıklatın **My akar**, ardından **boş oluştur**.

    ![boş iken oluşturma](media/functions-flow-scenario/create-from-blank.png)

2. SharePoint tetikleyici tıklatın **öğeyi oluşturulduğunda**.

    ![Tetikleyici seçin](media/functions-flow-scenario/choose-trigger.png)

    SharePoint ile zaten oturum açtınız, bunu yapmak için istenir.

3. İçin **Site adres**, SharePoint site adınızı girin ve **listesi adı**, Türbin veri içeren listesini girin.

    ![Tetikleyici seçin](media/functions-flow-scenario/site-list.png)

4. Tıklatın **yeni adım**, ardından **bir koşul eklemek**.

    ![Koşul ekle](media/functions-flow-scenario/add-condition.png)

    Microsoft Flow akışına iki dalı ekler: **Evet ise** ve **yoksa**. Eşleştirmek istediğiniz koşulu tanımladıktan sonra biri veya her ikisi dalları adımlarını ekleyin.

    ![Koşul dalları](media/functions-flow-scenario/condition-branches.png)

5. Üzerinde **koşulu** kartı, ilk kutusuna tıklayın ve sonra seçin **ServiceRequired** gelen **dinamik içerik** iletişim kutusu.

    ![Koşul için alanını seçin](media/functions-flow-scenario/condition1-field.png)

6. Değeri girin `True` koşul.

    ![Koşul için true girin](media/functions-flow-scenario/condition1-yes.png)

    Değeri olarak gösterilen `Yes` veya `No` SharePoint listesi, ancak depolanan olarak bir *boolean*, her iki `True` veya `False`. 

7. Tıklatın **akışı oluşturmak** sayfanın üst kısmındaki. Tıklattığınızdan emin olun **güncelleştirme akış** düzenli aralıklarla.

Listede oluşturulan tüm öğeler için akışını denetler **ServiceRequired** alan ayarlanmış `Yes`, ardından gider **Evet ise** şube veya **yoksa** olarak dal uygun. Yalnızca belirttiğiniz eylemler için bu konudaki zaman kazanmak için **Evet ise** dal.

### <a name="add-the-custom-connector"></a>Özel bağlayıcı ekleme

Şimdi, Azure'da işlevi çağırır özel bağlayıcı de ekleyin. Standart bir bağlayıcı gibi akış özel bağlayıcı ekleyin. 

1. İçinde **Evet ise** dal, tıklatın **Eylem Ekle**.

    ![Eylem ekleme](media/functions-flow-scenario/condition1-yes-add-action.png)

2. İçinde **bir eylem seçin** iletişim kutusu, arama `Turbine Repair`, eylemi seçin **Türbin onarma - maliyetleri hesaplar**.

    ![Eylem seçin](media/functions-flow-scenario/choose-turbine-repair.png)

    Aşağıdaki resimde akışına eklenir kartı gösterir. Alanları ve açıklamaları bağlayıcı OpenAPI tanımından gelir.

    ![Maliyetleri Varsayılanları hesaplar](media/functions-flow-scenario/calculates-costs-default.png)

3. Üzerinde **hesaplar maliyetleri** kartı, kullanın **dinamik içerik** işlevi için girişleri seçmek için iletişim kutusu. Microsoft Flow, sayısal alanlar ancak tarih alanı OpenAPI tanımı belirttiğinden gösterilmektedir **saatleri** ve **kapasite** sayısal şunlardır.

    İçin **saatleri**seçin **EstimatedEffort**ve **kapasite**seçin **MaxOutput**.

    ![Eylem seçin](media/functions-flow-scenario/calculates-costs-fields.png)

     Şimdi işlevi çıktıya dayalı başka bir koşul ekleyin.

4. Ekranın alt kısmındaki **Evet ise** dal, tıklatın **daha fazla**, ardından **bir koşul eklemek**.

    ![Koşul ekle](media/functions-flow-scenario/condition2-add.png)

5. Üzerinde **koşul 2** kartı, ilk kutusuna tıklayın ve sonra seçin **ileti** gelen **dinamik içerik** iletişim kutusu.

    ![Koşul için alanını seçin](media/functions-flow-scenario/condition2-field.png)

6. Değeri girin `Yes`. Akış sonraki gider **Evet ise** şube veya **yoksa** şube temel işlev tarafından döndürülen ileti (olun onarım) Evet veya Hayır alır (onarım yapmayın). 

    ![Koşul için Evet girin](media/functions-flow-scenario/condition2-yes.png)

Akış, aşağıdaki görüntü gibi görünmelidir.

![Koşul için Evet girin](media/functions-flow-scenario/flow-checkpoint1.png)

### <a name="send-email-based-on-function-results"></a>İşlev sonuçlarına dayalı e-posta Gönder

Akış içinde işlevi bu noktada döndürdü bir **ileti** değerini `Yes` veya `No` maliyetleri ve olası geliri üzerindeki diğer bilgilerin yanı sıra işlevi. İçinde **Evet ise** şube ikinci koşulu, bir e-posta gönderir, ancak herhangi bir sayıda SharePoint listesine geri yazma veya başlatma gibi şeyler yapabilirsiniz, bir [onay işlemi](https://flow.microsoft.com/documentation/modern-approvals/).

1. İçinde **Evet ise** şube ikinci durumunu tıklatın **Eylem Ekle**.

    ![Eylem ekleme](media/functions-flow-scenario/condition2-yes-add-action.png)

2. İçinde **bir eylem seçin** iletişim kutusu, arama `email`, (Bu örnek Outlook'ta) e-posta sistemine dayalı bir gönderme e-posta eylemi seçin.

    ![Outlook gönderme bir e-posta](media/functions-flow-scenario/outlook-send-email.png)

3. Üzerinde **bir e-posta Gönder** kart, bir e-posta oluştur. Kuruluşunuz için geçerli bir ad girin **için** alan. Aşağıdaki resimde, diğer alanları metin ve gelen belirteçleri bileşimi olduğunu görebilirsiniz **dinamik içerik** iletişim kutusu. 

    ![E-posta alanları](media/functions-flow-scenario/email-fields.png)

    **Başlık** belirteci gelen SharePoint listesinden ve **CostToFix** ve **RevenueOpportunity** işlev tarafından döndürülen.

    Tamamlanan akış aşağıdaki görüntü gibi görünmelidir (biz sol ilk **yoksa** alanından tasarruf etmek için şube).

    ![Tam akış](media/functions-flow-scenario/complete-flow.png)

4. Tıklatın **güncelleştirme akış** sayfanın en üstünde ardından **Bitti**.

## <a name="run-the-flow"></a>Akış çalıştırın

Akış tamamladığınıza göre SharePoint listesi bir satır ekleyin ve akışını nasıl yanıt vereceğini görebilirsiniz.

1. SharePoint listesine geri dönün ve tıklatın **hızlı Düzenle**.

    ![Hızlı Düzenle](media/functions-flow-scenario/quick-edit.png)

2. Düzen kılavuzunda aşağıdaki değerleri girin.

    | Liste sütunu     | Değer           |
    |-----------------|---------------------|
    | **Başlık**           | Türbin 60 |
    | **LastServiceDate** | 08/04/2017 |
    | **MaxOutput**       | 2500 |
    | **ServiceRequired** | Evet |
    | **EstimatedEffort** | 10 |

3. **Bitti**’ye tıklayın.

    ![Bitti hızlı Düzenle](media/functions-flow-scenario/quick-edit-done.png)

    Öğe eklediğinizde, sonraki bakalım akışını tetikler.

4. İçinde [flow.microsoft.com](https://flow.microsoft.com), tıklatın **My akar**, oluşturduğunuz akış'a tıklayın.

    ![My akışlar](media/functions-flow-scenario/my-flows.png)

5. Altında **ÇALIŞTIRMA GEÇMİŞİ**, akış Çalıştır'ı tıklatın.

    ![çalıştırma geçmişi](media/functions-flow-scenario/run-history.png)

    Çalıştır başarılı olduysa, bir sonraki sayfada akış işlemleri gözden geçirebilirsiniz. Çalıştır herhangi bir nedenle başarısız olursa, sonraki sayfaya sorun giderme bilgileri sağlar.

6. Akışı sırasında ne olduğunu görmek için kartları genişletin. Örneğin, genişletin **hesaplar maliyetleri** için girişleri ve çıkışları işlevden görmek için kart. 

    ![Maliyetleri girişleri ve çıkışları hesaplar](media/functions-flow-scenario/calculates-costs-outputs.png)

7. E-posta hesabı, belirttiğiniz kişi denetleyin **için** alanını **bir e-posta Gönder** kart. Akıştan gönderilen e-posta aşağıdaki görüntü gibi görünmelidir.

    ![Akış e-posta](media/functions-flow-scenario/flow-email.png)

    SharePoint listesi ve işlev doğru değerlerle belirteçleri nasıl değiştirilmiştir görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu konuda, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * SharePoint'te bir liste oluşturur.
> * API tanımı verin.
> * API için bağlantı ekleyin.
> * Onarım uygun maliyetli olması durumunda e-posta göndermek için bir akış oluşturun.
> * Akış çalıştırın.

Microsoft Flow hakkında daha fazla bilgi için bkz: [Microsoft Flow başlama](https://flow.microsoft.com/documentation/getting-started/).

Azure işlevleri kullanan diğer ilginç senaryoları hakkında bilgi edinmek için [PowerApps bir işlevi çağırmak](functions-powerapps-scenario.md) ve [Azure Logic Apps ile tümleşen bir işlev oluşturun](functions-twitter-email.md).