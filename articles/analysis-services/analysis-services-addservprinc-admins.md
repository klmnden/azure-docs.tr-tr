---
title: Azure Analysis Services sunucu Yönetici rolü için bir hizmet sorumlusu ekleme | Microsoft Docs
description: Sunucu Yönetici rolü için bir Otomasyon hizmet sorumlusu eklemeyi öğrenin
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8840dd9a8a8e8ba1f78d461824ac0cdce32b2ac9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596779"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Sunucu Yöneticisi rolünün bir hizmet sorumlusu ekleme 

 Katılımsız PowerShell görevleri otomatikleştirmek için bir hizmet sorumlusu olmalıdır **Sunucu Yöneticisi** yönetilen Analysis Services sunucu üzerinde ayrıcalıkları. Bu makalede, bir Azure AS sunucusunda sunucu yöneticileri rolüne bir hizmet sorumlusu eklemeyi açıklar.

## <a name="before-you-begin"></a>Başlamadan önce
Bu görevi tamamlamadan önce Azure Active Directory'de kayıtlı bir hizmet sorumlusu olması gerekir.

[Hizmet sorumlusu - Azure portal oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Hizmet sorumlusu oluşturma - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Gerekli izinler
Bu görevi tamamlamak için ihtiyacınız [Sunucu Yöneticisi](analysis-services-server-admins.md) Azure AS sunucu üzerindeki izinleri. 

## <a name="add-service-principal-to-server-administrators-role"></a>Sunucu yöneticileri rolüne hizmet sorumlusu ekler

1. SSMS, Azure AS server'ınıza bağlanın.
2. İçinde **sunucu özellikleri** > **güvenlik**, tıklatın **Ekle**.
3. İçinde **bir kullanıcı veya Grup Seç**, kayıtlı uygulamanızın adıyla seçin ve ardından arama **Ekle**.

    ![Hizmet sorumlusu hesabı arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Hizmet sorumlusu hesabı Kimliğini doğrulayın ve ardından **Tamam**.
    
    ![Hizmet sorumlusu hesabı arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> AzureRm cmdlet'lerini kullanarak sunucu işlemleri için hizmet asıl çalışan Zamanlayıcısı ayrıca ait olmalı **sahibi** kaynak için rol [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


