---
title: Kimlik gereksinimleri için karma kimlik tasarım Azure cloud | Microsoft Docs
description: Karma kimlik tasarımı gereksinimlerini tanımlamak için neden olan şirketin kendi iş ihtiyaçları belirleyin.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ecc90e13f49c231d8d3ab0cff1de91443b80f21
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65950905"
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için kimlik gereksinimleri belirleme
Karma kimlik çözümü tasarlamanın ilk adımı, bu çözüm yararlanarak iş kuruluş gereksinimlerini belirlemektir.  Karma kimlik (diğer tüm bulut çözümleri kimlik sağlayarak destekler) destekleyen bir rol olarak başlar ve kullanıcılar için yeni iş yüklerini kilidini yeni ve ilginç özellikler sağlamak için geçer.  Bu iş yükleri veya kullanıcılarınız için benimsemek için istediğiniz hizmetleri karma kimlik tasarımı gereksinimlerini gösterecektir.  Bu hizmetlerin ve iş yüklerini karma kimlik hem şirket içi gerekir ve bulut.  

Bu anahtar ne olduğunu anlamak için iş yönlerinin üzerinde gerek artık bir gereksinim ve şirket geleceğe yönelik planlarının ne. Karma kimlik tasarımı için uzun vadeli stratejisi görünürlüğünü yoksa büyür ve değişirken iş gereksinimleriniz değiştikçe çözümünüzü ölçeklenebilir olmayacağını yüksektir. Aşağıdaki diyagramda karma kimlik mimarisi ve kullanıcılar için kilidi iş yükleri bir örneği gösterilmektedir. Yalnızca örnek kilidi açılabilir ve kesintisiz karma kimlik stratejisi ile sunulan tüm yeni olasılık budur. 

Karma kimlik mimarisi parçası olan bazı bileşenleri ![karma kimlik mimarisi](./media/plan-hybrid-identity-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>İş gereksinimlerini belirleme
Bu şirketler aynı sektörde, gerçek iş gereksinimleri değişebilir bir parçası olsa bile, her şirketin farklı gereksinimleri vardır. Sektördeki en iyi yine yararlanabilirsiniz, ancak sonuçta karma kimlik tasarımı gereksinimlerini tanımlamak için neden olan şirketin kendi iş ihtiyaçları olacaktır. 

İş ihtiyaçlarınızı belirlemek için aşağıdaki soruları yanıtlamak emin olun:

* Şirketinizin BT operasyonel maliyetlerini kesmek için mi arıyorsunuz?
* Şirketiniz bulut varlıklarını (SaaS uygulamalarını, altyapı) güvenli hale getirmek için mi arıyorsunuz?
* Şirketinizin BT modernize etme mi arıyorsunuz?
  * Kullanıcılarınızın daha taşınabilir ve zorlu olan trafiği farklı kaynaklara erişmek için farklı türde izin vermek için DMZ'NİZDE özel durumları oluşturmak için BT'nin?
  * Şirketiniz bu modern kullanıcılara yayımlanacak gerekiyordu ancak yeniden kolay olmayan eski uygulamaları var mı?
  * Şirketiniz bu görevleri gerçekleştirmek ve denetimi altındaki aynı anda getirin gerekiyor mu?
* Şirketiniz kullanıcıların kimliklerini güvenliğini sağlamak ve Microsoft'un Azure güvenlik uzmanlığı şirket içi deneyimden yararlanın yeni araçları getirerek riskini azaltmak için mi arıyorsunuz?
* Şirketiniz, şirket içi dreaded "dış" hesaplarının kurtulun ve şirket içi ortamınızı içindeki etkin olmayan tehdit artık nerede buluta taşımak çalışıyor?

## <a name="analyze-on-premises-identity-infrastructure"></a>Şirket içi kimlik altyapınızı analiz edin
Şirket iş gereksinimlerinizi ile ilgili bir fikriniz olduğuna göre şirket içi kimlik altyapınızı değerlendirin gerekir. Bu değerlendirme sürümünü geçerli kimlik çözümünüze bulut kimlik yönetimi sistemi tümleştirmek için teknik gereksinimlerini tanımlamak için önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Hangi kimlik doğrulama ve yetkilendirme çözümü şirketiniz tarafından sağlanan şirket içi kullanma? 
* Şirketiniz herhangi bir şirket içi eşitleme hizmeti şu anda var mı?
* Şirketiniz tüm üçüncü taraf kimlik sağlayıcıları (IDP) kullanıyor mu?

Ayrıca şirketinizin sahip bulut hizmetlerini dikkat etmeniz gerekir. SaaS, Iaas veya PaaS modeli ortamınızdaki geçerli tümleştirmesiyle anlamak için bir değerlendirme gerçekleştiriliyor çok önemlidir. Bu değerlendirme sırasında aşağıdaki soruları yanıtlayan emin olun:

* Şirketiniz herhangi bir bulut hizmeti sağlayıcısı tümleştirmesiyle var mı?
* Yanıt Evet ise, hangi hizmetlerin kullanılıyor?
* Bu tümleştirme, şu anda üretim ortamında olduğundan veya pilot bir uygulamadır?

> [!NOTE]
> Microsoft Cloud App Security bulut uygulaması içeren kataloğuna göre derecelendirilmiş ve puanlanmış uygulamaları 70'ten fazla risk unsurlarına göre 16. 000'den fazla bulut trafik günlüklerinizi cloud Discovery'yi çözümler, sağlamak için bulut sürekli görünürlük kullanın, gölge BT ve riski Gölge BT tümleştirilmesi kuruluşunuz. Başlatılan bakın almak için [Cloud Discovery'yi ayarlama](/cloud-app-security/set-up-cloud-discovery).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Kimlik Tümleştirme gereksinimlerini değerlendirin
Ardından, kimlik Tümleştirme gereksinimlerini değerlendirmek gerekir. Bu değerlendirme, kullanıcıların kimliğini nasıl doğrulayacağınızı, kuruluşun varlığı bulutta nasıl görüneceğini, kuruluş yetkilendirme nasıl izin verir ve kullanıcı deneyimini olarak neler olduğunu teknik gereksinimlerini tanımlamak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Kuruluşunuz, Federasyon, standart kimlik doğrulama veya her ikisi de kullanacaksınız?
* Federasyon bir gereksinimi var mı?  Aşağıdaki nedeniyle:
  * Kerberos tabanlı SSO
  * Şirketiniz SAML veya benzer Federasyon özelliklerini kullanır (ya da şirket içi veya 3 taraf yerleşik) bir şirket içi uygulamalara sahiptir.
  * Mfa'yı akıllı kartlar ile. RSA Securıd, vb.
  * Aşağıdaki soruları istemci erişim kuralları:
    1. Ben, Office 365 istemci IP adresine göre tüm dış erişimi engelleyebilir miyim?
    2. Ben, Office 365, Exchange ActiveSync dışında tüm dış erişimi engelleyebilir miyim?
    3. Ben, Office 365 (OWA, SPO) tarayıcı tabanlı uygulamalar dışındaki tüm dış erişimi engelleyebilir miyim
    4. Belirtilen AD gruplarının üyeleri için Office 365 tüm dış erişimi engelleyebilir miyim
* Güvenlik ve denetim konuları
* Federe kimlik doğrulaması yatırım zaten var
* Hangi adını kuruluşumuz için bulut etki alanımızda kullanacak mısınız?
* Kuruluşunuzun özel etki alanı var mı?
  1. Bu etki alanı, ortak ve DNS üzerinden kolayca doğrulanabilir mi?
  2. Ardından değilse, alternatif bir UPN AD'de kaydetmek için kullanılan genel etki alanı gerekiyor?
* Kullanıcı tanımlayıcıları için bulut gösterimi tutarlı mı? 
* Kuruluş bulut Hizmetleri ile tümleştirme gerektiren uygulamalar var mı?
* Kuruluş birden çok etki alanına sahip ve tüm standart ya da Federasyon kimlik doğrulamasını kullanır?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Ortamınızda çalışan uygulamaları değerlendirin
Şirket içi ile ilgili bir fikriniz ve bulut altyapılarına göre bu ortamlarda çalışan uygulamaları değerlendirilecek gerekir. Bu değerlendirme, şu uygulamalar için bulut kimlik yönetimi sistemi tümleştirmek için teknik gereksinimleri tanımlamak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Burada uygulamalarımızın mi Barınıyor?
* Şirket içi uygulamalara erişen kullanıcıların?  Bulutta? Veya her ikisini birden mi?
* Mevcut uygulama iş yüklerini alabilir ve bunları buluta taşımak için plan var mı?
* Kullanacağınız bulutta Bulut veya şirket içinde bulunan yeni uygulamalar geliştirmek için planlar vardır kimlik doğrulaması?

## <a name="evaluate-user-requirements"></a>Kullanıcı gereksinimlerini değerlendirin
Ayrıca kullanıcı gereksinimlerini değerlendirmek vardır. Bu değerlendirme kolaylaşmasına ve bunlar buluta geçiş gibi kullanıcılara yardım için gerekli adımları tanımlamak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* Kullanıcılar, şirket içi uygulamaları erişecek?
* Kullanıcılar buluttaki erişecek?
* Kullanıcılar genellikle bunu nasıl oturum açma, şirket içi ortama?
* Nasıl kullanıcılar buluta oturum?

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md) seçeneklerini ve her bir seçeneğin olumlu/olumsuz üzerinden geçer.  En iyi seçeneği işletmenizi uygun seçersiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Dizin eşitleme gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

