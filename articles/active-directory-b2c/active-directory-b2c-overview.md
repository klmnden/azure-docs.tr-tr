---
title: "Azure Active Directory B2C: Genel Bakış | Microsoft Belgeleri"
description: "Azure Active Directory B2C ile tüketiciye yönelik uygulamalar geliştirme"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: swkrish
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 2f64c905d2304bfc94415e871012a783cd2cf328
ms.contentlocale: tr-tr
ms.lasthandoff: 05/03/2017


---
# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Tüketicilerinizin uygulamanıza kaydolmalarını ve oturum açmalarını sağlama
Azure Active Directory B2C, tüketiciye yönelik web ve mobil uygulamalarınız için kapsamlı bulut kimlik yönetimi çözümüdür. Yüz milyonlarca tüketici kimliği ölçeğinde yüksek oranda kullanılabilir bir küresel hizmettir. Kurumsal düzeyde güvenli bir platform üzerinde oluşturulan Azure Active Directory B2C uygulamalarınızın, işinizin ve tüketicilerinizin korunmasını sağlar.

Geçmişte tüketicilerin uygulamalarına kaydolmasını ve oturum açmasını isteyen uygulama geliştiricileri kendi kodlarını yazardı. Ayrıca, kullanıcı adları ile parolaları depolamak için şirket içi veritabanlarını veya sistemleri kullanırlardı. Azure Active Directory B2C, geliştiricilere tüketici kimlik yönetimini uygulamalarıyla tümleştirmek için daha iyi bir yol sunar. Bunu da güvenli, standart temelli bir platform ve bir dizi zengin ilkeler yardımıyla gerçekleştirir. Azure Active Directory B2C kullandığınızda tüketicileriniz, var olan sosyal medya hesaplarını (Facebook, Google, Amazon, LinkedIn) kullanarak veya yeni kimlik bilgileri (e-posta adresi ve parola veya kullanıcı adı ve parola) oluşturarak uygulamalarınıza kaydolabilir. İkinci yöntemde kullanılan kimlik bilgilerine "yerel hesaplar" diyoruz.

## <a name="get-started"></a>başlarken
Tüketicinin kaydolma ve oturum açma işlemlerini kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısı ile kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin.

[OAuth 2.0 veya Open ID Connect](active-directory-b2c-reference-protocols.md) ile protokol iletilerini doğrudan göndermeyi seçerek veya bu işlemi sizin yerinize yapmaları için kitaplıklarımızı kullanarak Azure Active Directory B2C hizmetinde kendi uygulamanızı yazabilirsiniz. Aşağıdaki tabloda en sevdiğiniz platformu seçin ve çalışmaya başlayın.

[!INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Yenilikler
Gelecekteki Azure Active Directory B2C değişiklikleri için burayı sıkça tekrar kontrol edin. @AzureAD kullanarak tüm değişiklikler hakkında tweet de göndereceğiz.

* [Genişletilebilir ilke çerçevemiz](active-directory-b2c-reference-policies.md) ve uygulamalarınızda oluşturup kullanabileceğiniz ilke türleri hakkında bilgi edinin.
* Küçük hizmet sorunları, güncelleştirmeler, durum ve düzeltmeler hakkında bildirimler için [hizmet blogumuzu](https://blogs.msdn.microsoft.com/azureadb2c/) yer işaretlerine ekleyin. [Azure durum panosunu](https://azure.microsoft.com/status/) da izlemeye devam edin.
* Geçerli [hizmet sınırlamaları, kısıtlamalar ve engeller](active-directory-b2c-limitations.md).
* Son olarak Azure AD B2C & ASP.NET Core kullanan bir [kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c).

## <a name="how-to-articles"></a>Nasıl yapılır makaleleri
Belirli Azure Active Directory B2C özelliklerinin nasıl kullanılacağını öğrenin:

* [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsoft hesabı](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) ve [LinkedIn](active-directory-b2c-setup-li-app.md) gibi hesapları, tüketiciye yönelik uygulamalarınızda kullanım için yapılandırın.
* [Tüketicileriniz hakkında bilgiler toplamak için özel öznitelikler kullanın](active-directory-b2c-reference-custom-attr.md).
* [Tüketiciye yönelik uygulamalarınızda Azure Multi-Factor Authentication'ı etkinleştirin](active-directory-b2c-reference-mfa.md).
* [Tüketicileriniz için self servis parola sıfırlama ayarlayın](active-directory-b2c-reference-sspr.md).
* Azure Active Directory B2C tarafından sunulan [kaydolma, oturum açma ve diğer tüketiciye yönelik sayfaların görünümünü özelleştirin](active-directory-b2c-reference-ui-customization.md).
* Azure Active Directory B2C kiracınızda [Tüketicileri programlama yoluyla oluşturma, okuma, güncelleştirme ve silme için Azure Active Directory Grafik API'sini kullanın](active-directory-b2c-devquickstarts-graph-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu bağlantılar hizmeti derinlemesine keşfetmek için kullanışlıdır:

* Bkz. [Azure Active Directory B2C fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Azure Active Directory B2C için [kod örneklerimizi](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) gözden geçirin. 
* Stack Overflow konusunda yardım almak için [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) etiketini kullanın.
* [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) kullanarak fikirlerinizi bize ulaştırabilirsiniz, fikirlerinizi duymak istiyoruz!
* [Azure AD B2C Protokolü Başvurusu](active-directory-b2c-reference-protocols.md)’nu gözden geçirin.
* [Azure AD B2C Belirteci Başvurusu](active-directory-b2c-reference-tokens.md)’nu gözden geçirin.
* [Azure Active Directory B2C ile ilgili SSS](active-directory-b2c-faqs.md) makalesini okuyun.
* [Azure Active Directory B2C için dosya desteği istekleri](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.


