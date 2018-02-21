---
title: "Azure Batch için PowerShell kullanmaya başlama | Microsoft Docs"
description: "Batch kaynaklarını yönetmek için kullanabileceğiniz Azure PowerShell cmdlet'lerine hızlı bir giriş."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e33be6ed658e00250ea1e80cd7da4d348fb18296
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Batch kaynaklarını PowerShell cmdlet'leriyle yönetme

Azure Batch PowerShell cmdlet’leri ile Batch API'leri, Azure portalı ve Azure Komut Satırı Arabirimi (CLI) ile gerçekleştirdiğiniz Batch aynı görevlerin çoğunu gerçekleştirebilir ve betik oluşturabilirsiniz. Bu bilgiler, Batch hesabınızı yönetmek, havuzlar, işler ve görevler gibi Batch kaynaklarınızla da çalışmak için kullanabildiğiniz cmdlet’lere hızlı bir giriş yapmanızı sağlar.

Tam Batch cmdlet’leri listesi ve ayrıntılı cmdlet sözdizimi için bkz. [Azure Batch cmdlet başvurusu](/powershell/module/azurerm.batch/#batch).

Bu makale, Azure PowerShell 3.0.0 sürümündeki cmdlet’leri temel almaktadır. Hizmet güncelleştirmeleri ve geliştirmeleri avantajlarından yararlanmak için Azure PowerShell’inizi sık sık güncelleştirin.

## <a name="prerequisites"></a>Ön koşullar
Batch kaynaklarınızı yönetmek üzere Azure PowerShell’i kullanmak için aşağıdaki işlemleri gerçekleştirin.

* [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview)
* Aboneliğinize bağlanmak için **Login-AzureRmAccount** cmdlet’ini çalıştırın (Azure Batch cmdlet’leri, Azure Resource Manager modülüyle birlikte verilir):
  
    `Login-AzureRmAccount`
* **Batch sağlayıcı ad alanı ile kaydolun**. Bu işlemin **her abonelik için yalnızca bir kez** gerçekleştirilmesi gerekir.
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Batch hesaplarını ve anahtarlarını yönetme
### <a name="create-a-batch-account"></a>Batch hesabı oluşturma
**New-AzureRmBatchAccount**, belirtilen kaynak grubunda bir Batch hesabı oluşturur. Zaten bir kaynak grubunuz yoksa [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet'ini çalıştırarak bir kaynak grubu oluşturun. **Location** parametresinde, "Orta ABD" gibi Azure bölgelerinden birini belirtin. Örnek:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Ardından, <*account_name*> içinde bir hesap adı ve kaynak grubunuzun konumunu ve adını belirterek kaynak grubunda bir Batch hesabı oluşturun. Batch hesabının oluşturulması biraz zaman alabilir. Örnek:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> Batch hesabı adı, kaynak grubunun Azure bölgesinde benzersiz olmalıdır; 3 - 24 arası karakter olmalı, yalnızca küçük harf ve rakam içermelidir.
> 
> 

### <a name="get-account-access-keys"></a>Hesap erişim anahtarı alma
**Get-AzureRmBatchAccountKeys** Azure Batch hesabıyla ilişkili erişim anahtarlarını gösterir. Örneğin, oluşturduğunuz birincil ve ikincil anahtarları almak için aşağıdakini çalıştırın.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Yeni erişim anahtarı oluşturma
**New-AzureRmBatchAccountKey**, Azure Batch hesabı için yeni bir birincil ya da ikincil hesap anahtarı oluşturur. Örneğin, Batch hesabınıza yeni bir birincil anahtar oluşturmak için şunu yazın:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> Yeni bir ikincil anahtar oluştururken **KeyType** parametresi için "Secondary" seçeneğini belirtin. Birincil ve ikincil anahtarları ayrı olarak yeniden oluşturmalısınız.
> 
> 

### <a name="delete-a-batch-account"></a>Batch hesabını silme
**Remove-AzureRmBatchAccount** Batch hesabını siler. Örnek:

    Remove-AzureRmBatchAccount -AccountName <account_name>

İstendiğinde, hesabı kaldırmak istediğinizi onaylayın. Hesap kaldırma işleminin tamamlanması biraz zaman alabilir.

## <a name="create-a-batchaccountcontext-object"></a>BatchAccountContext nesnesi oluşturma
Batch havuzları, işleri, görevleri ve başka kaynaklarını oluşturduğunuzda ve yönettiğinizde, Batch PowerShell cmdlet’lerini kullanarak kimlik doğrulamak için önce, hesap adınızı ve anahtarlarınızı depolayacak bir BatchAccountContext nesnesi oluşturun:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

BatchAccountContext nesnesini **BatchContext** parametresini kullanan cmdlet’lere geçirirsiniz.

> [!NOTE]
> Varsayılan olarak, hesabın birincil anahtarı kimlik doğrulaması amacıyla kullanılsa da, BatchAccountContext nesnenizin **KeyInUse** özelliğini değiştirerek kullanılacak anahtarı açıkça seçebilirsiniz: `$context.KeyInUse = "Secondary"`.
> 
> 

## <a name="create-and-modify-batch-resources"></a>Batch kaynaklarını oluşturma ve değiştirme
Batch hesabı altında kaynak oluşturmak için **New-AzureBatchPool**, **New-AzureBatchJob** ve **New-AzureBatchTask** gibi cmdlet’leri kullanın. Batch hesabı altında var olan kaynakların özelliklerini güncelleştirmek için **Get-** ve **Set-** cmdlet’leri, kaldırmak için de **Remove-** cmdlet’leri vardır.

Bu cmdlet’lerinin birçoğunu kullanırken bir BatchContext nesnesi geçirmeye ek olarak aşağıdaki örnekte gösterildiği gibi ayrıntılı kaynak ayarlarını içeren nesneleri oluşturmanız ya da geçirmeniz gerekir. Diğer örnekler için her bir cmdlet’e ilişkin ayrıntılı yardıma bakın.

### <a name="create-a-batch-pool"></a>Batch havuzu oluşturma
Bir Batch havuzu oluştururken ya da güncelleştirirken, işlem düğümlerindeki işletim sistemine yönelik bir bulut hizmeti yapılandırmasını veya sanal makine yapılandırmasını seçin (bkz. [Batch özelliğine genel bakış](batch-api-basics.md#pool)). Bulut hizmeti yapılandırmasını belirtirseniz işlem düğümleriniz [Azure konuk işletim sistemi sürümlerinden](../cloud-services/cloud-services-guestos-update-matrix.md#releases) biriyle görüntülenir. Sanal makinenin yapılandırmasını belirtirseniz [Azure Sanal Makineler Market görüntüleri][vm_marketplace] içindeki desteklenen Linux ya da Windows sanal makine görüntülerinden birini seçebilir veya hazırladığınız özel bir görüntüyü kullanabilirsiniz.

**New-AzureBatchPool** komutunu çalıştırdığınızda işletim sistemi ayarlarını bir PSCloudServiceConfiguration veya PSVirtualMachineConfiguration nesnesi içinde geçirin. Örneğin, aşağıdaki cmdlet, bulut hizmeti yapılandırmasında aile 3’ün en son işletim sistemi sürümü (Windows Server 2012) ile görüntü haline getirilen Küçük boyutlu işlem düğümleri ile yeni bir Batch havuzu oluşturur. Burada **CloudServiceConfiguration** parametresi *$configuration* değişkenini PSCloudServiceConfiguration nesnesi olarak belirtir. **BatchContext** parametresi önceden tanımlanmış *$context* değişkenini BatchAccountContext nesnesi olarak belirtir.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Yeni havuzdaki hedef işlem düğümü sayısı bir otomatik ölçeklendirme formülü ile belirlenir. Bu durumda, havuzdaki işlem düğümü sayısının en çok 4 olduğunu belirten basit bir **$TargetDedicated=4** formülüdür.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Havuzlar, işler, görevler ve diğer ayrıntılar için sorgulama
Batch hesabı altında oluşturulan varlıkların sorgulanması için **Get-AzureBatchPool**, **Get-AzureBatchJob** ve **Get-AzureBatchTask** gibi cmdlet’leri kullanın.

### <a name="query-for-data"></a>Verileri sorgulama
Bir örnek olarak, havuzlarınızı bulmak için **Get-AzureBatchPools** kullanın. Varsayılan olarak, hesabınız altındaki tüm havuzlarla ilgili bu sorgular zaten *$context* değişkenindeki BatchAccountContext nesnesinde depolanır:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>OData filtresini kullanma
Yalnızca ilgilendiğiniz nesneleri bulmak için **Filtre** parametresini kullanan bir OData filtresi sağlayabilirsiniz. Örneğin, kimlikleri “myPool” ile başlayan tüm havuzları bulabilirsiniz.

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Bu yöntem, yerel bir işlem hattında "Where-Object" kullanmak kadar esnek değildir. Ancak, sorgu Batch hizmetine doğrudan gönderilir; böylece, İnternet bant genişliği korunarak filtre işleminin tümü sunucu tarafında gerçekleşir.

### <a name="use-the-id-parameter"></a>Kimlik parametresini kullanma
OData filtresinin bir alternatifi de **Kimlik** parametresi kullanmaktır. "myPool" Kimliğine sahip belirli bir havuzu sorgulamak için:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

**Kimlik** parametresi yalnızca tam kimlik aramasını destekler, joker karakter veya OData stili filtreleri desteklemez.

### <a name="use-the-maxcount-parameter"></a>MaxCount parametresini kullanma
Varsayılan olarak, her cmdlet en çok 1000 nesne döndürür. Bu sınıra ulaştıysanız, daha az nesne döndürmek için filtreyi daraltın veya **MaxCount** parametresini kullanarak kesin bir üst sınır ayarlayın. Örnek:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Üst sınırı kaldırmak için **MaxCount**’u 0 veya daha az bir değere ayarlayın.

### <a name="use-the-powershell-pipeline"></a>PowerShell işlem hattını kullanma
Batch cmdlet'leri, verileri cmdlet'ler arasında göndermek için PowerShell işlem hattından yararlanabilirsiniz. İşlem hattı, parametre belirtmekle aynı etkiye sahip olsa da birden çok varlıkla çalışmayı kolaylaştırır.

Örneğin, hesabınızın altındaki tüm görevleri bulup görüntüleyin:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Havuzdaki her işlem düğümünü yeniden başlatın:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Uygulama paketi yönetimi
Uygulama paketleri havuzlarınızdaki işlem düğümlerine uygulama dağıtmanın basit bir yolunu sağlar. Batch PowerShell cmdlet'leriyle, Batch hesabınızdaki uygulama paketlerini yükleyebilir, yönetebilir ve paket sürümlerini işlem düğümlerine dağıtabilirsiniz.

Bir uygulama **oluşturun**:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

Bir uygulama paketi **ekleyin**:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Uygulamanın **varsayılan sürümünü** ayarlayın:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

Uygulamanın paketlerini **listeleme**

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

Uygulama paketini **silme**

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

Uygulamayı **silme**

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> Uygulamayı silmeden önce, uygulamanın tüm uygulama paketi sürümlerini silmeniz gerekir. Şu anda uygulama paketleri olan bir uygulamayı silmeye çalışırsanız bir 'Çakışma' hatası alırsınız.
> 
> 

### <a name="deploy-an-application-package"></a>Uygulama paketi dağıtma
Bir havuz oluşturduğunuzda dağıtım için bir veya daha fazla uygulama paketi belirtebilirsiniz. Havuz oluşturma saatinde bir paket belirttiğinizde düğüm havuza katıldıkça her bir düğüme dağıtılır. Paketler ayrıca bir düğüm yeniden başlatıldığında veya yeniden görüntüsü oluşturulduğunda dağıtılır.

Bir uygulama paketini havuza katıldıklarında havuzun düğümlerine dağıtmak üzere havuz oluştururken `-ApplicationPackageReference` seçeneğini belirtin. İlk olarak, **PSApplicationPackageReference** nesnesi oluşturun ve bunu havuzun işlem düğümlerine dağıtmak istediğiniz uygulama kimliği ve paket sürümüyle yapılandırın:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Şimdi havuzu oluşturun ve paket başvuru nesnesini, `ApplicationPackageReferences` seçeneğinin bağımsız değişkeni olarak belirtin:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

[Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı](batch-application-packages.md) konusunda, uygulama paketlerine ilişkin daha fazla bilgi bulabilirsiniz.

> [!IMPORTANT]
> Uygulama paketlerini kullanmak için Batch hesabınıza [bir Azure Depolama hesabı bağlamanız](#linked-storage-account-autostorage) gerekir.
> 
> 

### <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme
Mevcut bir havuza atanan uygulamaları güncelleştirmek için önce istediğiniz özelliklere (uygulama kimliği ve paket sürümü) sahip bir PSApplicationPackageReference nesnesi oluşturun:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Ardından, Batch’den havuzu alın, mevcut paketleri temizleyin, yeni paket başvurumuzu ekleyin ve Batch hizmetini yeni havuz ayarlarıyla güncelleştirin:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Batch hizmetinde havuzun özelliklerini güncelleştirmiş oldunuz. Bununla birlikte, yeni uygulama paketini havuzdaki işlem düğümlerine gerçekten dağıtmak için bu düğümleri yeniden başlatmanız ya da görüntülerini yeniden oluşturmanız gerekir. Bu komutla havuzdaki her düğümü yeniden başlatabilirsiniz:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> Havuzdaki işlem düğümlerine birden fazla uygulama paketini dağıtabilirsiniz. Hâlihazırda dağıtılmış paketleri değiştirmek yerine uygulama paketi *eklemek* isterseniz yukarıdaki `$pool.ApplicationPackageReferences.Clear()` satırını atlayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı cmdlet sözdizimi ve örnekleri için bkz. [Azure Batch cmdlet başvurusu](/powershell/module/azurerm.batch/#batch).
* Batch’deki uygulamalar ve uygulama paketleri hakkında daha fazla bilgi için bkz. [Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı](batch-application-packages.md).

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/