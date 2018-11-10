---
title: Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma | Microsoft Docs
description: Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma gösteren bir konu.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004609"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Tüketici Azure Active Directory B2C'de kayıt sırasında e-posta doğrulamayı devre dışı bırakma 
Etkin olduğunda, Azure Active Directory (Azure AD) B2C tüketici sağlayan bir e-posta adresi ve yerel bir hesap oluşturarak uygulamaları için kaydolma olanağı sağlar. Azure AD B2C kayıt işlemi sırasında doğrulanacak tüketiciler gerektirerek geçerli e-posta adresleri sağlar. Ayrıca kötü amaçlı otomatik bir işlem uygulamaları için sahte hesapları oluşturmasını engeller.

Bazı uygulama geliştiricilerinin tercih ettiğiniz e-posta doğrulaması sırasında kayıt işlemini atlayabilir ve bunun yerine e-posta adresini doğrulayın. daha sonra bir tüketiciye sahip. Bunu desteklemek için Azure AD B2C'yi e-posta doğrulamayı devre dışı bırakmak için yapılandırılabilir. Bunun yapılması, daha yumuşak bir kayıt işlemi oluşturur ve geliştiricilerin olmayan bu Tüketiciler, e-posta adresinden doğruladıktan tüketiciler birbirinden esnekliği sunar.

Varsayılan olarak, e-posta doğrulama açık kaydolma ilkeleri vardır. Devre dışı bırakmak için aşağıdaki adımları kullanın:

1. Tıklayın **kaydolma ilkeleri** veya **oturum açma veya kaydolma ilkeleri'ni** ne yapılandırdığınıza bağlı olarak için kaydolun.
2. Açmak için ilke (örneğin, "B2C_1_SiUp") tıklayın. 
3. Tıklayın **Düzenle** dikey penceresinin üstünde.
4. Tıklayın **sayfa UI özelleştirmesini**.
5. Tıklayın **yerel hesap kaydolma sayfası**.
6. Tıklayın **e-posta adresi** içinde **adı** sütununun altında **kaydolma özniteliklerini** bölümü.
7. İki durumlu **bölgedeki tüm sitelerden** seçeneğini **Hayır**.
8. Tıklayın **Tamam** ulaşana kadar altındaki **ilkesini Düzenle** dikey penceresi.
9. Dikey pencerenin en üstündeki **Kaydet**'e tıklayın. Hazırsınız!

> [!NOTE]
> Kayıt işlemini e-posta doğrulamayı devre dışı bırakma, istenmeyen posta göndermek için açabilir. Varsayılan devre dışı bırakırsanız, kendi doğrulama sisteminizle eklemenizi öneririz.
> 
>