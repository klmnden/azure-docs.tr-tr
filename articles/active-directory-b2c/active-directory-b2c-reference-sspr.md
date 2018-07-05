---
title: Self Servis parola sıfırlama Azure Active Directory B2C | Microsoft Docs
description: Self Servis parola sıfırlama için Azure Active Directory B2C, müşterilerinizin ayarlama işlemi gösterilmektedir
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 3612e10df12e2b18f32caae55bdd83b12a4e24a6
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450310"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Self Servis parola sıfırlama müşterileriniz için ayarlama
Self Servis parola sıfırlama özelliği ile yerel hesaplar için RMS'ye kaydolup, müşteriler kendi parolalarını sıfırlayabilir. Özellikle, uygulamanızın düzenli olarak kullanan müşteriler milyonlarca varsa bu destek ekibinize üzerindeki yükü önemli ölçüde azaltır. Şu anda doğrulanmış e-posta adresi kullanarak yalnızca desteklenen kurtarma yöntemidir.

> [!NOTE]
> Bu makale uygulanır Self Servis parola sıfırlama bir oturum açma ilkesi bağlamında kullanılan. Uygulamanızdan çağrılan tamamen özelleştirilebilir bir parola sıfırlama ilkelerini ihtiyaç duyarsanız bkz [bu makalede](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Varsayılan olarak, Self Servis parola dizin yok sıfırlama açık. Etkinleştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com/) Abonelik Yöneticisi olarak. Aynı iş veya Okul hesabı ya da dizin oluşturmak için kullandığınız aynı Microsoft hesabıyla budur.
2. Açık **Azure Active Directory** (Gezinti çubuğundaki sol tarafta).
4. Ayarlama **Self Servis parola sıfırlama etkin** için **tüm**. 
5. Tıklayın **Kaydet** sayfanın üstünde. Hazırsınız!

Test etmek için bir kimlik sağlayıcısı olarak yerel hesaplarına sahip tüm oturum açma ilkesinde "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma (burada, bir e-posta adresi ve parola veya kullanıcı adı ve parola girin) tıklatın sayfasında **hesabınıza erişemiyor?** müşteri deneyimini doğrulayın.

> [!NOTE]
> Self Servis parola sıfırlama sayfaları kullanarak özelleştirilebilir [şirket markası özelliğini](../active-directory/fundamentals/customize-branding.md).
> 
> 

