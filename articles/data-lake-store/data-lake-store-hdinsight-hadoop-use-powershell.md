---
title: 'PowerShell: Azure Data Lake depolama Gen1 ek depolama alanı olarak Azure HDInsight kümesiyle | Microsoft Docs'
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: f78ad8d58bb1bc760a31b792b44a4a39ed25e1f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66161389"
---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-as-additional-storage"></a>Azure Data Lake depolama Gen1 (olarak, ek depolama alanı) ile bir HDInsight kümesi oluşturmak için Azure PowerShell'i kullanma

> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanma (varsayılan depolama için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Azure Data Lake depolama Gen1 ile bir HDInsight kümesini yapılandırmak için Azure PowerShell kullanmayı öğrenirsiniz **ek depolama alanı olarak**. Varsayılan depolama alanı olarak Data Lake depolama Gen1 ile bir HDInsight kümesi oluşturma hakkında yönergeler için bkz: [ile varsayılan depolama alanı olarak Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

> [!NOTE]
> HDInsight kümesi için ek depolama alanı olarak Data Lake depolama Gen1 kullanılacak kullanacaksanız, bu makalede açıklandığı gibi kümeyi oluştururken yapmanız önemle önerilir. Data Lake depolama Gen1 var olan bir HDInsight için ek depolama alanı olarak ekleyerek bir karmaşık kümedir işlem ve hata potansiyeli.
>

Desteklenen küme türleri için varsayılan depolama alanı ya da ek depolama alanı hesabı Data Lake depolama Gen1 kullanılabilir. Ek depolama alanı olarak Data Lake depolama Gen1 kullanıldığında, kümeler için varsayılan depolama hesabı Azure depolama BLOB'ları (WASB) devam edebilir ve kümeyle ilişkili dosyalar (örneğin, günlük, vb.) hala varsayılan depolama alanı, istediğiniz veriler yazılır işlem, bir Data Lake depolama Gen1 hesabında depolanabilir. Data Lake depolama Gen1 ek depolama hesabı kullanarak, performans veya depolama kümesinden okuma/yazma olanağı etkilemez.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>HDInsight küme depolaması için Data Lake depolama Gen1 kullanma

HDInsight kullanarak Data Lake depolama Gen1 ile ilgili bazı önemli noktalar şunlardır:

* Ek depolama, HDInsight sürüm 3.2, 3.4, 3.5 ve 3.6 için kullanılabilir olarak Data Lake depolama Gen1 erişimi olan HDInsight kümeleri oluşturmak için seçeneği.

HDInsight, Data Lake depolama Gen1 ile çalışacak şekilde yapılandırma PowerShell kullanarak aşağıdaki adımları içerir:

* Bir Data Lake depolama Gen1 hesabı oluşturun
* Data Lake depolama Gen1 için rol tabanlı erişim için kimlik doğrulaması ayarlama
* Data Lake depolama Gen1 kimlik doğrulaması ile HDInsight kümesi oluşturma
* Test işi küme üzerinde çalıştırılır.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Windows SDK'sı**. [Buradan](https://dev.windows.com/en-us/downloads) yükleyebilirsiniz. Bir güvenlik sertifikası oluşturmak için bunu kullanın.
* **Azure Active Directory Hizmet sorumlusu**. Bu öğreticideki adımlar, Azure AD'de hizmet sorumlusu oluşturma hakkında yönergeler sağlar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir Azure AD Yöneticisi olarak, bu önkoşulu atlayın ve öğreticiyle devam edin.

    **Bir Azure AD yöneticisi değilseniz**, bir hizmet sorumlusu oluşturmak için gerekli adımları gerçekleştirmek mümkün olmayacaktır. Bir HDInsight kümesi ile Data Lake depolama Gen1 oluşturabilmeniz için önce böyle bir durumda, Azure AD yöneticinizin öncelikle bir hizmet sorumlusu oluşturmanız gerekir. Ayrıca, hizmet sorumlusunun bir sertifika kullanmayı anlatıldığı gibi oluşturulmalıdır [sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-a-data-lake-storage-gen1-account"></a>Bir Data Lake depolama Gen1 hesabı oluşturun
Bir Data Lake depolama Gen1 hesabı oluşturmak için aşağıdaki adımları izleyin.

1. Masaüstünüzde yeni bir Azure PowerShell penceresi açın ve aşağıdaki kod parçacığını girin. Oturum açmanız istendiğinde, bir abonelik Yöneticisi/sahibi oturum emin olun:

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Benzer bir hata alırsanız `Register-AzResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` Data Lake depolama Gen1 kaynak sağlayıcısı kaydedildiğinde, aboneliğinizin Data Lake depolama Gen1 için izin verilenler listesinde değil mümkündür. Bu takip ederek Azure aboneliğiniz için Data Lake depolama Gen1 etkinleştirdiğinizden emin olun [yönergeleri](data-lake-store-get-started-portal.md).
   >
   >
2. Bir Data Lake depolama Gen1 hesabı bir Azure kaynak grubu ile ilişkilidir. Azure Kaynak Grubu oluşturma işlemiyle başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    Bu gibi bir çıktı görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Data Lake depolama Gen1 hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harf ve rakam içermelidir.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

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

5. Bazı örnek veriler için Data Lake depolama Gen1 karşıya yükleyin. Bu makalenin sonraki bölümlerinde verilerin bir HDInsight kümesinden erişilebilir olduğunu doğrulamak için kullanacağız. Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

        $myrootdir = "/"
        Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-storage-gen1"></a>Data Lake depolama Gen1 için rol tabanlı erişim için kimlik doğrulaması ayarlama

Her Azure aboneliği bir Azure Active Directory ile ilişkilidir. İlk kullanıcı ve erişim Azure portalı veya Azure Resource Manager API'si kullanarak aboneliğin kaynaklarını hizmet, Azure Active Directory ile kimlik doğrulaması gerekir. Erişim Azure abonelik ve Hizmetleri için bir Azure kaynak üzerinde uygun role atayarak verilir.  Hizmetler için bir hizmet sorumlusu, Azure Active Directory (AAD) hizmet belirtir. Bu bölümde, bir Azure kaynağı (daha önce oluşturduğunuz Data Lake depolama Gen1 hesabı) erişimi olan HDInsight gibi bir uygulama hizmeti vermek verilmektedir uygulama için bir hizmet sorumlusu oluşturup rolü için Azure PowerShell atama.

Data Lake depolama Gen1 için Active Directory kimlik doğrulamasını ayarlamak için aşağıdaki görevleri gerçekleştirmeniz gerekir.

* Otomatik olarak imzalanan sertifika oluşturma
* Bir uygulamayı Azure Active Directory ve hizmet sorumlusu oluşturma

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Olduğundan emin olun [Windows SDK'sı](https://dev.windows.com/en-us/downloads) bu bölümdeki adımlar ile devam etmeden önce yüklü. Siz de bir dizin gibi oluşturmuş olmalısınız **C:\mycertdir**, sertifika oluşturulacağı.

1. PowerShell penceresinden Windows SDK'yı yüklediğiniz konuma gidin (genellikle `C:\Program Files (x86)\Windows Kits\10\bin\x86` ve [MakeCert] [ makecert] yardımcı programını kullanarak otomatik olarak imzalanan bir sertifika ve özel anahtar oluşturun. Aşağıdaki komutları kullanın.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar parolası girmeniz istenir. Komut başarıyla, görmelisiniz yürütüldükten sonra bir **CertFile.cer** ve **mykey.pvk** belirttiğiniz sertifika dizinde.
2. Kullanım [Pvk2Pfx] [ pvk2pfx] MakeCert oluşturulan .pvk ve .cer dosya bir .pfx dosyasına dönüştürmek için yardımcı program. Aşağıdaki komutu çalıştırın.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    İstendiğinde daha önce belirttiğiniz özel anahtar parolası girin. İçin belirttiğiniz değer **-SAS** .pfx dosyasıyla ilişkili parolayı parametredir. Komut başarıyla tamamlandıktan sonra belirtilen sertifika dizindeki bir CertFile.pfx da görmeniz gerekir.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Azure Active Directory ve hizmet sorumlusu oluşturma

Bu bölümde, bir hizmet sorumlusu için bir Azure Active Directory uygulaması oluşturma, hizmet sorumlusuna bir rol atamak ve bir sertifika sağlayarak hizmet sorumlusu kimlik doğrulaması için adımları uygulayın. Azure Active Directory'de uygulama oluşturmak için aşağıdaki komutları çalıştırın.

1. Aşağıdaki cmdlet'leri PowerShell konsol penceresine yapıştırın. İçin belirttiğiniz değer emin **- DisplayName** benzersiz özellik. Ayrıca, değerleri **giriş sayfası -** ve **- IdentiferUris** yer tutucu değerleri olan ve olmayan doğrulanır.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host -Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Uygulama kimliğini kullanarak bir hizmet sorumlusu oluşturma

        $servicePrincipal = New-AzADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Data Lake depolama Gen1 klasörünü ve HDInsight kümesinden erişecek dosyasını hizmet sorumlusu erişimi verin. Aşağıdaki kod parçacığında Data Lake depolama Gen1 hesabının (kopyaladığınız örnek veri dosyasını) kök erişim sağlar ve dosya.

        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path / -AceType User -Id $objectId -Permissions All
        Set-AzDataLakeStoreItemAclEntry -AccountName $dataLakeStorageGen1Name -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Bir HDInsight Linux kümesi, ek depolama alanı olarak Data Lake depolama Gen1 ile oluşturma

Bu bölümde, bir HDInsight Hadoop Linux kümesi ile Data Lake depolama Gen1 ek depolama alanı olarak oluştururuz. Bu sürümde, HDInsight kümesi ve Data Lake depolama Gen1 hesabı aynı konumda olmalıdır.

1. Abonelik Kiracı kimliğini alma Başlat Daha sonra gerekecektir.

        $tenantID = (Get-AzContext).Tenant.TenantId
2. Bu sürüm için bir Hadoop kümesi için Data Lake depolama Gen1 yalnızca ek depolama alanı olarak küme için kullanılabilir. Varsayılan depolama alanı Azure depolama BLOB'ları (WASB) görünmeye devam edecektir. Bu nedenle, ilk küme için gerekli depolama kapsayıcıları ve depolama hesabı oluşturacağız.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAccountName>"   # Provide a Storage account name

        New-AzStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzStorageContainer -Name $containerName -Context $destContext
3. HDInsight kümesi oluşturun. Aşağıdaki cmdlet'leri kullanın.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Cmdlet başarıyla tamamlandıktan sonra küme ayrıntıları listeleyen bir çıktı görmeniz gerekir.


## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-storage-gen1-account"></a>Data Lake depolama Gen1 hesabı kullanmak için HDInsight kümesinde test işleri çalıştırma
Bir HDInsight kümesi yapılandırdıktan sonra kümede HDInsight kümesini Data Lake depolama Gen1 erişebilirsiniz test etmek için test işleri çalıştırabilirsiniz. Bunu yapmak için daha önce Data Lake depolama Gen1 hesabınıza yüklediğiniz örnek verileri kullanarak bir tablo oluşturur bir örnek Hive işi çalıştıracağız.

Bu bölümde oluşturup çalıştırdınız HDInsight Linux kümesi SSH olacak bir örnek Hive sorgusu.

* SSH kümesine bir Windows istemci kullanıyorsanız bkz [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* SSH istemcisi Linux kümesinde kullanıyorsanız bkz [linux'taki HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Bağlandıktan sonra aşağıdaki komutu kullanarak Hive CLI'yı başlatın:

        hive
2. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki deyimleri **taşıtlardan** örnek verileri kullanarak Data Lake depolama Gen1 içinde:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
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

## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>Data Lake depolama Gen1 erişim HDFS komutları kullanma
Data Lake depolama Gen1 kullanmak için HDInsight küme yapılandırıldıktan sonra HDFS Kabuk komutları mağazaya erişmek için kullanabilirsiniz.

Bu bölümde oluşturduğunuz ve HDFS komutları çalıştırmak HDInsight Linux kümesi SSH olur.

* SSH kümesine bir Windows istemci kullanıyorsanız bkz [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* SSH istemcisi Linux kümesinde kullanıyorsanız bkz [linux'taki HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

Bağlantı kurulduktan sonra Data Lake depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki HDFS dosya sistemi komutu kullanın.

    hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/

Bu Data Lake depolama Gen1 için daha önce yüklediğiniz dosyanın listelemelisiniz.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder

Ayrıca `hdfs dfs -put` bazı dosyalar için Data Lake depolama Gen1 karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.

## <a name="see-also"></a>Ayrıca Bkz.
* [Data Lake depolama Gen1 Azure HDInsight kümeleri ile kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Portalı: Data Lake depolama Gen1 kullanılacak bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
