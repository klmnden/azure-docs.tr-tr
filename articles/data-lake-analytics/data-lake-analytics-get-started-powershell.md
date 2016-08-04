<properties 
   pageTitle="Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama | Azure" 
   description="Bir Data Lake Store hesabı oluşturmak, U-SQL'yi kullanarak Data Lake Analytics işi oluşturmak ve işi göndermek için Azure PowerShell'in nasıl kullanılacağını öğrenin. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="04/26/2016"
   ms.author="edmaca"/>

# Öğretici: Azure PowerShell'i kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure Data Lake Analytics hesapları oluşturmak, Data Lake Analytics işlerini [U-SQL](data-lake-analytics-u-sql-get-started.md) içinde tanımlamak ve Data Lake Analytic hesaplarına iş göndermek için Azure PowerShell olanağından nasıl yararlanılacağını öğrenin. Data Lake Analytics hakkında daha fazla bilgi için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).

Bu öğreticide, bir sekmeyle ayrılmış değerler (TSV) dosyasını okuyan ve bunu virgülle ayrılmış değerler (CSV) dosyasına dönüştüren bir iş geliştireceksiniz. Öğreticiyi desteklenen diğer araçları kullanarak tamamlamak için bu bölümün üst kısmındaki sekmelere tıklayın.

[AZURE.INCLUDE [basic-process-include](../../includes/data-lake-analytics-basic-process.md)]

##Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
- **Azure PowerShell içeren bir iş istasyonu**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md).
    
##Data Lake Analytics hesabı oluşturma

Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir. Bir Data Lake Analytics hesabı oluşturmak için aşağıdakileri belirtmeniz gerekir:

- **Azure Kaynak Grubu**: Data Lake Analytics hesabı bir Azure Kaynak grubu içinde oluşturulmalıdır. [Azure Resource Manager](../resource-group-overview.md), uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza yönelik tüm kaynakları tek ve eş güdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz.  

    Aboneliğinizdeki kaynak gruplarını numaralandırmak için:
    
        Get-AzureRmResourceGroup
    
    Yeni bir kaynak grubu oluşturmak için:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Data Lake Analytics hesap adı**
- **Konum**: Data Lake Analytics'i destekleyen Azure veri merkezlerinden biri.
- **Varsayılan Data Lake hesabı**: Her Data Lake Analytics hesabı, bir varsayılan Data Lake hesabına sahiptir.

    Yeni bir Data Lake hesabı oluşturmak için:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Data Lake hesap adı yalnızca küçük harflerden ve rakamlardan oluşmalıdır.



**Data Lake Analytics hesabı oluşturmak için**

1. Windows iş istasyonunuzdan PowerShell ISE'yi açın.
2. Şu betiği çalıştırın:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##Data Lake'e veri yükleme

Bu öğreticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir. 

Örnek bir arama günlüğü dosyası, ortak Azure Blob kapsayıcısına kopyalanmıştır. Dosyayı iş istasyonunuza indirmek ve ardından Data Lake Analytics hesabınızın varsayılan Data Lake Store hesabına yüklemek için aşağıdaki PowerShell betiğini kullanın.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Aşağıdaki PowerShell betiği, bir Data Lake Analytics hesabı için varsayılan Data Lake Store adını nasıl alacağınızı gösterir:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount

>[AZURE.NOTE] Azure Portal, örnek veri dosyalarını varsayılan Data Lake Store hesabına kopyalamak için bir kullanıcı arabirimi sağlar. Yönergeler için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics ile çalışmaya başlama](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Data Lake Analytics ayrıca Azure Blob depolama alanına da erişebilir.  Verileri Azure Blob depolama alanına yüklemek için bkz. [Azure PowerShell'i Azure Storage ile kullanma](../storage/storage-powershell-guide-full.md).

##Data Lake Analytics işlerini gönderme

Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](http://go.microsoft.com/fwlink/?LinkId=691348).

**Bir Data Lake Analytics işi betiği oluşturmak için**

- Aşağıdaki U-SQL betiği ile bir metin dosyası oluşturun ve metin dosyasını iş istasyonunuza kaydedin:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur. 
    
    Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.
    
    Varsayılan Data Lake hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örneğin: 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Mutlak yolları, bağlı Storage hesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Ortak blob veya ortak kapsayıcı erişim izinlerine sahip Azure Blob kapsayıcısı şu an için desteklenmemektedir.    
    
    
**İşi göndermek için**

1. Windows iş istasyonunuzdan PowerShell ISE'yi açın.
2. Şu betiği çalıştırın:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 
                        
        While (($t = Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId).State -ne "Ended"){
            Write-Host "Job status: "$t.State"..."
            Start-Sleep -seconds 5
        }
        
        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Betikte, U-SQL betiği c:\tutorials\data-lake-analytics\copyFile.usql konumunda depolanır. Dosya yolunu uygun şekilde güncelleştirin.
 
İş tamamlandıktan sonra, dosyayı listelemek için aşağıdaki cmdlet'leri kullanabilir ve dosyayı indirebilirsiniz:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## Ayrıca bkz.

- Aynı öğreticiyi diğer araçları kullanarak görmek için sayfanın üst kısmındaki sekme seçicilerine tıklayın.
- Daha karmaşık bir sorgu görmek için bkz. [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md).
- U-SQL uygulamalarını geliştirmeye başlamak için bkz. [Visual Studio için Data Lake Araçları'nı kullanarak U-SQL betikleri geliştirme](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL öğrenmek için bkz. [Azure Data Lake Analytics U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md).
- Yönetim görevleri için bkz. [Azure Portal'ı kullanarak Azure Data Lake Analytics'i yönetme](data-lake-analytics-manage-use-portal.md).
- Data Lake Analytics'e yönelik bir genel bakış için bkz. [Azure Data Lake Analytics'e genel bakış](data-lake-analytics-overview.md).




<!----HONumber=Jun16_HO2-->


