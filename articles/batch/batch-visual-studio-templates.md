---
title: Visual Studio şablonları - Azure Batch ile çözümler geliştirin | Microsoft Docs
description: Visual Studio Proje şablonları uygulamak ve Azure Batch'te işlem yoğunluklu iş yüklerinizi çalıştırmak nasıl yardımcı olabileceğini öğrenin.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 085bfa582b676f34a02e4c1c5ae7e69c49e5cb4e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60550095"
---
# <a name="use-visual-studio-project-templates-to-jump-start-batch-solutions"></a>Batch çözümleri oluşturma sürecini hızlandıracak biçimde Visual Studio Proje şablonları kullanın.

**İş Yöneticisi** ve **görev işlemci Visual Studio şablonları** toplu işlemi için uygulama ve en az çaba ile toplu işlem yoğunluklu iş yüklerinizi çalıştırmak için yardımcı olması için kod sağlayın. Bu belge, bu şablonları açıklar ve bunların nasıl kullanılacağını yönelik yönergeler sağlanmaktadır.

> [!IMPORTANT]
> Bu makalede, yalnızca bu iki şablonları için geçerli olan bilgiler ele alınmaktadır ve Batch hizmeti ve onunla ilgili temel kavramları hakkında bilgi sahibi olduğunuzu varsayar: havuzlar, işlem düğümleri, işler ve görevler, iş yöneticisi görevleri, ortam değişkenleri ve diğer ilgili bilgiler. Daha fazla bilgi bulabilirsiniz [Azure Batch temel bilgileri](batch-technical-overview.md) ve [Batch özelliğine genel bakış geliştiriciler için](batch-api-basics.md).
> 
> 

## <a name="high-level-overview"></a>Yüksek düzey genel bakış
İş Yöneticisi ve görev işlemci şablonları, iki yararlı bileşenler oluşturmak için kullanılabilir:

* Bir işi paralel olarak birbirinden bağımsız olarak çalışabilen birden çok görevlere bölebilirsiniz bir işi ayırıcı uygulayan bir iş yöneticisi görevi.
* Ön işleme ve bir uygulama komut satırı son işlemi gerçekleştirmek için kullanılabilir bir görev işlemci.

Bir film oluşturma senaryosunda, işi Bölümlendirici tek film iş yüzlerce veya binlerce tek çerçeve ayrı ayrı işlem ayrı görevler ör. Karşılık olarak, görev işlemci işleme uygulaması çağırır ve her işlemek için gereken tüm bağımlı işlemleri çerçeve yanı sıra (örneğin, bir depolama konumuna kareyi kopyalama) ek eylemleri gerçekleştirin.

> [!NOTE]
> İş Yöneticisi ve görev işlemci şablonları birbirinden bağımsız olduğundan hem veya yalnızca bir tanesi gereksinimlerine işlem işinizin ve tercihlerinize bağlı olarak kullanmayı seçebilirsiniz.
> 
> 

Aşağıdaki diyagramda gösterildiği gibi bu şablonları kullanan bir işlem işi üç aşamadan geçer:

1. İstemci kodu (örneğin, uygulama, web hizmeti, vb.), bir iş belirten, iş yöneticisi görevi gibi iş Yöneticisi programı azure'da Batch hizmetine gönderir.
2. Batch hizmeti iş yöneticisi görevi, bir işlem düğümünde çalışır ve birçok gerektiği gibi iş Bölümlendirici koddaki özellikleri ve parametreleri temel bilgi işlem düğümü gibi iş Bölümlendirici üzerinde belirtilen sayıda görev işlemci görevleri başlatır.
3. Görev işlemci görevler bağımsız olarak, girdi verilerini işlemek, çıktı verileri üretmek için paralel olarak çalıştırın.

![İstemci kodu Batch hizmeti ile nasıl etkileştiğini gösteren diyagram][diagram01]

## <a name="prerequisites"></a>Önkoşullar
Batch şablonlarını kullanmak için aşağıdakiler gerekir:

* Visual Studio 2015 yüklü bir bilgisayar. Batch şablonlarını şu anda yalnızca Visual Studio 2015 için desteklenir.
* Kullanılabilir Batch şablonlarını [Visual Studio Galerisi] [ vs_gallery] Visual Studio uzantıları olarak. Şablonları almak için iki yolu vardır:
  
  * Şablonları kullanarak yükleme **Uzantılar ve güncelleştirmeler** Visual Studio iletişim kutusunda (daha fazla bilgi için [bulma ve Visual Studio uzantılarını kullanarak][vs_find_use_ext]). İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusunda, aramak ve aşağıdaki iki uzantısını indirin:
    
    * Azure Batch İş Yöneticisi işi Ayırıcılı
    * Azure Batch görevi işlemci
  * Şablonları Visual Studio için çevrimiçi galeriden indirin: [Microsoft Azure Batch proje şablonları][vs_gallery_templates]
* Kullanmayı planlıyorsanız [uygulama paketleri](batch-application-packages.md) İş Yöneticisi'ni dağıtmak için özellik ve görev işlemci Batch işlem düğümleri, bir depolama hesabını Batch hesabınıza bağlamak gerekir.

## <a name="preparation"></a>Hazırlık
Bu, İş Yöneticisi ve görev işlemci programlar arasında kod paylaşma daha kolay zorlaştırabilir çünkü görev işlemciniz yanı sıra, İş Yöneticisi içerebilir bir çözüm oluşturmanızı öneririz. Bu çözümü oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio açıp seçin **dosya** > **yeni** > **proje**.
2. Altında **şablonları**, genişletme **diğer proje türleri**, tıklayın **Visual Studio çözümleri**ve ardından **boş çözüm**.
3. Uygulamanız ve bu çözüm (örneğin, "LitwareBatchTaskPrograms") amacını açıklayan bir ad yazın.
4. Yeni çözümü oluşturmak için tıklayın **Tamam**.

## <a name="job-manager-template"></a>İş Yöneticisi şablonu
İş Yöneticisi şablonu aşağıdaki eylemleri gerçekleştirebileceği bir iş yöneticisi görevi uygulamak için yardımcı olur:

* Bir iş birden çok görevlere bölün.
* Toplu olarak çalıştırmak için bu görevleri gönderin.

> [!NOTE]
> İş Yöneticisi görevleri hakkında daha fazla bilgi için bkz: [Batch özelliğine genel bakış geliştiriciler için](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-the-template"></a>Şablon kullanarak bir iş yöneticisi oluşturma
Bir iş yöneticisi, daha önce oluşturduğunuz çözüme eklemek için aşağıdaki adımları izleyin:

1. Var olan çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde çözüme sağ tıklayın, **Ekle** > **yeni proje**.
3. Altında **Visual C#** , tıklayın **bulut**ve ardından **Azure Batch İş Yöneticisi işi Ayırıcılı**.
4. Uygulamanızı açıklayan ve bu projeyi İş Yöneticisi (örn.) tanımlayan bir ad yazın "LitwareJobManager").
5. Projeyi oluşturmak için tıklayın **Tamam**.
6. Son olarak, tüm başvurulan NuGet paketlerini yükleme ve değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için Visual Studio zorlamak için projeyi derleyin.

### <a name="job-manager-template-files-and-their-purpose"></a>İş Yöneticisi şablon dosyaları ve amaçları
İş Yöneticisi şablonunu kullanarak bir proje oluşturduğunuzda, kod dosyası üç grupları oluşturur:

* Ana program dosyasına (Program.cs). Bu programın giriş noktası ve en üst düzey özel durum işlemeyi içerir. Normalde bunu değiştirmek gerekmez.
* Framework dizin. Bu parametreleri, görev eklemeyi bir Batch işi, vb. açmak İş Yöneticisi program – 'ortak' çalışmanın sorumlu dosyaları içerir. Normalde bu dosyalarda değişiklik gerekmez.
* İş bölme dosyası (JobSplitter.cs). Bir işi görevlere bölmek için uygulamaya özgü mantığınızı yerleştireceğiniz budur.

Elbette iş mantığı bölme karmaşıklığına bağlı olarak, proje Bölümlendirici kodu desteklemek için gerektiği gibi ek dosyalar ekleyebilirsiniz.

Şablonu ayrıca bir .csproj dosyasını, app.config, packages.config, vb. gibi standart .NET proje dosyalarını oluşturur.

Bu bölümün geri kalanında farklı dosyalar ve kendi kod yapısını ve her sınıf yaptığı açıklanmaktadır.

![Visual Studio Çözüm Gezgini'nde İş Yöneticisi şablonu çözümü gösteriliyor][solution_explorer01]

**Framework dosyaları**

* `Configuration.cs`: Batch hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametresi gibi iş yapılandırma verilerini yüklenmesini kapsüller. Ayrıca, (bkz. Batch belgelerinde görevler için ortam ayarları) Batch tarafından tanımlanan ortam değişkenleri Configuration.EnvironmentVariable sınıfı aracılığıyla erişim sağlar.
* `IConfiguration.cs`: Birim test, proje ayırıcı sahte veya sahte yapılandırma nesnesi kullanarak yapabilirsiniz yapılandırma sınıfın uygulaması soyutlar.
* `JobManager.cs`: İş Yöneticisi programı bileşenlerinin düzenler. İş bölme çağırma ve görevler için görevi göndereni iş Bölümlendirici tarafından döndürülen gönderme iş Bölümlendirici başlatmak için sorumludur.
* `JobManagerException.cs`: Sonlandırmak İş Yöneticisi gerektiren bir hatayı temsil eder. JobManagerException 'beklenen' hataları belirli tanılama bilgileri sonlandırma bir parçası olarak nerede sağlanabilir sarmalamak için kullanılır.
* `TaskSubmitter.cs`: Bu sınıf için toplu iş Bölümlendirici tarafından döndürülen görevlerin eklenmesi sorumludur. JobManager sınıfı toplamlar etkin ancak zamanında ek olarak, iş için toplu görev dizisine ardından çağırır TaskSubmitter.SubmitTasks arka plan iş parçacığında için her toplu işin.

**İş bölme**

`JobSplitter.cs`: Bu sınıf, iş görevlere bölmek için uygulamaya özgü mantığı içerir. Framework yöntem bunları döndürür gibi işe ekler ve görev sırasını edinme JobSplitter.Split yöntemi çağırır. Bu iş mantığını burada ekleyecektir sınıftır. İş bölüm istediğiniz görevleri temsil eden CloudTask örneklerinin bir dizisini döndürmek için Split yöntemini uygulayın.

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Programın giriş noktası ve en üst düzey özel durum işlemeyi içerir.

### <a name="implementing-the-job-splitter"></a>İş bölme uygulama
İş Yöneticisi şablonu projesi açtığınızda, projenin JobSplitter.cs dosyanın varsayılan olarak açık olacaktır. Aşağıdaki Split() yöntemi Göster'i kullanarak iş yükünüzü görevleri bölünmüş mantığını uygulayabilirsiniz:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

> [!NOTE]
> Ek açıklamalı bölümünde `Split()` mantığı ekleyerek değiştirmenizi işlerinizi farklı görevlere bölmek için kullanılmaya İş Yöneticisi şablonu kod yalnızca bölümünün yöntemidir. Şablonu farklı bir bölümünü değiştirmek istiyorsanız, lütfen familiarized nasıl çalıştığını Batch ile olduğundan emin olun ve birkaçını deneyin [Batch kod örnekleri][github_samples].
> 
> 

Split() uygulamanıza erişebilir:

* İş parametresi aracılığıyla `_parameters` alan.
* Aracılığıyla işi temsil eden CloudJob nesnesi `_job` alan.
* İş Yöneticisi görevi aracılığıyla temsil eden CloudTask nesnesi `_jobManagerTask` alan.

`Split()` Uygulama doğrudan işe görev ekleme gerekmez. Bunun yerine, kodunuzu CloudTask nesnelerinin bir dizisi döndürme zorunluluğu ve bu projeye otomatik olarak iş Bölümlendirici çağırma framework sınıfları tarafından eklenir. Yineleyici kullanmak C# için yaygındır (`yield return`) özelliği, bu görevleri hesaplanacak tüm görevlerin tamamlanmasını bekleyen yerine olabildiğince çabuk çalıştırma başlatmak izin verdiği ölçüde iş ayırıcılar uygulamak için.

**İş bölme hatası**

İş Bölümlendirici hatayla karşılaşılırsa, ya da yapmalısınız:

* C# kullanarak dizisi sonlandırmak `yield break` deyimi, hangi durumda İş Yöneticisi işleneceğini başarılı olarak; veya
* Durumda İş Yöneticisi, bir özel durum throw başarısız olarak kabul edilir ve nasıl istemci bunu yapılandırdığına bağlı olarak gerçekleştirilmesi).

Her iki durumda da, zaten proje Bölümlendirici tarafından döndürülen ve toplu işlem için eklenen herhangi bir görevi çalıştırma hakkına sahip olacaktır. Bunun olmasını istemiyorsanız, şunları yapabilirsiniz:

* İş Bölümlendiricinin döndürmeden önce işi Sonlandır
* Döndürmeden önce tüm görev koleksiyonu formüle (diğer bir deyişle dönüş bir `ICollection<CloudTask>` veya `IList<CloudTask>` bir C# yineleyici kullanarak, iş Bölümlendirici uygulamak yerine)
* Görev bağımlılıkları başarılı olarak tamamlanmasına İş Yöneticisi bağımlı tüm görevler yapma

**İş Yöneticisi'ni yeniden deneme**

İş Yöneticisi başarısız olursa, istemci yeniden deneme ayarlarına bağlı olarak Batch hizmeti tarafından denenen. Framework işe görevler eklediğinde, zaten mevcut herhangi bir görevi yoksayar. genel olarak, güvenli olmasıdır. Görevleri hesaplama pahalıdır, ancak projeye eklenmiş olan görevleri yeniden hesaplama ücret isteyebilirsiniz değil; yeniden çalıştırma kimlikleri aynı görevi oluşturmak için garanti edilmez, buna karşılık, ardından 'çoğaltmaları Yoksay' davranışı etkili olur değil. Bu gibi durumlarda zaten yapılmıştır ve, örneğin bir CloudJob.ListTasks görevleri yield başlamadan önce gerçekleştirerek yinelememesi çalışmayı algılamak için iş Bölümlendirici tasarlamanız gerekir.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Çıkış kodları ve İş Yöneticisi şablonunda özel durumları
Çıkış kodları ve özel durumlar bir programı çalıştırma sonucunu belirlemek için bir mekanizma sağlar ve bunlar programın yürütülmesini ile herhangi bir sorunu belirlemenize yardımcı olabilir. Bu bölümde açıklanan özel durumlar ve çıkış kodları İş Yöneticisi şablonunu uygular.

İş Yöneticisi şablonu ile gerçekleştirilen bir iş yöneticisi görevi üç olası çıkış kodunu döndürebilirsiniz:

| Kod | Açıklama |
| --- | --- |
| 0 |İş Yöneticisi başarıyla tamamlandı. Tamamlanana kadar iş Bölümlendirici kodunuzu çalıştıran ve tüm görevleri projeye eklendi. |
| 1 |İş Yöneticisi görevi, program 'beklenen' bir parçası olarak bir özel durum ile başarısız oldu. Özel durum için tanılama bilgileri içeren bir JobManagerException çevrilmiştir ve mümkün olan yerlerde, hatayı çözmek için öneriler. |
| 2 |İş Yöneticisi görevi 'beklenmeyen bir özel durumla başarısız oldu. Standart çıktıya bir özel durum günlüğe kaydedildi, ancak İş Yöneticisi ek tanılama veya düzeltme bilgileri ekleyemedi. |

Hatadan önce iş yöneticisi görevi başarısız olması durumunda bazı görevler yine de hizmete eklenmiş olabilir. Bu görevler, normal şekilde çalışır. "İş bölme hatası" Yukarıdaki Bu kod yolunun bir tartışma için bkz.

Özel durumları tarafından döndürülen tüm bilgileri stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için [hata işleme](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>İstemci konuları
Bu bölümde, bu şablonu temel alan bir iş yöneticisi çağrılırken bazı istemci uygulama gereksinimleri açıklanmaktadır. Bkz: [istemci kodundan parametreleri ve ortam değişkenlerini nasıl](#pass-environment-settings) parametreleri ve ortam ayarlarına geçirerek ilişkin ayrıntılar için.

**Zorunlu kimlik bilgileri**

Azure Batch işine görev eklemek için iş yöneticisi görevi, Azure Batch hesabı URL'si ve anahtarı gerektirir. Bu ortam değişkenlerini YOUR_BATCH_URL ve YOUR_BATCH_KEY adlı geçmesi gerekir. Bu İş Yöneticisi'ndeki görev ortam ayarlarını ayarlayabilirsiniz. Örneğin, C# istemci olarak:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Depolama kimlik bilgileri**

Genellikle, istemci (a) çoğu proje yöneticileri açıkça bağlantılı depolama hesabına erişmek zorunda değildir ve (b) bağlantılı bir depolama hesabı genellikle tüm görevleri olarak sağlanan olduğundan iş yöneticisi görevi bağlantılı depolama hesabı kimlik bilgilerini sağlayın gerekmeyen bir İş için ortak ortam ayarı. Ardından ortak ortam ayarları aracılığıyla bağlı depolama hesabındaki sağlamıyorsunuz ve İş Yöneticisi bağlı depolama alanına erişimi gerektirir, bağlantılı depolama kimlik bilgileri gibi vermeniz gerekir:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**İş Yöneticisi görevi ayarları**

İstemci iş yöneticisi ayarlamalısınız *killJobOnCompletion* bayrak **false**.

İstemcinin ayarlamak için genellikle güvenlidir *runExclusive* için **false**.

İstemcinin kullanması gereken *resourceFiles* veya *applicationPackageReferences* işi koleksiyon işlem düğümüne yüklenecek dağıtılan manager yürütülebilir (ve kendi gerekli bir dll).

Başarısız olursa varsayılan olarak, İş Yöneticisi denenen değil. İş Yöneticisi mantığa bağlı olarak istemci aracılığıyla yeniden denemeyi etkinleştir isteyebilirsiniz *kısıtlamaları*/*maxTaskRetryCount*.

**Proje ayarları**

İstemci iş Bölümlendirici görevleri bağımlılıkları ile yayar, işin usesTaskDependencies true olarak ayarlamanız gerekir.

İş bölme modelinde, istemcilerin hangi iş bölme oluşturur sağladığımız işleri görevleri eklemek istediğiniz olağan dışı. İstemci bu nedenle normalde işin ayarlamalısınız *onAllTasksComplete* için **terminatejob**.

## <a name="task-processor-template"></a>Görev işlemci şablonu
Bir görev işlemci şablon, aşağıdaki eylemleri gerçekleştirmek bir görev işlemci uygulamak için yardımcı olur:

* Her Batch görevinde tarafından çalıştırmak için gerekli bilgilerini ayarlayın.
* Her Batch görevi tarafından istenen tüm eylemleri çalıştırın.
* Görev çıkışları kalıcı depolama alanına kaydedin.

Görev işlemci toplu olarak görevleri çalıştırmak için gerekli olmamasına rağmen görev işlemci kullanımının önemli bir avantajı, tek bir yerde tüm görev yürütme eylemlerini uygulamak için bir sarmalayıcı sağlamasıdır. Örneğin, her görev bağlamında çeşitli uygulamaları çalıştırmak ihtiyacınız varsa veya kopyalamak gerekiyorsa tamamladıktan sonra kalıcı depolama alanına veri her görevi.

Basit veya karmaşık ve birçok veya olarak yükünüzün gerektirdiği gibi birkaç görev işlemcisi tarafından gerçekleştirilen eylemler olabilir. Ayrıca, bir görev işlemci halinde tüm görev eylemleri uygulayarak, kolayca güncelleştirebilir veya Eylemler, uygulamaları veya iş yükü gereksinimlerini değişikliklere göre ekleyin. Ancak, bazı durumlarda gereksiz karmaşıklık, örneğin hızla basit bir komut satırından başlatılabilir işleri çalıştırırken ekleyebilirsiniz olarak uygulamanız için en uygun çözümü görev işlemci olmayabilir.

### <a name="create-a-task-processor-using-the-template"></a>Şablon kullanarak bir görev işlemcisi oluşturmak
Bir görev işlemci, daha önce oluşturduğunuz çözüme eklemek için aşağıdaki adımları izleyin:

1. Var olan çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde çözüme sağ tıklayın, **Ekle**ve ardından **yeni proje**.
3. Altında **Visual C#** , tıklayın **bulut**ve ardından **Azure Batch görevi İşlemci**.
4. Uygulamanızı açıklayan ve bu projeyi (örn. görev işlemci olarak tanımlayan bir ad yazın "LitwareTaskProcessor").
5. Projeyi oluşturmak için tıklayın **Tamam**.
6. Son olarak, tüm başvurulan NuGet paketlerini yükleme ve değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için Visual Studio zorlamak için projeyi derleyin.

### <a name="task-processor-template-files-and-their-purpose"></a>Görev işlemci şablon dosyalarını ve amaçları
Görev işlemci şablonunu kullanarak bir proje oluşturduğunuzda, kod dosyası üç grupları oluşturur:

* Ana program dosyasına (Program.cs). Bu programın giriş noktası ve en üst düzey özel durum işlemeyi içerir. Normalde bunu değiştirmek gerekmez.
* Framework dizin. Bu parametreleri, görev eklemeyi bir Batch işi, vb. açmak İş Yöneticisi program – 'ortak' çalışmanın sorumlu dosyaları içerir. Normalde bu dosyalarda değişiklik gerekmez.
* Görev işlemci dosyası (TaskProcessor.cs). Bir görev (genellikle çağırarak varolan bir yürütülebilir dosya) yürütmek, uygulamaya özgü mantığınızı yerleştireceğiniz budur. Ayrıca ek veri indirme veya sonuç dosyaları karşıya yükleme gibi ön ve işlem sonrası kodu buraya ekleyin.

Elbette, iş mantığı bölme karmaşıklığına bağlı olarak, görev işlemci kodu desteklemek için gerekli ek dosyalar ekleyebilirsiniz.

Şablonu ayrıca bir .csproj dosyasını, app.config, packages.config, vb. gibi standart .NET proje dosyalarını oluşturur.

Bu bölümün geri kalanında farklı dosyalar ve kendi kod yapısını ve her sınıf yaptığı açıklanmaktadır.

![Visual Studio Çözüm Gezgini'nde görev işlemci şablon çözüm gösteriliyor][solution_explorer02]

**Framework dosyaları**

* `Configuration.cs`: Batch hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametresi gibi iş yapılandırma verilerini yüklenmesini kapsüller. Ayrıca, (bkz. Batch belgelerinde görevler için ortam ayarları) Batch tarafından tanımlanan ortam değişkenleri Configuration.EnvironmentVariable sınıfı aracılığıyla erişim sağlar.
* `IConfiguration.cs`: Birim test, proje ayırıcı sahte veya sahte yapılandırma nesnesi kullanarak yapabilirsiniz yapılandırma sınıfın uygulaması soyutlar.
* `TaskProcessorException.cs`: Sonlandırmak İş Yöneticisi gerektiren bir hatayı temsil eder. TaskProcessorException 'beklenen' hataları belirli tanılama bilgileri sonlandırma bir parçası olarak nerede sağlanabilir sarmalamak için kullanılır.

**Görev işlemci**

* `TaskProcessor.cs`: Görev çalışır. Framework TaskProcessor.Run yöntemi çağırır. Bu göreviniz, uygulamaya özgü mantık burada ekleyecektir sınıftır. Run yöntemi uygular:
  * Ayrıştırma ve herhangi bir görev parametreleri doğrulayın
  * Çağırmak için istediğiniz herhangi bir dış programı için komut satırının oluştur
  * Hata ayıklama amacıyla gerektirebilecek herhangi bir tanılama bilgileri günlüğe kaydetmek
  * Bu komut satırını kullanarak bir işlem başlatma
  * İşlemden çıkmak için bekle
  * Çıkış kodu işlemin başarılı veya başarısız olmadığını belirlemek için yakalama
  * Kalıcı depolama için tutmak istediğiniz tüm çıktı dosyaları kaydedin

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Programın giriş noktası ve en üst düzey özel durum işlemeyi içerir.

## <a name="implementing-the-task-processor"></a>Görev işlemci uygulama
Görev işlemci şablonu projesi açtığınızda, projenin TaskProcessor.cs dosyanın varsayılan olarak açık olacaktır. Aşağıda gösterilen uygulamasındaki Run() yönteminde kullanarak iş yükünüzü görevleri çalıştırma mantığını uygulayabilirsiniz:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
> [!NOTE]
> Uygulamasındaki Run() yönteminde açıklamalı bölümünde, iş yükünüzü görevleri çalıştırma mantığını ekleyerek değiştirmeye yönelik görev işlemci şablon kodunu yalnızca bölümüdür. Şablonu farklı bir bölümünü değiştirmek istiyorsanız, Lütfen ilk önce toplu Batch belgelerini gözden geçirerek ve Batch kod örnekleri birkaç çalışırken çalışma şeklinden edinin.
> 
> 

Uygulamasındaki Run() yönteminde sonuçlarını kaydetme ve son olarak bir çıkış kodu ile döndüren tüm işlemin tamamlanması beklenirken bir veya daha fazla işlemlerini başlatma komut satırı başlatmaktan sorumludur. Uygulamasındaki Run() yönteminde görevleriniz için işleme mantığı uygulamak burada ' dir. Görev işlemci framework uygulamasındaki Run() yönteminde için çağırır; Bunu çağırmanız gerekmez.

Uygulamasındaki Run() uygulamanıza erişebilir:

* Görev parametreleri aracılığıyla `_parameters` alan.
* İş ve görev kimlikleri aracılığıyla `_jobId` ve `_taskId` alanları.
* Görev yapılandırması aracılığıyla `_configuration` alan.

**Görev hatası**

Başarısız olması durumunda bir özel durum tarafından uygulamasındaki Run() yönteminde çıkabilirsiniz, ancak bu görevin çıkış kodu denetiminde en üst düzey özel durum işleyici ayrılmaz. Çıkış kodu hatası, farklı türlerde tanılama amacıyla örnek ayırabilirsiniz veya geçirilmemelidir ve bazı hata modlarını işlemini sonlandırması gerektiğini denetlemek gerekiyorsa, ardından uygulamasındaki Run() yönteminde döndüren ve sıfır olmayan çıkış çıkış kodu. Bu görevin çıkış kodu olur.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Çıkış kodları ve görev işlemci şablonunda özel durumları
Çıkış kodları ve özel durumlar bir programı çalıştırma sonucunu belirlemek için bir mekanizma sağlar ve bunlar programın yürütülmesini ile herhangi bir sorunu belirlemenize yardımcı olabilir. Bu bölümde açıklanan özel durumlar ve çıkış kodları görev işlemci şablonunu uygular.

Üç olası çıkış kodları görev işlemci şablonun uygulandığı bir işlemci görevi döndürebilir:

| Kod | Açıklama |
| --- | --- |
| [Process.ExitCode][process_exitcode] |Görev işlemci tamamlanana kadar çalıştı. Bu, çağrılan program yalnızca başarılı – olduğunu göstermez Not görev işlemci başarıyla çağrılır ve özel durumlar olmadan sonrası herhangi bir işlem gerçekleştirdi. Anlamı çıkış kodu, çağrılan programa bağlıdır: genellikle çıkış kodu 0 program başarılı ve başarısız programın herhangi bir çıkış kodu anlamına gelir anlamına gelir. |
| 1 |Görev İşlemci 'beklenen' bir programın parçası olarak bir özel durum ile başarısız oldu. Özel durum için çevrilmiştir bir `TaskProcessorException` tanılama bilgileri ve mümkün olan yerlerde, hatayı çözmek için öneriler. |
| 2 |Görev İşlemci 'beklenmeyen bir özel durumla başarısız oldu. Standart çıktıya bir özel durum günlüğe kaydedildi, ancak görev işlemci ek tanılama veya düzeltme bilgileri ekleyemedi. |

> [!NOTE]
> Ardından görev İşlemci hataları için çıkış kodları 1 ve 2 kullanarak, çağırma program belirli hata modlarını belirtmek için çıkış kodları 1 ve 2 kullanıyorsa, belirsiz. Program.cs dosyasındaki özel durum çalışmaları düzenleyerek farklı çıkış kodları için bu görev işlemci hata kodlarını değiştirebilirsiniz.
> 
> 

Özel durumları tarafından döndürülen tüm bilgileri stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için hata işleme, Batch belgelerinde bakın.

### <a name="client-considerations"></a>İstemci konuları
**Depolama kimlik bilgileri**

Görev işlemci çıktı, örneğin dosya kuralları Yardımcısı kitaplığı kullanarak kalıcı hale getirmek için Azure blob depolama kullanır. ardından erişmesi gereken *ya da* bulut depolama hesabı kimlik bilgilerini *veya* blob paylaşılan erişim imzası (SAS) içeren kapsayıcı URL'si. Şablona genel ortam değişkenlerini aracılığıyla kimlik bilgilerini sağlamak için destek içerir. İstemci depolama kimlik bilgilerini aşağıdaki gibi geçirebilirsiniz:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Depolama hesabı TaskProcessor sınıfında kullanılabilir `_configuration.StorageAccount` özelliği.

Bir kapsayıcı URL'si ile SAS kullanmak isterseniz, bu da bir iş ortak ortam ayarı aracılığıyla geçirebilirsiniz, ancak görev işlemci şablon şu anda bu için yerleşik destek içermez.

**Depolama Kurulumu**

İstemci veya iş yöneticisi görevi görevler tarafından işe Görev eklemeden önce gerekli tüm kapsayıcıları oluşturmanız önerilir. Bu, bir kapsayıcı URL'si ile SAS kullanırsanız, bu nedenle bir URL kapsayıcıyı oluşturmak için izni buna dahil değildir zorunludur. Depolama hesabı kimlik bilgileri, geçirdiğiniz olsa bile her görev, kapsayıcıdaki CloudBlobContainer.CreateIfNotExistsAsync aramak zorunda kaydedildikçe önerilir.

## <a name="pass-parameters-and-environment-variables"></a>Parametreleri ve ortam değişkenleri
### <a name="pass-environment-settings"></a>Ortam ayarı geçirin
Bir istemci için ortam ayarları biçiminde iş yöneticisi görevi bilgi geçirebilirsiniz. Bu bilgi işlem işin bir parçası çalışacak görev işlemci görevleri oluştururken iş yöneticisi görevi tarafından sonra kullanılabilir. Ortam ayarları geçirebileceğiniz bilgilerin örnekleri şunlardır:

* Depolama hesabı adını ve hesap anahtarları
* Batch hesabı URL'si
* Batch hesap anahtarı

Batch hizmetini kullanarak bir iş yöneticisi görevi için ortam ayarlarını geçirmek için basit bir mekanizma vardır `EnvironmentSettings` özelliğinde [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Örneğin, almak için `BatchClient` örnek bir Batch hesabı için istemciden gelen ortam değişkenleri Batch hesabı için paylaşılan anahtar kimlik bilgileri ve URL kod olarak geçirebilirsiniz. Benzer şekilde, Batch hesabıyla bağlantılı depolama hesabına erişmek için depolama hesabı adı ve depolama hesabı anahtarını ortam değişkenleri olarak geçirebilirsiniz.

### <a name="pass-parameters-to-the-job-manager-template"></a>İş Yöneticisi şablon parametreleri
Çoğu durumda, iş yöneticisi görevi, işlem bölme işlemini denetlemek için veya iş için görevleri yapılandırmak için iş başına parametreleri geçirmek kullanışlıdır. İş Yöneticisi görevinin kaynak dosyalarına parameters.json adlı bir JSON dosyası yükleyerek bunu yapabilirsiniz. Parametreleri ardından kullanılabilir hale gelebilir `JobSplitter._parameters` İş Yöneticisi şablonunda alan.

> [!NOTE]
> Yerleşik parametresini işleyicisi yalnızca dize-dize sözlükleri destekler. Karmaşık JSON değerleri parametre değerlerini geçirmek istiyorsanız, bu dize olarak geçirin ve bunları iş bölme ayrıştırma veya framework'ün değiştirme izni gerekir `Configuration.GetJobParameters` yöntemi.
> 
> 

### <a name="pass-parameters-to-the-task-processor-template"></a>Görev işlemci şablon parametreleri
Ayrıca, tek tek görevler görev işlemci şablonu kullanılarak uygulanan parametreler geçirebilir. İş Yöneticisi şablonuyla gibi adlı bir kaynak dosyası için görev işlemci şablonu arar

Parameters.JSON ve bulduğu if parametreleri sözlük olarak yükler. Birkaç görev işlemci görevlere parametreleri geçirmek nasıl seçenek vardır:

* İş parametresi JSON yeniden kullanın. Bu da tek parametre iş genelinde (için örnek, bir işleme yükseklik ve genişlik) kaynaklarsa çalışır. İçinde iş bölme bir CloudTask oluştururken, bunu yapmak için iş yöneticisi görevi'nın ResourceFiles parameters.json kaynak dosya nesnesine bir başvuru ekleyin. (`JobSplitter._jobManagerTask.ResourceFiles`) CloudTask'ın ResourceFiles koleksiyonunda için.
* Oluşturmak bir göreve özel parameters.json belge iş Bölümlendirici yürütme işleminin bir parçası olarak yükleyin ve o görev kaynak dosyaları koleksiyonunu blob başvurusu. Farklı görevler farklı parametreler varsa, bu gereklidir. Çerçeve dizinini göreve bir parametre olarak geçirildiği bir 3B işleme senaryosu örneği olabilir.

> [!NOTE]
> Yerleşik parametresini işleyicisi yalnızca dize-dize sözlükleri destekler. Karmaşık JSON değerleri parametre değerlerini geçirmek istiyorsanız, bunları dize olarak geçirin ve bunları görev işlemci ayrıştırmak veya framework'ün değiştirmek ihtiyacınız olacak `Configuration.GetTaskParameters` yöntemi.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
### <a name="persist-job-and-task-output-to-azure-storage"></a>Azure depolama için iş ve görev çıktılarını kalıcı hale getirme
Batch çözümü geliştirme başka bir yardımcı araç olan [Azure Batch dosya kuralları][nuget_package]. Bu .NET sınıf kitaplığı (şu anda önizlemede), Batch .NET uygulamalarında kolayca depolayın ve görev çıktıları ve Azure depolama alanından almak için kullanın. [Azure Batch, iş ve görev çıktılarını kalıcı hale getirme](batch-task-output.md) tam bir irdelemesi ve kitaplığa ve kullanımını içerir.


[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
