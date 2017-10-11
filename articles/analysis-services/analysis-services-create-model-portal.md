---
title: "Azure Analysis Services Web Tasarımcısı kullanarak tablolu model oluşturma | Microsoft Docs"
description: "Azure portalında Web Tasarımcısını kullanarak bir Azure Analysis Services tablolu model oluşturmayı öğrenin."
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
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: bd58f1845dabf6afb47ce27236d14479677a8808
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-model-in-azure-portal"></a>Azure portalında bir model oluşturma

Azure portalında Azure Analysis Services web Tasarımcısı (Önizleme) özelliği oluşturmak ve tablolu modeller düzenleyin ve model veri sağ tarayıcınızda sorgulamak için hızlı ve kolay bir yol sağlar. 

Unutmayın, web Designer **Önizleme**. Her zaman Önizleme, eklenmiş yeni işlevi sırada işlevselliği sınırlıdır. Daha gelişmiş modeli geliştirme ve test için Visual Studio (SSDT) ve SQL Server Management Studio (SSMS) kullanmak en iyisidir.

## <a name="prerequisites"></a>Ön koşullar

- Bir standart veya Geliştirici katmanı Azure Analysis Services sunucusunda. Web Tasarımcısı'nı kullanarak oluşturduğunuz yeni modelleri DirectQuery, bu Katmanlar tarafından desteklenen ' dir.
- Bir Azure SQL Database, Azure SQL Data Warehouse veya Power BI Desktop (.pbix) dosyası olarak bir veri kaynağı. Yeni modelleri veri kaynaklarını Power BI Desktop dosyaları destek Azure SQL Database, Azure SQL Data Warehouse, Oracle ve Teradata oluşturulur.
- Bir SQL Server hesabı ve Azure SQL Database veya Azure SQL veri ambarı veri kaynaklarına bağlanmak için parola.

## <a name="to-create-a-new-tabular-model"></a>Yeni bir tablolu model oluşturmak için

1. Sunucunuzun içinde **genel bakış** dikey > **Web Tasarımcısı**, tıklatın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklatın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni model**, model adını yazın ve ardından bir veri kaynağı seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. İçinde **Bağlan**, bağlantı özelliklerini girin. Kullanıcı adı ve parola bir SQL Server hesabı olması gerekir.

     ![Azure portalında iletişim Bağlan](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. İçinde **tabloları ve görünümleri**, modelinize eklemek ve ardından istediğiniz tabloları seçin **oluşturma**. Bir anahtar çifti ile tablolar arasında ilişkiler otomatik olarak oluşturulur.

     ![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Yeni modelinizi tarayıcınızda görüntülenir. Buradan, şunları yapabilirsiniz:   

- Model verileri sorgu alanları Sorgu Tasarımcısı sürüklemek ve filtreleri ekleme.
- Yeni ölçüleri tablolarda oluşturun.
- Json Düzenleyicisi'ni kullanarak model meta verilerini düzenleyin.
- Model, Visual Studio (SSDT), Power BI Desktop veya Excel'de açın.

![Tabloları ve görünümleri seçin](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Model meta verilerini düzenleyin veya yeni ölçüleri tarayıcınızda oluşturduğunuzda, bu değişiklikleri modelinizi Azure kaydetmeyi. Ayrıca SSDT, Power BI Desktop veya Excel modelinizde üzerinde çalışıyorsanız, modelinizi eşitlenmemiş elde edebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar 
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Excel ile bağlanma](analysis-services-connect-excel.md)  


