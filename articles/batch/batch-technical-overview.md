---
title: "Azure Batch bulutta büyük ölçekli paralel bilgi işlem çözümleri çalıştırır | Microsoft Docs"
description: "Büyük ölçekli paralel ve HPC iş yükleri için Azure Batch hizmetini kullanma hakkında bilgi edinin"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: a99f96db0c1e8bcd0cf29c564e5badf0eb728e56
ms.contentlocale: tr-tr
ms.lasthandoff: 09/14/2017

---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Batch ile doğası gereği paralel iş yüklerini çalıştırın

Azure Batch, büyük ölçekli paralel ve yüksek performanslı bilgi işlem (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmanızı sağlayan bir platform hizmetidir. Azure Batch, yönetilen sanal makineler koleksiyonunda çalıştırılacak işlem yoğunluklu işi zamanlar ve işinizin gereksinimlerini karşılayacak işlem kaynaklarını otomatik olarak ölçekler.

Azure Batch ile uygulamalarınızı paralel olarak ve uygun ölçekte yürütmek için Azure işlem kaynaklarını kolayca tanımlayabilirsiniz. HPC kümesi, tek tek sanal makineler, sanal ağlar veya karmaşık iş ve görev zamanlama altyapısını el ile oluşturmanız, yapılandırmanız ve yönetmeniz gerekmez. Azure Batch bu görevleri sizin için otomatikleştirir ya da basitleştirir.

## <a name="use-cases-for-batch"></a>Batch için örnek kullanma
Batch, *toplu işleme* veya *toplu bilgi işlem* için kullanılan bir Azure hizmetidir; bazı istenen sonuçları almak için büyük miktarlarda benzer görev çalıştırır. Toplu bilgi işlem, düzenli olarak büyük miktarlarda veriyi işleyen, dönüştüren ve analiz eden kuruluşlar tarafından yaygın olarak kullanılır.

Toplu işlem, doğası gereği paralel ("utandırıcı derecede paralel" olarak da bilinir) uygulamalar ve iş yükleriyle düzgün çalışır. Doğası gereği paralel iş yükleri, çalışmayı aynı anda birden çok bilgisayarda gerçekleştiren birden çok göreve kolayca ayrılır.

![Paralel görevler][1]<br/>

Bu teknik kullanılarak yaygın olarak işlenen iş yüklerinin bazı örnekleri şunlardır:

* Finansal risk modelleme
* İklim ve hidroloji veri analizi
* Görüntü işleme ve analizi
* Medya kodlama ve kodlama dönüştürme
* Genetik dizi analizi
* Mühendislik gerilimi analizi
* Yazılım testi

Batch, sonda bir azaltma adımı uygulayarak paralel hesaplamalar gerçekleştirebilir ve [İleti Geçirme Arabirimi (MPI)](batch-mpi.md) uygulamaları gibi daha karmaşık HPC iş yüklerini yürütebilir.

Azure'daki Batch ve diğer HPC çözüm seçenekleri arasında bir karşılaştırma için bkz. [HPC, Batch ve Big Compute çözümleri](../virtual-machines/linux/high-performance-computing.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Senaryo: Paralel iş yükünü ölçeklendirme
Batch hizmetiyle etkileşim kurmak için Batch API'lerini kullanan ortak çözüm, işlem düğümlerinin havuzunda 3B görüntülerin işlenmesi gibi paralel işi aslında ölçeklendirmeyi kapsar. Örneğin, bu işlem düğümleri havuzu, işleme işinize onlarca, yüzlerce, hatta binlerce çekirdek sağlayan size ait bir "işleme çiftliği" olabilir.

Aşağıdaki diyagramda, istemci uygulamasının yanı sıra paralel iş yükünü çalıştıracak Batch’i kullanan barındırma hizmetiyle birlikte ortak bir Batch iş akışı gösterilmektedir.

![Batch çözümü iş akışı][2]

Bu ortak senaryoda, uygulamanız veya hizmetiniz aşağıdaki adımları gerçekleştirerek Azure Batch’te bir hesaplama iş yükünü işlemektedir:

1. Azure Storage hesabınıza bu dosyaları işleyecek **girdi dosyalarını** ve **uygulamayı** indirin. Girdi dosyaları uygulamanızın işleyeceği herhangi bir veri olabilir; örneğin, finansal modelleme verileri veya dönüştürülecek video dosyaları. Uygulama dosyaları, 3B işleme uygulaması veya medya dönüştürücü gibi verilerin işlenmesinde kullanılan herhangi bir uygulama olabilir.
2. Batch hesabınızda işlem düğümlerine ait bir Batch **havuzu** oluşturun; bu düğümler görevlerinizi yürütecek sanal makinelerdir. Düğümler havuza katıldığında (1. adımda yüklediğiniz uygulama) [düğüm boyutu](../cloud-services/cloud-services-sizes-specs.md), işletim sistemi Azure Storage’da uygulamanın yükleneceği konum gibi özellikleri belirtirsiniz. Havuzu, görevlerinizin oluşturduğu iş yüküne karşılık olarak [otomatik olarak ölçeklendirilecek](batch-automatic-scaling.md) şekilde de yapılandırabilirsiniz. Otomatik ölçeklendirme, havuzdaki işlem düğümü sayısını dinamik olarak ayarlar.
3. İşlem düğümleri havuzunda iş yükünü çalıştırmak için bir Batch **işi** oluşturun. Proje oluşturduğunuzda, bunu Batch havuzuyla ilişkilendirirsiniz.
4. İşe **görevler** ekleyin. Bir işe görev eklediğinizde, Batch hizmeti havuzundaki işlem düğümlerinde yürütülmesi için görevleri otomatik olarak zamanlar. Her görev, girdi dosyalarını işlemek için yüklediğiniz uygulamayı kullanır.
   
   * 4a. Görev yürütülmeden önce, atandığı işlem düğümünde işlenmesi için verileri (girdi dosyaları) indirebilir. Uygulama zaten düğümde yüklü değilse (bkz. 2. adım), bunun yerine burada da indirilebilir. İndirme işlemi tamamlandığında görevler atanmış düğümlerinde yürütülür.
5. Görevler çalışırken, işin ve ona ait görevlerin ilerleyişini izlemek için Batch’i sorgulayabilirsiniz. İstemci uygulamanız veya hizmetiniz, Batch hizmetiyle HTTPS üzerinden iletişim kurabilir. Binlerce işlem düğümünde çalışan binlerce görevi izliyor olabileceğinizden [Batch hizmetini verimli şekilde sorguladığınızdan](batch-efficient-list-queries.md) emin olun.
6. Görevler tamamlanınca sonuç verilerini Azure Storage’a yükleyebilirler. Dosyaları doğrudan bir işlem düğümündeki dosya sisteminden de alabilirsiniz.
7. İzleme işleminiz işinizdeki görevlerin tamamlandığını algıladığında, istemci uygulamanız veya hizmetiniz daha fazla işleme ya da değerlendirme için çıktı verilerini indirebilir.

Bunun, Batch’i kullanma yollarından yalnızca biri olduğunu ve bu senaryoda, özelliklerden yalnızca birkaçının açıklandığını unutmayın. Örneğin, her işlem düğümünde [birden çok görevi paralel olarak](batch-parallel-node-tasks.md) yürütebilir, işlerle ilgili düğümleri hazırlamak için de [iş hazırlama ve tamamlama görevlerini](batch-job-prep-release.md) kullanabilir, sonra da bunları temizleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Batch hizmetine yüksek düzeyli bir genel bakışı gördüğünüze göre işlem yoğunluklu iş yüklerinizi işlemek için nasıl kullanacağınızı öğrenmek üzere daha derine inebiliriz.

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Genel bir Batch iş akışını kullanarak basit bir iş yükü yürütmek üzere C# ve Batch .NET kitaplığını kullanma hakkında bilgi için bkz. [.NET için Azure Batch kitaplığını kullanmaya başlama](batch-dotnet-get-started.md). Bu makale, Batch hizmetini kullanmayı öğrenirken ilk başvuracağınız kaynaklardan biri olmalıdır. Öğreticinin bir [Python sürümü](batch-python-tutorial.md) de mevcuttur.
* Hem C# hem de Python'un örnek iş yüklerini zamanlamak ve işlemek üzere Batch ile arabirim oluşturmasını görmek için [GitHub'daki kod örneklerini][github_samples] indirin.
* Batch'le çalışmayı öğrenirken size uygun kaynaklar hakkında bir fikir edinmek için [Batch Öğrenme Yolu][learning_path] konusunu inceleyin.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

