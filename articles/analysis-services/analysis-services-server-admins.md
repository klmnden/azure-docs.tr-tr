---
title: Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs
description: Azure Analysis Services sunucusu için sunucu yöneticileri yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ec1630f4de70f77c13e335c68aff16180e524c12
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307817"
---
# <a name="manage-server-administrators"></a>Sunucu yöneticileri yönetin
Sunucu yöneticileri, sunucunun bulunduğu Kiracı için Azure Active Directory'de (Azure AD) geçerli bir kullanıcı veya güvenlik grubu olmalıdır. Kullanabileceğiniz **Analysis Services Admins** sunucunuzun Azure portalında veya sunucu yöneticileri yönetmek için SSMS sunucu özelliklerinde. 

> [!NOTE]
> Güvenlik grupları olmalıdır `MailEnabled` özelliğini `True`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Azure portal kullanarak sunucu yöneticileri eklemek için
1. İçinde, sunucunuz için portal'ı tıklatın **Analysis Services Admins**.
2. İçinde  **\<sunucuadı >-Analysis Services Admins**, tıklatın **Ekle**.
3. İçinde **sunucu yöneticileri ekleme**, kullanıcı hesapları Azure AD'nizi seçin veya e-posta adresiyle dış kullanıcıları davet.

    ![Sunucu yöneticileri Azure portalında](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>SSMS kullanarak sunucu yöneticileri eklemek için
1. Sunucuya sağ tıklayın > **özellikleri**.
2. İçinde **Analysis Server özellikleri**, tıklatın **güvenlik**.
3. Tıklatın **Ekle**ve ardından e-posta adresi, Azure AD'de kullanıcı veya grup için girin.
   
    ![Sunucu yöneticileri SSMS Ekle](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Sonraki adımlar 
[Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md)  
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Rol Tabanlı Access Control](../role-based-access-control/overview.md)  

