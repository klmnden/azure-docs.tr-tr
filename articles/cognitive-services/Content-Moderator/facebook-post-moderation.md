---
title: 'Öğretici: Facebook içeriğinin moderasyonu - Azure Content Moderator'
titlesuffix: Azure Cognitive Services
description: Facebook sayfalarını Content Moderator ile denetleyin.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: ead8c1d445bf32ecaaf236b4e73c2a583c755049
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223947"
---
# <a name="tutorial-facebook-content-moderation-with-content-moderator"></a>Öğretici: Content Moderator ile Facebook içeriğinin moderasyonu

Bu öğreticide, makine öğrenmesi tabanlı Content Moderator'ı kullanarak Facebook gönderilerini ve yorumlarını denetlemeyi öğreniyoruz.

Öğretici, şu adımlarda size yol gösterir:

1. Content Moderator takımı oluşturma.
2. Content Moderator'dan ve Facebook'tan HTTP olaylarını dinleyen Azure İşlevleri oluşturma.
3. Facebook Sayfası ve Uygulaması oluşturma, bunu Content Moderator'a bağlama.

İşimiz bittiğinde, Facebook ziyaretçiler tarafından gönderilen içeriği Content Moderator'a gönderecek. Eşleştirme eşikleri temelinde, Content Moderator iş akışlarınız içeriği yayımlayacak veya inceleme aracı içinde incelemeler oluşturacak. 

Aşağıdaki şekilde, çözümün yapı taşları gösterilir.

![Facebook gönderilerini denetleme](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Content Moderator takımı oluşturma

Content Moderator'a kaydolmak ve takım oluşturmak için [Hızlı Başlangıç](quick-start.md) sayfasına bakın.

## <a name="configure-image-moderation-workflow-threshold"></a>Görüntü moderasyonu iş akışını (eşik) yapılandırma

Özel bir görüntü iş akışı (eşik) yapılandırmak için [İş Akışları](review-tool-user-guide/workflows.md) sayfasına bakın. İş akışının **adını** not alın.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Metin moderasyonu iş akışını (eşik) yapılandırma

Özel bir metin eşiği ve iş akışı yapılandırmak için [İş Akışları](review-tool-user-guide/workflows.md) sayfasındakilere benzer adımlar kullanın. İş akışının **adını** not alın.

![Metin İş Akışını Yapılandırma](images/text-workflow-configure.PNG)

"İş Akışını Yürüt" düğmesini kullanarak iş akışınızı test edin.

![Metin İş Akışını Test Etme](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure İşlevleri oluşturma

Azure İşlevlerinizi oluşturmak için [Azure Yönetim Portalı](https://portal.azure.com/)'nda oturum açın. Şu adımları uygulayın:

1. [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasında gösterildiği gibi bir Azure İşlev Uygulaması oluşturun.
2. Yeni oluşturulan İşlev Uygulamasını açın.
3. Uygulamanın içinde, **Platform özellikleri -> Uygulama Ayarları**'na gidin
4. Aşağıdaki [uygulama ayarlarını](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings) tanımlayın:

> [!NOTE]
> **cm: Region**, bölgenin adı olmalıdır (boşluk içermemelidir).
> Örneğin Batı Avrupa değil **westeurope**, Orta Batı ABD değil **westcentralus** kullanılır.
>

| Uygulama Ayarı | Açıklama   | 
| -------------------- |-------------|
| cm:TeamId   | Content Moderator Takım Kimliğiniz  | 
| cm:SubscriptionKey | Content Moderator abonelik anahtarınız. Bkz. [Kimlik Bilgileri](review-tool-user-guide/credentials.md) | 
| cm:Region | Content Moderator bölge adınız (boşluk içermez). Önceki nota bakın. |
| cm:ImageWorkflow | Görüntüler üzerinde çalıştırılacak iş akışının adı |
| cm:TextWorkflow | Metinler üzerinde çalıştırılacak iş akışının adı |
| cm:CallbackEndpoint | Bu kılavuzda daha sonra oluşturacağınız CMListener İşlev Uygulamasının Url'si |
| fb:VerificationToken | Facebook akış olaylarına abone olmak için de kullanılan gizli dizi belirteci |
| fb:PageAccessToken | Facebook graph api'si erişim belirtecinin süresi dolmaz ve işlevin sizin adınıza gönderileri Gizlemesini/Silmesini sağlar. |

5. **FBListener** adlı yeni bir **HttpTrigger-CSharp** işlevi oluşturun. Bu işlev Facebook'tan olayları alır. Bu işlevi oluşturmak için aşağıdaki adımları izleyin:

    1. Başvuru amacıyla [Azure İşlevleri Oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasını açık bırakın.
    2. Yeni işlev oluşturmak için **+** ekle'ye tıklayın.
    3. Yerleşik şablonlar yerine, **Kendi/özel işlevinizi başlatma** seçeneğini kullanın.
    4. **HttpTrigger-CSharp** etiketli kutucuğa tıklayın.
    5. **FBListener** adını girin. **Yetkilendirme Düzeyi** alanı **İşlev** olarak ayarlanmalıdır.
    6. **Oluştur**’a tıklayın.
    7. **run.csx**'in içeriğini [**FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx)'ten alınan içerikle değiştirin.

6. **CMListener** adlı yeni bir **HttpTrigger-CSharp** işlevi oluşturun. Bu işlev Content Moderator'dan olayları alır. Bu işlevi oluşturmak için şu adımları izleyin.

    1. Başvuru amacıyla [Azure İşlevleri Oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasını açık bırakın.
    2. Yeni işlev oluşturmak için **+** ekle'ye tıklayın.
    3. Yerleşik şablonlar yerine, **Kendi/özel işlevinizi başlatma** seçeneğini kullanın.
    4. **HttpTrigger-CSharp** etiketli kutucuğa tıklayın
    5. **CMListener** adını girin. **Yetkilendirme Düzeyi** alanı **İşlev** olarak ayarlanmalıdır.
    6. **Oluştur**’a tıklayın.
    7. **run.csx**'in içeriğini [**CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx)'ten alınan içerikle değiştirin.

## <a name="configure-the-facebook-page-and-app"></a>Facebook sayfasını ve Uygulamasını yapılandırma
1. Facebook Uygulaması oluşturun.

    1. [Facebook geliştirici sitesine](https://developers.facebook.com/) gidin
    2. **My Apps** (Uygulamalarım) öğesine tıklayın.
    3. Yeni Uygulama ekleyin.
    4. **Webhooks -> Get Started** (Web Kancaları -> Başlarken) seçeneğini kullanın
    5. **Page -> Subscribe to this topic** (Sayfa -> Bu konuya abone ol) öğesini seçin
    6. Geri Arama URL'si olarak **FBListener Url'sini** sağlayın ve **İşlevi Uygulaması Ayarları** altında yapılandırdığınız **Belirteci Doğrulayın**
    7. Abone olduktan sonra, akışı aşağı kaydırıp **subscribe** (abone ol) öğesini seçin.

2. Facebook Sayfası oluşturun.

    1. [Facebook](https://www.facebook.com/bookmarks/pages)'a gidin ve **yeni bir Facebook Sayfası** oluşturun.
    2. Şu adımları izleyerek Facebook Uygulamasının bu sayfaya erişmesine izin verin:
        1. [Graph API Explorer](https://developers.facebook.com/tools/explorer/)'a gidin.
        2. **Uygulama**'yı seçin.
        3. **Sayfa Erişim Belirteci**'ni seçin. Bir **Get** isteği gönderin.
        4. Yanıtta **Sayfa Kimliği**'ne tıklayın.
        5. Şimdi **/subscribed_apps** bölümünü URL'ye ekleyin ve bir **Get** (boş yanıt) isteği gönderin.
        6. **Post** isteği gönderin. **success: true** gibi bir yanıt alırsınız.

3. Süresi dolmayan bir Graph API erişim belirteci oluşturun.

    1. [Graph API Explorer](https://developers.facebook.com/tools/explorer/)'a gidin.
    2. **Uygulama** seçeneğini belirtin.
    3. **Kullanıcı Erişim Belirteci Al** seçeneğini belirtin.
    4. **İzin Seç** bölümünde **manage_pages** ve **publish_pages** seçeneklerini belirtin.
    5. **Erişim belirtecini** (Kısa Ömürlü Belirteç) sonraki adımda kullanacağız.

4. Önümüzdeki birkaç adımda Postman kullanıyoruz.

    1. **Postman**'ı açın (veya [buradan](https://www.getpostman.com/) alın).
    2. Şu iki dosyayı içeri aktarın:
        1. [Postman Collection](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman Environment](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Şu ortam değişkenlerini güncelleştirin:
    
    | Anahtar | Değer   | 
    | -------------------- |-------------|
    | appId   | Buraya Facebook Uygulama Tanımlayıcınızı ekleyin  | 
    | appSecret | Buraya Facebook Uygulamanızın gizli dizisini ekleyin | 
    | short_lived_token | Önceki adımda oluşturduğunuz kısa ömürlü kullanıcı erişim belirtecini ekleyin |
    4. Şimdi koleksiyonda listelenen 3 API'yi çalıştırın: 
        1. **Uzun Ömürlü Erişim Belirteci Oluştur**'u seçin ve **Gönder**'e tıklayın.
        2. **Kullanıcı Kimliğini Al**'ı seçin ve **Gönder**'e tıklayın.
        3. **Kalıcı Sayfa Erişim Belirtecini Al**'ı seçin ve **Gönder**'e tıklayın.
    5. Yanıttan **access_token** değerini kopyalayın ve bunu **fb:PageAccessToken** Uygulama ayarına atayın.

İşte bu kadar!

Çözüm, Facebook sayfanıza gönderilen tüm resimleri ve metinleri Content Moderator'a gönderir. Daha önce yapılandırdığınız iş akışları çağrılır. İş akışlarında tanımlanan ölçütlerinizi geçemeyen içerikler sonuçta inceleme aracında incelemeye alınır. İçeriğin kalan bölümü yayımlanır.

## <a name="license"></a>Lisans

Tüm Microsoft Bilişsel Hizmetler SDK'ları ve örnekler, MIT Lisansı ile lisanslanmıştır. Diğer ayrıntılar için bkz. [LİSANS](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları

Bu istemci kitaplığı ve örnek de dahil olmak üzere Bilişsel Hizmetler'i kullanan geliştiricilerin, http://go.microsoft.com/fwlink/?LinkId=698895 adresinde bulunan "Microsoft Bilişsel Hizmetler için Geliştirici Kullanım Kuralları"na uyması beklenir.

## <a name="next-steps"></a>Sonraki adımlar

1. Microsoft Build 2017'den bu çözümün [tanıtımını izleyin (video)](https://channel9.msdn.com/Events/Build/2017/T6033).
1. [GitHub'da Facebook örneği](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
