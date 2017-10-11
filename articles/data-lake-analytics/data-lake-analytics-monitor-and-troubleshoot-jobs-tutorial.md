---
title: "Azure Portal kullanarak Azure Data Lake Analytics işlerini sorunlarını giderme | Microsoft Docs"
description: "Data Lake Analytics işlerini gidermek için Azure Portalı'nı kullanmayı öğrenin. "
services: data-lake-analytics
documentationcenter: 
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
ms.author: edmaca
ms.openlocfilehash: b9c7453cc0a94f70d0098ed83e5f127832065a62
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-data-lake-analytics-jobs-using-azure-portal"></a>Azure Portal kullanarak Azure Data Lake Analytics işlerini sorun giderme
Data Lake Analytics işlerini gidermek için Azure Portalı'nı kullanmayı öğrenin.

Bu öğreticide, bir eksik kaynak dosyası sorunu Kurulum ve sorun giderme için Azure Portalı'nı kullanın.

## <a name="submit-a-data-lake-analytics-job"></a>Data Lake Analytics işi gönderme

Aşağıdaki U-SQL işi gönder:

```
@searchlog =
   EXTRACT UserId          int,
           Start           DateTime,
           Region          string,
           Query           string,
           Duration        int?,
           Urls            string,
           ClickedUrls     string
   FROM "/Samples/Data/SearchLog.tsv1"
   USING Extractors.Tsv();

OUTPUT @searchlog   
   TO "/output/SearchLog-from-adls.csv"
   USING Outputters.Csv();
```
    
Komut dosyasında tanımlı kaynak dosyası **/Samples/Data/SearchLog.tsv1**, burada olmalıdır **/Samples/Data/SearchLog.tsv**.


## <a name="troubleshoot-the-job"></a>İş sorunlarını giderme

**Tüm işleri görmek için**

1. Azure portalından tıklatın **Microsoft Azure** sol üst köşedeki.
2. Data Lake Analytics hesap adınızı içeren kutucuğa tıklayın.  İş özeti gösterilir **iş yönetimi** döşeme.

    ![Azure Data Lake Analytics iş yönetimi](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)

    Proje yönetimi, iş durumunu bir bakışta sağlar. Başarısız bir işi olduğuna dikkat edin.
3. Tıklatın **iş yönetimi** işleri görmek için döşeme. İşlerini kategorilere ayrılır **çalıştıran**, **sıraya alınan**, ve **sona erdi**. Başarısız işinizde göreceksiniz **sona erdi** bölümü. Listedeki ilk bir olacaktır. İşlerini çok sahip olduğunuzda, tıklayabilirsiniz **filtre** işleri bulmanıza yardımcı olacak.

    ![Azure Data Lake Analytics işleri filtreleyin](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)
4. Yeni bir dikey pencerede iş ayrıntılarını açmak için listeden başarısız işi tıklayın:

    ![Azure Data Lake Analytics işi başarısız oldu](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)

    Bildirim **yeniden gönderin** düğmesi. Sorunu düzelttikten sonra işi yeniden gönderebilirsiniz.
5. Hata ayrıntılarını açmak için önceki ekran görüntüsünde vurgulanan bölümünden'ı tıklatın.  Benzer bir şey göreceksiniz:

    ![Azure Data Lake Analytics işi ayrıntıları ile başarısız oldu.](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    Kaynak klasörü bulunamadı söyler.
6. Tıklatın **yinelenen komut dosyası**.
7. Güncelleştirme **FROM** aşağıdaki yolu:

    "/ Samples/Data/SearchLog.tsv"
8. **İşi Gönder**'e tıklayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md)
* [Azure Data Lake Azure PowerShell kullanarak Analytics ile çalışmaya başlama](data-lake-analytics-get-started-powershell.md)
* [Azure Data Lake Analytics ve U-SQL Visual Studio kullanarak kullanmaya başlama](data-lake-analytics-u-sql-get-started.md)
* [Azure Data Lake Analytics'i Azure Portal'ı kullanarak yönetme](data-lake-analytics-manage-use-portal.md)
