<properties
    pageTitle="HBase öğreticisi: Hadoop’ta Linux tabanlı HBase kümelerini kullanmaya başlayın | Microsoft Azure"
    description="HDInsight’ta Hadoop ile Apache HBase kullanmaya başlamak için bu HBase öğreticisini izleyin. HBase kabuğundan tablolar oluşturun ve Hive kullanarak bunları sorgulayın."
    keywords="apache hbase,hbase,hbase kabuğu,hbase öğreticisi"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/25/2016"
    ms.author="jgao"/>




# HBase öğreticisi: HDInsight’ta Linux tabanlı Hadoop ile Apache HBase kullanmaya başlayın 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

HDInsight’ta HBase kümesi oluşturma, HBase tabloları oluşturma ve tabloları Hive kullanarak sorgulama hakkında bilgi edinin. Genel HBase bilgileri için bkz. [HDInsight HBase’e genel bakış][hdinsight-hbase-overview].

Bu belgedeki bilgiler Linux tabanlı HDInsight kümelerine özeldir. Windows tabanlı kümeler hakkında daha fazla bilgi için sayfanın üst kısmındaki sekme seçicisini kullanarak geçiş yapın.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

###Ön koşullar

HBase öğreticisine başlamadan önce aşağıdakilere sahip olmanız gerekir:

- **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Secure Shell(SSU)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

## HBase kümesi oluşturma

Aşağıdaki yordamda, HBase kümesi oluşturmak için bir Azure Resource Manager şablonu kullanılıyor. Yordamda ve diğer küme oluşturma yöntemlerinde kullanılan parametreleri anlamak için bkz. [HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

1. Şablonu Azure Portal'da açmak için aşağıdaki görüntüye tıklayın. Şablon, ortak bir blob kapsayıcısında bulunur. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **Parametreler** dikey penceresinde aşağıdakileri girin:

    - **ClusterName**: Oluşturacağınız HBase kümesi için bir ad girin.
    - **Küme oturum açma adı ve parolası**: Varsayılan oturum açma adı **admin** şeklindedir.
    - **SSH kullanıcı adı ve parolası**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz.
     
    Diğer parametreler isteğe bağlıdır.  
    
    Her kümenin bir Azure Blob Storage hesabı bağımlılığı vardır. Bir küme silindikten sonra veriler depolama hesabında saklanır. Kümenin varsayılan depolama hesabı adı, "depo" ifadesi eklenmiş küme adıdır. Şablon değişkenleri bölümüne sabit kodlanır.
        
3. Parametreleri kaydetmek için **Tamam**’a tıklayın.
4. **Özel dağıtım** dikey penceresinde **Kaynak grubu** açılır kutusuna ve ardından **Yeni**’ye tıklayarak yeni bir kaynak grubu oluşturun.  Kaynak grubu; küme, bağımlı depolama hesabını ve diğer bağlı kaynağı gruplandıran bir kapsayıcıdır.
5. **Yasal koşullar**’a ve ardından **Oluştur**’a tıklayın.
6. **Oluştur**’a tıklayın. Bir küme oluşturmak yaklaşık 20 dakika sürer.


>[AZURE.NOTE] Bir HBase kümesi silindikten sonra aynı varsayılan blob kapsayıcısını kullanarak başka bir HBase kümesi oluşturabilirsiniz. Yeni küme özgün kümede oluşturduğunuz HBase tablolarını seçer. Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir.

## Tablo oluşturma ve veri ekleme

HBase kümelerine bağlanmak ve HBase Kabuğu kullanarak HBase tabloları oluşturmak, veri eklemek ve verileri sorgulamak için SSH kullanabilirsiniz. Linux, Unix, OS X ve Windows işletim sistemlerinde SSH kullanma hakkında bilgi için bkz. [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) ve [Windows’da HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Çoğu kişi için veriler tablo biçiminde görünür:

![hdinsight hbase tabular data][img-hbase-sample-data-tabular]

Bir BigTable uygulaması olan HBase’de aynı veriler şu şekilde görünür:

![hdinsight hbase bigtable data][img-hbase-sample-data-bigtable]

Bir sonraki yordamı tamamladıktan sonra bunlar daha anlamlı olacaktır.  


**HBase kabuğunu kullanmak için**

1. SSH’den aşağıdaki komutu çalıştırın:

        hbase shell

4. İki sütun ailesi ile bir HBase oluşturun:

        create 'Contacts', 'Personal', 'Office'
        list
5. Bazı verileri ekleyin:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. Tek bir satır alın

        get 'Contacts', '1000'

    Yalnızca bir satır olduğundan tarama komutunu kullanmanızla aynı sonuçları görürsünüz.

    HBase tablo şeması hakkında daha fazla bilgi için bkz. [HBase Şema Tasarımına Giriş][hbase-schema]. HBase komutları hakkında daha fazla bilgi için bkz. [Apache HBase başvuru kılavuzu][hbase-quick-start].

6. Kabuktan çıkış yapma

        exit



**Verileri kişi HBase tablosuna toplu olarak yüklemek için**

HBase’de verileri tablolara yüklemek için bazı yöntemler vardır.  Daha fazla bilgi için bkz. [Toplu yükleme](http://hbase.apache.org/book.html#arch.bulk.load).


*wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt* konumundaki ortak blob kapsayıcısına örnek bir veri dosyası yüklenmiştir.  Veri dosyasının içeriği şudur:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Bir metin dosyası oluşturabilir ve isterseniz dosyayı kendi depolama hesabınıza yükleyebilirsiniz. Yönergeler için bkz. [HDInsight’ta Hadoop işleri için verileri karşıya yükleme][hdinsight-upload-data].

> [AZURE.NOTE] Bu yordam son yordamda oluşturduğunuz Kişiler HBase tablosunu kullanır.

1. SSH’de, veri dosyalarını StoreFiles’a dönüştürmek ve Dimporttsv.bulk.output tarafından belirtilen göreli bir yola depolamak için aşağıdaki komutu çalıştırın:  HBase Kabuğu'ndan çıkış yapmak için çıkış komutunu kullanın.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Verileri /example/data/storeDataFileOutput konumundan HBase tablosuna yüklemek için aşağıdaki komutu çalıştırın:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. HBase kabuğunu açabilir ve tarama komutunu kullanarak tablo içeriğini listeleyebilirsiniz.



## Hive kullanarak HBase sorgulama

Hive kullanarak HBase tablolarındaki verileri sorgulayabilirsiniz. Bu bölüm HBase tablosuyla eşlenen bir Hive tablosu oluşturur ve HBase tablosunda verileri sorgulamak için kullanır.

1. **PuTTY** uygulamasını açın ve kümeye bağlanın.  Önceki yordamda bulunan yönergelere bakın.
2. Hive kabuğunu açın.

       hive
3. HBase tablosuyla eşlenen bir Hive Tablosu oluşturmak için aşağıdaki HiveQL betiğini çalıştırın. Bu deyimi çalıştırmadan önce HBase kabuğunu kullanarak bu öğreticinin daha önceki bölümlerinde başvurulan örnek tablosunu oluşturduğunuzdan emin olun.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Aşağıdaki HiveQL komutunu çalıştırın. Hive sorgusu, HBase tablosundaki verileri sorgular:

        SELECT count(*) FROM hbasecontacts;

## Curl kullanarak HBase REST API’lerini kullanma

> [AZURE.NOTE] Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir.
>
> Bu bölümdeki komutlar için **USERNAME** ifadesini küme kimliğini doğrulayacak kullanıcı ile, **PASSWORD** ifadesini ise kullanıcı hesabının parolası ile değiştirin. **CLUSTERNAME** değerini kümenizin adıyla değiştirin.
>
> REST API’sinin güvenliği [temel kimlik doğrulaması](http://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmanız gerekir.

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Aşağıdakine benzer bir yanıt almanız gerekir:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **-u** - İstek kimliğini doğrulamak için kullanılan kullanıcı adı ve parola.
    * **-G** - Bunun bir GET isteği olduğunu belirtir.

2. Mevcut HBase tablolarını listelemek için şu komutu kullanın:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. İki sütun ailesi ile yeni bir HBase tablosu oluşturmak için aşağıdaki komutu kullanın:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Şema JSon biçiminde sağlanır.

4. Bazı verileri eklemek için aşağıdaki komutu kullanın:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    -d anahtarında belirtilen değerleri base64 ile kodlamanız gerekir.  Örnekte:

    - MTAwMA==: 1000
    - UGVyc29uYWw6TmFtZQ==: Personal:Name
    - Sm9obiBEb2xl: John Dole

    [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) birden fazla (toplu) değer eklemenizi sağlar.

5. Bir satır almak için aşağıdaki komutu kullanın:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

HBase Rest hakkında daha fazla bilgi için bkz. [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html#_rest).

## Küme durumunu denetleme

HDInsight içinde HBase, kümelerin izlenmesi için bir Web Kullanıcı Arabirimi ile birlikte gönderilir. Web Kullanıcı Arabirimini kullanarak istatistikler veya bölgeler hakkında bilgi isteyebilirsiniz.

SSH web istekleri gibi yerel istekler için HDInsight kümesine tünel oluşturmak üzere de kullanılabilir. Daha sonra, istek HDInsight kümesi baş düğümünde oluşturulmuş gibi istenen kaynağa iletilir. Daha fazla bilgi için bkz. [Windows’dan HDInsight’ta Linux tabanlı Hadoop ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Bir SSH tünel oluşturma oturumu oluşturmak için**

1. **PuTTY**’yi açın.  
2. Oluşturma işleminde kullanıcı hesabınızı oluştururken bir SSH anahtarı sağladıysanız, küme kimlik doğrulaması yapılırken özel anahtarı seçmek amacıyla aşağıdaki adımı gerçekleştirmelisiniz.

    **Kategori**’de **Bağlantı**’yı genişletin, **SSH**’yi genişletin ve **Kimlik Doğrulaması**’nı seçin. Son olarak, **Gözat**’a tıklayın ve özel anahtarınızı içeren .ppk dosyasını seçin.

3. **Kategori**’de **Oturum**’a tıklayın.
4. PuTTY oturum ekranınızın Temel seçeneklerinden aşağıdaki değerleri girin:

    - **Ana Bilgisayar Adı**: Ana bilgisayar adı (veya IP adresi) alanındaki HDInsight sunucusu SSH adresidir. SSH adresi kümenizin adıdır, bu durumda **-ssh.azurehdinsight.net**. Örneğin, *mycluster-ssh.azurehdinsight.net*.
    - **Bağlantı noktası**: 22. Birincil baş düğümdeki SSH bağlantı noktası 22'dir.  
5. İletişim kutusunun solundaki **Kategori** bölümünde **Bağlantı**’yı genişletin, **SSH**’yi genişletin ve ardından **Tüneller**’e tıklayın.
6. SSH bağlantı noktası iletme biçimini denetleyen Seçenekler bölümünde aşağıdaki bilgileri sağlayın:

    - **Kaynak bağlantı noktası** - İletmek istediğiniz istemci üzerindeki bağlantı noktası. Örneğin, 9876.
    - **Dinamik** - Dinamik SOCKS proxy yönlendirmesini etkinleştirir.
7. Ayarları eklemek için **Ekle**’ye tıklayın.
8. Bir SSH bağlantısı açmak için iletişim kutusunun altındaki **Aç** seçeneğine tıklayın.
9. İstendiğinde, bir SSH hesabı kullanarak sunucuda oturum açın. Bunun yapılması bir SSH oturumu oluşturur ve tüneli etkinleştirir.

**Ambari kullanarak zookeepers FQDN’sini bulmak için**

1. https://<ClusterName>.azurehdinsight.net/ adresine göz atın.
2. Küme kullanıcı hesabı kimlik bilgilerinizi iki kez girin.
3. Sol menüden **zookeeper** seçeneğine tıklayın.
4. Özet listesindeki üç **ZooKeeper Sunucusu** bağlantısından birine tıklayın.
5. **Ana Bilgisayar Adı**’nı kopyalayın. Örneğin, zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Bir istemci programını (Firefox) yapılandırmak ve küme durumunu denetlemek için**

1. Firefox’u açın.
2. **Menü Aç** düğmesine tıklayın.
3. **Seçenekler**’e tıklayın.
4. **Gelişmiş**, **Ağ** ve ardından **Ayarlar**’a tıklayın.
5. **El ile proxy yapılandırması** öğesini seçin.
6. Aşağıdaki değerleri girin:

    - **Socks Ana Bilgisayarı**: localhost
    - **Bağlantı noktası**: Putty SSH tünel oluşturma işleminde yapılandırdığınız aynı bağlantı noktasını kullanın.  Örneğin, 9876.
    - **SOCKS v5**: (seçili)
    - **Uzak DNS**: (seçili)
7. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.
8. http://&lt;The FQDN of a ZooKeeper>:60010/master-status adresine göz atın.

Yüksek kullanılabilirlik kümesinde Web Kullanıcı Arabirimini barındıran ve o anda etkin olan HBase ana düğümünün bağlantısını bulabilirsiniz.

##Küme silme

Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Sonraki adımlar

HDInsight’a yönelik bir HBase öğreticisinde bir HBase kümesi oluşturmayı ve tablo oluşturup bu tablolardaki verileri HBase kabuğundan görüntülemeyi öğrendiniz. Ayrıca HBase tablolarındaki veriler üzerinde bir Hive sorgusu kullanmayı, HBase C# REST API’lerini kullanarak bir HBase tablosu oluşturmayı ve tablodan veri almayı öğrendiniz.

Daha fazla bilgi için bkz:

- [HDInsight HBase’e genel bakış][hdinsight-hbase-overview]: HBase büyük miktarda yapılandırılmamış ve yarı yapılandırılmış veri için rastgele erişim ve güçlü tutarlılık sağlayan, Hadoop’ta yerleşik bir Apache, açık kaynak, NoSQL veritabanıdır.


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png



<!--HONumber=Sep16_HO3-->


