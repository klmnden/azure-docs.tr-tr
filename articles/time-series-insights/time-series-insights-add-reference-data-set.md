---
title: "Azure Zaman Serisi Görüşleri ortamınıza başvuru veri kümesi ekleme | Microsoft Docs"
description: "Bu öğreticide, Zaman Serisi Görüşleri ortamınıza başvuru veri kümesi ekleyeceksiniz"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017

---

# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Ibiza portalını kullanarak Zaman Serisi Görüşleri ortamınız için bir başvuru veri kümesi oluşturma

Başvuru Veri Kümesi, olay kaynağınızdan alınan olaylarla genişletilen bir öğe koleksiyonudur. Zaman Serisi Görüşleri giriş altyapısı, olay kaynağınızdaki bir olay ile başvuru veri kümenizdeki bir öğeyi birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleştirme işlemi için başvuru veri kümenizde tanımlı anahtarlar temel alınır.

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a>Ortamınıza başvuru veri kümesi ekleme adımları

1. [Ibiza portalında](https://portal.azure.com) oturum açın.
2. Ibiza portalının sol tarafındaki menüde bulunan "Tüm kaynaklar" seçeneğine tıklayın.
3. Zaman Serisi Görüşleri ortamınızı seçin.

    ![Zaman Serisi Görüşleri başvuru veri kümenizi oluşturma](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. "Başvuru Veri Kümeleri" seçeneğini belirleyin ve "+ Ekle" öğesine tıklayın.

    ![Zaman Serisi Görüşleri başvuru veri kümesi oluşturma - ayrıntılar](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. Başvuru veri kümesinin adını belirtin.
6. Anahtar adını ve türünü belirtin. Bu ad ve tür, olay kaynağınızdaki olaydan doğru özelliği seçmek için kullanılır. Örneğin, adı "DeviceId" ve türü "Dize" olarak belirlerseniz Zaman Serisi Görüşleri giriş altyapısı, gelen olayda "Dize" türüne sahip "DeviceId" adlı bir özellik arar. Olay ile birleştirilecek birden fazla anahtar sağlayabilirsiniz. Özellik adı eşleştirmesi büyük/küçük harfe duyarlıdır.

     ![Zaman Serisi Görüşleri başvuru veri kümesi oluşturma - ayrıntılar](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. “Oluştur” düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Başvuru verilerini programlamayla yönetin](time-series-insights-manage-reference-data-csharp.md).
* Tam API başvurusu için [Başvuru Verileri API'si](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) belgesine bakın.
