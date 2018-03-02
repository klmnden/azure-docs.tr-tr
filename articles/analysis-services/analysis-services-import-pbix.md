---
title: "Azure Analysis Services bir Power BI Desktop dosyası alma | Microsoft Docs"
description: "Azure portalını kullanarak Power BI Desktop dosyasının (pbıx) nasıl içe aktarılacağını açıklar."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: 43eab587a1e5209069a248f1e2e1f57af158a2b8
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Power BI Desktop dosyasını içeri aktarma

Power BI Desktop dosyası (pbıx) dosyasını içeri aktararak, yeni bir model Azure AS oluşturabilirsiniz. Model meta verileri, önbelleğe alınan veriler ve veri kaynağı bağlantıları alınır. Raporlar ve görselleştirmeleri içeri aktarılmadı. Bir kez sunucunuzda, model değişiklikleri güncelleştirme ve pbıx portalında web Tasarımcısı (Önizleme) özelliğini kullanarak veya SQL Server Management Studio (SSMS) kullanarak yeniden içe aktarılması tarafından yapılabilir. İçeri aktarılan modelleri açılamıyor veya Visual Studio dışarı.

> [!NOTE]
> Şirket içi veri kaynaklarına pbıx modelinizi bağlanırsa, bir [şirket içi ağ geçidi](analysis-services-gateway.md) sunucunuz için yapılandırılması gerekir.

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
