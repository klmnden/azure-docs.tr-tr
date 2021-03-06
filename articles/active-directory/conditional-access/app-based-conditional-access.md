---
title: Bulut uygulama erişimi Azure Active Directory'de koşullu erişim için onaylı istemci uygulamalarını zorunlu kılma | Microsoft Docs
description: Bulut uygulama erişimi Azure Active Directory'de koşullu erişim için onaylı istemci uygulamalarını gerektiren öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff4e81f5533622c8afbfb0d7c683c76b530aefec
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509498"
---
# <a name="how-to-require-approved-client-apps-for-cloud-app-access-with-conditional-access"></a>Nasıl Yapılır: Koşullu erişim ile bulut uygulaması erişimi için onaylı istemci uygulama gerektir 

Çalışanlarınızın mobil cihazları hem kişisel hem de iş görevleri için kullanın. Çalışanlarınızın üretken olabilirler, ancak ayrıca veri kayıplarını da önlemek istersiniz. Azure Active Directory (Azure AD) koşullu erişimle şirket verilerinizi koruyabilirsiniz onaylı istemci uygulamaları için bulut uygulamalarınıza erişimi kısıtlayabilirsiniz.  

Bu konu, onaylı istemci uygulamalarını gerektiren koşul erişim ilkelerinin nasıl yapılandırılacağını açıklar.

## <a name="overview"></a>Genel Bakış

İle [Azure AD koşullu erişim](overview.md), hassas ayarlamalar yapabilirsiniz kaynaklarınızı yetkili kullanıcıların erişebildiğinden nasıl. Örneğin, bulut uygulamalarınıza güvenilen cihazlara erişimi sınırlayabilirsiniz.

Kullanabileceğiniz [Intune uygulama koruma ilkeleri](https://docs.microsoft.com/intune/app-protection-policy) Şirketinizin verilerini korumaya yardımcı olmak için. Intune uygulama koruma ilkeleri, cihazları bir cihaz Yönetimi çözümüne kaydederek veya kaydetmeden Şirketinizin verilerini korumanıza olanak tanır, mobil cihaz Yönetimi (MDM) çözümü gerektirmez.

Azure Active Directory koşullu erişimi Intune uygulama koruma ilkelerini destekleyen istemci uygulamaları için bulut uygulamalarınıza erişimi sınırlamanıza olanak sağlar. Örneğin, erişimi Exchange Online için Outlook uygulamasında kısıtlayabilirsiniz.

Koşullu erişim terminolojisinde, bu istemci uygulamaları olarak bilinen **onaylı istemci uygulamalar**.  

![Koşullu Erişim](./media/app-based-conditional-access/05.png)

Onaylı istemci uygulamalarının listesi için bkz. [onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement).

Diğer ilkeleri ile uygulama tabanlı koşullu erişim ilkeleri gibi birleştirebilirsiniz [cihaz tabanlı koşullu erişim ilkeleri](require-managed-devices.md) kişisel ve kurumsal aygıtlar için veri koruma konusunda esneklik sağlamak için.

## <a name="before-you-begin"></a>Başlamadan önce

Bu konu, aşina olduğunuzu varsayar:

- [Onaylı istemci uygulaması gereksinimi](technical-reference.md#approved-client-app-requirement) teknik başvuru.
- Temel kavramlarını [Azure Active Directory'de koşullu erişim](overview.md).
- Nasıl yapılır [bir koşullu erişim ilkesini yapılandırma](app-based-mfa.md).
- [Koşullu erişim ilkeleri geçişini](best-practices.md#policy-migration).

## <a name="prerequisites"></a>Önkoşullar

Bir uygulama tabanlı koşullu erişim ilkesi oluşturmak için Enterprise Mobility + Security veya bir Azure Active Directory premium aboneliğinizin olması ve kullanıcıların EMS veya Azure AD lisansına sahip olması gerekir. 

## <a name="exchange-online-policy"></a>Exchange Online İlkesi 

Bu senaryo, bir erişim Exchange Online için uygulama tabanlı koşullu erişim ilkesi oluşur.

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

![Koşullu Erişim](./media/app-based-conditional-access/01.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

   ![Koşullu Erişim](./media/app-based-conditional-access/07.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**:
   1. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

      ![Koşullu Erişim](./media/app-based-conditional-access/03.png)

   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve Masaüstü uygulamaları** ve **Modern kimlik doğrulaması istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/91.png)

1. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

   ![Koşullu Erişim](./media/app-based-conditional-access/05.png)

**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/06.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**.

   ![Koşullu Erişim](./media/app-based-conditional-access/07.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları (Önizleme)** . 
   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/92.png)

   1. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

      ![Koşullu Erişim](./media/app-based-conditional-access/05.png)

**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**

![Koşullu Erişim](./media/app-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.

## <a name="exchange-online-and-sharepoint-online-policy"></a>Exchange Online ve SharePoint Online İlkesi

Bu senaryo, mobil uygulama yönetimi ilkesi ile Exchange Online'a erişimi için bir koşullu erişim ve SharePoint Online ile onaylı uygulamalar oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:

- Bağlanmak ve kurumsal sitelerini görüntülemek için SharePoint uygulaması kullanmayı dener
- Outlook uygulaması kimlik bilgileri ile aynı kimlik bilgileri ile oturum dener
- Yeniden kaydetmeniz gerekmez ve kaynaklara erişin

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/71.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

   ![Koşullu Erişim](./media/app-based-conditional-access/02.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**:
   1. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

      ![Koşullu Erişim](./media/app-based-conditional-access/03.png)

   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/91.png)

1. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

   ![Koşullu Erişim](./media/app-based-conditional-access/05.png)

**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/06.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. Çevrimiçi 

   ![Koşullu Erişim](./media/app-based-conditional-access/07.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**:
   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/92.png)

   1. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.

      ![Koşullu Erişim](./media/app-based-conditional-access/05.png)

**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**

![Koşullu Erişim](./media/app-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.

## <a name="app-based-or-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı veya uyumlu bir cihaz İlkesi

Bu senaryo, bir uygulama tabanlı veya uyumlu cihaz koşullu erişim ilkesi için Exchange Online'a erişimini oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar:
 
- Bazı kullanıcılar (ile veya olmadan şirket cihazları) zaten kayıtlı
- Uygulama kaynaklarına erişmek için bir cihazı kaydetme gerek olmayan kaydedilmiş ve kullanarak bir uygulamayı Azure AD'ye kayıtlı kullanıcılar korumalı
- Kayıtlı kullanıcıların korunan uygulama uygulamasını kullanarak cihazı yeniden kaydetmeniz gerekmez

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/62.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

     ![Koşullu Erişim](./media/app-based-conditional-access/02.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
   1. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

      ![Koşullu Erişim](./media/app-based-conditional-access/03.png)

   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Modern kimlik doğrulaması istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/91.png)

1. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:
   - **Cihazın uyumlu olarak işaretlenmesini gerektir**
   - **(Önizleme) onaylı istemci uygulaması gerektir**
   - **Seçilen denetimlerden birini gerektir**   
 
      ![Koşullu Erişim](./media/app-based-conditional-access/11.png)

**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/61.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

   ![Koşullu Erişim](./media/app-based-conditional-access/07.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları**. 

   Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

   ![Koşullu Erişim](./media/app-based-conditional-access/91.png)

1. Olarak **erişim denetimleri**, ihtiyacınız **(Önizleme) onaylı istemci uygulaması gerektir** seçili.
 
   ![Koşullu Erişim](./media/app-based-conditional-access/11.png)

**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**

![Koşullu Erişim](./media/app-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.

## <a name="app-based-and-compliant-device-policy-for-exchange-online-and-sharepoint-online"></a>Exchange Online ve SharePoint Online için uygulama tabanlı ve uyumlu bir cihaz İlkesi

Bu senaryo, bir uygulama tabanlı ve uyumlu bir cihaz koşullu erişim ilkesi için Exchange Online'a erişimini oluşur.

### <a name="scenario-playbook"></a>Senaryo playbook

Bu senaryoyu olduğunu varsayar. kullanıcı:
 
- İOS veya Android üzerinde bir Exchange'e bağlanmak için bir yerel posta uygulaması kullanarak e-posta yapılandırır
- Erişimi Cihazınızı kaydedilmesi gerektiğini belirten bir e-posta alır
- Şirket portalı'nı yükler ve şirket portalında oturum açmaktadır
- Posta denetler ve Outlook uygulaması kullanmasını istenir.
- Outlook uygulamasını indirir
- Outlook uygulamasını açar ve kaydında kullanılan kimlik bilgilerini girer.
- Kullanıcı e-postaya

Herhangi bir Intune uygulama koruma İlkesi zaman kurumsal verilere erişim etkinleştirilir ve kullanıcıdan uygulamayı yeniden başlatın, ek bir PIN vs. kullan (uygulama ve platform için yapılandırılmışsa)

### <a name="configuration"></a>Yapılandırma

**1. adım: Exchange Online ve SharePoint Online için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/62.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online** ve **Office 365 SharePoint Online**. 

   ![Koşullu Erişim](./media/app-based-conditional-access/02.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **cihaz platformlarını** ve **istemci uygulamaları**. 
   1. Olarak **cihaz platformlarını**seçin **Android** ve **iOS**.

      ![Koşullu Erişim](./media/app-based-conditional-access/03.png)

   1. Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve Masaüstü uygulamaları** ve **Modern kimlik doğrulaması istemcileri**.

      ![Koşullu Erişim](./media/app-based-conditional-access/91.png)

1. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:
   - **Cihazın uyumlu olarak işaretlenmesini gerektir**
   - **(Önizleme) onaylı istemci uygulaması gerektir**
   - **Seçilen tüm denetimleri gerekli kıl**   
 
      ![Koşullu Erişim](./media/app-based-conditional-access/13.png)



**2. adım: Exchange Online ile Active Sync (EAS) için bir Azure AD koşullu erişim ilkesi yapılandırma**

Bu adımda koşullu erişim ilkesi için aşağıdaki bileşenleri yapılandırmanız gerekir:

![Koşullu Erişim](./media/app-based-conditional-access/61.png)

1. **Adı** , koşullu erişim ilkesi.
1. **Kullanıcılar ve gruplar**: Her bir koşullu erişim ilkesi, en az bir kullanıcı veya grup seçili olması gerekir.
1. **Bulut uygulamaları:** Bulut uygulamaları seçmeniz gerekir. **Office 365 Exchange Online**. 

   ![Koşullu Erişim](./media/app-based-conditional-access/07.png)

1. **Koşullar:** Olarak **koşullar**, yapılandırmanız gereken **istemci uygulamaları (Önizleme)** . 

   Olarak **istemci uygulamaları (Önizleme)** seçin **mobil uygulamalar ve masaüstü istemciler** ve **Exchange ActiveSync istemcileri**.

   ![Koşullu Erişim](./media/app-based-conditional-access/92.png)

1. Olarak **erişim denetimleri**, aşağıdaki seçili olması gerekir:
   - **Cihazın uyumlu olarak işaretlenmesini gerektir**
   - **(Önizleme) onaylı istemci uygulaması gerektir**
   - **Seçilen tüm denetimleri gerekli kıl**   
 
      ![Koşullu Erişim](./media/app-based-conditional-access/64.png)

**Adım 3 - iOS ve Android istemci uygulamaları için Intune uygulama koruma ilkesi yapılandırma**

![Koşullu Erişim](./media/app-based-conditional-access/09.png)

Bkz: [Intune ile uygulamaları ve verileri koruma](https://docs.microsoft.com/intune-classic/deploy-use/protect-apps-and-data-with-microsoft-intune) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [gerektiren MFA belirli uygulamalar için Azure Active Directory koşullu erişim ile](app-based-mfa.md).

Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md). 
