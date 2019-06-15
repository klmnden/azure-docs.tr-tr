---
title: Yeni bir sürümü - Azure HDInsight HBase küme geçirme
description: HBase kümeleri yeni bir sürüme geçiş yapma.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: ashishth
ms.openlocfilehash: a152b815daeefa4c199af9b159eee8e5783971e2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65143314"
---
# <a name="migrate-an-apache-hbase-cluster-to-a-new-version"></a>Bir Apache HBase kümesi yeni sürüme geçirme

Bu makalede, Azure HDInsight üzerinde Apache HBase kümenizi daha yeni bir sürüme güncelleştirmek için gereken adımlar açıklanmaktadır.

> [!NOTE]  
> Yükseltilirken kapalı kalma süresi dakika bazında en az olmalıdır. Bu kapalı kalma süresi, tüm bellek içi verileri ve ardından yeni kümede hizmetlerini yeniden başlatın ve yapılandırma için zaman temizlemeye adımlarla neden olur. Sonuçlarınız, düğümler, veri miktarı ve diğer değişkenleri sayısına bağlı olarak değişir.

## <a name="review-apache-hbase-compatibility"></a>Apache HBase uyumluluğu gözden geçirin

Apache HBase yükseltmeden önce kaynak ve hedef kümeler HBase sürümlerinde uyumlu olduğundan emin olun. Daha fazla bilgi için [Apache Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen](../hdinsight-component-versioning.md).

> [!NOTE]  
> Sürüm uyumluluk matrisi gözden geçirmenizi öneririz [HBase kitap](https://hbase.apache.org/book.html#upgrading).

Bir örnek sürüm uyumluluk matrisi aşağıda verilmiştir. Uyumluluk Y gösterir ve olası bir uyumsuzluk N gösterir:

| Uyumluluk türü | Ana sürüm| Alt sürüm | Düzeltme Eki |
| --- | --- | --- | --- |
| İstemci-sunucu kablo uyumluluğu | N | E | E |
| Sunucudan sunucuya uyumluluğu | N | E | E |
| Dosya biçimi uyumluluğu | N | E | E |
| İstemci API'si uyumluluğu | N | E | E |
| İstemci ikili uyumluluğu | N | N | E |
| **Sunucu tarafı API uyumluluğuna sınırlı** |  |  |  |
| Dengeli | N | E | E |
| Gelişen | N | N | E |
| Kararsız | N | N | N |
| Bağımlılık uyumluluğu | N | E | E |
| İşletimsel uyumluluğu | N | N | E |

> [!NOTE]  
> En son uyumsuzlukları HBase sürümü bu sürüm notlarında açıklanmalıdır.

## <a name="upgrade-with-same-apache-hbase-major-version"></a>Aynı Apache HBase ile ana sürüm yükseltme

Azure HDInsight üzerinde Apache HBase kümenizi yükseltmek için aşağıdaki adımları tamamlayın:

1. HBase uyumluluk matrisi ve sürüm notları gösterildiği gibi uygulamanızın yeni bir sürümü ile uyumlu olduğundan emin olun. HDInsight HBase, hedef sürümünü çalıştıran bir kümede uygulamanızı test edin.

2. [Yeni bir hedef HDInsight kümesi ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) aynı depolama hesabını kullanarak ancak farklı bir kapsayıcı adı:

    ![Aynı depolama hesabını kullanırsınız, ancak farklı bir kapsayıcı oluşturma](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Küme yükseltme yapıyorsanız, kaynak HBase kümesi temizler. HBase Yazar gelen verileri bir bellek içi depoya adlı bir _memstore_. Belirli bir boyutun memstore ulaştıktan sonra HBase, kümenin depolama hesabındaki uzun vadeli depolama için diske aktarır. Eski kümede silerken memstores, olası veri kaybı dönüştürülünceye. Memstore her tablo için diske el ile temizlemek için aşağıdaki betiği çalıştırın. Bu betik en son sürümü Azure üzerinde değil [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh).

    ```bash
    #!/bin/bash
    
    #-------------------------------------------------------------------------------#
    # SCRIPT TO FLUSH ALL HBASE TABLES.
    #-------------------------------------------------------------------------------#
    
    LIST_OF_TABLES=/tmp/tables.txt
    HBASE_SCRIPT=/tmp/hbase_script.txt
    TARGET_HOST=$1
    
    usage ()
    {
        if [[ "$1" == "-h" ]] || [[ "$1" == "--help" ]]
        then
            cat << ...
    
    Usage: 
    
    $0 [hostname]
    
    Providing hostname is optional and not required when the script is executed within HDInsight cluster with access to 'hbase shell'.
    
    However hostname should be provided when executing the script as a script-action from HDInsight portal.
    
    For Example:
    
        1.  Executing script inside HDInsight cluster (where 'hbase shell' is 
            accessible):
    
            $0 
    
            [No need to provide hostname]
    
        2.  Executing script from HDinsight Azure portal:
    
            Provide Script URL.
    
            Provide hostname as a parameter (i.e. hn0, hn1 or wn2 etc.).
    ...
            exit
        fi
    }
    
    validate_machine ()
    {
        THIS_HOST=`hostname`
    
        if [[ ! -z "$TARGET_HOST" ]] && [[ $THIS_HOST  != $TARGET_HOST* ]]
        then
            echo "[INFO] This machine '$THIS_HOST' is not the right machine ($TARGET_HOST) to execute the script."
            exit 0
        fi
    }
    
    get_tables_list ()
    {
    hbase shell << ... > $LIST_OF_TABLES 2> /dev/null
        list
        exit
    ...
    }
    
    add_table_for_flush ()
    {
        TABLE_NAME=$1
        echo "[INFO] Adding table '$TABLE_NAME' to flush list..."
        cat << ... >> $HBASE_SCRIPT
            flush '$TABLE_NAME'
    ...
    }
    
    clean_up ()
    {
        rm -f $LIST_OF_TABLES
        rm -f $HBASE_SCRIPT
    }
    
    ########
    # MAIN #
    ########
    
    usage $1
    
    validate_machine
    
    clean_up
    
    get_tables_list
    
    START=false
    
    while read LINE 
    do 
        if [[ $LINE == TABLE ]] 
        then
            START=true
            continue
        elif [[ $LINE == *row*in*seconds ]]
        then
            break
        elif [[ $START == true ]]
        then
            add_table_for_flush $LINE
        fi
    
    done < $LIST_OF_TABLES
    
    cat $HBASE_SCRIPT
    
    hbase shell $HBASE_SCRIPT << ... 2> /dev/null
    exit
    ...
    
    ```
    
4. Eski bir HBase kümesi alımıyla durdurun.
5. Memstore son tüm veriler aktarılmadan emin olmak için önceki betiği yeniden çalıştırın.
6. Oturum [Apache Ambari](https://ambari.apache.org/) eski kümede (https://OLDCLUSTERNAME.azurehdidnsight.net) ve HBase hizmetleri durdurun. Hizmetlerini durdurmak istediğinizi onaylamak için sorulduğunda, HBase için bakım modunu açmak için kutuyu işaretleyin. Bağlanmak ve Ambari kullanarak daha fazla bilgi için bkz: [yönetme HDInsight kümeleri Ambari Web kullanıcı arabirimini kullanarak](../hdinsight-hadoop-manage-ambari.md).

    ![Ambari Hizmetleri > HBase > altında hizmet eylemleri Durdur](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Bakım modunu aç için HBase onay kutusunu işaretleyin, sonra onaylayın](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Ambari için yeni bir HDInsight kümesinde oturum açın. Değişiklik `fs.defaultFS` özgün küme tarafından kullanılan kapsayıcı adını işaret edecek şekilde HDFS ayarı. Bu ayar altındadır **HDFS > yapılandırmaları > Gelişmiş > Gelişmiş çekirdek site**.

    ![Ambari Hizmetleri > HDFS > yapılandırmaları > Gelişmiş](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Ambari kapsayıcı adı değiştirin.](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. **Gelişmiş yazma özelliği olan HBase kümeleri kullanmıyorsanız, bu adımı atlayın. Yalnızca gelişmiş yazma özelliğiyle HBase kümeleri için gereklidir.**
   
   Değişiklik `hbase.rootdir` özgün kümeyi kapsayıcıya işaret edecek şekilde yolu.

    ![Ambari HBase rootdir kapsayıcı adını değiştirme](./media/apache-hbase-migrate-new-version/change-container-name-for-hbase-rootdir.png)
1. 4\. 0'HDInsight 3.6 yükseltiyorsanız aşağıdaki adımları izleyin, aksi takdirde 10. adıma geçin:
    1. Tüm gerekli hizmetler Ambari seçerek yeniden **Hizmetleri** > **tüm gerekli yeniden**.
    1. HBase hizmetini durdurun.
    1. Zookeeper düğümüne SSH ve yürütme [zkCli](https://github.com/go-zkcli/zkcli) komut `rmr /hbase-unsecure` HBase kök znode Zookeeper kaldırılamadı.
    1. HBase yeniden başlatın.
1. Başka bir HDInsight sürüm 4.0 yanı sıra yükseltiyorsanız aşağıdaki adımları izleyin:
    1. Yaptığınız değişiklikleri kaydedin.
    1. Gerekli tüm hizmetleri Ambari tarafından belirtildiği gibi yeniden başlatın.
1. Uygulamanızı yeni kümesine gelin.

    > [!NOTE]  
    > Statik uygulamanız için DNS yükseltirken değiştirir. Bu DNS kodlamak yerine, bir CNAME kümenin adına işaret eden etki alanı adı DNS ayarları yapılandırabilirsiniz. Başka bir seçenek yeniden dağıtmaya gerek kalmadan güncelleştirebilirsiniz uygulamanız için bir yapılandırma dosyası kullanmaktır.

12. Her şeyin beklendiği gibi çalışıp çalışmadığını görmek için alımı başlatın.
13. Yeni küme tatmin edici ise, özgün kümeyi silin.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında daha fazla bilgi edinmek için [Apache HBase](https://hbase.apache.org/) ve HDInsight kümelerini yükseltme, aşağıdaki makalelere bakın:

* [Bir HDInsight kümesine yeni bir sürüme yükseltme](../hdinsight-upgrade-cluster.md)
* [İzleme ve Azure HDInsight, Apache Ambari Web kullanıcı arabirimini kullanarak yönetme](../hdinsight-hadoop-manage-ambari.md)
* [Apache Hadoop bileşenleri ve sürümleri](../hdinsight-component-versioning.md)
* [Apache Ambari kullanarak yapılandırmaları en iyi duruma getirme](../hdinsight-changing-configs-via-ambari.md#apache-hbase-optimization-with-the-ambari-web-ui)
