---
title: İçeri aktarma, dışarı aktarma Azure IOT Hub cihaz kimliklerinin | Microsoft Docs
description: İçeri ve dışarı cihaz kimliklerini bir kimlik kayıt defteri üzerinde toplu işlemler gerçekleştirmek için Azure IOT hizmeti SDK'sını kullanma İçeri aktarma işlemleri oluşturma, güncelleştirme ve cihaz kimliklerinin toplu silme olanak sağlar.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/11/2019
ms.author: robinsh
ms.openlocfilehash: 5dd93af7deec2b0c8c90f6a8586de905207ad0a6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65796356"
---
# <a name="import-and-export-iot-hub-device-identities-in-bulk"></a>İçeri ve dışarı aktarma IOT Hub cihaz kimliklerinin toplu

Her IOT hub cihaz başına kaynak hizmeti oluşturmak için kullanabileceğiniz bir kimlik kayıt defteri sahiptir. Kimlik kayıt defteri, cihaz'e yönelik uç noktalarına erişimi denetlemenize olanak sağlar. Bu makalede, alma ve cihaz kimliklerinin toplu gelen bir kimlik kayıt defteri ve dışarı aktarma açıklanır.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

İçeri ve dışarı aktarma işlemlerinin bağlamında yer alıyor *işleri* toplu karşı bir IOT hub'ı hizmet işlemleri yürütmek etkinleştirin.

**RegistryManager** sınıfı içeren **ExportDevicesAsync** ve **ImportDevicesAsync** kullanan yöntemleri **iş** framework. Bu yöntemler, dışarı aktarma, alma ve bir IOT hub kimlik kayıt defteri tamamen eşitleme sağlar.

Bu konuda kullanımını açıklar **RegistryManager** sınıfı ve **işi** toplu içeri ve dışarı aktarmalar cihazların bir IOT hub'ının kimlik kayıt defteri gelen ve giden gerçekleştirmek için sistemi. Azure IOT Hub cihazı sağlama hizmeti, sıfır dokunma, yalnızca bir veya daha fazla IOT hub'lara kullanıcı müdahalesine gerek kalmadan sağlama zamanında etkinleştirmek için de kullanabilirsiniz. Daha fazla bilgi için bkz. [sağlama hizmeti belgeleri](/azure/iot-dps).


## <a name="what-are-jobs"></a>İşleri nelerdir?

Kimlik kayıt defteri işlemlerini kullanmak **iş** sistem olduğunda işlemi:

* Bir uzun yürütme süresi için standart çalışma zamanı operations karşılaştırıldığında.

* Kullanıcıya büyük miktarda veri döndürür.

Bekliyor veya işlem sonucu üzerinde engelleme tek bir API çağrısı yerine, işlemi zaman uyumsuz olarak oluşturur bir **iş** IOT hub'ı. İşlem sonra hemen döndürür bir **JobProperties** nesne.

Aşağıdaki C# kod parçacığı, dışarı aktarma işi oluşturma işlemi gösterilmektedir:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await 
  registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> Kullanılacak **RegistryManager** sınıfı, C# kodunuzda, ekleme **Microsoft.Azure.Devices** NuGet paketini projenize. **RegistryManager** sınıfı **Microsoft.Azure.Devices** ad alanı.

Kullanabileceğiniz **RegistryManager** durumunu sorgulamak için sınıf **iş** döndürülen kullanarak **JobProperties** meta verileri. Bir örneğini oluşturmak için **RegistryManager** sınıfı, kullanın **CreateFromConnectionString** yöntemi.

```csharp
RegistryManager registryManager =
  RegistryManager.CreateFromConnectionString("{your IoT Hub connection string}");
```

IOT hub'ınız için Azure portalında bağlantı dizesi bulmak için:

- IoT Hub'ınıza gidin.

- Seçin **paylaşılan erişim ilkeleri**.

- İhtiyaç duyduğunuz izinleri dikkate alarak, bir ilke seçin.

- Ekranın sağ taraftaki panelden connectionstring kopyalayın.

Aşağıdaki C# kod parçacığı, her beş saniyede bir işin yürütülmesi tamamlandı görmek için yoklama işlemi gösterilmektedir:

```csharp
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="device-importexport-job-limits"></a>Cihaz içeri/dışarı aktarma işi sınırları

Yalnızca 1 etkin cihaz içeri veya dışarı aktarma işi tüm IOT Hub katmanları için aynı anda izin verilir. IOT hub'ı, ayrıca işleri işlemlerinin oranı için sınırlara sahiptir. Daha fazla bilgi için bkz. [başvuru - IOT Hub kotaları ve azaltma](iot-hub-devguide-quotas-throttling.md).

## <a name="export-devices"></a>Cihazlar dışarı aktarma

Kullanım **ExportDevicesAsync** yöntemi, IOT hub kimlik kayıt defterine tamamen dışarı aktarmak için bir [Azure depolama](../storage/index.yml) blob kapsayıcısını kullanan bir [paylaşılan erişim imzası](../storage/common/storage-security-guide.md#data-plane-security).

Bu yöntem, cihaz bilgilerinizi güvenilir yedeklerini, sizin denetlediğiniz bir blob kapsayıcısını oluşturmak sağlar.

**ExportDevicesAsync** yöntem iki parametre gerektirir:

* A *dize* , bir blob kapsayıcısının bir URI içeriyor. Bu URI, kapsayıcıya yazma erişimi veren bir SAS belirteci içermelidir. İş, seri hale getirilmiş dışarı aktarma cihaz verilerini depolamak için bu kapsayıcıda bir blok blobu oluşturur. SAS belirteci, bu izinleri bulunmalıdır:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

* A *Boole* kimlik doğrulama anahtarlarını dışarı aktarma verilerinizden hariç tutmak isteyip istemediğinizi belirtir. Varsa **false**, kimlik doğrulama anahtarlarını dışarı aktarma çıktısında dahil edilir. Aksi takdirde, anahtarlar olarak dışarı aktarılır **null**.

Aşağıdaki C# kod parçacığı sonra tamamlanması için yoklama ve cihaz kimlik doğrulaması anahtarlarını dışarı aktarma veriler içeren dışarı aktarma işi başlatmak gösterir:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
  await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

İş çıktısını ada sahip bir blok blobu olarak belirtilen blob kapsayıcısında yer depolar **devices.txt**. Çıktı verilerini, her satırda bir cihaz ile JSON seri hale getirilmiş cihaz verilerinin oluşur.

Aşağıdaki örnek, çıktı verilerini gösterir:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Bir cihaz ikizi veri varsa ikizi verileri ayrıca cihaz verilerle birlikte verilir. Aşağıdaki örnek, bu biçimi gösterir. "TwinETag" satırın sonuna ikizi veri olana kadar tüm veriler.

```json
{
   "id":"export-6d84f075-0",
   "eTag":"MQ==",
   "status":"enabled",
   "statusReason":"firstUpdate",
   "authentication":null,
   "twinETag":"AAAAAAAAAAI=",
   "tags":{
      "Location":"LivingRoom"
   },
   "properties":{
      "desired":{
         "Thermostat":{
            "Temperature":75.1,
            "Unit":"F"
         },
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
            "$lastUpdatedVersion":2,
            "Thermostat":{
               "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
               "$lastUpdatedVersion":2,
               "Temperature":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               },
               "Unit":{
                  "$lastUpdated":"2017-03-09T18:30:52.3167248Z",
                  "$lastUpdatedVersion":2
               }
            }
         },
         "$version":2
      },
      "reported":{
         "$metadata":{
            "$lastUpdated":"2017-03-09T18:30:51.1309437Z"
         },
         "$version":1
      }
   }
}
```

Bu verilere kod erişmeniz gerekiyorsa, kolayca kullanarak bu verileri seri durumdan çıkarabiliyorsa **ExportImportDevice** sınıfı. Aşağıdaki C# kod parçacığı daha önce bir blok blobuna aktarılmış cihaz bilgilerini okuma işlemini gösterir:

```csharp
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), null, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

## <a name="import-devices"></a>Cihazları İçeri Aktar

**ImportDevicesAsync** yönteminde **RegistryManager** sınıfı bir IOT hub kimlik kayıt defterinde toplu içeri aktarma ve eşitleme işlemleri gerçekleştirmenize olanak sağlar. Gibi **ExportDevicesAsync** yöntemi **ImportDevicesAsync** yöntemi kullanan **iş** framework.

Kullanarak ilgileniriz **ImportDevicesAsync** yöntemi olduğundan, kimlik kayıt defterinde yeni cihazları sağlamaya ek olarak, ayrıca güncelleştirin ve var olan cihazları silin.

> [!WARNING]
> İçeri aktarma işlemi geri alınamaz. Her zaman kullanarak mevcut verilerinizi yedekleyin **ExportDevicesAsync** yöntemi toplu hale getirmeden önce başka bir blob kapsayıcısını, kimlik kayıt defterine değiştirir.

**ImportDevicesAsync** yöntem iki parametre alır:

* A *dize* URI değerini içeren bir [Azure depolama](../storage/index.yml) olarak kullanılacak blob kapsayıcısı *giriş* işe. Bu URI, kapsayıcı okuma erişimi veren bir SAS belirteci içermelidir. Bu kapsayıcı adı ile bir blob içermelidir **devices.txt** , kimlik kayıt defterine içeri aktarmak için seri hale getirilmiş cihaz verilerini içerir. İçeri aktarma verileri aynı cihaz bilgileri içermelidir JSON biçimi **ExportImportDevice** proje oluştururken kullandığı bir **devices.txt** blob. SAS belirteci, bu izinleri bulunmalıdır:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```

* A *dize* URI değerini içeren bir [Azure depolama](https://azure.microsoft.com/documentation/services/storage/) olarak kullanılacak blob kapsayıcısı *çıkış* projeden. Tamamlanan alma işlemi hata bilgileri depolamak için bu kapsayıcıdaki blok blobu işi oluşturur **iş**. SAS belirteci, bu izinleri bulunmalıdır:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read 
     | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> İki parametre için aynı blob kapsayıcısını işaret edebilir. Ayrı parametreleri verilerinizi çıkış kapsayıcısı gerektirdiği ek izinleri hakkında daha fazla denetime etkinleştirmeniz yeterlidir.

Aşağıdaki C# kod parçacığı, içeri aktarma işi başlatmak gösterilmektedir:

```csharp
JobProperties importJob = 
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Bu yöntem, cihaz çiftinin veri almak için de kullanılabilir. Giriş veri biçimini gösterilen biçimi aynıdır **ExportDevicesAsync** bölümü. Bu şekilde, dışarı aktarılan verileri içeri aktarabilirsiniz. **$Metadata** isteğe bağlıdır.

## <a name="import-behavior"></a>Alma davranışı

Kullanabileceğiniz **ImportDevicesAsync** yöntemi, kimlik kayıt defterinde aşağıdaki toplu işlemleri gerçekleştirmek için:

* Yeni cihazların toplu kayıt
* Var olan cihazların toplu silme
* Yığın durumu değiştiğinde (etkinleştirmek veya cihazları devre dışı)
* Yeni cihaz kimlik doğrulaması anahtarlarını toplu atama
* Toplu cihaz kimlik doğrulaması anahtarları, otomatik yeniden oluşturma
* İkiz verilerin toplu güncelleştirme

Tek bir önceki işlemleri herhangi bir birleşimini gerçekleştirebilir **ImportDevicesAsync** çağırın. Örneğin, yeni cihazları kaydetme ve silebilir veya aynı anda var olan cihazları güncelleştirin. İle birlikte kullanıldığında **ExportDevicesAsync** yöntemi, tamamen tüm cihazlarınızı bir IOT hub'ından diğerine geçişini yapabilirsiniz.

İçeri aktarma dosyası çifti meta veri içeriyorsa, bu meta veriler mevcut ikizi meta verileri geçersiz kılar. İçeri aktarma dosyası çifti meta verileri, ardından yalnızca içermez, `lastUpdateTime` geçerli zamanı kullanarak meta verileri güncelleştirilir.

İsteğe bağlı **importMode** içeri aktarma işlemi cihaz başına denetlemek her bir cihaz için seri hale getirme verileri içeri aktar özelliği. **İmportMode** özelliği şu seçeneklere sahiptir:

| importMode | Açıklama |
| --- | --- |
| **createOrUpdate** |Bir cihaz belirtilen mevcut değilse **kimliği**, yeni kaydedilir. <br/>Cihaz zaten varsa var olan bir bilgi olmadan regard için sağlanan giriş verileriyle yazılır **ETag** değeri. <br> Kullanıcı, cihaz verileriyle birlikte ikizi veri isteğe bağlı olarak belirtebilirsiniz. İkizinin etag, belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Bir mevcut ikizinin etag uyuşmazlığı varsa, hata günlük dosyasına yazılır. |
| **oluşturmaya** |Bir cihaz belirtilen mevcut değilse **kimliği**, yeni kaydedilir. <br/>Cihaz zaten varsa, hata günlük dosyasına yazılır. <br> Kullanıcı, cihaz verileriyle birlikte ikizi veri isteğe bağlı olarak belirtebilirsiniz. İkizinin etag, belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Bir mevcut ikizinin etag uyuşmazlığı varsa, hata günlük dosyasına yazılır. |
| **Güncelleştirme** |Bir cihaz zaten belirtilen varsa **kimliği**, var olan bir bilgi olmadan regard için sağlanan giriş verileriyle yazılır **ETag** değeri. <br/>Cihaz mevcut değilse bir hata için günlük dosyasına yazılır. |
| **updateIfMatchETag** |Bir cihaz zaten belirtilen varsa **kimliği**, mevcut bilgi ancak varsa sağlanan giriş verileriyle üzerine bir **ETag** eşleşmesi. <br/>Cihaz mevcut değilse bir hata için günlük dosyasına yazılır. <br/>Varsa bir **ETag** uyuşmazlığı, bir hata için günlük dosyasına yazılır. |
| **createOrUpdateIfMatchETag** |Bir cihaz belirtilen mevcut değilse **kimliği**, yeni kaydedilir. <br/>Cihaz zaten varsa, varsa var olan bilgi ile sağlanan giriş veri yazılır bir **ETag** eşleşmesi. <br/>Varsa bir **ETag** uyuşmazlığı, bir hata için günlük dosyasına yazılır. <br> Kullanıcı, cihaz verileriyle birlikte ikizi veri isteğe bağlı olarak belirtebilirsiniz. İkizinin etag, belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Bir mevcut ikizinin etag uyuşmazlığı varsa, hata günlük dosyasına yazılır. |
| **sil** |Bir cihaz zaten belirtilen varsa **kimliği**, olmadan regard için silinmiş **ETag** değeri. <br/>Cihaz mevcut değilse bir hata için günlük dosyasına yazılır. |
| **deleteIfMatchETag** |Bir cihaz zaten belirtilen varsa **kimliği**, yalnızca silinmiş bir **ETag** eşleşmesi. Cihaz mevcut değilse bir hata için günlük dosyasına yazılır. <br/>ETag uyumsuzluğu varsa, hata günlük dosyasına yazılır. |

> [!NOTE]
> Serileştirme verileri açıkça tanımlamazsa bir **importMode** bayrağı için varsayılan bir cihaz için **createOrUpdate** içeri aktarma işlemi sırasında.

## <a name="import-devices-example--bulk-device-provisioning"></a>Cihazları örnek alma – toplu cihaz sağlama

Aşağıdaki C# kod örneği birden çok cihaz kimliği oluşturma gösterilmektedir:

* Kimlik doğrulama anahtarlarını içerir.
* Bu cihaz bilgileri, bir blok blobuna yazma.
* Cihazları kimlik kayıt defterine alın.

```csharp
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
  // Create a new ExportImportDevice
  // CryptoKeyGenerator is in the Microsoft.Azure.Devices.Common namespace
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to the list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write the list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the blob to add new devices
// Log information related to the job is written to the same container
// This normally takes 1 minute per 100 devices
JobProperties importJob =
   await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>İçeri aktarma cihazları örnek – toplu silme

Aşağıdaki kod örneği, yukarıdaki örnek kod kullanarak eklediğiniz cihazları silme işlemini göstermektedir:

```csharp
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="get-the-container-sas-uri"></a>' % S'kapsayıcı SAS URI'sini Al

Aşağıdaki kod örneği, nasıl oluşturulacağını gösterir bir [SAS URI'sini](../storage/common/storage-dotnet-shared-access-signature-part-1.md) okuma, yazma ve silme izinleri bir blob kapsayıcısı için:

```csharp
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir IOT hub kimlik kayıt defteri üzerinde toplu işlemler gerçekleştirmek öğrendiniz. Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
* [IOT hub'ı günlükleri](iot-hub-monitor-resource-health.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)
* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)

IOT Hub cihazı sağlama hizmeti kullanarak müdahalesi gerektirmeyen, tam zamanında sağlama etkinleştireceğinizi öğrenmek için keşfetmek için: 

* [Azure IoT Hub Cihazı Sağlama Hizmeti](/azure/iot-dps)
