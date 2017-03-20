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
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 9ea205cb5034fea66c770ec71934e302ee818a89
ms.lasthandoff: 03/09/2017


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

Azure’deki Batch ve diğer HPC çözüm seçenekleri arasında bir karşılaştırma için bkz. [ Batch ve HPC çözümleri](batch-hpc-solutions.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="developing-with-batch"></a>Batch ile geliştirme
Azure Batch ile paralel iş yükleri genellikle [Batch API'lerinden](#batch-development-apis) biri kullanılarak programlı bir şekilde işlenir. İstemci uygulamanız veya hizmetiniz, Batch hizmetiyle iletişim kurmak için Batch API'lerini kullanabilir. Batch API'leriyle, işlem düğümü havuzları (sanal makineler veya bulut hizmetleri) oluşturabilir veya yönetebilirsiniz. Ardından bu düğümlerde çalıştırılacak işleri ve görevleri zamanlayabilirsiniz. 

Kuruluşunuz için büyük ölçekli iş yüklerini verimli bir şekilde işleyebilir ya da müşterilerinizin tek, yüzlerce ve hatta binlerce düğümde istek üzerine veya planlı olarak işleri ve görevleri çalıştırabileceği şekilde onlara bir hizmet ön ucu sağlayabilirsiniz. [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md) gibi araçlarla yönetilen Azure Batch'i daha büyük bir iş akışının parçası olarak da kullanabilirsiniz.

> [!TIP]
> Sağladığı özellikleri daha derinlemesine anlamak amacıyla Batch API'sinin ayrıntılarına gitmeye hazır olduğunuzda, [Geliştiriciler için Batch özelliklerine genel bakış](batch-api-basics.md) konusunu inceleyin.
> 
> 

### <a name="azure-accounts-youll-need"></a>Gereken Azure hesapları
Batch çözümleri geliştirdiğinizde, Microsoft Azure'de aşağıdaki hesapları kullanacaksınız.

* **Azure hesabı ve aboneliği** -Henüz Azure aboneliğiniz yoksa [MSDN abone avantajlarınızı][msdn_benefits] etkinleştirebilir veya [ücretsiz Azure hesabı][free_account] için kaydolabilirsiniz. Hesap oluşturduğunuzda sizin için varsayılan bir abonelik oluşturulur.
* **Batch hesabı** - Havuzlar, işlem düğümleri, işler ve görevler gibi bir Azure Batch hesabıyla ilişkilendirilmiş Azure Batch kaynakları. Uygulamanız, Batch hizmetinden bir istekte bulunduğunda istek Azure Batch hesabı adı, hesabın URL'si ve erişim anahtarı kullanılarak kimlik doğrulamasından geçirilir. Azure portalında [Batch hesabı oluşturabilirsiniz](batch-account-create-portal.md).
* **Depolama hesabı** - Batch’te [Azure Depolama][azure_storage] dosyalarıyla çalışmak için yerleşik destek bulunur. Neredeyse tüm Batch senaryolarında, görevlerinizin çalıştırdığı programların ve işlediği verilerin hazırlanmasının yanı sıra oluşturduğu çıktı verilerinin depolanması için Azure Blob depolama kullanılır. Storage hesabı oluşturmak için bkz. [Azure depolama hesapları hakkında](../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Batch geliştirme API'leri
Uygulamalarınız ve hizmetleriniz doğrudan REST API çağrıları kullanabilir veya Azure Batch iş yüklerinizi çalıştırmak ya da yönetmek için aşağıdaki istemci kitaplıklardan birini veya daha fazlasını kullanabilir.

| API | API başvurusu | İndir | Öğretici | Kod örnekleri |
| --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |Yok |- |- |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Öğretici](batch-dotnet-get-started.md) |[GitHub][api_sample_net] |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Öğretici](batch-python-tutorial.md)|[GitHub][api_sample_python] |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- |
| **Batch Java** (önizleme) |[github.io][api_java] |[Maven][api_java_jar] |- |[GitHub][api_sample_java] |

### <a name="batch-command-line-tools"></a>Batch komut satırı araçları

Geliştirme API'leri tarafından sunulan işlevlere komut satırı araçlarını kullanarak da ulaşabilirsiniz: 

* [Batch PowerShell cmdlet'leri][batch_ps]: [Azure PowerShell](/powershell/azureps-cmdlets-docs) modülündeki Azure Batch cmdlet'leri PowerShell ile Batch kaynaklarını yönetmenizi sağlar.
* [Azure CLI](../xplat-cli-install.md): Azure Komut Satırı Arabirimi (Azure CLI), Batch de içinde olmak üzere çok sayıda Azure hizmetiyle etkileşim için kabuk komutları sağlayan platformlar arası bir araç takımıdır.

### <a name="batch-resource-management"></a>Batch kaynak yönetimi

Batch için Azure Resource Manager API'leri, Batch hesaplarına programlı erişim sağlar. Bu API'leri kullanarak, Batch hesaplarını, kotalarını ve uygulama paketlerini programlı olarak yönetebilirsiniz.  

| API | API başvurusu | İndir | Öğretici | Kod örnekleri |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |Yok |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Öğretici](batch-management-dotnet.md) |[GitHub][api_sample_net] |


### <a name="batch-tools"></a>Batch araçları
Batch kullanarak çözüm derlemek için gerekmese de Batch uygulamalarınızı ve hizmetlerinizi derlerken veya bunların hatalarını ayıklarken kullanabileceğiniz değerli araçlardan bazıları aşağıda verilmiştir.

* [Azure portalı][portal]: Azure portalının Batch dikey pencerelerinde, Batch havuzlarını, işlerini ve görevlerini oluşturabilir, izleyebilir ve silebilirsiniz. Bu ve diğer kaynakların durum bilgilerini işlerinizi çalıştırırken görüntüleyebilir, hatta havuzlarınızdaki işlem düğümlerinden dosya indirebilirsiniz (örneğin sorun giderme sırasında, başarısız olan bir görevin `stderr.txt`’ini indirebilirsiniz). İşlem düğümlerinde oturum açmak için kullanabileceğiniz Uzak Masaüstü (RDP) dosyalarını da indirebilirsiniz.
* [Azure Batch Gezgini][batch_explorer]: Batch Gezgini, Azure portalının sağladığına benzer bir Batch kaynak yönetimi işlevselliği sağlar, ancak bunu bağımsız bir Windows Presentation Foundation (WPF) istemci uygulamasında yapar. [GitHub][github_samples] üzerinde kullanılabilir Batch .NET örnek uygulamalarından biridir; Batch Gezgini'ni Visual Studio 2015 veya daha yeni sürümlerle derleyebilir, Batch çözümlerinizi geliştirdiğiniz ve bunların hatalarını ayıkladığınız sırada da Batch hesabınızdaki kaynaklara göz atıp yönetmek için kullanabilirsiniz. İşi, havuzu ve görev ayrıntılarını görüntüleyin, işlem düğümlerinden dosya indirin ve Batch Gezgini ile indirebileceğiniz Uzak Masaüstü (RDP) dosyalarını kullanarak düğümlere bağlanın.
* [Microsoft Azure Depolama Gezgini][storage_explorer]: Azure Batch aracı kesinlikle olmadığında, Batch çözümlerinizi geliştirdiğiniz ve hatalarını ayıkladığınız sırada Depolama Gezgini sahip olunması gereken başka bir değerli araçtır.

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
* Genel bir Batch iş akışını kullanarak basit bir iş yükü yürütmek üzere C# ve Batch .NET kitaplığını kullanma hakkında bilgi için bkz. [.NET için Azure Batch kitaplığını kullanmaya başlama](batch-dotnet-get-started.md). Bu makale, Batch hizmetini kullanmayı öğrenirken ilk başvuracağınız kaynaklardan biri olmalıdır. Öğreticinin bir [Python sürümü](batch-python-tutorial.md) de mevcuttur.
* Hem C# hem de Python'un örnek iş yüklerini zamanlamak ve işlemek üzere Batch ile arabirim oluşturmasını görmek için [GitHub'daki kod örneklerini][github_samples] indirin.
* Batch'le çalışmayı öğrenirken size uygun kaynaklar hakkında bir fikir edinmek için [Batch Öğrenme Yolu][learning_path] konusunu inceleyin.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/en-us/rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

