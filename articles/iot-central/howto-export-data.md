---
title: Azure IOT Central verilerinizi dışarı aktarma | Microsoft Docs
description: Azure IOT Central uygulamanızdan veri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/3/2018
ms.topic: article
ms.prod: azure-iot-central
manager: peterpr
ms.openlocfilehash: 6d35e3cfefcefef0b4ff40364cbdab92d486b769
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008814"
---
# <a name="export-your-data-in-azure-iot-central"></a>Azure IOT Central verilerinizi dışarı aktarma

Verileri sürekli dışarı aktarma verileri Azure Blob Depolama hesabınızda düzenli aralıklarla dışarı aktarmak için kullanın. Dışa aktarmayı seçin **ölçümleri**, **cihazları**, ve **cihaz şablonları** dosyalarındaki [Apache AVRO](https://avro.apache.org/docs/current/index.html) biçimi. Azure Machine Learning eğitim modelleri veya Power bı'da uzun vadeli eğilim analizi gibi Durgun yoldaki analiz için dışarı aktarılan verileri kullanın.

> [!Note]
> Verileri sürekli dışarı açtığınızda, yalnızca o andan itibaren başlayarak gelen verileri elde edersiniz. Şu anda veri, verileri sürekli dışarı aktarma kapatıldı almak mümkün değildir. Sürekli veri daha fazla geçmiş verileri korumak için verme erken dönüştürebilirsiniz!

## <a name="prerequisites"></a>Önkoşullar

- Bir genişletilmiş 30 günlük deneme veya Ücretli uygulaması
- Azure aboneliği ile Azure hesabı
- Aynı Azure IOT Central uygulamanızda bir yönetici hesabıdır
- Aynı Azure hesabı, bir depolama hesabı oluşturun veya mevcut bir depolama hesabı aynı Azure aboneliğinde erişim izni

## <a name="types-of-data-to-export"></a>Dışarı aktarmak için veri türleri

### <a name="measurements"></a>Ölçümler

Depolama hesabınızda oturum cihazları gönderir ve ölçümleri dışa aktarılır. Ölçümleri veriler kez yaklaşık bir dakika, söz konusu zaman penceresi içinde tüm cihazlardan IOT Central tarafından alınan tüm yeni iletileri içeren aktarılır. Dışarı aktarılan AVRO dosyalarını tarafından dışarı aktarılan iletileri dosyaları ile aynı biçimde olan [IOT Hub ileti yönlendirme](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) blob depolama.

> [!NOTE]
> Ölçümler gönderilen cihazlar, cihaz kimlikleri (aşağıya bakın) tarafından temsil edilir. Cihaz adları almak için cihazları anlık görüntüleri çok dışarı aktarmanız gerekir. Cihaz kimliği için eşleşen connectionDeviceId kullanarak her bir ileti kaydı ilişkilendirebilirsiniz.

Bu, bir kayıt kodu çözülmüş AVRO örneğidir.

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "2383d8ba-c98c-403a-b4d5-8963859643bb",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "636614021491644195",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Cihazlar

Verileri sürekli dışarı aktarma üzerinde ilk kez açtığınızda, tüm cihazları içeren tek bir anlık görüntüyü dışarı aktarılır. Buna aşağıdakiler dahildir:
- Cihaz kimlikleri
- Cihaz adları
- Cihaz şablon kimlikleri
- Özellik değerleri
- Ayarları değerleri

Bir kez yaklaşık bir dakika, yeni bir anlık görüntü içeren yazılır:

- Son anlık görüntünün bu yana eklenen yeni cihazları
- Son anlık görüntünün beri değiştirilmiş özellikleri ve ayarları değerleri olan cihazlar

> [!NOTE]
> Bu yana son anlık görüntü silinmiş olan cihazları dışa aktarılmaz. Şu anda silinen cihazlar için anlık görüntüleri gösterge yoktur.

> [!NOTE]
> Her bir cihaza ait cihaz şablonu cihaz şablon kimliği ile temsil edilir Cihaz şablonunun adını almak için cihaz şablonu anlık görüntüleri çok dışarı aktarmanız gerekir.

Kodu çözülen AVRO dosyasında her kaydı şöyle görünür:

```json
{
    "id": "2383d8ba-c98c-403a-b4d5-8963859643bb",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceTemplate": {
        "id": "c318d580-39fc-4aca-b995-843719821049",
        "version": "1.0.0"
    },
    "properties": {
        "cloud": {
            "location": "New York",
            "maintCon": true,
            "tempThresh": 20
        },
        "device": {
            "lastReboot": "2018-02-09T22:22:47.156Z"
        }
    },
    "settings": {
        "device": {
            "fanSpeed": 0
        }
    }
}
```

### <a name="device-templates"></a>Cihaz şablonları

Verileri sürekli dışarı aktarma üzerinde ilk kez açtığınızda, tüm cihaz şablonlarını içeren tek bir anlık görüntüyü dışarı aktarılır. Buna aşağıdakiler dahildir: 
- Cihaz şablon kimlikleri
- Ölçüm veri türleri ve en düşük/en yüksek değerleri
- Özellikleri veri türleri ve varsayılan değerleri
- Ayarları veri türleri ve varsayılan değerleri

Bir kez yaklaşık bir dakika, yeni bir anlık görüntü içeren yazılır:

- Son anlık görüntünün bu yana eklenen yeni cihaz şablonları
- Ölçümler, özellikler ve son anlık görüntünün beri değiştirilen ayarlar tanımları olan cihaz şablonları

> [!NOTE]
> Bu yana son anlık görüntü silinmiş olan cihaz şablonlarını dışa aktarılmaz. Şu anda silinen bir cihaz şablonları için anlık görüntüleri, gösterge yoktur.

Kodu çözülen AVRO dosyasında her kaydı şöyle görünür:

```json
{
    "id": "c318d580-39fc-4aca-b995-843719821049",
    "name": "Refrigerated Vending Machine",
    "version": "1.0.0",
    "measurements": {
        "telemetry": {
            "humidity": {
                "dataType": "double",
                "name": "Humidity"
            },
            "magnetometerX": {
                "dataType": "double",
                "name": "Magnetometer X"
            },
            "magnetometerY": {
                "dataType": "double",
                "name": "Magnetometer Y"
            },
            "magnetometerZ": {
                "dataType": "double",
                "name": "Magnetometer Z"
            }
        },
        "states": {
            "connectivity": {
                "dataType": "enum",
                "name": "Connectivity"
            }
        },
        "events": {
            "opened": {
                "name": "Door Opened",
                "category": "informational"
            }
        }
    },
    "settings": {
        "device": {
            "fanSpeed": {
                "dataType": "double",
                "name": "Fan Speed",
                "initialValue": 0
            }
        }
    },
    "properties": {
        "cloud": {
            "location": {
                "dataType": "string",
                "name": "Location",
                "initialValue": "Seattle"
            },
            "maintCon": {
                "dataType": "boolean",
                "name": "Maintenance Contract",
                "initialValue": true
            },
            "tempThresh": {
                "dataType": "double",
                "name": "Temperature Alert Threshold",
                "initialValue": 30
            }
        },
        "device": {
            "lastReboot": {
                "dataType": "dateTime",
                "name": "Last Reboot"
            }
        }
    }
}
```

## <a name="how-to-set-up-data-export"></a>Verileri dışarı aktarma ' ayarlama

1. Zaten yoksa, bir Azure depolama hesabı oluşturma **uygulamanızın bulunduğu Azure aboneliğinde**. [Buraya](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) yeni bir Azure depolama hesabı oluşturmak için Azure portalına gitmek için.

    - Seçin *genel amaçlı* veya *Blob Depolama* hesap türleri
    - IOT Central uygulamanızın bulunduğu abonelik seçin. Abonelik görmüyorsanız, aboneliğe erişim isteyin veya farklı bir Azure hesabı ile oturum gerekebilir.
    - Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun. Hakkında bilgi edinin [yeni bir depolama hesabı oluşturma.](https://aka.ms/blobdocscreatestorageaccount)

2. IOT Central verilerinizi dışarı aktarmak için depolama hesabınızdaki bir kapsayıcı oluşturun. Depolama hesabınıza Blob'lara göz at -> Yeni bir kapsayıcı oluşturmak gidin. ![Kapsayıcı görüntüsü oluşturma](media/howto-export-data/createcontainer.png)

3. Azure hesap ile IOT Central uygulamasına oturum açın.
1. Verileri sürekli dışarı aktarma -> Yönetim gidin.
![IOT Central Değerinde](media/howto-export-data/continuousdataexport.PNG)
1. Açılır menüleri kullanarak kullanarak depolama hesabı ve kapsayıcı seçin. Ardından verileri dışarı aktarmak için farklı türde açıp kapatması için değiştirme düğmelerini kullanın.
1. Son olarak, sürekli veri iki durumlu düğmeyi kullanarak dışarı aktarma üzerinde açın ve "Kaydet" basın.
1. Birkaç dakika bekleyin ve verileriniz depolama hesabınızda görünmesi görmeniz gerekir. Depolama hesabınıza gidin, göz atma BLOB'ları seçin, Kapsayıcınızı seçin ve üç klasör görürsünüz. Farklı veri türlerini içeren AVRO dosyaları için varsayılan yollar şunlardır:
- İletileri: **{container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Aygıtlar: **{container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**
- Cihaz şablonları: **{container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro**

## <a name="how-to-read-exported-avro-files"></a>AVRO dosyalarının nasıl okunacağını dışarı

AVRO bir ikili biçimi olduğundan ham durumlarını dosyaları okunamıyor. JSON biçimine çözülebilir. Aşağıdaki örnekler, ölçümleri, cihazları ve cihaz şablonları yukarıdaki örnekleri kullanarak AVRO dosyalarını ayrıştırmak gösterilmektedir.

## <a name="python"></a>Python

### <a name="install-pandas-and-the-pandaavro-package"></a>Pandas ve PandaAvro paketi yükleyin:

```python
pip install pandas
pip install pandavro
```
### <a name="parse-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    measurements = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary with the device id
    # located under the "connectionDeviceId" key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of utf-8 bytes that is stringified and parsed
    # as json. In this example, we pull the "humidity" property off of each one
    # to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, we print the new DataFrame with our device ids and humidities
    print(transformed)

```
### <a name="parse-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    devices = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device id is available directly in the "id" column
    transformed["device_id"] = devices["id"]

    # The template id and version are present in a dictionary under the
    # deviceTemplate column
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)

```

### <a name="parse-device-templates-avro-file"></a>Cihaz şablonları AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the avro file into a pandas DataFrame where each record is
    # a single row
    templates = pdx.from_avro(filePath)

    # In this example, we create a new DataFrame and load a series for each
    # column we map into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available directly in the "id" and "version"
    # columns, respectively
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary under the
    # settings column
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, we print the new DataFrame with our device and template
    # information, along with the value of the fan speed
    print(transformed)
```

## <a name="c"></a>C#

### <a name="install-microsofthadoopavro"></a>Microsoft.Hadoop.Avro yükleyin

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

### <a name="parse-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;
using Newtonsoft.Json;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contain multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the fields
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    var systemProperties = record.GetField<IDictionary<string, object>>("SystemProperties");
                    var deviceId = systemProperties["connectionDeviceId"] as string;
                    Console.WriteLine("Device ID: {0}", deviceId);

                    using (var stream = new MemoryStream(record.GetField<byte[]>("Body")))
                    {
                        using (var streamReader = new StreamReader(stream, Encoding.UTF8))
                        {
                            var body = JsonSerializer.Create().Deserialize(streamReader, typeof(IDictionary<string, dynamic>)) as IDictionary<string, dynamic>;
                            var humidity = body["humidity"];
                            Console.WriteLine("Humidity: {0}", humidity);
                        }
                    }
                }
            }
        }
    }
}
```

### <a name="parse-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the "deviceTemplate" field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
                        deviceId,
                        templateId,
                        templateVersion,
                        fanSpeed
                    );
                }
            }
        }
    }
}

```
### <a name="parse-device-templates-avro-file"></a>Cihaz şablonları AVRO dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro Container, it may contains multiple blocks
            // Loop through each block within the container
            while (reader.MoveNext())
            {
                // Loop through Avro record inside the block and extract the
                // fields from it
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // record and make the function IEnumerable<AvroRecord>
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep,
                    // with the first level indicating settings or properties
                    // and the second indicating the kind of setting/property
                    var settingsRecord = record.GetField<AvroRecord>("settings");
                    var deviceSettingsRecord = settingsRecord.GetField<IDictionary<string, dynamic>>("device");
                    var fanSpeed = deviceSettingsRecord["fanSpeed"];
                    
                    Console.WriteLine(
                        "ID: {1}, Version: {2}, Fan Speed: {3}",
                        id,
                        version,
                        fanSpeed
                    );
                }
            }
        }
    }
}
```

## <a name="javascript"></a>Javascript

### <a name="install-avsc"></a>Avsc yükleyin

```javascript
npm install avsc
```

### <a name="parse-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the avro file and parse the device id and humidity from each record
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the system properties
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the Body from a Buffer to a string and parse it
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property off of the body
        const humidity = body.humidity;

        // Log the device id and humidity we found
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device id from the id property
        const deviceId = record.id;

        // Fetch the tempalte id and version from the deviceTemplate property
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

### <a name="parse-device-templates-avro-file"></a>Cihaz şablonları AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the avro file and parse the device and template identification info as
// well as the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template id and version from the id and verison properties
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed off the nested device settings property
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the device id and humidity we found
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream. We
        // collect them up into an array and return them when we get to the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [cihazlarınızı yönetme](howto-manage-devices.md) cihaz Gezgini'nde. 
