---
title: -Nasıl gerekli yönetilen cihazlar için Azure Active Directory koşullu erişim ile bulut uygulama erişimi | Microsoft Docs
description: Bulut uygulama erişimi için yönetilen cihazları gerektiren Azure Active Directory (Azure AD) cihaz tabanlı koşullu erişim ilkelerinin nasıl yapılandırılacağını öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.subservice: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75f55f1058537da255a2611f544239f693615678
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60354800"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Nasıl Yapılır: Koşullu erişim ile bulut uygulaması erişimi için yönetilen cihazları gerektirir

Bir mobil öncelikli ve bulut öncelikli dünyada, Azure Active Directory (Azure AD), çoklu oturum açma için uygulama ve hizmetlere her yerden sağlar. Yetkili kullanıcıların bulut uygulamalarınızdaki çok geniş bir yelpazede mobil ve ayrıca kişisel cihazlar dahil olmak üzere cihazları erişebilirsiniz. Ancak, birçok ortamda yalnızca güvenlik ve uyumluluğa yönelik standartlarınızı karşılayan cihazlar tarafından erişilmesi gereken en az birkaç uygulama var. Bu cihazlar olarak da bilinen yönetilen cihazlardır. 

Bu makalede, ortamınızda belirli bulut uygulamalarına erişmek için yönetilen cihazları gerektiren bir koşullu erişim ilkelerini nasıl yapılandırabileceğiniz açıklanmaktadır. 


## <a name="prerequisites"></a>Önkoşullar

Yönetilen cihazlar için bulut uygulama erişimi TIES gerektiren **Azure AD koşullu erişim** ve **Azure AD cihaz Yönetimi** birlikte. Bu alanlardan biri ile aşina değilseniz, aşağıdaki konularda ilk şöyle olmalıdır:

- **[Azure Active Directory'de koşullu erişim](../active-directory-conditional-access-azure-portal.md)**  -bu makalede bir kavramsal genel bakış ilgili terminoloji ve koşullu erişimi sağlar.

- **[Azure Active Directory'de cihaz yönetimine giriş](../devices/overview.md)**  -bu makalede, bir genel kuruluş denetimi altında cihazları almak için sahip olduğunuz çeşitli seçenekler sağlar. 


## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi Uzmanlaşma kolay değildir. Bulut kaynaklarınıza erişmek için desteklenen cihazlar çoğalan kullanıcılarınızın verimliliğini artırmaya yardımcı olur. Diğer taraftan, bilinmeyen koruma düzeyine sahip cihazlar tarafından erişilecek, ortamınızdaki belirli kaynaklara büyük olasılıkla istemezsiniz. Etkilenen kaynaklar için kullanıcılar yalnızca yönetilen bir cihazı kullanarak erişebildiğinizden istemeniz gerekir. 

Azure AD koşullu erişim ile erişim veren tek bir ilke ile ilgili bu gereksinim karşılayabilirsiniz:

- Seçilen bulut uygulamaları için

- Seçili kullanıcılar ve gruplar için

- Yönetilen bir cihazı gerektirme


## <a name="managed-devices"></a>Yönetilen cihazlar  

Basit bir deyişle, yönetilen cihazlar altında olan cihazlardır *bazı sıralama* kuruluş denetimi. Azure AD'de yönetilen bir cihazı Azure AD'ye kaydedilmiş önkoşuldur. Bir cihaz kaydedilirken cihaz için bir kimlik bir cihaz nesnesi biçiminde oluşturur. Bu nesne, bir cihaz hakkında durum bilgileri izlemek için Azure tarafından kullanılır. Azure AD Yöneticisi, bu nesneye (etkinleştir/devre dışı bırak) bir cihaz durumunu değiştir halihazırda kullanabilirsiniz.
  
![Cihaz temelli koşullar](./media/require-managed-devices/32.png)

Azure AD'ye kayıtlı bir cihazı almak için üç seçeneğiniz vardır:

- **[Azure AD'ye kayıtlı cihazlar](../devices/overview.md#azure-ad-registered-devices)**  - kişisel bir cihazı Azure AD'ye kayıtlı almak için

- **[Azure AD'ye katılmış cihazları](../devices/overview.md#azure-ad-joined-devices)**  - kuruluş bir şirket içi katılmamış bir Windows 10 cihazına almak için AD, Azure AD'ye kayıtlı. 

- **[Hibrit Azure AD'ye katılmış cihazları](../devices/overview.md#hybrid-azure-ad-joined-devices)**  - Windows 10 veya bir şirket içi katıldığından desteklenen alt düzey cihaz almak için AD, Azure AD'ye kayıtlı.

Yönetilen bir cihazı olmak için kayıtlı bir cihazı olmalıdır bir **hibrit Azure AD'ye katılmış cihaz** veya **uyumlu olarak işaretli cihaz**.  

![Cihaz temelli koşullar](./media/require-managed-devices/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Gerekli hibrit Azure AD'ye katılmış cihazlar

Koşullu erişim ilkenizi seçtiğiniz **hibrit Azure AD'ye katılmış hizmet gerektir** seçilen bulut uygulamaları yalnızca bir yönetilen CİHAZDAN erişilebilir olduğunu belirtir. 

![Cihaz temelli koşullar](./media/require-managed-devices/10.png)

Bu ayar yalnızca Windows 10 veya Windows 7 veya Windows 8 gibi bir şirket içi katılmış alt düzey cihazları için geçerlidir AD. Yalnızca olan bir hibrit Azure AD'ye katılma kullanarak Azure AD ile bu cihazları kaydedebilirsiniz bir [işlem otomatik](../devices/hybrid-azuread-join-plan.md) kayıtlı Windows 10 cihazın alınamıyor. 

![Cihaz temelli koşullar](./media/require-managed-devices/45.png)

Cihaz hibrit Azure AD'ye kılan yönetilen bir cihazı alanına?  Bir şirket içi katılan cihazlar için AD, yönetim çözümleri gibi kullanarak bu cihazlar üzerinde denetim zorunlu varsayılır **System Center Configuration Manager (SCCM)** veya **Grup İlkesi (GP)** bunları yönetmek için. Bu yöntemlerin herhangi biriyle uygulanmış olup olmadığını bir cihaza belirlemek Azure AD için hiçbir yöntemi olduğundan, hibrit Azure AD'ye katılmış gerektiren bir yönetilen cihazın gerektirmek için görece zayıf bir mekanizmadır. Cihazları böyle bir cihaz hibrit Azure AD'ye katılmış ise, yönetilen bir cihazı niteliğinde sağlam, şirket içi için uygulanan yöntemleri etki alanına katılmış olup olmadığını değerlendirmek için yönetici size kalmıştır olduğu.


## <a name="require-device-to-be-marked-as-compliant"></a>Cihazın uyumlu olarak işaretlenmesini gerektir

Seçeneğini *bir cihazın uyumlu olarak işaretlenmesini gerektir* yönetilen bir cihazı istemek için en güçlü biçimidir.

![Cihaz temelli koşullar](./media/require-managed-devices/11.png)

Bu seçenek, bir cihaz Azure AD'ye kayıtlı olması ve uyumlu olarak işaretlenmesini gerektirir:
         
- Intune.
- Azure AD tümleştirmesi aracılığıyla Windows 10 cihazları yöneten bir üçüncü taraf mobil cihaz Yönetimi (MDM) sistemi. Windows 10 dışında cihaz işletim sistemi türleri için üçüncü taraf MDM sistemleri desteklenmez.
 
![Cihaz temelli koşullar](./media/require-managed-devices/46.png)



Uyumlu olarak işaretlenmiş bir cihaz için kabul edilebilir: 

- Çalışanlarınızın şirket verilerine erişmek için kullandığı mobil cihazlar olarak yönetilir
- Çalışanlarınızın kullandığı mobil uygulamaları yönetilir
- Şirket bilgilerinizi çalışanlarınızın erişim ve Paylaşım yöntemlerinin denetlenmesine yardımcı olarak korunuyor
- Cihaz ve kendi uygulamaları şirket güvenlik gereksinimleriyle uyumlu




## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda bir cihaz tabanlı koşullu erişim ilkesi yapılandırmadan önce bir göz atın [Azure Active Directory'de koşullu erişim için en iyi yöntemler](best-practices.md).

