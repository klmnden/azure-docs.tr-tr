---
title: Özellikler - Azure Batch işleme
description: Azure batch'te belirli işleme özellikleri
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: be6c0f9a8874507433606903bcbd58c7723d6a8a
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62118696"
---
# <a name="azure-batch-rendering-capabilities"></a>Azure Batch işleme özellikleri

Standart Azure Batch özellikleri, işleme iş yüklerini ve uygulamaları çalıştırmak için kullanılır. Toplu işleme iş yüklerini desteklemek için belirli özellikler de içerir.

Havuzlar, işler ve görevler gibi Batch kavramları, genel bakış için bkz. [bu makalede](https://docs.microsoft.com/azure/batch/batch-api-basics).

## <a name="batch-pools"></a>Batch havuzları

### <a name="rendering-application-installation"></a>Uygulama yüklemesini oluşturma

Azure Market işleme VM görüntüsü, önceden yüklenmiş uygulamalara gereksinimini yalnızca havuzu yapılandırması belirtilebilir.

Bir Windows 2016 görüntüsü ve bir CentOS görüntüsü yok.  İçinde [Azure Marketi](https://azuremarketplace.microsoft.com), VM görüntüleri 'için batch rendering' arayarak bulabilirsiniz.

Bir örnek havuzu yapılandırma için bkz: [Azure CLI işleme öğretici](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli).  Azure portalı ve Batch Gezgini, bir havuz oluşturduğunuzda, bir işleme VM görüntüsü seçme için GUI araçları sağlar.  Batch API'sini kullanarak, aşağıdaki özellik değerlerini belirtmeniz [Imagereference](https://docs.microsoft.com/rest/api/batchservice/pool/add#imagereference) havuz oluştururken:

| Yayımcı | Sunduğu | Sku | Version |
|---------|---------|---------|--------|
| toplu iş | işleme centos73 | işleme | en son |
| toplu iş | rendering-windows2016 | işleme | en son |

VM'ler havuzda ek uygulamalar gerekirse, diğer seçenekler kullanılabilir:

* Standart bir Market görüntü temel alan özel bir görüntü:
  * Bu seçeneği kullanarak VM'nizi ihtiyacınız olan uygulamalar ve sürümlerle yapılandırabilirsiniz. Daha fazla bilgi için [sanal makine havuzu oluşturmak için özel görüntü kullanma](https://docs.microsoft.com/azure/batch/batch-custom-images). Autodesk ve Chaos Group Arnold ve V-Ray, sırasıyla, bir Azure Toplu Lisanslama hizmeti karşı doğrulamak için değiştirdiniz. Kullandıkça Öde lisansının çalışması bu desteği, aksi takdirde bu uygulamaların sürümlerine sahip olduğunuzdan emin olun. Geçerli sürümler Maya veya 3ds Max yok gerektiren lisans sunucusu (toplu iş/komut satırı modunda) gözetimsiz çalışırken. Bu seçenekle nasıl emin değilseniz Azure desteğine başvurun.
* [Uygulama paketleri](https://docs.microsoft.com/azure/batch/batch-application-packages):
  * Paket bir veya daha fazla ZIP dosyalarını kullanarak uygulama dosyaları, Azure portal aracılığıyla karşıya yükleme ve havuzu yapılandırması paketi belirtin. Havuzu Vm'leri oluşturulduğunda, ZIP dosyaları karşıdan yüklenir ve dosyaları ayıklanır.
* Kaynak dosyaları:
  * Uygulama dosyaları, Azure blob depolama alanına yüklenir ve dosya başvuruları belirttiğiniz [havuzunu Başlat görev](https://docs.microsoft.com/rest/api/batchservice/pool/add#starttask). Kaynak dosyaları, havuzu Vm'leri oluştururken, her VM indirilir.

### <a name="pay-for-use-licensing-for-pre-installed-applications"></a>Uygulamalar için Kullandıkça Öde lisansı önceden yüklenmiş

Kullanılacak ve lisans ücreti olan uygulamalar havuz yapılandırmasına belirtilmesi gerekir.

* Belirtin `applicationLicenses` özelliği olduğunda [bir havuz oluşturma](https://docs.microsoft.com/rest/api/batchservice/pool/add#request-body).  Aşağıdaki değerleri dizisini - "vray", "arnold", "3dsmax", "maya" belirtilebilir.
* Daha sonra bir veya daha fazla uygulama belirttiğinizde, sanal makinelerinin maliyeti için söz konusu uygulamaların maliyeti eklenir.  Uygulama fiyatları listelenen [Azure fiyatlandırma sayfasını Batch](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

> [!NOTE]
> Bunun yerine, işleme uygulamalarını kullanmak için bir lisans sunucusuna bağlanmanız durumunda, belirtmeyin `applicationLicenses` özelliği.

Azure portalı ya da Batch Gezgini, uygulamaları seçin ve uygulama fiyatlar göstermek için kullanabilirsiniz.

Bir uygulamayı kullanmak için bir girişimde, ancak uygulama içinde belirtilmedi `applicationLicenses` havuz yapılandırmasını veya yoksa lisans sunucusu, uygulama yürütme başarısız olursa bir lisans hatası ve sıfır olmayan çıkış kodu ile ulaşma özelliği.

### <a name="environment-variables-for-pre-installed-applications"></a>Önceden yüklenmiş uygulamalara için ortam değişkenleri

İşleme görevleri için komut satırının oluşturabilmek için işleme uygulama yürütülebilir dosyaları yükleme konumunu belirtilmelidir.  Sistem ortam değişkenlerini gerçek yolları belirtmek zorunda yerine kullanılabilir Azure Market VM görüntülerini üzerinde oluşturulmuştur.  Ek olarak bu ortam değişkenleri olan [standart Batch ortam değişkenlerini](https://docs.microsoft.com/azure/batch/batch-compute-node-environment-variables) her görev için oluşturuldu.

|Uygulama|Uygulama yürütülebilir|Ortam Değişkeni|
|---------|---------|---------|
|Autodesk 3ds Max 2018|3dsmaxcmdio.exe|3DSMAX_2018_EXEC|
|Autodesk 3ds Max 2019|3dsmaxcmdio.exe|3DSMAX_2019_EXEC|
|Autodesk Maya 2017|Render.exe|MAYA_2017_EXEC|
|Autodesk Maya 2018|Render.exe|MAYA_2018_EXEC|
|Chaos Group V-Ray tek başına|vray.exe|VRAY_3.60.4_EXEC|
ARNOLD 2017 komut satırı|kick.exe|ARNOLD_2017_EXEC|
|ARNOLD 2018 komut satırı|kick.exe|ARNOLD_2018_EXEC|
|Blender'ı|Blender.exe|BLENDER_2018_EXEC|

### <a name="azure-vm-families"></a>Azure VM aileleri

Diğer iş yükleri gibi işleme uygulaması sistem gereksinimleri farklılık gösterir ve işleri ve projeleri için performans gereksinimleri farklılık gösterir.  Çok çeşitli VM aileleri maliyeti Azure gereksinimlerinizi – en düşük bağlı olarak, en iyi fiyat/performansından, en iyi performans ve benzeri kullanılabilir.
Arnold gibi bazı işleme uygulamaları CPU tabanlıdır; V-Ray ve Blender döngüleri gibi diğer CPU ve/veya GPU'ları kullanabilirsiniz.
Kullanılabilir VM aileleri ve VM boyutları açıklamasını [VM türleri ve boyutları görmek](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="low-priority-vms"></a>Düşük öncelikli sanal makineler

Diğer iş yükleri ile gibi düşük öncelikli VM'ler işleme için Batch havuzlarında yararlanılabilir.  Düşük öncelikli VM'ler, normal özel VM'ler ile aynı gerçekleştirin ancak Azure kapasiteden yararlanmak ve büyük bir indirim için kullanılabilir.  Düşük öncelikli VM'ler kullanma zorunluluğunu getirir, bu sanal makineler ayrılacak kullanılamıyor olabilir veya kullanılabilir kapasiteye bağlı olarak herhangi bir zamanda etkisiz hale getirilebilir ' dir. Bu nedenle, düşük öncelikli VM'ler için tüm işleme işlerini uygun olacağı değildir. Görüntüleri büyük olasılıkla daha sonra işlemek için kaç saat sürerse gibi kesintiye ve boşaltılması Vm'leri nedeniyle yeniden başlatılması bu görüntülerin işlenmesi ilgili kabul edilebilir değildir.

Düşük öncelikli VM'ler ve bunları yapılandırmak için çeşitli yollar özellikleri hakkında daha fazla bilgi için Batch kullanırken, bkz: [Batch ile düşük öncelikli VM'ler kullanma](https://docs.microsoft.com/azure/batch/batch-low-pri-vms).

## <a name="jobs-and-tasks"></a>İşleri ve görevleri

Hiçbir işleme özgü desteği, işleri ve görevleri için gereklidir.  Ana yapılandırma öğesi gerekli uygulama başvurmak için gereken görev komut satırı ' dir.
Azure Market VM görüntülerini kullanıldığında, en iyi yöntem, yürütülebilir uygulama ve yolunu belirtmek için ortam değişkenlerini kullanma olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme deneme iki öğreticiler, örnekler için:

* [Azure CLI kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-cli)
* [Batch Explorer'ı kullanarak oluşturma](https://docs.microsoft.com/azure/batch/tutorial-rendering-batchexplorer-blender)
