---
title: "Stream Analytics penceresi işlevleri giriş | Microsoft Docs"
description: "Akış (dönen atlamalı, kayan) analizi üç pencere işlevleri hakkında bilgi edinin."
keywords: "dönen pencere, kayan pencere atlamalı pencere"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 2a6559551f608cf435e89997392a6a0ba995c583
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-stream-analytics-window-functions"></a>Stream Analytics penceresi işlevleri giriş
Pek çok gerçek senaryolar akışı sırasında yalnızca zamana bağlı Windows'da bulunan veriler üzerinde işlem gerçekleştirmek gereklidir. Pencereleme işlevler için yerel destek karmaşık akış işleme işleri yazma, geliştirici üretkenliğine iğnenin taşır Azure akış analizi, anahtar bir özelliğidir. Akış analizi sağlar geliştiriciler [ **dönen**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) ve [ **hareketli** ](https://msdn.microsoft.com/library/dn835051.aspx) veri akışı üzerinde zamana bağlı işlemleri gerçekleştirmek için windows. Değer belirtmeye olan tüm [penceresi](https://msdn.microsoft.com/library/dn835019.aspx) operations çıktı sonuçlarına **son** penceresinin. Çıktı penceresi tek olay kullanılan üzerinde toplama işlevi tabanlı olacaktır. Olay penceresinin bitiş zaman damgası sahip olur ve tüm pencere işlevleri sabit uzunluk ile tanımlanır. Son olarak, tüm pencere işlevleri de kullanılması gerektiğini dikkate almak önemlidir bir [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) yan tümcesi.

![Stream Analytics penceresi işlevleri kavramları](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Atlayan pencere
Dönen pencere işlevleri ayrı zaman kesimler halinde veri akışı ayırabilir ve bunları karşı işlevi aşağıdaki örnek gibi gerçekleştirmek için kullanılır. Atlayan pencere, anahtar differentiators, bunlar yinelemeniz, çakışmaması ve bir olay için birden fazla atlayan pencere ait olamaz ' dir.

![Giriş dönen stream Analytics penceresi işlevleri](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Atlamalı pencere
Atlamalı pencere işlevleri atlama İleri zamanına göre sabit bir süre içinde. Olayları birden fazla Hopping penceresi sonuç kümesine ait olabilir. böylece bunları binebilir, dönen windows düşünmek kolay olabilir. Bir Hopping penceresi bir dönen aynı yapmak için bir pencere yalnızca pencere boyutu ile aynı olacak şekilde atlama boyutu belirtmeniz gerekir. 

![Stream Analytics penceresi atlamalı giriş işlevleri](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Kayan pencere
Dönen veya atlamalı windows, farklı olarak, kayan pencere işlevleri bir çıktı üretir **yalnızca** olay oluştuğunda. Her penceresinde en az bir olay sahip olur ve pencere sürekli bir € (epsilon) ileri taşır. Atlamalı Windows gibi olaylar için birden fazla kayan pencere ait olabilir.

![Stream Analytics penceresi kayan giriş işlevleri](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Pencere işlevleri ile ilgili Yardım alma
Daha fazla yardım için [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

