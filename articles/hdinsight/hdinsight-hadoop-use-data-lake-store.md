---
title: Azure HDInsight, Hadoop ile Data Lake depolama kullanma
description: Azure Data Lake Storage verileri sorgulamaya ve analiz sonuçlarınızı depolamak için hakkında bilgi edinin.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: dfbce1afcefe7f03636d42ffa363fe29b47259e8
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742567"
---
# <a name="use-data-lake-storage-with-azure-hdinsight-clusters"></a>Data Lake Storage Azure HDInsight kümeleri ile kullanma

HDInsight kümesindeki verileri çözümlemek için veri ya da depolayabilirsiniz içinde [Azure depolama](../storage/common/storage-introduction.md), [Azure Data Lake Storage](../data-lake-store/data-lake-store-overview.md), veya her ikisini de. İki depolama seçeneği de işlem için kullanılan HDInsight kümelerini kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Bu makalede, Data Lake Storage HDInsight kümeleri ile nasıl çalıştığını öğrenin. Azure Depolama’nın HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi edinmek için bkz. [Azure Depolama’yı Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-blob-storage.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [Apache Hadoop kümeleri oluşturma HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]  
> Bu yüzden Data Lake depolama güvenli bir kanal üzerinden her zaman erişilebilir hiçbir `adls` dosya sistemi Düzen adı. Her zaman `adl` kullanırsınız.


## <a name="availability-for-hdinsight-clusters"></a>HDInsight kümeleri için kullanılabilirlik

Apache Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında varsayılan dosya sistemi olarak Azure storage'da bir blob kapsayıcısını belirtin veya HDInsight 3.5 ve yeni sürümler ile Azure depolama veya Azure Data Lake Storage varsayılan dosya sistemi ile birkaç seçebilirsiniz özel durumlar. 

HDInsight kümeleri Data Lake Storage iki şekilde kullanabilir:

* Varsayılan depolama alanı olarak
* Azure Depolama Blobunun varsayılan depolama alanı olduğu durumlarda ek depolama alanı olarak.

Şu anda, varsayılan depolama alanı ve ek depolama hesapları Data Lake depolama kullanan türleri/sürümleri desteği yalnızca bazı HDInsight küme:

| HDInsight küme türü | Varsayılan depolama alanı olarak Data Lake depolama | Ek depolama alanı olarak Data Lake depolama| Notlar |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight sürümü 3.6 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.5 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.4 | Hayır | Evet | |
| HDInsight sürümü 3.3 | Hayır | Hayır | |
| HDInsight sürümü 3.2 | Hayır | Evet | |
| Storm | | |Data Lake Storage'ı bir Storm topolojisinden veri yazmak için kullanabilirsiniz. Data Lake Storage, ardından okunabilir bir Storm topolojisinden okunabilecek başvuru verileri için de kullanabilirsiniz.|

> [!WARNING]  
> HDInsight HBase, Azure Data Lake depolama Gen 1 ile desteklenmiyor

Ek depolama hesabı Data Lake Storage'ı kullanarak, performans veya okuma veya kümeden Azure depolamaya yazma olanağı etkilemez.
## <a name="use-data-lake-storage-as-default-storage"></a>Varsayılan depolama alanı olarak Data Lake Storage kullanma

HDInsight ile varsayılan depolama alanı olarak Data Lake Store dağıtıldığında, kümeyle ilişkili dosyalar Data Lake Storage şu konumda depolanır:

    adl://mydatalakestore/<cluster_root_path>/

Burada `<cluster_root_path>` Data Lake Store içinde oluşturduğunuz klasörün adıdır. Her küme için kök yolu belirterek, birden fazla küme için aynı Data Lake Storage hesabını kullanabilirsiniz. Bunu yaptığınızda şöyle bir durum olabilir:

* Cluster1 `adl://mydatalakestore/cluster1storage` yolunu kullanabilir.
* Cluster2 `adl://mydatalakestore/cluster2storage` yolunu kullanabilir.

Her iki aynı Data Lake Storage hesabını kullandığına dikkat edin **mydatalakestore**. Her küme Data Lake Store içinde kendi kök dosya sistemine erişebilir. Özellikle Azure portalı dağıtımı deneyimi sizden kök yol olarak **/clusters/\<clustername>** gibi bir klasör adı kullanmanızı ister.

Varsayılan depolama alanı olarak Data Lake Storage kullanabilmek için aşağıdaki yollara hizmet sorumlusu erişimi vermeniz gerekir:

- Data Lake Store hesabının kökü.  Örneğin: adl://mydatalakestore/.
- Tüm küme klasörlerine yönelik klasör.  Örneğin: adl://mydatalakestore/clusters.
- Kümenin klasörü.  Örneğin: adl://mydatalakestore/clusters/cluster1storage.

Hizmet sorumlusu oluşturma ve erişim verme için daha fazla bilgi için [yapılandırma Data Lake Storage erişim](#configure-data-lake-store-access).

### <a name="extracting-a-certificate-from-azure-keyvault-for-use-in-cluster-creation"></a>Küme oluşturma kullanmak için Azure anahtar Kasası ' bir sertifika ayıklanıyor

Kurulum ADLS, varsayılan depolama alanı olarak yeni bir küme için istediğiniz ve hizmet asıl sertifikasını Azure anahtar Kasası'nda depolanır, sertifikanın doğru biçime dönüştürmek için gereken bazı ek adımlar vardır. Aşağıdaki kod parçacıkları nasıl dönüştürme yapılacağı gösterir.

İlk olarak, sertifika Key Vault'tan indirilip ayıklanması `SecretValueText`.

```powershell
$certPassword = Read-Host "Enter Certificate Password"
$cert = (Get-AzureKeyVaultSecret -VaultName 'MY-KEY-VAULT' -Name 'MY-SECRET-NAME')
$certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
```

Ardından, dönüştürme `SecretValueText` bir sertifikaya.

```powershell
$certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue,$null,"Exportable, PersistKeySet"
$certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);
$identityCertificate = [System.Convert]::ToBase64String($certBytes)
```

Kullanabileceğiniz sonra `$identityCertificate` aşağıdaki kod parçacığını olduğu gibi yeni bir kümeye dağıtmak için:

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile $pathToArmTemplate `
    -identityCertificate $identityCertificate `
    -identityCertificatePassword $certPassword.SecretValueText `
    -clusterName  $clusterName `
    -clusterLoginPassword $SSHpassword `
    -sshPassword $SSHpassword `
    -servicePrincipalApplicationId $application.ApplicationId
```

## <a name="use-data-lake-storage-as-additional-storage"></a>Ek depolama alanı olarak Data Lake Storage kullanma

Ek depolama alanı olarak Data Lake depolama kümesi için kullanabilirsiniz. Böyle durumlarda kümenin varsayılan depolama ya da bir Azure depolama blobu veya Data Lake Store hesabı olabilir. HDInsight işlerini ek depolama alanı olarak Data Lake Store içinde depolanan verilere göre çalıştırıyorsanız, dosyaları tam yolu kullanmanız gerekir. Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Artık URL'de **cluster_root_path** olmadığını unutmayın. Tüm yapmanız gereken dosyaların yolunu sağlamanız Data Lake Storage varsayılan depolama alanı bu durumda olmadığından olmasıdır.

Ek depolama alanı olarak Data Lake Storage kullanabilmek için yalnızca dosyalarınızın depolandığı yollara hizmet sorumlusu erişimi vermeniz gerekir.  Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Hizmet sorumlusu oluşturma ve erişim verme için daha fazla bilgi için [yapılandırma Data Lake Storage erişim](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-storage-accounts"></a>Birden fazla Data Lake Storage hesaplarını kullanma

Bir Data Lake Store hesabına ek olarak ekleme ve birden fazla Data Lake Storage ekleme hesapları bir veya birden fazla Data Lake Storage hesaplarını verilerde HDInsight kümesine izin verilerek gerçekleştirilir. Bkz: [yapılandırma Data Lake Storage erişim](#configure-data-lake-store-access).

## <a name="configure-data-lake-storage-access"></a>Data Lake Store erişimini yapılandırma

HDInsight kümenizden Data Lake Store erişimi yapılandırmak için bir Azure Active directory (Azure AD) hizmet sorumlusu olması gerekir. Hizmet sorumlusu yalnızca bir Azure AD yöneticisi tarafından oluşturulabilir. Hizmet sorumlusunun bir sertifika ile oluşturulması gerekir. Daha fazla bilgi için [hızlı başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md), ve [self-otomatik olarak imzalanan sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]  
> HDInsight kümesi için ek depolama alanı olarak Azure Data Lake Storage kullanılacak kullanacaksanız, bu makalede açıklandığı gibi kümeyi oluştururken yapmanız önemle önerilir. Var olan bir HDInsight kümesine ek depolama alanı olarak ekleyerek Azure Data Lake Storage desteklenen bir senaryo değildir.
>

## <a name="access-files-from-the-cluster"></a>Kümeden dosyalara erişme

Data Lake Storage dosyalarında bir HDInsight kümesinden erişmenin birkaç yolu vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Kısaltılmış yol biçimi kullanarak**. Bu yöntemle, yolu küme kökü ve adl ile değiştirirsiniz: / / /. Yukarıdaki örnekte `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` ile `adl:///` değerini değiştirebilirsiniz.

        adl:///<file path>

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz. Örneğin dosyanın tam yolu şöyleyse:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Aynı sample.log dosyasına göreli yolu kullanarak erişebilirsiniz.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-storage"></a>Data Lake Store erişimi olan HDInsight kümeleri oluşturma

Data Lake Store erişimi olan HDInsight kümeleri oluşturma konusunda ayrıntılı yönergeler için aşağıdaki bağlantıları kullanın.

* [Portalı kullanma](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [PowerShell kullanma (ile varsayılan depolama alanı olarak Data Lake depolama)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell kullanma (ek depolama alanı olarak Data Lake depolama ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure şablonlarını kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-storage-access"></a>Data Lake depolama erişimi için HDInsight Sertifikayı Yenile

Aşağıdaki örnek PowerShell kodu yerel dosya ya da Azure Key Vault, sertifika okur ve HDInsight kümenizi Azure Data Lake depolamaya erişmek için yeni sertifika ile güncelleştirir. Kendi HDInsight küme adı, kaynak grubu adı, abonelik kimliği, uygulama kimliği, sertifika yerel yolu belirtin. İstendiğinde parolayı yazın.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$KeyVaultName = "my-key-vault-name"
$KeyVaultSecretName = "my-key-vault-secret-name"
$certPassword = Read-Host "Enter Certificate Password"
# certSource
# 0 - create self signed cert
# 1 - read cert from file path
# 2 - read cert from key vault
$certSource = 0

if($certSource -eq 0)
{
    Write-Host "Generating new SelfSigned certificate"

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
elseif($certSource -eq 1)
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
}
elseif($certSource -eq 2)
{

    Write-Host "Reading the cert file from Azure Key Vault $KeyVaultName"

    $cert = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName)
    $certValue = [System.Convert]::FromBase64String($cert.SecretValueText)
    $certObject = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certValue, $null,"Exportable, PersistKeySet"

    $certBytes = $certObject.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword.SecretValueText);

    $certString =[System.Convert]::ToBase64String($certBytes)
}

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subscriptionId

if($addNewCertKeyCredential)
{
    Write-Host "Creating new KeyCredential for the app"
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    New-AzureRmADAppCredential -ApplicationId $appId -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    Write-Host "Waiting for 30 seconds for the permissions to get propagated"
    Start-Sleep -s 30
}

Write-Host "Updating the certificate on HDInsight cluster..."

Invoke-AzureRmResourceAction `
    -ResourceGroupName $resourceGroupName `
    -ResourceType 'Microsoft.HDInsight/clusters' `
    -ResourceName $clusterName `
    -ApiVersion '2015-03-01-preview' `
    -Action 'updateclusteridentitycertificate' `
    -Parameters @{ ApplicationId = $appId; Certificate = $certString; CertificatePassword = $certPassword.ToString() } `
    -Force
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, HDFS uyumlu Azure Data Lake kullanmayı öğrendiniz HDInsight ile depolama. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Hızlı Başlangıç: HDInsight kümelerinde ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Azure PowerShell kullanarak Data Lake Storage kullanılacak bir HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: https://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
