---
title: Bir HBase kümesi - Azure Hdınsight gibi yeni bir sürüm olarak geçirme | Microsoft Docs
description: HBase geçirmek nasıl yeni bir sürüme kümeleri.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 3ca982e7fc0ce56bee2ee2e193c82a78fac44362
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="migrate-an-hbase-cluster-to-a-new-version"></a>Yeni bir sürüme bir HBase kümesi geçirme

İş tabanlı kümeler, Spark ve Hadoop, gibi yükseltmeye - basit bkz [daha yeni bir sürüme yükseltme Hdınsight kümesi](../hdinsight-upgrade-cluster.md):

1. Geçici (yerel olarak saklanan) verileri yedekleyin.
2. Var olan küme silin.
3. Yeni bir küme aynı sanal alt ağdaki oluşturun.
4. Geçici veri içeri aktarın.
5. İşlerini başlatma ve yeni kümede işleme devam edin.

Bir HBase kümesi yükseltmek için bazı ek adımlar, bu makalede açıklanan gerekir.

> [!NOTE]
> Yükselttiğiniz sırada kapalı kalma süresi dakika terabayt en az olmalıdır. Bu kesinti adımlarla tüm bellek içi veri ve ardından yapılandırın ve yeni kümede hizmetlerini yeniden başlatmak için gereken süre temizlemek için neden olur. Sonuçlarınızı düğümleri, veri miktarı ve diğer değişkenler sayısına bağlı olarak değişir.

## <a name="review-hbase-compatibility"></a>HBase uyumluluk gözden geçirin

HBase yükseltmeden önce kaynak ve hedef kümeler HBase sürümlerinde uyumlu olduğundan emin olun. Daha fazla bilgi için bkz: [Hadoop bileşenleri ve Hdınsight ile kullanılabilir sürümlerini](../hdinsight-component-versioning.md).

> [!NOTE]
> Sürüm uyumluluk matrisi gözden geçirmeniz önerilir [HBase defteri](https://hbase.apache.org/book.html#upgrading).

Burada uyumluluk Y gösterir ve olası bir uyumsuzluk N gösteren bir örnek sürüm uyumluluk matrisi, şöyledir:

| Uyumluluk türü | Ana sürüm| Alt sürümü | Yama |
| --- | --- | --- | --- |
| İstemci-sunucu kablo uyumluluk | N | E | E |
| Sunucudan sunucuya uyumluluk | N | E | E |
| Dosya biçimi uyumluluğu | N | E | E |
| İstemci API Uyumluluk | N | E | E |
| İstemci ikili uyumluluğu | N | N | E |
| **Sunucu tarafı API Uyumluluk sınırlı** |  |  |  |
| Dengeli | N | E | E |
| Gelişen | N | N | E |
| Kararsız | N | N | N |
| Bağımlılık uyumluluk | N | E | E |
| İşletimsel uyumluluk | N | N | E |

> [!NOTE]
> Kesme uyumsuzlukları HBase sürüm Sürüm Notları'nda açıklanan.

## <a name="upgrade-with-same-hbase-major-version"></a>İle aynı HBase ana sürüm yükseltme

Hdınsight 3.4 aynı HBase ana sürümle (her ikisi de 1.1.2 Apache HBase ile gelir) 3.6 yükseltmek için aşağıdaki senaryodur. Var olduğu sürece uyumluluk sorunu yok kaynak ve hedef sürümler arasında başka sürüm yükseltmesi benzerdir.

1. HBase uyumluluk matrisi ve sürüm notları gösterildiği gibi uygulamanızın yeni sürümüyle uyumlu olduğundan emin olun. Hdınsight ve HBase hedef sürümünü çalıştıran bir kümede uygulamanızı test edin.

2. [Yeni hedef Hdınsight küme ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) aynı depolama hesabı kullanarak ancak farklı kapsayıcı adı:

    ![Aynı depolama hesabı kullanır, ancak farklı bir kapsayıcı oluşturma](./media/apache-hbase-migrate-new-version/same-storage-different-container.png)

3. Kaynak HBase kümesi temizlenir. Yükseltmekte olduğunuz küme budur. HBase Yazar gelen verileri bir bellek içi depoya adlı bir _memstore_. Belirli bir boyuta memstore ulaştıktan sonra memstore Temizlenen kümenin depolama hesabındaki uzun vadeli depolama için diske. Eski kümeyi silerken memstores, olası veri kaybı dönüştürülür. Memstore her tablo için diske el ile temizlemek için aşağıdaki betiği çalıştırın. Bu komut dosyasının en son sürümünü Azure'un üzerinde olduğu [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/flush_all_tables.sh).

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
    
4. Eski HBase kümesi için alım durdurun.
5. Memstore son tüm verilerde aktarılmadan emin olmak için önceki komut dosyası'nı yeniden çalıştırın.
6. Oturum açmak için Ambari eski kümede (https://OLDCLUSTERNAME.azurehdidnsight.net) ve HBase hizmetlerini durdurun. Hizmetlerini durdurmak istediğiniz onaylamanız istendiğinde, HBase için bakım modunu açmak için onay kutusunu işaretleyin. Bağlanma ve Ambari kullanarak daha fazla bilgi için bkz: [Hdınsight kümelerini yönetme Ambari Web kullanıcı arabirimini kullanarak](../hdinsight-hadoop-manage-ambari.md).

    ![Ambari, hizmetler, ardından sol menüsünde HBase sekmesini ardından altında hizmet işlemlerini durdur](./media/apache-hbase-migrate-new-version/stop-hbase-services.png)

    ![Kapatma üzerinde Bakım modu HBase onay kutusunu işaretleyin, sonra onaylayın](./media/apache-hbase-migrate-new-version/turn-on-maintenance-mode.png)

7. Yeni Hdınsight kümesinde Ambari için oturum açın. Değişiklik `fs.defaultFS` özgün küme tarafından kullanılan kapsayıcı adı işaret edecek şekilde HDFS ayarı. Bu ayar altındadır **HDFS > yapılandırmalar > Gelişmiş > çekirdek site Gelişmiş**.

    ![Ambari hizmetler, ardından sol menüsünde sonra yapılandırmalar sekmesi altında sonra Gelişmiş sekmesinde HDFS sekmesini](./media/apache-hbase-migrate-new-version/hdfs-advanced-settings.png)

    ![Ambari kapsayıcı adını değiştirin](./media/apache-hbase-migrate-new-version/change-container-name.png)

8. Yaptığınız değişiklikleri kaydedin.
9. Ambari tarafından belirtildiği şekilde gerekli tüm hizmetleri yeniden başlatın.
10. Yeni küme için uygulamanız gelin.

    > [!NOTE]
    > Statik yükseltirken, uygulamanız için DNS değiştirir. Bu DNS kodlamak yerine, kümenin adına işaret etki alanı adı DNS ayarları'nda bir CNAME yapılandırabilirsiniz. Yeniden dağıtmadan güncelleştirebilirsiniz, uygulamanız için bir yapılandırma dosyası kullanın başka bir seçenektir.

11. Her şeyin beklendiği gibi çalışıp çalışmadığını görmek için alım başlatın.
12. Yeni küme tatmin edici ise, özgün kümeyi silin.

## <a name="next-steps"></a>Sonraki adımlar

HBase ve Hdınsight kümeleri yükseltme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight kümesi daha yeni bir sürüme yükseltin](../hdinsight-upgrade-cluster.md)
* [İzleme ve Azure Hdınsight Ambari Web kullanıcı arabirimini kullanarak yönetme](../hdinsight-hadoop-manage-ambari.md)
* [Hadoop bileşenleri ve sürümleri](../hdinsight-component-versioning.md)
* [Ambari kullanarak yapılandırmaları en iyi duruma getirme](../hdinsight-changing-configs-via-ambari.md#hbase-optimization-with-the-ambari-web-ui)
