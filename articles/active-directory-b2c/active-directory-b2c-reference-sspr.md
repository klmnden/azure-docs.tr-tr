---
title: "Azure Active Directory B2C: Self Servis parola sıfırlama | Microsoft Docs"
description: "Self Servis parola sıfırlama, Azure Active Directory B2C tüketici için ayarlamak üzere nasıl gösteren bir konu"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: c0a10869477647820828b22b6291522be255844d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Self Servis parola sıfırlama tüketicileriniz ayarlama
Self Servis parola sıfırlama özelliğiyle (kaydolan yerel hesaplar için) tüketicileriniz üzerinde kendi parolalarını sıfırlayabilir. Özellikle, uygulamanızın düzenli olarak kullanan tüketicilerin milyonlarca varsa bu destek ekibiniz üzerindeki yük önemli ölçüde azaltır. Şu anda yalnızca bir kurtarma yöntemi olarak doğrulanmış e-posta adresini kullanarak destekliyoruz. Ek kurtarma yöntemleri (doğrulanmış telefon numarası, güvenlik soruları, vb.) gelecekte ekleyeceğiz.

> [!NOTE]
> Bu makalede Self Servis parola için geçerli bir oturum açma ilkesi bağlamında kullanılan sıfırlama. Tam olarak özelleştirilebilir parola sıfırlama ilkeleri uygulamanızdan çağrılan gerekirse bkz [bu makalede](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Varsayılan olarak, Self Servis parola dizininize sahip olmaz sıfırlama açık. Etkinleştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portal](https://portal.azure.com/) Abonelik Yöneticisi olarak. Aynı iş veya Okul hesabı veya dizin oluşturmak için kullanılan aynı Microsoft hesabını budur.
2. Active Directory (sol taraftaki gezinti çubuğunda) açın.
3. Seçin **özellikleri**.
4. Ekranı aşağı kaydırarak **Self Servis parola sıfırlama etkin** bölümünde ve kendisine geçiş **tüm**. 
5. Tıklatın **kaydetmek** sayfanın üst kısmındaki. İşiniz bittiğinde!

Test etmek için bir kimlik sağlayıcısı yerel hesaplarına sahip tüm oturum açma ilkesinde "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma (burada, bir e-posta adresi ve parola veya kullanıcı adı ve parola girin) tıklatın sayfa **hesabınıza erişemiyor?** Müşteri Deneyimi doğrulanamadı.

> [!NOTE]
> Self Servis parola sıfırlama sayfalarını kullanarak özelleştirilebilir [şirket markası özelliğini](../active-directory/customize-branding.md).
> 
> 

