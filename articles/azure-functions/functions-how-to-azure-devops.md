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
ms.openlocfilehash: 86f1c36bd5f972a6ebd573019a22b0c0d07dc480
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928100"
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
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>JavaScript

Aşağıdaki örnek, JavaScript uygulamanızı oluşturmak için YAML dosyası oluşturmak için kullanabilirsiniz:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Aşağıdaki örnek Python uygulamanızı oluşturmak için YAML dosyası oluşturmak için kullanabileceğiniz, Python, yalnızca Linux Azure işlevleri için desteklenir:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
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
```

## <a name="template-based-pipeline"></a>Şablona dayalı bir işlem hattı

Azure DevOps şablonlarında tanımlanmış oluşturduğunuzda veya bir uygulamayı dağıttığınızda görev grubu var.

### <a name="build-your-app"></a>Uygulamanızı oluşturun

Uygulamanızı Azure işlem hatları oluşturmaya uygulamanızın programlama diline bağlıdır. Her dilin azure'daki işlev uygulamanızın güncelleştirmek için kullanılan bir dağıtım yapı oluşturmak için özel yapı adımları vardır.
Yerleşik derleme şablonları yeni bir derleme işlem hattı oluşturma sırasında kullanmak üzere, **Klasik Düzenleyicisi'ni** Tasarımcı şablonları kullanarak bir işlem hattı oluşturmak için

![Azure işlem hatları Klasik Düzenleyici](media/functions-how-to-azure-devops/classic-editor.png)

Kaynak kodunuzun yapılandırmak sonra Azure işlevleri için derleme şablonları arayın ve uygulama dilinizi eşleşen şablonunu seçin.

![Azure işlevleri şablonları oluşturma](media/functions-how-to-azure-devops/build-templates.png)

#### <a name="javascript-apps"></a>JavaScript uygulamaları

JavaScript uygulamanızı Windows yerel modülleri bir bağımlılık varsa, güncelleştirmeniz gerekir:

- Aracı havuzu sürümüne **Hosted VS2017**

  ![İşletim sistemi derleme Aracısını Değiştir](media/functions-how-to-azure-devops/change-agent.png)

- Betikte **yapı uzantıları** şablona adımda `IF EXIST *.csproj dotnet build extensions.csproj --output ./bin`

  ![Değişiklik betiğini](media/functions-how-to-azure-devops/change-script.png)

### <a name="deploy-your-app"></a>Uygulamanızı dağıtma

Yeni bir yayın ardışık düzeni oluştururken, Azure işlevleri sürüm şablonu için arama yapın.

![](media/functions-how-to-azure-devops/release-template.png)

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Azure CLI kullanarak bir Azure işlem hattı oluşturma

Kullanarak `az functionapp devops-pipeline create` [komut](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create), derleme ve yayın deponuzda herhangi bir kod değişikliği için bir Azure işlem hattı oluşturulup. Bu komut, derleme ve yayın işlem hattı tanımlayan yeni bir YAML dosyası oluşturmak ve deponuza işleyin.
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
