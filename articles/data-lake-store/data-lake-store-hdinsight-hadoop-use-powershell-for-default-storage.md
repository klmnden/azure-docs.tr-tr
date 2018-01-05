---
title: "Varsayılan depolama alanı olarak Data Lake Store ile PowerShell kullanarak Hdınsight kümeleri oluşturma | Microsoft Docs"
description: "Oluşturma ve Hdınsight kümeleri Azure Data Lake Store ile kullanmak için Azure PowerShell kullanın"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 2f1793c2de2b68a8b155ada73044c6bc36882612
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>Varsayılan depolama alanı olarak Data Lake Store ile PowerShell kullanarak Hdınsight kümeleri oluşturma
> [!div class="op_single_selector"]
> * [Azure portal’ı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell (için ek depolama alanı) kullanın](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanın](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Varsayılan depolama alanı olarak Azure Data Lake Store ile Azure Hdınsight kümeleri yapılandırmak için Azure PowerShell kullanmayı öğrenin. Ek depolama alanı olarak Data Lake Store ile bir Hdınsight kümesi oluşturma ile ilgili yönergeler için bkz: [ek depolama alanı olarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-powershell.md).

Hdınsight Data Lake Store ile kullanmak için bazı önemli noktalar şunlardır:

* Data Lake Store varsayılan depolama erişim Hdınsight kümeleri oluşturma seçeneğini Hdınsight sürüm 3.5 ve 3.6 için kullanılabilir.

* Hdınsight oluşturma seçeneğini varsayılan depolama olduğundan bu erişim ile Data Lake Store'a kümeleri *kullanılamaz* Hdınsight Premium kümeleri için.

PowerShell kullanarak Data Lake Store ile çalışmak üzere Hdınsight yapılandırmak için sonraki beş bölümler'ndaki yönergeleri izleyin.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdaki gereksinimleri karşıladığından emin olun:

* **Bir Azure aboneliği**: Git [alma Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya büyük**: bkz [PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).
* **Windows Yazılım Geliştirme Seti (SDK)**: Windows SDK'sını yüklemek için Git [indirir ve Windows 10 için Araçlar](https://dev.windows.com/en-us/downloads). SDK'sı bir güvenlik sertifikası oluşturmak için kullanılır.
* **Azure Active Directory hizmet asıl**: Bu öğreticide, Azure Active Directory (Azure AD) bir hizmet sorumlusu oluşturmak açıklar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir yöneticiyseniz, bu önkoşulu atlamak ve bu öğreticiyi devam edin.

    >[!NOTE]
    >Yalnızca, Azure AD yöneticisiyseniz, bir hizmet asıl oluşturabilirsiniz. Azure AD yöneticinizin Data Lake Store ile Hdınsight kümesi oluşturmadan önce bir hizmet asıl oluşturmanız gerekir. Bölümünde açıklandığı gibi bir sertifika ile hizmet sorumlusu oluşturulmalıdır [sertifikayla bir hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).
    >

## <a name="create-a-data-lake-store-account"></a>Data Lake Store hesabı oluşturma
Bir Data Lake Store hesabı oluşturmak için aşağıdakileri yapın:

1. Masaüstünüzde bir PowerShell penceresi açın ve aşağıdaki kod parçacıkları girin. Sorulduğunda, oturum açmak için bir abonelik yöneticileri veya sahipleri oturum açın. 

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Data Lake Store kaynak sağlayıcısı kaydetme ve benzer bir hata alırsanız `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid`, aboneliğinizin Data Lake Store için Güvenilenler listesine olmayabilir. Data Lake Store genel önizlemesi için Azure aboneliğinizi etkinleştirmek için yönergeleri izleyin [Azure portalını kullanarak Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md).
    >

2. Bir Azure kaynak grubu ile ilişkili bir Data Lake Store hesabıdır. Bir kaynak grubu oluşturarak başlayın.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    Bu gibi bir çıktı görmeniz gerekir:

        ResourceGroupName : hdiadlgrp
        Location          : eastus2
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/<subscription-id>/resourceGroups/hdiadlgrp

3. Bir Data Lake Store hesabı oluşturun. Belirttiğiniz hesap adı yalnızca küçük harf ve sayı içermelidir.

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

4. Data Lake Store varsayılan depolama alanı olarak kullanarak kümeye özgü dosyaları küme oluşturma sırasında kopyalanan sanal kök yolu belirtmenizi gerektirir. Olan bir kök yolu oluşturmak için **/kümeleri/hdiadlcluster** kod parçacığında, aşağıdaki cmdlet'leri kullanın:

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Data Lake Store için rol tabanlı erişim için kimlik doğrulaması ayarlama
Her Azure aboneliği bir Azure AD varlıkla ilişkilidir. İlk kullanıcılar ve Azure portalında veya Azure Resource Manager API'sini kullanarak abonelik kaynaklarına hizmetler Azure AD ile kimlik doğrulaması gerekir. Erişim Azure abonelikleri ve Hizmetleri için bir Azure kaynak üzerinde uygun rol atama tarafından verilir. Hizmetler için bir hizmet sorumlusu Azure AD'de hizmeti tanımlar.

Bu bölümde, Hdınsight, bir Azure kaynağı (daha önce oluşturduğunuz Data Lake Store hesabı) erişimi gibi bir uygulama hizmet vermek göstermektedir. Bunun için bir hizmet uygulama ve PowerShell aracılığıyla ona atama rolleri için asıl oluşturarak.

Active Directory kimlik doğrulaması için Azure Data Lake ayarlamak için aşağıdaki iki bölümde görevlerini gerçekleştirin.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma
Olduğundan emin olun [Windows SDK](https://dev.windows.com/en-us/downloads) bu bölümdeki adımlar ile devam etmeden önce yüklü. Ayrıca bir dizin gibi oluşturduğunuz gerekir *C:\mycertdir*burada sertifika oluşturun.

1. PowerShell penceresinden Windows SDK'yı yüklediğiniz konuma gidin (genellikle *C:\Program Files (x86) \Windows Kits\10\bin\x86*) ve [MakeCert] [ makecert] kendinden imzalı bir sertifika ve özel bir anahtar oluşturmak için yardımcı programı. Aşağıdaki komutları kullanın:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Özel anahtar parolası girmeniz istenir. Komut başarılı bir şekilde yürütüldükten sonra görmelisiniz **CertFile.cer** ve **mykey.pvk** , belirtilen sertifika dizininde.
2. Kullanım [Pvk2Pfx] [ pvk2pfx] MakeCert oluşturulan .pvk ve .cer dosya bir .pfx dosyasına dönüştürmek için yardımcı programı. Şu komutu çalıştırın:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    İstendiğinde, daha önce belirtilen özel anahtar parolası girin. İçin belirttiğiniz değer **-SAS** .pfx dosyasıyla ilişkili parolayı bir parametredir. Komut başarıyla tamamlandıktan sonra da görmeniz gerekir bir **CertFile.pfx** , belirtilen sertifika dizininde.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Azure AD oluşturmak ve bir hizmet sorumlusu
Bu bölümde, Azure AD uygulaması için bir hizmet sorumlusu oluşturmak, hizmet sorumlusu rol atama ve hizmet sorumlusu bir sertifika sağlayarak kimlik doğrulaması. Azure AD'de bir uygulama oluşturmak için aşağıdaki komutları çalıştırın:

1. Aşağıdaki cmdlet'leri PowerShell konsol penceresine yapıştırın. Değer, için belirttiğinizden emin olun **- DisplayName** özelliği benzersizdir. Değerleri **- giriş sayfası** ve **- IdentiferUris** yer tutucu değerlerini olan ve olmayan doğrulanır.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

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
3. Hizmet asıl veri Gölü deposu kök ve kök yolundaki daha önce belirtilen tüm klasörlere erişim. Aşağıdaki cmdlet'leri kullanın:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a>Hdınsight Linux kümesi Data Lake Store ile varsayılan depolama alanı olarak oluşturun.

Bu bölümde, bir Hdınsight Hadoop Linux kümesi varsayılan depolama alanı olarak Data Lake Store ile oluşturun. Bu sürüm için Hdınsight kümesi ve Data Lake Store aynı konumda olması gerekir.

1. Abonelik Kiracı Kimliği almak ve daha sonra kullanmak üzere saklayın.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Aşağıdaki cmdlet'leri kullanarak Hdınsight kümesi oluşturun:

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

    Cmdlet başarıyla tamamlandıktan sonra küme ayrıntılarını listeler bir çıktı görmeniz gerekir.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a>Data Lake Store kullanacak şekilde Hdınsight kümesinde test işleri çalıştırma
Hdınsight kümesi yapılandırdıktan sonra Data Lake Store erişebildiğinden emin olmak için test işleri çalıştırabilirsiniz. Bunu yapmak için Data Lake Store'da kullanılabilir zaten örnek verileri kullanan bir tablo oluşturmak için örnek Hive işini çalıştırın  *<cluster root>/example/data/sample.log*.

Bu bölümde oluşturduğunuz Hdınsight Linux kümesine güvenli Kabuk (SSH) bağlantısı ve sonra bir örnek Hive sorgusunu çalıştırın.

* Bir SSH bağlantısı kümesine yapmak için bir Windows istemcisi kullanıyorsanız bkz [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Bir SSH bağlantısı kümesine yapmak için bir Linux istemcisi kullanıyorsanız bkz [Linux Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Bağlantı yaptıktan sonra aşağıdaki komutu kullanarak Hive komut satırı arabirimi (CLI) başlatın:

        hive
2. Adlı yeni bir tablo oluşturmak için aşağıdaki ifadeleri girmek için CLI kullanın **taşıtlardan** Data Lake Store'da örnek verileri kullanarak:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Sorgu çıktısı SSH konsolunda görmeniz gerekir.

    >[!NOTE]
    >Yukarıdaki CREATE TABLE komut örnek verileri yolu `adl:///example/data/`, burada `adl:///` küme köküdür. Aşağıdaki komut Bu öğreticide, belirtilen küme kök örneğine `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Daha kısa bir alternatif kullanabilir veya küme kök tam yolunu girin.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>HDFS komutlarını kullanarak Data Lake Store'a erişme
Hdınsight kümesi Data Lake Store kullanacak şekilde yapılandırdıktan sonra deposuna erişim için Hadoop dağıtılmış dosya sistemi (HDFS) kabuk komutlarını kullanabilirsiniz.

Bu bölümde oluşturduğunuz Hdınsight Linux kümesine bir SSH bağlantısı ve ardından HDFS komutları çalıştırın.

* Bir SSH bağlantısı kümesine yapmak için bir Windows istemcisi kullanıyorsanız bkz [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Bir SSH bağlantısı kümesine yapmak için bir Linux istemcisi kullanıyorsanız bkz [Linux Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Bağlantı yaptıktan sonra aşağıdaki HDFS dosya sistemi komutunu kullanarak Data Lake Store'da dosyaları listeleyin.

    hdfs dfs -ls adl:///

Aynı zamanda `hdfs dfs -put` bazı dosyalar için Data Lake Store karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Hdınsight kümeleri ile kullanım Data Lake Store](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [Azure portal: Data Lake Store kullanmak için bir Hdınsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
