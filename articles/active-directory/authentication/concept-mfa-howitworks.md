---
title: Azure multi-Factor Authentication - nasıl çalışır? - Azure Active Directory
description: Azure Multi-Factor Authentication, kullanıcıların basit bir oturum açma işlemi taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa25e8a965b89c4e97263e3767a9400079fcad7a
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66496799"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Nasıl çalışır? Azure Multi-Factor Authentication

İki aşamalı doğrulama, güvenlik, katmanlı bir yaklaşım arasındadır. Birden çok kimlik doğrulama faktörleri ödün saldırganlar için önemli bir sınama gösterir. Bir saldırgan kullanıcı parolasının öğrenmek yönetir olsa bile, bu da ek kimlik doğrulama yöntemi olarak sahip zorunda kalmadan kullanışsızdır. İki veya daha fazla aşağıdaki kimlik doğrulama yöntemlerini isteyerek çalışır:

* Bir şey (genellikle parola) bildirin
* (Kolayca, bir telefon gibi kopyalaması değil güvenilir bir cihaz) sahip olduğunuz şey
* Bir şey (Biyometri) olan

<center>

![Kavramsal kimlik doğrulama yöntemleri görüntüsü](./media/concept-mfa-howitworks/methods.png)</center>

Azure multi-Factor Authentication (MFA) erişimi korumaya yardımcı olur ve kolaylık olması için kullanıcıların sürdürürken uygulamalarınıza. İkinci bir form kimlik doğrulaması gerektirerek ek güvenlik sağlar ve kullanımı kolay bir dizi aracılığıyla güçlü kimlik doğrulaması sağladığını [kimlik doğrulama yöntemleri](concept-authentication-methods.md). Kullanıcılar olabilir veya yönetici sağlayan yapılandırma kararları dayalı mfa beden değil.

## <a name="how-to-get-multi-factor-authentication"></a>Multi-Factor Authentication'ı almak nasıl?

Çok faktörlü kimlik doğrulaması aşağıdaki tekliflere bir parçası olarak sunulur:

* **Azure Active Directory Premium** veya **Microsoft 365 iş** -tam özellikli çok faktörlü kimlik doğrulaması gerektirmek için koşullu erişim ilkeleri kullanarak Azure multi-Factor Authentication kullanımı.

* **Azure AD ücretsiz**, **Azure AD temel**, veya tek başına **Office 365** lisanslar - önceden oluşturulmuş kullanım [koşullu erişim temel koruma ilkeleri](../conditional-access/concept-baseline-protection.md) gerektirmek için Kullanıcılar ve Yöneticiler için multi-Factor authentication.

* **Azure Active Directory küresel yöneticileri** -Azure multi-Factor Authentication yeteneklerinin bir alt kümesini genel yönetici hesapları korumak için bir araç olarak kullanılabilir.

> [!NOTE]
> Yeni müşteriler artık etkin 1 Eylül Mayıs 2018 sunan bir tek başına olarak Azure multi-Factor Authentication satın alabilirsiniz. Çok faktörlü kimlik doğrulaması, Azure AD Premium lisansınız kullanılabilir bir özellik olmaya devam edecektir.

## <a name="supportability"></a>Desteklenebilirlik

Çoğu kullanıcı kimlik doğrulaması için yalnızca parola kullanmaya alışkın olduğundan, kuruluşunuz bu işlem ile ilgili tüm kullanıcılara iletişim kuran önemlidir. Farkındalık, kullanıcılar için mfa'yı ilgili önemsiz sorunlar için Yardım Masasını arayın. olasılığını azaltabilirsiniz. Ancak, geçici olarak mfa'yı devre dışı bırakma gerekli olduğu bazı senaryolar vardır. Bu senaryoları nasıl ele alınacağını anlamak için aşağıdaki yönergeleri kullanın:

* Burada kullanıcı, kendi kimlik doğrulama yöntemlerini erişimi olmadığı veya düzgün çalışmayan olduğundan oturum açamaz senaryoları işlemek için destek ekibinize eğitin.
   * Koşullu erişim ilkeleri için Azure MFA hizmetini kullanarak, destek ekibinize, MFA gerektirme ilkesinden hariç tutulan bir gruba kullanıcı ekleyebilirsiniz.
* Koşullu erişim adlandırılmış konumlar ister iki aşamalı doğrulamayı en aza indirmek için bir yol olarak kullanmayı düşünün. Bu işlevsellik ile yöneticiler ağı gibi güvenilen bir ağa güvenli konumdan oturum açan kullanıcılar için iki aşamalı doğrulamayı atlayabilir yeni kullanıcı ekleme için kullanılan kesimi.
* Dağıtma [Azure AD kimlik koruması](../active-directory-identityprotection.md) ve risk etkinliklere göre iki aşamalı doğrulamayı tetikler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure multi-Factor Authentication adım adım dağıtım](howto-mfa-getstarted.md)
