---
title: API'ler ve Araçlar geliştiricilerin - Azure Batch için | Microsoft Docs
description: Azure Batch hizmeti ile çözüm geliştirmek için kullanılabilen API’ler ve araçlar hakkında bilgi edinin.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 912e3342b4e8031b4404dffb56f1add2cc705f8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60721768"
---
# <a name="overview-of-batch-apis-and-tools"></a>Batch API'lerine ve araçlarına genel bakış

Azure Batch ile paralel iş yükleri genellikle programlı olarak Batch API'lerini kullanarak gerçekleştirilir. İstemci uygulamanız veya hizmetiniz, Batch hizmetiyle iletişim kurmak için Batch API'lerini kullanabilir. Batch API'leriyle, işlem düğümü havuzları (sanal makineler veya bulut hizmetleri) oluşturabilir veya yönetebilirsiniz. Ardından bu düğümlerde çalıştırılacak işleri ve görevleri zamanlayabilirsiniz. 

Kuruluşunuz için büyük ölçekli iş yüklerini verimli bir şekilde işleyebilir ya da müşterilerinizin tek, yüzlerce ve hatta binlerce düğümde istek üzerine veya planlı olarak işleri ve görevleri çalıştırabileceği şekilde onlara bir hizmet ön ucu sağlayabilirsiniz. [Azure Data Factory](../data-factory/transform-data-using-dotnet-custom-activity.md?toc=%2fazure%2fbatch%2ftoc.json) gibi araçlarla yönetilen Azure Batch'i daha büyük bir iş akışının parçası olarak da kullanabilirsiniz.

> [!TIP]
> Sağladığı özellikleri daha derinlemesine anlamak amacıyla Batch API'sinin ayrıntılarına gitmeye hazır olduğunuzda, [Geliştiriciler için Batch özelliklerine genel bakış](batch-api-basics.md) konusunu inceleyin.
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Batch geliştirme için Azure hesapları
Batch çözümleri geliştirdiğinizde, Azure aboneliğinizdeki aşağıdaki hesapları kullanacaksınız:

* **Batch hesabı** - Havuzlar, işlem düğümleri, işler ve görevler gibi bir Azure [Batch hesabıyla](batch-api-basics.md#account) ilişkilendirilmiş Azure Batch kaynakları. Uygulamanız, Batch hizmetinden bir istekte bulunduğunda istek Azure Batch hesabı adı, hesabın URL'si ve erişim anahtarı veya Azure Active Directory belirteci kullanılarak kimlik doğrulamasından geçirilir. Azure portalından veya kod kullanarak [Batch hesabı oluşturabilirsiniz](batch-account-create-portal.md).
* **Depolama hesabı** - Batch’te [Azure Depolama][azure_storage] dosyalarıyla çalışmak için yerleşik destek bulunur. Neredeyse tüm Batch senaryolarında, görevlerinizin çalıştırdığı programların ve işlediği verilerin hazırlanmasının yanı sıra oluşturduğu çıktı verilerinin depolanması için Azure Blob depolama kullanılır. Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](batch-api-basics.md#azure-storage-account).

## <a name="batch-service-apis"></a>Batch hizmeti API’leri

Uygulamalarınız ve hizmetleriniz doğrudan REST API çağrıları kullanabilir veya Azure Batch iş yüklerinizi çalıştırmak ya da yönetmek için aşağıdaki istemci kitaplıklardan birini veya daha fazlasını kullanabilir.

| API | API başvurusu | İndirme | Öğretici | Kod örnekleri | Daha Fazla Bilgi |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[docs.microsoft.com][batch_rest] |Yok |- |- | [Desteklenen Sürümler](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet][api_net_nuget] |[Öğretici](tutorial-parallel-dotnet.md) |[GitHub][api_sample_net] | [Sürüm notları](https://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[docs.microsoft.com][api_python] |[PyPI][api_python_pypi] |[Öğretici](tutorial-parallel-python.md)|[GitHub][api_sample_python] | [Benioku](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[npm][api_nodejs_npm] |[Öğretici](batch-nodejs-get-started.md) |- | [Benioku](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[docs.microsoft.com][api_java] |[Maven][api_java_jar] |- |[Benioku][api_sample_java] | [Benioku](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Yönetimi API’leri

Batch için Azure Resource Manager API'leri, Batch hesaplarına programlı erişim sağlar. Bu API’leri kullanarak Batch hesaplarını, kotaları, uygulama paketlerini ve diğer kaynakları Microsoft.Batch sağlayıcısı üzerinden programlı bir şekilde yönetebilirsiniz.  

| API | API başvurusu | İndirme | Öğretici | Kod örnekleri |
| --- | --- | --- | --- | --- |
| **Batch Yönetimi REST** |[docs.microsoft.com][api_rest_mgmt] |Yok |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Yönetimi .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet][api_net_mgmt_nuget] | [Öğretici](batch-management-dotnet.md) |[GitHub][api_sample_net] |
| **Batch Yönetimi Python** |[docs.microsoft.com][api_python_mgmt] |[PyPI][api_python_mgmt_pypi] |- |- |
| **Batch Yönetimi Node.js** |[docs.microsoft.com][api_nodejs_mgmt] |[npm][api_nodejs_mgmt_npm] |- |- | 
| **Batch Yönetimi Java** |- |[Maven][api_java_mgmt_jar] |- |- |
## <a name="batch-command-line-tools"></a>Batch komut satırı araçları

Bu komut satırı araçları, Batch hizmeti ve Batch Yönetimi API'leri ile aynı işlevi sağlar: 

* [Batch PowerShell cmdlet'leri][batch_ps]: Azure Batch cmdlet'leri [Azure PowerShell](/powershell/azure/overview) modülü PowerShell ile Batch kaynaklarını yönetmek etkinleştirin.
* [Azure CLI](/cli/azure): Azure CLI Batch hizmeti ve Batch Yönetimi hizmeti de dahil olmak üzere çok sayıda Azure hizmetiyle etkileşim için Kabuk komutları sağlayan platformlar arası bir araç takımı ' dir. Azure CLI’yı Batch ile birlikte kullanma hakkında daha fazla bilgi için bkz. [Azure CLI ile Batch kaynaklarını yönetme](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Uygulama geliştirme için diğer araçlar

Batch uygulamalarınızı ve hizmetlerinizi oluşturmak ve bunlarda hata ayıklamak için yararlı olabilecek bazı ek araçlar şunlardır:

* [Azure portalında][portal]: Oluşturma, izleme ve Batch havuzları, işleri ve görevleri Azure portalından silin. Bu ve diğer kaynakların durum bilgilerini, işlerinizi çalıştırırken görüntüleyebilir, hatta havuzlarınızdaki işlem düğümlerinden dosya indirebilirsiniz. Örneğin, sorun giderme sırasında başarısız bir görevin `stderr.txt` öğesini indirebilirsiniz. İşlem düğümlerinde oturum açmak için kullanabileceğiniz Uzak Masaüstü (RDP) dosyalarını da indirebilirsiniz.
* [Azure Batch Gezgini][batch_labs]: Batch Gezgini (eski adıyla. BatchLabs) oluşturma, hata ayıklama ve Azure Batch uygulamalarıyla izlemenize yardımcı olacak bir ücretsiz ve zengin özellikli tek başına istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz.
* [Azure Batch Shipyard](https://github.com/Azure/batch-shipyard): Batch Shipyard, sağlama yardımcı, yürütün ve kapsayıcı tabanlı toplu işlem ve HPC iş yüklerini Azure batch izlemek için kullanılan bir araçtır.
* [Azure Depolama Gezgini][storage_explorer]: Kesinlikle olmadığında bir Azure Batch aracı Depolama Gezgini, Batch çözümlerinizi geliştirdiğiniz ve sırada için başka bir değerli araçtır.

## <a name="additional-resources"></a>Ek kaynaklar

- Batch uygulamanızdaki olayları günlüğe kaydetme hakkında bilgi edinmek için bkz. [Batch çözümlerine tanılama değerlendirmesi yapmak ve Batch çözümlerini izlemek üzere olayları günlüğe kaydetme](batch-diagnostics.md). Batch hizmeti tarafından oluşturulan olaylarla ilgili bir başvuru için bkz. [Batch Analizi](batch-analytics.md).
- İşlem düğümlerine yönelik ortam değişkenleri hakkında bilgi için bkz. [Azure Batch işlem düğümü ortam değişkenleri](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Sonraki adımlar

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Genel bir Batch iş akışını kullanarak basit bir iş yükü yürütmek üzere C# ve Batch .NET kitaplığını kullanma hakkında bilgi için bkz. [.NET için Azure Batch kitaplığını kullanmaya başlama](tutorial-parallel-dotnet.md). Bu öğreticinin [Python sürümü](tutorial-parallel-python.md) ve [Node.js öğreticisi](batch-nodejs-get-started.md) de vardır.
* Hem C# hem de Python'un örnek iş yüklerini zamanlamak ve işlemek üzere Batch ile arabirim oluşturmasını görmek için [GitHub'daki kod örneklerini][github_samples] indirin.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: /java/api/overview/azure/batch
[api_java_mgmt]: /java/api/overview/azure/batch/managementapi
[api_java_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_java_mgmt_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/
[api_net_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /javascript/api/overview/azure/batch/client
[api_nodejs_mgmt]: /javascript/api/overview/azure/batch/management
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_nodejs_mgmt_npm]: https://www.npmjs.com/package/azure-arm-batch
[api_python]: /python/api/overview/azure/batch/client
[api_python_mgmt]: /python/api/overview/azure/batch/management
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_python_mgmt_pypi]: https://pypi.python.org/pypi/azure-mgmt-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/az.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchExplorer/
[storage_explorer]: https://storageexplorer.com/
[portal]: https://portal.azure.com
