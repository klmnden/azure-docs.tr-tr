---
title: "Azure IOT kenar modül bileşimi | Microsoft Docs"
description: "Azure IOT kenar modüllerine unsurları ve bunların nasıl kullanılabilme öğrenin"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5de67b6f1ce79934a3a6aab623d2e77a56a8ce76
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="understand-how-iot-edge-modules-can-be-used-configured-and-reused---preview"></a>IOT kenar modülleri nasıl kullanılabileceğini, yapılandırılmış, anlamak ve yeniden - önizleme

Azure IOT kenar IOT sınır cihazları dağıtmadan önce birden çok IOT kenar modülleri oluşturma olanak tanır. Bu makalede dağıtmadan önce birden çok IOT kenar modülleri oluşturma geçici en önemli kavramlar açıklanmaktadır. 

## <a name="the-deployment-manifest"></a>Dağıtım bildirimi
*Dağıtım bildirimi* açıklayan bir JSON belgesi:

* IOT kenar modüllerine kendi oluşturulması ve yönetimi seçenekleri ile birlikte dağıtılması gerekir;
* İletileri modülleri arasında ve modülleri ve IOT hub'ı arasında nasıl gerçekleştiğini tanımlayan sınır hub yapılandırmasını;
* Tek tek modülü uygulamalarını yapılandırmak için modülü çiftlerini istenen özelliklerinde ayarlamak için isteğe bağlı olarak, değerleri.

Azure IOT kenar eğitimlerine Azure IOT kenar portalında Sihirbazı aracılığıyla giderek bir dağıtım bildirimi oluşturun. Program aracılığıyla REST veya IOT Hub hizmeti SDK'sını kullanarak dağıtım bildirimi de uygulayabilirsiniz. Başvurmak [dağıtma ve izleme] [ lnk-deploy] IOT kenar dağıtımları hakkında daha fazla bilgi için.

Yüksek bir düzeyde dağıtım bildirimi IOT kenar cihazda dağıtılan IOT kenar modülleri için bir modül twin'ın istenen özelliklerini yapılandırır. İki bu modüllerin her zaman mevcut: kenar Aracısı'nı ve kenar hub'ı.

Bildirim bu yapı aşağıdaki gibidir:

    {
        "moduleContent": {
            "$edgeAgent": {
                "properties.desired": {
                    // desired properties of the Edge agent
                }
            },
            "$edgeHub": {
                "properties.desired": {
                    // desired properties of the Edge hub
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

Bir dağıtım bildirimi örneği, bu bölümün sonunda bildirilir.

> [!IMPORTANT]
> Tüm IOT sınır cihazları dağıtım bildirimi ile yapılandırılması gerekir. Yeni yüklenen bir IOT kenar çalışma zamanı hata kodu ile geçerli bir bildirim yapılandırılana kadar bildirir. 

### <a name="specify-the-modules"></a>Modülleri belirtin
Edge Aracısı'nın modülü twin istenen özelliklerini içerir: İstenen modülleri, kenar aracısı için yapılandırma parametreleri birlikte, yapılandırma ve yönetim seçenekleri.

Yüksek düzeyde, bu bölümde bildiriminin IOT kenar çalışma zamanı modülleri (Kenar aracısı ve kenar hub) ve kullanıcı tarafından belirtilen modülleri için yönetim seçenekleri ve modül görüntüleri başvurular içeriyor.

Başvurmak [kenar aracının özelliklerini istenen] [ lnk-edgeagent-desired] bildiriminin bu bölümde ayrıntılı açıklaması için.

> [!NOTE]
> Yalnızca IOT kenar çalışma (aracı ve hub) içeren bir dağıtım bildirimi geçerlidir.


### <a name="specify-the-routes"></a>Rotaları belirtin
Edge hub bildirimli olarak modülleri arasında ve modülleri ve IOT hub'ı arasında iletileri yönlendirmek için bir yol sağlar.

Yollar aşağıdaki sözdizimi vardır:

        FROM <source>
        [WHERE <condition>]
        INTO <sink>

*Kaynak* aşağıdakilerin herhangi bir şey olabilir:

| Kaynak | Açıklama |
| ------ | ----------- |
| `/*` | Tüm cihaz bulut iletilerini herhangi bir aygıt veya modülü |
| `/messages/*` | Bir aygıt veya bazı veya hiçbir çıktı aracılığıyla bir modül tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/*` | Bazı veya hiçbir çıktı aracılığıyla bir modül tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/*` | Hiçbir çıktı {Moduleıd} tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/outputs/*` | Bazı çıkışıyla {Moduleıd} tarafından gönderilen herhangi bir cihaz bulut iletisi |
| `/messages/modules/{moduleId}/outputs/{output}` | {Moduleıd} kullanılarak {çıkış} gönderilen herhangi bir cihaz bulut iletisi |

Koşul tarafından desteklenen herhangi bir koşul olabilir [IOT hub'ı sorgu dili] [ lnk-iothub-query] IOT hub'ı yönlendirme kuralları için.

Havuz aşağıdakilerden biri olabilir:

| Havuz | Açıklama |
| ---- | ----------- |
| `$upstream` | IOT Hub'ına ileti gönderme |
| `BrokeredEndpoint("/modules/{moduleId}/inputs/{input}")` | Giriş için ileti gönderilirken `{input}` Modülü `{moduleId}` |

Edge hub en az bir kere garanti iletileri yerel olarak bir yol, havuz için ileti teslim edilemez, örneğin kenar hub IOT Hub'ına bağlanamıyor ya da hedef modülü bağlı değil durumunda depolanır, yani sağladığını dikkate almak önemlidir.

Edge hub belirtilen süre kadar iletileri depolayan `storeAndForwardConfiguration.timeToLiveSecs` özelliği kenar hub'ın istenen özellikleri.

### <a name="specifying-the-desired-properties-of-the-module-twin"></a>Modül twin istenen özelliklerini belirtme

Dağıtım bildirimi modülü twin her bir sınır Aracısı bölümünde belirtilen kullanıcı modüller istenen özelliklerini belirtebilirsiniz.

İstenen özelliklerde dağıtım bildiriminde belirtildiğinde, tüm istenen özelliklerinde şu anda modülü twin üzerine yazın.

Dağıtım bildiriminde bir modül twin'ın istenen özellikleri belirtmezseniz, IOT hub'ı herhangi bir şekilde modülü twin değiştirmez ve istenen özellikleri programlı olarak ayarlamak mümkün olacaktır.

Cihaz çiftlerini değiştirmenize izin mekanizmalarının aynısını modülü çiftlerini değiştirmek için kullanılır. Lütfen [cihaz çifti Geliştirici Kılavuzu](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins) daha fazla bilgi için.   

### <a name="deployment-manifest-example"></a>Dağıtım bildirim örneği

Bu dağıtım bildirim JSON belgesini örneği.

    {
    "moduleContent": {
        "$edgeAgent": {
            "properties.desired": {
                "schemaVersion": "1.0",
                "runtime": {
                    "type": "docker",
                    "settings": {
                        "minDockerVersion": "v1.25",
                        "loggingOptions": ""
                    }
                },
                "systemModules": {
                    "edgeAgent": {
                        "type": "docker",
                        "settings": {
                        "image": "microsoft/azureiotedge-agent:1.0-preview",
                        "createOptions": ""
                        }
                    },
                    "edgeHub": {
                        "type": "docker",
                        "status": "running",
                        "restartPolicy": "always",
                        "settings": {
                        "image": "microsoft/azureiotedge-hub:1.0-preview",
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
                        "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
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

## <a name="reference-edge-agent-module-twin"></a>Başvuru: Kenar Aracısı modülü twin

Edge Aracısı modülü twin adlı `$edgeAgent` ve bir cihaz ve IOT hub'ı çalıştıran kenar Aracısı arasındaki iletişimi düzenler.
İstenen özellikleri, bir dağıtım bildirimi tek aygıt veya ölçekli dağıtımının bir parçası belirli bir cihazda uygularken ayarlanır. Bkz: [dağıtım ve izleme] [ lnk-deploy] IOT sınır cihazları modülleri dağıtma hakkında daha fazla bilgi için.

### <a name="edge-agent-twin-desired-properties"></a>Edge Aracısı istenen twin özellikleri

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| Runtime.Type | "Docker" olması gerekir | Evet |
| runtime.settings.minDockerVersion | Bu dağıtım listesi tarafından gereken en düşük Docker sürüm için ayarlayın | Evet |
| runtime.settings.loggingOptions | Edge Aracısı kapsayıcısı için günlüğe kaydetme seçeneklerini içeren stringified JSON. [Docker günlük seçenekleri][lnk-docker-logging-options] | Hayır |
| systemModules.edgeAgent.type | "Docker" olması gerekir | Evet |
| systemModules.edgeAgent.settings.image | Edge Aracısı görüntüsü URI'si. Şu anda, kenar Aracısı kendisini güncelleştirmek mümkün değil. | Evet |
| systemModules.edgeAgent.settings.createOptions | Edge Aracısı kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| systemModules.edgeAgent.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| systemModules.edgeHub.type | "Docker" olması gerekir | Evet |
| systemModules.edgeHub.status | "Çalışıyor olması gerektiğini" | Evet |
| systemModules.edgeHub.restartPolicy | "Her zaman" olması gerekir | Evet |
| systemModules.edgeHub.settings.image | Edge hub görüntüsü URI'si. | Evet |
| systemModules.edgeHub.settings.createOptions | Edge hub kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| systemModules.edgeHub.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |
| modules.{moduleId}.version | Bu modül sürümü temsil eden kullanıcı tanımlı bir dize. | Evet |
| modules.{moduleId}.type | "Docker" olması gerekir | Evet |
| modules.{moduleId}.restartPolicy | {"hiçbir zaman" \| "üzerinde başarısız oldu-" \| "üzerinde-sağlıksız" \| "her zaman"} | Evet |
| modules.{moduleId}.settings.image | Modül görüntü URI'si. | Evet |
| modules.{moduleId}.settings.createOptions | Modül kapsayıcı oluşturma seçeneklerini içeren stringified JSON. [Docker oluşturma seçenekleri][lnk-docker-create-options] | Hayır |
| modules.{moduleId}.configuration.id | Bu modül dağıtılan dağıtım kimliği. | Bir dağıtım kullanarak bu bildirimi uygulandığında bu IOT Hub tarafından ayarlanır. Parçası olmayan bir dağıtım bildirimi. |

### <a name="edge-agent-twin-reported-properties"></a>Edge Aracısı twin özellikleri bildirdi

Edge Aracısı özellikleri üç ana parça bilgi dahil bildirdi:

1. Uygulama son görülen istenen özelliklerinin durumunu;
2. Şu anda kenar aracı tarafından raporlanan cihaz üzerinde çalışan modüllerin durumunu; ve
3. Şu anda cihazda çalışan istenen özelliklerinin kopyası.

Bu son parça bilgi son istenen özellikler çalışma zamanı tarafından başarıyla uygulanmaz ve aygıt hala bir önceki dağıtım bildirimi çalışıyor durumda yararlıdır.

> [!NOTE]
> İle sorgulanabilir gibi kenar aracının bildirilen özelliklerini yararlı [IOT hub'ı sorgu dili] [ lnk-iothub-query] ölçekte dağıtımların durumunu incelemek için. Başvurmak [dağıtımları] [ lnk-deploy] bu özelliğinin nasıl kullanılacağı hakkında daha fazla bilgi için.

Aşağıdaki tabloda, istenen özelliklerinden kopyalandığında bilgileri içermez.

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu int son kenar aracı tarafından işlenen istenen özellik sürümünü gösterir. |
| lastDesiredStatus.code | Bu sınır aracı tarafından görülen son istenen özelliklerine başvuran durum kodudur. İzin verilen değerler: `200` başarılı, `400` geçersiz yapılandırma `412` geçersiz şema sürümüne `417` İstenen özelliklerde boş `500` başarısız oldu |
| lastDesiredStatus.description | Durum metin açıklaması |
| deviceHealth | `healthy` tüm modülleri çalışma zamanı durumunu ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| configurationHealth.{deploymentId}.health | `healthy` {Deploymentıd} dağıtımı tarafından ayarlanmış olan tüm modülleri çalışma zamanı durumunu ya da ise `running` veya `stopped`, `unhealthy` Aksi takdirde |
| runtime.platform.OS | Aygıtta çalışan işletim sistemi raporlama |
| runtime.platform.architecture | Cihazda raporlama CPU mimarisi |
| systemModules.edgeAgent.runtimeStatus | Edge Aracısı'nın bildirilen durum: {"çalışır" \| "sağlıksız"} |
| systemModules.edgeAgent.statusDescription | Edge aracının bildirilen durumunu metin açıklaması. |
| systemModules.edgeHub.runtimeStatus | Edge hub'ın geçerli durum: {"çalışır" \| "durduruldu" \| "başarısız" \| "geri Çekilme" \| "sağlıksız"} |
| systemModules.edgeHub.statusDescription | Edge hub sağlıksız varsa geçerli durumunu metin açıklaması. |
| systemModules.edgeHub.exitCode | Çıktı, çıkış kodu kenar hub kapsayıcı tarafından bildirilen varsa |
| systemModules.edgeHub.startTimeUtc | Edge hub son başlatıldığı zaman |
| systemModules.edgeHub.lastExitTimeUtc | Zaman zaman kenar hub'ı son çıkıldı |
| systemModules.edgeHub.lastRestartTimeUtc | Zaman kenar hub en son ne zaman yeniden başlatıldı |
| systemModules.edgeHub.restartCount | Bu modül yeniden başlatma ilkesi bir parçası olarak yeniden sayısı. |
| modules.{moduleId}.runtimeStatus | Modülün geçerli durum: {"çalışır" \| "durduruldu" \| "başarısız" \| "geri Çekilme" \| "sağlıksız"} |
| modules.{moduleId}.statusDescription | Sağlıksız durumunda modülünü geçerli durumunu metin açıklaması. |
| modules.{moduleId}.exitCode | Çıktı, çıkış kodu modülü kapsayıcı tarafından bildirilen varsa |
| modules.{moduleId}.startTimeUtc | Modül son başlatıldığı zaman |
| modules.{moduleId}.lastExitTimeUtc | Zaman zaman modülü son çıkıldı |
| modules.{moduleId}.lastRestartTimeUtc | Zaman modülü en son ne zaman yeniden başlatıldı |
| modules.{moduleId}.restartCount | Bu modül yeniden başlatma ilkesi bir parçası olarak yeniden sayısı. |

## <a name="reference-edge-hub-module-twin"></a>Başvuru: Kenar hub modülü twin

Edge hub'ına yönelik modülü twin adlı `$edgeHub` ve bir cihaz ve IOT hub'ı çalıştıran kenar hub arasındaki iletişimi düzenler.
İstenen özellikleri, bir dağıtım bildirimi tek aygıt veya ölçekli dağıtımının bir parçası belirli bir cihazda uygularken ayarlanır. Bkz: [dağıtımları] [ lnk-deploy] IOT sınır cihazları modülleri dağıtma hakkında daha fazla bilgi için.

### <a name="edge-hub-twin-desired-properties"></a>Edge hub istenen twin özellikleri

| Özellik | Açıklama | Dağıtım bildiriminde gerekli |
| -------- | ----------- | -------- |
| schemaVersion | "1.0" olması gerekir | Evet |
| routes.{routeName} | Bir sınır hub yolunu temsil eden dize. | `routes` Öğesi var ancak boş olabilir. |
| storeAndForwardConfiguration.timeToLiveSecs | Edge hub tutar saniye cinsinden zaman bağlantısı kesilmiş yönlendirme uç noktaları durumunda iletileri örneğin kesilmiş IOT hub'ı veya yerel modül | Evet |

### <a name="edge-hub-twin-reported-properties"></a>Edge hub twin özellikleri bildirdi

| Özellik | Açıklama |
| -------- | ----------- |
| lastDesiredVersion | Bu int son kenar hub tarafından işlenen istenen özellik sürümünü gösterir. |
| lastDesiredStatus.code | Bu sınır hub tarafından görülen son istenen özelliklerine başvuran durum kodudur. İzin verilen değerler: `200` başarılı, `400` geçersiz yapılandırma `500` başarısız oldu |
| lastDesiredStatus.description | Durum metin açıklaması |
| istemciler. {aygıt veya modül kimliği} .status | Bu cihaz ya da modül bağlantı durumu. Olası değerler {"bağlı" \| "bağlantısı kesildi"}. Yalnızca modülü kimlikleri bağlantısı kesik bir durumda olabilir. Edge hub'a bağlanan Aşağı Akış aygıtları yalnızca bağlıyken görünür. |
| istemciler. {aygıt veya modül kimliği} .lastConnectTime | Aygıt ya da bağlı modülü son saat |
| istemciler. {aygıt veya modül kimliği} .lastDisconnectTime | Cihaz veya modülü son zamanı bağlantısı kesildi |

## <a name="next-steps"></a>Sonraki adımlar

Artık IOT kenar modülleri nasıl kullanıldığı bildiğinize göre [IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlamanız][lnk-module-dev].

[lnk-deploy]: module-deployment-monitoring.md
[lnk-edgeagent-desired]: #edge-agent-twin-desired-properties
[lnk-edgeagent-reported]: #edge-agent-twin-reported-properties
[lnk=edgehub-desired]: #edge-hub-twin-desired-properties
[lnk-edgehub-reported]: #edge-hub-twin-reported-properties
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-module-dev]: module-development.md
