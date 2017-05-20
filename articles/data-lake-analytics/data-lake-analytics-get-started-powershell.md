---
title: "Azure PowerShell&quot;i kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Microsoft Belgeleri"
description: "Azure PowerShell kullanarak bir Data Lake Analytics hesabı oluşturun, U-SQL&quot;yi kullanarak Data Lake Analytics işi oluşturun ve bu işi gönderin. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 6985dff332928d704f30e167c3bddb62bcc6cac1
ms.contentlocale: tr-tr
ms.lasthandoff: 05/09/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Öğretici: Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure PowerShell kullanarak Azure Data Lake Analytics hesapları oluşturma ve sonra U-SQL işleri gönderip çalıştırma hakkında bilgi edinin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdaki bilgilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell içeren bir iş istasyonu**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="preparing-for-the-tutorial"></a>Öğreticiye hazırlanma
Bir Data Lake Analytics hesabı oluşturmak için öncelikle şunları tanımlamanız gerekir:

* **Azure Kaynak Grubu**: Data Lake Analytics hesabı bir Azure Kaynak grubu içinde oluşturulmalıdır.
* **Data Lake Analytics hesap adı**: Data Lake hesabı adı yalnızca küçük harflerden ve rakamlardan oluşmalıdır.
* **Konum**: Data Lake Analytics'i destekleyen Azure veri merkezlerinden biri.
* **Varsayılan Data Lake Store hesabı**: Her Data Lake Analytics hesabı, bir varsayılan Data Lake Store hesabına sahiptir. Bu hesaplar aynı konumda olmalıdır.

Bu öğreticideki PowerShell kod parçacıkları, bu bilgileri depolamak için bu değişkenleri kullanır

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="create-a-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Kullanılacak bir Kaynak Grubunuz henüz yoksa bir tane oluşturun. 

```
New-AzureRmResourceGroup -Name  $rg -Location $location
```

Her Data Lake Analytics hesabı, günlükleri depolamak için kullandığı varsayılan bir Data Lake Store hesabı gerektirir. Var olan bir hesabı yeniden kullanabilir veya yeni bir hesap oluşturabilirsiniz. 

```
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
```

Bir Kaynak Grubu ve Data Lake Store hesabı oluşturulduktan sonra Data Lake Analytics hesabı oluşturun.

```
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Bir Data Lake Analytics hesabı hakkında bilgi edinme

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>U-SQL işi gönderme

Aşağıdaki U-SQL betiği ile bir metin dosyası oluşturun.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Betiği gönderin.

```
Submit-AdlJob -AccountName $adla –ScriptPath "d:\test.usql"Submit
```

## <a name="monitor-u-sql-jobs"></a>U-SQL İşlerini izleme

Hesaptaki tüm işleri listeleyin. Çıktı o anda çalışan işleri ve yakın zamanda tamamlanan işleri içerir.

```
Get-AdlJob -Account $adla
```

Belirli bir işin durumunu alın.

```
Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Bir iş tamamlanana kadar Get-AdlAnalyticsJob yöntemini tekrar tekrar çağırmak yerine, Wait-AdlJob cmdlet’ini kullanabilirsiniz.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

İş tamamlandıktan sonra, dosyaları bir klasörde listeleyerek çıktı dosyasının mevcut olup olmadığını denetleyin.

```
Get-AdlStoreChildItem -Account $adls -Path "/"
```

Bir dosyanın varlığını denetleyin.

```
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

## <a name="uploading-and-downloading-files"></a>Dosyaları karşıya yükleme ve indirme

U-SQL betiğinin çıktısını indirin.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv"  -Destination "D:\data.csv"
```


U-SQL betiğinin girdisi olarak kullanılacak bir dosyayı karşıya yükleyin.

```
Import-AdlStoreItem -AccountName $adls -Path "D:\data.tsv" -Destination "/data_copy.csv" 
```

## <a name="see-also"></a>Ayrıca bkz.
* Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

