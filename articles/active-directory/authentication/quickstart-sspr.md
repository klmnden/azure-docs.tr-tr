---
title: 'Hızlı başlangıç: Azure AD self servis parola sıfırlama'
description: Bu hızlı başlangıçta kullanıcıların kendi parolalarını sıfırlamalarını sağlamak için Azure AD self servis parola sıfırlama özelliğini hızlı bir şekilde yapılandıracaksınız
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: quickstart
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: c40cb3192d514d990ea2a5d66e1484ff204e9b10
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223566"
---
# <a name="quickstart-self-service-password-reset"></a>Hızlı Başlangıç: Self servis parola sıfırlama

Bu hızlı başlangıçta BT uzmanlarının kullanıcılara parolalarını sıfırlama veya hesaplarının kilidini açma yetkisi vermesi için basit bir yöntem sunan self servis parola sıfırlama (SSPR) özelliğini yapılandıracaksınız.

## <a name="prerequisites"></a>Ön koşullar

* En az deneme sürümü lisansı etkinleştirilmiş çalışan bir Azure AD kiracısına erişim.
* Genel Yönetici ayrıcalıklarına sahip olan bir hesap.
* Test için kullanacağınız yönetici olmayan bir kullanıcı adı ve parola. Kullanıcı oluşturmanız gerekiyorsa [Hızlı başlangıç: Azure Active Directory'ye yeni kullanıcı ekleme](../add-users-azure-active-directory.md) makalesine bakın.
* Bu yönetici olmayan test kullanıcısının üyesi olduğu pilot grup. Grup oluşturmanız gerekiyorsa [Azure Active Directory'de grup oluşturma ve üye ekleme](../active-directory-groups-create-azure-portal.md) makalesine bakın.

## <a name="enable-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı etkinleştirme

> [!VIDEO https://www.youtube.com/embed/Pa0eyqjEjvQ]

1. Mevcut Azure AD kiracınızdan, **Azure Active Directory** altında **Azure portal** üzerinde **Parola sıfırlama**’yı seçin.

2. **Özellikler** sayfasında, **Self servis parola sıfırlama etkinleştirildi** seçeneğinin altında **Seçili**'yi seçin.
    * **Grup seçin** bölümünde bu makalenin ön koşullar bölümünde oluşturduğunuz pilot grubunuzu seçin.
    * **Kaydet**’e tıklayın.

3. **Kimlik doğrulama yöntemleri** sayfasında aşağıdaki seçimleri yapın:
   * Sıfırlama için gereken yöntemlerin sayısı: **1**
   * Kullanıcıların yararlanabileceği yöntemler:
      * **Cep telefonu**
      * **Ofis telefonu**
   * **Kaydet**’e tıklayın.

    ![Kimlik doğrulaması][Authentication]

4. **Kayıt** sayfasında aşağıdaki seçimleri yapın:
   * Kullanıcılardan oturum açarken kaydolmalarını isteme: **Evet**
   * Kullanıcıların kimlik doğrulaması bilgilerini yeniden onaylamasını istemeden önce geçen gün sayısını ayarlama: **365**

## <a name="test-self-service-password-reset"></a>Self servis parola sıfırlamayı test etme

Şimdi SSPR yapılandırmanızı test kullanıcısıyla test edelim. Microsoft, Azure yönetici hesapları için güçlü kimlik doğrulama gereksinimleri uyguladığından, testi yönetici ayrıcalıklarına sahip bir kullanıcıyla yapmanız halinde alacağınız sonuç farklı olabilir. Yönetici parolası ilkesiyle ilgili daha fazla bilgi için [parola ilkesi](concept-sspr-policy.md) makalemize bakın.

1. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) adresine gidin.
2. Yönetici olmayan test kullanıcısı ile oturum açın ve kimlik doğrulaması için kullanacağınız telefonu kaydedin.
3. İşlemi tamamladıktan sonra **iyi görünüyor** düğmesine tıklayın ve tarayıcı penceresini kapatın.
4. InPrivate modunda veya gizli modda yeni bir tarayıcı penceresi açın ve [https://aka.ms/sspr](https://aka.ms/sspr) adresine gidin.
5. Yönetici olmayan test kullanıcınızın Kullanıcı Kimliğini ve CAPTCHA karakterlerini girdikten sonra **İleri**'ye tıklayın.
6. Parolanızı sıfırlamak için doğrulama adımlarını izleyin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Self servis parola sıfırlama kolayca devre dışı bırakılabilir. Azure AD kiracınızı açın, **Parola Sıfırlama** > **Özellikler**'e gidin ve sonra da **Self Servis Parola Sıfırlama Etkinleştirildi** alanında **Hiçbiri**'ni seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta yalnızca bulut üzerindeki kullanıcılarınız için self servis parola sıfırlamasını hızlıca nasıl yapılandıracağınızı öğrendiniz. Daha ayrıntılı bir dağıtım için dağıtım kılavuzuna geçin.

> [!div class="nextstepaction"]
> [Self servis parola sıfırlamayı dağıtma](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Kullanılabilir Azure AD kimlik doğrulama yöntemleri ve gereken miktar"
