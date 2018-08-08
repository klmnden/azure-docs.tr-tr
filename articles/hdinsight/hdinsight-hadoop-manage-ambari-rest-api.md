---
title: İzleme ve Ambari REST API - Azure HDInsight ile Hadoop yönetme
description: Azure HDInsight Hadoop kümelerini yönetmek ve izlemek için Ambari kullanmayı öğrenin. Bu belgede, HDInsight kümeleriyle dahil Ambari REST API'sini kullanmayı öğreneceksiniz.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: jasonh
ms.openlocfilehash: 2208b1e2ef88bc1dc928daffa6036bfac813201f
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39598646"
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Ambari REST API'yi kullanarak HDInsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Ambari REST API'yi yönetme ve Azure HDInsight Hadoop kümelerini izleme için kullanmayı öğrenin.

Apache Ambari, yönetim ve bir kolayca web UI ve REST API'si kullanma sağlayarak bir Hadoop kümesini izleme basitleştirir. Linux işletim sistemini kullanan HDInsight kümelerinde Ambari dahildir. Ambari kümesini izleme ve yapılandırma değişiklikleri yapmak için kullanabilirsiniz.

## <a id="whatis"></a>Ambari nedir

[Apache Ambari](http://ambari.apache.org) web yönetmek ve Hadoop kümeleri izlemek için kullanılan kullanıcı Arabirimi sağlar. Geliştiriciler tümleştirilebilir yeteneklere uygulamalarına kullanarak [Ambari REST API'lerini](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari, Linux tabanlı HDInsight kümeleri ile varsayılan olarak sağlanır.

## <a name="how-to-use-the-ambari-rest-api"></a>Ambari REST API'sini kullanma

> [!IMPORTANT]
> Linux işletim sistemi kullanan bir HDInsight kümesi, bu belgedeki örnekler ve bilgi gerektirir. Daha fazla bilgi için [HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

Bu belgedeki örneklerde (bash) Uluç shell ve PowerShell için sağlanır. Bash örnekler 4.3.11 GNU bash sürümle test edilmiş, ancak diğer UNIX Kabukları ile çalışması gerekir. PowerShell örneklerini PowerShell 5. 0'ile test edilmiş, ancak PowerShell 3.0 veya üzeri çalışması gerekir.

Kullanıyorsanız __Uluç Kabuk__ (Bash) aşağıdakilerin yüklü olması gerekir:

* [cURL](http://curl.haxx.se/): cURL komut satırından REST API'leri ile çalışmak için kullanılan bir yardımcı programdır. Bu belgede, Ambari REST API ile iletişim kurmak için kullanılır.

Bash veya PowerShell kullanarak olmadığını sahip olmalısınız [jq](https://stedolan.github.io/jq/) yüklü. Jq JSON belgeleri ile çalışmak için bir yardımcı programdır. İçinde kullanılan **tüm** Bash örnekleri ve **bir** PowerShell örnekler.

### <a name="base-uri-for-ambari-rest-api"></a>Ambari Rest API URI'sini temel

HDInsight üzerinde Ambari REST API için taban URI https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAMEburada **CLUSTERNAME** kümenizin adıdır.

> [!IMPORTANT]
> Küme adı tam etki alanı adı (FQDN) bölümünde (CLUSTERNAME.azurehdinsight.net) URI'ın büyük/küçük harfe duyarsızdır sırasında diğer örnekleri urı'sindeki büyük küçük harfe duyarlıdır. Örneğin, kümenizi adlandırılmışsa `MyCluster`, geçerli bir URI'leri aşağıda verilmiştir:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Aşağıdaki bir URI'leri adı ikinci oluşum doğru durumda olduğundan bir hata döndürür.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Kimlik Doğrulaması

HDInsight üzerinde Ambari bağlanma HTTPS gerektirir. Yönetici hesabı adını kullanın (varsayılan değer **yönetici**) ve küme oluşturma sırasında sağladığınız parola.

## <a name="examples-authentication-and-parsing-json"></a>Örnekler: Kimlik doğrulaması ve JSON ayrıştırma

Aşağıdaki örnekler temel Ambari REST API'sine karşı bir GET isteği oluşturmak nasıl göstermektedir:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> Bu belge Bash örneklerde aşağıdaki varsayımlar:
>
> * Oturum açma adı küme için varsayılan değeridir `admin`.
> * `$CLUSTERNAME` Küme adını içerir. Kullanarak bu değeri ayarlayabilirsiniz. `set CLUSTERNAME='clustername'`
> * İstendiğinde, küme oturum açma (Yönetici) için parolayı girin.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> Bu belgedeki PowerShell örnekleri aşağıdaki varsayımlar:
>
> * `$creds` yönetici oturum açma ve kümenin parolasını içeren bir kimlik bilgisi nesnesidir. Bu değeri kullanarak ayarlayabilirsiniz `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` ve istendiğinde kimlik bilgileri sağlama.
> * `$clusterName` Küme adını içeren bir dizedir. Bu değeri kullanarak ayarlayabilirsiniz `$clusterName="clustername"`.

Örneklerin her ikisi de, aşağıdaki örneğe benzer bilgiler ile başlayan bir JSON belgesini döndürür:

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

Aşağıdaki örnekte `jq` JSON yanıtı belgesini ayrıştırmak ve yalnızca görüntülemek için `health_report` sonuçları bilgileri.

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 ve daha yüksek sağlar `ConvertFrom-Json` cmdlet'ini JSON belgesini Powershell'den çalışmak daha kolay bir nesneye dönüştürür. Aşağıdaki örnekte `ConvertFrom-Json` yalnızca görüntülemek için `health_report` sonuçları bilgileri.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Bu belgenin kullanımı çoğu örnek while `ConvertFrom-Json` yanıt belgedeki öğeleri görüntülemek için [güncelleştirme Ambari yapılandırma](#example-update-ambari-configuration) örnek jq kullanır. Jq, bu örnekte, JSON yanıtı belgedeki yeni bir şablon oluşturmak için kullanılır.

Eksiksiz bir REST API başvuru için bkz: [Ambari API Başvurusu V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Örnek: küme düğümlerine FQDN'sini alın

HDInsight ile çalışırken, bir küme düğümünün tam etki alanı adı (FQDN) bilmeniz gerekebilir. Aşağıdaki örnekler kullanarak kümedeki çeşitli düğümler için FQDN kolayca alabilirsiniz:

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

* **Baş düğüm**

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

* **Çalışan düğümleri**

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

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Örnek: küme düğümlerinin iç IP adresini alma

> [!IMPORTANT]
> Bu bölümdeki örnekler tarafından döndürülen IP adresleri internet üzerinden doğrudan erişilemez. Yalnızca Azure sanal içeren HDInsight küme ağından erişim sağlanabilir.
>
> HDInsight ve sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [kullanarak özel bir Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md).

IP adresini bulmak için küme düğümlerinin dahili tam etki alanı adı (FQDN) bilmeniz gerekir. FQDN oluşturduktan sonra ana bilgisayarın IP adresini alabilirsiniz. Aşağıdaki örnekler önce tüm konak düğümleri FQDN'si için Ambari sorgulamak ve ardından Ambari her ana bilgisayarın IP adresini sorgular.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

> [!TIP]
> Önceki örneklerde parolasını ister. Bu örnek çalıştırılır `curl` parola olarak sağlanan şekilde birden çok kez komut `$PASSWORD` birden çok komut istemlerini önlemek için.

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

## <a name="example-get-the-default-storage"></a>Örnek: varsayılan depolama alanı edinin

Bir HDInsight kümesi oluşturduğunuzda, varsayılan depolama alanı olarak bir Azure depolama hesabı veya Data Lake Store küme için kullanmanız gerekir. Ambari, Küme oluşturulduktan sonra bu bilgileri almak için kullanabilirsiniz. Örneğin, istediğinizde/veri kapsayıcı HDInsight dışında okuma.

Aşağıdaki örnekler, kümeden varsayılan depolama yapılandırması Al:

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
> Bu örnekler sunucuya uygulanan ilk yapılandırmayı döndürür (`service_config_version=1`) bu bilgileri içerir. Küme oluşturulduktan sonra değiştirilmiş bir değer almak, yapılandırma sürümlerini listelemek ve en son almak gerekebilir.

Dönüş değeri aşağıdaki örneklerden birini benzer:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` -Bu değer, kümenin varsayılan depolama alanı için bir Azure depolama hesabı kullandığını gösterir. `ACCOUNTNAME` Değerini depolama hesabının adıdır. `CONTAINER` Bölümüdür depolama hesabındaki blob kapsayıcısının adı. Kapsayıcı küme için HDFS uyumlu depolama köküdür.

* `adl://home` -Bu değer, kümenin varsayılan depolama alanı için bir Azure Data Lake Store kullandığını gösterir.

    Data Lake Store hesap adını bulmak için aşağıdaki örnekleri kullanın:

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

    Dönüş değeri benzer `ACCOUNTNAME.azuredatalakestore.net`burada `ACCOUNTNAME` Data Lake Store hesabının adıdır.

    Küme için depolama içeren Data Lake Store içinde dizin bulmak için aşağıdaki örnekleri kullanın:

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

    Dönüş değeri benzer `/clusters/CLUSTERNAME/`. Bu değer, Data Lake Store hesabındaki bir yoludur. Küme için HDFS uyumlu dosya sisteminin kökünü yoludur. 

> [!NOTE]
> `Get-AzureRmHDInsightCluster` Cmdlet'i tarafından sağlanan [Azure PowerShell](/powershell/azure/overview) ayrıca küme için depolama bilgilerini döndürür.


## <a name="example-get-configuration"></a>Örnek: Get yapılandırma

1. Kümeniz için kullanılabilir yapılandırmaları alın.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Bu örnek geçerli yapılandırmasını içeren bir JSON belgesini döndürür (tarafından tanımlanan *etiketi* değeri) kümeye yüklü bileşenler için. Aşağıdaki örnekte bir Spark kümesi türden döndürülen veriler kitabından bulunur.
   
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

2. İlgilendiğiniz bileşeni için yapılandırma alın. Aşağıdaki örnekte, değiştirin `INITIAL` etiket değeri ile önceki istekten döndürdü.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Bu örnek geçerli yapılandırmasını içeren bir JSON belgesini döndürür `core-site` bileşeni.

## <a name="example-update-configuration"></a>Örnek: Güncelleştirme yapılandırması

1. Ambari "istenen yapılandırma" depolayan geçerli yapılandırmasını alın:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Bu örnek geçerli yapılandırmasını içeren bir JSON belgesini döndürür (tarafından tanımlanan *etiketi* değeri) kümeye yüklü bileşenler için. Aşağıdaki örnekte bir Spark kümesi türden döndürülen veriler kitabından bulunur.
   
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
   
    Bu listede, bileşenin adını kopyalamanız gerekir (örneğin, **spark\_thrift\_sparkconf** ve **etiketi** değeri.

2. Aşağıdaki komutları kullanarak etiket ve Bileşen yapılandırmasını alın:
   
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
    > Değiştirin **spark thrift sparkconf** ve **ilk** bileşen ve yapılandırmasını almak istediğiniz etiketi.
   
    Jq, yeni bir yapılandırma şablonuna HDInsight alınan verileri döndürmek için kullanılır. Özellikle, bu örnekler, aşağıdaki eylemleri gerçekleştirin:
   
    * "Sürüm" dizesi ve depolanır tarihi içeren benzersiz bir değer oluşturur `newtag`.

    * Yeni bir istenen yapılandırma için bir kök belge oluşturulur.

    * İçeriğini alır `.items[]` altına ekler ve dizi **desired_config** öğesi.

    * Siler `href`, `version`, ve `Config` öğeleri, bu öğeleri gerekli olmayan yeni bir yapılandırma göndermek için.

    * Ekler bir `tag` öğe değerini `version#################`. Sayısal bölümü geçerli tarihi temel alır. Her yapılandırma benzersiz etiket olmalıdır.
     
    Son olarak, veri için kaydedilen `newconfig.json` belge. Belge yapısı aşağıdaki örneğe benzer görünmelidir:
     
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

3. Açık `newconfig.json` belge ve değiştirme/ekleme değerleri `properties` nesne. Aşağıdaki örnek değiştirir `"spark.yarn.am.memory"` gelen `"1g"` için `"3g"`. Ayrıca ekler `"spark.kryoserializer.buffer.max"` değeriyle `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Değişiklikler yapmayı tamamladıktan sonra dosyayı kaydedin.

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
   
    Bu komutlar içeriğini gönderme **newconfig.json** dosyasını kümeye yeni bir istenen yapılandırma olarak. İstek bir JSON belgesini döndürür. **VersionTag** bu belgedeki öğe, sunduğunuz sürüm eşleşmelidir ve **yapılandırmaları** nesne, istenen yapılandırma değişiklikleri içerir.

### <a name="example-restart-a-service-component"></a>Örnek: bir hizmet bileşeni yeniden başlatın.

Bu noktada, Ambari web kullanıcı arabirimini bakarsanız, bir Spark hizmeti yeni yapılandırmayı gerçekleştirilmeden önce yeniden başlatılması gerektiğini belirtir. Hizmeti yeniden başlatmak için aşağıdaki adımları kullanın.

1. Bir Spark hizmeti bakım modunu etkinleştirmek için aşağıdakileri kullanın:

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
   
    Bu komutları bir JSON belgesi bakım modunu açar sunucusuna gönderir. Hizmetin artık aşağıdaki isteği kullanarak bakım modunda olduğunu doğrulayabilirsiniz:
   
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
    > `href` Bu URI tarafından döndürülen değeri küme düğümünün iç IP adresi kullanıyor. Buradan küme dışında kullanmak için '10.0.0.18:8080' bölümüne küme FQDN ile değiştirin. 
    
    Aşağıdaki komutlar isteğinin durumunu alır:

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

    Yanıtın `COMPLETED` istek bitirdiğini belirtir.

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
    Hizmet yeni yapılandırmayı artık kullanıyor.

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

Eksiksiz bir REST API başvuru için bkz: [Ambari API Başvurusu V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

