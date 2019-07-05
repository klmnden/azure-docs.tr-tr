---
title: Azure Active Directory'de koşullu erişim ilkeleri planlama | Microsoft Docs
description: Bu makalede, Azure Active Directory koşullu erişim ilkeleri planlama hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56ddc2738305600c611cab1e09d654164f78b3d6
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509437"
---
# <a name="how-to-plan-your-conditional-access-deployment-in-azure-active-directory"></a>Nasıl Yapılır: Azure Active Directory'de koşullu erişim dağıtımınızı planlama

Koşullu erişim dağıtımınızı planlama, uygulamalar ve kaynaklar için gerekli erişim stratejisi, kuruluşunuzda elde emin olmak önemlidir. Çoğu vermek veya seçtiğiniz koşullar altında kullanıcılarınızın erişimi engellemek için ihtiyaç duyduğunuz çeşitli ilkeler tasarlamak için dağıtımınızın planlama aşamasında zaman ayırın. Bu belge, güvenli ve etkili koşullu erişim ilkeleri uygulamak için atmanız gereken adımları açıklar. Başlamadan önce anladığınızdan emin olun nasıl [koşullu erişim](overview.md) çalışır ve hangi durumlarda kullanmalıdır.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Koşullu erişim, kuruluşunuzun uygulama ve kaynakları, tek başına bir özelliği yerine erişimi denetlemenize olanak tanıyan bir altyapı olarak düşünebilirsiniz. Sonuç olarak, bazı koşullu erişim ayarlarını yapılandırılacak ek özellikleri gerektirir. Örneğin, belirli bir yanıt veren bir ilke yapılandırabilirsiniz [oturum açma risk düzeyini](../identity-protection/howto-sign-in-risk-policy.md#what-is-the-sign-in-risk-policy). Ancak, bir oturum açma risk düzeyini temel alan bir ilke gerektirir [Azure Active Directory kimlik koruması](../identity-protection/overview.md) etkinleştirilecek.

Ek özellik gerekmez, ilgili lisansları almak gerekebilir. Örneğin, koşullu erişim Özelliği Azure AD Premium P1 olsa da, kimlik koruması, bir Azure AD Premium P2 lisansı gerektirir.

Koşullu erişim ilkeleri iki tür vardır: temel ve standart. A [temel ilke](baseline-protection.md) önceden tanımlanmış bir koşullu erişim ilkesi. Bu ilkelerin etkin güvenlik temel düzeyde en az sahip olduğunuzdan emin olmaktır. Temel ilkeler. Temel ilkeleri, Azure AD tüm sürümlerinde kullanılabilir ve yalnızca sınırlı özelleştirme seçenekleri sağlar. Bir senaryo daha fazla esneklik gerekiyorsa, temel ilke devre dışı bırakın ve gereksinimlerinizi standart özel bir ilke uygulayın.

Standart bir koşullu erişim ilkesi, iş gereksinimleriniz İlkesi ayarlamak için tüm ayarlarını özelleştirebilirsiniz. Standart ilkeleri, bir Azure AD Premium P1 lisansı gerekir.

## <a name="draft-policies"></a>Taslak ilkeleri

Azure Active Directory koşullu erişim, bulut uygulamalarınızın koruma yepyeni bir düzeye getirmek sağlar. Bu yeni düzeyinde bir bulut uygulaması nasıl erişebileceğiniz bir statik erişim yapılandırması yerine bir dinamik ilke değerlendirmesi temel alır. Bir koşullu erişim ilkesi ile bir yanıt tanımlama (**bunu**) erişim durumuna (**bu durumda**).

![Neden ve yanıt](./media/plan-conditional-access/10.png)

Bu planlama modelini kullanarak, uygulamak istediğiniz her bir koşullu erişim ilkesini tanımlayın. Planlama alıştırma:

- Her ilke için bir koşul ve yanıtlar anahat yardımcı olur.
- Kuruluşunuz için iyi belgelendirilmiş koşullu erişim ilkesi katalog sonuçlanır. 

Kataloğunuzu ilke uygulamanızı kuruluşunuzun iş gereksinimlerini yansıtan olup olmadığını değerlendirmek için kullanabilirsiniz. 

Kuruluşunuz için koşullu erişim ilkeleri oluşturmak için aşağıdaki örnek şablonu kullanın:

|Zaman *bu* olur:|Ardından yapın *bu*:|
|-|-|
|Erişim denemesi yapılır:<br>-Bulut uygulaması için *<br>- kullanıcılar ve gruplar*<br>Kullanarak:<br>-Koşul 1 (örneğin, Corp ağın dışında)<br>-Koşul 2 (örneğin, cihaz platformları)|Uygulama erişimi engelle|
|Erişim denemesi yapılır:<br>-Bulut uygulaması için *<br>- kullanıcılar ve gruplar*<br>Kullanarak:<br>-Koşul 1 (örneğin, Corp ağın dışında)<br>-Koşul 2 (örneğin, cihaz platformları)|İle erişim verin (AND):<br>-Gereksinim 1 (örneğin, MFA)<br>-Gereksinim 2 (örneğin, cihaz uyumluluk)|
|Erişim denemesi yapılır:<br>-Bulut uygulaması için *<br>- kullanıcılar ve gruplar*<br>Kullanarak:<br>-Koşul 1 (örneğin, Corp ağın dışında)<br>-Koşul 2 (örneğin, cihaz platformları)|İle erişim verin (veya):<br>-Gereksinim 1 (örneğin, MFA)<br>-Gereksinim 2 (örneğin, cihaz uyumluluk)|

En azından **bu durumda** asıl tanımlar (**kimin**) bir bulut uygulamasına erişmeye çalışır (**ne**). Gerekli de ekleyebilirsiniz, **nasıl** erişim denemesi gerçekleştirilir. Koşullu erişimi'nde öğeleri tanımlayan kim, ne ve nasıl koşullar bilinir. Daha fazla bilgi için [Azure Active Directory koşullu erişim koşulları nelerdir?](conditions.md) 

İle **Bunu yapmak**, bir erişim durumuna ilkenizin yanıtı tanımlayın. Uygulamanızın yanıt engellemek veya ek gereksinimleri, örneğin, çok faktörlü kimlik doğrulaması (MFA) erişim. Eksiksiz bir genel bakış için bkz. [Azure Active Directory koşullu erişim denetimleri erişim nelerdir?](controls.md)  

Erişim denetimleri koşullarla bir koşullu erişim ilkesini temsil eder.

![Neden ve yanıt](./media/plan-conditional-access/51.png)

Daha fazla bilgi için [ne bir ilkeyi çalışması için gerekli olan](best-practices.md#whats-required-to-make-a-policy-work).

Bu noktada, ilkelerinize yönelik standart bir adlandırma karar vermek üzere zamanı geldi. Adlandırma standardı, ilkeleri bulun ve Azure Yönetim Portalı'nda açmaya gerek kalmadan kendi amacı anlamak yardımcı olur. Gösterilecek ilkenizi adlandırın:

- Bir sıra numarası
- Geçerli bulut uygulaması
- Yanıt
- Kimler için geçerlidir
- (Eğer varsa) geçerli olduğunda
 
![Adlandırma standardı](./media/plan-conditional-access/11.png)

Açıklayıcı bir ad, koşullu erişim uygulamanızı genel bir bakış tutmaya yardımcı olurken, sıra numarası bir konuşma ilkesinde başvurmak gerekiyorsa yararlıdır. Örneğin, diğer yönetici telefonla görüşmek, ilke bir sorunu çözmek için EM063 açacak biçimde isteyebilirsiniz.

Örneğin, şu ad ilke pazarlama Dynamics CRP uygulamasını kullanarak dış ağlardaki kullanıcıları için MFA gerektiriyorsa durumları:

`CA01 - Dynamics CRP: Require MFA For marketing When on external networks`

Etkin ilkeler yanı sıra, ikincil olarak davranan de devre dışı uygulama ilkeleri için tavsiye edilir [esnek erişim denetimleri kesinti/Acil Durum senaryolarda](../authentication/concept-resilient-controls.md). Yedek ilkeleri, adlandırma standardı, birkaç daha fazla öğe içermelidir: 

- `ENABLE IN EMERGENCY` diğer ilkelere göze adı başında.
- Uygulanması gereken kesintisi adı.
- Hangi sırayla ilkeleri etkinleştirilmesi gerektiğini bilmeniz yönetici yardımcı olmak için bir sıralama sıra numarası. 

Örneğin, aşağıdaki ad Bu ilke ilk ilke dört MFA kesintisi ise etkinleştirmelisiniz dışında olduğunu gösterir:

`EM01 - ENABLE IN EMERGENCY, MFA Disruption[1/4] - Exchange SharePoint: Require hybrid Azure AD join For VIP users`

## <a name="plan-policies"></a>İlkeleri planlama

Koşullu erişim ilkesi çözümünüzü planlarken, aşağıdaki sonuçları elde etmek için ilkeler oluşturmaya ihtiyacınız olup olmadığını değerlendirin. 

### <a name="block-access"></a>Erişimi engelle

Erişimi engellemek için en güçlü seçenektir çünkü bu:

- Bir kullanıcı için diğer tüm atamaları trumps
- Tüm kuruluşunuzu kiracınıza imzalama engelleyin gücüne sahip
 
Tüm kullanıcılar için erişimi engellemek istiyorsanız, en az bir kullanıcı (genellikle Acil Durum erişim hesapları) ilkeden dışlamanız gerekir. Daha fazla bilgi için [kullanıcıları ve grupları seçin](block-legacy-authentication.md#select-users-and-cloud-apps).  

### <a name="require-mfa"></a>MFA gerektirme

Kullanıcıların oturum açma deneyimini basitleştirmek için bulut uygulamalarınızdaki kullanıcı adı ve parola kullanarak oturum açmak izin vermek isteyebilirsiniz. Ancak, genellikle, kendisi için daha güçlü bir hesap doğrulaması biçimini zorunlu tutmak için tavsiye edilir en azından bazı senaryolar vardır. Bir koşullu erişim ilkesi ile MFA gereksinimi belirli senaryolar için sınırlayabilirsiniz. 

Erişim MFA gerektirmek için yaygın kullanım örnekleri şunlardır:

- [Yöneticileri tarafından](howto-baseline-protect-administrators.md)
- [Belirli uygulamalar için](app-based-mfa.md) 
- [Ağ Konumları güvenmediğiniz](untrusted-networks.md).

### <a name="respond-to-potentially-compromised-accounts"></a>Riskli olabilecek hesaplarına yanıt

Koşullu erişim ilkeleriyle riskli olabilecek kimlikler gelen oturum açma işlemleri için otomatik yanıtları uygulayabilirsiniz. Bir hesap tehlikede olasılık risk düzeyleri biçiminde ifade edilir. Kimlik koruması tarafından hesaplanan iki risk düzeyi vardır: oturum açma riski ve kullanıcı riski. Oturum açma riski yanıt uygulamak için iki seçeneğiniz vardır:

- [Oturum açma riski koşuluna](conditions.md#sign-in-risk) koşullu erişim ilkesi
- [Oturum açma riski İlkesi](../identity-protection/howto-sign-in-risk-policy.md) kimlik koruması 

Daha fazla özelleştirme seçenekleri verdiğinden koşulu olarak oturum açma riski adresleme tercih edilen yöntemdir.

Kullanıcı risk düzeyi yalnızca olarak kullanılabilir [kullanıcı riski İlkesi](../identity-protection/howto-user-risk-policy.md) kimlik Koruması'nda. 

Daha fazla bilgi için [Azure Active Directory kimlik koruması nedir?](../identity-protection/overview.md) 

### <a name="require-managed-devices"></a>Yönetilen cihazları gerektirme

Bulut kaynaklarınıza erişmek için desteklenen cihazlar çoğalan kullanıcılarınızın verimliliğini artırmaya yardımcı olur. Diğer taraftan, bilinmeyen koruma düzeyine sahip cihazlar tarafından erişilecek, ortamınızdaki belirli kaynaklara büyük olasılıkla istemezsiniz. Etkilenen kaynaklar için kullanıcılar yalnızca yönetilen bir cihazı kullanarak erişebildiğinizden istemeniz gerekir. Daha fazla bilgi için [yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi zorunlu kılma](require-managed-devices.md). 

### <a name="require-approved-client-apps"></a>Onaylı istemci uygulamaları gerektirme

Yapmanız gereken ilk kararlardan biri (KCG) senaryolarında kendi cihazlarını getirmesine, cihazın tamamını ya da yalnızca üzerindeki verileri yönetmek için ihtiyacınız olan. Çalışanlarınızın mobil cihazları hem kişisel hem de iş görevleri için kullanın. Çalışanlarınızın üretken olabilirler, ancak ayrıca veri kayıplarını da önlemek istersiniz. Azure Active Directory (Azure AD) koşullu erişimle şirket verilerinizi koruyabilirsiniz onaylı istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlayabilirsiniz. Daha fazla bilgi için [bulut uygulama erişimi ile koşullu erişim için onaylı istemci uygulamalarını zorunlu kılma](app-based-conditional-access.md).

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme

Azure AD birkaç eski bir kimlik doğrulama dahil olmak üzere en yaygın olarak kullanılan kimlik doğrulama ve yetkilendirme protokolünü destekler. Nasıl kiracınızın kaynaklara erişimini eski kimlik doğrulaması kullanan uygulamalar engelleyebilir miyim? Koşullu erişim ilkesi ile engellemek için önerilir. Gerekirse, yalnızca belirli kullanıcı ve belirli ağ konumlarını eski kimlik doğrulaması tabanlı uygulamaları kullanmasını sağlar. Daha fazla bilgi için [eski kimlik doğrulaması için Azure AD koşullu erişim ile nasıl](block-legacy-authentication.md).

## <a name="test-your-policy"></a>İlkenizi test

Bir ilke üretime çalışırken önce test etmelisiniz beklendiği gibi çalıştığını doğrulamak için.

1. Test kullanıcıları oluştur
1. Test planı oluşturma
1. İlkeyi yapılandırın
1. Bir sanal oturum değerlendir
1. İlkenizi test
1. Temizleme

### <a name="create-test-users"></a>Test kullanıcıları oluştur

Bir ilkeyi test etmek için ortamınızdaki kullanıcılar benzer bir kullanıcı kümesini oluşturun. Test kullanıcılarını oluşturulması, ilkelerinizi gerçek kullanıcıları etkilemeden ve potansiyel olarak uygulama ve kaynaklara olan erişimleri kesintiye önce beklendiği gibi çalıştığını doğrulamak sağlar. 

Bazı kuruluşların test kiracılar bu amaçla vardır. Ancak, tüm koşullar ve tam olarak bir ilkenin sonucunu test etmek için bir test kiracısında uygulamaları yeniden oluşturmak zor olabilir. 

### <a name="create-a-test-plan"></a>Test planı oluşturma

Test planı beklenen sonuçları ve gerçek sonuçların arasında bir karşılaştırma olması önemlidir. Her zaman bir test başlamadan önce bir Beklenti olmalıdır. Aşağıdaki tabloda, test örnekleri özetlenmektedir. Senaryolar ve beklenen sonuçları, CA ilkelerin nasıl yapılandırıldığına göre ayarlayın.

|İlke |Senaryo |Beklenen sonuç | Sonuç |
|---|---|---|---|
|[Çalışmıyorken MFA gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Kullanıcı oturum açtığında yetkili *uygulama* while güvenilir bir konumda / iş|MFA için kullanıcıdan değil| |
|[Çalışmıyorken MFA gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Kullanıcı oturum açtığında yetkili *uygulama* while güvenilen konumunda / iş|Kullanıcı MFA için istenir ve başarılı bir şekilde oturum açabilirsiniz| |
|[(Yönetici için) MFA gerektirme](https://docs.microsoft.com/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)|Genel yönetici imzalar içine *uygulama*|MFA yönetim istenir| |
|[Riskli oturum açma işlemleri](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)|Kullanıcı oturum açtığında içine *uygulama* kullanarak bir [Tor tarayıcı](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-playbook)|MFA yönetim istenir| |
|[Cihaz yönetimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Yetkili bir CİHAZDAN oturum açmak yetkili bir kullanıcı çalışır|Erişim izni verildi| |
|[Cihaz yönetimi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Yetkisiz bir CİHAZDAN oturum açmak için denemelerini yetkili|Erişim engellendi| |
|[Riskli kullanıcılar için parola değiştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)|Tehlikeye atılmış kimlik bilgilerine (yüksek riskli oturum açın) oturum açmanız denemelerini yetkili|Kullanıcı parolasını değiştirmesi istenir ya da kendi ilkesine göre erişimi engellenir| |

### <a name="configure-the-policy"></a>İlkeyi yapılandırın

Koşullu erişim ilkelerini yönetme el ile gerçekleştirilen bir görevdir. Azure portalında koşullu erişim ilkelerinizi tek bir merkezi konumda - koşullu erişim sayfasının yönetebilirsiniz. Koşullu erişim sayfasına bir giriş noktası **güvenlik** konusundaki **Active Directory** Gezinti Bölmesi. 

![Koşullu Erişim](media/plan-conditional-access/03.png)

Koşullu erişim ilkeleri oluşturmak için bkz hakkında daha fazla bilgi edinmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md). Bu hızlı başlangıçta size yardımcı olur:

- Kullanıcı arabirimi ile sahibi.
- Koşullu erişim nasıl çalıştığına ilişkin bir ilk izlenim alın. 

### <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre büyük olasılıkla beklenen şekilde çalışıp çalışmadığını bilmek ister. İlk adım, koşullu erişim kullanmak [ne İlkesi aracı](what-if-tool.md) bir oturum açma, test kullanıcının benzetimini yapmak için. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.

>[!NOTE]
> Sanal bir çalıştırma bir koşullu erişim ilkesinin etkisini izlenimi getirirken, gerçek bir test çalıştırması değiştirmez.

### <a name="test-your-policy"></a>İlkenizi test

Test planınıza göre test çalışmalarını Çalıştır. Bu adımda, her bir ilkenin her ilke düzgün şekilde davranan emin olmak test kullanıcılarınız için bir uçtan uca test çalıştırın. Her testi yürütmek için yukarıda oluşturulan senaryoları kullanın.

Dışlama ölçütleri ilkesinin test emin olmak önemlidir. Örneğin, bir kullanıcı veya grup MFA gerektiren bir ilkeden dışlayabilir. Dışlanan kullanıcılar için MFA'yı istenirse diğer ilkeleri birleşimi söz konusu kullanıcılar için mfa'yı gerekli çünkü test edin.

### <a name="cleanup"></a>Temizleme

Temizleme yordamı, aşağıdaki adımlardan oluşur:

1. Bu ilkeyi devre dışı.
1. Atanan kullanıcılar ve grupları kaldırın.
1. Test kullanıcıları silin.  

## <a name="move-to-production"></a>Üretime taşıma

Yeni ilkeler ortamınız için hazır olduğunuzda, bunları aşamalar dağıtın:

- Son kullanıcılara iç değişiklik iletişimi sağlar.
- Küçük bir kullanıcı kümesini başlatın ve ilkenin beklendiği gibi çalıştığını doğrulayın.
- Daha fazla kullanıcı eklemek için bir ilke genişlettiğinizde tüm yöneticiler hariç tutmak devam edin. Yöneticiler hariç olmak üzere bir değişiklik olursa birinin hala erişim ilkesine sahip olmasını sağlar.
- Bir ilke, yalnızca gerekli tüm kullanıcılar için geçerlidir.

En iyi uygulama, en az bir kullanıcı hesabı oluşturun:

- İlke yönetimi için ayrılmış
- Tüm ilkelerden hariç

## <a name="rollback-steps"></a>Geri alma adımları

Yeni uygulanan ilkelerinizi geri almak gerektiği durumlarda, geri almak için bir veya daha fazla aşağıdaki seçeneklerden birini kullanın:

1. **Bu ilkeyi devre dışı** -ilke devre dışı bırakma emin değil geçerli bir kullanıcı oturum açmaya çalıştığında getirir. Her zaman geri dönün ve kullanmak istediğiniz ilkeyi etkinleştirin.

   ![İlkeyi devre dışı bırakma](media/plan-conditional-access/07.png)

1. **Bir kullanıcıyı hariç tutma / Grup İlkesi'nden** -bir kullanıcı uygulamaya erişemez ise, kullanıcı, ilkeden dışlamak seçebilirsiniz

   ![Exluce kullanıcılar](media/plan-conditional-access/08.png)

   > [!NOTE]
   > Bu seçenek, gerektiğinde, yalnızca kullanıcı güvenilir olduğu durumlarda kullanılmalıdır. Kullanıcı ilke veya grup olabildiğince çabuk eklenmelidir.

1. **İlkeyi silmek** - ilke artık gerekli değilse silin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıma [Azure AD koşullu erişim belgelerine](index.yml) kullanılabilir bilgilerin bir özetini almak için.
