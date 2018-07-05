---
title: Azure Analysis Services Web Tasarımcısı kullanarak bir tablosal model oluşturma | Microsoft Docs
description: Azure portalında Web Tasarımcısı kullanarak bir Azure Analysis Services tablosal model oluşturmayı öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 17ff6ebed615971b4157831431d9e2395ca68b48
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441684"
---
# <a name="create-a-model-in-azure-portal"></a>Azure portalında bir model oluşturma

Azure portalında Azure Analysis Services web Tasarımcısı (Önizleme) özelliği ve tablosal modeller düzenleme ve model veri sağ tarayıcınızda sorgu oluşturmanın hızlı ve kolay bir yolunu sağlar. 

Web Tasarımcısı göz önünde bulundurun, **Önizleme**. İşlevselliği sınırlıdır. Daha gelişmiş model geliştirme ve test için Visual Studio (SSDT) ve SQL Server Management Studio (SSMS) en iyisidir.

## <a name="before-you-begin"></a>Başlamadan önce

- Standart veya Geliştirici katmanı, bir Azure Analysis Services sunucusu. Web Tasarımcısı kullanılarak oluşturulan yeni DirectQuery, yalnızca bu katmanları tarafından desteklenen modelleridir.
- Bir Azure SQL veritabanı, Azure SQL veri ambarı veya bir veri kaynağı olarak Power BI Desktop (.pbix) dosyası. Power BI Desktop dosyaları desteği Azure SQL veritabanı ve Azure SQL veri ambarı ' oluşturulan yeni modeller.
- Bir SQL Server hesabı ve Azure SQL veritabanı veya Azure SQL veri ambarı veri kaynaklarına bağlanmak için parola.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="to-create-a-new-tabular-model"></a>Yeni bir tablosal model oluşturmak için

1. Sunucunuzdaki **genel bakış** > **Web Tasarımcısı**, tıklayın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklayın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni modeli**, model adını yazın ve ardından veri kaynağını seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. İçinde **Connect**, bağlantı özelliklerini girin. Kullanıcı adı ve parola, bir SQL Server hesabı olması gerekir.

     ![Azure portalında iletişim bağlanma](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. İçinde **tabloları ve görünümleri**, modelinize eklenmesi ve ardından istediğiniz tabloları seçin **Oluştur**. İlişkiler bir anahtar çifti ile tablolar arasında otomatik olarak oluşturulur.

     ![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Yeni modelinizi tarayıcınızda görünür. Buradan şunları yapabilirsiniz:   

- Model verileri alanlar sorgu tasarımcısına sürükleyerek ve filtreler ekleyerek sorgulayın.
- Tablolar yeni ölçümler oluşturabilirsiniz.
- Json düzenleyicisini kullanarak model meta verilerini düzenleyin.
- Model, Visual Studio (SSDT), Power BI Desktop veya Excel'de açın.

![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Model meta verilerini düzenleme veya tarayıcınızda yeni ölçümler oluşturabilirsiniz, bu değişiklikleri Azure modelinizde kaydettiğiniz. SSDT, Power BI Desktop veya Excel modelinizde üzerinde çalışıyoruz, modelinizi eşitlenmemiş alabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar 
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Excel ile bağlanma](analysis-services-connect-excel.md)  


