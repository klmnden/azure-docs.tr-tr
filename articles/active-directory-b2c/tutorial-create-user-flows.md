---
title: Öğretici - Azure Active Directory B2C - kullanıcı akışları oluşturma
description: Azure portalında oturum yukarı etkinleştirmek için oturum ve kullanıcı için Azure Active Directory B2C uygulamalarınızın, profil düzenleme kullanıcı akışları oluşturmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 111196388d0e17ecde8a2055959f2f573e43ade8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056373"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'de kullanıcı akışları oluşturma

Uygulamalarınızda etkinleştirmiş olabilirsiniz [kullanıcı akışları](active-directory-b2c-reference-policies.md) kaydolma, oturum açın veya profillerini yönetmek kullanıcıları etkinleştirin. Azure Active Directory (Azure AD) B2C kiracınızda farklı türlerde birden çok kullanıcı akışları oluşturma ve bunları gerektiği şekilde uygulamalarınızda kullanın. Kullanıcı akışları uygulamalar arasında yeniden kullanılabilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kaydolma ve oturum açma kullanıcı akışı oluştur
> * Kullanıcı akışı düzenleme profili oluşturma
> * Parola sıfırlama kullanıcı akışı oluştur

Bu öğreticide Azure portalını kullanarak bazı önerilen kullanıcı akışları oluşturulacağını gösterir. Kaynak sahibi parola kimlik bilgileri (ROPC) akış uygulamanızda ayarlama hakkında bilgi arıyorsanız bkz [kaynak sahibi parola kimlik bilgileri akışı Azure AD B2C'de yapılandırma](configure-ropc.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Uygulamalarınızı kaydetme](tutorial-register-applications.md) oluşturmak istediğiniz kullanıcı akışları bir parçası.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Kaydolma ve oturum açma kullanıcı akışı oluştur

Kullanıcı, kaydolma ve oturum açma akışını tek bir yapılandırma ile hem kaydolma ve oturum açma deneyimlerini işler. Uygulamanızın kullanıcılarının, bağlama bağlı olarak doğru yolunu gerektiriyordu.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.

    ![Abonelik dizinine geçin](./media/tutorial-create-user-flows/switch-directories.PNG)

1. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Altında soldaki menüde **ilkeleri**seçin **kullanıcı akışları (ilke)** ve ardından **yeni kullanıcı akışı**.

    ![Yeni kullanıcı akışı seçin](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

1. Üzerinde **önerilen** sekmesinde **oturum yukarı ve oturum açma** kullanıcı akışı.

    ![Kaydolma ve oturum açma kullanıcı akışı seçin](./media/tutorial-create-user-flows/signup-signin-type.png)

1. Girin bir **adı** kullanıcı akışı için. Örneğin, *signupsignin1*.
1. İçin **kimlik sağlayıcıları**seçin **e-posta kaydolma**.

    ![Flow özelliklerini ayarlama](./media/tutorial-create-user-flows/signup-signin-properties.png)

1. İçin **kullanıcı öznitelikleri ve talepler**, talepleri ve toplamak ve kayıt sırasında kullanıcıdan göndermek istediğiniz öznitelikleri seçin. Örneğin, **daha fazla Göster**, öznitelikleri ve talepler için seçin **ülke/bölge**, **görünen ad**, ve **posta kodu**. **Tamam**'ı tıklatın.

    ![Öznitelikleri ve talepler seçin](./media/tutorial-create-user-flows/signup-signin-attributes.png)

1. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Kullanıcı akışı oluşturuldu, genel bakış sayfasını açın ve ardından seçin **kullanıcı akışı çalıştırma**.
1. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
1. Tıklayın **kullanıcı akışı çalıştırma**ve ardından **şimdi kaydolun**.

    ![Kullanıcı akışı çalıştırma](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Geçerli bir e-posta adresini girin, tıklayın **doğrulama kodu Gönder**, alırsınız ve ardından seçin doğrulama kodunu girin **kodunu doğrulayın**.
1. Yeni bir parola girin ve parolayı onaylayın.
1. Ülke ve bölge seçin, görüntülenmesini istediğiniz adı girin, bir posta kodu girin ve ardından **Oluştur**. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.
1. Artık kullanıcı akışı tekrar çalıştırabilirsiniz ve oluşturduğunuz hesapla oturum açabilmelisiniz. Döndürülen belirteç, ülke/bölge, ad ve posta kodu seçtiğiniz talepleri içerir.

## <a name="create-a-profile-editing-user-flow"></a>Kullanıcı akışı düzenleme profili oluşturma

Kullanıcıların uygulamanızda profillerini düzenleyebilir etkinleştirmek istiyorsanız, kullanıcı akışı düzenleme profili kullanın.

1. Azure AD B2C Kiracı genel bakış sayfasında sol taraftaki menüde seçin **kullanıcı akışları (ilke)** ve ardından **yeni kullanıcı akışı**.
1. Seçin **profil düzenleme** önerilen sekmesinde kullanıcı akışı.
1. Girin bir **adı** kullanıcı akışı için. Örneğin, *profileediting1*.
1. İçin **kimlik sağlayıcıları**seçin **yerel hesapla oturum aç**.
1. İçin **kullanıcı öznitelikleri**, müşterinin profilinde düzenleyebilmek için istediğiniz öznitelikleri seçin. Örneğin, **daha fazla Göster**, öznitelikleri ve talepler için hem seçin **görünen ad** ve **iş unvanı**. **Tamam**'ı tıklatın.
1. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Kullanıcı akışı oluşturuldu, genel bakış sayfasını açın ve ardından seçin **kullanıcı akışı çalıştırma**.
1. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
1. Tıklayın **kullanıcı akışı çalıştırma**ve ardından daha önce oluşturduğunuz hesabıyla oturum açın.
1. Artık kullanıcı görünen adı ve iş başlığını değiştirme fırsatını var. **Devam**’a tıklayın. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı oluştur

Kullanıcının parolasını sıfırlamak için uygulamanızın kullanıcılarının etkinleştirmek için bir parola sıfırlama kullanıcı akışını kullanın.

1. Sol menüde **kullanıcı akışları (ilke)** ve ardından **yeni kullanıcı akışı**.
1. Seçin **parola sıfırlama** önerilen sekmesinde kullanıcı akışı.
1. Girin bir **adı** kullanıcı akışı için. Örneğin, *passwordreset1*.
1. İçin **kimlik sağlayıcıları**, etkinleştirme **e-posta adresi kullanarak parola Sıfırla**.
1. Uygulama talepleri altında tıklayın **daha fazla Göster** ve uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.
1. **Tamam** düğmesine tıklayın.
1. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Kullanıcı akışı oluşturuldu, genel bakış sayfasını açın ve ardından seçin **kullanıcı akışı çalıştırma**.
1. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
1. Tıklayın **kullanıcı akışı çalıştırma**, daha önce oluşturduğunuz ve seçin hesabının e-posta adresi doğrulama **devam**.
1. Artık kullanıcı parolası değiştirme fırsatını var. Parola değiştirme ve seçin **devam**. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Kaydolma ve oturum açma kullanıcı akışı oluştur
> * Kullanıcı akışı düzenleme profili oluşturma
> * Parola sıfırlama kullanıcı akışı oluştur

Ardından, kullanıcı oturum açma sağlayıcıları gibi Azure AD ile etkinleştirmek için uygulamalarınıza kimlik sağlayıcıları ekleme hakkında bilgi edinin Amazon, Facebook, GitHub, LinkedIn, Microsoft veya Twitter.

> [!div class="nextstepaction"]
> [Kimlik sağlayıcıları uygulamalarınıza ekleyin >](tutorial-add-identity-providers.md)