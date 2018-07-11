---
title: Azure Active Directory B2C nedir? | Microsoft Docs
description: Azure Active Directory B2C'yi kullanarak uygulamanızın oturum açma deneyimini oluşturmayı ve yönetmeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 04/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6949ab89cf806818783c86199e6df334e263b046
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440890"
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory (Azure AD) B2C, müşterilerin uygulamalarınızı kullanırken nasıl kaydolduğunu, oturum açtığını ve profillerini yönettiğini özelleştirip denetlemenizi sağlayan bir kimlik yönetimi sistemidir. Buna diğerlerinin yanında iOS, Android ve .NET için geliştirilmiş uygulamalar dahildir. Azure AD B2C, aynı anda müşterilerinizin kimliklerini korurken bu eylemleri mümkün kılar.

Azure AD B2C ile kaydedilmiş olan bir uygulamayı farklı kimlik yönetimi eylemlerini gerçekleştirecek şekilde yapılandırabilirsiniz. Bazı örnekler şunlardır:

- Müşterinin kayıtlı uygulamanızı kullanarak kaydolmasını sağlama
- Kaydolan müşterinin oturum açmasını ve uygulamanızı kullanmaya başlamasını sağlama
- Kaydolan müşterinin profilini düzenlemesini sağlama
- Uygulamanızda Multi-Factor Authentication'ı etkinleştirme
- Müşterinin belirli kimlik sağlayıcılarıyla kaydolmasını ve oturum açmasını sağlama
- Uygulamanızdan derlediğiniz API'lere erişim sağlama
- Kaydolma ve oturum açma deneyiminin genel görünümünü özelleştirme
- Uygulamanızın çoklu oturum açma oturumlarını yönetme

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>Azure AD B2C'yi kullanmadan önce dikkat etmem gereken noktalar nelerdir?

- Müşterinin uygulamamla nasıl etkileşim kurmasını istiyorum?
- Müşterilere sağlamak istediğim kullanıcı arabirimi (UI) deneyimi nedir?
- Müşterilerin uygulamamda hangi kimlik sağlayıcıları seçmesine izin vermek istiyorum?
- Oturum açma işlemim ek API'lerin çalıştırılmasını gerektiriyor mu?

### <a name="customer-interaction"></a>Müşteri etkileşimi

Azure AD B2C, tüm müşteri deneyimleri için [OpenID Connect](https://openid.net/connect/) desteği sunar. OpenID Connect'in Azure AD B2C'ye uygulanması sırasında uygulamanız Azure AD B2C'ye kimlik doğrulaması istekleri sağlayarak kullanıcı yolculuğunu başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci, müşterinin kimliğini temsil eder.

Azure AD B2C'yi kullanan tüm uygulamaların Azure portal aracılığıyla Azure AD B2C kiracısına kaydedilmesi gerekir. Kayıt işlemi değerler toplar ve bunları uygulamanıza atar. Bu değerler uygulamayı tanımlayan benzersiz uygulama kimliği ve yanıtları yönlendirmek için kullanılabilen yönlendirme URI'sini içerir.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, müşteriyi bir ilkeyi çalıştırmak üzere yönlendirir.
2. Müşteri, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama, güvenlik belirtecini alır.
4. Uygulama, korunan kaynağa erişmek için güvenlik belirtecini kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama, güvenlik belirtecini düzenli aralıklarla yeniler.

Bu adımlar, oluşturduğunuz uygulama türüne göre biraz değişebilir.

Azure AD B2C bir kimlik görevini tamamlamak için kimlik sağlayıcılar, müşteriler, diğer sistemler ve yerel dizinle etkileşim kurar. Buna müşteri oturumunu açma, yeni müşteri kaydetme veya parola sıfırlama gibi görevler dahildir. Çok taraflı güveni kuran ve bu adımları tamamlayan temel platform Kimlik Deneyimi Çerçevesi olarak adlandırılır. Bu çerçeve ve bir ilke (kullanıcı yolculuğu veya Güven Altyapısı ilkesi olarak da adlandırılır) aktörleri, eylemleri, protokolleri ve tamamlanacak adımların sırasını açıkça tanımlar.

Azure AD B2C, uygulamalarınızı hizmet reddi ve parola saldırılarından korumak için birçok yönteme sahiptir. Azure AD B2C, kaynakları hizmet reddi saldırılarına karşı korumak için SYN tanımlama bilgileri gibi algılama ve risk azaltma tekniklerinin yanı sıra hız ve bağlantı sınırları kullanır. Risk azaltma özelliği ayrıca parolalara yönelik deneme yanılma saldırısı ve sözlük saldırıları için de sunulur.

#### <a name="built-in-policies"></a>Yerleşik ilkeler

Azure AD B2C'ye gönderilen her istek bir ilke belirtir. Uygulamanızın Azure AD B2C ile etkileşim kurma şekli bir ilke tarafından denetlenir. Kaydolma, oturum açma ve profil düzenleme gibi en çok gerçekleştirilen kimlik görevleri için önceden tanımlı yerleşik ilkeler bulunur.  Örneğin bir kaydolma ilkesi aşağıdaki ayarları yapılandırarak davranışları denetlemenizi sağlar:

- Müşterinin uygulamaya kaydolmak için kullanabileceği sosyal hesaplar
- Ad veya posta kodu gibi müşteriden alınan veriler
- Multi-factor authentication
- Tüm kaydolma sayfalarının genel görünümü
- Uygulamaya döndürülen bilgiler

#### <a name="custom-policies"></a>Özel ilkeler 

[Özel ilkeler](active-directory-b2c-overview-custom.md), Kimlik Deneyimi Çerçevesinin Azure AD B2C kiracınızdaki davranışlarını tanımlayan yapılandırma dosyalarıdır. Özel ilkeler birçok görevi tamamlama amacıyla tamamen düzenlenebilir. Özel ilke bir veya hiyerarşi zincirinde birbirine başvuran daha fazla XML biçimindeki dosyadan oluşabilir. 

Azure AD B2C kiracınızda gerekirse farklı türlerde birden fazla özel ilke kullanılabilir ve bunlar farklı uygulamalarda tekrar kullanılabilir. Bu esneklik kodunuzda minimum değişiklikle veya hiç değişiklik yapmadan müşteri kimliği deneyimlerini tanımlamanızı ve değiştirmenizi sağlar. İlkeler HTTP kimlik doğrulaması isteklerine özel sorgu parametresi eklenerek kullanılabilir.

Özel ilkeler kullanıcı yolculuğunu denetlemek için şu şekilde kullanılabilir:

- Ek bilgiler almak, müşteri tarafından sağlanan talepleri doğrulamak veya dış işlemleri tetiklemek için API etkileşimini tanımlama.
- Davranışları API'lerden gelen taleplere veya *migrationStatus* gibi dizin içindeki taleplere göre değiştirme.
- Yerleşik ilkelerin kapsamında olmayan iş akışları. Örneğin oturum açma sırasında müşteriden daha fazla bilgi toplama ve bir kaynağa erişmek için yetkilendirme denetimi gerçekleştirme.

### <a name="identity-providers"></a>Kimlik sağlayıcıları

Kimlik sağlayıcısı, müşterilerin kimliklerini doğrulayan ve güvenlik belirteçlerini düzenleyen bir hizmettir. Azure AD B2C'de kiracınızda Microsoft hesabı, Facebook veya Amazon gibi bir dizi kimlik sağlayıcısını yapılandırabilirsiniz. 

Azure AD B2C kiracınızda bir kimlik sağlayıcısı yapılandırmak için oluşturduğunuz kimlik sağlayıcısı uygulamasından aldığınız uygulama tanımlayıcısı veya istemci tanımlayıcısı ile parola veya gizli anahtarı kaydetmeniz gerekir. Bu tanımlayıcı ve parola daha sonra uygulamanızı yapılandırmak için kullanılır.

### <a name="user-interface-experience"></a>Kullanıcı Arabirimi deneyimi

Müşterilere sunulan HTML ve CSS içeriğinin çoğu denetlenebilir. Sayfa kullanıcı arabirimi özelleştirme seçeneğini kullanarak ilkelerin genel görünümünü özelleştirebilirsiniz. Ayrıca bu sayede uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı sağlayabilirsiniz.

Azure AD B2C kodu müşterinin tarayıcısında çalıştırır ve Çıkış Noktaları Arası Kaynak Paylaşma (CORS) olarak adlandırılan modern bir yaklaşım kullanır. İlk olarak özelleştirilmiş HTML içeriğine sahip ilkede bir URL belirtmeniz gerekir. Azure AD B2C, kullanıcı arabirimi öğelerini URL'nizden yüklenen HTML içeriğiyle birleştirdikten sonra sayfayı müşteriye gösterir.

Bir sorgu dizesi kullanarak Azure AD B2C'ye parametre gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Azure AD B2C'yi kullanmaya nasıl başlayabilirim?

Azure AD B2C'de kiracı, kuruluşunuzu ve kullanıcı dizinini temsil eder. Her Azure AD B2C kiracısı benzersizdir ve diğer Azure AD B2C kiracılarından ayrıdır. Kiracı, uygulamanızı kullanmak üzere kaydolan müşteriler hakkında bilgi içerir. Buna parolalar, profil verileri ve izinler dahil olabilir.

Tüm işlevleri etkinleştirmek ve kullanım ücretlerini ödemek için Azure AD B2C kiracınızı Azure aboneliğinize bağlamanız gerekir. Azure AD B2C müşterilerinin uygulamanızda oturum açmasına olanak tanımak için uygulamanızı Azure AD B2C kiracınıza kaydetmeniz gerekir.

Uygulamanızı Azure AD B2C'yi kullanacak şekilde yapılandırmadan önce bir Azure AD B2C kiracısı oluşturmanız ve uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydetmek için [Öğretici: Azure AD B2C'yi kullanarak kaydolma ve oturum açma özelliklerini etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md) bölümündeki adımları uygulayın.
  
ASP.NET web uygulaması geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak bir web uygulamasının hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-web-app.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

Masaüstü uygulaması geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak bir masaüstü uygulamasının hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-desktop-app.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

Node.js kullanarak tek sayfalı bir uygulama geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak tek sayfalı bir uygulamanın hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-spa.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiye devam ederek uygulamanızın kaydolma ve oturum açma deneyimini yapılandırmaya başlayın.

> [!div class="nextstepaction"]
> [Öğretici: Azure AD B2C'yi kullanarak kaydolma ve oturum açma özelliklerini etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md)
