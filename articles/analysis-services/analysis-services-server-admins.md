---
title: "Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs"
description: "Azure Analysis Services sunucusu için sunucu yöneticileri yönetmeyi öğrenin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="manage-server-administrators"></a>Sunucu yöneticileri yönetin
Sunucu yöneticileri, sunucunun bulunduğu Kiracı için geçerli bir kullanıcı veya grup Azure Active Directory'de (Azure AD) olması gerekir. Kullanabileceğiniz **Analysis Services Admins** denetim dikey penceresinde sunucunuz Azure portalında veya sunucu yöneticileri yönetmek için SSMS sunucu özellikleri için. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Azure portal kullanarak sunucu yöneticileri eklemek için
1. Sunucunuz için Denetim dikey penceresinde tıklayın **Analysis Services Admins**.
2. İçinde  **\<sunucuadı >-Analysis Services Admins** dikey penceresinde tıklatın **Ekle**.
3. İçinde **sunucu yöneticileri ekleme** dikey penceresinde kullanıcı hesapları Azure AD'nizi seçin veya e-posta adresiyle dış kullanıcıları davet.

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

