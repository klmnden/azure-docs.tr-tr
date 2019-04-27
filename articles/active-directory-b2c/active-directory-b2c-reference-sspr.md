---
title: Self Servis parola sıfırlama Azure Active Directory B2C | Microsoft Docs
description: Self Servis parola sıfırlama için Azure Active Directory B2C, müşterilerinizin ayarlama işlemi gösterilmektedir
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 544da5143fd58aae17bd517da9ab21a321f4db22
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316927"
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

