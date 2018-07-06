---
title: Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs
description: Sunucu yöneticileri için bir Azure Analysis Services sunucusu'nı yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9d8f74bd66fc7c980c4fc5f83492aad7d8a4aa5c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866976"
---
# <a name="manage-server-administrators"></a>Sunucu yöneticilerini yönetme
Sunucu yöneticileri, sunucunun bulunduğu Kiracı için Azure Active Directory (Azure AD) geçerli bir kullanıcı veya güvenlik grubu olmalıdır. Kullanabileceğiniz **Analysis Services yöneticileri** sunucunuzun Azure portalında veya sunucu yöneticilerini yönetme ssms'de sunucu özellikleri. 

> [!NOTE]
> Güvenlik grupları olmalıdır `MailEnabled` özelliğini `True`.

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Sunucu yöneticileri, Azure portalını kullanarak eklemek için
1. Sunucunuz için portal'ı tıklatın **Analysis Services yöneticileri**.
2. İçinde  **\<servername >-Analysis Services yöneticileri**, tıklayın **Ekle**.
3. İçinde **sunucu yöneticileri ekleme**, kullanıcı hesapları Azure AD'nizi seçin veya dış kullanıcılar tarafından e-posta adresi davet edin.

    ![Azure portalında sunucu yöneticileri](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>SSMS kullanarak sunucu yöneticileri eklemek için
1. Sunucuya sağ tıklayın > **özellikleri**.
2. İçinde **Analiz sunucusu özellikleri**, tıklayın **güvenlik**.
3. Tıklayın **Ekle**ve ardından e-posta adresi, Azure AD'de bir kullanıcı veya grup için girin.
   
    ![SSMS'de Sunucu Yöneticisi ekleme](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Sonraki adımlar 
[Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md)  
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Rol Tabanlı Access Control](../role-based-access-control/overview.md)  

