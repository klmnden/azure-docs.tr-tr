---
title: Oluşturma - Azure Batch için depolama ve veri taşıma
description: İşleme iş yükleri için depolama ve veri taşıma seçenekleri
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 5a0d4dc82995e63697cc673bc54695c9c6d586df
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60774004"
---
# <a name="storage-and-data-movement-options-for-rendering-asset-and-output-files"></a>Varlık ve Çıkış dosyalarını işlemek için depolama ve veri taşıma seçenekleri

Sahne ve varlık dosyaları Vm'leri havuzu işleme uygulamaları için kullanılabilir hale getirme için birden çok seçenek vardır:

* [Azure blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction):
  * Sahne ve varlık dosyaları, BLOB depolamaya bir yerel dosya sisteminden yüklenir. Bir görev tarafından uygulamayı çalıştırdığınızda, işleme uygulama tarafından erişilecek şekilde ardından gerekli dosyaları blob depolama sanal kopyalanır. Çıkış dosyalarının VM disk işleme uygulama tarafından yazılan ve ardından blob depolama alanına kopyalanır.  Gerekirse, çıktı dosyaları blob depolama alanından bir yerel dosya sistemine yüklenebilir.
  * Azure blob depolama, daha küçük projeler için basit ve ekonomik bir seçenektir.  Tüm varlık sayısı ve varlık dosyalarının boyutunu artırır sonra dosyaları her VM havuzunda ardından gerekli bakım gereksinimlerini dosya aktarımları sağlamak için yapılması mümkün olduğunca etkili.  
* Azure depolama kullanarak bir dosya sistemi olarak [blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux):
  * Linux VM'ler için bir depolama hesabı kullanıma sunulan ve blobfuse sanal dosya sistemi sürücüsü kullanıldığında bir dosya sistemi olarak kullanılır.
  * Bu seçenek olarak çok maliyetli olan kullanabilmenizi hiçbir VM dosya sistemi için gerekli olan yanı sıra Vm'lerde blobfuse önbelleğe alma aynı dosyaları birden çok işler ve görevler için yinelenen indirmeleri önler.  Veri taşıma, ayrıca dosyaların yalnızca bloblardır ve standart API'ler ve Araçlar, azcopy gibi bir şirket içi dosya sistemi ve Azure depolama arasında dosya kopyalamak için kullanılabilir olarak basittir.
* Dosya sistemi veya dosya paylaşımı:
  * VM işletim sistemi ve performans/ölçek gereksinimlerine bağlı olarak, daha sonra seçenekleriniz [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)bağlı diskleri olan kullanarak bir VM için NFS, GlusterFS gibi bir dağıtılmış dosya sistemi için bağlı diskleri olan birden fazla VM kullanarak veya bir üçüncü taraf teklifini kullanarak.
  * [Avere sistemleri](https://www.averesystems.com/) artık Microsoft'un bir parçası olan ve yakın gelecekte büyük ölçekli, yüksek performanslı işleme için ideal çözüm olacaktır.  Azure tabanlı bir NFS Avere çözüm etkinleştirir veya SMB önbellek, oluşturulacak veya şirket içi NAS cihazları blob depolama ile birlikte çalışır.
  * Bir dosya sistemi ile dosyaları okunabilir ve doğrudan dosya sistemine yazılması veya dosya sistemi ve havuzu VM'ler arasında kopyalanabilir.
  * Paylaşılan bir dosya sistemi, çok sayıda projeleri ve işleri arasında paylaşılan varlıklar, yalnızca gerekli olan erişim görevleri rendering'i kullanılmasına olanak sağlar.

## <a name="using-azure-blob-storage"></a>Azure blob depolama kullanma

Blob depolama hesabı veya genel amaçlı v2 depolama hesabı kullanılmalıdır.  Bu iki depolama hesabı türleri içinde ayrıntılı olarak bir genel amaçlı v1 depolama hesabına kıyasla önemli ölçüde daha yüksek sınırlar yapılandırılabilir [bu blog gönderisini](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).  Özellikle çok sayıda havuzu VM depolama hesabına erişilirken olduğunda yapılandırıldığında, daha yüksek sınırlar çok daha iyi performans ve ölçeklenebilirlik, sağlayacaktır.

### <a name="copying-files-between-client-and-blob-storage"></a>İstemci ve blob depolama arasında dosyaları kopyalanıyor

Azure depolama gelen ve giden dosyaları kopyalamak için çeşitli mekanizmalar depolama blobu API dahil olmak üzere kullanılabilir [Azure Storage veri hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement), azcopy komut satırı aracını [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux), [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), ve [Azure Batch Gezgini](https://azure.github.io/BatchExplorer/).

Örneğin, azcopy kullanarak, bir klasördeki tüm varlıkları aşağıdaki gibi aktarılabilir:


`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /Y`

Yalnızca değiştirilen dosyaları kopyalamak için /XO parametre kullanılabilir:

`azcopy /source:. /dest:https://account.blob.core.windows.net/rendering/project /destsas:"?st=2018-03-30T16%3A26%3A00Z&se=2020-03-31T16%3A26%3A00Z&sp=rwdl&sv=2017-04-17&sr=c&sig=sig" /XO /Y`

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Batch havuzu Vm'leri için blob depolamadan kopyalama giriş varlık dosyaları

Birkaç iş varlıkları boyutuna göre belirlenen en iyi yaklaşım ile dosyaları kopyalamak için farklı yaklaşımlara vardır.
Her iş için havuz Vm'leri tüm varlık dosyaları kopyalamak için en basit yaklaşımdır bakın:

* Bir projeye benzersiz dosya olmadığında, ancak bir işin tüm görevleri için gerekli olan bir [iş hazırlama görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask) tüm dosyaları kopyalamak için belirtilebilir.  İlk iş görevi bir VM'de çalıştırılır, ancak sonraki iş görevleri için yeniden çalıştırılmaz iş hazırlama görevi bir kez çalıştırın.
* A [iş bırakma görevi](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask) iş tamamlandığında iş başına dosyaları kaldırmak için belirtilmelidir; Bu, bütün iş varlık dosyaları göre doldurulmuş VM disk önlenmiş olur.
* Her iş için varlıklar için yalnızca artımlı değişiklikler aynı varlıkları kullanarak birden çok iş olduğunda bile yalnızca bir alt güncelleştirildi sonra tüm varlık dosyaları yine de kopyalanır.  Çok sayıda varlık büyük dosya olduğunda bu verimsiz olabilir.

Varlık dosyaları işlerle işleri arasında yalnızca artımlı değişiklikler arasında yeniden sonra daha verimli ancak biraz daha karmaşık bir yaklaşım VM üzerinde paylaşılan klasörde varlıkları depolamak ve değiştirilen dosyaları eşitlemek için andır.

* İş hazırlama görevi AZ_BATCH_NODE_SHARED_DIR ortam değişkeni tarafından belirtilen VM paylaşılan klasöre /XO parametresiyle azcopy kullanarak kopyalama gerçekleştirmelisiniz.  Bu yalnızca değiştirilen dosyaların her VM için kopyalar.
* Düşünce havuzu Vm'leri geçici sürücüde sığması sağlanır emin olmak için tüm varlıkların boyutuna verilmesi gerekir.

Azure Batch, bir depolama hesabı ve Batch havuzu Vm'leri arasında dosya kopyalamak için yerleşik destek sunmaktadır.  Görev [kaynak dosyaları](https://docs.microsoft.com/rest/api/batchservice/job/add#resourcefile) kopyalama havuzu Vm'leri için depolama biriminden dosyaları ve iş hazırlama görevi için belirtilebilir.  Ne yazık ki, dosyaları yüzlerce olduğunda ve başarısız görevler sınırlaması isabet mümkündür.  Çok sayıda varlık olduğunda, azcopy komut satırı joker karakterler kullanabilirsiniz ve sınır olan iş hazırlama görevi içinde kullanmak için önerilir.

### <a name="copying-output-files-to-blob-storage-from-batch-pool-vms"></a>BLOB Depolama'yı Batch havuzu Vm'leri Çıkış dosyalarını kopyalama

[Çıkış dosyalarını](https://docs.microsoft.com/rest/api/batchservice/task/add#outputfile) kullanılan dosyaların kopyalanacağı bir havuzu sanal makinesi için depolama olabilir.  Görev tamamlandıktan sonra belirtilen depolama hesabına VM'den bir veya daha fazla dosya kopyalanabilir.  İşlenen çıkışı kopyalanması gerekir, ancak de günlük dosyalarını depolamak için uygun olabilir.

## <a name="using-a-blobfuse-virtual-file-system-for-linux-vm-pools"></a>Linux sanal makine havuzlarında blobfuse sanal dosya sistemi kullanma

Blobfuse bloblar Linux dosya sistemi üzerinden bir depolama hesabında depolanan dosyalar erişmenize olanak sağlayan Azure Blob Depolama için sanal dosya sistemi sürücüsüdür.

Havuz düğümleri başlatıldığında dosya sistemine bağlayabilir veya bağlama, iş hazırlama görevi – bir işi ilk görevi bir düğümde çalıştığında yalnızca çalıştırdığınız bir görevin bir parçası olarak gerçekleşebilir.  Blobfuse bir ramdisk hem de birden çok görevi bir düğümde aynı dosyaların bazıları erişirseniz, performansı önemli ölçüde artıracak dosyaları önbelleğe alma için Vm'leri yerel SSD yararlanmak için yapılandırılabilir.

[Örnek şablonları kullanılabilir](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/vray/render-linux-with-blobfuse-mount) V-Ray blobfuse dosya sistemini kullanarak oluşturur ve diğer uygulamalar için şablonları için temel olarak kullanılan tek başına çalıştırılacak.

### <a name="accessing-files"></a>Dosyalara erişme

İş görevleri için girdi dosyalarını ve Çıkış dosyalarını kullanarak bağlı dosya sistemi yolları belirtin.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Batch havuzu Vm'leri için blob depolamadan kopyalama giriş varlık dosyaları

Yalnızca Azure Depolama'daki blobları dosyalar gibi ardından standart blob API'leri, araçları ve kullanıcı arabirimleri bir şirket içi dosya sistemi ve blob depolama arasında dosyaları kopyalamak için kullanılabilir; Örneğin, azcopy, Depolama Gezgini, Batch Gezgini, vs.

## <a name="using-azure-files-with-windows-vms"></a>Azure dosyaları ile Windows Vm'leri kullanma

[Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) tam olarak yönetilen dosya paylaşımları SMB protokolü aracılığıyla erişilebilen bulutta sunar.  Azure dosyaları, Azure blob depolama alanında dayanır; Bu [maliyet açısından verimli](https://azure.microsoft.com/pricing/details/storage/files/) ve bu nedenle Global yedekli başka bir bölgeye veri çoğaltma ile yapılandırılabilir.  [Ölçeklendirme hedeflerini](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#azure-files-scale-targets) Azure dosyaları tahmin havuzu boyutunda ve varlık dosyaların sayısı, verilen kullanılması gerekip gerekmediğini belirlemek için incelenmelidir.

Var. bir [blog gönderisi](https://blogs.msdn.microsoft.com/windowsazurestorage/2014/05/26/persisting-connections-to-microsoft-azure-files/) ve [belgeleri](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) kapsayan bir Azure dosya paylaşımını bağlayabilmeniz.

### <a name="mounting-an-azure-files-share"></a>Azure dosyaları paylaşımına bağlama

Batch hizmetinde kullanmak için bir bağlama işlemi bir görevde görevler arasında bağlantı kalıcı hale getirmek mümkün olmadığından her çalıştırdığınızda yapılması gerekiyor.  Bunu yapmanın en kolay yolu, başlangıç görevinin havuz yapılandırmasını kullanarak kimlik bilgilerini kalıcı hale getirmek ve ardından her görev önce paylaşımını cmdkey kullanmaktır.

Örnek kullanımını (JSON dosyası kullanmak için kaçış) havuzu şablonunda – cmdkey net kullanım çağrısından cmdkey çağrı ayırarak, başlangıç görevi için kullanıcı bağlamına görevleri çalıştırmak için kullanılan ile aynı olması gerektiğini unutmayın:

```
"startTask": {
  "commandLine": "cmdkey /add:storageaccountname.file.core.windows.net
    /user:AZURE\\markscuscusbatch /pass:storage_account_key",
  "userIdentity":{
    "autoUser": {
      "elevationLevel": "nonadmin",
      "scope": "pool"
    }
}
```

Örnek Proje Görev komut satırı:
```
"commandLine":"net use S:
  \\\\storageaccountname.file.core.windows.net\\rendering &
3dsmaxcmdio.exe -v:5 -rfw:0 -10 -end:10
  -bitmapPath:\"s:\\3dsMax\\Dragon\\Assets\"
  -outputName:\"s:\\3dsMax\\Dragon\\RenderOutput\\dragon.jpg\"
  -w:1280 -h:720
  \"s:\\3dsMax\\Dragon\\Assets\\Dragon_Character_Rig.max\""
```

### <a name="accessing-files"></a>Dosyalara erişme

İş görevleri için girdi dosyalarını ve Çıkış dosyalarını kullanarak ya da eşlenen bir sürücü veya UNC yolunu kullanarak bağlı dosya sistemi yolları belirtin.

### <a name="copying-input-asset-files-from-blob-storage-to-batch-pool-vms"></a>Batch havuzu Vm'leri için blob depolamadan kopyalama giriş varlık dosyaları

Azure dosyaları, tüm ana API'ler ve Azure depolama desteği olan araçlar tarafından desteklenir; Örneğin azcopy, Azure CLI, Depolama Gezgini, Azure PowerShell, Batch Gezgini, vs.

[Azure dosya eşitleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning) otomatik olarak dosyaları bir şirket içi dosya sistemi ile bir Azure dosya paylaşımı arasında eşitlemek kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Depolama seçenekleri hakkında daha fazla bilgi için ayrıntılı belgelere bakın:

* [Azure blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)
* [Blobfuse](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-mount-container-linux)
* [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)
