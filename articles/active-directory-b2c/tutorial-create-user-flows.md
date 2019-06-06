---
title: Öğretici - Azure Active Directory B2C - kullanıcı akışları oluşturma | Microsoft Docs
description: Azure portalını kullanarak Azure Active Directory B2C, uygulamalar için kullanıcı akışları oluşturmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/01/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: fd8aefa924b927d9a55d0e91f35f40060097c53b
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512147"
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
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.

    ![Abonelik dizinine geçin](./media/tutorial-create-user-flows/switch-directories.png)

3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Sol menüde **kullanıcı akışları**ve ardından **yeni kullanıcı akışı**.

    ![Yeni kullanıcı akışı seçin](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

5. Seçin **kaydolma ve oturum açma** önerilen sekmesinde kullanıcı akışı.

    ![Kaydolma ve oturum açma kullanıcı akışı seçin](./media/tutorial-create-user-flows/signup-signin-type.png)

6. Girin bir **adı** kullanıcı akışı için. Örneğin, *signupsignin1*.
7. İçin **kimlik sağlayıcıları**seçin **e-posta kaydolma**.

    ![Flow özelliklerini ayarlama](./media/tutorial-create-user-flows/signup-signin-properties.png)

8. İçin **kullanıcı öznitelikleri ve talepler**, talepleri ve toplamak ve kayıt sırasında kullanıcıdan göndermek istediğiniz öznitelikleri seçin. Örneğin, **daha fazla Göster**ve ardından **ülke/bölge**, **görünen ad**, ve **posta kodu**. **Tamam** düğmesine tıklayın.

    ![Öznitelikleri ve talepler seçin](./media/tutorial-create-user-flows/signup-signin-attributes.png)

9. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Oluşturduğunuz kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından **şimdi kaydolun**.

    ![Kullanıcı akışı çalıştırma](./media/tutorial-create-user-flows/signup-signin-run-now.png)

4. Geçerli bir e-posta adresini girin, tıklayın **doğrulama kodu Gönder**ve ardından aldığınız doğrulama kodunu girin.
5. Yeni bir parola girin ve parolayı onaylayın.
6. Görüntülenmesini istediğiniz adı girin, ülke ve bölge seçin, bir posta kodu girin ve ardından **Oluştur**. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.
7. Artık kullanıcı akışı tekrar çalıştırabilirsiniz ve oluşturduğunuz hesapla oturum açabilmelisiniz. Döndürülen belirteç adı, ülke/bölge ve posta kodu seçtiğiniz talepleri içerir.

## <a name="create-a-profile-editing-user-flow"></a>Kullanıcı akışı düzenleme profili oluşturma

Kullanıcıların uygulamanızda profillerini düzenleyebilir etkinleştirmek istiyorsanız, kullanıcı akışı düzenleme profili kullanın.

1. Sol menüde **kullanıcı akışları**ve ardından **yeni kullanıcı akışı**.
2. Seçin **profil düzenleme** önerilen sekmesinde kullanıcı akışı.
3. Girin bir **adı** kullanıcı akışı için. Örneğin, *profileediting1*.
4. İçin **kimlik sağlayıcıları**seçin **yerel hesapla oturum aç**.
5. İçin **kullanıcı öznitelikleri**, müşterinin profilinde düzenleyebilmek için istediğiniz öznitelikleri seçin. Örneğin, **daha fazla Göster**ve ardından **görünen ad** ve **iş unvanı**. **Tamam**'ı tıklatın.
6. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Oluşturduğunuz kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından daha önce oluşturduğunuz hesabıyla oturum açın.
4. Artık kullanıcı görünen adı ve iş başlığını değiştirme fırsatını var. **Devam**’a tıklayın. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.

## <a name="create-a-password-reset-user-flow"></a>Parola sıfırlama kullanıcı akışı oluştur

Gerekirse, kullanıcının parolasını sıfırlamak için uygulamanızın kullanıcı etkinleştirmeniz için mümkündür. Parola sıfırlamayı etkinleştirmek için bir parola sıfırlama kullanıcı akışını kullanın.

1. Sol menüde **kullanıcı akışları**ve ardından **yeni kullanıcı akışı**.
2. Seçin **parola sıfırlama** önerilen sekmesinde kullanıcı akışı.
3. Girin bir **adı** kullanıcı akışı için. Örneğin, *passwordreset1*.
4. İçin **kimlik sağlayıcıları**, etkinleştirme **e-posta adresi kullanarak parola Sıfırla**.
5. Uygulama talepleri altında tıklayın **daha fazla Göster** ve uygulamanıza geri gönderilen yetkilendirme belirteçlerinde döndürülmesini istediğiniz talepleri seçin. Örneğin, **Kullanıcının Nesne Kimliği**’ni seçin.
6. **Tamam**'ı tıklatın.
7. Tıklayın **Oluştur** kullanıcı akışı eklemek için. Bir önek *B2C_1* adına otomatik olarak eklenir.

### <a name="test-the-user-flow"></a>Kullanıcı akışı test edin

1. Oluşturduğunuz kullanıcı akışı genel bakış sayfasında **kullanıcı akışı çalıştırma**.
2. İçin **uygulama**, adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. **Yanıt URL'si** göstermelidir `https://jwt.ms`.
3. Tıklayın **kullanıcı akışı çalıştırma**ve ardından daha önce oluşturduğunuz hesabıyla oturum açın.
4. Artık kullanıcı parolası değiştirme fırsatını var. **Devam**’a tıklayın. Döndürülen belirteç `https://jwt.ms` ve size görüntülenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Kaydolma ve oturum açma kullanıcı akışı oluştur
> * Kullanıcı akışı düzenleme profili oluşturma
> * Parola sıfırlama kullanıcı akışı oluştur

> [!div class="nextstepaction"]
> [Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin](tutorial-add-identity-providers.md)