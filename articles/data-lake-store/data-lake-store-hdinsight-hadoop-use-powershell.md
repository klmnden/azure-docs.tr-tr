---
title: 'PowerShell: Azure Hdınsight kümesini Data Lake Store eklenti depolama | Microsoft Docs'
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 01/30/2018
ms.author: nitinme
ms.openlocfilehash: 4c08dac95a2d2b52f1a1d28f6933b94ad4db10b7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Data Lake Store (olarak ek depolama alanı) ile bir Hdınsight kümesi oluşturmak için Azure PowerShell'i kullanma

> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell'i kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell kullanarak (için ek depolama alanı)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Azure Data Lake Store ile Hdınsight kümesi yapılandırmak için Azure PowerShell kullanmayı öğrenin **ek depolama alanı olarak**. Varsayılan depolama alanı olarak Azure Data Lake Store ile Hdınsight kümesi oluşturma hakkında yönergeler için bkz: [varsayılan depolama olarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> Azure Data Lake Store’u HDInsight kümesi için ek depolama alanı olarak kullanacaksanız, bunu bu makalede açıklandığı gibi kümeyi oluştururken yapmanız önemle önerilir. Azure Data Lake Store’u mevcut bir HDInsight kümesine ek depolama alanı olarak ekleme, karmaşık ve hatalara yol açabilecek bir işlemdir.
>

Desteklenen küme türleri için Data Lake Store varsayılan depolama veya ek depolama alanı hesabı olarak kullanılabilir. Data Lake Store ek depolama alanı olarak kullanıldığında, kümeler için varsayılan depolama hesabı Azure Storage Blobları (WASB) olmaya devam edecektir ve işlem yapmak istediğiniz veriler bir Data Lake Store hesabında depolanabilir sırada küme ilgili dosyalar (örneğin, günlükleri, vb.) hala varsayılan depolama alanına yazılır. Data Lake Store ek depolama alanı hesabı olarak kullanarak performansı veya depolama birimine kümeden okuma/yazma özelliğini etkilemez.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Hdınsight küme depolaması için Data Lake Store kullanma

Hdınsight Data Lake Store ile kullanmak için bazı önemli noktalar şunlardır:

* Ek depolama alanı Hdınsight sürüm 3.2, 3.4, 3.5 ve 3.6 için kullanılabilir hale Data Lake Store erişimi olan Hdınsight kümeleri oluşturmak için seçeneği.

Data Lake Store ile çalışmaya Hdınsight yapılandırma PowerShell kullanarak, aşağıdaki adımları içerir:

* Bir Azure Data Lake deposu oluşturma
* Data Lake Store için rol tabanlı erişim için kimlik doğrulaması ayarlama
* Data Lake Store için kimlik doğrulaması ile Hdınsight kümesi oluşturma
* Küme üzerinde bir test çalıştırın

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Windows SDK**. [Buradan](https://dev.windows.com/en-us/downloads) yükleyebilirsiniz. Bu bir güvenlik sertifikası oluşturmak için kullanın.
* **Azure Active Directory hizmet asıl**. Bu öğreticideki adımlar, Azure AD hizmet sorumlusu oluşturma konusunda yönergeler sağlar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Azure AD Yöneticiyseniz, bu önkoşulu atlayın ve bu öğreticiyi devam edin.

    **Azure AD Yönetici değilseniz**, bir hizmet sorumlusu oluşturmak için gerekli adımları gerçekleştirmek mümkün olmaz. Data Lake Store ile Hdınsight kümesi oluşturmadan önce böyle bir durumda, Azure AD yöneticinizin önce bir hizmet sorumlusu oluşturmanız gerekir. Ayrıca, hizmet sorumlusu bir sertifika kullanılarak konusunda açıklandığı gibi oluşturulmalıdır [sertifikayla bir hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-azure-data-lake-store"></a>Bir Azure Data Lake deposu oluşturma
Bir Data Lake Store oluşturmak için aşağıdaki adımları izleyin.

1. Masaüstünüzde yeni bir Azure PowerShell penceresi açın ve aşağıdaki kod parçacığını girin. Oturum açmanız istendiğinde, bir abonelik Yöneticisi/sahibi oturum açtığınızdan emin olun:

        # Log in to your Azure account
        Connect-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Benzer bir hata alırsanız `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` Data Lake Store kaynak sağlayıcısını kaydetme, aboneliğinizi Azure Data Lake Store için Güvenilenler listesine değil mümkündür. Bunlar izleyerek, Azure aboneliğinizin Data Lake Store genel önizlemesi için etkinleştirdiğinizden emin olun [yönergeleri](data-lake-store-get-started-portal.md).
   >
   >
2. Azure Data Lake Store hesabı, bir Azure Kaynak Grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Bu gibi bir çıktı görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Azure Data Lake Store hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harf ve sayı içermelidir.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    Aşağıdaki gibi bir çıktı görmeniz gerekir:

        ...
        ProvisioningState           : Succeeded
        State                       : Active
        CreationTime                : 5/5/2017 10:53:56 PM
        EncryptionState             : Enabled
        ...
        LastModifiedTime            : 5/5/2017 10:53:56 PM
        Endpoint                    : hdiadlstore.azuredatalakestore.net
        DefaultGroup                :
        Id                          : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp/providers/Microsoft.DataLakeStore/accounts/hdiadlstore
        Name                        : hdiadlstore
        Type                        : Microsoft.DataLakeStore/accounts
        Location                    : East US 2
        Tags                        : {}

5. Bazı örnek veriler için Azure Data Lake karşıya yükleyin. Bu bu makalenin sonraki bölümlerinde verileri bir Hdınsight kümeden erişilebilir olduğunu doğrulamak için kullanacağız. Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Data Lake Store için rol tabanlı erişim için kimlik doğrulaması ayarlama

Her Azure aboneliği bir Azure Active Directory ile ilişkilidir. Önce kullanıcılar Azure portalında veya Azure Kaynak Yöneticisi API'si kullanılarak aboneliğin kaynaklarını erişen ve Hizmetleri bu Azure Active Directory ile kimlik doğrulaması gerekir. Erişim Azure abonelikleri ve Hizmetleri için bir Azure kaynak üzerinde uygun rol atama tarafından verilir.  Hizmetler için bir hizmet sorumlusu hizmeti Azure Active Directory (AAD) de tanımlar. Bu bölümde, Hdınsight, bir Azure kaynağı (daha önce oluşturduğunuz Azure Data Lake Store hesabı) erişimi gibi bir uygulama hizmet vermek verilmektedir uygulama için bir hizmet sorumlusu oluşturarak ve rolleri için Azure PowerShell atama.

Active Directory kimlik doğrulaması için Azure Data Lake ayarlamak için aşağıdaki görevleri gerçekleştirmeniz gerekir.

* Otomatik olarak imzalanan sertifika oluşturma
* Uygulama Azure Active Directory ve bir hizmet sorumlusu oluşturma

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Olduğundan emin olun [Windows SDK](https://dev.windows.com/en-us/downloads) bu bölümdeki adımlar ile devam etmeden önce yüklü. Ayrıca bir dizin gibi oluşturduğunuz gerekir **C:\mycertdir**, sertifika oluşturulacağı.

1. PowerShell penceresinden Windows SDK'yı yüklediğiniz konuma gidin (genellikle `C:\Program Files (x86)\Windows Kits\10\bin\x86` ve [MakeCert] [ makecert] otomatik olarak imzalanan bir sertifika ve özel bir anahtar oluşturmak için yardımcı programı. Aşağıdaki komutları kullanın.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar parolası girmeniz istenir. Komut başarıyla, görmeniz gerekir yürütüldükten sonra bir **CertFile.cer** ve **mykey.pvk** , belirtilen sertifika dizininde.
2. Kullanım [Pvk2Pfx] [ pvk2pfx] MakeCert oluşturulan .pvk ve .cer dosya bir .pfx dosyasına dönüştürmek için yardımcı programı. Aşağıdaki komutu çalıştırın.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    İstendiğinde daha önce belirttiğiniz özel anahtar parolası girin. İçin belirttiğiniz değer **-SAS** .pfx dosyasıyla ilişkili parolayı bir parametredir. Komut başarıyla tamamlandıktan sonra belirtilen sertifika dizininde CertFile.pfx görmeniz gerekir.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Bir Azure Active Directory ve bir hizmet sorumlusu oluşturma

Bu bölümde, bir hizmet sorumlusu için bir Azure Active Directory uygulaması oluşturma, hizmet sorumlusu rol atamak ve bir sertifika sağlayarak hizmet sorumlusunun kimliğini doğrulamak için adımları uygulayın. Azure Active Directory'de bir uygulama oluşturmak için aşağıdaki komutları çalıştırın.

1. Aşağıdaki cmdlet'leri PowerShell konsol penceresine yapıştırın. İçin belirttiğiniz değer emin olun **- DisplayName** özelliği benzersizdir. Ayrıca, değerlerini **- giriş sayfası** ve **- IdentiferUris** yer tutucu değerlerini olan ve olmayan doğrulanır.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host -Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Uygulama kimliğini kullanarak bir hizmet sorumlusu oluşturma

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Data Lake Store klasörü ve Hdınsight küme erişecek dosya için hizmet asıl erişim verin. Aşağıdaki kod parçacığını (kopyaladığınız örnek veri dosyası) Data Lake Store hesabını kök erişim sağlar ve dosya.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Hdınsight Linux kümesi Data Lake Store ile ek depolama alanı olarak oluşturun.

Bu bölümde, bir Hdınsight Hadoop Linux kümesi Data Lake Store ile ek depolama alanı olarak oluşturuyoruz. Bu sürüm için Hdınsight kümesi ve Data Lake Store aynı konumda olması gerekir.

1. Abonelik Kiracı kimliği alma Başlat Daha sonra ihtiyacınız olacak.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. Bu sürüm için Hadoop kümesi için Data Lake Store yalnızca ek depolama alanı küme için kullanılabilir. Varsayılan storage (WASB) Azure storage bloblarında olmaya devam eder. Bu nedenle, ilk küme için gerekli depolama kapsayıcıları ve depolama hesabı oluşturacağız.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Hdınsight kümesi oluşturun. Aşağıdaki cmdlet'leri kullanın.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Cmdlet başarıyla tamamlandıktan sonra küme ayrıntıları listeleyen bir çıktı görmeniz gerekir.


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Data Lake Store kullanacak şekilde Hdınsight kümesinde test işleri çalıştırma
Hdınsight kümesi yapılandırdıktan sonra Hdınsight kümesi Data Lake Store erişebilmesini test etmek için kümede test işleri çalıştırabilirsiniz. Bunu yapmak için Data Lake Store için daha önce yüklenen örnek verileri kullanarak bir tablo oluşturur bir örnek Hive işi çalışır.

Bu bölümde oluşturulan ve Çalıştır Hdınsight Linux küme SSH olacak örnek Hive sorgusu.

* SSH Windows istemciye kümesine kullanıyorsanız bkz [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* SSH Linux istemciye kümesine kullanıyorsanız, bkz: [Linux Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Bağlantı kurulduktan sonra aşağıdaki komutu kullanarak Hive CLI başlatın:

        hive
2. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki ifadeleri **taşıtlardan** Data Lake Store'da örnek verileri kullanarak:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a>HDFS komutları kullanarak erişim Data Lake Store
Hdınsight kümesi Data Lake Store kullanacak şekilde yapılandırdıktan sonra deposuna erişim için HDFS kabuk komutlarını kullanabilirsiniz.

Bu bölümde oluşturulan ve HDFS komutları çalıştırmak Hdınsight Linux kümesi SSH olur.

* SSH Windows istemciye kümesine kullanıyorsanız bkz [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* SSH Linux istemciye kümesine kullanıyorsanız, bkz: [Linux Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Bağlantı kurulduktan sonra Data Lake Store'da dosyaları listelemek için aşağıdaki HDFS filesystem komutunu kullanın.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Bu Data Lake Store için daha önce karşıya dosya listelenmelidir.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Aynı zamanda `hdfs dfs -put` bazı dosyalar için Data Lake Store karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure Hdınsight kümeleri ile kullanım Data Lake Store](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Portal: Data Lake Store kullanmak için bir Hdınsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
