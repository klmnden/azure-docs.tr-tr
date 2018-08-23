---
title: Bir kullanıcıyı yönetmek nasıl Azure portalını kullanarak yönetilen kimlik atanan
description: Adım adım oluşturmak, liste ve kullanıcı silme hakkında yönergeler yönetilen kimlik atanır.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: bd6327aa5c16d57c550025667659f9245a5b0b65
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060911"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-portal"></a>Oluşturma, liste veya bir kullanıcı tarafından atanan kimliği Azure portalını kullanarak silme

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Yönetilen hizmet kimliği Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, listeleme ve bir kullanıcı tarafından atanan kimliği Azure portalını kullanarak silme öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Yönetilen hizmet kimliği ile bilmiyorsanız, kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan kimliği arasındaki fark](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve bir kullanıcı tarafından atanan kimliği silinemiyor.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) (liste), bir kullanıcı tarafından atanan kimlik özelliklerini okumak için rol.

## <a name="create-a-user-assigned-managed-identity"></a>Yönetilen kimlik atanmış bir kullanıcı oluşturun

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı oluşturmak için Azure aboneliği ile ilişkili bir hesap kullanarak, yönetilen kimlik atanır.
2. Arama kutusuna *yönetilen kimlikleri*, altında **Hizmetleri**, tıklayın **yönetilen kimlikleri**.
3. Tıklayın **Ekle** altında aşağıdaki alanlarda değerleri girin **oluşturma kullanıcı tarafından atanan yönetilen** kimlik bölmesi:
   - **Kaynak adı**: atanan kullanıcı kimlik, örneğin UAI1 yönetilen için ad budur.
   - **Abonelik**: kullanıcı tarafından atanan yönetilen kimliği altında oluşturulacağı aboneliği seçin
   - **Kaynak grubu**: atanan kullanıcı kimliğinizi içeren veya seçmek için yeni bir kaynak grubu oluşturma **var olanı kullan** kullanıcı oluşturmak için var olan bir kaynak grubundaki yönetilen kimlik atanır.
   - **Konum**: kullanıcı tarafından atanan yönetilen kimliği, örneğin dağıtmak için bir konum seçin **Batı ABD**.
4. **Oluştur**’a tıklayın.

![Yönetilen kimlik atanmış bir kullanıcı oluşturun](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-identities"></a>Liste kullanıcı kimlikleri atandı

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı listelemek için Azure aboneliği ile ilişkili bir hesap kullanarak yönetilen kimlikleri atandı.
2. Arama kutusuna *yönetilen kimlikleri*ve Hizmetleri altında **yönetilen kimlikleri**.
3. Aboneliğiniz için yönetilen kimliklerinin atanmış kullanıcı listesi döndürülür.  Bir kullanıcı tarafından atanan ayrıntılarını görmek için kendi adına tıklayın.

![Kullanıcı tarafından atanan yönetilen kimliği listesi](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-identity"></a>Bir kullanıcı tarafından atanan Kimliği Sil

1. Oturum [Azure portalında](https://portal.azure.com) bir kullanıcıyı silmek için Azure aboneliği ile ilişkili bir hesap kullanarak, yönetilen kimlik atanır.
2. Atanan kullanıcı kimliği'ni seçip tıklayın **Sil**.
3. Onay kutusunu seçin, **Evet**.

![Kullanıcı tarafından atanan yönetilen kimliği sil](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)