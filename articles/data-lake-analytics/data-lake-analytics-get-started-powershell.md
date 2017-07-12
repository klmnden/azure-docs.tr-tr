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
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: faf17bcac66a70fc78bb171e172886fd2dcadca8
ms.contentlocale: tr-tr
ms.lasthandoff: 06/16/2017


---
<a id="get-started-with-azure-data-lake-analytics-using-azure-powershell" class="xliff"></a>

# Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure PowerShell kullanarak Azure Data Lake Analytics hesapları oluşturma ve sonra U-SQL işleri gönderip çalıştırma hakkında bilgi edinin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki bilgilere sahip olmanız gerekir:

* **Azure Data Lake Analytics hesabı**. Bkz. [Data Lake Analytics ile çalışmaya başlama](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-get-started-portal).
* **Azure PowerShell içeren bir iş istasyonu**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

<a id="log-in-to-azure" class="xliff"></a>

## Azure'da oturum açma

Bu öğreticide, Azure PowerShell kullanımıyla ilgili bilgi sahibi olduğunuz varsayılır. Özellikle Azure'da oturum açmayı bilmeniz gerekir. Yardıma ihtiyacınız varsa bkz. [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/en-us/powershell/azure/get-started-azureps).

Abonelik adı ile oturum açmak için:

```
Login-AzureRmAccount -SubscriptionName "ContosoSubscription"
```

Oturum açmak için abonelik adı yerine abonelik kimliğini de kullanabilirsiniz:

```
Login-AzureRmAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Başarılı olması halinde bu komutun çıkışı şu metin gibi görünür:

```
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

<a id="preparing-for-the-tutorial" class="xliff"></a>

## Öğreticiye hazırlanma

Bu öğreticideki PowerShell kod parçacıkları bu bilgileri depolamak için aşağıdaki değişkenleri kullanır:

```
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

<a id="get-information-about-a-data-lake-analytics-account" class="xliff"></a>

## Bir Data Lake Analytics hesabı hakkında bilgi edinme

```
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

<a id="submit-a-u-sql-job" class="xliff"></a>

## U-SQL işi gönderme

U-SQL betiğini tutmak için bir PowerShell değişkeni oluşturun.

```
$script = @"
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

"@
```

Betiği gönderin.

```
$job = Submit-AdlJob -AccountName $adla –Script $script
```

Alternatif olarak, betiği dosya olarak kaydedebilir ve şu komutla gönderebilirsiniz:

```
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -AccountName $adla –ScriptPath $filename
```


Belirli bir işin durumunu alın. İş tamamlanana kadar bu cmdlet'i kullanmaya devam edin.

```
$job = Get-AdlJob -AccountName $adla -JobId $job.JobId
```

Bir iş tamamlanana kadar Get-AdlAnalyticsJob yöntemini tekrar tekrar çağırmak yerine, Wait-AdlJob cmdlet’ini kullanabilirsiniz.

```
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Çıkış dosyasını indirin.

```
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "C:\data.csv"
```

<a id="see-also" class="xliff"></a>

## Ayrıca bkz.
* Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
* U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
* Yönetim görevleri için bkz. [Azure portalı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).

