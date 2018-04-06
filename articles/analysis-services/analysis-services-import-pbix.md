---
title: Azure Analysis Services bir Power BI Desktop dosyası alma | Microsoft Docs
description: Azure portalını kullanarak Power BI Desktop dosyasının (pbıx) nasıl içe aktarılacağını açıklar.
services: analysis-services
documentationcenter: ''
author: minewiskan
manager: kfile
editor: ''
tags: ''
ms.assetid: ''
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 04/03/2018
ms.author: owend
ms.openlocfilehash: 2ba9bc0e4b9a55312875fe120ee179800aeefb23
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
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
