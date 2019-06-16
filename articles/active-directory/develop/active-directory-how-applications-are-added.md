---
title: Uygulamaları Azure Active Directory'ye neden ve nasıl eklenir
description: Bir uygulamanın Azure AD'ye eklenmesi ne anlama geliyor ve nasıl bunlar burada elde ederim?
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/04/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: elisol, lenalepa
ms.collection: M365-identity-device-management
ms.openlocfilehash: b784cafce08634f1026a908e8ccdaaed41b62a42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111625"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Uygulamaları Azure AD'ye neden ve nasıl eklenir

Azure AD'de uygulamaları iki sunumu vardır: 
* [Uygulama nesneleri](app-objects-and-service-principals.md#application-object) - olmasına rağmen [özel durumları](#notes-and-exceptions), uygulama nesneleri bir uygulama tanımını kabul.
* [Hizmet sorumluları](app-objects-and-service-principals.md#service-principal-object) -uygulamanın bir örneği olarak düşünülebilir. Hizmet sorumluları genellikle bir uygulama nesnesine başvurursanız ve bir uygulama nesnesi dizinler arasında birden çok hizmet sorumluları tarafından başvurulabilir.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Uygulama nesneleri nedir ve burada geliyorlar?
Yönetebileceğiniz [uygulama nesneleri](app-objects-and-service-principals.md#application-object) Azure portalında [uygulama kayıtları](https://aka.ms/appregistrations) karşılaşırsınız. Uygulama nesneleri Azure AD'ye uygulamayı tanımlayan ve kendi ayarlarınızı temel alan uygulama belirteçlerini vermek nasıl hizmet verme uygulaması tanımını kabul edilebilir. Hizmet sorumluları diğer dizinlerde destekleyen çok kiracılı bir uygulama olsa bile, uygulama nesnesinin yalnızca kendi giriş dizininde bulunur. Uygulama nesnesi (olarak da gibi ek bilgileri burada bahsedilen değil) aşağıdakilerden herhangi birini içerebilir:
* Ad, logo ve yayımcı
* Yeniden yönlendirme URI'leri
* Gizli anahtarları (uygulama kimliğini doğrulamak için kullanılan simetrik ve/veya asimetrik anahtarlar)
* API bağımlılıkları (OAuth)
* Yayımlanan API'leri/kaynak/kapsamlar (OAuth)
* Uygulama rolleri (RBAC)
* SSO meta verileri ve yapılandırma
* Kullanıcı sağlama meta verileri ve yapılandırma
* Proxy meta verileri ve yapılandırma

Uygulama nesneleri dahil olmak üzere birden çok yollarla oluşturulabilir:
* Azure portalında uygulama kayıtları
* Azure AD kimlik doğrulamasını kullanmak için Visual Studio kullanarak ve yapılandırarak yeni bir uygulama oluşturma
* Ne zaman bir yönetici (Bu da bir hizmet sorumlusu oluşturur) uygulama galerisinden bir uygulama ekler.
* Yeni bir uygulama oluşturmak için Microsoft Graph API, Azure AD Graph API'si veya PowerShell kullanarak
* Diğer birçok geliştirici merkezinde çeşitli Geliştirici deneyimleri azure'da ve API Gezgini deneyimleri dahil

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Hizmet sorumluları nedir ve burada geliyorlar?
Yönetebileceğiniz [hizmet sorumluları](app-objects-and-service-principals.md#service-principal-object) Azure portalında [kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) karşılaşırsınız. Hizmet sorumluları, hangi uygulamanın Azure AD'ye bağlanma yönetecek olan ve uygulama dizininizde örneğini kabul edilebilir. Verilen herhangi bir uygulama için bir "Giriş" dizinine kaydedilir) en fazla bir uygulama nesnesi (ve uygulamanın içinde yapan her dizinde örneklerini temsil eden bir veya daha fazla hizmet sorumlusu nesneleri olabilir. 

Hizmet sorumlusu şunları içerebilir:

* Uygulama Kimliği özelliği aracılığıyla bir uygulama nesnesi geri başvuru
* Yerel kullanıcı ve Grup uygulama rolü atamalarını kayıtları
* Uygulamaya verilen yerel kullanıcı ve yönetici izinleri kayıtları
  * Örneğin: uygulamanın belirli bir kullanıcının e-postaya erişmek izni
* Koşullu erişim ilkesi dahil olmak üzere yerel ilkeler kayıtları
* Bir uygulama için diğer yerel ayarların kayıt
  * Talep dönüştürme kuralları
  * Öznitelik eşlemeleri (kullanıcı hazırlama)
  * (Özel roller uygulamanın desteklediği durumlarda) dizin özgü uygulama rolleri
  * Özel dizin adı veya logosu

Uygulama nesneleri gibi hizmet sorumluları dahil olmak üzere birden çok yollarla oluşturulabilir:

* Ne zaman bir üçüncü taraf uygulama için oturum açan kullanıcılar, Azure AD ile tümleşik
  * Oturum açma sırasında kullanıcıların profillerini erişmek için uygulamayı izni ve diğer izinler vermeniz istenir. İlk izin verene kişiye dizine eklenmesi için uygulamayı temsil eden bir hizmet sorumlusu neden olur.
* Microsoft online services kullanıcılar oturum açtığında ister [Office 365](https://products.office.com/)
  * Office 365'e abone veya bir deneme sürümünü başlatmak, dizindeki tüm Office 365 ile ilişkili işlevselliği sunmak için kullanılan çeşitli Hizmetleri temsil eden bir veya daha fazla hizmet sorumluları oluşturulur.
  * SharePoint gibi bazı Office 365 hizmetlerine, iş akışları dahil olmak üzere bileşenleri arasında güvenli iletişim sağlamak için düzenli olarak hizmet sorumluları oluşturma.
* Ne zaman bir yönetici (Bu da uygulama nesnesini oluşturur) uygulama galerisinden bir uygulama ekler.
* Kullanmak için uygulama ekleme [Azure AD uygulama ara sunucusu](/azure/active-directory/manage-apps/application-proxy)
* Bir uygulama için çoklu oturum açma bağlanmak SAML veya parola kullanarak çoklu oturum açma (SSO)
* Program aracılığıyla Azure AD Graph API'si veya PowerShell aracılığıyla

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Nasıl uygulama nesneleri ve hizmet sorumluları birbiriyle ilgili?
Uygulamanın kendi ana dizini (uygulama giriş dizini dahil) nerede çalıştığını dizinlerin her bir veya daha fazla hizmet sorumluları tarafından başvurulan bir uygulama nesnesi vardır.
![Birbirine ve Azure AD örneğinde uygulama nesneleri ve hizmet sorumluları nasıl etkileşime gösteren diyagram.][apps_service_principals_directory]

Önceki diyagramda iki dizin dahili olarak Microsoft'un (sol tarafta, uygulamaları yayımlamak için kullandığı gösterilmiştir):

* Bir Microsoft Apps (Microsoft Hizmetleri dizini)
* Önceden tümleştirilmiş üçüncü taraf uygulamaları (uygulama Galerisi dizin) için

Azure AD ile tümleştirme uygulama yayımcıları/satıcıların yayımlama dizini (sağdaki "Bazı SaaS dizini" olarak gösterilir) için gereklidir.

Kendiniz eklediğiniz uygulama (olarak temsil edilen **uygulama (sizin)** diyagramdaki) içerir:

* Geliştirdiğiniz uygulamaları (Azure AD ile tümleşik)
* İçin bağlı uygulamalar çoklu oturum açma
* Azure AD uygulama proxy'si kullanılarak yayımlanmış uygulamalar

### <a name="notes-and-exceptions"></a>Notlar ve özel durumlar
* Tüm hizmet sorumlularını uygulama nesneye geri gelin. Azure AD başlangıçta oluşturulduğunda uygulamalar için sağlanan hizmetleri daha sınırlıydı ve hizmet sorumlusu uygulama kimliği oluşturmak için yeterli. Özgün hizmet sorumlusu için Windows Server Active Directory hizmet hesabı şeklinde yakın. Bu nedenle, bir uygulama nesnesi oluşturmadan, Azure AD PowerShell kullanarak gibi farklı yollarla, hizmet sorumlularının oluşturmak yine de mümkündür. Azure AD Graph API, hizmet sorumlusu oluşturma önce bir uygulama nesnesi gerektirir.
* Tüm yukarıda açıklanan bilgileri şu anda sunulmuştur programlı olarak. Yalnızca kullanıcı Arabiriminde kullanılabilen şunlardır:
  * Talep dönüştürme kuralları
  * Öznitelik eşlemeleri (kullanıcı hazırlama)
* Uygulama nesneleri ve hizmet sorumlusu hakkında daha ayrıntılı bilgi için Azure AD Graph REST API başvuru belgelerine bakın:
  * [Uygulama](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Hizmet sorumlusu](/previous-versions/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Neden Azure AD ile uygulamaları tümleştirme?
Uygulamaları Azure AD'ye bir veya daha fazlası dahil olmak üzere sağladığı hizmetler için eklenir:

* Uygulama kimlik doğrulaması ve yetkilendirme
* Kullanıcı kimlik doğrulaması ve yetkilendirme
* Federasyon veya parola kullanarak SSO
* Kullanıcı sağlamayı ve eşitleme
* Rol tabanlı erişim denetimi - uygulama rolleri, rol tabanlı yetkilendirme gerçekleştirmeye tanımlamak için dizini kullanılacak bir uygulamada denetler.
* OAuth yetkilendirme hizmetleri - API/kaynaklara erişim yetkisi vermek için Office 365 ve diğer Microsoft uygulamaları tarafından kullanılan
* Uygulama yayımlama ve proxy - özel bir ağ üzerinden bir uygulama İnternet'e yayımlama

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Uygulamaları Azure AD'ye Örneğim ekleme izni kimler?
Yalnızca genel Yöneticiler (uygulamalar uygulama galerisinden ekleme ve uygulama proxy'si kullanmak için bir uygulama yapılandırma gibi) varsayılan olarak dizininizdeki tüm kullanıcılara yapabileceğiniz bazı görevleri uygulamayı kaydetmek için haklara sahip olduğunuzda, nesneleri Bunlar, geliştirme ve hangi uygulamaların bunlar paylaşımı/erişim izni aracılığıyla kuruluş verilerine verin takdirine bağlı olarak. Bir kişi bir uygulamada oturum ve izin vermek için ilk kullanıcı dizininizdeki ise, kiracınızda bir hizmet sorumlusu oluşturur; Aksi takdirde, mevcut hizmet sorumlusu üzerinde izin verme bilgileri depolanır.

Kaydolun ve uygulamaları kaybolabileceğini başlangıçta ses ilgili için onay ancak aşağıdakileri göz önünde bulundurun izin vererek:


* Uygulamalar, uygulamanın kayıtlı veya dizinde kayıtlı gerek kalmadan Windows Server Active Directory kullanıcı kimlik doğrulaması için yıllardır yararlanabilmelidir olmuştur. Artık kuruluş görünürlük kaç uygulamalar dizinin tam olarak kullanıyor ve hangi amaçla Gelişmiş.
* Kullanıcılara bu sorumlulukları temsilci olarak görevlendirme gereken bir yönetici temelli bir uygulama kaydı ve yayımlama işlemi için geçersiz hale getirir. Active Directory Federasyon Hizmetleri (ADFS) ile bir yönetici olarak bir bağlı olan taraf geliştiricileri adına bir uygulama eklemek sahip olabilir. Artık geliştiriciler Self Servis edebilirsiniz.
* Ticari amaçlarla kendi kuruluş hesapları kullanan uygulamalar için oturum açan kullanıcılar iyi bir şeydir. Sonradan kuruluştan ayrılan kullanıcılar otomatik olarak erişim için kullanmakta olduğunuz uygulama hesaplarındaki kaybedersiniz.
* Hangi verilerin uygulama dâhil olduğu paylaşılan kaydını sahip. Veri her zamankinden çok daha taşınabilir ve kimin hangi uygulamaları ile hangi verilerin paylaşılan Temizle kaydını sağlamak kullanışlıdır.
* Azure AD için OAuth kullanan API sahipleri, tam olarak hangi izinleri kullanıcılar uygulamalara verebilir ve kabul etmek için yönetici izinleri gerektiren karar verin. Kullanıcı onayı özellikleri ve kullanıcıların kendi verilerini kapsamlıdır sırada geniş kapsam ve daha önemli izinleri yalnızca Yöneticiler onay verebilir.
* Bir kullanıcı ekler veya verilerine erişmek bir uygulama sağlar, Azure portalındaki denetim uygulamanın dizinine nasıl eklendiğini belirlemek için raporları görüntülemek için olayı denetlenebilir.

Dizininizdeki kullanıcılar uygulamaları kaydetme ve yönetici onayı olmadan uygulamaları açmayı önlemek istiyorsanız, bu özellikleri devre dışı kapatmak için değiştirebileceğiniz iki ayarı vardır:

* Kullanıcılar uygulamalara kendi adınıza verme konusunda çekince engellemek için:
  1. Azure portalında Git [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) kurumsal uygulamalar bölümünde.
  2. Değişiklik **kullanıcılar uygulamalara kendileri adına şirket verilerine erişme izni verebilir** için **Hayır**. 
     
     > [!NOTE]
     > Kullanıcı onayı devre dışı dönüştürmeye karar verirseniz, bir yönetici bir kullanıcı kullanmak için gereken tüm yeni uygulama onayı gerekli olacaktır.    
* Kullanıcıların kendi uygulamalarını kaydetmesini engellemek için:
  1. Azure portalında Git [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) bölümü altında Azure Active Directory
  2. Değişiklik **kullanıcılar uygulamaları kaydedebilir** için **Hayır**.

> [!NOTE]
> Microsoft kendi varsayılan yapılandırmayı kullanıcılarla uygulamaları ve kendi adınıza uygulamalara onay kullanır.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

