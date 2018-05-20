---
title: Hdınsight ve Data Lake Store oluşturmak için Azure şablonlarını kullanma | Microsoft Docs
description: Azure Resource Manager şablonları oluşturmak ve Hdınsight kümeleri Azure Data Lake Store ile kullanmak için kullanın
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 1a5651e9e1b8c46e8151e44308de44245aa2ac35
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanarak Data Lake Store ile Hdınsight kümesi oluşturma
> [!div class="op_single_selector"]
> * [Portalı kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [(Varsayılan depolama için) PowerShell'i kullanma](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell kullanarak (için ek depolama alanı)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Kaynak Yöneticisi'ni kullanma](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Azure Data Lake Store ile Hdınsight kümesi yapılandırmak için Azure PowerShell kullanmayı öğrenin **ek depolama alanı olarak**.

Desteklenen küme türleri için Data Lake Store bir varsayılan depolama veya ek depolama alanı hesabı olarak kullanılması. Data Lake Store ek depolama alanı olarak kullanıldığında, kümeler için varsayılan depolama hesabı Azure Storage Blobları (WASB) olmaya devam edecektir ve işlem yapmak istediğiniz veriler bir Data Lake Store hesabında depolanabilir sırada küme ilgili dosyalar (örneğin, günlükleri, vb.) hala varsayılan depolama alanına yazılır. Data Lake Store ek depolama alanı hesabı olarak kullanarak performansı veya depolama birimine kümeden okuma/yazma özelliğini etkilemez.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Hdınsight küme depolaması için Data Lake Store kullanma

Hdınsight Data Lake Store ile kullanmak için bazı önemli noktalar şunlardır:

* Varsayılan depolama Hdınsight sürüm 3.5 ve 3.6 için kullanılabilir hale Data Lake Store erişimi olan Hdınsight kümeleri oluşturmak için seçeneği.

* Ek depolama alanı Hdınsight sürüm 3.2, 3.4, 3.5 ve 3.6 için kullanılabilir hale Data Lake Store erişimi olan Hdınsight kümeleri oluşturmak için seçeneği.

Bu makalede, sizi ek depolama alanı olarak Data Lake Store ile Hadoop kümesi sağlayın. Varsayılan depolama olarak Data Lake Store ile Hadoop kümesi oluşturma hakkında yönergeler için bkz: [Azure Portal'ı kullanarak Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 veya üstü**. Bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* **Azure Active Directory hizmet asıl**. Bu öğreticideki adımlar, Azure AD hizmet sorumlusu oluşturma konusunda yönergeler sağlar. Ancak, bir hizmet sorumlusu oluşturmak için Azure AD yönetici olmanız gerekir. Azure AD Yöneticiyseniz, bu önkoşulu atlayın ve bu öğreticiyi devam edin.

    **Azure AD Yönetici değilseniz**, bir hizmet sorumlusu oluşturmak için gerekli adımları gerçekleştirmek mümkün olmaz. Data Lake Store ile Hdınsight kümesi oluşturmadan önce böyle bir durumda, Azure AD yöneticinizin önce bir hizmet sorumlusu oluşturmanız gerekir. Ayrıca, hizmet sorumlusu bir sertifika kullanılarak konusunda açıklandığı gibi oluşturulmalıdır [sertifikayla bir hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a>Azure Data Lake Store ile Hdınsight kümesi oluşturma
Resource Manager şablonu ve şablonu kullanmak için önkoşulları github'da kullanılabilir [yeni Data Lake Store ile Hdınsight Linux kümesi dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Ek depolama alanı olarak Azure Data Lake Store ile Hdınsight kümesi oluşturmak için bu bağlantıyı sağlanan yönergeleri izleyin.

Yukarıda belirtilen bağlantı yönergeleri PowerShell gerektirir. Bu yönergeler içeren başlamadan önce Azure hesabınızda oturum açtığınızdan emin olun. Masaüstünüzde yeni bir Azure PowerShell penceresi açın ve aşağıdaki kod parçacıkları girin. Oturum açmanız istendiğinde, bir abonelik yöneticisi/sahibi olarak oturum açtığınızdan emin olun.

```
# Log in to your Azure account
Connect-AzureRmAccount

# List all the subscriptions associated to your account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-to-the-azure-data-lake-store"></a>Azure Data Lake Store için örnek veri yükleme
Resource Manager şablonu yeni bir Data Lake Store hesabı oluşturur ve Hdınsight kümesi ile ilişkilendirir. Data Lake Store için bazı örnek veriler artık yüklemeniz gerekir. Bu veriler, Data Lake Store'da verilerin erişim bir Hdınsight kümesine ait işlerini çalıştırmak için daha sonra öğreticide gerekir. Veri yükleme hakkında daha fazla yönerge için bkz: [bir dosyayı karşıya yüklemek için Data Lake Store](data-lake-store-get-started-portal.md#uploaddata). Karşıya yüklenecek örnek veri arıyorsanız [Azure Data Lake Git Deposu](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData)'ndan **Ambulance Data** klasörünü alabilirsiniz.

## <a name="set-relevant-acls-on-the-sample-data"></a>Örnek veri kümesi ilgili ACL'leri
Karşıya yüklediğiniz örnek veriler Hdınsight kümeden erişilebilir olduğundan emin olmak için Hdınsight kümesi ve Data Lake Store arasında kimlik erişmeye çalıştığınız dosya/klasör erişimi kurmak için kullanılan Azure AD uygulaması emin olmalısınız. Bunu yapmak için aşağıdaki adımları gerçekleştirin.

1. Hdınsight kümesi ve Data Lake Store ile ilişkili Azure AD uygulaması adını bulun. Resource Manager şablonu kullanılarak oluşturulan Hdınsight küme dikey penceresini açmak için adı için aramak için bir yolu tıklatın **kümeye özgü AAD kimliği** sekmesini tıklatın ve değeri aramak **hizmet sorumlusu görünen adı**.
2. Şimdi, Hdınsight küme erişmek istediğiniz dosyayı/klasörü üzerinde bu Azure AD uygulaması erişim sağlar. Data Lake Store'da dosyayı/klasörü sağ ACL'leri ayarlamak için bkz [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Data Lake Store kullanacak şekilde Hdınsight kümesinde test işleri çalıştırma
Hdınsight kümesi yapılandırdıktan sonra Hdınsight kümesi Data Lake Store erişebilmesini test etmek için kümede test işleri çalıştırabilirsiniz. Bunu yapmak için Data Lake Store için daha önce yüklenen örnek verileri kullanarak bir tablo oluşturur bir örnek Hive işi çalışır.

Bu bölümde, SSH Hdınsight Linux kümesi ve Çalıştır halinde olacak örnek Hive sorgusu. Windows istemcisi kullanıyorsanız, kullanmanızı öneririz **PuTTY**, hangi adresinden yüklenebilir [ http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kullanma hakkında daha fazla bilgi için bkz: [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Bağlantı kurulduktan sonra aşağıdaki komutu kullanarak Hive CLI başlatın:

   ```
   hive
   ```
2. CLI kullanarak girin adlı yeni bir tablo oluşturmak için aşağıdaki ifadeleri **taşıtlardan** Data Lake Store'da örnek verileri kullanarak:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
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


## <a name="access-data-lake-store-using-hdfs-commands"></a>HDFS komutları kullanarak erişim Data Lake Store
Hdınsight kümesi Data Lake Store kullanacak şekilde yapılandırdıktan sonra deposuna erişim için HDFS kabuk komutlarını kullanabilirsiniz.

Bu bölümde bir Hdınsight Linux SSH olacak küme ve HDFS komutları çalıştırın. Windows istemcisi kullanıyorsanız, kullanmanızı öneririz **PuTTY**, hangi adresinden yüklenebilir [ http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kullanma hakkında daha fazla bilgi için bkz: [Windows'dan hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Bağlantı kurulduktan sonra Data Lake Store'da dosyaları listelemek için aşağıdaki HDFS filesystem komutunu kullanın.

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

Bu Data Lake Store için daha önce karşıya dosya listelenmelidir.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

Aynı zamanda `hdfs dfs -put` bazı dosyalar için Data Lake Store karşıya yükleyin ve ardından komut `hdfs dfs -ls` dosyaları başarıyla karşıya yüklendi olup olmadığını doğrulamak için.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-wasb-distcp.md)
* [Azure Hdınsight kümeleri ile kullanım Data Lake Store](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
