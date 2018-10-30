---
title: Azure Active Directory B2C nedir? | Microsoft Docs
description: Azure Active Directory B2C'yi kullanarak uygulamanızın oturum açma deneyimini oluşturmayı ve yönetmeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 10/23/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fdb856f92bf790df0065cfc74ce5896f1d10c47b
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954118"
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory (Azure AD) B2C, müşterilerin uygulamanızla etkileşme biçimini özelleştirmenize ve denetlemenize olanak tanıyan bir kimlik yönetim hizmetidir. Bu etkileşim abone olmayı, oturum açmayı ve uygulamalarını kullanırken profillerini yönetmeyi içerir. Diğerlerinin yanı sıra iOS, Android ve .NET için uygulamalar arasından seçim yapabilirsiniz. Azure AD B2C, aynı anda müşterilerinizin kimliklerini korurken bu eylemleri mümkün kılar.

Azure AD B2C'ye kaydolan bir uygulamayı çoğu kimlik yönetim görevini halletmek için yapılandırırsınız. Bazı örnekler şunlardır:

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

Azure AD B2C, tüm müşteri deneyimleri için [OpenID Connect](https://openid.net/connect/) desteği sunar. OpenID Connect'in Azure AD B2C gerçekleştirmesinde uygulamanız kullanıcı yolculuğunu Azure AD B2C'ye kimlik doğrulama istekleri göndererek başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci müşterinin kimliğini tanımlar.

Azure AD B2C'yi kullanan tüm uygulamaların Azure portal aracılığıyla Azure AD B2C kiracısına kaydedilmesi gerekir. Kayıt işlemi değerler toplar ve bunları uygulamanıza atar. Bu değerler, uygulamayı benzersiz olarak tanımlayan bir uygulama kimliği içerir. Yanıtları yeniden uygulamaya yönlendirmek için bir yeniden yönlendirme URI'si tanımlanır.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, müşteriyi bir ilkeyi çalıştırmak üzere yönlendirir.
2. Müşteri, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama, güvenlik belirtecini alır.
4. Uygulama, korunan kaynağa erişmeyi denemek için güvenlik belirtecini kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama, güvenlik belirtecini düzenli aralıklarla yeniler.

Bu adımlar, geliştirdiğiniz uygulama türüne göre biraz değişebilir.

Azure AD B2C bir kimlik görevini tamamlamak için kimlik sağlayıcılar, müşteriler, diğer sistemler ve yerel dizinle etkileşim kurar. Buna müşteri oturumunu açma, yeni müşteri kaydetme veya parola sıfırlama gibi görevler dahildir. Çok taraflı güveni kuran ve bu adımları tamamlayan temel platform Kimlik Deneyimi Çerçevesi olarak adlandırılır. Bu çerçeve ve bir ilke (kullanıcı yolculuğu veya Güven Altyapısı ilkesi olarak da adlandırılır) aktörleri, eylemleri, protokolleri ve tamamlanacak adımların sırasını açıkça tanımlar.

Azure AD B2C, uygulamalarınızı hizmet engelleme ve parola saldırılarından korur. Azure AD B2C, kaynakları hizmet reddi saldırılarına karşı korumak için SYN tanımlama bilgileri gibi algılama ve risk azaltma tekniklerinin yanı sıra hız ve bağlantı sınırları kullanır. Risk azaltma özelliği ayrıca parolalara yönelik deneme yanılma saldırısı ve sözlük saldırıları için de sunulur.

#### <a name="built-in-policies"></a>Yerleşik ilkeler

Azure AD B2C'ye gönderilen her istek bir ilke belirtir. Uygulamanızın Azure AD B2C ile etkileşim kurma şekli bir ilke tarafından denetlenir. Kaydolma, oturum açma ve profil düzenleme gibi en çok gerçekleştirilen kimlik görevleri için önceden tanımlı yerleşik ilkeler bulunur.  Örneğin bir kaydolma ilkesi aşağıdaki ayarları yapılandırarak davranışları denetlemenizi sağlar:

- Müşterinin uygulamaya kaydolmak için kullanabileceği sosyal hesaplar
- Ad veya posta kodu gibi müşteriden alınan veriler
- Multi-factor authentication
- Tüm kaydolma sayfalarının genel görünümü
- Uygulamaya döndürülen bilgiler

#### <a name="custom-policies"></a>Özel ilkeler 

[Özel ilkeler](active-directory-b2c-overview-custom.md), [Kimlik Deneyimi Çerçevesi](trustframeworkpolicy.md)'nin Azure AD B2C kiracınızdaki davranışlarını tanımlayan yapılandırma dosyalarıdır. Özel ilkeler, birçok görevi yapacak şekilde değiştirilebilir. Özel ilke, bir hiyerarşi zincirinde birbirine başvuran bir veya daha fazla XML biçimli dosyadır. 

Azure AD B2C kiracınızda gerekiyorsa değişik türlerde özel ilkeler kullanılır; bunlar çeşitli uygulamalarda tekrar kullanılabilir. Bu esneklik kodunuzda minimum değişiklikle veya hiç değişiklik yapmadan müşteri kimliği deneyimlerini tanımlamanızı ve değiştirmenizi sağlar. İlkeler HTTP kimlik doğrulaması isteklerine özel bir sorgu parametresi eklenerek kullanılır.

Özel ilkeler kullanıcı yolculuğunu denetlemek için şu şekilde kullanılır:

- Ek bilgiler almak, müşteri tarafından sağlanan talepleri doğrulamak veya dış işlemleri tetiklemek için API etkileşimini tanımlama.
- Davranışları API'lerden gelen taleplere veya *migrationStatus* gibi dizin içindeki taleplere göre değiştirme.
- Yerleşik ilkelerin kapsamında olmayan iş akışları. Örneğin oturum açma sırasında müşteriden daha fazla bilgi toplama ve bir kaynağa erişmek için yetkilendirme denetimi gerçekleştirme.

### <a name="identity-providers"></a>Kimlik sağlayıcıları

Kimlik sağlayıcısı, müşterilerin kimliklerini doğrulayan ve güvenlik belirteçlerini düzenleyen bir hizmettir. Azure AD B2C'de kiracınızda diğerlerinin yanı sıra [Microsoft hesabı](active-directory-b2c-setup-msa-app.md), [Facebook](active-directory-b2c-setup-fb-app.md) veya [Amazon](active-directory-b2c-setup-amzn-app.md) gibi bir dizi kimlik sağlayıcısını yapılandırırsınız. 

Azure AD B2C kiracınızda bir kimlik sağlayıcısı yapılandırmak için oluşturduğunuz kimlik sağlayıcısı uygulamasından aldığınız uygulama tanımlayıcısı veya istemci tanımlayıcısı ile parola veya gizli anahtarı kaydetmeniz gerekir. Bu tanımlayıcı ve parola daha sonra uygulamanızı yapılandırmak için kullanılır.

### <a name="user-interface-experience"></a>Kullanıcı Arabirimi deneyimi

Müşterilere sunulan HTML ve CSS içeriğinin çoğu denetlenebilir. Sayfa kullanıcı arabirimi özelleştirme seçeneğini kullanarak ilkelerin genel görünümünü özelleştirebilirsiniz. Uygulamanız ve Azure AD B2C arasında marka ve görsellik tutarlılığını bu özelleştirme işlevini kullanarak korursunuz.

Azure AD B2C kodu müşterinin tarayıcısında çalıştırır ve Çıkış Noktaları Arası Kaynak Paylaşma (CORS) olarak adlandırılan modern bir yaklaşım kullanır. İlk olarak özelleştirilmiş HTML içeriğine sahip ilkede bir URL belirtmeniz gerekir. Azure AD B2C, kullanıcı arabirimi öğelerini URL'nizden yüklenen HTML içeriğiyle birleştirdikten sonra sayfayı müşteriye gösterir.

Bir sorgu dizesi kullanarak Azure AD B2C'ye parametre gönderirsiniz. Parametre HTML uç noktanıza iletilerek sayfa içeriği dinamik olarak değiştirilir. Örneğin Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre değiştirirsiniz.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Azure AD B2C'yi kullanmaya nasıl başlayabilirim?

Azure AD B2C'de kiracı, kuruluşunuzu ve kullanıcı dizinini temsil eder. Her Azure AD B2C kiracısı benzersizdir ve diğer Azure AD B2C kiracılarından ayrıdır. Kiracı, uygulamanızı kullanmak üzere kaydolan müşteriler hakkında bilgi içerir. Buna parolalar, profil verileri ve izinler dahil olabilir.

Tüm işlevleri etkinleştirmek ve kullanım ücretlerini ödemek için Azure AD B2C kiracınızı Azure aboneliğinize bağlayın. Müşterilerinizin uygulamanızda oturum açmasına izin vermek için uygulamayı bir Azure AD B2C kiracısında kaydedin.

Uygulamanızı Azure AD B2C'yi kullanacak şekilde yapılandırmadan önce bir Azure AD B2C kiracısı oluşturmanız ve uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydetmek için [Öğretici: Azure AD B2C'yi kullanarak kaydolma ve oturum açma özelliklerini etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md) bölümündeki adımları uygulayın.
  
ASP.NET web uygulaması geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak bir web uygulamasının hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-web-app.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

Masaüstü uygulaması geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak bir masaüstü uygulamasının hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-desktop-app.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

Node.js kullanarak tek sayfalı bir uygulama geliştiriyorsanız uygulamanızı [Öğretici: Azure AD B2C'yi kullanarak tek sayfalı bir uygulamanın hesapların kimliğini doğrulamasını sağlama](active-directory-b2c-tutorials-spa.md) bölümündeki adımları uygulayarak hesaplarda kimlik doğrulaması yapacak şekilde ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiye devam ederek uygulamanızın kaydolma ve oturum açma deneyimini yapılandırmaya başlayın.

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)
