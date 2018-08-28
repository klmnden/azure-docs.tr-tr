---
title: Azure Content Moderator ile Facebook içerik denetleme | Microsoft Docs
description: Content Moderator Orta Facebook sayfaları ile makine öğrenimi tabanlı
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/18/2017
ms.author: sajagtap
ms.openlocfilehash: 66caea65c21bb1f8bb6efa9b50c917599bb71e2f
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43093986"
---
# <a name="facebook-content-moderation-with-content-moderator"></a>Content Moderator ile Facebook içerik denetleme

Bu öğreticide, biz Orta Facebook gönderilerinizi ve açıklamalar yardımcı olması için makine öğrenme tabanlı Content Moderator kullanmayı öğrenin.

Öğreticinin Bu adımlarda size yol gösterir:

1. Content Moderator takım oluşturun.
2. Content Moderator ve Facebook HTTP olayları dinler Azure işlevleri oluşturun.
3. Bir Facebook sayfasında ve uygulama oluşturun ve Content Moderator için bağlanın.

Biz tamamladıktan sonra Facebook Content Moderator yayımlanacağını ve Ziyaretçiler tarafından gönderilen içerik gönderir. Eşleşme eşiklere dayanarak, Content Moderator akışlarınızı içerik yayımlama veya gözden geçirme aracı içinde gözden geçirmeleri oluşturabilirsiniz. 

Çözüm yapı taşları aşağıdaki şekilde gösterilmiştir.

![Facebook gönderilerini denetleme](images/tutorial-facebook-moderation.png)

## <a name="create-a-content-moderator-team"></a>Content Moderator takım oluştur

Başvurmak [hızlı](quick-start.md) Content Moderator için kaydolun ve bir takım oluşturmak için sayfa.

## <a name="configure-image-moderation-workflow-threshold"></a>Görüntü denetimi iş akışı (eşik) yapılandırma

Başvurmak [iş akışları](review-tool-user-guide/workflows.md) özel görüntü (eşik) iş akışı yapılandırma sayfası. İş akışı Not **adı**.

## <a name="3-configure-text-moderation-workflow-threshold"></a>3. Metin denetimi iş akışı (eşik) yapılandırma

Benzer adımları [iş akışları](review-tool-user-guide/workflows.md) bir özel metin eşik ve iş akışı yapılandırma sayfası. İş akışı Not **adı**.

![Metin iş akışını yapılandırın](images/text-workflow-configure.PNG)

İş akışınızı "İş akışı yürütme" düğmesini kullanarak test edin.

![Metin iş akışını test etme](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure işlevleri oluşturun

Oturum [Azure Yönetim Portalı](https://portal.azure.com/) , Azure işlevleri. Şu adımları uygulayın:

1. Gösterilen şekilde bir Azure işlev uygulaması oluşturma [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfası.
2. Yeni oluşturulan işlev uygulaması'nı açın.
3. Uygulamanın içinde gidin **Platform özellikleri -> Uygulama ayarları**
4. Aşağıdakileri tanımlayın [uygulama ayarları](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#settings):

> [!NOTE]
> **Cm: Bölge** (tüm boşluksuz) bölge adı olmalıdır.
> Örneğin, **westeurope**, Batı Avrupa değil, **westcentralus**, değil Batı Orta ABD ve benzeri.
>

| Uygulama ayarı | Açıklama   | 
| -------------------- |-------------|
| cm:TeamId   | Content Moderator teamıd değeri  | 
| cm:SubscriptionKey | Content Moderator abonelik anahtarınızı - bakın [kimlik bilgileri](review-tool-user-guide/credentials.md) | 
| cm:Region | Content Moderator bölgesi adınız boşluk olmadan. Önceki nota bakın. |
| cm:ImageWorkflow | İş akışının görüntülerde çalışmasını adı |
| cm:TextWorkflow | Metin üzerinde çalıştırmak için iş akışının adı |
| cm:CallbackEndpoint | Bu kılavuzda daha sonra oluşturduğunuz CMListener işlev uygulaması için URL |
| FB:VerificationToken | Olaylar için Facebook abone olmak için de kullanılan gizli belirteç akışı |
| FB:PageAccessToken | Facebook graph API'si erişim belirteci dolmaz ve işlev Gizle/Delete gönderileri sizin adınıza sağlar. |

5. Yeni bir **HttpTrigger-CSharp** adlı işlev **FBListener**. Bu işlev, Facebook'tan olaylarını alır. Bu işlev, aşağıdaki adımları izleyerek oluşturun:

    1. Tutun [Azure işlevleri oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasını başvurusunu açık.
    2. Tıklayın **+** yeni işlev oluşturmak için ekleyin.
    3. Yerleşik şablonlar yerine seçin **kendi özel işlevinizi başlama** seçeneği.
    4. İfadesini içeren kutucuğa tıklayın **HttpTrigger-CSharp**.
    5. Bir ad girin **FBListener**. **Yetkilendirme düzeyi** alan ayarlanmalıdır **işlevi**.
    6. **Oluştur**’a tıklayın.
    7. Öğesinin içeriğini değiştirin **run.csx** içeriğiyle [ **FbListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FbListener/run.csx).

6. Yeni bir **HttpTrigger-CSharp** adlı işlev **CMListener**. Bu işlev, Content Moderator olayları alır. Bu işlev oluşturmak için aşağıdaki adımları izleyin.

    1. Tutun [Azure işlevleri oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasını başvurusunu açık.
    2. Tıklayın **+** yeni işlev oluşturmak için ekleyin.
    3. Yerleşik şablonlar yerine seçin **kendi özel işlevinizi başlama** seçeneği.
    4. İfadesini içeren kutucuğa tıklayın **HttpTrigger-CSharp**
    5. Bir ad girin **CMListener**. **Yetkilendirme düzeyi** alan ayarlanmalıdır **işlevi**.
    6. **Oluştur**’a tıklayın.
    7. Öğesinin içeriğini değiştirin **run.csx** içeriğiyle [ **CMListener/run.csx**](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/CmListener/run.csx).

## <a name="configure-the-facebook-page-and-app"></a>Facebook sayfası ve uygulama yapılandırma
1. Facebook uygulaması oluşturun.

    1. Gidin [Facebook Geliştirici sitesi](https://developers.facebook.com/)
    2. Tıklayarak **uygulamalarım**.
    3. Yeni bir uygulama ekleyin.
    4. Seçin **Web kancaları, Get -> başlatıldı**
    5. Seçin **sayfasında bu konuya abone ol ->**
    6. Sağlamak **FBListener Url** geri çağırma URL'si olarak ve **doğrulama belirteci** altında yapılandırmış **işlev uygulaması ayarları**
    7. Abone sonra akış ve select aşağı kaydırma **abone**.

2. Bir Facebook sayfası oluşturun.

    1. Gidin [Facebook](https://www.facebook.com/bookmarks/pages) oluşturup bir **yeni Facebook sayfasında**.
    2. Facebook uygulaması, aşağıdaki adımları izleyerek bu sayfaya erişmek izin ver:
        1. Gidin [grafik API'si Gezgini](https://developers.facebook.com/tools/explorer/).
        2. Seçin **uygulama**.
        3. Seçin **sayfası erişim belirteci**, gönderme bir **alma** isteği.
        4. Tıklayın **sayfa kimliği** yanıt.
        5. Artık ekleme **/subscribed_apps** URL'si ve gönderme bir **alma** (boş yanıt) istek.
        6. Gönderme bir **Post** isteği. Yanıt olarak alma **başarılı: true**.

3. Süresiz Graph API'sine erişim belirteci oluşturun.

    1. Gidin [grafik API'si Gezgini](https://developers.facebook.com/tools/explorer/).
    2. Seçin **uygulama** seçeneği.
    3. Seçin **kullanıcı erişim belirteci Al** seçeneği.
    4. Altında **Select izinleri**seçin **manage_pages** ve **publish_pages** seçenekleri.
    5. Kullanacağız **erişim belirteci** (kısa ömürlü belirteci) sonraki adımda.

4. Sonraki birkaç adımı için Postman kullanıyoruz.

    1. Açık **Postman** (veya bunu [burada](https://www.getpostman.com/)).
    2. Bu iki dosyayı içeri aktarın:
        1. [Postman koleksiyonu](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman ortamı](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Bu ortam değişkenleri güncelleştirin:
    
    | Anahtar | Değer   | 
    | -------------------- |-------------|
    | Uygulama Kimliği   | Facebook uygulama tanımlayıcısı ekleme  | 
    | appSecret | Burada Facebook uygulama gizli anahtarı Ekle | 
    | short_lived_token | Önceki adımda oluşturulan kısa ömürlüdür kullanıcı erişim belirteci Ekle |
    4. 3 çalıştırmam API'leri koleksiyonunda listelenir: 
        1. Seçin **Long-Lived erişim belirteci oluşturma** tıklatıp **Gönder**.
        2. Seçin **kullanıcı kimliği Al** tıklatıp **Gönder**.
        3. Seçin **kalıcı sayfası erişim belirteci Al** tıklatıp **Gönder**.
    5. Kopyalama **access_token** değer yanıttan ve uygulama ayarının, Ata **fb:PageAccessToken**.

İşte bu kadar!

Çözüm, tüm görüntüler ve metinler için Content Moderator Facebook sayfanızda gönderilen gönderir. Daha önce yapılandırdığınız iş akışları çağrılır. İş akışları sonuçlarında incelemelerde İnceleme aracını içinde tanımlanan ölçütlerinizle geçirmez içeriği. İçeriği geri kalanını yayımlanan.

## <a name="license"></a>Lisans

MIT lisansı ile birlikte, tüm Microsoft Bilişsel hizmetler SDK'lar ve örnekler lisanslanır. Daha fazla ayrıntı için [lisans](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Geliştirici Kullanım Kuralları

Bu istemci kitaplığı ve örnek, Bilişsel hizmetler kullanan geliştiriciler bekleniyor "Geliştirici kod birini gerçekleştirmek için Microsoft Bilişsel Hizmetler" izleyin, bulunan http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Sonraki adımlar

1. [(Video) tanıtımı izleyin](https://channel9.msdn.com/Events/Build/2017/T6033) Microsoft Build 2017'den bu çözümü.
1. [Github'da Facebook örneği](https://github.com/MicrosoftContentModerator/samples-fbPageModeration)
1. https://docs.microsoft.com/azure/azure-functions/functions-create-github-webhook-triggered-function
2. http://ukimiawz.github.io/facebook/2015/08/12/webhook-facebook-subscriptions/
3. http://stackoverflow.com/questions/17197970/facebook-permanent-page-access-token
