---
title: Öğretici - Azure Active Directory B2C kiracısı oluşturma | Microsoft Docs
description: Azure Portalı'nı kullanarak bir Azure Active Directory B2C Kiracı oluşturarak uygulamalarınızı kayıt için hazırlama hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/19/2018
ms.author: davidmu
ms.openlocfilehash: 04f3dbbe461bfe0f07b6930a92bdd8a721e55098
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296102"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracısı oluşturma

Uygulamalarınızı Azure Active Directory (Azure AD) B2C ile etkileşim kurabilmesi için yönettiğiniz Kiracı kaydedilmelidir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlantı

Sonraki öğreticide bir uygulamayı kaydetme öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. Seçin **kaynak oluşturma** Azure portalının sol üst köşedeki.
2. Arama kutusuna Azure Marketi kaynakların listesi yukarıda aramak ve seçmek **Active Directory B2C**ve ardından **oluşturma**.
3. Seçin **yeni bir Azure AD B2C Kiracısı oluşturma**, bir kuruluş adı ve Kiracı adında kullanılan ilk etki alanı adını girin, ülkeyi seçin ve ardından **oluşturma**. Daha sonra değiştirilemez çünkü Kiracı ülke emin olun.

    ![Kiracı oluşturma](./media/tutorial-create-tenant/create-tenant.png)

    Bu örnekte contoso0522Tenant.onmicrosoft.com Kiracı adıdır

Yeni Kiracı yönetmeye başlamak için word'ü tıklatın. **burada** burada yazacaktır **yeni dizininize yönetmek için buraya tıklayın**. Göreceğiniz bir **sorun giderme** yeni Kiracı aboneliğinize bağlanmak gereken bildiren iletisi. 

## <a name="link-your-tenant-to-your-subscription"></a>Kiracı aboneliğinize bağlantı

Azure aboneliğiniz için kullanım ücretleri ödeme tüm işlevselliğini etkinleştirmek için Azure AD B2C kiracınızın bağlamanız gerekir. Aboneliğiniz için Kiracı yok bağlarsanız, uygulamalarınızın düzgün çalışmaz.

1. Azure portalının sağ üst köşesinde dizin geçerek yeni kiracıya ilişkilendirmek istediğiniz aboneliği içeren dizine kullandığınızdan emin olun.

    ![Dizinleri değiştir](./media/tutorial-create-tenant/switch-directories.png)

    Ve aboneliğinizi içeren dizine seçme.

    ![Dizin seçme](./media/tutorial-create-tenant/select-directory.png)

2. Seçin **kaynak oluşturma** Azure portal'ın üst sol üst köşedeki.
3. Arama kutusuna Azure Marketi kaynakların listesi yukarıda aramak ve seçmek **Active Directory B2C**ve ardından **oluşturma**.
4. Seçin **Azure Aboneliğim bağlantı var olan bir Azure AD B2C Kiracısına**, oluşturduğunuz Kiracı seçin, aboneliğinizi seçin, girin *myContosoTenantRG* ve kaynak grubu adı kabul edin konum ve ardından **oluşturma**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlantı

> [!div class="nextstepaction"]
> [Bir web uygulaması ile hesaplarının kimliğini doğrulamak amacıyla etkinleştir](active-directory-b2c-tutorials-web-app.md)