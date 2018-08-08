---
title: Nasıl-için koşullu erişim ilkeleri Azure Active Directory'yi Yapılandır güvenilmeyen ağlardan erişimlerin | Microsoft Docs
description: Azure Active Directory'de (Azure AD) koşullu erişim ilkesi için erişim denemesi için güvenilmeyen ağlardan yapılandırmayı öğrenin.
services: active-directory
keywords: uygulamalar, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: protection
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d170cdb42f7e3a368a50279f8a311a4ae94689df
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601811"
---
# <a name="how-to-configure-conditional-access-policies-for-access-attempts-from-untrusted-networks"></a>Nasıl yapılır: güvenilmeyen ağlara erişim girişimi için koşullu erişim ilkelerini yapılandırma   

Bir mobil öncelikli ve bulut öncelikli dünyada, Azure Active Directory (Azure AD), çoklu oturum açma cihazlar, uygulamalar ve hizmetlere her yerden sağlar. Yalnızca kuruluşunuzun ağ, aynı zamanda her bir güvenilmeyen Internet konumdan bunun bir sonucu olarak, kullanıcılarınızın bulut uygulamalarınızda erişebilirsiniz. İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), nasıl yetkili kullanıcılar denetleyebilir, bulut uygulamalarınızı erişebilirsiniz. Bu bağlamda bir ortak gereksinim güvenilmeyen ağlardan başlatılan erişim denemesi denetlemektir. Bu makalede, bu gereksinim işleyen bir koşullu erişim ilkesini yapılandırmak gereken bilgileri sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar: 

- Azure AD koşullu erişim temel kavramları 
- Azure portalında koşullu erişim ilkelerini yapılandırma

Bkz.

- [Azure Active Directory'de koşullu erişim nedir](../active-directory-conditional-access-azure-portal.md) - koşullu erişim genel bakış 

- [Hızlı Başlangıç: Azure Active Directory koşullu erişimiyle belirli uygulamalar için mfa'yı gerekli](app-based-mfa.md) - koşullu erişim ilkelerini yapılandırma ile biraz deneyim alınamıyor. 


## <a name="scenario-description"></a>Senaryo açıklaması

Güvenlik ve üretkenlik arasındaki dengeyi Yöneticisi için bir parola kullanarak kimlik doğrulaması kullanıcı yalnızca gereksinim için yeterli olabilir. Ancak, bir güvenilmeyen bir ağ konumundan erişim denemesi yapıldığında yoktur artan olmayan oturum açma riski yasal kullanıcılar tarafından gerçekleştirilir. Bu sorunu gidermek için güvenilmeyen ağlara gelen erişim denemelerini engelleyebilir. Alternatif olarak, çok faktörlü kimlik doğrulaması (MFA) hesabının meşru sahibi tarafından bir girişimde bulunuldu geri ek güvence elde etmek için de gerektirebilir. 

Azure AD koşullu erişim ile erişim veren tek bir ilke ile ilgili bu gereksinim karşılayabilirsiniz: 

- Seçilen bulut uygulamaları için

- Seçili kullanıcılar ve gruplar için  

- Çok faktörlü kimlik doğrulaması gerektiren 

- Ne zaman erişim denemesi gelen yapılır: 

    - Güvenilir olmayan bir konum


## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu senaryonun çevrilecek zorluktur *zaman erişim denemesi yapıldığında güvenilmeyen bir konumdan* içine bir koşullu erişim koşulu. Bir koşullu erişim ilkesinde yapılandırdığınız [konumları koşul](location-condition.md) ağ konumları için ilgili bir senaryoya. Konum koşulu, IP adres aralıkları, ülke ve bölgelerden mantıksal gruplandırmalarını göstermek adlandırılmış konumlar seçmenizi sağlar.  

Genellikle, bir veya daha fazla adres aralıkları, örneğin, 199.30.16.0 - 199.30.16.24 kuruluşunuza ait.
Adlandırılmış bir konuma göre yapılandırabilirsiniz:

- Bu aralık (199.30.16.0/24) belirtme 

- Aşağıdakiler gibi açıklayıcı bir ad atama **şirket ağı** 


Güvenilir olmayan tüm konumlara nelerdir tanımlamak çalışmak yerine, şunları yapabilirsiniz:

- Ekle 

    ![Koşullu erişim](./media/untrusted-networks/02.png)

- Tüm güvenilen konumları hariç tut 

    ![Koşullu erişim](./media/untrusted-networks/01.png)



## <a name="implementation"></a>Uygulama

Bu makalede açıklanan yaklaşımda, güvenilmeyen konumlardaki için koşullu erişim ilkesi yapılandırabilirsiniz. Her zaman beklendiği gibi çalıştığından emin olmak için üretim ortamına sunulmadan önce ilkenizi test etmeniz gerekir. İdeal olarak, ilk testlerinizi bir test kiracısında mümkünse yapmanız gerekir. Daha fazla bilgi için [dağıtımı yeni bir ilke](best-practices.md#how-should-you-deploy-a-new-policy). 



## <a name="next-steps"></a>Sonraki adımlar

Koşullu erişim hakkında daha fazla bilgi edinmek istiyorsanız bkz [Azure Active Directory'de koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md)