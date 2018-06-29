---
title: Azure Data Lake Storage Gen1 ile çalışmaya başlamak için PowerShell kullanma | Microsoft Docs
description: Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanma
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: f208722d768e2bccf2e5b4d7b4543f8cbba4f185
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036794"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-azure-powershell"></a>Azure Data Lake Storage Gen1 ile çalışmaya başlama Azure PowerShell'i kullanma
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Azure Data Lake Store hesabı oluşturma ve klasör oluşturma, veri dosyalarını yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure PowerShell'in nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz: [genel bakış Data Lake Storage Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="authentication"></a>Kimlik Doğrulaması
Bu makalede, Azure hesabı kimlik bilgilerinizi girmenizi isteyen daha basit bir Data Lake Store kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake Store hesabına ve dosya sistemine erişim düzeyi bu durumda oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, Data Lake Store kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** şeklinde diğer yaklaşımlar da mevcuttur. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma
1. Masaüstünüzde yeni bir Windows PowerShell penceresi açın. Azure hesabınızda oturum açmak, aboneliği ayarlamak ve Data Lake Store sağlayıcısını kaydetmek için aşağıdaki kod parçacığını girin. Oturum açmanız istendiğinde, bir abonelik yöneticisi/sahibi olarak oturum açtığınızdan emin olun.

        # Log in to your Azure account
        Connect-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Azure Data Lake Store hesabı, bir Azure Kaynak Grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Bir Azure kaynak grubu oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Bir Azure Data Lake Store hesabı oluşturun. Belirttiğiniz ad yalnızca küçük harflerden ve rakamlardan oluşmalıdır.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Store hesabı oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")
4. Hesabın başarıyla oluşturulduğunu doğrulayın.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Bu cmdlet'in çıktısı **True** olmalıdır.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Azure Data Lake Store'unuzda dizin yapıları oluşturma
Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında dizin oluşturabilirsiniz.

1. Bir kök dizin belirtin.

        $myrootdir = "/"
2. Belirtilen kökün altında **mynewdirectory** adlı yeni bir dizin oluşturun.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Yeni dizinin başarıyla oluşturulduğunu doğrulayın.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Aşağıdaki ekran görüntüsünde gösterildiği gibi bir çıkış göstermelidir:

    ![Dizin Doğrulama](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Azure Data Lake Store'unuza veri yükleme
Verilerinizi Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir dizine yüklenecek şekilde yükleyebilirsiniz. Bu bölümdeki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz dizine (**mynewdirectory**) nasıl yükleneceğini göstermektedir.

Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz. Dosyayı indirin ve bilgisayarınızda C:\sampledata\ gibi yerel bir dizinde depolayın.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Data Lake Store'unuzda verileri yeniden adlandırma, indirme ve silme
Bir dosyayı yeniden adlandırmak için aşağıdaki komutu kullanın:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Bir dosyayı indirmek için aşağıdaki komutu kullanın.

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Bir dosyayı silmek için aşağıdaki komutu kullanın:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

İstendiğinde, öğeyi silmek için **Y** yazın. Birden fazla dosyayı silmek istiyorsanız tüm yolları virgülle ayrılmış olarak sağlayabilirsiniz.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Azure Data Lake Store hesabınızı silme
Data Lake Store hesabınızı silmek için aşağıdaki komutu kullanın.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

İstendiğinde, hesabı silmek için **Y** yazın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Data Lake Store ile PowerShell’i kullanmaya yönelik performans ayarlama kılavuzu](data-lake-store-performance-tuning-powershell.md)
* [Azure Data Lake Store'u büyük veri gereksinimleri için kullanma](data-lake-store-data-scenarios.md) 
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
