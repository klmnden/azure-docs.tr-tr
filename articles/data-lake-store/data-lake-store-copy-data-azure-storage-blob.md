---
title: "Verileri Azure Storage Bloblarından Data Lake Store kopyalamak | Microsoft Docs"
description: "Azure Storage Bloblarından Data Lake Store'a veri kopyalamak için AdlCopy aracını kullanın"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/03/2017
ms.author: nitinme
ms.openlocfilehash: 2dd327f4e4abf19d41a54919c8b9c2e488d34d68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Azure Depolama Bloblarından Data Lake Store’a veri kopyalama
> [!div class="op_single_selector"]
> * [DistCp’yi kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy’yi kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Store sağlayan bir komut satırı aracı [AdlCopy](http://aka.ms/downloadadlcopy), aşağıdaki kaynaklardan verileri kopyalamak için:

* Data Lake Store içinde Azure Storage Bloblarından. Verileri Azure Storage bloblarında Data Lake Deposu'ndan veri kopyalamak için AdlCopy kullanamazsınız.
* İki Azure Data Lake Store hesapları arasında.

Ayrıca, iki farklı modda AdlCopy Aracı'nı kullanabilirsiniz:

* **Tek başına**, burada aracı görevi gerçekleştirmek için Data Lake Store kaynakları kullanır.
* **Bir Data Lake Analytics hesabı kullanarak**, Data Lake Analytics hesabınızı atanan birim kopyalama işlemi gerçekleştirmek için kullanıldığı. Bu seçenek tahmin edilebilir bir biçimde kopyalama görevleri gerçekleştirmek için ararken kullanmak isteyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Storage Bloblarında** bazı verileri içeren kapsayıcı.
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Data Lake Analytics hesabı (isteğe bağlı)** -bkz [Azure Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md) bir Data Lake Store hesabı oluşturma hakkında yönergeler için.
* **AdlCopy aracı**. AdlCopy aracından yüklemek [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>AdlCopy aracının sözdizimi
AdlCopy aracı ile çalışmak için aşağıdaki sözdizimini kullanın

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

Sözdizimi parametrelerinde aşağıda açıklanmıştır:

| Seçenek | Açıklama |
| --- | --- |
| Kaynak |Azure depolama blobunu kaynak verilerin konumunu belirtir. Kaynak, bir blob kapsayıcı, blob veya başka bir Data Lake Store hesabı olabilir. |
| Hedef |Kopyalamak için Data Lake Store hedef belirtir. |
| SourceKey |Azure storage blobu kaynağı için depolama erişim tuşu belirtir. Bu, yalnızca kaynak blob kapsayıcısı veya bir blobu ise gereklidir. |
| Hesap |**İsteğe bağlı**. Kopyalama işini çalıştırmak için Azure Data Lake Analytics hesabı kullanmak istiyorsanız, bunu kullanın. ApplicationTier/account seçeneği söz dizimi kullanın, ancak bir Data Lake Analytics hesabı belirtmezseniz, AdlCopy işi çalıştırmak için varsayılan hesabını kullanır. Bu seçeneği kullanırsanız, ayrıca, kaynak (Azure depolama Blob) ve hedef (Azure Data Lake Store) veri kaynakları olarak Data Lake Analytics hesabınız için eklemeniz gerekir. |
| Birimler |Kopyalama işlemi için kullanılacak Data Lake Analytics birim sayısını belirtir. Bu seçeneği kullanırsanız, zorunlu **/hesabını** Data Lake Analytics hesabı belirtmek için seçeneği. |
| düzeni |Hangi BLOB veya kopyalanacak dosyaların gösteren bir regex düzenidir belirtir. Büyük küçük harfe duyarlı eşleşen AdlCopy kullanır. Hiçbir düzeni belirtildiğinde kullanılan varsayılan düzen tüm öğeleri kopyalamaktır. Birden çok dosya desenlerinin belirtilmesi desteklenmiyor. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (bağımsız) bir Azure Storage blobundan verileri kopyalamak için kullanın.
1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizinine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Belirli bir blobu bir Data Lake Store'a kaynak kapsayıcıdan kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Örneğin:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] Aşağıdaki söz dizimini Data Lake Store hesabındaki bir klasöre kopyalanacak dosyayı belirtir. Belirtilen klasör adı yoksa, AdlCopy aracı bir klasör oluşturur.

    Data Lake Store hesabınızın altında elinizde Azure aboneliği için kimlik bilgilerini girmeniz istenir. Aşağıdakine benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Ayrıca aşağıdaki komutu kullanarak Data Lake Store hesabına bir kapsayıcıdan tüm BLOB'lar kopyalayabilirsiniz:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Örneğin:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Bir Azure Blob Depolama hesabından kopyalıyorsanız blob depolama tarafında kopyalama sırasında kısıtlanan. Bu, kopyalama işini performansını bozar. Azure Blob Depolama sınırları hakkında daha fazla bilgi için bkz: Azure Storage sınırlarını adresindeki [Azure aboneliği ve hizmet sınırları](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>AdlCopy (bağımsız) başka bir Data Lake Store hesabından veri kopyalamak için kullanın.
AdlCopy, iki Data Lake Store hesapları arasında veri kopyalamak için de kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizinine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Belirli bir dosya bir Data Lake Store hesabından diğerine kopyalamak için aşağıdaki komutu çalıştırın.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Örneğin:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > Aşağıdaki söz dizimini Data Lake Store hesabı hedef klasöre kopyalanacak dosyayı belirtir. Belirtilen klasör adı yoksa, AdlCopy aracı bir klasör oluşturur.
   >
   >

    Data Lake Store hesabınızın altında elinizde Azure aboneliği için kimlik bilgilerini girmeniz istenir. Aşağıdakine benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. Aşağıdaki komutu bir Data Lake Store hesabı hedef klasöre Data Lake Store hesabı kaynak belirli bir klasördeki tüm dosyaları kopyalar.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

AdlCopy ayrı bir araç olarak kullanırken, kopya paylaşılan çalıştırıldığı, yönetilen Azure kaynakları. Bu ortamda alabilirsiniz performans sistem yükü ve kullanılabilir kaynakları bağlıdır. Bu mod, en iyi geçici düzenli olarak küçük aktarımlar için kullanılır. Hiçbir parametre AdlCopy ayrı bir araç olarak kullanırken ayarlanması gerekir.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>AdlCopy (Data Lake Analytics hesabıyla) verileri kopyalamak için kullanın.
Data Lake Analytics hesabı, Azure storage bloblarından Data Lake Store'a veri kopyalamak için AdlCopy işi çalıştırmak için de kullanabilirsiniz. Taşınacak veri gigabayt ve terabayt aralıkta olduğunda bu seçenek genellikle kullanırsınız ve daha iyi ve tahmin edilebilir performans verimlilik istiyor.

Bir Azure Storage Blobundan kopyalamak için Data Lake Analytics hesabınızı AdlCopy ile kullanmak için kaynak (Azure depolama Blob), Data Lake Analytics hesabı için bir veri kaynağı olarak eklenmelidir. Data Lake Analytics hesabınızı ek veri kaynaklarını ekleme ile ilgili yönergeler için bkz: [yönetmek Data Lake Analytics hesabı veri kaynaklarını](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Bir Data Lake Analytics hesabı kullanarak kaynağı olarak bir Azure Data Lake Store hesabından kopyalıyorsanız, Data Lake Store hesabını Data Lake Analytics hesabıyla ilişkilendirmek gerekmez. Yalnızca kaynak bir Azure Storage hesabı olduğunda kaynak deposu Data Lake Analytics hesabıyla ilişkilendirmek için gereksinimdir.
>
>

Bir Azure Storage blobundan Data Lake Analytics hesabı kullanarak bir Data Lake Store hesabına kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Örneğin:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Benzer şekilde, Data Lake Store hesabı kaynak belirli bir klasördeki tüm dosyaları hedef Data Lake Store hesabını Data Lake Analytics hesabı kullanarak bir klasöre kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Veri terabayt aralığında kopyalarken, kendi Azure Data Lake Analytics hesabıyla AdlCopy kullanarak daha iyi ve daha tahmin edilebilir performans sağlar. Ayarlanması parametresi için kopyalama işini kullanmak için Azure Data Lake Analytics birimleri sayısıdır. Birim sayısını artırmayı kopyalama işini performansını artırır. Kopyalanacak her dosya en fazla bir birimi kullanabilirsiniz. Kopyalanan dosyalar sayısından daha fazla birimi belirtme performans da artar.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Desen eşleştirme kullanarak verileri kopyalamak için AdlCopy kullanın
Bu bölümde, bir kaynaktan veri kopyalamak için AdlCopy kullanmayı öğrenin (Bizim örneğimizde Azure Storage Blobuna kullanırız) desen eşleştirme kullanarak bir hedef Data Lake Store hesabına. Örneğin, hedef kaynak blobundan .csv uzantısına sahip tüm dosyaları kopyalamak için aşağıdaki adımları kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizinine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Kaynak kapsayıcısından belirli bir blobu bir Data Lake Store'a *.csv uzantılı tüm dosyaları kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Örneğin:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Faturalandırma
* Tek başına olarak AdlCopy Aracı'nı kullanırsanız, Azure depolama hesabı kaynağı Data Lake Store ile aynı bölgede değilse, veri taşıma çıkışı maliyeti için Fatura edilecek.
* Data Lake Analytics ile AdlCopy aracı kullanırsanız, standart hesap [Data Lake Analytics oranları faturalama](https://azure.microsoft.com/pricing/details/data-lake-analytics/) uygulanır.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy kullanma konuları
* AdlCopy (için sürüm 1.0.5), topluca binlerce dosyaları ve klasörleri birden fazla sahip kaynaklardan veri kopyalamayı destekler. Ancak, büyük bir veri kümesini kopyalama sorunlarla karşılaşırsanız, dosyaların/klasörlerin farklı alt klasörler halinde dağıtabilir ve yolun bu alt klasörleri kaynağı olarak kullanın.

## <a name="performance-considerations-for-using-adlcopy"></a>Başarım Değerlendirmeleri AdlCopy kullanma

AdlCopy binlerce dosyaları ve klasörleri içeren veri kopyalamayı destekler. Ancak, büyük bir veri kümesini kopyalama sorunlarla karşılaşırsanız, daha küçük alt klasörler halinde dosyaların/klasörlerin dağıtabilirsiniz. AdlCopy için geçici kopya oluşturuldu. Yinelenen aralıklarla verileri kopyalamak çalışıyorsanız kullanmayı düşünmelisiniz [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) kopyalama işlemleri geçici tam yönetim sağlar.

## <a name="release-notes"></a>Sürüm notları
* 1.0.13 - birden çok adlcopy komutları arasında aynı Azure Data Lake Store hesabına veri kopyalıyorsanız gerektirmeyen her çalışma için kimlik bilgilerinizi yeniden girmek artık. Adlcopy artık birden çok çalıştırmaları arasında bu bilgileri önbelleğe alır.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
