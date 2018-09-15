---
title: Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik yönetme
description: Adım adım yönergeler, listesi oluşturmak silin ve kullanıcı tarafından atanan bir yönetilen kimlik bir rol atayın.
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
ms.openlocfilehash: 180d4092a2570b719e77d98319ab9b329f2e48c5
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637432"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Oluşturma, listeleme, silme veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik için rol atama

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Azure kaynakları için yönetilen kimlikleri Azure Active Directory'deki yönetilen bir kimlikle Azure hizmetleri sağlar. Bu kimlik, kimlik bilgileri, kodunuzda gerek kalmadan Azure AD kimlik doğrulaması, Destek Hizmetleri kimlik doğrulaması için kullanabilirsiniz. 

Bu makalede, oluşturma, listesinde, silmek veya rol atama öğrenin bir kullanıcı tarafından atanan kimliği Azure portalını kullanarak yönetilen.

## <a name="prerequisites"></a>Önkoşullar

- Azure kaynakları için yönetilen kimliklerle bilmiyorsanız kullanıma [genel bakış bölümünde](overview.md). **Gözden geçirmeyi unutmayın [sistem tarafından atanan ve kullanıcı tarafından atanan bir yönetilen kimlik arasındaki farkı](overview.md#how-does-it-work)**.
- Henüz bir Azure hesabınız yoksa, devam etmeden önce [ücretsiz bir hesaba kaydolun](https://azure.microsoft.com/free/).
- Bu makalede yönetim işlemlerini gerçekleştirmek için aşağıdaki rol atamaları hesabınızın gerekir:
    - [Yönetilen kimlik Katılımcısı](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) rolü oluşturmak için (liste) okuma, güncelleştirme ve kullanıcı tarafından atanan bir yönetilen kimlik silin.
    - [Yönetilen kimlik işleci](/azure/role-based-access-control/built-in-roles#managed-identity-operator) rol (liste) kullanıcı tarafından atanan bir yönetilen kimlik özelliklerini okuyun.

## <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı tarafından atanan bir yönetilen kimlik oluşturmak için Azure aboneliği ile ilişkili bir hesap kullanarak.
2. Arama kutusuna *yönetilen kimlikleri*, altında **Hizmetleri**, tıklayın **yönetilen kimlikleri**.
3. Tıklayın **Ekle** altında aşağıdaki alanlarda değerleri girin **oluşturma kullanıcı tarafından atanan yönetilen** kimlik bölmesi:
   - **Kaynak adı**: Bu kullanıcı tarafından atanan yönetilen kimliğinizi, örneğin UAI1 adıdır.
   - **Abonelik**: kullanıcı tarafından atanan yönetilen kimlik altında oluşturulacağı aboneliği seçin
   - **Kaynak grubu**: kullanıcı tarafından atanan yönetilen kimliğinizi içeren veya seçmek için yeni bir kaynak grubu oluşturma **var olanı kullan** kullanıcı tarafından atanan bir yönetilen kimlik mevcut bir kaynak grubu oluşturmak için.
   - **Konum**: kullanıcı tarafından atanan yönetilen kimlik, örneğin dağıtmak için bir konum seçin **Batı ABD**.
4. **Oluştur**’a tıklayın.

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri listesi

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliği ile ilişkili bir hesap kullanarak.
2. Arama kutusuna *yönetilen kimlikleri*ve Hizmetleri altında **yönetilen kimlikleri**.
3. Aboneliğiniz için kullanıcı tarafından atanan yönetilen kimliklerinin bir listesi döndürülür.  Kullanıcı tarafından atanan bir yönetilen kimlik adına tıklayın ayrıntılarını görmek için.

![Kullanıcı tarafından atanan yönetilen kimlik listesi](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik Sil

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı tarafından atanan bir yönetilen kimlik silmek için Azure aboneliği ile ilişkili bir hesap kullanarak.
2. Kullanıcı tarafından atanan bir yönetilen kimlik seçip tıklayın **Sil**.
3. Onay kutusunu seçin, **Evet**.

![Kullanıcı tarafından atanan yönetilen kimlik Sil](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan bir yönetilen kimlik rol atama 

1. Oturum [Azure portalında](https://portal.azure.com) kullanıcı tarafından atanan yönetilen kimlikleri listelemek için Azure aboneliği ile ilişkili bir hesap kullanarak.
2. Arama kutusuna *yönetilen kimlikleri*ve Hizmetleri altında **yönetilen kimlikleri**.
3. Aboneliğiniz için kullanıcı tarafından atanan yönetilen kimliklerinin bir listesi döndürülür.  Kullanıcı tarafından atanan ve bir rol atamak istediğiniz yönetilen kimlik seçin.
4. Seçin **erişim denetimi (IAM)** seçip **Ekle**.

   ![Kullanıcı tarafından atanan yönetilen kimlik Başlat](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Ekle izinler dikey penceresinde aşağıdaki değerleri yapılandırın ve ardından **Kaydet**:
   - **Rol** -rol atamak için
   - **Erişim Ata** -kullanıcı tarafından atanan atamak için kaynak yönetilen kimlik
   - **Seçin** -üye erişimi atamak için
   
   ![Kullanıcı tarafından atanan yönetilen kimlik IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  