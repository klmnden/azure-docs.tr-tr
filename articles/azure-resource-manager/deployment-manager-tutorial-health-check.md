---
title: Azure Deployment Manager’ı Resource Manager şablonlarıyla kullanma | Microsoft Docs
description: Azure kaynaklarını dağıtmak için Azure Deployment Manager ile Resource Manager şablonlarını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.date: 05/31/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0e8a9fefdf92f568001cc3352fe83a85157acf9a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442594"
---
# <a name="tutorial-use-health-check-in-azure-deployment-manager-public-preview"></a>Öğretici: Azure Dağıtım Yöneticisi'nde (genel Önizleme) sistem durumu denetimi kullanın

Sistem durumu denetiminde tümleştirmeyi öğrenin [Azure Deployment Manager](./deployment-manager-overview.md). Bu öğreticide, temel [Resource Manager şablonları ile kullanımı Azure Deployment Manager](./deployment-manager-tutorial.md) öğretici. Bu dosyayla devam etmeden önce o öğreticiyi tamamlamanız gerekir.

Kullanılan dağıtım şablonu olarak [Resource Manager şablonları ile kullanımı Azure Deployment Manager](./deployment-manager-tutorial.md), kullandığınız bir bekleme adımı. Bu öğreticide, bir sistem durumu denetimi adımıyla bekleme adım değiştirin.

> [!IMPORTANT]
> Kanarya yeni Azure özellikleri test etmek için aboneliğinizi işaretlenmişse Kanarya bölgeye dağıtma yalnızca Azure Dağıtım Yöneticisi'ni kullanabilirsiniz. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir sistem durumu denetimi hizmeti simülatör'ü oluşturma
> * Dağıtım şablonu gözden geçirme
> * Topolojiyi dağıtma
> * Piyasaya çıkma sistem durumunun iyi olmadığını ile dağıtma
> * Piyasaya çıkma dağıtımı doğrulama
> * Dağıtım durumu sağlıklı ile dağıtma
> * Piyasaya çıkma dağıtımı doğrulama
> * Kaynakları temizleme

Ek kaynaklar:

- [Azure Deployment Manager REST API'si başvurusunda](https://docs.microsoft.com/rest/api/deploymentmanager/).
- [Azure Deployment Manager örnek](https://github.com/Azure-Samples/adm-quickstart).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Tam [Resource Manager şablonları ile kullanımı Azure Deployment Manager](./deployment-manager-tutorial.md).
* İndirme [şablonlar ve yapıtlar](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip) Bu öğretici tarafından kullanılır.

## <a name="create-a-health-check-service-simulator"></a>Bir sistem durumu denetimi hizmeti simülatör'ü oluşturma

Üretim ortamında genellikle bir veya daha fazla izleme sağlayıcılarını kullanın. Sistem durumu tümleştirme mümkün olduğunca kolaylaştırmak için Microsoft bazı şirketler, sistem durumu denetimleri ile dağıtımlarınızı tümleştirmek için bir basit kopyala/yapıştır çözümü sağlamak için izleme üst hizmet durumu ile çalışmaktadır. Bu şirketlerin listesi için bkz. [sistem durumu izleme sağlayıcılarını](./deployment-manager-health-check.md#health-monitoring-providers). Bu öğreticinin amaçları doğrultusunda, oluşturduğunuz bir [Azure işlevi](/azure/azure-functions/) sistem durumu hizmeti izleme benzetimini yapmak için. Bu işlev, bir durum kodunu alır ve aynı kodu döndürür. Azure Deployment Manager şablonunuzu dağıtıma devam etmek nasıl belirlemek için durum kodunu kullanır.

Aşağıdaki iki dosyada Azure işlevi dağıtmak için kullanılır. Öğreticiyi incelemek için bu dosyaları indirmeniz gerekmez.

* Resource Manager şablonu konumundaki [ https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json ](https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json). Bir Azure işlevi oluşturmak için bu şablonu dağıtın.
* Bir Azure işlevi kaynak kodu zip dosyası [ https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip ](https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip). Olarak adlandırılan bu zip Resource Manager şablonu tarafından çağrılır.

Azure işlevini dağıtmak için seçebileceğiniz **deneyin** Azure Cloud Shell'i açın ve aşağıdaki betiği shell penceresine yapıştırın.  Kod yapıştırmak için shell penceresine sağ tıklayın ve ardından **yapıştırın**.

> [!IMPORTANT]
> **projectName** PowerShell, betik Bu öğreticide dağıtılan Azure hizmetlerinin adları oluşturmak için kullanılır. Farklı Azure hizmetlerini adları farklı gereksinimleri vardır. Dağıtımın başarılı olmasını sağlamak için yalnızca küçük harf ve sayı ile 12'den az karakter içeren bir ad seçin.
> Proje adı bir kopyasını kaydedin. Bu öğreticide aynı projectName kullanırsınız.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json" -projectName $projectName
```

Doğrulayın ve test Azure işlevi için:

1. [Azure portalı](https://portal.azure.com) açın.
1. Kaynak grubunu açın.  Varsayılan ad proje adıdır ile **rg** eklenir.
1. App service kaynak grubundan'ı seçin.  Proje adı ile app Service varsayılan addır **webapp** eklenir.
1. Genişletin **işlevleri**ve ardından **HttpTrigger1**.

    ![Azure Deployment Manager sistem durumu denetimi Azure işlevi](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-function.png)

1. Seçin  **&lt;/ > işlev URL'sini Al**.
1. Seçin **kopyalama** URL'yi panoya kopyalamak için.  URL'ye benzer:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/{healthStatus}?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Değiştirin `{healthStatus}` durum koduna sahip bir URL. Bu öğreticide, **sağlıksız** sağlıksız senaryoyu test etmek ve ya da **sağlıklı** veya **uyarı** sağlıklı senaryoyu test etmek için. Sistem durumunun iyi olmadığını ve diğer iki URL ile durumu sağlıklı oluşturun. Örnekler için:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/healthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Bu öğretici tamamlanmış için her iki URL'leri ihtiyacınız vardır.

1. Sistem durumu izleme simülatör test etmek için son adımda oluşturduğunuz URL'leri açın.  Sağlıksız durum sonuçları benzer olacaktır:

    ```
    Status: unhealthy
    ```

## <a name="revise-the-rollout-template"></a>Dağıtım şablonu gözden geçirme

Bu bölümün amacı, size bir sistem durumu denetimi adım piyasaya çıkma şablona dahil nasıl göstermektir. Bu öğreticiyi tamamlamak için kendi CreateADMRollout.json dosyası oluşturmanız gerekmez. Değiştirilen bir dağıtım şablonu, sonraki bölümlerde kullanılan bir depolama hesabında paylaşılır.

1. Açık **CreateADMRollout.json**. Bu JSON dosyasını indirme işleminin bir parçasıdır.  [Ön koşullara](#prerequisites) bakın.
1. İki parametre daha ekleyin:

    ```json
    "healthCheckUrl": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check URL."
        }
    },
    "healthCheckAuthAPIKey": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check Azure Function function authorization key."
        }
    }
    ```

1. Bekleme adım kaynak tanımı, bir sistem durumu denetimi adım kaynak tanımı ile değiştirin:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

    Tanımına dayalı olarak, dağıtım durumu geçerli olduğunda geçer *sağlıklı* veya *uyarı*.

1. Güncelleştirme **dependsON** yeni tanımlanan sistem durumu Denetim adımı eklemek için ürün tanımı:

    ```json
    "dependsOn": [
        "[resourceId('Microsoft.DeploymentManager/artifactSources', variables('rolloutArtifactSource').name)]",
        "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
    ],
    ```

1. Güncelleştirme **stepGroups** sistem durumu onay adımı dahil etmek için. **HealthCheckStep** çağrılma yeri **postDeploymentSteps** , **stepGroup2**. **stepGroup3** ve **stepGroup4** durumu sağlıklı geçerli olduğunda yalnızca dağıtılan *sağlıklı* veya *uyarı*.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

    Karşılaştırırsanız **stepGroup3** önce bölümünde ve düzeltilmiş sonra bu bölümde artık bağlıdır **stepGroup2**.  Bu gerekli olduğunda **stepGroup3** ve sonraki adımı grupları, sistem durumu izleme sonuçlarına bağlıdır.

    Değiştirilen alanları aşağıdaki ekran gösterilir ve sistem durumu Denetim adımı nasıl kullanılır:

    ![Azure Deployment Manager sistem durumu onay şablonu](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-rollout-template.png)

## <a name="deploy-the-topology"></a>Topolojiyi dağıtma

Kendi kopyanızı hazırlamanız gerekmez, öğreticiyi basitleştirmek için topoloji şablonu ve yapılar aşağıdaki konumlarda paylaşılır. Kullanmak kendi isterseniz yönergeleri [Öğreticisi: Deployment Manager'ı Azure Resource Manager şablonlarıyla kullanma](./deployment-manager-tutorial.md).

* Topoloji şablonu: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplates/CreateADMServiceTopology.json
* Yapıt deposu: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Topoloji dağıtmak için seçebileceğiniz **deneyin** Cloud Shell'i açın ve ardından PowerShell betiğini yapıştırın.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the service topology
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMServiceTopology.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation
```

Azure portalı kullanarak hizmet topolojisinin ve temel kaynakların başarıyla oluşturulduğunu doğrulayın:

![Azure Deployment Manager öğreticisi dağıtılan hizmet topolojisi kaynakları](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

Kaynakları görmek için **Gizli türleri göster** seçeneği belirlenmelidir.

## <a name="deploy-the-rollout-with-the-unhealthy-status"></a>Piyasaya çıkma sağlıksız durum ile dağıtma

Öğreticiyi basitleştirmek için değiştirilen bir dağıtım şablonu kendi kopyanızı hazırlamanız gerekmez, aşağıdaki konumlarda paylaşılır. Kullanmak kendi isterseniz yönergeleri [Öğreticisi: Deployment Manager'ı Azure Resource Manager şablonlarıyla kullanma](./deployment-manager-tutorial.md).

* Topoloji şablonu: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json
* Yapıt deposu: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Oluşturduğunuz sağlıksız durum URL'sini kullanarak [bir sistem durumu denetimi hizmeti simülatör'ü oluşturma](#create-a-health-check-service-simulator). İçin **managedIdentityID**, bkz: [kullanıcı tarafından atanan bir yönetilen kimlik oluşturma](./deployment-manager-tutorial.md#create-the-user-assigned-managed-identity).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$managedIdentityID = Read-Host -Prompt "Enter a user-assigned managed identity"
$healthCheckUrl = Read-Host -Prompt "Enter the health check Azure function URL"
$healthCheckAuthAPIKey = $healthCheckUrl.Substring($healthCheckUrl.IndexOf("?code=")+6, $healthCheckUrl.Length-$healthCheckUrl.IndexOf("?code=")-6)
$healthCheckUrl = $healthCheckUrl.Substring(0, $healthCheckUrl.IndexOf("?"))

$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the rollout
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation `
    -managedIdentityID $managedIdentityID `
    -healthCheckUrl $healthCheckUrl `
    -healthCheckAuthAPIKey $healthCheckAuthAPIKey
```

> [!NOTE]
> `New-AzResourceGroupDeployment` zaman uyumsuz bir çağrıdır. Başarı iletisi yalnızca dağıtım başarıyla başladı anlamına gelir. Dağıtımı doğrulamak için `Get-AZDeploymentManagerRollout`.  Aşağıdaki yordama bakın.

Aşağıdaki PowerShell betiğini kullanarak dağıtım ilerleme durumunu denetlemek için:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$resourceGroupName = "${projectName}rg"
$rolloutName = "${projectName}Rollout"

# Get the rollout status
Get-AzDeploymentManagerRollout `
    -ResourceGroupName $resourceGroupName `
    -Name $rolloutName `
    -Verbose
```

Aşağıdaki örnek çıktıda dağıtımı iyi durumda olmayan durum nedeniyle başarısız oldu gösterir:

```output
Service: myhc0417ServiceWUSrg
    TargetLocation: WestUS
    TargetSubscriptionId: <Subscription ID>

    ServiceUnit: myhc0417ServiceWUSWeb
        TargetResourceGroup: myhc0417ServiceWUSrg

        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
Get-AzDeploymentManagerRollout :
Service: myhc0417ServiceWUSrg
    ServiceUnit: myhc0417ServiceWUSWeb
        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
At line:1 char:1
+ Get-AzDeploymentManagerRollout `
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Get-AzDeploymentManagerRollout], Exception
+ FullyQualifiedErrorId : RolloutFailed,Microsoft.Azure.Commands.DeploymentManager.Commands.GetRollout


ResourceGroupName       : myhc0417rg
BuildVersion            : 1.0.0.0
ArtifactSourceId        : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/artifactSources/myhc0417ArtifactSourceRollout
TargetServiceTopologyId : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/serviceTopologies/myhc0417ServiceTopology
Status                  : Failed
TotalRetryAttempts      : 0
Identity                : Microsoft.Azure.Commands.DeploymentManager.Models.PSIdentity
OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
Services                : {myhc0417ServiceWUS, myhc0417ServiceWUSrg}
Name                    : myhc0417Rollout
Type                    : Microsoft.DeploymentManager/rollouts
Location                : centralus
Id                      : /subscriptions/<Subscription ID>/resourcegroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/rollouts/myhc0417Rollout
Tags                    :
```

Dağıtım tamamlandıktan sonra bir ek kaynak grubu Batı ABD için oluşturulan göreceksiniz.

## <a name="deploy-the-rollout-with-the-healthy-status"></a>Dağıtım durumu sağlıklı ile dağıtma

Bu bölümde dağıtım durumu sağlıklı URL'si ile yeniden dağıtmak için yineleyin.  Dağıtım tamamlandıktan sonra Doğu ABD için oluşturulan bir daha fazla kaynak grubu görmeniz gerekir.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure portalı](https://portal.azure.com) açın.
2. Piyasaya çıkarma dağıtımı tarafından oluşturulan yeni kaynak gruplarındaki yeni oluşturulan web uygulamalarına gidin.
3. Web uygulamasını bir web tarayıcıda açın. Index.html dosyasında konumu ve sürümü doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. Bu öğreticide oluşturulan kaynak gruplarını daraltmak için **Ada göre filtrele** alanını kullanın. 3-4 adet olacaktır:

    * **&lt;namePrefix>rg**: Deployment Manager kaynaklarını içerir.
    * **&lt;namePrefix>ServiceWUSrg**: ServiceWUS tarafından tanımlanan kaynakları içerir.
    * **&lt;namePrefix>ServiceEUSrg**: ServiceEUS tarafından tanımlanan kaynakları içerir.
    * Kullanıcı tanımlı yönetilen kimlik için kaynak grubu.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.
5. Bu öğretici tarafından oluşturulan diğer kaynak gruplarını silmek için son iki adımı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Deployment Manager'ın sistem durumu denetimi özelliğini nasıl kullanacağınızı öğrendiniz. Daha fazla bilgi edinmek için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).
