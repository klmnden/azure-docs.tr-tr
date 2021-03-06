---
title: Azure Active Directory (Azure AD) koşullu erişim ile güvenilmeyen ağlara erişim için çok faktörlü kimlik doğrulaması (MFA) gerektirme | Microsoft Docs
description: Azure Active Directory'de (Azure AD) koşullu erişim ilkesi için erişim denemesi için güvenilmeyen ağlardan yapılandırmayı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 12/10/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b75e9aa3c588f5046ec55c0d809ca74060ad9c2
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509328"
---
# <a name="how-to-require-mfa-for-access-from-untrusted-networks-with-conditional-access"></a>Nasıl yapılır: Koşullu erişim ile güvenilmeyen ağlara erişim için MFA gerektirme   

Azure Active Directory (Azure AD) etkinleştirir çoklu oturum açma cihazlar, uygulamalar ve hizmetlere her yerden. Kullanıcılarınıza bulut uygulamalarınızda yalnızca kuruluşunuzun ağ, aynı zamanda her bir güvenilmeyen Internet konumdan erişebilirsiniz. Güvenilmeyen ağlara erişim için ortak bir en iyi yöntem çok faktörlü kimlik doğrulaması (MFA) gerektirmektir.

Bu makalede, güvenilmeyen ağlara erişim için MFA gerektiren bir koşullu erişim ilkesi yapılandırmak gereken bilgileri sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar: 

- [Temel kavramları](overview.md) Azure AD koşullu erişim 
- [En iyi uygulamalar](best-practices.md) Azure portalında koşullu erişim ilkelerini yapılandırma

## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi gelmek için yalnızca kuruluşunuzun ağına oturum açma işlemleri için bir parola gerektirme yeterli olabilir. Ancak, bir güvenilmeyen bir ağ konumundan erişim için yoktur artan olmayan oturum açma riski yasal kullanıcılar tarafından gerçekleştirilir. Bu sorunu gidermek için güvenilmeyen ağlara erişimi engelleyebilirsiniz. Alternatif olarak, çok faktörlü kimlik doğrulaması (MFA) hesabının meşru sahibi tarafından bir girişimde bulunuldu geri ek güvence elde etmek için de gerektirebilir. 

Azure AD koşullu erişim ile erişim veren tek bir ilke ile ilgili bu gereksinim karşılayabilirsiniz: 

- Seçilen bulut uygulamaları için
- Seçili kullanıcılar ve gruplar için  
- Çok faktörlü kimlik doğrulaması gerektiren 
- Ne zaman erişim kaynaklandığı: 
   - Güvenilir olmayan bir konum

## <a name="implementation"></a>Uygulama

Bu senaryonun çevrilecek zorluktur *güvensiz bir ağa bir konumdan erişim* içine bir koşullu erişim koşulu. Bir koşullu erişim ilkesinde yapılandırdığınız [konumları koşul](location-condition.md) ağ konumları için ilgili bir senaryoya. Konum koşulu mantıksal gruplandırmaları olan IP adres aralıkları, ülke ve bölgelerden adlandırılmış konumlar seçmenize olanak sağlar.  

Genellikle, bir veya daha fazla adres aralıkları, örneğin, 199.30.16.0 - 199.30.16.24 kuruluşunuza ait.
Adlandırılmış bir konuma göre yapılandırabilirsiniz:

- Bu aralık (199.30.16.0/24) belirtme 
- Aşağıdakiler gibi açıklayıcı bir ad atama **şirket ağı** 

Güvenilir olmayan tüm konumlara nelerdir tanımlamak çalışmak yerine, şunları yapabilirsiniz:

- Herhangi bir yere ekleyin 

   ![Koşullu Erişim](./media/untrusted-networks/02.png)

- Tüm güvenilen konumları hariç tut 

   ![Koşullu Erişim](./media/untrusted-networks/01.png)

## <a name="policy-deployment"></a>İlke dağıtımı

Bu makalede açıklanan yaklaşımda, güvenilmeyen konumlardaki için koşullu erişim ilkesi yapılandırabilirsiniz. İlkenizi beklendiği gibi çalıştığından emin olmak için üretim ortamına sunulmadan önce test etmek için önerilen en iyi yöntem olacaktır. İdeal olarak, bir de test kiracılığınız yeni ilkeniz beklendiği gibi çalıştığını doğrulamak için kullanın. Daha fazla bilgi için [yeni bir ilkeyi dağıtmak nasıl](best-practices.md#how-should-you-deploy-a-new-policy). 

## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure Active Directory'de koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md)
