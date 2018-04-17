---
title: Data Factory - Azure Hdınsight kullanarak isteğe bağlı Hadoop kümeleri oluşturma | Microsoft Docs
description: Azure Data Factory kullanarak Hdınsight'ta isteğe bağlı Hadoop kümeleri oluşturmayı öğrenin.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: 213f1122dc9f616474005070ae3aefa45641fecc
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Azure Data Factory kullanarak Hdınsight'ta isteğe bağlı Hadoop kümeleri oluşturma
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Azure Data Factory](../data-factory/introduction.md) düzenler ve taşınmasını ve dönüştürülmesini veri otomatikleştiren bir bulut tabanlı veri tümleştirme hizmetidir. Bir Hdınsight Hadoop kümesi yalnızca bir giriş veri dilimi işlemek ve işlem tamamlandıktan sonra kümeyi silmek için zaman oluşturabilirsiniz. Bir isteğe bağlı Hdınsight Hadoop kümesi kullanarak avantajlarından bazıları şunlardır:

- Hdınsight Hadoop kümesi (artı kısa yapılandırılabilir boşta kalma süresi), yalnızca ödeme zaman işi çalışıyor. Hdınsight kümeleri Faturalaması dakika başına, bunları veya kullanıp kullanmadığınızı Faturalaması. Veri fabrikasında bir isteğe bağlı Hdınsight bağlı hizmeti kullandığınızda, isteğe bağlı kümeleri oluşturulur. Ve işleri tamamlandığında kümeleri otomatik olarak silinir. Bu nedenle, yalnızca zaman ve kısa boşta kalma süresi (yaşam süresi ayarı) çalışan iş için ödeme yaparsınız.
- Data Factory işlem hattı kullanarak bir iş akışı oluşturabilirsiniz. Örneğin, bir şirket içi SQL Server'dan Azure blob depolama alanına veri kopyalama, bir isteğe bağlı Hdınsight Hadoop kümesinde bir Hive betiği ve Pig betiği çalıştırılarak verileri işlemek için ardışık düzen olabilir. Sonra sonuç verilerini Azure SQL Data Warehouse kullanacak BI uygulamalar için kopyalayın.
- İş akışını (saatlik, günlük, haftalık, aylık, vb.) düzenli aralıklarla çalışacak şekilde zamanlayabilirsiniz.

Azure Data Factory'de veri fabrikası bir veya daha fazla veri işlem hattı olabilir. Veri ardışık bir veya daha fazla etkinlikler içeriyor. İki tür etkinlik vardır: [veri taşıma etkinlikleri](../data-factory/copy-activity-overview.md) ve [veri dönüştürme etkinlikleri](../data-factory/transform-data.md). Veri kaynağına veri deposundan hedef veri deposuna taşımak için veri taşıma etkinlikleri (şu anda, yalnızca kopyalama etkinliği) kullanın. Veri dönüştürme/işlem için veri dönüştürme etkinlikleri kullanın. Hdınsight Hive etkinliği Data Factory ile desteklenen dönüştürme etkinlikleri biridir. Bu öğreticide Hive dönüşüm etkinliği kullanın.

Bir hive etkinliği kendi Hdınsight Hadoop kümesi veya bir isteğe bağlı Hdınsight Hadoop kümesi kullanmak üzere yapılandırabilirsiniz. Bu öğreticide, data factory işlem hattı Hive etkinliğinde bir isteğe bağlı Hdınsight kümesi kullanmak üzere yapılandırılır. Bu nedenle, etkinlik veri dilimi işlemeye çalıştığında, şunlar olur:

1. Hdınsight Hadoop kümesi, yalnızca dilim işlemek zaman için otomatik olarak oluşturulur.  
2. Giriş verisi HiveQL betiğini küme üzerinde çalışan tarafından işlenir.
3. İşlem tamamlandıktan sonra kümeyi yapılandırılan süreyi (timeToLive ayarını) için boşta Hdınsight Hadoop kümesi silindi. Sonraki veri dilimi bu timeToLive boşta kalma süresi ile işlemek için kullanılabilir durumda, aynı küme dilim işlemek için kullanılır.  

Bu öğreticide, hive etkinliği ile ilişkili HiveQL betiğini aşağıdaki eylemleri gerçekleştirir:

1. Bir Azure Blob depolamada depolanan ham web günlüğü verilerini başvuran bir dış tablo oluşturur.
2. Ham verileri, ay ve yıl bölümler.
3. Bölümlenmiş verileri Azure blob depolama alanında depolar.

Bu öğreticide, Azure Blob Storage'da depolanan ham web günlüğü verilerini başvuran bir dış tablo hive etkinliği ile ilişkili HiveQL betiğini oluşturur. Girdi dosyasındaki, her aya ait örnek satırlar şunlardır.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

HiveQL betiğini ham verileri ay ve yıl bölümler. Önceki girişinize göre üç çıkış klasör oluşturur. Her klasör her ay girişlerinden sahip bir dosya içerir.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

Data Factory veri dönüştürme etkinlikleri Hive etkinliği yanı sıra bir listesi için bkz: [dönüştürme ve Azure Data Factory kullanarak Analiz](../data-factory/transform-data.md).

> [!NOTE]
> Şu anda, Azure veri fabrikası'ndan yalnızca Hdınsight kümesi sürüm 3.2 oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki yönergeleri başlamadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* [Azure aboneliği](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Depolama hesabını hazırlama
Bu senaryoda, en çok üç depolama hesaplarını kullanabilirsiniz:

- Hdınsight kümesi için varsayılan depolama hesabı
- giriş verisi için depolama hesabı
- Çıktı verileri için depolama hesabı

Öğretici basitleştirmek için üç amaçlara hizmet için bir depolama hesabı kullanın. Bu bölümde bulunan Azure PowerShell örnek betiği aşağıdaki görevleri gerçekleştirir:

1. Azure'da oturum açın.
2. Bir Azure kaynak grubu oluşturun.
3. Bir Azure Depolama hesabı oluşturun.
4. Depolama hesabında Blob kapsayıcısı oluşturma
5. Aşağıdaki iki dosyaları Blob kapsayıcısına kopyalayın:

   * Giriş veri dosyası: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * HiveQL betiğini: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Her iki dosyaları bir ortak Blob kapsayıcısında depolanır.


**Depolama birimini hazırlayın ve Azure PowerShell kullanarak dosyaları kopyalamak için:**
> [!IMPORTANT]
> Azure kaynak grubu ve komut dosyası tarafından oluşturulan Azure depolama hesabı adlarını belirtin.
> Yazma **kaynak grubu adı**, **depolama hesabı adı**, ve **depolama hesabı anahtarı** komut dosyası tarafından yüzdelik. Sonraki bölümde gereksinim duyarsınız.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use the following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

PowerShell Betiği yardıma gereksinim duyarsanız, bkz: [Azure Storage ile Azure PowerShell'i kullanarak](../storage/common/storage-powershell-guide-full.md). Azure CLI kullanmayı istiyorsanız, bkz: [ek](#appendix) Azure CLI komut dosyası için bölüm.

**Depolama hesabı ve içeriğini incelemek için**

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **kaynak grupları** sol bölmede.
3. PowerShell komut dosyasında oluşturulan kaynak grubu adı çift tıklatın. Listelenen çok fazla kaynak grupları varsa, filtrenin kullanın.
4. Üzerinde **kaynakları** kutucuğu, bir kaynak kaynak grubu diğer projelerle paylaşmak sürece listelenen sahip. Bu kaynak, daha önce belirttiğiniz ada sahip bir depolama hesabıdır. Depolama hesabının adına tıklayın.
5. Tıklatın **BLOB'lar** döşeme.
6. Tıklatın **adfgetstarted** kapsayıcı. İki klasör bkz: **inputdata** ve **betik**.
7. Klasörü açın ve klasörlerdeki dosyaların denetleyin. İnputdata giriş verisi input.log dosyasıyla ve HiveQL komut dosyası komut dosyası klasör içerir.

## <a name="create-a-data-factory-using-resource-manager-template"></a>Resource Manager şablonu kullanarak bir veri fabrikası oluşturun
Depolama hesabı, girdi verileri ve hazırlanan HiveQL betiğini ile bir Azure data factory oluşturmak hazır olursunuz. Veri Fabrikası oluşturmaya yönelik birkaç yöntem vardır. Bu öğreticide, Azure portalını kullanarak bir Azure Resource Manager şablonu dağıtarak veri fabrikası oluşturun. Kullanarak bir Resource Manager şablonu dağıtabilirsiniz [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) ve [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template). Diğer veri fabrikası oluşturma yöntemleri için bkz: [Öğreticisi: ilk data factory'nizi derleme](../data-factory/quickstart-create-data-factory-dot-net.md).

1. Aşağıdaki resme tıklayarak Azure'da oturum açın ve Azure portalında Resource Manager şablonunu açın. Şablonunda konumdadır: https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json. Bkz: [Data Factory varlıklarını şablondaki](#data-factory-entities-in-the-template) şablonda tanımlı varlıklar hakkında ayrıntılı bilgi için bölüm. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Seçin **kullanım varolan** seçenek için **kaynak grubu** ayarlama ve (PowerShell Betiği kullanılarak) önceki adımda oluşturduğunuz kaynak grubunun adını seçin.
3. Veri Fabrikası için bir ad girin (**veri fabrikası adı**). Bu ad genel olarak benzersiz olmalıdır.
4. Girin **depolama hesabı adı** ve **depolama hesabı anahtarı** önceki adımda yazdığınız.
5. Seçin **hüküm ve koşulları kabul ediyorum** okuma sonra yukarıda belirtilen **hüküm ve koşullar**.
6. Seçin **panoya Sabitle** seçeneği.
6. Tıklatın **satın alma ve oluşturma**. Panoda bir kutucuğu adlı gördüğünüz **dağıtma şablon dağıtımı**. Bekle **kaynak grubu** kaynak grubunuz için bir dikey pencere açılır. Kaynak grubu dikey penceresini açmak için kaynak grubu adı başlıklı kutucuk de tıklayabilirsiniz.
6. Kaynak grubu dikey zaten açık değilse, kaynak grubu açmak için kutucuğa tıklayın. Şimdi depolama hesabı kaynağı ek olarak listelenen bir daha fazla veri fabrikası kaynak göreceksiniz.
7. Veri fabrikanızın adına tıklayın (için belirtilen değere **veri fabrikası adı** parametresi).
8. Veri Fabrikası dikey penceresinde tıklayın **diyagramı** döşeme. Bir etkinliği bir girdi veri kümesi ve bir çıkış veri kümesi ile diyagramda gösterilmektedir:

    ![Azure veri fabrikası Hdınsight isteğe bağlı Hive etkinlik ardışık düzeni diyagramı](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    Adları Resource Manager şablonunda tanımlanır.
9. Çift **AzureBlobOutput**.
10. Üzerinde **son güncelleştirilen dilimler**, bir dilim göreceksiniz. Durum ise **devam eden**, olarak değiştirilmesini bekleyin **hazır**. Genellikle hakkında sürdüğünü **20 dakika** Hdınsight kümesi oluşturmak için.

### <a name="check-the-data-factory-output"></a>Veri Fabrikası çıktısını denetleyin

1. Aynı yordamı son oturumda adfgetstarted kapsayıcısının kapsayıcıları denetlemek için kullanın. Ek olarak iki yeni kapsayıcı vardır **adfgetsarted**:

   * Bir kapsayıcı adıyla deseni izler: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Bu kapsayıcı Hdınsight kümesi için varsayılan kapsayıcıdır.
   * adfjobs: Bu kapsayıcı ADF iş günlükleri için kapsayıcıdır.

     Veri Fabrikası çıkış depolanan **afgetstarted** Resource Manager şablonunda yapılandırıldığı şekilde.
2. Tıklatın **adfgetstarted**.
3. Çift **partitioneddata**. Gördüğünüz bir **yıl 2014 =** klasörü tüm web günlüklerini yıl 2014 tarihli olduğundan.

    ![Azure veri fabrikası Hdınsight isteğe bağlı Hive etkinliği ardışık düzeni çıkış](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Listede aşağı ayrıntıya, Ocak, Şubat ve Mart için üç klasörleri göreceksiniz. Ve her ay için bir günlük yok.

    ![Azure veri fabrikası Hdınsight isteğe bağlı Hive etkinliği ardışık düzeni çıkış](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-the-template"></a>Şablondaki Data Factory varlıkları
Veri Fabrikası için üst düzey Resource Manager şablonu benzer nasıl aşağıda verilmiştir:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```

### <a name="define-data-factory"></a>Veri fabrikası tanımlama
Resource Manager şablonunda bir veri fabrikasını aşağıdaki örnekte gösterildiği gibi tanımlayın:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
DataFactoryName şablonu dağıttığınızda, belirttiğiniz veri fabrikası adıdır. Veri Fabrikası şu anda yalnızca Doğu ABD, Batı ABD ve Kuzey Avrupa bölgelerde desteklenir.

### <a name="defining-entities-within-the-data-factory"></a>Data factory varlıkları tanımlama
Aşağıdaki Data Factory varlıkları JSON şablonunda tanımlanır:

* [Azure Storage bağlı hizmeti](#azure-storage-linked-service)
* [İsteğe bağlı HDInsight bağlı hizmeti](#hdinsight-on-demand-linked-service)
* [Azure Blob girdi veri kümesi](#azure-blob-input-dataset)
* [Azure Blob çıktı veri kümesi](#azure-blob-output-dataset)
* [Kopyalama etkinliği içeren bir veri işlem hattı](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
Azure Depolama bağlı hizmeti, Azure depolama hesabınızı veri fabrikasına bağlar. Bu öğreticide, aynı depolama hesabındaki varsayılan Hdınsight depolama hesabı, giriş veri depolama ve çıktı veri depolama kullanılır. Bu nedenle, bir Azure Storage bağlı hizmeti yalnızca tanımlayın. Bağlantılı hizmet tanımı'nda ad ve Azure depolama hesabınızın anahtar belirtin. Bir Azure Storage bağlı hizmetini tanımlamak için kullanılan JSON özelliklerine ilişkin ayrıntılar için bkz. [Azure Storage bağlı hizmeti](../data-factory/connector-azure-blob-storage.md).

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
**ConnectionString**, storageAccountName ve storageAccountKey parametrelerini kullanır. Şablon dağıtımı sırasında bu parametrelerin değerlerini belirtin.  

#### <a name="hdinsight-on-demand-linked-service"></a>İsteğe bağlı HDInsight bağlantılı hizmeti
İsteğe bağlı Hdınsight bağlı hizmeti tanımı'nda, çalışma zamanında Hdınsight Hadoop kümesi oluşturmak için Data Factory hizmeti tarafından kullanılan yapılandırma parametreleri için değerleri belirtin. İsteğe bağlı bir HDInsight bağlı hizmetini tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar için [İşlem bağlı hizmetleri](../data-factory/compute-linked-services.md#azure-hdinsight-on-demand-linked-service) makalesine bakın.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Aşağıdaki noktalara dikkat edin:

* Veri Fabrikası oluşturur bir **Linux tabanlı** , Hdınsight kümesi.
* Hdınsight Hadoop kümesi depolama hesabı ile aynı bölgede oluşturulur.
* Bildirim *timeToLive* ayarı. Küme için 30 dakika boşta kaldıktan sonra data factory küme otomatik olarak siler.
* HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir.

Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](../data-factory/compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

> [!IMPORTANT]
> Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adları şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

#### <a name="azure-blob-input-dataset"></a>Azure blob girdi veri kümesi
Girdi veri kümesi tanımında blob kapsayıcı, klasör ve giriş verilerini içeren dosya adını belirtin. Bir Azure Blob veri kümesini tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob veri kümesi özellikleri](../data-factory/connector-azure-blob-storage.md).

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}

```

JSON tanımında belirli şunlara dikkat edin:

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Azure Blob çıktı veri kümesi
Çıktı veri kümesi tanımında blob kapsayıcısı ve çıktı verilerini tutan klasör adlarını belirtin. Bir Azure Blob veri kümesini tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar için bkz. [Azure Blob veri kümesi özellikleri](../data-factory/connector-azure-blob-storage.md).  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

FolderPath çıktı verilerini tutan klasör yolunu belirtir:

```json
"folderPath": "adfgetstarted/partitioneddata",
```

[Dataset kullanılabilirliği](../data-factory/concepts-datasets-linked-services.md) ayarı aşağıdaki gibidir:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

Azure Data Factory'de ardışık düzeni çıkış veri kümesi kullanılabilirlik sürücüleri. Bu örnekte, dilim aylık (EndOfInterval) ayın son gününde oluşturulur. 

#### <a name="data-pipeline"></a>Veri işlem hattı
İsteğe bağlı Azure Hdınsight kümesinde bir Hive betiği çalıştırılarak verileri dönüştüren bir ardışık düzen tanımlayın. Bu örnekte bir işlem hattı tanımlamak için kullanılan JSON öğelerinin açıklamaları için bkz. [İşlem Hattı JSON](../data-factory/concepts-pipelines-activities.md).

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                    }
                },
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

Ardışık düzen bir etkinlik, Hdınsighthive etkinliğini içerir. Yalnızca bir ay (bir dilim) işlenir için Ocak 2016'da, veri hem de başlangıç ve bitiş tarihleri gibi. Her ikisi de *Başlat* ve *son* etkinliğini hemen ay için verileri Data Factory işler için geçmiş bir tarihe sahiptir. Son gelecekteki bir tarih ise, veri fabrikası zamanı geldiğinde başka bir dilim oluşturur. Daha fazla bilgi için bkz: [veri fabrikası zamanlama ve yürütme](../data-factory/v1/data-factory-scheduling-and-execution.md).

## <a name="clean-up-the-tutorial"></a>Öğreticiyi silme

### <a name="delete-the-blob-containers-created-by-on-demand-hdinsight-cluster"></a>İsteğe bağlı Hdınsight kümesi tarafından oluşturulan blob kapsayıcıları Sil
Bir dilim mevcut canlı bir küme (timeToLive); olmadıkça işlenmesi gereken her zaman isteğe bağlı Hdınsight bağlı hizmetiyle, Hdınsight kümesi oluşturulur ve küme işlem bittiğinde silinir. Her küme için Azure Data Factory varsayılan stroage hesap olarak küme için kullanılan Azure blob storage'da bir blob kapsayıcısını oluşturur. Hdınsight kümesi silindi olsa bile, varsayılan blob depolama kapsayıcısını ve ilişkili depolama hesabı silinmez. Bu davranış tasarım gereğidir. Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Silme **adfjobs** ve **adfyourdatafactoryname linkedservicename datetimestamp** klasörler. Adfjobs kapsayıcısı Azure Data Factory iş günlükleri içerir.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) dağıtmak, yönetmek ve çözümünüzün bir grup olarak izlemek için kullanılır.  Bir kaynak grubunun silinmesi grup içinde tüm bileşenleri siler.  

1. [Azure portalı](https://portal.azure.com) üzerinde oturum açın.
2. Tıklatın **kaynak grupları** sol bölmede.
3. PowerShell komut dosyasında oluşturulan kaynak grubu adı'ı tıklatın. Listelenen çok fazla kaynak grupları varsa, filtrenin kullanın. Kaynak grubu yeni bir dikey pencerede açar.
4. Üzerinde **kaynakları** kutucuğu elinizde varsayılan depolama hesabı ve kaynak grubu diğer projelerle paylaşmak sürece listelenen veri fabrikası.
5. Tıklatın **silmek** dikey pencerenin üst kısmında. Bunun yapılması, depolama hesabı ve depolama hesabında depolanan verileri siler.
6. Silme işlemini onaylayın ve ardından kaynak grubu adı girin **silmek**.

Kaynak grubu sildiğinizde depolama hesabını silmek istemediğiniz durumda aşağıdaki mimarisi varsayılan depolama hesabından iş verilerini ayırarak göz önünde bulundurun. Bu durumda, iş verileriyle depolama hesabı için bir kaynak grubuna sahip ve bir kaynak grubu Hdınsight için varsayılan depolama hesabı için hizmet ve data factory bağlanır. İkinci kaynak grubu sildiğinizde, iş veri depolama hesabı etkilemez. Bunu yapmak için:

* Aşağıdaki kaynak yöneticisi şablonunuzu Microsoft.DataFactory/datafactories kaynak birlikte üst düzey kaynak grubuna ekleyin. Bir depolama hesabı oluşturur:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Yeni bir bağlantılı hizmet noktası için yeni depolama hesabı ekleyin:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* Hdınsight bağlantılı ondemand hizmeti ek dependsOn ve bir additionalLinkedServiceNames yapılandırın:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Data Factory Hive işleri işlemek için isteğe bağlı Hdınsight kümesi oluşturmak için nasıl kullanılacağını öğrendiniz. Daha fazla bilgi için:

* [Hadoop Öğreticisi: Hdınsight'ta Linux tabanlı Hadoop ile çalışmaya başlamak](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Hdınsight'ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md)
* [Hdınsight belgeleri](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Veri Fabrikası belgeleri](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Ek

### <a name="azure-cli-script"></a>Azure CLI komut dosyası
Öğretici yapmak için Azure PowerShell kullanmak yerine, Azure CLI kullanabilirsiniz. Azure CLI kullanmak için önce aşağıdaki yönergelere uygun şekilde Azure CLI yükleyin:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-to-prepare-the-storage-and-copy-the-files"></a>Depolama birimini hazırlayın ve dosyaları kopyalamak için Azure CLI kullanın

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

Kapsayıcı adı *adfgetstarted*. Olduğu gibi tutun. Aksi takdirde Resource Manager şablonunu güncellemeniz gerekir. Bu CLI betik yardıma gereksinim duyarsanız, bkz: [Azure Storage ile Azure CLI kullanarak](../storage/common/storage-azure-cli.md).
