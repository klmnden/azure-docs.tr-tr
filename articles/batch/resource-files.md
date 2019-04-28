---
title: Kaynak dosyaları - Azure Batch oluşturma ve kullanma | Microsoft Docs
description: Azure Batch kaynak dosyaları çeşitli giriş kaynaklarından oluşturmayı öğrenin.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 03/14/2019
ms.author: lahugh
ms.openlocfilehash: 679a1c60e44694bde86cafba21d7f1d2c6fb94d9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60616560"
---
# <a name="creating-and-using-resource-files"></a>Kaynak dosyaları oluşturma ve kullanma

Bir Azure Batch görevinde, genellikle işlemek için verilerin bazı formlarıyla gerektirir. Kaynak dosyalar, Batch sanal makinenize (VM) aracılığıyla bir görev bu verilerini sağlamak için araçlarıdır. Kaynak dosyaları tüm tür görevleri destekler: görevler, başlangıç görevleri, iş hazırlama görevleri, iş sürüm görevleri vb. Bu makale, birkaç ortak kaynak dosyaları oluşturma ve bunları bir VM yerleştirmek yöntemleri kapsar.  

Kaynak dosyalar toplu bir VM'de oturum verileri yerleştirmek için bir mekanizma, ancak veri türünü ve nasıl kullanıldığını esnektir. Ancak, bazı ortak kullanım örnekleri vardır:

1. Başlangıç görevinin kaynak dosyalarını kullanarak her VM'de ortak dosyaları sağlama
1. Görevler tarafından işlenmek üzere giriş verilerini sağlayın

Ortak dosyaları, örneğin, görevlerinizin çalıştıracağı uygulamaları yükleme için kullanılan bir başlangıç görevi dosyalarda olabilir. Giriş verileri ham görüntü veya video veri ya da Batch tarafından işlenecek herhangi bir bilgi olabilir.

## <a name="types-of-resource-files"></a>Kaynak dosyaları türleri

Kaynak dosyaları oluşturmak kullanılabilen birkaç farklı seçenek vardır. Kaynak dosyaları oluşturma işlemi, özgün verilerin depolandığı bağlı olarak değişir.

Bir kaynak dosyası oluşturmak için seçenekleri:

- [Depolama kapsayıcısı URL'si](#storage-container-url): Azure'da herhangi bir depolama kapsayıcısına bir kaynak dosyası oluşturur
- [Depolama kapsayıcısı adı](#storage-container-name): Batch için bağlı bir Azure depolama hesabında bir kapsayıcı adı bir kaynak dosyası oluşturur
- [Web uç noktası](#web-endpoint): Bir kaynak dosyası geçerli bir HTTP URL oluşturur.

### <a name="storage-container-url"></a>Depolama kapsayıcısı URL'si

Bir depolama kapsayıcısı URL'si kullanarak tüm Azure depolama kapsayıcısında dosyalara erişebilir anlamına gelir. Doğru izinlere sahip

Bu C# örnek, dosyaları için blob depolama alanı olarak Azure depolama kapsayıcısı zaten yüklenmiş. Bir kaynak dosyası oluşturmak için gerekli verilere erişmek için önce depolama kapsayıcısına erişmek ihtiyacımız var.

Paylaşılan erişim imzası (SAS) depolama kapsayıcısına erişmek için doğru izinlere sahip URI oluşturun. Sona erme zamanını ve SAS izinlerini ayarlayın. SAS hemen geçerli olur ve iki saat üretildikten sonra süresi dolar. Bu nedenle bu durumda, hiçbir başlangıç zamanı, belirtilir.

```csharp
SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
{
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
    Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List
};
```

> [!NOTE]
> Kapsayıcı erişimi için her ikisi de olmalıdır `Read` ve `List` izinler, yalnızca blob erişimle gerekir ancak `Read` izni.

İzinleri yapılandırıldıktan sonra SAS belirteci oluşturma ve erişim depolama kapsayıcısı için SAS URL'sini biçimlendirin. Depolama kapsayıcısı için biçimlendirilmiş SAS URL'sini kullanarak, bir kaynak dosyası oluştur [ `FromStorageContainerUrl` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.resourcefile.fromstoragecontainerurl?view=azure-dotnet).

```csharp
CloudBlobContainer container = blobClient.GetContainerReference(containerName);

string sasToken = container.GetSharedAccessSignature(sasConstraints);
string containerSasUrl = String.Format("{0}{1}", container.Uri, sasToken);

ResourceFile inputFile = ResourceFile.FromStorageContainerUrl(containerSasUrl);
```

Bir SAS URL'si oluşturmak için alternatif bir kapsayıcı ve bloblarını Azure Blob Depolama alanında anonim, genel okuma erişimi etkinleştirmektir. Bunu yaptığınızda, hesap anahtarınız paylaşımı ve bir SAS gerek olmadan bu kaynaklara salt okunur erişim verebilirsiniz. Genel okuma erişimini, genellikle belirli bloblar her zaman için anonim okuma erişimi kullanılabilir olmasını istediğiniz senaryolar için kullanılır. Bu senaryo, çözümünüzün uygun değilse [bloblar için anonim erişim](../storage/blobs/storage-manage-access-to-resources.md) makale blob verilerinize erişimi yönetme hakkında daha fazla bilgi edinmek için.

### <a name="storage-container-name"></a>Depolama kapsayıcısı adı

Yapılandırma ve bir SAS URL'si oluşturmak yerine, blob verilerinize erişmek için Azure depolama kapsayıcısının adını kullanabilirsiniz. Depolama kapsayıcısı gereksinimlerine autostorage hesabı olarak da bilinir, Batch hesabınıza bağlı Azure depolama hesabı kullanılır. Autostorage hesabınız depolama kapsayıcı adını kullanarak bir depolama kapsayıcısına erişmek için bir SAS URL'si oluşturma ve yapılandırma atlamanızı sağlar.

Bu örnekte, kaynak dosyası oluşturmak için kullanılacak veri zaten bir Azure depolama hesabını Batch hesabınıza bağlı olduğunu varsayıyoruz. Adımları autostorage hesabınız yoksa bkz [Batch hesabı oluşturma](batch-account-create-portal.md) oluşturmak ve bir hesabı bağlamak hakkında ayrıntılar için.

Bağlı depolama hesabı'nı kullanarak oluşturun ve bir depolama kapsayıcısı için SAS URL yapılandırma gerekmez. Bunun yerine, bağlantılı depolama hesabınızda depolama kapsayıcısını adını sağlayın.

```csharp
ResourceFile inputFile = ResourceFile.FromAutoStorageContainer(containerName);
```

### <a name="web-endpoint"></a>Web uç noktası

Azure depolama alanına yüklenir olmayan verileri, kaynak dosyaları oluşturmak için hala kullanılabilir. Geçerli bir HTTP URL'si, girdi verilerini içeren belirtebilirsiniz. URL ve Batch API'sini için sağlanır ve sonra veri kaynak dosyası oluşturmak için kullanılır.

Aşağıdaki C# örnek, giriş verilerinin kurgusal GitHub uç noktasında barındırılır. API, dosya geçerli web uç noktasından alır ve göreviniz tarafından kullanılacak bir kaynak dosyası oluşturur. Bu senaryo için gerekli kimlik bilgileri yok.

```csharp
ResourceFile inputFile = ResourceFile.FromUrl("https://github.com/foo/file.txt", filePath);
```

## <a name="tips-and-suggestions"></a>İpuçları ve öneriler

Her Azure Batch görevinde dosyaları, Batch görevleri dosyalarını yönetmek için kullanılabilir seçenekleri sahip farklı şekilde kullanır. Aşağıdaki senaryolarda, kapsamlı, ancak bunun yerine birkaç yaygın durumlar kapsar ve öneriler sağlamak için kuruluşunuz değildir.

### <a name="many-resource-files"></a>Birçok kaynak dosyaları

Batch işinizi tüm ortak dosyalar kullanmak çeşitli görevleri içerebilir. Birçok görevler arasında paylaşılan ortak görev dosyaları, kaynak dosyalar yerine dosyaları içeren bir uygulama paketi kullanarak daha iyi bir seçenek olabilir. Uygulama paketleri indirme hızını iyileştirmesini sağlar. Ayrıca, uygulama paketleri veri görevler arasında önbelleğe alınan görev dosyalarınızı genellikle değiştirmezseniz, uygulama paketleri, çözümünüz için uygun olabilir. Uygulama paketleri ile el ile birden çok kaynak dosyalarını yönetme ya da Azure depolamadaki dosyalara erişmek için SAS URL'lerini oluşturmak gerekmez. Batch, depolamak ve uygulama paketleriyle işlem düğümlerine dağıtmak amacıyla Azure Storage ile arka planda çalışır.

Her görev, görev için benzersiz olan çok sayıda dosya varsa, kaynak dosyaları çoğunlukla büyük olasılıkla en iyi seçenektir. Uygulama paketleri içerikle yapmak kadar kolay değil güncelleştirilmesi veya değiştirilmesi genellikle benzersiz dosyaları kullanan görevler gerekir. Kaynak dosyaları güncelleştirme, ekleme veya tek tek dosyaları düzenleme için ek esneklik sağlar.

### <a name="number-of-resource-files-per-task"></a>Görev başına kaynak dosya sayısı

Görev için belirtilen birkaç yüz kaynak dosyası varsa, Batch, çok büyük olması için görev reddedebilir. Kaynak dosyalarını görev sayısını en aza indirerek görevlerinizi küçük tutmak en iyisidir.

Hiçbir yolu ise göreviniz dosyalarının sayısını en aza indirmek için gereken, görevin kaynak dosyalarının bir depolama kapsayıcısı başvuruda tek bir kaynak dosyası oluşturarak iyileştirebilirsiniz. Bunu yapmak için kaynak dosyalarını bir Azure depolama kapsayıcısının içine yerleştirin ve kaynak dosyalarının farklı "Container" modu kullanın. Blob ön eki seçenekleri koleksiyonları görevleriniz için yüklenecek dosyaları belirtmek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [uygulama paketleri](batch-application-packages.md) alternatif olarak kaynak dosyaları.
- Kapsayıcılar için kaynak dosyalarını kullanma hakkında daha fazla bilgi için bkz. [kapsayıcı iş yüklerinin](batch-docker-container-workloads.md).
- Toplayın ve görevlerinizi çıktı verilerini kaydetmek öğrenmek için bkz: [iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md).
- Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.