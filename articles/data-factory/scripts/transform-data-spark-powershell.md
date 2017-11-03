---
title: "PowerShell komut dosyası dönüştürme verileri Data Factory kullanarak bulutta | Microsoft Docs"
description: "Bu PowerShell Betiği, Azure Hdınsight Spark kümesi üzerinde Spark programını çalıştırarak verileri bulutta dönüştürür."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: spelluru
ms.openlocfilehash: 195b7276346827479fbbe10dfaaaa9ed1d754967
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---transform-data-in-cloud-using-azure-data-factory"></a>PowerShell Betiği - Azure Data Factory kullanarak bulut verilerde dönüştürme

Bu örnek PowerShell komut dosyasını bir Azure Hdınsight Spark kümesi üzerinde Spark programını çalıştırarak verileri bulutta dönüştüren bir işlem hattı oluşturur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı**. Bir python betiğini ve girdi dosyası oluşturun ve bunları Azure Storage'a yükler. Spark programının çıktısı bu depolama hesabında depolanır. İsteğe bağlı Spark kümesi, birincil depolama alanıyla aynı depolama hesabını kullanır.  

### <a name="upload-python-script-to-your-blob-storage-account"></a>Python betiğini Blob Depolama hesabınıza yükleme
1. Aşağıdaki içeriğe sahip **WordCount_Spark.py** adlı bir python dosyası oluşturun: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. **&lt;storageAccountName&gt;**’i Azure Depolama hesabınızın adıyla değiştirin. Ardından dosyayı kaydedin. 
3. Azure Blob depolama alanınızda henüz yoksa **adftutorial** adlı bir kapsayıcı oluşturun. 
4. **Spark** adlı bir klasör oluşturun.
5. **Spark** klasörünün altında **script** adlı bir alt klasör oluşturun. 
6. **WordCount_Spark.py** dosyasını **script** alt klasörüne yükleyin. 


### <a name="upload-the-input-file"></a>Girdi dosyasını yükleme
1. Bazı metinlerle **minecraftstory.txt** adlı bir dosya oluşturun. Spark programı bu metindeki sözcükleri sayar. 
2. Adlı bir alt klasör oluşturun `inputfiles` içinde `spark` blob kapsayıcısının klasör. 
3. `minecraftstory.txt` dosyasını `inputfiles` alt klasörüne yükleyin. 

## <a name="sample-script"></a>Örnek komut dosyası
> [!IMPORTANT]
> Bu komut dosyası c:\ klasöründe sabit diskinizde Data Factory varlıkları (bağlı hizmet, veri kümesi ve ardışık düzeni) tanımlayan JSON dosyaları oluşturur.

[!code-powershell[main](../../../powershell_scripts/data-factory/transform-data-using-spark/transform-data-using-spark.ps1 "Transform data using Spark")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırma sonra aşağıdaki komutu kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için kullanabilirsiniz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Veri Fabrikası kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır:

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2linkedservice) | Veri fabrikasında bağlı hizmet oluşturur. Bağlı hizmet veri deposunda veya işlem bir data factory'ye bağlar. |
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactorv2ypipeline) | Data factory işlem hattı oluşturur. Bir işlem hattı belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık düzeninde bir spark etkinlik verileri Azure Hdınsight Spark kümesi üzerinde bir program çalıştırarak dönüştürür. |
| [Çağırma AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/new-azurermdatafactoryv2pipelinerun) | Ardışık düzeni için bir farklı çalıştır oluşturur. Diğer bir deyişle, ardışık düzen çalışır. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | (Etkinlik) etkinlik çalışma ayrıntıları ardışık düzeninde alır. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](https://docs.microsoft.com/powershell/).

Ek Azure veri fabrikası PowerShell komut dosyası örnekleri bulunabilir [Azure veri fabrikası PowerShell örnekleri](../samples-powershell.md).