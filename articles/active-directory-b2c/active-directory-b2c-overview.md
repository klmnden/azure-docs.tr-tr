---
title: Azure Active Directory B2C nedir? | Microsoft Docs
description: Nasıl oluşturabileceğinizi ve Azure Active Directory B2C kullanarak uygulamanızda kayıt oturum açma ve profil yönetimi gibi kimlik deneyimi yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 02/20/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9e01ba8ae53dbcca686a9844600a5df416a685ae
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60317369"
---
# <a name="what-is-azure-active-directory-b2c"></a>Azure Active Directory B2C nedir?

Azure Active Directory (Azure AD) B2C Kimlik Yönetimi hizmetidir. Bu hizmetin özelleştirmek ve denetlemek nasıl kullanıcıların güvenli bir şekilde, web ve Masaüstü, mobil veya tek sayfa uygulamaları etkileşim sağlar. Azure AD B2C kullanarak, kullanıcılar kaydolabilir, oturum açın, parolaları sıfırlama ve profilleri düzenleme. Azure AD B2C, bir form Openıd Connect ve OAuth 2.0 protokolünü uygular. Bu protokollerin uygulanması önemli anahtarı güvenlik belirteçleri ve kaynaklarına güvenli erişim sağlamaya olanak tanıyan, talepleri ' dir.

A *kullanıcı yolculuğu* kullanıcı ve uygulamanızı Azure AD B2C ile nasıl etkileşime davranışını denetleyen bir ilke belirten isteğidir. Azure AD B2C'de kullanıcı yolculuklarından tanımlamak için kullanabileceğiniz iki yolu. 

Siz ile veya kimlik uzmanlığı gerektirmeden bir uygulama geliştiricisi gibiyseniz, Azure portalını kullanarak genel kimlik kullanıcı akışları tanımlamak isteyebilirsiniz. Olan bir kimlik professional, sistem entegratörü, Danışman, veya bir şirket içi kimlik takımda Openıd Connect akışlarıyla deneyimliyseniz ve kimlik sağlayıcısı ve talep tabanlı kimlik doğrulaması anlamak, XML tabanlı özel ilkeleri seçebilirsiniz.

Kullanıcı yolculuğu tanımlama başlamadan önce bir Azure AD B2C kiracısı oluşturma ve uygulama ve API kiracıda kaydetmeniz gerekir. Bu görevleri tamamladıktan sonra bir kullanıcı yolculuğu kullanıcı akışları ya da özel ilkeler tanımlayarak başlayabilirsiniz. Ayrıca isteğe bağlı olarak ekleyin veya kimlik sağlayıcıları değiştirin veya kullanıcı yolculuğu düştüğünü şekilde özelleştirebilirsiniz.

## <a name="protocols-and-tokens"></a>Protokoller ve belirteçler

Azure AD B2C'yi destekleyen [protokolleri, Openıd Connect ve OAuth 2.0](active-directory-b2c-reference-protocols.md) kullanıcı yolculuklarından için. OpenID Connect'in Azure AD B2C gerçekleştirmesinde uygulamanız kullanıcı yolculuğunu Azure AD B2C'ye kimlik doğrulama istekleri göndererek başlatır. 

Azure AD B2C'ye bir isteğin sonucu gibi bir güvenlik belirteci olan bir [kimlik belirteci veya erişim belirteci](active-directory-b2c-reference-tokens.md). Bu güvenlik belirtecinin kullanıcı kimliğini tanımlar. Belirteçleri alınan Azure AD B2C uç noktalarından gibi bir `/token` veya `/authorize` uç noktası. Bu belirteçleri, kimlik doğrulama ve kaynakları güvenli hale getirmek erişim izni vermek için kullanılabilir talep erişebilirsiniz.

## <a name="tenants-and-applications"></a>Kiracılar ve uygulamalar

Azure AD B2C'de bir *Kiracı* kuruluşunuzu temsil eden ve kullanıcıların dizinidir. Her Azure AD B2C kiracısı benzersizdir ve diğer Azure AD B2C kiracılarından ayrıdır. Bir Azure Active Directory kiracısına zaten sahip olabilir, farklı bir kiracıya Azure AD B2C kiracısı olur. Bir kiracı uygulamanızı kullanmak için kaydolan kullanıcılar hakkında bilgiler içerir. Buna parolalar, profil verileri ve izinler dahil olabilir. Daha fazla bilgi için [Öğreticisi: Bir Azure Active Directory B2C kiracısı oluşturmayı](tutorial-create-tenant.md).

Uygulamanızı Azure AD B2C'yi kullanacak şekilde yapılandırmadan önce öncelikle Azure portalını kullanarak kiracıda kaydetmeniz gerekir. Kayıt işlemi değerler toplar ve bunları uygulamanıza atar. Bu değerler, uygulama ve bir yeniden yönlendirme yanıtlarını uygulamaya yeniden yönlendirmek için kullanılan URI benzersiz olarak tanımlayan bir uygulama kimliği içerir.

Her uygulamanın etkileşimini benzer bir üst düzey deseni izler:

1. Uygulama, kullanıcı bir ilke çalıştırmak için yönlendirir.
2. Kullanıcı, ilkeyi ilke tanımına göre tamamlar.
3. Uygulama bir belirteç alır.
4. Uygulama belirteci bir kaynağa erişmek için kullanır.
5. Kaynak sunucuda erişim izni verilebileceğini doğrulamak için belirteci doğrular.
6. Uygulama belirteci düzenli aralıklarla yeniler.

Bir web uygulaması kaydetmek için adımları tamamlamanız [Öğreticisi: Kaydolma ve oturum açma Azure AD B2C kullanarak etkinleştirmek için bir uygulamayı kaydetme](tutorial-register-applications.md). Ayrıca [bir web API uygulaması Azure Active Directory B2C kiracınıza ekleme](add-web-application.md) veya [yerel istemci uygulaması Azure Active Directory B2C kiracınıza ekleyin](add-native-application.md).

## <a name="user-journeys"></a>Kullanıcı yolculuklarından

Kullanıcı yolculuğu ilkesinde olarak tanımlanabilir bir [kullanıcı akışı](active-directory-b2c-reference-policies.md) veya [özel ilke](active-directory-b2c-overview-custom.md). Yaygın kimlik görevler için kullanıcı akışları gibi önceden tanımlanmış kaydolma, oturum açma ve profil düzenleme, Azure portalında kullanılabilir.

Kullanıcı yolculuklardan, aşağıdaki ayarları yapılandırarak davranışları denetlemenize olanak sağlar:

- Uygulama için kaydolmak için kullanıcının kullandığı sosyal hesaplar
- Kullanıcı adı veya posta kodu gibi toplanan verileri
- Multi-factor authentication
- Sayfa görünümü ve deneyimini
- Uygulamaya döndürülen bilgiler

Özel ilkelerdir davranışını tanımlayan yapılandırma dosyalarını [kimlik deneyimi çerçevesi](trustframeworkpolicy.md) Azure AD B2C kiracınızdaki. Kimlik deneyimi çerçevesi çok taraflı güven oluşturur ve bir kullanıcı yolculuğu'ndaki adımları tamamlandıktan temel alınan bir platformdur. 

Özel ilkeler, birçok görevi yapacak şekilde değiştirilebilir. Özel ilke, bir hiyerarşi zincirinde birbirine başvuran bir veya daha fazla XML biçimli dosyadır. A [başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) ortak kimlik görevlerini etkinleştirmek özel ilkeler kullanılabilir. 

Özel ilkeleri veya kullanıcı akışları farklı türlerde gerektiğinde Azure AD B2C kiracınızda kullanılan ve uygulamalar arasında yeniden kullanılabilir. Bu esneklik, tanımlamak ve kullanıcı kimlik deneyimi değişikliğiyle veya hiç değişiklik kodunuzda değişiklik sağlar. İlkeler HTTP kimlik doğrulaması isteklerine özel bir sorgu parametresi eklenerek kullanılır. Kendi özel bir ilke oluşturmak için bkz: [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="identity-providers"></a>Kimlik sağlayıcıları 

Uygulamalarınızda farklı kimlik sağlayıcıları ile oturum açmasını sağlamak isteyebilirsiniz. Bir *kimlik sağlayıcısı* oluşturur, korur ve uygulamalar için kimlik doğrulama hizmetleri sağlarken kimlik bilgilerini yönetir. Azure AD B2C tarafından desteklenen kimlik sağlayıcıları ekleyebileceğiniz Azure portalını kullanarak. 

Uygulamanızda genellikle yalnızca bir kimlik sağlayıcısı kullanın, ancak daha fazla ekleme seçeneğine sahipsiniz. Azure AD B2C kiracınızda bir kimlik sağlayıcısı yapılandırmak için önce bir uygulama kimlik sağlayıcısının Geliştirici sitesinde oluşturmanız ve ardından uygulama tanımlayıcısı veya istemci tanımlayıcısı ve parola veya kimlik sağlayıcısından gizli bir istemci kaydı Oluşturduğunuz uygulama. Bu tanımlayıcı ve parola daha sonra uygulamanızı yapılandırmak için kullanılır. 

Aşağıdaki makaleler, bazı yaygın kimlik sağlayıcıları için kullanıcı akışları ekleme adımlarını açıklar:

- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Microsoft hesabı](active-directory-b2c-setup-msa-app.md)

Aşağıdaki makaleler, bazı yaygın kimlik sağlayıcıları için özel ilkeler ekleme adımlarını açıklar:
- [Amazon](setup-amazon-custom.md)
- [Google](active-directory-b2c-custom-setup-goog-idp.md)
- [Microsoft hesabı](active-directory-b2c-custom-setup-msa-idp.md)

Daha fazla bilgi için [Öğreticisi: Azure Active Directory B2C, uygulamalarınızda kimlik sağlayıcıları eklemek](tutorial-add-identity-providers.md).


## <a name="page-customization"></a>Sayfa özelleştirmesi

Çoğu kullanıcı yolculuğu müşterilere sunulan HTML ve CSS içeriği denetlenebilir. Sayfayı özelleştirme kullanarak, herhangi özel bir ilke veya kullanıcı Akış görünümünü özelleştirebilirsiniz. Uygulamanız ve Azure AD B2C arasında marka ve görsellik tutarlılığını bu özelleştirme işlevini kullanarak korursunuz. 

Azure AD B2C, kullanıcının tarayıcısında kodu çalıştırır ve çıkış noktaları arası kaynak paylaşımı (CORS) olarak adlandırılan modern bir yaklaşımı kullanır. İlk olarak özelleştirilmiş HTML içeriğine sahip ilkede bir URL belirtmeniz gerekir. Azure AD B2C kullanıcı arabirimi öğeleri, URL'den yüklenir ve ardından sayfanın kullanıcıya görüntüleyen bir HTML içeriği ile birleştirir.

Bir sorgu dizesi kullanarak Azure AD B2C'ye parametre gönderirsiniz. Parametre HTML uç noktanıza iletilerek sayfa içeriği dinamik olarak değiştirilir. Örneğin, arka plan resminin kaydolma veya oturum açma sayfasında, web veya mobil uygulamanıza geçirdiğiniz parametre göre değiştirin.

Kullanıcı akışı sayfalarında özelleştirmek için bkz [Öğreticisi: Azure Active Directory B2C kullanıcı deneyimleri özelleştirebilir](tutorial-customize-ui.md). Özel bir ilkede sayfalarını özelleştirme için bkz: [özel bir ilke kullanarak Azure Active Directory B2C'de, uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md) veya [kullanıcı arabirimini dinamik içerik ile Azure'da özel ilkeler kullanarak yapılandırma Active Directory B2C](active-directory-b2c-ui-customization-custom-dynamic.md).

## <a name="developer-resources"></a>Geliştirici kaynakları

### <a name="client-applications"></a>İstemci uygulamaları

Uygulamalar için seçeneğiniz [iOS](active-directory-b2c-devquickstarts-ios.md), [Android](active-directory-b2c-devquickstarts-android.md)ve .NET, diğerlerinin yanı sıra. Azure AD B2C, aynı zamanda, kullanıcı kimliklerini korurken bu eylemleri sağlar.

Siz bir ASP.NET web uygulama geliştiricisi gibiyseniz, hesapları içindeki adımları kullanarak kimlik doğrulaması için uygulamanızı ayarlayın [Öğreticisi: Azure AD B2C kullanarak hesaplarla kimlik doğrulaması için bir web uygulamasına etkinleştirme](active-directory-b2c-tutorials-web-app.md).

İçindeki adımları kullanarak hesaplarının kimliğini doğrulamak için uygulamanıza bir masaüstü uygulaması geliştiricisiyseniz ayarlama [Öğreticisi: Azure AD B2C kullanarak hesaplarla kimlik doğrulaması bir masaüstü uygulaması etkinleştirme](active-directory-b2c-tutorials-desktop-app.md).

İçindeki adımları kullanarak hesaplarının kimliğini doğrulamak için uygulamanıza, Node.js kullanarak bir tek sayfalı uygulama geliştiricisiyseniz ayarlama [Öğreticisi: Azure AD B2C kullanarak hesaplarla kimlik doğrulaması için bir tek sayfalı uygulamayı etkinleştir](active-directory-b2c-tutorials-spa.md).

### <a name="apis"></a>API'ler
API'leri çağırmak, istemci veya web uygulamalarınızın ihtiyacınız varsa, Azure AD B2C'de bu kaynaklara güvenli erişim için ayarlayabilirsiniz.

Uygulamanızın içindeki adımları kullanarak korumalı bir API çağrısı, bir ASP.NET web uygulama geliştiricisiyseniz ayarlama [Öğreticisi: Bir ASP.NET web Azure Active Directory B2C kullanarak API erişimi verme](active-directory-b2c-tutorials-web-api.md).

Masaüstü uygulaması geliştiricisiyseniz içindeki adımları kullanarak korumalı bir API'yi çağırmak için uygulamanızı ayarlayın [Öğreticisi: Erişim ver Node.js web API'si için Azure Active Directory B2C kullanarak bir masaüstü uygulamasından](active-directory-b2c-tutorials-desktop-app-webapi.md).

Node.js kullanarak bir tek sayfalı uygulama geliştiricisi olarak, içindeki adımları kullanarak hesapları kimlik doğrulaması için uygulamanızı ayarlayın [Öğreticisi: Erişim ver bir ASP.NET Core web API'si için Azure Active Directory B2C kullanarak tek sayfalı uygulamasından](active-directory-b2c-tutorials-spa-webapi.md).

### <a name="javascript"></a>JavaScript

Azure AD B2C uygulamalarınızda kendi JavaScript istemci tarafı kod ekleyebilirsiniz. JavaScript ' uygulamanızda ayarlamak için tanımladığınız bir [sayfa sözleşme](page-contract.md) ve etkinleştirme [JavaScript](javascript-samples.md) kullanıcı akışları veya özel ilkeler.

### <a name="user-accounts"></a>Kullanıcı hesapları

Birçok genel kiracı yönetim görevlerini programlı bir şekilde gerçekleştirilmesi gerekir. Kullanıcı Yönetimi birincil örnektir. Azure AD B2C kiracısı için mevcut bir kullanıcı deposu geçirmeniz gerekebilir. Kullanıcı kayıt sayfasında, kendi ana bilgisayar ve kullanıcı hesaplarını arka planda, Azure AD B2C dizini oluşturmak isteyebilirsiniz. Bu tür görevleri oluşturabilir, okuyabilir, güncelleştirebilir olanağına sahip olmalıdır ve kullanıcı hesapları silebilirsiniz. Bu görevleri kullanarak yapabileceğiniz [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

Öğreticiye devam ederek uygulamanızın kaydolma ve oturum açma deneyimini yapılandırmaya başlayın.

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)
