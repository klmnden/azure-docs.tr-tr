---
title: "Azure zaman serisi Öngörüler ortamınız için bir başvuru veri kümesi ekleme | Microsoft Docs"
description: "Bu makalede, Azure zaman serisi Öngörüler ortamınız için bir başvuru veri kümesi eklemeyi açıklar. Başvuru verileri değerleri genişletmek için kaynak verilerinizi katılmak kullanışlıdır."
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 7cdcefbd0daec3b7bab59680afa1470624583c74
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure Portalı'nı kullanarak, zaman serisinin Öngörüler ortamınız için bir başvuru veri kümesi oluşturma

Bu makalede, Azure zaman serisi Öngörüler ortamınız için bir başvuru veri kümesi eklemeyi açıklar. Başvuru verileri değerleri genişletmek için kaynak verilerinizi katılmak kullanışlıdır.

Başvuru Veri Kümesi, olay kaynağınızdan alınan olaylarla genişletilen bir öğe koleksiyonudur. Zaman Serisi Görüşleri giriş altyapısı, olay kaynağınızdaki bir olay ile başvuru veri kümenizdeki bir öğeyi birleştirir. Bu genişletilmiş olay daha sonra sorgu için kullanılabilir. Bu birleştirme işlemi için başvuru veri kümenizde tanımlı anahtarlar temel alınır.

## <a name="add-a-reference-data-set"></a>Bir başvuru veri kümesi Ekle

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut zaman serisi Öngörüler ortamınızın bulun. Tıklatın **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ortamı topolojisi** başlığını seçin **başvuru veri kümelerini**.

    ![Zaman Serisi Görüşleri başvuru veri kümenizi oluşturma](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Seçin **+ Ekle** yeni başvuru veri kümesi eklemek için.

5. Benzersiz bir belirtin **başvuru veri kümesi adı**.

    ![Zaman Serisi Görüşleri başvuru veri kümesi oluşturma - ayrıntılar](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

6. Belirtin **anahtar adı** boş alanı seçip kendi **türü**. Bu ada ve türe başvuru verileri katılmak için olay kaynağınızda olayları doğru özelliğinden seçmek için kullanılır. 

   Anahtar adı olarak sağlarsanız, örneğin, **DeviceID** ve olarak yazın **dize**, adında bir özellik için zaman serisi Öngörüler giriş altyapısı arar sonra **DeviceID** türü **Dize** aramak için her gelen olay ayarlama ve katılma. Olay ile birleştirilecek birden fazla anahtar sağlayabilirsiniz. Anahtar adı eşleştirmesi büyük/küçük harfe duyarlıdır.

7. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar
* [Başvuru verilerini programlamayla yönetin](time-series-insights-manage-reference-data-csharp.md).
* Tam API başvurusu için [Başvuru Verileri API'si](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) belgesine bakın.
