---
title: Karma kimlik tasarımı - yaşam döngüsü benimseme stratejisi Azure | Microsoft Docs
description: Karma kimlik yönetimi görevlerini göre her yaşam döngüsü aşaması için kullanılabilir seçenekleri tanımlamaya yardımcı olur.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85f600c8bd46e699e80bf7b596574dc01467ef79
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109309"
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Karma kimlik yaşam döngüsü benimseme stratejisi belirleme
Bu görevde, tanımladığınız iş gereksinimlerini karşılamak, karma kimlik çözümü için Kimlik Yönetimi stratejisi tanımlarsınız [karma kimlik yönetimi görevleri belirlemek](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

Bu adım tarafından sunulan uçtan uca kimlik yaşam döngüsü göre karma kimlik yönetimi görevleri tanımlamak için her yaşam döngüsü aşaması için kullanılabilir seçenekleri dikkate almanız gerekir.

## <a name="access-management-and-provisioning"></a>Erişim denetimi ve sağlaması
İyi hesap erişim yönetimi çözümü ile kuruluşunuzun tam olarak kuruluş genelinde hangi bilgilerin erişimi olan izleyebilirsiniz.

Erişim denetimi, bir merkezi, tek noktası sağlama sistemin kritik bir işlevdir. Erişim denetimleri olan mevcut hesapları kullanıma hassas bilgileri koruyan yanı sıra, onaylanmamış yetkilendirmeleri veya artık gerekli are. Eski hesapları, yetkili hesaplar sahibi olan kullanıcıların bilgilerle sağlama sistem bağlantıları birlikte hesabı bilgilerini denetlemek için. Yetkili bir kullanıcı kimlik bilgileri, genellikle veritabanları ve İnsan Kaynakları dizinleri korunur.

Bu ayrıntıları sağlama sisteminiz tarafından denetlenebilir ve karmaşık BT kuruluşları hesaplarında yetkilileri tanımlayan parametreler yüzlerce içerir. Yeni kullanıcılar, yetkili kaynak sağladığınız verilerle tanımlanabilir. Erişim isteği onay özelliği, kaynak için sağlama onaylama (veya reddetme) işlemleri başlatır.

| Yaşam döngüsü yönetimi aşaması | Şirket içi | Bulut | Hibrit |
| --- | --- | --- | --- |
| Hesap Yönetimi ve sağlama |Active Directory® etki alanı Hizmetleri (AD DS) sunucu rolünü kullanarak, kullanıcı ve kaynak yönetimi için ölçeklenebilir, güvenli ve yönetilebilir bir altyapı oluşturabilir ve Microsoft® Exchange Server gibi dizin özellikli uygulamalar için destek sağlar. <br><br> [Identity manager aracılığıyla AD DS'deki grupları sağlayabilirsiniz.](https://technet.microsoft.com/library/ff686261.aspx) <br>[AD DS'de kullanıcı sağlayabilirsiniz.](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Yöneticiler, güvenlik nedenleriyle paylaşılan kaynaklara kullanıcı erişimini yönetmek için erişim denetimini kullanabilirsiniz. Nesne düzeyinde erişim izni nesneleri veya farklı düzeylerde ayarı tarafından yönetilen Active Directory'de, erişim denetimi gibi tam denetim, yazma, okuma veya erişim yok. Active Directory erişim denetimini tanımlar nasıl farklı kullanıcılar Active Directory nesnelerini kullanabilirsiniz. Varsayılan olarak, Active Directory içindeki nesneleri izinleri için en güvenli ayar ayarlanır. |Bir Microsoft bulut hizmeti erişen her kullanıcı için bir hesap oluşturmanız gerekir. Ayrıca, kullanıcı hesaplarını değiştirmek veya artık zaman silebilirsiniz. Varsayılan olarak, kullanıcılara yönetici izinleri olmayan, ancak bunları isteğe bağlı olarak atayabilirsiniz. <br><br> Azure Active Directory içinde ilgili önemli özellikleri kaynaklara erişimi yönetme olanağı biridir. Bu kaynaklar dizindeki roller aracılığıyla nesneleri yönetme izinlerinde olduğu gibi bir dizine veya SaaS uygulamaları, Azure hizmetleri ve SharePoint siteleri ya da şirket içi kaynaklar gibi dizin dışındaki kaynaklara ait olabilir. <br><br> Merkezi Azure Active Directory erişim yönetimi çözümü güvenlik grubudur. Kaynak sahibi (veya dizinin yöneticisi), bir gruba sahip oldukları kaynaklara belirli bir erişim hakkı atayabilir. Grubun üyelerini erişim sağlanır ve kaynak sahibi sağındaki bölüm yöneticisi veya bir Yardım Masası Yöneticisi gibi başka – birisi bir gruba üye listesini yönetmek için yetkilendirme yapabilirsiniz<br> <br> Azure AD bölümdeki yönetme grupları grupları aracılığıyla erişimi yönetme hakkında daha fazla bilgi sağlar. |Active Directory kimlik eşitleme ve Federasyon aracılığıyla buluta genişletin |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Rol tabanlı erişim denetimi (RBAC) kullanır rolleri ve sağlama ilkelerini değerlendirmek için test ve iş süreçleri ve kullanıcılara erişim verme kurallarını zorla. Anahtar yöneticileri sağlama ilkeleri oluşturma ve kullanıcıları rollere atamak ve bu rolleri için kaynaklara yetkilendirmeler kümesini tanımlayan. RBAC, yazılım tabanlı işlemler kullanır ve el ile kullanıcı etkileşimi sağlama işleminde azaltmak için kimlik yönetimi çözümü genişletir.
Azure AD RBAC, Azure portalına erişiminiz sonra bireysel yapabilirsiniz işlemlerin sayısını sınırlamak şirket sağlar. Portal erişimini denetlemek için RBAC kullanarak, BT yöneticilerinin ca temsilci erişimi aşağıdaki erişim yönetimi yaklaşımlardan kullanarak:

* **Grup tabanlı rol ataması**: Erişim, yerel Active Directory'nizden eşitlenebilen Azure AD gruplarına atayabilirsiniz. Bu, kuruluşunuz, araçları ve grupları yönetmek için işlemlerdeki yaptı mevcut yatırımlardan yararlanma sağlar. Azure AD Premium Temsilcili Grup Yönetimi özelliğini de kullanabilirsiniz.
* **Rolleri azure'da yerleşik yararlanarak**: Üç rol kullanabilirsiniz — sahibi, katkıda bulunan ve okuyucu, kullanıcılar ve gruplar yalnızca kullanıcıların işlerini yapmak için ihtiyaç duydukları görevleri gerçekleştirme izniniz olduğundan emin olun.
* **Kaynakları için ayrıntılı erişim**: Kullanıcılar ve gruplar için belirli bir abonelikte, kaynak grubu veya bir Web sitesi veya veritabanı gibi ayrı bir Azure kaynak rolleri atayabilirsiniz. Bu şekilde, kullanıcıların ihtiyaç duydukları tüm kaynaklara erişim ve yönetmek için gerekmeyen kaynaklara erişemez olmasını sağlayabilirsiniz.

## <a name="provisioning-and-other-customization-options"></a>Sağlama ve diğer özelleştirme seçeneklerinin
Takımınız ne kadar kimlik çözümü özelleştirmek karar vermek için iş planları ve gereksinimleri kullanabilirsiniz. Örneğin, büyük bir kuruluş, iş akışları ve artımlı olarak coğrafi bölgelerde yaygın olarak kullanılan uygulamaları sağlamak için bir zaman çizgisi temel alan özel bağdaştırıcıları için aşamalı bir sunum planı gerektirebilir. Başarılı test sonra tüm kuruluş, sağlanan iki veya daha fazla uygulama için başka bir özelleştirme planı sağlayabilir. Uygulama kullanıcı etkileşimi özelleştirilebilir ve otomatik sağlama uyum sağlamak için kaynak sağlamak için yordamlar değiştirilebilir.

Bir hizmet veya bileşen kaldırmak için yetkisini kaldırabilirsiniz. Örneğin, bir hesap sağlama kaldırma hesabı bir kaynaktan silinir anlamına gelir.

Kaynak sağlama karma modeli, istek ve Azure AD tarafından desteklenen rol tabanlı yaklaşımlar birleştirir. Bir alt kümesi, çalışanların veya yönetilen sistemler için bir iş erişim rol tabanlı atama ile otomatik hale getirmek isteyebilirsiniz. Bir iş, diğer tüm erişim isteklerini veya talep tabanlı bir modeli aracılığıyla özel durumları da işleyebilirsiniz. Bazı şirketler el ile atama ile başlatın ve bir amaç gelecek tam rol tabanlı bir dağıtım ile bir karma model doğru evrim Geçiren.

Diğer şirketlerin tam rol tabanlı sağlama elde etmek iş nedenleri için pratik bulun ve istenen hedef olarak karma bir yaklaşım hedef. Hala başka şirketlerle istek tabanlı yalnızca sağlama ile memnun ve tanımlamak ve rol tabanlı ve otomatik sağlama ilkeleri yönetmek için ek çaba yatırım istemiyorsanız.

## <a name="license-management"></a>Lisans Yönetimi
Azure AD'de grup tabanlı lisans yönetimi, yöneticilerin kullanıcıların bir güvenlik grubuna atayın sağlar ve Azure AD lisanslarını grubun tüm üyelerine otomatik olarak atar. Bir kullanıcı daha sonra eklenen veya gruptan, bir lisans otomatik olarak atanan veya yüklenecek uygun şekilde kaldırıldı.

Grupları, eşitleme kullanabilirsiniz şirket içi veya Azure AD'de yönetme. Bu Self Servis Grup Yönetimi Azure AD premium ile eşleştirme, lisans ataması uygun karar alıcılar için kolayca yetkilendirebilirsiniz. Lisans çakışıyor ve eksik konum verileri gibi sorunları otomatik olarak sıralandığını olabilirsiniz.

## <a name="self-regulating-user-administration"></a>Kendi kendine regulating kullanıcı yönetimi
Kuruluşunuzun tüm iç kuruluş genelinde kaynakları sağlamak başladığında, kendi kendine regulating kullanıcı yönetim özelliği uygular. Kuruluş sınırlarında avantajları ve sağlama kullanıcılar avantajlarını hayata geçirebilirsiniz. Bu ortamda, otomatik olarak erişim hakları bir kullanıcının durum değişikliği, kuruluş sınırları ve coğrafyalarda yansıtılır. Sağlama maliyetlerini azaltmak ve erişim ve onay süreçlerini kolaylaştırabilirsiniz. Uygulama, rol tabanlı erişim denetimi uçtan uca erişim yönetimi için kuruluşunuzdaki uygulama tam potansiyelini fark etti. Otomatik kullanıcı hazırlama yöneten yordamlar aracılığıyla yönetim maliyetlerini azaltabilirsiniz. Güvenlik İlkesi zorlaması otomatik hale getirerek güvenlik artırmak ve daha verimli hale getirin ve kullanıcı yaşam döngüsü yönetimi ve çok sayıda kullanıcı yerleştirme için kaynak sağlama merkezileştirme.

> [!NOTE]
> Daha fazla bilgi için bkz. Azure AD self servis uygulamaya erişim yönetimi için ayarlama
> 
> 

Azure AD tabanlı lisans (yetkilendirmesini tabanlı) bir aboneliği Azure AD directory/hizmet kiracınızdaki etkinleştirerek iş Hizmetleri. Abonelik etkin hale geldikten sonra hizmet özellikleri dizin/hizmet yöneticileri tarafından yönetilebilir ve lisanslı kullanıcı tarafından kullanılan. 

## <a name="integration-with-other-3rd-party-providers"></a>Diğer 3. taraf sağlayıcılar ile tümleştirme

Azure Active Directory çoklu oturum sağlar ve binlerce SaaS uygulamaları ve şirket içi web uygulamaları için uygulama erişimi güvenliği artırılmış. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

## <a name="define-synchronization-management"></a>Eşitleme management tanımlama
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. Bu tümleştirme sayesinde, kullanıcılar ve kuruluşlar aşağıdaki avantajlarından yararlanabilirsiniz:

* Kuruluşlar şirket içi veya bulut tabanlı hizmetlere yararlanarak Windows Server Active Directory ve ardından Azure Active Directory'ye bağlanılırken arasında ortak bir karma kimlik kullanıcılarla sağlayabilir.
* Yöneticiler Uygulama kaynağı, cihaz ve kullanıcı kimliği, ağ konumu ve çok faktörlü kimlik doğrulaması tabanlı koşullu erişim sağlayabilir.
* Kullanıcılar, Office 365, Intune, SaaS uygulamaları, Azure AD'de kendi ortak kimlik hesabından yararlanabilir ve üçüncü taraf uygulamaları.
* Geliştiriciler, uygulamaları bulut tabanlı uygulamalar için Active Directory şirket içi veya Azure tümleştirme ortak kimlik modeli yararlanan uygulamalar oluşturabilir

Aşağıdaki şekilde kimlik eşitleme işlemi üst düzey görünümü örneği vardır.

![Sync](./media/plan-hybrid-identity-design-considerations/identitysync.png)

Kimlik eşitleme işlemi

Eşitleme seçeneklerini karşılaştırmak için aşağıdaki tabloyu gözden geçirin:

| Eşitleme yönetim seçeneği | Yararları | Dezavantajları |
| --- | --- | --- |
| Eşitleme tabanlı (üzerinden eşitleme veya DirSync) |Kullanıcıları ve grupları şirket içi ve bulut eşitlendi <br>  **İlke denetimi**: Hesap ilkeleri, yönetici parola ilkeleri, iş istasyonu, kısıtlamalar, kilitleme denetimleri ve daha fazla bulutta ek görevleri gerçekleştirmek zorunda kalmadan yönetme olanağı sağlayan, Active Directory aracılığıyla ayarlanabilir.  <br>  **Erişim denetimi**: Böylece Hizmetleri çevrimiçi sunucuları üzerinden şirket ortamında veya her ikisi de erişilebilen, bulut hizmetine erişimi kısıtlayabilirsiniz. <br>  Destek çağrılarının sayısını azalttık: Kullanıcıların hatırlamak zorunda olduğunuz şifre varsa, bunlar unutmanız olma olasılığını azaltacak. <br>  Güvenlik: Kullanıcı kimliklerini ve bilgi tüm sunucuları ve çoklu oturum açma içinde kullanılan hizmetlere yönetilen çünkü korunur ve şirket içi denetlenir. <br>  Güçlü kimlik doğrulaması için destek: Bulut hizmeti ile güçlü kimlik doğrulaması (iki öğeli kimlik doğrulama olarak da bilinir) kullanabilirsiniz. Ancak, güçlü kimlik doğrulaması kullanıyorsanız, çoklu oturum açma kullanmanız gerekir. | |
| Federasyon tabanlı (AD FS) aracılığıyla |Güvenlik belirteci hizmeti (STS) tarafından etkin. Bir Microsoft bulut hizmeti ile çoklu oturum açma erişimi sağlamak için bir STS'ye yapılandırırken, federasyon güveni, şirket içi STS'LERİNE ve Azure AD kiracınızda belirttiniz. Federasyon etki alanı arasında oluşturacağınız. <br> Son birden çok kaynağa erişim sağlamak için aynı kimlik bilgileri kümesi kullanmasına olanak tanır <br>Son kullanıcılar birden çok kimlik bilgileri kümesi sürdürmenize gerek yoktur. Kullanıcıların henüz katılımcı kaynaklar., her biri için kimlik bilgilerini sağlamanız gereken B2B ve B2C senaryolar desteklenir. |Dağıtım ve adanmış Bakımı şirket içi AD FS sunucuları için personele özel yetki verilmesini gerektirir. AD FS için STS kullanmayı planlıyorsanız, güçlü kimlik doğrulaması kullanılması ile ilgili kısıtlamalar vardır. Daha fazla bilgi için [AD FS 2.0 için Gelişmiş Seçenekleri yapılandırma](https://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Daha fazla bilgi edinmek, [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

