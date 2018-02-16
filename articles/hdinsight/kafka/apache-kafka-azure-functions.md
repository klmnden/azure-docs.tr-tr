---
title: "Hdınsight üzerinde Kafka veri göndermek için Azure işlevleri kullanın | Microsoft Docs"
description: "Hdınsight üzerinde Kafka için veri yazmak için bir Azure işlevi kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/09/2018
ms.author: larryfr
ms.openlocfilehash: c1c03cfcbcb7e0bfdb4a631b9e2ae568f0684069
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="use-kafka-on-hdinsight-from-an-azure-function-app"></a>Kafka Hdınsight'ta bir Azure işlevi uygulamadan kullanın.

Hdınsight üzerinde Kafka veri gönderdiğini bir Azure işlevi uygulama oluşturmayı öğrenin.

[Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/) sunucusuz hesaplama hizmetidir. Açıkça sağlamak veya altyapısını yönetmek zorunda kalmadan kodu isteğe bağlı çalıştırmanıza olanak sağlar.

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar. HDInsight üzerinde Kafka, Microsoft Azure bulutunda yönetilen, yüksek oranda ölçeklenebilir ve yüksek oranda kullanılabilir bir hizmet sağlar.

Hdınsight üzerinde Kafka, ortak internet'te bir API sağlamaz. Yayımlamak veya Kafka verileri kullanmak için bir Azure sanal ağı kullanarak Kafka kümeye bağlanmanız gerekir. Azure işlevleri veriler Kafka Hdınsight üzerinde bir sanal ağ geçidinden iter genel bir uç nokta oluşturmak için kolay bir yol sağlar.

> [!NOTE]
> Azure hdınsight'ta Kafka ile iletişim kurmak işlevleri etkinleştirmek için gereken adımlar bu belgenin odak noktasıdır. Örnek Yapılandırması çalıştığını göstermek için yalnızca bir temel Kafka üretici ' dir.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure işlevi uygulama. Daha fazla bilgi için bkz: [ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md) belgeleri.

* Bir Azure sanal ağı. Azure işlevleri ile çalışmak için sanal ağ aşağıdaki IP aralıklarını birini kullanmanız gerekir:

    * 10.0.0.0-10.255.255.255
    * 172.16.0.0-172.31.255.255
    * 192.168.0.0-192.168.255.255

    Daha fazla bilgi için bkz: [uygulamanız bir Azure sanal ağı ile tümleştirmek](../../app-service/web-sites-integrate-with-vnet.md) belge.

* Hdınsight kümesi üzerinde Kafka. Hdınsight kümesinde bir Kafka oluşturma hakkında daha fazla bilgi için bkz: [Kafka küme oluşturma](apache-kafka-get-started.md) belge.

    > [!IMPORTANT]
    > Küme yapılandırması sırasında kullanmalısınız __Gelişmiş ayarları__ adım Hdınsight kullanan alt ağ ve Azure sanal ağı seçin. Önceki adımda oluşturduğunuz alt ağ ve sanal ağ seçin.

    Kafka ve sanal ağlar hakkında daha fazla bilgi için bkz: [Kafka bir sanal ağ üzerinden Bağlan](apache-kafka-connect-vpn-gateway.md) belge.

## <a name="architecture"></a>Mimari

Hdınsight üzerinde Kafka, Azure sanal ağında yer alıyor. Azure işlevleri, bir noktadan siteye ağ geçidi kullanarak sanal ağ ile iletişim kurabilir. Aşağıdaki resimde, bu ağ topolojisinin bir diyagramıdır:

![Azure Hdınsight için bağlanma işlevleri mimarisi](./media/apache-kafka-azure-functions/kafka-azure-functions.png)

## <a name="configure-kafka"></a>Kafka yapılandırın

Bu bölümdeki bilgiler, Kafka küme Azure işlevi verileri kabul etmek üzere hazırlar. Aşağıdaki yapılandırma işlemleri kapsar:

* IP reklam
* Toplama Kafka Aracısı IP adresleri
* Bir Kafka konu oluşturma

### <a name="configure-kafka-for-ip-advertising"></a>Kafka IP reklam için yapılandırma

Varsayılan olarak, Zookeeper istemcilere Kafka aracıların etki alanı adını döndürür. Sanal ağ adları (Azure işlevleri) istemci çözümlenemiyor gibi bu yapılandırma bir DNS sunucusu çalışmaz. Bu yapılandırma için IP adresleri etki alanı adları yerine tanıtmak için Kafka yapılandırmak için aşağıdaki adımları kullanın:

1. Bir web tarayıcısı kullanarak https://CLUSTERNAME.azurehdinsight.net için gidin. Değiştir __CLUSTERNAME__ Hdınsight kümesinde Kafka adı.

    İstendiğinde, küme için HTTPS kullanıcı adı ve parola kullanın. Küme için Ambari Web kullanıcı Arabirimi görüntülenir.

2. Kafka hakkında bilgileri görüntülemek için seçin __Kafka__ sol taraftaki listeden.

    ![Hizmet listesi Kafka ile vurgulanan](./media/apache-kafka-azure-functions/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için seçin __yapılandırmalar__ üst ortasından.

    ![Kafka için yapılandırmalar bağlantıları](./media/apache-kafka-azure-functions/select-kafka-config.png)

4. Bulunacak __kafka env__ yapılandırma, girin `kafka-env` içinde __filtre__ sağ üst alanını.

    ![Kafka env için Kafka yapılandırma](./media/apache-kafka-azure-functions/search-for-kafka-env.png)

5. IP adresleri tanıtmak için Kafka yapılandırmak için aşağıdaki metni altına ekleyin __kafka env şablonu__ alan:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka dinlediği arabirimi yapılandırmak için girin `listeners` içinde __filtre__ sağ üst alanını.

7. Tüm ağ arabirimleri üzerinde dinlenecek Kafka yapılandırmak için değeri değiştirin __dinleyicileri__ alanı `PLAINTEXT://0.0.0.0:9092`.

8. Yapılandırma değişikliklerini kaydetmek için kullanın __kaydetmek__ düğmesi. Değişiklikleri açıklayan bir metin iletisi girin. Seçin __Tamam__ değişiklikler kaydedildikten sonra.

    ![Kaydet Yapılandırma düğmesi](./media/apache-kafka-azure-functions/save-button.png)

9. Kafka yeniden başlatılırken hataları önlemek için kullanmak __hizmet eylemleri__ düğmesine tıklayın ve ardından __kapatma üzerinde Bakım modu__. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Hizmet eylemlerle vurgulanmış bakım etkinleştirin](./media/apache-kafka-azure-functions/turn-on-maintenance-mode.png)

10. Kafka yeniden başlatmak için kullanın __yeniden__ düğmesine tıklayın ve ardından __yeniden tüm etkilenen__. Yeniden başlatma onaylayın ve ardından __Tamam__ işlemi tamamlandıktan sonra düğmesine tıklayın.

    ![Etkilenen yeniden başlatma ile tüm düğmesi vurgulanmış yeniden başlatın](./media/apache-kafka-azure-functions/restart-button.png)

11. Bakım modu devre dışı bırakmak için __hizmet eylemleri__ düğmesine tıklayın ve ardından __Bakım modu devre dışı bırakma__. Seçin **Tamam** bu işlemi tamamlamak için.

### <a name="get-the-kafka-broker-ip-address"></a>Kafka Aracısı IP adresi al

Tam etki alanı adı (FQDN) ve Kafka kümedeki düğümlerin IP adreslerini almak için aşağıdaki yöntemlerden birini kullanın:

```powershell
$resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

$clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

$nodes = @()
foreach($nic in $clusterNICs) {
    $node = new-object System.Object
    $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
    $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
    $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
    $nodes += $node
}
$nodes | sort-object Type
```

```azurecli
az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
```

Bu komut varsayar `<resourcegroupname>` sanal ağı içeren Azure kaynak grubunun adı.

Döndürülen adları listesinden bir workernode IP adresini seçin. İç düğümün tam etki alanı adı harfler ile başlayan `wn`. Bu IP adresi işlev tarafından Kafka ile iletişim kurmak için kullanılır.

### <a name="create-a-kafka-topic"></a>Kafka konu oluştur

Kafka verileri depolayan __konuları__. Bir Azure işlevinden Kafka için verileri göndermeden önce işlevi oluşturmanız gerekir.

Bir konu oluşturmak için adımlarda kullanın [Kafka küme oluşturma](apache-kafka-get-started.md) belge.

## <a name="create-a-function-that-sends-to-kafka"></a>Kafka için gönderen bir işlev oluşturun

> [!NOTE]
> Bu bölümdeki adımları kullanan bir JavaScript işlevi oluşturma [kafka düğümlü](https://www.npmjs.com/package/kafka-node) Kafka için veri yayımlamak için paket:

1. Yeni bir __Web kancası + API__ işlev ve seçin __JavaScript__ dili olarak. Yeni işlevler oluşturma hakkında daha fazla bilgi için bkz: [ilk işlevinizi oluşturma](../../azure-functions/functions-create-first-azure-function.md#create-function) belge.

2. Aşağıdaki kod işlev gövdesi olarak kullanın:

    ```javascript
    var kafka = require('kafka-node');

    // The topic and a Kafka broker host for your HDInsight cluster
    var topic = 'test';
    var brokerHost = '10.1.0.7:9092';
    // Create the client
    var client= new kafka.KafkaClient({kafkaHost: brokerHost});

    module.exports = function (context, req) {
        // Create the producer
        var producer = new kafka.Producer(client, {requireAcks: 1});

        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name || (req.body && req.body.name)) {
            var name = req.query.name || req.body.name
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "Hello " + name
            };
            // Create the payload, using the name as the body
            var payloads = [
                    { topic: topic, messages: [name]}
            ];
            context.log("calling producer.send");
            // Send the data to Kafka
            producer.send(payloads, function(err, data) {
                if(err) {
                    context.log(err);
                } else {
                    context.log(data);
                }
            });
        }
        else {
            context.res = {
                status: 400,
                body: "Please pass a name on the query string or in the request body"
            };
        }
        context.done();
    };
    ```

    Değiştir `'test'` Hdınsight kümenize oluşturduğunuz Kafka konu adı.

    Değiştir `10.1.0.7` daha önce IP adresine sahip. Bırakın `:9092` değeri. 9092 Kafka dinlediği bağlantı noktasıdır.

    Kullanım __kaydetmek__ değişiklikleri kaydetmek için düğmesi.

    ![Düzenleyici ile Kaydet düğmesi](./media/apache-kafka-azure-functions/function-editor.png)

3. İşlev Düzenleyicisi'ni sağdan seçin __dosyaları görüntüleyebilir__. Seçin __+ Ekle__ ve adlı yeni bir dosya ekleyin `package.json`. Bu dosya üzerinde bağımlılık belirtmek için kullanılan `kafka-node` paket.

    ![Bir dosya ekleme](./media/apache-kafka-azure-functions/add-files.png)

4. Yeni dosyayı düzenlemek için seçin `package.json` gelen __dosyaları görüntüleyebilir__ listesi. Aşağıdaki metin dosyasının içeriği kullanın:

    ```json
    {
    "name": "kafkatest",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "author": "",
    "license": "ISC",
    "dependencies": {
            "kafka-node": "^2.4.1"
        }
    }
    ```

    Kullanım __kaydetmek__ değişiklikleri kaydetmek için düğmesi.

5. Yüklemek için `kafka-node` paketini, kullanın _düğüm sürümü ve paket Yönetimi_ bölümünü [Azure işlevleri JavaScript Geliştirici Kılavuzu](../../azure-functions/functions-reference-node.md#node-version-and-package-management).

    > [!NOTE]
    > Kafka düğümü paketin yüklü olarak çeşitli hatalar alabilirsiniz. Bu hataları güvenle yoksayabilirsiniz.

## <a name="run-the-function"></a>İşlevi çalıştırmak

İşlev Düzenleyicisi'ni sağdan seçin __Test__. Test için varsayılan ayarları bırakın ve ardından __çalıştırmak__. Test çalışmaları gibi başarılı bir `name` işlev parametresi. Bu parametre değerini içeren `Azure`, hangi işlev Kafka ekler.

![Test iletişim ekran görüntüsü](./media/apache-kafka-azure-functions/function-test-dialog.png)

Test çalıştırılırken işlev tarafından günlüğe kaydedilen bilgi görüntülemek için seçin __günlükleri__ sayfanın sonundaki. Günlük bilgileri yeniden oluşturmak için testi çalıştırın.

![İşlev günlük çıktı örneği](./media/apache-kafka-azure-functions/function-log.png)

## <a name="verify-the-data-is-in-kafka"></a>Veriler Kafka içinde olduğunu doğrulayın

Veriler Kafka konusunda geldiğini doğrulamak için yer alan bilgileri kullanın. _üretmek ve kayıtları kullanma_ bölümünü [Kafka küme oluşturma](apache-kafka-get-started.md#produce-and-consume-records) belge. `kafka-console-consumer` Konusundan veri okuyan ve konu başlığı altında depolanan iletilerinin listesini görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Kafka kullanmaya başlama](apache-kafka-get-started.md)

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](apache-kafka-mirroring.md)

* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)