---
title: Azure DevOps - Azure işlevleri'ni kullanarak kod güncelleştirmelerini işlevi sürekli teslim
description: Azure işlevleri hedefleyen bir Azure DevOps işlem hattı ayarlayın öğrenin.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.openlocfilehash: 0fdad0caa2deef0d7d55b30a85632f72f4ff0ecc
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594470"
---
# <a name="continuous-delivery-by-using-azure-devops"></a>Azure DevOps kullanarak sürekli teslim

Kullanarak işleviniz için bir Azure işlev uygulaması otomatik olarak dağıtabilirsiniz [Azure işlem hatları](/azure/devops/pipelines/).

Ardışık düzeninize tanımlama için iki seçeneğiniz vardır:

- **YAML dosyası**: Bir YAML dosyası, potansiyel açıklar. Dosyanın bir derleme adımları ve yayın bölümleri olabilir. YAML dosyası uygulama ile aynı depodaki olması gerekir.
- **Şablon**: Şablonları, yapı veya uygulamanızı dağıtma hazır görevlerdir.

## <a name="yaml-based-pipeline"></a>YAML tabanlı işlem hattı

Bir YAML tabanlı işlem hattı oluşturmak için ilk uygulamanızı oluşturun ve sonra uygulamayı dağıtma.

### <a name="build-your-app"></a>Uygulamanızı oluşturun

Azure işlem hatları uygulamanızda nasıl oluşturacağınız uygulamanızın programlama diline bağlıdır. Her dilin dağıtım yapıt oluşturma özel derleme adımları vardır. Bir dağıtım yapı, işlev uygulamanızı Azure'a dağıtmak için kullanılır.

#### <a name="net"></a>.NET

Aşağıdaki örnek, bir .NET uygulaması derlemek için bir YAML dosyası oluşturmak için kullanabilirsiniz:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- script: |
    dotnet restore
    dotnet build --configuration Release
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    arguments: '--configuration Release --output publish_output'
    projects: '*.csproj'
    publishWebProjects: false
    modifyOutputPath: true
    zipAfterPublish: false
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>JavaScript

Aşağıdaki örnek, bir JavaScript uygulaması oluşturmak için bir YAML dosyası oluşturmak için kullanabilirsiniz:

```yaml
pool:
      vmImage: ubuntu-16.04 # Use 'VS2017-Win2016' if you have Windows native +Node modules
steps:
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    npm install 
    npm run build --if-present
    npm prune --production
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Aşağıdaki örnek, bir Python uygulaması oluşturmak için bir YAML dosyası oluşturmak için kullanabilirsiniz. Python, yalnızca Linux Azure işlevleri için desteklenir.

```yaml
pool:
      vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.6 as required by functions"
  inputs:
    versionSpec: '3.6'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    python3.6 -m venv worker_venv
    source worker_venv/bin/activate
    pip3.6 install setuptools
    pip3.6 install -r requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```
#### <a name="powershell"></a>PowerShell

Aşağıdaki örnek bir PowerShell uygulama paketini yönelik bir YAML dosyası oluşturmak için kullanabilirsiniz. PowerShell, yalnızca Windows Azure işlevleri için desteklenir.

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

YAML dosyanızdaki barındırma işletim sistemi bağlı olarak aşağıdaki YAML örneklerden birini içermelidir.

#### <a name="windows-function-app"></a>Windows işlev uygulaması

Aşağıdaki kod parçacığında, bir Windows işlevi uygulaması dağıtmak için kullanabilirsiniz:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

#### <a name="linux-function-app"></a>Linux işlev uygulaması

Aşağıdaki kod parçacığında, bir Linux işlev uygulamasına dağıtmak için kullanabilirsiniz:

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #Note that deployment slots is not supported for Linux Dynamic SKU
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

## <a name="template-based-pipeline"></a>Şablona dayalı bir işlem hattı

Azure DevOps şablonlarında derleme veya uygulama dağıtım görevleri önceden tanımlanmış gruplarıdır.

### <a name="build-your-app"></a>Uygulamanızı oluşturun

Azure işlem hatları uygulamanızda nasıl oluşturacağınız uygulamanızın programlama diline bağlıdır. Her dilin dağıtım yapıt oluşturma özel derleme adımları vardır. Bir dağıtım yapı, işlev uygulamanızı azure'da güncelleştirmek için kullanılır.

Yeni bir derleme işlem hattı oluşturduğunuzda yerleşik derleme şablonları kullanmak için **Klasik Düzenleyicisi'ni** Tasarımcı şablonlarını kullanarak bir işlem hattı oluşturursunuz.

![Azure işlem hatları Klasik Düzenleyicisi'ni seçin](media/functions-how-to-azure-devops/classic-editor.png)

Kaynak kodunuzun yapılandırdıktan sonra arama deneyimi için Azure işlevleri şablonları oluşturun. Uygulama dilinizi eşleşen şablonunu seçin.

![Bir Azure işlevleri derleme şablonu seçin](media/functions-how-to-azure-devops/build-templates.png)

Bazı durumlarda, derleme yapıtları bir belirli klasör yapısına sahiptir. Seçmeniz gerekebilir **yolları arşivlemek için kök klasör adı Prepend** onay kutusu.

![Kök klasör adının önüne ekleyin seçeneğine](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript uygulamaları

JavaScript uygulamanızı Windows yerel modülleri bir bağımlılık varsa, aracı havuzu sürüme güncelleştirmelisiniz **Hosted VS2017**.

![Aracı havuzu sürümü güncelleştir](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

Yeni bir yayın ardışık düzeni oluşturduğunuzda, Azure işlevleri sürüm şablonu için arama yapın.

![Azure işlevleri şablonu sürüm arayın](media/functions-how-to-azure-devops/release-template.png)

Bir dağıtım yuvasına dağıtma, sürüm şablonunda desteklenmiyor.

## <a name="create-a-build-pipeline-by-using-the-azure-cli"></a>Azure CLI kullanarak bir derleme işlem hattı oluşturma

Azure'da bir derleme işlem hattı oluşturmak için kullanın `az functionapp devops-pipeline create` [komut](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create). Derleme işlem hattı, derleme ve yayın deponuzda yapılan herhangi bir kod değişikliği için oluşturulur. Komutun, derleme ve yayın işlem hattı tanımlar ve deponuza işlemeleri yeni bir YAML dosyası oluşturur. Bu komut için önkoşulları kodunuzu konumuna göre değişir.

- Kodunuzu GitHub ise:

    - Olmalıdır **yazma** aboneliğiniz için izinleri.

    - Azure DevOps projesi Yöneticisi olması gerekir.

    - Yeterli izinlere sahip bir GitHub kişisel erişim belirteci (PAT) oluşturmak için izinleri olmalıdır. Daha fazla bilgi için [GitHub PAT izin gereksinimleri.](https://aka.ms/azure-devops-source-repos)

    - GitHub deponuzda ana dala yönelik otomatik olarak oluşturulan YAML dosyası tamamlayabilir şekilde kaydetmek için izinleri olmalıdır.

- Kodunuzu Azure depolarda ise:

    - Olmalıdır **yazma** aboneliğiniz için izinleri.

    - Azure DevOps projesi Yöneticisi olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [Azure işlevlerine genel bakış](functions-overview.md).
- Gözden geçirme [Azure DevOps genel bakış](/azure/devops/pipelines/).
