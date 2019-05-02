---
title: Verilerinizi Azure Blob depolama alanına dışarı aktarma | Microsoft Docs
description: Azure Blob Depolama için Azure IOT Central uygulamanızdan veri dışarı aktarma
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: f81ca34931e2ee4bce35fa06195fb64c47ef9a7b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682024"
---
# <a name="export-your-data-to-azure-blob-storage"></a>Verilerinizi Azure Blob depolama alanına dışarı aktarma

*Bu konu, Yöneticiler için geçerlidir.*

Bu makalede sürekli veri dışa aktarma Özelliği Azure IOT Central düzenli aralıklarla verileri dışarı aktarmak için nasıl kullanılacağını açıklar, **Azure Blob Depolama hesabı**. Dışarı aktarabilirsiniz **ölçümleri**, **cihazları**, ve **cihaz şablonları** dosyalara Apache Avro biçimi. Dışarı aktarılan verileri eğitim modeller Azure Machine Learning veya Microsoft Power BI uzun vadeli eğilim analizi gibi Durgun yoldaki analiz için kullanılabilir.

> [!Note]
> Bir kez daha, verileri sürekli dışarı aktarma üzerinde etkinleştirdiğinizde, ileriye doğru o andan itibaren yalnızca verileri alın. Şu anda, verileri sürekli dışarı aktarma kapalıydı ne zaman bir kez verileri alınamıyor. Daha fazla geçmiş verileri korumak için verileri sürekli dışarı aktarma üzerinde erken açın.


## <a name="prerequisites"></a>Önkoşullar

- IOT Central uygulamanızda yönetici olmanız gerekir


## <a name="set-up-export-destination"></a>Dışarı aktarma hedef ayarlayın

Vermek için mevcut bir depolama yoksa, şu adımları izleyin:

## <a name="create-storage-account"></a>Depolama hesabı oluşturma

1. Oluşturma bir [Azure portalında yeni depolama hesabı](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Daha fazla bilgi [Azure depolama belgeleri](https://aka.ms/blobdocscreatestorageaccount).
2. Hesap türü seçin **genel amaçlı** veya **Blob Depolama**.
3. Bir abonelik seçin. 

    > [!Note] 
    > Artık olan diğer abonelikler için verileri dışarı aktarabilirsiniz **aynı** bir Kullandıkça Öde IOT Central uygulamanız için. Bu durumda bir bağlantı dizesi kullanarak bağlanır.

4. Depolama hesabınızdaki bir kapsayıcı oluşturun. Depolama hesabınıza gidin. Altında **Blob hizmeti**seçin **Blob'lara göz at**. Seçin **+ kapsayıcı** üst yeni bir kapsayıcı oluşturmak için


## <a name="set-up-continuous-data-export"></a>Verileri sürekli dışarı aktarma ayarlayın

Verileri dışarı aktarmak için bir depolama hedefi olduğuna göre verileri sürekli dışarı aktarma ' için bu adımları izleyin. 

1. IOT Central uygulamanız için oturum açın.

2. Sol menüde **verileri sürekli dışarı aktarma**.

    > [!Note]
    > Verileri sürekli dışarı aktarma sol taraftaki menüde görmüyorsanız, yöneticinin uygulamanızda değildir. Verileri dışarı aktarma ' için yöneticinin konuşun.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export_menu.PNG)

3. Seçin **+ yeni** sağ üst köşesindeki düğme. Seçin **Azure Blob Depolama** dışarı aktarma hedefi olarak. 

    > [!NOTE] 
    > Dışarı aktarmalar uygulama başına en fazla sayısı beştir. 

    ![Yeni verileri sürekli dışarı aktarma oluştur](media/howto-export-data/export_new.PNG)

4. Aşağı açılan liste kutusunda, **depolama hesabı ad alanı**. Son seçenek, listeden seçebilirsiniz **bir bağlantı dizesi girin**. 

    > [!NOTE] 
    > Depolama hesapları ad alanlarında yalnızca göreceksiniz **IOT Central uygulamanız ile aynı abonelikte**. Bu abonelik dışında bir hedefe dışarı aktarmak istiyorsanız seçin **bir bağlantı dizesi girin** ve 5. adıma bakın.

    > [!NOTE] 
    > 7 günlük deneme uygulamaları, verileri sürekli yapılandırmak için tek yolu dışarı aktarmak için bir bağlantı dizesidir. 7 günlük deneme uygulamalar, ilişkili Azure aboneliği olmadığı için budur.

    ![Yeni değerinde olay hub'ı oluşturma](media/howto-export-data/export-create-blob.png)

5. (İsteğe bağlı) Seçerseniz, **bir bağlantı dizesi girin**, bağlantı dizenizi yapıştırmak için yeni kutusu görünür. Bağlantı dizesini almak için:
    - Depolama hesabı, Azure portalında depolama hesabı'na gidin.
        - Altında **ayarları**seçin **erişim anahtarları**
        - Key1 bağlantı dizesini veya key2 bağlantı dizesini kopyalayın.
 
6. Aşağı açılan liste kutusundan bir kapsayıcı seçin.

7. Altında **dışarı aktarmak için veri**, her tür ayarlayarak dışarı aktarmak için veri türü belirtin **üzerinde**.

6. Verileri sürekli dışarı aktarma üzerinde etkinleştirmek için emin **verileri dışarı aktarma** olduğu **üzerinde**. **Kaydet**’i seçin.

  ![Yapılandırma verileri sürekli dışarı aktarma](media/howto-export-data/export-list-blob.png)

7. Birkaç dakika sonra verilerinizi, seçtiğiniz hedef olarak görünür.


## <a name="export-to-azure-blob-storage"></a>Azure Blob depolamaya Aktar

Depolama hesabınıza bir kez dakikada ölçümleri, cihazları ve cihaz şablonları verileri son dosyasına dışarı aktardığınız beri toplu değişiklikler içeren her bir dosya ile aktarılır. Dışarı aktarılan veriler [Apache Avro](https://avro.apache.org/docs/current/index.html) biçimlendirmek ve içinde üç klasöre aktarılır. Depolama hesabınızdaki varsayılan yollar şunlardır:
- İleti: {container}/measurements/{hubname}/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
- Aygıtlar: {container}/devices/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro
- Cihaz şablonları: {container}/deviceTemplates/{YYYY}/{MM}/{dd}/{hh}/{mm}/{filename}.avro

### <a name="measurements"></a>Ölçümler

Dışarı aktarılan ölçümleri veri ile IOT Central, bu süre boyunca tüm cihazlardan alınan tüm yeni iletisi yok. Dışarı aktarılan dosyalar tarafından dışarı aktarılan ileti dosyaları olarak aynı biçimi kullanır. [IOT Hub ileti yönlendirme](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) Blob Depolama.

> [!NOTE]
> Ölçümler gönderen cihazları (aşağıdaki bölümlere bakın) cihaz kimlikleri tarafından temsil edilir. Cihaz adları almak için cihaz anlık görüntülerin dışarı aktarın. Her bir ileti kaydı kullanarak ilişkilendirmek **connectionDeviceId** eşleşen **DeviceID** cihaz kaydının.

Aşağıdaki örnek, bir kaydı bir kodu çözülmüş Avro dosyasında gösterir:

```json
{
    "EnqueuedTimeUtc": "2018-06-11T00:00:08.2250000Z",
    "Properties": {},
    "SystemProperties": {
        "connectionDeviceId": "<connectionDeviceId>",
        "connectionAuthMethod": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
        "connectionDeviceGenerationId": "<generationId>",
        "enqueuedTime": "2018-06-11T00:00:08.2250000Z"
    },
    "Body": "{\"humidity\":80.59100954598546,\"magnetometerX\":0.29451796907056726,\"magnetometerY\":0.5550332126050068,\"magnetometerZ\":-0.04116681874733441,\"connectivity\":\"connected\",\"opened\":\"triggered\"}"
}
```

### <a name="devices"></a>Cihazlar

Verileri sürekli dışarı aktarma ilk açıldığında, tüm cihazlar ile tek bir anlık görüntüyü dışarı aktarılır. Her cihaz içerir:
- `id` IOT Central'ın aygıt
- `name` Aygıt
- `deviceId` gelen [cihaz sağlama hizmeti](https://aka.ms/iotcentraldocsdps)
- Cihaz şablon bilgisi
- Özellik değerleri
- Ayar değerleri

Yeni bir anlık görüntü bir kez dakikada yazılır. Anlık görüntü içerir:

- Yeni cihazları son anlık görüntünün beri eklenmiş.
- Değiştirilmiş özellik ve değerlerini beri son anlık görüntünün ayarlama olan cihazlar.

> [!NOTE]
> Cihazlar olmayan son anlık görüntünün dışarı beri silindi. Şu anda anlık görüntüleri göstergeleri silinen cihazlar için yok.
>
> Her bir cihaza ait cihaz şablonu cihaz şablon kimliği ile temsil edilir Cihaz şablonunun adını almak için cihaz şablonu anlık görüntülerin dışarı aktarın.

Kodu çözülen Avro dosyasında bir kaydı gibi görünebilir:

```json
{
    "id": "<id>",
    "name": "Refrigerator 2",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
        "id": "<template id>",
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

Verileri sürekli dışarı aktarma ilk açıldığında, tüm cihaz şablonları ile tek bir anlık görüntüyü dışarı aktarılır. Her cihaz şablonu içerir:
- `id` cihaz şablonu
- `name` cihaz şablonu
- `version` cihaz şablonu
- Ölçüm veri türleri ve en düşük/en yüksek değerleri.
- Özellik veri türleri ve varsayılan değerleri.
- Veri türleri ve varsayılan değerleri ayarlama.

Yeni bir anlık görüntü bir kez dakikada yazılır. Anlık görüntü içerir:

- Son anlık görüntünün bu yana eklenen yeni cihaz şablonları.
- Değiştirilen ölçümleri, özellik ve tanımları beri son anlık görüntünün ayarlama şablonlarıyla cihaz.

> [!NOTE]
> Bu yana son anlık görüntü silinmiş cihaz şablonlarını dışa aktarılmaz. Şu anda anlık görüntüleri göstergeleri silinen cihaz şablonları için yok.

Bir kayıt kodu çözülmüş Avro dosyasında şöyle görünebilir:

```json
{
    "id": "<id>",
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

## <a name="read-exported-avro-files"></a>Verilen Avro dosyaları okuma

Avro bir ikili biçimi olduğundan ham durumlarını dosyaları okunamıyor. Dosyaları JSON biçimine çözülebilir. Aşağıdaki örnekler, ölçümleri, cihazları ve cihaz şablonları Avro dosyalarını ayrıştırmak gösterilmektedir. Örnekler önceki bölümde açıklanan örnekler karşılık gelir.

### <a name="read-avro-files-by-using-python"></a>Python kullanarak avro dosyalarını okuma

#### <a name="install-pandas-and-the-pandavro-package"></a>Pandas ve pandavro paket yükleme

```python
pip install pandas
pip install pandavro
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri Avro dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
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

#### <a name="parse-a-devices-avro-file"></a>Cihazları Avro dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
    # where each record is a single row.
    devices = pdx.from_avro(filePath)

    # This example creates a new DataFrame and loads a series
    # for each column that's mapped into a column in our new DataFrame.
    transformed = pd.DataFrame()

    # The device ID is available in the id column.
    transformed["device_id"] = devices["deviceId"]

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

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları Avro dosyası ayrıştırılamıyor

```python
import json
import pandavro as pdx
import pandas as pd

def parse(filePath):
    # Pandavro loads the Avro file into a pandas DataFrame
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

### <a name="read-avro-files-by-using-c"></a>Okuma Avro dosyalarını kullanarakC#

#### <a name="install-the-microsofthadoopavro-package"></a>Microsoft.Hadoop.Avro paketi yükleyin

```csharp
Install-Package Microsoft.Hadoop.Avro -Version 1.5.6
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri Avro dosyası ayrıştırılamıyor

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
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
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

#### <a name="parse-a-devices-avro-file"></a>Cihazları Avro dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
                foreach (AvroRecord record in reader.Current.Objects)
                {
                    // Get the field value directly. You can also yield return
                    // records and make the function IEnumerable<AvroRecord>.
                    var deviceId = record.GetField<string>("deviceId");

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
                        "Device ID: {0}, Template ID: {1}, Template Version: {2}, Fan Speed: {3}",
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

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları Avro dosyası ayrıştırılamıyor

```csharp
using Microsoft.Hadoop.Avro;
using Microsoft.Hadoop.Avro.Container;

public static async Task Run(string filePath)
{
    using (var fileStream = File.OpenRead(filePath))
    {
        using (var reader = AvroContainer.CreateGenericReader(fileStream))
        {
            // For one Avro container, where a container can contain multiple blocks,
            // loop through each block in the container.
            while (reader.MoveNext())
            {
                // Loop through the Avro records in the block and extract the fields.
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

### <a name="read-avro-files-by-using-javascript"></a>Javascript kullanarak avro dosyalarını okuma

#### <a name="install-the-avsc-package"></a>Avsc paketi yükleyin

```javascript
npm install avsc
```

#### <a name="parse-a-measurements-avro-file"></a>Ölçümleri Avro dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device ID and humidity from each record.
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

#### <a name="parse-a-devices-avro-file"></a>Cihazları Avro dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
// information and the fanSpeed setting for each device record.
async function parse(filePath) {
    const records = await load(filePath);
    for (const record of records) {
        // Fetch the device ID from the deviceId property.
        const deviceId = record.deviceId;

        // Fetch the template ID and version from the deviceTemplate property.
        const deviceTemplateId = record.deviceTemplate.id;
        const deviceTemplateVersion = record.deviceTemplate.version;

        // Get the fanSpeed from the nested device settings property.
        const fanSpeed = record.settings.device.fanSpeed;

        // Log the retrieved device ID and humidity.
        console.log(`deviceID: ${deviceId}, Template ID: ${deviceTemplateId}, Template Version: ${deviceTemplateVersion}, Fan Speed: ${fanSpeed}`);
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

#### <a name="parse-a-device-templates-avro-file"></a>Bir cihaz şablonları Avro dosyası ayrıştırılamıyor

```javascript
const avro = require('avsc');

// Read the Avro file. Parse the device and template identification
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
