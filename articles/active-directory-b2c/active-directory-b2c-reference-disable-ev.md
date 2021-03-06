---
title: Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma | Microsoft Docs
description: Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma gösteren bir konu.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8b50b59b6a1f99787b842923cd6ec77ee6500f0a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509525"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma 
Etkin olduğunda, Azure Active Directory (Azure AD) B2C tüketici sağlayan bir e-posta adresi ve yerel bir hesap oluşturarak uygulamaları için kaydolma olanağı sağlar. Azure AD B2C kayıt işlemi sırasında doğrulanacak tüketiciler gerektirerek geçerli e-posta adresleri sağlar. Ayrıca kötü amaçlı otomatik bir işlem uygulamaları için sahte hesapları oluşturmasını engeller.

Bazı uygulama geliştiricilerinin tercih ettiğiniz e-posta doğrulaması sırasında kayıt işlemini atlayabilir ve bunun yerine e-posta adresini doğrulayın. daha sonra bir tüketiciye sahip. Bunu desteklemek için Azure AD B2C'yi e-posta doğrulamayı devre dışı bırakmak için yapılandırılabilir. Bunun yapılması, daha yumuşak bir kayıt işlemi oluşturur ve geliştiricilerin olmayan bu Tüketiciler, e-posta adresinden doğruladıktan tüketiciler birbirinden esnekliği sunar.

Varsayılan olarak, e-posta doğrulama açık kaydolma kullanıcı akışları sahiptir. Devre dışı bırakmak için aşağıdaki adımları kullanın:

1. Tıklayın **kullanıcı akışları**.
2. Bunu açmak, kullanıcı akışınızı (örneğin, "B2C_1_SiUp") tıklayın. 
3. Tıklayın **sayfa düzenleri**.
4. Tıklayın **yerel hesap kaydolma sayfası**.
5. Tıklayın **e-posta adresi** içinde **adı** sütununun altında **kullanıcı öznitelikleri** bölümü.
6. Altında **doğrulama gerektiren**seçin **Hayır**.
7. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın. Hazırsınız!

> [!NOTE]
> Kayıt işlemini e-posta doğrulamayı devre dışı bırakma, istenmeyen posta göndermek için açabilir. Varsayılan devre dışı bırakırsanız, kendi doğrulama sisteminizle eklemenizi öneririz.
> 
>
