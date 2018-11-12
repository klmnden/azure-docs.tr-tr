---
title: Azure IOT Edge modülü oluşturma | Microsoft Docs
description: Bir dağıtım bildirimi dağıtmak için hangi modülü nasıl bildirir, nasıl dağıtacağınızı ve bunlar arasında ileti yollarını nasıl oluşturulacağını öğrenin.
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 12c53b1fdad4ab8f55c000ca1cb4f08dab7c8a74
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230385"
---
# <a name="learn-how-to-deploy-modules-and-establish-routes-in-iot-edge"></a>IOT Edge'de yollar oluşturmak ve modülleri dağıtma hakkında bilgi edinin

Her IOT Edge cihazı en az iki modüllerini çalıştırır: IOT Edge çalışma zamanını oluşturan $edgeAgent ve $edgeHub,. Ayrıca, tüm IOT Edge cihazı herhangi bir sayıda işlemleri gerçekleştirmek için birden çok modül çalıştırabilirsiniz. Bu modülleri bir cihaza aynı anda dağıttığınızda, hangi modüllerin dahil edilir ve birbiriyle nasıl etkileşim kurduklarını bildirmek için bir yol gerekir. 

*Dağıtım bildirimi* açıklayan bir JSON belgesidir:

* Her modülü, kimlik bilgilerinin erişim özel kapsayıcı kayıt defterleri için kapsayıcı görüntüsü ve nasıl her modülü oluşturulabilir ve yönetilebilir yönergeler içerir Edge Aracısı'nın yapılandırması.
* Modüller arasında ve sonunda IOT hub'ına iletileri nasıl gerçekleştiğini içeren Edge hub'ı yapılandırma.
* İsteğe bağlı olarak, istenen özellikleri modül ikizlerini.

Tüm IOT Edge cihazları, bir dağıtım bildirimi ile yapılandırılmış olması gerekmez. Yeni yüklenen bir IOT Edge çalışma zamanı, geçerli bir bildirim ile yapılandırılana kadar bir hata kodu bildirir. 

Azure IOT Edge öğreticilerde, Azure IOT Edge Portalı'nda bir Sihirbazı aracılığıyla giderek bir dağıtım bildirimi oluşturun. Ayrıca, REST veya IOT Hub hizmeti SDK'sını kullanarak program aracılığıyla bir dağıtım bildirimi de uygulayabilirsiniz. Daha fazla bilgi için [IOT Edge dağıtımlarını anlama](module-deployment-monitoring.md).

## <a name="create-a-deployment-manifest"></a>Bir dağıtım bildirimi oluşturma

Yüksek düzeyde, dağıtım bildirimini bir modül ikizinin istenen özellikleri IOT Edge modülleri bir IOT Edge cihazında dağıtılan için yapılandırır. Bu modüllerin her zaman mevcut ikisidir: `$edgeAgent`, ve `$edgeHub`.

Yalnızca IOT Edge çalışma zamanı (aracısı ve hub) içeren bir dağıtım bildirimi geçerli değil.

Bildirim, bu yapı aşağıdaki gibidir:

```json
{
    "modulesContent": {
        "$edgeAgent": {
            "properties.desired": {
                // desired properties of the Edge agent
                // includes the image URIs of all modules
                // includes container registry credentials
            }
        },
        "$edgeHub": {
            "properties.desired": {
                // desired properties of the Edge hub
                // includes the routing information between modules, and to IoT Hub
            }
        },
        "{module1}": {  // optional
            "properties.desired": {
                // desired properties of module with id {module1}
            }
        },
        "{module2}": {  // optional
            ...
        },
        ...
    }
}
```

## <a name="configure-modules"></a>Modülleri Yapılandır

IOT Edge çalışma zamanı, dağıtımınızdaki modüllerini yüklemek nasıl söylemeniz gerekir. İçindeki tüm modüller için yapılandırma ve yönetim bilgi gider **$edgeAgent** istenen özellikleri. Bu bilgiler, Edge Aracısı kendisi için yapılandırma parametrelerini içerir. 

Veya dahil edilmesi gereken özellikleri tam bir listesi için bkz. [Edge aracısı ve Edge hub'ı özelliklerini](module-edgeagent-edgehub.md).

$EdgeAgent özellikleri şu yapıyı izler:

```json
"$edgeAgent": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
            "settings":{
                "registryCredentials":{ // give the edge agent access to container images that aren't public
                    }
                }
            }
        },
        "systemModules": {
            "edgeAgent": {
                // configuration and management details
            },
            "edgeHub": {
                // configuration and management details
            }
        },
        "modules": {
            "{module1}": { // optional
                // configuration and management details
            },
            "{module2}": { // optional
                // configuration and management details
            }
        }
    }
},
```

## <a name="declare-routes"></a>Yollar bildirme

Edge hub'ı bildirimli olarak modülleri ve IOT hub'ı arasında ve modüller arasında iletileri yönlendirmek için bir yol sağlar. İçindeki rota bilgilerini gelecek şekilde Edge hub'ı tüm iletişimi yönetir **$edgeHub** istenen özellikleri. Aynı dağıtım içinde birden çok yol olabilir.

İçinde tanımlanmış rotalar **$edgeHub** istenen özellikleri aşağıdaki sözdizimine sahip:

```json
"$edgeHub": {
    "properties.desired": {
        "routes": {
            "{route1}": "FROM <source> WHERE <condition> INTO <sink>",
            "{route2}": "FROM <source> WHERE <condition> INTO <sink>"
        },
    }
}
```

Her yol bir kaynak ve havuz gerekiyor ancak iletileri filtrelemek için kullanabileceğiniz isteğe bağlı bir durumdur. 


### <a name="source"></a>Kaynak
Kaynak iletileri nereden geldiğini belirtir. Aşağıdaki değerlerden biri olabilir:

| Kaynak | Açıklama |
| ------ | ----------- |
| `/*` | Herhangi bir cihaz veya modülü alınan tüm cihaz-bulut iletileri |
| `/messages/*` | Bir cihaz veya bazı veya hiç çıkış aracılığıyla bir modül tarafından gönderilen herhangi bir CİHAZDAN buluta ileti |
| `/messages/modules/*` | Bazı veya hiç çıkış aracılığıyla bir modül tarafından gönderilen herhangi bir CİHAZDAN buluta ileti |
| `/messages/modules/{moduleId}/*` | {Moduleıd} tarafından hiçbir çıktı ile gönderilen herhangi bir CİHAZDAN buluta ileti |
| `/messages/modules/{moduleId}/outputs/*` | Bazı çıkış {Moduleıd} tarafından gönderilen herhangi bir CİHAZDAN buluta ileti |
| `/messages/modules/{moduleId}/outputs/{output}` | {Moduleıd} kullanarak {çıkış} gönderilen herhangi bir CİHAZDAN buluta ileti |

### <a name="condition"></a>Koşul
Koşul, bir rota bildiriminde isteğe bağlıdır. Kaynak havuz tüm iletileri geçirmek istiyorsanız, yalnızca dışlamayı **burada** yan tümcesi tamamen. Ya da [IOT Hub sorgu dili](../iot-hub/iot-hub-devguide-routing-query-syntax.md) belirli iletileri veya koşulu karşılayan ileti türlerini filtrelemek için.

IOT edge'deki modüller arasında iletileri aynı şekilde Azure IOT Hub ve cihazlar arasında iletileri biçimlendirilir. Tüm iletileri JSON biçimindedir ve sahip **systemProperties**, **appProperties**, ve **gövdesi** parametreleri. 

Tüm üç parametre aşağıdaki söz dizimi ile geçici sorgular oluşturabilirsiniz: 

* Sistem Özellikleri: `$<propertyName>` veya `{$<propertyName>}`
* Uygulama özellikleri: `<propertyName>`
* Gövde özellikleri: `$body.<propertyName>` 

İleti özellikleri için sorguları oluşturma hakkında daha fazla örnek için bkz: [yollar sorgu ifadelerinde CİHAZDAN buluta ileti](../iot-hub/iot-hub-devguide-routing-query-syntax.md).

Bir ağ geçidi cihazı bir yaprak CİHAZDAN gelen iletiler için filtreleme yapmak istediğinizde, IOT Edge için belirli bir örnektir. Modüllerden gelen iletileri içeren bir sistem özelliği olarak adlandırılan **connectionModuleId**. Bu nedenle iletileri yönlendirmek için yaprak cihazlarıyla IOT Hub'ına doğrudan istiyorsanız, aşağıdaki yol modülü iletileri hariç tutmak için kullanın:

```sql
FROM /messages/\* WHERE NOT IS_DEFINED($connectionModuleId) INTO $upstream
```

### <a name="sink"></a>Havuz
Havuz iletilerinin nereye gönderildiğini tanımlar. Aşağıdaki değerlerden biri olabilir:

| Havuz | Açıklama |
| ---- | ----------- |
| `$upstream` | IOT Hub'ına ileti gönderin |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Giriş ileti gönder `{input}` Modülü `{moduleId}` |

IOT Edge, en az bir kez garantileri sağlar. Bir rota ileti havuzu için yerel olarak iletilmiyor durumunda Edge hub'ı iletileri depolar. Edge hub'ı, IOT hub'ı veya hedef modülü bağlanamazsa, örneğin, bağlı değil.

Edge hub'ı belirtilen süre kadar iletileri depolayan `storeAndForwardConfiguration.timeToLiveSecs` özelliği [Edge hub'ı istenen özelliklerini](module-edgeagent-edgehub.md).

## <a name="define-or-update-desired-properties"></a>İstenen özellikleri güncelleştirme veya tanımlama 

Dağıtım bildirimi, IOT Edge cihazına dağıtılan her modülün modül ikizi için istenen özellikleri belirtebilirsiniz. İstenen özellikleri dağıtım bildiriminde belirtildiğinde, tüm istenen özellikleri şu anda modül ikizi üzerine yazın.

Modül ikizinin istenen özellikleri dağıtım bildiriminde belirtmezseniz, IOT hub'ı modül ikizi hiçbir şekilde değiştirmez ve istenen özellikler program üzerinden ayarlamak mümkün olacaktır.

Cihaz ikizlerini değiştirmenize olanak tanıyan mekanizmalarının aynısını, modül ikizlerini değiştirmek için kullanılır. Daha fazla bilgi için [modül ikizi Geliştirici Kılavuzu](../iot-hub/iot-hub-devguide-module-twins.md).   

## <a name="deployment-manifest-example"></a>Dağıtım bildirimi örneği

Bu örnek bir dağıtım bildirimi JSON belgesi olarak.

```json
{
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "ContosoRegistry": {
                "username": "myacr",
                "password": "{password}",
                "address": "myacr.azurecr.io"
              }
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": ""
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
              "createOptions": ""
            }
          }
        },
        "modules": {
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
              "createOptions": "{}"
            }
          },
          "filtermodule": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "myacr.azurecr.io/filtermodule:latest",
              "createOptions": "{}"
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 10
        }
      }
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* $EdgeAgent ve $edgeHub eklenmelidir ya da özellikler tam bir listesi için bkz. [Edge aracısı ve Edge hub'ı özelliklerini](module-edgeagent-edgehub.md).

* IOT Edge modüllerinin nasıl kullanıldığı, öğrendiğinize göre [IOT Edge modülleri geliştirmek için Araçlar ve gereksinimleri anlamak](module-development.md).
