---
title: Microsoft Authentication Library (MSAL) uygulamalarını test etme | Azure
description: Microsoft Authentication Library (MSAL) uygulamalarını test etme hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/28/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3495ad5f691ac57b69ab5d3efcd5436cc66e9a6e
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307531"
---
# <a name="testing-msal-applications"></a>MSAL uygulamalarını test etme
Bu makalede, Microsoft kimlik doğrulama kitaplığı (MSAL) uygulamalarını test etme için öneriler ele alınmaktadır.

## <a name="unit-testing"></a>Birim testi
MSAL API'leri kullanan [Oluşturucu deseni](https://wikipedia.org/wiki/Builder_pattern) yoğun olarak. Oluşturucular zor ve yorucu bir süreç sahte. Bunun yerine, tüm kimlik doğrulama mantığınız arabirimin kaydırma ve, uygulamanızda sahte öneririz.

## <a name="end-to-end-testing"></a>Baştan sona test etme
Baştan sona test etmek için test hesapları ayarlamanıza, uygulamaları test veya dizinleri bile ayırın. Kullanıcı adı ve parola, sürekli tümleştirme işlem hattı (örneğin, Azure DevOps gizli yapı değişkenleri) aracılığıyla dağıtılabilir. Kimlik bilgilerini test et tutmak için başka bir stratejidir [Key Vault](/azure/key-vault/) ve anahtar kasasına erişmek için testleri çalıştıran makine yapılandırma), bir sertifika yükleyerek örneğin. MSAL'ın araştırmalarında [stratejisi, anahtar kasası erişim](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/master/tests/Microsoft.Identity.Test.LabInfrastructure/KeyVaultSecretsProvider.cs#L112).

Belirteç edinme işlemi gerçekleşir. sonra hem bir erişim belirteci ve yenileme belirteci önbelleğe alınır. İlk 1 saat, ikincisi birkaç ay, ömrü vardır. Erişim belirtecinin süresi dolduğunda MSAL kullanıcı etkileşimi olmadan yeni bir tane almak için otomatik olarak yenileme belirtecini kullanır. Testlerinizi sağlamak için bu davranışı güvenebilirsiniz.

Koşullu erişim varsa, etrafında otomatikleştirme zor olacaktır. MSAL önbelleğe belirteçleri ekleyin ve ardından sessiz belirteç edinme üzerinde diğer bir deyişle, güvenin anlaşmalar koşullu erişimle (örneğin, çok faktörlü kimlik doğrulaması), daha önce oturum açmış bir kullanıcı kullanan el ile bir adım daha kolay olacaktır.

## <a name="web-apps"></a>Web uygulamaları
Selenium veya eşdeğer bir teknoloji web uygulaması otomatikleştirmek için kullanın. Kullanıcı adı ve parolayı güvenli bir konumdan getirin.

## <a name="daemon-apps"></a>Daemon uygulamaları
Arka plan programları önceden dağıtılan gizli anahtarları (parola veya sertifika) Azure Active Directory (AAD) konuşmak için kullanın. Gizli dizi, test ortamına dağıtın veya testlerinizi sağlamak için teknik önbelleğe alma belirteci kullanın. [İstemci kimlik bilgileri verme](msal-authentication-flows.md#client-credentials) değil yenileme belirteçleri getirmek, yalnızca bir saat içinde süresi dolacak belirteçleri, erişim.

## <a name="native-client-apps"></a>Yerel istemci uygulamaları
Yerel istemciler için test etmek için birçok yaklaşım vardır:

* Kullanım [kullanıcı adı/parola grant](msal-authentication-flows.md#usernamepassword) etkileşimli olmayan bir şekilde bir belirteç getirilemedi. Bu akış, üretimde önerilmez, ancak test etmek için kullanılacak şüphelenilebilir.

* Appium veya hem uygulamanızı hem de MSAL için Otomasyon arabirimidir Xamarin.Test, tarayıcı oluşturduğunuz gibi bir çerçeve kullanın.

* MSAL, geliştiricilerin kendi tarayıcı deneyimini eklemesine olanak tanıyan bir genişletilebilirlik noktası kullanıma sunar. MSAL takım bu dahili olarak etkileşimli kimlik doğrulaması senaryolarını test etmek için kullanır. Göz atın [bu .NET test projesi](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/blob/master/tests/Microsoft.Identity.Test.Integration.net45/SeleniumTests/InteractiveFlowTests.cs) ekleme yapılacağını görmek için bir [Selenium desteklenen tarayıcı](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/tree/master/tests/Microsoft.Identity.Test.Integration.net45/Infrastructure) kimlik doğrulaması işleyebilir.

## <a name="xamarin-apps"></a>Xamarin uygulamaları
MSAL takım MSAL.NET kullanan bir Xamarin uygulaması testleri çalışmakta olan; Kullanıyoruz [App Center](https://appcenter.ms/apps) cihazları yönetmek için test vb. çalıştırır. Test çerçevesi [Xamarin.UITest](/appcenter/test-cloud/uitest/). Bulduk bir sınırlama biz tarayıcılar katıştırılmış yalnızca sistem tarayıcıları test etmek mümkün olmamasıdır.

Bir test çerçevesi değerlendirirken Appium ve diğer test çerçevelerini yanı sıra, diğer CI/CD sağlayıcıları göz mobil alana sahip ayrıca ile değer istenir.

## <a name="testing-the-token-cache"></a>Belirteç önbelleği test etme
Kullandığınız platformdan bağımsız olarak, MSAL belirteçlerini belirteci bir önbellekte depolar. Bazı platformlarda, bu önbelleğin serileştirmek nasıl MSAL söyleyin. Mobil platformlarda MSAL önbellek sizin için serileştirir. Bir uygulama açısından bakıldığında, belirteç önbelleği için üç şeyi sorumludur:

* sonra satın alınan belirteçleri önbellekte depolamak (örneğin, kullanarak `AcquireTokenInteractive` yöntemi)
* belirteçleri önbellekten çağırırken getirilirken `AcquireTokenSilent` yöntemi
* Hesap meta verileri önbellekten çağırırken getirilirken `GetAccount` yöntemi

Önbellek senaryolarını test etmek isterseniz, bu nedenle olduğu bir senaryoda yazma göz önünde bulundurun:

* bir veya daha fazla belirteçlerini almak (örneğin, kullanarak [kullanıcı adı/parola verin](msal-authentication-flows.md#usernamepassword) test etmek için en basit olan akış)
* doğrulayın `GetAccounts` çalışır
* doğrulayın `AcquireTokenSilent` çalışır

Ayrıca, test isteyebilirsiniz:

* uygulamayı yeniden başlatmayı önbellek kaldırmaz
* `AcquireTokenSilent` yenileme belirteci yenileme değil (diğer bir deyişle, AAD çağrısına ağ), ancak erişim belirtecinin değil dolmuşsa işlevi görür. HTTP istemci denetimini üstlenerek tarafından bu ve diğer HTTP ilgili senaryoları elde edebilirsiniz.  Bir .NET örneği için bkz. [kendi HttpClient sağlama](msal-net-provide-httpclient.md)

## <a name="feedback"></a>Geri Bildirim
Yapabilecekleriniz [oturum sorunları](developer-support-help-options.md#create-a-github-issue) veya test için ilgili sorular sorun. İyi bir deneyim ekibin hedeflerini biridir.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama hakkında bilgi edinmek [günlüğü](msal-logging.md) MSAL uygulamanızdaki.