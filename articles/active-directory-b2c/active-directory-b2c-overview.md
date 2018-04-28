---
title: Azure Active Directory B2C nedir? | Microsoft Docs
description: Nasıl oluşturabilir ve Azure Active Directory B2C kullanarak, uygulama oturum açma deneyimini yönetme hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/05/2018
ms.author: davidmu
ms.openlocfilehash: ca9e45a214639da86cf8e0c4a39b3e3d6b6d6491
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory (Azure AD) B2C, müşterilerin uygulamalarınızı kullanırken nasıl kaydolduğunu, oturum açtığını ve profillerini yönettiğini özelleştirip denetlemenizi sağlayan bir kimlik yönetimi sistemidir. Buna diğerlerinin yanında iOS, Android ve .NET için geliştirilmiş uygulamalar dahildir. Azure AD B2C bu eylemler aynı anda müşteri kimlikleriniz korunmasını sağlar.

Çeşitli kimlik yönetimi eylemler gerçekleştirmek için Azure AD B2C ile kayıtlı uygulama yapılandırabilirsiniz. Bazı örnekler şunlardır:

- Kayıtlı uygulamanızın kullanmak kaydolmak bir müşteri etkinleştir
- Oturum açmak ve uygulamanızı kullanmaya başlamak imzalanmış yukarı müşteri etkinleştir
- Kendi profili düzenlemek imzalanmış yukarı müşteri etkinleştir
- Uygulamanızdaki çok faktörlü kimlik doğrulamasını etkinleştir
- Kaydolma ve oturum belirli bir kimlik sağlayıcıları oturum açma müşteriye etkinleştir
- Oluşturacağınız API'lerine uygulamanızdan erişim izni ver
- Kaydolma ve oturum açma deneyiminin Görünüm ve yapısını özelleştirme
- Uygulamanız için tek oturum açma oturumları yönetme

## <a name="what-do-i-need-to-think-about-before-using-azure-ad-b2c"></a>Azure AD B2C kullanmadan önce dikkat etmeniz gereken ne gerekiyor?

- Uygulamam ile etkileşim kurmak için müşteri nasıl istiyor musunuz?
- Müşteriler için sağlamak istediğiniz kullanıcı arabirimi (UI) deneyimi nedir?
- Müşterilerin uygulamamda arasından hangi kimlik sağlayıcıları istiyor musunuz?
- Oturum açma işlemi çalıştırmak ek API'leri gerektiriyor mu?

### <a name="customer-interaction"></a>Müşteri etkileşimi

Azure AD B2C destekleyen [Openıd Connect](https://openid.net/connect/) için tüm müşteri deneyimleri. Openıd Connect'e ilişkin Azure AD B2C uygulamasında, Azure AD B2C'ye kimlik doğrulama isteklerini vererek, bu kullanıcı gezisine uygulamanızı başlatır. İstek sonucu `id_token` şeklindedir. Bu güvenlik belirteci Müşteri'nin kimliğini temsil eder.

Azure AD B2C kullanan her uygulamayı Azure Portalı'nı kullanarak, Azure AD B2C kiracısı kayıtlı olması gerekir. Kayıt işlemine toplar ve değerleri uygulamanıza atar. Bu değerler, uygulama ve yeniden yönlendirme yanıtlarını geri dönün yönlendirmek için kullanılan URI benzersiz olarak tanımlayan bir uygulama kimliği içerir.

Her uygulama etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, müşterinin bir ilke çalıştırmak için yönlendirir.
2. Müşteri, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama güvenlik belirtecini alır.
4. Uygulama, korunan bir kaynağa erişme girişimi için güvenlik belirtecini kullanır.
5. Kaynak sunucu, erişim izni verilebileceğini doğrulamak için güvenlik belirtecini doğrular.
6. Uygulama güvenlik belirtecini düzenli aralıklarla yeniler.

Bu adımları biraz, oluşturduğunuz uygulama türüne göre farklı olabilir.

Azure AD B2C kimlik görevi tamamlamak için sırada kimlik sağlayıcıları, müşteriler, diğer sistemler ve yerel dizin ile etkileşime girer. Örneğin, bir müşteri olarak oturum açın, yeni müşteri kaydetmek veya parola sıfırlama. Çok taraflı güven oluşturur ve bu adımları tamamlandığında temel platform kimlik deneyimi Framework adı verilir. Bu çerçeve ve (bir kullanıcı gezisine veya bir güven Framework ilke olarak da adlandırılır) bir ilke aktörler, Eylemler, protokolleri ve adımların sırasını tamamlamak için açıkça tanımlar.

Azure AD B2C, hizmet reddi ve parola korur uygulamalarınızı birden çok yolla saldırıları. Azure AD B2C algılama ve azaltma teknikler kullanır Eşitlemeye tanımlama bilgileri ve kaynakları hizmet reddi saldırılarına karşı korumak için oranı ve bağlantı sınırları ister. Azaltma da kaba kuvvet parola saldırıları ve sözlük parola saldırılarını için dahil edilmiştir.

#### <a name="built-in-policies"></a>Yerleşik ilkeler

Azure AD B2C'ye gönderilen her isteği ilke belirtir. Bir ilke uygulamanızı Azure AD B2C ile nasıl etkileşim kurduğu davranışını denetler. Yerleşik ilkeleri önceden tanımlanmış en yaygın kimlik görevlerde gibi kaydolma, oturum açma ve profil düzenleme.  Örneği için bir kayıt ilkesi, aşağıdaki ayarları yapılandırarak davranışları denetlemenize olanak sağlar:

- Müşteri uygulaması için kaydolmak için kullanabileceğiniz sosyal hesapları
- Adı veya posta kodu gibi müşteriden toplanan veriler
- Multi-factor authentication
- Görünüm ve yapısını tüm kayıt sayfaları
- Uygulamaya döndürülen bilgileri

#### <a name="custom-policies"></a>Özel ilkeler 

[Özel ilkeler](active-directory-b2c-overview-custom.md) , Azure AD B2C kiracınızda kimlik deneyimi Framework davranışını tanımlamak yapılandırma dosyalarıdır. Özel ilkeler, birçok görevi tamamlamak için tam olarak düzenlenebilir. Özel bir ilke birbirleriyle hiyerarşik bir zincirindeki başvuran bir veya birkaç XML biçimli dosya olarak temsil edilir. 

Farklı türlerde birden çok özel ilkeler, gerektiğinde Azure AD B2C kiracınızda kullanılabilir ve uygulamalar arasında yeniden kullanılabilir. Bu esneklik tanımlayın ve müşteri kimlik deneyimi ile en az veya herhangi bir değişiklik kodunuzu değiştirmenize olanak sağlar. HTTP kimlik doğrulama istekleri için özel sorgu parametresini ekleyerek ilkeleri kullanılabilir.

Özel ilkeler, kullanıcı Yolculuklar bu yolla denetlemek için kullanılabilir:

- Ek bilgileri yakalamak üzere API'leri ile etkileşim tanımlama, sağlanan Müşteri talep doğrulamak veya dış işlemlerini tetikler.
- Davranışı değiştirme temel alarak talep API'leri veya dizinde talep gibi *migrationStatus*.
- Yerleşik ilkeleri tarafından kapsanmayan tüm iş akışı. Örneğin, bir kaynağa erişmek için bir oturum açma deneyimi sırasında müşteriden daha fazla bilgi toplama ve bir yetkilendirme gerçekleştirme denetleyin.

### <a name="identity-providers"></a>Kimlik sağlayıcıları

Bir kimlik sağlayıcısı müşteri kimliklerini doğrular ve güvenlik belirteçleri bir hizmettir. Azure AD B2C'de bir Microsoft hesabı gibi Facebook veya Amazon diğerlerinin yanı sıra, Kiracı kimlik sağlayıcıları sayısını yapılandırabilirsiniz. 

Bir kimlik sağlayıcısı, Azure AD B2C kiracınızda yapılandırmak için uygulama tanımlayıcısı veya istemci tanımlayıcısı ve parola veya istemci gizli anahtarı oluşturduğunuz kimlik sağlayıcısı uygulamadan kaydetmeniz gerekir. Ardından bu tanımlayıcı ve parola Uygulamanızı yapılandırmak için kullanılır.

### <a name="user-interface-experience"></a>Kullanıcı arabirimi deneyimi

Çoğu müşterilere sunulan HTML ve CSS içeriği denetlenebilir. Sayfanın UI Özelleştirme özelliğini kullanarak, herhangi bir ilke görünümünü özelleştirin. Ayrıca, marka ve uygulama ve Azure AD B2C arasında visual tutarlılık koruyabilirsiniz.

Azure AD B2C Müşteri'nin tarayıcıda kodu çalıştırır ve çıkış noktaları arası kaynak paylaşımı (CORS) olarak adlandırılan modern bir yaklaşım kullanır. İlk olarak, özelleştirilmiş HTML içeriğe sahip bir ilke bir URL belirtin. Azure AD B2C, URL'den yüklenir ve ardından sayfanın müşteriye görüntüleyen HTML içeriğe sahip kullanıcı Arabirimi öğeleri birleştirir.

Azure AD B2C'ye bir sorgu dizesi parametreleri gönderebilirsiniz. HTML uç noktanızı parametresini geçirerek, sayfa içeriği dinamik olarak değiştirebilirsiniz. Örneğin, web ya da mobil uygulama geçirdiğiniz parametre temel Azure AD B2C kaydolma veya oturum açma sayfasında, arka plan resmi değiştirebilirsiniz.

## <a name="how-do-i-get-started-with-azure-ad-b2c"></a>Azure AD B2C ile çalışmaya nasıl?

Azure AD B2C'de, bir kiracı, kuruluşunuzun temsil eder ve kullanıcılar dizinidir. Her Azure AD B2C kiracısı farklı ve diğer Azure AD B2C kiracılar ayrıdır. Bir kiracı, uygulamanızın kullanmak için kaydolan müşteriler hakkında bilgiler içerir. Örneğin, parolalar, profil verileri ve izinleri.

Azure aboneliğiniz için kullanım ücretleri ödeme tüm işlevselliğini etkinleştirmek için Azure AD B2C kiracınızın bağlamanız gerekir. Uygulamanıza oturum açmak Azure AD B2C müşteriler izin vermek için bir Azure AD B2C kiracısı uygulamanızı kaydetmeniz gerekir.

Uygulamanızı Azure AD B2C kullanacak şekilde yapılandırmadan önce ilk Azure AD B2C kiracısı oluşturmak ve uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydetmek için adımları tamamlamanız [Öğreticisi: Kaydolma ve oturum açma Azure AD B2C kullanarak etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md).
  
Bir ASP.NET web uygulaması geliştiricisi varsa, içinde yer alan adımları kullanarak hesaplarının kimliğini doğrulamak için uygulamanızı ayarlayın [Öğreticisi: Azure AD B2C kullanarak hesapları ile kimlik doğrulaması bir web uygulamasını etkinleştirecek](active-directory-b2c-tutorials-web-app.md).

Olduğunuz bir masaüstü uygulaması Geliştirici ayarlamışsa adımları kullanarak hesaplarının kimliğini doğrulamak için uygulamanızın [Öğreticisi: Azure AD B2C kullanarak hesapları ile kimlik doğrulaması bir masaüstü uygulaması etkinleştirmek](active-directory-b2c-tutorials-desktop-app.md).

Node.js kullanarak bir tek sayfalı uygulama geliştiricisi varsa, içinde yer alan adımları kullanarak hesaplarının kimliğini doğrulamak için uygulamanızı ayarlayın [Öğreticisi: Azure AD B2C kullanarak hesapları ile kimlik doğrulaması bir tek sayfalı uygulama etkinleştirme](active-directory-b2c-tutorials-spa.md).

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanız kaydolma ve oturum açma deneyimi için Öğreticisine devam ederek yapılandırma başlatın.

> [!div class="nextstepaction"]
> [Öğretici: Kaydolma ve oturum açma Azure AD B2C kullanarak etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md)
