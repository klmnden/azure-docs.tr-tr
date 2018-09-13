---
title: Varsayılan depolama alanı olarak Data Lake Store ile PowerShell'i kullanarak HDInsight kümeleri oluşturma | Microsoft Docs
description: Oluşturma ve Azure Data Lake Store ile HDInsight kümeleri kullanmak için Azure PowerShell'i kullanma
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: da48602bddc61b0df93cfdda613219381aed1e8c
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651271"
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>Varsayılan depolama alanı olarak Data Lake Store ile PowerShell'i kullanarak HDInsight kümeleri oluşturma

> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanın](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Azure PowerShell, Azure HDInsight kümeleri varsayılan depolama alanı olarak Azure Data Lake Store ile yapılandırmak için kullanmayı öğrenin. Ek depolama alanı olarak Data Lake Store ile HDInsight kümesi oluşturma ile ilgili yönergeler için bkz: [ek depolama alanı olarak Data Lake Store ile HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-powershell.md).

Data Lake Store ile HDInsight'ı kullanmaya yönelik bazı önemli noktalar şunlardır:

* Varsayılan depolama alanı olarak Data Lake Store erişimi olan HDInsight kümeleri oluşturma seçeneğini, HDInsight sürüm 3.5 ve 3.6 için kullanılabilir.

* HDInsight'ı oluşturmak için bu seçeneği varsayılan depolama alanı olduğundan bu erişim ile Data Lake Store için kümeleri *kullanılamıyor* HDInsight Premium kümeleri için.

PowerShell kullanarak Data Lake Store ile çalışmak için HDInsight'ı yapılandırmak için sonraki beş bölümlerdeki yönergeleri izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri karşıladığından emin olun:

* **Bir Azure aboneliğine**: Git [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**: bkz [PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).
* **Windows Yazılım Geliştirme Seti (SDK)**: Windows SDK'sını yüklemek için Git [Windows 10 için indirmeler ve Araçlar](https://dev.windows.com/downloads). SDK, bir güvenlik sertifikası oluşturmak için kullanılır.
* **Azure Active Directory Hizmet sorumlusu**: Bu öğretici, Azure Active Directory (Azure AD) bir hizmet sorumlusu oluşturmak açıklar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir yöneticiyseniz, bu önkoşulu atlayabilirsiniz ve öğreticiyle devam edin.

    >[!NOTE]
    >Yalnızca bir Azure AD Yöneticisi olduğunuz bir hizmet sorumlusu oluşturabilirsiniz. Data Lake Store ile HDInsight kümesi oluşturmadan önce Azure AD yöneticinizin bir hizmet sorumlusu oluşturmanız gerekir. Bölümünde anlatıldığı gibi bir sertifika ile hizmet sorumlusu oluşturulmalıdır [sertifika ile hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma

Bir Data Lake Store hesabı oluşturmak için aşağıdakileri yapın:

1. Masaüstünüzde bir PowerShell penceresi açın ve ardından aşağıdaki kod parçacıkları girin. Sorulduğunda, oturum açmak için bir abonelik yöneticileri veya sahipleri oturum açın. 

        # Sign in to your Azure account
        Connect-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Data Lake Store kaynak sağlayıcısını kaydedin ve benzer şekilde bir hatayla karşılaştıysanız `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, aboneliğinizin Data Lake Store için beyaz listeye olmayabilir. Data Lake Store genel önizlemesi için Azure aboneliğinizi etkinleştirmek için yönergeleri izleyin. [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).
    >

2. Bir Data Lake Store hesabı, bir Azure kaynak grubu ile ilişkilidir. Bir kaynak grubu oluşturarak başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Bu gibi bir çıktı görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Data Lake Store hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harf ve rakam içermelidir.

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

4. Varsayılan depolama alanı olarak Data Lake Store kullanarak küme oluşturma sırasında kopyalanır kümeye özgü dosyaları için bir kök yol belirtmenizi gerektirir. Olan bir kök yolu oluşturmak için **/kümeleri/hdiadlcluster** kod parçacığında, aşağıdaki cmdlet'leri kullanın:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Data Lake Store için rol tabanlı erişim için kimlik doğrulaması ayarlama
Her Azure aboneliği bir Azure AD varlıkla ilişkilidir. İlk kullanıcılar ve Azure portalı veya Azure Resource Manager API'si kullanarak abonelik kaynaklarına erişim Hizmetleri Azure AD ile kimlik doğrulaması gerekir. Erişim Azure abonelik ve Hizmetleri için bir Azure kaynak üzerinde uygun role atayarak verilir. Hizmetler için bir hizmet sorumlusu, Azure AD'de hizmet belirtir.

Bu bölümde, bir Azure kaynağı (daha önce oluşturduğunuz Data Lake Store hesabı) erişimi olan HDInsight gibi bir uygulama hizmeti vermek gösterilmektedir. Bunun için bir hizmet asıl uygulama ve PowerShell aracılığıyla ona atama rolleri oluşturarak.

Azure Data Lake için Active Directory kimlik doğrulamasını ayarlamak için aşağıdaki iki bölümü görevleri gerçekleştirin.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Olduğundan emin olun [Windows SDK'sı](https://dev.windows.com/en-us/downloads) bu bölümdeki adımlar ile devam etmeden önce yüklü. Siz de bir dizin gibi oluşturmuş olmalısınız *C:\mycertdir*, sertifikayı oluşturduğunuz yerdir.

1. PowerShell penceresinden Windows SDK'yı yüklediğiniz konuma gidin (genellikle *C:\Program Files (x86) \Windows Kits\10\bin\x86*) ve [MakeCert] [ makecert] kendinden imzalı bir sertifika ve özel bir anahtar oluşturmak için yardımcı program. Aşağıdaki komutları kullanın:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar parolası girmeniz istenir. Komut başarılı bir şekilde yürütüldükten sonra görmelisiniz **CertFile.cer** ve **mykey.pvk** belirttiğiniz sertifika dizine.
2. Kullanım [Pvk2Pfx] [ pvk2pfx] MakeCert oluşturulan .pvk ve .cer dosya bir .pfx dosyasına dönüştürmek için yardımcı program. Şu komutu çalıştırın:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    İstendiğinde, daha önce belirttiğiniz özel anahtar parolası girin. İçin belirttiğiniz değer **-SAS** .pfx dosyasıyla ilişkili parolayı parametredir. Komut başarıyla tamamlandıktan sonra da görmelisiniz bir **CertFile.pfx** belirttiğiniz sertifika dizine.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Azure AD'yi oluşturmak ve bir hizmet sorumlusu
Bu bölümde, Azure AD uygulaması için hizmet sorumlusu oluşturma, hizmet sorumlusuna bir rol atamak ve bir sertifika sağlayarak hizmet sorumlusu kimlik doğrulaması. Azure AD'de bir uygulama oluşturmak için aşağıdaki komutları çalıştırın:

1. Aşağıdaki cmdlet'leri PowerShell konsol penceresine yapıştırın. Değer, için belirttiğinizden emin olun **- DisplayName** benzersiz özellik. Değerleri **giriş sayfası -** ve **- IdentiferUris** yer tutucu değerleri olan ve olmayan doğrulanır.

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
3. Data Lake Store kök ve daha önce belirttiğiniz kök yolundaki tüm klasörler için hizmet sorumlusu erişimi verin. Aşağıdaki cmdlet'leri kullanın:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a>Bir HDInsight Linux kümesi, varsayılan depolama alanı olarak Data Lake Store ile oluşturma

Bu bölümde, bir HDInsight Hadoop Linux kümesi varsayılan depolama alanı olarak Data Lake Store ile oluşturun. Bu sürüm için Data Lake Store ve HDInsight kümesi aynı konumda olması gerekir.

1. Abonelik Kiracı Kimliğini almak ve daha sonra kullanmak üzere saklayın.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Aşağıdaki cmdlet'leri kullanarak HDInsight kümesi oluşturun:

        # Set these variables

        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                       # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = "<unique cluster name>"
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.6" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Cmdlet başarıyla tamamlandıktan sonra küme ayrıntıları listeleyen bir çıktı görmeniz gerekir.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a>Data Lake Store kullanmak için HDInsight kümesinde test işleri çalıştırma
Bir HDInsight kümesi yapılandırdıktan sonra Data Lake Store erişebildiğinden emin olmak için test işleri çalıştırabilirsiniz. Bunu yapmak için Data Lake Store, kullanılabilir zaten örnek verileri kullanan bir tablo oluşturmak için bir örnek Hive işi Çalıştır  *<cluster root>/example/data/sample.log*.

Bu bölümde, oluşturduğunuz HDInsight Linux kümesine güvenli Kabuk (SSH) bağlantısı ve ardından, bir örnek Hive sorgusu çalıştırın.

* Bir SSH bağlantısı kümesine bir Windows istemci kullanıyorsanız bkz [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Bir SSH bağlantısı kümesinde Linux istemci kullanıyorsanız bkz [linux'taki HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Bir bağlantı yaptıktan sonra aşağıdaki komutu kullanarak Hive komut satırı arabirimi (CLI) başlatın:

        hive
2. Adlı yeni bir tablo oluşturmak için aşağıdaki deyimleri girmek için CLI kullanma **taşıtlardan** örnek verileri kullanarak Data Lake Store içinde:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Sorgu çıkışının SSH konsolunda görmeniz gerekir.

    >[!NOTE]
    >Önceki bir CREATE TABLE komut örnek veri yolu `adl:///example/data/`burada `adl:///` küme kökü. Aşağıdaki örnek komut Bu öğreticide, belirtilen küme kök `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Daha kısa bir alternatif kullanabilir veya küme kökü tam yolunu belirtin.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>HDFS komutlarını kullanarak Data Lake Store erişimi
Data Lake Store kullanmak için HDInsight küme yapılandırdıktan sonra Hadoop dağıtılmış dosya sistemi (HDFS) Kabuk komutları mağazaya erişmek için kullanabilirsiniz.

Bu bölümde, oluşturduğunuz HDInsight Linux kümesi içine SSH bağlantısı olun ve ardından, HDFS komutları çalıştırın.

* Bir SSH bağlantısı kümesine bir Windows istemci kullanıyorsanız bkz [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Bir SSH bağlantısı kümesinde Linux istemci kullanıyorsanız bkz [linux'taki HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Bir bağlantı yaptıktan sonra aşağıdaki HDFS dosya sisteminin komutu kullanarak Data Lake Store dosyaları listeler.

    hdfs dfs -ls adl:///

Ayrıca `hdfs dfs -put` Data Lake Store için bazı dosyaları karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure HDInsight kümeleriyle kullanımı Data Lake Store](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Azure portalı: Data Lake Store kullanmak için bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
