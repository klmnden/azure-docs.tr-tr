---
title: Uygulamaları Azure Active Directory'ye neden ve nasıl eklenir
description: Azure AD ile eklenecek bir uygulama için ne anlama geliyor ve nasıl var. elde?
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: c9ebfcba59e3f46fb30f4cd2402ec4ebb606f6d0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156179"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Neden ve nasıl uygulamaları için Azure AD eklenir
Azure AD'de uygulamalar iki sunumu vardır: 
* [Uygulama nesneleri](active-directory-application-objects.md#application-object) - vardır, ancak [özel durumları](#notes-and-exceptions), bu uygulamanın tanımını kabul edilebilir.
* [Hizmet sorumluları](active-directory-application-objects.md#service-principal-object) -bu uygulamanın bir örneği olarak düşünülebilir. Hizmet asıl adı genellikle bir uygulama nesne başvurusu ve bir uygulama nesnesi tarafından birden çok hizmet asıl adı dizinlerde başvurulabilir.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Uygulama nesneleri nelerdir ve bunlar nereden geldiği?
[Uygulama nesneleri](active-directory-application-objects.md#application-object) (hangi Azure portalı üzerinden yönetebilirsiniz [uygulama kayıtlar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ApplicationsListBlade) deneyimi) Azure ad uygulamayı tanımlayan ve uygulama tanımını kabul edilebilir izin verme ayarlarına bağlı uygulama belirteçleri vermek üzere nasıl bilmeniz hizmeti. Hizmet sorumluları diğer dizinlerde destekleyen çok kiracılı uygulama olsa bile uygulama nesnesi yalnızca kendi giriş dizininde bulunur. Uygulama nesnesi (olarak iyi gibi ek bilgileri burada sözü edilen değil) aşağıdakilerden herhangi birini içerebilir:
* Adı, logosu ve yayımcı
* Yanıt URL'leri
* Gizli (uygulama kimliğini doğrulamak için kullanılan simetrik ve/veya asimetrik anahtarlar)
* API bağımlılıkları (OAuth)
* Yayımlanan API'leri/kaynakları/kapsamları (OAuth)
* Uygulama rolleri (RBAC)
* SSO meta verileri ve yapılandırma
* Kullanıcı meta verileri ve yapılandırma hazırlama
* Proxy meta verileri ve yapılandırma

Uygulama nesneleri dahil olmak üzere birden çok yollarıyla oluşturulabilir:
* Azure portalında uygulama kayıtlar
* Azure AD kimlik doğrulaması kullanmak için Visual Studio kullanarak ve onu yapılandırma yeni bir uygulama oluşturma
* Bir yönetici (Bu da bir hizmet sorumlusu oluşturur) uygulama Galeriden bir uygulama eklediğinde
* Yeni bir uygulama oluşturmak için Microsoft Graph API, Azure AD Graph API veya PowerShell kullanma
* Diğer birçok çeşitli Geliştirici deneyimleri Azure hem de dahil olmak üzere API explorer Geliştirici merkezleri arasında deneyimleri

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Hizmet sorumluları nelerdir ve bunlar nereden geldiği?
[Hizmet sorumluları](active-directory-application-objects.md#service-principal-object) (hangi aracılığıyla yönetebilirsiniz [kurumsal uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) deneyimi) ne gerçekten Azure AD'ye bağlanma uygulama yöneten olan ve uygulama örneğini kabul edilebilir, Dizin. Verilen herhangi bir uygulama için bir "Giriş" dizinine kaydedilir) en fazla bir uygulama nesnesi (ve hangi davranır her dizin uygulamada örneklerini temsil eden bir veya daha fazla hizmet asıl nesneleri olabilir. 

Hizmet sorumlusu içerebilir:

* Bir başvuru geri uygulama kimliği özelliği aracılığıyla uygulama nesnesi
* Yerel kullanıcı ve Grup uygulama rol atamalarını kayıtları
* Uygulamayı yerel kullanıcı ve yönetici izinler kayıtları
  * Örneğin: uygulamanın belirli bir kullanıcının e-postasına erişmek izni
* Koşullu erişim ilkesi dahil olmak üzere yerel ilkeler kayıtları
* Bir uygulama için alternatif yerel ayarların kayıtları
  * Talep dönüştürme kuralları
  * Özellik eşlemeleri (kullanıcı hazırlama)
  * (Uygulama özel roller destekliyorsa) dizin özgü uygulama rolleri
  * Dizin özgü adı veya logosu

Uygulama nesneleri gibi hizmet asıl adı dahil olmak üzere birden çok yollarıyla oluşturulabilir:

* Bir üçüncü taraf uygulama için kullanıcıların oturum açma zaman Azure AD ile tümleşik
  * Oturum açma sırasında kullanıcılar profillerini erişmek için uygulama izni ve diğer izinler vermeniz istenir. İzin vermek için ilk kişi dizine eklenecek uygulamanın gösteren bir hizmet sorumlusu neden olur.
* Microsoft online services kullanıcılar oturum açtığında ister [Office 365](http://products.office.com/)
  * Office 365'e abone ya da bir deneme başlatmak, bir veya daha fazla hizmet asıl adı tüm Office 365 ile ilişkili işlevselliğini sağlamak için kullanılan çeşitli hizmetlere temsil eden dizinde oluşturulur.
  * SharePoint gibi bazı Office 365 Hizmetleri hizmet asıl adı iş akışları dahil olmak üzere bileşenleri arasında güvenli iletişim sağlamak için sürekli olarak oluşturun.
* Bir yönetici (Bu da uygulama nesnesini oluşturur) uygulama Galeriden bir uygulama eklediğinde
* Kullanmak için uygulama ekleme [Azure AD uygulama proxy'si](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* SAML ya da parola çoklu oturum açma (SSO) kullanarak çoklu oturum açma için uygulama Bağlan
* Programlı şekilde Azure AD Graph API veya PowerShell aracılığıyla

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Nasıl uygulama nesneleri ve hizmet asıl adı birbiriyle?
Bir uygulama bir uygulama nesnesi bir veya daha fazla hizmet asıl adı (uygulamanın giriş dizini dahil) burada çalışır dizinlerin her tarafından başvurulan kendi giriş dizininde bulunur.
![Birbirine ve Azure AD örneğinde uygulama nesneleri ve hizmet asıl adı nasıl etkileşime gösteren diyagram.][apps_service_principals_directory]

Önceki diyagramda iki dizini dahili olarak Microsoft'un (sol tarafta uygulamaları yayımlamak için kullandığı gösterilmiştir):

* Bir Microsoft Apps (Microsoft Hizmetleri dizini)
* Önceden tümleştirilmiş üçüncü taraf uygulamaları (uygulama Galerisi dizin) için bir tane

Uygulama yayımcıları/Azure AD ile tümleştirmek satıcıları (sağa "Bazı SaaS dizin" olarak gösterilir) yayımlama dizini gerek vardır.

Kendiniz eklediğiniz uygulama (olarak temsil **uygulama (sizin hesabınız)** diyagramdaki) içerir:

* Geliştirilmiş uygulamalara (Azure AD ile tümleşik)
* Uygulamaları bağlı için çoklu oturum açma
* Azure AD uygulama proxy'si kullanarak yayımlanan uygulama

### <a name="notes-and-exceptions"></a>Notlar ve özel durumlar
* Tüm hizmet asıl adı uygulama nesneyi geri gelin. Azure AD ilk olarak yapılandırıldığında uygulamalara sağlanan hizmetlerin daha kısıtlı ve hizmet sorumlusu uygulama kimliği oluşturmak için yeterli. Özgün hizmet sorumlusu için Windows Server Active Directory hizmet hesabı şeklinde yakın. Bu nedenle, bir uygulama nesnesi oluşturmadan Azure AD PowerShell kullanarak gibi farklı yollarla aracılığıyla hizmet asıl adı oluşturmak hala mümkündür. Azure AD grafik API'si, bir hizmet asıl oluşturmadan önce bir uygulama nesnesi gerektirir.
* Yukarıda açıklanan bilgiler tüm şu anda açık program aracılığıyla. Yalnızca kullanıcı Arabiriminde kullanılabilir şunlardır:
  * Talep dönüştürme kuralları
  * Özellik eşlemeleri (kullanıcı hazırlama)
* Uygulama nesneleri ve hizmet sorumlusu hakkında daha ayrıntılı bilgi için Azure AD grafik REST API başvuru belgelerine bakın:
  * [Uygulama](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Hizmet sorumlusu](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Neden uygulamaları Azure AD ile tümleştirme?
Bir veya daha fazla dahil olmak üzere sağladığı hizmetler yararlanmak için Azure AD ile uygulamaları eklendi:

* Uygulama kimlik doğrulama ve yetkilendirme
* Kullanıcı kimlik doğrulaması ve yetkilendirme
* Federasyon ya da parolanızı kullanan SSO
* Kullanıcı sağlama ve eşitleme
* Rol tabanlı erişim denetimi - bir uygulamada yetkilendirme denetimleri bağlı olarak rolleri gerçekleştirmek için uygulama rolleri tanımlamak için dizin kullanın
* API/kaynaklara erişim yetkisi vermek için Office 365 ve diğer Microsoft uygulamaları tarafından kullanılan OAuth yetkilendirme hizmetleri-
* Uygulama yayımlama ve proxy - uygulamaya özel bir ağ üzerinden Internet yayımlama

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kimin my Azure AD örneğinde uygulamaları ekleme iznine sahip mi?
Yalnızca genel Yöneticiler (uygulamalar uygulama Galerisi'nden ekleme ve uygulama proxy'sini kullanmak için bir uygulama yapılandırma gibi) varsayılan olarak dizininizdeki tüm kullanıcılara yapabildiğinizi bazı görevler uygulama kaydetmek için haklarınız varken nesneler geliştirme ve hangi uygulamaların bunlar paylaşım/erişim izni aracılığıyla kendi Kurumsal verilerine verin takdirine bağlı oldukları. Bir kişi bir uygulamada oturum açmak ve izin vermek için ilk kullanıcı dizininizde ise, kiracınızda bir hizmet sorumlusu oluşturur; Aksi takdirde, izin verme bilgi asıl varolan hizmet üzerinde depolanır.

Kullanıcıların kaydetmek ve uygulamaları kaybolabileceğini başlangıçta ses ile ilgili için onay ancak aşağıdakileri göz önünde bulundurun olanak tanır:


* Uygulamaları uygulamanın kayıtlı veya dizinde kayıtlı gerek kalmadan yıllardır için kullanıcı kimlik doğrulaması için Windows Server Active Directory yararlanamaz olmuştur. Artık kuruluş görünürlük kaç uygulamalar dizin tam olarak kullanıyor ve hangi amaçla geliştirilmiş.
* Bu Sorumluluklar kullanıcıları için temsilci bir yönetici tabanlı uygulama kaydı ve yayımlama işlemi için gereken üzerindeki geçersiz kılar. Active Directory Federasyon Hizmetleri (ADFS) olan bir yönetici kendi geliştiriciler adına bağlı olan taraf olarak bir uygulama eklemek sahip olabilir. Şimdi geliştiriciler Self Servis edebilirsiniz.
* İş amaçlı kuruluş hesaplarını kullanarak uygulamalarına oturum açan kullanıcılar iyi bir şeydir. Daha sonra kuruluş bırakırsanız otomatik olarak erişim için kullanmakta olduğunuz uygulama hesabında kaybederler.
* Hangi verilerin uygulama dâhil olduğu paylaşıldı kaydını sahip. Veri zamankinden daha aktarılabilir ve kimlerin hangi uygulamaları ile hangi verilerin paylaşılan Temizle kaydını olması yararlı olacaktır.
* Azure AD için OAuth kullanan API sahipleri tam olarak hangi izinleri kullanıcıların uygulamaları verebilir ve kabul etmek için bir yönetici izinleri gerektiren karar verin. Kullanıcı izni kullanıcıların kendi verilerini ve yetenekleri kapsamlıdır sırasında yalnızca yöneticileri büyük kapsamlar ve daha önemli izinleri onay.
* Bir kullanıcı ekler veya verilerine erişmek bir uygulama sağlar, bir uygulama dizinine nasıl eklendiğini belirlemek için Denetim raporları Azure portalındaki görüntüleyebilmeniz olay denetlenebilir.

Dizininizde kullanıcıların uygulamaları kaydetme ve yönetici onayı olmadan uygulamaları açmayı önlemek istiyorsanız, bu özellikler devre dışı bırakma değiştirebileceğiniz iki ayarı vardır:

* Kullanıcılar uygulamalara kendi adınıza onaylıyorsunuz önlemek için:
  1. Azure portalında Git [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) kurumsal uygulamalar bölümünde.
  2. Değişiklik **kullanıcıların şirket adına şirket verilerine erişen uygulamaları izin** için **Hayır**. 
     *Kullanıcı izni devre dışı bırakma karar verirseniz, bir yönetici bir kullanıcı herhangi bir yeni uygulama onayı gerekli olacağını unutmayın kullanması gerekir.*
* Kullanıcıların kendi uygulamalarını kaydetmesini önlemek için:
  1. Azure portalında Git [kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) Azure Active Directory altında bölümü
  2. Değişiklik **kullanıcıları uygulamaları kaydetme** için **Hayır**.

> [!NOTE]
> Microsoft kendisini uygulamalarını kaydetmek ve uygulamaları kendi adınıza sayılırsınız kullanıcılarla varsayılan yapılandırmayı kullanır.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg

