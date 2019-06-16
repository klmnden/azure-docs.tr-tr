---
title: HPC çözümleri - Azure Batch oluşturma ve dağıtma için Azure işlem hatlarını kullanın | Microsoft Docs
description: Bir derleme/yayın işlem hattı için Azure Batch üzerinde çalışan bir HPC uygulaması dağıtmayı öğrenin.
author: christianreddington
ms.author: chredd
ms.date: 03/28/2019
ms.topic: conceptual
ms.custom: fasttrack-new
services: batch
ms.openlocfilehash: a811a9cb1b124aff7c64d25cf71a1b84bff0c173
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65541737"
---
# <a name="use-azure-pipelines-to-build-and-deploy-hpc-solutions"></a>HPC çözümleri oluşturma ve dağıtma için Azure işlem hatlarını kullanın

Azure DevOps Hizmetleri, çeşitli geliştirme ekipleri tarafından özel bir uygulama oluştururken kullanılan araçlar sağlar. Azure DevOps tarafından sağlanan araçları otomatik oluşturmak ve yüksek performanslı bilgi işlem çözümlerini test etmek içine çevirebilir. Bu makalede, bir sürekli tümleştirme (CI) ve sürekli dağıtım (CD) kullanarak Azure Batch'te dağıtılan çözümü Azure işlem hatları için yüksek performanslı bilgi işlem nasıl ayarlanacağı gösterilmektedir.

Azure işlem hatları oluşturma, dağıtma, test etme ve izleme yazılım için bir dizi modern CI/CD işlemleri sağlar. Bu işlemler, böylece kodunuza odaklanın yerine altyapı ve işlemleri desteklemek, yazılım teslimini hızlandırın.

## <a name="create-an-azure-pipeline"></a>Bir Azure işlem hattı oluşturma

Bu örnekte biz bir yapı oluşturmak ve bir Azure Batch altyapıyı ve bir uygulama paketi yayın işlem hattı bırakın. Kodu yerel olarak geliştirilmiş varsayılarak, genel dağıtım akışı şudur:

![Bizim işlem hattı, dağıtım akışını gösteren diyagram](media/batch-ci-cd/DeploymentFlow.png)

### <a name="setup"></a>Kurulum

Bu makaledeki adımları izlemek için Azure DevOps kuruluş ve takım projesi gerekir.

* [Azure DevOps kuruluş oluştur](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops)
* [Azure DevOps proje oluşturma](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

### <a name="source-control-for-your-environment"></a>Ortamınız için kaynak denetimi

Ekiplerin kod tabanıyla yapılan değişiklikleri izlemek ve önceki kod sürümlerini incelemek için kaynak denetimi sağlar.

Genellikle, kaynak denetimi el yakından yazılım kodla zorlayıcı. Temel alınan altyapı ne dersiniz? Bu altyapı için burada Azure Resource Manager şablonları ya da diğer açık kaynak seçenekleri temel altyapımızı bildirimli olarak tanımlamanızı kullanacağız kod olarak olduk.

Bu örnek bir Resource Manager şablonları (JSON belgeleri için) ve var olan ikili sayısına yoğun olarak kullanır. Bu örneklerde, depoya kopyalayın ve bunları Azure DevOps gönderin.

Bu örnekte kullanılan codebase yapısı; şuna benzer

* Bir **arm şablonları** çeşitli Azure Resource Manager şablonları içeren klasör. Şablonlar, bu makalede açıklanmıştır.
* A **istemci uygulaması** kopyasını klasörüne, [Azure Batch .NET dosyasını işleme ffmpeg ile](https://github.com/Azure-Samples/batch-dotnet-ffmpeg-tutorial) örnek. Bu makalede için gerekli değildir.
* Bir **hpc uygulaması** Windows 64-bit klasörüne sürümünü [ffmpeg 3.4](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip).
* A **işlem hatları** klasör. Bu derleme sürecimiz anahat bir YAML dosyası içerir. Bu makalede ele alınmıştır.

Bu bölümde, sürüm denetimi ve tasarım Resource Manager şablonları ile ilgili bilgi sahibi olduğunuz varsayılır. Bu kavramlarına alışık değilseniz, daha fazla bilgi için aşağıdaki sayfalara bakın.

* [Kaynak Denetimi nedir?](https://docs.microsoft.com/azure/devops/user-guide/source-control?view=azure-devops)
* [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](../azure-resource-manager/resource-group-authoring-templates.md)

#### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Bu örnek, birden çok Resource Manager şablonları çözümüzü yararlanır. Bunu yapmak için belirli bir işlevsellik parçası uygulama özelliği şablonu (birimleri veya modülleri için benzer) sayısını kullanırız. Bu özellikler birlikte temel alınan getirmek için sorumlu bir uçtan uca çözüm şablonu de kullanırız. Bu yaklaşımın avantajları vardır:

* Temel alınan özelliği şablonları, tek tek birim test olabilir.
* Temel alınan özelliği şablonlar bir kuruluş içinde standart olarak tanımlanabilir ve birden çok çözüm yeniden kullanılabilir.

Bu örnekte, üç şablonları dağıtan bir uçtan uca çözüm şablonu (deployment.json) yoktur. Temel alınan özelliği şablonları, belirli bir yönüne çözümün dağıtmaktan sorumlu şablonlardır.

![Azure Resource Manager şablonlarını kullanarak bağlı şablon yapısı örneği](media/batch-ci-cd/ARMTemplateHierarchy.png)

Atacağız ilk şablon için bir Azure depolama hesabıdır. Çözümümüz bizim Batch hesabına uygulama dağıtmak için bir depolama hesabı gerektirir. Farkında olan değer olduğu [Microsoft.Storage kaynak türlerine için Resource Manager şablon başvurusu Kılavuzu](https://docs.microsoft.com/azure/templates/microsoft.storage/allversions) depolama hesapları için Resource Manager şablonları oluştururken.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('accountName')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "apiVersion": "2018-02-01",
            "location": "[resourceGroup().location]",
            "properties": {}
        }
    ],
    "outputs": {
        "blobEndpoint": {
          "type": "string",
          "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))).primaryEndpoints.blob]"
        },
        "resourceId": {
          "type": "string",
          "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('accountName'))]"
        }
    }
}
```

Ardından, size Azure Batch hesabı şablon görünecektir. Azure Batch hesabı (gruplandırmaları makine) havuzu genelinde çeşitli uygulamaları çalıştırmak için bir platform olarak davranır. Farkında olan değer olduğu [Microsoft.Batch kaynak türleri için Resource Manager şablon başvurusu Kılavuzu](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) Batch hesapları için Resource Manager şablonları oluştururken.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
            }
        },
        "storageAccountId": {
           "type": "string",
           "metadata": {
                "description": "ID of the Azure Storage Account"
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "name": "[parameters('batchAccountName')]",
            "type": "Microsoft.Batch/batchAccounts",
            "apiVersion": "2017-09-01",
            "location": "[resourceGroup().location]",
            "properties": {
              "poolAllocationMode": "BatchService",
              "autoStorage": {
                  "storageAccountId": "[parameters('storageAccountId')]"
              }
            }
          }
    ],
    "outputs": {}
}
```

Bir sonraki şablonu (makineler uygulamalarımızın işlemek için arka uç) bir Azure Batch havuzu oluşturma örneği gösterilmektedir. Farkında olan değer olduğu [Microsoft.Batch kaynak türleri için Resource Manager şablon başvurusu Kılavuzu](https://docs.microsoft.com/azure/templates/microsoft.batch/allversions) Resource Manager şablonları Batch hesabı havuzlar için oluşturma sırasında.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "batchAccountName": {
           "type": "string",
           "metadata": {
                "description": "Name of the Azure Batch Account"
           }
        },
        "batchAccountPoolName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account Pool"
             }
         }
    },
    "variables": {},
    "resources": [
        {
            "name": "[concat(parameters('batchAccountName'),'/', parameters('batchAccountPoolName'))]",
            "type": "Microsoft.Batch/batchAccounts/pools",
            "apiVersion": "2017-09-01",
            "properties": {
                "deploymentConfiguration": {
                    "virtualMachineConfiguration": {
                        "imageReference": {
                            "publisher": "Canonical",
                            "offer": "UbuntuServer",
                            "sku": "18.04-LTS",
                            "version": "latest"
                        },
                        "nodeAgentSkuId": "batch.node.ubuntu 18.04"
                    }
                },
                "vmSize": "Standard_D1_v2"
            }
          }
    ],
    "outputs": {}
}
```

Son olarak davranan bir orchestrator için benzer bir şablon sahibiz. Bu yetenek şablonları dağıtmaktan sorumlu şablonudur.

Ayrıca daha fazla bilgi hakkında bulabilirsiniz [bağlı Azure Resource Manager şablonları oluşturmaya](../azure-resource-manager/resource-manager-tutorial-create-linked-templates.md) ayrı bir makaledeki.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "templateContainerUri": {
           "type": "string",
           "metadata": {
                "description": "URI of the Blob Storage Container containing the Azure Resouce Manager templates"
            }
        },
        "templateContainerSasToken": {
           "type": "string",
           "metadata": {
                "description": "The SAS token of the container containing the Azure Resouce Manager templates"
            }
        },
        "applicationStorageAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Storage Account"
            }
         },
        "batchAccountName": {
            "type": "string",
            "metadata": {
                 "description": "Name of the Azure Batch Account"
            }
         },
         "batchAccountPoolName": {
             "type": "string",
             "metadata": {
                  "description": "Name of the Azure Batch Account Pool"
              }
          }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "storageAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/storageAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "accountName": {"value": "[parameters('applicationStorageAccountName')]"}
                }
            }
        },  
        {
            "apiVersion": "2017-05-10",
            "name": "batchAccountDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "storageAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccount.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "storageAccountId": {"value": "[reference('storageAccountDeployment').outputs.resourceId.value]"}
                }
            }
        },
        {
            "apiVersion": "2017-05-10",
            "name": "poolDeployment",
            "type": "Microsoft.Resources/deployments",
            "dependsOn": [
                "batchAccountDeployment"
            ],
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                  "uri": "[concat(parameters('templateContainerUri'), '/batchAccountPool.json', parameters('templateContainerSasToken'))]",
                  "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "batchAccountName": {"value": "[parameters('batchAccountName')]"},
                    "batchAccountPoolName": {"value": "[parameters('batchAccountPoolName')]"}
                }
            }
        }
    ],
    "outputs": {}
}
```

#### <a name="the-hpc-solution"></a>HPC çözümü

Altyapı ve yazılım kod olarak tanımlanabilir ve aynı depoda birlikte.

Bu çözüm için ffmpeg uygulama paketi olarak kullanılır. Ffmpeg paketinin indirilebilir [burada](https://ffmpeg.zeranoe.com/builds/win64/static/ffmpeg-3.4-win64-static.zip).

![Örnek Git deposu yapısı](media/batch-ci-cd/git-repository.jpg)

Bu depo için dört ana bölümü vardır:

* **Arm şablonları** Altyapımızı kod olarak depolayan klasör
* **Hpc uygulaması** ffmpeg ikili dosyaları içeren klasör
* **İşlem hatları** bizim derleme işlem hattı tanımını içeren klasör.
* **İsteğe bağlı**: **İstemci uygulaması** .NET uygulaması için kodu depolamak klasörü. Biz bu örnekte kullanmayın, ancak kendi projesinde bir istemci uygulaması aracılığıyla HPC Batch uygulama çalıştırmalarını yürütmek isteyebilir.

> [!NOTE]
> Bir yapının bir örnek için bir kod temeli budur. Bu yaklaşım, uygulama, altyapı ve işlem hattı kodu, aynı depoda depolanan tanıtmak amacıyla kullanılır.

Kaynak kodu ayarlamak, biz ilk derleme başlayabilirsiniz.

## <a name="continuous-integration"></a>Sürekli tümleştirme

[Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/?view=azure-devops), içinde Azure DevOps hizmetleriyle, uygulamalarınızın derleme, test ve dağıtım işlem hattı uygulamanıza yardımcı olur.

Bu aşamada, işlem hattının testleri kodu doğrulamak ve uygun yazılımlar oluşturmak için genellikle çalıştırın. Sayısını ve türlerini testler çalıştırdığınızda ek görevleri ve daha geniş yapınızı bağlıdır ve Sürüm stratejisi.

## <a name="preparing-the-hpc-application"></a>HPC uygulama hazırlama

Bu örnekte, odaklanacağız **hpc uygulaması** klasör. **Hpc uygulaması** Azure Batch hesap içinde çalışacak ffmpeg yazılım klasördür.

1. Azure DevOps kuruluşunuzdaki Azure işlem hatları derlemeler bölümüne gidin. Oluşturma bir **yeni işlem hattı**.

    ![Yeni bir derleme işlem hattı oluşturma](media/batch-ci-cd/new-build-pipeline.jpg)

1. Bir derleme işlem hattı oluşturmak için iki seçeneğiniz vardır:

    a. [Görsel Tasarımcı kullanılarak](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=azure-devops&tabs=new-nav). Bunu kullanmak için "görsel tasarımcıyı kullanmak" tıklayın **yeni işlem hattı** sayfası.

    b. [YAML derlemeleri](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=azure-devops). Azure depoları veya yeni işlem hattı sayfanın GitHub seçeneğine tıklayarak yeni bir YAML işlem hattı oluşturabilirsiniz. Alternatif olarak, aşağıdaki örnekte, kaynak denetiminde depolayabilir ve var olan bir YAML dosyası üzerinde görsel tasarımcı ve ardından YAML şablonu kullanarak başvuru.

    ```yml
    # To publish an application into Azure Batch, we need to
    # first zip the file, and then publish an artifact, so that
    # we can take the necessary steps in our release pipeline.
    steps:
    # First, we Zip up the files required in the Batch Account
    # For this instance, those are the ffmpeg files
    - task: ArchiveFiles@2
      displayName: 'Archive applications'
      inputs:
        rootFolderOrFile: hpc-application
        includeRootFolder: false
        archiveFile: '$(Build.ArtifactStagingDirectory)/package/$(Build.BuildId).zip'
    # Publish that zip file, so that we can use it as part
    # of our Release Pipeline later
    - task: PublishPipelineArtifact@0
      inputs:
        artifactName: 'hpc-application'
        targetPath: '$(Build.ArtifactStagingDirectory)/package'
    ```

1. Derleme gerektiği şekilde yapılandırıldıktan sonra seçin **Kaydet ve kuyruğa**. Sürekli Tümleştirme etkin varsa (içinde **Tetikleyicileri** bölümünde), yapı depoya yeni bir işleme yapıldığında otomatik olarak tetikleyecek, koşullara uyan yapı ayarlayın.

    ![Mevcut bir derleme işlem hattı örneği](media/batch-ci-cd/existing-build-pipeline.jpg)

1. Görünüm Canlı güncelleştirmeleri Azure DevOps yapınızda ilerlemesini giderek **derleme** Azure işlem hatları bölümü. Derleme tanımınızdan uygun derlemeyi seçin.

    ![Derleme Canlı çıkışları görüntüle](media/batch-ci-cd/Build-1.jpg)

> [!NOTE]
> HPC Batch uygulamanızı yürütmek için bir istemci uygulaması kullanıyorsanız, bu uygulama için ayrı bir yapı tanımı oluşturmak gerekir. Nasıl yapılır kılavuzlarında sayısı bulabilirsiniz [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/index?view=azure-devops) belgeleri.

## <a name="continuous-deployment"></a>Sürekli dağıtım

Azure işlem hatları, temel alınan altyapı ve uygulama dağıtmak için de kullanılır. [Yayın işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/release) sürekli dağıtımı sağlar ve sürüm işlemini otomatikleştiren bileşendir.

### <a name="deploying-your-application-and-underlying-infrastructure"></a>Uygulamanızı dağıtmak ve temel altyapıyı

Altyapı dağıtımı olarak kullanılan birkaç adım vardır. Biz kullanılan [bağlı şablonları](../azure-resource-manager/resource-group-linked-templates.md), bu şablonlar bir genel uç noktasından (HTTP veya HTTPS) erişilebilir olması gerekir. Bu, GitHub, depo veya bir Azure Blob Depolama hesabı veya başka bir depolama konumu olabilir. Karşıya yüklenen şablonu yapıları gizli modda tutulan gibi güvenli kalır ancak paylaşılan erişim imzası (SAS) belirteç biçimi kullanılarak erişilir. Aşağıdaki örnek, Azure depolama blobundan şablonlarla bir altyapı dağıtmayı gösterir.

1. Oluşturma bir **yeni yayın tanımı**, boş bir tanımı seçin. Ardından, işlem hattı için ilgili bir sorun için yeni oluşturulan ortamı yeniden adlandırmak ihtiyacımız var.

    ![İlk yayın ardışık düzeni](media/batch-ci-cd/Release-0.jpg)

1. Bir bağımlılık oluşturmak HPC uygulamamız için işlemin çıktısını almak için işlem hattı oluşturun.

    > [!NOTE]
    > Bir kez daha, Not **kaynak diğer adı**gibi yayın tanımı içinde görevleri oluştururken bu gerekecektir.

    ![HPCApplicationPackage bir yapıt bağlantı uygun derleme işlem hattı oluşturma](media/batch-ci-cd/Release-1.jpg)

1. Bu kez, bir Azure deponun başka bir yapıt için bir bağlantı oluşturun. Bu depoda depolanan Resource Manager şablonları erişmek için gereklidir. Resource Manager şablonları, derleme gerektirmeyen gibi bunları bir derleme işlem hattı anında iletme gerekmez.

    > [!NOTE]
    > Bir kez daha, Not **kaynak diğer adı**gibi yayın tanımı içinde görevleri oluştururken bu gerekecektir.

    ![Bir Azure depoları yapıt bağlantı oluşturma](media/batch-ci-cd/Release-2.jpg)

1. Gidin **değişkenleri** bölümü. Bir değişken sayısı aynı bilgilerin birden çok görevlere giriş yapma değil, işlem hattınızda oluşturarak önerilir. Bu örnekte ve bunların dağıtım nasıl etkilediği kullanılan değişkenleri şunlardır.

    * **applicationStorageAccountName**: HPC uygulama ikili dosyalarını tutmak için depolama hesabının adı
    * **batchAccountApplicationName**: Azure Batch hesabında uygulamanın adı
    * **batchAccountName**: Azure Batch hesabının adı
    * **batchAccountPoolName**: İşleme yapılıyor VM havuzu adı
    * **batchApplicationId**: Azure Batch uygulama için benzersiz kimlik
    * **batchApplicationVersion**: Toplu işlem uygulamanızı (diğer bir deyişle, ffmpeg ikili değerleri) anlam sürümü
    * **Konum**: Dağıtılacak Azure kaynakları için konum
    * **ResourceGroupName**: Oluşturulacak kaynak grubu adı ve kaynaklarınızı dağıtılacağı
    * **StorageAccountName**: Resource Manager şablonları bağlı tutmak için depolama hesabının adı

    ![Azure işlem hatlarını sürümde ayarlanan değişkenler örneği](media/batch-ci-cd/Release-4.jpg)

1. Geliştirme ortamı için görevleri gidin. İçinde altı görevden anlık görüntü görebilirsiniz. Bu görevleri olacaktır: Sıkıştırılmış ffmpeg dosyaları indirmek, iç içe geçmiş Resource Manager şablonları barındırmak, bu Resource Manager şablonları depolama hesabına kopyalayın, gerekli bağımlılıkları ve batch hesabı dağıtma, uygulamayı oluşturmak için bir depolama hesabı dağıtın Azure Batch hesabı ve Azure Batch hesabına uygulama paketini karşıya yükleme.

    ![Azure Batch HPC uygulamaya serbest bırakmak için kullanılan görevler örneği](media/batch-ci-cd/Release-3.jpg)

1. Ekleme **işlem hattı Yapıtı indir (Önizleme)** görev ve aşağıdaki özellikleri ayarlayın:
    * **Görünen ad:** Aracıya ApplicationPackage indirin
    * **İndirmek için yapıt adı:** hpc uygulaması
    * **Yüklenmeye yol**: $(System.DefaultWorkingDirectory)

1. Yapıtları depolamak için bir depolama hesabı oluşturun. Mevcut bir depolama hesabını çözümünden kullanılabilir, ancak kendi içinde örnek ve içerik yalıtımı için ayrılmış bir depolama hesabı bizim yapıtları (Resource Manager şablonları özellikle) yapıyoruz.

    Ekleme **Azure kaynak grubu dağıtımı** görev ve aşağıdaki özellikleri ayarlayın:
    * **Görünen ad:** Depolama hesabı için Resource Manager şablonları dağıtma
    * **Azure aboneliği:** Uygun Azure aboneliğini seçin
    * **Eylem**: Kaynak grubu oluşturma veya güncelleştirme
    * **Kaynak grubu**: $(resourceGroupName)
    * **Konum**: $(location)
    * **Şablon**: $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates/storageAccount.json
    * **Şablon parametrelerini geçersiz kıl**: - accountName $(storageAccountName)

1. Kaynak denetiminden yapıtları depolama hesabına yükleyin. Bu işlemi gerçekleştirmeye yönelik bir Azure işlem hattı görev yoktur. Bu görev bir parçası olarak, depolama hesabı kapsayıcı URL'sini ve SAS belirteci Azure işlem hatları değişkene yüzdelik. Başka bir deyişle, bu aracı aşaması yeniden kullanılabilir.

    Ekleme **Azure dosya kopyalama** görev ve aşağıdaki özellikleri ayarlayın:
    * **Kaynak:** $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates /
    * **Azure bağlantı türü**: Azure Resource Manager
    * **Azure aboneliği:** Uygun Azure aboneliğini seçin
    * **Hedef türü**: Azure Blob
    * **RM depolama hesabı**: $(storageAccountName)
    * **Kapsayıcı adı**: şablonları
    * **Depolama kapsayıcı URİ'si**: templateContainerUri
    * **Depolama kapsayıcısı SAS belirteci**: templateContainerSasToken

1. Orchestrator şablonu dağıtın. Orchestrator şablondan daha önce geri çağırma, SAS belirteci ek olarak bir depolama hesabı kapsayıcı URL'sini parametrelerini olduğunu fark edeceksiniz. Resource Manager şablonunda gereken değişkenleri ya da yayın tanımının değişkenler bölümünde tutulur veya başka bir Azure işlem hatları görevden (örneğin, Azure Blob kopyalama görevinin bir parçası) ayarlanan göreceksiniz.

    Ekleme **Azure kaynak grubu dağıtımı** görev ve aşağıdaki özellikleri ayarlayın:
    * **Görünen ad:** Azure Batch dağıtma
    * **Azure aboneliği:** Uygun Azure aboneliğini seçin
    * **Eylem**: Kaynak grubu oluşturma veya güncelleştirme
    * **Kaynak grubu**: $(resourceGroupName)
    * **Konum**: $(location)
    * **Şablon**: $(System.ArtifactsDirectory)/ **{YourAzureRepoArtifactSourceAlias}** /arm-templates/deployment.json
    * **Şablon parametrelerini geçersiz kıl**: ```-templateContainerUri $(templateContainerUri) -templateContainerSasToken $(templateContainerSasToken) -batchAccountName $(batchAccountName) -batchAccountPoolName $(batchAccountPoolName) -applicationStorageAccountName $(applicationStorageAccountName)```

Azure Key Vault görevlerini kullanmak yaygın bir uygulamadır. Hizmet sorumlusu (Azure aboneliğinize bağlantı) belirlenen bir uygun erişim ilkeleri varsa, bir Azure Key Vault'tan gizli dizileri indirebilirsiniz ve işlem hattınızdaki değişkenleri olarak kullanılır. Gizli dizi adı ile ilişkili değer ayarlanır. Örneğin, bir gizli dizi sshPassword, yayın tanımındaki $(sshPassword) ile başvurulan.

1. Sonraki adımlar, Azure CLI'yı çağırın. İlk Azure Batch'te bir uygulama oluşturmak için kullanılır. ve ilişkili paketleri yükleyin.

    Ekleme **Azure CLI** görev ve aşağıdaki özellikleri ayarlayın:
    * **Görünen ad:** Uygulama Azure Batch hesabı oluşturma
    * **Azure aboneliği:** Uygun Azure aboneliğini seçin
    * **Betik konumu**: Satır içi betik
    * **Satır içi betik**: ```az batch application create --application-id $(batchApplicationId) --name $(batchAccountName) --resource-group $(resourceGroupName)```

1. İkinci adım, ilişkili paketleri uygulamayı karşıya yüklemek için kullanılır. Bu örnekte, ffmpeg dosyaları.

    Ekleme **Azure CLI** görev ve aşağıdaki özellikleri ayarlayın:
    * **Görünen ad:** Azure Batch hesabına paketini karşıya yükleyin
    * **Azure aboneliği:** Uygun Azure aboneliğini seçin
    * **Betik konumu**: Satır içi betik
    * **Satır içi betik**: ```az batch application package create --application-id $(batchApplicationId)  --name $(batchAccountName)  --resource-group $(resourceGroupName) --version $(batchApplicationVersion) --package-file=$(System.DefaultWorkingDirectory)/$(Release.Artifacts.{YourBuildArtifactSourceAlias}.BuildId).zip```

    > [!NOTE]
    > Uygulama paketinin sürüm numarasını bir değişkene ayarlanır. Bu, önceki paket sürümleri üzerine yazılması sizin için çalışır ve el ile Azure Batch'e gönderildi paketin sürüm numarasını denetlemek istiyorsanız kullanışlıdır.

1. Yeni bir sürüm seçerek oluşturma **sürüm > Yeni yayın oluştur**. Tetiklenen sonra durumu görüntülemek için yeni yayın bağlantısını seçin.

1. Aracısı'ndan Canlı çıkış seçerek görüntüleyebilirsiniz **günlükleri** ortamınızı altında düğmesi.

    ![Yayınlama durumunu görüntüleme](media/batch-ci-cd/Release-5.jpg)

### <a name="testing-the-environment"></a>Sınama ortamı

Ortam ayarladıktan sonra aşağıdaki testler başarıyla tamamlanabilmesi için onaylayın.

Yeni Azure Batch PowerShell komut isteminden Azure CLI kullanarak hesap bağlanın.

* Azure hesabınızda oturum açın `az login` ve kimlik doğrulaması yapmak için yönergeleri izleyin.
* Batch hesabı Şimdi Doğrula: `az batch account login -g <resourceGroup> -n <batchAccount>`

#### <a name="list-the-available-applications"></a>Kullanılabilir uygulamaların listesi

```azurecli
az batch application list -g <resourcegroup> -n <batchaccountname>
```

#### <a name="check-the-pool-is-valid"></a>Havuz geçerli olup olmadığını denetleyin

```azurecli
az batch pool list
```

Değerini not edin `currentDedicatedNodes` bu komutun çıktısından. Bu değer sonraki teste ayarlanır.

#### <a name="resize-the-pool"></a>Havuz yeniden boyutlandırma

İşlem düğümleri, iş ve görev test için kullanılabilen, yeniden boyutlandırma tamamlandıktan ve kullanılabilir düğümler kadar geçerli durumu görmek için havuz Listele komutu denetleyin olduğundan havuz yeniden boyutlandırma

```azurecli
az batch pool resize --pool-id <poolname> --target-dedicated-nodes 4
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede yanı sıra ffmpeg, .NET ve Python kullanarak kullanan iki öğreticiler vardır. Basit bir uygulama üzerinden bir Batch hesabı ile etkileşimde hakkında daha fazla bilgi için bu öğreticileri bakın.

* [Python API'sini kullanarak Azure Batch ile paralel iş yükü çalıştırma](tutorial-parallel-python.md)
* [.NET API kullanarak Azure Batch ile paralel iş yükü çalıştırma](tutorial-parallel-dotnet.md)
