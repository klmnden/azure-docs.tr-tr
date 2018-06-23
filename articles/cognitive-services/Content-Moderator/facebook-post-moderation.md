---
title: Azure içerik Denetleyici ile içerik yönetimini Facebook | Microsoft Docs
description: Machine learning ile orta Facebook sayfaları içerik yönetici tabanlı
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 316c7212c30e9fe1151cdf5ceabf875439ad4c65
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351868"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>İçerik Denetleyici ile Facebook içeriği denetleme

Bu öğreticide, Orta Facebook gönderileri ve açıklamalar yardımcı olması için makine öğrenme tabanlı içerik denetleyici kullanmayı öğrenin.

Öğretici, bu adımları uygularken size yol gösterir:

1. İçerik denetleyici ekibi oluşturun.
2. İçerik denetleyici ve Facebook HTTP olayları dinler Azure işlevleri oluşturun.
3. Bir Facebook sayfa ve uygulama oluşturun ve içerik denetleyiciye bağlayın.

Biz tamamladıktan sonra Facebook ziyaretçileri içerik aracı tarafından gönderilen içerik gönderir. Eşleşme eşiklere dayanarak, içerik denetleyici akışlarınızı içeriği yayımlamak ya da gözden geçirme Aracı'nı içinde incelemeler oluşturun. 

Aşağıdaki şekilde çözümün yapı taşları gösterilmektedir.

![Facebook gönderilerini denetleme](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>İçerik denetleyici takım oluşturma

Başvurmak [Hızlı Başlangıç](quick-start.md) içerik denetleyici için kaydolun ve bir ekip oluşturmak için sayfa.

## <a name="configure-image-moderation-workflow-threshold"></a>Görüntü Yönetimi iş akışı (eşik) yapılandırma

Başvurmak [iş akışları](review-tool-user-guide/workflows.md) sayfasını bir özel görüntü iş akışı (eşik) yapılandırmak için. İş akışı Not **adı**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Metin Yönetimi iş akışı (eşik) yapılandırma

Benzer adımları kullanmak [iş akışları](review-tool-user-guide/workflows.md) özel metin eşik ve iş akışı yapılandırma sayfası. İş akışı Not **adı**.

![Metin iş akışını yapılandırmak](images/text-workflow-configure.PNG)

İş akışınızı "İş akışı yürütme" düğmesini kullanarak test edin.

![Test metin iş akışı](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure işlevleri oluşturma

Oturum [Azure Yönetim Portalı](https://portal.azure.com/) Azure işlevlerinizi oluşturmak için. Şu adımları uygulayın:

1. Bir Azure işlevi uygulaması üzerinde gösterildiği gibi oluşturun [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfası.
2. Yeni oluşturulan işlevi uygulamasını açın.
3. Uygulama içinde gitmek **Platform özellikleri uygulama ayarları ->**
4. Aşağıdaki tanımlamak [uygulama ayarları](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> **Cm: Bölge** (olmadan boşluk) bölge adı olması gerekir.
> Örneğin, **westeurope**, değil Batı Avrupa **westcentralus**, değil Batı Orta ABD ve benzeri.
>

| Uygulama ayarı | Açıklama   | 
| -------------------- |-------------|
| cm:TeamId   | İçerik denetleyici Sistem kullanıcısı kimliği  | 
| cm:SubscriptionKey | İçerik denetleyici abonelik anahtarınızı - bakın [kimlik bilgileri](/review-tool-user-guide/credentials.md) | 
| cm:Region | Boşluk içermeyen, içerik denetleyici bölge adı. Önceki nota bakın. |
| cm:ImageWorkflow | Görüntülerde çalıştırmak için iş akışının adı |
| cm:TextWorkflow | Metin üzerinde çalıştırmak için iş akışının adı |
| cm:CallbackEndpoint | Bu kılavuzda oluşturduğunuz CMListener işlev uygulaması URL'si |
| FB:VerificationToken | Olaylar için Facebook abone olmak için de kullanılan gizli belirteci akışı |
| FB:PageAccessToken | Facebook grafik API'si erişim belirteci dolmayan ve sizin adınıza Gizle/silme gönderileri işlevi sağlar. |

5. Yeni bir **HttpTrigger CSharp** adlı işlev **FBListener**. Bu işlev, Facebook olayları alır. Bu işlev, aşağıdaki adımları izleyerek oluşturun:

    1. Tutmak [Azure işlevleri oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfa başvurusunu açın.
    2. Tıklatın **+** yeni işlev oluşturmak için ekleyin.
    3. Yerleşik şablonlar yerine seçin **kendi özel işlevinizi başlamak** seçeneği.
    4. Bildiren kutucuğa tıklayın **HttpTrigger CSharp**.
    5. Bir ad girin **FBListener**. **Yetki düzeyini** alan ayarlanmalıdır **işlevi**.
    6. **Oluştur**’a tıklayın.
    7. Değiştir **run.csx** içeriğiyle [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Yeni bir **HttpTrigger CSharp** adlı işlev **CMListener**. Bu işlev, Facebook olayları alır. Bu işlev oluşturmak için aşağıdaki adımları izleyin.

    1. Tutmak [Azure işlevleri oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfa başvurusunu açın.
    2. Tıklatın **+** yeni işlev oluşturmak için ekleyin.
    3. Yerleşik şablonlar yerine seçin **kendi özel işlevinizi başlamak** seçeneği.
    4. Bildiren kutucuğa tıklayın **HttpTrigger CSharp**
    5. Bir ad girin **CMListener**. **Yetki düzeyini** alan ayarlanmalıdır **işlevi**.
    6. **Oluştur**’a tıklayın.
    7. Değiştir **run.csx** içeriğiyle [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Facebook sayfası ve uygulama yapılandırma
1. Bir Facebook uygulaması oluşturursunuz.

    1. Gidin [Facebook Geliştirici sitesi](https://developers.facebook.com/)
    2. Tıklayın **uygulamalarım**.
    3. Yeni bir uygulama ekleyin.
    4. Seçin **Web Kancalarını Get -> başlatıldı**
    5. Seçin **sayfa bu konuya abone ol ->**
    6. Sağlamak **FBListener Url** geri çağırma URL'si olarak ve **doğrulama belirteci** , altında yapılandırılmış **işlev uygulama ayarları**
    7. Abone sonra akış ve select aşağıya doğru kaydırma **abone**.

2. Bir Facebook sayfası oluşturun.

    1. Gidin [Facebook](https://www.facebook.com/bookmarks/pages) ve oluşturma bir **yeni Facebook sayfa**.
    2. Aşağıdaki adımları izleyerek bu sayfaya erişmek Facebook uygulaması izin ver:
        1. Gidin [grafik API'si Gezgini](https://developers.facebook.com/tools/explorer/).
        2. Seçin **uygulama**.
        3. Seçin **sayfa erişim belirteci**, gönderme bir **almak** isteği.
        4. Tıklatın **sayfa kimliği** yanıt.
        5. Şimdi append **/subscribed_apps** gönderme ve URL için bir **almak** (boş yanıt) istek.
        6. Gönderme bir **Post** isteği. Yanıt olarak alma **başarı: true**.

3. Süresiz grafik API'sine erişim belirteci oluşturun.

    1. Gidin [grafik API'si Gezgini](https://developers.facebook.com/tools/explorer/).
    2. Seçin **uygulama** seçeneği.
    3. Seçin **kullanıcı erişim belirteci almak** seçeneği.
    4. Altında **Select izinleri**seçin **manage_pages** ve **publish_pages** seçenekleri.
    5. Kullanacağız **erişim belirteci** (kısa ömürlü belirteç) bir sonraki adımda.

4. Postman sonraki birkaç adımda için kullanırız.

    1. Açık **Postman** (veya buradan edinin [burada](https://www.getpostman.com/)).
    2. Bu iki dosyayı içeri aktarın:
        1. [Postman koleksiyonu](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman ortamı](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Bu ortam değişkenleri güncelleştirin:
    
    | Anahtar | Değer   | 
    | -------------------- |-------------|
    | AppID   | Facebook uygulama tanımlayıcısını Ekle  | 
    | appSecret | Burada Facebook uygulama gizli anahtarı Ekle | 
    | short_lived_token | Önceki adımda oluşturulan kısa süreli kullanıcı erişim belirteci Ekle |
    4. 3 Şimdi Çalıştır API'leri koleksiyonda listelenen: 
        1. Seçin **Long-Lived erişim belirteci oluşturmak** tıklatıp **Gönder**.
        2. Seçin **kullanıcı kimliği Al** tıklatıp **Gönder**.
        3. Seçin **alma kalıcı sayfa erişim belirteci** tıklatıp **Gönder**.
    5. Kopya **access_token** değer yanıttan ve uygulama ayarının atayın **fb:PageAccessToken**.

İşte bu kadar!

Çözüm, tüm görüntüler ve metinler Facebook sayfanızda içerik denetleyiciye gönderilen gönderir. Daha önce yapılandırılmış iş akışları çağrılır. İncelemeler gözden geçirme Aracı'nı içinde iş akışları sonuçlarında tanımlanan ölçütlerinizi iletmez içeriği. İçerik rest yayımlanan.

## <a name="license"></a>Lisans

Tüm Microsoft Bilişsel Services SDK'ları ve örnekleri MIT lisansı ile lisanslanır. Daha fazla ayrıntı için bkz: [lisans](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları

Bilişsel hizmetler, istemci Kitaplığı & örnek, bu da dahil olmak üzere kullanan geliştiriciler bekleniyor "Developer kod, yürütmek için Microsoft Bilişsel Hizmetler" izleyin, bulunan http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Sonraki adımlar

1. [(Video) gösterisini izleyin](https://channel9.msdn.com/Events/Build/2017/T6033) Microsoft yapı 2017'den bu çözümün.
1. [Github'da Facebook örnek](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
