---
title: "Karma kimlik tasarımı - yaşam döngüsü benimseme stratejinizi Azure | Microsoft Docs"
description: "Karma kimlik yönetimi görevleri her yaşam döngüsü aşaması için kullanılabilir seçenekleri göre tanımlamasına yardımcı olur."
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: 420b6046-bd9b-4fce-83b0-72625878ae71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: bfa74c7557819bbef334fc94eb42e5ba83cf3fee
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Karma kimlik yaşam döngüsü benimseme stratejinizi belirleme
İçinde tanımlanan iş gereksinimlerini karşılamak, karma kimlik çözümü için Kimlik Yönetimi stratejisini tanımlayın bu görevde [karma kimlik yönetimi görevleri belirlemek](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).

Bu adım tarafından sunulan uçtan uca kimlik yaşam döngüsü göre karma kimlik yönetimi görevleri tanımlamak için her yaşam döngüsü aşaması için kullanılabilir seçenekleri dikkate almanız gerekir.

## <a name="access-management-and-provisioning"></a>Erişim yönetimi ve sağlama
İyi hesap erişim yönetimi çözümüyle kuruluşunuzun tam olarak kuruluş genelinde hangi bilgilere erişimi olan izleyebilirsiniz.

Erişim denetimi, bir merkezi, tek noktası sağlama sistemin kritik bir işlevdir. Hassas bilgileri koruma yanı sıra erişim denetimleri sahip var olan hesapları kullanıma onaylanmamış yetkilerini veya artık gerekli olan. Artık kullanılmayan hesapları, yetkili hesaplar sahibi olan kullanıcılara bilgiler sağlama sistem bağlantılar birlikte hesap bilgilerini denetlemek için. Yetkili kullanıcı kimlik bilgileri genellikle veritabanları ve İnsan Kaynakları dizinleri korunur.

Gelişmiş BT kuruluşları hesaplarında yetkilileri tanımlayan parametreleri yüzlerce içerir ve bu ayrıntıları sağlama sisteminiz tarafından denetlenebilir. Yeni kullanıcılar, yetkili kaynak sağlayan verilerle tanımlanabilir. Erişim isteği onay özelliği kendileri için sağlama kaynak onaylama (veya reddetme) işlemleri başlatır.

| Yaşam döngüsü yönetimi aşaması | Şirket içinde | Bulut | Karma |
| --- | --- | --- | --- |
| Hesap Yönetimi ve sağlama |Active Directory® etki alanı Hizmetleri (AD DS) sunucu rolünü kullanarak, kullanıcı ve kaynak yönetimi için ölçeklenebilir, güvenli ve yönetilebilir bir altyapı oluşturabilir ve Microsoft® Exchange Server gibi dizin özellikli uygulamalar için destek sağlayabilirsiniz. <br><br> [Bir kimlik Yöneticisi aracılığıyla AD DS'deki gruplarının sağlayabilirsiniz](https://technet.microsoft.com/library/ff686261.aspx) <br>[Kullanıcıların AD DS'de sağlayabilirsiniz](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Yöneticiler, erişim denetimi güvenlik amacıyla paylaşılan kaynakları için kullanıcı erişimi yönetmek üzere kullanabilirsiniz. Active Directory'de ayarı farklı düzeylerde erişim veya nesneler için izinleri tarafından nesne düzeyinde erişim denetimi yönetilen gibi tam denetim, okuma, yazma ya da erişim yok. Active Directory'de erişim denetimini tanımlar nasıl farklı kullanıcılar Active Directory nesnelerini kullanabilirsiniz. Varsayılan olarak, Active Directory içindeki nesneleri izinlerini en güvenli ayar ayarlanır. |Bir Microsoft bulut hizmeti erişen her kullanıcı için bir hesap oluşturmanız gerekir. Ayrıca, kullanıcı hesaplarını değiştirmek veya artık gerekmediğinde silin. Varsayılan olarak, kullanıcı yönetici izinlerine sahip değilse, ancak bunları isteğe bağlı olarak atayabilirsiniz. Daha fazla bilgi için bkz: [yöneten kullanıcılar Azure AD'de](active-directory-create-users.md). <br><br> Azure Active Directory içinde bulunan önemli özelliklerin kaynaklara erişimi yönetme olanağı biridir. Bu kaynaklar dizin ya da SaaS uygulamaları, Azure Hizmetleri ve SharePoint siteleri veya şirket içi kaynaklar gibi dizin dışı olan kaynaklar rolleri aracılığıyla nesneleri yönetmek için izinleri durumunda olduğu gibi dizinin parçası olabilir. <br><br> Merkezi Azure Active Directory'nin erişim yönetimi çözümü güvenlik grubudur. Kaynak sahibi (veya dizinin Yöneticisi) sağ sahip oldukları kaynakları belirli bir erişim sağlamak için bir grup atayabilir. Grubun üyelerini erişim sağlanması ve kaynak sahibine bir bölüm Yöneticisi'ni veya bir Yardım Masası Yöneticisi gibi başka – birisi bir gruba üye listesini yönetmek için sağa devredebilirsiniz<br> <br> Azure AD konu yönetme gruplarında grupları üzerinden erişimi yönetme hakkında daha fazla bilgi sağlar. |Active Directory kimlik eşitleme ve Federasyon aracılığıyla buluta genişletme |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Rol tabanlı erişim (RBAC) kullanan rolleri denetler ve sağlama ilkelerini değerlendirmek için test ve iş süreçlerini ve kullanıcılara erişim izni verme için kuralları zorlar. Anahtar yöneticileri sağlama ilkeleri oluşturma ve kullanıcıları rollere atamak ve bu rolleri kaynaklarına yetkilendirmeler kümelerini tanımlayın. RBAC yazılım tabanlı işlemler kullanmak ve kullanıcı sağlama işlemini el ile etkileşimini azaltmak için kimlik yönetimi çözümü genişletir.
Azure AD RBAC kendisinin Azure Yönetim Portalı erişimi sonra bir kişinin yapabileceği işlemleri miktarını sınırlamak şirket sağlar. Portal erişimini denetlemek için RBAC kullanarak, BT yöneticilerinin ca temsilci erişimi aşağıdaki erişim yönetimi yaklaşım kullanarak:

* **Grup tabanlı rol ataması**: erişim eşitlenebilen Azure AD grupları yerel Active Directory'den atayabilirsiniz. Bu, kuruluşunuzun araçları ve grupları yönetmek için işlemlerdeki yaptı Yatırımlar yararlanmanızı sağlar. Azure AD Premium temsilci Grup Yönetimi özelliği de kullanabilirsiniz.
* **Azure rollerinde yerleşik Dengeleme**: üç rol kullanabilirsiniz — sahibi, katkıda bulunan ve okuyucu, kullanıcılar ve gruplar yalnızca işlerini gerçekleştirmek için ihtiyaç duydukları görevleri gerçekleştirme izniniz olduğundan emin olun.
* **Kaynaklar için ayrıntılı erişim**: Kullanıcıları ve grupları belirli bir abonelik için kaynak grubu veya bir Web sitesi veya veritabanı gibi ayrı bir Azure kaynağı roller atayabilirsiniz. Bu şekilde, kullanıcıların ihtiyaç duydukları tüm kaynaklara ve yönetmek zorunda kalmazsınız kaynaklara erişemez sahip olduğunuzdan emin olabilirsiniz.

## <a name="provisioning-and-other-customization-options"></a>Sağlama ve diğer özelleştirme seçenekleri
Ekibinizin iş planları ve gereksinimleri kimlik çözümü özelleştirmek ne kadar karar vermek için kullanabilirsiniz. Örneğin, büyük bir kuruluş, iş akışları ve artımlı olarak coğrafyalara yaygın olarak kullanılan uygulamaların sağlama için bir zaman çizgisi dayalı özel bağdaştırıcıları için aşamalı bir üretimini planı gerektirebilir. Başka bir planı özelleştirme başarılı test sonra tüm kuruluş, sağlanacak iki veya daha fazla uygulama için sağlayabilir. Uygulama kullanıcı etkileşimi özelleştirilebilir ve kaynakları hazırlamaya yönelik yordamlar otomatik sağlama uyum sağlayacak şekilde değiştirilebilir.

Bir hizmet veya bileşenin kaldırmak için yetkisini kaldırma. Örneğin, bir hesap sağlama kaldırma hesabı bir kaynaktan silinir anlamına gelir.

Kaynak sağlama karma modeli istek ve her ikisi de Azure AD tarafından desteklenen rol tabanlı yaklaşımlar birleştirir. Bir alt kümesi çalışanlar veya yönetilen sistemler için bir iş rol tabanlı atama ile erişimini otomatik hale getirmek isteyebilirsiniz. Bir iş ayrıca diğer tüm erişim isteklerini veya istek tabanlı modeli aracılığıyla özel durumları işlemek. Bazı işletmeler el ile atama ile başlamalı ve bir sonraki seferde tam rol tabanlı bir dağıtım amacı ile karma bir modelinin doğru gelişmesi.

Diğer şirketlerin tam rol tabanlı sağlama elde etmek iş nedenleri için pratik bulun ve karma yaklaşım istenen hedef olarak hedef. Hala diğer şirketlerin yalnızca istek tabanlı sağlamada memnun ve tanımlamak ve rol tabanlı, otomatik sağlama ilkeleri yönetmek için ek çaba yatırım istemiyorsanız.

## <a name="license-management"></a>Lisans Yönetimi
Azure AD'de grup tabanlı lisans yönetimi, yöneticilerin kullanıcıların bir güvenlik grubuna atayın sağlar ve Azure AD lisanslarını grubun tüm üyelerine otomatik olarak atar. Bir kullanıcı daha sonra eklenen veya gruptan kaldırılmış, bir lisans otomatik olarak atanmış veya kaldırılacak uygun şekilde kaldırıldı.

Grupları, eşitleme kullanabilirsiniz şirket içi AD veya Azure AD içinde yönetin. Bu Azure AD premium Self Servis Grup Yönetimi ile birlikte kullanılması lisans atamasını uygun karar alıcılar için kolayca devredebilirsiniz. Lisans çakışmaları ve eksik konum verileri gibi sorunları otomatik olarak sıralandığını olabilirsiniz.

## <a name="self-regulating-user-administration"></a>Kendi kendine regulating kullanıcı yönetimi
Kuruluşunuzun tüm iç kuruluşlar arasında kaynakları sağlamak üzere başladığında, kendi kendine regulating kullanıcı yönetim özelliği uygulayın. Kuruluş sınırları boyunca avantajları ve sağlama kullanıcılar yararları sağlarsınız. Bu ortamda otomatik olarak erişim haklarını kullanıcının durumundaki bir değişiklik, kuruluş sınırları ve coğrafyalara arasında yansıtılır. Sağlama maliyetlerini azaltma ve erişim ve onay işlemlerini kolaylaştırır. Uygulama rol tabanlı erişim denetimi uçtan uca erişim yönetimi, kuruluşunuzda uygulamanın tam olası gerçekleştirir. Kullanıcı sağlamayı yöneten otomatik yordamlar üzerinden yönetim maliyetlerini azaltabilir. Güvenlik İlkesi zorlaması otomatikleştirerek güvenliğini artırmak ve kolaylaştırmak ve kullanıcının yaşam döngüsü yönetimi ve büyük kullanıcı yerleştirme için kaynak sağlama merkezileştirme.

> [!NOTE]
> Daha fazla bilgi için bkz: self servis uygulamaya erişim yönetimi için Azure AD kurma
> 
> 

Lisans tabanlı Azure AD (yetkilendirme tabanlı), Azure AD directory/hizmet kiracınız abonelikte etkinleştirerek iş Hizmetleri. Abonelik etkinleştirildikten sonra hizmet özellikleri dizin/hizmet yöneticileri tarafından yönetilen ve lisanslı kullanıcılar tarafından kullanılır. Daha fazla bilgi için bkz: Azure AD iş lisanslama nasıl yapar?
Diğer 3. taraf sağlayıcılar ile tümleştirme

Azure Active Directory çoklu oturum sağlar ve Gelişmiş binlerce SaaS uygulamaları ve şirket içi web uygulamaları için uygulama erişim güvenlik. Azure Active Directory Federasyon Uyumluluğu Listesi desteklenen SaaS uygulamaları için Azure Active Directory Uygulama galerisinde ayrıntılı bir listesi için bkz: çoklu oturum açmayı uygulamak için kullanılan üçüncü taraf kimlik sağlayıcıları

## <a name="define-synchronization-management"></a>Eşitleme management tanımlama
Şirket içi dizinlerinizin Azure AD ile tümleştirilmesi, kullanıcılarınızın hem bulut kaynaklarına hem de şirket içi kaynaklara erişmesi için ortak bir kimlik oluşturarak daha üretken olmalarını sağlar. İle tümleştirme, kullanıcılar ve kuruluşlar aşağıdaki özelliklerden yararlanabilirsiniz:

* Kuruluşlar, şirket içi veya Windows Server Active Directory yararlanan ve Azure Active Directory'ye bağlanırken bulut tabanlı hizmetleri arasında ortak bir karma kimlik kullanıcılarla sağlayabilir.
* Yöneticiler, Uygulama kaynağı, aygıt ve kullanıcı kimliği, ağ konumu ve çok faktörlü kimlik doğrulaması göre koşullu erişim sağlayabilir.
* Kullanıcılar, Office 365, Intune, SaaS uygulamaları ve üçüncü taraf uygulamaları için Azure AD'de ortak kimliklerini hesapları yoluyla yararlanabilirsiniz.
* Geliştiriciler, uygulamaları içi Active Directory veya Azure içinde bulut tabanlı uygulamalar için tümleştirme ortak kimlik modeli yararlanan uygulamalar oluşturabilir

Aşağıdaki şekil kimlik eşitleme işlemi üst düzey bir görünümünü örneği vardır.

![](./media/hybrid-id-design-considerations/identitysync.png)

Kimlik eşitleme işlemi

Eşitleme seçenekleri karşılaştırmak için aşağıdaki tabloyu gözden geçirin:

| Eşitleme yönetim seçeneği | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| Eşitleme tabanlı (aracılığıyla, DirSync veya AADConnect) |Kullanıcılar ve gruplar şirket içi ve bulut eşitlendi <br>  **İlke denetimi**: hesap ilkeleri yönetici parola ilkeleri, iş istasyonu, kısıtlamalar, kilitleme denetimleri yönetmenizi sağlar, Active Directory üzerinden ve daha fazlasını kalmadan bulutta ek görevleri gerçekleştirmek ayarlanabilir.  <br>  **Erişim denetimi**: Hizmetleri çevrimiçi sunucuları veya her ikisi de aracılığıyla şirket ortamında aracılığıyla erişilebilir böylece bulut hizmetine erişimi kısıtlayabilirsiniz. <br>  Destek aramaları azaltılmış: kullanıcıların anımsaması daha az parolaları varsa, bunlar unutmanız olasılığı daha düşüktür. <br>  Güvenlik: Kullanıcı kimlikleri ve bilgileri tüm sunucuların ve çoklu oturum açma içinde kullanılan hizmetler yönetilen çünkü korumalı ve şirket içi denetlenir. <br>  Güçlü kimlik doğrulaması için destek: güçlü kimlik doğrulaması (iki öğeli kimlik doğrulama olarak da bilinir) bulut hizmetiyle kullanabilirsiniz. Ancak, güçlü kimlik doğrulaması kullanırsanız, çoklu oturum açma kullanmanız gerekir. | |
| Federasyon tabanlı (aracılığıyla AD FS) |Güvenlik belirteci hizmeti (STS) tarafından etkinleştirilmiş. Bir Microsoft bulut hizmeti ile oturum açma tek erişim sağlamak için bir STS yapılandırdığınızda, bir federasyon güveni, şirket içi STS ile Azure AD kiracınızda belirlediğiniz Federasyon etki alanı arasında oluşturacağınız. <br> Son kullanıcıların birden çok kaynaklarına erişim elde etmek için aynı kimlik bilgileri kümesini kullanın olanak sağlar <br>Son kullanıcılar birden çok kimlik bilgileri kümesi sağlamak zorunda değildir. Kullanıcıların katılımcı kaynakları., her biri için kimlik bilgilerini sağlamak henüz B2B ve B2C senaryoları desteklenir. |Dağıtım ve içi adanmış bakım için uzman personeli gerektirir AD FS sunucuları. AD FS, STS için kullanmayı planlıyorsanız, güçlü kimlik doğrulaması kullanma kısıtlamaları vardır. Daha fazla bilgi için bkz: [AD FS 2.0 için Gelişmiş Seçenekleri yapılandırma](http://go.microsoft.com/fwlink/?linkid=235649). |

> [!NOTE]
> Daha fazla bilgi için [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

