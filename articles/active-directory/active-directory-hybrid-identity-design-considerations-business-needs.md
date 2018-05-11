---
title: Kimlik gereksinimleri için karma kimlik tasarımı Azure bulut | Microsoft Docs
description: Karma kimlik tasarımı gereksinimlerini tanımlamak için yol şirketin işletme gereksinimlerini tanımlama.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 5741a5024b5f5105a71d9404191601b951a301e4
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünü kimlik gereklilikleri
Karma kimlik çözümü tasarlamanın ilk adımı, bu çözüm yararlanarak iş kuruluş gereksinimlerini belirlemektir.  Karma kimlik (diğer tüm bulut çözümleri kimlik doğrulaması sağlayarak destekler) destekleyen bir rol olarak başlatılır ve kullanıcılar için yeni iş yüklerine kilidini yeni ve ilginç yetenekleri sağlamak için geçer.  Bu iş yükleri veya kullanıcılarınız için benimsemeyi istediğiniz hizmetleri karma kimlik tasarım gereksinimleri benimsendiği belirler.  Her iki şirket içi karma kimlik yararlanmak bu hizmetleri ve iş yükleri gerekir ve bulutta.  

İş ne olduğunu anlamak için bu önemli yönlerinin üzerinde gitmeniz şimdi bir gereksinim ve şirket geleceğe yönelik planlarının ne. Karma kimlik tasarımı için uzun vadeli stratejisi görünürlüğünü yoksa, işleriniz büyür ve değişirken iş gereksinimleri değişirken çözümünüzün ölçeklenebilir olmayacağını kalabilirsiniz.   T bir karma kimlik mimarisi ve kullanıcılar için kilidi iş yükleri örneği kendisinin diyagram gösterilmektedir. Bu yalnızca kilidi açılabilir ve düz karma kimlik stratejisi ile sunulan tüm yeni olanaklar örneğidir. 

Karma kimlik mimarisi parçası olan bazı bileşenleri ![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>İş gereksinimlerini belirleme
Bu şirketler aynı sektörde, gerçek iş gereksinimleri farklılık gösterebilir yer alsa bile her şirketin farklı gereksinimleri vardır. Sektörün en iyi yöntemlerinden yine yararlanabilirsiniz, ancak sonuç olarak, karma kimlik tasarımı gereksinimlerini tanımlamak için yol açacaktır şirketin iş ihtiyaçları olacaktır. 

İş gereksinimlerinizi belirlemek için aşağıdaki soruları yanıtlamak emin olun:

* Şirketinizin BT işletim maliyeti kesme istiyorsunuz?
* Şirketiniz bulut varlıklar (SaaS uygulamaları, altyapı) güvenli hale getirmek için istiyorsunuz?
* Şirketinizin BT modernize mı planlıyorsunuz?
  * Kullanıcılarınızın daha hareketli ve yoğun olan farklı tür farklı kaynaklara erişmek için trafiğe izin çevre ağınız içine özel durum oluşturmak için BT?
  * Şirketiniz bu modern kullanıcılara yayımlanmasını gerekli ancak yeniden kolay olmayan eski uygulamalar var mı?
  * Şirketiniz bu görevleri gerçekleştirmek ve aynı anda denetiminde hale getirmek gerekiyor mu?
* Şirketinizin, kullanıcıların kimliklerini güvenli ve Microsoft'un Azure güvenlik uzmanlık şirket içi uzmanlığa yararlanan yeni araçları getirerek riskini azaltmak için istiyorsunuz?
* Şirket içi dreaded "dış" hesapları elden ve şirket içi ortamınız içinde etkinliği olmayan bir tehdit artık nerede buluta taşımak çalışıyor?

## <a name="analyze-on-premises-identity-infrastructure"></a>Şirket içi kimlik altyapınızı Çözümle
Şirket iş gereksinimlerinizi bir fikir sahip olduğunuza göre şirket içi kimlik altyapınızı değerlendirmeniz gerekir. Bu değerlendirme, geçerli kimlik çözümünüze bulut kimlik yönetimi sistemi tümleştirmek için teknik gereksinimleri tanımlamak için önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Hangi kimlik doğrulama ve yetkilendirme çözümü şirketiniz tarafından sağlanan şirket içi kullanma? 
* Şirket içi eşitleme hizmetlerin şu anda var mı?
* Şirketiniz tüm üçüncü taraf kimlik sağlayıcıları (IDP) kullanıyor mu?

Ayrıca şirketinizin olabilir bulut hizmetlerin farkında olmanız gerekir. Geçerli tümleştirme, ortamınızdaki SaaS, Iaas veya PaaS modellerle anlamak için bir değerlendirme gerçekleştirme çok önemlidir. Bu değerlendirme sırasında aşağıdaki soruları yanıtlayın emin olun:

* Şirketiniz herhangi bir bulut hizmeti sağlayıcısı ile tümleştirme var mı?
* Yanıt Evet ise, hangi hizmetlerin kullanılıyor?
* Bu tümleştirme şu anda üretimde olan yoksa bir pilot mı?

> [!NOTE]
> Tüm uygulamalarınızın doğru bir eşleme varsa ve bulut Hizmetleri yok, Cloud App Discovery aracını kullanabilirsiniz. Bu araç, BT departmanınızın tüm kuruluşunuzun iş ve tüketici bulut uygulamalarını görünürlük sağlayabilirsiniz. Bu, kullanım desenleri ve bulut uygulamalarınıza erişen tüm kullanıcılar ile ilgili ayrıntılar dahil olmak üzere kuruluşunuzdaki gölge BT uygulamalarını bulmayı her zamankinden daha kolay hale getirir. Başlatılan bakın almak için [Cloud app discovery](manage-apps/cloud-app-discovery.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Kimlik Tümleştirme gereksinimlerini değerlendirin
Ardından kimlik Tümleştirme gereksinimlerini değerlendirmek gerekir. Bu değerlendirme nasıl kullanıcılar kimlik doğrulaması, kuruluşun varlığı bulutta nasıl görüneceğine, kuruluşunuzun yetkilendirme nasıl izin verecek ve hangi kullanıcı deneyimini olacağını teknik gereksinimlerini tanımlamak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Kuruluşunuz, Federasyon, standart kimlik doğrulama veya her ikisini kullanacaksınız?
* Federasyon bir gereksinimdir?  Aşağıdaki nedeniyle:
  * Kerberos tabanlı SSO
  * Şirketiniz SAML veya benzer Federasyon yeteneklerini kullanır (ya da şirket içi veya 3 taraf yerleşik) bir şirket içi uygulamalara sahiptir.
  * Akıllı kartlar ile MFA. RSA Securıd, vb.
  * Aşağıdaki sorular adres istemci erişim kuralları:
    1. Office 365 istemci IP adresine göre tüm dış erişimi engelleyebilir miyim?
    2. Office 365, Exchange ActiveSync dışında tüm dış erişimi engelleyebilir miyim?
    3. Tarayıcı tabanlı uygulamalar (OWA, SPO) dışında Office 365 tüm dış erişimi engelleyebilir miyim
    4. Belirtilen AD gruplarının üyeleri için Office 365 tüm dış erişimi engelleyebilir miyim
* Güvenlik ve denetim konuları
* Federe kimlik doğrulaması yatırım zaten var
* Hangi ad bulutta bizim etki alanı için kuruluşunuzun kullanacak mısınız?
* Kuruluşunuz özel bir etki alanı var mı?
  1. Bu etki alanı, ortak ve DNS aracılığıyla kolayca doğrulanabilen nedir?
  2. Değilse, alternatif bir UPN AD kaydetmek için kullanılan genel etki alanı var mı?
* Kullanıcı tanımlayıcıları için bulut gösterimi tutarlı misiniz? 
* Kuruluş, bulut Hizmetleri ile tümleştirme gerektiren uygulamalar var mı?
* Kuruluş birden çok etki alanına sahip ve tüm standart ya da federe kimlik doğrulamasını kullanır?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Ortamınızda çalışan uygulamaları değerlendir
Şirket içi bir fikir sahip ve altyapı bulut göre bu ortamlarda çalışan uygulamaları değerlendirmeniz gerekir. Bu değerlendirme, bulut kimlik yönetimi sistemi için bu uygulamaları tümleştirmek için teknik gereksinimleri tanımlamak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Burada bizim uygulamaları Canlı?
* Kullanıcıların şirket içi uygulamalara erişecek?  Bulutta? Veya her ikisini birden mi?
* Mevcut uygulama iş yükleri almak ve bunları buluta taşımak için planları var mı?
* Hem şirket içinde bulunan veya kullanacağı bulutta bulut yeni uygulamaları geliştirmek için planları vardır kimlik doğrulaması?

## <a name="evaluate-user-requirements"></a>Kullanıcı gereksinimlerini değerlendirin
Ayrıca kullanıcı gereksinimlerini değerlendirmeniz gerekir. Bu değerlendirme devreye alma ve buluta geçiş olarak kullanıcılara yardım için gerekli adımları tanımlamak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Kullanıcılar uygulamaları şirket içi erişecek?
* Kullanıcıların bulut uygulamalarında erişecek?
* Nasıl kullanıcıların genellikle yerine kendi şirket içi ortamına oturum açma?
* Nasıl kullanıcılar oturum buluta açma?

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıtlama gereksinimlerini belirlemek](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) seçenekleri ve her seçeneğin olumlu/eksilerini üzerinden geçer.  Seçeneği, iş gereksinimlerinize en uygun belirleyeceksiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Dizin eşitleme gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

