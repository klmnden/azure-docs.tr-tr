---
title: Azure Data Lake depolama Gen1 Azure depolama Bloblarından veri kopyalama | Microsoft Docs
description: Azure Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalamak için AdlCopy aracını kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: be66fd51b37c0e62b2b757a88ee1db9319b2093a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60878843"
---
# <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalama
> [!div class="op_single_selector"]
> * [DistCp’yi kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy’yi kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake depolama Gen1 sağlayan bir komut satırı aracı [AdlCopy](https://aka.ms/downloadadlcopy), aşağıdaki kaynaklardan gelen verileri kopyalamak için:

* Data Lake depolama Gen1 içine Azure depolama Bloblarından. Azure depolama BLOB'ları için Data Lake depolama Gen1 verileri kopyalamak için AdlCopy kullanamazsınız.
* İki Azure Data Lake depolama Gen1 hesapları arasında.

Ayrıca, iki farklı modda AdlCopy Aracı'nı kullanabilirsiniz:

* **Tek başına**, burada aracı görevi gerçekleştirmek için Data Lake depolama Gen1 kaynaklarını kullanır.
* **Bir Data Lake Analytics hesabı kullanarak**kopyalama işlemini gerçekleştirmek için Data Lake Analytics hesabınıza atanmış birimleri nerede kullanılır. Bu seçenek, tahmin edilebilir bir biçimde kopyalama görevleri gerçekleştirmek için ararken kullanmak isteyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaleye başlamadan önce aşağıdakilere sahip olmanız ve aşağıdaki işlemleri yapmış olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure depolama BLOB'ları** bazı verileri içeren kapsayıcı.
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **(İsteğe bağlı) Azure Data Lake Analytics hesabı** -bkz [Azure Data Lake Analytics ile çalışmaya başlama](../data-lake-analytics/data-lake-analytics-get-started-portal.md) bir Data Lake Analytics hesabı oluşturmak yönergeler.
* **AdlCopy aracı**. AdlCopy aracını yükleme [ https://aka.ms/downloadadlcopy ](https://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>AdlCopy aracının sözdizimi
AdlCopy aracı ile çalışmak için aşağıdaki sözdizimini kullanın.

    AdlCopy /Source <Blob or Data Lake Storage Gen1 source> /Dest <Data Lake Storage Gen1 destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Units <Number of Analytics units> /Pattern

Söz dizimi parametrelerinde aşağıda açıklanmıştır:

| Seçenek | Açıklama |
| --- | --- |
| Kaynak |Azure depolama blobunda kaynak verilerin konumu belirtir. Kaynak, bir blob kapsayıcı, blob veya başka bir Data Lake depolama Gen1 hesabı olabilir. |
| Hedef |Kopyalamak için Data Lake depolama Gen1 hedef belirtir. |
| SourceKey |Azure depolama blob kaynağı için depolama erişim anahtarını belirtir. Bu, yalnızca kaynak blob bir kapsayıcı veya blob ise gereklidir. |
| Hesap |**İsteğe bağlı**. Kopyalama işini çalıştırmak için Azure Data Lake Analytics hesabı kullanmak istiyorsanız bunu kullanın. / Account seçeneği sözdiziminde kullanın, ancak bir Data Lake Analytics hesabı belirtmeyin AdlCopy işi çalıştırmak için bir varsayılan hesabı kullanır. Bu seçeneği kullanırsanız, ayrıca, kaynak (Azure Blob Depolama) hem de hedef (Azure Data Lake depolama Gen1) veri kaynakları olarak Data Lake Analytics hesabınız için eklemeniz gerekir. |
| Birimler |Kopyalama işlemi için kullanılacak olan Data Lake Analytics birimi sayısını belirtir. Bu seçeneği kullanırsanız zorunludur **/hesabı** Data Lake Analytics hesabı belirtmek için seçeneği. |
| Desen |Hangi BLOB veya kopyalanacak dosyaları belirten bir normal ifade desenini belirtir. AdlCopy büyük küçük harf duyarlı eşleme kullanır. Tüm öğeleri herhangi bir desen belirtildiğinde kullanılan varsayılan düzen kopyalamaktır. Birden çok dosya desenlerinin belirtilmesi desteklenmiyor. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (tek başına) bir Azure depolama blobu'ndan veri kopyalamak için kullanın.
1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Belirli bir blobu kaynak kapsayıcıdan bir Data Lake depolama Gen1 klasörüne kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Örneğin:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[!NOTE] 
    >Aşağıdaki söz dizimini Data Lake depolama Gen1 hesabındaki bir klasöre kopyalanacak dosyasını belirtir. Belirtilen klasör adı yoksa, AdlCopy aracı bir klasör oluşturur.

    Data Lake depolama Gen1 hesabınızın sahip olması için Azure aboneliği kimlik bilgilerini girmeniz istenir. Aşağıdakine benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Ayrıca, tüm bloblar bir kapsayıcıdan aşağıdaki komutu kullanarak Data Lake depolama Gen1 hesaba kopyalayabilirsiniz:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Örneğin:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Bir Azure Blob Depolama hesabından kopyalanıyorsa, blob depolama tarafında kopyalama sırasında kısıtlanmış. Bu, kopyalama işinizin performansını bozar. Azure Blob Depolama sınırları hakkında daha fazla bilgi için Azure depolama sınırları görmek [Azure aboneliği ve hizmet sınırlamaları](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-storage-gen1-account"></a>AdlCopy (tek başına) başka bir Data Lake depolama Gen1 hesabından veri kopyalamak için kullanın.
Data Lake depolama Gen1 iki hesap arasında veri kopyalamak için AdlCopy de kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Belirli bir dosyayı başka bir Data Lake depolama Gen1 hesabından diğerine kopyalamak için aşağıdaki komutu çalıştırın.

        AdlCopy /Source adl://<source_adlsg1_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adlsg1_account>.azuredatalakestore.net/<path>/

    Örneğin:

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > Aşağıdaki söz dizimini Data Lake depolama Gen1 hesabı hedef bir klasöre kopyalanacak dosyasını belirtir. Belirtilen klasör adı yoksa, AdlCopy aracı bir klasör oluşturur.
   >
   >

    Data Lake depolama Gen1 hesabınızın sahip olması için Azure aboneliği kimlik bilgilerini girmeniz istenir. Aşağıdakine benzer bir çıktı görürsünüz:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. Aşağıdaki komutu tüm dosyaları kaynak Data Lake depolama Gen1 hesabını belirli bir klasörde Data Lake depolama Gen1 hesabı hedef bir klasöre kopyalar.

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

AdlCopy bağımsız bir araç olarak kullanırken, kopya paylaşılan çalıştırıldığı, Azure yönetilen kaynakları. Bu ortamda alabilirsiniz performans sistem yüküne ve kullanılabilir kaynaklar bağlıdır. Bu mod, en iyi geçici olarak küçük aktarımları için kullanılır. Parametresiz AdlCopy bağımsız bir araç olarak kullanılırken ayarlanması gerekir.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Veri kopyalama (Data Lake Analytics hesabı ile) AdlCopy kullanma
Data Lake Analytics hesabınızı, Data Lake depolama Gen1 için Azure depolama bloblarından veri kopyalamak için AdlCopy işi çalıştırmak için de kullanabilirsiniz. Genellikle taşınacak veri gigabayt ve terabayt aralığında olduğunda bu seçeneği kullanırsınız ve daha iyi ve öngörülebilir performans verimliliği istiyorsunuz.

Data Lake Analytics hesabınızı bir Azure Storage Blobundan kopyalamak için AdlCopy ile kullanmak için (Azure depolama blobu) kaynak Data Lake Analytics hesabınız için bir veri kaynağı olarak eklenmiş olması gerekir. Data Lake Analytics hesabınıza ek veri kaynaklarını ekleme ile ilgili yönergeler için bkz: [yönetme Data Lake Analytics hesabı veri kaynaklarını](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Bir Data Lake Analytics hesabı kullanarak kaynağı olarak bir Azure Data Lake depolama Gen1 hesabından kopyalıyorsanız Data Lake depolama Gen1 hesabını Data Lake Analytics hesabı ile ilişkilendirmek gerekmez. Kaynak deposu Data Lake Analytics hesabıyla ilişkilendirmek için gereksinim yalnızca kaynak bir Azure depolama hesabı olur.
>
>

Bir Azure depolama blobundan Data Lake Analytics hesabı kullanarak bir Data Lake depolama Gen1 hesabına kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Units <number_of_data_lake_analytics_units_to_be_used>

Örneğin:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Benzer şekilde, kaynak Data Lake depolama Gen1 hesabını belirli bir klasörde tüm dosyaları hedef Data Lake depolama Gen1 hesabını kullanarak Data Lake Analytics hesabı bir klasöre kopyalamak için aşağıdaki komutu çalıştırın:

    AdlCopy /Source adl://mysourcedatalakestorage.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastorage.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Veri terabayt aralığında kopyalarken, kendi Azure Data Lake Analytics hesabınızla AdlCopy kullanarak daha iyi ve daha öngörülebilir bir performans sağlar. Ayarlanmalıdır parametresi için bir kopyalama işi kullanmak için Azure Data Lake analiz birimi sayısıdır. Birimi sayısını artırabilir, kopya işinizin performansını artırır. Kopyalanacak her dosya en fazla bir birimi kullanabilirsiniz. Kopyalanan dosyalar sayısından daha fazla birimi belirtme performans artırmaz.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Desen eşleştirme kullanarak verileri kopyalamak için AdlCopy kullanma
Bu bölümde, bir kaynaktan veri kopyalamak için AdlCopy kullanma konusunda bilgi edinin (Bizim örneğimizde biz, Azure depolama Blob'kullanın) desen eşleştirme kullanarak Data Lake depolama Gen1 hedef hesabı için. Örneğin, .csv uzantılı tüm dosyaları kaynak blob bir hedefe kopyalamak için aşağıdaki adımları kullanabilirsiniz.

1. Bir komut istemi açın ve AdlCopy yüklü olduğu, genellikle dizine gidin `%HOMEPATH%\Documents\adlcopy`.
2. Belirli bir kaynak kapsayıcı blobundan Data Lake depolama Gen1 klasöre *.csv uzantılı tüm dosyaları kopyalamak için aşağıdaki komutu çalıştırın:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Örneğin:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Faturalandırma
* Tek başına olarak AdlCopy aracı kullanırsanız, kaynak Azure depolama hesabını, Data Lake depolama Gen1 hesabıyla aynı bölgede değil, verileri taşımak için kullanım maliyetleri için faturalandırılırsınız.
* AdlCopy Aracı'nı kullanarak Data Lake Analytics ile kullanırsanız, standart hesap [Data Lake Analytics ücretler fatura](https://azure.microsoft.com/pricing/details/data-lake-analytics/) uygulanır.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy kullanma konuları
* AdlCopy (için sürüm 1.0.5), birden çok dosya ve klasörleri binlerce topluca sahip kaynaklardan veri kopyalamayı destekler. Ancak, büyük bir veri kümesi kopyalama sorunlarla karşılaşırsanız, dosyaların/klasörlerin farklı alt klasörler halinde dağıtın ve kaynak olarak bu alt klasörleri için yol yerine kullanın.

## <a name="performance-considerations-for-using-adlcopy"></a>AdlCopy kullanma performansla ilgili önemli noktalar

AdlCopy binlerce dosya ve klasörleri içeren veri kopyalamayı destekler. Ancak, büyük bir veri kümesi kopyalama sorunlarla karşılaşırsanız, daha küçük bir alt klasörler halinde dosyaların/klasörlerin dağıtabilirsiniz. AdlCopy geçici kopya için oluşturulmuştur. Yinelenen bir düzende veri kopyalamak çalışıyorsanız kullanmayı düşünmelisiniz [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) kopyalama işlemleri geçici olarak tam yönetim sağlar.

## <a name="release-notes"></a>Sürüm notları
* 1.0.13 - birden çok adlcopy komutları arasında aynı Azure Data Lake depolama Gen1 hesaba veri kopyalıyorsanız gerektirmeyen her çalıştırma için kimlik bilgilerinizi yeniden girmeniz artık. Adlcopy artık birden fazla çalıştırma sonucunda bu bilgileri önbelleğe alır.

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
