---
title: "Power BI ile Azure Analysis Services'a bağlanmak | Microsoft Docs"
description: "Power BI kullanarak bir Azure Analysis Services sunucusuna bağlanmak öğrenin."
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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 5f3ad1c4df2c21a8d52a8a3c4d7d311af32720a9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="connect-with-power-bi"></a>Power BI ile bağlanma

Azure üzerinde bir sunucu oluşturulur ve bir tablo modeline dağıtılmış sonra kuruluşunuzdaki kullanıcılar bağlanmak ve veri araştırmaya başlamak hazırsınız. 

> [!TIP]
> En son sürümünü kullandığınızdan emin olun [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Power BI Desktop'ta Bağlan

1. Power BI Desktop'ta tıklatın **Veri Al** > **Azure** > **Azure Analysis Services veritabanı**.

2. İçinde **Server**, sunucu adını girin. 
    
    Tam URL eklediğinizden emin olun. Örneğin, asazure://westcentralus.asazure.windows.net/advworks.

3. İçinde **veritabanı**, burada yapıştırmak için bağlanmak istediğiniz Perspektif veya tablolu model veritabanı adını biliyorsanız. Aksi takdirde, bu alanı boş bırakın ve daha sonra bir veritabanı veya perspektif seçin.

4. Varsayılan adı bırakın **Bağlan canlı** seçeneğini ve ardından basın **Bağlan**. 

5. İstenirse, oturum açma kimlik bilgilerinizi girin. 

6. İçinde **Gezgini**, sunucuyu genişletin ve ardından model veya perspektif ardından bağlanmak istediğiniz **Bağlan**. Bir model veya perspektif bu görünümün tüm nesneleri görüntülemek için tıklatın.

    Model Power BI Desktop'ta rapor görünümü boş bir rapor ile açılır. Tüm gizli olmayan model nesneleri alanlar listesini görüntüler. Bağlantı durumu sağ alt köşesinde görüntülenir.

## <a name="connect-in-power-bi-service"></a>Power BI (hizmeti) bağlanma

1. Sunucunuzda modelinizi canlı bir bağlantısı olan bir Power BI Desktop dosyası oluşturun.
2. İçinde [Power BI](https://powerbi.microsoft.com), tıklatın **Veri Al** > **dosyaları**. Dosyasını bulup seçin.



## <a name="see-also"></a>Ayrıca bkz.
[Azure Analysis Services'a bağlanın](analysis-services-connect.md)   
[İstemci kitaplıkları](analysis-services-data-providers.md)

