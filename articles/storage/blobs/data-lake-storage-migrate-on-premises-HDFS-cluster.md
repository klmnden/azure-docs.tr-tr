---
title: Şirket içinde HDFS depolamak için Azure depolama veri geçirmek üzere Azure Data Box kullanın
description: Verileri şirket içi HDFS Mağazası'ndan Azure Depolama'ya geçirin.
services: storage
author: normesta
ms.service: storage
ms.date: 06/11/2019
ms.author: normesta
ms.topic: article
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 4445a8566c04d30cfb8743cbd33623f2e23f0dde
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595390"
---
# <a name="use-azure-data-box-to-migrate-data-from-an-on-premises-hdfs-store-to-azure-storage"></a>Azure depolama için bir şirket içi HDFS Mağazası'ndan veri taşımak için Azure Data Box'ı kullanın

Data Box cihaz kullanarak Azure Depolama'ya (blob depolama veya Data Lake depolama Gen2'ye) Hadoop kümenizin bir şirket içi HDFS deposundaki verileri geçirebilirsiniz. Bir 80 TB Data Box veya 770 TB veri kutusu ağır seçebilirsiniz.

Bu makale, şu görevleri tamamlamanıza yardımcı olur:

> [!div class="checklist"]
> * Verilerinizi geçirme hazırlayın.
> * Data Box veya veri kutusu ağır bir cihaza veri kopyalayın.
> * Cihaz Microsoft'a gönderin.
> * Verileri Data Lake depolama Gen2'ye üzerine taşıyın.

## <a name="prerequisites"></a>Önkoşullar

Bunlar, geçişi tamamlamak için ihtiyacınız vardır.

* İki depolama hesabı; hiyerarşik ad alanı üzerinde etkin olan tek ve olmayan bir.

* Kaynak verileri içeren bir şirket içi Hadoop kümesi.

* Bir [Azure Data Box cihazınızın](https://azure.microsoft.com/services/storage/databox/).

  * [Data Box'ınızı sipariş](https://docs.microsoft.com/azure/databox/data-box-deploy-ordered) veya [veri kutusu ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-ordered). Bir depolama hesabı seçmek Cihazınızı sıralama sırasında unutmayın **değil** sahip hiyerarşik ad alanları üzerinde etkin. Data Box cihaz henüz Azure Data Lake depolama Gen2 içine doğrudan alma desteği olmayan olmasıdır. Bir depolama hesabına kopyalayın ve ardından ikinci bir kopyasını Gen2 ADLS hesabına yapmak gerekir. Yönergeler için bu adımları verilmiştir.

  * Kablo ve bağlantı, [Data Box](https://docs.microsoft.com/azure/databox/data-box-deploy-set-up) veya [veri kutusu ağır](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-set-up) bir şirket içi ağa.

Hazır olduğunuzda başlayalım.

## <a name="copy-your-data-to-a-data-box-device"></a>Bir Data Box cihazına veri kopyalama

Verilerinizi tek bir Data Box cihaz uyuyorsa, Data Box cihaza veri kopyalayın. 

Veri boyutu Data Box cihazınızın kapasitesini aşıyorsa kullanın, ardından [verileri Data Box cihazlar arasında bölmek için isteğe bağlı yordamı](#appendix-split-data-across-multiple-data-box-devices) ve bu adımı gerçekleştirin. 

Bir Data Box cihazına, şirket içi HDFS deposundan veri kopyalamak için birkaç şey ayarlayabilir ve ardından [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) aracı.

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

4. Kabuk değişkeni `azjars` konumunu `hadoop-azure` ve `azure-storage` jar dosyaları. Bu dosyalar Hadoop yükleme dizininin altında bulabilirsiniz.

    Bu dosyalar olup olmadığını belirlemek için aşağıdaki komutu kullanın: `ls -l $<hadoop_install_dir>/share/hadoop/tools/lib/ | grep azure`. Değiştirin `<hadoop_install_dir>` yüklediğiniz Hadoop dizine olan yolu ile yer tutucu. Tam yolları kullan emin olun.

    Örnekler:

    `azjars=$hadoop_install_dir/share/hadoop/tools/lib/hadoop-azure-2.6.0-cdh5.14.0.jar``azjars=$azjars,$hadoop_install_dir/share/hadoop/tools/lib/microsoft-windowsazure-storage-sdk-0.6.0.jar`

5. Veri kopyalama için kullanmak istediğiniz depolama kapsayıcısı oluşturun. Ayrıca, bu komut bir parçası olarak bir hedef dizin belirtmeniz gerekir. Bu noktada bu bir işlevsiz hedef dizin olabilir.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -mkdir -p  wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Değiştirin `<blob_service_endpoint>` blob Hizmeti uç noktanızı adıyla yer tutucu.

    * Değiştirin `<account_key>` hesabınızın erişim anahtarı ile yer tutucu.

    * Değiştirin `<container-name>` kapsayıcınızı adıyla yer tutucu.

    * Değiştirin `<destination_directory>` yer tutucu ile verilerinizi kopyalamak istediğiniz dizinin adı.

6. Kapsayıcı ve dizini oluşturulmuş emin olmak için bir liste komutunu çalıştırın.

    ```
    hadoop fs -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint>=<account_key> \
    -ls -R  wasb://<container_name>@<blob_service_endpoint>/
    ```

   * Değiştirin `<blob_service_endpoint>` blob Hizmeti uç noktanızı adıyla yer tutucu.

   * Değiştirin `<account_key>` hesabınızın erişim anahtarı ile yer tutucu.

   * Değiştirin `<container-name>` kapsayıcınızı adıyla yer tutucu.

7. Verileri Hadoop HDFS, daha önce oluşturduğunuz kapsayıcıya veri kutusu Blob depolama alanına kopyalayın. Kopyalamakta olduğunuz dizine bulunmazsa komutu otomatik olarak oluşturur.

    ```
    hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.<blob_service_endpoint<>=<account_key> \
    -filters <exclusion_filelist_file> \
    [-f filelist_file | /<source_directory> \
           wasb://<container_name>@<blob_service_endpoint>/<destination_directory>
    ```

    * Değiştirin `<blob_service_endpoint>` blob Hizmeti uç noktanızı adıyla yer tutucu.

    * Değiştirin `<account_key>` hesabınızın erişim anahtarı ile yer tutucu.

    * Değiştirin `<container-name>` kapsayıcınızı adıyla yer tutucu.

    * Değiştirin `<exlusion_filelist_file>` yer tutucu dosya dışlamaları listesini içeren dosyanın adı.

    * Değiştirin `<source_directory>` yer tutucu ile kopyalamak istediğiniz verileri içeren dizinin adı.

    * Değiştirin `<destination_directory>` yer tutucu ile verilerinizi kopyalamak istediğiniz dizinin adı.

    `-libjars` Yapma seçeneği kullanıldığında `hadoop-azure*.jar` ve bağımlı `azure-storage*.jar` kullanılabilir dosyaları `distcp`. Bu, zaten başka bazı kümeler için oluşabilir.

    Aşağıdaki örnekte gösterildiği nasıl `distcp` komutu, verileri kopyalamak için kullanılır.

    ```
     hadoop distcp \
    -libjars $azjars \
    -D fs.AbstractFileSystem.wasb.Impl=org.apache.hadoop.fs.azure.Wasb \
    -D fs.azure.account.key.mystorageaccount.blob.mydataboxno.microsoftdatabox.com=myaccountkey \
    -filter ./exclusions.lst -f /tmp/copylist1 -m 4 \
    /data/testfiles \
    wasb://hdfscontainer@mystorageaccount.blob.mydataboxno.microsoftdatabox.com/data
    ```
  
    Kopyalama hızını artırmak için:

    * Azaltıcının sayısını değiştirmeyi deneyin. (Yukarıdaki örnekte `m` = 4 azaltıcının.)

    * Birden çok çalıştırmayı deneyin `distcp` paralel.

    * Büyük dosyaları daha küçük dosyalar iyi gerçekleştireceğini unutmayın.

## <a name="ship-the-data-box-to-microsoft"></a>Data Box Microsoft'a gönderin

Microsoft Data Box cihaz hazırlayıp için bu adımları izleyin.

1. İlk olarak, [, Data Box veya veri kutusu ağır göndermeye hazırlama](https://docs.microsoft.com/azure/databox/data-box-deploy-copy-data-via-rest).

2. Cihaz hazırlığı tamamlandıktan sonra ürün reçetesi dosyalarını indirin. Bu ürün reçetesi veya bildirim dosyaları daha sonra verileri Azure'a karşıya doğrulamak için zamanlanacak.

3. Cihazı kapatın ve kabloların kaldırın.

4. UPS ile bir toplama zamanlayın.

    * Data Box cihazlar için bkz [Data Box'ınızı sevk](https://docs.microsoft.com/azure/databox/data-box-deploy-picked-up).

    * Veri kutusu ağır cihazlar için bkz [, veri kutusu ağır sevk](https://docs.microsoft.com/azure/databox/data-box-heavy-deploy-picked-up).

5. Ne zaman Cihazınızı Microsoft alır, veri merkezi ağa bağlı ve verileri (devre dışı hiyerarşik ad alanları ile) belirtilen depolama hesabına yüklendikten sonra cihaz siparişi veren. BOM dosyalara karşı tüm verileri Azure'a karşıya yüklendiğini doğrulayın. Artık bu veri bir Data Lake depolama Gen2'ye depolama hesabına geçebilirsiniz.

## <a name="move-the-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama 2. nesil ile verileri taşıma

Azure depolama hesabınızı veri zaten sahip. Artık verileri Azure Data Lake depolama hesabınıza kopyalamak ve erişim izinleri dosyalar ve dizinler için geçerlidir.

> [!NOTE]
> Azure Data Lake depolama Gen2'ye veri deponuz olarak kullanıyorsanız bu adım gereklidir. Yalnızca bir blob depolama hesabı hiyerarşik ad alanı olmayan veri deponuz olarak kullanıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="copy-data-to-the-azure-data-lake-storage-gen-2-account"></a>Azure Data Lake depolama Gen 2 hesabına veri kopyalama

Azure Data Factory kullanarak veya, Azure tabanlı bir Hadoop kümesi kullanarak veri kopyalayabilirsiniz.

* Azure Data Factory kullanmak için bkz: [verileri ADLS Gen2'ye taşımak için Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2). Belirttiğinizden emin olun **Azure Blob Depolama** kaynağı olarak.

* Azure tabanlı bir Hadoop kümenizi kullanmak için bu DistCp komutu çalıştırın:

    ```bash
    hadoop distcp -Dfs.azure.account.key.<source_account>.dfs.windows.net=<source_account_key> abfs://<source_container> @<source_account>.dfs.windows.net/<source_path> abfs://<dest_container>@<dest_account>.dfs.windows.net/<dest_path>
    ```

    * Değiştirin `<source_account>` ve `<dest_account>` kaynak ve hedef depolama hesabı adları ile yer tutucu.

    * Değiştirin `<source_container>` ve `<dest_container>` yer tutucuları olan kaynak ve hedef kapsayıcıların adları.

    * Değiştirin `<source_path>` ve `<dest_path>` kaynak ve hedef dizin yolu ile yer tutucu.

    * Değiştirin `<source_account_key>` verilerini içeren depolama hesabının erişim anahtarıyla yer tutucu.

    Bu komut hem verileri hem de meta verileri depolama hesabınızdan Data Lake depolama Gen2'ye depolama hesabınıza kopyalar.

### <a name="create-a-service-principal-for-your-azure-data-lake-storage-gen2-account"></a>Azure Data Lake depolama Gen2 hesabınız için hizmet sorumlusu oluşturma

Hizmet sorumlusu oluşturmak için bkz: [nasıl yapılır: Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

* Adımları gerçekleştirirken [uygulamanızı bir role atama](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) bölümü makalenin atadığınızdan emin olun **depolama Blob verileri katkıda bulunan** rolüne hizmet sorumlusu.

* Adımları gerçekleştirirken [oturum açma için değerleri alma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) makale, uygulama kimliği ve istemci gizli değerleri bir metin dosyasına bölümü. Bu kısa süre içinde olması gerekir.

### <a name="generate-a-list-of-copied-files-with-their-permissions"></a>İzinleri ile kopyalanan dosyaların listesini oluşturun

Şirket içi Hadoop küme, şu komutu çalıştırın:

```bash

sudo -u hdfs ./copy-acls.sh -s /{hdfs_path} > ./filelist.json
```

Bu komut, izinleri ile kopyalanan dosyaların bir listesini oluşturur.

> [!NOTE]
> HDFS dosyalarında sayısına bağlı olarak, bu komutu çalıştırmak uzun sürebilir.

### <a name="generate-a-list-of-identities-and-map-them-to-azure-active-directory-add-identities"></a>Kimlikleri listesi oluşturmak ve bunları kimlikleri Azure Active Directory (Ekle) map

1. İndirme `copy-acls.py` betiği. Bkz: [yardımcı betikleri indirin ve çalıştırmasına için edge düğümünüzü](#download-helper-scripts) bu makalenin.

2. Benzersiz kimliklerinin bir listesini oluşturmak için şu komutu çalıştırın.

   ```bash
   
   ./copy-acls.py -s ./filelist.json -i ./id_map.json -g
   ```

   Bu betik adlı bir dosya oluşturur `id_map.json` Ekle tabanlı kimlikleri eşleştirmek için gereken kimliklerini içerir.

3. Açık `id_map.json` dosyasını bir metin düzenleyicisinde.

4. Dosyada göründüğü her JSON nesnesi için güncelleştirme `target` özniteliğiyle bir AAD kullanıcı asıl adı (UPN) veya nesne kimliği (OID), uygun kimlik eşlenmiş. Bitirdikten sonra dosyayı kaydedin. Sonraki adımda bu dosya gerekir.

### <a name="apply-permissions-to-copied-files-and-apply-identity-mappings"></a>Kopyalanan dosyalar için izinleri uygulamak ve kimlik eşlemelerini uygulayın

Data Lake depolama Gen2 hesaba kopyalanan veriler izinleri uygulamak için şu komutu çalıştırın:

```bash
./copy-acls.py -s ./filelist.json -i ./id_map.json  -A <storage-account-name> -C <container-name> --dest-spn-id <application-id>  --dest-spn-secret <client-secret>
```

* Değiştirin `<storage-account-name>` depolama hesabınızın adıyla yer tutucu.

* Değiştirin `<container-name>` kapsayıcınızı adıyla yer tutucu.

* Değiştirin `<application-id>` ve `<client-secret>` yer tutucu ile hizmet sorumlusu oluştururken topladığınız uygulama Kimliğini ve istemci gizli anahtarı.

## <a name="appendix-split-data-across-multiple-data-box-devices"></a>Ek: Data Box cihazlar arasında verileri bölme

Verilerinizi Data Box cihaza taşımadan önce ihtiyacınız bazı yardımcı betikleri indirin, verilerinizi Data Box cihaza uyacak ve gereksiz dosyaları hariç tutmak için düzenlenir emin olun.

<a id="download-helper-scripts" />

### <a name="download-helper-scripts-and-set-up-your-edge-node-to-run-them"></a>Yardımcı betikleri indirin ve bunları çalıştırmaya karar edge düğümünüzü ayarlayın

1. Edge veya şirket içi Hadoop kümenizin baş düğümü, şu komutu çalıştırın:

   ```bash
   
   git clone https://github.com/jamesbak/databox-adls-loader.git
   cd databox-adls-loader
   ```

   Bu komut, yardımcı betikleri içeren GitHub deposuna kopyalar.

2. Sahip olduğundan emin olun [jq](https://stedolan.github.io/jq/) yerel bilgisayarınızda yüklü paket.

   ```bash
   
   sudo apt-get install jq
   ```

3. Yükleme [istekleri](http://docs.python-requests.org/en/master/) python paket.

   ```bash
   
   pip install requests
   ```

4. Gerekli komut yürütme izinleri kümesi.

   ```bash
   
   chmod +x *.py *.sh

   ```

### <a name="ensure-that-your-data-is-organized-to-fit-onto-a-data-box-device"></a>Verilerinizi Data Box cihaza uyacak şekilde düzenlenmiştir emin olun.

Veri boyutu tek bir Data Box cihazı boyutu aşarsa, dosyaları birden çok Data Box cihazı depolayabilirsiniz gruplara bölebilirsiniz.

Verilerinizi Data Box cihaz bir singe boyutunu aşmıyorsa, sonraki bölüme geçebilirsiniz.

1. Yükseltilmiş izinlerle çalışan `generate-file-list` önceki bölümdeki yönergeleri izleyerek indirilen komut dosyası.

   Komut parametreleri açıklaması aşağıda verilmiştir:

   ```
   sudo -u hdfs ./generate-file-list.py [-h] [-s DATABOX_SIZE] [-b FILELIST_BASENAME]
                    [-f LOG_CONFIG] [-l LOG_FILE]
                    [-v {DEBUG,INFO,WARNING,ERROR}]
                    path

   where:
   positional arguments:
   path                  The base HDFS path to process.

   optional arguments:
   -h, --help            show this help message and exit
   -s DATABOX_SIZE, --databox-size DATABOX_SIZE
                        The size of each Data Box in bytes.
   -b FILELIST_BASENAME, --filelist-basename FILELIST_BASENAME
                        The base name for the output filelists. Lists will be
                        named basename1, basename2, ... .
   -f LOG_CONFIG, --log-config LOG_CONFIG
                        The name of a configuration file for logging.
   -l LOG_FILE, --log-file LOG_FILE
                        Name of file to have log output written to (default is
                        stdout/stderr)
   -v {DEBUG,INFO,WARNING,ERROR}, --log-level {DEBUG,INFO,WARNING,ERROR}
                        Level of log information to output. Default is 'INFO'.
   ```

2. Oluşturulan dosyanın kullanıcılar için erişilebilir olmasını sağlamak için HDFS listeler kopyalama [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) işi.

   ```
   hadoop fs -copyFromLocal {filelist_pattern} /[hdfs directory]
   ```

### <a name="exclude-unnecessary-files"></a>Gereksiz dosyaları dışarıda bırak

Bazı dizinler DisCp işleminden hariç tutmak gerekir. Örneğin, durum bilgisi içeren çalıştıran kümeye tutmak dizinleri hariç tutun.

DistCp işini başlatmak planladığınız şirket içi Hadoop kümesinde listesini çıkarmak istediğiniz dizinleri belirtir bir dosya oluşturun.

Bir örneği aşağıda verilmiştir:

```
.*ranger/audit.*
.*/hbase/data/WALs.*
```

## <a name="next-steps"></a>Sonraki adımlar

Data Lake depolama Gen2 HDInsight kümeleri ile nasıl çalıştığını öğrenin. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](../../hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2.md).
