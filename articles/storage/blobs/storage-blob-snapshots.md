---
title: "Azure Storage'da bir BLOB salt okunur anlık görüntü oluşturma | Microsoft Docs"
description: "Zaman içinde belirli bir anda blob verileri yedeklemek için bir blobun bir anlık görüntü oluşturmayı öğrenin. Anlık görüntüler nasıl faturalandırılır ve bunların kapasite ücretleri en aza indirmek için nasıl kullanılacağını anlayın."
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 04/11/2017
ms.author: tamram
ms.openlocfilehash: cba28ada79ea806ead4ae9165abba2dc4e04f001
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="create-a-blob-snapshot"></a>Blob anlık görüntüsü oluşturma

Bir anlık görüntüsü, bir noktada geçen süre içinde bir blob, salt okunur bir sürümüdür. Anlık görüntüler, BLOB'ları yedekleme için kullanışlıdır. Bir anlık görüntü oluşturduktan sonra okuma, kopyalama veya silin, ancak değişiklik yapamazsınız.

Blob URI'si olan bir anlık görüntü bir BLOB kendi temel blob için aynıdır bir **DateTime** anlık görüntünün alındığı zaman belirtmek için URI blob eklenmiş değeri. Örneğin, URI bir sayfa blob ise `http://storagesample.core.blob.windows.net/mydrives/myvhd`, URI benzer anlık görüntü `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Tüm anlık görüntüleri temel blob'un URI paylaşır. Temel blob ve anlık görüntü arasındaki tek fark eklenmiş olan **DateTime** değeri.
>

Bir blob herhangi bir sayıda anlık görüntü olabilir. Açıkça silinene kadar anlık görüntüleri kalıcı olmasını sağlar. Bir anlık görüntü, temel blob outlive olamaz. Geçerli anlık izlemek için temel blob ile ilişkili anlık görüntüleri sıralayabilirsiniz.

Bir blob görüntüsünü oluşturduğunuzda, blob'un Sistem özellikleri aynı değerleri anlık görüntü kopyalanır. Oluşturduğunuzda anlık görüntü için ayrı meta verileri belirtmediğiniz sürece temel blob'un meta veriler ayrıca anlık görüntüye kopyalanır.

Temel blob ile ilişkili kiraları anlık görüntü etkilemez. Bir anlık görüntü üzerinde bir kira edinemez.

Bir VHD dosyasının VM diskinin durumu ve geçerli bilgi depolamak için kullanılır. VM dahilinde bir diski kullanımdan çıkarın veya VM kapatma ve sonra VHD dosyasını bir anlık görüntüsünü. Daha sonra zamandaki o noktada VHD dosyasını alın ve VM yeniden oluşturmak için bu anlık görüntü dosyasını kullanabilirsiniz.

Depolama hizmeti şifreleme (SSE) blob bulunduğu için depolama hesabına etkinleştirilmişse bu blob geçen tüm anlık görüntüleri bekleyen şifrelenir.

## <a name="create-a-snapshot"></a>Bir anlık görüntü oluşturma
Aşağıdaki kod örneği kullanarak bir anlık görüntü oluşturmak gösterilmiştir [.NET için Azure Storage istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/). Oluşturulduğunda bu örnek anlık görüntü için ek meta veri belirtir.

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
BLOB'ları ve anlık görüntüleri içeren kopyalama işlemleri bu kurallar izleyin:

* Bir anlık görüntü, temel blob kopyalayabilirsiniz. Anlık görüntü temel blob konumuna yükselterek bir blob önceki bir sürümünü geri yükleyebilirsiniz. Anlık görüntü kalır, ancak temel blob üzerine yazılabilir bir anlık görüntü kopyası ile yazılır.
* Hedef blob farklı ada sahip bir anlık görüntü kopyalayabilirsiniz. Sonuçta elde edilen hedef blob yazılabilir bir blob ve anlık görüntü olmayan ' dir.
* Kaynak blob kopyalandığında, tüm anlık görüntüleri kaynak BLOB hedefe kopyalanmaz. Hedef blob bir kopya ile yazılır, özgün hedef blob ile ilişkili tüm anlık görüntüleri değişmeden kalır.
* Bir blok blobu görüntüsünü oluşturduğunuzda, blob'un taahhüt engelleme listesi da anlık görüntüye kopyalanır. Herhangi bir kaydedilmeyen bloğu kopyalanmaz.

## <a name="specify-an-access-condition"></a>Bir erişim koşulu belirtin
Çağırdığınızda [CreateSnapshotAsync][dotnet_CreateSnapshotAsync], böylece yalnızca bir koşul karşılandığında anlık görüntü oluşturulduğunda bir erişim koşulu belirtebilirsiniz. Bir erişim koşulu belirtmek için kullanın [AccessCondition] [ dotnet_AccessCondition] parametresi. Belirtilen koşulu karşılanmadı, anlık görüntü oluşturulmadı ve Blob hizmeti durum kodunu döndüren [HTTPStatusCode][dotnet_HTTPStatusCode]. PreconditionFailed.

## <a name="delete-snapshots"></a>Anlık görüntüleri silin
Anlık görüntüler aynı zamanda silinene kadar anlık görüntüleri içeren bir blobu silemezsiniz. Tek tek bir anlık görüntüyü silmek ya da kaynak blob silindiğinde tüm anlık görüntülerin silinmesi belirtin. Hala anlık görüntülere sahip bir blobu silmek çalışırsanız, bir hata oluşur.

Aşağıdaki kod örneği, bir blob ve .NET, kendi anlık görüntüleri silmek gösterilmiştir nerede `blockBlob` türünde bir nesne [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Azure Premium Storage ile anlık görüntüler
Anlık görüntü Premium Storage ile kullanırken, aşağıdaki kurallar geçerlidir:

* Maksimum sayıda anlık görüntü premium depolama hesabındaki bir sayfa blobu başına 100'dür. Bu sınır aşılırsa, anlık görüntü Blob işlem hata kodu 409 döndürür (`SnapshotCountExceeded`).
* Premium depolama hesabı her 10 dakikada bir sayfa blob'u görüntüsünü alabilir. Hızı aşılırsa, anlık görüntü Blob işlem hata kodu 409 döndürür (`SnapshotOperationRateExceeded`).
* Bir anlık görüntü okumak için başka bir sayfa blobu hesabındaki bir anlık görüntü kopyalamak için Kopyala Blob işlemi kullanabilirsiniz. Kopyalama işleminin hedef blob mevcut tüm anlık görüntüleri olmaması gerekir. Hedef blob anlık görüntüleri sahip sonra Blob kopyalama işlemi hata kodunu 409 döndürür (`SnapshotsPresent`).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Bir anlık görüntü mutlak URI dön
Bu C# kod örneği, bir anlık görüntü oluşturur ve birincil konumu için mutlak URI çıkışı yazar.

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
Bir blob salt okunur bir kopyası olan, bir anlık görüntü oluşturma hesabınıza ek veri depolama ücretlere neden olabilir. Uygulamanızı tasarlarken, böylece maliyetleri en aza indirebilirsiniz nasıl bu ücretler tahakkuk haberdar olmanız önemlidir.

### <a name="important-billing-considerations"></a>Fatura ile ilgili önemli noktalar
Aşağıdaki listede, bir anlık görüntü oluştururken dikkate alınması gereken önemli noktaları içerir.

* Blob veya anlık görüntü olup depolama hesabınızın benzersiz blokları veya sayfalar için ücret doğurur. Hesabınız, bunlar temel alır blob güncelleştirilene kadar blob ile ilişkili anlık görüntüler için ek ücretler uygulanır değil. Temel blob güncelleştirdikten sonra anlık görüntülerden kareninkinden. Bu gerçekleştiğinde, benzersiz blokları veya her bir blob veya anlık görüntü sayfalarında için ücretlendirilirsiniz.
* Bir blok blobu blokta değiştirdiğinizde, bu blok sonradan benzersiz bloğu olarak ücretlendirilir. Anlık görüntüdeki olduğu gibi blok aynı blok Kimliğini ve aynı verilere sahip olsa bile bu geçerlidir. Blok kaydedildikten sonra yine, kendisine karşılık gelen hiçbir anlık görüntüdeki gelen kareninkinden ve verileri için sizden ücret alınır. Aynı aynı verilerle güncelleştirilir bir sayfa blob'u içinde bir sayfa için geçerlidir.
* Bir blok blobu çağırarak değiştirme [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], veya [UploadFromByteArray] [ dotnet_UploadFromByteArray] yöntemi blob tüm bloklarında değiştirir. Bu blob ile ilişkili bir anlık görüntü varsa, tüm blokların anlık görüntü ve temel blob şimdi ayırmak ve hem BLOB'ları tüm bloklarında için ücretlendirilir. Temel blob ve anlık görüntü verileri aynı kalır olsa bile bu geçerlidir.
* Azure Blob hizmetine iki blokları özdeş veriler içeren olup olmadığını belirlemek için bir yol yok. Aynı veri ve aynı blok kimliğe sahip olsa bile karşıya ve kaydedilen her bloğu benzersiz olarak kabul edilir Benzersiz blokları için Ücret tahakkuk olduğundan, ek benzersiz blokları ve ek ücretlere anlık görüntü sonuçlarında sahip bir blob güncelleştirme dikkate almak önemlidir.

### <a name="minimize-cost-with-snapshot-management"></a>Anlık Görüntü Yönetimi ile maliyeti en aza indir

Dikkatli bir şekilde ek ücretlerden kaçınmak için anlık görüntüleri yönetme öneririz. Anlık görüntü Depolama tarafından maliyetleri en aza indirmek için bu en iyi uygulamaları takip edebilirsiniz:

* Silin ve blob güncelleştirdiğinizde Uygulama tasarımınız anlık görüntüleri tutmak gerektirmedikçe aynı verilerle güncelleştirdiğiniz olsa bile bir blob ile ilişkili anlık görüntüleri yeniden oluşturun. Silme ve blob'un anlık görüntü yeniden oluşturma, anlık görüntüler ve blob değil ayırmak emin olabilirsiniz.
* Bir blob için anlık görüntü bakımı, arama kaçının. [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], veya [UploadFromByteArray] [ dotnet_UploadFromByteArray] blob güncelleştirmek için. Bu yöntemler tüm temel blob ve önemli ölçüde ayırmak için anlık görüntüleri neden blob bloklarında değiştirin. Bunun yerine, bloğu en az olası sayısı kullanarak güncelleştirme [PutBlock] [ dotnet_PutBlock] ve [PutBlockList] [ dotnet_PutBlockList] yöntemleri.

### <a name="snapshot-billing-scenarios"></a>Anlık görüntü senaryoları faturalama
Bir blok blobu ve onun anlık görüntüleri için nasıl ücretler tahakkuk aşağıdaki senaryolar gösterilmektedir.

**Senaryo 1**

Anlık görüntü alındıktan sonra yalnızca benzersiz blokları için 1, 2 ve 3 ücretler tahakkuk eden şekilde Senaryo 1'de, temel blob güncelleştirilmemiş.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

**Senaryo 2**

2. senaryo, temel blob güncelleştirildi ancak anlık görüntüye sahip değil. Blok 3 güncelleştirildi ve aynı veri ve aynı kimliği içeriyor olsa bile, onu anlık görüntüdeki 3 engelleme aynı değil. Sonuç olarak, hesap için dört blokları doludur.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

**Senaryo 3**

Senaryo 3, temel blob güncelleştirildi ancak anlık görüntüye sahip değil. Blok 3 temel blob 4 bloğunda ile değiştirildi, ancak anlık görüntü hala Blok 3 yansıtır. Sonuç olarak, hesap için dört blokları doludur.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

**Senaryo 4**

Senaryo 4, temel blob tamamen güncelleştirildi ve kendi özgün blokları hiçbiri içerir. Sonuç olarak, hesap için tüm sekiz benzersiz blokları doludur. Bir güncelleştirme yöntemi gibi kullanıyorsanız, bu senaryo ortaya çıkabilir [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream], veya [UploadFromByteArray][dotnet_UploadFromByteArray], bu yöntemler tüm blob içeriğinin değiştirin.

![Azure Storage kaynakları](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makine (VM) disk anlık görüntüleri ile çalışma hakkında daha fazla bilgi bulabilirsiniz [artımlı anlık görüntüleri ile Azure yönetilmeyen VM diskleri yedekleme](../../virtual-machines/windows/incremental-snapshots.md)

* BLOB storage kullanarak ek kod örnekleri için bkz: [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Örnek uygulamayı indirin ve çalıştırın veya github'daki kod göz atın.

[dotnet_AccessCondition]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.accesscondition.aspx
[dotnet_CloudBlockBlob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx
[dotnet_CreateSnapshotAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.createsnapshotasync.aspx
[dotnet_HTTPStatusCode]: https://msdn.microsoft.com/library/system.net.httpstatuscode(v=vs.110).aspx
[dotnet_PutBlockList]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblocklist.aspx
[dotnet_PutBlock]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.putblock.aspx
[dotnet_UploadFromByteArray]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfrombytearray.aspx
[dotnet_UploadFromFile]: https://msdn.microsoft.com/library/azure/mt705654.aspx
[dotnet_UploadFromStream]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstream.aspx
[dotnet_UploadText]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.uploadtext.aspx