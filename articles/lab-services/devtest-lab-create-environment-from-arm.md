---
title: Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma | Microsoft Docs
description: Çoklu VM ortamları ve PaaS kaynakları Azure DevTest Labs'de bir Azure Resource Manager şablonundan oluşturmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: spelluru
ms.openlocfilehash: 144fd11e9c1ee3e00412320840e864a3190ccdb0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833981"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma

[Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) kolayca sağlayan [bir VM aynı anda bir laboratuvara ekleme](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Ancak, ortamı birden fazla VM varsa, her VM'yi ayrı ayrı oluşturulmalıdır. Çok katmanlı Web uygulaması veya SharePoint grubu gibi senaryolar için bir mekanizma tek bir adımda birden çok VM oluşturulmasına izin vermek için gereklidir. Azure Resource Manager şablonlarını kullanarak, şimdi altyapısını ve yapılandırmasını kullanarak Azure çözümünüzün tanımlayın ve sürekli olarak tutarlı bir durumda birden çok VM dağıtın. Bu özellik aşağıdaki yararları sağlar:

- Azure Resource Manager şablonları, doğrudan, kaynak denetimi deposundan (GitHub veya Azure DevOps Hizmetleri Git) yüklenir.
- Diğer türleri ile yaparken yapılandırıldıktan sonra kullanıcılarınızın bir ortam Azure portalından, bir Azure Resource Manager şablonu seçerek oluşturabilirsiniz [VM tabanlarını](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS kaynakları Iaas Vm'leri yanı sıra bir Azure Resource Manager şablonundan bir ortamda sağlanabilir.
- Ortamları maliyetini Laboratuvardaki diğer tabanların türleri tarafından oluşturulan tek VM'ler ek olarak izlenebilir.
- PaaS kaynaklarına oluşturulur ve izleme maliyeti görünür; Ancak, VM otomatik kapatma PaaS kaynakları için geçerli değildir.

Çok hakkında daha fazla bilgi [Resource Manager şablonlarını kullanmanın avantajları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) dağıtmak için güncelleştirme veya tüm Laboratuvar kaynaklarınızı tek bir işlemde silme.

> [!NOTE]
> Daha fazla Laboratuvar VM'ler oluşturmak için temel olarak Resource Manager şablonunu kullandığınızda, çoklu VM veya tek Vm'leri oluştururken aklınızda tutmanız gereken bazı farklar vardır. [Bir sanal makinenin Azure Resource Manager şablonu kullanma](devtest-lab-use-resource-manager-template.md) bu farklar daha ayrıntılı açıklanmaktadır.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="devtest-labs-public-environments"></a>DevTest Labs genel ortamları
Azure DevTest Labs sahip bir [Azure Resource Manager şablonları, ortak depo](https://github.com/Azure/azure-devtestlab/tree/master/Environments) kendiniz bir dış GitHub kaynağına bağlanmak zorunda kalmadan ortamlar oluşturmak için kullanabileceğiniz. Bu depo, Azure Web Apps, Service Fabric kümesi ve SharePoint grubu ortamı geliştirme gibi sık kullanılan şablonları içerir. Bu özellik, oluşturduğunuz her bir laboratuvar için gelen ortak depo yapıtları benzer. Ortam depo labs PaaS kaynaklar için bir yumuşak Başlarken deneyimini ile sağlamak için en düşük giriş parametrelerini içeren önceden yazılmış ortam şablonlarını kullanmaya hızlıca başlayın sağlar. Daha fazla bilgi için [yapılandırma ve kullanım ortak ortamlardaki DevTest labs'deki](devtest-lab-configure-use-public-environments.md).

## <a name="configure-your-own-template-repositories"></a>Kendi şablon depoları yapılandırın
Kod olarak altyapı ve yapılandırma olarak kodu ile en iyi biri olarak ortam şablonlarını kaynak denetiminde yönetilmelidir. Azure DevTest Labs, bu yöntem izler ve doğrudan, GitHub veya Azure DevOps Hizmetleri Git depolarından tüm Azure Resource Manager şablonlarını yükler. Sonuç olarak, Resource Manager şablonları, test ortamından üretim ortamına tüm sürüm döngüsü boyunca kullanılabilir.

DevTest Labs ekibi tarafından oluşturulan şablonlarını kullanıma [ortak GitHub deposu](https://github.com/Azure/azure-devtestlab/tree/master/Environments). Bu genel depoda başkaları tarafından doğrudan kullanın veya bunları ihtiyaçlarınıza uyacak şekilde özelleştirmeniz paylaşılan şablonları görüntüleyebilirsiniz. Şablonunuzu oluşturduktan sonra başkalarıyla paylaşmak için bu depoyu depolayın. Ayrıca, kendi Git deposu ile bulut ortamlarında ayarlamak için kullanılan şablonları da ayarlayabilirsiniz. 

Birkaç deposunda Azure Resource Manager şablonlarınızı düzenlemek için izlemek için kurallar şunlardır:

- Ana şablon dosyası adlandırılmalıdır `azuredeploy.json`. 

    ![Anahtar Azure Resource Manager şablon dosyaları](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Bir parametre dosyasında tanımlanan parametre değerlerini kullanmak istiyorsanız, parametre dosyası adlandırılmalıdır `azuredeploy.parameters.json`.
- Parametreleri kullanabilirsiniz `_artifactsLocation` ve `_artifactsLocationSasToken` parametersLink URI değerini oluşturmak için otomatik olarak iç içe geçmiş şablonlarını yönetmek DevTest Labs izin verme. Daha fazla bilgi için [dağıtmak için test ortamlarında iç içe geçmiş Azure Resource Manager şablonları](deploy-nested-template-environments.md).
- Meta veri tanımı ve şablon görünen adı belirtmek için tanımlanabilir. Bu meta verileri, adlı bir dosyaya olmalıdır `metadata.json`. Aşağıdaki örnek meta veri dosyası, görünen ad ve açıklama belirtin gösterilmektedir: 

    ```json
    { 
        "itemDisplayName": "<your template name>", 
        "description": "<description of the template>" 
    }
    ```

Aşağıdaki adımları, laboratuvarınız için Azure portalını kullanarak bir havuz eklerken size kılavuzluk. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin.   
1. Laboratuvar'ın **genel bakış** bölmesinde **yapılandırması ve ilkelerini**.

    ![Yapılandırma ve ilkeler](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Gelen **yapılandırması ve ilkelerini** ayarları listesinden **depoları**. **Depoları** bölmesi laboratuvara eklenmiş olan depoları listeler. Adlı bir depo `Public Repo` tüm Laboratuvar için otomatik olarak oluşturulur ve bağlandığı [DevTest Labs GitHub deposunu](https://github.com/Azure/azure-devtestlab) , kullanımınız için birden fazla VM yapıtları içerir.

    ![Genel depo](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Seçin **Ekle +** Azure Resource Manager şablonu deponuza eklemek için.
1. Zaman ikinci **depoları** bölmesini açar, gerekli bilgileri aşağıdaki gibi girin:
    - **Ad** -laboratuar ortamında kullanılan depo adını girin.
    - **Git kopya URL'si** -GitHub ya da Azure DevOps Hizmetleri HTTPS GIT kopya URL'si girin.  
    - **Dal** -Azure Resource Manager şablonu tanımlarınızı erişmek için dal adı girin. 
    - **Kişisel erişim belirteci** -kişisel erişim belirteci güvenli bir şekilde deponuza erişmek için kullanılır. Azure DevOps hizmetlerinden belirtecinizi ulaşmak için  **&lt;adınız >> Profilimi > Güvenlik > Genel erişim belirteci**. Github'dan belirtecinizi almak için avatarınız seçerek ve ardından seçin **ayarlar > Genel erişim belirteci**. 
    - **Klasör yolları** -iki giriş alanlarını birini kullanarak bir eğik çizgiyle - / - başlar ve göreli ya da, Git clone URI'si yapıt tanımları klasör yolunu girin (ilk giriş alanı) veya Azure Resource Manager şablonu tanımlarınızı .   
    
        ![Genel depo](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Tüm gerekli alanları girildiğinden ve geçemediğinden sonra seçin **Kaydet**.

Sonraki bölümde, ortamlarında bir Azure Resource Manager şablonundan oluşturmada size yol gösterir.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Azure portalını kullanarak Resource Manager şablonundan bir ortam oluşturun

Bir Azure Resource Manager şablon deposu laboratuvarda yapılandırıldıktan sonra Laboratuvar kullanıcılarınızın Azure portalında aşağıdaki adımları kullanarak bir ortam oluşturabilirsiniz:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin.   
1. Laboratuvar bölmeden **Ekle +** .
1. **Temel seçin** bölmesinde listelenen ilk Azure Resource Manager şablonları ile kullanabileceğiniz temel görüntüleri görüntüler. İstenen Azure Resource Manager şablonu seçin.

    ![Bir temel seçin](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Üzerinde **Ekle** bölmesinde girin **ortam adı** değeri. Ortam kullanıcılarınıza Laboratuvardaki görüntülenen addır. Kalan giriş alanları, Azure Resource Manager şablonunda tanımlanır. Varsayılan değerleri şablon tanımlanmışsa veya `azuredeploy.parameter.json` dosya varsa, varsayılan değerleri, bu giriş alanları görüntülenir. Tür parametreleri için *güvenli dize*, Azure anahtar kasasında depolanan gizli dizileri kullanabilirsiniz. Bir anahtar kasasındaki gizli dizileri kaydetme ve Laboratuvar kaynaklarını oluştururken kullanma hakkında bilgi edinmek için [gizli dizileri Azure Key Vault'ta Store](devtest-lab-store-secrets-in-key-vault.md).  

    ![Bölmesi ekleme](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > -Boş değerler olarak belirtilmiş olsa bile - görüntülenir birkaç parametre değeri vardır. Bu nedenle, kullanıcıların bir Azure Resource Manager şablonu parametreleri için değerler atarsanız, DevTest Labs değerlerini görüntülemez. Bunun yerine, boş giriş alanlarını burada Laboratuvar kullanıcıları bir değer ortam oluşturulurken girmelisiniz gösterilir.
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - GENEL-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. Seçin **Ekle** ortam oluşturma. Ortamı başlatır durumu görüntüleme ile hemen sağlama **sanal makinelerim** listesi. Yeni bir kaynak grubu, Azure Resource Manager şablonunda tanımlı tüm kaynakları sağlamak üzere Laboratuvar tarafından otomatik olarak oluşturulur.
1. Ortam oluşturulduktan sonra ortam seçin **sanal makinelerim** listesi kaynak grubu bölmesini açın ve tüm ortamda sağlanmış olan kaynakları göz atın.
    
    ![Sanal makineler listem](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Ortamda yalnızca sağlanan VM'lerin listesini görüntülemek için ortama daha da genişletebilirsiniz.
    
    ![Sanal makineler listem](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Veri diskleri, değişen otomatik kapatma saatinin ve diğer ekleme yapıtları uygulamadan gibi kullanılabilir eylemler - görüntülemek için ortamları birine tıklayın.

    ![Ortam eylemleri](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="automate-deployment-of-environments"></a>Ortamların dağıtımını otomatikleştirme
Azure DevTest Labs kullanma olanağı sağlayan bir [Azure kaynak yönetimi Yöneticisi şablonu](../azure-resource-manager/resource-group-authoring-templates.md) bir grup kaynakla Laboratuvardaki ortam oluşturma. Bu ortamların Resource Manager şablonları kullanılarak oluşturulabilir tüm Azure kaynakları içerebilir. DevTest Labs ortamları kolayca karmaşık altyapı kurmanız laboratuvarı sınırlar içinde tutarlı bir şekilde dağıtmak kullanıcılara izin verin. Şu anda bir ortam, Azure portalını kullanarak bir laboratuvara ekleme kez oluştururken uygun olsa da bir geliştirme veya birden çok oluşturma nerede meydana, bir test durumu otomatik bir dağıtım için geliştirilmiş bir deneyim sağlar.

Aşağıdaki tüm adımları [kendi şablon depoları yapılandırma](#configure-your-own-template-repositories) bölümden devam etmeden önce: 

1. Oluşturulan kaynakları tanımlayan Resource Manager şablonu oluşturun. 
2. Resource Manager şablonu bir depo Git'te ayarlayın. 
3. Git deposu laboratuvar'a Bağlan. 

### <a name="powershell-script-to-deploy-the-resource-manager-template"></a>Resource Manager şablonu dağıtmak için PowerShell Betiği
Sabit disk için sonraki bölümde PowerShell betiğini kaydetme (örneğin: deployenv.ps1) ve değerleri belirttikten sonra Subscriptionıd, ResourceGroupName, LabName, RepositoryName, komut dosyasını çalıştırmak Git deposunda EnvironmentName TemplateName (klasör).

```powershell
./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"  
```

#### <a name="sample-script"></a>Örnek betik
Örnek betik, bir laboratuar ortamında bir ortam oluşturmak için aşağıda verilmiştir. Komut dosyasındaki yorumlar komut dosyası daha iyi anlamanıza yardımcı olur. 

```powershell
#Requires -Module Az.Resources

[CmdletBinding()]

param (
# ID of the Azure Subscription where the lab is created.
[string] [Parameter(Mandatory=$true)] $SubscriptionId,

# Name of the lab (existing) in which to create the environment.
[string] [Parameter(Mandatory=$true)] $LabName,

# Name of the connected repository in the lab. 
[string] [Parameter(Mandatory=$true)] $RepositoryName,

# Name of the template (folder name in the Git repository) based on which the environment will be created.
[string] [Parameter(Mandatory=$true)] $TemplateName,

# Name of the environment to be created in the lab.
[string] [Parameter(Mandatory=$true)] $EnvironmentName,

# The parameters to be passed to the template. Each parameter is prefixed with “-param_”. 
# For example, if the template has a parameter named “TestVMName” with a value of “MyVMName”, the string in $Params will have the form: `-param_TestVMName MyVMName`. 
# This convention allows the script to dynamically handle different templates.
[Parameter(ValueFromRemainingArguments=$true)]
    $Params
)

# Save this script as the deployenv.ps1 file
# Run the script after you specify values for SubscriptionId, ResourceGroupName, LabName, RepositoryName, TemplateName (folder) in the Git repo, EnvironmentName
# ./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"    

# Comment this statement to completely automate the environment creation.    
# Sign in to Azure. 
Connect-AzAccount

# Select the subscription that has the lab.  
Set-AzContext -SubscriptionId $SubscriptionId | Out-Null

# Get information about the user, specifically the user ID, which is used later in the script.  
$UserId = $((Get-AzADUser -UserPrincipalName (Get-AzContext).Account).Id.Guid)
        
# Get information about the lab such as lab location. 
$lab = Get-AzResource -ResourceType "Microsoft.DevTestLab/labs" -Name $LabName -ResourceGroupName $ResourceGroupName 
if ($lab -eq $null) { throw "Unable to find lab $LabName in subscription $SubscriptionId." } 
    
# Get information about the repository in the lab. 
$repository = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
    -ResourceName $LabName `
    -ApiVersion 2016-05-15 `
    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($repository -eq $null) { throw "Unable to find repository $RepositoryName in lab $LabName." } 

# Get information about the Resource Manager template based on which the environment will be created. 
$template = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType "Microsoft.DevTestLab/labs/artifactSources/armTemplates" `
    -ResourceName "$LabName/$($repository.Name)" `
    -ApiVersion 2016-05-15 `
    | Where-Object { $TemplateName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($template -eq $null) { throw "Unable to find template $TemplateName in lab $LabName." } 

# Build the template parameters with parameter name and values.     
$parameters = Get-Member -InputObject $template.Properties.contents.parameters -MemberType NoteProperty | Select-Object -ExpandProperty Name
$templateParameters = @()

# The custom parameters need to be extracted from $Params and formatted as name/value pairs.
$Params | ForEach-Object {
    if ($_ -match '^-param_(.*)' -and $Matches[1] -in $parameters) {
        $name = $Matches[1]                
    } elseif ( $name ) {
        $templateParameters += @{ "name" = "$name"; "value" = "$_" }
        $name = $null #reset name variable
    }
}

# Once name/value pairs are isolated, create an object to hold the necessary template properties
$templateProperties = @{ "deploymentProperties" = @{ "armTemplateId" = "$($template.ResourceId)"; "parameters" = $templateParameters }; } 

# Now, create or deploy the environment in the lab by using the New-AzResource command. 
New-AzResource -Location $Lab.Location `
    -ResourceGroupName $lab.ResourceGroupName `
    -Properties $templateProperties `
    -ResourceType 'Microsoft.DevTestLab/labs/users/environments' `
    -ResourceName "$LabName/$UserId/$EnvironmentName" `
    -ApiVersion '2016-05-15' -Force 
 
Write-Output "Environment $EnvironmentName completed."
```

Kaynakları Resource Manager şablonları ile dağıtmak için Azure CLI de kullanabilirsiniz. Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli).

> [!NOTE]
> Laboratuvar sahibi izinlere sahip bir kullanıcı Azure PowerShell kullanarak Resource Manager şablonundan Vm'leri oluşturabilirsiniz. Resource Manager şablonu kullanarak VM oluşturmayı otomatikleştirmek istediğiniz ve yalnızca kullanıcı izinlerine sahip, kullanabileceğiniz [ **az lab vm oluşturma** CLI komutunu](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create).

### <a name="general-limitations"></a>Genel sınırlamalar 

Bu sınırlamalar, DevTest Labs'de bir Resource Manager şablonu kullanarak yaparken dikkate alın:

- Oluşturduğunuz herhangi bir Resource Manager şablonu mevcut kaynaklara başvuramaz; yalnızca yeni kaynaklar için de başvurabilir. Ayrıca, genellikle DevTest Labs dışında dağıttığınız var olan bir Resource Manager şablonu sahip ve mevcut kaynaklara başvurular içeriyorsa, laboratuar ortamında kullanılamaz.

   Bunun tek özel durumu olan, **olabilir** bir sanal ağınız başvuru. 

- Formülleri Laboratuvar bir Resource Manager şablonundan oluşturulan VM'ler öğesinden oluşturulamaz. 

- Özel görüntüler, Laboratuvar bir Resource Manager şablonundan oluşturulan VM'ler öğesinden oluşturulamaz. 

- Resource Manager şablonları dağıttığınızda çoğu ilkeleri değerlendirilmez.

   Örneğin, bir kullanıcı yalnızca beş VM'ler oluşturabilirsiniz belirten bir laboratuvar ilkesi olabilir. Ancak, bir kullanıcı VM onlarca oluşturan Resource Manager şablonu dağıtabilirsiniz. Değerlendirilmez ilkeleri şunlardır:

   - Kullanıcı başına sanal makine sayısı
   - Laboratuvar kullanıcı başına premium VM sayısı
   - Premium disk Laboratuvar kullanıcı sayısı


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Ortam kaynak grubuna erişim hakları Laboratuvar kullanıcıları için yapılandırma

Laboratuvar kullanıcıları, Resource Manager şablonu dağıtabilirsiniz. Ancak varsayılan olarak, bu ortam kaynak grubundaki kaynakları değiştiremez anlamına gelir okuyucu erişim hakları sahiptir. Örneğin, bunlar durduramaz veya kaynaklarını başlatın.

Katkıda bulunan erişimi hakları Laboratuvar kullanıcılarınıza sunmak için aşağıdaki adımları izleyin. Resource Manager şablonu dağıttıklarında, uygulanırsa Bu ortamdaki kaynakları düzenlemeniz mümkün olur. 


1. Üzerinde Laboratuvar **genel bakış** bölmesinde **yapılandırması ve ilkelerini**.
1. Seçin **Laboratuvar ayarları**.
1. Laboratuvar ayarları bölmesinde seçin **katkıda bulunan** Laboratuvar kullanıcılara yazma izinleri vermek için.

    ![Laboratuvar kullanıcı erişim haklarını yapılandırma](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Connect** sanal makinenin yönetim bölmesinde.
* Görüntüleme ve ortamda seçerek ortamdaki kaynakları yönetme **sanal makinelerim** laboratuvarınızda listesi. 
* Keşfedin [Azure hızlı başlangıç Şablon Galerisi, Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates).
