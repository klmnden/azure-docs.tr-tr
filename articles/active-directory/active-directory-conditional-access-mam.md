---
title: Azure Active Directory uygulama tabanlı koşullu erişim | Microsoft Docs
description: Azure Active Directory uygulama tabanlı koşullu erişim nasıl çalıştığını öğrenin.
services: active-directory
keywords: uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: d7409c98a37c0455a95840cb2cdff4c3c7dbf52d
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39531709"
---
# <a name="azure-active-directory-app-based-conditional-access"></a>Azure Active Directory uygulama tabanlı koşullu erişim  

Çalışanlarınızın mobil cihazları hem kişisel hem de iş görevleri için kullanın. Çalışanlarınızın üretken olabilirler, ancak ayrıca veri kayıplarını da önlemek istersiniz. Azure Active Directory (Azure AD) uygulama tabanlı koşullu erişim ile şirket verilerinizi koruyabilirsiniz istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlayabilirsiniz.  

Bu konuda, Azure AD uygulama tabanlı koşullu erişim yapılandırma açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

İle [Azure AD koşullu erişim](active-directory-conditional-access-azure-portal.md), hassas ayarlamalar yapabilirsiniz kaynaklarınızı yetkili kullanıcıların erişebildiğinden nasıl. Örneğin, bulut uygulamalarınıza güvenilen cihazlara erişimi sınırlayabilirsiniz.

Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) Şirketinizin verilerini korumaya yardımcı olmak için. Intune uygulama koruma ilkeleri, cihazları bir cihaz Yönetimi çözümüne kaydederek veya kaydetmeden Şirketinizin verilerini korumanıza olanak tanır, mobil cihaz Yönetimi (MDM) çözümü gerektirmez.

Intune uygulama koruma ilkelerini destekleyen istemci uygulamaları için bulut uygulamalarınıza erişimi sınırlamak azure Active Directory uygulama tabanlı koşullu erişim sağlar. Örneğin, erişimi Exchange Online için Outlook uygulamasında kısıtlayabilirsiniz.

Koşullu erişim terminolojisinde, bu istemci uygulamaları olarak bilinen **onaylı istemci uygulamalar**.  


![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)


Onaylı istemci uygulamalarının listesi için bkz. [onaylı istemci uygulaması gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


Diğer ilkeler uygulama tabanlı koşullu erişim ilkeleriyle gibi birleştirebilirsiniz [cihaz tabanlı koşullu erişim ilkeleri](active-directory-conditional-access-policy-connected-applications.md) kişisel ve kurumsal aygıtlar için veri koruma konusunda esneklik sağlamak için.

 


## <a name="before-you-begin"></a>Başlamadan önce

Bu konu, aşina olduğunuzu varsayar:

- [Onaylı istemci uygulaması gereksinimi](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) teknik başvuru.


- Temel kavramlarını [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).

- Nasıl yapılır [bir koşullu erişim ilkesini yapılandırma](conditional-access/app-based-mfa.md).

- [Koşullu erişim ilkeleri geçişini](conditional-access/best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Önkoşullar

Bir uygulama tabanlı koşullu erişim ilkesi oluşturmak için Enterprise Mobility + Security veya bir Azure Active Directory premium aboneliğinizin olması ve kullanıcıların EMS veya Azure AD lisansına sahip olması gerekir. 


## <a name="exchange-online-policy"></a>Exchange Online İlkesi 

Bu senaryo, Exchange Online'a erişimi için bir uygulama tabanlı koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- İOS veya Android üzerinde bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır

- Erişim yalnızca Outlook uygulamasını kullanırken kullanılabilir olduğunu bildiren bir e-posta alır

- Uygulama bağlantısını içeren indirir

- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında

- Authenticator (iOS) veya şirket Portalı'nı (Android) devam etmek için yüklemesi istenir

- Devam etmek için Outlook uygulamasına yükler uygulama ve can döndürür

- Bir cihazı kaydetmeleri istenir.

- E-posta erişemiyor

Herhangi bir Intune uygulama koruma ilkesi olan zaman şirket verilerine erişmek etkinleştirilir ve uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa) kullanıcıya isteyebilir.

### <a name="configuration"></a>Yapılandırma 

**1. adım: Exchange Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/01.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**:

    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)
 

**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.


3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    a. Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

    b. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)


**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online ve SharePoint Online İlkesi

Bu senaryo, mobil uygulama yönetimi ilkesiyle onaylı uygulamalar ile Exchange Online ve SharePoint Online erişim için bir koşullu erişim oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- Bağlanmak ve kurumsal sitelerini görüntülemek için SharePoint uygulaması kullanmayı dener

- Outlook uygulaması kimlik bilgileri ile aynı kimlik bilgileri ile oturum girişimi

- Yeniden kaydetmeniz gerekmez ve kaynaklara erişin


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/71.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.


3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**:

    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)




**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. Çevrimiçi 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**:

    a. Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

    b. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı veya uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için uygulama tabanlı veya uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar:
 
- Bazı kullanıcı zaten (ile veya olmadan şirket cihazları) kayıtlı

- Uygulama kaynaklarına erişmek için bir cihazı kaydetme gerek olmayan kaydedilmiş ve kullanarak bir uygulamayı Azure AD'ye kayıtlı kullanıcılar korumalı

- Kayıtlı kullanıcıların korunan uygulama uygulamasını kullanarak cihazı yeniden kaydetmeniz gerekmez


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

     ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

    - **Cihazın uyumlu olarak işaretlenmesini gerektir**

    - **(Önizleme) onaylı istemci uygulaması gerektir**

    - **Seçilen denetimlerden birini gerektir**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/11.png)



**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/61.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    Olarak **istemci uygulamaları*seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

5. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/11.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı ve uyumlu bir cihaz İlkesi

Bu senaryo, Exchange Online'a erişimi için uygulama tabanlı ve uyumlu bir cihaz koşullu erişim ilkesi oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:
 
-   İOS veya Android üzerinde bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır
-   Erişimi Cihazınızı kaydedilmesi gerektiğini belirten bir e-posta alır
-   Şirket portalı'nı yükler ve şirket portalında oturum açmaktadır
-   Posta denetler ve Outlook uygulaması kullanmasını istenir.
-   Outlook uygulamasını indirir
-   Outlook uygulamasını açar ve kaydında kullanılan kimlik bilgilerini girer.
-   Kullanıcı e-postaya

Herhangi bir Intune uygulama koruma İlkesi zaman kurumsal verilere erişim etkinleştirilir ve kullanıcıdan uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa)


### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

     ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamaları**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

    - **Cihazın uyumlu olarak işaretlenmesini gerektir**

    - **(Önizleme) onaylı istemci uygulaması gerektir**

    - **Seçilen tüm denetimleri gerekli kıl**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/13.png)



**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/61.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

5. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:

    - **Cihazın uyumlu olarak işaretlenmesini gerektir**

    - **(Önizleme) onaylı istemci uygulaması gerektir**

    - **Seçilen tüm denetimleri gerekli kıl**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/64.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.






## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](conditional-access/app-based-mfa.md).

Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](conditional-access/best-practices.md). 
