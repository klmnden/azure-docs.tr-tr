---
title: Azure DevTest Labs Laboratuvarınızı yapıt deposu ekleme | Microsoft Docs
description: Azure DevTest Labs Laboratuvarınızı yapıt deposu ekleme hakkında bilgi edinin.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: c391aa157e35bdc389bd30efe48fa380d06c193e
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508364"
---
# <a name="add-an-artifact-repository-to-your-lab-in-devtest-labs"></a>DevTest Labs Laboratuvarınızı yapıt deposu ekleme
DevTest Labs, VM oluşturma veya VM oluşturulduktan sonra sırada bir sanal makineye eklenecek bir yapıt belirtmenizi sağlar. Bu yapıt, bir aracı veya VM üzerinde yüklemek istediğiniz bir uygulama olabilir. Yapıtlar, GitHub veya Azure DevOps Git deposundan yüklenen bir JSON dosyasında tanımlanır. 

[Genel yapıt deposuna](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)DevTest Labs tarafından korunan, hem Windows hem de Linux için birçok yaygın araçları sunar. Bu depo için bir bağlantı, laboratuvarınız için otomatik olarak eklenir. Genel yapıt deposunda kullanılamayan belirli araçlarla kendi yapıt deposu oluşturabilirsiniz. Özel yapıtlar oluşturma hakkında bilgi edinmek için [özel yapıtlar oluşturma](devtest-lab-artifact-author.md).

Bu makalede, Azure portalı, Azure Resource Manager şablonlarını ve Azure PowerShell kullanarak, özel yapıt deposu ekleme hakkında bilgi sağlar. PowerShell veya CLI komutları yazarak bir laboratuvara yapıt deposu ekleme otomatik hale getirebilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar
Laboratuvarınız için bir depo eklemek için ilk olarak, deponuzdan anahtarı bilgilerini alın. Aşağıdaki bölümlerde üzerinde barındırılan depolar için gerekli bilgileri almak nasıl **GitHub** veya **Azure DevOps**.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>GitHub depo kopya URL'si ve kişisel erişim belirteci alma

1. Yapıt ya da Resource Manager şablonu tanımlarını içeren GitHub deposunun giriş sayfasına gidin.
2. **Clone or download**'u (Kopyala veya indir) seçin.
3. URL'yi panoya kopyalamak için seçin **HTTPS kopyası URL'si** düğmesi. Daha sonra kullanmak için URL'yi kaydedin.
4. GitHub sağ üst köşesindeki profil resmini seçin ve ardından **ayarları**.
5. İçinde **kişisel ayarlarını** seçin sol taraftaki menüsünden **Geliştirici ayarları**.
6. Seçin **kişisel erişim belirteçleri** sol menüsünde.
7. Seçin **yeni belirteç Oluştur**.
8. Üzerinde **yeni bir kişisel erişim belirteci** sayfasındaki **belirteç açıklaması**, bir açıklama girin. Altında varsayılan öğeleri kabul **kapsamları seçin**ve ardından **belirteç Oluştur**.
9. Oluşturulan belirtecin kaydedin. Belirteci daha sonra kullanın.
10. GitHub'ı kapatın.   

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>Azure depolarda kopya URL'si ve kişisel erişim belirteci alma
1. Takım koleksiyonunuzun giriş sayfasına gidin (örneğin, https://contoso-web-team.visualstudio.com) ve ardından projenizi seçin.
2. Proje giriş sayfasında **kod**.
3. Kopya URL'si projede görüntülemek için **kod** sayfasında **kopya**.
4. URL'yi kaydedin. Daha sonra URL'yi kullanın.
5. Kullanıcı hesabı aşağı açılan menüden bir kişisel erişim belirteci oluşturmak için seçin **Profilimi**.
6. Profil bilgileri sayfasında seçin **güvenlik**.
7. Üzerinde **güvenlik** sekmesinde **Ekle**.
8. Üzerinde **bir kişisel erişim belirteci oluşturma** sayfası:
   1. Girin bir **açıklama** belirteç için.
   2. İçinde **süresi içinde** listesinden **180 gün**.
   3. İçinde **hesapları** listesinden **tüm erişilebilir hesaplar**.
   4. Seçin **tüm kapsamlar** seçeneği.
   5. Seçin **belirteci oluşturma**.
9. Yeni belirteç görünür **kişisel erişim belirteçleri** listesi. Seçin **kopyalama belirteci**ve daha sonra kullanmak için belirteç değeri kaydedin.
10. Bağlan Laboratuvarınızı depo bölümüne geçin.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, Azure portalında bir laboratuvar yapıt deposuna eklemek için adımları sağlar. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **diğer hizmetler**ve ardından **DevTest Labs** hizmetler listesinden.
3. Laboratuvarınızı labs listesinden seçin. 
4. Seçin **yapılandırması ve ilkelerini** sol menüsünde.
5. Seçin **depoları** altında **dış kaynaklara** sol menüde bölümü.
6. Seçin **+ Ekle** araç.

    ![Depo Ekle düğmesi](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. Üzerinde **depoları** sayfasında, aşağıdaki bilgileri belirtin:
   1. **Ad**. Depo için bir ad girin.
   2. **Git kopya URL'si**. Daha önce kopyaladığınız Azure DevOps Services veya GitHub Git HTTPS kopya URL girin.
   3. **Dal**. Tanımlarınızı almak için dal girin.
   4. **Kişisel erişim belirteci**. Azure DevOps Services veya GitHub daha önce aldığınız kişisel erişim belirteci girin.
   5. **Klasör yolları**. Yapı ya da Resource Manager şablonu tanımlarını içeren kopya URL göreli en az bir klasör yolu girin. Bir alt belirttiğinizde, klasör yoluna eğik çizgi dahil emin olun.

        ![Depoları alanı](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. **Kaydet**’i seçin.

## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanma
Azure kaynak yönetimi (Azure Resource Manager) şablonları oluşturmak istediğiniz Azure kaynakları tanımlayan JSON dosyalarıdır. Bu şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Bu bölümde Azure Resource Manager şablonu kullanarak bir laboratuvara yapıt deposu ekleme adımları sağlar.  Zaten mevcut değilse laboratuvar şablonu oluşturur. 

### <a name="template"></a>Şablon
Bu makalede kullanılan örnek şablon parametreleri aracılığıyla aşağıdaki bilgileri toplar. Parametrelerin çoğu akıllı varsayılanlar olduğu, ancak belirtilmesi gereken birkaç değer vardır. Yapıt deposu URI'sini ve depo için güvenlik belirteci, Laboratuvar adı belirtmeniz gerekir. 

- Laboratuvar adı.
- DevTest Labs kullanıcı arabirimi (UI) yapıt deposunda görünen adı. Varsayılan değer: `Team Repository`.
- Depo URI'si (örnek: `https://github.com/<myteam>/<nameofrepo>.git` veya `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.
- Dal depodaki yapıtları içerir. Varsayılan değer: `master`.
- Yapıtlar içeren klasörün adı. Varsayılan değer: `/Artifacts`.
- Depo türü. İzin verilen değerler `VsoGit` veya `GitHub`.
- Depo için erişim belirteci. 

    ```json
    {
    
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "labName": {
                "type": "string"
            },
            "artifactRepositoryDisplayName": {
                "type": "string",
                "defaultValue": "Team Repository"
            },
            "artifactRepoUri": {
                "type": "string"
            },
            "artifactRepoBranch": {
                "type": "string",
                "defaultValue": "master"
            },
            "artifactRepoFolder": {
                "type": "string",
                "defaultValue": "/Artifacts"
            },
            "artifactRepoType": {
                "type": "string",
                "allowedValues": ["VsoGit", "GitHub"]
            },
            "artifactRepoSecurityToken": {
                "type": "securestring"
            }
        },
        "variables": {
            "artifactRepositoryName": "[concat('Repo-', uniqueString(subscription().subscriptionId))]"
        },
        "resources": [{
                "apiVersion": "2016-05-15",
                "type": "Microsoft.DevTestLab/labs",
                "name": "[parameters('labName')]",
                "location": "[resourceGroup().location]",
                "resources": [
                    {
                        "apiVersion": "2016-05-15",
                        "name": "[variables('artifactRepositoryName')]",
                        "type": "artifactSources",
                        "dependsOn": [
                            "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
                        ],
                        "properties": {
                            "uri": "[parameters('artifactRepoUri')]",
                            "folderPath": "[parameters('artifactRepoFolder')]",
                            "branchRef": "[parameters('artifactRepoBranch')]",
                            "displayName": "[parameters('artifactRepositoryDisplayName')]",
                            "securityToken": "[parameters('artifactRepoSecurityToken')]",
                            "sourceType": "[parameters('artifactRepoType')]",
                            "status": "Enabled"
                        }
                    }
                ]
            }
        ]
    }
    ```


### <a name="deploy-the-template"></a>Şablonu dağıtma
Şablonu Azure'a dağıtma ve mevcut değilse mevcut olmaması halinde, oluşturulduğunda veya güncelleştirildiğinde, kaynak sağlamak için birkaç yolu vardır. Ayrıntılar için aşağıdaki makalelere bakın:

- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
- [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md)
- [Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md)
- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/resource-group-template-deploy-rest.md)

Yeni bir ubuntu ve PowerShell'de şablonu dağıtmak bkz. Şablonu dağıtmak için kullanılan cmdlet'ler bağlam özgü olduğundan geçerli Kiracı ve geçerli aboneliğin kullanılır. Kullanım [kümesi AzContext](/powershell/module/az.accounts/set-azcontext) gerekirse bağlamı değiştirmek için şablonu dağıtmadan önce.

İlk olarak kullanarak bir kaynak grubu oluşturun. [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Zaten kullanmak istediğiniz kaynak grubu varsa, bu adımı atlayın.

```powershell
New-AzResourceGroup -Name MyLabResourceGroup1 -Location westus
```

Ardından, kaynak grubu kullanarak bir dağıtım oluşturun [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Bu cmdlet, Azure'a kaynak değişiklikleri uygular. Çeşitli kaynak dağıtımlar, herhangi belirli bir kaynak grubuna yapılabilir. Birkaç kez aynı kaynak grubuna dağıtıyorsanız, her dağıtım adının benzersiz olduğundan emin olun.

```powershell
New-AzResourceGroupDeployment `
    -Name MyLabResourceGroup-Deployment1 `
    -ResourceGroupName MyLabResourceGroup1 `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

New-AzResourceGroupDeployment çalıştırdıktan sonra başarıyla komutu (başarılı olması) sağlama durumu ve şablon için herhangi bir çıkış gibi önemli bilgiler çıkarır.
 
## <a name="use-azure-powershell"></a>Azure PowerShell kullanma 
Bu bölümde bir laboratuvara yapıt deposu ekleme için kullanılan bir örnek PowerShell Betiği sağlanır. Azure PowerShell yoksa bkz [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview?view=azps-1.2.0) yüklemek ayrıntılı yönergeler için.

### <a name="full-script"></a>Tam betik
Aşağıda, bazı ayrıntılı ileti çıkışı yapar ve yorumlarla birlikte tam komut verilmiştir:

**Yeni DevTestLabArtifactRepository.ps1**:

```powershell

<#

.SYNOPSIS
This script creates a new custom repository and adds it to an existing DevTest Lab.

.PARAMETER LabName
The name of the lab.

.PARAMETER LabResourceGroupName
The name of the resource group that contains the lab. 

.PARAMETER ArtifactRepositoryName
Name for the new artifact repository.
Script creates a random name for the respository if it is not specified.

.PARAMETER ArtifactRepositoryDisplayName
Display name for the artifact repository.
This is the name that shows up in the Azure portal (https://portal.azure.com) when viewing all the artifact repositories for a lab.

.PARAMETER RepositoryUri
Uri to the repository.

.PARAMETER RepositoryBranch
Branch in which artifact files can be found. Defaults to 'master'.

.PARAMETER FolderPath
Folder under which artifacts can be found. Defaults to '/Artifacts'

.PARAMETER PersonalAccessToken
Security token for access to GitHub or VSOGit repository.
See https://azure.microsoft.com/documentation/articles/devtest-lab-add-artifact-repo/ for instructions to get personal access token

.PARAMETER SourceType
Whether artifact is VSOGit or GitHub repository.

.EXAMPLE
Set-AzContext -SubscriptionId 11111111-1111-1111-1111-111111111111
.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"

.NOTES
Script uses the current Az context. To set the context, use the Set-AzContext cmdlet

#>

 
[CmdletBinding()]
Param(

    [Parameter(Mandatory=$true)]
    $LabName,

    [Parameter(Mandatory=$true)]
    $LabResourceGroupName,
    $ArtifactRepositoryName,
    $ArtifactRepositoryDisplayName  = 'Team Artifact Repository',

    [Parameter(Mandatory=$true)]
    $RepositoryUri,
    $RepositoryBranch = 'master',
    $FolderPath = '/Artifacts',
    
    [Parameter(Mandatory=$true)]
    $PersonalAccessToken ,
    
    [Parameter(Mandatory=$true)]
    [ValidateSet('VsoGit', 'GitHub')]
    $SourceType
)


#Set artifact repository internal name,
# if not set by user.

if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}

# Sign in to Azure
Connect-AzAccount


#Get Lab Resource
$LabResource = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceName $LabName -ResourceGroupName $LabResourceGroupName

Write-Verbose "Lab Name: $($LabResource.Name)"
Write-Verbose "Lab Resource Group Name: $($LabResource.ResourceGroupName)"
Write-Verbose "Lab Resource Location: $($LabResource.Location)"

Write-Verbose "Artifact Repository Internal Name: $ArtifactRepositoryName"

#Prepare properties object for call to New-AzResource
$propertiesObject = @{
    uri = $RepositoryUri;
    folderPath = $FolderPath;
    branchRef = $RepositoryBranch;
    displayName = $ArtifactRepositoryDisplayName;
    securityToken = $PersonalAccessToken;
    sourceType = $SourceType;
    status = 'Enabled'
}

Write-Verbose @"Properties to be passed to New-AzResource:$($propertiesObject | Out-String)"

#Resource will be added to current subscription.
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
Write-Verbose "Az ResourceType: $resourcetype"
Write-Verbose "Az ResourceName: $resourceName"
 
Write-Verbose "Creating artifact repository '$ArtifactRepositoryDisplayName'..."
$result = New-AzResource -Location $LabResource.Location -ResourceGroupName $LabResource.ResourceGroupName -properties $propertiesObject -ResourceType $resourcetype -ResourceName $resourceName -ApiVersion 2016-05-15 -Force


#Alternate implementation:
# Use resourceId rather than resourcetype and resourcename parameters.
# Using resourceId allows you to specify the $SubscriptionId rather than using the
# subscription id of Get-AzContext.
#$resourceId = "/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
#$result = New-AzResource -properties $propertiesObject -ResourceId $resourceId -ApiVersion 2016-05-15 -Force


# Check the result
if ($result.Properties.ProvisioningState -eq "Succeeded") {
    Write-Verbose ("Successfully added artifact repository source '$ArtifactRepositoryDisplayName'")
}
else {
    Write-Error ("Error adding artifact repository source '$ArtifactRepositoryDisplayName'")
}

#Return the newly created resource so it may be used in subsequent scripts
return $result
```

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma
Aşağıdaki örnek komut dosyasının nasıl çalıştırılacağı gösterilmektedir: 

```powershell
Set-AzContext -SubscriptionId <Your Azure subscription ID>

.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"
```


### <a name="parameters"></a>Parametreler
Bu makaledeki örnek PowerShell Betiği aşağıdaki parametreleri alır:

| Parametre | Açıklama | 
| --------- | ----------- | 
| LabName | Laboratuvar adı. |
| ArtifactRepositoryName | Yeni yapıt deposu adı. Belirtilmezse betik deposu için rastgele bir ad oluşturur. |
| ArtifactRepositoryDisplayName | Yapıt deposu için görünen ad. Bu, Azure portalında gösterilir addır (https://portal.azure.com) bir laboratuvara yönelik tüm yapıt depolar görüntülerken. |
| RepositoryUri | Depo URI'si. Örnekler: `https://github.com/<myteam>/<nameofrepo>.git` veya `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.| 
| RepositoryBranch | Dal dosyalarını hangi yapıt içinde bulunabilir. Varsayılan olarak 'master'. | 
| folderPath | Klasör yapıtları altında bulunabilir. Varsayılan olarak ' / Yapıtları |
| personalAccessToken | GitHub veya VSOGit depoya erişmek için güvenlik belirteci. Kişisel erişim belirteci almak yönergeler için Önkoşullar bölümüne bakın |
| KaynakTürü | Yapıt VSOGit veya GitHub deposunda olup olmadığı. |

Depo bir iç gereken farklı kimlik doğrulaması için Azure portalında görünen görünen adı. Azure portalını kullanarak iç adı görmüyorsanız, ancak Azure REST API'lerini veya Azure PowerShell kullanırken görmeyi. Bir betiğimizi kullanıcı tarafından belirtilmezse, betik, bir ad sağlar.

```powershell
#Set artifact repository name, if not set by user
if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}
```

### <a name="powershell-commands-used-in-the-script"></a>Betikte kullanılan PowerShell komutları

| PowerShell komutu | Notlar |
| ------------------ | ----- |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Bu komut, konumu gibi Laboratuvar hakkında bilgi almak için kullanılır. |
| [Yeni AzResource](/powershell/module/az.resources/new-azresource) | Yapıt deposu ekleme için belirli bir komut yoktur. Genel [yeni AzResource](/powershell/module/az.resources/new-azresource) cmdlet işi yapar. Bu cmdlet ya da gereken **ResourceId** veya **ResourceName** ve **ResourceType** oluşturmak için kaynak türünü bilmeniz çifti. Bu örnek betik, kaynak adı ve kaynak türü çifti kullanır. <br/><br/>Yapıt deposu kaynağı ile aynı konumda ve Laboratuvar aynı kaynak grubunda oluşturduğunuzu dikkat edin.|

Betik yeni bir kaynak mevcut aboneliğe ekler. Kullanım [Get-AzContext](/powershell/module/az.accounts/get-azcontext) bu bilgileri görmek için. Kullanım [kümesi AzContext](/powershell/module/az.accounts/set-azcontext) geçerli Kiracı ve abonelik ayarlamak için.

Kaynak adı ve kaynak türü bilgilerini bulmak için en iyi yolu kullanmaktır [Test sürücü Azure REST API'leri](https://azure.github.io/projects/apis/) Web sitesi. Kullanıma [DevTest Labs: 2016-05-15](https://aka.ms/dtlrestapis) REST API'leri için DevTest Labs sağlayıcısı görmek için sağlayıcı. Betik kullanıcılar aşağıdaki kaynak kimliği. 

```powershell
"/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
```
 
Her şeyi kaşlı ayraçlar içinde listelenen öğeleri hariç URI 'providers' sonra listelenen kaynak türüdür. Kaynak adı kaşlı ayraçlar içinde görülen her şey değildir. Kaynak adı için birden fazla öğe bekleniyorsa, her öğe biz yaptığınız gibi bir eğik çizgi ile ayırın. 

```powershell
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
```


## <a name="next-steps"></a>Sonraki adımlar
- [Azure DevTest Labs'de laboratuvarınız için zorunlu yapıtları belirtin](devtest-lab-mandatory-artifacts.md)
- [DevTest Labs sanal makineniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
- [Laboratuvardaki yapıt hatalarını tanılama](devtest-lab-troubleshoot-artifact-failure.md)

