---
title: "Azure Active Directory Uygulama temelli koşullu erişim | Microsoft Docs"
description: "Azure Active Directory uygulama tabanlı koşullu erişimin nasıl çalıştığını öğrenin."
services: active-directory
keywords: "uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/07/2017
ms.author: markvi
ms.reviewer: spunukol
ms.openlocfilehash: aaf2da57d8653371ab0b46e47474442aa4be1d65
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-app-based-conditional-access"></a>Azure Active Directory Uygulama temelli koşullu erişim  

Çalışanlarınız hem kişisel hem de iş görevleri için mobil cihazlar kullanır. Çalışanlarınızın üretken olabilirler, ancak aynı zamanda veri kayıplarını önlemek isteyebilirsiniz. Azure Active Directory (Azure AD) uygulama tabanlı ile koşullu erişim, şirket verilerinizi koruyabilirsiniz istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlayabilirsiniz.  

Bu konuda, Azure AD uygulama tabanlı koşullu erişimi nasıl yapılandırılacağı açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

İle [Azure AD koşullu erişimi](active-directory-conditional-access-azure-portal.md), ince ayar yapabilirsiniz nasıl yetkili kullanıcılar, kaynaklara erişebilir. Örneğin, güvenilen cihazlar için bulut uygulamalarınıza erişimi sınırlayabilirsiniz.

Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) Şirketinizin verilerini korumaya yardımcı olmak için. Intune uygulama koruma ilkeleri ve bu sayede cihazları bir cihaz yönetim çözümüne kaydederek veya kaydetmeden, şirketinizin verilerini korumak mobil cihaz Yönetimi (MDM) çözümü gerektirmez.

Intune uygulama koruma ilkeleri destekleyen istemci uygulamaları için bulut uygulamalarınıza erişimi sınırlamak azure Active Directory Uygulama bağlı olarak koşullu erişim sağlar. Örneğin, erişimi Exchange Online için Outlook uygulaması kısıtlayabilirsiniz.

Koşullu erişim terminolojisinde bu istemci uygulamaları olarak bilinen **istemci uygulamaları onaylanan**.  


![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)


Onaylanmış istemci uygulamaları listesi için bkz: [istemci uygulama gereksinimi onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement).


Diğer ilkeleri ile uygulama bağlı olarak koşullu erişim ilkeleri gibi birleştirebilirsiniz [cihaz temelli koşullu erişim ilkeleri](active-directory-conditional-access-policy-connected-applications.md) kişisel ve kurumsal aygıtlar için verileri korumak nasıl esneklik sağlamak için.

 


##<a name="before-you-begin"></a>Başlamadan önce

Bu konu, aşina olduğunuzu varsayar:

- [İstemci uygulama gereksinimi onaylanan](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) teknik başvuru.


- Temel kavramlarını [Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md).

- Nasıl yapılır [koşullu erişim ilkesini yapılandırma](active-directory-conditional-access-azure-portal-get-started.md).

- [Koşullu erişim ilkeleri geçişini](active-directory-conditional-access-best-practices.md#policy-migration).
 

## <a name="prerequisites"></a>Ön koşullar

Bir uygulama bağlı olarak koşullu erişim ilkesi oluşturmak için bir Enterprise Mobility + güvenlik veya bir Azure Active Directory premium aboneliği varsa ve kullanıcıları EMS veya Azure AD için lisansına sahip olması gerekir. 


## <a name="exchange-online-policy"></a>Exchange Online İlkesi 

Exchange Online'a erişimi için bir uygulama bağlı olarak koşullu erişim ilkesi, bu senaryo oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryo varsayar kullanıcı:

- İOS veya Android bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır

- Erişimi yalnızca Outlook uygulamasını kullanarak kullanılabilir olduğunu bildiren bir e-posta alır

- Uygulama bağlantı ile indirir

- Outlook uygulamasını açar ve Azure AD kimlik bilgileriyle oturum açtığında

- Devam etmek için doğrulayıcı (iOS) veya Şirket portalı (Android) yüklemeniz istenir.

- Devam etmek için Outlook uygulaması yükler uygulama ve can Döndür

- Bir cihaz kaydetme istenir.

- E-posta erişebilir

Tüm Intune uygulama koruma ilkelerinin erişim kurumsal verilerin aynı anda etkinleştirilir ve uygulamayı yeniden başlatın, ek bir PIN vb. kullan (uygulama ve platform için yapılandırılmışsa) kullanıcıya isteyebilir.

### <a name="configuration"></a>Yapılandırma 

**1. adım - bir Azure AD koşullu erişim ilkesini Exchange Online için yapılandırın**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/01.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformları** ve **istemci uygulamaları**:

    a. Olarak **cihaz platformları**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamalar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, olmasına gerek **onaylanmış istemci uygulamasını (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)
 

**2. adım - Exchange Online ile ActiveSync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.


3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    a. Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

    b. Olarak **erişim denetimleri**, olmasına gerek **onaylanmış istemci uygulamasını (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)


**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [uygulamaları ve Microsoft Intune ile verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.


## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online ve SharePoint Online İlkesi

Mobil uygulama yönetimi ilkesi ile koşullu erişim erişim Exchange Online ve SharePoint Online için onaylanan uygulamalarıyla, bu senaryo oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryo varsayar kullanıcı:

- SharePoint uygulama bağlanmak ve onların şirket sitelerine görüntülemek için kullanmayı dener

- Oturum Outlook uygulaması kimlik bilgileri ile aynı kimlik bilgileri ile açma girişimi

- Yeniden kayıt olması gerekmez ve kaynaklara erişim elde edebilirsiniz


### <a name="configuration"></a>Yapılandırma

**Adım 1 - Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/71.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.


3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformları** ve **istemci uygulamaları**:

    a. Olarak **cihaz platformları**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamalar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, olmasına gerek **onaylanmış istemci uygulamasını (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)




**2. adım - Exchange Online ile ActiveSync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/06.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. Çevrimiçi 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**:

    a. Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

    b. Olarak **erişim denetimleri**, olmasına gerek **onaylanmış istemci uygulamasını (Önizleme) gerektiren** seçili.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/05.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [uygulamaları ve Microsoft Intune ile verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.


## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı veya uyumlu aygıt İlkesi

Exchange Online'a erişimi için bir uygulama tabanlı veya uyumlu aygıt koşullu erişim ilkesi, bu senaryo oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryo, varsayar:
 
- Bazı kullanıcı zaten (ile veya Kurumsal cihazları olmadan) kayıtlı

- Uygulama kaynaklarına erişmek için bir cihaz kaydetme gerek olmayan kayıtlı ve bir uygulama kullanarak Azure AD ile kayıtlı kullanıcılar korumalı

- Korunan bir uygulamada uygulama kullanarak kayıtlı kullanıcıların cihazı yeniden kaydetmeniz gerekmez


### <a name="configuration"></a>Yapılandırma

**Adım 1 - Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

     ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformları** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformları**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamalar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, seçili şunlara sahip olmanız gerekir:

    - **Cihaz uyumlu olarak işaretlenmesini gerektirir**

    - **Onaylanmış istemci uygulamasını (Önizleme) gerektirir**

    - **Seçili denetimleri birini gerektirir**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/11.png)



**2. adım - Exchange Online ile ActiveSync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/61.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    Olarak **istemci uygulamaları*seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

5. Olarak **erişim denetimleri**, olmasına gerek **onaylanmış istemci uygulamasını (Önizleme) gerektiren** seçili.
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/11.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [uygulamaları ve Microsoft Intune ile verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.





## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı ve uyumlu aygıt İlkesi

Exchange Online'a erişimi için bir uygulama tabanlı ve uyumlu aygıt koşullu erişim ilkesi, bu senaryo oluşur.


### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryo varsayar kullanıcı:
 
-   İOS veya Android bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır
-   Cihazınızı kaydedilmesini erişmesi belirten bir e-posta alır
-   Şirket portalı'nı yükler ve şirket portalında oturum açmaktadır
-   Posta denetler ve Outlook uygulamasını kullanmak için istenir.
-   Outlook uygulamasını yükler
-   Outlook uygulamasını açar ve kaydında kullanılan kimlik bilgilerini girer.
-   Kullanıcı e-postaya erişememesi

Tüm Intune uygulama koruma ilkeleri şirket verilerine erişim için zaman etkinleştirilir ve uygulamayı yeniden başlatın, ek bir PIN vb. kullan (uygulama ve platform için yapılandırılmışsa) kullanıcıya isteyebilir


### <a name="configuration"></a>Yapılandırma

**Adım 1 - Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/62.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

     ![Koşullu erişim](./media/active-directory-conditional-access-mam/02.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **cihaz platformları** ve **istemci uygulamaları**. 
 
    a. Olarak **cihaz platformları**seçin **Android** ve **iOS**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/03.png)

    b. Olarak **istemci uygulamaları**seçin **mobil uygulamalar ve Masaüstü uygulamalar**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/04.png)

5. Olarak **erişim denetimleri**, seçili şunlara sahip olmanız gerekir:

    - **Cihaz uyumlu olarak işaretlenmesini gerektirir**

    - **Onaylanmış istemci uygulamasını (Önizleme) gerektirir**

    - **Seçili denetimleri birini gerektirir**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/11.png)



**2. adım - Exchange Online ile ActiveSync (EAS) için bir Azure AD koşullu erişim ilkesini yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu erişim](./media/active-directory-conditional-access-mam/61.png)

1. **Adı** , koşullu erişim ilkesi.

2. **Kullanıcılar ve gruplar**: her koşullu erişim ilkesinin en az bir kullanıcı veya grup seçili olmalıdır.

3. **Bulut uygulamaları:** bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/07.png)

4. **Koşullar:** olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

    Olarak **istemci uygulamaları**seçin **Exchange Active Sync**.

    ![Koşullu erişim](./media/active-directory-conditional-access-mam/08.png)

5. Olarak **erişim denetimleri**, seçili şunlara sahip olmanız gerekir:

    - **Cihaz uyumlu olarak işaretlenmesini gerektirir**

    - **Onaylanmış istemci uygulamasını (Önizleme) gerektirir**

    - **Seçili tüm denetimler gerektirir**   
 
    ![Koşullu erişim](./media/active-directory-conditional-access-mam/64.png)




**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**


![Koşullu erişim](./media/active-directory-conditional-access-mam/09.png)

Bkz: [uygulamaları ve Microsoft Intune ile verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.






## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
