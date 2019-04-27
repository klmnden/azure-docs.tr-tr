---
title: Azure AD kullanarak uygulamalara erişimi yönetme | Microsoft Docs
description: Azure Active Directory kuruluşlar her kullanıcının erişimi olan uygulamaları belirtmek nasıl sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70513675d09a663c65c6f5b3e18059467a8ba388
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60440739"
---
# <a name="managing-access-to-apps"></a>Uygulamalara erişimi yönetme
Devam eden erişim yönetimi, kullanım değerlendirme ve raporlama bir uygulama, kuruluşunuzun kimlik sistemine tümleştirildikten sonra bir mücadele haline devam edin. Çoğu durumda, BT yöneticileri veya Yardım Masası sahip devam eden etkin bir rol, uygulamalara erişimi yönetme gerçekleştirilecek. Bazı durumlarda, atama, genel veya bölümsel BT ekibi tarafından gerçekleştirilir. Atama karar BT kolaylaştırır önce onay gerektiren iş karar mercii Devredilmiş olması sık yöneliktir atama.  Tümleştirme mevcut otomatik kimlik ve erişim yönetimi sistemi kullanarak, rol tabanlı erişim denetimi (RBAC) veya öznitelik tabanlı erişim denetimi (ABAC) gibi diğer kuruluşlar yaparlar. Tümleştirme ve kural geliştirme özelleştirilmiş ve pahalı olma eğilimindedir. İzleme veya her iki Yönetim yaklaşımını üzerinde raporlama kendi ayrı, yüksek maliyetli ve karmaşık bir yatırımdır.

## <a name="how-does-azure-active-directory-help"></a>Azure Active Directory nasıl yardımcı oldu mu?
 Azure AD, kuruluşlara temsilci seçme ve dahil olmak üzere otomatik, öznitelik tabanlı atama (senaryolarında ABAC veya RBAC) arasında değişen doğru erişim ilkelerini kolayca elde etmek yapılandırılan uygulamalar için kapsamlı erişim yönetimi destekler Yönetici Yönetimi. Azure AD ile karmaşık ilkeleri tek bir uygulama için birden çok yönetim modeli birleştirerek kolayca elde edebilirsiniz ve Yönetim kuralları aynı hedef kitle ile uygulamalar arasında bile yeniden kullanabilirsiniz.

* [Yeni veya var olan uygulamaları](configure-single-sign-on-portal.md)

  Azure AD'nin uygulama ataması iki birincil atama modlarını odaklanır:

* **Tek atama** directory genel yönetici izinlerine sahip bir BT yöneticisi, bireysel kullanıcı hesapları'yi seçin ve bunları uygulamaya erişim verin.
* **Grup tabanlı atama (yalnızca Azure AD Ücretli)** directory genel yönetici izinlerine sahip bir BT yöneticisi uygulamaya bir grup atayabilir. Belirli kullanıcıların erişimini mi grubunun üyeleri, uygulamaya erişmeye çalıştıklarında zaman oldukları tarafından belirlenir. Diğer bir deyişle, bir yönetici, "herhangi bir geçerli atanan grubun üyesi uygulama erişimi olan" belirten bir atama kuralı etkili bir şekilde oluşturabilirsiniz. Bu atama seçeneğini kullanarak, yöneticiler Azure AD Grup Yönetim seçenekleri dahil olmak üzere, birini yararlanabilir [öznitelik tabanlı dinamik gruplar](../fundamentals/active-directory-groups-create-azure-portal.md), dış sistem gruplarına (örneğin, şirket içi Active Directory veya Workday), veya yönetici tarafından yönetilen veya self-servis yönetilen grubu. Tek bir grup, genel yönetim karmaşıklığı azaltır, uygulamaları atama benzeşimi ataması kuralları paylaşabilirsiniz emin olmak için birden fazla uygulama kolayca atanabilir. İç içe geçmiş grup üyelikleri grup tabanlı atama uygulamalar için şu anda desteklenmeyen unutmayın.

Bu iki atama modları kullanarak, Yöneticiler tüm istenen atama Yönetimi yaklaşımı elde edebilirsiniz.

Azure AD ile kullanımı ve atama raporlama tam olarak tümleşiktir, atama durumu, atama hataları ve hatta kullanım kolayca bildirilecek yöneticileri etkinleştirme.

## <a name="complex-application-assignment-with-azure-ad"></a>Azure AD ile karmaşık uygulama atama
Salesforce gibi bir uygulamayı düşünün. Birçok kuruluşta, Salesforce, öncelikle pazarlama ve satış ekipleri tarafından kullanılır. Satış ekibi üyelerinin sınırlı erişimi olan ancak genellikle, pazarlama ekibinin üyeleri yüksek oranda salesforce'a, ayrıcalıklı erişimi. Çoğu durumda, bilgi çalışanları genel bir popülasyonunu uygulamaya erişimi kısıtladı. Bu kuralların istisnaları işleniyor. Genellikle bir kullanıcı erişimi vermek veya rollerine genel kurallar bağımsız olarak değiştirmek için pazarlama veya satış liderlik takımlar prerogative olur.

Azure AD ile uygulamaları Salesforce gibi çoklu oturum açma (SSO) ve otomatik sağlama için önceden yapılandırılmış olabilir. Uygulama yapılandırıldıktan sonra bir yönetici oluşturmak ve uygun gruplara atamak için bir kerelik eylem sürebilir. Bu örnekte, yönetici, aşağıdaki atamalardan yürütebilirsiniz:

* [Dinamik gruplar](../fundamentals/active-directory-groups-create-azure-portal.md) otomatik olarak departman veya rol gibi öznitelikleri kullanarak pazarlama ve satış ekipleri tüm üyeleri göstermek için tanımlanabilir:
  
  * Pazarlama grupların tüm üyeleri Salesforce "Pazarlama" rolüne atanması
  * Tüm üyeler satış ekibinin grupları Salesforce "sales" rolüne atanır. Daha fazla iyileştirme farklı Salesforce rollere atanmış bölgesel satış ekipleri temsil eden birden çok gruplarını kullanabilirsiniz.
* Özel durum mekanizması etkinleştirmek için her rol için bir Self Servis Grup oluşturulabilir. Örneğin, "Özel durum Salesforce pazarlama" grubu bir Self Servis Grup oluşturulabilir. Grup için Salesforce pazarlama rol atanabilir ve pazarlama liderlik ekibindeki sahip hale getirilebilir. Pazarlama liderlik takım üyeleri ekleyin veya kullanıcıları kaldırmak, bir birleşim İlkesi ayarlamak veya bile onaylayın veya bireysel kullanıcılar katılma isteklerini reddedin. Bu mekanizma, sahipleri veya üyeleri için özel eğitim gerektirmeyen bir bilgi çalışanı uygun deneyimi aracılığıyla desteklenir.

Bu durumda, kendi rol ataması Salesforce'ta güncelleştirilecek farklı gruplara eklendikçe atanmış tüm kullanıcılar Salesforce, otomatik olarak sağlanması. Kullanıcıları bulmak ve Salesforce, Office web istemcileri, Microsoft uygulama erişim panelinde veya kendi kuruluş Salesforce oturum açma sayfasına giderek bile erişmek mümkün olacaktır. Yöneticiler, kolayca Azure AD raporlama kullanarak kullanımı ve atama durumu görüntülemek hazırdır.

Yöneticiler görevlendirmek [Azure AD koşullu erişim](../active-directory-conditional-access-azure-portal.md) belirli rolleri için erişim ilkeleri ayarlamak için. Bu ilkeler çeşitli durumlarda erişim elde etmek için bile çok faktörlü kimlik doğrulaması veya cihaz gereksinimleri ve kurumsal bir ortamı dışında erişimine izin olup olmadığını içerir.

## <a name="next-steps"></a>Sonraki adımlar
* [Koşullu erişim ile uygulamaları koruma](../active-directory-conditional-access-azure-portal.md)
* [Self Servis Grup Yönetimi/SSAA](../users-groups-roles/groups-self-service-management.md)
