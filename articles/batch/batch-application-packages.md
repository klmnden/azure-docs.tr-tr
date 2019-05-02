---
title: İşlem düğümlerinde - Azure Batch uygulama paketleri yükle | Microsoft Docs
description: İşlem düğümlerini birden çok uygulamaları ve sürümlerini toplu yükleme için kolayca yönetmek için Azure Batch uygulama paketleri özelliği kullanın.
services: batch
documentationcenter: .net
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/05/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ee54d37050991763e60a6feb96c75d80384a42ac
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726676"
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı

Azure batch'in uygulama paketleri özelliği, kolay yönetim görev uygulamalarının ve havuzunuzdaki işlem düğümlerine dağıtımının sağlar. Uygulama paketleri ile karşıya yükleme ve birden çok sürümünü destekleme dosyalarına dahil olmak üzere görevlerinizin çalıştırdığı uygulamaları yönetin. Ardından otomatik olarak bir veya daha fazla bu uygulamaların işlem düğümlerine havuzunuzdaki dağıtabilirsiniz.

Bu makalede, karşıya yüklemek ve Azure portalında uygulama paketlerini yönetmek öğrenin. Daha sonra bunları bir havuzun işlem düğümleri ile yükleme öğrenin [Batch .NET] [ api_net] kitaplığı.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez.
>
> API oluşturma ve uygulama paketleri yönetme parçası olan [toplu işlem yönetimi .NET] [ api_net_mgmt] kitaplığı. Bir işlem düğümünde uygulama paketleri yüklemek için API'ler parçası olan [Batch .NET] [ api_net] kitaplığı. Diğer diller için kullanılabilir Batch API'lerine karşılaştırılabilir özellikler alır. 
>
> Burada açıklanan uygulama paketleri özelliği, ' service'nın önceki sürümlerinde kullanılabilir Batch Apps özelliğini yerini alır.

## <a name="application-package-requirements"></a>Uygulama paketi gereksinimleri
Uygulama paketlerini kullanmak için yapmanız [bir Azure depolama hesabı bağlantı](#link-a-storage-account) Batch hesabınıza.

## <a name="about-applications-and-application-packages"></a>Uygulamalar ve uygulama paketleri hakkında
Azure Batch içinde bir *uygulama* havuzunuzdaki işlem düğümlerine otomatik olarak indirilebilir tutulan ikili dosyaları kümesini ifade eder. Bir *uygulama paketini* başvurduğu bir *belirli* bu ikili dosyalar ve temsil eder bir verilen *sürüm* uygulama.

![Uygulamalar ve uygulama paketleri için yüksek düzey bir diyagramı][1]

### <a name="applications"></a>Uygulamalar
Bir Batch uygulamada içeriyor veya daha fazla uygulama paketleri ve uygulama için yapılandırma seçenekleri belirtir. Örneğin, uygulamanın işlem düğümleri, paketleri olup güncelleştirildi veya silinebilir yüklemek için varsayılan uygulama paket sürümünü belirtebilirsiniz.

### <a name="application-packages"></a>Uygulama paketleri
Bir uygulama paketi, uygulama ikili dosyalarını içeren bir .zip dosyası ve uygulamayı çalıştırmak görevler için gerekli destek dosyaları ' dir. Her uygulama paketi, uygulama belirli bir sürümünü temsil eder.

Uygulama paketlerini havuz ve görev düzeyinde belirtebilirsiniz. Bir havuzu veya göreviyle oluşturduğunuzda, bir veya daha fazla bu paketleri ve (isteğe bağlı olarak) bir sürümünü belirtebilirsiniz.

* **Havuz uygulama paketleri** dağıtılan *her* havuzdaki düğüm. Bir düğüm bir havuza katıldığında ve onu yeniden başlatıldığında veya zamanı dağıtılır.
  
    Havuz uygulama paketleri, bir havuzdaki tüm düğümlerin iş görevlerinin yürüttüğünüzde uygundur. Havuz oluşturma ve ekleme ya da mevcut bir havuzun paketleri Güncelleştir, bir veya daha fazla uygulama paketi belirtebilirsiniz. Mevcut bir havuzun uygulama paketlerini güncelleştirme, yeni paketi yüklemek için düğümlerini yeniden başlatmanız gerekir.
* **Görev uygulama paketleri** yalnızca bir görev, görevin komut satırı çalıştırılmadan önce çalışmak üzere zamanlanmış işlem düğümü dağıtılır. Belirtilen uygulama paketini ve sürüm ise zaten düğümde değil imzalanmasını ve var olan paketi kullanılır.
  
    Görev uygulama paketleri, burada tek havuzda farklı işler çalıştırılır ve bir iş tamamlandığında havuz silinmez paylaşılan havuz ortamlarında yararlıdır. İşinizin havuzdaki görevleri, düğümlerinden azsa uygulamanız yalnızca görevleri çalıştıran düğümlere dağıtıldığı için görev uygulama paketleri veri aktarımını azaltabilir.
  
    Büyük bir uygulama çalışan işleri görev uygulama paketleri avantaj elde edebileceği diğer senaryolar, ancak yalnızca birkaç görev için. Örneğin, ön işleme aşamasından veya önceden işleme ya da birleştirme uygulama ağır olduğu birleştirme görevi, görev uygulama paketleri kullanımından yararlanabilir.

> [!IMPORTANT]
> Uygulamalar ve uygulama paketleri bir Batch hesabında sayısını ve en fazla uygulama paketi boyutu sınırlamaları vardır. Bkz: [Azure Batch hizmeti için kotalar ve sınırlar](batch-quota-limit.md) bu sınırları hakkındaki ayrıntılar için.
> 
> 

### <a name="benefits-of-application-packages"></a>Uygulama paketleri avantajları
Uygulama paketleri, toplu iş çözümdeki kod basitleştirebilir ve görevlerinizin çalıştırdığı uygulamaları yönetmek için gerekli ek yükünü azaltın.

Uygulama paketleri ile uzun düğümlerine yüklemek için ayrı kaynak dosyaların listesini belirtmek, havuzun görev başlatma yok. Uygulama dosyalarınızı Azure Depolama'da veya düğümlerinizi birden çok sürümünü el ile yönetmeniz gerekmez. Ve oluşturma hakkında endişelenmeniz gerekmez [SAS URL'lerini](../storage/common/storage-dotnet-shared-access-signature-part-1.md) depolama hesabınızdaki dosyalara erişimi sağlamak için. Batch, uygulama paketlerini depolamak ve işlem düğümlerine dağıtmak için Azure depolama ile arka planda çalışır.

> [!NOTE] 
> Bir başlangıç görevinin toplam boyutunun kaynak dosyaları ve ortam değişkenleri dahil olmak üzere 32.768 karakter veya daha az olması gerekir. Başlangıç göreviniz bu sınırı aşarsa, ardından uygulama paketlerini kullanarak başka bir seçenektir. Ayrıca, kaynak dosyaları içeren bir sıkıştırılmış arşiv oluşturun, Azure Depolama'ya BLOB yüklemek ve başlangıç göreviniz komut satırından sıkıştırmasını açın. 
>
>

## <a name="upload-and-manage-applications"></a>Karşıya yükleme ve uygulamaları yönetme
Kullanabileceğiniz [Azure portalında] [ portal] ya da Batch hesabınızdaki uygulama paketlerini yönetmek için Batch Yönetimi API'leri. Sonraki birkaç bölümde, biz öncelikle bir depolama hesabı bağlamanız ve ardından ekleyerek uygulamaları ve paketleri ve portal ile yönetme işlemini göstermektedir.

### <a name="link-a-storage-account"></a>Bir depolama hesabına bağlama
Uygulama paketlerini kullanmak için önce bağlamalısınız bir [Azure depolama hesabı](batch-api-basics.md#azure-storage-account) Batch hesabınıza. Bir depolama hesabı henüz yapılandırmadıysanız, Azure portalını ilk kez tıkladığınızda bir uyarı görüntüler **uygulamaları** Batch hesabınızdaki.



![Azure portalında 'depolama hesabı yapılandırılmadı' uyarısı][9]

Batch hizmeti, uygulama paketlerinizi depolamak için ilişkili depolama hesabını kullanır. İki hesap bağladıktan sonra Batch, işlem düğümlerine bağlı depolama hesabında depolanan paketleri otomatik olarak dağıtabilirsiniz. Bir depolama hesabını Batch hesabınıza bağlamak için tıklatın **depolama hesabı** üzerinde **uyarı** penceresi ve ardından **depolama hesabı** yeniden.

![Azure portalında depolama hesabı dikey penceresi seçin][10]

Bir depolama hesabı oluşturmanızı öneririz *özellikle* ile Batch hesabınızda kullanmak ve burada seçin. Bir depolama hesabı oluşturduktan sonra daha sonra Batch hesabınıza kullanarak bağlayabilirsiniz **depolama hesabı** penceresi.

> [!NOTE] 
> Şu anda yapılandırılmış bir Azure depolama hesabı ile uygulama paketlerini kullanamaz [güvenlik duvarı kuralları](../storage/common/storage-network-security.md).
> 

Batch hizmeti, Azure depolama blok blobları olarak uygulama paketlerinizi depolamak için kullanır. İşiniz [normal olarak ücret] [ storage_pricing] için blok blobu veri ve her paket boyutunu aşamaz [en büyük blok blob boyutu](../storage/common/storage-scalability-targets.md#azure-blob-storage-scale-targets). Uygulama paketlerinizi sayısı ve boyutu göz önünde bulundurun ve düzenli aralıklarla maliyetleri en aza indirmek için kullanım dışı paketlerini kaldırmak emin olun.
> 
> 

### <a name="view-current-applications"></a>Geçerli uygulamaları görüntüle
Batch hesabınızda uygulamaları görüntülemek için tıklayın **uygulamaları** menü öğesi görüntüleme çalışırken soldaki menüde, **Batch hesabı**.

![Uygulama kutucuğu][2]

Bu menü seçeneğini belirleyerek açılır **uygulamaları** penceresi:

![Uygulamaları listeleme][3]

Bu pencereyi her uygulama Kimliğini hesabınızı ve aşağıdaki özellikleri görüntüler:

* **Paketleri**: Bu uygulamayla ilişkili sürüm sayısı.
* **Varsayılan sürüm**: Uygulama havuzu için belirttiğiniz zaman, bir sürüm belirtmiyorsa yüklü uygulama sürümü. Bu ayar isteğe bağlıdır.
* **Güncelleştirmelere izin ver**: Paket olup olmadığını güncelleştirir, silme ve ekleme belirten değeri izin verilir. Bu ayarlanırsa **Hayır**, paket güncelleştirmeleri ve silme işlemleri uygulama için devre dışıdır. Yalnızca yeni uygulama paketi sürümleri eklenebilir. Varsayılan değer **Evet**’tir.

Uygulama paketi dosyası yapısını, işlem düğümü üzerinde görmek istiyorsanız, portalda Batch hesabınıza gidin. Batch hesabınızdaki e gidin **havuzları**. İlgilendiğiniz işlem düğümlerini içeren bir havuz seçin.

![Havuzdaki düğümler][13]

Havuzunuz seçtikten sonra uygulama paketi yüklendiğinde işlem düğümüne gidin. Uygulama paketi ayrıntılarını bulunan buradan **uygulamaları** klasör. Ek klasörleri işlem düğümünde başlangıç görevleri, Çıkış dosyalarını, hata çıktısı, vb. gibi diğer dosyaları içerir.

![Düğümdeki dosyalar][14]

### <a name="view-application-details"></a>Uygulama ayrıntılarını görüntüle
Uygulama ayrıntılarını görmek için uygulamayı seçin. **uygulamaları** penceresi.

![Uygulama Ayrıntıları][4]

Uygulama ayrıntıları, uygulamanız için aşağıdaki ayarları yapılandırabilirsiniz.

* **Güncelleştirmelere izin ver**: Uygulama paketleri, güncelleştirildi veya silinebilir olup olmadığını belirtin. "Güncelleştirme veya uygulama paketi silme" Bu makalenin ilerleyen bölümlerinde bkz.
* **Varsayılan sürüm**: İşlem düğümlerine dağıtmak için bir varsayılan uygulama paketini belirtin.
* **Görünen ad**: Batch çözümünüz uygulamayla ilgili bilgileri gibi Batch aracılığıyla müşterilerinize sağlayan bir hizmetin kullanıcı arabirimini gösterildiğinde kullanabileceğiniz bir kolay ad belirtin.

### <a name="add-a-new-application"></a>Yeni uygulama Ekle
Yeni bir uygulama oluşturmak için bir uygulama paketi ekleme ve yeni, benzersiz bir uygulama kimliği belirtin. Yeni uygulama kimliği ile eklediğiniz ilk uygulama paketini, yeni uygulama da oluşturur.

**Uygulamalar** > **Ekle**’ye tıklayın.

![Azure portalında yeni uygulama dikey penceresi][5]

**Yeni uygulama** penceresi, yeni uygulama ve uygulama paketinin ayarlarını belirtmek için aşağıdaki alanları sağlar.

**Uygulama Kimliği**

Bu alan, standart Azure toplu işlem kimliği doğrulama kurallarına tabidir, yeni uygulama Kimliğini belirtir. Bir uygulama kimliği sağlamak için kurallar aşağıdaki gibidir:

* Windows düğümlerine kimlik herhangi bir birleşimini alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir. Linux düğümlerinde yalnızca alfasayısal karakterler ve alt çizgi izin verilir.
* 64'ten fazla karakter içeremez.
* Batch hesabı içinde benzersiz olmalıdır.
* Çalışması koruyarak ve büyük küçük harf duyarsız ' dir.

**Sürüm**

Bu alan karşıya yüklemekte olduğunuz uygulama paketi sürümünü belirtir. Sürüm dizeleri aşağıdaki doğrulama kurallarına tabidir şunlardır:

* Windows düğümlerine sürüm dizesi alfasayısal karakterler, tire, alt çizgi ve dönemleri herhangi bir birleşimini içerebilir. Linux düğümlerinde Sürüm dizesinde yalnızca alfasayısal karakterler ve alt çizgi içerebilir.
* 64'ten fazla karakter içeremez.
* Uygulama içinde benzersiz olmalıdır.
* Çalışması koruyarak ve büyük küçük harf duyarsız'dır.

**Uygulama paketi**

Bu alan, uygulamayı çalıştırmak için gerekli destekleyici dosyaları ve uygulama ikili dosyalarını içeren .zip dosyasını belirtir. Tıklayın **bir dosya seçin** kutusu ya da klasör simgesini bulun ve uygulamanızın dosyaları içeren bir .zip dosyası seçin.

Bir dosyayı seçtikten sonra tıklayın **Tamam** Azure Depolama'ya karşıya yüklemeyi başlatmak için. Karşıya yükleme işlemi tamamlandığında, portal bir bildirim görüntüler. Karşıya yüklemekte olduğunuz dosyanın boyutu ve ağ bağlantınızın hızına bağlı olarak, bu işlem biraz zaman alabilir.

> [!WARNING]
> Kapatmayın **yeni uygulama** karşıya yükleme işlemi tamamlanmadan önce penceresi. Bunun yapılması, karşıya yükleme işlemi durdurur.
> 
> 

### <a name="add-a-new-application-package"></a>Yeni bir uygulama paketi ekleme
Mevcut bir uygulama için bir uygulama Paket sürümü eklemek için uygulamayı seçin. **uygulamaları** windows seçeneğine tıklayıp **paketleri** > **Ekle**.

![Azure portalında uygulama paketi dikey penceresi ekleme][8]

Gördüğünüz gibi alanları eşleşen **yeni uygulama** penceresinde ancak **uygulama kimliği** kutusunu devre dışı bırakıldı. Yeni uygulama için yaptığınız gibi belirtin **sürüm** yeni paketiniz için göz atın, **uygulama paketini** .zip dosyası,'a tıklayın **Tamam** paketini karşıya yüklemek için.

### <a name="update-or-delete-an-application-package"></a>Güncelleştirme veya uygulama paketi silme
Var olan bir uygulama paketini silmek ve güncelleştirmek için uygulama ayrıntılarını açın, **paketleri**, tıklayın **üç nokta** değiştirmek ve istediğiniz uygulama paketini satırında gerçekleştirmek istediğiniz eylemi.

![Güncelleştirme veya Azure portalında paket silme][7]

**Güncelleştirme**

Tıkladığınızda **güncelleştirme**, **güncelleştirme paketini** windows görüntülenir. Bu pencere benzer **yeni uygulama paketini** penceresi, ancak yalnızca paket seçim alanı etkindir, karşıya yüklemek için yeni bir ZIP dosyası belirtmenize olanak sağlar.

![Azure portalında güncelleştirme paketi dikey penceresi][11]

**Silme**

Tıkladığınızda **Sil**, Paket sürümü silme işlemini onaylamak için istenir ve Batch, Azure Depolama'dan paket siler. Bir uygulamanın varsayılan sürümünü silerseniz **varsayılan sürüm** ayarını, uygulama için kaldırılır.

![Uygulamayı Sil ][12]

## <a name="install-applications-on-compute-nodes"></a>İşlem düğümlerinde uygulamaları yükleme
Azure portal ile uygulama paketlerini yönetmek öğrendiniz, işlem düğümleri ve bunları Batch görevleri çalıştırmak için bunları dağıtma tartışabiliriz.

### <a name="install-pool-application-packages"></a>Havuz uygulama paketleri yükle
Bir havuzdaki tüm işlem düğümlerinde bir uygulama paketini yüklemek için bir veya daha fazla uygulama paketini belirtin. *başvuruları* havuz için. Havuz için belirttiğiniz uygulama paketleri, düğüm havuza katıldığında ve düğüm yeniden başlatıldığında veya her işlem düğümünde yüklenir.

Batch .NET içinde bir veya daha fazla belirtin [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] yeni bir havuz oluşturduğunuzda veya mevcut bir havuz için. [ApplicationPackageReference] [ net_pkgref] sınıfı, bir uygulama Kimliğini ve sürümünü yüklemek bir havuzun işlem düğümleri belirtir.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

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
> Bir uygulama paketi dağıtımı için herhangi bir nedenle başarısız olursa, Batch hizmeti düğüm işaretler [kullanılamaz][net_nodestate], ve hiçbir görev o düğümde yürütme için zamanlanır. Bu durumda, yapmanız gerekenler **yeniden** düğüm paketi dağıtımı yeniden başlatın. Düğümü yeniden başlatmak, ayrıca görev düğümde yeniden zamanlama sağlar.
> 
> 

### <a name="install-task-application-packages"></a>Görev uygulama paketleri yükle
Benzer şekilde bir havuz, uygulama paketi belirttiğiniz *başvuruları* için bir görev. Bir görevi bir düğümde çalışmak üzere zamanlandığında paket indirilir ve yalnızca görevin komut satırı yürütülmeden önce ayıklanır. Belirtilen paket ve sürümü zaten yüklü değilse düğüm üzerinde paket yüklenmez ve var olan paketi kullanılır.

Bir görev uygulama paketi yüklemek için görev yapılandırma [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] özelliği:

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

## <a name="execute-the-installed-applications"></a>Yüklü uygulamalar yürütün
Bir havuzu ya da görev için belirlediğiniz paketleri indirilir ve adlandırılmış bir dizin içinde ayrıştırıcısıdır `AZ_BATCH_ROOT_DIR` düğümünün. Batch ayrıca adlandırılmış dizin yolunu içeren bir ortam değişkeni oluşturur. Görev komut satırları, düğümdeki uygulama başvururken bu ortam değişkenini kullanın. 

Windows düğümlerine, değişkeni aşağıdaki biçimdedir:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Linux düğümlerinde biçimi biraz farklıdır. Nokta (.), tire (-) ve sayı işaretleri (#) için alt çizgi ortam değişkenindeki düzleştirilir. Ayrıca, uygulama kimliğinin servis talebi korunur unutmayın. Örneğin:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

`APPLICATIONID` ve `version` dağıtımı için belirtilen uygulama ve Paket sürümü karşılık gelen değerlerdir. Örneğin, uygulamanın bu sürümü 2.7 belirtilmişse *blender* yüklenmelidir Windows düğümlerine, kendi dosyalarına erişmek için bu ortam değişkeni, görev komut satırları kullanırsınız:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Linux düğümlerinde ortam değişkeni şu biçimde belirtin. Nokta (.), tire (-) düzleştirmek sayı işaretleri (#) için alt çizgi ve uygulama kimliğinin servis talebi Koru:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
``` 

Bir uygulama paketini karşıya yüklediğinizde, işlem düğümlerine dağıtmak için bir varsayılan sürümü belirtebilirsiniz. Bir uygulama için varsayılan sürüm belirttiyseniz, uygulama başvurduğunuzda Sürüm soneki atlayabilirsiniz. Azure portalında, varsayılan uygulama sürümü belirleyebilirsiniz **uygulamaları** penceresinde gösterildiği gibi [karşıya yüklemek ve uygulamaları yönetmek](#upload-and-manage-applications).

Örneğin, "2.7" uygulama için varsayılan sürüm olarak ayarlarsanız *blender*ve görevlerinizi aşağıdaki ortam değişkeni başvurusu ve ardından Windows düğümlerinizi sürüm 2.7 çalıştırır:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Varsayılan sürümü başlatan bir örnek görev komut satırı aşağıdaki kod parçacığı gösterir *blender* uygulama:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Bkz: [görevler için ortam ayarları](batch-api-basics.md#environment-settings-for-tasks) içinde [Batch özelliğine genel bakış](batch-api-basics.md) işlem düğümü ortam ayarları hakkında daha fazla bilgi.
> 
> 

## <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme
Mevcut havuzlardan bir uygulama paketi ile yapılandırılmışsa, havuz için yeni bir paket belirtebilirsiniz. Aşağıdaki durumlardan bir havuz için yeni bir paket başvurusu belirtirseniz:

* Batch hizmeti, havuza katılan tüm yeni düğümler ve herhangi bir düğümde, yeniden başlatıldığında veya var olan yeni belirtilen paket yükler.
* İşlem düğümleri paket başvuruları güncelleştirdiğinizde, havuzda zaten olan yeni uygulama paketini otomatik olarak yüklemeyin. Bu, düğümler yeniden veya yeni paketi almaya görüntüsü yeniden oluşturulabildiği işlem.
* Yeni bir paket dağıtıldığında, yeni uygulama paket başvuruları oluşturulan ortam değişkenlerini yansıtır.

Bu örnekte, mevcut havuzu 2.7 sürümüne sahip *blender* biri olarak yapılandırılmış bir uygulama, [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref]. Havuzun düğümlerine 2.76b sürümüyle güncelleştirmek için yeni bir belirtin [ApplicationPackageReference] [ net_pkgref] yeni sürüm ve işleme Değiştir.

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

Yeni sürüm yapılandırıldı, Batch hizmeti sürüm 2.76b herhangi yükler *yeni* havuza katıldığında düğümü. 2.76b olan düğümlerine yüklemek için *zaten* havuzda yeniden başlatma veya bunları görüntüsünü yeniden oluştur. Yeniden başlatılan düğümleri önceki paket dağıtımlar dosyaları korumak unutmayın.

## <a name="list-the-applications-in-a-batch-account"></a>Bir Batch hesabında uygulamalar listesi
Kullanarak uygulamalar ve bir Batch hesabında paketlerini listeleyebilirsiniz [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] yöntemi.

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

## <a name="wrap-up"></a>Kaydır
Uygulama paketlerinde, müşterilerinizin işlerini uygulamaları seçin ve Batch özellikli hizmetinizi işlerle işlerken kullanılacak tam sürümünü belirtin yardımcı olabilir. Karşıya yükleme ve hizmet kendi uygulamaları izlemek müşterileriniz için özelliği de sağlayabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Batch REST API'si] [ api_rest] de uygulama paketleri ile çalışmak için destek sağlar. Örneğin, [applicationPackageReferences] [ rest_add_pool_with_packages] öğesinde [hesaba havuz ekleme] [ rest_add_pool] belirtme hakkında daha fazla bilgi için REST API kullanarak yüklemek için paketler. Bkz: [uygulamaları] [ rest_applications] Batch REST API'sini kullanarak uygulama bilgileri elde etme hakkında ayrıntılar için.
* Bilgi edinmek için nasıl program aracılığıyla [toplu işlem yönetimi .NET ile Azure Batch hesaplarını ve kotalarını yönetme](batch-management-dotnet.md). [Toplu işlem yönetimi .NET] [ api_net_mgmt] kitaplığı, toplu uygulama veya hizmet hesabı oluşturma ve silme özellikleri etkinleştirebilirsiniz.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
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
[2]: ./media/batch-application-packages/app_pkg_02.png "Azure portalında uygulama kutucuğu"
[3]: ./media/batch-application-packages/app_pkg_03.png "Azure portalında uygulamalar dikey penceresi"
[4]: ./media/batch-application-packages/app_pkg_04.png "Azure portalında uygulama ayrıntıları dikey penceresi"
[5]: ./media/batch-application-packages/app_pkg_05.png "Azure portalında yeni uygulama dikey penceresi"
[7]: ./media/batch-application-packages/app_pkg_07.png "Güncelleştirme veya açılan Azure Portalı'nda paketleri silme"
[8]: ./media/batch-application-packages/app_pkg_08.png "Azure portalında yeni uygulama paketini dikey penceresi"
[9]: ./media/batch-application-packages/app_pkg_09.png "Bağlantılı hiçbir depolama hesabı Uyarısı"
[10]: ./media/batch-application-packages/app_pkg_10.png "Azure portalında depolama hesabı dikey penceresi seçin"
[11]: ./media/batch-application-packages/app_pkg_11.png "Azure portalında güncelleştirme paketi dikey penceresi"
[12]: ./media/batch-application-packages/app_pkg_12.png "Paket onay iletişim kutusunda Azure portalında silme"
[13]: ./media/batch-application-packages/package-file-structure.png "Azure portalında düğümü bilgi işlem"
[14]: ./media/batch-application-packages/package-file-structure-node.png "Azure Portalı'nda görüntülenen işlem düğümünde dosyaları"