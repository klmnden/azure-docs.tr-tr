---
title: Azure Analysis Services bir Power BI Desktop dosyası alma | Microsoft Docs
description: Azure portalını kullanarak Power BI Desktop dosyasının (pbıx) nasıl içe aktarılacağını açıklar.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 35bf2ba85017de43788f802b6244d61ed2bb62df
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop dosyasını içeri aktarma

Power BI Desktop dosyası (pbıx) dosyasını içeri aktararak, yeni bir model Azure AS oluşturabilirsiniz. Model meta verileri, önbelleğe alınan veriler ve veri kaynağı bağlantıları alınır. Raporlar ve görselleştirmeleri içeri aktarılmadı.

**Kısıtlamaları**   
- Pbıx modeli Azure SQL Database ve Azure SQL Data Warehouse veri kaynakları için yalnızca bağlanabilir. 
- Pbıx model Canlı sahip olamaz veya DirectQuery bağlantıları. 
- Pbıx veri modelinizi Analysis Services içinde desteklenmeyen meta veri içeriyorsa alma işlemi başarısız.

## <a name="to-import-from-pbix"></a>Pbıx içeri aktarmak için

1. Sunucunuzun içinde **genel bakış** > **Web Tasarımcısı**, tıklatın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklatın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni model**bir model adı yazın ve ardından Power BI Desktop dosyasını seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. İçinde **alma**bulup dosyanızı seçin.

     ![Azure portalında iletişim Bağlan](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Ayrıca bkz.

[Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)   
[Azure Analysis Services'a bağlanın](analysis-services-connect.md)  
