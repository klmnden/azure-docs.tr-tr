---
title: Azure Active Directory'de koşullu erişim ile bulut uygulaması erişimi için uygulama koruma İlkesi gerektirir. | Microsoft Docs
description: Bulut uygulama erişimi Azure Active Directory'de koşullu erişim ile uygulama koruma İlkesi iste öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 4/4/2019
ms.author: joflore
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2250449c0ef342332945b80cb10cb9a02885b259
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60356070"
---
# <a name="require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Koşullu erişim (Önizleme) ile bulut uygulaması erişimi için uygulama koruma İlkesi gerektirir.

Çalışanlarınızın mobil cihazları hem kişisel hem de iş görevleri için kullanın. Çalışanlarınızın üretken olabilirler, ancak ayrıca veri kayıplarını da önlemek istersiniz. Azure Active Directory (Azure AD) koşullu erişim ile bulut uygulamalarınıza erişimi kısıtlayarak şirket verilerinizi koruyabilirsiniz. İstemci uygulamalar uygulama koruma İlkesi ilk olarak kullanın.

Bu makalede, verilere erişim sağlanmadan önce gerektiren bir uygulama koruma İlkesi koşullu erişim ilkelerinin nasıl yapılandırılacağını açıklar.

## <a name="overview"></a>Genel Bakış

İle [Azure AD koşullu erişim](overview.md), hassas ayarlamalar yapabilirsiniz kaynaklarınızı yetkili kullanıcıların erişebildiğinden nasıl. Örneğin, bulut uygulamalarınıza güvenilen cihazlara erişimi sınırlayabilirsiniz.

Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) Şirketinizin verilerini korumaya yardımcı olmak için. Intune uygulama koruma ilkeleri, mobil cihaz Yönetimi (MDM) çözümü gerektirmez. Şirketinizin verilerini içeren veya içermeyen hesabından cihazların kaydını bir cihaz Yönetimi çözümüne koruyabilirsiniz.

Azure Active Directory koşullu erişimi Intune uygulama koruma ilkesi alma olarak Azure AD'ye bildirilen istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlar. Örneğin, erişimi Exchange Online için Intune uygulama koruma ilkesi olan Outlook uygulamasında kısıtlayabilirsiniz.

Koşullu erişim terminolojisinde, bu istemci uygulamaları ile korumalı İlkesi olduğu bilinen bir *uygulama koruma İlkesi*.  

![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)

İlkeyle korunan istemci uygulamaların bir listesi için bkz. [uygulama koruma İlkesi gereksinimi](technical-reference.md#approved-client-app-requirement).

Gibi diğer ilkeleri, uygulama koruma-tabanlı koşullu erişim ilkeleriyle birleştirebilirsiniz [cihaz tabanlı koşullu erişim ilkeleri](require-managed-devices.md). Bu şekilde, kişisel ve kurumsal aygıtlar için veri koruma konusunda esneklik sağlayabilirsiniz.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Uygulama koruma tabanlı koşullu erişim gereksinimi avantajları

Intune tarafından yönetilen cihaz için Intune raporları bir uygulama koruma İlkesi, Azure AD'ye uygulandığında artık iOS ve Android için bildirilir uyumluluk benzer. Koşullu erişim Bu ilke, bir erişim denetimi kullanabilirsiniz. Uygulama koruma İlkesi bu yeni koşullu erişim ilkesi, güvenliği artırır. Gibi yönetici hatalarına karşı korur:

- Bir Intune lisansı bulunmayan kullanıcılar.
- Kullanıcılar Intune uygulama koruma İlkesi alamıyor.
- Bir ilkeyi almak için yapılandırılmış olmayan Intune uygulama koruma İlkesi uygulamalarının.


## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, alışık olduğunuz varsayılır:

- [Uygulama koruma İlkesi gereksinimi](technical-reference.md#app-protection-policy-requirement) teknik başvuru.
- [Onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement) teknik başvuru.
- Temel kavramlarını [Azure Active Directory'de koşullu erişim](overview.md).
- Nasıl yapılır [bir koşullu erişim ilkesini yapılandırma](app-based-mfa.md).


## <a name="prerequisites"></a>Önkoşullar

Uygulama koruma tabanlı koşullu erişim ilkesi oluşturmak için şunları yapmalısınız:

- Enterprise Mobility + Security veya bir Azure Active Directory premium aboneliğinizin + Intune vardır.
- Enterprise Mobility + Security veya Azure AD kullanıcıların lisansına sahip olduğundan emin olun ve Intune.
- İstemci uygulama, Intune uygulama koruma İlkesi almak için yapılandırıldığından emin olun.
- Kullanıcılara bir Intune uygulama koruma ilkesini almak için Intune'da yapılandırıldığından emin olun.

## <a name="app-protection-based-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı İlkesi

Bu senaryo, bir uygulama koruma tabanlı koşullu erişim ilkesi için Exchange Online'a erişimini oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- E-posta, bir yerel posta uygulaması iOS veya Android üzerinde Exchange'e bağlanmak için kullanarak yapılandırır.
- Yalnızca Outlook uygulaması kullanarak erişim kullanılabilir olduğunu belirten bir e-posta alır.
- Uygulama bağlantısını içeren indirir.
- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında.
- İOS kullanmak için Microsoft Authenticator ya da devam etmek Android kullanmak için Intune Şirket portalı yüklemesi istenir.
- Uygulamayı yükler ve devam etmek için Outlook uygulamasına döndürür.
- Bir cihazı kaydetmeleri istenir.
- Intune uygulama koruma İlkesi alabilir.
- E-postalarına erişebilir.

Herhangi bir Intune uygulama koruma İlkesi, Kurumsal verilere erişmek için uygulamayı olması gerekir. İlkeler uygulamayı yeniden başlatın veya ek bir PIN girmesini isteyebilir. İlkeleri, platform ve uygulama için yapılandırılmış olması halinde bu durum geçerlidir.

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/01.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **cihaz platformlarını** ve **istemci uygulamaları (Önizleme)** :

    a. İçinde **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Altında **erişim denetimleri**seçin **uygulama koruma İlkesi (Önizleme) gerektiren**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)
 

**2. adım: Çevrimiçi Exchange ActiveSync (EAS) ile bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.


3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **istemci uygulamaları (Önizleme)** . 

    a. İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

    b. Altında **erişim denetimleri**seçin **uygulama koruma İlkesi (Önizleme) gerektiren**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)


**3. adım: İOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Daha fazla bilgi için [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı veya uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için bir uygulama koruma tabanlı veya uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar:
 
- İle veya olmadan şirket cihazları bir kullanıcı zaten kaydedilir.
- Kayıtlı ve bir uygulama kullanarak Azure AD'ye kayıtlı olmayan kullanıcılar, kaynaklara erişmek için bir cihazı kaydetmek için uygulama ihtiyaçlarını korumalı.
- Korumalı uygulama uygulaması kullanan kayıtlı kullanıcıların cihazı yeniden kaydetmeniz gerekmez.
- Kullanıcının bir Intune uygulama koruma İlkesi alabilir değilse kayıtlı.
- Kullanıcı e-postaya Outlook ve Intune uygulama koruma İlkesi ile değilse kayıtlı.
- Kullanıcı, cihaz kaydedilirse Outlook e-posta erişim sağlayabilir.


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/62.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **cihaz platformlarını** ve **istemci uygulamaları (Önizleme)** . 
 
    a. İçinde **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Altında **erişim denetimleri**, aşağıdaki seçenekleri belirleyin:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma İlkesi (Önizleme) gerektirir**

   - **Seçilen denetimlerden birini gerektir**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/11.png)



**2. adım: Çevrimiçi Exchange ActiveSync ile bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **istemci uygulamaları (Önizleme)** . 

    İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

5. Altında **erişim denetimleri**, aşağıdaki seçenekleri belirleyin:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma İlkesi (Önizleme) gerektirir**

   - **Seçilen denetimlerden birini gerektir**

     ![Koşullu erişim](./media/app-protection-based-conditional-access/11.png)



**3. adım: İOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Daha fazla bilgi için [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı ve uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için app-protection-tabanlı ve uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:
 
-   E-posta, bir yerel posta uygulaması iOS veya Android üzerinde Exchange'e bağlanmak için kullanarak yapılandırır.
-   Erişimi cihazlarının kaydedilmesi gerektiğini belirten bir e-posta alır.
-   Intune şirket Portalı'nı indirip portalında oturum açmaktadır.
-   Posta denetler ve Outlook uygulaması kullanmasını istenir.
-   Outlook uygulamasını indirir.
-   Outlook uygulamasını açar ve kaydında kullanılan kimlik bilgilerini girer.
-   Intune uygulama koruma İlkesi alabilir.
-   Outlook ve Intune uygulama koruma İlkesi ile e-postaya erişebilir.

Kurumsal verilere erişim sağlanmadan önce herhangi bir Intune uygulama koruma ilkesi etkinleştirilir. İlkeler uygulamayı yeniden başlatın veya ek bir PIN girmesini isteyebilir. İlkeleri, platform ve uygulama için yapılandırılmış olması halinde bu durum geçerlidir.


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/01.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **cihaz platformlarını** ve **istemci uygulamaları (Önizleme)** . 
 
    a. İçinde **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Altında **erişim denetimleri**, aşağıdaki seçenekleri belirleyin:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma İlkesi (Önizleme) gerektirir**

   - **Seçilen tüm denetimleri gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/13.png)



**2. adım: Çevrimiçi Exchange ActiveSync ile bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. İçinde **koşullar**, yapılandırma **istemci uygulamaları (Önizleme)** . 

    İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

5. Altında **erişim denetimleri**, aşağıdaki seçenekleri belirleyin:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma İlkesi (Önizleme) gerektirir**

   - **Seçilen tüm denetimleri gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/13.png)




**3. adım: İOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Daha fazla bilgi için [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama koruma veya uygulama tabanlı İlkesi

Bu senaryo erişim Exchange Online ve SharePoint Online için uygulama koruma tabanlı veya onaylı uygulamalar İlkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- Yapılandırır ya da istemci uygulamalar uygulama koruma İlkesi gereksinimi veya onaylı uygulamalar gereksinim destekleyen uygulamaların listesi.  
- Intune uygulama koruma İlkesi alabilir ve uygulama koruma İlkesi gereksinimini karşılamayan istemci uygulamaları kullanır.
- Intune uygulama koruma ilkesini destekleyen onaylı uygulamalar İlkesi gereksinimini karşılamayan istemci uygulamaları kullanır.
- E-posta veya belge erişmek için uygulama açılır.
- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında.
- Henüz yüklenmemişse iOS kullanmak için Microsoft Authenticator ya da Android kullanmak için Intune Şirket portalı yüklemesi istenir.
- Devam etmek için Outlook uygulamasına yükler uygulama ve can döndürür.
- Bir cihazı kaydetmeleri istenir.
- Intune uygulama koruma İlkesi alabilir.
- Outlook ve Intune uygulama koruma İlkesi ile e-postaya erişebilir.
- Site ve uygulama koruma İlkesi gereksinimi yer almayan bir uygulama ile belgeleri erişebilir, ancak listelenen Onaylı uygulama gereksinimi.

Kurumsal verilere erişim sağlanmadan önce herhangi bir Intune uygulama koruma İlkesi gerekli değildir. İlkeler uygulamayı yeniden başlatın veya ek bir PIN girmesini isteyebilir. İlkeleri, platform ve uygulama için yapılandırılmış olması halinde bu durum geçerlidir.

**Açıklamalar**

- Her iki uygulama koruması ve uygulama tabanlı koşullu erişim ilkelerini desteklemek istiyorsanız, bu senaryoyu kullanabilirsiniz.
- Bu *veya* ilke, uygulama koruma İlkesi gereksinimi olan uygulamaları ilk önce onaylı istemci uygulamalarını gereksinim erişimi için değerlendirilir.

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırın:

![Koşullu erişim](./media/app-protection-based-conditional-access/62.png)

1. Koşullu erişim ilkenizi adını girin.

2. Altında **atamaları**, **kullanıcılar ve gruplar**, en az bir kullanıcı seçin veya grup için her bir koşullu erişim ilkesi.

3. İçinde **bulut uygulamaları**seçin **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/02.png)

4. İçinde **koşullar**, yapılandırma **cihaz platformlarını** ve **istemci uygulamaları (Önizleme)** . 
 
    a. İçinde **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. İçinde **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Altında **erişim denetimleri**, aşağıdaki seçenekleri belirleyin:

   - **Onaylı istemci uygulaması gerektir**

   - **Uygulama koruma İlkesi (Önizleme) gerektirir**

   - **Seçilen denetimlerden birini gerektir**
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/12.png)


**2. adım: İOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Daha fazla bilgi için [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune).




## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 