---
title: "Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs"
description: "Azure Analysis Services sunucusu için sunucu yöneticileri yönetmeyi öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: d90f1e3df8f5934d5c334ec72b5726f105842ca1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
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
[Rol Tabanlı Access Control](../active-directory/role-based-access-control-what-is.md)  

