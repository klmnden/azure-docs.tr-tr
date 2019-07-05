---
title: OPC Publisher - Azure çalıştırma | Microsoft Docs
description: OPC Yayımcısını Çalıştırma
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 0af4fcb6ea87069f3dea73f33828abd4f4bb06f4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450768"
---
# <a name="run-opc-publisher"></a>OPC Yayımcısını Çalıştırma

Bu makalede ad hata ayıklama OPC yayımcı çalıştırmayı öğrenin. Ayrıca, performans ve bellek konuları yöneliktir.

## <a name="command-line-options"></a>Komut satırı seçenekleri

Uygulama kullanımı gösterilmektedir kullanarak `--help` aşağıdaki gibi komut satırı seçeneği:

```sh/cmd
Current directory is: /appdata
Log file is: <hostname>-publisher.log
Log level is: info

OPC Publisher V2.3.0
Informational version: V2.3.0+Branch.develop_hans_methodlog.Sha.0985e54f01a0b0d7f143b1248936022ea5d749f9

Usage: opcpublisher.exe <applicationname> [<IoT Hubconnectionstring>] [<options>]

OPC Edge Publisher to subscribe to configured OPC UA servers and send telemetry to Azure IoT Hub.
To exit the application, just press CTRL-C while it is running.

applicationname: the OPC UA application name to use, required
                  The application name is also used to register the publisher under this name in the
                  IoT Hub device registry.

IoT Hubconnectionstring: the IoT Hub owner connectionstring, optional

There are a couple of environment variables which can be used to control the application:
_HUB_CS: sets the IoT Hub owner connectionstring
_GW_LOGP: sets the filename of the log file to use
_TPC_SP: sets the path to store certificates of trusted stations
_GW_PNFP: sets the filename of the publishing configuration file

Command line arguments overrule environment variable settings.

Options:
      --pf, --publishfile=VALUE
                              the filename to configure the nodes to publish.
                                Default: '/appdata/publishednodes.json'
      --tc, --telemetryconfigfile=VALUE
                              the filename to configure the ingested telemetry
                                Default: ''
  -s, --site=VALUE           the site OPC Publisher is working in. if specified
                                this domain is appended (delimited by a ':' to
                                the 'ApplicationURI' property when telemetry is
                                sent to IoT Hub.
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --ic, --iotcentral     publisher will send OPC UA data in IoTCentral
                                compatible format (DisplayName of a node is used
                                as key, this key is the Field name in IoTCentral)
                                . you need to ensure that all DisplayName's are
                                unique. (Auto enables fetch display name)
                                Default: False
      --sw, --sessionconnectwait=VALUE
                              specify the wait time in seconds publisher is
                                trying to connect to disconnected endpoints and
                                starts monitoring unmonitored items
                                Min: 10
                                Default: 10
      --mq, --monitoreditemqueuecapacity=VALUE
                              specify how many notifications of monitored items
                                can be stored in the internal queue, if the data
                                can not be sent quick enough to IoT Hub
                                Min: 1024
                                Default: 8192
      --di, --diagnosticsinterval=VALUE
                              shows publisher diagnostic info at the specified
                                interval in seconds (need log level info).
                                -1 disables remote diagnostic log and diagnostic
                                output
                                0 disables diagnostic output
                                Default: 0
      --ns, --noshutdown=VALUE
                              same as runforever.
                                Default: False
      --rf, --runforever     publisher can not be stopped by pressing a key on
                                the console, but will run forever.
                                Default: False
      --lf, --logfile=VALUE  the filename of the logfile to use.
                                Default: './<hostname>-publisher.log'
      --lt, --logflushtimespan=VALUE
                              the timespan in seconds when the logfile should be
                                flushed.
                                Default: 00:00:30 sec
      --ll, --loglevel=VALUE the loglevel to use (allowed: fatal, error, warn,
                                info, debug, verbose).
                                Default: info
        --ih, --IoT Hubprotocol=VALUE
                              the protocol to use for communication with IoT Hub (
                                allowed values: Amqp, Http1, Amqp_WebSocket_Only,
                                  Amqp_Tcp_Only, Mqtt, Mqtt_WebSocket_Only, Mqtt_
                                Tcp_Only) or IoT EdgeHub (allowed values: Mqtt_
                                Tcp_Only, Amqp_Tcp_Only).
                                Default for IoT Hub: Mqtt_WebSocket_Only
                                Default for IoT EdgeHub: Amqp_Tcp_Only
      --ms, --IoT Hubmessagesize=VALUE
                              the max size of a message which can be send to
                                IoT Hub. when telemetry of this size is available
                                it will be sent.
                                0 will enforce immediate send when telemetry is
                                available
                                Min: 0
                                Max: 262144
                                Default: 262144
      --si, --IoT Hubsendinterval=VALUE
                              the interval in seconds when telemetry should be
                                send to IoT Hub. If 0, then only the
                                IoT Hubmessagesize parameter controls when
                                telemetry is sent.
                                Default: '10'
      --dc, --deviceconnectionstring=VALUE
                              if publisher is not able to register itself with
                                IoT Hub, you can create a device with name <
                                applicationname> manually and pass in the
                                connectionstring of this device.
                                Default: none
  -c, --connectionstring=VALUE
                              the IoT Hub owner connectionstring.
                                Default: none
      --hb, --heartbeatinterval=VALUE
                              the publisher is using this as default value in
                                seconds for the heartbeat interval setting of
                                nodes without
                                a heartbeat interval setting.
                                Default: 0
      --sf, --skipfirstevent=VALUE
                              the publisher is using this as default value for
                                the skip first event setting of nodes without
                                a skip first event setting.
                                Default: False
      --pn, --portnum=VALUE  the server port of the publisher OPC server
                                endpoint.
                                Default: 62222
      --pa, --path=VALUE     the enpoint URL path part of the publisher OPC
                                server endpoint.
                                Default: '/UA/Publisher'
      --lr, --ldsreginterval=VALUE
                              the LDS(-ME) registration interval in ms. If 0,
                                then the registration is disabled.
                                Default: 0
      --ol, --opcmaxstringlen=VALUE
                              the max length of a string opc can transmit/
                                receive.
                                Default: 131072
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
      --aa, --autoaccept     the publisher trusts all servers it is
                                establishing a connection to.
                                Default: False
      --tm, --trustmyself=VALUE
                              same as trustowncert.
                                Default: False
      --to, --trustowncert   the publisher certificate is put into the trusted
                                certificate store automatically.
                                Default: False
      --fd, --fetchdisplayname=VALUE
                              same as fetchname.
                                Default: False
      --fn, --fetchname      enable to read the display name of a published
                                node from the server. this will increase the
                                runtime.
                                Default: False
      --ss, --suppressedopcstatuscodes=VALUE
                              specifies the OPC UA status codes for which no
                                events should be generated.
                                Default: BadNoCommunication,
                                BadWaitingForInitialData
      --at, --appcertstoretype=VALUE
                              the own application cert store type.
                                (allowed values: Directory, X509Store)
                                Default: 'Directory'
      --ap, --appcertstorepath=VALUE
                              the path where the own application cert should be
                                stored
                                Default (depends on store type):
                                X509Store: 'CurrentUser\UA_MachineDefault'
                                Directory: 'pki/own'
      --tp, --trustedcertstorepath=VALUE
                              the path of the trusted cert store
                                Default: 'pki/trusted'
      --rp, --rejectedcertstorepath=VALUE
                              the path of the rejected cert store
                                Default 'pki/rejected'
      --ip, --issuercertstorepath=VALUE
                              the path of the trusted issuer cert store
                                Default 'pki/issuer'
      --csr                  show data to create a certificate signing request
                                Default 'False'
      --ab, --applicationcertbase64=VALUE
                              update/set this applications certificate with the
                                certificate passed in as bas64 string
      --af, --applicationcertfile=VALUE
                              update/set this applications certificate with the
                                certificate file specified
      --pb, --privatekeybase64=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as base64 string
      --pk, --privatekeyfile=VALUE
                              initial provisioning of the application
                                certificate (with a PEM or PFX fomat) requires a
                                private key passed in as file
      --cp, --certpassword=VALUE
                              the optional password for the PEM or PFX or the
                                installed application certificate
      --tb, --addtrustedcertbase64=VALUE
                              adds the certificate to the applications trusted
                                cert store passed in as base64 string (multiple
                                strings supported)
      --tf, --addtrustedcertfile=VALUE
                              adds the certificate file(s) to the applications
                                trusted cert store passed in as base64 string (
                                multiple filenames supported)
      --ib, --addissuercertbase64=VALUE
                              adds the specified issuer certificate to the
                                applications trusted issuer cert store passed in
                                as base64 string (multiple strings supported)
      --if, --addissuercertfile=VALUE
                              adds the specified issuer certificate file(s) to
                                the applications trusted issuer cert store (
                                multiple filenames supported)
      --rb, --updatecrlbase64=VALUE
                              update the CRL passed in as base64 string to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --uc, --updatecrlfile=VALUE
                              update the CRL passed in as file to the
                                corresponding cert store (trusted or trusted
                                issuer)
      --rc, --removecert=VALUE
                              remove cert(s) with the given thumbprint(s) (
                                multiple thumbprints supported)
      --dt, --devicecertstoretype=VALUE
                              the IoT Hub device cert store type.
                                (allowed values: Directory, X509Store)
                                Default: X509Store
      --dp, --devicecertstorepath=VALUE
                              the path of the iot device cert store
                                Default Default (depends on store type):
                                X509Store: 'My'
                                Directory: 'CertificateStores/IoT Hub'
  -i, --install              register OPC Publisher with IoT Hub and then exits.
                                Default:  False
  -h, --help                 show this message and exit
      --st, --opcstacktracemask=VALUE
                              ignored, only supported for backward comaptibility.
      --sd, --shopfloordomain=VALUE
                              same as site option, only there for backward
                                compatibility
                                The value must follow the syntactical rules of a
                                DNS hostname.
                                Default: not set
      --vc, --verboseconsole=VALUE
                              ignored, only supported for backward comaptibility.
      --as, --autotrustservercerts=VALUE
                              same as autoaccept, only supported for backward
                                cmpatibility.
                                Default: False
      --tt, --trustedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted cert store will always reside in a
                                directory.
      --rt, --rejectedcertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the rejected cert store will always reside in a
                                directory.
      --it, --issuercertstoretype=VALUE
                              ignored, only supported for backward compatibility.
                                the trusted issuer cert store will always
                                reside in a directory.
```

Genellikle yalnızca uygulamasının ilk çalıştırılmasında IOT hub'ı sahibi bağlantı dizesini belirtin. Bağlantı dizesini şifrelenir ve platform sertifika deposunda depolanır. Sonraki çalışır, uygulama bağlantı dizesi sertifika deposundan okur. Her çalıştırıldığında, bağlantı dizesini belirtirseniz, oluşturulan bir cihaz IOT Hub cihaz kayıt defterinde uygulama için kaldırılmış ve yeniden.

## <a name="run-natively-on-windows"></a>Windows üzerinde yerel olarak çalıştırma

Açık **opcpublisher.sln** proje ile Visual Studio çözümü oluşturun ve yayımlayın. Uygulamayı başlatabilirsiniz **hedef dizin** gibi yayımlanan:

```cmd
dotnet opcpublisher.dll <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-self-built-container"></a>Kendi kapsayıcınızı kullanmak

Kendi kapsayıcınızı oluşturun ve şu şekilde başlatın:

```sh/cmd
docker run <your-container-name> <applicationname> [<IoT Hubconnectionstring>] [options]
```

## <a name="use-a-container-from-microsoft-container-registry"></a>Microsoft kapsayıcı kayıt defterinden bir kapsayıcı kullanın

Microsoft kapsayıcı kayıt defterini önceden oluşturulmuş kapsayıcı yok. Şu şekilde başlatılsın:

```sh/cmd
docker run mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Denetleme [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher) desteklenen işletim sistemleri ve işlemci mimarileri görmek için. İşletim sistemi ve CPU Mimarinizi destekleniyorsa, Docker, doğru kapsayıcının otomatik olarak seçer.

## <a name="run-as-an-azure-iot-edge-module"></a>Azure IOT Edge modülü çalıştırma

OPC yayımcı olarak kullanılmaya hazır bir [Azure IOT Edge](https://docs.microsoft.com/azure/iot-edge) modülü. OPC Publisher, IOT Edge modülü kullandığınızda, yalnızca aktarım protokolleri olan desteklenen **Amqp_Tcp_Only** ve **Mqtt_Tcp_Only**.

OPC Publisher, IOT Edge dağıtımı için modül olarak eklemek için Azure portalında, IOT hub'ı ayarlarına gidin ve aşağıdaki adımları tamamlayın:

1. Git **IOT Edge** oluşturun ve IOT Edge Cihazınızı seçin.
1. **Modülleri Ayarlama**'yı seçin.
1. Seçin **ekleme** altında **dağıtım modülleri** ardından **IOT Edge Modülü**.
1. İçinde **adı** alanına **yayımcı**.
1. İçinde **görüntü URI'si** girin `mcr.microsoft.com/iotedge/opc-publisher:<tag>`
1. Kullanılabilen etiketleri bulabilirsiniz [Docker Hub](https://hub.docker.com/_/microsoft-iotedge-opc-publisher)
1. İçine aşağıdaki JSON Yapıştır **kapsayıcı oluşturma seçenekleri** alan:

    ```json
    {
        "Hostname": "publisher",
        "Cmd": [
            "--aa"
        ]
    }
    ```

    Bu yapılandırma adlı bir kapsayıcı başlatma için IOT Edge yapılandırır **yayımcı** OPC yayımcı görüntüyü kullanarak. Kapsayıcının sistemin ana bilgisayar adı kümesine **yayımcı**. OPC Publisher, aşağıdaki komut satırı bağımsız değişkeniyle çağrılır: `--aa`. Bu seçenekle, OPC yayımcı bağlandığı OPC UA sunucuları sertifikalara güvenir. OPC Publisher komut satırı seçeneklerini kullanabilirsiniz. Tek sınırlama boyutudur **kapsayıcı oluşturma seçenekleri** IOT Edge tarafından desteklenir.

1. Diğer ayarları değiştirmeden bırakın ve **Kaydet**'i seçin.
1. Geri dön çıktısını OPC yayımcı başka bir IOT Edge modülü ile yerel olarak işlemek istiyorsanız, **modülleri ayarlama** sayfası. Ardından **yolları belirtin** sekmesini tıklatın ve aşağıdaki JSON gibi görünen yeni bir yol ekleyin:

    ```json
    {
      "routes": {
        "processingModuleToIoT Hub": "FROM /messages/modules/processingModule/outputs/* INTO $upstream",
        "opcPublisherToProcessingModule": "FROM /messages/modules/publisher INTO BrokeredEndpoint(\"/modules/processingModule/inputs/input1\")"
      }
    }
    ```

1. Geri **modülleri ayarlama** sayfasında **sonraki**yapılandırmasının son sayfaya erişinceye kadar.
1. Seçin **Gönder** yapılandırmanızı IOT Edge göndermek için.
1. Sizin başlatıldığında IOT Edge edge cihazınıza ve docker kapsayıcısı **yayımcı** çalıştıran, günlük çıktısını OPC Publisher'ın denetleyebilirsiniz kullanılarak `docker logs -f publisher` veya günlük dosyası denetleniyor. Önceki örnekte, yukarıda günlük dosyası olduğu `d:\iiotegde\publisher-publisher.log`. Ayrıca [IOT-edge-opc-publisher-Tanılama Aracı](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics).

### <a name="make-the-configuration-files-accessible-on-the-host"></a>Yapılandırma dosyalarını konakta erişilebilir olun

IOT Edge modülü yapılandırma dosyaları konak dosya sistemine erişilebilir hale getirmek için aşağıdakileri kullanın **kapsayıcı oluşturma seçenekleri**. Aşağıdaki örnek bir dağıtımı için Linux kapsayıcıları Windows kullanarak verilmiştir:

```json
{
    "Hostname": "publisher",
    "Cmd": [
        "--pf=./pn.json",
        "--aa"
    ],
    "HostConfig": {
        "Binds": [
            "d:/iiotedge:/appdata"
        ]
    }
}
```

Bu seçeneklerle OPC yayımcı dosyasından yayımlamadan düğümleri okur `./pn.json` ve kapsayıcının çalışma dizini kümesine `/appdata` başlangıçta. Bu ayarlarla OPC Publisher dosyasını okur `/appdata/pn.json` yapılandırmasını almak için kapsayıcısından. Olmadan `--pf` seçeneği, varsayılan yapılandırma dosyası okuma için OPC yayımcı çalıştığında `./publishednodes.json`.

Varsayılan adı kullanarak günlük dosyası `publisher-publisher.log`, yazılan `/appdata` ve `CertificateStores` dizin de bu dizinde oluşturulur.

Bu dosyaları konak dosya sistemine kullanabilmek için bir bağlama bağlama birim kapsayıcı yapılandırması gerektirir. `d://iiotedge:/appdata` Bağlama dizini eşleyen `/appdata`, geçerli çalışma dizini ana dizine kapsayıcı başlangıçta olduğu `d://iiotedge`. Bu seçenek olmadan, kapsayıcı bir sonraki başlatılmasında hiçbir dosya verileri kalıcı hale getirilir.

Windows kapsayıcıları ve ardından söz dizimi çalıştırıyorsanız `Binds` parametresi farklıdır. Kapsayıcı başlangıçta çalışma dizinidir `c:\appdata`. Yapılandırma dosyası dizinine koymak için `d:\iiotedge`konakta aşağıdaki eşlemesini belirtmektir `HostConfig` bölümü:

```json
"HostConfig": {
    "Binds": [
        "d:/iiotedge:c:/appdata"
    ]
}
```

Sözdizimi, Linux üzerinde çalıştırıyorsanız Linux kapsayıcıları `Binds` parametre başka yeniden. Kapsayıcı başlangıçta çalışma dizinidir `/appdata`. Yapılandırma dosyası dizinine koymak için `/iiotedge` konakta aşağıdaki eşlemesini belirtmektir `HostConfig` bölümü:

```json
"HostConfig": {
    "Binds": [
        "/iiotedge:/appdata"
    ]
}
```

## <a name="considerations-when-using-a-container"></a>Bir kapsayıcı kullanmayla ilgili konular

Aşağıdaki bölümlerde, bir kapsayıcı kullandığınızda göz önünde bulundurmanız gereken bazı hususlar listelenmektedir:

### <a name="access-to-the-opc-publisher-opc-ua-server"></a>OPC Publisher ve OPC UA sunucusuna erişimi

Varsayılan olarak, OPC yayımcı OPC UA sunucusu 62222 bağlantı noktasında dinler. Bu bağlantı noktasının bir kapsayıcıda kullanıma sunmak için aşağıdaki komutu kullanın:

```sh/cmd
docker run -p 62222:62222 mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="enable-intercontainer-name-resolution"></a>İntercontainer ad çözümlemesini etkinleştirin

Ad çözümleme gelen kapsayıcı içindeki diğer kapsayıcılar'ı etkinleştirmek için bir kullanıcı oluşturun. docker köprü ağı tanımlamak ve bu ağ kullanarak kapsayıcıya bağlanma `--network` seçeneği. Ayrıca bir adı kullanarak kapsayıcı atayın `--name` seçeneğini etkinleştirmelidir:

```sh/cmd
docker network create -d bridge iot_edge
docker run --network iot_edge --name publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

Kapsayıcı adı kullanılarak ulaşılamıyor `publisher` aynı ağdaki diğer kapsayıcılara göre.

### <a name="access-other-systems-from-within-the-container"></a>Kapsayıcı içindeki diğer sistemlerden erişim

Diğer kapsayıcılar önceki bölümde açıklanan parametreler kullanılarak erişilebilir. İşletim sistemi Docker barındırılan DNS etkin ise, ardından DNS'ye bilinen tüm sistemler erişme çalışır.

NetBIOS ad çözümlemesini kullanmasına ağlarda, diğer sistemler ile kapsayıcınız başlatarak erişmesini `--add-host` seçeneği. Bu seçenek, etkili bir şekilde kapsayıcının konak dosyasına bir giriş ekler:

```cmd/sh
docker run --add-host mydevbox:192.168.178.23  mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="assign-a-hostname"></a>Bir ana bilgisayar adı atayın

OPC Publisher, ana bilgisayar için sertifika ve uç nokta oluşturma çalıştığı bilgisayarın adını kullanır. Docker, rastgele bir ana bilgisayar adı ayarlanmamışsa seçer `-h` seçeneği. Aşağıdaki örnekte, iç ana bilgisayar adını kapsayıcıya ayarlanacağı gösterilmektedir `publisher`:

```sh/cmd
docker run -h publisher mcr.microsoft.com/iotedge/opc-publisher <applicationname> [<IoT Hubconnectionstring>] [options]
```

### <a name="use-bind-mounts-shared-filesystem"></a>Bağlama takar (paylaşılan dosya sistemi) kullanın

Kapsayıcı dosya sistemini kullanmak yerine, ana bilgisayar yapılandırma bilgilerini depolamak ve günlük dosyaları, dosya sistemi seçebilirsiniz. Bu seçeneği yapılandırmak için kullanın `-v` seçeneği `docker run` bağlama bağlama modunda.

## <a name="opc-ua-x509-certificates"></a>OPC UA X.509 sertifikaları

OPC UA X.509 sertifikaları bunlar bağlantı oluşturduğunuzda, OPC UA istemci ve sunucu kimlik doğrulaması ve bunların arasındaki iletişimi şifrelemek için kullanır. OPC Publisher, tüm sertifikaları yönetmek için OPC UA yığını tarafından tutulan sertifika depolarını kullanır. Başlangıçta, OPC Publisher, kendisi için bir sertifika olup olmadığını denetler. Varsa sertifika deposunda sertifika ve bir olmayan bir komut satırında geçirilen, OPC yayımcı otomatik olarak imzalanan bir sertifika oluşturur. Daha fazla bilgi için **InitApplicationSecurityAsync** yönteminde `OpcApplicationConfigurationSecurity.cs`.

Otomatik olarak imzalanan sertifikalar, güvenilir bir CA tarafından açmadıysanız gibi herhangi bir güvenlik sağlaması gerekmez.

OPC Publisher, komut satırı seçenekleri sağlar:

- OPC yayımcı tarafından kullanılan geçerli uygulama sertifikası CSR bilgileri alın.
- Sertifika sağlama OPC yayımcı bir CA ile imzalanmış.
- Yeni bir anahtar çifti ve CA eşleşen sağlama OPC yayımcı sertifikası imzalanmış.
- Sertifikaları, güvenilen eş ya da güvenilen veren sertifika deposuna ekleyin.
- Bir CRL ekleyin.
- Sertifika, güvenilen bir eş veya sertifika deposunu güvenilen verenler kaldırın.

Tüm bu seçenekler dosyaları veya base64 ile kodlanmış dizeleri kullanan parametrelerinde geçirmenize olanak tanır.

Tüm sertifika depoları için varsayılan depolama türü, komut satırı seçeneklerini kullanarak değiştirebileceğiniz dosya sistemidir. Kapsayıcı dosya sisteminde kalıcı depolama sağlamadığından, farklı depolama türü seçmeniz gerekir. Docker'ı kullanma `-v` seçeneği sertifikayı kalıcı hale getirmek için konak dosya sistemine veya bir Docker birimde depolar. Bir Docker birimi kullanırsanız, base64 olarak kodlanmış dizeler kullanarak sertifikaları geçirebilirsiniz.

Sertifikaların nasıl kalıcı bir çalışma zamanı ortamı etkiler. Yeni sertifika depoları, uygulamayı her çalıştırdığınızda oluşturmamaya özen gösterin:

- Windows üzerinde yerel olarak çalışan, bir uygulama sertifika deposuna türündeki kullanamazsınız `Directory` özel anahtar erişimi başarısız olduğu. Bu durumda, bu seçeneği kullanın `--at X509Store`.
- Linux docker kapsayıcısı çalışan, sertifika depolarını konak dosya sistemine çalıştırdığınız docker ile eşleyebilirsiniz `-v <hostdirectory>:/appdata`. Bu seçenek sertifika uygulama çalıştırmaları arasında kalıcı hale getirir.
- Linux çalışan docker kapsayıcısını ve x X509 kullanmak istediğiniz depolamak için uygulamanın sertifika, çalıştırdığınız docker'ı kullanma `-v x509certstores:/root/.dotnet/corefx/cryptography/x509stores` ve uygulama seçeneği `--at X509Store`

## <a name="performance-and-memory-considerations"></a>Performans ve bellek konuları

Bu bölümde, bellek ve performans yönetimi seçenekleri ele alınmaktadır:

### <a name="command-line-parameters-to-control-performance-and-memory"></a>Denetim performans ve bellek için komut satırı parametreleri

OPC Publisher çalıştırdığınızda, konakta bellek kaynakları ve performans gereksinimlerinizi farkında olmanız gerekir.

Bellek ve performans birbirine bağlıdır ve her ikisi de yayımlamak için yapılandırma kaç düğümü yapılandırmasına bağlıdır. Aşağıdaki parametreleri gereksinimlerinizi karşıladığından emin olun:

- IOT hub'ı aralığı gönderir: `--si`
- IOT Hub ileti boyutu (varsayılan `1`): `--ms`
- İzlenen öğe kapasitesi kuyruğa al: `--mq`

`--mq` Parametre tüm OPC düğümü değer değişikliği bildirimlerini arabelleği iç kuyruk kapasitesinin üst sınırı denetler. OPC Publisher gönderemiyorsanız iletileri IOT hub'a bu kuyruğu arabellek bildirimleri yeterince hızlı. Parametre arabelleğe alınıp bildirimleri sayısını ayarlar. Test çalıştırmalarınızın artan bu kuyruğundaki öğelerin sayısı görürseniz, daha sonra iletileri loosing kaçınmak için şunları yapmalısınız:

- IOT hub'ı gönderme zaman aralığını azaltın
- IOT Hub ileti boyutunu artırma

`--si` Parametre, belirtilen zaman aralığı IOT Hub'ına ileti göndermek için OPC yayımcısını zorlar. OPC yayımcı tarafından belirtilen ileti boyutu hemen sonra bir ileti gönderir `--ms` parametresi ulaştınız veya aralığı olarak hemen sonra tarafından belirtilen `--si` parametre ulaşıldı. İleti boyutu seçeneği devre dışı bırakmak için `--ms 0`. Bu durumda, OPC yayımcı en büyük olası IOT Hub ileti boyutu 256 kB toplu verileri kullanır.

`--ms` IOT hub'a gönderilen iletileri toplu parametresi sağlar. Kullanmakta olduğunuz protokol yükü gönderme gerçek zaman karşılaştırıldığında IOT Hub'ına ileti gönderme ek yükünden yüksek olup olmadığını belirler. IOT Hub tarafından verilerin toplanma, senaryonuz için gecikme süresini izin veriyorsa, OPC Publisher'ı en büyük ileti boyutu 256 kB'ı kullanmak için yapılandırın.

OPC Publisher üretim senaryolarında kullanmadan önce performans ve bellek kullanımı üretim koşullar altında test edin. Kullanabileceğiniz `--di` parametresini OPC yayımcı tanılama bilgilerini yazar saniye cinsinden aralığı belirtin.

### <a name="test-measurements"></a>Test ölçümleri

Aşağıdaki örnek tanılama için farklı değerlerle ölçümleri Göster `--si` ve `--ms` 500 düğümleri ile bir OPC yayımlama aralığı 1 saniye yayımlama parametreleri.  Test bir OPC yayımcı hata ayıklama derlemesi, Windows 10'da yerel olarak 120 saniye için kullanılır. IOT Hub protokol varsayılan MQTT Protokolü idi.

#### <a name="default-configuration---si-10---ms-262144"></a>Varsayılan Yapılandırma (--10--ms 262144 sı)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:33:05 (started @ 26.10.2017 15:31:09)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54363
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54363
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:33:04
bytes sent to IoT Hub: 12709429
avg msg size: 116600
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 10
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Varsayılan yapılandırma, IOT hub'ına 10 saniyede veya 256 kB veri alımı IOT Hub için kullanılabilir olduğunda verileri gönderir. Bu yapılandırma, bir orta gecikme süresi yaklaşık 10 saniye ekler, ancak büyük ileti boyutu nedeniyle düşük olasılığını loosing veri yok. Kayıp OPC düğüm güncelleştirme yok tanılama çıktısını gösterir: `monitored item notifications enqueue failure: 0`.

#### <a name="constant-send-interval---si-1---ms-0"></a>Sabit gönderme aralığı (--si 1--ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:35:59 (started @ 26.10.2017 15:34:03)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54243
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54243
---------------------------------
messages sent to IoT Hub: 109
last successful msg sent @: 26.10.2017 15:35:59
bytes sent to IoT Hub: 12683836
avg msg size: 116365
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 1
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

İleti boyutu 0 olarak ayarlandığında ardından OPC yayımcısı dahili olarak kullanarak, 256 kB en büyük desteklenen IOT Hub ileti boyutu, verileri toplu olarak işler. Ortalama ileti boyutu 115,019 bayttır tanılama çıktıları gösterir. Bu yapılandırmada, OPC yayımcı herhangi bir OPC düğüm değer güncelleştirmeleri kaybetmek değil ve varsayılan kıyasla daha düşük gecikme süresi vardır.

### <a name="send-each-opc-node-value-update---si-0---ms-0"></a>Her bir OPC düğümü değer güncelleştirmesi Gönder (--sı 0--ms 0)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:39:33 (started @ 26.10.2017 15:37:37)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 8184
monitored item notifications enqueued: 54232
monitored item notifications enqueue failure: 44624
monitored item notifications dequeued: 1424
---------------------------------
messages sent to IoT Hub: 1423
last successful msg sent @: 26.10.2017 15:39:33
bytes sent to IoT Hub: 333046
avg msg size: 234
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 96
--si setting: 0
--ms setting: 0
--ih setting: Mqtt
==========================================================================
```

IOT Hub'ına ileti her OPC düğüm değeri değiştirmek için bu yapılandırmayı gönderir. Tanılamayı Göster ortalama ileti boyutu 234, küçük bayttır. Bu yapılandırmanın avantajı, OPC yayımcı herhangi bir gecikme süresi eklemez ' dir. Kayıp OPC düğümü değer güncelleştirmelerin sayısını (`monitored item notifications enqueue failure: 44624`) olan yüksek, bu yapılandırma yüksek hacimlerde telemetri yayımlanacak ile senaryoları için uygun yapın.

### <a name="maximum-batching---si-0---ms-262144"></a>En büyük toplu işleme (--0--ms 262144 sı)

```log
==========================================================================
OpcPublisher status @ 26.10.2017 15:42:55 (started @ 26.10.2017 15:41:00)
---------------------------------
OPC sessions: 1
connected OPC sessions: 1
connected OPC subscriptions: 5
OPC monitored items: 500
---------------------------------
monitored items queue bounded capacity: 8192
monitored items queue current items: 0
monitored item notifications enqueued: 54137
monitored item notifications enqueue failure: 0
monitored item notifications dequeued: 54137
---------------------------------
messages sent to IoT Hub: 48
last successful msg sent @: 26.10.2017 15:42:55
bytes sent to IoT Hub: 12565544
avg msg size: 261782
msg send failures: 0
messages too large to sent to IoT Hub: 0
times we missed send interval: 0
---------------------------------
current working set in MB: 90
--si setting: 0
--ms setting: 262144
--ih setting: Mqtt
==========================================================================
```

Bu yapılandırma çok OPC düğüm değeri güncelleştirmeleri mümkün olduğunca toplu olarak işler. En fazla IOT hub'ı burada yapılandırılan 256 kB büyüklüğünde ileti. Hiçbir gönderme aralığı olduğundan istenen IOT Hub'ı veri almak için veri miktarını anlamına gelir gecikme süresini belirler. Bu yapılandırma tüm OPC düğüm değerleri loosing en az olasılığı vardır ve çok sayıda düğümünüz yayımlamak için uygundur. Bu yapılandırmayı kullandığınızda, senaryonuz ileti boyutu 256 kB eriştiyseniz değil, yüksek oranda gecikme süreleri Burada sunulan koşulları yok emin olun.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama

Uygulamada hata ayıklamak için açın **opcpublisher.sln** çözüm dosyası ile Visual Studio ve Visual Studio hata ayıklama araçları'nı kullanın.

OPC Publisher OPC UA sunucusuna erişmeye ihtiyacınız varsa, güvenlik duvarınızı sunucunun dinlediği bağlantı noktası erişimi verdiğinden emin olun. Varsayılan bağlantı noktası şöyledir: 62222.

## <a name="control-the-application-remotely"></a>Uygulama uzaktan denetimi

Yayımlamak için düğümleri yapılandırma yapılabilir doğrudan yöntemler IOT hub'ı kullanarak.

OPC Publisher okumak için birkaç ek IOT hub'ı doğrudan yöntem çağrıları uygular:

- Genel bilgiler.
- Tanılama bilgileri OPC oturumları, abonelikleri ve izlenen öğe.
- IOT hub'ı iletileri ve olayları tanılama bilgileri.
- Başlatma günlüğü.
- Günlüğün son 100 satır.
- Uygulamayı kapatın.

Araçlar için aşağıdaki GitHub depoları içeren [yayımlamak için düğümleri yapılandırma](https://github.com/Azure-Samples/iot-edge-opc-publisher-nodeconfiguration) ve [tanılama bilgilerini okuma](https://github.com/Azure-Samples/iot-edge-opc-publisher-diagnostics). Her iki araç da hub'ı Docker kapsayıcıları olarak kullanılabilir.

## <a name="use-a-sample-opc-ua-server"></a>Örnek bir OPC UA sunucusu kullanma

Gerçek bir OPC UA sunucusu yoksa, kullanabileceğiniz [OPC UA PLC örnek](https://github.com/Azure-Samples/iot-edge-opc-plc) kullanmaya başlamak için. Bu örnek PLC Docker Hub üzerinde de kullanılabilir.

Rastgele verileri ve anomalileri etiketlerle oluşturmak etiketler, bir dizi uygular. Ek etiket değerlerini benzetimini yapmak ihtiyacınız varsa, örnek genişletebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

OPC Publisher çalıştırmayı öğrendiniz, önerilen sonraki adımlar hakkında bilgi edinmek için olan [OPC İkizi](overview-opc-twin.md) ve [OPC kasası](overview-opc-vault.md).
