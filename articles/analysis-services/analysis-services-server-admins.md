---
title: Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs
description: Azure Analysis Services sunucusu için sunucu yöneticileri yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: def09f2853f761f3fefca80f341e6cc0557bac86
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="manage-server-administrators"></a>Sunucu yöneticileri yönetin
Sunucu yöneticileri, sunucunun bulunduğu Kiracı için geçerli bir kullanıcı veya grup Azure Active Directory'de (Azure AD) olması gerekir. Kullanabileceğiniz **Analysis Services Admins** sunucunuzun Azure portalında veya sunucu yöneticileri yönetmek için SSMS sunucu özelliklerinde. 

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

