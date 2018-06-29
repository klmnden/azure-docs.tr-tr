---
title: -Nasıl gerektiren yönetilen cihazlar Azure Active Directory koşullu erişimle bulut uygulama erişimi için | Microsoft Docs
description: Bulut uygulama erişimi için yönetilen cihazlara gerektiren Azure Active Directory (Azure AD) cihaz temelli koşullu erişim ilkeleri yapılandırma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 066d25e8953a2be4bd64cdd1af79b7f2a25dd5f9
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036361"
---
# <a name="how-to-require-managed-devices-for-cloud-app-access-with-conditional-access"></a>Nasıl yapılır: Gerektiren yönetilen cihazlar için koşullu erişimle bulut uygulama erişimi

Bir mobil ilk olarak, bulut ilk dünyasında, Azure Active Directory (Azure AD) çoklu oturum açma uygulamalara ve hizmetlere her yerden sağlar. Yetkili kullanıcıların geniş kapsamlı bir dizi cihazı mobil ve aynı zamanda kişisel aygıtlar dahil olmak üzere, bulut uygulamalarınızı erişebilir. Bununla birlikte, çoğu ortam yalnızca, güvenlik ve uyumluluk standartlarına uygun cihazlar tarafından erişilmesi gereken en az birkaç uygulama var. Bu cihazlar olarak da bilinen yönetilen cihazlardır. 

Bu makalede, ortamınızda belirli bulut uygulamalara erişmek için yönetilen cihazları gerektirir koşullu erişim ilkelerini nasıl yapılandırabilirsiniz açıklanmaktadır. 


## <a name="prerequisites"></a>Önkoşullar

Yönetilen cihazlar için bulut uygulama erişimi TIES gerektiren **Azure AD koşullu erişimi** ve **Azure AD cihaz Yönetimi** birlikte. Bu alanlardan biri ile tanıdık değilse, aşağıdaki konularda ilk şöyle olmalıdır:

- **[Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)**  -bu makalede bir kavramsal genel bakış koşullu erişim ve ilgili terminolojiyi sağlar.

- **[Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)**  -bu makalede sahip aygıtlar kuruluş denetimindeki almak için çeşitli seçenekler genel bir bakış sağlar. 


## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi yönetimine uygun bir sorundur. Bulut kaynaklarınıza erişmek için desteklenen aygıtların artışı kullanıcılarınızın verimliliğini artırmaya yardımcı olur. Çevir tarafında bilinmeyen koruma düzeyi ile cihazları tarafından erişilmek üzere ortamınızdaki belirli kaynaklara büyük olasılıkla istemezsiniz. Etkilenen kaynaklar için kullanıcılar yalnızca yönetilen bir cihazı kullanarak bunları erişebilmesini istemeniz gerekir. 

Azure AD koşullu erişimi ile erişim veren tek bir ilke ile bu gereksinim karşılayabilirsiniz:

- Seçilen bulut uygulamaları için

- Seçili kullanıcılar ve gruplar için

- Yönetilen bir cihazı gerektirme


## <a name="managed-devices"></a>Yönetilen cihazlar  

Basitçe, yönetilen cihazlar misiniz cihazlar altında *bazı sıralama* kuruluş denetimi. Azure AD'de yönetilen bir cihaz için Azure AD ile kaydedildi önkoşuldur. Bir cihaz kaydetme aygıtı için bir kimlik bir cihaz nesnesi formunda oluşturur. Bu nesne, bir cihaz hakkındaki durum bilgilerini izlemek için Azure tarafından kullanılır. Azure AD yönetici olarak, bu nesneye geçiş (etkinleştir/devre dışı bırak) aygıtın durumunu zaten kullanabilirsiniz.
  
![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/32.png)

Azure AD ile kayıtlı bir cihaz almak için üç seçeneğiniz vardır:

- **[Azure AD kayıtlı cihazlar](device-management-introduction.md#azure-ad-registered-devices)**  - Azure AD ile kaydedilen kişisel bir cihazı almak için

- **[Azure AD alanına katılmış aygıtlar](device-management-introduction.md#azure-ad-joined-devices)**  - bir şirket içi katılmamış bir kuruluş Windows 10 cihaz almak için AD, Azure AD ile kayıtlı. 

- **[Karma Azure AD alanına katılmış aygıtlar](device-management-introduction.md#hybrid-azure-ad-joined-devices)**  - bir şirket içi birleşik bir Windows 10 cihaz almak için AD, Azure AD ile kayıtlı.

Yönetilen bir aygıt için kayıtlı bir cihaza ya da olmalıdır bir **karma Azure AD alanına cihaz** veya **uyumlu olarak işaretlenmiş aygıt**.  

![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/47.png)

 
## <a name="require-hybrid-azure-ad-joined-devices"></a>Karma Azure gerektiren AD alanına katılmış aygıtlar

Koşullu erişim ilkenizi seçtiğiniz **karma Azure AD birleştirilmiş cihaz gerektiren** seçili bulut uygulamalarını yalnızca yönetilen cihaz kullanılarak erişilebilir olduğunu durumuna. 

![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/10.png)

Bu ayar yalnızca bir şirket içi katılmış Windows 10 cihazları için geçerlidir AD. Bu cihazlar yalnızca olan bir karma Azure AD birleştirme kullanarak Azure AD ile kaydedebilirsiniz bir [işlem otomatik](device-management-hybrid-azuread-joined-devices-setup.md) kayıtlı Windows 10 cihazına almak için. 

![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/45.png)

Azure AD karma kılan bir yönetilen cihaz alanına katılmış bir CİHAZDAN?  Bir şirket içi katılan cihazlar için AD, yönetimi çözümleri gibi kullanarak, bu cihazlar üzerinde denetim uygulandığını varsayılır **System Center Configuration Manager (SCCM)** veya **Grup İlkesi (GP)** bunları yönetmek için. Bu yöntemlerin herhangi biriyle uygulanmış olup olmadığını bir aygıta belirlemek Azure AD için bir yöntem olduğundan, karma Azure AD birleştirilmiş cihaz gerektiren bir yönetilen cihaz gerektirecek şekilde görece zayıf bir mekanizmadır. Bu size, şirket içi için uygulanan yöntemleri etki alanına katılmış olup olmadığını aygıtları böyle bir cihazı aynı zamanda karma Azure AD birleştirilmiş cihaz ise bir yönetilen cihaz niteliğinde güçlü değerlendirmek için bir yönetici olarak bağlıdır.


## <a name="require-device-to-be-marked-as-compliant"></a>Cihazın uyumlu olarak işaretlenmesini gerektir

Seçeneğini *uyumlu olarak işaretlenecek bir aygıt gerektirir* yönetilen cihaz istemek için güçlü biçimidir.

![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/11.png)

Bu seçenek, Azure AD ile kayıtlı olması ve uyumlu olarak işaretlenecek bir aygıt gerektirir:
         
- Intune.
- Azure AD tümleştirme yoluyla Windows 10 cihazları yöneten bir üçüncü taraf mobil cihaz Yönetimi (MDM) sistemi. Üçüncü taraf MDM sistemler için Windows 10 dışında aygıt işletim sistemi türleri desteklenmez.
 
![Cihaz tabanlı koşulları](./media/active-directory-conditional-access-policy-connected-applications/46.png)



Uyumlu olarak işaretlenmiş bir aygıt için çalıştığını kabul edebilirsiniz: 

- Şirket verilerine erişmek için iş gücü kullanan mobil cihazları yönetilen
- Yönetilen tanıdıkları kullanan mobil uygulamalar
- Şirket bilgilerinizi tanıdıkları erişir ve bu paylaşım şeklini denetlemek için yardımcı olarak korunuyor
- Cihaz ve onun uygulamaları şirket güvenlik gereksinimleriyle uyumlu




## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda bir cihaz temelli koşullu erişim ilkesini yapılandırmadan önce bir göz atalım [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md).

