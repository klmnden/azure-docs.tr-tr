---
title: Azure portalını kullanarak Azure Data Lake Analytics işleri izleme
description: Bu makalede, Azure Data Lake Analytics işlerinde sorun gidermek için Azure portalını kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 40864bab068659be016161f7dc40243ebbd45174
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60812612"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Azure portalı kullanarak Azure Data Lake Analytics işleri izleme

**Tüm işleri görmek için**

1. Azure portalından tıklayın **Microsoft Azure** sol üst köşedeki.
2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  İş özetinde gösterilir **iş yönetimi** Döşe.

    ![Azure Data Lake Analytics iş yönetimi](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    ' % S'proje yönetimi, iş durumunu bir bakışta sunar. Başarısız bir işi olduğuna dikkat edin.
3. Tıklayın **iş yönetimi** işleri görmek için kutucuğu. İşleri kategorize edilir **çalıştıran**, **sıraya alınan**, ve **Bitti**. İçinde başarısız işi göreceksiniz **Bitti** bölümü. Bu listedeki ilk öğe olmalıdır. İşleri çok fazla olduğunda tıklayabilirsiniz **filtre** işleri bulmanıza yardımcı olacak.

    ![Azure Data Lake Analytics işleri Filtrele](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. İş ayrıntılarını açmak için listeden başarısız işi tıklayın:

    ![Azure Data Lake Analytics işi başarısız oldu](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Bildirim **yeniden** düğmesi. Sorunu düzelttikten sonra işi yeniden gönderebilirsiniz.
5. Hata ayrıntılarını açmak için önceki ekran görüntüsünde vurgulanan bölümünden tıklayın.  Benzer bir şey görmeniz gerekir:

    ![Azure Data Lake Analytics işi ayrıntıları ile başarısız oldu.](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Kaynak klasörü bulunamadı söyler.
6. Tıklayın **yinelenen betik**.
7. Güncelleştirme **FROM** yolu:

    "/Samples/Data/SearchLog.tsv"
8. **İşi Gönder**'e tıklayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Azure PowerShell kullanarak Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
