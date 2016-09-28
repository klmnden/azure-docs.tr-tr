<properties
   pageTitle="Data Lake Store ile çalışmaya başlama | Azure"
   description="Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanma"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/13/2016"
   ms.author="nitinme"/>


# Azure PowerShell'i kullanarak Azure Data Lake Store ile çalışmaya başlama

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Azure Data Lake Store hesabı oluşturma ve klasör oluşturma, veri dosyalarını yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure PowerShell'in nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

## Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).


##Azure PowerShell 1.0 sürümünü veya sonraki bir sürümü yükleme

[Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md#prerequisites) konusunun Önkoşul bölümüne bakın.

## Azure Data Lake Store hesabı oluşturma

1. Masaüstünüzde yeni bir Windows PowerShell penceresi açın ve Azure hesabınızda oturum açmak, aboneliği ayarlamak ve Data Lake Store sağlayıcısını kaydetmek için aşağıdaki kod parçacığını girin. Oturum açmanız istendiğinde, bir abonelik yöneticisi/sahibi olarak oturum açtığınızdan emin olun.

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Azure Data Lake Store hesabı, bir Azure Kaynak Grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure Kaynak Grubu oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")

2. Bir Azure Data Lake Store hesabı oluşturun. Belirttiğiniz ad yalnızca küçük harflerden ve rakamlardan oluşmalıdır.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Store hesabı oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")

3. Hesabın başarıyla oluşturulduğunu doğrulayın.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Bu işlemin çıktısı **True** olmalıdır.

## Azure Data Lake Store'unuzda dizin yapıları oluşturma

Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında dizin oluşturabilirsiniz.

1. Bir kök dizin belirtin.

        $myrootdir = "/"

2. Belirtilen kökün altında **mynewdirectory** adlı yeni bir dizin oluşturun.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Yeni dizinin başarıyla oluşturulduğunu doğrulayın.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Bu işlemin aşağıdakine benzer bir çıktı göstermesi gerekir:

    ![Dizini Doğrulama](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")


## Azure Data Lake Store'unuza veri yükleme

Verilerinizi Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir dizine yüklenecek şekilde yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz dizine (**mynewdirectory**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınız üzerinde C:\sampledata gibi yerel bir dizinde depolayın\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Data Lake Store'unuzda verileri yeniden adlandırma, indirme ve silme

Bir dosyayı yeniden adlandırmak için aşağıdaki komutu kullanın:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Bir dosyayı indirmek için aşağıdaki komutu kullanın.

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Bir dosyayı silmek için aşağıdaki komutu kullanın:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

İstendiğinde, öğeyi silmek için **Y** yazın. Birden fazla dosyayı silmek istiyorsanız tüm yolları virgülle ayrılmış olarak sağlayabilirsiniz.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Azure Data Lake Store hesabınızı silme

Data Lake Store hesabınızı silmek için aşağıdaki komutu kullanın.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

İstendiğinde, hesabı silmek için **Y** yazın.


## Sonraki adımlar

- [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
- [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Sep16_HO3-->


