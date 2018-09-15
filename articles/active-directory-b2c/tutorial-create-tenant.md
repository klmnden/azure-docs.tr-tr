---
title: Öğretici - Azure Active Directory B2C kiracısı oluştur | Microsoft Docs
description: Azure portalını kullanarak bir Azure Active Directory B2C kiracısı oluşturarak, uygulamalarınızı kaydetmek için hazırlamayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d5831d868bec940c4e38f62e70e456ae84903ada
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604919"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracısı oluşturma

Uygulamalarınızı Azure Active Directory (Azure AD) B2C ile etkileşim kurabilmesi yönettiğiniz bir kiracıda kayıtlı olmaları gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

Sonraki öğreticide bir uygulamayı kaydetme öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. Seçin **kaynak Oluştur** Azure portalının sol üst köşedeki içinde.
2. Arama kutusuna Azure Market kaynakları yukarıdaki listede, aramak ve seçmek **Active Directory B2C**ve ardından **Oluştur**.
3. Seçin **yeni bir Azure AD B2C Kiracısı oluşturma**, bir kuruluş adı ve Kiracı adı kullanılan ilk etki alanı adı girin, bulunduğunuz ülkeyi seçin ve ardından **Oluştur**. Daha sonra değiştirilemez çünkü kiracının ülke emin olun.

    ![Kiracı oluşturma](./media/tutorial-create-tenant/create-tenant.png)

    Bu örnekte contoso0522Tenant.onmicrosoft.com Kiracı adı:

Yeni kiracınızı yönetmeye başlamak için word'ü tıklatın. **burada** burada yazan **yeni dizininizi yönetmek için buraya tıklayın**. Göreceğiniz bir **sorun giderme** aboneliğinizi yeni kiracıma gerektiğini belirten ileti. 

## <a name="link-your-tenant-to-your-subscription"></a>Kiracı aboneliğinize bağlayın

Tüm işlevleri etkinleştirmek ve kullanım ücretlerini ödemek için Azure AD B2C kiracınızı Azure aboneliğinize bağlamanız gerekir. Aboneliğinize kiracınızı bağlama, uygulamalarınızın düzgün çalışmaz.

Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme. 

![Azure AD B2C kiracınıza geçiş yapma](./media/tutorial-create-tenant/switch-directories.png)

1. Seçin **kaynak Oluştur** Azure portalının üst sol üst köşedeki.
2. Arama kutusuna Azure Market kaynakları yukarıdaki listede, aramak ve seçmek **Active Directory B2C**ve ardından **Oluştur**.
3. Seçin **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına**, oluşturduğunuz Kiracı seçin, aboneliğinizi seçin, girin *myContosoTenantRG* kaynak grubu adı kabul edin konum ve ardından **Oluştur**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

> [!div class="nextstepaction"]
> [Hesaplarla kimlik doğrulaması bir web uygulamasını etkinleştir](active-directory-b2c-tutorials-web-app.md)