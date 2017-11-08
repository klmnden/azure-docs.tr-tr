---
title: "Service Fabric yedekleme ve geri yükleme | Microsoft Docs"
description: "Service Fabric yedekleme ve geri yükleme için kavramsal belgeler"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,zhol
ms.assetid: 91ea6ca4-cc2a-4155-9823-dcbd0b996349
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/6/2017
ms.author: mcoskun
ms.openlocfilehash: d276ce9233da9137c49faf8c4d975bd1dcf2ff81
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="back-up-and-restore-reliable-services-and-reliable-actors"></a>Ve Reliable Services ve Reliable Actors geri yükleme
Azure Service Fabric durumu bu yüksek kullanılabilirliği sürdürmek için birden çok düğümlere çoğaltır yüksek kullanılabilirlik platformudur.  Kümedeki bir düğümün başarısız olsa bile, bu nedenle, hizmetlerin kullanılabilir olmaya devam edin. Platform tarafından sağlanan bu-yerleşik artıklık bazı için yeterli olabilir, ancak belirli durumlarda, (bir dış depoya) verileri yedeklemek hizmeti için önerilir.

> [!NOTE]
> Yedekleme ve verilerinizi geri (ve beklendiği gibi çalışıp çalışmadığını sınamak için) kritik veri kaybı senaryolarını kurtarabilirsiniz şekilde.
> 
> 

Örneğin, aşağıdaki senaryolardan birini korumak için verileri yedeklemek bir hizmet isteyebilirsiniz:

- Tüm bir Service Fabric kümesi kalıcı kaybolması durumunda.
- Bir hizmet bölüm kopyalarını çoğunu kalıcı kaybı
- Yapabildiği durumu yanlışlıkla silinen bozuk alır veya yönetimsel hatalar. Örneğin, yeterli ayrıcalığa sahip bir yönetici hizmet yanlışlıkla silerse bu ortaya çıkabilir.
- Veri bozulmasına neden hatalar hizmet. Örneğin, bir hizmet kod yükseltme güvenilir bir koleksiyona hatalı veri yazma başlatıldığında bu ortaya çıkabilir. Böyle bir durumda, önceki bir durumuna döndürülmesi hem kod hem de veri olabilir.
- Çevrimdışı veri işleme. Çevrimdışı veri üreten hizmetten ayrı ayrı gerçekleşir iş zekası verileri işlenmesini sağlamak kullanışlı olabilir.

Yedekleme/Geri Yükleme özelliği güvenilir hizmetleri oluşturmak ve yedekleri geri yüklemek için API üzerinde oluşturulmuş hizmetleri sağlar. Platform tarafından sağlanan yedekleme API'ler engelleme okuma veya yazma işlemleri olmadan bir hizmet bölümünün durumunun yedekleri izin verir. Geri yükleme, seçilen bir yedekten geri yüklenmesi bir hizmet bölümünün durumu API'ler izin verir.

## <a name="types-of-backup"></a>Yedekleme türleri
Yedekleme iki seçenek vardır: tam ve artımlı.
Tam yedekleme çoğaltma durumunu yeniden oluşturmak için gereken tüm verileri içeren yedeğidir: denetim noktaları ve tüm günlük kayıtları.
Denetim ve günlük olduğundan, tek başına bir tam yedekleme geri yüklenebilir.

Kontrol noktalarını büyük olduğunda tam yedeklemeler sorun ortaya çıkar.
Örneğin, durumu 16 GB olan bir çoğaltma ekleme kontrol noktaları olacaktır en fazla yaklaşık 16 GB.
Kurtarma noktası hedefi beş dakika sahibiz, çoğaltma beş dakikada yedeklenmesi gerekir.
50 MB yanı sıra denetim noktaları 16 GB kopyalamak gereken yedekler her zaman (yapılandırılabilir kullanarak `CheckpointThresholdInMB`) günlükleri tutarında.

![Tam yedekleme örnek.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Bu sorun için çözüm artımlı yedeklemeler, burada yedekleme yalnızca değiştirilen günlük kayıtları son yedeklemeden verilmektedir.

![Artımlı yedekleme örnek.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Artımlı yedeklemeler yalnızca değişiklikleri (kontrol noktalarını içermez) son yedeklemeden olduğundan, bunlar daha hızlı olma eğilimindedir ancak bunlar, kendi geri yüklenemez.
Artımlı yedekleme geri yüklemek için tüm yedekleme zinciri gereklidir.
Bir yedekleme zinciri, tam yedekleme ile başlayan yedekleme zinciri ve bitişik artımlı yedeklemeler sayısına göre gelmelidir.

## <a name="backup-reliable-services"></a>Yedekleme güvenilir hizmetler
Hizmet Yazar yedeklemeleri yapmak ne zaman ve yedeklemeleri depolanacağı üzerinde tam denetime sahiptir.

Bir yedekleme başlatmak için hizmet devralınan üye işlevi çağırmak gerek duyduğu `BackupAsync`.  
Yedeklemeler yalnızca birincil çoğaltmalardan yapılabilir ve verilebilmesi için yazma durumu gerektirir.

Aşağıda gösterildiği gibi `BackupAsync` alır bir `BackupDescription` nesnesi, burada bir belirtebilirsiniz bir geri çağırma işlevini yanı sıra, bir tam veya artımlı yedekleme, `Func<< BackupInfo, CancellationToken, Task<bool>>>` yedekleme klasörü yerel olarak oluşturulan ve bazı taşınmasını hazır olduğunda çağrılır harici depolama.

```csharp

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Artımlı yedekleme almak için isteği başarısız olabilir `FabricMissingFullBackupException`. Bu durum aşağıdakilerden birini gerçekleşmekte olduğunu gösterir:

- Birincil oldu beri çoğaltma hiçbir zaman bir tam yedekleme sürdü,
- Son yedekleme kesildi bu yana günlük kayıtlarını bazıları veya
- iletilen çoğaltma `MaxAccumulatedBackupLogSizeInMB` sınırı.

Kullanıcıların yapılandırarak artımlı yedeklemeler yapabileceklerinizi olma olasılığını artırmak `MinLogSizeInMB` veya `TruncationThresholdFactor`.
Bunlar artırma artar değerlerini not çoğaltma disk kullanımı başına.
Daha fazla bilgi için bkz: [güvenilir Hizmetleri Yapılandırması](service-fabric-reliable-services-configuration.md)

`BackupInfo`çalışma zamanı yedekleme kaydedildiği klasör konumunu dahil olmak üzere, yedekleme ile ilgili bilgiler sağlar (`BackupInfo.Directory`). Geri çağırma işlevi taşıyabilirsiniz `BackupInfo.Directory` bir dış depolama veya başka bir konum.  Bu işlev, aynı zamanda başarılı bir şekilde yedekleme klasörü hedef konumuna taşımak mümkün olup olmadığını gösteren bir Boole döndürür.

Aşağıdaki kodda nasıl `BackupCallbackAsync` yöntemi, yedekleme Azure depolama alanına yükleme için kullanılabilir:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

Önceki örnekte, `ExternalBackupStore` kullanılan örnek sınıf Azure Blob storage ile arabirimine ve `UploadBackupFolderAsync` klasörü sıkıştırır ve Azure Blob Mağazası'nda yerleştirir yöntemidir.

Şunlara dikkat edin:

  - Olabilir yalnızca bir yedekleme işlemi çoğaltma yürütülen herhangi bir zamanda. Birden fazla `BackupAsync` birer birer çağrısı throw `FabricBackupInProgressException` ınflight yedeklemeleri bir sınırlamak için.
  - Bir yedekleme işlemi devam ederken bir çoğaltma üzerinde başarısız olursa, yedekleme tamamlanmamış. Yük devretme işlemi tamamlandıktan sonra bu nedenle, onu çağırarak yedeklemeyi yeniden başlatmak için hizmetin sorumluluğundadır `BackupAsync` gerektiği.

## <a name="restore-reliable-services"></a>Güvenilir Hizmetleri geri yükleme
Genel olarak, bir geri yükleme işlemi gerçekleştirmeniz gerekebilir zaman durumlarda bu kategoriden ayrılır:

  - Veri kaybı hizmet bölümü. Örneğin, (birincil çoğaltma dahil) bir bölüm için iki üç çoğaltmaları için diski, temizlenmeden bozulmuş veya. Yeni birincil verileri bir yedekten geri yüklemek gerekebilir.
  - Tüm hizmet kaybolur. Örneğin, bir yönetici tüm hizmet ve bu nedenle hizmet kaldırır ve veri geri yüklenmesi gerekiyor.
  - Hizmet bozuk uygulama verileri (örneğin, bir uygulama hatası nedeniyle) çoğaltılan. Bu durumda, hizmet sahip yükseltilmiş veya bozulma nedenini kaldırmak için geri ve geri yüklenmesi bozuk olmayan veri içeriyor.

Birçok yaklaşım mümkün olsa da, kullanarak ilişkin bazı örnekler sunuyoruz `RestoreAsync` yukarıdaki senaryolarından kurtarılır.

## <a name="partition-data-loss-in-reliable-services"></a>Bölüm veri kaybına güvenilir hizmetler
Bu durumda, çalışma zamanı otomatik olarak veri kaybı algılaması ve çağırma `OnDataLossAsync` API.

Aşağıdaki kurtarma gerçekleştirmek hizmet Yazar gerekir:

  - Sanal taban sınıfı yöntemini geçersiz kılın `OnDataLossAsync`.
  - En son yedekleme hizmetin yedeklemeleri içeren dış konumu bulur.
  - En son yedekleme indirin (ve onu sıkıştırılmış yedekleme yedekleme klasörüne sıkıştırmasını açın).
  - `OnDataLossAsync` Yöntemi sağlayan bir `RestoreContext`. Çağrı `RestoreAsync` sağlanan API `RestoreContext`.
  - Geri yükleme işlemi başarılı olursa true döndürür.

Aşağıdaki örnek uygulaması olan `OnDataLossAsync` yöntemi:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription`geçirilen için `RestoreContext.RestoreAsync` çağrı içerir adında bir üyeye `BackupFolderPath`.
Tek bir tam yedekleme geri yüklerken bu `BackupFolderPath` tam yedekleme içeren klasörün yerel yol için ayarlamanız gerekir.
Tam yedekleme ve artımlı yedeklemeler, bir dizi geri yüklerken `BackupFolderPath` tam yedekleme, ancak aynı zamanda tüm artımlı yedeklemeler yalnızca içeren klasörün yerel yol için ayarlamanız gerekir.
`RestoreAsync`Arama throw `FabricMissingFullBackupException` varsa `BackupFolderPath` sağlanan tam yedekleme içermiyor.
Ayrıca atabilirsiniz `ArgumentException` varsa `BackupFolderPath` artımlı yedeklemeler bozuk zincirine sahiptir.
Örneğin, tam yedekleme öğesini içeriyorsa, ilk artımlı ve üçüncü artımlı yedekleme, ancak hiçbir ikinci artımlı yedekleme.

> [!NOTE]
> RestorePolicy kasaya varsayılan olarak ayarlanır.  Bunun anlamı `RestoreAsync` yedekleme klasörü Bu çoğaltma bulunan durumu eşit veya daha eski bir durumu içerir algılarsa, API ArgumentException ile başarısız olur.  `RestorePolicy.Force`Bu güvenlik denetimi atlayacak şekilde kullanılabilir. Bu bir parçası olarak belirtilen `RestoreDescription`.
> 

## <a name="deleted-or-lost-service"></a>Silinen veya kayıp hizmeti
Bir hizmet kaldırılırsa, verileri geri yüklenebilmesi için önce ilk hizmet yeniden oluşturmanız gerekir.  Böylece veriler sorunsuz bir şekilde geri yüklenebilir, bölümleme hizmetiyle aynı yapılandırması, örn., oluşturmak önemlidir.  Verileri geri yüklemek için API, hizmet başladıktan sonra (`OnDataLossAsync` yukarıda) bu hizmetin her bölüme çağrılacak sahiptir. Tek yönlü elde kullanarak bu olup `[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)` her bölüm üzerinde.  

Bu noktadan uygulama yukarıdaki senaryo ile aynıdır. Dış depodan ilgili en son yedeklemeyi geri yüklemek her bölüm gerekir. Bir uyarı çalışma zamanı bölüm kimlikleri dinamik olarak oluşturur sonra bölüm kimliği artık, değişmiş olabilir emin olur. Bu nedenle, her bölüm için geri doğru son yedekleme tanımlamak için uygun bölüm bilgileri ve hizmet adı depolamak hizmet gerekir.

> [!NOTE]
> Kullanılacak önerilmez `FabricClient.ServiceManager.InvokeDataLossAsync` , küme durumunu bozabilir bu yana tüm hizmet geri yüklemek için her bir bölüm.
> 

## <a name="replication-of-corrupt-application-data"></a>Bozuk uygulama verilerinin çoğaltma
Yeni dağıtılan uygulama yükseltmesi bir hata varsa, veri bozulmasına neden. Örneğin, her bir telefon numarası kaydı güvenilir sözlükteki geçersiz bir alan koduyla güncelleştirmek bir uygulama yükseltme başlayabilir.  Bu durumda, Service Fabric depolanıyor veri yapısını farkında olmadığından geçersiz telefon numaralarını çoğaltılır.

Veri bozulması neden olan böyle bir egregious hata algılamak sonra yapmak için ilk uygulama düzeyinde hizmet dondurma ve mümkünse, hatayı yok uygulama kodu sürümüne yükseltmek için şeydir.  Ancak, servis kodu dahi giderildikten sonra verileri hala bozuk olabilir ve bu nedenle veri geri yüklenmesi gerekebilir.  Böyle durumlarda, en son yedekleme de bozuk olabileceğinden son yedeğini geri yüklemek yeterli olmayabilir.  Bu nedenle, veriler bozuk önce yapan son yedekleme bulmak zorunda.

Hangi yedeklemeler bozuk olduğundan emin değilseniz, yeni bir Service Fabric kümesi dağıtma ve etkilenen bölümleri gibi yukarıdaki yedeklerini geri yükleyin "Silinen veya kayıp hizmeti" senaryo.  Her bölüm için yedeklemeleri en son geri yüklemeyi başlatmak için en az. Bozulması sahip olmayan bir yedek bulduktan sonra taşıma / (Yedekleme) daha yeni tüm yedeklemeler bu bölümün silme. Her bölüm için bu işlemi yineleyin. Şimdi, `OnDataLossAsync` olarak adlandırılır üretim kümedeki bölüme yukarıdaki işlem tarafından çekilen bir dış mağazada bulunan son yedekleme olacaktır.

Şimdi, adımları buggy kod durumu bozuk önce hizmetinin durumunu durumuna geri yüklemek için "silinen veya kayıp hizmeti" bölümüne kullanılabilir.

Şunlara dikkat edin:

  - Geri yüklendiğinde veriler kayboldu önce geri yüklenen yedekleme durumunu eski bir fırsat yok. Bu nedenle, mümkün olduğunca verileri kurtarmak için son çare olarak geri yüklemeniz gerekir.
  - Yedekleme klasörü yolunu ve yedekleme klasörü içindeki dosyaları yollarını temsil eden dize bağlı olarak FabricDataRoot yol ve uygulama türü adı'nın uzunluğu 255 karakterden daha büyük olabilir. Bu gibi bazı .NET yöntemleri neden `Directory.Move`throw için `PathTooLongException` özel durum. Bir geçici çözüm olduğu gibi kernel32 API ' larını doğrudan çağırmak için `CopyFile`.

## <a name="backup-and-restore-reliable-actors"></a>Yedekleme ve geri yükleme Reliable Actors


Güvenilir aktörler Framework Reliable Services üzerinde oluşturulmuştur. Actor(s) barındıran ActorService bir durum bilgisi olan güvenilir hizmetidir. Bu nedenle, tüm yedekleme ve geri yükleme işlevselliği güvenilir Hizmetleri'nde kullanılabilir de (durum sağlayıcısı belirli davranışlarını dışında) güvenilir aktörler için kullanılabilir. Yedeklemeler bir bölüm başına temelinde gerçekleştirilecek olduğundan, tüm aktörleri bölüm yedeklenecek (ve geri yükleme benzer ve bölüm başına temelinde gerçekleşir,) belirtir. Yedekleme/geri yükleme gerçekleştirmek için hizmet sahibi ActorService sınıfından türetilen özel aktör hizmeti sınıf oluşturun ve sonra yapın yedekleme/geri yükleme güvenilir yukarıda önceki bölümlerde açıklandığı gibi hizmetler için benzer.

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

Özel aktör hizmet sınıfı oluşturduğunuzda, aktör kaydederken de kaydetmeniz gerekir.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
   (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

Güvenilir aktörler için varsayılan durumu sağlayıcısı `KvsActorStateProvider`. Artımlı yedekleme için varsayılan olarak etkin değil `KvsActorStateProvider`. Artımlı yedekleme oluşturarak etkinleştirebilirsiniz `KvsActorStateProvider` kurucusu uygun ayarı ve aşağıdaki kod parçacığında gösterildiği gibi ActorService oluşturucuya geçirme:

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

Artımlı yedekleme etkinleştirildikten sonra bir artımlı yedekleme yapmayı FabricMissingFullBackupException ile aşağıdaki nedenlerden birinden dolayı başarısız olabilir ve artımlı yedekleri almadan önce tam bir yedekleme gerçekleştirmeniz gerekir:

  - Birincil hale geldi beri çoğaltma hiçbir zaman tam yedekleme sürdü.
  - Son yedekleme alındıktan sonra günlük kayıtlarını bazıları kesildi.

Artımlı yedekleme etkinleştirildiğinde, `KvsActorStateProvider` döngüsel arabellek günlük kayıtlarını yönetmenizi kullanmaz ve düzenli aralıklarla tamsayıya dönüştürür. Yedekleme, kullanıcı tarafından 45 dakika boyunca alınmışsa sistem günlük kayıtları otomatik olarak keser. Bu zaman aralığını belirterek yapılandırılabilir `logTrunctationIntervalInMinutes` içinde `KvsActorStateProvider` Oluşturucusu (artımlı yedekleme etkinleştirirken benzer). Birincil çoğaltma kendi veri göndererek başka bir çoğaltma oluşturmanız gerekiyorsa günlük kayıtlarını da kesilmiş.

Bir yedekleme zinciri geri yükleme yaparken, güvenilir hizmetler benzer BackupFolderPath tam yedekleme ve diğerleri artımlı yedekleri içeren alt dizinleri içeren bir alt alt dizinleri içermelidir. Yedekleme zinciri doğrulama başarısız olursa geri yükleme API ile ilgili hata iletisi FabricException durum oluşturur. 

> [!NOTE]
> `KvsActorStateProvider`şu anda RestorePolicy.Safe seçeneği yok sayar. Bu özellik için destek gelecek bir sürümde planlanmaktadır.
> 

## <a name="testing-backup-and-restore"></a>Yedekleme ve geri yükleme test etme
Kritik verilerin yedeklenmekte ve geri yüklenebileceği emin olmak önemlidir. Bu çağırarak yapılabilir `Start-ServiceFabricPartitionDataLoss` verileri yedekleme ve hizmetinizi beklendiği gibi çalıştığını için işlevselliği geri yükleme olup olmadığını sınamak için belirli bir bölüm veri kaybına anlamına PowerShell cmdlet'ini.  Program aracılığıyla veri kaybı çağırma ve bu olaydan geri yüklemek mümkündür.

> [!NOTE]
> Yedekleme için bir örnek uygulama bulmak ve GitHub Web başvuru uygulamasında işlevindeki geri yükleyin. Lütfen bakmak `Inventory.Service` daha fazla ayrıntı için hizmet.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Başlık altında: yedekleme ve geri yükleme hakkında daha fazla bilgi
Burada, bazı yedekleme ve geri yükleme hakkında daha fazla ayrıntı verilmiştir.

### <a name="backup"></a>Backup
Güvenilir durum Yöneticisi her okuma engellenmeden tutarlı yedeklemeler oluşturmak ya da yazma işlemleri olanağı sağlar. Bunu yapmak için bir denetim noktası ve günlük Kalıcılık mekanizması kullanır.  Güvenilir durum Yöneticisi işlem günlüğündeki baskısı hafifletmek ve kurtarma zamanları geliştirmek için bazı noktalarda belirsiz (Basit) kontrol noktalarını alır.  Zaman `BackupAsync` , güvenilir durum Yöneticisi tüm güvenilir nesneleri bildirir bunların en son denetim noktası dosyaları yerel bir yedekleme klasörüne kopyalamak için çağrılır.  Ardından, güvenilir durum Yöneticisi "Başlangıç işaretçi" yedekleme klasörüne en son günlük kaydı başlangıç tüm günlük kayıtları kopyalar.  En son günlük kaydının kadar tüm günlük kayıtları yedeklemeye dahil edilir ve güvenilir durum Yöneticisi yazma tamamlanan günlük korur olduğundan, tüm işlemler, kaydedilmiş olduğunu güvenilir durum Yöneticisi garanti eder (`CommitAsync` başarıyla verdi ) yedeklemeye dahil edilir.

Sonra uygulayan herhangi bir işlem `BackupAsync` Mayıs adlı veya yedekleme olmayabilir.  Yerel yedekleme klasörü platform tarafından doldurulmuş sonra (yani, yerel yedekleme çalışma zamanı tarafından tamamlandığını), hizmetin yedekleme geri çağırma çağrılır.  Yedekleme klasörü Azure depolama gibi harici bir konuma taşımak için bu geri çağırma sorumludur.

### <a name="restore"></a>Geri Yükleme
Güvenilir durum Yöneticisi'ni kullanarak bir yedekten geri yükleme yeteneği sağlar `RestoreAsync` API.  
`RestoreAsync` Yöntemi `RestoreContext` içinde yalnızca adlı `OnDataLossAsync` yöntemi.
Tarafından döndürülen bool `OnDataLossAsync` hizmet bir dış kaynaktan durumuna geri olup olmadığını gösterir.
Varsa `OnDataLossAsync` döndürür true, Service Fabric yeniden oluşturma bu birincil diğer tüm çoğaltmalardan. Service Fabric sağlar alacak çoğaltmaları `OnDataLossAsync` çağrısı birincil role ilk geçiş ancak durumu verilen okuma değil ya da durum yazma.
Bu, StatefulService uygulayıcılar için gelir `RunAsync` kadar çağrılmaz `OnDataLossAsync` başarıyla tamamlanır.
Ardından, `OnDataLossAsync` yeni birincil çağrılır.
Bir hizmet başarıyla (true veya false döndürerek) Bu API tamamlandıktan ve ilgili yeniden yapılandırma tamamlanana kadar API birer birer çağrılan tutmak.

`RestoreAsync`ilk kez çağrıldı birincil çoğaltma tüm mevcut durumda bırakır.  
Daha sonra güvenilir durum Yöneticisi Yedekleme klasörde bulunan tüm güvenilir nesneler oluşturur.  
Ardından, güvenilir nesneler yedekleme klasörü bunların denetim noktaları geri başlatmamanız.  
Son olarak, güvenilir durum Yöneticisi Yedekleme klasöründeki günlük kayıtlarından kendi durumuna kurtarır ve kurtarma işlemini gerçekleştirir.  
Kurtarma işleminin bir parçası olarak, tamamlama günlük kayıtlarını yedekleme klasörünüz "başlangıç noktasından" Başlangıç işlemleri güvenilir nesnelere yeniden oynatılır.  
Bu adım, kurtarılan durumu tutarlı olmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar
  - [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
  - [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  - [Güvenilir hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
  - [Güvenilir Hizmetleri Yapılandırması](service-fabric-reliable-services-configuration.md)
  - [Güvenilir koleksiyonlar için Geliştirici Başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

