---
title: Self Servis parola sıfırlama | Microsoft Docs
description: Self Servis parola sıfırlama için Azure Active Directory B2C uygulamasında müşterilerinizin ayarlanacağı gösterilmiştir
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/17/2018
ms.author: davidmu
ms.openlocfilehash: 5b75455ad604b594a5f85fea8299d35a7d02c848
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
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

