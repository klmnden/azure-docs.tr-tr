---
title: Self Servis parola sıfırlama Azure Active Directory B2C'de | Microsoft Docs
description: Self Servis parola sıfırlama için Azure Active Directory B2C uygulamasında müşterilerinizin ayarlanacağı gösterilmiştir
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/17/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ea8b23618b382f557340643afd62e56932bbfb2d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712105"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Self Servis parola sıfırlama müşterileriniz için ayarlama
Self Servis parola sıfırlama özelliği ile parolalarını kendi yerel hesaplar için kaydolduktan müşterilerinizin sıfırlayabilirsiniz. Özellikle, uygulamanızın düzenli olarak kullanan müşteriler milyonlarca varsa bu destek ekibiniz üzerindeki yük önemli ölçüde azaltır. Şu anda doğrulanmış e-posta adresini kullanarak yalnızca desteklenen kurtarma yöntemidir.

> [!NOTE]
> Bu makalede Self Servis parola için geçerli bir oturum açma ilkesi bağlamında kullanılan sıfırlama. Tam olarak özelleştirilebilir parola sıfırlama ilkeleri uygulamanızdan çağrılan gerekirse bkz [bu makalede](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Varsayılan olarak, Self Servis parola dizininize yok sıfırlama açık. Etkinleştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portal](https://portal.azure.com/) Abonelik Yöneticisi olarak. Aynı iş veya Okul hesabı veya dizin oluşturmak için kullanılan aynı Microsoft hesabını budur.
2. Açık **Azure Active Directory** (Gezinti çubuğundaki sol tarafta).
4. Ayarlama **Self Servis parola sıfırlama etkin** için **tüm**. 
5. Tıklatın **kaydetmek** sayfanın üst kısmındaki. İşiniz bittiğinde!

Test etmek için bir kimlik sağlayıcısı yerel hesaplarına sahip tüm oturum açma ilkesinde "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma (burada, bir e-posta adresi ve parola veya kullanıcı adı ve parola girin) tıklatın sayfa **hesabınıza erişemiyor?** müşteri deneyimini doğrulanamadı.

> [!NOTE]
> Self Servis parola sıfırlama sayfalarını kullanarak özelleştirilebilir [şirket markası özelliğini](../active-directory/customize-branding.md).
> 
> 

