---
title: Şirket içinde HDFS depolamak için Azure depolama veri geçirmek üzere Azure Data Box kullanın
description: Verileri şirket içi HDFS Mağazası'ndan Azure Depolama'ya geçirin.
services: storage
author: normesta
ms.service: storage
ms.date: 03/01/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: d0908e9edce8efb7a378ee04b6076b61cae2d2bf
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998303"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Azure depolama için bir şirket içi HDFS Mağazası'ndan veri taşımak için Azure Data Box'ı kullanın

Data Box cihaz kullanarak Azure Depolama'ya (blob depolama veya Data Lake depolama Gen2'ye) Hadoop kümenizin bir şirket içi HDFS deposundaki verileri geçirebilirsiniz.

Bu makale, şu görevleri tamamlamanıza yardımcı olur:

:heavy_check_mark: Verilerinizi bir Data Box cihazına kopyalayın.

:heavy_check_mark: Data Box cihazı Microsoft'a gönderin.

:heavy_check_mark: Verileri Data Lake depolama Gen2'ye depolama hesabınız üzerine taşıyın.

## <a name="prerequisites"></a>Önkoşullar

Bunlar, geçişi tamamlamak için ihtiyacınız vardır.

* Bir Azure depolama hesabı **değil** sahip hiyerarşik ad alanları üzerinde etkin.

* Azure Data Lake depolama Gen2 hesabı kullanmak istiyorsanız (bir depolama hesabı **mu** hiyerarşik ad alanları üzerinde etkin olan), sonra da bu noktada oluşturmak isteyebilirsiniz.

* Kaynak verileri içeren bir şirket içi Hadoop kümesi.

* Bir [Azure Data Box cihazınızın](https://azure.microsoft.com/services/storage/databox/). 

    - [Data Box'ınızı sipariş](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered). Bir depolama hesabı seçmek Box'ınızı sıralama sırasında unutmayın **değil** sahip hiyerarşik ad alanları üzerinde etkin. Azure Data Lake depolama Gen2 içine doğrudan alma Data Box henüz desteklemiyor olmasıdır. Bir depolama hesabına kopyalayın ve ardından ikinci bir kopyasını Gen2 ADLS hesabına yapmak gerekir. Yönergeler için bu adımları verilmiştir.
    - [Data Box'ınızı bağlanmak ve kablo](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) bir şirket içi ağa.

Hazır olduğunuzda başlayalım.

## <a name="copy-your-data-to-a-data-box-device"></a>Bir Data Box cihazına veri kopyalama

Bir Data Box cihazına, şirket içi HDFS deposundan veri kopyalamak için birkaç şey ayarlayabilir ve ardından [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) aracı.

Kopyalamakta olduğunuz veri miktarı tek bir veri kutusu kapasitesini birden fazla ise, Veri kümenizi, veri kutularına uyan boyutları halinde bölmeniz gerekir.

Data Box'ınızı için REST API'leri, Blob/nesne depolama aracılığıyla veri kopyalamak için aşağıdaki adımları izleyin. REST API Arabirimi Data Box, küme için HDFS deposu olarak görünür hale getirir. 


1. Data Box REST arabirimi bağlanmak için güvenlik ve bağlantı temelleri REST aracılığıyla veri kopyalamadan önce belirleyin. Yerel web kullanıcı Arabirimi, Data Box oturum açıp **Bağlan ve Kopyala** sayfası. Karşı Azure depolama hesabı, Data Box'a altında **erişim ayarları**bulup seçin **REST(Preview)**.

    !["Bağlan ve Kopyala" sayfası](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. Depolama hesabındaki erişim ve karşıya yükleme, kopyalama veri iletişim **Blob Hizmeti uç noktası** ve **depolama hesabı anahtarı**. Blob Hizmeti uç noktasından atlamak `https://` ve sonunda eğik çizgi.

    Bu durumda, uç noktadır: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. Kullanacağınız URI ana bilgisayarı bölümüdür: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Bir örnek için bkz. nasıl [http üzerinden REST Bağlan](/azure/databox/data-box-deploy-copy-data-via-rest). 

     !["Depolama hesabına erişme ve verileri karşıya yükleme" iletişim kutusu](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Uç nokta ekleyip veri kutusu IP adresine `/etc/hosts` her düğümde.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```
    DNS için başka bir mekanizma kullanıyorsanız, Data Box uç çözümlenebildiğinden emin olmalısınız.
    
3. Bir kabuk değişkeni `azjars` işaret edecek şekilde `hadoop-azure` ve `microsoft-windowsazure-storage-sdk` jar dosyaları. Bu dosyalar Hadoop yükleme dizininde (Bu komutu kullanarak bu dosyaları mevcut olup olmadığını kontrol edebilirsiniz `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure` burada `<hadoop_install_dir>` Hadoop yüklediğiniz dizindir) tam yolları kullan. 
    
    ```
    # azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar
    # azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar
    ```

4. Verileri Hadoop HDFS veri kutusu Blob depolama alanına kopyalayın.

    ```
    # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -strategy dynamic -m 4 -update \
    [source_directory] \
           wasb://[container_name]@[blob_service_endpoint]/[destination_folder]       
    ```
   `-libjars` Yapma seçeneği kullanıldığında `hadoop-azure*.jar` ve bağımlı `azure-storage*.jar` kullanılabilir dosyaları `distcp`. Bu, zaten başka bazı kümeler için oluşabilir.
   
   Aşağıdaki örnekte gösterildiği nasıl `distcp` komutu, verileri kopyalamak için kullanılır.
   
   ```
   # hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -strategy dynamic -m 4 -update \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/testfiles
   ```
  
Kopyalama hızını artırmak için:
- Azaltıcının sayısını değiştirmeyi deneyin. (Yukarıdaki örnekte `m` = 4 azaltıcının.)
- Birden fazla çalıştırmayı deneyin `distcp` paralel.
- Büyük dosyaları daha küçük dosyalar iyi gerçekleştireceğini unutmayın.       
    
## <a name="ship-the-data-box-to-microsoft"></a>Data Box Microsoft'a gönderin

Microsoft Data Box cihaz hazırlayıp için bu adımları izleyin.

1. Veri kopyalama işlemi tamamlandıktan sonra çalıştırın [göndermeye hazırlama](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest) Data Box'ınızı üzerinde. Cihaz hazırlığı tamamlandıktan sonra ürün reçetesi dosyalarını indirin. Bu ürün reçetesi veya bildirim dosyaları daha sonra verileri Azure'a karşıya doğrulamak için zamanlanacak. Cihazı kapatın ve kabloların kaldırın. 
2.  Bir toplama için UPS ile zamanlama [Data Box'ınızı azure'a geri gönderin](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up). 
3.  Ne zaman Cihazınızı Microsoft alır ve ağ veri merkezine bağlı olduğu (devre dışı hiyerarşik ad alanları ile) belirtilen depolama hesabı için veriler karşıya yüklendikten sonra Data Box sipariş. BOM dosyalara karşı tüm verileri Azure'a karşıya yüklendiğini doğrulayın. Artık bu veri bir Data Lake depolama Gen2'ye depolama hesabına geçebilirsiniz.

## <a name="move-the-data-onto-your-data-lake-storage-gen2-storage-account"></a>Data Lake depolama Gen2'ye depolama hesabınız oturum verileri taşıma

Azure Data Lake depolama Gen2'ye veri deponuz olarak kullanıyorsanız bu adım gereklidir. Yalnızca bir blob depolama hesabı hiyerarşik ad alanı olmayan veri deponuz olarak kullanıyorsanız, bu adımı gerekmez.

2 yollarla bunu yapabilirsiniz. 

- Kullanım [verileri ADLS Gen2'ye taşımak için Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Belirtmek zorunda kalacaksınız **Azure Blob Depolama** kaynağı olarak.

- Azure tabanlı bir Hadoop kümenizi kullanın. Bu DistCp komutu çalıştırabilirsiniz:

    ```bash
    hadoop distcp -Dfs.azure.account.key.{source_account}.dfs.windows.net={source_account_key} abfs://{source_container} @{source_account}.dfs.windows.net/[path] abfs://{dest_container}@{dest_account}.dfs.windows.net/[path]
    ```

Bu komut hem verileri hem de meta verileri depolama hesabınızdan Data Lake depolama Gen2'ye depolama hesabınıza kopyalar.

## <a name="next-steps"></a>Sonraki adımlar

Data Lake depolama Gen2 HDInsight kümeleri ile nasıl çalıştığını öğrenin. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
