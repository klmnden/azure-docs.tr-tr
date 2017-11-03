---
title: "Azure CDN kullanım desenlerini çözümleme | Microsoft Docs"
description: "Müşteri, Günlük çözümlemesi için Azure CDN etkinleştirebilirsiniz."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: 04e5499011e72dfcc20dff370d5d837227ed29b6
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Azure CDN kullanım desenlerini Çözümle

Uygulamanız için CDN etkinleştirdikten sonra CDN kullanımını izlemek, teslimat durumunu denetleyin ve olası sorunları gidermek. Azure CDN, bu özellikler aşağıdaki sağlayan iki yöntem sunar: 

## <a name="verizon-core-reports"></a>Verizon çekirdek raporları

Verizon standard veya Verizon premium profil ile bir Azure CDN kullanıcı olarak Verizon ek portalda Verizon çekirdek raporları görüntüleyebilir. Verizon çekirdek raporları aracılığıyla erişilebilir durumda **Yönet** Azure portalından seçeneği ve çeşitli grafikleri ve görünümler sunar. Daha fazla bilgi için bkz: [çekirdek raporları verizon'dan](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Verizon özel raporlar

Verizon standard veya Verizon premium profil ile bir Azure CDN kullanıcı olarak Verizon ek portalda Verizon özel raporları görüntüleyebilir. Verizon özel raporlar yoluyla erişilebilir **Yönet** Azure portalından seçeneği. Her biri için aktarılan isabet veya veri sayısı Verizon özel raporlar sayfa gösterir bir Azure CDN profili ait CName kenar. Verileri bir süre boyunca HTTP yanıtı kodu veya önbellek duruma göre gruplandırılabilir. Daha fazla bilgi için bkz: [özel raporlar verizon'dan](cdn-verizon-custom-reports.md).

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Temel analiz aracılığıyla Azure tanılama günlükleri

Temel analiz Verizon (standart ve Premium) ve Akamai (standart) CDN profilleri ait tüm CDN uç noktası için kullanılabilir. Azure tanılama günlükleri, Azure depolama, olay hub'ları ya da Operations Management Suite (OMS) günlük analizi için verilecek temel analiz izin verir. OMS günlük analizi kullanıcı tarafından yapılandırılabilir ve özelleştirilebilir grafikleri ile bir çözüm sunar. Daha fazla bilgi için bkz: [Azure tanılama günlükleri](cdn-azure-diagnostic-logs.md).

