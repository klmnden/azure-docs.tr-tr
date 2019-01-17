---
title: Self Servis parola sıfırlama Azure Active Directory B2C | Microsoft Docs
description: Self Servis parola sıfırlama için Azure Active Directory B2C, müşterilerinizin ayarlama işlemi gösterilmektedir
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3d2019101abf1086a58d0224ab31f2aa27afe8de
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54350600"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Self Servis parola sıfırlama müşterileriniz için ayarlama

Self Servis parola sıfırlama özelliği ile yerel hesaplar için RMS'ye kaydolup, müşteriler kendi parolalarını sıfırlayabilir. Özellikle, uygulamanızın düzenli olarak kullanan müşteriler milyonlarca varsa bu destek ekibinize üzerindeki yükü önemli ölçüde azaltır. Şu anda doğrulanmış e-posta adresi kullanarak yalnızca desteklenen kurtarma yöntemidir.

> [!NOTE]
> Bu makale uygulanır Self Servis parola sıfırlama V1 bağlamında kullanılan **oturum açın** kullanan kullanıcı akışı **yerel hesapla oturum aç** kimlik sağlayıcısı olarak. Uygulamanızdan çağrılan tamamen özelleştirilebilir bir parola sıfırlama kullanıcı akışları gerekirse bkz [bu makalede](active-directory-b2c-reference-policies.md).
> 
> 

Varsayılan olarak, Self Servis parola dizin yok sıfırlama açık. Etkinleştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com/) Abonelik Yöneticisi olarak. Aynı iş veya Okul hesabı ya da dizin oluşturmak için kullandığınız aynı Microsoft hesabıyla budur.
2. Açık **Azure Active Directory** (Gezinti çubuğundaki sol tarafta).
4. Ayarlama **Self Servis parola sıfırlama etkin** için **tüm**. 
5. Sayfanın üst kısmından **Kaydet**'e tıklayın. Hazırsınız!

Test etmek için bir kimlik sağlayıcısı olarak yerel hesaplarına sahip tüm kullanıcı oturum açma akışını "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma (burada, bir e-posta adresi ve parola veya kullanıcı adı ve parola girin) tıklatın sayfasında **hesabınıza erişemiyor?** müşteri deneyimini doğrulayın.

> [!NOTE]
> Self Servis parola sıfırlama sayfaları kullanarak özelleştirilebilir [şirket markası özelliğini](../active-directory/fundamentals/customize-branding.md).
> 
> 

