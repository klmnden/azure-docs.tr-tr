---
title: Güvenlik bilgileri (Önizleme) sayfası - Azure Active Directory Uygulama parolaları ayarlamanız | Microsoft Docs
description: Tarayıcı olmayan uygulamalarda veya iki aşamalı doğrulamayı desteklemeyen herhangi bir uygulama ile kullanmak için otomatik olarak oluşturulan parolaları (uygulama parolaları) kuruluşunuzda ayarlayın. Bu uygulama parolasını normal bir paroladan ayrıdır ve güvenlik bilgileri sayfasından ayarlanabilir.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2018
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 55dfab0c60e77b86157a005db34c37917a5e08d2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60475121"
---
# <a name="manage-app-passwords-from-your-security-info-preview-page"></a>Uygulama parolaları, güvenlik bilgisi (Önizleme) sayfasından yönetin
Outlook 2010 gibi bazı uygulamalar, iki aşamalı doğrulamayı desteklemez. Bu destek eksikliği kuruluşunuzda iki aşamalı doğrulama kullanıyorsanız, uygulama çalışmıyor anlamına gelir. Bu sorunla karşılaşmamak için normal parolasından ayrı her tarayıcı içi uygulaması ile birlikte kullanmak için otomatik olarak oluşturulan bir parola oluşturabilirsiniz.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Important]
>Yöneticiniz, uygulama parolaları kullanmak izin vermeyebilir. Görmüyorsanız **uygulama parolaları** bir seçenek olarak, bunlar kuruluşunuzda mevcut değil.

Uygulama parolaları kullanırken unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulan ve tek uygulama girilen.

- Kullanıcı başına 40 parolalık bir sınır yoktur. Bir sonraki bu sınırı oluşturmayı denerseniz, yeni bir tane oluşturmak için izin verilen önce var olan bir parola silmek için istenir.

- Cihaz başına, uygulama başına değil bir uygulama parolasını kullanın. Örneğin, dizüstü bilgisayarınızda tüm uygulamalar için tek bir parola ve masaüstünüzde tüm uygulamalar için tek başka bir parola oluşturun.

    >[!Note]
    >(Outlook gibi) office 2013 istemcilerindeki yeni kimlik doğrulama protokolleri destekleyen ve iki aşamalı doğrulamayla birlikte kullanılabilir. Bu destek, iki aşamalı doğrulama açıldıktan sonra artık uygulama parolaları için Office 2013 istemcilerindeki gerektiğini anlamına gelir. Daha fazla bilgi için bkz. [Office 2013 ve Office 2016 istemci uygulamaları için nasıl modern kimlik doğrulaması çalıştığı](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) makalesi.

## <a name="create-new-app-passwords"></a>Yeni uygulama parolaları oluşturma
İki aşamalı doğrulama ile iş veya Okul hesabınızı kullanın ve güvenlik bilgileri deneyimi yöneticinize etkinleştirdiyse, oluşturma ve kullanma, uygulama parolalarını Sil **güvenlik bilgisi** sayfası.

>[!Note]
>Yöneticiniz güvenlik bilgisi deneyimi etkinleştirdiyseniz, bu henüz bilgileri ve yönergeleri izlemeniz gereken [iki aşamalı doğrulama için uygulama parolaları yönetme](multi-factor-authentication-end-user-app-passwords.md) bölümü.

### <a name="to-create-a-new-app-password"></a>Yeni bir uygulama parolası oluşturmak için
1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **uygulama parolası** seçin ve açılır listede **Ekle**.

    ![Seçili uygulama parolasıyla yöntemi Kutusu Ekle](media/security-info/securityinfo-myprofile-addpassword.png)

4. Uygulama parolası gerektiren bir uygulama adı yazın ve ardından **sonraki**.

    ![Uygulama parolası sayfasında, uygulama adı](media/security-info/securityinfo-myprofile-password-appname.png)

5. Metni kopyalayıp **parola** kutusuna parolayı (Bu örnekte, Outlook 2010) uygulamasının parola alanına yapıştırın ve ardından **Bitti**.

    ![Uygulama parolası sayfasında, uygulama adı](media/security-info/securityinfo-myprofile-password-copytext.png)
    
    Parola eklenir ve bundan sonra uygulamanız başarıyla oturum açabilir.

## <a name="delete-your-app-passwords"></a>Uygulama parolalarını Sil
Artık bir uygulama parolası gerektiren bir uygulama kullanmanız gerekirse, ilişkili uygulama parolasını silebilirsiniz. Uygulama parolasını silme kullanmak için kullanılabilir uygulama parola Noktalar birinin gelecekte serbest bırakır.

>[!Important]
>Bir uygulama parolası yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. İçindeki adımları izleyerek yeni bir uygulama parolası oluşturun ve uygulamaya yeniden girmeniz gerekir [yeni uygulama parolaları oluşturmanız](#create-new-app-passwords) bu makalenin.

### <a name="to-delete-an-app-password"></a>Bir uygulama parolasını silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** yanındaki bağlantı **uygulama parolası** belirli uygulama seçeneği.

    ![Uygulama parolası yöntemi güvenlik bilgisi silmek için bağlantı](media/security-info/securityinfo-myprofile-password-appdelete.png)

2. Seçin **Evet** silmek için onay kutusundan **uygulama parolası**. Uygulama parolası silindi, güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası.

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
- Hakkında daha fazla bilgi için **güvenlik bilgisi** sayfası ve bunu ayarlamak için bkz [güvenlik bilgisi genel bakış](user-help-security-info-overview.md)
