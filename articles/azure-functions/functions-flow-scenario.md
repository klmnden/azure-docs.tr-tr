---
title: Microsoft Flow bir Azure işlevini çağırabilir | Microsoft Docs
description: Özel bağlayıcı oluşturma, ardından bu bağlayıcıyı kullanarak bir işlev çağırın.
services: functions
keywords: Bulut uygulamaları, bulut Hizmetleri, Microsoft Flow, iş süreçleri, iş kolu uygulaması
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: glenga
ms.reviewer: sunayv
ms.custom: ''
ms.openlocfilehash: d3e777b5611dec382dc4eaaac5ec1594abcdab31
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787682"
---
# <a name="call-a-function-from-microsoft-flow"></a>Microsoft Flow’dan işlev çağırma

[Microsoft Flow](https://flow.microsoft.com/) iş akışları ve sık kullandığınız uygulamalar ve hizmetler arasında iş süreçlerini otomatik hale getirmek kolay hale getirir. Profesyonel Geliştiriciler, Azure işlevleri, teknik ayrıntıları gelen akış oluşturucular koruma sırasında Microsoft Flow yeteneklerini genişletmek için kullanabilirsiniz.

Bu konuda bakım senaryosunda Rüzgar turbines için temel bir akış oluşturun. Bu konu içinde tanımlanan işlevinin nasıl çağrıldığını gösterir [bir işlev için Openapı tanımı oluşturma](functions-openapi-definition.md). İşlevi, bir Rüzgar türbini Acil onarımın uygun maliyetli olup olmadığını belirler. Akış, uygun maliyetli olması durumunda, onarım önermek için e-posta gönderir.

PowerApps ile aynı işlevi çağırma hakkında daha fazla bilgi için bkz: [Powerapps'ten bir işlev çağırma](functions-powerapps-scenario.md).

Bu konu başlığında, şunların nasıl yapılır:

> [!div class="checklist"]
> * SharePoint'te bir liste oluşturur.
> * Bir API tanımını dışarı aktarın.
> * API için bir bağlantı ekleyin.
> * Bir onarım uygun maliyetli olması durumunda e-posta göndermek için bir akış oluşturun.
> * Akışı çalıştırın.

[!INCLUDE [functions-openapi-note](../../includes/functions-openapi-note.md)]

## <a name="prerequisites"></a>Önkoşullar

+ Etkin bir [Microsoft Flow hesabı](https://flow.microsoft.com/documentation/sign-up-sign-in/) Azure hesabınızla oturum açma kimlik ile. 
+ SharePoint, bu akış için bir veri kaynağı olarak kullanın. Kaydolun [bir Office 365 deneme](https://signup.microsoft.com/Signup?OfferId=467eab54-127b-42d3-b046-3844b860bebf&dl=O365_BUSINESS_PREMIUM&ali=1) SharePoint zaten yoksa.
+ Bu öğreticiyi tamamlamak [bir işlev için Openapı tanımı oluşturma](functions-openapi-definition.md).

## <a name="create-a-sharepoint-list"></a>Bir SharePoint listesi oluşturun
Akış veri kaynağı olarak kullanan bir liste oluşturarak başlayın. Listesinde aşağıdaki sütunlar vardır.

| Liste sütunu     | Veri Türü           | Notlar                                    |
|-----------------|---------------------|------------------------------------------|
| **Başlık**           | Tek metin satırı | Türbinin adı                      |
| **LastServiceDate** | Tarih                |                                          |
| **MaxOutput**       | Sayı              | Kwh türbinin çıktısı            |
| **ServiceRequired** | Evet/Hayır              |                                          |
| **EstimatedEffort** | Sayı              | Saat cinsinden onarım için tahmini süre |

1. SharePoint sitenizde tıklayın veya dokunun **yeni**, ardından **listesi**.

    ![Yeni SharePoint listesi oluşturun](./media/functions-flow-scenario/new-list.png)

2. Bir ad girin `Turbines`'a tıklayın veya dokunun **Oluştur**.

    ![Yeni liste için bir ad belirtin](./media/functions-flow-scenario/create-list.png)

    **Turbines** listesi oluşturulur, varsayılan **başlık** alan.

    ![Turbines listesi](./media/functions-flow-scenario/initial-list.png)

3. ' A tıklayın veya dokunun ![yeni öğe simgesi](./media/functions-flow-scenario/icon-new.png) ardından **tarih**.

    ![Tek satır metin alanı ekleyin](./media/functions-flow-scenario/add-column.png)

4. Bir ad girin `LastServiceDate`'a tıklayın veya dokunun **Oluştur**.

    ![LastServiceDate sütunu oluşturma](./media/functions-flow-scenario/date-column.png)

5. Diğer üç sütun listesinde için son iki adımı yineleyin:

    1. **Sayı** > "MaxOutput"

    2. **Evet/Hayır** > "ServiceRequired"

    3. **Sayı** > "EstimatedEffort"

Şimdilik hepsi bu kadar - şu resimdeki gibi görünür, boş bir liste olması gerekir. Akış oluşturduktan sonra veri listesine ekleyin.

![Boş liste](media/functions-flow-scenario/empty-list.png)

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>API'ye bağlantı ekleme
Özel API (aynı zamanda özel bağlayıcı olarak da bilinir), Microsoft Flow içinde kullanılabilir, ancak bir akışta kullanabilmek için önce bir API'ye bağlantısı gerekir.

1. İçinde [flow.microsoft.com](https://flow.microsoft.com), dişli simgesine (sağ üst köşede) tıklayın ve ardından tıklayın **bağlantıları**.

    ![Akış bağlantıları](media/functions-flow-scenario/flow-connections.png)

1. Tıklayın **bağlantı oluştur**, ekranı aşağı kaydırarak **Türbin onarımı** bağlayıcısını bulup tıklayın.

    ![Bağlantı oluşturma](media/functions-flow-scenario/create-connection.png)

1. API anahtarını girin ve tıklayın **Mumbai'ye**.

    ![API anahtarını girin ve Oluştur](media/functions-flow-scenario/api-key.png)

> [!NOTE]
> Akışınız başkalarıyla paylaştığınızda, çalışıyor veya flow kullanan her kişi API'sine bağlanmak için API anahtarı da girmeniz gerekir. Bu davranış gelecekte değişebilir ve, değişimi yansıtmak için bu konuyu güncelleştireceğiz.


## <a name="create-a-flow"></a>Akış oluşturun

Artık özel bağlayıcı ile oluşturduğunuz SharePoint listesi kullanan bir akış oluşturmak hazırsınız.

### <a name="add-a-trigger-and-specify-a-condition"></a>Bir tetikleyici ekleme ve bir koşulu belirtin

İlk (şablon) olmadan boş bir akış oluşturma ve ekleme bir *tetikleyici* SharePoint listesinde bir öğe oluşturulduğunda tetiklenir. Daha sonra eklediğiniz bir *koşul* ne olacağını belirlemek için.

1. İçinde [flow.microsoft.com](https://flow.microsoft.com), tıklayın **Akışlarım**, ardından **boş akış Oluştur**.

    ![Boş akış oluştur](media/functions-flow-scenario/create-from-blank.png)

2. SharePoint tetikleyicisini tıklayın **bir öğe oluşturulduğunda**.

    ![Bir tetikleyici seçin](media/functions-flow-scenario/choose-trigger.png)

    SharePoint ile henüz oturum açmadıysanız, bunu yapmak için istenir.

3. İçin **Site adresi**, SharePoint site adınızı girin ve **liste adı**, türbinin verileri içeren bir liste girin.

    ![Bir tetikleyici seçin](media/functions-flow-scenario/site-list.png)

4. Tıklayın **yeni adım**, ardından **koşul Ekle**.

    ![Koşul Ekle](media/functions-flow-scenario/add-condition.png)

    Microsoft Flow, akışı için iki dal ekler: **Yanıt Evet ise** ve **hiçbir**. Eşleştirmek istediğiniz koşulu tanımlandıktan sonra bir veya iki dalı için adımları ekleyin.

    ![Koşul dallar](media/functions-flow-scenario/condition-branches.png)

5. Üzerinde **koşul** kart, ilk kutuyu tıklayın ve ardından seçin **ServiceRequired** gelen **dinamik içerik** iletişim kutusu.

    ![Koşul için bir alan seçin](media/functions-flow-scenario/condition1-field.png)

6. Değeri girin `True` koşul.

    ![Koşul için true girin](media/functions-flow-scenario/condition1-yes.png)

    Değeri olarak gösterilen `Yes` veya `No` SharePoint listesi, ancak depolanan olarak bir *Boole*, ya da `True` veya `False`. 

7. Tıklayın **akış oluşturma** sayfanın üstünde. Tıkladığınızdan emin olun **güncelleştirme akış** düzenli aralıklarla.

Varsa, akış listesinde oluşturduğunuz tüm öğeler için denetler **ServiceRequired** ayarlanmış `Yes`, ardından gider **Evet ise** dal veya **hiçbir** olarak dal uygun. Yalnızca belirttiğiniz eylemleri için bu konudaki zaman kazanmak için **Evet ise** dal.

### <a name="add-the-custom-connector"></a>Özel bağlayıcıyı ekleyin

Artık Azure'da işlevi çağıran özel bağlayıcıyı ekleyin. Flow standart bir bağlayıcı gibi özel bağlayıcıyı ekleyin. 

1. İçinde **Evet ise** dal, tıklayın **Eylem Ekle**.

    ![Eylem ekleme](media/functions-flow-scenario/condition1-yes-add-action.png)

2. İçinde **eylem seçin** iletişim kutusu, arama `Turbine Repair`, eylemin ardından **Türbin onarımı - maliyetleri hesaplar**.

    ![Eylem seçin](media/functions-flow-scenario/choose-turbine-repair.png)

    Aşağıdaki görüntüde akışına eklenen kart gösterilmektedir. Bağlayıcı için Openapı tanımını alanları ve açıklamalar gelir.

    ![Maliyetleri varsayılan hesaplar](media/functions-flow-scenario/calculates-costs-default.png)

3. Üzerinde **hesaplar maliyetleri** kartında, kullanın **dinamik içerik** girişleri için işlev seçmek için iletişim kutusu. Microsoft Flow, sayısal alanlar, ancak tarih alanı Openapı tanımını belirttiğinden gösterir **saat** ve **kapasite** sayısal olmasına.

    İçin **saat**seçin **EstimatedEffort**ve **kapasite**seçin **MaxOutput**.

    ![Eylem seçin](media/functions-flow-scenario/calculates-costs-fields.png)

     Şimdi temel işlev çıkışı üzerinde başka bir koşul ekleyin.

4. Sayfanın alt kısmında **Evet ise** dal, tıklayın **daha fazla**, ardından **koşul Ekle**.

    ![Koşul Ekle](media/functions-flow-scenario/condition2-add.png)

5. Üzerinde **koşul 2** kart, ilk kutuyu tıklayın ve ardından seçin **ileti** gelen **dinamik içerik** iletişim kutusu.

    ![Koşul için bir alan seçin](media/functions-flow-scenario/condition2-field.png)

6. Değeri girin `Yes`. Sonraki akışı gider **Evet ise** dal veya **hiçbir** dalı temel işlev tarafından döndürülen ileti (olun onarım) yes veya no alır (onarım yapmayın). 

    ![Evet koşulunu girin](media/functions-flow-scenario/condition2-yes.png)

Akış şimdi şu resimdeki gibi görünmelidir.

![Evet koşulunu girin](media/functions-flow-scenario/flow-checkpoint1.png)

### <a name="send-email-based-on-function-results"></a>İşlev sonuçlarına göre e-posta Gönder

Bu noktada, işlevi flow'da döndürdü bir **ileti** değerini `Yes` veya `No` maliyetleri ve olası gelir üzerindeki diğer bilgilerin yanı sıra işlev. İçinde **Evet ise** dal ikinci koşulu, bir e-posta gönderir, ancak herhangi bir sayıda SharePoint listesine geri yazma veya başlatma gibi şeyleri yapabilirsiniz bir [onay işlemi](https://flow.microsoft.com/documentation/modern-approvals/).

1. İçinde **Evet ise** dal ikinci koşulu, tıklayın **Eylem Ekle**.

    ![Eylem ekleme](media/functions-flow-scenario/condition2-yes-add-action.png)

2. İçinde **eylem seçin** iletişim kutusu, arama `email`, ardından (Bu durum Outlook'ta) e-posta sistemine dayalı bir gönderme e-posta eylemini seçin.

    ![Outlook Gönder e-posta](media/functions-flow-scenario/outlook-send-email.png)

3. Üzerinde **bir e-posta** kartında, e-posta oluştur. Kuruluşunuz için geçerli bir ad girin **için** alan. Aşağıdaki resimde, diğer alanlara metin ve belirteçleri olduğunu görebilirsiniz **dinamik içerik** iletişim kutusu. 

    ![E-posta alanları](media/functions-flow-scenario/email-fields.png)

    **Başlık** belirteci gelen SharePoint listesinden ve **CostToFix** ve **RevenueOpportunity** işlevi tarafından döndürülür.

    Tamamlanmış akış aşağıdaki görüntüye benzemesi gerekir (biz sol ilk **hiçbir** yer kazanmak için dal).

    ![Tam akış](media/functions-flow-scenario/complete-flow.png)

4. Tıklayın **güncelleştirme akış** sayfanın üstünde, ardından **Bitti**.

## <a name="run-the-flow"></a>Akışı Çalıştır

Akış tamamlandı, SharePoint listesine bir satır ekleyin ve akışın nasıl yanıt vereceğini bakın.

1. SharePoint listesine geri dönün ve tıklayın **hızlı düzenleme**.

    ![Hızlı Düzenle](media/functions-flow-scenario/quick-edit.png)

2. Düzen kılavuz aşağıdaki değerleri girin.

    | Liste sütunu     | Değer           |
    |-----------------|---------------------|
    | **Başlık**           | Türbinin 60 |
    | **LastServiceDate** | 08/04/2017 |
    | **MaxOutput**       | 2500 |
    | **ServiceRequired** | Evet |
    | **EstimatedEffort** | 10 |

3. **Bitti**’ye tıklayın.

    ![Hızlı düzenleme bitti](media/functions-flow-scenario/quick-edit-done.png)

    Öğe eklemek sonraki göz atın akışı tetikler.

4. İçinde [flow.microsoft.com](https://flow.microsoft.com), tıklayın **Akışlarım**, oluşturduğunuz akış'a tıklayın.

    ![Akışlarım](media/functions-flow-scenario/my-flows.png)

5. Altında **ÇALIŞTIRMA GEÇMİŞİ**, akış çalıştırması tıklayın.

    ![Çalıştırma geçmişi](media/functions-flow-scenario/run-history.png)

    Çalıştırma başarılı olduysa, akış işlemleri bir sonraki sayfada inceleyebilirsiniz. Çalıştırma için herhangi bir nedenle başarısız olursa, sonraki sayfaya sorun giderme bilgileri sağlar.

6. Kartların akışı sırasında ne olduğunu görmek için genişletin. Örneğin, genişletme **hesaplar maliyetleri** girişleri ve çıkışları işlevi görmek için kart. 

    ![Maliyetleri girişler ve çıkışlar hesaplar](media/functions-flow-scenario/calculates-costs-outputs.png)

7. E-posta hesabı olarak belirtilen kişinin kontrol edin **için** alanını **bir e-posta** kart. Flow'dan gönderilen e-posta, aşağıdaki resimdeki gibi görünmelidir.

    ![Flow e-posta](media/functions-flow-scenario/flow-email.png)

    SharePoint listesi ve işlevi doğru değerlerle belirteçlerin nasıl değiştirilmiştir görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu konu başlığında, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * SharePoint'te bir liste oluşturur.
> * Bir API tanımını dışarı aktarın.
> * API için bir bağlantı ekleyin.
> * Bir onarım uygun maliyetli olması durumunda e-posta göndermek için bir akış oluşturun.
> * Akışı çalıştırın.

Microsoft Flow hakkında daha fazla bilgi için bkz: [Microsoft Flow ile çalışmaya başlama](https://flow.microsoft.com/documentation/getting-started/).

Azure işlevleri'ni kullanan diğer ilginç senaryoları hakkında bilgi edinmek için [Powerapps'ten bir işlev çağırma](functions-powerapps-scenario.md) ve [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).
