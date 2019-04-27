---
title: Azure Batch ve Batch Explorer'ı kullanarak Blender sahnesi işleme
description: 'Öğretici: Azure Batch ve Batch Explorer istemci uygulamasını kullanarak bir Blender sahnesinden birden fazla kare işleme'
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: tutorial
ms.openlocfilehash: 8a512676ab0e56f51c0fb9c59f2e530cfcf73333
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60617682"
---
# <a name="tutorial-render-a-blender-scene-using-batch-explorer"></a>Öğretici: Batch Gezgini'ni kullanarak Blender'ı bir Sahneyi işleme

Bu öğreticide bir Blender tanıtım sahnesinden birden fazla kare işleme adımları gösterilmektedir. Hem istemci hem de işleme VM'lerinde kullanımı ücretsiz olduğundan öğreticide Blender kullanılmıştır ancak işlem Maya veya 3ds Max gibi diğer uygulamalarda da benzer olacaktır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Azure depolamasına Blender sahnesi yükleme
> * İşlemeyi gerçekleştirmek için birden fazla düğüme sahip Batch havuzu oluşturma
> * Birden çok çerçeve işleme
> * İşlenen kare dosyalarını görüntüleme ve indirme

## <a name="prerequisites"></a>Önkoşullar

Batch’teki işleme uygulamalarını kullandığın kadar öde esasıyla kullanmak için bir kullandıkça öde aboneliğine veya diğer Azure satın alma seçeneğine ihtiyacınız vardır. Para kredi sağlayan ücretsiz bir Azure teklifi kullanıyorsanız, kullandığın kadar öde lisansı desteklenmez.

İlişkilendirilmiş depolama hesabına sahip bir Azure Batch hesabına ihtiyacınız vardır.  Batch hesabı oluşturmak için [CLI makalesi](https://docs.microsoft.com/azure/batch/quick-create-cli) gibi Batch Hızlı Başlangıç makalelerinden birine bakın.

Bu öğreticide belirtilen VM boyutu ve VM sayısı için en az 50 çekirdekten oluşan düşük öncelikli bir çekirdek kotası yeterli olacaktır. Varsayılan kota kullanılabilir ancak bu durumda daha küçük bir VM boyutu kullanılması gerekir ve görüntüleri işleme süreci daha uzun sürer. Artırılmış çekirdek kotası isteme işlemi [bu makalede](https://docs.microsoft.com/azure/batch/batch-quota-limit) anlatılmıştır.

Son olarak [Batch Explorer](https://azure.github.io/BatchExplorer/)'ın yüklenmesi gerekir. Windows, OSX ve Linux sürümleri mevcuttur. Bu adım isteğe bağlıdır ancak [Blender](https://www.blender.org/download/) yüklüyse aynı örnek model görüntülenebilir.

## <a name="download-the-demo-scene"></a>Tanıtım sahnesini indirme

[Blender Demo Files web sayfasından](https://www.blender.org/download/demo-files/) "Class room" tanıtım ZIP dosyasını indirin ve yerel sürücüye çıkarın.

## <a name="upload-a-scene-to-azure-storage"></a>Azure depolamasına sahne yükleme

Tanıtım amaçlı sahne dosyaları için bir depolama hesabı kapsayıcısı oluşturun:

* Batch Explorer'ı başlatın
* Sol taraftaki ana menüden 'Data' (Veri) menü öğesini seçin.
* Açılan menüde 'File groups' (Dosya grupları) öğesinin seçildiğinden emin olun.
* '+' düğmesini seçin ve 'blender-classroom' adlı yeni bir 'Empty file group' (Boş dosya grubu) oluşturun
  * Dosya grubu aslında 'fgrp-' ön ekine sahip bir Azure Depolama blob kapsayıcısıdır. Depolama hesabındaki diğer kapsayıcıları filtrelemek için bu kullanım benimsenmiştir

![Sahne dosyaları için dosya grubu](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_scene_filegroup.png)

Sahne dosyalarını yükleyin:

* Yeni kapsayıcıyı seçin ve 'classroom' klasörünün içeriğini sürükleyip Batch Explorer'daki kapsayıcıya bırakın.

![Yüklenen sahne dosyaları](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_scene_filegroup_uploaded.png)

## <a name="create-azure-storage-container-for-output-images"></a>Çıkış görüntüleri için Azure depolama kapsayıcısı oluşturma

Tanıtım amaçlı sahne çıkış dosyaları için bir depolama hesabı kapsayıcısı oluşturun:

* Sol taraftaki ana menüden 'Data' (Veri) menü öğesini seçin.
* '+' düğmesini seçin ve 'render-output' adlı yeni bir 'Empty file group' (Boş dosya grubu) oluşturun

![Çıkış dosyaları için dosya grubu](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_filegroup.png)

## <a name="create-a-pool-of-vms-for-rendering"></a>İşleme için VM havuzu oluşturma

Blender uygulamasını içeren Azure Marketplace VM görüntüsünü işleyen Batch havuzu oluşturma:

* Sol taraftaki ana menüden 'Gallery' (Galeri) menü öğesini seçin.
* Uygulama öğe listesinden 'Blender' öğesini seçin.
* Windows Server'da kare işlemek için gerekli öğeleri seçin
* Öğenin sağ tarafındaki bağlantı simgesini seçerek havuz ve iş oluşturmak için kullanılacak şablon dosyalarını görüntüleyebilirsiniz.

![Blender galeri öğeleri](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_gallery_item.png)

* ‘Create pool for later use’ (Daha sonra kullanmak üzere havuz oluştur) düğmesini seçin
  * Havuz adını “blender-windows” olarak bırakın
  * ‘Dedicated vm count’ (Ayrılmış VM sayısı) değerini “0” olarak belirleyin
  * ‘Low priority vm count’ (Düşük öncelikli VM sayısı) değerini “3” olarak belirleyin
  * ‘Node size’ (Düğüm boyutu) değerini “Standard_F16” olarak belirleyin. Başka bir VM boyutu seçebilirsiniz ancak kare işleme süresi çekirdek sayısına göre değişecektir.
* Havuzu oluşturmak için yeşil düğmeyi seçin
  * Havuz neredeyse anında oluşturulur ancak VM'lerin ayrılması ve başlatılması birkaç dakika sürebilir.

![Blender için havuz şablonu](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_template.png)

> [!WARNING]
> Havuza eklenen VM'lerin maliyetinin Azure aboneliğinize yansıtılacağını unutmayın. Ücretlerin durdurulması için havuzun veya VM'lerin silinmesi gerekir. Ücret alınmasını önlemek için bu öğreticinin sonunda havuzu silin.

Havuz ve VM'lerin durumunu 'Havuzları' görünümü'nde izlenebilir; Aşağıdaki örnek, üç VM'nin tümünü ayrılan iki başlatılmış ve boşta olduğundan, bir hala başlatılıyor gösterir: ![Havuz ısı Haritası](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_heatmap.png)

## <a name="create-a-rendering-job"></a>İşleme işi oluşturma

Oluşturulan havuzu kullanarak kare işlemek için bir işleme işi oluşturun:
* Sol taraftaki ana menüden 'Gallery' (Galeri) menü öğesini seçin.
* Uygulama öğe listesinden 'Blender' öğesini seçin.
* Windows Server'da kare işlemek için gerekli öğeleri seçin.
* 'Run job with existing pool' (İşi var olan havuzla çalıştır) düğmesini seçin
* 'blender-windows' havuzunu seçin
* 'Job name' (İş adı) değerini 'blender-render-tutorial1' olarak ayarlayın
* 'Input data' (Giriş verileri) için 'fgrp-blender-classroom' seçeneğini belirleyin
* 'Blend file' (Blend dosyası) dosya simgesini ve 'classroom.blend' öğesini seçin
* 'Frame start' (Başlangıç karesi) değerini '1', 'Frame end' (Bitiş karesi) değerini '5' olarak ayarlayın
* 'Outputs' (Çıkışlar) değerini 'fgrp-render-output' olarak ayarlayın
* İşi oluşturmak için yeşil düğmeyi seçin. Her kare için bir adet olmak üzere beş görev içeren bir iş oluşturulur

![Blender için iş şablonu](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_job_template.png)

İş, işin ve tüm görevleri oluşturulduktan sonra iş görevleri ile birlikte görüntülenir: ![Proje Görev listesi](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_task_list.png)

Bir görev havuz VM'sinde ilk kez çalıştırıldığında Blender tarafından erişilebilmesi için sahne dosyalarını depolama dosya grubundan VM'ye kopyalayan bir Batch işi hazırlama görevi çalıştırılır.
İşleme durumunu görmek için Blender tarafından oluşturulan stdout.txt günlük dosyasını inceleyebilirsiniz.  Bir görevi seçtiğinizde varsayılan olarak 'Task Outputs' (Görev Çıkışları) penceresi görüntülenir ve buradan 'stdout.txt' dosyası seçilip izlenebilir.
![stdout dosyası](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_stdout.png)

'Windows blender' havuzu seçtiyseniz, Vm'leri havuzu çalışır durumda görülür: ![Düğümler ile havuz ısı Haritası](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_pool_heatmap_running.png)

İşlenmiş görüntülerin oluşturulma süresi seçilen VM boyutuna göre farklılık gösterir.  Önceden belirtilen F16 VM kullanılarak karelerin işlenmesi yaklaşık 16 dakika sürmüştür.

## <a name="view-the-rendering-output"></a>İşleme çıkışını görüntüleme

Çerçeve işleme bittiğinde, bu görevleri tamamlandı olarak gösterilir: ![Görevleri tamamlama](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_tasks_complete.png)

İşlenmiş resmin VM'ye ilk yazılır ve 'wd' klasörü seçerek görüntülenebilir: ![Görüntü havuz düğümünün üzerinde işlenen](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_image.png)

İş şablonu ayrıca çıkış karesinin ve günlük dosyalarının iş oluşturulduğunda belirtilen Azure Depolama hesabı dosya grubuna yazıldığını gösterir.  UI 'veri' çıkış dosyalarını ve günlükleri görüntülemek için kullanılabilir; Dosyaları indirmek için de kullanılabilir: ![Resim depolama dosya grubunda işlenen](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_output_image_storage.png)

Tüm görevler tamamlandığında işin tamamlandı olarak işaretlenir: ![İş ve tamamlanan tüm görevleri](./media/tutorial-rendering-batchexplorer-blender/batch_explorer_job_alltasks_complete.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!WARNING]
> VM'lerin Azure aboneliğinize ücret yansıtmasını durdurmak için havuzun silinmesi gerekir (sıfır düğüme indirilmesi de yeterli olacaktır).

* 'Pools' (Havuzlar) öğesini seçin
* 'blender-windows' havuzunu seçin
* Sağ tıklayıp 'Delete' (Sil) öğesini veya havuzun üzerindeki çöp kutusu simgesini seçin

## <a name="next-steps"></a>Sonraki adımlar
* ‘Gallery’ (Galeri) bölümünde Batch Explorer ile kullanılabilir durumdaki işleme uygulamalarını keşfedin.
* Her uygulama için birkaç şablon vardır ve şablon sayısı zaman içinde artacaktır.  Örneğin bir görüntüyü parçalara ayırarak bu parçaların paralel olarak işlenmesini sağlayan Blender şablonları bulunmaktadır.
* İşleme özelliklerinin kapsamlı bir açıklaması için [buradaki](https://docs.microsoft.com/azure/batch/batch-rendering-service) makalelere bakın.
