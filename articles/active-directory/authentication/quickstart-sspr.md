---
title: 'Hızlı başlangıç: Azure AD self servis parola sıfırlama'
description: Bu hızlı başlangıçta kullanıcıların kendi parolalarını sıfırlamalarını sağlamak için Azure AD self servis parola sıfırlama özelliğini hızlı bir şekilde yapılandıracaksınız
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: quickstart
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 58e3254d499e013dc686bf6b7d53f919a457c901
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371294"
---
# <a name="quickstart-self-service-password-reset"></a>Hızlı Başlangıç: Self servis parola sıfırlama

Bu hızlı başlangıçta BT uzmanlarının kullanıcılara parolalarını sıfırlama veya hesaplarının kilidini açma yetkisi vermesi için basit bir yöntem sunan self servis parola sıfırlama (SSPR) özelliğini yapılandıracaksınız.

## <a name="prerequisites"></a>Önkoşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.
* Bildiğiniz bir kullanıcı bakın makale oluşturmanız gerekiyorsa bir parola ile bir yönetici olmayan test kullanıcısı [hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](../add-users-azure-active-directory.md).
* Bu yönetici olmayan test kullanıcısının üyesi olduğu pilot grup. Grup oluşturmanız gerekiyorsa [Azure Active Directory'de grup oluşturma ve üye ekleme](../active-directory-groups-create-azure-portal.md) makalesine bakın.

## <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

> [!VIDEO https://www.youtube.com/embed/Pa0eyqjEjvQ]

1. Mevcut Azure AD kiracınızdan, **Azure Active Directory** altında **Azure portal** üzerinde **Parola sıfırlama**’yı seçin.

2. **Özellikler** sayfasında, **Self servis parola sıfırlama etkinleştirildi** seçeneğinin altında **Seçili**'yi seçin.
    * **Grup seçin** bölümünde bu makalenin ön koşullar bölümünde oluşturduğunuz pilot grubunuzu seçin.
    * **Kaydet**’e tıklayın.

3. **Kimlik doğrulama yöntemleri** sayfasında aşağıdaki seçimleri yapın:
   * Sıfırlama için gereken yöntem sayısı: **1**
   * Kullanıcıların yararlanabileceği yöntemler:
      * **E-posta**
      * **Mobil uygulama kodu (Önizleme)**
   * **Kaydet**’e tıklayın.

     ![SSPR için kimlik doğrulama yöntemi seçme][Authentication]

4. **Kayıt** sayfasında aşağıdaki seçimleri yapın:
   * Oturum açarken kaydolmalarını iste: **Evet**
   * Kullanıcıların kendi kimlik doğrulama bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarlayın: **365**

## <a name="test-self-service-password-reset"></a>Self servis parola sıfırlamayı test etme

Şimdi SSPR yapılandırmanızı test kullanıcısıyla test edelim. Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, testi yönetici ayrıcalıklarına sahip bir kullanıcıyla yapmanız halinde alacağınız sonuç farklı olabilir. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi](concept-sspr-policy.md) makalemize bakın.

1. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) adresine gidin.
2. Yönetici olmayan test kullanıcısı ile oturum açın ve kimlik doğrulaması için kullanacağınız telefonu kaydedin.
3. İşlemi tamamladıktan sonra **iyi görünüyor** düğmesine tıklayın ve tarayıcı penceresini kapatın.
4. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/sspr](https://aka.ms/sspr) adresine gidin.
5. Yönetici olmayan test kullanıcınızın Kullanıcı Kimliğini ve CAPTCHA karakterlerini girdikten sonra **İleri**'ye tıklayın.
6. Parolanızı sıfırlamak için doğrulama adımlarını izleyin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Self servis parola sıfırlama kolayca devre dışı bırakılabilir. Azure AD kiracınızı açın ve gidin **özellikleri** > **parola sıfırlama**ve ardından **hiçbiri** altında **Self Servis parola sıfırlama Etkin**.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yalnızca bulut üzerindeki kullanıcılarınız için self servis parola sıfırlamasını hızlıca nasıl yapılandıracağınızı öğrendiniz. Daha ayrıntılı bir dağıtım için dağıtım kılavuzuna geçin.

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı dağıtma](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
