---
title: Bir temel koruma Azure Active Directory koşullu erişim nedir? -Önizleme | Microsoft Docs
description: Nasıl temel koruma, Azure Active Directory ortamında etkin güvenlik temel düzeyde en az sahip olmasını sağlar. öğrenin.
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
ms.date: 07/02/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1e7eb3a0098dc27b6f3c47d8d4848b2b9b5f7e61
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447549"
---
# <a name="what-is-baseline-protection---preview"></a>Temel koruma nedir? -Önizleme  

Geçen yıl içinde kimlik saldırıları 300 oranında artırmıştır. Ortamınızı sürekli artan saldırılarına karşı korumak için Azure Active Directory (Azure AD) temel koruma adlı yeni bir özellik sunar. Temel koruması olan bir dizi önceden tanımlanmış [koşullu erişim ilkeleri](active-directory-conditional-access-azure-portal.md). Bu ilkelerin en az Azure AD tüm sürümlerinde etkin güvenlik temel düzeyde olmasını sağlamak için hedeftir. 

Bu makalede, Azure Active Directory'de temel koruma genel bir bakış sunulmaktadır.


 
## <a name="require-mfa-for-admins"></a>Yöneticiler için MFA gerektirme

Kullanıcılara ayrıcalıklı hesaplara erişim ortamınıza sınırsız erişimi vardır. Bu hesaplara sahip güç nedeniyle, bunları özel dikkatli düşünmelisiniz. Ayrıcalıklı hesapların geliştirmek için bir ortak oturum açma için kullanıldığında, daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir. Azure Active Directory'ye multi factor authentication (MFA) zorunlu daha güçlü bir hesap doğrulama alabilirsiniz.  

**Yöneticiler için mfa'yı gerekli** şu dizin rolleri için MFA gerektiren bir temel ilke: 

- Genel yönetici  

- SharePoint yöneticisi  

- Exchange yöneticisi  

- Koşullu erişim yöneticisi  

- Güvenlik yöneticisi  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

Bu temel ilke kullanıcıları ve grupları Dışla seçeneği sağlar. Biri hariç tutmak isteyebileceğiniz *[Acil Durum erişimi yönetici hesabı](users-groups-roles/directory-emergency-access.md)* , kilitli dışında Kiracı emin olmak için.


## <a name="enable-a-baseline-policy"></a>Temel ilke etkinleştir 

Temel ilkeleri önizlemede olsa da, bunlar varsayılan olarak etkin. El ile etkinleştirmek istiyorsanız bir İlkesi'ni etkinleştirmeniz gerekir. Bu özellik genel kullanılabilirlik ulaştı hemen sonra varsayılan olarak etkinleştirilen ilkelerdir. Planlanan davranış değişikliği etkinleştirmek ve bir ilke durumunu ayarlamak için üçüncü bir seçenek devre dışı bırakmak için ayrıca sahip olmasının nedeni nedeni: **ilke gelecekte otomatik olarak etkinleştir**. Bu seçeneği belirterek, bir ilkeyi etkinleştirmek ne zaman karar Microsoft olanak tanır.      


**Bir taban çizgisi ilkesini etkinleştirmek için:**  

1. Oturum [Azure portalında](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.

2. İçinde **Azure portalında**, sol gezinti tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-baseline-protection/05.png)

4. İlkeler listesinde ile başlayan bir ilkeye tıklayın **temel ilke:**. 

5. İlkeyi etkinleştirmek için tıklayın **ilkeyi hemen kullan**.

6. **Kaydet**’e tıklayın. 
 


  
 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler 

Özel koşullu erişim ilkelerini yönetme bir Azure AD Premium lisansı gerektirir, ancak temel ilkeleri Azure AD tüm sürümlerinde kullanılabilir.     

Temel ilkeye dahil Dizin rolleri en yüksek ayrıcalıklı bir Azure AD rolleridir. 

Komut dosyalarınızda kullanılan hesapları ayrıcalıklı, değiştirmelisiniz [yönetilen hizmet kimliği (MSI)](./managed-service-identity/overview.md) veya [hizmet sorumluları sertifikalarla](../azure-resource-manager/resource-group-authenticate-service-principal.md). Geçici bir çözüm, belirli kullanıcı hesaplarını temel ilkesinden hariç tutabilirsiniz. 

Temel ilkeleri POP, IMAP, eski Office masaüstü istemcisi gibi eski bir kimlik doğrulama akışları için geçerlidir. 




## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesi yapılandırmak için bkz. nasıl bilmek istiyorsanız [Azure Active Directory'de koşullu erişim ile çalışmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız bkz [Azure Active Directory'de koşullu erişim için en iyi yöntemler](active-directory-conditional-access-best-practices.md). 
