---
title: Azure Analysis Services sunucu Yönetici rolü için bir hizmet ilkesi ekleme | Microsoft Docs
description: Sunucu Yönetici rolü için bir Otomasyon hizmeti ilkesine eklemeyi öğrenin
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 9c6417e069bbed38b1f6e9317636a10834ce7197
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="add-a-service-principle-to-the-server-administrator-role"></a>Sunucu Yöneticisi rolünün bir hizmet İlkesi Ekle 

 Katılımsız PowerShell görevleri otomatikleştirmek için bir hizmet ilkesi olmalıdır **Sunucu Yöneticisi** yönetilen Analysis Services sunucu üzerinde ayrıcalıkları. Bu makalede, bir Azure AS sunucusunda sunucu yöneticileri rolüne hizmet ilkesi eklemeyi açıklar.

## <a name="before-you-begin"></a>Başlamadan önce
Bu görevi tamamlamadan önce Azure Active Directory'de kayıtlı bir hizmet ilkesi olması gerekir.

[Hizmet İlkesi - Azure portal oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Hizmet ilkesi oluşturma - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="required-permissions"></a>Gerekli izinler
Bu görevi tamamlamak için ihtiyacınız [Sunucu Yöneticisi](analysis-services-server-admins.md) Azure AS sunucu üzerindeki izinleri. 

## <a name="add-service-principle-to-server-administrators-role"></a>Sunucu yöneticileri rolüne hizmet İlkesi Ekle

1. SSMS, Azure AS server'ınıza bağlanın.
2. İçinde **sunucu özellikleri** > **güvenlik**, tıklatın **Ekle**.
3. İçinde **bir kullanıcı veya Grup Seç**, kayıtlı uygulamanızın adıyla seçin ve ardından arama **Ekle**.

    ![Hizmet İlkesi hesabını arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Hizmet İlkesi hesap Kimliğini doğrulayın ve ardından **Tamam**.
    
    ![Hizmet İlkesi hesabını arayın](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)


> [!NOTE]
> AzureRm cmdlet'lerini kullanarak sunucu işlemleri için Zamanlayıcı'yı çalıştıran hizmet ilkesi de ait olmalı **sahibi** kaynak için rol [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md). 

## <a name="related-information"></a>İlgili bilgiler

* [SQL Server PowerShell modülünü indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [SSMS indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


