---
title: LUIS uygulamalar - Azure erişimi anlama | Microsoft Docs
description: LUIS yazma erişmeyi öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: 13b769a0b5a940e0f3dd5f2e0cc3567d9879ee0d
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223788"
---
# <a name="authoring-and-endpoint-user-access"></a>Geliştirme ve uç noktası kullanıcı erişimi
Erişim geliştirme sahipleri ve ortak çalışanlar için kullanılabilir. Özel bir uygulama için erişim uç noktası sahipleri ve ortak çalışanlar için kullanılabilir. Genel bir uygulama için uç nokta kendi LUIS hesabını ve genel uygulama kimliğine sahip herkes için erişilebilir 

## <a name="access-to-authoring"></a>Yazma erişimi
Uygulama erişimi [LUIS](luis-reference-regions.md#luis-website) Web sitesi veya [yazma API'leri](https://aka.ms/luis-authoring-apis) uygulama sahibi tarafından denetlenir. 

Sahibi ve tüm ortak çalışanlar uygulama yazma erişimi vardır. 

|Yazma erişimi içerir|Notlar|
|--|--|
|Uç nokta anahtarları Ekle Kaldır||
|Sürüm dışarı aktarma||
|Uç nokta günlükleri Dışarı Aktar||
|Sürümü içeri aktarılıyor||
|Uygulama genel yap|Uygulama genel olduğunda, herhangi bir yazma ya da uç noktası anahtarı ile uygulama sorgulayabilirsiniz.|
|Modeli Değiştir|
|Yayımlama|
|Gözden geçirme için uç nokta konuşma [etkin öğrenme](luis-how-to-review-endoint-utt.md)|
|Eğitim|

## <a name="access-to-endpoint"></a>Erişim uç noktası
Sorgu LUIS uç noktaya erişim tarafından denetlenir **genel** uygulamanın ayarını **ayarları** sayfası. Özel bir uygulamanın uç nokta sorgu kalan kota isabet sayısı ile yetkili anahtar denetlenir. Bir genel uygulamanın uç nokta sorgu de (kişi sorgu yapıyor) gelen bir uç noktası anahtarı sağlamanız gerekir, ayrıca işaretli kalan kota isabet sayısı. 

Uç noktası anahtarı ya da GET isteğinin sorgu dizesinde geçirilen veya gönderinin üst bilgi isteyin.

![Kümesi uygulamasına genel](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Özel uygulama uç nokta güvenliği
Özel bir uygulamanın uç noktası yalnızca aşağıdakiler için kullanılabilir:

|Anahtar ve kullanıcı|Açıklama|
|--|--|--|
|Sahibin yazma anahtarı| En fazla 1000 uç noktası İsabeti|
|Ortak Çalışanlar yazma anahtarları| En fazla 1000 uç noktası İsabeti|
|Uç nokta anahtarları alanından eklenen **[Yayımla](luis-how-to-publish-app.md)** sayfası|Uç nokta anahtarları sahibi ve ortak çalışanlar ekleyebilirsiniz|

Diğer uç nokta yazma veya anahtarlara sahip **hiçbir** erişim.

### <a name="public-app-endpoint-access"></a>Genel Uygulama uç noktası erişimi
Uygulama olarak yapılandırmanıza **genel** üzerinde **ayarları** uygulamasının sayfası. Genel, bir uygulama yapılandırıldıktan sonra _herhangi_ anahtarı tüm uç nokta kota kullanmamış sürece yazma anahtarı veya LUIS uç noktası anahtarı geçerli LUIS uygulamanızı sorgulayabilir.

Uygulama kimliğini belirtilen bir sahibi veya ortak çalışan, bir kullanıcı bir genel uygulama yalnızca erişim Genel LUIS yoksa _Pazar_ veya diğer yolu genel bir uygulamayı arayın.  

## <a name="microsoft-user-accounts"></a>Microsoft kullanıcı hesapları
Yazarlar ve ortak çalışanlar anahtarları için LUIS Yayımla sayfasında ekleyebilirsiniz. LUIS anahtar oluşturur Azure portalında Microsoft kullanıcı hesabı için uygulama sahibi veya bir uygulama ortak çalışanı gerekir. 

Bkz: [Azure Active Directory Kiracı Kullanıcı](luis-how-to-account-settings.md#azure-active-directory-tenant-user) Active Directory kullanıcı hesapları hakkında daha fazla bilgi edinmek için. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Uç noktanın güvenliğini sağlama 
Bir sunucudan sunucuya ortamında çağırarak LUIS uç nokta anahtarınızı kimler görebilir kontrol edebilirsiniz. LUIS ile bot arasındaki bağlantı zaten bir robotun LUIS kullanıyorsanız, güvenlidir. LUIS uç noktası doğrudan çağırıyorsanız, sunucu tarafı API oluşturmanız gerekir (bir Azure gibi [işlevi](https://azure.microsoft.com/services/functions/)) denetimli erişim (gibi [AAD](https://azure.microsoft.com/services/active-directory/)). Sunucu tarafı API olarak adlandırılır ve kimlik doğrulama ve yetkilendirme doğrulandıktan sonra LUIS açın çağrı geçirin. Bu strateji adam-de-adam saldırılarına engellemez, ancak erişimi izlemenize olanak tanır, kullanıcılarınızın uç noktanızı bilgisayardan farklı gösterir ve uç nokta yanıt günlük eklemenize izin verir (gibi [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Güvenlik uyumluluk
LUIS ile sıfır olmayan-conformities (bulguları) denetim rapordaki ISO 27018:2014 denetim ve ISO 27001: 2013 başarıyla tamamlandı. Ayrıca, LUIS olgunluk özellik değerlendirmesi için en yüksek olası Gold ödül CSA STAR sertifikasına de alınır. Azure yalnızca büyük genel bulut hizmeti sağlayıcısının Bu sertifika almaya hak kazanma ' dir. Daha fazla bilgi için Azure'nın ana güncelleştirilmiş kapsam deyiminde LUIS dahil bulabilirsiniz [uyumluluk genel bakış](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) üzerinde başvurulan belge [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) ISO sayfaları.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [en iyi](luis-concept-best-practices.md) amaç ve varlıkları iyi tahminler elde etmek için kullanmayı öğrenin.