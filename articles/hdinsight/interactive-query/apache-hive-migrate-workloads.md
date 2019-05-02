---
title: Azure HDInsight 3.6 Hive iş yükleri için HDInsight 4.0 geçirme
description: Apache Hive iş yükleri HDInsight 3.6 üzerinde HDInsight 4.0 geçirmeyi öğrenin.
ms.service: hdinsight
author: msft-tacox
ms.author: tacox
ms.reviewer: jasonh
ms.topic: howto
ms.date: 04/24/2019
ms.openlocfilehash: b181edc08c51a5afa8682858b330acc84da7d73d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707005"
---
# <a name="migrate-azure-hdinsight-36-hive-workloads-to-hdinsight-40"></a>Azure HDInsight 3.6 Hive iş yükleri için HDInsight 4.0 geçirme

Bu belgede, HDInsight 3.6 üzerinde Apache Hive ve LLAP iş yükleri için HDInsight 4.0 geçirmek nasıl gösterir. HDInsight 4.0 gerçekleştirilmiş görünümler ve sorgu sonucu önbelleğe alma gibi yeni Hive ve LLAP özellikleri sağlar. HDInsight 4. 0'da iş yüklerinizi geçirin, HDInsight 3.6 üzerinde bulunmayan pek çok yeni özelliği Hive 3 kullanabilirsiniz.

Bu makalede, aşağıdaki konuları içerir:

* HDInsight 4.0 Hive meta veri geçişi
* ACID ACID olmayan tablolar ve güvenli geçiş
* HDInsight sürümleri arasında Hive güvenlik ilkelerinin korunması
* Sorgu yürütme ve HDInsight 4. 0 ' HDInsight 3.6 hata ayıklama

## <a name="migrate-apache-hive-metadata-to-hdinsight-40"></a>HDInsight 4.0 için Apache Hive meta veri geçirme

Hive bir avantajı (Hive meta veri deposu olarak adlandırılır) bir dış veritabanı için meta verileri dışarı aktarmak için yeteneğidir. **Hive meta veri deposu** tablo istatistikleri, tablo depolama konumu, sütun adlarının ve tablo dizin bilgileri dahil olmak üzere depolamak için sorumludur. Meta veri deposu veritabanı şemasını Hive sürümleri arasında farklılık gösterir. HDInsight 4.0 ile uyumlu olması, HDInsight 3.6 Hive meta veri deposu yükseltmek için aşağıdakileri yapın.

1. Uygulamanızın dış meta depo yeni bir kopyasını oluşturun. HDInsight 3.6 ve HDInsight 4.0 farklı meta veri deposu şemaları gerektirir ve tek bir meta veri deposu paylaşamazsınız.
1. Meta veri deposu yeni kopyasını bir) var olan HDInsight 4.0 kümesine veya ilk kez oluşturuyorsanız b) bir küme ekleyin. Bkz: [Azure HDInsight, harici meta veri depolarını kullanma](../hdinsight-use-external-metadata-stores.md) dış meta depo bir HDInsight kümesine ekleme hakkında daha fazla bilgi için. Meta veri deposu bağlandıktan sonra otomatik olarak 4.0 uyumlu meta veri deposu dönüştürülür.

> [!Warning]
> HDInsight 4.0 şeması, HDInsight 3.6 meta veri şema dönüştüren yükseltme ters olamaz.

## <a name="migrate-hive-tables-to-hdinsight-40"></a>Hive tablolarını HDInsight 4.0 geçirme

Önceki kümesini Hive meta veri deposu için HDInsight 4.0 geçirmek için adımları tamamladıktan sonra meta veri deposu kaydedilen veritabanlarını ve tabloları yürüterek HDInsight 4.0 küme içinde görünür olacak `show tables` veya `show databases` gelen kümedeki . Bkz: [HDInsight sürümleri arasında sorgu yürütmesi](#query-execution-across-hdinsight-versions) 4.0 HDInsight kümelerinde sorgu yürütme hakkında bilgi için.

Gerçek tabloları, verileri, ancak küme gerekli depolama hesapları için erişime sahip oluncaya kadar erişilemiyor. HDInsight 4.0 kümenizi eski, HDInsight 3.6 kümesi aynı verilere erişebildiğinden emin olmak için aşağıdaki adımları tamamlayın:

1. Tablonuzun Azure depolama hesabını belirleyin veya veritabanı kullanmayı açıklayan biçimlendirilmiş.
2. HDInsight 4.0 kümeniz zaten çalışıyorsa, kümeye Ambari aracılığıyla Azure depolama hesabı ekleyin. HDInsight 4.0 Küme henüz oluşturmadıysanız, Azure depolama hesabı, birincil veya ikincil küme depolama hesabı belirtildiğinden emin olun. HDInsight kümeleri için depolama hesapları ekleme hakkında daha fazla bilgi için bkz. [HDInsight için ek depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md).

> [!Note]
> Tablolar, HDInsight 3.6 ve HDInsight 4.0 farklı kabul edilir. Bu nedenle, farklı sürümlerin kümeler için aynı tabloları paylaşamazsınız. HDInsight 3.6 HDInsight 4.0 ile aynı anda kullanmak istiyorsanız, her bir sürümü için ayrı kopyası olması gerekir.

Hive iş yükünüz, ACID ve ACID olmayan tablolar bir karışımını içerebilir. (2 Hive) HDInsight 3.6 üzerinde Hive ve HDInsight 4. 0'ı (3 Hive) Hive arasında önemli bir fark ACID uyumluluk için Tablolar ' dir. HDInsight 3.6, Hive ACID uyumluluğu etkinleştirme, ek yapılandırma gerektirir, ancak HDInsight 4. 0'tabloları ACID varsayılan olarak uyumludur. Geçiş önemli bir sıkıştırma ACID tabloda 3.6 kümesi üzerinde çalıştırmak için önce gerekli yalnızca eylem. Hive görünümünden veya Beeline, aşağıdaki sorguyu çalıştırın:

```bash
alter table myacidtable compact 'major';
```

Bu sıkıştırma, HDInsight 3.6 ve HDInsight 4.0 ACID tabloları ACID deltaları farklı anlamak için gereklidir. Sıkıştırma tutarlılık garantileri temiz bir maskeleme görüntüsü zorlar. Bölüm 4'ü [Hive geçiş belgeleri](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.3.0/bk_ambari-upgrade-major/content/prepare_hive_for_upgrade.html) toplu düzenleme HDInsight 3.6 ACID tabloları için kılavuz içerir.

Meta veri deposu taşıma ve sıkıştırma adımları tamamladıktan sonra gerçek ambarı geçirebilirsiniz. HDInsight 4.0 ambar, Hive ambarı geçişi tamamladıktan sonra aşağıdaki özelliklere sahip:

* HDInsight 3.6 dış tablolarda HDInsight 4.0 dış tablolar olacaktır
* HDInsight 3.6 olmayan işlem yönetilen tablolarında HDInsight 4.0 dış tablolar olacaktır
* HDInsight 3.6 işlem yönetilen tablolarında HDInsight 4.0 yönetilen tablolarında olacaktır

Geçiş yürütmeden önce Ambarınızı özelliklerini ayarlamanız gerekebilir. Örneğin, bazı tablosu (örneğin, bir HDInsight 3.6 kümesi) bir üçüncü taraf tarafından erişilecek bekliyorsanız, geçiş tamamlandıktan sonra bu tabloya dış olarak olmalıdır. HDInsight 4. 0'da, tüm yönetilen tabloları işlem. Bu nedenle, yalnızca yönetilen HDInsight 4.0 tablolarında 4.0 HDInsight kümeleri tarafından erişilmelidir.

Hive ambarı geçiş aracı, tablo özelliklerinizi doğru ayarladıktan sonra SSH Kabuğu'nu kullanarak küme baş düğümüne birini yürütün:

1. SSH kullanarak, küme baş düğümüne bağlanın. Yönergeler için [SSH kullanarak HDInsight Bağlan](../hdinsight-hadoop-linux-use-ssh-unix.md)
1. Çalıştırarak Hive kullanıcı olarak oturum açma Kabuk açın `sudo su - hive`
1. Hortonworks Data Platform yığın yürüterek sürümünü `ls /usr/hdp`. Bu, sonraki komutta kullanması gereken bir sürüm dizesi görüntülenir.
1. Kabuktan aşağıdaki komutu yürütün. Değiştirin `${{STACK_VERSION}}` önceki adımdan gelen sürüm dizesi ile:

```bash
/usr/hdp/${{STACK_VERSION}}/hive/bin/hive --config /etc/hive/conf --service  strictmanagedmigration --hiveconf hive.strict.managed.tables=true  -m automatic  automatic  --modifyManagedTables --oldWarehouseRoot /apps/hive/warehouse
```

Geçiş Aracı tamamlandıktan sonra Hive Ambarınızı HDInsight 4.0 için hazır hale gelirsiniz. 

> [!Important]
> Diğer hizmetler veya uygulamalar, HDInsight 3.6 kümeleri gibi yönetilen HDInsight (dahil olmak üzere tabloları 3.6 geçirilmiş) 4.0 tablolarında erişilmemelidir.

## <a name="secure-hive-across-hdinsight-versions"></a>HDInsight sürümleri arasında güvenli Hive

HDInsight 3.6 beri HDInsight, Azure HDInsight Kurumsal güvenlik paketi (ESP) kullanarak Active Directory ile tümleşir. ESP küme içindeki belirli kaynakların izinleri yönetmek için Kerberos ve Apache Ranger'ı kullanır. HDInsight 3.6 kovanında karşı dağıtılan ranger ilkelerini HDInsight 4. 0, aşağıdaki adımlarla geçirilebilir:

1. Ranger Service Manager panelde, HDInsight 3.6 kümesi gidin.
2. Adlı ilkesine gidin **HIVE** ve ilkeyi bir json dosyasına aktarın.
3. Dışarı aktarılan İlkesi json başvurulan tüm kullanıcılar yeni kümede olmadığından emin olun. Bir kullanıcı ilkesi json olarak adlandırılır, ancak yeni kümede yok, kullanıcının yeni kümeye eklemek veya ilkeden başvurusunu kaldırın.
4. Gidin **Ranger Service Manager** 4.0 HDInsight kümenizdeki paneli.
5. Adlı ilkesine gidin **HIVE** ve 2. adımdaki ranger İlkesi json alın.

## <a name="query-execution-across-hdinsight-versions"></a>HDInsight sürümleri arasında sorgu yürütme

Yürütme ve Hive/LLAP sorguları bir HDInsight 3.6 kümesi içinde hata ayıklamak için iki yolu vardır. HiveCLI bir komut satırı deneyimi ve GUI tabanlı bir iş akışı Tez görünüm/Hive görünümünü sağlar.

HDInsight 4. 0'da, HiveCLI Beeline ile değiştirilmiştir. HiveCLI Hiveserver 1 için bir thrift istemcisi ve Beeline Hiveserver 2'ye erişim sağlayan bir JDBC istemcidir. Beeline herhangi diğer JDBC uyumlu veritabanı uç noktaya bağlanmak için de kullanılabilir. Beeline kullanılabilir kullanıma hazır HDInsight 4.0 gerekli herhangi bir kurulum olmadan ' dir.

HDInsight 3.6 Hive sunucusu ile etkileşim için GUI Ambari Hive görünümünü istemcisidir. 4.0 HDInsight Hive görünümü Hortonworks Data Analytics Studio (DAS) ile değiştirir. DAS HDInsight kümeleri kullanıma hazır sevk değil ve resmi olarak desteklenen bir paket değil. Ancak, DAS küme üzerinde aşağıdaki gibi yüklenebilir:

Kümenizde "Baş düğüm" ile betik eylemi yürütme için düğüm türü olarak başlatın. Aşağıdaki URI "Bash betiği URI'si" ile işaretlenmiş aşağıdaki metin kutusuna yapıştırın: https://hdiconfigactions.blob.core.windows.net/dasinstaller/LaunchDASInstaller.sh

Veri analizi Studio URL'si ile başlatılabilir: https://<clustername>.azurehdinsight.net/das/



Sorguları Görüntüleyicisi'nde çalıştırdıysanız sorguları görmüyorsanız, DAS yüklendikten sonra aşağıdaki adımları uygulayın:

1. Bölümünde anlatıldığı gibi Hive Tez ve DAS yapılandırmalarını ayarlamak [DAS yükleme sorunlarını gidermek için bu Kılavuzu](https://docs.hortonworks.com/HDPDocuments/DAS/DAS-1.2.0/troubleshooting/content/das_queries_not_appearing.html).
2. Aşağıdaki Azure depolama dizini yapılandırmaları sayfa blobları, ve bunlar altında listelenmiş olduklarına emin `fs.azure.page.blob.dirs`:
    * `hive.hook.proto.base-directory`
    * `tez.history.logging.proto-base-dir`
3. HDFS, Hive, Tez ve DAS iki baş düğümler üzerinde yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight 4.0 Duyurusu](../hdinsight-version-release.md)
* [HDInsight 4.0 derinlemesine bakış](https://azure.microsoft.com/blog/deep-dive-into-azure-hdinsight-4-0/)
* [3 ACID tablolar hive](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.1.0/using-hiveql/content/hive_3_internals.html)
