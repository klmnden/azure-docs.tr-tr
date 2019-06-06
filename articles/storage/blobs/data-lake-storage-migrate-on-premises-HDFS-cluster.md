---
title: Şirket içinde HDFS depolamak için Azure depolama veri geçirmek üzere Azure Data Box kullanın
description: Verileri şirket içi HDFS Mağazası'ndan Azure Depolama'ya geçirin.
services: storage
author: normesta
ms.service: storage
ms.date: 06/05/2019
ms.author: normesta
ms.topic: article
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9a42135df38cde91cc6626a3f7d0328334af0a5d
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66729059"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Azure depolama için bir şirket içi HDFS Mağazası'ndan veri taşımak için Azure Data Box'ı kullanın

Data Box cihaz kullanarak Azure Depolama'ya (blob depolama veya Data Lake depolama Gen2'ye) Hadoop kümenizin bir şirket içi HDFS deposundaki verileri geçirebilirsiniz. Bir 80 TB Data Box veya 770 TB veri kutusu ağır seçebilirsiniz.

Bu makale, şu görevleri tamamlamanıza yardımcı olur:

:heavy_check_mark: Data Box veya veri kutusu ağır bir cihaza veri kopyalayın.

:heavy_check_mark: Cihaz Microsoft'a gönderin.

:heavy_check_mark: Verileri Data Lake depolama Gen2'ye depolama hesabınız üzerine taşıyın.

## <a name="prerequisites"></a>Önkoşullar

Bunlar, geçişi tamamlamak için ihtiyacınız vardır.

* Bir Azure depolama hesabı **değil** sahip hiyerarşik ad alanları üzerinde etkin.

* Azure Data Lake depolama Gen2 hesabı kullanmak istiyorsanız (bir depolama hesabı **mu** hiyerarşik ad alanları üzerinde etkin olan), sonra da bu noktada oluşturmak isteyebilirsiniz.

* Kaynak verileri içeren bir şirket içi Hadoop kümesi.

* Bir [Azure Data Box cihazınızın](https://azure.microsoft.com/services/storage/databox/).

    - [Data Box'ınızı sipariş](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) veya [veri kutusu ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Bir depolama hesabı seçmek Cihazınızı sıralama sırasında unutmayın **değil** sahip hiyerarşik ad alanları üzerinde etkin. Data Box cihaz henüz Azure Data Lake depolama Gen2 içine doğrudan alma desteği olmayan olmasıdır. Bir depolama hesabına kopyalayın ve ardından ikinci bir kopyasını Gen2 ADLS hesabına yapmak gerekir. Yönergeler için bu adımları verilmiştir.
    - Kablo ve bağlantı, [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) veya [veri kutusu ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) bir şirket içi ağa.

Hazır olduğunuzda başlayalım.

## <a name="copy-your-data-to-a-data-box-device"></a>Bir Data Box cihazına veri kopyalama

Bir Data Box cihazına, şirket içi HDFS deposundan veri kopyalamak için birkaç şey ayarlayabilir ve ardından [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) aracı.

Kopyalamakta olduğunuz veri miktarı tek bir veri kutusu kapasitesi veya veri kutusu ağır tek düğümde, birden fazla ise, Veri kümenizi cihazlarınızı sığması boyutları halinde bölmeniz.

Data Box cihazınıza REST API'leri, Blob/nesne depolama aracılığıyla veri kopyalamak için aşağıdaki adımları izleyin. REST API Arabirimi cihaz kümenize bir HDFS deposu olarak görünür hale getirir. 


1. Data Box veya veri kutusu ağır REST arabirimini bağlanmak için güvenlik ve bağlantı temelleri REST aracılığıyla veri kopyalamadan önce belirleyin. Yerel web kullanıcı Arabirimi, Data Box oturum açıp **Bağlan ve Kopyala** sayfası. Azure depolama hesabı cihazınız için altında **erişim ayarları**bulun ve seçin **REST**.

    !["Bağlan ve Kopyala" sayfası](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connect-rest.png)

2. Depolama hesabındaki erişim ve karşıya yükleme, kopyalama veri iletişim **Blob Hizmeti uç noktası** ve **depolama hesabı anahtarı**. Blob Hizmeti uç noktasından atlamak `https://` ve sonunda eğik çizgi.

    Bu durumda, uç noktadır: `https://mystorageaccount.blob.mydataboxno.microsoftdatabox.com/`. Kullanacağınız URI ana bilgisayarı bölümüdür: `mystorageaccount.blob.mydataboxno.microsoftdatabox.com`. Bir örnek için bkz. nasıl [http üzerinden REST Bağlan](/azure/databox/data-box-deploy-copy-data-via-rest). 

     !["Depolama hesabına erişme ve verileri karşıya yükleme" iletişim kutusu](media/data-lake-storage-migrate-on-premises-HDFS-cluster/data-box-connection-string-http.png)

3. Uç nokta ekleyip Data Box veya veri kutusu ağır düğüm IP adresine `/etc/hosts` her düğümde.

    ```    
    10.128.5.42  mystorageaccount.blob.mydataboxno.microsoftdatabox.com
    ```
    DNS için başka bir mekanizma kullanıyorsanız, Data Box uç çözümlenebildiğinden emin olmalısınız.
    
4. Bir kabuk değişkeni `azjars` işaret edecek şekilde `hadoop-azure` ve `microsoft-windowsazure-storage-sdk` jar dosyaları. Bu dosyalar Hadoop yükleme dizininde (Bu komutu kullanarak bu dosyaları mevcut olup olmadığını kontrol edebilirsiniz `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure` burada `<hadoop_install_dir>` Hadoop yüklediğiniz dizindir) tam yolları kullan. 
    
    ```
    # azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar
    # azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar
    ```

5. Veri kopyalama için kullanmak istediğiniz depolama kapsayıcısı oluşturun. Ayrıca, bu komut bir parçası olarak bir hedef klasör belirtmeniz gerekir. Bu noktada bu bir işlevsiz bir hedef klasör olabilir.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -mkdir -p  wasb://[container_name]@[blob_service_endpoint]/[destination_folder]
    ```

6. Kapsayıcı ve klasöre oluşturulduğundan emin olmak için bir liste komutunu çalıştırın.

    ```
    # hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.[blob_service_endpoint]=[account_key] \
    -ls -R  wasb://[container_name]@[blob_service_endpoint]/
    ```

7. Verileri Hadoop HDFS, daha önce oluşturduğunuz kapsayıcıya veri kutusu Blob depolama alanına kopyalayın. İçine kopyaladığınız klasörü bulunmazsa komutu otomatik olarak oluşturur.

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
- Birden çok çalıştırmayı deneyin `distcp` paralel.
- Büyük dosyaları daha küçük dosyalar iyi gerçekleştireceğini unutmayın.
    
## <a name="ship-the-data-box-to-microsoft"></a>Data Box Microsoft'a gönderin

Microsoft Data Box cihaz hazırlayıp için bu adımları izleyin.

1. Veri kopyalama işlemi tamamlandıktan sonra çalıştırın:
    
    - [Data Box veya veri kutusu ağır göndermeye hazırlama](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).
    - Cihaz hazırlığı tamamlandıktan sonra ürün reçetesi dosyalarını indirin. Bu ürün reçetesi veya bildirim dosyaları daha sonra verileri Azure'a karşıya doğrulamak için zamanlanacak. 
    - Cihazı kapatın ve kabloların kaldırın.
2.  UPS ile bir toplama zamanlayın. Yönergeleri izleyin:

    - [Data Box'ınızı gönderin](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up) 
    - [Veri kutusu ağır sevk](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).
3.  Ne zaman Cihazınızı Microsoft alır, veri merkezi ağa bağlı ve verileri (devre dışı hiyerarşik ad alanları ile) belirtilen depolama hesabına yüklendikten sonra cihaz siparişi veren. BOM dosyalara karşı tüm verileri Azure'a karşıya yüklendiğini doğrulayın. Artık bu veri bir Data Lake depolama Gen2'ye depolama hesabına geçebilirsiniz.


## <a name="move-the-data-onto-your-data-lake-storage-gen2-storage-account"></a>Data Lake depolama Gen2'ye depolama hesabınız oturum verileri taşıma

Azure Data Lake depolama Gen2'ye veri deponuz olarak kullanıyorsanız bu adım gereklidir. Yalnızca bir blob depolama hesabı hiyerarşik ad alanı olmayan veri deponuz olarak kullanıyorsanız, bu adımı gerekmez.

İki yolla bunu yapabilirsiniz.

- Kullanım [verileri ADLS Gen2'ye taşımak için Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Belirtmek zorunda kalacaksınız **Azure Blob Depolama** kaynağı olarak.

- Azure tabanlı bir Hadoop kümenizi kullanın. Bu DistCp komutu çalıştırabilirsiniz:

    ```bash
    hadoop distcp -Dfs.azure.account.key.{source_account}.dfs.windows.net={source_account_key} abfs://{source_container} @{source_account}.dfs.windows.net/[path] abfs://{dest_container}@{dest_account}.dfs.windows.net/[path]
    ```

Bu komut hem verileri hem de meta verileri depolama hesabınızdan Data Lake depolama Gen2'ye depolama hesabınıza kopyalar.

## <a name="next-steps"></a>Sonraki adımlar

Data Lake depolama Gen2 HDInsight kümeleri ile nasıl çalıştığını öğrenin. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
