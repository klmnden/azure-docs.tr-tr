---
title: HALUK uygulamalara - Azure erişim anlama | Microsoft Docs
description: Yazma HALUK erişim öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 44380e12e6d095e8d40675af0b6b2fddc5e4c4e9
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264276"
---
# <a name="authoring-and-endpoint-user-access"></a>Kullanıcı erişim yazma ve uç noktası
Erişim yazma sahipleri ve ortak çalışanlar için kullanılabilir. Özel bir uygulama için uç nokta sahipleri ve ortak çalışanlar için erişilebilir. Ortak bir uygulama için uç nokta erişim kendi HALUK hesabını ve ortak uygulamanın kimliğine sahip herkes için kullanılabilir 

## <a name="access-to-authoring"></a>Yazma erişimi
Uygulama erişimi [HALUK] [ LUIS] Web sitesi veya [API'lerini geliştirme](https://aka.ms/luis-authoring-apis) uygulama sahibi tarafından denetlenir. 

Sahibi ve tüm ortak uygulama yazmak için erişime sahiptir. 

|Erişim yazma içerir|Notlar|
|--|--|
|Uç nokta anahtarları Ekle Kaldır||
|Sürüm dışarı aktarma||
|Uç nokta günlükleri dışarı aktarma||
|Sürüm alma||
|Uygulama genel yap|Bir uygulama genel olduğunda, yazma veya uç nokta anahtar kimseyle uygulama sorgulayabilirsiniz.|
|Modeli Değiştir|
|Yayımlama|
|Gözden geçirmek için uç nokta utterances [etkin öğrenme](label-suggested-utterances.md)|
|Eğitin|

## <a name="access-to-endpoint"></a>Uç nokta erişimi
Sorgu HALUK uç noktaya erişim tarafından denetlenir **ortak** uygulama ayarını **ayarları** sayfası. Özel bir uygulamanın uç nokta sorgu kota isabet kalan sahip bir yetkili anahtarı için denetlenir. Ayrıca (Başlangıç aktaranın sorgu yapıyor) bir uç nokta anahtar sağlamak ortak bir uygulamanın uç nokta sorgu sahip olduğu da işaretli kota isabet kalan için. 

Uç noktası anahtarı ya da GET isteğini querystring geçirilen veya posta üstbilgisinin isteyin.

![Ortak kümesi uygulamasına](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Özel uygulama uç noktası güvenlik
Özel bir uygulamanın uç nokta yalnızca aşağıdakiler için kullanılabilir:

|Anahtar ve kullanıcı|Açıklama|
|--|--|--|
|Sahibin geliştirme anahtarı| En fazla 1000 uç noktası isabet|
|Ortak Çalışanlar geliştirme anahtarları| En fazla 1000 uç noktası isabet|
|Uç nokta anahtarları konsolundan **[Yayımla](publishapp.md)** sayfası|Sahibi ve ortak uç nokta anahtarları ekleyebilirsiniz|

Diğer yazma veya uç nokta anahtarlara sahip **hiçbir** erişim.

### <a name="public-app-endpoint-access"></a>Genel Uygulama uç noktası erişimi
Uygulama olarak yapılandırmanıza **ortak** üzerinde **ayarları** uygulamanın sayfası. Genel olarak, bir uygulama yapılandırıldıktan sonra _herhangi_ anahtar veya HALUK uç noktası anahtarı yazma geçerli HALUK anahtarı tüm uç nokta kota kullanmamış sürece, uygulamanızın sorgulayabilir.

Bir uygulama kimliği verildiyse sahibi veya ortak çalışanı, olmayan bir kullanıcı bir genel uygulama yalnızca erişim HALUK, ortak bir yoksa _Pazar_ veya başka bir şekilde ortak bir uygulamayı arayın.  

## <a name="microsoft-user-accounts"></a>Microsoft kullanıcı hesapları
Yazarlar ve ortak anahtarları için HALUK Yayımla sayfasında ekleyebilirsiniz. Azure portalında HALUK anahtarı oluşturur Microsoft kullanıcı hesabının uygulama sahibi ya da bir uygulama ortak çalışanı gerekir. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Uç nokta güvenliğini sağlama 
Sunucu sunuculu bir ortamda çağırarak kimlerin HALUK uç nokta anahtarınızı görebileceğini kontrol edebilirsiniz. Bir bot HALUK kullanıyorsanız, bot ve HALUK arasındaki bağlantının güvenli zaten var. HALUK uç noktası doğrudan arıyorsanız, bir sunucu tarafı API oluşturmanız gerekir (bir Azure gibi [işlevi](https://azure.microsoft.com/services/functions/)) denetimli erişim (gibi [AAD](https://azure.microsoft.com/services/active-directory/)). Sunucu tarafı API adı verilir ve kimlik doğrulama ve yetkilendirme doğrulandı zaman HALUK açın çağrı başarılı. Bu strateji man-in--middle saldırıları engellemez, ancak erişim izlemenize olanak tanır, kullanıcılarınızın uç noktanızı bilgisayardan farklı gösterir ve uç nokta yanıt günlük eklemenize izin verir (gibi [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Güvenlik uyumluluk
HALUK, ISO 27001: 2013 ve ISO 27018:2014 denetim sıfır olmayan-conformities (bulgularını) denetim rapordaki ile başarıyla tamamlandı. Ayrıca, HALUK olgunluk yetenek değerlendirmesi için en yüksek olası altın ödül ile CSA yıldız sertifika elde. Azure Bu sertifika kazanmak için yalnızca ana genel bulut hizmeti sağlayıcısıdır. Daha fazla ayrıntı için Azure'nın ana güncelleştirilmiş kapsam deyiminde HALUK dahil bulabilirsiniz [uyumluluk genel bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) üzerinde başvurulan belge [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) ISO sayfaları.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [en iyi yöntemler](luis-concept-best-practices.md) hedefleri ve varlıklar için en iyi tahminleri nasıl kullanılacağını öğrenin.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website