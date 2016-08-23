<properties
   pageTitle="Azure Batch PowerShell kullanmaya başlama | Microsoft Azure"
   description="Azure Batchhizmetini yönetmek için kullanabileceğiniz Azure PowerShell cmdlet’lerine bir giriş gerçekleştirin."
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="07/28/2016"
   ms.author="danlep"/>

# Azure Batch PowerShell cmdlet’leri kullanmaya başlama
Azure Batch PowerShell cmdlet’leri ile Batch API'leri, Azure portalı ve Azure Komut Satırı Arabirimi (CLI) ile gerçekleştirdiğiniz Batch aynı görevlerin çoğunu gerçekleştirebilir ve betik oluşturabilirsiniz. Bu bilgiler, Batch hesabınızı yönetmek, havuzlar, işler ve görevler gibi Batch kaynaklarınızla da çalışmak için kullanabildiğiniz cmdlet’lere hızlı bir giriş yapmanızı sağlar. Bu makale Azure PowerShell 1.6.0 sürümündeki cmdlet’leri temel almaktadır.

Tam Batch cmdlet’leri listesi ve ayrıntılı cmdlet sözdizimi için bkz. [Azure Batch cmdlet başvurusu](https://msdn.microsoft.com/library/azure/mt125957.aspx). 


## Ön koşullar

* **Azure PowerShell** - Azure PowerShell indirme ve yükleme talimatları için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md). 
   
    * Azure Batch cmdlet'leri Azure Resource Manager modülüyle birlikte verildiğinden, aboneliğinize bağlanmak için **Login-AzureRmAccount** cmdlet'ini çalıştırmanız gerekir. 
    
    * Hizmet güncelleştirmeleri ve geliştirmeleri avantajlarından yararlanmak için Azure PowerShell’inizi sık sık güncelleştirin. 
    
* **Batch sağlayıcısı ad alanıyla kaydetme (bir defalık işlem) ** -Batch hesaplarınızla çalışmadan önce Batch sağlayıcı ad alanıyla kaydolmanız gerekir. Bu işlemin her abonelik için yalnızca bir kez gerçekleştirilmesi gerekir. Aşağıdaki cmdlet'i çalıştırın:

        Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch


## Batch hesaplarını ve anahtarlarını yönetme

### Batch hesabı oluşturma

**New-AzureRmBatchAccount**, belirtilen kaynak grubunda yeni bir Batch hesabı oluşturur. Henüz bir kaynak grubunuz yoksa, [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) cmdlet’ini çalıştırarak, **Konum** parametresinde "Orta ABD" gibi Azure bölgelerinden birini belirterek bir tane oluşturun. Örneğin:


    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"


Ardından, <*account_name*> içinde bir hesap adı ve kaynak grubunuzun konum ve adını belirterek kaynak grubunda yeni bir Batch hesabı oluşturun. Batch hesabının oluşturulması biraz zaman alabilir. Örneğin:


    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup

> [AZURE.NOTE] Batch hesabı adı, kaynak grubunun Azure bölgesinde benzersiz olmalıdır; 3 - 24 arası karakter olmalı, yalnızca küçük harf ve rakam içermelidir.

### Hesap erişim anahtarı alma
**Get-AzureRmBatchAccountKeys** Azure Batch hesabıyla ilişkili erişim anahtarlarını gösterir. Örneğin, oluşturduğunuz birincil ve ikincil anahtarları almak için aşağıdakini çalıştırın.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <accountname>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey


### Yeni erişim anahtarı oluşturma
**New-AzureRmBatchAccountKey**, Azure Batch hesabı için yeni bir birincil ya da ikincil hesap anahtarı oluşturur. Örneğin, Batch hesabınıza yeni bir birincil anahtar oluşturmak için şunu yazın:


    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary


> [AZURE.NOTE] Yeni bir ikincil anahtar oluştururken **KeyType** parametresi için "Secondary" seçeneğini belirtin. Birincil ve ikincil anahtarları ayrı olarak yeniden oluşturmalısınız.

### Batch hesabını silme
**Remove-AzureRmBatchAccount** Batch hesabını siler. Örneğin:


    Remove-AzureRmBatchAccount -AccountName <account_name>

İstendiğinde, hesabı kaldırmak istediğinizi onaylayın. Hesap kaldırma işleminin tamamlanması biraz zaman alabilir.

## BatchAccountContext nesnesi oluşturma

Batch havuzları, işleri, görevleri ve başka kaynaklarını oluşturduğunuzda ve yönettiğinizde, Batch PowerShell cmdlet’lerini kullanarak kimlik doğrulamak için önce, hesap adınızı ve anahtarlarınızı depolayacak bir BatchAccountContext nesnesi oluşturun:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

BatchAccountContext nesnesini **BatchContext** parametresini kullanan cmdlet’lere geçirirsiniz.

> [AZURE.NOTE] Varsayılan olarak, hesabın birincil anahtarı kimlik doğrulaması amacıyla kullanılsa da, BatchAccountContext nesnenizin **KeyInUse** özelliğini değiştirerek kullanılacak anahtarı açıkça seçebilirsiniz: `$context.KeyInUse = "Secondary"`.



## Batch kaynaklarını oluşturma ve değiştirme
Batch hesabı altında kaynak oluşturmak için **New-AzureBatchPool**, **New-AzureBatchJob** ve **New-AzureBatchTask** gibi cmdlet’leri kullanın. Batch hesabı altında var olan kaynakların özelliklerini güncelleştirmek için **Get-** ve **Set-** cmdlet’leri, kaldırmak için de **Remove-** cmdlet’leri vardır. 

Bu cmdlet’lerinin birçoğunu kullanırken bir BatchContext nesnesi geçirmeye ek olarak aşağıdaki örnekte gösterildiği gibi ayrıntılı kaynak ayarlarını içeren nesneleri oluşturmanız ya da geçirmeniz gerekir. Diğer örnekler için her bir cmdlet’e ilişkin ayrıntılı yardıma bakın.

### Batch havuzu oluşturma

Bir Batch havuzu oluştururken ya da güncelleştirirken, işlem düğümlerindeki işletim sistemine yönelik bir bulut hizmeti yapılandırması veya sanal makine yapılandırması seçin (bkz. [Batch özelliğine genel bakış](batch-api-basics.md#pool)). Tercihiniz işlem düğümlerinizin [Azure Konuk işletim sistemi sürümlerinden](../cloud-services/cloud-services-guestos-update-matrix.md#releases) biriyle ya da Azure Marketi’nde desteklenen Linux ya da Windows sanal makine görüntülerinden biriyle görüntü haline getirildiğini belirler. 

**New-AzureBatchPool** komutunu çalıştırdığınızda işletim sistemi ayarlarını bir PSCloudServiceConfiguration veya PSVirtualMachineConfiguration nesnesi içinde geçirin. Örneğin, aşağıdaki cmdlet, bulut hizmeti yapılandırmasında aile 3’ün en son işletim sistemi sürümü (Windows Server 2012) ile görüntü haline getirilen Küçük boyutlu işlem düğümleri ile yeni bir Batch havuzu oluşturur. Burada **CloudServiceConfiguration** parametresi *$configuration* değişkenini PSCloudServiceConfiguration nesnesi olarak belirtir. **BatchContext** parametresi önceden tanımlanmış *$context* değişkenini BatchAccountContext nesnesi olarak belirtir.


    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(3,"*")
    
    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Yeni havuzdaki hedef işlem düğümü sayısı bir otomatik ölçeklendirme formülü ile belirlenir. Bu durumda, havuzdaki işlem düğümü sayısının en çok 4 olduğunu belirten basit bir **$TargetDedicated=4** formülüdür. 

## Havuzlar, işler, görevler ve diğer ayrıntılar için sorgulama

Batch hesabı altında oluşturulan varlıkların sorgulanması için **Get-AzureBatchPool**, **Get-AzureBatchJob** ve **Get-AzureBatchTask** gibi cmdlet’leri kullanın.


### Verileri sorgulama

Bir örnek olarak, havuzlarınızı bulmak için **Get-AzureBatchPools** kullanın. Varsayılan olarak, hesabınız altındaki tüm havuzlarla ilgili bu sorgular zaten *$context* değişkenindeki BatchAccountContext nesnesinde depolanır:


    Get-AzureBatchPool -BatchContext $context

### OData filtresini kullanma

Yalnızca ilgilendiğiniz nesneleri bulmak için **Filtre** parametresini kullanan bir OData filtresi sağlayabilirsiniz. Örneğin, kimlikleri “myPool” ile başlayan tüm havuzları bulabilirsiniz.


    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context


Bu yöntem, yerel bir işlem hattında "Where-Object" kullanmak kadar esnek değildir. Ancak, sorgu Batch hizmetine doğrudan gönderilir; böylece, İnternet bant genişliği korunarak filtre işleminin tümü sunucu tarafında gerçekleşir.

### Kimlik parametresini kullanma

OData filtresinin bir alternatifi de **Kimlik** parametresi kullanmaktır. "myPool" Kimliğine sahip belirli bir havuzu sorgulamak için:


    Get-AzureBatchPool -Id "myPool" -BatchContext $context


**Kimlik** parametresi yalnızca tam kimlik aramasını destekler, joker karakter veya OData stili filtreleri desteklemez.



### MaxCount parametresini kullanma

Varsayılan olarak, her cmdlet en çok 1000 nesne döndürür. Bu sınıra ulaştıysanız, daha az nesne döndürmek için filtreyi daraltın veya **MaxCount** parametresini kullanarak kesin bir üst sınır ayarlayın. Örneğin:


    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Üst sınırı kaldırmak için **MaxCount**’u 0 veya daha az bir değere ayarlayın.

### İşlem hattını kullanma

Batch cmdlet'leri, verileri cmdlet'ler arasında göndermek için PowerShell işlem hattından yararlanabilirsiniz. Parametre belirtmekle aynı etkiye sahip olsa da, birden çok varlığın listelenmesini kolaylaştırır. Örneğin, aşağıdaki hesabın altındaki tüm görevleri bulmaktadır:


    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context


## Sonraki adımlar
* Ayrıntılı cmdlet sözdizimi ve örnekleri için bkz. [Azure Batch cmdlet başvurusu](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Batch’e sorgular için döndürülen bilgi türleri ve öğe sayısını azaltma hakkında bilgi için bkz. [Batch hizmetini etkin bir şekilde sorgulama](batch-efficient-list-queries.md). 



<!--HONumber=Aug16_HO1-->


