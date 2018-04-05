---
title: Visual Studio şablonları - Azure batch çözümleriyle yapı | Microsoft Docs
description: Visual Studio Proje şablonları uygulamak ve Azure Batch işlem yoğunluklu iş yüklerini çalıştırmak nasıl yardımcı olabileceğini öğrenin.
services: batch
documentationcenter: .net
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: 5e041ae2-25af-4882-a79e-3aa63c4bfb20
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5241c62e8b423b20477fc72c87303daf3d4ab43c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-visual-studio-project-templates-to-jump-start-batch-solutions"></a>Batch çözümleri hızla başlatmak için Visual Studio Proje şablonları kullanın

**İş Yöneticisi** ve **görev işlemci Visual Studio şablonları** toplu işlemi için uygulamak ve en az çaba ile toplu işlem yoğunluklu iş yüklerini çalıştırmak için yardımcı olması için kod sağlar. Bu belge, bu şablonları açıklar ve bunları nasıl kullanacağınızı yönelik yönergeler sağlanmaktadır.

> [!IMPORTANT]
> Bu makalede, yalnızca bu iki şablonlarını ilgili bilgileri açıklar ve Batch hizmeti ve ilgili temel kavramları hakkında bilgi sahibi olduğunuzu varsayar: havuzlar, işlem düğümleri, işler ve görevler, iş yöneticisi görevleri, ortam değişkenleri ve diğer ilgili bilgi. Daha fazla bilgi bulabilirsiniz [Azure Batch temel bilgileri](batch-technical-overview.md), [geliştiriciler için Batch özelliklerine genel bakış](batch-api-basics.md), ve [.NET için Azure Batch kitaplığını kullanmaya başlama](batch-dotnet-get-started.md).
> 
> 

## <a name="high-level-overview"></a>Yüksek düzey genel bakış
İş Yöneticisi ve görev işlemci şablonları iki yararlı bileşenleri oluşturmak için kullanılabilir:

* Bir işi bağımsız olarak, paralel olarak çalıştırılabilir birden çok görev içine ayırabilirsiniz iş Bölümlendirici uygulayan bir iş yöneticisi görevi.
* Ön işleme ve bir uygulama komut satırı sonrası işleme gerçekleştirmek için kullanılan bir görev işlemci.

Örneğin, bir filmi işleme senaryosunda, iş Bölümlendirici tek film iş yüzlerce veya binlerce tek tek çerçeveler ayrı ayrı işlemek ayrı görevlerin kapatmanız. Buna bağlı olarak, görev işlemci işleme uygulaması çağıracaktır ve her işlemek için gerekli tüm bağımlı işlemler çerçeve yanı sıra (örneğin, bir depolama konumuna işlenmiş çerçeve kopyalama) ek eylemleri gerçekleştirin.

> [!NOTE]
> Her ikisi de veya işlem işinizin ve tercihlerinize gereksinimlerine bağlı olarak bunlardan yalnızca birini kullanmak seçebilmeniz İş Yöneticisi ve görev işlemci şablonları birbirinden bağımsızdır.
> 
> 

Aşağıdaki çizimde gösterildiği gibi bu şablonları kullanan bir işlem iş üç aşamanın gidin:

1. İstemci kodu (örneğin, uygulama, web hizmeti, vb.) bir iş belirten kendi iş yöneticisi görevi gibi iş Yöneticisi programı Azure Batch hizmetine gönderir.
2. Batch hizmeti bir işlem düğümünde iş yöneticisi görevi çalıştırır ve birçok gerektiği gibi parametreleri ve iş Bölümlendirici kod teknik özelliklerine göre işlem düğümleri gibi iş Bölümlendirici üzerinde görev işlemci görevler, belirtilen sayıda başlatır.
3. Görev işlemci görevler bağımsız olarak, girdi verilerini işlemek, çıktı verileri oluşturmak için paralel olarak çalışır.

![İstemci kodu Batch hizmeti ile nasıl etkileşim kurduğunu gösteren diyagram][diagram01]

## <a name="prerequisites"></a>Önkoşullar
Toplu işlem şablonlarını kullanmak için aşağıdakiler gerekir:

* Visual Studio 2015 yüklü bir bilgisayar. Toplu işlem şablonları, şu anda yalnızca Visual Studio 2015 için desteklenir.
* Kullanılabilir toplu şablonları [Visual Studio Galerisi] [ vs_gallery] Visual Studio uzantıları olarak. Şablonları almak için iki yolu vardır:
  
  * Kullanarak şablonları yüklemek **Uzantılar ve güncelleştirmeler** Visual Studio iletişim kutusunda (daha fazla bilgi için bkz: [bulma ve Visual Studio uzantılarını kullanarak][vs_find_use_ext]). İçinde **Uzantılar ve güncelleştirmeler** iletişim kutusuna, aramak ve aşağıdaki iki uzantıları indirin:
    
    * Azure toplu iş yöneticisi işi Bölümlendirici ile
    * Azure Batch görev işlemci
  * Visual Studio için çevrimiçi galeriden belirlemeniz: [Microsoft Azure toplu işlem proje şablonları][vs_gallery_templates]
* Kullanmayı planlıyorsanız, [uygulama paketleri](batch-application-packages.md) İş Yöneticisi'ni dağıtmak için özellik ve görev işlemci toplu işlem düğümleri, bir depolama hesabı toplu işlem hesabınıza bağlamanız gerekir.

## <a name="preparation"></a>Hazırlama
Bu kod, İş Yöneticisi ve görev işlemci programlar arasında paylaşmak kolaylaştırabilir olduğundan iş yöneticisi olarak görev işlemcinizin içerebilir bir çözüm oluşturmanızı öneririz. Bu çözüm oluşturmak için aşağıdaki adımları izleyin:

1. Visual Studio'yu açın ve seçin **dosya** > **yeni** > **proje**.
2. Altında **şablonları**, genişletin **diğer proje türleri**, tıklatın **Visual Studio çözümleri**ve ardından **boş çözüm**.
3. Uygulamanız ve bu çözümü (örneğin, "LitwareBatchTaskPrograms") amacını açıklayan bir ad yazın.
4. Yeni bir çözüm oluşturmak için tıklatın **Tamam**.

## <a name="job-manager-template"></a>İş Yöneticisi şablonu
İş Yöneticisi şablonu, aşağıdaki eylemleri gerçekleştirebilirsiniz bir iş yöneticisi görevi uygulamak için yardımcı olur:

* Bir iş birden çok görevlere bölün.
* Toplu olarak çalıştırmak için bu görevler gönderebilir.

> [!NOTE]
> İş Yöneticisi görevleri hakkında daha fazla bilgi için bkz: [geliştiriciler için Batch özelliklerine genel bakış](batch-api-basics.md#job-manager-task).
> 
> 

### <a name="create-a-job-manager-using-the-template"></a>Şablonu kullanarak bir iş yöneticisi oluşturma
Bir iş yöneticisi daha önce oluşturduğunuz çözüme eklemek için aşağıdaki adımları izleyin:

1. Var olan çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde çözüme sağ tıklayın, **Ekle** > **yeni proje**.
3. Altında **Visual C#**, tıklatın **bulut**ve ardından **Azure toplu iş yöneticisi işi Bölümlendirici ile**.
4. Uygulamanızı açıklayan ve bu projeyi (örneğin, İş Yöneticisi tanımlayan bir ad yazın "LitwareJobManager").
5. Projeyi oluşturmak için tıklatın **Tamam**.
6. Son olarak, tüm başvurulan NuGet paketleri yüklenemedi ve onu değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için Visual Studio zorlamak için projeyi oluşturun.

### <a name="job-manager-template-files-and-their-purpose"></a>İş Yöneticisi şablon dosyalarını ve amaçları
İş Yöneticisi şablonunu kullanarak bir proje oluşturduğunuzda, kod dosyaları üç grupları oluşturur:

* Ana program dosyası (Program.cs). Bu program giriş noktası ve en üst düzey özel durum işleme içerir. Normalde bu değişiklik gerekmez.
* Framework dizini. Bu görevler ekleme toplu iş, vb. parametreleri açmak İş Yöneticisi program – 'ortak' çalışmanın sorumlu dosyalarını içerir. Normalde bu dosyalarda değişiklik gerekmez.
* İş Bölümlendirici dosyası (JobSplitter.cs). Bir işi görevlere bölmek için uygulamaya özgü mantığınızı yerleştireceğiniz budur.

Elbette mantığı bölme iş karmaşıklığına bağlı olarak, iş Bölümlendirici kodu desteklemek için gerektiği gibi ek dosyaları ekleyebilirsiniz.

Şablon ayrıca .csproj dosya, app.config, packages.config, vb. gibi standart .NET proje dosyalarını oluşturur.

Bu bölümde rest farklı dosya ve kendi kod yapısını ve her sınıf yaptığı açıklanmaktadır.

![İş Yöneticisi şablonu çözümünü gösteren Visual Studio Çözüm Gezgini][solution_explorer01]

**Framework dosyaları**

* `Configuration.cs`: Batch hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametreleri gibi iş yapılandırma verilerinin yüklenmesini yalıtır. Ayrıca, Configuration.EnvironmentVariable sınıfı aracılığıyla toplu tanımlı ortam değişkenlerini (toplu belgelerinde görevler için ortam ayarları bakın) erişim sağlar.
* `IConfiguration.cs`: Birim testi sahte veya sahte yapılandırma nesnesi kullanarak, iş Bölümlendirici olan yapılandırma sınıf uyarlamasını soyutlar.
* `JobManager.cs`: İş Yöneticisi program bileşenlerinin düzenler. İş Bölümlendirici çağırma ve görevi göndereni iş Bölümlendirici tarafından döndürülen görevleri gönderme iş Bölümlendirici başlatmak için sorumludur.
* `JobManagerException.cs`: Sonlandırmak İş Yöneticisi gerektiren bir hatayı temsil eder. JobManagerException belirli tanılama bilgileri sonlandırma bir parçası olarak burada sağlanabilir 'beklenen' hataları kaydırmak için kullanılır.
* `TaskSubmitter.cs`: Bu sınıf, toplu işlem işi Bölümlendirici tarafından döndürülen görevleri ekleme ile sorumludur. JobManager sınıfı toplamalar görev dizisini verimli ancak zamanında ekleme işlemi, iş için toplu içine sonra çağırır TaskSubmitter.SubmitTasks arka plan iş parçacığında her toplu işlemi için.

**İş Bölümlendirici**

`JobSplitter.cs`: Bu sınıf, iş görevleri bölmek için uygulamaya özgü mantığı içerir. Framework yöntem bunları döndürür gibi projeye ekler görevleri bir dizi elde etmek için JobSplitter.Split yöntemini çağırır. Bu Burada, iş mantığı ekleme sınıftır. İş bölüm istediğiniz görevleri temsil eden CloudTask örnekleri bir dizi döndürülecek Split yöntemini uygulayın.

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Program giriş noktası ve en üst düzey özel durum işleme içerir.

### <a name="implementing-the-job-splitter"></a>İş Bölümlendirici uygulama
İş Yöneticisi şablonu projeyi açtığınızda, projenin varsayılan olarak açık JobSplitter.cs dosyayı sahip olur. Aşağıdaki Split() yöntemi Göster kullanarak, iş yükü görevler için bölünmüş mantığı uygulayabilirsiniz:

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
> Açıklamalı bölümünde `Split()` mantığı ekleyerek değiştirmenizi işleriniz farklı görevlere bölmek için yönelik İş Yöneticisi şablonu kodu yalnızca bölümünü bir yöntemdir. Farklı bir şablon bölümünü değiştirmek istiyorsanız, lütfen familiarized toplu nasıl çalıştığı ile olduğundan emin olun ve bazılarını deneyin [toplu kod örnekleri][github_samples].
> 
> 

Split() uygulamanıza erişimi:

* İş parametresi aracılığıyla `_parameters` alan.
* Aracılığıyla işi temsil eden CloudJob nesnesi `_job` alan.
* İş Yöneticisi görevi aracılığıyla temsil eden CloudTask nesnesi `_jobManagerTask` alan.

`Split()` Uygulama doğrudan işe görev ekleme gerekmez. Bunun yerine, kodunuzu CloudTask nesnelerinin bir dizisi döndürmelidir ve bu projeye otomatik olarak iş Bölümlendirici çağırma framework sınıfları tarafından eklenir. Kullanım C# ' ın yineleyici için ortak olan (`yield return`) başlatmak hesaplanacak tüm görevler için bekleyen yerine mümkün olan en kısa sürede çalışan görevler böylece iş ayırıcılar uygulama özelliği.

**İş Bölümlendirici hatası**

İş Bölümlendirici hatayla karşılaşırsa ya da gerekir:

* C# kullanarak dizisi sonlandırmak `yield break` deyimi, hangi durumda İş Yöneticisi işleneceğini başarılı olarak; veya
* Bir özel durum, bu durumda İş Yöneticisi throw başarısız olarak kabul edilir ve istemci yapılandırmasına bağlı olarak denenen).

Her iki durumda da zaten iş Bölümlendirici tarafından döndürülen ve toplu işlem eklenen herhangi bir görevi çalıştırmak uygun olacaktır. Bunun olmasını istemiyorsanız, olabilir:

* İş iş Bölümlendirici döndürmeden önce sonlandırma
* Bunu dönmeden önce tüm görev koleksiyon formüle (diğer bir deyişle, dönüş bir `ICollection<CloudTask>` veya `IList<CloudTask>` bir C# yineleyici kullanarak, iş Bölümlendirici uygulamak yerine)
* Görev bağımlılıkları İş Yöneticisi başarılı tamamlanma bağımlı olan tüm görevleri yapmak için kullanın

**İş Yöneticisi'ni yeniden deneme**

İş Yöneticisi başarısız olursa, istemci yeniden deneme ayarlarına bağlı olarak Batch hizmeti tarafından denenen. Framework işe görevler eklediğinde, zaten mevcut herhangi bir görevi yoksayar genel olarak, güvenli olmasıdır. Ancak, görevleri hesaplama pahalı ise, projeye eklenmiş olan görevleri yeniden hesaplanması maliyetini tabi getirmek istemeyen; yeniden çalıştırın ve aynı görevle kimlikleri oluşturmak için kesin değildir, buna karşılık, ardından 'çoğaltmaları Yoksay' davranışı ilkenin etkisini gösterip değil. Bu durumlarda zaten yapılmış ve bu, örneğin görevleri elde etmek üzere başlatmadan önce bir CloudJob.ListTasks gerçekleştirerek kullanmamasını iş algılamak için iş Bölümlendirici tasarlamanız gerekir.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Çıkış kodları ve İş Yöneticisi şablonunda özel durumlar
Çıkış kodları ve özel durumları program çalıştırmanın sonucunu belirlemek için bir mekanizma sağlar ve programın yürütülmesi ile herhangi bir sorunu belirlemek için yardımcı olabilir. İş Yöneticisi şablon çıkış kodları ve bu bölümde açıklanan özel durumları uygular.

İş Yöneticisi şablonla uygulanan bir iş yöneticisi görevi üç olası çıkış kodlarını döndürebilirsiniz:

| Kod | Açıklama |
| --- | --- |
| 0 |İş Yöneticisi başarıyla tamamlandı. İş Bölümlendirici kodunuzu tamamlanıncaya kadar çalıştırılmış ve tüm görevler projeye eklendi. |
| 1 |İş Yöneticisi görevi program 'beklenen' bir parçası olarak bir özel durum ile başarısız oldu. Özel durum JobManagerException tanılama bilgileri ile çevrilmiştir ve mümkün olduğunda hata çözümü için öneriler. |
| 2 |İş Yöneticisi görevi 'beklenmeyen' bir özel durumla başarısız oldu. Standart çıktıya özel durum günlüğe kaydedildi, ancak İş Yöneticisi ek tanılama ya da düzeltme bilgileri ekleyemedi. |

Hatadan önce iş yöneticisi görevi başarısız olması durumunda, bazı görevler hala hizmete eklenmemiş olabilir. Bu görevleri normal olarak çalışır. "İş Bölümlendirici hatası" yukarıda bu kod yolu hakkında bilgi için bkz.

Özel durumlar tarafından döndürülen tüm bilgileri stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için bkz: [işleme hatası](batch-api-basics.md#error-handling).

### <a name="client-considerations"></a>İstemci değerlendirmeleri
Bu bölümde, bu şablona dayalı bir iş yöneticisi çağrılırken, bazı istemci uygulama gereksinimleri açıklanmaktadır. Bkz: [parametreleri ve ortam değişkenleri istemci kodundan geçirmek nasıl](#pass-environment-settings) parametreleri ve ortam ayarlarını geçirme hakkında bilgi.

**Zorunlu kimlik bilgileri**

Azure Batch işe görev ekleme için iş yöneticisi görevi Azure Batch hesabı URL'si ve anahtar gerektirir. Bu ortam değişkenleri YOUR_BATCH_URL ve YOUR_BATCH_KEY adlı geçmesi gerekir. Bu İş Yöneticisi'nde görev ortam ayarları ayarlayabilirsiniz. Örneğin, C# istemci olarak:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Depolama kimlik bilgileri**

Genellikle, istemci (a) çoğu iş yöneticileri açıkça bağlantılı depolama hesabına erişim gerekmez ve (b bağlantılı depolama hesabı genellikle tüm görevleri olarak sağlanan olduğundan iş yöneticisi görevi bağlantılı depolama hesabı kimlik bilgilerini sağlamak üzere gerekmez bir İş için ortak ortamı ayarı. Daha sonra ortak ortam ayarları aracılığıyla bağlantılı depolama hesabı sağlama değil ve İş Yöneticisi bağlı depolama alanına erişimi gerektiriyorsa, bağlantılı depolama kimlik bilgilerini aşağıdaki gibi sağlamanız:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**İş Yöneticisi görevi ayarları**

İstemci iş yöneticisi ayarlamalısınız *killJobOnCompletion* bayrağını **false**.

İstemcinin ayarlamak için genellikle güvenlidir *runExclusive* için **false**.

İstemcinin kullanması gereken *resourceFiles* veya *applicationPackageReferences* işi koleksiyonu işlem düğümüne dağıtılmış manager yürütülebilir (ve gerekli DLL'leri).

Başarısız olursa varsayılan olarak, İş Yöneticisi denenecek değil. İş Yöneticisi mantığınızı bağlı olarak, istemci yeniden deneme aracılığıyla etkinleştirmek isteyebilirsiniz *kısıtlamaları*/*maxTaskRetryCount*.

**Proje ayarları**

Görev bağımlılıkları ile iş Bölümlendirici yayar, istemci iş usesTaskDependencies true olarak ayarlanmalıdır.

İş Bölümlendirici model işlerinin ne iş Bölümlendirici oluşturur üzerine görevler eklemek istediğiniz istemciler için olağan dışı. İstemci bu nedenle normal iş ayarlamalısınız *onAllTasksComplete* için **terminatejob**.

## <a name="task-processor-template"></a>Görev işlemci şablonu
Bir görev işlemci şablonu, aşağıdaki eylemleri gerçekleştirebilirsiniz görev işlemci uygulamak için yardımcı olur:

* Her toplu iş görevi tarafından çalıştırmak için gerekli bilgilerini ayarlayın.
* Her toplu iş görevi tarafından istenen tüm eylemler çalıştırın.
* Görev çıktıları kalıcı depoya kaydedin.

Görev işlemci toplu olarak görevleri çalıştırmak için gerekli değildir, ancak görev işlemci kullanımının önemli bir avantajı, bir konumda bulunan tüm görev yürütme eylemlerini uygulamak için bir sarmalayıcı sağlamasıdır. Örneğin, her görevin bağlamında çeşitli uygulamalar çalıştırmanız gerekiyorsa ya da kopyalama gerekiyorsa tamamladıktan sonra kalıcı depolama alanına veri her görev.

Görev işlemcisi tarafından gerçekleştirilen eylemler, basit veya karmaşık, olarak ve birçok veya olarak az sayıda İş yükünüzün gerektirdiği olabilir. Ayrıca, bir görev işlemci halinde tüm görev eylemleri uygulayarak, taşımalarına güncelleştirebilir veya Eylemler uygulamalara veya iş yükü gereksinimlerini değişikliklere göre ekleyin. Ancak, bazı durumlarda Örneğin hızlı bir şekilde basit bir komut satırından başlatılan bir iş çalışmadığı zaman gereksiz karmaşıklık ekleyebilirsiniz gibi en iyi çözüm, uygulamanız için bir görev işlemci olmayabilir.

### <a name="create-a-task-processor-using-the-template"></a>Şablonu kullanarak bir görev işlemcisi oluşturma
Bir görev işlemci daha önce oluşturduğunuz çözüme eklemek için aşağıdaki adımları izleyin:

1. Var olan çözümünüzü Visual Studio'da açın.
2. Çözüm Gezgini'nde çözüme sağ tıklayın, **Ekle**ve ardından **yeni proje**.
3. Altında **Visual C#**, tıklatın **bulut**ve ardından **Azure Batch görev İşlemci**.
4. Uygulamanızı açıklayan ve bu projeyi (örneğin görev işlemci olarak tanımlayan bir ad yazın "LitwareTaskProcessor").
5. Projeyi oluşturmak için tıklatın **Tamam**.
6. Son olarak, tüm başvurulan NuGet paketleri yüklenemedi ve onu değiştirmeye başlamadan önce projenin geçerli olduğunu doğrulamak için Visual Studio zorlamak için projeyi oluşturun.

### <a name="task-processor-template-files-and-their-purpose"></a>Görev işlemci şablon dosyalarını ve amaçları
Görev işlemci şablonu kullanarak bir proje oluşturduğunuzda, kod dosyaları üç grupları oluşturur:

* Ana program dosyası (Program.cs). Bu program giriş noktası ve en üst düzey özel durum işleme içerir. Normalde bu değişiklik gerekmez.
* Framework dizini. Bu görevler ekleme toplu iş, vb. parametreleri açmak İş Yöneticisi program – 'ortak' çalışmanın sorumlu dosyalarını içerir. Normalde bu dosyalarda değişiklik gerekmez.
* Görev işlemcisi dosyasını (TaskProcessor.cs). Bir görev (genellikle çağırarak varolan çalıştırılabilir) yürütmek, uygulamaya özgü mantığınızı yerleştireceğiniz budur. Ek veri indirme veya sonuç dosyalarını karşıya yükleme gibi ön ve işlem sonrası kodu da buraya ekleyin.

Elbette mantığı bölme iş karmaşıklığına bağlı olarak, görev işlemci kodu desteklemek için gerektiği gibi ek dosyaları ekleyebilirsiniz.

Şablon ayrıca .csproj dosya, app.config, packages.config, vb. gibi standart .NET proje dosyalarını oluşturur.

Bu bölümde rest farklı dosya ve kendi kod yapısını ve her sınıf yaptığı açıklanmaktadır.

![Görev işlemci şablon çözümünü gösteren Visual Studio Çözüm Gezgini][solution_explorer02]

**Framework dosyaları**

* `Configuration.cs`: Batch hesabı ayrıntıları, bağlantılı depolama hesabı kimlik bilgileri, iş ve görev bilgileri ve iş parametreleri gibi iş yapılandırma verilerinin yüklenmesini yalıtır. Ayrıca, Configuration.EnvironmentVariable sınıfı aracılığıyla toplu tanımlı ortam değişkenlerini (toplu belgelerinde görevler için ortam ayarları bakın) erişim sağlar.
* `IConfiguration.cs`: Birim testi sahte veya sahte yapılandırma nesnesi kullanarak, iş Bölümlendirici olan yapılandırma sınıf uyarlamasını soyutlar.
* `TaskProcessorException.cs`: Sonlandırmak İş Yöneticisi gerektiren bir hatayı temsil eder. TaskProcessorException belirli tanılama bilgileri sonlandırma bir parçası olarak burada sağlanabilir 'beklenen' hataları kaydırmak için kullanılır.

**Görev işlemci**

* `TaskProcessor.cs`: Görev çalıştırır. Framework TaskProcessor.Run yöntemini çağırır. Bu burada göreviniz uygulamaya özgü mantığını Ekle sınıftır. Run yöntemi uygulayın:
  * Ayrıştırma ve herhangi bir görev parametre doğrulama
  * Çağrılacak istediğiniz herhangi bir dış programı için komut satırı oluştur
  * Hata ayıklama amacıyla gerektirebilecek herhangi tanılama bilgileri günlüğe kaydet
  * Bu komut satırını kullanarak bir işlem Başlat
  * Çıkış işlemini bekle
  * Başarılı veya başarısız olup olmadığı belirlenemiyor işlem çıkış kodu yakalama
  * Kalıcı depoya tutmak istediğiniz tüm çıktı dosyaları kaydedin

**Standart .NET komut satırı proje dosyaları**

* `App.config`: Standart .NET uygulama yapılandırma dosyası.
* `Packages.config`: Standart NuGet paket bağımlılık dosyası.
* `Program.cs`: Program giriş noktası ve en üst düzey özel durum işleme içerir.

## <a name="implementing-the-task-processor"></a>Görev işlemci uygulama
Görev işlemci şablon projeyi açtığınızda, projenin varsayılan olarak açık TaskProcessor.cs dosyayı sahip olur. Aşağıda gösterilen Run() yöntemi kullanarak, iş yükü görevleri çalıştırma mantığını uygulayabilirsiniz:

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
> Run() yöntemi açıklamalı bölümünde, görevler için çalışma mantığı, iş yükü ekleyerek değiştirmeye yönelik görev işlemci şablonu kodu yalnızca bölümüdür. Şablonu farklı bir bölümünü değiştirmek istiyorsanız, Lütfen ilk önce toplu toplu belgelerini gözden geçirin ve toplu kod örnekleri birkaç çalışırken işleyişi ile edinin.
> 
> 

Run() yöntemi sonuçları kaydetme ve son olarak bir çıkış kodu ile döndürme tüm işlemin tamamlanması beklenirken bir veya daha fazla işlemlerini başlatma komut satırı başlatmaktan sorumludur. Burada görevleriniz için işleme mantığı uygulamanız Run() yöntemidir. Görev işlemci framework Run() yöntemi sizin için çağırır; çağrısını kendiniz gerekmez.

Run() uygulamanıza erişimi:

* Görev parametrelerini aracılığıyla `_parameters` alan.
* İş ve görev kimlikleri aracılığıyla `_jobId` ve `_taskId` alanları.
* Görev yapılandırması aracılığıyla `_configuration` alan.

**Görev hatası**

Hata durumunda bir özel durum atma tarafından Run() yöntemi çıkabilirsiniz ancak bu üst düzey özel durum işleyici Görev çıkış kodu denetiminde durumda bırakır. Çıkış kodu hatası, farklı türlerde tanılama amacıyla örneğin ayırabilmesi veya çünkü bazı modu hatası iş sonlanmalıdır ve diğerleri vermemelisiniz denetlemeye ihtiyacınız varsa, daha sonra Run() yöntemi sıfır olmayan döndürerek çıkılması gerekiyor çıkış kodu. Bu görev çıkış kodu olur.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Çıkış kodları ve görev işlemci şablonunda özel durumlar
Çıkış kodları ve özel durumları program çalıştırmanın sonucunu belirlemek için bir mekanizma sağlar ve bunlar herhangi bir programın yürütülmesini sorunu tanımlamaya yardımcı olabilir. Görev işlemci şablon çıkış kodları ve bu bölümde açıklanan özel durumları uygular.

Görev işlemci şablonla uygulanan bir işlemci görevi üç olası çıkış kodlarını döndürebilirsiniz:

| Kod | Açıklama |
| --- | --- |
| [Process.ExitCode][process_exitcode] |Görev işlemci tamamlanıncaya kadar verdi. Bu, çağrılan programın yalnızca başarılı – olduğunu göstermez Not görev işlemci başarıyla çağrılır ve özel durumlar olmadan tüm sonrası işleme yapılır. Çıkış kodu anlamını çağrılan programa bağlıdır: çıkış kodu 0 program başarılı ve başarısız program herhangi bir çıkış kodu anlamına gelir genellikle anlamına gelir. |
| 1 |Görev işlemci program 'beklenen' bir parçası olarak bir özel durum ile başarısız oldu. Özel durum için çevrilmiştir bir `TaskProcessorException` tanılama bilgileriyle ve mümkün olduğunda hata çözümü için öneriler. |
| 2 |Görev İşlemci 'beklenmeyen' bir özel durumla başarısız oldu. Standart çıktıya özel durum günlüğe kaydedildi, ancak görev işlemci ek tanılama ya da düzeltme bilgileri ekleyemedi. |

> [!NOTE]
> Programın, çağırma modu belirli hatası belirtmek için çıkış kodları 1 ve 2 kullanıyorsa, ardından çıkış kodunu 1 ve 2 görev İşlemci hataları için belirsiz kullanmaktır. Program.cs dosyasının özel durumlarda düzenleyerek farklı çıkış kodları için bu görev işlemci hata kodları değiştirebilirsiniz.
> 
> 

Özel durumlar tarafından döndürülen tüm bilgileri stdout.txt ve stderr.txt dosyalarına yazılır. Daha fazla bilgi için işleme hatası, toplu belgelerine bakın.

### <a name="client-considerations"></a>İstemci değerlendirmeleri
**Depolama kimlik bilgileri**

Görev işlemcinizin çıkarır, örneğin dosya kuralları Yardımcısı kitaplığı kullanarak kalıcı hale getirmek için Azure blob depolama kullanır sonra erişmesi gereken *ya da* bulut depolama hesabının kimlik bilgilerini *veya* blob paylaşılan erişim imzası (SAS) içeren bir kapsayıcı URL. Şablon ortak ortam değişkenleri aracılığıyla kimlik bilgilerini sağlamak için destek içerir. İstemci depolama kimlik bilgilerini aşağıdaki gibi geçirebilirsiniz:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

Depolama hesabı TaskProcessor sınıfında sonra kullanılabilir `_configuration.StorageAccount` özelliği.

Kapsayıcı URL'si ile SAS kullanmayı tercih ederseniz, bu da bir iş ortak ortamı ayarı aracılığıyla geçirebilirsiniz ancak görev işlemci şablonu şu anda bu için yerleşik destek içermez.

**Depolama Kurulumu**

İstemci veya iş yöneticisi görevi işe Görev eklemeden önce gereken görevleri tarafından herhangi bir kapsayıcıdaki oluşturmanız önerilir. Bu, bir kapsayıcı URL'si ile SAS kullanırsanız, bu nedenle bir URL kapsayıcı oluşturma izni içermez zorunludur. Depolama hesabı kimlik bilgileri, geçirdiğiniz olsa bile CloudBlobContainer.CreateIfNotExistsAsync kapsayıcısında arama yapmak zorunda her görev kaydeder önerilir.

## <a name="pass-parameters-and-environment-variables"></a>Parametreleri ve ortam değişkenleri
### <a name="pass-environment-settings"></a>Geçişi ortam ayarları
Bir istemci iş yöneticisi görevi ortam ayarları biçiminde bilgi geçirebilirsiniz. Bu bilgi işlem iş parçası olarak çalışacak görev işlemci görevler oluştururken iş yöneticisi görevi tarafından sonra kullanılabilir. Ortam ayarları geçirebilirsiniz bilgileri örnekleri şunlardır:

* Depolama hesabı adı ve hesap anahtarları
* Batch hesabı URL'si
* Toplu işlem hesabı anahtarı

Batch hizmetini kullanarak bir iş yöneticisi görevi ortam ayarlarını geçirmek için basit bir mekanizma sahip `EnvironmentSettings` özelliğinde [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Örneğin, almak için `BatchClient` örneği bir toplu işlem hesabı için ortam değişkenleri istemciden kod URL ve toplu işlem hesabı için paylaşılan anahtar kimlik bilgileri olarak geçirebilirsiniz. Benzer şekilde, Batch hesabına bağlantılı depolama hesabına erişmek için depolama hesabı adı ve depolama hesabı anahtarı ortam değişkenleri olarak geçirebilirsiniz.

### <a name="pass-parameters-to-the-job-manager-template"></a>İş Yöneticisi şablon parametreleri
Çoğu durumda, iş yöneticisi görevi işlem bölme iş denetlemek veya iş için görevler yapılandırmak için iş başına parametreleri geçirmek kullanışlıdır. İş Yöneticisi görevinin kaynak dosyası olarak parameters.json adlı bir JSON dosyası karşıya yükleyerek bunu yapabilirsiniz. Parametreleri sonra kullanılabilir hale gelebilir `JobSplitter._parameters` İş Yöneticisi şablonunda alan.

> [!NOTE]
> Yerleşik parametre işleyicisi yalnızca dize-dize sözlükler destekler. Parametre değerleri karmaşık JSON değerleri geçirmek istiyorsanız, bunları dizeleri olarak geçirmek ve iş Bölümlendirici ayrıştırma veya framework'ün değiştirmek gerekir `Configuration.GetJobParameters` yöntemi.
> 
> 

### <a name="pass-parameters-to-the-task-processor-template"></a>Görev işlemci şablon parametreleri
Ayrıca, görev işlemci şablonu kullanılarak uygulanan ayrı görevlere parametreleri geçirebilirsiniz. İş Yöneticisi şablonla gibi adlı bir kaynak dosyasını görev işlemci şablonu arar

Parameters.JSON ve bulduğu IF parametreleri sözlüğü olarak yükler. Birkaç parametreleri görev işlemci görevlere geçirmek nasıl ilgili seçenekleri şunlardır:

* İş parametrelerini JSON yeniden kullanabilirsiniz. Bu, iyi yalnızca parametreleri (örneğin, bir işleme yükseklik ve genişlik için) iş genelinde olanları ise çalışır. Bu, bir CloudTask iş Bölümlendirici oluştururken yapmak için İş Yöneticisi görev ResourceFiles parameters.json kaynak dosya nesnesine başvuru Ekle (`JobSplitter._jobManagerTask.ResourceFiles`) CloudTask'ın ResourceFiles koleksiyonuna.
* Oluştur ve bir görev özgü parameters.json belge iş Bölümlendirici yürütme bir parçası olarak yüklemek ve görevin kaynak dosyaları koleksiyonundaki bu blob başvurusu. Farklı görevler farklı parametreler varsa, bu gereklidir. Bir örnek çerçeve dizini parametre olarak göreve geçirildiği 3B işleme senaryo olabilir.

> [!NOTE]
> Yerleşik parametre işleyicisi yalnızca dize-dize sözlükler destekler. Parametre değerleri karmaşık JSON değerleri geçirmek istiyorsanız, bunları dizeleri olarak geçirmek ve görev işlemcide ayrıştırma veya framework'ün değiştirmek gerekir `Configuration.GetTaskParameters` yöntemi.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
### <a name="persist-job-and-task-output-to-azure-storage"></a>Azure Storage iş ve Görev çıkış Sürdür
Batch çözümü geliştirme yararlı bir diğer araç [Azure toplu işlem dosyası kuralları][nuget_package]. Bu .NET sınıf kitaplığı (şu anda önizlemede), Batch .NET uygulamalarında kolayca depolamak ve görev çıktıları için ve Azure depolama biriminden almak için kullanın. [Azure toplu işlem iş ve görev çıktısı kalıcı](batch-task-output.md) kitaplığı ve kullanım tam bir irdelemesi içerir.

### <a name="batch-forum"></a>Toplu işlem Forumu
[Azure toplu işlem Forumu] [ forum] MSDN'de toplu ele almaktadır ve hizmet hakkında sorular sormak için iyi bir yerdir. HEAD üzerinde üzerinden faydalı "Yapışkan" gönderiler için ve Batch çözümlerinizi derleme sırasında çıktıkları anda sorularınızı gönderin.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
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
