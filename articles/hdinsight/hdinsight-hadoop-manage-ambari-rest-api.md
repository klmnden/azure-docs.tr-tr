---
title: İzleme ve Ambari REST API - Azure HDInsight ile Hadoop yönetme
description: Azure HDInsight Hadoop kümelerini yönetmek ve izlemek için Ambari kullanmayı öğrenin. Bu belgede, HDInsight kümeleriyle dahil Ambari REST API'sini kullanmayı öğreneceksiniz.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: hrasheed
ms.openlocfilehash: a56cc0c575a6e50a38aea91c8fc2e1855617457f
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648393"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-rest-api"></a>Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Yönetme ve Azure HDInsight, Apache Hadoop kümelerini izleme için Apache Ambari REST API'sini kullanmayı öğrenin.

## <a id="whatis"></a>Apache Ambari nedir
[Apache Ambari](https://ambari.apache.org) yönetimi ve kullanımı kolay bir web kullanıcı Arabirimi tarafından yedeklenen sağlayarak Hadoop kümelerini izleme basitleştirir, [REST API'leri](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).  Ambari, Linux tabanlı HDInsight kümeleri ile varsayılan olarak sağlanır.

## <a name="prerequisites"></a>Önkoşullar
* **HDInsight Hadoop kümesinde**. Bkz: [Linux'ta HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Windows 10 üzerinde ubuntu'da bash**.  Bu makaledeki örnekler Windows 10 üzerinde Bash kabuğunu kullanın. Bkz: [Linux Yükleme Kılavuzu için Windows 10 için Windows alt sistemi](https://docs.microsoft.com/windows/wsl/install-win10) yükleme adımları için.  Diğer [UNIX Kabukları](https://www.gnu.org/software/bash/) de çalışır.  Örnekler, bazı küçük değişiklikler ile bir Windows komut istemi üzerinde çalışabilir.  Alternatif olarak, Windows PowerShell kullanabilirsiniz.

* **jq**, bir komut satırı JSON işlemcisine giden.  Bkz: [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

* **Windows PowerShell**.  Alternatif olarak, [Bash](https://www.gnu.org/software/bash/).

## <a name="base-uri-for-ambari-rest-api"></a>Ambari Rest API URI'sini temel

 HDInsight üzerinde Ambari REST API için taban URI `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`burada `CLUSTERNAME` kümenizin adıdır.  Küme adları içinde bir URI'leri **büyük/küçük harfe**.  Küme adı tam etki alanı adı (FQDN) bölümünde (CLUSTERNAME.azurehdinsight.net) URI'ın büyük/küçük harfe duyarsızdır sırasında diğer örnekleri urı'sindeki büyük küçük harfe duyarlıdır.

## <a name="authentication"></a>Authentication

HDInsight üzerinde Ambari bağlanma HTTPS gerektirir. Yönetici hesabı adını kullanın (varsayılan değer **yönetici**) ve küme oluşturma sırasında sağladığınız parola.

## <a name="examples"></a>Örnekler

### <a name="setup-preserve-credentials"></a>Kurulum (kimlik bilgileri Koru)
Her örnek için bunları yeniden girildi önlemek için kimlik bilgilerinizi korur.  Küme adı, ayrı bir adımla korunur.

**BİR. Bash**  
Değiştirerek aşağıdaki betiği düzenleyin `PASSWORD` gerçek parolanızla.  Ardından komutu girin.

```bash
export password='PASSWORD'
```  

**B. PowerShell**  

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>Büyük küçük harfleri doğru küme adı belirleyin
Küme adı gerçek büyük küçük harfleri, küme nasıl oluşturulduğuna bağlı olarak beklediğinizden farklı olabilir.  Buradaki adımları gerçek büyük/küçük harf Göster ve sonraki tüm örnekleri için bir değişkende depolayın.

Değiştirmek için aşağıdaki betiklerini Düzenle `CLUSTERNAME` ile kümenizin adıdır. Ardından komutu girin. (Küme adı FQDN için büyük küçük harfe duyarlı değildir.)

**BİR. Bash**  

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLSUTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

### <a name="parsing-json-data"></a>JSON verilerini ayrıştırma

Aşağıdaki örnekte [jq](https://stedolan.github.io/jq/) veya [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json) JSON yanıtı belgesini ayrıştırmak ve yalnızca görüntülemek için `health_report` sonuçları bilgileri.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" \
| jq '.Clusters.health_report'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```   

### <a name="example-get-the-fqdn-of-cluster-nodes"></a> Küme düğümleri FQDN'sini alın

HDInsight ile çalışırken, bir küme düğümünün tam etki alanı adı (FQDN) bilmeniz gerekebilir. Aşağıdaki örnekler kullanarak kümedeki çeşitli düğümler için FQDN kolayca alabilirsiniz:

**Tüm düğümler**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq '.items[].Hosts.host_name'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```

**Baş düğüm**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Çalışan düğümleri**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Zookeeper düğümleri**

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
| jq ".host_components[].HostRoles.host_name"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

### <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a> Küme düğümlerinin iç IP adresini alma

Bu bölümdeki örnekler tarafından döndürülen IP adresleri internet üzerinden doğrudan erişilemez. Yalnızca Azure sanal içeren HDInsight küme ağından erişim sağlanabilir.

HDInsight ve sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [kullanarak özel bir Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md).

IP adresini bulmak için küme düğümlerinin dahili tam etki alanı adı (FQDN) bilmeniz gerekir. FQDN oluşturduktan sonra ana bilgisayarın IP adresini alabilirsiniz. Aşağıdaki örnekler önce tüm konak düğümleri FQDN'si için Ambari sorgulamak ve ardından Ambari her ana bilgisayarın IP adresini sorgular.

```bash
for HOSTNAME in $(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```  

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" 
$resp = Invoke-WebRequest -Uri $uri -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds -UseBasicParsing
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

### <a name="get-the-default-storage"></a>Varsayılan depolama alanı edinin

Bir HDInsight kümesi oluşturduğunuzda, varsayılan depolama alanı olarak bir Azure depolama hesabı veya Data Lake depolama kümesi için kullanmanız gerekir. Ambari, Küme oluşturulduktan sonra bu bilgileri almak için kullanabilirsiniz. Örneğin, istediğinizde/veri kapsayıcı HDInsight dışında okuma.

Aşağıdaki örnekler, kümeden varsayılan depolama yapılandırması Al:

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]  
> Bu örnekler sunucuya uygulanan ilk yapılandırmayı döndürür (`service_config_version=1`) bu bilgileri içerir. Küme oluşturulduktan sonra değiştirilmiş bir değer almak, yapılandırma sürümlerini listelemek ve en son almak gerekebilir.

Dönüş değeri aşağıdaki örneklerden birini benzer:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` -Bu değer, kümenin varsayılan depolama alanı için bir Azure depolama hesabı kullandığını gösterir. `ACCOUNTNAME` Değerini depolama hesabının adıdır. `CONTAINER` Bölümüdür depolama hesabındaki blob kapsayıcısının adı. Kapsayıcı küme için HDFS uyumlu depolama köküdür.

* `abfs://CONTAINER@ACCOUNTNAME.dfs.core.windows.net` -Bu değer, kümenin varsayılan depolama alanı için Azure Data Lake depolama Gen2'ye kullandığını gösterir. `ACCOUNTNAME` Ve `CONTAINER` değerlerini Azure depolama, daha önce belirtildiği gibi aynı anlama sahiptir.

* `adl://home` -Bu değer, kümenin varsayılan depolama alanı için Azure Data Lake depolama Gen1 kullandığını gösterir.

    Data Lake Store hesap adını bulmak için aşağıdaki örnekleri kullanın:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    Dönüş değeri benzer `ACCOUNTNAME.azuredatalakestore.net`burada `ACCOUNTNAME` Data Lake depolama hesabının adıdır.

    Data Lake depolama kümesi için depolamayı içeren dizininde bulmak için aşağıdaki örnekleri kullanın:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```  

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    Dönüş değeri benzer `/clusters/CLUSTERNAME/`. Bu değer, Data Lake Store hesabındaki bir yoludur. Küme için HDFS uyumlu dosya sisteminin kökünü yoludur.  

> [!NOTE]  
> [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) cmdlet'i tarafından sağlanan [Azure PowerShell](/powershell/azure/overview) ayrıca küme için depolama bilgilerini döndürür.


### <a name="get-all-configurations"></a> Tüm yapılandırmalarını alma

Kümeniz için kullanılabilir yapılandırmaları alın.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName?fields=Clusters/desired_configs"
```

```powershell
$respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
    -Credential $creds -UseBasicParsing
$respObj.Content
```

Bu örnek geçerli yapılandırmasını içeren bir JSON belgesini döndürür (tarafından tanımlanan *etiketi* değeri) kümeye yüklü bileşenler için. Aşağıdaki örnekte bir Spark kümesi türden döndürülen veriler kitabından bulunur.
   
```json
"jupyter-site" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-client-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
```

### <a name="get-configuration-for-specific-component"></a>Belirli bir bileşeni için yapılandırmasını alma

İlgilendiğiniz bileşeni için yapılandırma alın. Aşağıdaki örnekte, değiştirin `INITIAL` etiket değeri ile önceki istekten döndürdü.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
    -Credential $creds -UseBasicParsing
$resp.Content
```

Bu örnek geçerli yapılandırmasını içeren bir JSON belgesini döndürür `livy2-conf` bileşeni.

### <a name="update-configuration"></a>Güncelleştirme yapılandırması

1. Oluşturma `newconfig.json`.  
   Değiştirin ve ardından aşağıdaki komutları girin:

   * Değiştirin `livy2-conf` istenen bileşeni ile.
   * Değiştirin `INITIAL` alınan gerçek değerle `tag` gelen [tüm yapılandırmalarını alma](#get-all-configurations).

     **BİR. Bash**  
     ```bash
     curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" \
     | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json

     ```

     **B. powershell**  
     PowerShell betiğini kullanır [jq](https://stedolan.github.io/jq/).  Düzen `C:\HD\jq\jq-win64` gerçek yol ve sürümünü yansıtacak şekilde aşağıdaki [jq](https://stedolan.github.io/jq/).

     ```powershell
     $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
     $now = Get-Date
     $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
     $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
       -Credential $creds -UseBasicParsing
     $resp.Content | C:\HD\jq\jq-win64 --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
     ```

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
           "tag": "version1552064778014",
           "type": "livy2-conf",
           "properties": {
             "livy.environment": "production",
             "livy.impersonation.enabled": "true",
             "livy.repl.enableHiveContext": "true",
             "livy.server.csrf_protection.enabled": "true",
               ....
           },
         },
       }
     }
     ```

2. Düzen `newconfig.json`.  
   Açık `newconfig.json` belge ve değiştirme/ekleme değerleri `properties` nesne. Aşağıdaki örnek değiştirir `"livy.server.csrf_protection.enabled"` gelen `"true"` için `"false"`.

        "livy.server.csrf_protection.enabled": "false",

    Değişiklikler yapmayı tamamladıktan sonra dosyayı kaydedin.

3. Gönderme `newconfig.json`.  
   Ambari güncelleştirilmiş yapılandırmaya göndermek için aşağıdaki komutları kullanın.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```  

    Bu komutlar içeriğini gönderme **newconfig.json** dosyasını kümeye yeni bir istenen yapılandırma olarak. İstek bir JSON belgesini döndürür. **VersionTag** bu belgedeki öğe, sunduğunuz sürüm eşleşmelidir ve **yapılandırmaları** nesne, istenen yapılandırma değişiklikleri içerir.

### <a name="restart-a-service-component"></a>Bir hizmet bileşeni yeniden Başlat

Bu noktada, Ambari web kullanıcı arabirimini bakarsanız, bir Spark hizmeti yeni yapılandırmayı gerçekleştirilmeden önce yeniden başlatılması gerektiğini belirtir. Hizmeti yeniden başlatmak için aşağıdaki adımları kullanın.

1. Spark2 hizmeti için bakım modunu etkinleştirmek için aşağıdakileri kullanın:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```

2. Bakım modu doğrulayın  

    Bu komutları bir JSON belgesi bakım modunu açar sunucusuna gönderir. Hizmetin artık aşağıdaki isteği kullanarak bakım modunda olduğunu doğrulayabilirsiniz:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```

    Dönüş değeri `ON`.

3. Ardından, Spark2 hizmeti kapatmak için aşağıdakileri kullanın:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://girouxSpark.azurehdinsight.net/api/v1/clusters/girouxspark/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
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

4. İstek doğrulayın.  
    Değiştirerek aşağıdaki komutu düzenleyin `29` gerçek değeri ile `id` önceki adımdan döndürdü.  Aşağıdaki komutlar isteğinin durumunu alır:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Yanıtın `COMPLETED` istek bitirdiğini belirtir.

5. Önceki istek tamamlandığında Spark2 hizmetini başlatmak için aşağıdakileri kullanın.
   
    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```

    Hizmet yeni yapılandırmayı artık kullanıyor.

6. Son olarak, bakım modunu devre dışı bırakmak için aşağıdakileri kullanın.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'

    ```

## <a name="next-steps"></a>Sonraki adımlar

Eksiksiz bir REST API başvuru için bkz: [Apache Ambari API Başvurusu V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

