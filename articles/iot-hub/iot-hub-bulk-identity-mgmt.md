---
title: "Dışarı aktarma Azure IOT Hub cihaz kimlikleri alma | Microsoft Docs"
description: "İçeri ve dışarı cihaz kimliklerini kimlik kayıt defteri karşı toplu işlemleri gerçekleştirmek için Azure IOT hizmeti SDK'sını kullanma İçeri aktarma işlemleri oluşturma, güncelleştirme ve cihaz kimliklerini toplu silme olanak sağlar."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: 699237c68258243b5f654f5dc57e616e3a22177a
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>IOT Hub cihaz kimliklerinizi toplu yönetme

Her IOT hub kimlik kayıt defteri hizmeti aygıt başına kaynakları oluşturmak için kullanabilirsiniz sahiptir. Kimlik kayıt defteri aygıt'e yönelik uç noktalar için erişim denetim sağlar. Bu makalede, alabilir ve toplu cihaz kimliklerini bir kimlik kayıt defterinden verin açıklar.

İçeri ve dışarı aktarma işlemleri sürer bağlamında yerinde *işleri* IOT hub'ı karşı toplu hizmet işlemlerini yürütmek etkinleştirin.

**RegistryManager** sınıfı içerir **ExportDevicesAsync** ve **ImportDevicesAsync** kullanan yöntemleri **iş** framework. Bu yöntemler, verme, almak ve bir IOT hub kimlik kayıt defteri tamamen eşitlemek etkinleştirin.

Bu konuda ele alınmıştır kullanarak **RegistryManager** sınıfı ve **iş** toplu içeri ve dışarı aktarmalar için ve bir IOT hub'ın kimlik kayıt defterinden aygıtların gerçekleştirmek için sistem. Azure IOT Hub cihaz sağlama hizmeti, sıfır-touch, yalnızca insan etkileşimi olmadan bir veya daha fazla IOT hub'ları için sağlama zaman etkinleştirmek için de kullanabilirsiniz. Daha fazla bilgi için bkz: [hizmet belgeleri sağlama][lnk-dps].

## <a name="what-are-jobs"></a>İşlerini nelerdir?

Kimlik kayıt defteri işlemlerini kullanmak **iş** sistem olduğunda işlemi:

* Potansiyel olarak uzun yürütme zaman standart çalışma zamanı işlemleri karşılaştırıldığında.
* Büyük miktarda veri kullanıcıya döndürür.

İşlemi bekliyor veya işlem sonucu üzerinde engelleme tek bir API çağrısı yerine, zaman uyumsuz olarak oluşturur bir **iş** bu IOT hub'ın. İşlemi sonra hemen döndürür bir **JobProperties** nesnesi.

Aşağıdaki C# kod parçacığını bir dışarı aktarma işinin oluşturulacağını gösterir:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> Kullanılacak **RegistryManager** sınıfı, C# kodunda, ekleme **Microsoft.Azure.Devices** NuGet paketini projenize. **RegistryManager** sınıfı olan **Microsoft.Azure.Devices** ad alanı.

Kullanabileceğiniz **RegistryManager** durumunu sorgulamak için sınıf **iş** döndürülen kullanarak **JobProperties** meta verileri. Örneği oluşturmak için **RegistryManager** sınıfı, kullanın **CreateFromConnectionString** yöntemi:

```csharp
RegistryManager registryManager = RegistryManager.CreateFromConnectionString("{your IoT Hub connection string}");
```

Bağlantı dizesi, IOT hub ' ınızı Azure Portalı'nda bulmak için:

- IOT hub'ına gidin.
- Seçin **paylaşılan erişim ilkeleri**.
- Gereksinim duyduğunuz izinleri dikkate alarak, bir ilke seçin.
- Ekranın sağ taraftaki panelinden connectionstring kopyalayın.

Aşağıdaki C# kod parçacığını işin yürütülmesi tamamlandı görmek için beş saniyede yoklamak gösterilmektedir:

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

## <a name="export-devices"></a>Dışarı aktarma cihazları

Kullanım **ExportDevicesAsync** yöntemi bir IOT hub kimlik kayıt defterine tamamen dışarı aktarmak için bir [Azure Storage](../storage/index.yml) blob kapsayıcısı kullanarak bir [paylaşılan erişim imzası](../storage/common/storage-security-guide.md#data-plane-security).

Bu yöntem, sizin denetlediğiniz bir blob kapsayıcısında cihaz bilgilerinizi güvenilir yedeklerini oluşturmanıza olanak sağlar.

**ExportDevicesAsync** yöntemi iki parametre gerektirir:

* A *dize* blob kapsayıcısının bir URI içeriyor. Bu URI, kapsayıcıya yazma erişimi veren bir SAS belirteci içermesi gerekir. İş bir blok blobu serileştirilmiş verme aygıt verilerini depolamak için bu kapsayıcıda oluşturur. SAS belirteci bu izinleri şunları içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

* A *boolean* kimlik doğrulaması anahtarları, dışa aktarma verilerinin dışında tutmak isteyip istemediğinizi belirtir. Varsa **yanlış**, kimlik doğrulama anahtarları dışa aktarma çıktısında dahil edilir. Aksi takdirde, olarak anahtarları dışarı **null**.

Aşağıdaki C# kod parçacığını verme verilerde cihaz kimlik doğrulaması anahtarları içeren bir dışarı aktarma işini başlatmak nasıl gösterir ve tamamlanması için yoklama:

```csharp
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

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

İş çıktısı ada sahip bir blok blobu olarak sağlanan blob kapsayıcısında depolar **devices.txt**. Çıktı verileri JSON seri hale getirilmiş aygıt verileri, satır başına bir aygıtla oluşur.

Aşağıdaki örnek, çıktı verilerini gösterir:

```json
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Bir cihaz çifti veri varsa, twin verileri de aygıt verilerini birlikte verilir. Aşağıdaki örnekte bu biçimi gösterir. "TwinETag" satırın sonuna kadar tüm veriler twin verilerdir.

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

Kodda bu verilere erişmesi gerekiyorsa, kolayca kullanarak bu verileri seri durumdan çıkarabiliyorsa **ExportImportDevice** sınıfı. Aşağıdaki C# kod parçacığında, daha önce bir blok blobuna aktarılmış aygıt bilgileri okumak gösterilmektedir:

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

> [!NOTE]
> Aynı zamanda **GetDevicesAsync** yöntemi **RegistryManager** , cihazların bir listesini almak için sınıf. Ancak, bu yaklaşım döndürülen cihaz nesnelerinin sayısı 1000 sabit sınır vardır. Beklenen kullanım durumu için **GetDevicesAsync** yöntemi hata ayıklama yardımcı olmak geliştirme senaryoları için ve üretim iş yükleri için önerilmez.

## <a name="import-devices"></a>Cihazları içeri aktarma

**ImportDevicesAsync** yönteminde **RegistryManager** sınıfı bir IOT hub kimlik kayıt defterinde toplu içeri aktarma ve eşitleme işlemleri gerçekleştirmenizi sağlar. Gibi **ExportDevicesAsync** yöntemi, **ImportDevicesAsync** yöntemi kullanan **iş** framework.

Kullanarak ilgilenebilmek **ImportDevicesAsync** yöntemi kimlik kayıt defterinizde yeni cihazları sağlamaya ek olarak, ayrıca güncelleştirme ve var olan cihazları silmek için.

> [!WARNING]
> İçeri aktarma işlemi geri alınamaz. Her zaman kullanarak var olan verileri yedekleme **ExportDevicesAsync** yöntemi toplu yapmadan önce başka bir blob kapsayıcısını, kimlik kayıt defterine değiştirir.

**ImportDevicesAsync** yöntemi iki parametre alır:

* A *dize* bir URI'sini içeren bir [Azure Storage](../storage/index.yml) olarak kullanılacak blob kapsayıcısı *giriş* işi. Bu URI, kapsayıcı için okuma erişimi veren bir SAS belirteci içermesi gerekir. Bu kapsayıcı ada sahip bir blob içermelidir **devices.txt** kimlik kayıt defterine alın serileştirilmiş cihaz verileri içerir. İçeri aktarma verileri aynı aygıt bilgileri içermelidir JSON biçiminde **ExportImportDevice** iş kullanır oluştururken bir **devices.txt** blob. SAS belirteci bu izinleri şunları içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Read
   ```
* A *dize* bir URI'sini içeren bir [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) olarak kullanılacak blob kapsayıcısı *çıkış* işten. Bu kapsayıcıdaki tüm Tamamlanan alma işlemi hata bilgileri depolamak için bir blok blobu işi oluşturur **iş**. SAS belirteci bu izinleri şunları içermelidir:

   ```csharp
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> İki parametre blob kapsayıcıya işaret edebilir. Ayrı parametreleri yalnızca verilerinizi üzerinde çıkış kapsayıcı ek izinler gerektirdiğinden daha fazla denetim sağlar.

Aşağıdaki C# kod parçacığında, bir içeri aktarma işlemini başlatmak gösterilmektedir:

```csharp
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

Bu yöntem, cihaz çiftinin veri almak için de kullanılabilir. Giriş veri biçimi gösterilen biçim aynıdır **ExportDevicesAsync** bölümü. Bu şekilde, dışarı aktarılan verileri yeniden alın. **$Metadata** isteğe bağlıdır.

## <a name="import-behavior"></a>Alma davranışı

Kullanabileceğiniz **ImportDevicesAsync** yöntemi, kimlik kayıt defterinde aşağıdaki toplu işlemleri gerçekleştirmek için:

* Yeni cihazların toplu kayıt
* Var olan cihazların toplu silme
* Toplu durum değişikliklerini (etkinleştirmek veya aygıtları devre dışı)
* Yeni cihaz kimlik doğrulama anahtarlarının toplu atama
* Otomatik yeniden üretme cihaz kimlik doğrulama anahtarlarının toplu
* Toplu güncelleştirme twin veri

Tek bir önceki işlemleri herhangi bir birleşimini gerçekleştirebilir **ImportDevicesAsync** çağırın. Örneğin, yeni cihazları kaydetmek ve silebilir veya aynı anda var olan cihazları güncelleştirin. İle birlikte kullanıldığında **ExportDevicesAsync** yöntemi, tamamen tüm aygıtlar bir IOT hub'ından diğerine geçirebilirsiniz.

İçeri aktarma dosyası twin meta veri içeriyorsa, bu meta veriler varolan twin meta verileri üzerine yazar. İçeri aktarma dosyası twin meta veriler, ardından yalnızca içermez varsa `lastUpdateTime` meta veriler, geçerli saati kullanılarak güncelleştirilir.

İsteğe bağlı kullanmak **importMode** içeri aktarma işlemi cihaz başına denetlemek için her cihaz için seri hale getirme veri içeri aktar özelliği. **İmportMode** özelliği aşağıdaki seçenekler vardır:

| importMode | Açıklama |
| --- | --- |
| **createOrUpdate** |Bir aygıt ile belirtilen yoksa **kimliği**, yeni kayıtlı. <br/>Cihaz zaten varsa, varolan bilgileri olmadan regard için sağlanan giriş verilerle üzerine yazılır **ETag** değeri. <br> Kullanıcı, cihaz verileri birlikte twin verileri isteğe bağlı olarak belirtebilirsiniz. Twin'ın etag belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Varolan twin'ın etag ile uyumsuzluğa ise, bir hata günlük dosyasına yazılır. |
| **oluşturmaya** |Bir aygıt ile belirtilen yoksa **kimliği**, yeni kayıtlı. <br/>Cihaz zaten varsa, bir hata günlük dosyasına yazılır. <br> Kullanıcı, cihaz verileri birlikte twin verileri isteğe bağlı olarak belirtebilirsiniz. Twin'ın etag belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Varolan twin'ın etag ile uyumsuzluğa ise, bir hata günlük dosyasına yazılır. |
| **Güncelleştirme** |Bir aygıt belirtilen ile zaten varsa **kimliği**, varolan bilgileri olmadan regard için sağlanan giriş verisi olarak üzerine **ETag** değeri. <br/>Cihaz yok, hata günlük dosyasına yazılır. |
| **updateIfMatchETag** |Bir aygıt belirtilen ile zaten varsa **kimliği**, varolan bilgileri yalnızca varsa sağlanan girdi verileriyle üzerine bir **ETag** eşleşmesi. <br/>Cihaz yok, hata günlük dosyasına yazılır. <br/>Varsa bir **ETag** uyuşmazlığı, hata günlük dosyasına yazılır. |
| **createOrUpdateIfMatchETag** |Bir aygıt ile belirtilen yoksa **kimliği**, yeni kayıtlı. <br/>Cihaz zaten varsa, yalnızca varsa mevcut bilgileri sağlanan girdi verileriyle yazılır bir **ETag** eşleşmesi. <br/>Varsa bir **ETag** uyuşmazlığı, hata günlük dosyasına yazılır. <br> Kullanıcı, cihaz verileri birlikte twin verileri isteğe bağlı olarak belirtebilirsiniz. Twin'ın etag belirtilmişse, cihazın etag'den bağımsız olarak işlenir. Varolan twin'ın etag ile uyumsuzluğa ise, bir hata günlük dosyasına yazılır. |
| **sil** |Bir aygıt belirtilen ile zaten varsa **kimliği**, olmadan regard için silinmiş **ETag** değeri. <br/>Cihaz yok, hata günlük dosyasına yazılır. |
| **deleteIfMatchETag** |Bir aygıt belirtilen ile zaten varsa **kimliği**, yalnızca varsa silinmiş bir **ETag** eşleşmesi. Cihaz yok, hata günlük dosyasına yazılır. <br/>Bir ETag uyuşmazlığı ise, bir hata günlük dosyasına yazılır. |

> [!NOTE]
> Serileştirme verileri açıkça tanımlamaz varsa bir **importMode** bayrağı için varsayılan bir aygıt için **createOrUpdate** içeri aktarma işlemi sırasında.

## <a name="import-devices-example--bulk-device-provisioning"></a>Aygıtları örnek alma – cihaz sağlamayı toplu

Aşağıdaki C# kod örneği birden çok aygıt kimlikleri oluşturmak nasıl gösterir:

* Kimlik doğrulama anahtarlarını içerir.
* Bu cihaz bilgileri bir blok blobuna yazma.
* Cihaz kimlik kayıt defterine alın.

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
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

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

## <a name="import-devices-example--bulk-deletion"></a>İçeri aktarma aygıtları örnek – toplu silme

Aşağıdaki kod örneği, yukarıdaki örnek kod kullanarak eklediğiniz cihazları silmek nasıl gösterir:

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

## <a name="get-the-container-sas-uri"></a>Kapsayıcı SAS URI'sini Al

Aşağıdaki kod örneği, nasıl oluşturulacağını gösterir bir [SAS URI'sini](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md) okuma, yazma ve blob kapsayıcısı için izinleri Sil:

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

Bu makalede, bir IOT hub kimlik kayıt defteri karşı toplu işlemler gerçekleştirme öğrendiniz. Azure IOT hub'ı yönetme hakkında daha fazla bilgi için bu bağlantıları izleyin:

* [IOT hub'ı ölçümleri][lnk-metrics]
* [İzleme işlemleri][lnk-monitor]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

IOT Hub cihaz sağlama hizmeti kullanarak zero touch, yalnızca zaman sağlama etkinleştirmek için bkz: keşfetmek için: 

* [Azure IOT Hub cihaz hizmet sağlama][lnk-dps]


[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dps]: https://azure.microsoft.com/documentation/services/iot-dps
