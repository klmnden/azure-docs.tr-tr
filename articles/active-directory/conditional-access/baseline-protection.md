---
title: Bir temel koruma Azure Active Directory koşullu erişim nedir? - önizleme | Microsoft Docs
description: Nasıl temel koruma, Azure Active Directory ortamında etkin güvenlik temel düzeyde en az sahip olmasını sağlar. öğrenin.
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
ms.date: 08/08/2018
ms.author: joflore
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b3d8b1daef9f780bbba53f33ae451c63f80249b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58519774"
---
# <a name="what-is-baseline-protection-preview"></a>Taban çizgisi protection (Önizleme) nedir?  

Geçen yıl içinde kimlik saldırıları 300 oranında artırmıştır. Ortamınızı sürekli artan saldırılarına karşı korumak için Azure Active Directory (Azure AD) temel koruma adlı yeni bir özellik sunar. Temel koruması olan bir dizi önceden tanımlanmış [koşullu erişim ilkeleri](../active-directory-conditional-access-azure-portal.md). Bu ilkelerin en az Azure AD tüm sürümlerinde etkin güvenlik temel düzeyde olmasını sağlamak için hedeftir. 

Bu makalede, Azure Active Directory'de temel koruma genel bir bakış sunulmaktadır.


 
## <a name="require-mfa-for-admins"></a>Yöneticiler için MFA gerektirme

Kullanıcılara ayrıcalıklı hesaplara erişim ortamınıza sınırsız erişimi vardır. Bu hesaplara sahip güç nedeniyle, bunları özel dikkatli düşünmelisiniz. Ayrıcalıklı hesapların geliştirmek için bir ortak oturum açma için kullanıldığında, daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir. Azure Active Directory'ye multi factor authentication (MFA) zorunlu daha güçlü bir hesap doğrulama alabilirsiniz.  

**Yöneticiler için mfa'yı gerekli** şu dizin rolleri için MFA gerektiren bir temel ilke: 

- Genel yönetici  

- SharePoint yöneticisi  

- Exchange yöneticisi  

- Koşullu erişim yöneticisi  

- Güvenlik yöneticisi  


![Azure Active Directory](./media/baseline-protection/01.png)

Bu temel ilke kullanıcılar dışında seçeneği sağlar. Biri hariç tutmak isteyebileceğiniz *[Acil Durum erişimi yönetici hesabı](../users-groups-roles/directory-emergency-access.md)* , kilitli dışında Kiracı emin olmak için.


## <a name="enable-a-baseline-policy"></a>Temel ilke etkinleştir 

Temel ilkeleri önizlemede olsa da, bunlar varsayılan olarak etkin. El ile etkinleştirmek istiyorsanız bir İlkesi'ni etkinleştirmeniz gerekir. Önizleme aşamasında açıkça temel ilkeleri etkinleştirirseniz, bu özellik genel kullanılabilirlik ulaştığında, etkin kalır. Planlanan davranış değişikliği neden neden olduğunu, ayrıca etkinleştirmek ve devre dışı bırakmak için üçüncü bir seçeneğiniz vardır ilke durumunu ayarlamak için: **İlkeyi gelecekte otomatik olarak etkinleştir**. Bu seçeneği tercih ederek, ilkeleri Önizleme sırasında devre dışı bırakın ancak Microsoft bunları otomatik olarak bu özellik genel kullanılabilirlik ulaştığında etkinleştirmek sahip. Temel ilkeleri artık açıkça etkinleştirmeyin ve seçmeyin **ilke gelecekte otomatik olarak etkinleştir** seçeneği, ilkeler devre dışı kalır bu özellik genel kullanılabilirlik ulaştığında.


**Bir taban çizgisi ilkesini etkinleştirmek için:**  

1. Oturum [Azure portalında](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak.

2. İçinde **Azure portalında**, sol gezinti tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/baseline-protection/05.png)

4. İlkeler listesinde ile başlayan bir ilkeye tıklayın **temel ilke:**. 

5. İlkeyi etkinleştirmek için tıklayın **ilkeyi hemen kullan**.

6. **Kaydet**’e tıklayın. 
 
  
 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler 

Özel koşullu erişim ilkelerini yönetme bir Azure AD Premium lisansı gerektirir, ancak temel ilkeleri Azure AD tüm sürümlerinde kullanılabilir.     

Temel ilkeye dahil Dizin rolleri en yüksek ayrıcalıklı bir Azure AD rolleridir. 

Betiğinizde kullanılan hesapları ayrıcalıklı, değiştirmelisiniz [kimliklerini Azure kaynakları için yönetilen](../managed-identities-azure-resources/overview.md) veya [hizmet sorumluları sertifikalarla](../develop/howto-authenticate-service-principal-powershell.md). Geçici bir çözüm, belirli kullanıcı hesaplarını temel ilkesinden hariç tutabilirsiniz. 

Temel ilkeleri POP, IMAP, eski Office masaüstü istemcisi gibi eski bir kimlik doğrulama akışları için geçerlidir. 




## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

- [Kimlik altyapınızın güvenliğini sağlamak için beş adım](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [Azure Active Directory'de koşullu erişim nedir?](overview.md) 

