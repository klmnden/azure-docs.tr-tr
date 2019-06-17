---
title: Öğretici - Azure Active Directory B2C kiracısı oluşturma
description: Azure portalını kullanarak bir Azure Active Directory B2C kiracısı oluşturarak, uygulamalarınızı kaydetmek için hazırlamayı öğrenin.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 711b9152f9f3fa1b3573e39d1950f18b628c268a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056327"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracısı oluşturma

Uygulamalarınızı Azure Active Directory (Azure AD) B2C ile etkileşim kurabilmesi yönettiğiniz bir kiracıda kayıtlı olmaları gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

Sonraki öğreticide bir uygulamayı kaydetme öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Aboneliğinizi içeren dizine kullandığınızdan emin olun. Tıklayın **dizin ve abonelik filtresi** üst menüden, ardından aboneliğinizi içeren dizini seçin. Bu Azure AD B2C kiracınızı içerecek farklı dizindir.

    ![Abonelik dizinine geçin](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Seçin **kaynak Oluştur** Azure portalının sol üst köşedeki içinde.
4. Arayın ve seçin **Active Directory B2C**ve ardından **Oluştur**.
5. Seçin **yeni bir Azure AD B2C Kiracısı oluşturma** bir kuruluş adı ve ilk etki alanı adı girin. (Bunu daha sonra değiştirilemez) ülke/bölge seçin ve ardından **Oluştur**.

    İlk etki alanı adı, kiracınızın adının bir parçası olarak kullanılır. Bu örnekte, Kiracı addır *contoso0926Tenant.onmicrosoft.com*:

    ![Kiracı oluşturma](./media/tutorial-create-tenant/create-tenant.PNG)

6. Üzerinde **yeni B2C Kiracısı oluşturun veya mevcut Kiracıya bağlantı** sayfasında **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına**.

    Oluşturduğunuz Kiracı seçin ve aboneliğinizi seçin.

    Kaynak grubunu seçin **Yeni Oluştur**. Kiracı içeren, konumu seçin ve ardından kaynak grubu için bir ad girin **Oluştur**.
1. Yeni Kiracı kullanmaya başlamak için Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve onu içeren dizine seçme.

    İlk olarak yeni Azure B2C kiracınızın listede görmüyorsanız, tarayıcı pencerenizi yenileyin ve ardından **dizin ve abonelik filtresi** üst menüde.

    ![Dizin Kiracı anahtarı](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

Ardından, yeni kiracınızda bir web uygulaması kaydetme konusunda bilgi edinin.

> [!div class="nextstepaction"]
> [Uygulamalarınızı kayıt >](tutorial-register-applications.md)
