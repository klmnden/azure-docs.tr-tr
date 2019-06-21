---
title: Azure işlem hatları ve Resource Manager şablonları ile CI/CD
description: Resource Manager şablonları dağıtmak için Visual Studio'da Azure kaynak grubu dağıtım projeleri kullanarak sürekli tümleştirme, Azure işlem hatlarını ayarlama işlemi açıklanmaktadır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: article
ms.date: 06/12/2019
ms.author: tomfitz
ms.openlocfilehash: b70b38904c0373c53c3731dd0442511116d9c4de
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191466"
---
# <a name="integrate-resource-manager-templates-with-azure-pipelines"></a>Resource Manager şablonları, Azure işlem hattı ile tümleştirin

Visual Studio şablonları oluşturmak ve bunları Azure aboneliğinize dağıtmak için Azure kaynak grubu projesi sağlar. Bu proje, sürekli tümleştirme ve sürekli dağıtım (CI/CD) için Azure işlem hatları ile tümleştirebilirsiniz.

Azure işlem hatları ile şablon dağıtımı iki yolu vardır:

* **Azure PowerShell Betiği çalıştıran bir görev eklemek**. Bu seçenek (AzureResourceGroup.ps1 dağıtma) Visual Studio projesinde bulunan aynı betiği kullandığından geliştirme yaşam döngüsü boyunca tutarlılık sağlayan avantajı vardır. Betik aşamaları yapılara projenizden Resource Manager erişimi olan bir depolama hesabı. Yapıtlar, bağlı şablonların, betikler ve uygulama ikili dosyalar gibi projenizdeki öğelerdir. Ardından, betiği şablonu dağıtır.

* **Görev kopyalayın ve dağıtım görevleri ekleme**. Bu seçenek Proje betik uygun bir alternatif sunar. İşlem hattı,'de iki görevleri yapılandırın. Yapıtlar bir görev aşamaları ve başka bir görev şablonu dağıtır.

Bu makalede her iki yaklaşım gösterilmektedir.

## <a name="prepare-your-project"></a>Projenizi hazırlama

Bu makalede, Visual Studio projesi ve Azure DevOps kuruluş işlem hattı oluşturmak için hazır olduğunu varsayar. Aşağıdaki adımlarda, hazır olduğunuzdan emin olmak gösterilmektedir:

* Azure DevOps kuruluş var. Biri yoksa [ücretsiz oluşturun](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops). Takımınızın zaten bir Azure DevOps kuruluş varsa, kullanmak istediğiniz Azure DevOps projesi Yöneticisi olduğunuzdan emin olun.

* Yapılandırdığınız bir [hizmeti bağlantısı](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops) Azure aboneliğinize. İşlem hattında görevleri, hizmet sorumlusu kimliği altında yürütün. Bağlantı oluşturma adımları için bkz: [DevOps projesi oluşturma](resource-manager-tutorial-use-azure-pipelines.md#create-a-devops-project).

* Sahip olduğunuz Visual Studio projesi oluşturulduğu **Azure kaynak grubu** Başlangıç şablonu. Bu türde bir proje oluşturma hakkında daha fazla bilgi için bkz: [Visual Studio aracılığıyla Azure kaynak gruplarını oluşturma ve dağıtma](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

* Visual Studio projeniz [için bir Azure DevOps projesi bağlı](/azure/devops/repos/git/share-your-code-in-git-vs-2017?view=azure-devops).

## <a name="create-pipeline"></a>İşlem hattı oluşturma

1. Daha önce bir işlem hattı eklemediyseniz, yeni bir işlem hattı oluşturmak gerekir. Azure DevOps kuruluşunuzdan, seçmek **işlem hatları** ve **yeni işlem hattı**.

   ![Yeni işlem hattı ekleyin](./media/vs-resource-groups-project-devops-pipelines/new-pipeline.png)

1. Kodunuzu nerede depolandığını belirtin. Aşağıdaki resimde gösterilmektedir seçerek **Azure depoları Git**.

   ![Kod kaynağı seçin](./media/vs-resource-groups-project-devops-pipelines/select-source.png)

1. Bu kaynak, projeniz için kod deposu seçin.

   ![Depo seçin](./media/vs-resource-groups-project-devops-pipelines/select-repo.png)

1. İşlem hattı oluşturmak için türünü seçin. Seçebileceğiniz **başlangıç işlem hattı**.

   ![İşlem hattını seçin](./media/vs-resource-groups-project-devops-pipelines/select-pipeline.png)

Ya da Azure PowerShell görev veya dosya Kopyala ekleme ve dağıtım görevleri hazırsınız demektir.

## <a name="azure-powershell-task"></a>Azure PowerShell görevi

Bu bölümde, projenizde PowerShell Betiği çalıştıran tek bir görevi kullanarak sürekli dağıtım yapılandırma işlemi gösterilmektedir. Aşağıdaki YAML dosyası oluşturur bir [Azure PowerShell görev](/azure/devops/pipelines/tasks/deploy/azure-powershell?view=azure-devops):

```yaml
pool:
  name: Hosted Windows 2019 with VS2019
  demands: azureps

steps:
- task: AzurePowerShell@3
  inputs:
    azureSubscription: 'demo-deploy-sp'
    ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
    ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' 
    azurePowerShellVersion: LatestVersion
```

Görev ayarlandığında `AzurePowerShell@3`, işlem hattı, bağlantı kimliğini doğrulamak için AzureRM modülünü komutları kullanır. Varsayılan olarak, Visual Studio projesinde PowerShell betiğini AzureRM modülünü kullanır. Kullanılacak betiğinizi güncelleştirdiyseniz [Az modül](/powershell/azure/new-azureps-module-az), görev kümesine `AzurePowerShell@4`.

```yaml
steps:
- task: AzurePowerShell@4
```

İçin `azureSubscription`, oluşturduğunuz hizmet bağlantı adını sağlayın.

```yaml
inputs:
    azureSubscription: '<your-connection-name>'
```

İçin `scriptPath`, ardışık düzen dosyası betiğinizi göreli yolunu belirtin. Yolun görmek için deponuzda bakabilirsiniz.

```yaml
ScriptPath: '<your-relative-path>/<script-file-name>.ps1'
```

Aşama yapılar için gereksinim duymuyorsanız dağıtımı için kullanılacak bir kaynak grubunun konumunu ve adını geçirmeniz yeterlidir. Zaten mevcut değilse Visual Studio komut dosyası kaynak grubu oluşturur.

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>'
```

Mevcut bir depolama hesabı için aşama yapıtları gerekiyorsa kullanın:

```yaml
ScriptArguments: -ResourceGroupName '<resource-group-name>' -ResourceGroupLocation '<location>' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName '<your-storage-account>'
```

Şimdi, görev oluşturma, işlem hattını düzenleme adımlarını Bahsedelim anlama.

1. İşlem hattınızı açın ve içeriğini, YAML ile değiştirin:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019
     demands: azureps

   steps:
   - task: AzurePowerShell@3
     inputs:
       azureSubscription: 'demo-deploy-sp'
       ScriptPath: 'AzureResourceGroupDemo/Deploy-AzureResourceGroup.ps1'
       ScriptArguments: -ResourceGroupName 'demogroup' -ResourceGroupLocation 'centralus' -UploadArtifacts -ArtifactStagingDirectory '$(Build.StagingDirectory)' -StorageAccountName 'stage3a4176e058d34bb88cc'
       azurePowerShellVersion: LatestVersion
   ```

1. **Kaydet**’i seçin.

   ![İşlem hattı Kaydet](./media/vs-resource-groups-project-devops-pipelines/save-pipeline.png)

1. İşleme için bir ileti girin ve doğrudan işleme **ana**.

1. Seçtiğinizde, **Kaydet**, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızı özeti dönün ve durumunu izleyin.

   ![Sonuçları görüntüleme](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Görevler hakkında daha fazla ayrıntı görmek için şu anda çalışan işlem hattı seçebilirsiniz. Tamamlandığında, sonuçları her adım için bkz.

## <a name="copy-and-deploy-tasks"></a>Kopyalama ve dağıtım görevleri

Bu bölümde, yapıtlar hazırlayın ve şablonu dağıtmak için iki görevi kullanarak sürekli dağıtım yapılandırma işlemi gösterilmektedir. 

Aşağıdaki YAML gösterildiği [Azure dosya kopyalama görevi](/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops):

```yaml
- task: AzureFileCopy@3
  displayName: 'Stage files'
  inputs:
    SourcePath: 'AzureResourceGroup1'
    azureSubscription: 'demo-deploy-sp'
    Destination: 'AzureBlob'
    storage: 'stage3a4176e058d34bb88cc'
    ContainerName: 'democontainer'
    outputStorageUri: 'artifactsLocation'
    outputStorageContainerSasToken: 'artifactsLocationSasToken'
    sasTokenTimeOutInMinutes: '240'
```

Ortamınız için gözden geçirmek için bu görevi, birden çok bölümü vardır. `SourcePath` Yapıtlar ardışık düzen dosyası göreli konumunu belirtir. Bu örnekte, dosyaları adlı bir klasörde mevcut `AzureResourceGroup1` olduğu projenin adı.

```yaml
SourcePath: '<path-to-artifacts>'
```

İçin `azureSubscription`, oluşturduğunuz hizmet bağlantı adını sağlayın.

```yaml
azureSubscription: '<your-connection-name>'
```

Depolama ve kapsayıcı adı için depolama hesabı ve kapsayıcı yapıtları depolamak için kullanmak istediğiniz adlarını sağlar. Depolama hesabının mevcut olması gerekir.

```yaml
storage: '<your-storage-account-name>'
ContainerName: '<container-name>'
```

Aşağıdaki YAML gösterildiği [Azure kaynak grubu dağıtım görevine](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment?view=azure-devops):

```yaml
- task: AzureResourceGroupDeployment@2
  displayName: 'Deploy template'
  inputs:
    azureSubscription: 'demo-deploy-sp'
    resourceGroupName: 'demogroup'
    location: 'centralus'
    templateLocation: 'URL of the file'
    csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
    csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
    overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
```

Ortamınız için gözden geçirmek için bu görevi, birden çok bölümü vardır. İçin `azureSubscription`, oluşturduğunuz hizmet bağlantı adını sağlayın.

```yaml
azureSubscription: '<your-connection-name>'
```

İçin `resourceGroupName` ve `location`, dağıtmak istediğiniz kaynak grubunun konumunu ve adını sağlayın. Mevcut değilse, görev kaynak grubu oluşturur.

```yaml
resourceGroupName: '<resource-group-name>'
location: '<location>'
```

Adlı bir şablonu dağıtım görev bağlantılar `WebSite.json` ve WebSite.parameters.json adlı bir parametre dosyası. Şablonu ve parametre dosyalarınızı adlarını kullanın.

Şimdi, görevleri oluşturun, Haydi adımlarını ardışık düzen işlemini anlama.

1. İşlem hattınızı açın ve içeriğini, YAML ile değiştirin:

   ```yaml
   pool:
     name: Hosted Windows 2019 with VS2019

   steps:
   - task: AzureFileCopy@3
     displayName: 'Stage files'
     inputs:
       SourcePath: 'AzureResourceGroup1'
       azureSubscription: 'demo-deploy-sp'
       Destination: 'AzureBlob'
       storage: 'stage3a4176e058d34bb88cc'
       ContainerName: 'democontainer'
       outputStorageUri: 'artifactsLocation'
       outputStorageContainerSasToken: 'artifactsLocationSasToken'
       sasTokenTimeOutInMinutes: '240'
   - task: AzureResourceGroupDeployment@2
     displayName: 'Deploy template'
     inputs:
       azureSubscription: 'demo-deploy-sp'
       resourceGroupName: demogroup
       location: 'centralus'
       templateLocation: 'URL of the file'
       csmFileLink: '$(artifactsLocation)WebSite.json$(artifactsLocationSasToken)'
       csmParametersFileLink: '$(artifactsLocation)WebSite.parameters.json$(artifactsLocationSasToken)'
       overrideParameters: '-_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken "$(artifactsLocationSasToken)"'
   ```

1. **Kaydet**’i seçin.

1. İşleme için bir ileti girin ve doğrudan işleme **ana**.

1. Seçtiğinizde, **Kaydet**, derleme işlem hattı otomatik olarak çalıştırılır. Derleme işlem hattınızı özeti dönün ve durumunu izleyin.

   ![Sonuçları görüntüleme](./media/vs-resource-groups-project-devops-pipelines/view-results.png)

Görevler hakkında daha fazla ayrıntı görmek için şu anda çalışan işlem hattı seçebilirsiniz. Tamamlandığında, sonuçları her adım için bkz.

## <a name="next-steps"></a>Sonraki adımlar

Azure işlem hatları ile Resource Manager şablonları kullanarak adım adım işlemi için bkz [Öğreticisi: Azure Resource Manager şablonlarının Azure işlem hatları ile sürekli tümleştirme](resource-manager-tutorial-use-azure-pipelines.md).
