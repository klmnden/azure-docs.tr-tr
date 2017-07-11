---
title: "Azure HDInsight'ta Data Lake Store’u Hadoop ile Kullanma | Microsoft Docs"
description: "Azure Data Lake Store’daki verileri sorgulama ve analiz sonuçlarınızı depolama işlemlerinin nasıl gerçekleştirildiğini öğrenin."
keywords: "blob depolama,hdfs,yapılandırılmış veriler,yapılandırılmamış veriler, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 993cff63037017e37ff5b0787f50ba002df28d03
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017


---
<a id="use-data-lake-store-with-azure-hdinsight-clusters" class="xliff"></a>

# Data Lake Store’u Azure HDInsight kümeleriyle kullanma

HDInsight kümesinde çözümlemek istediğiniz verileri [Azure Depolama](../storage/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) veya her ikisinde birden depolayabilirsiniz. İki depolama seçeneği de işlem için kullanılan HDInsight kümelerini kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Bu makalede Data Lake Store’un HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Azure Depolama’nın HDInsight kümeleriyle nasıl çalıştığı hakkında bilgi edinmek için bkz. [Azure Depolama’yı Azure HDInsight kümeleri ile kullanma](hdinsight-hadoop-use-blob-storage.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi edinmek için bkz. [HDInsight'ta Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!NOTE]
> Data Lake Store’a her zaman güvenli bir kanal üzerinden erişildiğinden, `adls` dosya sistemi düzen adı yoktur. Her zaman `adl` kullanırsınız.
> 
> 

<a id="availabilities-for-hdinsight-clusters" class="xliff"></a>

## HDInsight kümeleri için kullanılabilirlik durumları

Hadoop varsayılan dosya sistemi kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure Depolama'da bir blob kapsayıcısını varsayılan dosya sistemi olarak belirtebilir veya HDInsight 3.5 veya daha yeni sürümlerde, birkaç özel durum dışında Azure Depolama'yı ya da Azure Data Lake Store'u varsayılan dosya sistemi olarak seçebilirsiniz. 

HDInsight kümeleri Data Lake Store’u iki şekilde kullanabilir:

* Varsayılan depolama alanı olarak
* Azure Depolama Blobunun varsayılan depolama alanı olduğu durumlarda ek depolama alanı olarak.

Şu anda, Data Lake Store’un varsayılan depolama alanı ve ek depolama hesapları olarak kullanılmasını yalnızca bazı HDInsight küme türleri/sürümleri destekler:

| HDInsight küme türü | Varsayılan depolama alanı olarak Azure Data Lake Store | Ek depolama alanı olarak Azure Data Lake Store| Notlar |
|------------------------|------------------------------------|---------------------------------------|------|
| HDInsight sürümü 3.6 | Evet | Evet | |
| HDInsight sürümü 3.5 | Evet | Evet | HBase dışında|
| HDInsight sürümü 3.4 | Hayır | Evet | |
| HDInsight sürümü 3.3 | Hayır | Hayır | |
| HDInsight sürümü 3.2 | Hayır | Evet | |
| HDInsight Premium (katman)| Hayır | Hayır | |
| Storm | | |Data Lake Store’u kullanarak bir Storm topolojisinden veri yazabilirsiniz. Data Lake Store’u daha sonra bir Storm topolojisinden okunabilecek başvuru verileri için de kullanabilirsiniz.|

Data Lake Store’un ek depolama hesabı olarak kullanılması, kümeden Azure depolamaya yazma veya buradan okuma performansını ya da bu özelliğin kullanılabilirliğini etkilemez.


<a id="use-data-lake-store-as-default-storage" class="xliff"></a>

## Azure Data Lake Store’u varsayılan depolama alanı olarak kullanma

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


<a id="use-data-lake-store-as-additional-storage" class="xliff"></a>

## Azure Data Lake Store’u ek depolama alanı olarak kullanma

Data Lake Store'u da küme için ek depolama alanı olarak kullanabilirsiniz. Böyle durumlarda, kümenin varsayılan depolama alanı Azure Depolama Blobu veya Data Lake Store hesabı olabilir. HDInsight işlerini ek depolama alanı olarak kullanılan Data Lake Store'da depolanan verilere göre çalıştırıyorsanız, dosyaların tam yolunu kullanmanız gerekir. Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Artık URL'de **cluster_root_path** olmadığını unutmayın. Bunun nedeni Data Lake Store’un artık varsayılan depolama alanı olmamasıdır. Artık tüm yapmanız gereken dosyaların yolunu belirtmektir.

Data Lake Store’u ek depolama alanı olarak kullanabilmeniz için yalnızca dosyalarınızın depolandığı konumlara hizmet sorumlusu erişimi vermeniz gerekir.  Örneğin:

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

Hizmet sorumlusu oluşturma ve erişim verme hakkında daha fazla bilgi edinmek için bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).


<a id="use-more-than-one-data-lake-store-accounts" class="xliff"></a>

## Birden çok Data Lake Store hesabı kullanma

Bir Data Lake Store hesabını ek depolama olarak ekleme ve birden fazla Data Lake Store hesabı ekleme işlemi, bir veya daha çok Data Lake Store hesabındaki veriler için HDInsight kümesine izin verilerek gerçekleştirilir. Bkz. [Data Lake Store erişimini yapılandırma](#configure-data-lake-store-access).

<a id="configure-data-lake-store-access" class="xliff"></a>

## Data Lake Store erişimini yapılandırma

HDInsight kümenizden Data Lake store erişimini yapılandırabilmeniz için bir Azure Active Directory (Azure AD) hizmet sorumlunuz olmalıdır. Hizmet sorumlusu yalnızca bir Azure AD yöneticisi tarafından oluşturulabilir. Hizmet sorumlusunun bir sertifika ile oluşturulması gerekir. Daha fazla bilgi edinmek için bkz. [Data Lake Store erişimini yapılandırma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access) ve [Otomatik olarak imzalanan sertifika ile hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).

> [!NOTE]
> Azure Data Lake Store’u HDInsight kümesi için ek depolama alanı olarak kullanacaksanız, bunu bu makalede açıklandığı gibi kümeyi oluştururken yapmanız önemle önerilir. Azure Data Lake Store’u mevcut bir HDInsight kümesine ek depolama alanı olarak ekleme, karmaşık ve hatalara yol açabilecek bir işlemdir.
>

<a id="access-files-from-the-cluster" class="xliff"></a>

## Kümeden dosyalara erişme

Data Lake Store dosyalarına bir HDInsight kümesinden erişmenin çeşitli yolları vardır.

* **Tam adı kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın tam yolunu girersiniz.

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* **Kısaltılmış yol biçimi kullanarak**. Bu yöntemle, yolu küme kökü ve adl ile değiştirirsiniz: / / /. Yukarıdaki örnekte `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` ile `adl:///` değerini değiştirebilirsiniz.

        adl:///<file path>

* **Göreli yolu kullanarak**. Bu yöntemle, erişmek istediğiniz dosyanın yalnızca göreli yolunu girersiniz. Örneğin dosyanın tam yolu şöyleyse:

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    Aynı sample.log dosyasına göreli yolu kullanarak erişebilirsiniz.

        /example/data/sample.log

<a id="create-hdinsight-clusters-with-access-to-data-lake-store" class="xliff"></a>

## Data Lake Store erişimi olan HDInsight kümeleri oluşturma

Data Lake Store erişimi olan HDInsight kümeleri oluşturma hakkındaki ayrıntılı yönergeler için aşağıdaki bağlantıları kullanın.

* [Portalı kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell kullanma (varsayılan depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [PowerShell kullanma (ek depolama alanı olarak Data Lake Store ile)](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [Azure şablonlarını kullanma](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
Bu makalede, HDInsight ile HDFS uyumlu Azure Data Lake Store’u kullanmayı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]
* [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight'a veri yükleme][hdinsight-upload-data]
* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [HDInsight ile verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzaları kullanma][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

