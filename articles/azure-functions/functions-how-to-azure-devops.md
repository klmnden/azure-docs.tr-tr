---
title: Azure DevOps kullanarak işlev kod güncelleştirmelerini sürekli teslim
description: Azure işlevleri hedefleyen bir Azure DevOps işlem hattı ayarlayın öğrenin.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.custom: ''
ms.openlocfilehash: 9806a982982971b1b3ac9c28454e17813b2ad2a5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67479876"
---
# <a name="continuous-delivery-using-azure-devops"></a>Azure DevOps kullanarak sürekli teslim

İşlevinizi kullanarak bir Azure işlevi uygulaması otomatik olarak dağıtabilirsiniz [Azure işlem hatları](/azure/devops/pipelines/).
İşlem hattınızı tanımlamak için kullanabilirsiniz:

- YAML dosyası: Bu dosya, potansiyel açıklar, bir derleme adımları bölümü ve bir sürüm bölümü olabilir. Uygulama olarak aynı depoda YAML dosyası olmalıdır.

- Şablonları: Şablonları oluşturduğunuzda veya dağıttığınızda uygulamanızı görevleri yapılan hazır olursunuz.

## <a name="yaml-based-pipeline"></a>YAML tabanlı işlem hattı

### <a name="build-your-app"></a>Uygulamanızı oluşturun

Uygulamanızı Azure işlem hatları oluşturmaya uygulamanızın programlama diline bağlıdır.
Her dilin işlevi uygulamanızı Azure'a dağıtmak için kullanılan bir dağıtım yapıt oluşturmak için özel yapı adımları vardır.

#### <a name="net"></a>.NET

Aşağıdaki örnek, .NET uygulamanızı oluşturmak için YAML dosyası oluşturmak için kullanabilirsiniz.

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

Aşağıdaki örnek, JavaScript uygulamanızı oluşturmak için YAML dosyası oluşturmak için kullanabilirsiniz:

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

Aşağıdaki örnek Python uygulamanızı oluşturmak için YAML dosyası oluşturmak için kullanabileceğiniz, Python, yalnızca Linux Azure işlevleri için desteklenir:

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

Aşağıdaki örnek PowerShell uygulamanızı paketlemek için YAML dosyanızı oluşturmak için kullanabileceğiniz, PowerShell, yalnızca Windows Azure işlevleri için desteklenir:

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

Barındıran işletim sistemi bağlı olarak aşağıdaki YAML örnek YAML dosyanıza eklemeniz gerekir.

#### <a name="windows-function-app"></a>Uygulama Windows işlevi

Aşağıdaki kod parçacığında, bir Windows işlev uygulamasına dağıtmak için kullanılabilir

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

Aşağıdaki kod parçacığında, bir Linux işlev uygulamasına dağıtmak için kullanılabilir

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

Azure DevOps şablonlarında tanımlanmış oluşturduğunuzda veya bir uygulamayı dağıttığınızda görev grubu var.

### <a name="build-your-app"></a>Uygulamanızı oluşturun

Uygulamanızı Azure işlem hatları oluşturmaya uygulamanızın programlama diline bağlıdır. Her dilin azure'daki işlev uygulamanızın güncelleştirmek için kullanılan bir dağıtım yapı oluşturmak için özel yapı adımları vardır.
Yerleşik derleme şablonları yeni bir derleme işlem hattı oluşturma sırasında kullanmak üzere, **Klasik Düzenleyicisi'ni** Tasarımcı şablonları kullanarak bir işlem hattı oluşturmak için

![Azure işlem hatları Klasik Düzenleyici](media/functions-how-to-azure-devops/classic-editor.png)

Kaynak kodunuzun yapılandırmak sonra Azure işlevleri için derleme şablonları arayın ve uygulama dilinizi eşleşen şablonunu seçin.

![Azure işlevleri şablonları oluşturma](media/functions-how-to-azure-devops/build-templates.png)

Bazı durumlarda, belirli bir klasör yapısı derleme yapıtları sahip ve denetlemeniz gerekebilir **yolları arşivlemek için kök klasör adı Prepend** seçeneği.

![Kök klasör önüne ekleyin](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>JavaScript uygulamaları

JavaScript uygulamanızı Windows yerel modülleri bir bağımlılık varsa, güncelleştirmeniz gerekir:

- Aracı havuzu sürümüne **Hosted VS2017**

  ![İşletim sistemi derleme Aracısını Değiştir](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

Yeni bir yayın ardışık düzeni oluştururken, Azure işlevleri sürüm şablonu için arama yapın.

![](media/functions-how-to-azure-devops/release-template.png)

Bir dağıtım yuvasına dağıtma, sürüm şablonunda desteklenmiyor.

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Azure CLI kullanarak bir Azure işlem hattı oluşturma

Kullanarak `az functionapp devops-pipeline create` [komut](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create), derleme ve yayın deponuzda herhangi bir kod değişikliği için bir Azure işlem hattı oluşturulup. Bu komut, derleme ve yayın işlem hattı tanımlayan yeni bir YAML dosyası oluşturmak ve deponuza işleyin. Bir dağıtım yuvasına dağıtma Azure CLI komutu tarafından desteklenmiyor.
Bu komut için ön koşullar, kodun konumuna bağlıdır:

- Kodunuzu GitHub ise:

    - İhtiyacınız **yazma** aboneliğinizi izni.

    - Azure DevOps projesi Yöneticisi olursunuz.

    - Yeterli izinlere sahip bir GitHub kişisel erişim belirteci oluşturma izni var. [GitHub PAT izin gereksinimleri.](https://aka.ms/azure-devops-source-repos)

    - Otomatik olarak oluşturulan YAML dosyası kaydetmek için GitHub deposundaki ana dal için yürütme izni var.

- Kodunuzu Azure depolarda ise:

    - İhtiyacınız **yazma** aboneliğinizi izni.

    - Azure DevOps projesi Yöneticisi olursunuz.

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure işlevlerine genel bakış](functions-overview.md)
+ [Azure DevOps genel bakış](/azure/devops/pipelines/)
