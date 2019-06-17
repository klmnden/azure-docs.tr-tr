---
title: HDInsight ile Azure Data Lake depolama Gen1 oluşturmak için Azure şablonlarını kullanma | Microsoft Docs
description: Oluşturup HDInsight kümeleri ile Azure Data Lake depolama Gen1 kullanmak için Azure Resource Manager şablonlarını kullanma
services: data-lake-store,hdinsight
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b09ca2cc358107c5f95fe3426351d380380db3c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66161381"
---
# <a name="create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Azure Data Lake depolama Gen1 bir HDInsight kümesi oluşturun
> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell'i kullanma (varsayılan depolama için)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell'i kullanma (ek depolama için)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Azure Data Lake depolama Gen1 ile bir HDInsight kümesini yapılandırmak için Azure PowerShell kullanmayı öğrenirsiniz **ek depolama alanı olarak**.

Desteklenen küme türleri için Data Lake depolama Gen1 varsayılan depolama alanı veya ek depolama alanı hesabı olarak kullanılabilir. Ek depolama alanı olarak Data Lake depolama Gen1 kullanıldığında, kümeler için varsayılan depolama hesabı Azure depolama BLOB'ları (WASB) devam edebilir ve kümeyle ilişkili dosyalar (örneğin, günlük, vb.) hala varsayılan depolama alanı, istediğiniz veriler yazılır işlem, bir Data Lake depolama Gen1 hesabında depolanabilir. Data Lake depolama Gen1 ek depolama hesabı kullanarak, performans veya depolama kümesinden okuma/yazma olanağı etkilemez.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>HDInsight küme depolaması için Data Lake depolama Gen1 kullanma

HDInsight kullanarak Data Lake depolama Gen1 ile ilgili bazı önemli noktalar şunlardır:

* Varsayılan depolama, HDInsight sürüm 3.5 ve 3.6 için kullanılabilir olarak Data Lake depolama Gen1 erişimi olan HDInsight kümeleri oluşturmak için seçeneği.

* Ek depolama, HDInsight sürüm 3.2, 3.4, 3.5 ve 3.6 için kullanılabilir olarak Data Lake depolama Gen1 erişimi olan HDInsight kümeleri oluşturmak için seçeneği.

Bu makalede, biz ek depolama alanı olarak Data Lake depolama Gen1 ile bir Hadoop kümesi sağlayın. Varsayılan depolama alanı olarak Data Lake depolama Gen1 ile bir Hadoop kümesi oluşturma hakkında yönergeler için bkz: [Azure portalını kullanarak bir HDInsight kümesiyle Data Lake depolama Gen1 oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Azure Active Directory Hizmet sorumlusu**. Bu öğreticideki adımlar, Azure AD'de hizmet sorumlusu oluşturma hakkında yönergeler sağlar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Bir Azure AD Yöneticisi olarak, bu önkoşulu atlayın ve öğreticiyle devam edin.

    **Bir Azure AD yöneticisi değilseniz**, bir hizmet sorumlusu oluşturmak için gerekli adımları gerçekleştirmek mümkün olmayacaktır. Bir HDInsight kümesi ile Data Lake depolama Gen1 oluşturabilmeniz için önce böyle bir durumda, Azure AD yöneticinizin öncelikle bir hizmet sorumlusu oluşturmanız gerekir. Ayrıca, hizmet sorumlusunun bir sertifika kullanmayı anlatıldığı gibi oluşturulmalıdır [sertifika ile hizmet sorumlusu oluşturma](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-data-lake-storage-gen1"></a>Data Lake depolama Gen1 ile bir HDInsight kümesi oluşturma
Resource Manager şablonu ve bir şablonu kullanmak için önkoşulları github'da kullanılabilir [yeni Data Lake depolama Gen1 ile HDInsight Linux kümesi dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Ek depolama alanı olarak Data Lake depolama Gen1 ile bir HDInsight kümesi oluşturmak için bu bağlantıda sağlanan yönergeleri izleyin.

Yönergeler yukarıda belirtilen bağlantıdaki PowerShell gerektirir. Bu yönergelerle başlamadan önce Azure hesabınızda oturum emin olun. Masaüstünüzde yeni bir Azure PowerShell penceresi açın ve aşağıdaki kod parçacıklarının girin. Oturum açmanız istendiğinde, bir abonelik yöneticileri/sahibi oturum emin olun:

```
# Log in to your Azure account
Connect-AzAccount

# List all the subscriptions associated to your account
Get-AzSubscription

# Select a subscription
Set-AzContext -SubscriptionId <subscription ID>
```

Şablon, bu kaynak türleri dağıtır:

* [Microsoft.DataLakeStore/accounts](/azure/templates/microsoft.datalakestore/accounts)
* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.HDInsight/clusters](/azure/templates/microsoft.hdinsight/clusters)

## <a name="upload-sample-data-to-data-lake-storage-gen1"></a>Data Lake depolama Gen1 için örnek verileri karşıya yükleme
Resource Manager şablonu yeni bir Data Lake depolama Gen1 hesabı oluşturur ve HDInsight küme ile ilişkilendirir. Şimdi bazı örnek veriler için Data Lake depolama Gen1 yüklemeniz gerekir. Data Lake depolama Gen1 hesaptaki verilere erişim bir HDInsight kümesinden işlerini çalıştırmak için öğreticinin sonraki bölümlerinde, bu verileri gerekir. Karşıya veri yükleme konusunda yönergeler için bkz. [Data Lake depolama Gen1 hesabınıza bir dosya yükleme](data-lake-store-get-started-portal.md#uploaddata). Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="set-relevant-acls-on-the-sample-data"></a>Örnek veri kümesi ilgili ACL'leri
Örnek verileri karşıya yüklediğiniz HDInsight kümesinden erişilebilir olduğundan emin olmak için HDInsight kümenizle Data Lake depolama Gen1 arasında kimlik çalıştığınız dosya/klasör erişimi kurmak için kullanılan Azure AD uygulaması emin olmanız gerekir erişim. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. HDInsight kümesi ve Data Lake depolama Gen1 hesabıyla ilişkili Azure AD uygulamasının adı bulun. Adı Resource Manager şablonunu kullanarak oluşturduğunuz HDInsight küme dikey penceresini açmak için konum yollarından biri tıklayın **küme AAD kimlik** sekme ve değerini arayın **hizmet sorumlusu görünen adı**.
2. Şimdi, HDInsight kümesinden erişmek istediğiniz dosyayı/klasörü üzerinde bu Azure AD uygulamaya erişimi sağlar. Data Lake depolama Gen1 dosya/klasör üzerinde doğru ACL'leri ayarlamak için bkz [Data Lake depolama Gen1 verileri güvenli hale getirme](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Data Lake depolama Gen1 kullanmak için HDInsight kümesinde test işleri çalıştırma
Bir HDInsight kümesi yapılandırdıktan sonra kümede HDInsight kümesini Data Lake depolama Gen1 erişebilirsiniz test etmek için test işleri çalıştırabilirsiniz. Bunu yapmak için daha önce Data Lake depolama Gen1 hesabınıza yüklediğiniz örnek verileri kullanarak bir tablo oluşturur bir örnek Hive işi çalıştıracağız.

Bu bölümde, SSH bir HDInsight Linux kümesi ve çalışma halinde olacak bir örnek Hive sorgusu. Bir Windows istemci kullanıyorsanız kullanmanızı öneririz **PuTTY**, hangi nden indirilebilir [ https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kullanma hakkında daha fazla bilgi için bkz. [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Bağlandıktan sonra aşağıdaki komutu kullanarak Hive CLI'yı başlatın:

   ```
   hive
   ```
2. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki deyimleri **taşıtlardan** örnek verileri kullanarak Data Lake depolama Gen1 içinde:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Aşağıdakine benzer bir çıktı görmeniz gerekir:

   ```
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
   ```


## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>Data Lake depolama Gen1 erişim HDFS komutları kullanma
Data Lake depolama Gen1 kullanmak için HDInsight küme yapılandırıldıktan sonra HDFS Kabuk komutları mağazaya erişmek için kullanabilirsiniz.

Bu bölümde bir HDInsight Linux SSH olacak küme ve HDFS komutları çalıştırın. Bir Windows istemci kullanıyorsanız kullanmanızı öneririz **PuTTY**, hangi nden indirilebilir [ https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kullanma hakkında daha fazla bilgi için bkz. [Windows gelen HDInsight üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Bağlantı kurulduktan sonra Data Lake depolama Gen1 hesabındaki dosyaları listelemek için aşağıdaki HDFS dosya sistemi komutu kullanın.

```
hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/
```

Bu Data Lake depolama Gen1 için daha önce yüklediğiniz dosyanın listelemelisiniz.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder
```

Ayrıca `hdfs dfs -put` bazı dosyalar için Data Lake depolama Gen1 karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.


## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-wasb-distcp.md)
* [Data Lake depolama Gen1 Azure HDInsight kümeleri ile kullanma](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
