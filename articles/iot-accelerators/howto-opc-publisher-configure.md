---
title: OPC Publisher - Azure yapılandırma | Microsoft Docs
description: OPC Publisher yapılandırma
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: bccab4dde5e17ec30a0b8c5e36dd78bdd1bdff93
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605715"
---
# <a name="configure-opc-publisher"></a>OPC Yayımcısını Yapılandırma

OPC Publisher'ı belirtmek için yapılandırabilirsiniz:

- Yayımlanacak OPC UA düğüm veri değişiklikleri.
- Yayımlanacak OPC UA olayları.
- Telemetri biçimi.

OPC yapılandırma dosyalarının veya yöntem çağrıları kullanarak yayımcı yapılandırabilirsiniz.

## <a name="use-configuration-files"></a>Yapılandırma dosyalarını kullanma

Bu bölümde, OPC UA düğüm yayımlama yapılandırma dosyalarıyla yapılandırma seçenekleri açıklanmaktadır.

### <a name="use-a-configuration-file-to-configure-publishing-data-changes"></a>Veri değişikliklerini yayımlama yapılandırmak için bir yapılandırma dosyası kullanın

Bir yapılandırma dosyası yayımlanacak OPC UA düğümlerini yapılandırmak için en kolay yoludur. Yapılandırma dosyasının biçimi belgelenen [publishednodes.json](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/publishednodes.json) depodaki.

Yapılandırma dosyası sözdizimi, zaman içinde değişti. OPC Publisher hala eski biçimleri okumaktadır, ancak yapılandırması devam olduğunda bunları en son biçime dönüştürür.

Aşağıdaki örnek, yapılandırma dosyasının biçimi gösterir:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62541/Quickstarts/ReferenceServer",
    "UseSecurity": true,
    "OpcNodes": [
      {
        "Id": "i=2258",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```

### <a name="use-a-configuration-file-to-configure-publishing-events"></a>Yayımlama olayları yapılandırmak için bir yapılandırma dosyası kullanın

OPC UA olayları yayımlamak için veri değişikliklerini olduğu gibi aynı yapılandırma dosyası kullanın.

Aşağıdaki örnek, yayımlama tarafından oluşturulan olayları için yapılandırma işlemi gösterilmektedir [SimpleEvents sunucu](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/SimpleEvents/Server). SimpleEvents sunucunun bulunabilir [OPC Fundation depo](https://github.com/OPCFoundation/UA-.NETStandard) olan:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62563/Quickstarts/SimpleEventsServer",
    "OpcEvents": [
      {
        "Id": "i=2253",
        "DisplayName": "SimpleEventServerEvents",
        "SelectClauses": [
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "EventId"
            ]
          },
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "Message"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CycleId"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CurrentStep"
            ]
          }
        ],
        "WhereClause": [
          {
            "Operator": "OfType",
            "Operands": [
              {
                "Literal": "ns=2;i=235"
              }
            ]
          }
        ]
      }
    ]
  }
]
```

## <a name="use-method-calls"></a>Yöntem çağrıları kullanın

Bu bölümde, OPC yayımcı yapılandırmak için kullanabileceğiniz yöntem çağrıları açıklar.

### <a name="configure-using-opc-ua-method-calls"></a>OPC UA yöntem çağrıları kullanarak yapılandırma

OPC Publisher 62222 bağlantı noktasında erişilebilen bir OPC UA sunucusu içerir. Ana bilgisayar ise **yayımcı**, uç nokta ise URI: `opc.tcp://publisher:62222/UA/Publisher`.

Bu uç nokta aşağıdaki dört yöntemi kullanıma sunar:

- PublishNode
- UnpublishNode
- GetPublishedNodes
- IOT HubDirectMethod

### <a name="configure-using-iot-hub-direct-method-calls"></a>IOT hub'ı doğrudan yöntem çağrıları kullanarak yapılandırma

OPC Publisher, şu IOT hub'ı doğrudan yöntem çağrıları uygular:

- PublishNodes
- UnpublishNodes
- UnpublishAllNodes
- GetConfiguredEndpoints
- GetConfiguredNodesOnEndpoint
- GetDiagnosticInfo
- GetDiagnosticLog
- GetDiagnosticStartupLog
- ExitApplication
- GetInfo

JSON yükü yöntemi istek ve yanıtların biçimi tanımlanmış [opcpublisher/HubMethodModel.cs](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/HubMethodModel.cs).

Modülde bilinmeyen bir yöntem çağırırsanız, yöntem uygulanmadı belirten bir dize ile yanıt verir. Modül ping işlemi yapmak için bir yol bilinmeyen bir yöntem çağırabilirsiniz.

### <a name="configure-username-and-password-for-authentication"></a>Kullanıcı adı ve parola kimlik doğrulaması için yapılandırma

Kimlik doğrulama modu, bir IOT hub'ı aracılığıyla ayarlanabilir doğrudan yöntemini çağırır. Yükü özelliği içermelidir **OpcAuthenticationMode** ve kullanıcı adı ve parola:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "UsernamePassword",
    "Username": "<Username>",
    "Password": "<Password>"
    ...
}
```

Parola IOT hub'ı iş yükü istemci tarafından şifrelenmiş ve yayımcının yapılandırmasında saklanır. Kimlik doğrulama geri anonim olarak değiştirmek için aşağıdaki yük ile yöntemi kullanın:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "Anonymous"
    ...
}
```

Varsa **OpcAuthenticationMode** yükünde özelliği ayarlanmamış, kimlik doğrulama ayarlarını yapılandırmada değişmeden kalır.

## <a name="configure-telemetry-publishing"></a>Telemetri yayımlamayı yapılandırma

OPC Publisher yayımlanan bir düğümünde bir bildirimi bir değeri aldığında, IOT Hub'ına gönderilen JSON biçimlendirilmiş bir ileti oluşturur.

Bir yapılandırma dosyası kullanarak bu JSON biçimli iletisinin içeriği yapılandırabilirsiniz. Herhangi bir yapılandırma dosyası belirtilmediyse `--tc` seçeneği, varsayılan yapılandırma ile uyumlu olan kullanıldığında [bağlı Fabrika Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory).

OPC Publisher toplu iletileri için yapılandırıldıysa, bunlar geçerli bir JSON dizisi olarak gönderildiniz.

Telemetri, aşağıdaki kaynaklardan elde edilir:

- Düğüm için OPC yayımcısını düğüm yapılandırması
- **MonitoredItem** OPC yayımcı için bir bildirim alındı OPC UA yığını nesnesi.
- Veri değeri değişiklik ayrıntıları sağlayan bu bildirim için geçirilen bağımsız değişken.

JSON biçimli iletiye put telemetri önemli özellikleri, bu nesnelerin bir seçimdir. Daha fazla özellik gerekiyorsa, OPC yayımcı kod tabanı değiştirmeniz gerekir.

Yapılandırma dosyasının söz dizimi aşağıdaki gibidir:

```json
// The configuration settings file consists of two objects:
// 1) The 'Defaults' object, which defines defaults for the telemetry configuration
// 2) An array 'EndpointSpecific' of endpoint specific configuration
// Both objects are optional and if they are not specified, then publisher uses
// its internal default configuration, which generates telemetry messages compatible
// with the Microsoft Connected factory Preconfigured Solution (https://github.com/Azure/azure-iot-connected-factory).

// A JSON telemetry message for Connected factory looks like:
//  {
//      "NodeId": "i=2058",
//      "ApplicationUri": "urn:myopcserver",
//      "DisplayName": "CurrentTime",
//      "Value": {
//          "Value": "10.11.2017 14:03:17",
//          "SourceTimestamp": "2017-11-10T14:03:17Z"
//      }
//  }

// The 'Defaults' object in the sample below, are similar to what publisher is
// using as its internal default telemetry configuration.
{
    "Defaults": {
        // The first two properties ('EndpointUrl' and 'NodeId' are configuring data
        // taken from the OpcPublisher node configuration.
        "EndpointUrl": {

            // The following three properties can be used to configure the 'EndpointUrl'
            // property in the JSON message send by publisher to IoT Hub.

            // Publish controls if the property should be part of the JSON message at all.
            "Publish": false,

            // Pattern is a regular expression, which is applied to the actual value of the
            // property (here 'EndpointUrl').
            // If this key is ommited (which is the default), then no regex matching is done
            // at all, which improves performance.
            // If the key is used you need to define groups in the regular expression.
            // Publisher applies the regular expression and then concatenates all groups
            // found and use the resulting string as the value in the JSON message to
            //sent to IoT Hub.
            // This example mimics the default behaviour and defines a group,
            // which matches the conplete value:
            "Pattern": "(.*)",
            // Here some more exaples for 'Pattern' values and the generated result:
            // "Pattern": "i=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of '2058'to be sent by publisher
            // "Pattern": "(i)=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of 'i2058' to be sent by publisher

            // Name allows you to use a shorter string as property name in the JSON message
            // sent by publisher. By default the property name is unchanged and will be
            // here 'EndpointUrl'.
            // The 'Name' property can only be set in the 'Defaults' object to ensure
            // all messages from publisher sent to IoT Hub have a similar layout.
            "Name": "EndpointUrl"

        },
        "NodeId": {
            "Publish": true,

            // If you set Defaults.NodeId.Name to "ni", then the "NodeId" key/value pair
            // (from the above example) will change to:
            //      "ni": "i=2058",
            "Name": "NodeId"
        },

        // The MonitoredItem object is configuring the data taken from the MonitoredItem
        // OPC UA object for published nodes.
        "MonitoredItem": {

            // If you set the Defaults.MonitoredItem.Flat to 'false', then a
            // 'MonitoredItem' object will appear, which contains 'ApplicationUri'
            // and 'DisplayNode' proerties:
            //      "NodeId": "i=2058",
            //      "MonitoredItem": {
            //          "ApplicationUri": "urn:myopcserver",
            //          "DisplayName": "CurrentTime",
            //      }
            // The 'Flat' property can only be used in the 'MonitoredItem' and
            // 'Value' objects of the 'Defaults' object and will be used
            // for all JSON messages sent by publisher.
            "Flat": true,

            "ApplicationUri": {
                "Publish": true,
                "Name": "ApplicationUri"
            },
            "DisplayName": {
                "Publish": true,
                "Name": "DisplayName"
            }
        },
        // The Value object is configuring the properties taken from the event object
        // the OPC UA stack provided in the value change notification event.
        "Value": {
            // If you set the Defaults.Value.Flat to 'true', then the 'Value'
            // object will disappear completely and the 'Value' and 'SourceTimestamp'
            // members won't be nested:
            //      "DisplayName": "CurrentTime",
            //      "Value": "10.11.2017 14:03:17",
            //      "SourceTimestamp": "2017-11-10T14:03:17Z"
            // The 'Flat' property can only be used for the 'MonitoredItem' and 'Value'
            // objects of the 'Defaults' object and will be used for all
            // messages sent by publisher.
            "Flat": false,

            "Value": {
                "Publish": true,
                "Name": "Value"
            },
            "SourceTimestamp": {
                "Publish": true,
                "Name": "SourceTimestamp"
            },
            // 'StatusCode' is the 32 bit OPC UA status code
            "StatusCode": {
                "Publish": false,
                "Name": "StatusCode"
                // 'Pattern' is ignored for the 'StatusCode' value
            },
            // 'Status' is the symbolic name of 'StatusCode'
            "Status": {
                "Publish": false,
                "Name": "Status"
            }
        }
    },

    // The next object allows to configure 'Publish' and 'Pattern' for specific
    // endpoint URLs. Those will overwrite the ones specified in the 'Defaults' object
    // or the defaults used by publisher.
    // It is not allowed to specify 'Name' and 'Flat' properties in this object.
    "EndpointSpecific": [
        // The following shows how a endpoint specific configuration can look like:
        {
            // 'ForEndpointUrl' allows to configure for which OPC UA server this
            // object applies and is a required property for all objects in the
            // 'EndpointSpecific' array.
            // The value of 'ForEndpointUrl' must be an 'EndpointUrl' configured in
            // the publishednodes.json confguration file.
            "ForEndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
            "EndpointUrl": {
                // We overwrite the default behaviour and publish the
                // endpoint URL in this case.
                "Publish": true,
                // We are only interested in the URL part following the 'opc.tcp://' prefix
                // and define a group matching this.
                "Pattern": "opc.tcp://(.*)"
            },
            "NodeId": {
                // We are not interested in the configured 'NodeId' value, 
                // so we do not publish it.
                "Publish": false
                // No 'Pattern' key is specified here, so the 'NodeId' value will be
                // taken as specified in the publishednodes configuration file.
            },
            "MonitoredItem": {
                "ApplicationUri": {
                    // We already publish the endpoint URL, so we do not want
                    // the ApplicationUri of the MonitoredItem to be published.
                    "Publish": false
                },
                "DisplayName": {
                    "Publish": true
                }
            },
            "Value": {
                "Value": {
                    // The value of the node is important for us, everything else we
                    // are not interested in to keep the data ingest as small as possible.
                    "Publish": true
                },
                "SourceTimestamp": {
                    "Publish": false
                },
                "StatusCode": {
                    "Publish": false
                },
                "Status": {
                    "Publish": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

OPC Publisher yapılandırma öğrendiniz. artık, önerilen sonraki adıma öğrenmektir nasıl [çalıştırma OPC yayımcı](howto-opc-publisher-run.md).
