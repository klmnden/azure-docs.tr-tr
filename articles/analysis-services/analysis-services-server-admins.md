---
title: Azure Analysis Services sunucu yöneticileri yönetme | Microsoft Docs
description: Sunucu yöneticileri için bir Azure Analysis Services sunucusu'nı yönetmeyi öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 25eb111871bbe3b18f59b0d6c123c72f3e55c859
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60498721"
---
# <a name="manage-server-administrators"></a>Sunucu yöneticilerini yönetme

Sunucu yöneticileri, sunucunun bulunduğu Kiracı için Azure Active Directory (Azure AD) geçerli bir kullanıcı veya güvenlik grubu olmalıdır. Kullanabileceğiniz **Analysis Services yöneticileri** Azure portalında sunucu özelliklerinde sunucu yöneticileri yönetmek için SSMS, PowerShell veya REST API sunucunuz için. 

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

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Kullanım [yeni AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/new-azanalysisservicesserver) cmdlet'i yeni bir sunucu oluştururken zaman yönetici parametresini belirtin. <br>
Kullanım [kümesi AzAnalysisServicesServer](https://docs.microsoft.com/powershell/module/az.analysisservices/set-azanalysisservicesserver) cmdlet'ini yönetici parametresi mevcut bir sunucu için değiştirin.

## <a name="rest-api"></a>REST API

Kullanım [Oluştur](https://docs.microsoft.com/rest/api/analysisservices/servers/create) asAdministrator özelliği yeni bir sunucu oluştururken zaman belirtmek için. <br>
Kullanım [güncelleştirme](https://docs.microsoft.com/rest/api/analysisservices/servers/update) asAdministrator özelliğini mevcut bir sunucu değiştirilirken belirtmek için. <br>



## <a name="next-steps"></a>Sonraki adımlar 

[Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md)  
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Rol Tabanlı Access Control](../role-based-access-control/overview.md)  

