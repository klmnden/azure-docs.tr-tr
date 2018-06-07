---
title: Azure portalını kullanarak Azure Data Lake Analytics işlerini izleme
description: Bu makalede, Azure Data Lake Analytics işlerini gidermek için Azure Portalı'nı kullanmayı açıklar.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 8c61b8736dfb13f0c2c2520f22979ac51646e05f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623664"
---
# <a name="monitor-jobs-in-azure-data-lake-analytics-using-the-azure-portal"></a>Azure Portal kullanarak Azure Data Lake Analytics işlerini izleme

**Tüm işleri görmek için**

1. Azure portalından tıklatın **Microsoft Azure** sol üst köşedeki.
2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  İş özeti gösterilir **iş yönetimi** döşeme.

    ![Azure Data Lake Analytics iş yönetimi](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Proje yönetimi, iş durumunu bir bakışta sağlar. Başarısız bir işi olduğuna dikkat edin.
3. Tıklatın **iş yönetimi** işleri görmek için döşeme. İşlerini kategorilere ayrılır **çalıştıran**, **sıraya alınan**, ve **sona erdi**. Başarısız işinizde göreceksiniz **sona erdi** bölümü. Listedeki ilk bir olacaktır. İşlerini çok sahip olduğunuzda, tıklayabilirsiniz **filtre** işleri bulmanıza yardımcı olacak.

    ![Azure Data Lake Analytics işleri filtreleyin](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. İş ayrıntılarını açmak için listeden başarısız işi tıklayın:

    ![Azure Data Lake Analytics işi başarısız oldu](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Bildirim **yeniden gönderin** düğmesi. Sorunu düzelttikten sonra işi yeniden gönderebilirsiniz.
5. Hata ayrıntılarını açmak için önceki ekran görüntüsünde vurgulanan bölümünden'ı tıklatın.  Benzer bir şey göreceksiniz:

    ![Azure Data Lake Analytics işi ayrıntıları ile başarısız oldu.](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Kaynak klasörü bulunamadı söyler.
6. Tıklatın **yinelenen komut dosyası**.
7. Güncelleştirme **FROM** yolu:

    "/ Samples/Data/SearchLog.tsv"
8. **İşi Gönder**'e tıklayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Azure PowerShell kullanarak Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Data Lake Analytics'i Azure portalını kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
