---
title: Azure Analysis Services'e Power BI Desktop dosyasını içeri aktarma | Microsoft Docs
description: Azure portalını kullanarak bir Power BI Desktop dosyası (pbix) içeri aktarma açıklar.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3dd90fc862e64812c0ba17bef74818d18788f4b5
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441000"
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop dosyasını içeri aktarın

Azure Analysis Services'e Power BI Desktop dosyası (pbıx) veri modelinde içeri aktarabilirsiniz. Model meta verileri, önbelleğe alınmış verileri ve veri kaynağı bağlantıları içeri aktarılır. Raporlar ve görselleştirmeler içeri aktarılmaz. Power BI Desktop'tan modelleri kullanarak 1400 uyumluluk düzeyinde veri alma işlemi.

**Kısıtlamaları**   
- Pbıx modelin bağlanıp **Azure SQL veritabanı** ve **Azure SQL veri ambarı** veri kaynakları yalnızca. 
- Pbıx modeli Canlı olamaz veya DirectQuery bağlantıları. 
- Pbıx veri modelinizi Analysis Services'de desteklenmeyen meta veri içeriyorsa, içeri aktarma işlemi başarısız.

## <a name="to-import-from-pbix"></a>Pbıx dosyasını içeri aktarmak için

1. Sunucunuzun içinde **genel bakış** > **Web Tasarımcısı**, tıklayın **açık**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. İçinde **Web Tasarımcısı** > **modelleri**, tıklayın **+ Ekle**.

    ![Azure portalında bir model oluşturma](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. İçinde **yeni modeli**, model adını yazın ve ardından Power BI Desktop dosyasını seçin.

    ![Azure portalında yeni model iletişim kutusu](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. İçinde **alma**bulup dosyanızı seçin.

     ![Azure portalında iletişim bağlanma](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Ayrıca bkz.

[Azure portalında bir model oluşturma](analysis-services-create-model-portal.md)   
[Azure Analysis Services'a bağlanın](analysis-services-connect.md)  
