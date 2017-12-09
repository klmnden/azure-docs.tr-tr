---
title: "İşlem düğümlerinde - Azure Batch uygulama paketleri yükleme | Microsoft Docs"
description: "İşlem düğümlerini birden fazla uygulamaları ve sürümlerini toplu yükleme için kolayca yönetmek için Azure Batch uygulama paketleri özelliği kullanın."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d1f9951c9cc1b9380e166834afaeb18a4687e2d8
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>İşlem düğümleri Batch uygulama paketleri ile uygulamaları dağıtma

Azure Batch uygulama paketleri özelliği kolay yönetim görev uygulamaları ve bunların dağıtımı havuzunuzdaki işlem düğümlerine sağlar. Uygulama paketleri ile karşıya yükleyin ve bunların destekleyici dosyaları dahil olmak üzere, görevlerinizin çalışan uygulamaların birden fazla sürümünü yönetin. Ardından otomatik olarak bir veya daha fazla işlem düğümlerine bu uygulamaları havuzunuzdaki dağıtabilirsiniz.

Bu makalede, karşıya yükleme ve uygulama paketleri Azure portalında yönetmek öğreneceksiniz. Ardından bunları bir havuzun işlem düğümleri ile yüklemek hakkında bilgi edineceksiniz [Batch .NET] [ api_net] kitaplığı.

> [!NOTE]
> 
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez.
>
> API'ları oluşturma ve uygulama paketlerini Yönetme [Batch yönetimi .NET] parçası olan [[api_net_mgmt]] kitaplık. Bir işlem düğümünde uygulama paketleri yüklemek için API'ler parçası olan [Batch .NET] [ api_net] kitaplığı.  
>
> Burada açıklanan uygulama paketleri özelliği, ' service'nın önceki sürümlerinde kullanılabilir toplu işlem uygulamaları özelliği yerini alır.
> 
> 

## <a name="application-package-requirements"></a>Uygulama paketi gereksinimleri
Uygulama paketlerini kullanmak için gerek [bir Azure depolama hesabı bağlantı](#link-a-storage-account) Batch hesabınıza.

Bu özellik sunulmuştur [Batch REST API'si] [ api_rest] sürüm 2015 12 01.2.2 ve karşılık gelen [Batch .NET] [ api_net] kitaplığı sürüm 3.1.0. Her zaman son API sürümü Batch ile çalışırken kullanmanızı öneririz.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez.
>
>

## <a name="about-applications-and-application-packages"></a>Uygulamalar ve uygulama paketleri hakkında
Azure Batch içindeki bir *uygulama* havuzunuzdaki işlem düğümlerine otomatik olarak indirilebilir sürümü tutulan ikili dosyaları kümesine başvuruyor. Bir *uygulama paketi* başvurduğu bir *belirli* bu ikili dosyaları ve temsil bir verilen *sürüm* uygulamanın.

![Uygulamalar ve uygulama paketleri üst düzey diyagramı][1]

### <a name="applications"></a>Uygulamalar
Toplu bir uygulamada içeriyor veya daha fazla uygulama paketleri ve uygulama için yapılandırma seçeneklerini belirtir. Örneğin, uygulamanın işlem düğümlerini ve kendi paketleri olup güncelleştirilmiş veya silinebilir yüklemek için varsayılan uygulama paketi sürümü belirtebilirsiniz.

### <a name="application-packages"></a>Uygulama paketleri
Bir uygulama paketi uygulama ikili dosyaları içeren bir .zip dosyası ve görevlerinizi uygulamayı çalıştırmak gerekli destek dosyaları ' dir. Her uygulama paketi uygulamanın belirli bir sürümünü temsil eder.

Uygulama paketleri havuzu ve görev düzeylerde belirtebilirsiniz. Bir havuz veya görev oluşturduğunuzda, bir veya daha fazla bu paketleri ve (isteğe bağlı) bir sürüm belirtebilirsiniz.

* **Havuz uygulama paketleri** dağıtılan *her* havuzdaki düğüm. Bir düğüm bir havuzu katıldığında ve onu yeniden başlatıldığı ya da dağıtılır.
  
    Bir havuzdaki tüm düğümlerin işe ait görevleri yürüttüğünüzde havuzu uygulama paketleri uygundur. Bir veya daha fazla uygulama paketlerini bir havuz oluşturduğunuzda ve ekleme veya güncelleştirme mevcut havuzun paketleri belirtebilirsiniz. Varolan bir havuzun uygulama paketleri güncelleştirirseniz, düğümlerinden yeni paketi yüklemek için yeniden başlatmanız gerekir.
* **Görev uygulama paketleri** yalnızca görevin komut satırı çalıştırmadan önce bir görevi çalıştırmak için zamanlanan bir işlem düğümünde dağıtılır. Belirtilen uygulama paketinin ve sürüm ise zaten düğümde değil imzalanmasını ve var olan paketi kullanılır.
  
    Görev uygulama paketleri Burada farklı işleri bir havuzu üzerinde çalışır ve bir iş tamamlandığında havuzu silinmez paylaşılan havuzu ortamlarında yararlıdır. İşinizin havuzdaki görevleri, düğümlerinden azsa uygulamanız yalnızca görevleri çalıştıran düğümlere dağıtıldığı için görev uygulama paketleri veri aktarımını azaltabilir.
  
    Büyük bir uygulamayı çalıştırma işleri görev uygulama paketi yararlanabilir diğer senaryolar verilmiştir ancak yalnızca birkaç görevler için. Örneğin, önceden işlem aşamanın veya ön işleme veya birleştirme uygulama ağır olduğu, bir birleştirme görev görev uygulama paketlerini kullanma yararlı.

> [!IMPORTANT]
> Uygulamalar ve toplu işlem hesabı içindeki uygulama paketleri sayısını ve en fazla uygulama paket boyutu kısıtlamalar vardır. Bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) bu sınırları hakkında ayrıntılı bilgi için.
> 
> 

### <a name="benefits-of-application-packages"></a>Uygulama paketleri yararları
Uygulama paketleri, toplu Çözümünüzdeki kodu basitleştirmek ve görevlerinizi çalışan uygulamaları yönetmek için gerekli ek yükünü azaltın.

Uygulama paketleri ile uzun düğümlerine yüklemek için tek kaynak dosyaların listesini belirtmek, havuzun görev başlatma sahip değil. El ile uygulama dosyalarınızı Azure Storage veya düğümlerinizi birden fazla sürümünü yönetmek zorunda değilsiniz. Ve oluşturma hakkında endişelenmeniz gerekmez [SAS URL'leri](../storage/common/storage-dotnet-shared-access-signature-part-1.md) depolama hesabınızdaki dosyalarına erişim sağlamak için. Batch uygulama paketleri depolamak ve bunlara işlem düğümleri dağıtmak için Azure Storage ile arka planda çalışır.

> [!NOTE] 
> Bir başlangıç görevinin toplam boyutunun kaynak dosyaları ve ortam değişkenleri dahil olmak üzere 32.768 karakter veya daha az olması gerekir. Başlangıç görevi bu sınırı aşarsa, uygulama paketleri kullanarak başka bir seçenektir. Ayrıca, kaynak dosyaları içeren bir sıkıştırılmış arşivi oluşturmak, bir BLOB Azure Storage olarak karşıya yükleme ve başlangıç görevinin komut satırından sıkıştırmasını açın. 
>
>

## <a name="upload-and-manage-applications"></a>Karşıya yükleme ve uygulamalarını yönetme
Kullanabileceğiniz [Azure portal] [ portal] veya [Batch yönetimi .NET](batch-management-dotnet.md) Batch hesabınızdaki uygulama paketlerini yönetmek için kitaplık. Sonraki birkaç bölümlerde, biz öncelikle bir depolama hesabı bağlantı sonra ekleme uygulamaları ve paketleri ve portal ile yönetme ele gösterilmektedir.

### <a name="link-a-storage-account"></a>Bir depolama hesabı bağlantı
Uygulama paketlerini kullanmak için bir Azure depolama hesabı toplu işlem hesabınıza bağlamanız gerekir. Henüz bir depolama hesabı yapılandırmadıysanız, Azure portal'ı ilk kez bir uyarı görüntüler **uygulamaları** parçasında **Batch hesabı** dikey.

> [!IMPORTANT]
> Batch şu anda destekler *yalnızca* **genel amaçlı** 5. adımda açıklandığı gibi depolama hesabı türü [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account), [Azure storage hesapları hakkında](../storage/common/storage-create-storage-account.md). Batch hesabınıza bir Azure Storage hesabı bağladığınızda, bağlantı *yalnızca* bir **genel amaçlı** depolama hesabı.
> 
> 

![Azure portalında 'depolama hesabı yapılandırıldı' uyarısı][9]

Batch hizmeti, uygulama paketlerinizi depolamak için ilişkili depolama hesabı kullanır. İki hesap bağladığınız sonra toplu işlem düğümleriniz bağlantılı depolama hesabına depolanan paketleri otomatik olarak dağıtabilirsiniz. Bir depolama hesabı toplu işlem hesabınıza bağlamak için tıklatın **depolama hesabı ayarlarını** üzerinde **uyarı** dikey ve ardından **depolama hesabı** üzerinde **depolama hesabı** dikey.

![Azure Portal'da depolama hesabı dikey seçin][10]

Bir depolama hesabı oluşturmanızı öneririz *özellikle* , toplu işlem hesabı ile kullanmak için ve burada seçin. Bir depolama hesabının nasıl oluşturulacağı hakkında daha fazla ayrıntı için "Bir depolama hesabı oluşturma" görmek [hakkında Azure depolama hesapları](../storage/common/storage-create-storage-account.md). Bir depolama hesabı oluşturduktan sonra daha sonra bu toplu işlem hesabınızı kullanarak bağlayabilirsiniz **depolama hesabı** dikey.

> [!WARNING]
> Batch hizmeti Azure Storage blok blobları, uygulama paketlerinizi depolamak için kullanır. Olduğunuz [normal olarak ücretlendirilir] [ storage_pricing] blok blobu veri. Uygulama paketlerinizi sayısı ve boyutu göz önünde bulundurun ve düzenli aralıklarla maliyetleri en aza indirmek için kullanım dışı paketleri kaldırmak emin olun.
> 
> 

### <a name="view-current-applications"></a>Geçerli uygulamaları görüntüle
Batch hesabınızda uygulamaları görüntülemek için **uygulamaları** görüntüleme çalışırken soldaki menüde menü öğesi **Batch hesabı** dikey.

![Uygulamaları döşeme][2]

Bu menü seçeneğini seçerek açılır **uygulamaları** dikey penceresinde:

![Uygulamaları listeleme][3]

**Uygulamaları** dikey hesabınızı ve aşağıdaki özellikleri her uygulama Kimliğini görüntüler:

* **Paketleri**: Bu uygulama ile ilişkili sürüm sayısı.
* **Varsayılan sürüm**: uygulama havuzu için belirttiğinizde bir sürüm belirtmezseniz yüklü uygulama sürümü. Bu ayar isteğe bağlıdır.
* **Güncelleştirmelere izin**: olup paketini güncelleştirir, silme ve eklemeleri belirten değeri izin verilir. Bu ayarlanırsa **Hayır**, paket güncelleştirme ve silme işlemleri uygulama için devre dışıdır. Yalnızca yeni uygulama paketi sürümleri eklenebilir. Varsayılan değer **Evet**.

### <a name="view-application-details"></a>Uygulama Ayrıntıları görüntüle
Bir uygulama ayrıntılarını içeren dikey penceresini açmak için uygulamada seçin **uygulamaları** dikey.

![Uygulama Ayrıntıları][4]

Uygulama Ayrıntıları dikey penceresinde, uygulamanız için aşağıdaki ayarları yapılandırabilirsiniz.

* **Güncelleştirmelere izin**: uygulama paketlerinin güncelleştirilmiş veya silinebilir olup olmadığını belirtin. Bu makalenin sonraki bölümlerinde "Güncelleştirmek veya bir uygulama paketini silmeniz" bakın.
* **Varsayılan sürüm**: işlem düğümleri dağıtmak için bir varsayılan uygulama paketi belirleyin.
* **Görünen ad**: Batch çözümünüzü uygulama hakkında bilgi Örneğin, Batch aracılığıyla müşterilerinize sağlayan bir hizmet UI gösterildiğinde kullanabileceğiniz bir kolay ad belirtin.

### <a name="add-a-new-application"></a>Yeni bir uygulama Ekle
Yeni bir uygulama oluşturmak için bir uygulama paketi eklemek ve yeni ve benzersiz uygulama kimliği belirtin. Yeni uygulama kimliği ile eklediğiniz ilk uygulama paketi, yeni uygulama da oluşturur.

Tıklatın **Ekle** üzerinde **uygulamaları** açmak için dikey **yeni uygulama** dikey.

![Azure portalında yeni uygulama dikey penceresi][5]

**Yeni uygulama** dikey yeni uygulama ve uygulama paketi ayarlarını belirlemek için aşağıdaki alanları sağlar.

**Uygulama Kimliği**

Bu alan tabi standart Azure toplu işlem kimliği doğrulama kuralları, yeni uygulama Kimliğini belirtir. Bir uygulama kimliği sağlamak için kurallar aşağıdaki gibidir:

* Windows düğümlerinde kimlik alfasayısal karakterler, tire ve alt çizgi, herhangi bir birleşimini içerebilir. Linux düğümleri üzerinde yalnızca alfasayısal karakterler ve alt çizgi izin verilir.
* Birden fazla 64 karakter içeremez.
* Toplu işlem hesabı içinde benzersiz olmalıdır.
* Servis talebi koruyarak ve büyük küçük harf duyarsız ' dir.

**Sürüm**

Bu alan karşıya yüklemekte olduğunuz uygulama paketi sürümünü belirtir. Sürüm dizelerini tabi aşağıdaki doğrulama kurallar şunlardır:

* Windows düğümlerinde sürüm dizesi alfasayısal karakterler, tire, alt çizgi ve nokta herhangi bir birleşimini içerebilir. Linux düğümleri üzerinde sürüm dizesi yalnızca alfasayısal karakterler ve alt çizgi içerebilir.
* Birden fazla 64 karakter içeremez.
* Uygulama içinde benzersiz olmalıdır.
* Servis talebi koruyarak ve büyük küçük harf duyarsız'dır.

**Uygulama paketi**

Bu alan, uygulama yürütmek için gerekli destek dosyaları ve uygulama ikili dosyaları içeren .zip dosyasını belirtir. Tıklatın **bir dosya seçin** kutusu veya göz atın ve uygulama dosyalarını içeren bir .zip dosyası seçmek için klasör simgesine.

Bir dosyayı seçtikten sonra tıklayın **Tamam** Azure Storage yüklenecek başlamak için. Karşıya yükleme işlemi tamamlandığında, portal bir bildirim görüntüler ve dikey penceresi kapanır. Karşıya yüklemekte olduğunuz dosya boyutu ve ağ bağlantınızın hızına bağlı olarak, bu işlem biraz zaman alabilir.

> [!WARNING]
> Kapatmayın **yeni uygulama** karşıya yükleme işlemi tamamlanmadan önce dikey. Bunun yapılması, karşıya yükleme işlemi durdurur.
> 
> 

### <a name="add-a-new-application-package"></a>Yeni bir uygulama paketi ekleme
Olan bir uygulamanın yeni bir uygulama Paket sürümü eklemek için bir uygulama seçin **uygulamaları** dikey penceresinde'ı tıklatın **paketleri**, ardından **Ekle** açmak için **Ekle paket** dikey.

![Azure portalında uygulama paketi dikey ekleme][8]

Gördüğünüz gibi alanların eşleşen **yeni uygulama** dikey penceresinde, ancak **uygulama kimliği** kutusu devre dışıdır. Yeni uygulama için yaptığınız gibi belirtin **sürüm** yeni paketiniz için göz atın, **uygulama paketi** .zip dosyası ve ardından **Tamam** paketini karşıya yüklemek için.

### <a name="update-or-delete-an-application-package"></a>Güncelleştirme veya uygulama paketi silme
Güncelleştirmek veya var olan uygulama paketini silmek için uygulama ayrıntıları dikey penceresini açmak, **paketleri** açmak için **paketleri** dikey penceresinde tıklatın **üç nokta** gerçekleştirmek istediğiniz eylemi seçin ve değiştirmek için istediğiniz uygulama paketi satırında.

![Güncelleştirme veya Azure portalında paketi silme][7]

**Güncelleştirme**

Tıkladığınızda **güncelleştirme**, *güncelleştirme paketini* dikey penceresi görüntülenir. Bu dikey benzer *yeni uygulama paketi* dikey penceresinde, ancak yalnızca karşıya yüklemek için yeni bir ZIP dosyası belirtmenize olanak sağlayan paket seçimi alan etkinleştirilir.

![Güncelleştirme paketi dikey Azure portalında][11]

**Silme**

Tıkladığınızda **silmek**, Paket sürümü silinmesini onaylaması istenir ve toplu Azure depolama biriminden paket siler. Bir uygulamanın varsayılan sürümü silerseniz **varsayılan sürüm** ayarını, uygulama için kaldırılır.

![Uygulama silme][12]

## <a name="install-applications-on-compute-nodes"></a>İşlem düğümlerinde uygulamaları yükleme
Azure portal ile uygulama paketlerini yönetmek nasıl öğrendiğinize göre bunları işlem düğümleri ve toplu görevleri çalıştırmak için dağıtma aşağıdakiler ele.

### <a name="install-pool-application-packages"></a>Havuz uygulama paketlerini yükleme
Bir havuzdaki tüm işlem düğümlerinde bir uygulama paketi yüklemek için bir veya daha fazla uygulama paketi belirleyin *başvuruları* havuzu için. Bir havuz için belirttiğiniz uygulama paketlerini her işlem düğümünde o düğüm havuza katıldığında ve düğümün yeniden başlatıldığı ya da yüklenir.

Batch .NET içinde bir veya daha fazla belirtin [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] yeni bir havuz oluşturduğunuzda veya mevcut bir havuzu. [ApplicationPackageReference] [ net_pkgref] sınıfı, bir uygulama Kimliğini ve sürümünü bir havuzun üzerinde yüklemek için işlem düğümlerini belirtir.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Bir uygulama paketi dağıtım için herhangi bir nedenle başarısız olursa, Batch hizmeti düğüm işaretler [kullanılamaz][net_nodestate], ve bu düğümde yürütülmek zamanlanmış hiçbir görevler. Bu durumda, aşağıdakileri yapmalısınız **yeniden** paketi dağıtımı yeniden başlatmanız düğüme. Düğümü yeniden başlatmak, ayrıca görev düğümde yeniden zamanlamayı sağlar.
> 
> 

### <a name="install-task-application-packages"></a>Görev uygulama paketlerini yükleme
Benzer bir havuz için uygulama paketi belirttiğiniz *başvuruları* bir görev için. Bir görev, bir düğümü üzerinde çalışacak şekilde zamanlandığı paket indirilir ve yalnızca görevin komut satırı yürütülmeden önce ayıklanır. Belirtilen paket ve sürümü zaten yüklüyse, düğümde, paket yüklenmez ve var olan paketi kullanılır.

Bir görev uygulama paketini yüklemek için görev yapılandırmanız [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] özelliği:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Yüklü uygulamalar yürütme
Bir havuz ya da görev için belirlediğiniz paketleri indirilir ve adlandırılmış bir dizin içinde ayıklanan `AZ_BATCH_ROOT_DIR` düğümün. Toplu da adlandırılan bir dizin yolunu içeren bir ortam değişkeni oluşturur. Görev komut satırları, düğümdeki uygulama başvururken bu ortam değişkenini kullanın. 

Windows düğümlerinde değişkeni şu biçimdedir:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Linux düğümleri üzerinde biçimi biraz farklıdır. Nokta (.), kısa çizgi (-) ve numara işareti (#) düzleştirilmiş ortam değişkeninde alt çizgi için. Ayrıca, uygulama kimliği durumunun korunduğundan unutmayın. Örneğin:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

`APPLICATIONID`ve `version` dağıtımı için belirtilen uygulama ve Paket sürümü karşılık gelen değerler. Örneğin, uygulama sürümünün 2.7 belirtilmişse *blender* yüklenmesi gereken Windows düğümlerinde, kendi dosyalarına erişmek için bu ortam değişkenini görev komut satırları kullanırsınız:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Linux düğümleri üzerinde ortam değişkeni bu biçiminde belirtin. Nokta (.), kısa çizgi (-) düzleştirmek ve alt çizgileri sayı işareti (#) ve uygulama kimliği harfleri doğru:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
``` 

Bir uygulama paketi yüklediğinizde, hesaplama düğümlerini dağıtmak için bir varsayılan sürümü belirtebilirsiniz. Bir uygulama için varsayılan bir sürümünün belirtilirse, uygulama başvurduğunuzda Sürüm soneki atlayabilirsiniz. Varsayılan Uygulama sürümü Azure portalında uygulamalar dikey penceresinde gösterildiği gibi belirleyebilirsiniz [karşıya yükleyin ve uygulamalarını yönetin](#upload-and-manage-applications).

Örneğin, "2.7" uygulaması için varsayılan sürüm olarak ayarlarsanız *blender*ve görevlerinizi aşağıdaki ortam değişkeni başvurusu, sonra Windows düğümleriniz sürüm 2.7 çalıştırır:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Aşağıdaki kod parçacığını varsayılan sürümü başlatan bir örnek görev komut satırı gösterir *blender* uygulama:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Bkz: [görevler için ortam ayarları](batch-api-basics.md#environment-settings-for-tasks) içinde [Batch özelliklerine genel bakış](batch-api-basics.md) işlem düğümü ortam ayarları hakkında daha fazla bilgi.
> 
> 

## <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme
Var olan bir havuzu bir uygulama paketiyle yapılandırılmış havuzu için yeni bir paket belirtebilirsiniz. Aşağıdaki durumlardan bir havuz için yeni bir paket başvuru belirtirseniz:

* Toplu işlem hizmeti yeni belirtilen paket havuzuna Katıl tüm yeni düğümler ve herhangi bir düğümde, yeniden başlatıldığı ya da varolan yükler.
* Paket referanslarını güncelleştirdiğinizde zaten havuzun düğümlerini otomatik olarak yeni uygulama paketi yükleme işlem. Bu, düğümler yeniden veya yeni paket almak için yeniden işlem.
* Yeni bir paket dağıtıldığında, oluşturulan ortam değişkenleri yeni uygulama paket referanslarını yansıtır.

Bu örnekte, var olan bir havuzu 2.7 sürümüne sahip *blender* biri olarak yapılandırılmış bir uygulama, [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. Havuzun düğümleri 2.76b sürümüyle güncelleştirmek için yeni bir belirtin [ApplicationPackageReference] [ net_pkgref] yeni sürümü ve yürütme Değiştir.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Yeni sürüm yapılandırılmış, Batch hizmeti sürüm 2.76b hiçbirine yükler *yeni* havuzuna katılır düğümü. 2.76b olan düğümlerine yüklemek için *zaten* havuzunda yeniden veya onları yeniden görüntü oluşturma. Yeniden başlatılan düğümleri önceki paket dağıtımları dosyalarından korumak unutmayın.

## <a name="list-the-applications-in-a-batch-account"></a>Bir Batch hesabında uygulamaları Listele
Kullanarak uygulamalar ve bunların paketleri bir Batch hesabında listeleyebilirsiniz [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] yöntemi.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a>Kaydırma
Uygulama paketleri ile işlerini istediğiniz uygulamaları seçin ve Batch özellikli hizmetinizi işleriyle işlerken kullanılacak tam sürümünü belirtin, müşterilerinizin yardımcı olabilir. Karşıya yükleme ve kendi uygulamalarında hizmetinizde izlemek müşterilerinize özelliği de sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Batch REST API'si] [ api_rest] uygulama paketleri ile çalışmak için destek de sağlar. Örneğin, [applicationPackageReferences] [ rest_add_pool_with_packages] öğesinde [bir havuz için bir hesap eklemek] [ rest_add_pool] REST API kullanarak yüklemek için paketler belirtme hakkında bilgi için. Bkz: [uygulamaları] [ rest_applications] Batch REST API'sini kullanarak uygulama bilgilerini elde etme hakkında ayrıntılar için.
* Bilgi edinmek için nasıl programlı olarak [Azure Batch hesaplarını ve kotalarını Batch yönetimi .NET ile yönetme](batch-management-dotnet.md). [Batch yönetimi .NET][api_net_mgmt] kitaplığı, toplu uygulama veya hizmet hesabı oluşturma ve silme özellikleri etkinleştirebilir.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Uygulama paketleri üst düzey diyagramı"
[2]: ./media/batch-application-packages/app_pkg_02.png "Azure portalında uygulamaları kutucuğu"
[3]: ./media/batch-application-packages/app_pkg_03.png "Azure portalında uygulamalar dikey penceresi"
[4]: ./media/batch-application-packages/app_pkg_04.png "Uygulama Ayrıntıları dikey Azure portalında"
[5]: ./media/batch-application-packages/app_pkg_05.png "Azure portalında yeni uygulama dikey penceresi"
[7]: ./media/batch-application-packages/app_pkg_07.png "Update veya delete paketleri açılan Azure portalında"
[8]: ./media/batch-application-packages/app_pkg_08.png "Azure portalında yeni uygulama paketi dikey penceresi"
[9]: ./media/batch-application-packages/app_pkg_09.png "Bağlantılı Depolama hesabı uyarı"
[10]: ./media/batch-application-packages/app_pkg_10.png "Azure Portal'da depolama hesabı dikey seçin"
[11]: ./media/batch-application-packages/app_pkg_11.png "Güncelleştirme paketi dikey Azure portalında"
[12]: ./media/batch-application-packages/app_pkg_12.png "Azure portalında paket onay iletişim Sil"
