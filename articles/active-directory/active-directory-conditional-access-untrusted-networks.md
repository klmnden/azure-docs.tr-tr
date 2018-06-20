---
title: Nasıl-için koşullu erişim ilkeleri Azure Active Directory'yi Yapılandır güvenilmeyen ağlardan erişimlerin | Microsoft Docs
description: Azure Active Directory'de (Azure AD) bir koşullu erişim ilkesi için erişim denemesi için güvenilmeyen ağ bağlantılarını yapılandırma konusunda bilgi edinin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 952a3a3952a96c7125e0b0dbe770b72c17a57101
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232603"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Nasıl yapılır: erişim güvenilmeyen ağlardan deneme için koşullu erişim ilkelerini yapılandırma   

Bir mobil ilk olarak, bulut ilk dünyasında, Azure Active Directory (Azure AD) çoklu oturum açma cihazları, uygulamaları ve Hizmetleri için yerden sağlar. Yalnızca, kuruluşunuzun ağ üzerinden, aynı zamanda güvenilmeyen herhangi bir Internet konumdan bunun sonucunda, kullanıcılarınızın, bulut uygulamalarınızı erişebilir. İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), nasıl yetkili kullanıcılar denetleyebilir, bulut uygulamalarınızı erişebilirsiniz. Güvenilmeyen ağlardan başlatılan erişim girişimlerini denetlemek için bu bağlamda bir ortak gerekli değildir. Bu makalede, bu gereksinim işleyen bir koşullu erişim ilkesini yapılandırmak gereken bilgiler sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede aşina olduğunuzu varsayar: 

- Azure AD koşullu erişim temel kavramları 
- Azure portalında koşullu erişim ilkelerini yapılandırma

Bkz:

- [Azure Active Directory'de koşullu erişim nedir](active-directory-conditional-access-azure-portal.md) - koşullu erişim genel bakış 

- [Hızlı Başlangıç: Azure Active Directory koşullu erişimi olan belirli uygulamalar için MFA'ya gerek](active-directory-conditional-access-app-based-mfa.md) - koşullu erişim ilkelerini yapılandırma ile biraz deneyim almak için. 


## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi Yöneticisi için yalnızca bir parola kullanarak kimlik doğrulaması kullanıcı gereksinim için yeterli olabilir. Ancak, bir güvenilmeyen ağ konumundan erişim girişiminde bulunulduğunda yoktur artan yasal kullanıcılar tarafından gerçekleştirilen oturum açma işlemleri olmayan risk. Bu sorunu çözmek için güvenilir olmayan ağlara erişim denemeleri engelleyebilirsiniz. Alternatif olarak, çok faktörlü kimlik doğrulaması (MFA) hesap yasal sahibi tarafından girişimde bulunuldu geri ek güvence kazanmak için gerektirebilir. 

Azure AD koşullu erişimi ile erişim veren tek bir ilke ile bu gereksinim karşılayabilirsiniz: 

- Seçilen bulut uygulamaları için

- Seçili kullanıcılar ve gruplar için  

- Çok faktörlü kimlik doğrulama gerektirme 

- Ne zaman bir erişim gelen girişimde: 

    - Güvenilir olmayan bir konum


## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu senaryonun çevirmek için iştir *zaman erişim denemesi yapıldığında, güvenilir olmayan bir konumdan* içine bir koşullu erişim koşulu. Bir koşullu erişim ilkesinde yapılandırdığınız [konumları koşulu](active-directory-conditional-access-locations.md) ağ konumlarını ilgili adres senaryoları için. Konumları koşul seçmenize olanak tanır [konumları adlı](active-directory-conditional-access-locations.md#named-locations), IP adres aralıkları, ülke ve bölgelerden mantıksal gruplandırmalarını temsil eder.  

Genellikle, kuruluşunuzun bir veya daha fazla adres aralıkları, örneğin, 199.30.16.0 - 199.30.16.24 sahip olur.
Adlandırılmış bir konuma göre yapılandırabilirsiniz:

- Bu aralık (199.30.16.0/24) belirtme 

- Gibi açıklayıcı bir ad atama **şirket ağı** 


Güvenilir olmayan tüm konumları nelerdir tanımlamak çalışırken yerine, şunları yapabilirsiniz:

- Ekle 

    ![Koşullu erişim](./media/active-directory-conditional-access-untrusted-networks/02.png)

- Tüm güvenilen konumları Dışla 

    ![Koşullu erişim](./media/active-directory-conditional-access-untrusted-networks/01.png)



## <a name="implementation"></a>Uygulama

Bu makalede açıklanan yaklaşımda, güvenilmeyen konumlardan için bir koşullu erişim ilkesi yapılandırabilirsiniz. İlkeniz, beklendiği gibi çalıştığını doğrulayın üretime sunulmadan önce her zaman sınamalısınız. İdeal olarak, ilk test Kiracı testlerinizde mümkünse yapmanız gerekir. Daha fazla bilgi için bkz: [dağıtımı yeni bir ilke](active-directory-conditional-access-best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory'de koşullu erişim nedir?](active-directory-conditional-access-azure-portal.md)