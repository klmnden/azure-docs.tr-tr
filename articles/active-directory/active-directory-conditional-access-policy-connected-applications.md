---
title: "Azure Active Directory cihaz temelli koşullu erişim ilkelerini yapılandırma | Microsoft Docs"
description: "Azure Active Directory cihaz temelli koşullu erişim ilkeleri yapılandırma hakkında bilgi edinin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: a27862a6-d513-43ba-97c1-1c0d400bf243
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2354a8bf81189f70bb8d0d63c3df3236403c11fc
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="configure-azure-active-directory-device-based-conditional-access-policies"></a>Azure Active Directory cihaz temelli koşullu erişim ilkelerini yapılandırma

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), ince ayar yapabilirsiniz nasıl yetkili kullanıcılar, kaynaklara erişebilir. Örneğin, güvenilen cihazlara belirli kaynaklara erişimi sınırlayın. Güvenilir bir aygıt gerektiren bir koşullu erişim ilkesi cihaz temelli koşullu erişim ilkesi de denir.

Bu konu, Azure AD bağlı uygulamalar için cihaz temelli koşullu erişim ilkelerinin nasıl yapılandırılacağını hakkında bilgi sağlar. 


## <a name="before-you-begin"></a>Başlamadan önce

Cihaz temelli koşullu erişim TIES **Azure AD koşullu erişimi** ve **Azure AD cihaz Yönetimi birlikte**. Bu alanlardan biri ile tanıdık değilse, aşağıdaki konularda ilk şöyle olmalıdır:

- **[Azure Active Directory'de koşullu erişim](active-directory-conditional-access-azure-portal.md)**  -Bu konu ile kavramsal bir genel bakış koşullu erişim ve ilgili terminolojiyi sağlar.

- **[Azure Active Directory'de cihaz yönetimine giriş](device-management-introduction.md)**  -Bu konu, sahip Azure AD ile cihazları bağlamak için çeşitli seçenekler genel bir bakış sunar. 


## <a name="trusted-devices"></a>Güvenilen cihazlar

Bir mobil ilk olarak, bulut ilk dünyasında, Azure Active Directory çoklu oturum açma cihazları, uygulamaları ve Hizmetleri için yerden sağlar. Belirli sağ kullanıcılara erişim izni verme ortamınızdaki kaynakların iyi yeterli olmayabilir. Doğru kullanıcılar yanı sıra bir kaynağa erişmek için kullanılacak güvenilir bir aygıt gerektirebilir. Ortamınızda, güvenilir bir aygıt, dayanır tanımlayabilirsiniz aşağıdaki bileşenleri:

- [Cihaz platformları](active-directory-conditional-access-conditions.md#device-platforms) bir cihazda
- Bir cihazın uyumlu olup
- Bir cihaz etki alanına katılmış olup olmadığı 

[Cihaz platformları](active-directory-conditional-access-conditions.md#device-platforms) , Cihazınızda çalıştırılan işletim sistemi tarafından belirlenir. Cihaz temelli koşullu erişim ilkenizi, belirli cihaz platformları için belirli kaynaklara erişimi sınırlayabilirsiniz.



Bir cihaz temelli koşullu erişim ilkesinde güvenilir cihazlar uyumlu olarak işaretlenecek gerektirebilir.

![Bulut uygulamaları](./media/active-directory-conditional-access-policy-connected-applications/24.png)

Aygıtları dizininde tarafından uyumlu olarak işaretlenebilir:

- Intune 
- Azure AD tümleştirme yoluyla Windows 10 cihazları yöneten sistem üçüncü taraf bir mobil cihaz yönetilen 
 
  

Azure AD ile bağlı cihazlar uyumlu olarak işaretlenebilir. Bir aygıtı Azure Active Directory'ye bağlamak için aşağıdaki seçenekler vardır: 

- Azure AD kayıtlı
- Azure AD alanına katılmış
- Karma Azure AD alanına katılmış

    ![Bulut uygulamaları](./media/active-directory-conditional-access-policy-connected-applications/26.png)

Bir şirket içi Active Directory (AD) ayak izini varsa değil Azure AD ile bağlı ancak güvenilir olması için AD alanına katılmış aygıtlar düşünebilirsiniz.

![Bulut uygulamaları](./media/active-directory-conditional-access-policy-connected-applications/25.png)


## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda bir cihaz temelli koşullu erişim ilkesini yapılandırmadan önce bir göz atalım [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md).

