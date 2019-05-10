---
title: İşbirliği, güvenlik
titleSuffix: Language Understanding - Azure Cognitive Services
description: Erişim geliştirme sahipleri ve ortak çalışanlar için kullanılabilir. Özel bir uygulama için erişim uç noktası sahipleri ve ortak çalışanlar için kullanılabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 499854bcf6774c3e4eee350c1dd4a2204885f3b1
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65522487"
---
# <a name="authoring-and-endpoint-user-access"></a>Geliştirme ve uç noktası kullanıcı erişimi
Erişim geliştirme sahipleri ve ortak çalışanlar için kullanılabilir. Özel bir uygulama için erişim uç noktası sahipleri ve ortak çalışanlar için kullanılabilir. Genel bir uygulama için uç nokta kendi LUIS hesabını ve genel uygulama kimliğine sahip herkes için erişilebilir 

## <a name="access-to-authoring"></a>Yazma erişimi
Uygulama erişimi [LUIS](luis-reference-regions.md#luis-website) Web sitesi veya [yazma API'leri](https://go.microsoft.com/fwlink/?linkid=2092087) uygulama sahibi tarafından denetlenir. 

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
|Gözden geçirme için uç nokta konuşma [etkin öğrenme](luis-how-to-review-endpoint-utterances.md)|
|Eğitim|

## <a name="access-to-endpoint"></a>Erişim uç noktası

Erişim uç noktası'nı sorgulamak için bir ayar tarafından denetlenir **uygulama bilgilerini** sayfasını **Yönet** bölümü. 

![Kümesi uygulamasına genel](./media/luis-concept-security/set-application-as-public.png)

|[Özel uç nokta](#private-app-endpoint-security)|[Genel bir uç nokta](#public-app-endpoint-access)|
|:--|:--|
|Sahibi ve ortak çalışanlar için kullanılabilir|Sahip, çalışanlar ve herkes tarafından kullanılabilir değilse, uygulama kimliği bilir|

### <a name="private-app-endpoint-security"></a>Özel uygulama uç nokta güvenliği

Özel bir uygulamanın uç noktası yalnızca aşağıdakiler için kullanılabilir:

|Anahtar ve kullanıcı|Açıklama|
|--|--|
|Sahibin yazma anahtarı| En fazla 1000 uç noktası İsabeti|
|Ortak Çalışanlar yazma anahtarları| En fazla 1000 uç noktası İsabeti|
|LUIS için bir yazar ya da ortak çalışanı tarafından atanan herhangi bir tuşa|Anahtar kullanımı katmanını temel alan|

#### <a name="microsoft-user-accounts"></a>Microsoft kullanıcı hesapları

Yazarlar ve ortak çalışanlar anahtarları için özel bir LUIS uygulaması atayabilirsiniz. LUIS anahtar oluşturur Azure portalında Microsoft kullanıcı hesabının uygulama sahibine ya da bir uygulama ortak çalışanı olması gerekir. Özel bir uygulama için bir anahtar başka bir Azure hesabından diğerine atayamazsınız.

Bkz: [Azure Active Directory Kiracı Kullanıcı](luis-how-to-collaborate.md#azure-active-directory-tenant-user) Active Directory kullanıcı hesapları hakkında daha fazla bilgi edinmek için. 

### <a name="public-app-endpoint-access"></a>Genel Uygulama uç noktası erişimi

Genel, bir uygulama yapılandırıldıktan sonra _herhangi_ anahtarı tüm uç nokta kota kullanmamış sürece yazma anahtarı veya LUIS uç noktası anahtarı geçerli LUIS uygulamanızı sorgulayabilir.

Uygulama kimliğini belirtilen bir sahibi veya ortak çalışan, bir kullanıcı bir genel uygulama yalnızca erişim Genel LUIS yoksa _Pazar_ veya diğer yolu genel bir uygulamayı arayın.  

LUIS kaynak bölge tabanlı anahtarı bir kullanıcıyla uygulama hangi bölge kaynak anahtarla ilişkilendirilen erişebilmesi için tüm bölgelerde genel bir uygulama yayımlanır.

## <a name="microsoft-user-accounts"></a>Microsoft kullanıcı hesapları

Yazarlar ve ortak çalışanlar anahtarları için LUIS Yayımla sayfasında ekleyebilirsiniz. LUIS anahtar oluşturur Azure portalında Microsoft kullanıcı hesabının uygulama sahibine ya da bir uygulama ortak çalışanı olması gerekir. 

Bkz: [Azure Active Directory Kiracı Kullanıcı](luis-how-to-collaborate.md#azure-active-directory-tenant-user) Active Directory kullanıcı hesapları hakkında daha fazla bilgi edinmek için. 

## <a name="securing-the-endpoint"></a>Uç noktanın güvenliğini sağlama 

Bir sunucudan sunucuya ortamında çağırarak LUIS uç nokta anahtarınızı kimler görebilir kontrol edebilirsiniz. LUIS ile bot arasındaki bağlantı zaten bir robotun LUIS kullanıyorsanız, güvenlidir. LUIS uç noktası doğrudan çağırıyorsanız, sunucu tarafı API oluşturmanız gerekir (bir Azure gibi [işlevi](https://azure.microsoft.com/services/functions/)) denetimli erişim (gibi [AAD](https://azure.microsoft.com/services/active-directory/)). Sunucu tarafı API olarak adlandırılır ve kimlik doğrulama ve yetkilendirme doğrulandıktan sonra LUIS açın çağrı geçirin. Bu strateji adam-de-adam saldırılarına engellemez, ancak erişimi izlemenize olanak tanır, kullanıcılarınızın uç noktanızı bilgisayardan farklı gösterir ve uç nokta yanıt günlük eklemenize izin verir (gibi [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Güvenlik uyumluluk
 
[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-security-compliance.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [en iyi](luis-concept-best-practices.md) amaç ve varlıkları iyi tahminler elde etmek için kullanmayı öğrenin.
