---
title: Azure HDInsight, Hadoop ile kullanım Data Lake Store
description: Azure Data Lake Store’daki verileri sorgulama ve analiz sonuçlarınızı depolama işlemlerinin nasıl gerçekleştirildiğini öğrenin.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 0859e480df0111e26d5b64bf835f94b3852b3414
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277367"
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a>Data Lake Store’u Azure HDInsight kümeleriyle kullanma

HDInsight kümesinde çözümlemek istediğiniz verileri [Azure Depolama](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) veya her ikisinde birden depolayabilirsiniz. İki depolama seçeneği de işlem için kullanılan HDInsight kümelerini kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Bu makalede Data Lake Store’un HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Azure Depolama’nın HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi edinmek için bkz. [Azure Depolama’yı Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-blob-storage.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi edinmek için bkz. [HDInsight'ta Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> Data Lake Store’a her zaman güvenli bir kanal üzerinden erişildiğinden, `adls` dosya sistemi düzen adı yoktur. Her zaman `adl` kullanırsınız.
> 


## <a name="availability-for-hdinsight-clusters"></a>HDInsight kümeleri için kullanılabilirlik

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure Depolama'da bir blob kapsayıcısını varsayılan dosya sistemi olarak belirtebilir veya HDInsight 3.5 veya daha yeni sürümlerde, birkaç özel durum dışında Azure Depolama'yı ya da Azure Data Lake Store'u varsayılan dosya sistemi olarak seçebilirsiniz. 

HDInsight kümeleri Data Lake Store’u iki şekilde kullanabilir:

* Varsayılan depolama alanı olarak
* Azure Depolama Blobunun varsayılan depolama alanı olduğu durumlarda ek depolama alanı olarak.

Şu anda, Data Lake Store’un varsayılan depolama alanı ve ek depolama hesapları olarak kullanılmasını yalnızca bazı HDInsight küme türleri/sürümleri destekler:

| HDInsight küme türü | Varsayılan depolama alanı olarak Azure Data Lake Store | Ek depolama alanı olarak Azure Data Lake Store| Notlar |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight sürümü 3.6 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.5 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.4 | Hayır | Evet | |
| HDInsight sürümü 3.3 | Hayır | Hayır | |
| HDInsight sürümü 3.2 | Hayır | Evet | |
| Storm | | |Data Lake Store’u kullanarak bir Storm topolojisinden veri yazabilirsiniz. Data Lake Store’u daha sonra bir Storm topolojisinden okunabilecek başvuru verileri için de kullanabilirsiniz.|

[!WARNING]
> HDInsight HBase, Azure Data Lake depolama Gen 1 ile desteklenmiyor

Data Lake Store’un ek depolama hesabı olarak kullanılması, kümeden Azure depolamaya yazma veya buradan okuma performansını ya da bu özelliğin kullanılabilirliğini etkilemez.
## <a name="use-data-lake-store-as-default-storage"></a>Azure Data Lake Store’u varsayılan depolama alanı olarak kullanma

HDInsight ile varsayılan depolama alanı olarak Data Lake Store dağıtıldığında, kümeyle ilişkili dosyalar Data Lake Store içindeki şu konumda depolanır:

    adl://mydatalakestore/<cluster_root_path>/

Buradaki `<cluster_root_path>`, Azure Data Lake Store’da oluşturduğunuz klasörün adıdır. Her küme için bir kök yolu belirterek, birden fazla küme için aynı Data Lake Store hesabını kullanabilirsiniz. Bunu yaptığınızda şöyle bir durum olabilir:

* Cluster1 `adl://mydatalakestore/cluster1storage` yolunu kullanabilir.
* Cluster2 `adl://mydatalakestore/cluster2storage` yolunu kullanabilir.

Her iki kümenin aynı **mydatalakestore** Data Lake Store hesabını kullandığına dikkat edin. Her küme Data Lake Store içinde kendi kök dosya sistemine erişebilir. Özellikle Azure portalı dağıtımı deneyimi sizden kök yol olarak **/clusters/\<clustername>** gibi bir klasör adı kullanmanızı ister.

Bir Data Lake Store’u varsayılan depolama alanı olarak kullanabilmeniz için aşağıdaki yollara hizmet sorumlusu erişimi vermeniz gerekir:

- Data Lake Store hesabının kökü.  Örneğin: adl://mydatalakestore/.
- Tüm küme klasörlerine yönelik klasör.  Örneğin: adl://mydatalakestore/clusters.
- Kümenin klasörü.  Örneğin: adl://mydatalakestore/clusters/cluster1storage.

Hizmet sorumlusu oluşturma ve erişim verme hakkında daha fazla bilgi edinmek için bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).


## <a name="use-data-lake-store-as-additional-storage"></a>Azure Data Lake Store’u ek depolama alanı olarak kullanma

Data Lake Store'u da küme için ek depolama alanı olarak kullanabilirsiniz. Böyle durumlarda, kümenin varsayılan depolama alanı Azure Depolama Blobu veya Data Lake Store hesabı olabilir. HDInsight işlerini ek depolama alanı olarak kullanılan Data Lake Store'da depolanan verilere göre çalıştırıyorsanız, dosyaların tam yolunu kullanmanız gerekir. Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Artık URL'de **cluster_root_path** olmadığını unutmayın. Bunun nedeni Data Lake Store’un artık varsayılan depolama alanı olmamasıdır. Artık tüm yapmanız gereken dosyaların yolunu belirtmektir.

Data Lake Store’u ek depolama alanı olarak kullanabilmeniz için yalnızca dosyalarınızın depolandığı konumlara hizmet sorumlusu erişimi vermeniz gerekir.  Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Hizmet sorumlusu oluşturma ve erişim verme hakkında daha fazla bilgi edinmek için bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).


## <a name="use-more-than-one-data-lake-store-accounts"></a>Birden çok Data Lake Store hesabı kullanma

Bir Data Lake Store hesabını ek depolama olarak ekleme ve birden fazla Data Lake Store hesabı ekleme işlemi, bir veya daha çok Data Lake Store hesabındaki veriler için HDInsight kümesine izin verilerek gerçekleştirilir. Bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Data Lake Store erişimini yapılandırma

HDInsight kümenizden Data Lake store erişimini yapılandırabilmeniz için bir Azure Active Directory (Azure AD) hizmet sorumlunuz olmalıdır. Hizmet sorumlusu yalnızca bir Azure AD yöneticisi tarafından oluşturulabilir. Hizmet sorumlusunun bir sertifika ile oluşturulması gerekir. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) ve [Otomatik olarak imzalanan sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]
> Azure Data Lake Store’u HDInsight kümesi için ek depolama alanı olarak kullanacaksanız, bunu bu makalede açıklandığı gibi kümeyi oluştururken yapmanız önemle önerilir. Azure Data Lake Store’u mevcut bir HDInsight kümesine ek depolama alanı olarak ekleme, desteklenmeyen bir senaryodur.
>

## <a name="access-files-from-the-cluster"></a>Kümeden dosyalara erişme

Data Lake Store dosyalarına bir HDInsight kümesinden erişmenin çeşitli yolları vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Kısaltılmış yol biçimi kullanarak**. Bu yöntemle, yolu küme kökü ve adl ile değiştirirsiniz: / / /. Yukarıdaki örnekte `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` ile `adl:///` değerini değiştirebilirsiniz.

        adl:///<file path>

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz. Örneğin dosyanın tam yolu şöyleyse:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Aynı sample.log dosyasına göreli yolu kullanarak erişebilirsiniz.

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-to-data-lake-store"></a>Data Lake Store erişimi olan HDInsight kümeleri oluşturma

Data Lake Store erişimi olan HDInsight kümeleri oluşturma hakkındaki ayrıntılı yönergeler için aşağıdaki bağlantıları kullanın.

* [Portalı kullanma](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [PowerShell kullanma (varsayılan depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell kullanma (ek depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure şablonlarını kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

## <a name="refresh-the-hdinsight-certificate-for-data-lake-store-access"></a>Data Lake Store erişimi için HDInsight Sertifikayı Yenile

Aşağıdaki örnek PowerShell kodu yerel sertifika dosyasını okur ve HDInsight kümenizi Azure Data Lake Store erişim için yeni sertifika ile güncelleştirir. Kendi HDInsight küme adı, kaynak grubu adı, abonelik kimliği, uygulama kimliği, sertifika yerel yolu belirtin. İstendiğinde parolayı yazın.

```powershell-interactive
$clusterName = '<clustername>'
$resourceGroupName = '<resourcegroupname>'
$subscriptionId = '01234567-8a6c-43bc-83d3-6b318c6c7305'
$appId = '01234567-e100-4118-8ba6-c25834f4e938'
$generateSelfSignedCert = $false
$addNewCertKeyCredential = $true
$certFilePath = 'C:\localfolder\adls.pfx'
$certPassword = Read-Host "Enter Certificate Password"

if($generateSelfSignedCert)
{
    Write-Host "Generating new SelfSigned certificate"
    
    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=hdinsightAdlsCert" -KeySpec KeyExchange
    $certBytes = $cert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12, $certPassword);
    $certString = [System.Convert]::ToBase64String($certBytes)
}
else
{

    Write-Host "Reading the cert file from path $certFilePath"

    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($certFilePath, $certPassword)
    $certString = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes($certFilePath))
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
Bu makalede, HDInsight ile HDFS uyumlu Azure Data Lake Store’u kullanmayı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* [Azure PowerShell ile Data Lake Store’u kullanmak için HDInsight kümesi oluşturma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
