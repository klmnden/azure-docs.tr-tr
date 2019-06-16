---
title: PowerShell kullanarak Azure Data Lake depolama Gen1 ile çalışmaya başlama | Microsoft Docs
description: Bir Azure Data Lake depolama Gen1 hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanma
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 5bec627f114a20033ca4364c39c048763df36b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161463"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-azure-powershell"></a>Azure Data Lake depolama Gen1 ile çalışmaya başlama Azure PowerShell'i kullanma
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Hesabı, silme, bir Azure Data Lake depolama Gen1 hesabı oluşturmak ve klasör oluşturma karşıya yükleme ve veri dosyalarını indirir temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanma hakkında bilgi edinin. Data Lake depolama Gen1 hakkında daha fazla bilgi için bkz: [genel bakış Data Lake depolama Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="authentication"></a>Kimlik Doğrulaması
Bu makalede Data Lake depolama burada Azure hesabı kimlik bilgilerinizi girmeniz istenir Gen1 ile basit bir kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake depolama hesabı ve dosya sistemi sonra oturum açmış olan kullanıcının erişim düzeyi tarafından yönetilir Gen1 için erişim düzeyi. Ancak, diğer yaklaşımlar da Data Lake depolama Gen1 ile kimlik doğrulaması için olan vardır **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulaması**. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.

## <a name="create-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı oluşturun
1. Masaüstünüzde yeni bir Windows PowerShell penceresi açın. Azure hesabınızda oturum açmak, aboneliği ayarlamak ve Data Lake depolama Gen1 sağlayıcısını kaydetmek için aşağıdaki kod parçacığını girin. Oturum açmanız istendiğinde, bir abonelik yöneticileri/sahibi oturum emin olun:

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Bir Data Lake depolama Gen1 hesabı bir Azure kaynak grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Bir Azure kaynak grubu oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Bir Data Lake depolama Gen1 hesabı oluşturun. Belirttiğiniz ad yalnızca küçük harflerden ve rakamlardan oluşmalıdır.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    ![Bir Data Lake depolama Gen1 hesabı oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "bir Data Lake depolama Gen1 hesabı oluşturun")
4. Hesabın başarıyla oluşturulduğunu doğrulayın.

        Test-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

    Bu cmdlet'in çıktısı **True** olmalıdır.

## <a name="create-directory-structures-in-your-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabınızdaki dizin yapıları oluşturma
Veri depolamak ve yönetmek için Data Lake depolama Gen1 hesabınızın altında dizin oluşturabilirsiniz.

1. Bir kök dizin belirtin.

        $myrootdir = "/"
2. Belirtilen kökün altında **mynewdirectory** adlı yeni bir dizin oluşturun.

        New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/mynewdirectory
3. Yeni dizinin başarıyla oluşturulduğunu doğrulayın.

        Get-AzDataLakeStoreChildItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir

    Aşağıdaki ekran görüntüsünde gösterildiği gibi bir çıkış göstermelidir:

    ![Dizin Doğrulama](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

## <a name="upload-data-to-your-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabınıza veri yükleme
Verilerinizi Data Lake depolama Gen1 doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir dizine yükleyebilirsiniz. Bu bölümdeki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz dizine (**mynewdirectory**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

    Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-storage-gen1-account"></a>Yeniden adlandırma, indirme ve Data Lake depolama Gen1 hesabınızdan veri silme
Bir dosyayı yeniden adlandırmak için aşağıdaki komutu kullanın:

    Move-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Bir dosyayı indirmek için aşağıdaki komutu kullanın.

    Export-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Bir dosyayı silmek için aşağıdaki komutu kullanın:

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

İstendiğinde, öğeyi silmek için **Y** yazın. Birden fazla dosyayı silmek istiyorsanız tüm yolları virgülle ayrılmış olarak sağlayabilirsiniz.

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabınızı silme
Data Lake depolama Gen1 hesabınızı silmek için aşağıdaki komutu kullanın.

    Remove-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Performans ayarlama Kılavuzu, Azure Data Lake depolama Gen1 ile PowerShell kullanma](data-lake-store-performance-tuning-powershell.md)
* [Büyük veri gereksinimleri için Azure Data Lake depolama Gen1 kullanın](data-lake-store-data-scenarios.md) 
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
