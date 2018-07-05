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
ms.openlocfilehash: c1bebe46832226e822d9eeb002cb555b72a1d7fa
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441616"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>Azure Active Directory B2C: Tüketicinin kaydolma devre dışı bırakma e-posta doğrulama
Etkin olduğunda, Azure Active Directory (Azure AD) B2C tüketici sağlayan bir e-posta adresi ve yerel bir hesap oluşturarak uygulamaları için kaydolma olanağı sağlar. Azure AD B2C kayıt işlemi sırasında doğrulanacak tüketiciler gerektirerek geçerli e-posta adresleri sağlar. Ayrıca kötü amaçlı otomatik bir işlem uygulamaları için sahte hesapları oluşturmasını engeller.

Bazı uygulama geliştiricilerinin tercih ettiğiniz e-posta doğrulaması sırasında kayıt işlemini atlayabilir ve bunun yerine e-posta adresini doğrulayın. daha sonra bir tüketiciye sahip. Bunu desteklemek için Azure AD B2C'yi e-posta doğrulamayı devre dışı bırakmak için yapılandırılabilir. Bunun yapılması, daha yumuşak bir kayıt işlemi oluşturur ve geliştiricilerin olmayan bu Tüketiciler, e-posta adresinden doğruladıktan tüketiciler birbirinden esnekliği sunar.

Varsayılan olarak, e-posta doğrulama açık kaydolma ilkeleri vardır. Devre dışı bırakmak için aşağıdaki adımları kullanın:

1. [Azure portalındaki B2C özellikleri dikey penceresine gitmek için aşağıdaki adımları izleyin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Tıklayın **kaydolma ilkeleri** veya **oturum açma veya kaydolma ilkeleri'ni** ne yapılandırdığınıza bağlı olarak için kaydolun.
3. Açmak için ilke (örneğin, "B2C_1_SiUp") tıklayın. Tıklayın **Düzenle** dikey penceresinin üstünde.
4. Tıklayın **sayfa UI özelleştirmesini**.
5. Tıklayın **yerel hesap kaydolma sayfası**.
6. Tıklayın **e-posta adresi** içinde **adı** sütununun altında **kaydolma özniteliklerini** bölümü.
7. İki durumlu **bölgedeki tüm sitelerden** seçeneğini **Hayır**.
8. Tıklayın **Tamam** ulaşana kadar altındaki **ilkesini Düzenle** dikey penceresi.
9. Tıklayın **Kaydet** dikey penceresinin üstünde. Hazırsınız!

> [!NOTE]
> Kayıt işlemini e-posta doğrulamayı devre dışı bırakma, istenmeyen posta göndermek için açabilir. Varsayılan devre dışı bırakırsanız, kendi doğrulama sisteminizle eklemenizi öneririz.
> 
> 

Her zaman geri bildirim ve öneriler için açık duyuyoruz! Bu konu ile ilgili zorluklarla sahip veya bu içeriğin geliştirilmesi için öneri, sayfanın sonundaki geri BİLDİRİMİNİZE değer veriyoruz. Özellik istekleri için ekleyebilmesi [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
