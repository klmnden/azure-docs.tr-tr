---
title: Azure Active Directory'de koşullu erişim ile bulut uygulaması erişimi için uygulama koruma ilkesini zorunlu kılma | Microsoft Docs
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
ms.openlocfilehash: f695d50e251d0104cf9f0d38fe4489a0e66dfe15
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288509"
---
# <a name="how-to-require-app-protection-policy-for-cloud-app-access-with-conditional-access-preview"></a>Nasıl Yapılır: Koşullu erişim (Önizleme) ile bulut uygulaması erişimi için uygulama koruma İlkesi gerektirir.

Çalışanlarınızın mobil cihazları hem kişisel hem de iş görevleri için kullanın. Çalışanlarınızın üretken olabilirler, ancak ayrıca veri kayıplarını da önlemek istersiniz. Azure Active Directory (Azure AD) koşullu erişim ile bir uygulama koruma ilkesi olan istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlayarak şirket verilerinizi koruyabilirsiniz.

Bu konu, uygulama koruma İlkesi önce verilere erişim gerektiren koşullu erişim ilkelerinin nasıl yapılandırılacağını açıklar.

## <a name="overview"></a>Genel Bakış

İle [Azure AD koşullu erişim](overview.md), hassas ayarlamalar yapabilirsiniz kaynaklarınızı yetkili kullanıcıların erişebildiğinden nasıl. Örneğin, bulut uygulamalarınıza güvenilen cihazlara erişimi sınırlayabilirsiniz.

Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) Şirketinizin verilerini korumaya yardımcı olmak için. Intune uygulama koruma ilkeleri, cihazları bir cihaz Yönetimi çözümüne kaydederek veya kaydetmeden Şirketinizin verilerini korumanıza olanak tanır, mobil cihaz Yönetimi (MDM) çözümü gerektirmez.

Azure Active Directory koşullu erişimi Intune uygulama koruma ilkesi alma olarak Azure AD'ye bildirilen istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlar. Örneğin, erişimi Exchange Online için Intune uygulama koruma ilkesi olan Outlook uygulamasında kısıtlayabilirsiniz.

Koşullu erişim terminolojisinde, bu istemci uygulamaları ile korumalı İlkesi olduğu bilinen bir **uygulama koruma İlkesi**.  

![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)

Korumalı istemci uygulamaları, ilke listesi için bkz. [uygulama koruma İlkesi gereksinimi](technical-reference.md#approved-client-app-requirement).

App-protection-tabanlı koşullu erişim ilkeleriyle diğer ilkeleri gibi birleştirebilirsiniz [cihaz tabanlı koşullu erişim ilkeleri](require-managed-devices.md) kişisel ve kurumsal aygıtlar için veri koruma konusunda esneklik sağlamak için.

## <a name="benefits-of-app-protection-based-conditional-access-requirement"></a>Uygulama koruma tabanlı koşullu erişim gereksinimi avantajları

Koşullu erişim bu bir erişim denetimi kullanabilmesi için uygulama koruma İlkesi uygulanırsa uyumluluk benzer iOS ve Android için Intune tarafından raporlanan cihaz, Intune artık Azure ad raporlar yönetilen. Bu yeni koşullu erişim ilkesi **uygulama koruma İlkesi** yönetici hatalarına karşı koruyarak güvenliği artırır:

- bir Intune lisansı olmayan kullanıcılar
- Intune uygulama koruma İlkesi alamıyor kullanıcılar
- Bir ilkeyi almak için yapılandırılmamış olan Intune uygulama koruma İlkesi uygulamalarının


## <a name="before-you-begin"></a>Başlamadan önce

Bu konu, aşina olduğunuzu varsayar:

- [Uygulama koruma İlkesi gereksinimi](technical-reference.md#app-protection-policy-requirement) teknik başvuru.

- [Onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement) teknik başvuru.

- Temel kavramlarını [Azure Active Directory'de koşullu erişim](overview.md).

- Nasıl yapılır [bir koşullu erişim ilkesini yapılandırma](app-based-mfa.md).


## <a name="prerequisites"></a>Önkoşullar

Uygulama koruma tabanlı koşullu erişim ilkesi oluşturma için şunları yapmanız gerekir
- Enterprise Mobility + Security veya bir Azure Active Directory premium aboneliğinizin + Intune sahip
- EMS veya Azure AD kullanıcıların lisansına sahip olmak + Intune
- İstemci uygulama, Intune uygulama koruma İlkesi almak için yapılandırıldığından emin olun
- Kullanıcılara bir Intune uygulama koruma ilkesini almak için Intune'da yapılandırıldığından emin olun

## <a name="app-protection-based-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı İlkesi

Bu senaryo, bir uygulama koruma tabanlı koşullu erişim ilkesi için Exchange Online'a erişimini oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- İOS veya Android üzerinde bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır

- Erişim yalnızca Outlook uygulamasını kullanırken kullanılabilir olduğunu bildiren bir e-posta alır

- Uygulama bağlantısını içeren indirir

- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında

- Authenticator (iOS) veya şirket Portalı'nı (Android) devam etmek için yüklemesi istenir

- Devam etmek için Outlook uygulamasına yükler uygulama ve can döndürür

- Bir cihazı kaydetmeleri istenir.

- Intune uygulama koruma İlkesi alabilir

- E-posta erişemiyor

Herhangi bir Intune uygulama koruma ilkesi için Kurumsal verilere erişmek için uygulamayı olmalıdır ve kullanıcıdan uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa).

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/01.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**:

    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve Masaüstü uygulamaları** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Olarak **erişim denetimleri**, ihtiyacınız **uygulama koruma İlkesi (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)
 

**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.


3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları (Önizleme)**. 

    a. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

    b. Olarak **erişim denetimleri**, ihtiyacınız **uygulama koruma İlkesi (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/05.png)


**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.



## <a name="app-protection-based-or-compliant-device-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı veya uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için bir uygulama koruma tabanlı veya uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar:
 
- Bazı kullanıcı (ile veya olmadan şirket cihazları) zaten kayıtlı

- Uygulama kaynaklarına erişmek için bir cihazı kaydetme gerek olmayan kaydedilmiş ve kullanarak bir uygulamayı Azure AD'ye kayıtlı kullanıcılar korumalı

- Kayıtlı kullanıcıların korunan uygulama uygulamasını kullanarak cihazı yeniden kaydetmeniz gerekmez

- Kullanıcı bir Intune uygulama koruma İlkesi alabilir değilse kayıtlı

- Kullanıcı e-postaya Outlook ve Intune uygulama koruma İlkesi ile değilse kayıtlı

- Kullanıcı, cihaz kaydedilirse Outlook e-posta erişebilir


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma ilkesi gerektir (önizleme)**

   - **Seçilen denetimlerden birini gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/11.png)



**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma ilkesi gerektir (önizleme)**

   - **Seçilen denetimlerden birini gerekli kıl**   
    ![Koşullu erişim](./media/app-protection-based-conditional-access/11.png)



**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.





## <a name="app-protection-based-and-compliant-device-policy-for-exchange-online"></a>Exchange Online için uygulama koruma tabanlı ve uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için app-protection-tabanlı ve uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:
 
-   İOS veya Android üzerinde bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır
-   Erişimi Cihazınızı kaydedilmesi gerektiğini belirten bir e-posta alır
-   Şirket portalı'nı yükler ve şirket portalında oturum açmaktadır
-   Posta denetler ve Outlook uygulaması kullanmasını istenir.
-   Outlook uygulamasını indirir
-   Outlook uygulamasını açar ve kaydında kullanılan kimlik bilgilerini girer.
-   Bir Intune uygulama koruma ilkesini almak için alamıyor
-   Outlook ve Intune uygulama koruma İlkesi ile e-postaya erişebilir

Tüm Intune uygulama koruma ilkeleri şirket verilerine erişim önce etkinleştirilir ve kullanıcıdan uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa)


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/01.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve Masaüstü uygulamaları** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **Uygulama koruma ilkesi gerektir (önizleme)**

   - **Seçilen tüm denetimleri gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/13.png)



**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/app-protection-based-conditional-access/07.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları (Önizleme)**. 

    Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/92.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

   - **Cihazın uyumlu olarak işaretlenmesini gerektir**

   - **(Önizleme) onaylı istemci uygulaması gerektir**

   - **Seçilen tüm denetimleri gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/13.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.


## <a name="app-protection-based-or-app-based-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama koruma veya uygulama tabanlı İlkesi

Bu senaryo erişim Exchange Online ve SharePoint Online için uygulama koruma tabanlı veya onaylı uygulamalar İlkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- Yapılandırır ya da istemci uygulamalar uygulama koruma İlkesi gereksinimi veya onaylı uygulamalar gereksinim destekleyen uygulamaların listesi.  
- Uygulama koruma İlkesi gereksinimini karşılamayan kullanıcı kullanan istemci uygulamalar Intune uygulama koruma İlkesi alabilir.
- Intune uygulama koruma ilkesini destekleyen onaylı uygulamalar İlkesi gereksinimini karşılamayan istemci uygulamaları kullanıcı kullanır
- E-posta veya belge erişmek için uygulama açılır
- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında
- Authenticator (iOS) veya şirket Portalı'nı (Android) devam etmek için yüklemek isteyip istemediğiniz sorulur (olmasa bile yüklü)
- Devam etmek için Outlook uygulamasına yükler uygulama ve can döndürür
- Bir cihazı kaydetmeleri istenir.
- Intune uygulama koruma İlkesi alabilir
- Outlook ve Intune uygulama koruma İlkesi ile e-postaya erişebilir
- Site/uygulama koruma İlkesi gereksinimi yer almayan bir uygulama ile belgelere erişmek kullanabilirsiniz, ancak listelenen Onaylı uygulama gereksinimi olur.

Tüm Intune uygulama koruma ilkeleri şirket verilerine erişim önce gereklidir ve kullanıcıdan uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa)

**Açıklamalar**

- Her iki uygulama koruması ve uygulama tabanlı koşullu erişim ilkelerini desteklemek istiyorsanız, bu senaryoyu kullanabilirsiniz.

- Bu *veya* İlkesi, uygulamalarla **uygulama koruma İlkesi** gereksinim erişimi için değerlendirilir önce **onaylı istemci uygulamalar** gereksinim.

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/app-protection-based-conditional-access/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

     ![Koşullu erişim](./media/app-protection-based-conditional-access/02.png)

4. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/03.png)

    b. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve Masaüstü uygulamaları** ve **Modern kimlik doğrulaması istemcileri**.

    ![Koşullu erişim](./media/app-protection-based-conditional-access/91.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

   - **Onaylı istemci uygulaması gerektir**

   - **Uygulama koruma ilkesi gerektir (önizleme)**

   - **Seçilen denetimlerden birini gerekli kıl**   
 
     ![Koşullu erişim](./media/app-protection-based-conditional-access/12.png)


**2. adım - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/app-protection-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.




## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).

Ortamınızda koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz. [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md). 