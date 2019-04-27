---
title: 'Öğretici: Facebook içerik - Content Moderator Orta'
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, Orta Facebook gönderilerinizi ve açıklamalar yardımcı olması için makine öğrenme tabanlı Content Moderator kullanmayı öğreneceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 01/18/2019
ms.author: pafarley
ms.openlocfilehash: 662eca2a727f3112f169ab8d669bf18c81700275
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699587"
---
# <a name="tutorial-moderate-facebook-posts-and-commands-with-azure-content-moderator"></a>Öğretici: Orta Facebook gönderilerinizi ve Azure Content Moderator ile komutları

Bu öğreticide, Azure Content Moderator bir Facebook sayfasında yorumları ve gönderileri Orta yardımcı olmak için nasıl kullanılacağını öğreneceksiniz. Facebook Content Moderator hizmet yayımlanacağını ve Ziyaretçiler tarafından gönderilen içerik gönderir. Ardından Content Moderator iş akışlarınızı içerik yayımlama veya içerik puanları ve eşiklere göre gözden geçirme aracı içinde gözden geçirmeleri oluşturabilirsiniz. Bkz: [Build 2017 tanıtım videosu](https://channel9.msdn.com/Events/Build/2017/T6033) bu senaryonun çalışan bir örnek için.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Content Moderator takımı oluşturma.
> * Content Moderator'dan ve Facebook'tan HTTP olaylarını dinleyen Azure İşlevleri oluşturma.
> * Bir Facebook sayfasında bir Facebook uygulaması kullanarak Content Moderator bağlayın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

Bu diyagramda bu senaryonun her bileşenin gösterilir:

![Content Moderator facebook'taki "FBListener" üzerinden bilgi almak ve "CMListener" üzerinden bilgi gönderme diyagramı](images/tutorial-facebook-moderation.png)

## <a name="prerequisites"></a>Önkoşullar

- Content Moderator abonelik anahtarı. Bölümündeki yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Content Moderator hizmete abone olmak ve anahtarınızı alın.
- A [Facebook hesabıyla](https://www.facebook.com/).

## <a name="create-a-review-team"></a>Bir gözden geçirme takım oluştur

Başvurmak [deneyin Content Moderator Web'de](quick-start.md) kaydolmak yönergeler için Hızlı Başlangıç [Content Moderator gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com/) ve bir gözden geçirme ekibi oluşturun. Not **Takım Kimliği** değerini **kimlik bilgilerini** sayfası.

## <a name="configure-image-moderation-workflow"></a>Görüntü denetimi iş akışını yapılandırın

Başvurmak [tanımlayın ve test kullanım iş akışları](review-tool-user-guide/workflows.md) özel görüntü iş akışı oluşturma kılavuzu. Bu, otomatik olarak Facebook görüntülerinde denetleyip bazıları gözden geçirme aracı olarak göndermek Content Moderator olanak tanır. İş akışı Not **adı**.

## <a name="configure-text-moderation-workflow"></a>Metin denetimi iş akışını yapılandırın

Yeniden bakın [tanımlayın ve test kullanım iş akışları](review-tool-user-guide/workflows.md) Kılavuzu; bu kez, bir özel metin iş akışı oluşturun. Bu, Content Moderator'ı metin içeriği otomatik olarak denetlemek izin verir. İş akışı Not **adı**.

![Metin İş Akışını Yapılandırma](images/text-workflow-configure.PNG)

İş akışı kullanarak test **iş akışı yürütme** düğmesi.

![Metin İş Akışını Test Etme](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure İşlevleri oluşturma

Oturum [Azure portalı](https://portal.azure.com/) ve aşağıdaki adımları izleyin:

1. [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal) sayfasında gösterildiği gibi bir Azure İşlev Uygulaması oluşturun.
2. Yeni oluşturulan işlev uygulamasına gidin.
3. Uygulamanın içinde Git **Platform özellikleri** sekmenize **uygulama ayarları**. İçinde **uygulama ayarları** bölümü, bir sonraki sayfada, listenin sonuna kaydırın ve **yeni ayar Ekle**. Aşağıdaki anahtar/değer çifti Ekle
    
    | Uygulama ayarı adı | value   | 
    | -------------------- |-------------|
    | cm:TeamId   | Content Moderator Takım Kimliğiniz  | 
    | cm:SubscriptionKey | Content Moderator abonelik anahtarınız. Bkz. [Kimlik Bilgileri](review-tool-user-guide/credentials.md) | 
    | cm:Region | Content Moderator bölge adınız (boşluk içermez). Önceki nota bakın. |
    | cm:ImageWorkflow | Görüntüler üzerinde çalıştırılacak iş akışının adı |
    | cm:TextWorkflow | Metinler üzerinde çalıştırılacak iş akışının adı |
    | cm:CallbackEndpoint | Bu kılavuzda daha sonra oluşturacağınız CMListener İşlev Uygulamasının Url'si |
    | fb:VerificationToken | Facebook akış olaylarına abone olmak için de kullanılan gizli dizi belirteci |
    | fb:PageAccessToken | Facebook graph api'si erişim belirtecinin süresi dolmaz ve işlevin sizin adınıza gönderileri Gizlemesini/Silmesini sağlar. |

    Tıklayın **Kaydet** sayfanın üstünde düğme.

1. Kullanarak **+** yeni işlev bölmesini açmak için sol bölmedeki düğmesi.

    ![İşlev Ekle düğmesi ile Azure işlevleri bölmesi.](images/new-function.png)

    Ardından **+ yeni işlev** sayfanın üstünde. Bu işlev Facebook'tan olayları alır. Bu işlevi oluşturmak için aşağıdaki adımları izleyin:

    1. İfadesini içeren kutucuğa tıklayın **Http tetikleyicisi**.
    1. **FBListener** adını girin. **Yetkilendirme Düzeyi** alanı **İşlev** olarak ayarlanmalıdır.
    1. **Oluştur**’a tıklayın.
    1. Öğesinin içeriğini değiştirin **run.csx** içeriğiyle **FbListener/run.csx**

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/FbListener/run.csx?range=1-160)]

1. Yeni bir **Http tetikleyicisi** adlı işlev **CMListener**. Bu işlev Content Moderator'dan olayları alır. Öğesinin içeriğini değiştirin **run.csx** içeriğiyle **CMListener/run.csx**

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/CmListener/run.csx?range=1-106)]

---

## <a name="configure-the-facebook-page-and-app"></a>Facebook sayfasını ve Uygulamasını yapılandırma
1. Facebook Uygulaması oluşturun.

    ![facebook Geliştirici sayfası](images/facebook-developer-app.png)

    1. [Facebook geliştirici sitesine](https://developers.facebook.com/) gidin
    2. **My Apps** (Uygulamalarım) öğesine tıklayın.
    3. Yeni Uygulama ekleyin.
    1. bir ad
    1. Seçin **Web kancaları -> kümesi ayarlama**
    1. Seçin **sayfa** açılır menüsünü seçip **bu nesne için abone olun**
    1. Geri Arama URL'si olarak **FBListener Url'sini** sağlayın ve **İşlevi Uygulaması Ayarları** altında yapılandırdığınız **Belirteci Doğrulayın**
    1. Abone olduktan sonra, akışı aşağı kaydırıp **subscribe** (abone ol) öğesini seçin.

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

Çözüm, Facebook sayfanıza gönderilen tüm resimleri ve metinleri Content Moderator'a gönderir. Ardından, daha önce yapılandırdığınız iş akışları çağrılır. İş akışlarında tanımlanan ölçütlerinizle geçirmez içerik İnceleme aracını içinde incelemelere geçirilir. İçeriği geri kalanını otomatik olarak yayımlanan.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, ürün görüntüleri bunları ürün türüne göre etiketleme ve bir gözden geçirme takım, içerik denetleme hakkında bilinçli kararlar verme amacıyla analiz etmek için program ayarlama. Ardından, görüntü denetimi ile ilgili ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Görüntü denetimi](./image-moderation-api.md)
