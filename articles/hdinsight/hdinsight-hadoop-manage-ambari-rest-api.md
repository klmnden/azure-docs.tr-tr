---
title: "İzleme ve Hadoop Ambari REST API - Azure Hdınsight ile yönetme | Microsoft Docs"
description: "Azure hdınsight'ta Hadoop kümelerini yönetmek ve izlemek için Ambari kullanmayı öğrenin. Bu belgede, Hdınsight kümeleriyle dahil Ambari REST API kullanmayı öğreneceksiniz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2017
ms.author: larryfr
ms.openlocfilehash: 33ff4b69a4a914e6c183b10bc47a077eea537e61
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Ambari REST API kullanarak Hdınsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Azure hdınsight'ta Hadoop kümelerini izlemek için Ambari REST API kullanmayı öğrenin.

Apache Ambari, yönetim ve kolay bir web kullanıcı Arabirimi ve REST API kullanmayı sağlayarak Hadoop kümesi izleme basitleştirir. Ambari, Linux işletim sistemi kullanan Hdınsight kümelerine dahil edilir. Ambari, kümeyi izlemek ve yapılandırma değişiklikleri yapmak için kullanabilirsiniz.

## <a id="whatis"></a>Ambari nedir

[Apache Ambari](http://ambari.apache.org) web Hadoop kümeleri izlemek ve yönetmek için kullanılan kullanıcı Arabirimi sağlar. Geliştiriciler tümleştirebilir Bu yetenekler uygulamalarına kullanarak [Ambari REST API'leri](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari, Linux tabanlı Hdınsight kümeleri ile varsayılan olarak sağlanır.

## <a name="how-to-use-the-ambari-rest-api"></a>Ambari REST API kullanma

> [!IMPORTANT]
> Bu belgedeki örneklerde ve bilgi Linux işletim sistemi kullanan bir Hdınsight kümesi gerektirir. Daha fazla bilgi için bkz: [Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

Bu belgedeki örneklerde Uluç Kabuğu (bash) ve PowerShell için sağlanır. Bash örnekler GNU bash sürüm 4.3.11 test edilmiş, ancak diğer UNIX Kabukları ile çalışması gerekir. PowerShell örneklerini PowerShell 5.0 ile test edilmiş, ancak PowerShell 3.0 veya üstünü çalışması gerekir.

Kullanıyorsanız __Uluç Kabuk__ (Bash) aşağıdakilerin yüklü olması gerekir:

* [cURL](http://curl.haxx.se/): cURL olduğundan komut satırından REST API'leri ile çalışmak için kullanılan bir yardımcı programı. Bu belgede, Ambari REST API'si ile iletişim kurmak için kullanılır.

Bash veya PowerShell kullanarak olup olmadığını oluşturmuş olmanız da gerekir [jq](https://stedolan.github.io/jq/) yüklü. Jq JSON belgeleri ile çalışmaya yönelik bir yardımcı programdır. İçinde kullanılan **tüm** Bash örnekler ve **bir** PowerShell örnekler.

### <a name="base-uri-for-ambari-rest-api"></a>Temel Ambari Rest API için URI

Https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, hdınsight'ta Ambari REST API için ana URI olduğu yere **CLUSTERNAME** kümenizin adıdır.

> [!IMPORTANT]
> URI (CLUSTERNAME.azurehdinsight.net) tam etki alanı adı (FQDN) parçası olarak küme adı büyük küçük harf duyarsız olsa da, diğer örnekleri URI büyük küçük harfe duyarlıdır. Örneğin, kümenizi adlı `MyCluster`, geçerli URI'ler şunlardır:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Aşağıdaki URI'ler ikinci oluşum adının doğru durumda olmadığı için bir hata döndürür.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Kimlik Doğrulaması

Hdınsight üzerinde Ambari bağlanma HTTPS gerektirir. Yönetici hesabı adı kullanın (varsayılan **yönetici**) ve küme oluşturma sırasında sağlanan parola.

## <a name="examples-authentication-and-parsing-json"></a>Örnekler: Kimlik doğrulaması ve JSON ayrıştırma

Aşağıdaki örnekler bir GET isteği temel Ambari REST API'sine karşı nasıl yapılacağını göstermek:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> Bu belgedeki Bash örnekler aşağıdaki varsayımlar olun:
>
> * Küme için oturum açma adı varsayılan değeri `admin`.
> * `$CLUSTERNAME`Küme adını içerir. Bu değer kullanılarak ayarlanır`set CLUSTERNAME='clustername'`
> * İstendiğinde, küme oturum açma (Yönetici) için parolayı girin.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> Bu belgedeki PowerShell örnekleri aşağıdaki varsayımlar olun:
>
> * `$creds`yönetici oturum açma ve küme için parola içeren bir kimlik bilgisi nesnesidir. Bu değer kullanılarak ayarlanır `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` ve istendiğinde kimlik bilgileri sağlama.
> * `$clusterName`Küme adını içeren bir dizedir. Bu değer kullanılarak ayarlanır `$clusterName="clustername"`.

Örneklerin her ikisi de aşağıdaki örneğe benzer bilgiler ile başlayan bir JSON belgesi döndürün:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>JSON verilerini ayrıştırma

Aşağıdaki örnek kullanır `jq` JSON yanıt belge ayrıştırma ve yalnızca görüntülemek için `health_report` sonuçları bilgileri.

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 ve üstü sağlar `ConvertFrom-Json` Powershell'den çalışmak daha kolay olan bir nesneyi JSON belgesini dönüştürür cmdlet'i. Aşağıdaki örnek kullanır `ConvertFrom-Json` yalnızca görüntülenecek `health_report` sonuçları bilgileri.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Ancak bu belgenin kullanımı çoğu örneklerde `ConvertFrom-Json` yanıt belgedeki öğeleri görüntülemek için [güncelleştirme Ambari yapılandırma](#example-update-ambari-configuration) örnek jq kullanır. Jq Bu örnekte, JSON yanıt belgeden yeni bir şablon oluşturmak için kullanılır.

REST API tam başvuru için bkz: [Ambari API Başvurusu V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Örnek: küme düğümleri FQDN'sini Al

Hdınsight ile çalışırken, bir küme düğümü tam etki alanı adını (FQDN) bilmeniz gerekebilir. Aşağıdaki örnekleri kullanarak çeşitli düğümleri için FQDN'yi kolayca alabilir:

* **Tüm düğümler**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **Baş düğümler**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Çalışan düğümü**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Zookeeper düğümleri**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Örnek: küme düğümlerinin iç IP adresi al

> [!IMPORTANT]
> Bu bölümdeki örnekleri tarafından döndürülen IP adresleri internet üzerinden doğrudan erişilebilir değildir. Yalnızca Azure sanal Hdınsight kümesi içeren ağ içinde erişilebilir.
>
> Hdınsight ve sanal ağlarla çalışma hakkında daha fazla bilgi için bkz: [özel bir Azure Virtual Network kullanarak genişletme Hdınsight yetenekleri](hdinsight-extend-hadoop-virtual-network.md).

IP adresini bulmak için küme düğümleri iç tam etki alanı adını (FQDN) bilmesi gerekir. FQDN olduktan sonra ana bilgisayarın IP adresi sonra alabilirsiniz. Aşağıdaki örnekler ilk düğümlerinin tüm ana bilgisayar FQDN için Ambari sorgu ve ardından her ana bilgisayarın IP adresini Ambari sorgular.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

> [!TIP]
> Önceki örneklerde parolasını ister. Bu örnekte çalışan `curl` parola olarak sağlanan şekilde birden çok kez komutu `$PASSWORD` birden çok istemleri önlenir.

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-the-default-storage"></a>Örnek: varsayılan depolama alanı alabilir

Bir Hdınsight kümesi oluştururken, varsayılan depolama alanı olarak bir Azure depolama hesabı ya da Data Lake Store küme için kullanmanız gerekir. Ambari, Küme oluşturulduktan sonra bu bilgileri almak için kullanabilirsiniz. Örneğin, okuma/veri Hdınsight dışında kapsayıcıya yazma istiyorsanız.

Aşağıdaki örnekler, varsayılan depolama yapılandırması kümeden Al:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Bu örnekler sunucuya uygulanan ilk yapılandırmaya dönmek (`service_config_version=1`) bu bilgileri içerir. Küme oluşturulduktan sonra değiştiren bir değer almak, yapılandırma sürümlerini listelemek ve en son almak gerekebilir.

Dönüş değeri aşağıdaki örneklerde birine benzer:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Bu değer, kümenin varsayılan depolama için bir Azure Storage hesabı kullanarak gösterir. `ACCOUNTNAME` Değeri, depolama hesabının adıdır. `CONTAINER` Bölümüdür depolama hesabındaki blob kapsayıcısının adı. Kapsayıcı küme için HDFS uyumlu depolama köküdür.

* `adl://home`-Bu değer, kümenin varsayılan depolama için bir Azure Data Lake Store kullandığını gösterir.

    Data Lake Store hesap adını bulmak için aşağıdaki örneklerde kullanın:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    Dönüş değeri benzer `ACCOUNTNAME.azuredatalakestore.net`, burada `ACCOUNTNAME` Data Lake Store hesabı adıdır.

    Küme için depolama alanını içeren bir Data Lake Store içinde dizin bulmak için aşağıdaki örneklerde kullanın:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    Dönüş değeri benzer `/clusters/CLUSTERNAME/`. Bu değer, Data Lake Store hesabındaki bir yoludur. Küme için HDFS uyumlu bir dosya sisteminin kök yoludur. 

> [!NOTE]
> `Get-AzureRmHDInsightCluster` Tarafından sağlanan cmdlet [Azure PowerShell](/powershell/azure/overview) ayrıca küme için Depolama bilgileri döndürür.


## <a name="example-get-configuration"></a>Örnek: Get yapılandırma

1. Kümeniz için kullanılabilir olan yapılandırmaları alın.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Bu örnek geçerli yapılandırmasını içeren bir JSON belgesi döndürür (tarafından tanımlanan *etiketi* değeri) kümeye yüklü bileşenleri için. Aşağıdaki örnek bir Spark kümesi türünden döndürülen verileri bir alıntı aynıdır.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. İlgilendiğiniz bileşeni için yapılandırma alın. Aşağıdaki örnekte `INITIAL` etiket değeri ile önceki istekten döndürdü.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Bu örnek için geçerli yapılandırmasını içeren bir JSON belgesi döndürür `core-site` bileşeni.

## <a name="example-update-configuration"></a>Örnek: Güncelleştirme yapılandırma

1. "İstenen yapılandırma" Ambari depolar geçerli yapılandırmasını alın:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Bu örnek geçerli yapılandırmasını içeren bir JSON belgesi döndürür (tarafından tanımlanan *etiketi* değeri) kümeye yüklü bileşenleri için. Aşağıdaki örnek bir Spark kümesi türünden döndürülen verileri bir alıntı aynıdır.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    Bu listeden bileşenin adını kopyalamanız gerekir (örneğin, **spark\_thrift\_sparkconf** ve **etiketi** değeri.

2. Etiket ve Bileşen yapılandırmasını aşağıdaki komutları kullanarak Al:
   
    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > Değiştir **spark thrift sparkconf** ve **ilk** bileşeni ve yapılandırmasını almak istediğiniz etiketi.
   
    Jq Hdınsight'ta yeni bir yapılandırma şablonuna alınan verileri döndürmek için kullanılır. Özellikle, bu örnekler aşağıdaki eylemleri gerçekleştirin:
   
    * "Sürüm" dizesi ve depolanan tarihi içeren benzersiz bir değer oluşturur `newtag`.

    * Yeni istenen yapılandırma için bir kök belge oluşturur.

    * İçeriğini alır `.items[]` dizi ve altında ekler **desired_config** öğesi.

    * Siler `href`, `version`, ve `Config` öğeleri, bu öğeleri gerekli olmayan yeni bir yapılandırma göndermek için.

    * Ekler bir `tag` değerini bir öğesiyle `version#################`. Öğesinin sayısal bölümü geçerli tarih temel alır. Her yapılandırma benzersiz bir etiketi olması gerekiyor.
     
    Son olarak, verileri kaydedilen `newconfig.json` belge. Belge yapısı aşağıdaki örneğe benzer görünmelidir:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Açık `newconfig.json` belge ve değiştirebilir ve ekleyebilir değerleri `properties` nesnesi. Aşağıdaki örnek değerini değiştirir `"spark.yarn.am.memory"` gelen `"1g"` için `"3g"`. Ayrıca ekler `"spark.kryoserializer.buffer.max"` değerini `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Yapmayı değişiklikleri tamamladıktan sonra dosyayı kaydedin.

4. Ambari güncelleştirilmiş yapılandırmaya göndermek için aşağıdaki komutları kullanın.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Bu komutlar içeriğini gönderme **newconfig.json** yeni istenen yapılandırma olarak dosyaya. İstek bir JSON belgesi döndürür. **VersionTag** bu belgedeki öğe gönderdiğiniz, sürüm eşleşmelidir ve **yapılandırmalar** nesne, istenen yapılandırma değişiklikleri içerir.

### <a name="example-restart-a-service-component"></a>Örnek: bir hizmet bileşeni yeniden başlatın

Bu noktada, Ambari web kullanıcı Arabirimi bakarsanız, Spark hizmeti yeni yapılandırmanın etkili olması için önce yeniden başlatılması gerektiğini belirtir. Hizmetini yeniden başlatmak için aşağıdaki adımları kullanın.

1. Bir Spark hizmeti için bakım modunu etkinleştirmek için aşağıdakileri kullanın:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Bu komutlar bir JSON belgesi bakım modunu açar sunucusuna gönderir. Hizmet artık aşağıdaki isteği kullanarak bakım modunda olduğunu doğrulayabilirsiniz:
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    Dönüş değeri `ON`.

2. Ardından, hizmeti kapatmak için aşağıdakileri kullanın:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    Yanıt aşağıdaki örneğe benzer:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > `href` Bu URI tarafından döndürülen değer, küme düğümü iç IP adresini kullanıyor. Buradan küme dışında kullanmak için '10.0.0.18:8080' bölümüne küme FQDN ile değiştirin. 
    
    Aşağıdaki komutları isteğinin durumunu al:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Yanıtın `COMPLETED` istek tamamladığını gösterir.

3. Önceki istek tamamlandığında, hizmeti başlatmak için aşağıdakileri kullanın.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    Hizmet yeni yapılandırmayı şimdi kullanıyor.

4. Son olarak, bakım modunu devre dışı bırakmak için aşağıdakileri kullanın.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Sonraki adımlar

REST API tam başvuru için bkz: [Ambari API Başvurusu V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

