---
title: Bir Azure Active Directory B2C kiracısı oluşturma | Microsoft Docs
description: Azure Active Directory B2C kiracısının nasıl oluşturulacağına ilişkin konu başlığı
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 06/07/2017
ms.author: davidmu
ms.openlocfilehash: 56e0ae7454e86911c894da88b5aa8ccc03a08af3
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a>Azure portalında bir Azure Active Directory B2C kiracısı oluşturma

Bu Hızlı Başlangıç, yalnızca birkaç dakika içinde bir Microsoft Azure Active Directory (Azure AD) B2C Kiracı oluşturmanıza yardımcı olur. İşlemi tamamladığınızda, B2C uygulamaları kaydetmek için kullanılacak bir B2C kiracısı (dizin olarak da bilinir) sahip.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

B2C özellikleri, var olan kiracılar etkinleştirilemez. Azure AD B2C kiracısı oluşturmanız gerekir.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Tebrikler, Azure Active Directory B2C kiracısı oluşturdunuz. Kiracı genel Yöneticisi olduğunuz. Gerektiğinde başka Genel Yöneticiler ekleyebilirsiniz. Yeni Kiracı için geçiş yapmak için tıklatın *yeni Kiracı bağlantıyı Yönet*.

![Yeni Kiracı bağlantıyı Yönet](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Bir üretim uygulaması için bir B2C Kiracı kullanmayı planlıyorsanız, makaleyi okuyun [Önizleme B2C kiracılar ve üretim ölçekli](active-directory-b2c-reference-tenant-type.md). Mevcut bir B2C Kiracı silin ve aynı etki alanı adıyla yeniden oluştururken bilinen sorunlar vardır. Farklı bir etki alanı adı ile bir B2C Kiracı oluşturmanız gerekir.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Kiracı aboneliğinize bağlantı

Azure aboneliğiniz için kullanım ücretleri ödeme tüm B2C işlevselliğini etkinleştirmek için Azure AD B2C kiracınızın bağlamanız gerekir. Daha fazla bilgi edinmek için okuma [bu makalede](active-directory-b2c-how-to-enable-billing.md). Azure aboneliğinize Azure AD B2C kiracınızın bağlantı yok, bazı işlevler engellenir ve bir uyarı iletisi ("abonelik bu B2C Kiracı veya abonelik gereksinimlerinize, ilgilenilmesi gerekiyor. B2C ayarlarında bağlı") bakın. Üretime uygulamalarınızı sevk önce bu adımı uygulamanız önemlidir.

## <a name="easy-access-to-settings"></a>Kolay erişim ayarları

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Girerek dikey erişebilirsiniz `Azure AD B2C` içinde **arama kaynakları** portalı üstündeki. Sonuçlar listesinde **Azure AD B2C** B2C ayarlar dikey erişmek için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [B2C Kiracı B2C uygulamanızı kaydetme](active-directory-b2c-app-registration.md)