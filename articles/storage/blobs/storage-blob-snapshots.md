---
title: Azure Depolama'da bir blob salt okunur bir görüntüsünü oluşturma | Microsoft Docs
description: Zaman içinde belirli bir anda blob verileri yedeklemek için bir blobun anlık görüntüsünü oluşturmayı öğrenin. Anlık görüntüleri nasıl faturalandırılır ve bunları kapasite ücretleri en aza indirmek için nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2018
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: de600770b35922d35f1d9066489f582aa61aec59
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205082"
---
# <a name="create-a-blob-snapshot"></a>Blob anlık görüntüsü oluşturma

Salt okunur bir sürümünü bir noktada zamanında alınmış bir blobun anlık görüntüsüdür. Anlık görüntüler, BLOB'ları yedekleme için kullanışlıdır. Bir anlık görüntüsünü oluşturduktan sonra okuma, kopyalama veya silin, ancak değişiklik yapamazsınız.

Blob URI'si olan bir blobun anlık görüntüsünü temel kendi blob için aynı bir **DateTime** eklenmiş blob anlık görüntünün alındığı zaman göstermek için URI değeri. Örneğin, bir sayfa URI blob ise `http://storagesample.core.blob.windows.net/mydrives/myvhd`, URI benzer anlık görüntü `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Tüm anlık görüntülerin temel blob URI'si paylaşın. Temel blob ve anlık görüntü arasındaki tek fark eklenmiş olan **DateTime** değeri.
>

Blob anlık görüntüleri herhangi bir sayıda olabilir. Açıkça silinene kadar anlık görüntüleri kalıcı hale getirin. Anlık görüntü, temel bir blob daha uzun sürmesi olamaz. Geçerli anlık görüntüleriniz izlemek için temel blob ile ilişkili anlık görüntüleri sıralayabilirsiniz.

Bir blobun anlık görüntüsünü oluşturduğunuzda, aynı değerlerle anlık görüntüye blobun Sistem özellikleri kopyalanır. Oluşturduğunuzda, anlık görüntü için ayrı bir meta veri belirtmediğiniz sürece temel blobun meta veri anlık görüntüye da kopyalanır.

Temel blob ile ilişkili herhangi bir kira anlık görüntü etkilemez. Bir kira bir anlık görüntü alınamıyor.

Bir VHD dosyasını geçerli bilgileri ve sanal makine diskini için durumu depolamak için kullanılır. VM içinde bir diski kullanımdan çıkarın veya VM'yi kapatın ve ardından, VHD dosyasının anlık görüntüsünü alın. Daha sonra o noktasında VHD dosyasını almak ve VM yeniden bu anlık görüntü dosyası kullanabilirsiniz.

## <a name="create-a-snapshot"></a>Anlık görüntü oluşturma
Aşağıdaki kod örneği kullanarak bir anlık görüntü oluşturma işlemi gösterilmektedir [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/). Bu örnek, oluşturulduğu sırada ek meta veriler anlık görüntü belirtir.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>Anlık görüntü kopyalama
BLOB'ları ve anlık görüntüleri içeren kopyalama işlemleri bu kuralları izleyin:

* Anlık görüntü kendi temel blob kopyalayabilirsiniz. Anlık görüntü temel blob konumuna teşvik ederek, bir blob daha önceki bir sürümünü geri yükleyebilirsiniz. Anlık görüntü kalır, ancak temel blob üzerine yazılabilir bir anlık görüntü kopyası ile yazılır.
* Farklı bir ada sahip bir hedef blob anlık görüntüsünü kopyalayabilirsiniz. Sonuçta elde edilen hedef blob yazılabilir bir blob ve değil bir anlık görüntü ' dir.
* Kaynak blob kopyalandığında, kaynak BLOB anlık görüntüsü için hedef kopyalanmaz. Bir kopya hedef blobun üzerine yazıldığında, özgün hedef blob ile ilişkili tüm anlık görüntüleri değişmeden kalır.
* Bir blok blobun anlık görüntüsünü oluşturduğunuzda, blobun taahhüt Engellenenler listesi anlık görüntüye da kopyalanır. Kaydedilmemiş herhangi bir bloğu kopyalanmaz.

## <a name="specify-an-access-condition"></a>Bir erişim koşulu belirtin
Çağırdığınızda [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], böylece yalnızca bir koşul karşılanıyorsa anlık görüntü oluşturulduktan erişim koşulu belirtebilirsiniz. Bir erişim koşulu belirtmek için kullanın [AccessCondition] [ dotnet_AccessCondition] parametresi. Belirtilen koşulu karşılanmadı, anlık görüntü oluşturulmadı ve Blob hizmeti durum kodu döndürür [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Anlık görüntüleri silin
Anlık görüntüleri de silinene kadar anlık görüntüleri olan nelze odstranit. Tek tek bir anlık görüntüsünü silmek ya da kaynak blobun silindiğinde tüm anlık görüntülerin silinmesi belirtin. Hala anlık görüntülerine sahip bir blob silmeye çalışıyorsanız, bir hata oluşur.

Aşağıdaki kod örneği, bir blob ve. NET'te, anlık görüntüleri silmek gösterilmektedir burada `blockBlob` türünde bir nesnedir [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Azure Premium depolama anlık görüntüleri
Premium depolama ile anlık görüntülerini kullanarak, aşağıdaki kurallar geçerlidir:

* Bir premium depolama hesabında sayfa blobu başına anlık görüntü sayısı 100'dür. Bu sınır aşılırsa, Blob anlık görüntü işlemi hata kodu: 409 döndürür (`SnapshotCountExceeded`).
* Her 10 dakikada bir premium depolama hesabında sayfa blob anlık görüntüsünü alabilir. Hızı aşılıyorsa, Blob anlık görüntü işlemi hata kodu: 409 döndürür (`SnapshotOperationRateExceeded`).
* Anlık görüntü okumak için hesaptaki başka bir sayfa blobu için anlık görüntü kopyalamak için kopyalama Blob işlemi kullanabilirsiniz. Kopyalama işlemi için hedef blob mevcut tüm anlık görüntüleri olmaması gerekir. Hedef blob anlık görüntüleri sahip sonra Blob kopyalama işlemi, hata kodu: 409 döndürür (`SnapshotsPresent`).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Bir mutlak URI bir anlık görüntüye Döndür
Bu C# kod örneği, bir anlık görüntüsünü oluşturur ve birincil konumu olarak bir mutlak URI kullanıma yazar.

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Nasıl anlık görüntüleri ücretler tahakkuk anlama
Bir blob salt okunur bir kopyası olan bir anlık görüntü oluşturma hesabınıza ek veri depolama ücretleri neden olabilir. Uygulamanızı tasarlarken, böylece maliyetleri en aza indirebilirsiniz nasıl bu ücretler tahakkuk dikkat etmeniz önemlidir.

### <a name="important-billing-considerations"></a>Önemli fatura değerlendirmeleri
Aşağıdaki listede, bir anlık görüntü oluştururken dikkate alınması gereken önemli noktaları içerir.

* BLOB veya anlık görüntü olmalarından depolama hesabınız için benzersiz bir blok veya sayfa ücreti alınmaz. Hesabınız, bunlar temel blob güncelleştirilene kadar blob ile ilişkili anlık görüntüler için ek ücrete tabi değildir. Temel blob güncelleştirdikten sonra anlık görüntülerden kareninkinden. Bu durumda, her blob veya anlık görüntü sayfalara ve benzersiz blokları için ücretlendirilirsiniz.
* Bir blok içinde bir blok blobu değiştirdiğiniz zaman, o blok sonradan benzersiz bir blok olarak ücretlendirilir. Anlık görüntüde olduğu gibi blok aynı blok Kimliğini ve aynı verilere sahip olsa bile bu geçerlidir. Blok kaydedildikten sonra yine, kendisine karşılık gelen herhangi bir anlık görüntüdeki gelen kareninkinden ve içerdiği veriler için ücretlendirilirsiniz. Aynı ile aynı verileri güncelleştiren bir sayfa blobu, sayfa için geçerlidir.
* Bir blok blobu çağırarak değiştirerek [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream] [ dotnet_UploadFromStream], veya [UploadFromByteArray] [ dotnet_UploadFromByteArray] yöntemi BLOB tüm blokları değiştirir. Bu blob ile ilişkili bir anlık görüntü varsa, tüm blokların anlık görüntü ve taban blob içinde artık birleştirmek ve her iki blob'larda tüm blokları için ücretlendirilirsiniz. Temel blob ve anlık görüntü verileri aynı kalmasını olsa bile bu geçerlidir.
* Azure Blob hizmetinin iki bloklar aynı verileri içeren olup olmadığını belirlemek için bir yol yok. Aynı veriler ve aynı blok kimliğe sahip olsa bile yüklenmiş ve kaydedilmiş her blok benzersiz olarak kabul edilir Benzersiz blokları için Ücret tahakkuk olduğundan, ek benzersiz engeller ve ek ücretler bir anlık görüntü sonuçları olan bir blob güncelleştirme göz önünde bulundurmanız önemlidir.

### <a name="minimize-cost-with-snapshot-management"></a>Anlık Görüntü Yönetimi ile maliyeti en aza indir

Dikkatli bir şekilde ek maliyetlerden kaçınmak için anlık görüntüleri yönetme öneririz. Tarafından anlık görüntü depolama maliyetleri en aza indirmek için bu en iyi uygulamaları takip edebilirsiniz:

* Silin ve blob güncelleştirdiğinizde ile bir blob anlık görüntüleri tutmak uygulama tasarımınızı gerektirmediği sürece aynı verilerle güncelleştirmekte olduğunuz olsa bile ilişkili anlık görüntüleri yeniden oluşturun. Silme ve blob anlık görüntüleri yeniden oluşturma, anlık görüntüler ve blob değil birleştirmek emin olabilirsiniz.
* Bir blobun anlık görüntülerini muhafaza ediyorsanız çağırmaktan kaçınmanız [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], veya [UploadFromByteArray] [ dotnet_UploadFromByteArray] blob güncelleştirilecek. Bu yöntemler tüm temel blobunuza ve önemli ölçüde ayırmak için anlık görüntüleri neden blob bloklarında değiştirin. Bunun yerine, blokları olası en az sayıda kullanarak güncelleştirme [PutBlock] [ dotnet_PutBlock] ve [PutBlockList] [ dotnet_PutBlockList] yöntemleri.

### <a name="snapshot-billing-scenarios"></a>Anlık görüntü senaryoları faturalama
Nasıl bir blok blobu ve anlık görüntüleri için Ücret tahakkuk aşağıdaki senaryolar gösterilmektedir.

**Senaryo 1**

Anlık görüntü alındıktan sonra yalnızca benzersiz blokları için 1, 2 ve 3 ücretleri uygulanır şekilde Senaryo 1'de, temel blob güncelleştirilmemiş.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Senaryo 2**

2. senaryo, temel blob güncelleştirildi, ancak anlık görüntüye sahip değil. Blok 3 güncelleştirildi ve aynı verileri ve aynı kimliği içeriyor olsa bile, bu anlık görüntüdeki 3 block aynı değil. Sonuç olarak, hesap dört blokları için ücretlendirilir.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Senaryo 3**

3. senaryo, temel blob güncelleştirildi, ancak anlık görüntüye sahip değil. Blok 3, 4 bloğunda temel blob ile değiştirildi, ancak anlık görüntü hala 3 blok yansıtır. Sonuç olarak, hesap dört blokları için ücretlendirilir.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Senaryo 4**

Senaryo 4, temel blob tamamen güncelleştirildi ve kendi özgün blokların hiçbiri içerir. Sonuç olarak, hesap için tüm sekiz benzersiz blokları ücretlendirilir. Gibi bir güncelleştirme yöntemini kullanıyorsanız, bu senaryo gerçekleşebilir [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [ UploadFromStream][dotnet_UploadFromStream], veya [UploadFromByteArray][dotnet_UploadFromByteArray], bu yöntemler tüm bir blobun içeriklerini değiştirin.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makine (VM) disk anlık görüntüleri ile çalışma hakkında daha fazla bilgi bulabilirsiniz [artımlı anlık görüntülerle Azure yönetilmeyen VM disklerini yedekleme](../../virtual-machines/windows/incremental-snapshots.md)

* BLOB depolamayı kullanarak ek kod örnekleri için bkz. [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Örnek uygulamayı indirin ve çalıştırın veya github'da koduna göz atın.

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.generatedblobs.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.uploadfromfile
[dotnet_UploadFromStream]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext