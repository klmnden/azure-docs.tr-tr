---
title: Öğretici - Azure Active Directory B2C kiracısı oluştur | Microsoft Docs
description: Azure portalını kullanarak bir Azure Active Directory B2C kiracısı oluşturarak, uygulamalarınızı kaydetmek için hazırlamayı öğrenin.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7571e5f4d95320ab92fa3b69b0ea1f05ff9c771f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408411"
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
2. Tıklayarak aboneliğinizi içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve onu içeren dizine seçme. Bu, Azure AD B2C kiracınızı içerecek olandan farklı bir dizindir.

    ![Abonelik dizinine geçin](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Seçin **kaynak Oluştur** Azure portalının sol üst köşedeki içinde.
4. Arayın ve seçin **Active Directory B2C**ve ardından **Oluştur**.
5. Seçin **yeni bir Azure AD B2C Kiracısı oluşturma**bir kuruluş adı ve Kiracı adı kullanılan ilk etki alanı adı girin (bunu daha sonra değiştirilemez) ülkeyi seçin ve ardından **Oluştur**.

    ![Kiracı oluşturma](./media/tutorial-create-tenant/create-tenant.png)

    Bu örnekte contoso0926Tenant.onmicrosoft.com Kiracı adı:

6. Üzerinde **yeni B2C Kiracısı oluşturun veya mevcut bir Kiracınız için bağlantı** sayfasında **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına**, oluşturduğunuz Kiracı seçin, aboneliğinizi seçin,'a tıklayın **Yeni Oluştur** ve Kiracı içeren, konumu seçin ve ardından kaynak grubu için bir ad girin **Oluştur**.
7. Yeni Kiracı kullanmaya başlamak için Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve onu içeren dizine seçme.

    ![Dizin Kiracı anahtarı](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

> [!div class="nextstepaction"]
> [Hesaplarla kimlik doğrulaması bir web uygulamasını etkinleştir](active-directory-b2c-tutorials-web-app.md)