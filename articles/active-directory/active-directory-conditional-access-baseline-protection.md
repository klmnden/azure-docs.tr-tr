---
title: Azure Active Directory koşullu erişimin bir taban çizgisi koruma nedir? -Önizleme | Microsoft Docs
description: Azure Active Directory ortamınızda etkin güvenlik temel düzeyde en az sahip temel koruması nasıl sağlar öğrenin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
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
ms.date: 06/21/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 86b57a82573760ac73975e851b2bb4caf769845b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308569"
---
# <a name="what-is-baseline-protection---preview"></a>Taban çizgisi koruma nedir? -Önizleme  

Geçen yıl içinde kimlik saldırıları % 300 artırmıştır. Ortamınızı gitgide artan saldırılara karşı korumak için Azure Active Directory (Azure AD) temel koruma adı verilen yeni bir özellik sunmaktadır. Taban çizgisi koruması olan bir dizi önceden tanımlanmış [koşullu erişim ilkeleri](active-directory-conditional-access-azure-portal.md). Bu ilkelerin en az tüm Azure AD sürümlerinde etkin güvenlik temel düzeyde olmasını sağlamak için hedeftir. 

Bu makalede, Azure Active Directory'de temel koruma genel bir bakış sağlar.


 
## <a name="require-mfa-for-admins"></a>Yöneticiler için MFA gerekir

Kullanıcılara ayrıcalıklı hesaplara erişim ortamınız için sınırsız erişimi vardır. Bu hesaplara sahip güç nedeniyle, bunları özel dikkatli düşünmelisiniz. Ayrıcalıklı hesapların korumasını geliştirmek için bir ortak oturum açma için kullanıldığında daha güçlü bir form Hesap doğrulama gerektirecek şekilde yöntemidir. Azure Active Directory'de, çok faktörlü kimlik doğrulaması (MFA) isteyerek daha güçlü bir hesap doğrulama alabilirsiniz.  

**Yöneticiler için MFA'ya gerek** aşağıdaki dizin roller için MFA gerektiren bir temel ilke: 

- Genel yönetici  

- SharePoint yöneticisi  

- Exchange yöneticisi  

- Koşullu erişim yöneticisi  

- Güvenlik yöneticisi  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

Bu temel ilke kullanıcılar ve gruplar dışlamak için seçenek sunar. Bir dışarıda bırakmak isteyebilirsiniz *[Acil Durum erişimi yönetici hesabı](active-directory-admin-manage-emergency-access-accounts.md)* , değil kilitli dışında Kiracı emin olmak için.


## <a name="enable-a-baseline-policy"></a>Taban çizgisi ilkesini etkinleştir 

Temel ilkeler önizlemede olsa da, bunlar etkinleştirilmedi varsayılan olarak gelir. Etkinleştirmek istiyorsanız, bir ilke el ile etkinleştirmeniz gerekir. Bu özellik genel kullanılabilirlik ulaştı hemen etkinleştirildi varsayılan ilkelerdir. Planlanan davranış değişikliği sahip neden etkinleştirmek ve bir ilke durumunu ayarlamak için üçüncü bir seçenek devre dışı bırakmak için ayrıca nedeni: **İlkesi gelecekte otomatik etkinleştir**. Bu seçeneği belirterek Microsoft ilke etkinleştirme karar vermenize olanak tanır.      


**Bir taban çizgisi ilkesini etkinleştirmek için:**  

1. Oturum [Azure portal](https://portal.azure.com) genel yönetici, güvenlik veya koşullu erişim yönetici olarak.

2. İçinde **Azure portal**, sol gezinti çubuğu üzerinde tıklatın **Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **Yönet** 'yi tıklatın **koşullu erişim**.

    ![Koşullu erişim](./media/active-directory-conditional-access-baseline-protection/03.png)

4. İlkeler listesinde ile başlayan bir İlkesi'ni **temel ilke:**. 

5. İlkeyi etkinleştirmek için **ilkeyi hemen kullanmak**.

6. **Kaydet**’e tıklayın. 
 


  
 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler 

Özel koşullu erişim ilkelerini yönetme bir Azure AD Premium lisansı gerektirir, ancak temel ilkeleri, Azure ad tüm sürümlerinde kullanılabilir.     

Taban çizgisi ilkesine dahil directory rolleri en yüksek ayrıcalıklı bir Azure AD rolleridir. 

Komut dosyalarınızı kullanılan hesapları ayrıcalıklı, değiştirmelisiniz [yönetilen hizmet kimliği (MSI)](./managed-service-identity/overview.md) veya [hizmet sorumluları sertifikalarla](../azure-resource-manager/resource-group-authenticate-service-principal.md). Geçici bir çözüm olarak, belirli kullanıcı hesaplarını temel ilkesinden hariç tutabilirsiniz. 

POP, IMAP, eski Office masaüstü istemcisi gibi eski kimlik doğrulama akışı temel ilkeleri uygulanır. 




## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
