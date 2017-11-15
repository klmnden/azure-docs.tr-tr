---
title: "Azure Active Directory B2C: Self Servis parola sıfırlama | Microsoft Docs"
description: "Self Servis parola sıfırlama, Azure Active Directory B2C tüketici için ayarlamak üzere nasıl gösteren bir konu"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: beaf7dc6260db7509b2202c7801bcc0d2dd2c69e
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Self Servis parola sıfırlama tüketicileriniz ayarlama
Self Servis parola sıfırlama özelliğiyle (kaydolan yerel hesaplar için) tüketicileriniz üzerinde kendi parolalarını sıfırlayabilir. Özellikle, uygulamanızın düzenli olarak kullanan tüketicilerin milyonlarca varsa bu destek ekibiniz üzerindeki yük önemli ölçüde azaltır. Şu anda yalnızca bir kurtarma yöntemi olarak doğrulanmış e-posta adresini kullanarak destekliyoruz. Ek kurtarma yöntemleri (doğrulanmış telefon numarası, güvenlik soruları, vb.) gelecekte ekleyeceğiz.

> [!NOTE]
> Bu makalede Self Servis parola için geçerli bir oturum açma ilkesi bağlamında kullanılan sıfırlama. Tam olarak özelleştirilebilir parola sıfırlama ilkeleri uygulamanızdan çağrılan gerekirse bkz [bu makalede](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).
> 
> 

Varsayılan olarak, Self Servis parola dizininize sahip olmaz sıfırlama açık. Etkinleştirmek için aşağıdaki adımları kullanın:

1. [Klasik Azure portalında](https://manage.windowsazure.com/) Abonelik Yöneticisi olarak oturum açın. Aynı iş veya Okul hesabı veya dizin oluşturmak için kullanılan aynı Microsoft hesabını budur.
2. Sol taraftaki gezinti çubuğunda Active Directory uzantısına gidin.
3. Dizininizi altında Bul **Directory** sekmesinde ve tıklatın.
4. **Configure (Yapılandır)** sekmesine tıklayın.
5. Ekranı aşağı kaydırarak **kullanıcı parolası sıfırlama İlkesi** bölümü ve geçiş **kullanıcılar parola sıfırlama için etkin** için seçenek **Evet**. Dikkat **alternatif e-posta adresi** seçeneğini işaretli; olduğu gibi bırakın.
   
    ![Self servis parola sıfırlama](./media/active-directory-b2c-reference-sspr/sspr.png)
6. Sayfanın alt kısmındaki **Kaydet**’e tıklayın. İşiniz bittiğinde!

Test etmek için bir kimlik sağlayıcısı yerel hesaplarına sahip tüm oturum açma ilkesinde "Şimdi Çalıştır" özelliğini kullanın. Yerel hesap oturum açma (burada, bir e-posta adresi ve parola veya kullanıcı adı ve parola girin) tıklatın sayfa **hesabınıza erişemiyor?** Müşteri Deneyimi doğrulanamadı.

> [!NOTE]
> Self Servis parola sıfırlama sayfalarını kullanarak özelleştirilebilir [şirket markası özelliğini](../active-directory/customize-branding.md).
> 
> 

