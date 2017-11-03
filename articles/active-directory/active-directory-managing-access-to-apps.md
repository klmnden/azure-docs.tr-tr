---
title: "Azure AD kullanarak uygulamalara erişimi yönetme | Microsoft Docs"
description: "Nasıl Azure Active Directory her kullanıcı erişimi olan uygulamaları belirtmek kuruluşların açıklar."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: b0829f18-9e57-4107-925d-5f0457d81671
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 46e001b440802e0d5d16b7cf75344c7b9ce6fad3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="managing-access-to-apps"></a>Uygulamalara erişimi yönetme
Devam eden erişim yönetimi, kullanım değerlendirme ve raporlama bir uygulama, kuruluşunuzun kimlik sisteme tümleştirildikten sonra bir sınama devam etmektedir. Çoğu durumda, BT yöneticileri veya Yardım Masası sahip uygulamalarınıza erişimi yönetme bir devam eden etkin rol gerçekleştirilecek. Bazı durumlarda, atama genel veya bölümsel BT ekibi tarafından gerçekleştirilir. Genellikle, atama karar onaylarını BT yapar önce gerektiren iş karar veren temsilci amaçlanmıştır atama.  Varolan otomatik kimlik ve erişim yönetimi sistemi kullanarak, rol tabanlı erişim denetimi (RBAC) veya öznitelik tabanlı erişim denetimi (ABAC) gibi tümleştirme, diğer kuruluşların yatırım. Tümleştirme ve kural geliştirme özelleştirilmiş ve pahalı olma eğilimindedir. İzleme veya raporlama ya da Yönetim yaklaşımını kendi ayrı, pahalı ve karmaşık bir yatırımdır.

## <a name="how-does-azure-active-directory-help"></a>Azure Active Directory nasıl yardımcı olur?
 Azure AD, kolayca temsilci aracılığıyla ve yönetici yönetimi dahil olmak üzere otomatik, öznitelik tabanlı atama (ABAC veya RBAC senaryolarında) arasında değişen doğru erişim ilkeleri elde etmek kuruluşların etkinleştirme, yapılandırılan uygulamalar için kapsamlı erişim yönetimini destekler. Azure AD ile karmaşık ilkeleri, tek bir uygulama için birden fazla yönetim modeli birleştirme kolayca elde edebilirsiniz ve hatta Yönetim kuralları aynı izleyicileri uygulamalarla arasında yeniden kullanabilirsiniz.

* [Uygulamaların mevcut veya yeni ekleme](active-directory-sso-integrate-saas-apps.md)

 Azure AD uygulama atama iki birincil atama modlarını odaklanır:

* **Tek tek atama** directory genel yönetici izinlerine sahip bir BT yöneticisi bireysel kullanıcı hesapları seçin ve uygulama için erişim verin.
* **Grup tabanlı atama (yalnızca Azure AD Ücretli)** directory genel yönetici izinlerine sahip bir BT yöneticisi, uygulamaya bir grup atayabilir. Belirli kullanıcıların erişimini grubunun üyeleri, bunlar uygulama erişme girişimi zaman olup olmadıklarının tarafından belirlenir. Diğer bir deyişle, bir yönetici etkili bir şekilde "herhangi bir geçerli üye atanmış olan Grup, uygulama erişimi" belirten bir atama kuralı oluşturabilirsiniz. Bu atama seçeneğini kullanarak, yöneticiler de dahil olmak üzere, Azure AD Grup Yönetim seçeneklerinden birini yararlanabilir [öznitelik tabanlı dinamik grupların](active-directory-accessmanagement-manage-groups.md), dış sistem grupları (örneğin, şirket içi Active Directory veya Workday) veya yönetici tarafından yönetilen veya self-servis yönetilen grupları. Tek bir grup, birden fazla uygulama için genel yönetim karmaşıklığını azaltması atama benzeşim uygulamalarla atama kurallarının paylaşabilirsiniz sağlama kolayca atanabilir. Lütfen unutmayın. Bu iç içe geçmiş grup üyelikleri grup tabanlı atama uygulamalar için şu anda desteklenmiyor.

Bu iki atama modlarını kullanarak, Yöneticiler tüm arzu atama yönetim yaklaşım elde edebilirsiniz.

Azure AD ile kullanım ve atama raporlama tam olarak tümleşiktir, kolayca ataması durumu, atama hataları ve hatta kullanım bildirmek Yöneticiler etkinleştirme.

## <a name="complex-application-assignment-with-azure-ad"></a>Azure AD ile karmaşık bir uygulama atama
Salesforce gibi bir uygulamayı göz önünde bulundurun. Birçok kuruluşta, Salesforce öncelikle pazarlama ve satış kuruluşlar tarafından kullanılır. Satış ekibinin üyeleri Sınırlı erişime ancak genellikle, pazarlama ekibinin üyeleri yüksek oranda Salesforce, ayrıcalıklı erişimi olan. Çoğu durumda, bilgi çalışanı geniş bir popülasyonun bir kısıtlı erişim uygulama. Bu kurallar için özel durumlar da zorlaştırabilir. Genellikle, bir kullanıcı erişimi vermek veya rollerine genel kurallar bağımsız olarak değiştirmek için pazarlama veya satış liderlik takımlar prerogative olur.

Azure AD ile uygulamaları Salesforce gibi tek oturum açma (SSO) ve otomatik sağlama için önceden yapılandırılmış olabilir. Uygulama yapılandırıldıktan sonra bir yönetici oluşturmak ve uygun gruplara atamak için bir kerelik eylem alabilir. Bu örnekte, yönetici aşağıdaki atamaları yürütebilir:

* [Dinamik grupların](active-directory-accessmanagement-manage-groups.md) otomatik olarak pazarlama ve satış takımları bölüm ya da rolü gibi özniteliklere kullanarak tüm üyeleri göstermek için tanımlanan:
  
  * Pazarlama grupların tüm üyelerinin Salesforce "Pazarlama" role atanması
  * Grupları Salesforce "Satış" role atanması satış ekibinin tüm üyeleri. Daha fazla iyileştirme farklı Salesforce rollere atanan bölgesel satış ekipleri temsil eden birden çok gruplarını kullanabilirsiniz.
* Özel durum mekanizması etkinleştirmek için her rol için bir Self Servis Grup oluşturulamadı. Örneğin, "Özel durum Salesforce pazarlama" grubu Self Servis grup olarak oluşturulabilir. Grup Salesforce pazarlama rolüne atanabilir ve pazarlama liderlik ekibindeki sahipleri yapılabilir. Pazarlama liderlik takım üyeleri ekleyin veya kullanıcıları Kaldır, bir birleşim İlkesi ayarlamak bile onaylamak veya katılmak için tek tek kullanıcılar istekleri reddetmesini. Bu, özel eğitim sahipleri veya üyeleri için gerektirmez bir bilgi çalışanı uygun deneyimi aracılığıyla desteklenir.

Bu durumda, kendi rol ataması Salesforce'ta güncelleştirilmesi farklı gruplara eklenen tüm atanan kullanıcılar Salesforce için otomatik olarak sağlanan. Kullanıcıları bulmak ve Salesforce yoluyla Microsoft uygulama erişim paneli, Office web istemcileri veya hatta kendi kuruluş Salesforce oturum açma sayfasına giderek erişebilir olacaktır. Yöneticilerin, kolayca kullanarak Azure AD raporlama kullanım ve atama durumunu görüntülemek gerçekleştirebilir.

Yöneticiler işe [Azure AD koşullu erişimi](active-directory-conditional-access.md) belirli rol için erişim ilkeleri ayarlamak için. Bu ilkeler, şirket ortamında ve çeşitli durumlarda erişim elde etmek için bile çok faktörlü kimlik doğrulaması veya cihaz gereksinimleri dışında erişime izin olup olmadığını içerebilir.

## <a name="how-can-i-get-started"></a>Çalışmaya nasıl?
İlk Azure AD zaten kullanmadığınız ve bir BT yöneticisi misiniz:

* [Deneyin!](https://azure.microsoft.com/trial/get-started-active-directory/) -bir ücretsiz 30 günlük deneme için kaydolabilirsiniz Bugün ve ilk bulut çözümünüzü altında 5 bu bağlantıyı kullanarak dakika içinde dağıtma

Hesap paylaşımını etkinleştir azure AD özellikleri içerir:

* [Grup ataması](active-directory-accessmanagement-self-service-group-management.md)
* Azure AD uygulamalarına ekleme
* Atama ile çalışmaya başlama
* Uygulama atama hakkında SSS
* [Uygulama kullanım Pano/raporları](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Burada daha fazla bilgi edinebilirsiniz?
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Koşullu erişim ile uygulamaları koruma](active-directory-conditional-access.md)
* [Self Servis Grup Yönetimi/SSAA](active-directory-accessmanagement-self-service-group-management.md)

