---
title: Azure Stack izleme verilerini kullanma | Microsoft Docs
description: Azure Stack izleme verilerini kullanan seçenekler hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2018
ms.author: mabrigg
ms.openlocfilehash: fbd4552a9e40f16a6fedec4e04be0d7d6d39351d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724494"
---
# <a name="how-to-consume-monitoring-data-from-azure-stack"></a>Azure Stack izleme verilerini kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri*

Azure İzleyici'de genel Azure izleme verilerini Azure İzleyici işlem hattının bulunduğu, yalnızca tek bir yerde ister bulabilirsiniz. Ancak, tüm genel Azure izleme verileri Azure Stack'te kullanılabilir. Bu makalede, hizmet izleme verilerini programlama yoluyla alabilen çeşitli şekillerde özetini bulabilirsiniz.
 
## <a name="options-for-data-consumption"></a>Veri tüketimi seçeneklerini

| Veri türü | Kategori | Desteklenen hizmetler | Erişim yöntemi |
|-------------------------------------------------------------|----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Azure platform düzeyi ölçümlerini izleme | Ölçümler | [Azure Stack'te Azure İzleyici ile desteklenen ölçümler](azure-stack-metrics-supported.md) | REST API |
| Konuk işletim sistemi ölçümleri (örneğin, Perf sayısı) işlem | Ölçümler | Windows ve Linux sanal makineleri | Depolama tablo veya blob:<br>Windows veya Linux Azure tanılama <br>Olay hub'ı:<br>Windows Azure Tanılama |
| Depolama ölçümleri | Ölçümler | Azure Storage | Depolama tablosu:<br>Depolama Analizi |
| Etkinlik günlüğü | Olaylar | Tüm Azure Hizmetleri | REST API:<br>Azure izleyici olay API'si |
| Konuk işletim sistemi günlükleri (örneğin, IIS, ETW, Syslog) işlem | Olaylar | Windows ve Linux sanal makineleri | Depolama tablo veya blob:<br>Windows veya Linux Azure tanılama <br>Olay hub'ı:<br>Windows Azure Tanılama |
| Depolama günlükleri | Olaylar | Azure Storage | Depolama tablosu:<br>Depolama Analizi |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Stack'te Azure izleme](azure-stack-metrics-azure-data.md).
