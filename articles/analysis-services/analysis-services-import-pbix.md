---
title: Azure Analysis Services bir Power BI Desktop dosyası alma | Microsoft Docs
description: Azure portalını kullanarak Power BI Desktop dosyasının (pbıx) nasıl içe aktarılacağını açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: aea6f3efcf3740527c43b75a30caadf6b2a8b623
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601080"
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop dosyasını içeri aktarma

Power BI Desktop dosyası (pbıx) veri modelinde Azure Analysis Services aktarabilirsiniz. Model meta verileri, önbelleğe alınan veriler ve veri kaynağı bağlantıları alınır. Raporlar ve görselleştirmeleri içeri aktarılmadı. Power BI Desktop modellerinden 1400 uyumluluk düzeyindedir verileri içeri aktardı.

**Kısıtlamaları**   
- Pbıx modelin bağlanıp **Azure SQL veritabanı** ve **Azure SQL Data Warehouse** veri kaynakları yalnızca. 
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
