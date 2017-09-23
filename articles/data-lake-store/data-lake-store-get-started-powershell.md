---
title: "PowerShell kullanarak Azure Data Lake Store ile çalışmaya başlama | Microsoft Docs"
description: "Bir Data Lake Store hesabı oluşturmak ve temel işlemleri gerçekleştirmek için Azure PowerShell'i kullanma"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: f78385e8ce96567f5a1e669ecf4a6c2efce3aaeb
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017

---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Azure PowerShell'i kullanarak Azure Data Lake Store ile çalışmaya başlama
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Azure Data Lake Store hesabı oluşturma ve klasör oluşturma, veri dosyalarını yükleme ve indirme, hesabınızı silme gibi temel işlemleri gerçekleştirmek için Azure PowerShell'in nasıl kullanılacağını öğrenin. Data Lake Store hakkında daha fazla bilgi için bkz. [Data Lake Store'a Genel Bakış](data-lake-store-overview.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

## <a name="authentication"></a>Kimlik Doğrulaması
Bu makalede, Azure hesabı kimlik bilgilerinizi girmenizi isteyen daha basit bir Data Lake Store kimlik doğrulama yaklaşımı kullanılmaktadır. Data Lake Store hesabına ve dosya sistemine erişim düzeyi bu durumda oturum açmış kullanıcının erişim düzeyine göre yönetilir. Ancak, Data Lake Store kimlik doğrulaması için **son kullanıcı kimlik doğrulaması** veya **hizmetten hizmete kimlik doğrulama** şeklinde diğer yaklaşımlar da mevcuttur. Kimlik doğrulaması gerçekleştirmeyle ilgili yönergeler ve daha fazla bilgi için [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md) veya [Hizmetten hizmete kimlik doğrulaması](data-lake-store-authenticate-using-active-directory.md) bölümlerine göz atın.

## <a name="create-an-azure-data-lake-store-account"></a>Azure Data Lake Store hesabı oluşturma
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

    ![Bir Azure kaynak grubu oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Bir Azure Data Lake Store hesabı oluşturun. Belirttiğiniz ad yalnızca küçük harflerden ve rakamlardan oluşmalıdır.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Store hesabı oluşturma](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")
4. Hesabın başarıyla oluşturulduğunu doğrulayın.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Bu işlemin çıktısı **True** olmalıdır.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Azure Data Lake Store'unuzda dizin yapıları oluşturma
Veri depolamak ve yönetmek için Azure Data Lake Store hesabınızın altında dizin oluşturabilirsiniz.

1. Bir kök dizin belirtin.

        $myrootdir = "/"
2. Belirtilen kökün altında **mynewdirectory** adlı yeni bir dizin oluşturun.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Yeni dizinin başarıyla oluşturulduğunu doğrulayın.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Bu işlemin aşağıdakine benzer bir çıktı göstermesi gerekir:

    ![Dizin Doğrulama](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Azure Data Lake Store'unuza veri yükleme
Verilerinizi Data Lake Store'a doğrudan kök düzeyinde veya hesap içinde oluşturduğunuz bir dizine yüklenecek şekilde yükleyebilirsiniz. Aşağıdaki kod parçacıkları, birtakım örnek verilerin önceki bölümde oluşturduğunuz dizine (**mynewdirectory**) nasıl yükleneceğini göstermektedir.

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

## <a name="performance-guidance-while-using-powershell"></a>PowerShell’i kullanırken performans rehberi

Aşağıda, Data Lake Store ile çalışmak için PowerShell’i kullanırken en iyi performansı elde etmek için ayarlanabilecek en önemli ayarlar sağlanmıştır:

| Özellik            | Varsayılan | Açıklama |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Bu parametre, her bir dosya karşıya yüklenirken veya indirilirken kaç paralel iş parçacığı kullanılacağını seçmenize olanak tanır. Bu sayı dosya başına ayrılabilecek en fazla iş parçacığı sayısını temsil eder, ancak senaryonuza bağlı olarak daha az iş parçacığı elde edebilirsiniz (örneğin, 1 KB boyutlu bir dosyayı karşıya yüklüyorsanız 20 iş parçacığı isteseniz bile bir iş parçacığınız olur).  |
| ConcurrentFileCount | 10      | Bu parametre özellikle klasörlerin karşıya yüklenmesi ve indirilmesi içindir. Bu parametre, karşıya yüklenebilecek veya indirilebilecek eş zamanlı dosya sayısını belirler. Bu sayı, tek seferde karşıya yüklenebilecek veya indirilebilecek en fazla eş zamanlı dosya sayısını temsil eder, ancak senaryonuza bağlı olarak daha az eş zamanlılık elde edebilirsiniz (örneğin, karşıya iki dosya yüklüyorsanız 15 eş zamanlı dosya yükleme isteseniz bile iki yükleme sağlanır). |

**Örnek**

Bu komut, dosya başına 20 iş parçacığı ve 100 eşzamanlı dosya kullanarak dosyaları Azure Data Lake Store’dan kullanıcının yerel sürücüsüne indirir.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a>Bu parametreler için ayarlanacak değerleri nasıl belirlerim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. Adım: Toplam iş parçacığı sayısını belirleyin** - İlk olarak kullanılması gereken toplam iş parçacığı sayısını hesaplayabilirsiniz. Genel bir kural olarak her fiziksel çekirdek için 6 iş parçacığı kullanmalısınız.

        Total thread count = total physical cores * 6

    **Örnek**

    PowerShell komutlarını 16 çekirdekli bir D14 VM’den çalıştırdığınız varsayılmıştır

        Total thread count = 16 cores * 6 = 96 threads


* **2. Adım: PerFileThreadCount değerini hesaplayın**  - Kendi PerFileThreadCount değerimizi dosyaların boyutunu temel olarak hesaplıyoruz. 2,5 GB’den küçük dosyalar için varsayılan değer olan 10 yeterli olduğundan bu parametreyi değiştirmek gerekmez. 2,5 GB'tan büyük dosyalar için 10 iş parçacığını ilk 2,5 GB için bir temel olarak kullanın ve dosya boyutundaki her 256 MB’lık ek artış için 1 iş parçacığı ekleyin. Birçok farklı boyutta dosya içeren bir klasörü kopyalıyorsanız dosyaları benzer dosya boyutları halinde gruplandırmayı göz önünde bulundurun. Dosya boyutlarının benzer olmaması performansın düşmesine neden olabilir. Benzer boyutlu dosyaları gruplandırmanız mümkün değilse PerFileThreadCount değerini en büyük dosya boyutuna göre ayarlamanız gerekir.

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Örnek**

    Boyutu 1 GB ile 10 GB arasında değişen 100 dosyanız olduğunu varsayarsak, denklemde en büyük dosya boyutu olan 10 GB’ı kullanırız ve denklem aşağıdaki gibi görünür.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **3. Adım: ConcurrentFilecount değerini hesaplayın** - Aşağıdaki denklemi temel alıp toplam iş parçacığı sayısı ve PerFileThreadCount değerini kullanarak ConcurrentFileCount değerini hesaplayın.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Örnek**

    Kullandığımız örnek değerler temel alınmıştır

        96 = 40 * ConcurrentFileCount

    Sonuç olarak **ConcurrentFileCount** değeri **2,4** ve bunu **2**’ye yuvarlayabiliriz.

### <a name="further-tuning"></a>Daha fazla ayar

Üzerinde çalışılan dosyaların boyutu çeşitlilik gösterdiğinden daha fazla ayar yapmanız gerekebilir. Dosyaların hepsi veya çoğu 10 GB aralığından büyükse veya bu aralığa daha yakınsa yukarıdaki hesaplama iyi bir sonuç verir. Bunun yerine çoğu dosya küçük olacak şekilde birçok farklı dosya boyutu varsa PerFileThreadCount değerini azaltabilirsiniz. PerFileThreadCount değerini azaltarak ConcurrentFileCount değerini artırabiliriz. Dolayısıyla, dosyalarımızın çoğunun daha küçük (5 GB aralığında) olduğunu varsayarsak hesaplamayı yeniden yapabiliriz:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Bu durumda **ConcurrentFileCount** değeri 96/20 işlemi sonucunda 4.8 olarak bulunur **4**’e yuvarlanır.

Dosya boyutlarınızın dağılımına göre **PerFileThreadCount** değerini artırıp azaltarak bu ayarları değiştirmeye devam edebilirsiniz.

### <a name="limitation"></a>Sınırlama

* **Dosya sayısı ConcurrentFileCount değerinden az**: Karşıya yüklemekte olduğunuz dosyaların sayısı hesapladığınız **ConcurrentFileCount** değerinden azsa **ConcurrentFileCount** değerini dosya sayısına eşit olacak şekilde azaltmanız gerekir. **PerFileThreadCount** değerini artırmak için kalan iş parçacıklarından dilediğinizi kullanabilirsiniz.

* **Çok fazla iş parçacığı**: Kümenizin boyutunu artırmadan iş parçacığı sayısını çok fazla artırırsanız düşük performans riskiyle karşılaşabilirsiniz. CPU’da bağlam değişimi sırasında çekişme sorunları olabilir.

* **Yetersiz eşzamanlılık**: Eşzamanlılık yeterli değilse kümeniz çok küçük olabilir. Eşzamanlılığı artırmak için kümenizdeki düğüm sayısını artırabilirsiniz.

* **Azaltma hataları**: Eşzamanlılığınız çok yüksekse azaltma hataları görebilirsiniz. Azaltma hataları görüyorsanız eşzamanlılığı azaltmanız veya bize ulaşmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)


