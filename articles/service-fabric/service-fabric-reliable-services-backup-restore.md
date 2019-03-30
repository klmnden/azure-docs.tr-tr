---
title: Service Fabric yedekleme ve geri yükleme | Microsoft Docs
description: Service Fabric yedekleme ve geri yükleme için kavramsal belgelerde
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: chackdan
editor: subramar,zhol
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: mcoskun
ms.openlocfilehash: cd40f59cfa7846911c68206c3bc1e85a770b0fcc
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670142"
---
# <a name="backup-and-restore-reliable-services-and-reliable-actors"></a>Yedekleme ve Reliable Services ve Reliable Actors geri yükleme
Azure Service Fabric durum bu yüksek kullanılabilirliği sürdürmek için birden fazla düğümde çoğaltan bir yüksek kullanılabilirlik platformudur.  Bu nedenle, kümedeki bir düğümün başarısız olsa bile, hizmetler kullanılabilir olmaya devam. Bazı durumlarda daha bu yerleşik yedeklilik platform tarafından sağlanan bazı için yeterli olmakla birlikte bile hizmeti (bir dış depoya) verileri yedeklemek tercih edilir.

> [!NOTE]
> Yedekleme verilerinizi geri yükleme (ve beklendiği gibi çalışıp çalışmadığını test etmek için) kritik için veri kaybı senaryolarından kurtarabilirsiniz.
> 

> [!NOTE]
> Microsoft öneriyor kullanılacak [düzenli yedekleme ve geri yükleme](service-fabric-backuprestoreservice-quickstart-azurecluster.md) güvenilir durum bilgisi olan hizmetler ve Reliable Actors veri yedeklemeyi yapılandırmak için. 
> 


Örneğin, bir hizmeti, aşağıdaki senaryolardan birini korumak için yedekleme isteyebilirsiniz:

- Tüm Service Fabric kümesinin kalıcı kaybı durumunda.
- Hizmet bölüm çoğaltmalarını çoğunu kalıcı kaybı
- Yönetim hataları yapabildiği durumu yanlışlıkla silinirse veya bozulursa. Örneğin, yeterli ayrıcalığa sahip bir yönetici hizmet yanlışlıkla silerse, bu sorunla karşılaşabilirsiniz.
- Veri bozulması neden hataları hizmetinde. Örneğin, bozuk verileri güvenilir bir koleksiyona yazma hizmeti kod yükseltmesi başladığında, bu sorunla karşılaşabilirsiniz. Böyle bir durumda, bir önceki durumuna geri döndürülmesi hem kod hem de veri olabilir.
- Çevrimdışı veri işleme. Çevrimdışı verileri üreten hizmetten ayrı olarak gerçekleşen iş zekası için verilerin işlenmesi kullanışlı olabilir.

Yedekleme/Geri Yükleme özelliği güvenilir hizmetler oluşturma ve yedekleri geri yüklemek için API üzerinde derlenmiş hizmetleri sağlar. Platform tarafından sağlanan yedekleme API'leri engelleme okuma veya yazma işlemleri olmadan bir hizmet bölümün durumunun yedekleri izin verin. Geri yükleme, seçilen bir yedekten geri yüklenmesi durumu hizmeti bölümün API'leri sağlar.

## <a name="types-of-backup"></a>Yedekleme türleri
Yedekleme iki seçenek vardır: Tam ve artımlı.
Çoğaltma durumu yeniden oluşturmak için gereken tüm verileri içeren bir yedekleme tam bir yedeklemedir: denetim noktaları ve tüm günlük kayıtları.
Kontrol noktalarına ve günlük olduğundan, kendi kendine tam bir yedekleme geri yüklenebilir.

Kontrol noktalarını büyük olduğunda tam yedeklemeler sorun ortaya çıkar.
Örneğin, durumu 16 GB olan bir çoğaltma ekleme kontrol noktaları olur en fazla yaklaşık 16 GB.
Şu beş dakikalık bir kurtarma noktası hedefi varsa, çoğaltma beş dakikada desteklenmesi gerekir.
50 MB üzerinde ek denetim noktaları 16 GB kopyalamak gereken her zaman bu yedekler (yapılandırılabilir kullanarak `CheckpointThresholdInMB`) tutarında günlükleri.

![Tam yedekleme örneği.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Bu sorunun çözümü artımlı yedeklemeler, burada yedekleme yalnızca değiştirilen günlük kayıtlarını son yedeklemeden bu yana verilmektedir.

![Artımlı yedekleme örneği.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Artımlı yedeklemeler (kontrol noktalarını içermez) son yedeklemeden yalnızca değişiklikler olduğundan, bunlar daha hızlı olma eğilimindedir ancak bunlar, kendi geri yüklenemez.
Artımlı yedekleme geri yüklemek için tüm yedekleme zinciri gereklidir.
Bir yedekleme zinciri yedekleme tam yedekleme ile başlayan bir zinciri ve bitişik artımlı yedeklemelerin bir sayı takip eder.

## <a name="backup-reliable-services"></a>Yedekleme güvenilir hizmetler
Hizmet Yazar yedeklemeleri yapmak ne zaman ve yedeklemeleri depolanacağı tam denetime sahiptir.

Bir yedekleme başlatmak için devralınan üye işlevini çağırmak hizmet gerekiyor `BackupAsync`.  
Yedeklemeler yalnızca birincil çoğaltmalardan yapılabilir ve verilecek yazma durumu duyarlar.

Aşağıda gösterildiği gibi `BackupAsync` alır bir `BackupDescription` burada bir belirtebilirsiniz tam veya artımlı yedekleme yanı sıra, bir geri çağırma işlevi, nesne `Func<< BackupInfo, CancellationToken, Task<bool>>>` yedekleme klasörü yerel olarak oluşturuldu ve bazı taşınabilir hazır olduğunda çağrılır dış depolama.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Artımlı yedekleme almak için istek başarısız olabilir `FabricMissingFullBackupException`. Bu durum, aşağıdaki şeylerden biri gerçekleşmekte olduğunu gösterir:

- Birincil haline gelmiştir olduğundan çoğaltma hiçbir zaman bir tam yedekleme sürdü,
- son yedeklemeden kesildi bu yana günlük kayıtlarını bazıları veya
- iletilen çoğaltma `MaxAccumulatedBackupLogSizeInMB` sınırı.

Kullanıcılar, artımlı yedeklemeler yapılandırarak işaretleyebilmesine olasılığını artırabilir `MinLogSizeInMB` veya `TruncationThresholdFactor`.
Bunlar artan değerleri arttıkça başına çoğaltma disk kullanımı.
Daha fazla bilgi için [güvenilir Hizmetleri Yapılandırması](service-fabric-reliable-services-configuration.md)

`BackupInfo` çalışma zamanı, yedekleme kaydedildiği klasörün konumu dahil olmak üzere yedekleme, ilgili bilgi sağlar (`BackupInfo.Directory`). Geri çağırma işlevi taşıyabilirsiniz `BackupInfo.Directory` harici bir depolama veya başka bir konuma.  Bu işlev, aynı zamanda başarılı bir şekilde yedekleme klasörü hedef konumuna taşımak mümkün olup olmadığını gösteren bir Boole döndürür.

Aşağıdaki kodda nasıl `BackupCallbackAsync` yöntemi, yedekleme Azure Depolama'ya yüklemek için kullanılabilir:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

Önceki örnekte `ExternalBackupStore` kullanılan örnek sınıfı, Azure Blob Depolama ile arabirim oluşturmak için ve `UploadBackupFolderAsync` klasörü sıkıştırır ve Azure Blob deposuna yerleştirir yöntemidir.

Aşağıdakilere dikkat edin:

  - Olabilir yalnızca bir yedekleme işlemi çoğaltma uçuşan herhangi bir zamanda. Birden fazla `BackupAsync` çağrısı aynı anda oluşturur `FabricBackupInProgressException` bir hareket halindeki yedeklemeleri sınırlamak için.
  - Bir yedekleme işlemi devam ederken bir çoğaltma üzerinde başarısız olursa, yedekleme tamamlanmamış. Bu nedenle, yük devretme bittikten sonra çağırarak yedeklemeyi yeniden başlatmak için hizmetin sorumluluğu gelir `BackupAsync` gerektiği şekilde.

## <a name="restore-reliable-services"></a>Güvenilir Hizmetleri geri yükleme
Genel olarak, bir geri yükleme işlemi gerçekleştirmek ihtiyacınız olabilecek durumlar bu kategoriden birine ayrılır:

  - Hizmet bölüm veri kaybı. Örneğin, bir bölüm (birincil çoğaltma dahil) için iki tanesi üç çoğaltmalar için disk bozuk silinebilen veya. Yeni birincil verileri bir yedekten geri yüklemek gerekebilir.
  - Tüm hizmet kaybolur. Örneğin, tüm hizmet ve bu nedenle hizmet yönetici kaldırır ve verilerin geri yüklenmesi gerekir.
  - Hizmet çoğaltılan bozuk uygulama verileri (örneğin, bir uygulama hatası nedeniyle). Bu durumda, hizmet yükseltilebilir veya bozulma nedenini kaldırmak için geri gerekir ve bozuk olmayan veri geri yüklenmesi gerekir.

Birçok yaklaşım mümkün olsa da, bazı örnekler kullanarak sunuyoruz `RestoreAsync` yukarıdaki senaryolarından, kurtarılır.

## <a name="partition-data-loss-in-reliable-services"></a>Reliable Services özelliğinde bölüm veri kaybı
Bu durumda, çalışma zamanı otomatik olarak veri kaybı algılaması ve çağırma `OnDataLossAsync` API.

Kurtarmak için aşağıdakileri gerçekleştirmek hizmet Yazar gerekir:

  - Sanal temel sınıf yöntemini geçersiz kılma `OnDataLossAsync`.
  - En son yedeklemesini hizmetin yedeklemeleri içeren dış konumu bulun.
  - En son yedeği indir (ve bu sıkıştırılmış yedekleme klasörüne yedekleme sıkıştırmasını açın).
  - `OnDataLossAsync` Yöntemi sağlayan bir `RestoreContext`. Çağrı `RestoreAsync` sağlanan API `RestoreContext`.
  - Geri yükleme başarılı olduysa true döndürür.

Örnek aşağıdadır `OnDataLossAsync` yöntemi:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription` için geçirilen `RestoreContext.RestoreAsync` çağrı adlı bir üyeyi içeren `BackupFolderPath`.
Tek bir tam yedekleme, geri yüklerken bu `BackupFolderPath` tam yedekleme içeren klasörü yerel yoluna ayarlanmalıdır.
Tam yedekleme ve artımlı yedeklemeler, bir dizi geri yüklerken `BackupFolderPath` tam yedekleme, ancak aynı zamanda tüm artımlı yedeklemeler yalnızca içeren klasörü yerel yoluna ayarlanmalıdır.
`RestoreAsync` Çağrı throw `FabricMissingFullBackupException` varsa `BackupFolderPath` sağlanan tam yedeği içermiyor.
Ayrıca oluşturabilecek `ArgumentException` varsa `BackupFolderPath` bozuk bir artımlı yedeklemeler zincirine sahiptir.
Örneğin, tam yedekleme öğesini içeriyorsa, ilk artımlı ancak eşleşme sayısı ve üçüncü artımlı yedekleme ikinci artımlı yedekleme.

> [!NOTE]
> RestorePolicy kasaya varsayılan olarak ayarlanır.  Diğer bir deyişle `RestoreAsync` yedekleme klasörü bu yineleme içinde yer alan durumuna eşit veya daha eski bir duruma içerdiğini algılarsa, API ArgumentException ile başarısız olur.  `RestorePolicy.Force` Bu güvenlik denetimi atlamak için kullanılabilir. Bu bir parçası olarak belirtilen `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Silinmiş veya kayıp hizmeti
Hizmet kaldırılırsa, verileri geri yüklenebilmesi için önce öncelikle hizmeti yeniden oluşturmanız gerekir.  Aynı yapılandırmaya sahip, örneğin, verileri sorunsuz bir şekilde geri yüklenebilir, böylece düzeni, bölümleme hizmeti oluşturmak önemlidir.  Verileri geri yüklemek için API, hizmet başladıktan sonra (`OnDataLossAsync` yukarıda) Bu hizmet, her bölümde çağrılması gerekir. Elde biri kullanılarak budur [FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx) her bölüm üzerinde.  

Bu noktadan sonra yukarıdaki senaryo aynı uygulamasıdır. Dış depodan en son ilgili yedeklemeyi geri yüklemek her bölüm gerekir. Bir uyarı, çalışma zamanı dinamik olarak bölüm kimlikleri oluşturduğundan bölüm kimliği artık, değişmiş olabilir emin olur. Bu nedenle, hizmet her bölüm için geri yükleme için en son yedekleme tanımlamak için uygun bölüm bilgileri ve hizmet adını depolamak gerekir.

> [!NOTE]
> Kullanılacak önerilmez `FabricClient.ServiceManager.InvokeDataLossAsync` , küme durumunuz bozabilir olduğundan tüm hizmet geri yüklemek için her bir bölüm.
> 

## <a name="replication-of-corrupt-application-data"></a>Bozuk bir uygulama veri çoğaltma
Yeni dağıtılmış uygulama yükseltmesi bir hata varsa, veri bozulmasına neden olabilir. Örneğin, geçersiz bir alan kodu ile güvenilir bir sözlükte her telefon numarası kaydı güncelleştirmek bir uygulama yükseltmesi başlayabilir.  Bu durumda, Service Fabric depolanan verilerin yapısını farkında olmadığından geçersiz telefon numaralarını çoğaltılır.

Uygulama düzeyinde hizmet dondurma ve mümkünse, hata olmayan uygulama kodu sürümüne yükseltmek için veri bozulması neden böyle bir egregious hatayı algılayan sonra yapılacak ilk şey var.  Ancak, bile hizmet kodunu düzeltildikten sonra veriler hala bozuk olabilir ve bu nedenle veri geri yüklenmesi gerekebilir.  Bu gibi durumlarda, son yedeklemelerin da bozuk olabileceğinden en son yedekleme, geri yüklemek yeterli olmayabilir.  Bu nedenle, veriler bozuk önce yapılan son yedek bulmak sahip.

Hangi yedeklemeleri bozuk olduğundan emin değilseniz, yeni bir Service Fabric kümesi dağıtmayı ve etkilenen bölümleri olduğu gibi yukarıdaki yedeklerini geri "Silinmiş veya kayıp hizmeti" senaryo.  Her bölüm için yedeklemeleri en son geri yüklemeyi başlatmak için en az. Bozulma sahip olmayan bir yedekleme bulduğunuzda, taşıma / (Bu yedekleme) daha yeni olan tüm yedeklemeler bu bölümü silin. Her bölüm için bu işlemi yineleyin. Şimdi, `OnDataLossAsync` çağrılır üretim kümesindeki bir bölüme yukarıdaki işlemin çekilen bir dış depoda son yedeklemeden olacaktır.

Şimdi, adımları "silinmiş veya kayıp hizmet içinde" bölümü hatalı kod durumu bozulmuş önce hizmetinin durumunu durumuna geri yüklemek için kullanılabilir.

Aşağıdakilere dikkat edin:

  - Geri yüklendiğinde, veriler kayboldu önce geri yüklenen yedekleme bölüm durumunu eski bir fırsat yoktur. Bu nedenle, mümkün olduğunca verileri kurtarmak için son çare olarak geri yüklemeniz gerekir.
  - Yedekleme klasörü yolunu ve yedekleme klasörün içindeki dosyaların yollarını temsil eden dize FabricDataRoot yol ve uygulama türü adı'nın uzunluğu 255 karakterden daha uzun olabilir. Bu gibi bazı .NET yöntemleri neden `Directory.Move`, throw `PathTooLongException` özel durum. Bir geçici çözüm olduğu gibi doğrudan kernel32 API'leri çağırmak için `CopyFile`.

## <a name="back-up-and-restore-reliable-actors"></a>Yedekleme ve geri yükleme Reliable Actors


Reliable Actors Framework Reliable Services üzerinde oluşturulmuştur. Actor(s) barındıran ActorService bir durum bilgisi olan güvenilir hizmetidir. Bu nedenle, tüm yedekleme ve geri yükleme işlevlerini güvenilir hizmetlerin kullanılabilir de (durumu sağlayıcısı belirli davranışlarını) için Reliable Actors kullanılabilir. Bölüm başına temelinde yedeklemeler alınmayacak beri tüm aktörler için bölüm yedeklenecek (ve geri yükleme benzer ve bölüm başına temelinde gerçekleşir,) belirtir. Yedekleme/geri yükleme gerçekleştirmek için hizmet sahibi ActorService sınıfından türetilen özel aktör hizmeti sınıfı oluşturun ve ardından yapın yedekleme/geri yükleme yukarıda önceki bölümlerde açıklandığı gibi Reliable Services benzer.

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo)
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Özel aktör hizmeti sınıfı oluşturduğunuzda, aktör kaydı sırasında da kaydetmeniz gerekir.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
    (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

Reliable Actors için varsayılan durum sağlayıcısı `KvsActorStateProvider`. Artımlı yedekleme için varsayılan olarak etkin değil `KvsActorStateProvider`. Oluşturarak, artımlı yedeklemeyi etkinleştirebilirsiniz `KvsActorStateProvider` oluşturucusuna uygun ayar ve aşağıdaki kod parçacığında gösterildiği gibi ActorService oluşturucusuna geçirerek:

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Artımlı yedekleme etkinleştirildikten sonra bir artımlı yedekleme ile FabricMissingFullBackupException aşağıdaki nedenlerden birinden dolayı başarısız olabilir ve artımlı yedekleri almadan önce tam bir yedekleme olması gerekir:

  - Birincil duyurulduğu çoğaltmayı hiçbir zaman bir tam yedekleme duruma getirdi.
  - Son yedeklemeden alındıktan sonra bazı günlük kayıtlarını kesildi.

Artımlı yedekleme etkinleştirildiğinde `KvsActorStateProvider` döngüsel arabellek günlük kayıtlarını yönetmenizi kullanmaz ve düzenli aralıklarla keser. Kullanıcı tarafından 45 dakika boyunca hiçbir yedekleme alınmışsa, sistem otomatik olarak günlük kayıtlarını keser. Bu aralığı belirterek yapılandırılabilir `logTruncationIntervalInMinutes` içinde `KvsActorStateProvider` Oluşturucusu (artımlı yedeklemeyi etkinleştirilirken benzer). Tüm verileri göndererek başka bir çoğaltma oluşturmak birincil çoğaltma gerekiyorsa günlük kayıtlarını da kesilmiş.

Bir yedekleme zinciri geri yükleme yaparken, Reliable Services benzer BackupFolderPath tam yedekleme ve diğer artımlı yedekleri içeren alt dizinleri içeren bir alt ile alt dizinleri içermelidir. Yedekleme zinciri doğrulama başarısız olursa geri yükleme API ile ilgili hata iletisi FabricException durum oluşturur. 

> [!NOTE]
> `KvsActorStateProvider` şu anda ' % s'seçeneği RestorePolicy.Safe yok sayar. Bu özellik için destek önümüzdeki sürümlerden birinde planlanmaktadır.
> 

## <a name="testing-back-up-and-restore"></a>Yedekleme test ve geri yükleme
Kritik verilerin yedeklenmekte ve geri yüklenebileceği emin olmak önemlidir. Bu çağırarak yapılabilir `Start-ServiceFabricPartitionDataLoss` veri kaybı veri yedekleme ve geri yükleme işlevselliğinin hizmetiniz beklenen şekilde çalışması için olup olmadığını test etmek için belirli bir bölüme zorlarsınız PowerShell cmdlet'i.  Program aracılığıyla veri kaybı çağırma ve bu olaydan geri yüklemek mümkündür.

> [!NOTE]
> Yedekleme için bir örnek uygulama bulmak ve geri yükleme işlevselliğinin github'da Web başvuru uygulaması. Lütfen bakmak `Inventory.Service` daha fazla ayrıntı için hizmet.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Başlık altında: yedekleme ve geri yükleme hakkında daha fazla bilgi
Yedekleme ve geri yükleme hakkında daha fazla ayrıntı aşağıdadır.

### <a name="backup"></a>Yedekle
Güvenilir durum Yöneticisi tüm okuma engellemeden tutarlı yedeklemeler oluşturmak ya da yazma işlemleri olanağı sağlar. Bunu yapmak için bir denetim noktası ve günlük Kalıcılık mekanizması kullanır.  Güvenilir durum Yöneticisi kurtarma sürelerini geliştirmeye ve işlem günlük baskısı hafifletmek için belirli noktaları belirsiz (Basit) denetim noktası alır.  Zaman `BackupAsync` , güvenilir durum Yöneticisi tüm güvenilir nesneler bildirir, en son kontrol noktası dosyalarını yerel bir yedekleme klasörüne kopyalamak için çağrılır.  Ardından, güvenilir durum Yöneticisi, "Başlangıç işaretçisinden" yedekleme klasörüne en son günlük kaydı için başlangıç tüm günlük kayıtları kopyalar.  En son günlük kaydının kadar tüm günlük kayıtları yedeklemeye dahildir ve güvenilir durum Yöneticisi yazma tamamlanan günlük korur olduğundan, güvenilir durum Yöneticisi tüm işlemleri, kaydedilmiş olduğunu garanti (`CommitAsync` başarıyla verdi ) yedeklemeye dahil edilir.

Tamamlandıktan sonra herhangi bir işlem `BackupAsync` Mayıs adlı veya yedeklemeye olmayabilir.  Yerel yedekleme klasörü, platform tarafından doldurulmadı sonra (diğer bir deyişle, yerel yedekleme çalışma zamanı tarafından tamamlandığında), hizmetin yedekleme geri çağırma çağrılır.  Yedekleme klasörü Azure depolama gibi bir dış konuma taşımak için bu geri çağırma sorumludur.

### <a name="restore"></a>Geri Yükle
Güvenilir durum Yöneticisi'ni kullanarak bir yedekten geri yükleme yeteneği sağlar `RestoreAsync` API.  
`RestoreAsync` Metodunda `RestoreContext` yalnızca içinde çağrılabilir `OnDataLossAsync` yöntemi.
Tarafından döndürülen bool `OnDataLossAsync` hizmet, bir dış kaynaktan durumuna geri olup olmadığını gösterir.
Varsa `OnDataLossAsync` döndürür true, Service Fabric, diğer tüm çoğaltmaları bu birincil sunucudan yeniden. Service Fabric sağlar alacak çoğaltmaları `OnDataLossAsync` çağrı ilk birincil role geçiş ancak durumu verilen okuma değil ya da durumu yazma.
Bu, StatefulService uygulayıcıları için sunduğu gelir `RunAsync` kadar çağrılmaz `OnDataLossAsync` başarıyla tamamlanır.
Ardından, `OnDataLossAsync` yeni birincil çağrılır.
Hizmet başarıyla (true veya false döndürerek) Bu API tamamlandıktan ve ilgili yeniden yapılandırma tamamlandıktan kadar API teker teker çağrılan tutmak.

`RestoreAsync` ilk kez çağrıldı birincil çoğaltma tüm mevcut durumda bırakır. Daha sonra güvenilir durum Yöneticisi Yedekleme klasörde bulunan tüm güvenilir nesneler oluşturur. Ardından, bunların denetim noktaları yedekleme klasörü içinde geri güvenilir nesneler başlatmamanız. Son olarak, güvenilir durum Yöneticisi yedekleme klasörü içinde günlük kayıtları kendi durumunu kurtarır ve kurtarma işlemini gerçekleştirir. Kurtarma işleminin bir parçası olarak, yedek klasöründe günlük kayıtları taahhüdüne "başlangıç noktasından" başlatma işlemleri güvenilir nesneleri yeniden oynatılır. Bu adım, kurtarılan durumu tutarlı olmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar
  - [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
  - [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  - [Reliable Services bildirimleri](service-fabric-reliable-services-notifications.md)
  - [Reliable Services yapılandırması](service-fabric-reliable-services-configuration.md)
  - [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  - [Azure Service Fabric’te düzenli yedekleme ve geri yükleme](service-fabric-backuprestoreservice-quickstart-azurecluster.md)

