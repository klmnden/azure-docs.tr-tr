---
title: "Uygulamaları Azure Active Directory'ye nasıl eklenir."
description: "Bu makalede, Azure Active Directory örneğine uygulamaları nasıl eklenir açıklanmaktadır."
services: active-directory
documentationcenter: 
author: shoatman
manager: mtillman
editor: 
ms.assetid: 3321d130-f2a8-4e38-b35e-0959693f3576
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2016
ms.author: shoatman
ms.custom: aaddev
ms.openlocfilehash: 51ef7e554b6fd3764893f0fd35464088e42e49f8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Neden ve nasıl uygulamaları için Azure AD eklenir
Uygulamaların bir listesini, Azure Active Directory örneğinde görüntülerken başlangıçta puzzling özelliklerinden biri, uygulamaları nereden geldiğini ve orada bulunma anlamaktır.  Bu makalede uygulamaların nasıl dizininde sunulur ve nasıl dizininizde olması için bir uygulama gelen anlaşılmasına yardımcı olacak bağlamına sahip sağlayan bir üst düzey genel bakış sağlar.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Hangi hizmetlerin Azure AD uygulamaları sağlar?
Uygulamaları bir veya daha fazla sağladığı hizmetler yararlanmak için Azure AD ile eklenir.  Bu hizmetler şunlardır:

* Uygulama kimlik doğrulaması ve yetkilendirme
* Kullanıcı kimlik doğrulaması ve yetkilendirme
* Çoklu oturum Federasyon veya parola kullanarak açma (SSO)
* Kullanıcı sağlama ve eşitleme
* Rol tabanlı erişim denetimi; Bir uygulamada yetkilendirme denetimleri bağlı rolleri gerçekleştirmek için uygulama rolleri tanımlamak için dizin kullanın.
* oAuth yetkilendirme Hizmetleri (Office 365 ve diğer Microsoft uygulamaları tarafından API'leri/kaynaklara erişim yetkisi vermek için kullanılır.)
* Uygulama yayımlama & proxy; Özel bir ağ üzerinden internet uygulama yayımlama

## <a name="how-are-applications-represented-in-the-directory"></a>Nasıl uygulamaları dizininde sunulur?
2 nesneleri kullanarak Azure AD'de uygulamalar temsil edilen: uygulama nesnesi ve bir hizmet sorumlusu nesnesi.  "Home" kayıtlı bir uygulama nesnesi / "sahip" veya "yayımlama" dizini ve bir veya daha içinde davranır her dizin uygulamada temsil eden asıl nesneleri hizmet.  

Uygulama Azure ad uygulama nesnesi açıklar (çok kiracılı hizmeti) ve aşağıdakilerden herhangi birini içerebilir: (*Not*: Bu kapsamlı bir liste değildir.)

* Adı, logosu ve yayımcı
* Gizli (uygulama kimliğini doğrulamak için kullanılan simetrik ve/veya asimetrik anahtarlar)
* API bağımlılıkları (oAuth)
* API/kaynakları/yayımlanan kapsamları (oAuth)
* Uygulama rolleri (RBAC)
* SSO meta verileri ve yapılandırma (SSO)
* Kullanıcı meta verileri ve yapılandırma hazırlama
* Proxy meta verileri ve yapılandırma

Hizmet sorumlusu burada kendi ana dizini de dahil olmak üzere uygulama davranır her dizin uygulamada kaydıdır.  Hizmet sorumlusu:

* Uygulama Kimliği özelliği aracılığıyla uygulama nesnesini geri başvurduğu
* Kayıtları yerel kullanıcı ve Grup uygulama rol atamaları
* Yerel kullanıcı ve yönetici izinleri verilmiş uygulamaya kaydeder
  * Örneğin: uygulamanın belirli kullanıcılara e-postasına erişmek için izni
* Koşullu erişim ilkesi dahil olmak üzere kayıtları yerel ilkeler
* Kayıtları yerel alternatif yerel ayarları bir uygulama için
  * Talep dönüştürme kuralları
  * Özellik eşlemeleri (kullanıcı hazırlama)
  * (Uygulama özel roller destekliyorsa) belirli uygulama rolleri Kiracı
  * Ad/logosu

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Uygulama nesneleri ve hizmet asıl adı dizinler genelinde diyagramı
![Nasıl uygulama nesneleri ve Azure AD örnekleriyle varolan ilkeleri hizmet gösteren diyagram.][apps_service_principals_directory]

Yukarıdaki diyagramda görüldüğü gibi.  Microsoft'un iki dizini uygulamaları yayımlamak için kullandığı dahili (sol tarafta).

* Bir Microsoft Apps (Microsoft Hizmetleri dizini)
* Önceden tümleştirilmiş 3 taraf uygulamaları (uygulama Galerisi dizin) için bir tane

Azure AD ile tümleştirmek uygulama yayımcıları/satıcıları yayımlama dizini gerek vardır.  (Bazı SAAS dizin).

Kendinizi ekleyin uygulamalar şunlardır:

* Geliştirilmiş uygulamalara (AAD ile tümleşik)
* Uygulamaları bağlı için çoklu oturum açma
* Azure AD uygulama proxy'si kullanılarak yayımlanan uygulamalar.

### <a name="a-couple-of-notes-and-exceptions"></a>Birkaç notlar ve özel durumlar
* Tüm hizmet asıl adı uygulama nesneleri için geri gelin.  Huh? Azure AD ilk olarak yapılandırıldığında uygulamalara sağlanan hizmetlerin çok daha kısıtlı ve hizmet sorumlusu bir uygulama kimliği oluşturmak için yeterli.  Özgün hizmet sorumlusu için Windows Server Active Directory hizmet hesabı şeklinde yakın.  Bu nedenle hala uygulama nesnesi oluşturmadan Azure AD PowerShell kullanarak hizmet sorumluları oluşturmak mümkündür.  Grafik API'si, bir hizmet asıl oluşturmadan önce bir uygulama nesnesi gerektirir.
* Yukarıda açıklanan bilgiler tüm şu anda açık program aracılığıyla.  Yalnızca kullanıcı Arabiriminde kullanılabilir şunlardır:
  * Talep dönüştürme kuralları
  * Özellik eşlemeleri (kullanıcı hazırlama)
* İçin hizmet sorumlusu hakkında ayrıntılı bilgi ve uygulama nesneleri Lütfen Azure AD grafik REST API Başvurusu belgelerine başvurun.  *İpucu*: Azure AD Graph API belgesidir şema başvurusu için en yakın bir şey için şu anda Azure AD.  
  * [Uygulama](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#application-entity)
  * [Hizmet sorumlusu](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#serviceprincipal-entity)

## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Nasıl uygulamaları my Azure AD örneğinde eklenir?
Bir uygulama için Azure AD eklenebilir birçok yolu vardır:

* Uygulama Ekle [Azure Active Directory Uygulama Galerisi](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Oturum/taraf uygulama Azure Active Directory ile tümleşik 3 halinde (örneğin: [Smartsheet](https://app.smartsheet.com/b/home) veya [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
  * Yukarı/kullanıcılar oturum sırasında profillerini erişim izni uygulama ve diğer izinler vermeniz istenir.  İzin vermek için ilk kişi dizine eklenecek uygulama temsil eden bir hizmet sorumlusu neden olur.
* Oturum/Microsoft çevrimiçi hizmetlerine ister [Office 365](http://products.office.com/)
  * Ne zaman Office 365'e abone veya bir veya daha fazla hizmet asıl adı tüm Office 365 ile ilişkili işlevselliğini sağlamak için kullanılan çeşitli hizmetlere temsil eden dizin oluşturulan bir deneme başlatmak.
  * SharePoint gibi bazı Office 365 Hizmetleri hizmet asıl adı iş akışları dahil olmak üzere bileşenleri arasında güvenli iletişim sağlamak için devam eden düzenli olarak oluşturun.
* Azure Yönetim Portalı bakın, geliştirmekte bir uygulama ekleyin: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Visual Studio kullanarak geliştirirken bir uygulama görmek ekleyin:
  * [ASP.Net kimlik doğrulama yöntemleri](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
  * [Bağlı hizmetler](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Kullanmak için bir uygulama ekleyin [Azure AD uygulama proxy'si](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* SAML ya da parola SSO kullanma tek oturum için bir uygulamaya Bağlan
* Diğer birçok Azure içinde çeşitli Geliştirici deneyimleri de dahil olmak üzere ve/içinde API'si Gezgini Geliştirici merkezleri arasında deneyimleri

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kimin my Azure AD örneğinde uygulamaları ekleme iznine sahip mi?
Yalnızca genel yöneticiler şunları yapabilir:

* Azure AD uygulama galerisinde (önceden tümleştirilmiş 3 taraf uygulamaları) uygulama ekleme
* Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama

Dizininizdeki tüm kullanıcılara, geliştirmekte olduğunuz uygulamalar ve hangi uygulamaların bunlar paylaşım/erişim kuruluş verilerini verin tedbirli eklemek için haklarınız.  *Bir uygulama için kullanıcı oturum yukarı/unutmayın ve izinleri verme, bir hizmet sorumlusu oluşturuluyor neden olabilir.*

Bu başlangıçta ses ilgili olabilir, ancak aşağıdaki aklınızda tutun:

* Uygulamalar, uygulamanızın dizinde kayıtlı/kaydedilen gerek kalmadan yıllardır için kullanıcı kimlik doğrulaması için Windows Server Active Directory yararlanamaz olmuştur.  Artık kuruluş dizin ve neler için kaç tane uygulamaları tam olarak kullanmaya görünürlük geliştirilmiş.
* Uygulama yayımlama/kayıt sürecine kullanımlı yönetici gerek yoktur.  Active Directory Federasyon Hizmetleri ile bir yönetici olarak bir bağlı olan taraf geliştiriciler adına bir uygulama eklemek sahip olabilir.  Şimdi geliştiriciler Self Servis edebilirsiniz.
* Oturum/iş amaçlı kuruluş hesaplarını kullanarak uygulamaları kadar kullanıcılar iyi bir şeydir.  Daha sonra kuruluş bırakırsanız kullanmakta olduğunuz uygulamada kendi hesabına erişimi kaybedersiniz.
* Hangi verilerin uygulama dâhil olduğu paylaşıldı kaydını sahip.  Veri zamankinden daha aktarılabilir ve kimlerin hangi uygulamaları ile hangi verilerin paylaşılan Temizle kaydını sahip yararlıdır.
* Azure AD için oAuth kullanan uygulamalar, kullanıcıların uygulamaları verebilir ve kabul etmek için bir yönetici izinleri gerektiren tam olarak hangi izinlerin karar verin.  Yalnızca Yöneticiler büyük kapsamlar ve daha önemli izinleri onayı olduğunu bildiren olmadan tamamlamalıdır.
* Bir uygulama dizinine nasıl eklendiğini belirlemek için Denetim raporları Azure Yönetimi portalı içinde görüntüleyebilmeniz için kullanıcıları ekleme ve uygulamaların verilerine erişmesini sağlayan olayları denetlendi.

**Not:** *Microsoft kendisini işletim şimdi birçok ay için varsayılan yapılandırmayı kullanarak.*

Tüm bu uygulamaları eklemesini dizininizdeki kullanıcılara önlemek mümkündür ve hangi bilgilerin tedbirli kullanan uygulamalarla Azure Yönetim Portalı'nda Dizin yapılandırma değiştirerek paylaştıkları belirtti.  Aşağıdaki yapılandırma, Azure Yönetim Portalı, dizinin "Yapılandırma" sekmesinde içinde erişilebilir.

![Tümleşik uygulama ayarlarını yapılandırma kullanıcı arabiriminin ekran görüntüsü][app_settings]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
Azure ad uygulamaları ekleme ve uygulamalar için hizmetlerin nasıl yapılandırılacağı hakkında daha fazla bilgi edinin.

* Geliştiricileri: [bir uygulamanın AAD ile tümleştirmek öğrenin](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Geliştiricileri: [github'da Azure Active Directory ile tümleşik uygulamalar için örnek kod gözden geçirme](https://github.com/AzureADSamples)
* Geliştiriciler ve BT uzmanlarının: [Azure Active Directory grafik API'si için REST API belgelerini gözden geçirin](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* BT uzmanları: [Azure Active Directory önceden tümleştirilmiş uygulamaların uygulama Galeriden kullanmayı öğrenin](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* BT uzmanları: [Bul belirli önceden tümleştirilen uygulamalar yapılandırma öğreticileri](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* BT uzmanları: [Azure Active Directory Uygulama Ara sunucusu kullanarak uygulama yayımlama öğrenin](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:../media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
