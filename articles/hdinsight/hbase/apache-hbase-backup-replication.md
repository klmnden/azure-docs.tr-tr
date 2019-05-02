---
title: Apache HBase ve Apache Phoenix yedekleme ve çoğaltma - Azure HDInsight'ı ayarlama
description: HBase ve Phoenix için yedekleme ve çoğaltma ayarlayın.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: e60aef7b1848197f41f96a1b5f5414bb0c8f4a15
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696373"
---
# <a name="set-up-backup-and-replication-for-apache-hbase-and-apache-phoenix-on-hdinsight"></a>Apache HBase ve HDInsight üzerinde Apache Phoenix için yedekleme ve çoğaltma ayarlama

Apache HBase, kullanılan veri kaybına karşı koruyarak için çeşitli yaklaşımları destekler:

* Kopyalama `hbase` klasörü
* Ardından içeri dışarı aktarma
* Tabloyu kopyalama
* Anlık Görüntüler
* Çoğaltma

> [!NOTE]  
> Sistem kataloğu HBase tablolarını yedeklediğinizde meta verileri yedeklenir, Apache Phoenix HBase tablolarda, meta verileri depolar.

Aşağıdaki bölümlerde bu yaklaşımların her için kullanım senaryosu açıklanmaktadır.

## <a name="copy-the-hbase-folder"></a>Hbase klasörünü kopyalayın

Bu yaklaşımda, tablo ve sütun ailesi alt kümesine işaretleyebilmesine olmadan tüm HBase veri kopyalayın. Sonraki yaklaşım daha fazla denetim sağlar.

HDInsight içinde HBase kümesi, Azure depolama blobları veya Azure Data Lake Storage oluştururken seçili varsayılan depolama alanı kullanır. Her iki durumda da, HBase aşağıdaki yolda bulunan verileri ve meta veri dosyalarını depolar:

    /hbase

* Bir Azure depolama hesabındaki `hbase` klasör blob kapsayıcısının kök dizininde bulunur:

    ```
    wasbs://<containername>@<accountname>.blob.core.windows.net/hbase
    ```

* Azure Data Lake Storage içinde `hbase` klasörün bulunduğu bir kümesi sağlanırken belirtilen kök yolunun altında. Genellikle bu kök yolu olan bir `clusters` sonra HDInsight kümenizi adlı bir alt klasörle:

    ```
    /clusters/<clusterName>/hbase
    ```

Her iki durumda da `hbase` klasör HBase diske alınma tüm verileri içerir, ancak bellek içi veri içermeyebilir. HBase verileri doğru bir temsili olarak bu klasörde güvenebilirsiniz önce kümeyi kapatmanız gerekir.

Küme silindikten sonra verileri yerinde bırakın veya verileri yeni bir konuma kopyalayın:

* Geçerli bir depolama konumuna işaret eden yeni bir HDInsight örneği oluşturun. Yeni örnek ile mevcut olan tüm verileri oluşturulur.

* Kopyalama `hbase` klasör için farklı bir Azure depolama blob kapsayıcı ya da Data Lake depolama konumu ve yeni bir küme bu verilerle başlatın. Azure depolama için kullanmak [AzCopy](../../storage/common/storage-use-azcopy.md)ve Data Lake depolama kullanmak için [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md).

## <a name="export-then-import"></a>Ardından içeri dışarı aktarma

Kaynak HDInsight kümesinde bir kaynak tablosundan varsayılan bağlı depolama alanına veri aktarmak (ile HBase dahil) dışarı aktarma yardımcı programı kullanın. Dışarı aktarılan klasörü hedef depolama konumuna kopyalamanız ve içeri aktarma yardımcı programı hedef HDInsight küme üzerinde çalıştırılır.

İlk SSH kaynak HDInsight kümenizin baş düğümüne bir tabloyu dışarı aktarma ve ardından aşağıdaki komutu çalıştırın `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>"

Bir tabloyu, SSH hedef HDInsight kümenizin baş düğümüne aktarın ve ardından aşağıdaki komutu çalıştırın `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.Import "<tableName>" "/<path>/<to>/<export>"

Tam dışarı aktarma yolu varsayılan depolama veya bağlı depolama seçeneklerinden birini belirtin. Örneğin, Azure Depolama'da:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

Azure Data Lake depolama Gen2 ' sözdizimi aşağıdaki gibidir:

    abfs://<containername>@<accountname>.dfs.core.windows.net/<path>

Azure Data Lake depolama Gen1 içinde sözdizimi aşağıdaki gibidir:

    adl://<accountName>.azuredatalakestore.net:443/<path>

Bu yaklaşım, tablo düzeyi ayrıntı düzeyi sunar. Artımlı olarak işlemi gerçekleştirmenize olanak tanıyan dahil etmek bir satır için bir tarih aralığı belirtebilirsiniz. Her tarihten bu yana UNIX dönem milisaniye cinsindendir.

    hbase org.apache.hadoop.hbase.mapreduce.Export "<tableName>" "/<path>/<to>/<export>" <numberOfVersions> <startTimeInMS> <endTimeInMS>

Sürümleri dışarı aktarmak için her bir satır sayısını belirtmek gerektiğini unutmayın. Tüm sürümleri, tarih aralığı içerecek şekilde, `<numberOfVersions>` için olası en büyük satır sürümlerinizi 100000 gibi daha büyük bir değer.

## <a name="copy-tables"></a>Tabloyu kopyalama

CopyTable yardımcı programı veri kaynağı ile aynı şemaya sahip mevcut bir hedef tabloya bir kaynak tablosundan satır satır kopyalar. Hedef tablo aynı kümede veya farklı bir HBase kümesi olabilir.

CopyTable bir küme içinde SSH kaynak HDInsight kümenizin baş düğümüne kullanın ve ardından bunu çalıştırmak için `hbase` komutu:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> <srcTableName>

Farklı bir kümedeki bir tabloya kopyalamak için CopyTable kullanmak için ekleme `peer` geçiş hedef kümenin adresiyle:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=<destTableName> --peer.adr=<destinationAddress> <srcTableName>

Hedef adresini, aşağıdaki üç bölümden oluşur:

    <destinationAddress> = <ZooKeeperQuorum>:<Port>:<ZnodeParent>

* `<ZooKeeperQuorum>` Örneğin, Apache ZooKeeper düğümleri, virgülle ayrılmış bir listesi verilmiştir:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net

* `<Port>` Varsayılan olarak 2181, HDInsight üzerinde ve `<ZnodeParent>` olduğu `/hbase-unsecure`, bu nedenle tam `<destinationAddress>` olacaktır:

    zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net:2181:/hbase-unsecure

Bkz: [el ile Apache ZooKeeper çekirdek listesi toplama](#manually-collect-the-apache-zookeeper-quorum-list) HDInsight kümeniz için bu değerleri almak hakkında ayrıntılar için bu makaledeki.

CopyTable yardımcı programını Ayrıca satır kopyalamak ve kopyalamak için bir tablodaki sütun ailesi kümesini belirtmek için zaman aralığını belirtmek için parametreleri destekler. CopyTable tarafından desteklenen parametreler tam listesini görmek için hiçbir parametre olmadan CopyTable çalıştırın:

    hbase org.apache.hadoop.hbase.mapreduce.CopyTable

CopyTable üzerinden hedef tabloya kopyalanacak tüm kaynak tablo içeriğini tarar. CopyTable çalışırken bu HBase kümenizin performansını düşürebilir.

> [!NOTE]  
> Tablolar arasında veri kopyalama otomatik hale getirmek için bkz: `hdi_copy_table.sh` betiğini [Azure HBase Utils](https://github.com/Azure/hbase-utils/tree/master/replication) github deposu.

### <a name="manually-collect-the-apache-zookeeper-quorum-list"></a>Apache ZooKeeper çekirdek listesini el ile toplayın

HDInsight kümeleri hem daha önce açıklandığı gibi aynı sanal ağda olduğunda iç ana bilgisayar adı çözümlemesi otomatik olarak gerçekleşir. İki ayrı sanal ağ tarafından VPN ağ geçidi bağlı HDInsight kümelerinde CopyTable kullanmak için ana bilgisayar çekirdek Zookeeper düğümleri IP adreslerini sağlamanız gerekir.

Çekirdek ana bilgisayar adlarını almak için aşağıdaki curl komutu çalıştırın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/configurations?type=hbase-site&tag=TOPOLOGY_RESOLVED" | grep "hbase.zookeeper.quorum"

Curl komutu bir JSON belge HBase yapılandırma bilgilerini alır ve grep komutu yalnızca "hbase.zookeeper.quorum" girişi, örneğin döndürür:

    "hbase.zookeeper.quorum" : "zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk4-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net,zk3-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net"

Çekirdek ana makine adları değeri, iki nokta üst üste sağındaki tüm dizedir.

Bu konaklar için IP adreslerini almak için önceki listede bulunan her konak için aşağıdaki curl komutunu kullanın:

    curl -u admin:<password> -X GET -H "X-Requested-By: ambari" "https://<clusterName>.azurehdinsight.net/api/v1/clusters/<clusterName>/hosts/<zookeeperHostFullName>" | grep "ip"

Bu curl komutu `<zookeeperHostFullName>` tam DNS adı, örnek gibi ZooKeeper konak `zk0-hdizc2.54o2oqawzlwevlfxgay2500xtg.dx.internal.cloudapp.net`. Komut çıktısı IP adresi için belirtilen ana bilgisayar, örneğin içerir:

    100    "ip" : "10.0.0.9",

Tüm ZooKeeper düğümleri için IP adresleri, çekirdeği topladıktan sonra hedef adresini yeniden oluşturun:

    <destinationAddress>  = <Host_1_IP>,<Host_2_IP>,<Host_3_IP>:<Port>:<ZnodeParent>

Bizim örneğimizde:

    <destinationAddress> = 10.0.0.9,10.0.0.8,10.0.0.12:2181:/hbase-unsecure

## <a name="snapshots"></a>Anlık Görüntüler

Anlık görüntü noktası-ın-time, HBase veri deposunda verilerin yedeğini almak etkinleştirin. Anlık görüntüleri, en düşük ek yükleri vardır ve bir anlık görüntü işlemi etkili bir şekilde bu anlık depodaki tüm dosyaların adlarını yakalama bir meta veri işlemi olduğundan, saniyeler içinde tamamlayın. Anlık görüntü zaman gerçek veri kopyalanır. Anlık görüntüleri burada güncelleştirme, silme ve ekler tüm yeni veriler temsil edilir HDFS, depolanan veriler sabit yapısını kullanır. Geri yükleyebilirsiniz (*kopya*) bir anlık görüntüsünü aynı küme veya başka bir küme için bir anlık görüntü verme.

Küme ve başlangıç anlık görüntüsü, SSH'yi kullanarak HDInsight HBase, baş düğüme oluşturmak için `hbase` Kabuk:

    hbase shell

Hbase kabuğunu içinde anlık görüntü komutu ile bir tablo ve bu anlık görüntü adlarını kullanın:

    snapshot '<tableName>', '<snapshotName>'

İçinde ada göre bir anlık görüntü geri yüklemek için `hbase` Kabuk, ilk tablo devre dışı sonra anlık görüntüyü geri yükle ve tabloyu yeniden etkinleştirin:

    disable '<tableName>'
    restore_snapshot '<snapshotName>'
    enable '<tableName>'

Yeni bir tabloya bir anlık görüntü geri yüklemek için clone_snapshot kullanın:

    clone_snapshot '<snapshotName>', '<newTableName>'

Anlık görüntü için HDFS kullanılmak üzere başka bir küme tarafından dışa aktarmak için önce anlık görüntü daha önce açıklandığı gibi oluşturun ve ardından ExportSnapshot yardımcı programını kullanın. Baş düğümüne SSH oturumundan bu yardımcı programı değil içinde çalıştırmak `hbase` Kabuk:

     hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot <snapshotName> -copy-to <hdfsHBaseLocation>

`<hdfsHBaseLocation>` Depolama konumlardan herhangi birinde kaynak kümenize erişilebilir ve hedef kümeniz tarafından kullanılan hbase klasöre işaret etmelidir. Örneğin, kaynak kümenize bağlı ikincil bir Azure depolama hesabına sahip ve bu hesabı hedef kümenin varsayılan depolama alanı tarafından kullanılan kapsayıcısı için erişim sağlar, bu komutu kullanabilirsiniz:

    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot 'Snapshot1' -copy-to 'wasbs://secondcluster@myaccount.blob.core.windows.net/hbase'

Anlık görüntüyü dışarı aktardıktan sonra restore_snapshot kullanılarak anlık görüntü geri yükleme ve hedef küme baş düğümüne SSH uygulayın daha önce açıklandığı gibi komutu.

Anlık görüntüleri sağlayan bir tablonun tam bir yedekleme zamanında `snapshot` komutu. Anlık görüntüler özelliği artımlı anlık görüntüleri zaman windows tarafından ya da sütun aileleri anlık eklenecek alt kümelerini belirtmek için sağlamaz.

## <a name="replication"></a>Çoğaltma

HBase çoğaltmasını kaynak kümesinden bir işlem kaynak kümede en az bir Giderle zaman uyumsuz bir mekanizma kullanarak, bir hedef kümeye otomatik olarak iter. HDInsight küme arasında çoğaltma ayarlayabilirsiniz burada:

* Kaynak ve hedef kümeler, aynı sanal ağ içinde sunulur.
* Kaynak ve hedefleri kümeleri farklı sanal ağlar bir VPN ağ geçidi tarafından bağlı olduğundan, ancak her iki küme aynı coğrafi konumda mevcut.
* Hedefleri kümeleri ve kaynak kümesi farklı sanal ağlar bir VPN ağ geçidi tarafından bağlı olduğundan ve her küme farklı bir coğrafi konumda mevcut.

Çoğaltmayı ayarlama için genel adımlar şunlardır:

1. Kaynak kümede tablolar oluşturmak ve verileri doldurun.
2. Hedef kümenin kaynak tablonun şeması ile boş hedef tablo oluşturma.
3. Hedef kümenin kaynak kümesi için bir eş olarak kaydedin.
4. İstenen kaynak tablolarda çoğaltmayı etkinleştirin.
5. Mevcut verilerin kaynak tablolardan hedef tablolara kopyalayın.
6. Çoğaltma, kaynak tabloları yeni veri değişikliklerine hedef tablolarla otomatik olarak kopyalar.

HDInsight üzerinde çoğaltmayı etkinleştirmek için bir betik eylemi çalıştıran kaynak HDInsight kümenize uygulayın. Çoğaltma, kümenizdeki veya Azure Resource Manager şablonlarını kullanarak sanal ağları oluşturulmuş örnek kümeleri üzerinde çoğaltma denemek için etkinleştirme yönergeleri için bkz [yapılandırma Apache HBase çoğaltmayı](apache-hbase-replication.md). Bu makalede, Phoenix meta verilerin etkinleştirmek için yönergeleri de içerir.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
