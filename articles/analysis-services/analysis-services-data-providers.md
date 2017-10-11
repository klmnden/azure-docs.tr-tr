---
title: "Azure Analysis Services'a bağlanmak için gereken istemci kitaplığı | Microsoft Docs"
description: "Azure Analysis Services bağlanmak istemci uygulamaları ve araçları için gerekli istemci kitaplıkları açıklar"
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
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a96e7fe671dc051da35168fa7b49ecba53b4c4a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Azure Analysis Services'a bağlanmak için istemci kitaplıkları

İstemci kitaplıkları, Analysis Services sunucularına bağlanmak için istemci uygulamaları ve araçları için gereklidir. 

Analysis Services üç istemci kitaplıkları kullanın. ADOMD.NET ve Analysis Services yönetim nesneleri (AMO), yönetilen istemci kitaplıkları markalarıdır. Analysis Services OLE DB sağlayıcısı (MSOLAP DLL) yerel istemci Kitaplığı ' dir. Genellikle, tüm üç aynı anda yüklenir. Azure Analysis Services en son sürümlerini gerektirir. 

Microsoft Power BI Desktop ve Excel gibi istemci uygulamalarını, tüm üç istemci kitaplıkları yükleyin. Ancak, sürüm veya güncelleştirmelerinin sıklığını bağlı olarak, istemci kitaplıkları Azure Analysis Services tarafından gerekli en son sürümlerini olmayabilir. Aynı durum AsCmd, TOM, ADOMD.NET gibi özel uygulamalar ve diğer arabirimler için de geçerlidir. Bu uygulamalar, el ile kitaplıkları yükleme gerektirir. İstemci kitaplıkları el ile yükleme için SQL Server özellik paketlerinde dağıtılabilir paketleri dahil edilir. Ancak, bu istemci kitaplıkları, SQL Server sürümüne bağlıdır ve en son olmayabilir.  

İstemci bağlantıları için istemci kitaplıkları, Azure Analysis Services sunucusundan bir veri kaynağına bağlanmak için gereken veri sağlayıcıları farklıdır. Veri kaynağı bağlantıları hakkında daha fazla bilgi için bkz: [veri kaynağı bağlantıları](analysis-services-datasource.md).

## <a name="download-the-latest-client-libraries"></a>Son istemci kitaplıkları indirin  
Aşağıdaki istemci kitaplıklarından kullanın bir üretim ortamında olan ve tam olarak yayımlanan ve desteklenen sürümlerini gerektirir.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Sonraki adımlar
[Excel ile bağlanma](analysis-services-connect-excel.md)    
[Power BI ile bağlanma](analysis-services-connect-pbi.md)
