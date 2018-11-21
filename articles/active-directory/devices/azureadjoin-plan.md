---
title: Azure Active Directory (Azure AD) birleştirme sürecinizi planlamak nasıl | Microsoft Docs
description: Ortamınızda katılmış cihazların Azure AD'ye uygulamanız için gerekli adımları açıklar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: fdc8b5db9316e463f8ec46ca5e235ea53cf44265
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275981"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Nasıl yapılır: Azure AD join uygulamanızı planlama


Azure AD'ye katılım, Azure AD kullanıcılarınızın üretken ve güvende tutarken şirket içi Active Directory'ye katılmasını gerek kalmadan doğrudan cihazları birleştirmek olanak tanır.  

Bu makalede, Azure AD ile aşinalık olduğunu varsayar ve [Azure AD'de cihaz durumları](overview.md). 

 

## <a name="review-your-scenarios"></a>Senaryolarınız gözden geçirin 

Azure AD'ye katılım bir kuruluş ortamında çoğu senaryo için geçerli olsa da, bunun yerine hibrit Azure AD'ye katılma dağıtmak için isteyebileceğiniz belirli kullanım örnekleri olabilir. Kurumsal kullanıma hazır Azure AD'ye katılım ölçekli veya kapsam dağıtımlar için. 

 
Göz önünde Azure AD'ye katılım, kuruluşunuz için aşağıdaki ölçütleri temel:  

- Plan ya da Windows 10'da Windows cihazlarınızı çoğunluğu sahip. 

- Bir bulut tabanlı dağıtım ve Windows cihazların yönetimi için taşıma planlayın. 

- Şirket içi altyapı sınırlı veya kendi şirket içi ayak izini azaltmak planlama. 

- Cihazları yönetmek için Grup İlkeleri üzerinde yoğun bir bağımlılık yoktur. 

- Active Directory makine kimlik doğrulamasını kullanan eski uygulamaları yok. 

- Tüm veya seçili kullanıcılar, kuruluşunuzda gerekli uygulamaları ve kaynaklarına erişmek için kurumsal ağ üzerinde olması gerekmez.  

 

Aşağıdaki değerlendirmesini temel alan, senaryolarını gözden geçirme  

 

## <a name="assess-infrastructure"></a>Altyapı değerlendirin  

 

### <a name="users"></a>Kullanıcılar 

Kullanıcılarınız şirket içi Active Directory'de oluşturduysanız, bunların Azure AD Connect aracılığıyla Azure AD'ye eşitlenmesi gerekir. Kullanıcıları eşitleme hakkında daha fazla bilgi için bkz: [Azure AD Connect nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity#what-is-azure-ad-connect) 

Kullanıcılarınızın doğrudan Azure AD'de oluşturduysanız, ek kurulum gerekli değildir 

 

Alternatif oturum açma kimliklerini Azure AD'ye katılmış cihazlarda desteklenmez. Kullanıcıların Azure AD'de geçerli bir UPN olmalıdır.  

 

### <a name="identity-infrastructure"></a>Kimlik altyapısı 

Azure AD'ye katılım hem yönetilen hem de Federasyon ortamlar ile çalışır.  

#### <a name="managed-environment"></a>Yönetilen ortam 

Parola Karması eşitleme ya da kimlik doğrulaması ile sorunsuz çoklu oturum açma geçiş aracılığıyla yönetilen ortamlarda dağıtılabilir. <read more here> 

#### <a name="federated-environment"></a>Federasyon ortamı  

Birleştirilmiş bir ortamda hem WS-Güven ve WS-Federasyon protokollerini destekleyen bir kimlik sağlayıcısına sahip olmalıdır. WS-Federasyon, bir cihaz Azure AD'ye gereklidir ve WS-Trust, bir Azure AD alanına katılmış cihaz oturum açmak için gereklidir. Kimlik sağlayıcınız onları desteklemiyorsa, bu nedenle, Azure AD'ye katılım çalışamaz. 


#### <a name="smartcards-and-certificate-based-authentication"></a>Akıllı kart ve sertifika tabanlı kimlik doğrulaması 

 

Azure AD'ye katılmış cihazlarda çalışmıyordu için şu anda Akıllı kart ve sertifika tabanlı kimlik doğrulaması Azure AD'de desteklenmiyor 

 

### <a name="devices"></a>Cihazlar 

 

#### <a name="supported-devices"></a>Desteklenen cihazlar 

Windows 10 cihazlarını Azure AD'ye katılım özeldir. Windows veya diğer işletim sistemlerinin önceki sürümleri için geçerli değildir. Windows 7/8.1 hala varsa, Azure AD'ye katılım'ı dağıtmak için Windows 10'a yükseltmelisiniz. 

 

Öneri: En son Windows 10 sürüm için güncelleştirilen özellikler her zaman kullanın. 

 

### <a name="applications--other-resources"></a>Uygulamalar ve diğer kaynaklar 

Uygulamalarınızı bulut için daha iyi kullanıcı deneyimini ve erişim denetimi için şirket içinden geçiş öneririz. Ancak, Azure AD'ye katılmış cihazların, sorunsuz bir şekilde hem şirket içi erişim sağlar ve bulut uygulamalarına. Daha fazla bilgi için [nasıl SSO için şirket içi kaynakları çalışır Azure AD alanına katılmış cihazlar](azuread-join-sso.md).  


Farklı türde uygulamalar ve kaynaklar için ilgili önemli noktalar aşağıda verilmiştir 

 

- **Bulut tabanlı uygulamalar:** bir uygulama için Azure AD uygulama galerisinde eklenirse, kullanıcılarınızın Azure AD üzerinden SSO alanına katılmış cihazlar. Ek yapılandırma gerekli değildir 

 

- **Şirket içi web uygulamaları:** uygulamalarınızı özel olması durumunda yerleşik ve/veya şirket içinde barındırılan, tarayıcınızın güvenilen siteler eklemeniz gerekir. Bu çalışma ve kullanıcılar için istem yok, SSO bir deneyim sağlamak Windows tümleşik kimlik doğrulamasını etkinleştirir. AD FS kullanıyorsanız bkz [doğrulayın ve çoklu oturum açma AD FS ile yönetme](https://docs.microsoft.com/en-us/previous-versions/azure/azure-services/jj151809(v%3dazure.100)) daha fazla bilgi için. Öneri: (örneğin, Azure) bulutta barındırma ve daha iyi bir deneyim için Azure AD ile tümleştirme göz önünde bulundurun. 

- **Şirket içi uygulamalara eski protokollerine bağlı olan:** kullanıcılarınızın Azure ad SSO alanına katılmış cihazlar cihaz görebilmesi için etki alanı denetleyicisi varsa. Öneri: Bu uygulamalar için güvenli erişim sağlamak için Azure AD uygulama ara sunucusu dağıtın. Daha fazla bilgi için [neden daha iyi bir çözüm uygulama proxy'si?](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy#why-is-application-proxy-a-better-solution) 

- **Şirket içi ağ paylaşımlara:** kullanıcılarınızın kendi ağ paylaşımlara SSO Azure ad alanına katılmış cihazlar bir etki alanı denetleyicisine bağlantısı cihazını sahip olduğunda. Daha fazla bilgi için [nasıl SSO için şirket içi kaynakları çalışır Azure AD alanına katılmış cihazlar](azuread-join-sso.md).

 

- **Yazıcılar:** Azure AD'den yazıcılarını bulmak için Dağıt karma bulut yazdırma alanına katılmış cihazlar. Alternatif olarak, kullanıcılar bunları doğrudan eklemek için de yazıcıların UNC yolunu kullanabilirsiniz. Yazıcılar yalnızca bulut ortamında otomatik olarak bulunamıyor. 

 

- **Şirket içi makine kimlik doğrulamasını bağlı olan uygulamalara:** bu uygulamaları Azure AD'ye katılmış cihazlarda desteklenmez. Öneri: Bu uygulamaları devre dışı bırakma ve bunların modern alternatifleri taşımayı düşünün.  

 

### <a name="device-management"></a>Cihaz yönetimi  

 

Cihaz Yönetimi Azure AD'ye katılmış cihazlar için öncelikle bir EMM Platformu (örneğin, Intune) ve MDM CSP ' dir. Windows 10 ile uyumlu tüm EMM çözümleri çalışır yerleşik MDM Aracısı var.  

 

Active Directory'ye bağlı değil olarak grup ilkeleri, Azure AD'ye katılmış cihazlarda desteklenmez.  

 

Grup ilkeleri kullanılarak MDM İlkesi eşlik değerlendirmek [MDM geçiş analiz aracı [MMAT](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/THR3002R). EMM çözümü yerine grup ilkeleri kullanarak Uygulanabilirlik belirlemek için desteklenen ve desteklenmeyen ilkelerini gözden geçirin. Desteklenmeyen ilkeleri için göz önünde bulundurun:  

- Desteklenmeyen ilkeleri kullanıcılar için gerekli olan veya cihazları Azure AD'ye katılım dağıtılıyor? 

- Desteklenmeyen ilkeleri dağıtım yönetilen bir bulutta geçerlidir? 

 

EMM çözümünüzü Azure AD uygulama galerisinde kullanılabilir durumda değilse, bunu bölümünde açıklanan işlemi izleyerek ekleyebilirsiniz [MDM ile Azure Active Directory Tümleştirme](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm).

 

Comanagement SCCM ilkeleri, EMM platformu teslim edilir olsa bazı yönlerini cihazları yönetmek için kullanılabilir. Microsoft Intune comanagement SCCM ile sağlar. Daha fazla bilgi için [ortak yönetim için Windows 10 cihazları](https://docs.microsoft.com/sccm/core/clients/manage/co-management-overview). Intune dışında bir EMM ürünü kullanıyorsanız, Lütfen geçerli bir ortak yönetim senaryolarında EMM sağlayıcınızla denetleyin.  

 

Azure AD'ye katılmış cihazları yönetme geniş iki yolu vardır: 

 

- **Yalnızca MDM** -cihaz Intune gibi bir EMM sağlayıcısı tarafından özel olarak yönetilir. Tüm ilkeler, MDM kayıt işleminin bir parçası teslim edilir. Azure AD Premium veya EMS müşterileri için Azure AD'ye katılım bir parçası olarak otomatik MDM kaydı olmaktan çıkar. 

- **Ortak yönetim** -cihaz SCCM ve EMM sağlayıcısı tarafından eşzamanlı olarak yönetilir. Bu yaklaşımda, SCCM aracının belirli yönlerini yönetmek için bir MDM ile yönetilen cihaza yüklenir. 

Öneri: MDM yönetimi için Azure AD'ye katılmış cihazlar yalnızca göz önünde bulundurun.  

 

## <a name="configuration"></a>Yapılandırma 

 

Azure AD'ye katılım belirli bazı ayarlarınızı temel alan Azure AD portalında yapılandırılabilir. Git `Devices -> Device settings` ve aşağıdaki seçenekleri yapılandırın:   

- Kullanıcılar cihazları Azure AD'ye Katıl: Bu ayarı "Tüm" veya dağıtımınızın kapsamını tabanlı "Seçili".  

- Ek yerel Yöneticiler Azure AD alanına katılmış cihazlar: "Seçili" ve seçer kullanıcılar için tüm Azure AD üzerinde local administrators grubuna eklemek isteyen kuruluşunuza dağıtmış katılmış cihazlar. Daha fazla bilgi için [katılmış cihazların Azure AD yerel Yöneticiler grubuna yönetme](assign-local-admin.md). 

- Cihazları eklemek çok öğeli kimlik doğrulama gerektiren: kullanıcılar cihazları Azure AD'ye katılma sırasında MFA gerçekleştirmek gerekiyorsa "Evet" olarak ayarlayın. Mfa'yı kullanarak Azure AD'de cihaz birleştiren kullanıcı için 2 Faktörlü cihazı haline gelir. 

Böylece cihaz ayarlarına eşitleyebilir, Azure AD'ye durumda Dolaşım etkinleştirmek istiyorsanız, bkz. [ne olduğu kurumsal durumda Dolaşım?](enterprise-state-roaming-overview.md). Karma Azure için bile bu ayarı etkinleştirmenizi öneriyoruz AD alanına katılmış cihazlar 

### <a name="deployment-options"></a>Dağıtım seçenekleri 

 

Azure AD'ye katılım üç farklı yaklaşımlara dağıtılabilir:  

- **Self Servis OOBE/ayarlarında** - modunda Self Servis kullanıcıları Azure AD'ye katılım aracılığıyla Git ayarları ya da deneyimi (OOBE) veya Windows dışı sırasında Windows işlem.  

- **Windows Autopilot** -Windows Autopilot cihazların Azure AD'ye katılma işlemi için OOBE içinde daha sorunsuz bir deneyim için yapılandırma öncesi sağlar.   

- **Toplu kayıt** -toplu kayıt cihazları yapılandırmak için bir toplu sağlama aracı kullanarak bir yönetici yönetilen Azure AD'ye katılmasını sağlamaya olanak tanır.  


Bu üç yaklaşımların bir karşılaştırması aşağıdadır. 

 
||Self Servis Kurulumu|Windows Autopilot|Toplu kayıt|
|---|---|---|---|
|Ayarlamak için kullanıcı etkileşimi gerektirir|Evet|Evet|Hayır|
|BT çaba gerektirir|Hayır|Evet|Evet|
|Geçerli akışlar|OOBE & ayarları|Yalnızca OOBE|Yalnızca OOBE|
|Birincil kullanıcı için yerel yönetici hakları|Evet, varsayılan olarak|Yapılandırılabilir|Hayır|
|Cihaz OEM desteği gerektirir|Hayır|Evet|Hayır|
|Desteklenen sürümler|1511 +|1709 +|1703 +|
 
Yukarıdaki tabloda gözden geçirme ve her iki yaklaşımı benimsemeyi aşağıdaki konuları gözden geçirerek, dağıtım yaklaşımını veya yaklaşımları seçin:  

- Kullanıcıların teknik kurulumunu girebilmeleri için deneyimli misiniz? 

    - Self Servis, bu kullanıcılar için en iyi çalışabilir. Windows Autopilot'ı kullanıcı deneyimini iyileştirmek için göz önünde bulundurun.  

- Kullanıcılarınızın, uzak ya da şirket içi içinde misiniz? 

    - Self Servis veya uzak kullanıcılar için sorunsuz bir kurulum için en iyi Autopilot iş. 
 
- Yönetilen bir kullanıcı veya yönetici tarafından yönetilen bir yapılandırma tercih ediyorsunuz? 

    - Toplu kayıt için yönetici kullanıcılara vermekten önce cihazları ayarlamak için dağıtım temelli daha iyi çalışır.     

- 1-2 OEM'ler cihazlar satın veya OEM cihazların geniş bir dağıtım sahip?  

    - Ayrıca Autopilot desteği sınırlı OEM'ler satın alma, Autopilot ile sıkı tümleştirme yararlı olabilir. 
 

 


## <a name="next-steps"></a>Sonraki adımlar

- [Cihaz yönetimi](overview.md)

