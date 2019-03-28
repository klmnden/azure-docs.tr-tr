---
title: Uygulama parolaları - Azure Active Directory yönetme | Microsoft Docs
description: Uygulama parolaları ve ne olduğu hakkında bilgi edinmek için iki aşamalı doğrulama ile ilgili olarak kullanılır.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6790fa1cbb10999a751b31bcb27db2edcb67b4a
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517768"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>İki aşamalı doğrulama için uygulama parolaları yönetme

Outlook 2010 gibi belirli tarayıcı olmayan uygulamalar, iki aşamalı doğrulamayı desteklemez. Bu destek eksikliği, iki aşamalı doğrulama kullanıyorsanız, uygulama çalışmıyor anlamına gelir. Bu sorunla karşılaşmamak için normal parolasından ayrı her tarayıcı içi uygulaması ile birlikte kullanmak için otomatik olarak oluşturulan bir parola oluşturabilirsiniz.

Uygulama parolaları kullanırken unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulan ve tek uygulama girilen.

- Kullanıcı başına 40 parolalık bir sınır yoktur. Bir sonraki bu sınırı oluşturmayı denerseniz, yeni bir tane oluşturmak için izin verilen önce var olan bir parola silmek için istenir.

- Cihaz başına, uygulama başına değil bir uygulama parolasını kullanın. Örneğin, dizüstü bilgisayarınızda tüm uygulamalar için tek bir parola ve masaüstünüzde tüm uygulamalar için tek başka bir parola oluşturun.

    >[!Note]
    >(Outlook gibi) office 2013 istemcilerindeki yeni kimlik doğrulama protokolleri destekleyen ve iki aşamalı doğrulamayla birlikte kullanılabilir. Bu destek, iki aşamalı doğrulama açıldıktan sonra artık uygulama parolaları için Office 2013 istemcilerindeki gerektiğini anlamına gelir. Daha fazla bilgi için bkz. [Office 2013 ve Office 2016 istemci uygulamaları için nasıl modern kimlik doğrulaması çalıştığı](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) makalesi.

## <a name="where-to-create-and-delete-your-app-passwords"></a>Where oluşturup, uygulama parolalarını Sil

Size bir uygulama parolası, ilk iki aşamalı doğrulama kaydı sırasında verilir. En fazla bir parola gerekiyorsa, ek parolalar, iki aşamalı doğrulamanın nasıl kullandığınıza bağlı oluşturabilirsiniz:

- **İki aşamalı doğrulama ile iş veya Okul hesabı ve MyApps portalında kullanın.** Oluşturun ve Oluştur yönergeleri kullanarak, uygulama parolalarını Sil ve bu makalede portal MyApps bölümünü kullanarak uygulama parolalarını Sil. MyApps portalında ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [MyApps portalında Azure Active Directory nedir?](active-directory-saas-access-panel-introduction.md).

- **İki aşamalı doğrulama ile iş veya Okul hesabı ve Office 365 portalını kullanın.** Oluşturma ve kullanma yönergeleri, uygulama parolalarını Sil [Office 365 portalını kullanarak uygulama parolaları oluşturma ve silme](#create-and-delete-app-passwords-using-the-office-365-portal) bu makalenin.

- **İki aşamalı doğrulama, kişisel Microsoft hesabınızla kullandığınız.** Oluşturma ve kullanma, uygulama parolalarını Sil [güvenlik temel kavramları](https://account.microsoft.com/account/) kişisel Microsoft hesabınızla sayfası. Daha fazla bilgi için bkz. [uygulama parolaları ve iki aşamalı doğrulama](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) makalesi.

## <a name="create-and-delete-app-passwords-using-the-myapps-portal"></a>Oluşturma ve MyApps portalında kullanarak uygulama parolalarını Sil
Oluşturun ve MyApps portalı üzerinden uygulama parolalarını Sil.

### <a name="to-create-an-app-password-using-the-myapps-portal"></a>MyApps portalında kullanarak bir uygulama parolası oluşturmak için

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) adresinde oturum açın.

2. Sağ üst köşedeki adınızın seçip seçin **profili**.

3. Seçin **ek güvenlik doğrulaması**.

   ![Ek güvenlik doğrulaması - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/myapps1.png)

4. Seçin **uygulama parolaları**.

   ![Uygulama parolaları - ekran görüntüsü seçin](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. **Oluştur**’a tıklayın.

6. Uygulama parolası için bir ad yazın ve ardından **sonraki**.

7. Uygulama parolasını panoya kopyalayın ve uygulamanıza yapıştırın.
   
    ![Bir uygulama parolası oluşturma](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="to-delete-an-app-password-using-the-myapps-portal"></a>MyApps portalında kullanarak bir uygulama parolasını silmek için

1. Profilinize gidin ve ardından **ek güvenlik doğrulama**.

2. Seçin **uygulama parolaları**ve ardından **Sil** yanındaki silmek istediğiniz uygulama parolası.

   ![Bir uygulama parolasını silme](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

3. Seçin **Evet** parola silin ve ardından istediğinizi onaylamak için **Kapat**.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Oluşturma ve Office 365 portalını kullanarak uygulama parolalarını Sil

İki aşamalı doğrulama ile iş veya Okul hesabı ve Office 365 uygulamalarınıza kullanırsanız, oluşturabilir ve Office 365 portalını kullanarak, uygulama parolalarını Sil. Bir seferde en fazla 40 uygulama parolaları olabilir. Bu sınırdan sonra başka bir uygulama parolasına ihtiyacınız varsa, varolan uygulama parolalarından birini silmeniz gerekir.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>Office 365 portalını kullanarak uygulama parolaları oluşturmak için

1. İş veya Okul hesabınızda oturum açın.

2. Git [ https://portal.office.com ](https://portal.office.com)seçin **ayarları** simgesini sağ üst kısmındaki **Office 365 portalında** sayfasında ve ardından **ek güvenlik doğrulama**.

    ![Office portalı gösteren ek güvenlik doğrulama alanı genişletilmiş](media/security-info/security-info-o365password.png)

3. Şunu metni seçin **oluşturma ve uygulama parolaları yönetme** açmak için **uygulama parolaları** sayfası.

4. Seçin **Oluştur**uygulama parolası gerekiyor uygulama için bir kolay ad yazın ve ardından **sonraki**.

5. Seçin **parolayı Panoya Kopyala**ve ardından **Kapat**.

6. Tarayıcı olmayan uygulamanıza oturum açmak için kopyalanan uygulama parolasını kullanın. Bu parola bir kere girmeniz yeterlidir ve geleceğe yönelik hatırlanır.

### <a name="to-delete-app-passwords-using-the-office-365-portal"></a>Uygulama parolaları Office 365 portalını kullanarak silmek için

1. İş veya Okul hesabınızda oturum açın.

2. Git [ https://portal.office.com ](https://portal.office.com)seçin **ayarları** simgesini sağ üst kısmındaki **Office 365 portalında** sayfasında ve ardından **ek güvenlik doğrulama**.

3. Şunu metni seçin **oluşturma ve uygulama parolaları yönetme** açmak için **uygulama parolaları** sayfası.

4. Seçin **Sil** uygulama parolasını silmek işaretleyin **Evet** onay kutusunu ve ardından **Kapat**.

    Uygulama parolası başarıyla silindi.

5. Yeni uygulama parolası oluşturmak için bir uygulama parolası oluşturmak için adımları izleyin.

## <a name="if-your-app-passwords-arent-working-properly"></a>Varsa, uygulama parolaları düzgün çalışmayan

Parolanızı doğru yazdığınızdan emin olun. Parolanızı doğru girildiğinden emin değilseniz, yeniden oturum açın ve yeni bir uygulama parolası oluşturmak deneyebilirsiniz. Bu seçeneklerden hiçbiri sorununuzu gidermek, mevcut uygulama parolalarınızın almadığınızdan olanları oluşturmanızı, silebileceğiniz şirketinizin Destek birimine. 

## <a name="next-steps"></a>Sonraki adımlar

- [İki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- Denemenin [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) oturum açma işlemlerini uygulama bildirimleri, metinleri veya çağrıları almak yerine ile doğrulayın.
