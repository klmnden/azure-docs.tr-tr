---
title: Azure Portal kullanarak Azure Data Lake Analytics işleri izleme | Microsoft Docs
description: "Data Lake Analytics işlerini gidermek için Azure Portalı'nı kullanmayı öğrenin. "
services: data-lake-analytics
documentationcenter: ''
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: b7066d81-3142-474f-8a34-32b0b39656dc
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: 14b1f4ec9dff78e4b5d2480755a4b1f2579ec135
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
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
