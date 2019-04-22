---
title: Azure PowerShell - Hızlı Başlangıç'ı kullanarak HDInsight üzerinde Apache kafka'yı ayarlama
description: Bu hızlı başlangıçta, Azure PowerShell kullanarak Azure HDInsight üzerinde bir Apache Kafka kümesi oluşturmayı öğrenirsiniz. Kafka konuları, aboneleri ve tüketicileri hakkında da bilgi edinirsiniz.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: mvc
ms.topic: quickstart
ms.date: 04/16/2018
ms.openlocfilehash: c86e5faa212fb6458326e00cba02fbe2ea83c8f7
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58850331"
---
# <a name="quickstart-create-an-apache-kafka-on-hdinsight-cluster"></a>Hızlı Başlangıç: HDInsight kümesi üzerinde Apache Kafka oluşturma

[Apache Kafka](https://kafka.apache.org/) bir açık kaynaklı, dağıtılmış akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır. 

Bu hızlı başlangıçta, Azure PowerShell kullanarak [Apache Kafka](https://kafka.apache.org) kümesi oluşturmayı öğrenirsiniz. Ayrıca Kafka kullanarak ileti göndermek ve almak için, birlikte sunulan yardımcı programları kullanmayı da öğrenirsiniz.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

> [!IMPORTANT]  
> Kafka API’sine yalnızca aynı sanal ağ içindeki kaynaklar tarafından erişilebilir. Bu hızlı başlangıçta, doğrudan SSH kullanarak kümeye erişirsiniz. Diğer hizmetleri, ağları veya sanal makineleri Kafka’ya bağlamak için önce bir sanal ağ oluşturmanız e sonra ağ içinde kaynakları oluşturmanız gerekir.
>
> Daha fazla bilgi için [Sanal ağ kullanarak Apache Kafka'ya bağlanma](apache-kafka-connect-vpn-gateway.md) belgesine bakın.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Azure PowerShell. Daha fazla bilgi için [Azure PowerShell’i Yükleme ve Yapılandırma](https://docs.microsoft.com/powershell/azure/install-az-ps) belgesine bakın.

* Bir SSH istemcisi. Bu belgede yer alan adımlarda, kümeye bağlanmak için SSH kullanılır.

    Linux, Unix ve macOS sistemlerinde varsayılan olarak `ssh` komutu sağlanır. Windows 10’da, `ssh` komutunu yüklemek için aşağıdaki yöntemlerden birini kullanın:

  * [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)’i kullanın. Cloud shell, `ssh` komutunu sağlar ve kabuk ortamı olarak Bash veya PowerShell kullanılacak şekilde yapılandırılabilir.

  * [Linux için Windows Alt Sistemini yükleyin](https://docs.microsoft.com/windows/wsl/install-win10). Microsoft Store aracılığıyla kullanılabilir olan Linux dağıtımları, `ssh` komutunu sağlar.

    > [!IMPORTANT]  
    > Bu belgede yer alan adımlarda, yukarıda belirtilen SSH istemcilerinden birisini kullandığınız varsayılır. Farklı bir SSH istemcisi kullanıyorsanız ve sorunlarla karşılaşıyorsanız lütfen SSH istemciniz için belgelere başvurun.
    >
    > Daha fazla bilgi için [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Login-AzAccount` cmdlet’i ile Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Login-AzAccount
```

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnekte, adınız ve konumunuz istenir, daha sonra yeni bir kaynak grubu oluşturulur:

```powershell
$resourceGroup = Read-Input -Prompt "Enter the resource group name"
$location = Read-Input -Prompt "Enter the Azure region to use"

New-AzResourceGroup -Name $resourceGroup -Location $location
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

HDInsight üzerinde Kafka, Kafka verilerini depolamak için Azure Yönetilen Diskler’i kullanırken, küme de günlükler gibi bilgileri depolamak için Azure Depolama’yı kullanır. Kullanım [yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) yeni bir depolama hesabı oluşturmak için.

```powershell
$storageName = Read-Host -Prompt "Enter the storage account name"

New-AzStorageAccount `
        -ResourceGroupName $resourceGroup `
        -Name $storageName `
        -Type Standard_LRS `
        -Location $location
```

HDInsight, depolama hesabındaki verileri bir blob kapsayıcısında depolar. Kullanım [yeni AzStorageContainer](/powershell/module/Az.Storage/New-AzStorageContainer) yeni bir kapsayıcı oluşturmak için.

```powershell
$containerName = Read-Host -Prompt "Enter the container name"

$storageKey = (Get-AzStorageAccountKey `
                -ResourceGroupName $resourceGroup `
                -Name $storageName)[0].Value
$storageContext = New-AzStorageContext `
                    -StorageAccountName $storageName `
                    -StorageAccountKey $storageKey
New-AzStorageContainer -Name $containerName -Context $storageContext 
```

## <a name="create-an-apache-kafka-cluster"></a>Apache Kafka kümesi oluşturma

Apache Kafka ile HDInsight kümesi oluşturmak [yeni AzHDInsightCluster](/powershell/module/az.HDInsight/New-azHDInsightCluster).

```powershell
# Create a Kafka 1.0 cluster
$clusterName = Read-Host -Prompt "Enter the name of the Kafka cluster"
$httpCredential = Get-Credential -Message "Enter the cluster login credentials" `
    -UserName "admin"
$sshCredentials = Get-Credential -Message "Enter the SSH user credentials"

$numberOfWorkerNodes = "4"
$clusterVersion = "3.6"
$clusterType="Kafka"
$clusterOS="Linux"
$disksPerNode=2

$kafkaConfig = New-Object "System.Collections.Generic.Dictionary``2[System.String,System.String]"
$kafkaConfig.Add("kafka", "1.0")

New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroup `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $numberOfWorkerNodes `
        -ClusterType $clusterType `
        -OSType $clusterOS `
        -Version $clusterVersion `
        -ComponentVersion $kafkaConfig `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageKey `
        -DefaultStorageContainer $clusterName `
        -SshCredential $sshCredentials `
        -DisksPerWorkerNode $disksPerNode
```

> [!WARNING]  
> HDInsight kümesinin oluşturulması 20 dakika kadar sürebilir.

> [!TIP]  
> `-DisksPerWorkerNode` parametresi, HDInsight üzerinde Kafka’nın ölçeklenebilirliğini yapılandırır. HDInsight üzerinde Kafka, verileri depolamak için kümedeki sanal makinelerin yerel diskini kullanır. Kafka, G/Ç açısından yoğun olduğundan, yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. 
>
> Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Disk türü, çalışan düğümleri (Kafka aracıları) tarafından kullanılan sanal makine boyutuna bağlıdır. Premium diskler otomatik olarak DS ve GS serisi sanal makinelerle kullanılır. Diğer tüm VM türleri standart disk kullanır. `-WorkerNodeSize` parametresini kullanarak sanal makine türünü ayarlayabilirsiniz. Parametreler hakkında daha fazla bilgi için bkz. [yeni AzHDInsightCluster](/powershell/module/az.HDInsight/New-azHDInsightCluster) belgeleri.


> [!IMPORTANT]  
> 32’den fazla çalışan düğümü kullanmayı planlıyorsanız (küme oluşturma sırasında veya oluşturma işleminden sonra kümeyi ölçeklendirerek), en az 8 çekirdek ve 14 GB RAM ile bir sanal makine boyutu belirtmek için `-HeadNodeSize` parametresini kullanmanız gerekir.
>
> Düğüm boyutları ve ilişkili maliyetler hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

1. Kafka kümesinin birincil baş düğümüne bağlanmak için aşağıdaki komutu kullanın. `sshuser` değerini, SSH kullanıcı adıyla değiştirin. `mykafka` değerini, Kafka kümenizin adıyla değiştirin

    ```bash
    ssh sshuser@mykafka-ssh.azurehdinsight.net
    ```

2. Kümeye ilk kez bağlandığınızda SSH istemciniz ana bilgisayarın orijinalliğinin belirlenemediği yönünde bir uyarı görüntüleyebilir. Ana bilgisayarı SSH istemcinizin güvenilen sunucular listesine eklemek isteyip istemediğiniz sorulduğunda __evet__’i seçin ve sonra __Enter__ tuşuna basın.

3. İstendiğinde, SSH kullanıcısının parolasını girin.

Bağlandığında, aşağıdaki metne benzer bilgiler görürsünüz:

```text
Authorized uses only. All activity may be monitored and reported.
Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-1011-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    https://www.ubuntu.com/business/services/cloud

83 packages can be updated.
37 updates are security updates.



Welcome to Kafka on HDInsight.

Last login: Thu Mar 29 13:25:27 2018 from 108.252.109.241
ssuhuser@hn0-mykafk:~$
```

## <a id="getkafkainfo"></a>Apache Zookeeper ve aracı konak bilgilerini alma

Kafka ile çalışırken bilmeniz gerekir *Apache Zookeeper* ve *Aracısı* konaklar. Bu konaklar Kafka API’si ve Kafka ile gönderilen yardımcı programların birçoğu ile birlikte kullanılır.

Bu bölümde, küme üzerinde Apache Ambari REST API konak bilgilerini alın.

1. Küme ile SSH bağlantısından aşağıdaki komutu kullanarak `jq` yardımcı programını yükleyin. Bu yardımcı program, JSON belgelerini ayrıştırmak için kullanılır ve ana bilgisayar bilgilerini almak için yararlıdır:
   
    ```bash
    sudo apt -y install jq
    ```

2. Bir ortam değişkenini küme adına ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    read -p "Enter the Kafka on HDInsight cluster name: " CLUSTERNAME
    ```

    İstendiğinde, Kafka kümesinin adını girin.

3. Bir ortam değişkenini Zookeeper konak bilgileriyle ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    İstendiğinde, küme oturum açma hesabı (SSH hesabı değil) için parolayı girin.

    > [!NOTE]  
    > Bu komut tüm Zookeeper ana bilgisayarlarını alır ve yalnızca ilk iki girişi döndürür. Bunun nedeni, bir ana bilgisayarın ulaşılamaz olması durumunda yedeklilik istemenizdir.

4. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
     echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

5. Bir ortam değişkenini Kafka aracı konak bilgileriyle ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

    İstendiğinde, küme oturum açma hesabı (SSH hesabı değil) için parolayı girin.

6. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash   
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Apache Kafka konularını yönetme

Kafka, veri akışlarını *konular* içinde depolar. Konuları yönetmek için `kafka-topics.sh` yardımcı programını kullanabilirsiniz.

* **Bir konu oluşturmak için**, SSH bağlantısında aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `$KAFKAZKHOSTS` içinde depolanan konak bilgilerini kullanarak Zookeeper’a bağlanır. Ardından **test** adlı bir Kafka konusu oluşturur. 

    * Bu konuda depolanan veriler, sekiz bölüm halinde bölümlenir.

    * Her bölüm, kümedeki üç çalışan düğümü arasında çoğaltılır.

        > [!IMPORTANT]  
        > Üç hata etki alanı sağlayan bir Azure bölgesinde kümeyi oluşturduysanız, 3 çoğaltma katsayısını kullanın. Aksi takdirde 4 çoğaltma katsayısını kullanın.
        
        Üç hata etki alanı içeren bölgelerde 3 çoğaltma katsayısı, çoğaltmaların hata etki alanları arasında yayılmasına olanak sağlar. İki hata etki alanı içeren bölgelerde dört çoğaltma katsayısı, çoğaltmaların etki alanları arasında eşit şekilde yayılmasına olanak sağlar.
        
        Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

        > [!IMPORTANT]   
        > Kafka, Azure hata etki alanları ile uyumlu değildir. Konular için bölüm çoğaltmaları oluşturulurken, çoğaltmalar yüksek kullanılabilirlik için düzgün şekilde dağıtılmayabilir.

        Yüksek kullanılabilirlik sağlamak için kullanın [Apache Kafka bölüm yeniden Dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools). Bu araç bir SSH bağlantısından Kafka kümenizin baş düğümüne doğru çalıştırılmalıdır.

        Kafka verilerinizin en yüksek kullanılabilirliğe sahip olması için, aşağıdaki durumlarda konunuz için bölüm çoğaltmalarını yeniden dengelemeniz gerekir:

        * Yeni bir konu veya bölüm oluşturduğunuzda

        * Bir kümenin ölçeğini artırdığınızda

* **Konuları listelemek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, Kafka kümesinde kullanılabilir olan konuları listeler.

* **Bir konuyu silmek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `topicname` adlı konuyu siler.

    > [!WARNING]  
    > Daha önce oluşturulan `test` konusunu silerseniz, yeniden oluşturmanız gerekir. Bu belgenin ilerleyen kısmındaki adımlarda kullanılacaktır.

`kafka-topics.sh` yardımcı programı ile kullanılabilen komutlar hakkında daha fazla bilgi için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Kayıt oluşturma ve kullanma

Kafka, konu başlıklarında *records* (kayıtlar) depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler ve tüketiciler, *Kafka aracısı* hizmetiyle iletişim kurar. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısı ana bilgisayarıdır.

Daha önce oluşturduğunuz test konu başlığında kayıtları depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. Konuya kayıtlar yazmak için SSH bağlantısından `kafka-console-producer.sh` yardımcı programını kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Bu komuttan sonra boş bir satıra ulaşırsınız.

2. Boş satıra bir metin iletisi yazın ve Enter tuşuna basın. Bu şekilde birkaç ileti girin ve sonra normal isteme geri dönmek için **Ctrl + C** tuşlarını kullanın. Her satır, Kafka konusuna ayrı bir kayıt olarak gönderilir.

3. Konudan kayıtları okumak için SSH bağlantısından `kafka-console-consumer.sh` yardımcı programını kullanın:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Bu komutla, kayıtlar konu başlığından alınır ve görüntülenir. `--from-beginning` kullanılması, tüketiciye akışın başından başlamasını söyler, böylece tüm kayıtlar alınır.

    > [!NOTE]  
    > Kafka’nın eski bir sürümünü kullanıyorsanız `--bootstrap-server $KAFKABROKERS` değerini `--zookeeper $KAFKAZKHOSTS` ile değiştirin.

4. Tüketiciyi durdurmak için __Ctrl + C__ tuşlarını kullanın.

Ayrıca programlı olarak üretici ve tüketici de oluşturabilirsiniz. Bu API kullanma örneği için bkz: [Apache Kafka üretici ve tüketici API'si HDInsight ile](apache-kafka-producer-consumer-api.md) belge.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) HDInsight, kaynak grubunu kaldırmak için komutu ve tüm ilgili kaynakları.

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

> [!WARNING]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
> 
> HDInsight üzerinde Kafka kümesinin silinmesi Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
