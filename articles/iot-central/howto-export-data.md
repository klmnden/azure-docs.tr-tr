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
ms.openlocfilehash: 3ca2bc56c03e5bbabbd9b2f17edc621bdd94b02f
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622492"
---
# <a name="export-your-data-in-azure-iot-central"></a>Azure IOT Central verilerinizi dışarı aktarma

Bu makalede sürekli veri dışa aktarma Özelliği Azure IOT Central verileri Azure Blob Depolama hesabınıza düzenli aralıklarla dışarı aktarmak için nasıl kullanılacağını açıklar. Dışarı aktarabilirsiniz **ölçümleri**, **cihazları**, ve **cihaz şablonları** dosyalarla [Apache AVRO](https://avro.apache.org/docs/current/index.html) biçimi. Dışarı aktarılan verileri eğitim modeller Azure Machine Learning veya Microsoft Power BI uzun vadeli eğilim analizi gibi Durgun yoldaki analiz için kullanılabilir.

> [!Note]
> Verileri sürekli dışarı aktarma üzerinde etkinleştirdiğinizde, ileriye doğru o andan itibaren yalnızca verileri alın. Şu anda, verileri sürekli dışarı aktarma kapalıydı ne zaman bir kez verileri alınamıyor. Daha fazla geçmiş verileri korumak için verileri sürekli dışarı aktarma üzerinde erken açın.

## <a name="prerequisites"></a>Önkoşullar

- Bir genişletilmiş 30 günlük deneme IOT Central uygulamasına veya Ücretli bir uygulama.
- Bir Azure aboneliği bir Azure hesabı.
- Aynı Azure IOT Central uygulamanızdaki bir yönetici hesabıdır.
- Aynı Azure hesabı, bir depolama hesabı oluşturun veya mevcut bir depolama hesabı aynı Azure aboneliğinde erişmek için izinlere sahiptir.

## <a name="types-of-data-to-export"></a>Dışarı aktarmak için veri türleri

### <a name="measurements"></a>Ölçümler

Cihazları gönderir ve ölçümler, depolama hesabınıza dakikada bir kez verilir. Veri ile IOT Central, bu süre boyunca tüm cihazlardan alınan tüm yeni ileti yok. Dışarı aktarılan AVRO dosyalarını tarafından dışarı aktarılan ileti dosyaları olarak aynı biçimi kullanır. [IOT Hub ileti yönlendirme](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) Blob Depolama.

> [!NOTE]
> Ölçümler gönderen cihazları (aşağıdaki bölümlere bakın) cihaz kimlikleri tarafından temsil edilir. Cihaz adları almak için cihaz anlık görüntülerin dışarı aktarın. Her bir ileti kaydı kullanarak ilişkilendirmek **connectionDeviceId** cihaz kimliği ile eşleşen

Aşağıdaki örnek, bir kaydı bir kodu çözülmüş AVRO dosyasında gösterir:

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

Verileri sürekli dışarı aktarma ilk açıldığında, tüm cihazlar ile tek bir anlık görüntüyü dışarı aktarılır. Anlık görüntü içerir:
- Cihaz kimlikleri.
- Cihaz adları.
- Cihaz şablon kimlikleri belirtilmeli.
- Özellik değeri.
- Ayar değerleri.

Yeni bir anlık görüntü bir kez dakikada yazılır. Anlık görüntü içerir:

- Yeni cihazları son anlık görüntünün beri eklenmiş.
- Değiştirilmiş özellik ve değerlerini beri son anlık görüntünün ayarlama olan cihazlar.

> [!NOTE]
> Cihazlar olmayan son anlık görüntünün dışarı beri silindi. Şu anda anlık görüntüleri göstergeleri silinen cihazlar için yok.
>
> Her bir cihaza ait cihaz şablonu cihaz şablon kimliği ile temsil edilir Cihaz şablonunun adını almak için cihaz şablonu anlık görüntülerin dışarı aktarın.

Kodu çözülen AVRO dosyasında her kaydı şuna benzer:

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

Verileri sürekli dışarı aktarma ilk açıldığında, tüm cihaz şablonları ile tek bir anlık görüntüyü dışarı aktarılır. Anlık görüntü içerir: 
- Cihaz şablon kimlikleri belirtilmeli.
- Ölçüm veri türleri ve en düşük/en yüksek değerleri.
- Özellik veri türleri ve varsayılan değerleri.
- Veri türleri ve varsayılan değerleri ayarlama.

Yeni bir anlık görüntü bir kez dakikada yazılır. Anlık görüntü içerir:

- Son anlık görüntünün bu yana eklenen yeni cihaz şablonları.
- Değiştirilen ölçümleri, özellik ve tanımları beri son anlık görüntünün ayarlama şablonlarıyla cihaz.

> [!NOTE]
> Bu yana son anlık görüntü silinmiş cihaz şablonlarını dışa aktarılmaz. Şu anda anlık görüntüleri göstergeleri silinen cihaz şablonları için yok.

Kodu çözülen AVRO dosyasında her kaydı şuna benzer:

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

## <a name="set-up-continuous-data-export"></a>Verileri sürekli dışarı aktarma ayarlayın

1. Azure depolama hesabınız yoksa, [yeni depolama hesabı oluşturma](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) Azure portalında. Depolama hesabı oluşturma **IOT Central uygulamanızın Azure aboneliğinde**.
    - Hesap türü seçin **genel amaçlı** veya **Blob Depolama**.
    - IOT Central uygulamanızın aboneliği seçin. Abonelik görmüyorsanız, bir farklı Azure hesabını veya istek aboneliğe erişim için oturum açmanız gerekebilir.
    - Mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun. Hakkında bilgi edinin [yeni bir depolama hesabı oluşturma](https://aka.ms/blobdocscreatestorageaccount).

2. IOT Central verilerinizi dışarı aktarmak için depolama hesabında bir kapsayıcı oluşturun. Depolama hesabınıza gidin. Altında **Blob hizmeti**seçin **Blob'lara göz at**. Seçin **kapsayıcı** yeni bir kapsayıcı oluşturmak için.

   ![Bir kapsayıcı oluşturma](media/howto-export-data/createcontainer.png)

3. IOT Central uygulamanız aynı Azure hesabını kullanarak oturum açın.

4. Altında **Yönetim**seçin **verileri dışarı aktarma**.

   ![Yapılandırma verileri sürekli dışarı aktarma](media/howto-export-data/continuousdataexport.PNG)

5. İçinde **depolama hesabı** aşağı açılan liste kutusunda, depolama hesabınızı seçin. İçinde **kapsayıcı** aşağı açılan liste kutusunda, kapsayıcınızı seçin. Altında **dışarı aktarmak için veri**, her tür ayarlayarak dışarı aktarmak için veri türü belirtin **üzerinde**.

6. Verileri sürekli dışarı aktarma üzerinde etkinleştirmek için ayarlanmış **verileri dışarı aktarma** için **üzerinde**. **Kaydet**’i seçin.

7. Birkaç dakika sonra verileriniz depolama hesabınızda görünür. Depolama hesabınıza gidin. Seçin **Gözat blobları** > kapsayıcı. Dışarı aktarma verileri için üç klasör görürsünüz. AVRO dosyalarını dışarı aktarma verileri için varsayılan yollar şunlardır:
    - İleti: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Aygıtlar: {container}/devices/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro
    - Cihaz şablonları: {container}/deviceTemplates/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/00.avro

## <a name="read-exported-avro-files"></a>Verilen AVRO dosyaları okuma

AVRO bir ikili biçimi olduğundan ham durumlarını dosyaları okunamıyor. Dosyaları JSON biçimine çözülebilir. Aşağıdaki örnekler, ölçümleri, cihazları ve cihaz şablonları AVRO dosyalarını ayrıştırmak gösterilmektedir. Örnekler önceki bölümde açıklanan örnekler karşılık gelir.

### <a name="read-avro-files-by-using-python"></a>Python kullanarak AVRO dosyalarını okuma

#### <a name="install-pandas-and-the-pandavro-package"></a>Pandas ve pandavro paket yükleme

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    measurements = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The SystemProperties column contains a dictionary
    # with the device ID located under the connectionDeviceId key.
    transformed["device_id"] = measurements["SystemProperties"].apply(lambda x: x["connectionDeviceId"])

    # The Body column is a series of UTF-8 bytes that is stringified
    # and parsed as JSON. This example pulls the humidity property
    # from each column to get the humidity field.
    transformed["humidity"] = measurements["Body"].apply(lambda x: json.loads(bytes(x).decode('utf-8'))["humidity"])

    # Finally, print the new DataFrame with our device IDs and humidities.
    print(transformed)

```

#### <a name="parse-a-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["id"]

    # The template ID and version are present in a dictionary under
    # the deviceTemplate column.
    transformed["template_id"] = devices["deviceTemplate"].apply(lambda x: x["id"])
    transformed["template_version"] = devices["deviceTemplate"].apply(lambda x: x["version"])

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = devices["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)

```

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları AVRO dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the AVRO file into a pandas DataFrame
    # where each record is a single row.
    templates = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The template and version are available in the id and version columns.
    transformed["template_id"] = templates["id"]
    transformed["template_version"] = templates["version"]

    # The fanSpeed setting value is located in a nested dictionary
    # under the settings column.
    transformed["fan_speed"] = templates["settings"].apply(lambda x: x["device"]["fanSpeed"])

    # Finally, print the new DataFrame with our device and template
    # information, along with the value of the fan speed.
    print(transformed)
```

### <a name="read-avro-files-by-using-c"></a>C# kullanarak okuma AVRO dosyalarını

#### <a name="install-the-microsofthadoopavro-package"></a>Microsoft.Hadoop.Avro paketi yükleyin

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

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
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("id");

                    // The device template information is stored in a sub-record
                    // under the deviceTemplate field.
                    var deviceTemplateRecord = record.GetField<AvroRecord>("deviceTemplate");
                    var templateId = deviceTemplateRecord.GetField<string>("id");
                    var templateVersion = deviceTemplateRecord.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
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

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları AVRO dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one AVRO container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the AVRO records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var id = record.GetField<string>("id");
                    var version = record.GetField<string>("version");

                    // The settings and properties are nested two levels deep.
                    // The first level indicates settings or properties.
                    // The second level indicates the type of setting or property.
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

### <a name="read-avro-files-by-using-javascript"></a>Javascript kullanarak AVRO dosyalarını okuma

#### <a name="install-the-avsc-package"></a>Avsc paketi yükleyin

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device ID and humidity from each record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the system properties.
        const deviceId = record.SystemProperties.connectionDeviceId;

        // Convert the body from a buffer to a string and parse it.
        const body = JSON.parse(record.Body.toString());

        // Get the humidty property from the body.
        const humidity = body.humidity;

        // Log the retrieved device ID and humidity.
        console.log(`Device ID: ${deviceId}`);
        console.log(`Humidity: ${humidity}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-devices-avro-file"></a>Cihazları AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the id property.
        const deviceId = record.id;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`ID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları AVRO dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the AVRO file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the template ID and version from the id and verison properties.
        const templateId = record.id;
        const templateVersion = record.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device id and humidity.
        console.log(`Template ID: ${templateId}, Template Version: ${templateVersion}, Fan Speed: ${fanSpeed}`);
    }
}

function load(filePath) {
    return new Promise((resolve, reject) => {
        // The file decoder emits each record as a data event on a stream.
        // Collect the records into an array and return them at the end.
        const records = [];
        avro.createFileDecoder(filePath)
            .on('data', record => { records.push(record); })
            .on('end', () => resolve(records))
            .on('error', reject);
    });
}
```

## <a name="next-steps"></a>Sonraki adımlar

Verileriniz dışarı aktarma artık bildiğinize göre sonraki adıma devam edin:

> [!div class="nextstepaction"]
> [Nasıl verilerinizi Power bı'da görselleştirin](howto-connect-powerbi.md)
