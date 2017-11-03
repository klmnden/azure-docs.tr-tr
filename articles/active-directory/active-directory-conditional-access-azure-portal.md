---
title: "Azure Active Directory koşullu erişim | Microsoft Docs"
description: "Koşullu erişim denetimi için belirli koşullar uygulamalara erişim için kimlik doğrulaması yapılırken denetlemek için Azure Active Directory'de kullanın."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/27/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4cf30130907151ade9eaf9db28748b8141dac8e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişim

Bir mobil ilk olarak, bulut ilk dünyasında, Azure Active Directory çoklu oturum açma cihazları, uygulamaları ve Hizmetleri için yerden sağlar. Devre dışı şirket ağlarına (KCG dahil) aygıtların artışı ile çalışır ve 3 taraf SaaS uygulamalar, BT uzmanları iki rakip hedefle karşılaştığı:

- Yerde ve zamanda üretken olmak için son kullanıcılara güç kazandırma
- Herhangi bir zamanda Kurumsal varlıklar koruma

Üretkenliği artırmak için Azure Active Directory kullanıcılarınıza çok çeşitli şirket varlıklarınızı erişmek için seçenekler sağlar. Azure Active Directory, yalnızca emin olmak uygulama erişim yönetimiyle sağlar *doğru kişilerin* uygulamalarınızı erişebilirsiniz. Ne doğru kişilerin kaynaklarınızı belirli koşullar altında nasıl erişiyorsunuz üzerinde daha fazla denetim sahip olmak istiyorsunuz? Koşullar altında istediğiniz belirli uygulamalar için bile erişimi engellemek bile sahip ne *sağ kişiler*? Örneğin, belirli uygulamaların güvenilen bir ağdan doğru kişilerin erişiyorsanız, Tamam olması olabilir; Ancak, bu uygulamaları güvenmediğiniz bir ağdan erişmelerini istemeyebilirsiniz. Koşullu erişim kullanarak bu soruları ele alabilir.

Koşullu erişim, belirli koşullara göre ortamınızda uygulamalar için erişim denetimleri zorunlu tutmanıza olanak sağlayan Azure Active Directory bir yetenektir. Denetimleri ile erişim için ek gereksinimler ya da bağlayabilirsiniz veya, engelleyebilirsiniz. Koşullu erişim ilkelerini temel alır. Erişim gereksinimlerinizi hakkında düşünme yolu izleyen gerektiğinden, ilke tabanlı bir yaklaşım yapılandırma deneyiminizi kolaylaştırır.  

Genellikle, aşağıdaki deseni temel alınarak deyimleri kullanarak erişim gereksinimlerinizi tanımlayın:

![denetimi](./media/active-directory-conditional-access-azure-portal/10.png)

İki oluşumları yerine ne zaman "*bu*" gerçek bilgilerle bir örnek için büyük olasılıkla size tanıdık benzeyen bir ilke bildirimi vardır:

*Yükleniciler güvenilmeyen ağlardan bizim bulut uygulamalarını erişmeye çalışırken erişimini engelleyin.*

Yukarıdaki ilke bildirimi koşullu erişim gücünü vurgular. Temel olarak, bulut uygulamalarınızı erişmek Yükleniciler olanak sağlarken (**kimin**), koşullu erişimle altında erişim olası koşullar da tanımlayabilirsiniz (**nasıl**).

Azure Active Directory koşullu erişim bağlamında

- "**Bu durumda**" olarak adlandırılır **koşul deyimi**
- "**Sonra bunu**" olarak adlandırılır **denetimleri**

![denetimi](./media/active-directory-conditional-access-azure-portal/11.png)

Bir koşul deyimi, denetimleri ile birlikte bir koşullu erişim ilkesi temsil eder.

![denetimi](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Denetimler

Bir koşullu erişim ilkesi, bir koşul deyimi yerine getirdiğinizde olacağını nedir denetimleri tanımlayın.  
Denetimler ile için erişimi engellemek veya ek gereksinimler erişimle izin verebilirsiniz.
Erişim sağlayan bir ilkesi yapılandırdığınızda, en az bir gereksinim seçmeniz gerekir.  

İki tür denetimi vardır: 

- **Verme denetimlerini** -verme denetimleri yöneten bir kullanıcı, kimlik doğrulamasını tamamlamak ve oturum için açmak için denediğiniz kaynak ulaşmak olup olmadığına bakılmaksızın. Seçilen birden çok denetim varsa, ilkenizi işlendiğinde, bunların tümünün gerekli olup olmadığını yapılandırabilirsiniz.
Geçerli uygulama Azure Active Directory aşağıdaki grant denetimi gereksinimlerini yapılandırmanıza olanak sağlar:

    ![denetimi](./media/active-directory-conditional-access-azure-portal/73.png)

- **Oturum denetimleri** -oturum bir bulut uygulaması deneyiminde sınırlama etkinleştir denetler. Oturum denetimleri bulut uygulamaları tarafından zorunlu tutulmaz ve oturumla ilgili uygulama için Azure AD tarafından sağlanan ek bilgileri kullanır.

    ![denetimi](./media/active-directory-conditional-access-azure-portal/31.png)


Daha fazla bilgi için bkz: [Azure Active Directory koşullu erişim denetimleri](active-directory-conditional-access-controls.md).


## <a name="condition-statement"></a>Koşul deyimi

Önceki bölümde engellemek veya denetimlerin formunda kaynaklarınıza erişimi kısıtlamak için desteklenen seçenekler için sunulan. Bir koşullu erişim ilkesi, bir koşul deyimi formunda uygulanacak denetimleri için karşılanması gereken ölçütleri tanımlayın.  

Aşağıdaki atamaları, bir koşul deyimi içerebilir:

![denetimi](./media/active-directory-conditional-access-azure-portal/07.png)


### <a name="who"></a>Kim?

Koşullu erişim ilkesi yapılandırdığınızda, kullanıcılar veya gruplar, ilkenin uygulandığı seçmeniz gerekir. Çoğu durumda, belirli bir kullanıcı kümesi için uygulanacak denetimlerinizi istiyor. Bir koşul deyimi gerekli kullanıcılar ve gruplar, ilkenin uygulandığı seçerek bu kümesi tanımlayabilirsiniz. Gerekirse, ilkeden muaf tutma tarafından bir kullanıcı kümesini açıkça dışlayabilirsiniz.  

![denetimi](./media/active-directory-conditional-access-azure-portal/08.png)



### <a name="what"></a>Ne?

Bir koşullu erişim ilkesi yapılandırdığınızda, ilkenin uygulandığı bulut uygulamalarını seçmeniz gerekir.
Genellikle, belirli uygulamalar vardır, koruma açısından bakıldığında, diğerlerinden daha fazla dikkat gerektiren ortamınızdaki. Bu, örneğin, hassas verilere erişimi uygulamaları etkiler.
Bulut uygulamalarını seçerek, ilkenin uygulandığı bulut uygulamalarını kapsamını tanımlayın. Gerekirse, ilkeden uygulamaları kümesi de açıkça hariç tutabilirsiniz.

![denetimi](./media/active-directory-conditional-access-azure-portal/09.png)

Koşullu erişim ilkenizi kullanabileceğiniz bulut uygulamalarının tam listesi için bkz: [Azure Active Directory koşullu erişim teknik başvuru](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).

### <a name="how"></a>Nasıl mı?

Uygulamalarınıza kontrol edebilirsiniz koşullarda gerçekleştirilen sürece, bulut uygulamalarınızı kullanıcılarınız tarafından nasıl erişilir üzerinde ek denetimler izlenmesi için gerekli olabilir. Ancak, noktalar, bulut uygulamalarınızı erişimi, örneğin, güvenilir olmayan ağlara veya uyumlu olmayan cihazlar gerçekleştirilirse farklı görünebilir. Bir koşul deyimi, uygulamalarınıza nasıl gerçekleştirildiğini için ek gereksinimlerin belirli erişim koşulları tanımlayabilirsiniz.

![Koşullar](./media/active-directory-conditional-access-azure-portal/01.png)


## <a name="conditions"></a>Koşullar

Azure Active Directory geçerli uygulama, aşağıdaki alanlar için koşullar tanımlayabilirsiniz:

- Oturum açma riski
- Cihaz platformları
- Konumlar
- İstemci uygulamaları


![Koşullar](./media/active-directory-conditional-access-azure-portal/01.png)

### <a name="sign-in-risk"></a>Oturum açma riski

Azure Active Directory tarafından bir oturum açma denemesi olasılığını izlemek için kullanılan bir nesne meşru bir kullanıcı hesabı sahibi tarafından gerçekleştirilmedi bir oturum açma riski oluşturur. Bu nesne olasılığını (yüksek, Orta veya düşük) adlı bir öznitelik biçiminde depolanır [oturum açma risk düzeyi](active-directory-reporting-risk-events.md#risk-level). Oturum açma riskleri Azure Active Directory tarafından algılanmış ise bu nesne bir oturum açma sırasında bir kullanıcının oluşturulur. Daha fazla bilgi için bkz. [Riskli oturum açma işlemleri](active-directory-identityprotection.md#risky-sign-ins).  
Oturum açma hesaplanan risk düzeyi, bir koşullu erişim ilkesi koşulu olarak kullanabilirsiniz. 

![Koşullar](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Cihaz platformları

Cihaz platformu, Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir:

- Android
- iOS
- Windows Phone
- Windows
- macOS (Önizleme). 

![Koşullar](./media/active-directory-conditional-access-azure-portal/02.png)

Bir ilkeden muaf tutulan cihaz platformları yanı sıra dahil edilen cihaz platformları tanımlayabilirsiniz.  
Cihaz platformları ilkesinde kullanmak için önce yapılandırma değiştirir değiştirmeniz **Evet**ve tüm seçin veya bireysel cihaz platformları ilkesi uygulanır. Tek tek cihaz platformlarını seçin İlkesi bu platformlarda yalnızca bir etkisi yoktur. Bu durumda, oturum açma işlemlerine diğer desteklenen platformlar ilke tarafından etkilenmez.


### <a name="locations"></a>Konumlar

Konumları ile bir bağlantı girişimini gelen başlatıldığı üzerinde temel koşullarını tanımlamak için seçeneğiniz vardır. Konumları listesinde ya da girişler **konumları adlı** veya **MFA güvenilen IP'leri**.  

**Konumları adlı** konumları bağlantı denemeleri yapılmış olan için etiketleri tanımlamanıza olanak verir Azure Active Directory özelliğidir. Bir konum tanımlamak için ya da bir IP yapılandırabilirsiniz adres aralıklarını veya seçin bir ülke / bölge.  

![Koşullar](./media/active-directory-conditional-access-azure-portal/42.png)

Ayrıca, adlandırılmış bir konumu güvenilir konumu olarak işaretleyebilirsiniz. Bir koşullu erişim ilkesi için güvenilir bir konumdayken seçmenize olanak sağlayan başka bir filtre seçenektir *tüm Güvenilen Konumlar* konumları koşulunda.
Adlandırılmış konumlara algılanması bağlamında önemli de [risk olayları](active-directory-reporting-risk-events.md) risk olayı alışılmadık konumlara imkansız seyahat için yanlış pozitif uyarıların sayısını azaltmak için. 

Adlandırılmış konumu yapılandırabilirsiniz Azure AD'de ilgili nesne boyutuyla sınırlıdır. Aşağıdakileri yapılandırabilirsiniz:
 
 - Bir adlandırılmış konumla en fazla 500 IP aralıkları
 - En fazla 60 adlandırılmış (Önizleme) konumlarla her birine atanan bir IP aralığı 

Daha fazla bilgi için bkz: [Azure Active Directory konumlarında adlı](active-directory-named-locations.md).


**MFA güvenilen IP'leri** kuruluşunuzun yerel intranet temsil eden güvenilen IP adres aralıklarını tanımlamanızı sağlayan çok faktörlü kimlik doğrulaması, bir özelliğidir. Bir konum koşul yapılandırdığınızda, güvenilen IP'ler, kuruluşunuzun ağ ve diğer tüm konumlara yapılan bağlantılar arasında ayrım sağlar. Daha fazla bilgi için bkz: [güvenilen IP'leri](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

Koşullu erişim ilkenizi şunları yapabilirsiniz:

- İçerir
    - Herhangi bir konumu
    - Tüm güvenilen konumları
    - Seçilen konumları
- Hariç tutma
    - Tüm güvenilen konumları
    - Seçilen konumları
     
![Koşullar](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-apps"></a>İstemci uygulamaları

İstemci uygulama bir genel düzeyde Azure Active Directory'ye bağlamak için kullanılan uygulama (web tarayıcısı, mobil uygulama, masaüstü istemcisi) ya da olabilir veya Exchange Active Sync özellikle seçebilirsiniz.  
Eski kimlik doğrulaması modern kimlik doğrulaması kullanmayan eski Office istemcileri gibi temel kimlik doğrulaması kullanan istemciler ifade eder. Koşullu erişim eski kimlik doğrulaması ile şu anda desteklenmiyor.

![Koşullar](./media/active-directory-conditional-access-azure-portal/04.png)


Koşullu erişim ilkenizi kullanabileceğiniz istemci uygulamaları tam bir listesi için bkz: [Azure Active Directory koşullu erişim teknik başvuru](active-directory-conditional-access-technical-reference.md#client-apps-condition).




## <a name="common-scenarios"></a>Genel senaryolar

### <a name="requiring-multi-factor-authentication-for-apps"></a>Çok faktörlü kimlik doğrulaması gerektiren uygulamalar için

Çoğu ortam daha yüksek düzeyde koruma diğerlerinden gerektiren uygulamalar vardır.
Bu, örneğin, hassas verilere erişimi uygulamalar için geçerlidir.
Bu uygulamalar için bir başka koruma katmanı eklemek istiyorsanız, kullanıcılar bu uygulamaları erişirken, çok faktörlü kimlik doğrulaması gerektiren bir koşullu erişim ilkesi yapılandırabilirsiniz.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Güvenilir olmayan ağlara erişim için çok faktörlü kimlik doğrulama gerektirme

Çok faktörlü kimlik doğrulama gereksinimini eklediğinden bu önceki senaryoya benzer bir senaryodur.
Bununla birlikte, ikisi arasındaki temel fark bu gereksinim durumdur.  
Önceki senaryoda odak sensitve verilere erişim uygulamalarını ederken güvenilen konumlarının bu senaryonun odak noktasıdır.  
Diğer bir deyişle, bir uygulama güvenmediğiniz bir ağdan bir kullanıcı tarafından erişilen, çok faktörlü kimlik doğrulama gereksinimini olabilir.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Yalnızca güvenilir cihazlar Office 365 hizmetlerine erişebilir

Ortamınızda Intune kullanıyorsanız, Azure konsolunda koşullu erişim ilkesi arabirimini kullanarak hemen başlatabilirsiniz.

Birçok Intune müşterileri, yalnızca güvenilir cihazların Office 365 hizmetlerine erişmesini sağlamak için koşullu erişim kullanıyor. Bu, mobil cihazları Intune'a kayıtlı ve uyumluluk ilkesi gereksinimlerini ve Windows bilgisayarları için bir şirket içi etki alanına katılmış olan anlamına gelir. Önemli geliştirmelerden her Office 365 Hizmetleri için aynı ilkesini ayarlamanız gerekmez ' dir.  Yeni bir ilke oluşturduğunuzda, her biri, koşullu erişim ile korumak istediğiniz O365 uygulamalar dahil etmek için bulut uygulamalarını yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
