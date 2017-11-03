---
title: "Azure IOT paketi bağlı Fabrika OPC yayımcı kullanın | Microsoft Docs"
description: "Derleme ve bağlı Üreteç OPC yayımcı başvuru uygulaması dağıtma nasıl."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: dobett
ms.openlocfilehash: fd823194f6e51600b9d4ca1daa053db837871fef
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="opc-publisher-for-azure-iot-edge"></a>Azure IOT kenar OPC yayımcısı

Bu makalede, OPC yayımcı başvuru uygulaması kullanmayı açıklar. Başvuru uygulaması Azure IOT kenarına kullanımı gösterilmiştir:

- Varolan OPC UA sunucuya bağlanın.
- JSON olarak kodlanmış telemetri verilerini bu sunuculardan OPC UA yayımlama *Pub/alt* biçimi, Azure IOT Hub'ına bir JSON yükü kullanılarak. Azure IOT kenar destekleyen aktarım protokolünü kullanabilirsiniz.

Bu başvuru uygulaması içerir:

- OPC UA *istemci* ağınızdaki varolan OPC UA sunucularına bağlanmak için.
- OPC UA *server* ne yayımlanan yönetmek için kullanabileceğiniz 62222 bağlantı noktasında dinleme.

Uygulama, .NET Core kullanılarak uygulanan ve .NET Core tarafından desteklenen platformlardaki çalıştırabilirsiniz.

Uç noktaları için bağlantı kurduğunda yayımcı yeniden deneme mantığı uygular. Yayımcı Canlı Tut isteklerini belirli bir süre içinde yanıt için uç nokta bekler. Bu yeniden deneme mantığı publisher'ın bir OPC UA sunucusu için bir güç kesintisi gibi koşulları algılamak sağlar.

OPC UA sunucusuna her ayrı yayımlama aralığı için bu yayımlama aralığı ile tüm düğümleri güncelleştirildiği ayrı bir abonelik oluşturur.

Ağ yükünü azaltmak için IOT Hub'ına gönderilen verilerin toplu işleme yayımcı destekler. Yalnızca yapılandırılan paket boyutu ulaşıldığında toplu IOT Hub'ına gönderilir.

Bu uygulama OPC temelleri'nın OPC UA başvuru yığını kullanır ve bu nedenle lisans kısıtlamaları uygulayın. Http://opcfoundation.github.io/UA-.NETStandardLibrary/ OPC UA belgelerine ve Lisans Koşulları'nı ziyaret edin.

OPC yayımcı kaynak kodunda bulabilirsiniz [OPC yayımcı için Azure IOT kenar](https://github.com/Azure/iot-edge-opc-publisher) GitHub depo.

## <a name="prerequisites"></a>Ön koşullar

Uygulama oluşturmak için ihtiyacınız [.NET Core SDK 1.1.](https://docs.microsoft.com/dotnet/core/sdk) işletim sisteminiz için.

## <a name="build-the-application"></a>Uygulama oluşturma

### <a name="as-native-windows-application"></a>Yerel Windows uygulaması olarak

Açık `OpcPublisher.sln` proje ile Visual Studio 2017 ve F7 basarsa tarafından çözümü oluşturun.

### <a name="as-docker-container"></a>Docker kapsayıcısı

Uygulamayı bir Windows Docker kapsayıcısı olarak oluşturmak için kullanmak `Dockerfile.Windows` yapılandırma dosyası.

Linux Docker kapsayıcısı olarak uygulama oluşturmak için kullanmak `Dockerfile` yapılandırma dosyası.

Geliştirme makinenizde depo kökünden konsol penceresinde aşağıdaki komutu yazın:

`docker build -f <docker-configfile-to-use> -t <your-container-name> .`

`-f` Seçenek için `docker build` isteğe bağlıdır ve varsayılan kullanmaktır `Dockerfile` yapılandırma dosyası.

Docker doğrudan bir git deposundan oluşturmanıza olanak sağlar. Linux Docker kapsayıcısı aşağıdaki komutla oluşturabilirsiniz:

`docker build -t <your-container-name> .https://github.com/Azure/iot-edge-opc-publisher`

## <a name="configure-the-opc-ua-nodes-to-publish"></a>OPC UA düğümleri yayımlamak için yapılandırma

Hangi OPC UA düğümlerin değerlerine Azure IOT Hub'ına yayımladığınız yapılandırmak için bir JSON biçimli yapılandırma dosyası oluşturun. Bu yapılandırma dosyası için varsayılan ad `publishednodes.json`. Uygulama güncelleştirmeleri ve OPC UA kullanırken bu yapılandırma dosyası sunucu yöntemlerini kaydeder **PublishNode** veya **UnpublishNode**.

Yapılandırma dosyasının söz dizimi aşağıdaki gibidir:

```json
[
    {
        // example for an EnpointUrl is: opc.tcp://win10iot:51210/UA/SampleServer
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "OpcNodes": [
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised publishing interval).
            {
                // The identifier specifies the NamespaceUri and the node identifier in XML notation as specified in Part 6 of the OPC UA specification in the XML Mapping section.
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258"
            },
            // Publisher will request the server at EndpointUrl to sample the node with the OPC sampling interval specified on command line (or the default value: OPC publishing interval)
            // and the subscription will publish the node value with an OPC publishing interval of 4 seconds.
            // Publisher will use for each dinstinct publishing interval (of nodes on the same EndpointUrl) a separate subscription. All nodes without a publishing interval setting,
            // will be on the same subscription and the OPC UA stack will publish with the lowest sampling interval of a node.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                "OpcPublishingInterval": 4000
            },
            // Publisher will request the server at EndpointUrl to sample the node with the given sampling interval of 1 second
            // and the subscription will publish the node value with the OPC publishing interval specified on command line (or the default value: server revised interval).
            // If the OPC publishing interval is set to a lower value, Publisher will adjust the OPC publishing interval of the subscription to the OPC sampling interval value.
            {
                "ExpandedNodeId": "nsu=http://opcfoundation.org/UA/;i=2258",
                // the OPC sampling interval to use for this node.
                "OpcSamplingInterval": 1000
            }
        ]
    },

    // the format below is only supported for backward compatibility. you need to ensure that the
    // OPC UA server on the configured EndpointUrl has the namespaceindex you expect with your configuration.
    // please use the ExpandedNodeId syntax instead.
    {
        "EndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
        "NodeId": {
            "Identifier": "ns=0;i=2258"
        }
    }
    // please consult the OPC UA specification for details on how OPC monitored node sampling interval and OPC subscription publishing interval settings are handled by the OPC UA stack.
    // the publishing interval of the data to Azure IoT Hub is controlled by the command line settings (or the default: publish data to IoT Hub at least each 1 second).
]
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

### <a name="command-line-options"></a>Komut satırı seçenekleri

Uygulamanın tam kullanımını görmek için `--help` komut satırı seçeneği. Aşağıdaki örnek, bir komut yapısını gösterir:

```cmd/sh
OpcPublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]
```

`applicationname`kullanılacak OPC UA uygulama adıdır. Bu parametre gereklidir. Uygulama adı, yayımcı IOT Hub cihaz kayıt defterinde kaydetmek için de kullanılır.

`IoT Hubconnectionstring`IOT hub'ı sahibi bağlantı dizesidir. Bu parametre isteğe bağlıdır.

Aşağıdaki seçenekleri desteklenir:

```cmd/sh
--pf, --publishfile=VALUE
  the filename to configure the nodes to publish.
    Default: './publishednodes.json'
--sd, --shopfloordomain=VALUE
  the domain of the shopfloor. if specified this
    domain is appended (delimited by a ':' to the '
    ApplicationURI' property when telemetry is
    sent to IoT Hub.
    The value must follow the syntactical rules of a
    DNS hostname.
    Default: not set
--sw, --sessionconnectwait=VALUE
  specify the wait time in seconds publisher is
    trying to connect to disconnected endpoints and
    starts monitoring unmonitored items
    Min: 10
    Default: 10
--vc, --verboseconsole=VALUE
  the output of publisher is shown on the console.
    Default: False
--ih, --IoT Hubprotocol=VALUE
  the protocol to use for communication with Azure
    IoT Hub (allowed values: Amqp, Http1, Amqp_
    WebSocket_Only, Amqp_Tcp_Only, Mqtt, Mqtt_
    WebSocket_Only, Mqtt_Tcp_Only).
    Default: Mqtt
--ms, --IoT Hubmessagesize=VALUE
  the max size of a message which can be send to
    IoT Hub. when telemetry of this size is available
    it will be sent.
    0 will enforce immediate send when telemetry is
    available
    Min: 0
    Max: 256 * 1024
    Default: 4096
--si, --IoT Hubsendinterval=VALUE
  the interval in seconds when telemetry should be
    send to IoT Hub. If 0, then only the
    IoT Hubmessagesize parameter controls when
    telemetry is sent.
    Default: '1'
--lf, --logfile=VALUE
  the filename of the logfile to use.
    Default: './Logs/<applicationname>.log.txt'
--pn, --portnum=VALUE
  the server port of the publisher OPC server
    endpoint.
    Default: 62222
--pa, --path=VALUE
  the endpoint URL path part of the publisher OPC
    server endpoint.
    Default: '/UA/Publisher'
--lr, --ldsreginterval=VALUE
  the LDS(-ME) registration interval in ms. If 0,
    then the registration is disabled.
    Default: 0
--ot, --operationtimeout=VALUE
  the operation timeout of the publisher OPC UA
    client in ms.
    Default: 120000
--oi, --opcsamplinginterval=VALUE
  the publisher is using this as default value in
    milliseconds to request the servers to sample
    the nodes with this interval
    this value might be revised by the OPC UA
    servers to a supported sampling interval.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a negative value will set the sampling interval
    to the publishing interval of the subscription
    this node is on.
    0 will configure the OPC UA server to sample in
    the highest possible resolution and should be
    taken with care.
    Default: 1000
--op, --opcpublishinginterval=VALUE
  the publisher is using this as default value in
    milliseconds for the publishing interval setting
    of the subscriptions established to the OPC UA
    servers.
    please check the OPC UA specification for
    details how this is handled by the OPC UA stack.
    a value less than or equal zero will let the
    server revise the publishing interval.
    Default: 0
--ct, --createsessiontimeout=VALUE
  specify the timeout in seconds used when creating
    a session to an endpoint. On unsuccessful
    connection attemps a backoff up to 5 times the
    specified timeout value is used.
    Min: 1
    Default: 10
--ki, --keepaliveinterval=VALUE
  specify the interval in seconds the publisher is
    sending keep alive messages to the OPC servers
    on the endpoints it is connected to.
    Min: 2
    Default: 2
--kt, --keepalivethreshold=VALUE
  specify the number of keep alive packets a server
    can miss, before the session is disconneced
    Min: 1
    Default: 5
--st, --opcstacktracemask=VALUE
  the trace mask for the OPC stack. See github OPC .
    NET stack for definitions.
    To enable IoT Hub telemetry tracing set it to 711.
    Default: 285  (645)
--as, --autotrustservercerts=VALUE
  the publisher trusts all servers it is
    establishing a connection to.
    Default: False
--tm, --trustmyself=VALUE
  the publisher certificate is put into the trusted
    certificate store automatically.
    Default: True
--fd, --fetchdisplayname=VALUE
  enable to read the display name of a published
    node from the server. this will increase the
    runtime.
    Default: False
--at, --appcertstoretype=VALUE
  the own application cert store type.
    (allowed values: Directory, X509Store)
    Default: 'X509Store'
--ap, --appcertstorepath=VALUE
  the path where the own application cert should be
    stored
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/own'
--tt, --trustedcertstoretype=VALUE
  the trusted cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--tp, --trustedcertstorepath=VALUE
  the path of the trusted cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/trusted'
--rt, --rejectedcertstoretype=VALUE
  the rejected cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--rp, --rejectedcertstorepath=VALUE
  the path of the rejected cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/rejected'
--it, --issuercertstoretype=VALUE
  the trusted issuer cert store type.
    (allowed values: Directory, X509Store)
    Default: Directory
--ip, --issuercertstorepath=VALUE
  the path of the trusted issuer cert store
    Default (depends on store type):
    X509Store: 'CurrentUser\UA_MachineDefault'
    Directory: 'CertificateStores/issuers'
--dt, --devicecertstoretype=VALUE
  the IoT Hub device cert store type.
    (allowed values: Directory, X509Store)
    Default: X509Store
--dp, --devicecertstorepath=VALUE
  the path of the iot device cert store
    Default Default (depends on store type):
    X509Store: 'My'
    Directory: 'CertificateStores/IoT Hub'
-h, --help
  show this message and exit
```

Uygulama denetimi için aşağıdaki ortam değişkenleri kullanabilirsiniz:
- `_HUB_CS`: IOT hub'ı sahibi bağlantı dizesini ayarlar
- `_GW_LOGP`: kullanılacak günlük dosyasının dosya adını ayarlar
- `_TPC_SP`: sertifikaları Güvenilen istasyonları depolamak için yolunu ayarlar
- `_GW_PNFP`: yayımlama yapılandırma dosyasının dosya adını ayarlar

Komut satırı bağımsız değişkenleri ortam değişkeni ayarları geçersiz kılın.

Genellikle, uygulamanın yalnızca ilk başlangıcı IOT hub'ı sahibi bağlantı dizesini belirtin. Bağlantı dizesi şifrelenir ve platformun sertifika deposunda depolanır.

Sonraki çağrılar üzerinde bağlantı dizesi platformun sertifika depolama alanından okuyun ve yeniden. Her başlangıç bağlantı dizesini belirtirseniz, cihaz IOT Hub cihaz kayıt defterinde kaldırıldı ve her zaman yeniden.

### <a name="native-on-windows"></a>Windows yerel

Uygulamayı Windows üzerinde yerel olarak çalıştırmak için açık `OpcPublisher.sln` proje ile Visual Studio 2017, çözümü derlemek ve yayımlayın. Uygulamayı başlatabileceğini **hedef dizin** ile yayımladığınız:

```cmd
dotnet OpcPublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-self-built-container"></a>Kendinden oluşturulmuş bir kapsayıcıyı kullan

Kendinden oluşturulmuş bir kapsayıcıda uygulamayı çalıştırmak için oluşturun ve kendi kapsayıcı başlatın:

```cmd/sh
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-a-container-from-hubdockercom"></a>Bir kapsayıcı hub.docker.com kullanın

DockerHub üzerinde önceden oluşturulmuş bir kapsayıcı yok. Başlatmak için aşağıdaki komutu çalıştırın:

```cmd/sh
docker run microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="important-when-using-a-container"></a>Bir kapsayıcı kullanırken önemli

#### <a name="access-to-the-publisher-opc-ua-server"></a>Yayımcı OPC UA sunucuya erişimi

Yayımcı OPC UA sunucunun varsayılan 62222 bağlantı noktasını dinler. Bu bağlantı noktasının bir kapsayıcıda kullanıma sunmak için kullanmanız gerekir `docker run` seçeneği `-p`:

```cmd/sh
docker run -p 62222:62222 microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="enable-intercontainer-name-resolution"></a>İntercontainer ad çözümlemesini etkinleştirme

Name resolution kapsayıcı içindeki diğer kapsayıcılara etkinleştirmek için şunları yapmalısınız:

- Bir kullanıcı tarafından tanımlanan docker köprüsü ağ oluşturun.
- Kapsayıcı ağ bağlamak için `--network`seçeneği.
- Kapsayıcı adı kullanarak Ata `--name` seçeneği.

Aşağıdaki örnek, bu yapılandırma seçenekleri gösterir:

```cmd/sh
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Kapsayıcı şimdi göre diğer kapsayıcı adı kullanarak ağ üzerinden ulaşılabilen `publisher`.

#### <a name="assign-a-hostname"></a>Bir ana bilgisayar adı atayın

Yayımcı sertifikası ve uç nokta oluşturma için çalıştığı makinenin ana bilgisayar adını kullanır. Docker seçer rastgele bir ana bilgisayar adı ile ayarlamadıysanız `-h` seçeneği. Burada kapsayıcıya iç konak adı ayarlamak için bir örnek `publisher`:

```cmd/sh
docker run -h publisher microsoft/iot-edge-opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

#### <a name="using-bind-mounts-shared-filesystem"></a>BIND kullanarak bağlar (paylaşılan dosya sistemi)

Bazı senaryolarda, yapılandırma bilgilerini okumak veya kapsayıcı dosya sistemini kullanmak yerine ana bilgisayardaki konumlara, günlük dosyalarını yazmak istediğiniz. Bu davranışı yapılandırmak için kullanın `-v` seçeneği `docker run` bağlama bağlama modunda. Örneğin:

```cmd/sh
-v //D/docker:/build/out/Logs
-v //D/docker:/build/out/CertificateStores
-v //D/docker:/shared
-v //D/docker:/root/.dotnet/corefx/cryptography/x509stores
```

#### <a name="store-for-x509-certificates"></a>Mağaza X509 için sertifikalar

X509 depolama sertifikaları çalışmıyor bağlama başlatmalar ile deposunun yolunu izinleri olması gerektiğinden `rw` sahip. Bunun yerine kullanmanıza gerek `-v` seçeneği `docker run` birim modunda.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama

### <a name="native-on-windows"></a>Windows yerel

Açık `OpcPublisher.sln` proje ile Visual Studio 2017 ve F5 tuşuna göre uygulama hata ayıklamayı Başlat.

### <a name="in-a-docker-container"></a>Bir docker kapsayıcısı

Visual Studio 2017 hata ayıklama uygulamaları bir Docker kapsayıcısı kullanarak destekler `docker-compose`. Ancak, bu yöntem, komut satırı parametreleri geçmesine izin vermiyor.

Hata ayıklama VS2017 destekleyen seçeneği üzerinden hata ayıklamak için alternatiftir `ssh`. Docker yapı yapılandırma dosyası kullanabilirsiniz `Dockerfile.ssh` etkin SSH kapsayıcısı oluşturmak için depo kökündeki:

```cmd/sh
docker build -f .\Dockerfile.ssh -t publisherssh .
```

Yayımcı hata ayıklamak için kapsayıcı şimdi başlatabilirsiniz:

```cmd/sh
docker run -it publisherssh
```

Kapsayıcıda başlaması gerekir **ssh** arka plan programı el ile:

```cmd/sh
service ssh start
```

Bu noktada, oluşturabileceğiniz bir **ssh** kullanıcı olarak oturum `root` parolayla `Passw0rd`.

Kapsayıcı uygulamada hata ayıklamak hazırlamak için aşağıdaki ek adımları tamamlayın:

1. Ana bilgisayar tarafında oluşturma bir `launch.json` dosyası:

    ```json
    {
      "version": "0.2.0",
      "adapter": "<path>\\plink.exe",
      "adapterArgs": "root@localhost -pw Passw0rd -batch -T ~/vsdbg/vsdbg --interpreter=vscode",
      "languageMappings": {
        "C#": {
          "languageId": "3F5162F8-07C6-11D3-9053-00C04FA302A1",
          "extensions": [ "*" ]
        }
      },
      "exceptionCategoryMappings": {
        "CLR": "449EC4CC-30D2-4032-9256-EE18EB41B62B",
        "MDA": "6ECE07A9-0EDE-45C4-8296-818D8FC401D4"
      },
      "configurations": [
        {
          "name": ".NET Core Launch",
          "type": "coreclr",
          "cwd": "~/publisher",
          "program": "Opc.Ua.Publisher.dll",
          "args": "<put-the-publisher-command-line-options-here>",

          "request": "launch"
        }
      ]
    }
    ```

1. Projenizi derleme ve tercih ettiğiniz bir dizine yayımlayın.

1. Gibi bir araç kullanın `WinSCP` dizine yayımlanan dosyaları kopyalamak için `/root/publisher` kapsayıcısında. Farklı bir dizin kullanmayı seçerseniz, güncelleştirme `cdw` özelliğinde `launch.json` dosya.

Şimdi Visual Studio'nun komut penceresinde aşağıdaki komutu kullanarak hata ayıklama başlatabilirsiniz:

```cmd
DebugAdapterHost.Launch /LaunchJson:"<path-to-the-launch.json-file-you-saved>"
```

## <a name="next-steps"></a>Sonraki adımlar

A önerilen sonraki adımdır öğrenmek için nasıl [bağlı Fabrika önceden yapılandırılmış çözüm için Windows veya Linux üzerinde bir ağ geçidi dağıtma](iot-suite-connected-factory-gateway-deployment.md).