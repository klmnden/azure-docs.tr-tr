---
title: Sürekli tümleştirme ve Azure Data Factory dağıtımda | Microsoft Docs
description: Data Factory işlem hatlarını (geliştirme, test, üretim) bir ortamdan diğerine taşımak için sürekli tümleştirme ve dağıtım kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: douglasl
ms.openlocfilehash: 17fb10f4b39361a99d3f51ed753d333c6ec0bf15
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34618598"
---
# <a name="continuous-integration-and-deployment-in-azure-data-factory"></a>Sürekli tümleştirme ve Azure Data Factory dağıtımında

Sürekli Tümleştirme yapılan her değişiklik test uygulaması olduğundan, otomatik olarak ve mümkün olduğunca erken codebase. Sürekli dağıtım sırasında sürekli tümleştirme olur ve değişiklikler bir hazırlık veya üretim sisteme iter test izler.

Azure Data Factory için sürekli tümleştirme ve dağıtım taşıma Data Factory işlem hatlarını (geliştirme, test, üretim) bir ortamdan diğerine gelir. Sürekli tümleştirme ve dağıtım yapmak için Azure Resource Manager şablonları ile veri fabrikası UI tümleştirmesi kullanabilirsiniz. Seçtiğiniz Data Factory UI Resource Manager şablonu oluşturabilir **ARM şablonu** seçenekleri. Seçtiğinizde, **verme ARM şablonu**, portal data factory ve tüm bağlantı dizeleri ve diğer parametreleri içeren bir yapılandırma dosyası için Resource Manager şablonu oluşturur. Ardından (geliştirme, test, üretim) her ortam için bir yapılandırma dosyası oluşturmanız gerekir. Ana Resource Manager şablon dosyası tüm ortamlar için aynı kalır.

Dokuz dakikalık bir giriş ve bu özellik tanıtımı için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

## <a name="create-a-resource-manager-template-for-each-environment"></a>Her ortam için Resource Manager şablonu oluşturma
Seçin **verme ARM şablonu** geliştirme ortamında veri fabrikanızın Resource Manager şablonu dışarı aktarmak için.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

Ardından, test veri fabrikası ve üretim veri fabrikası gidin ve seçin **alma ARM şablonu**.

![](media/continuous-integration-deployment/continuous-integration-image2.png)

Bu eylem, dışarı aktarılan şablon burada aktarabilirsiniz Azure portalına alır. Seçin **kendi şablonunuzu Düzenleyicisi'nde yapı** ve ardından **yükleme dosyası** ve oluşturulan Resource Manager şablonu seçin. Ayarlar ve data factory sağlar ve tüm ardışık düzen, üretim ortamınızda alınır.

![](media/continuous-integration-deployment/continuous-integration-image3.png)

![](media/continuous-integration-deployment/continuous-integration-image4.png)

Seçin **yükleme dosyası** dışarı aktarılan Resource Manager şablonunu seçin ve tüm yapılandırma değerleri (örneğin, bağlantılı hizmetler) girin.

![](media/continuous-integration-deployment/continuous-integration-image5.png)

## <a name="continuous-integration-lifecycle"></a>Sürekli Tümleştirme yaşam döngüsü
Sürekli Tümleştirme & kullanabileceğiniz dağıtımı için tüm yaşam döngüsü İşte VSTS GIT tümleştirmesi veri fabrikası kullanıcı arabiriminde etkinleştirdikten sonra:

1.  Bir geliştirme data factory tüm geliştiriciler ardışık düzen, veri kümeleri ve diğerleri gibi Veri Fabrikası Kaynakları yazabilirsiniz VSTS ile ayarlayın.

2.  Ardından geliştiriciler ardışık düzen gibi kaynakları değiştirebilirsiniz. Kendi değişiklikler yapma gibi seçebilecekleri **hata ayıklama** ardışık düzen en son değişikliklerle birlikte nasıl çalıştığını görmek için.

3.  Geliştiriciler kendi değişikliklerden memnunsanız sonra bunlar eşleri tarafından gözden kendi değişiklikleri almak amacıyla ana dala (veya işbirliği şube) kendi daldan bir çekme isteği oluşturabilirsiniz.

4.  Değişiklikleri ana dala sonra bunlar seçerek geliştirme Fabrika yayımlayabilirsiniz **Yayımla**.

5.  Takım test Fabrika ve üretim fabrikası değişiklikler yükseltmek hazır olduğunda, kendi ana dala Canlı geliştirme Data Factory yedekler durumda bunlar Resource Manager şablonu ana dala veya başka bir şube dışarı aktarabilirsiniz.

6.  Dışarı aktarılan Resource Manager şablonu test Fabrika ve üretim fabrikası farklı parametre dosyalarla dağıtılabilir.

## <a name="automate-continuous-integration-with-vsts-releases"></a>VSTS sürümleriyle sürekli tümleştirme otomatikleştirme

Veri Fabrikası birden çok ortamlara dağıtımını otomatik hale getirebilmeniz VSTS serbest bırakma ayarı ayarlamak için adımlar şunlardır.

![VSTS ile sürekli tümleştirme diyagramı](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Gereksinimler

-   Bir Azure aboneliğine bağlı Team Foundation Server veya VSTS kullanılarak [ *Azure Resource Manager Hizmeti uç noktası*](https://docs.microsoft.com/vsts/build-release/concepts/library/service-endpoints#sep-azure-rm).

-   Veri Fabrikası VSTS yapılandırılmış Git ile.

-   Bir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) gizli içeren.

### <a name="set-up-a-vsts-release"></a>VSTS yayın ayarlayın

1.  Data Factory ile yapılandırılmış bir aynı projede VSTS sayfanıza gidin.

2.  Üst menü çubuğunda **derleme ve sürüm** &gt; **sürümleri** &gt; **oluşturma yayın tanımı**.

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

3.  Seçin **boş işlem** şablonu.

4.  Ortamınızı adını girin.

5.  Git yapı ekleyin ve Data Factory ile yapılandırılmış aynı deponun seçin. Seçin `adf\_publish` son varsayılan sürümle varsayılan dalı olarak.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

6.  Gizli anahtarları Azure anahtar Kasası'alın. Gizli işlemek için iki yolu vardır:

    a.  Parolaları parametre dosyasına ekleyin:

       -   Yayımla dalının karşıya parametreleri dosyanın bir kopyasını oluşturun ve aşağıdaki biçimde anahtar Kasası'ndan almak istediğiniz parametrelerinin değerlerini ayarlayın:

        ```json
        {
            "parameters": {
                "azureSqlReportingDbPassword": {
                    "reference": {
                        "keyVault": {
                            "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                        },
                        "secretName": " < secret - name > "
                    }
                }
            }
        }
        ```

       -   Bu yöntemi kullandığınızda, gizli anahtar Kasası'nı otomatik olarak alınır.

       -   Parametreler dosyası da Yayımla dalında olması gerekir.

    b.  Ekleme bir [Azure anahtar kasası görev](https://docs.microsoft.com/vsts/build-release/tasks/deploy/azure-key-vault):

       -   Seçin **görevleri** sekmesinde, yeni bir görev oluşturun, arama **Azure anahtar kasası** ve bunu ekleyin.

       -   Anahtar kasası oluşturulan abonelik anahtar kasası görevde seçin, sağlamanız gerekiyorsa, kimlik bilgileri ve anahtar Kasası'ı seçin.

       ![](media/continuous-integration-deployment/continuous-integration-image8.png)

7.  Bir Azure Resource Manager dağıtım görev ekleyin:

    a.  Yeni bir görev oluşturun, arama **Azure kaynak grubu dağıtımı**ve bunu ekleyin.

    b.  Dağıtım görev abonelik, kaynak grubu ve hedef veri fabrikası için konum seçin ve gerekirse kimlik bilgilerini sağlayın.

    c.  Seçin **oluştur veya güncelleştir kaynak grubu** eylem.

    d.  Seçin **...** içindeki "**şablonu**" alanı. Resource Manager şablonu için Gözat (*ARMTemplateForFactory.json*) Portalı'nda Yayımla eylemi tarafından oluşturuldu. Bu dosya kök klasöründe arayın `adf\_publish` dal.

    e.  Parametreler dosyası için aynı işlevi görür. Bir kopyasını oluşturduğunuz bağlı olarak doğru dosya seçin veya varsayılan dosya kullanmakta olduğunuz *ARMTemplateParametersForFactory.json*.

    f.  Seçin **...** yanına **şablon parametreleri geçersiz kılma** alanına ve veri fabrikası hedefi için bilgileri girin. Şu biçimde gizliliği için aynı adı anahtar Kasası'nı gelen kimlik bilgilerini kullanmak için: varsayarsak parolanın olduğu `cred1`, girin `"$(cred1)"` (arasında tırnak).

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

8.  Yayın tanımını kaydedin.

9.  Bu sürüm tanımdan yeni bir sürüm oluşturun.

    ![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="grant-permissions-to-the-vsts-agent"></a>VSTS aracıya izinleri
Azure anahtar kasası Görev ilk kez bir erişim reddedildi hatası ile başarısız olabilir. Yayın günlükleri indirmek ve bulun `.ps1` VSTS aracıya izin vermek için komut dosyasıyla. Komutu doğrudan çalıştırabilirsiniz veya dosyadan sorumlusunun Kimliğini kopyalayın ve erişim ilkesini Azure portalında el ile ekleyin. (*Almak* ve *listesi* olan gerekli minimum izinleri).

### <a name="update-active-triggers"></a>Güncelleştirme etkin Tetikleyicileri
Etkin Tetikleyiciler güncelleştirmeye çalıştığınızda, dağıtım başarısız olabilir. Etkin Tetikleyiciler güncelleştirmek için el ile sonlandırmasına ve dağıtımdan sonra başlatmak gerekir. Aşağıdaki örnekte gösterildiği gibi bu amaç için bir Azure Powershell görev ekleyebilirsiniz:

1.  VSTS yayın görevleri sekmesinde, arama **Azure Powershell** ve bunu ekleyin.

2.  Seçin **Azure Resource Manager** bağlantı olarak yazın ve aboneliğinizi seçin.

3.  Seçin **satır içi betiği** komut dosyası olarak yazın ve ardından kodunuzu sağlayın. Aşağıdaki örnek Tetikleyiciler durdurur:

    ```powershell
    $armTemplate="$(env:System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json"

    $templateJson = Get-Content "$(env:System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json" | ConvertFrom-Json

    $triggersADF = Get-AzureRmDataFactoryV2Trigger -DataFactoryName
    $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $\_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

Benzer adımları izleyin ve benzer bir kod kullanın (ile `Start-AzureRmDataFactoryV2Trigger` işlevi) Tetikleyiciler dağıtımdan sonra yeniden başlatmak için.

## <a name="sample-template-and-script"></a>Örnek şablon ve komut dosyası
Sürekli tümleştirme ve dağıtım için Data Factory ile çalışmaya başlamak için kullanabileceğiniz iki örnekleri şunlardır:

-   VSTS içeri aktarabilirsiniz örnek dağıtım şablonu.
-   Örnek komut dosyası Tetikleyicileri dağıtmadan önce durdurmak ve yeniden başlatmak için daha sonra tetikler. Komut ayrıca kaldırılmış olan kaynakları silmek için kodu içerir.

VSTS içeri aktarabilirsiniz bir örnek dağıtım şablonu aşağıda verilmiştir.

```json
{
    "source": 2,
    "id": 1,
    "revision": 51,
    "name": "Data Factory Prod Deployment",
    "description": null,
    "createdBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "createdOn": "2018-03-01T22:57:25.660Z",
    "modifiedBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "modifiedOn": "2018-03-14T17:58:11.643Z",
    "isDeleted": false,
    "path": "\\",
    "variables": {},
    "variableGroups": [],
    "environments": [{
        "id": 1,
        "name": "Prod",
        "rank": 1,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserprod"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 1
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 2
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 3
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param\n(\n    [parameter(Mandatory = $false)] [String] $rootFolder=\"C:\\Users\\sampleuser\\Downloads\\arm_template\",\n    [parameter(Mandatory = $false)] [String] $armTemplate=\"$rootFolder\\arm_template.json\",\n    [parameter(Mandatory = $false)] [String] $armTemplateParameters=\"$rootFolder\\arm_template_parameters.json\",\n    [parameter(Mandatory = $false)] [String] $domain=\"microsoft.onmicrosoft.com\",\n    [parameter(Mandatory = $false)] [String] $TenantId=\"72f988bf-86f1-41af-91ab-2d7cd011db47\",\n    [parame",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": "5.*"
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "secret1",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/1"
    }, {
        "id": 2,
        "name": "Staging",
        "rank": 2,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserstaging"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 4
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 5
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 6
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "16a37943-8b58-4c2f-a3d6-052d6f032a07",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param(\n$x,\n$y,\n$z)\nwrite-host \"----------\"\nwrite-host $x\nwrite-host $y\nwrite-host $z | ConvertTo-SecureString\nwrite-host \"----------\"",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/2"
    }],
    "artifacts": [{
        "sourceId": "19749ef3-2f42-49b5-9696-f28b49faebcb:a6c88f30-5e1f-4de8-b24d-279bb209d85f",
        "type": "Git",
        "alias": "Dev",
        "definitionReference": {
            "branches": {
                "id": "adf_publish",
                "name": "adf_publish"
            },
            "checkoutSubmodules": {
                "id": "",
                "name": ""
            },
            "defaultVersionSpecific": {
                "id": "",
                "name": ""
            },
            "defaultVersionType": {
                "id": "latestFromBranchType",
                "name": "Latest from default branch"
            },
            "definition": {
                "id": "a6c88f30-5e1f-4de8-b24d-279bb209d85f",
                "name": "Dev"
            },
            "fetchDepth": {
                "id": "",
                "name": ""
            },
            "gitLfsSupport": {
                "id": "",
                "name": ""
            },
            "project": {
                "id": "19749ef3-2f42-49b5-9696-f28b49faebcb",
                "name": "Prod"
            }
        },
        "isPrimary": true
    }],
    "triggers": [{
        "schedule": {
            "jobId": "b5ef09b6-8dfd-4b91-8b48-0709e3e67b2d",
            "timeZoneId": "UTC",
            "startHours": 3,
            "startMinutes": 0,
            "daysToRelease": 31
        },
        "triggerType": 2
    }],
    "releaseNameFormat": "Release-$(rev:r)",
    "url": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1",
    "_links": {
        "self": {
            "href": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1"
        },
        "web": {
            "href": "https://sampleuser.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_release?definitionId=1"
        }
    },
    "tags": [],
    "properties": {
        "DefinitionCreationSource": {
            "$type": "System.String",
            "$value": "ReleaseNew"
        }
    }
}
```

Örnek komut dosyası Tetikleyicileri dağıtmadan önce durdurmak ve Tetikleyicileri daha sonra yeniden başlatmak için şöyledir:

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder="$(env:System.DefaultWorkingDirectory)/Dev/",
    [parameter(Mandatory = $false)] [String] $armTemplate="$rootFolder\arm_template.json",
    [parameter(Mandatory = $false)] [String] $ResourceGroupName="sampleuser-datafactory",
    [parameter(Mandatory = $false)] [String] $DataFactoryName="sampleuserdemo2",
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true

)


$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzureRmDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and $_.properties.pipelines.Count -gt 0} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { Stop-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force }
}
else {

    #start Active triggers
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force }

    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzureRmDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #delte resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { Remove-AzureRmDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { Remove-AzureRmDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { Remove-AzureRmDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { Remove-AzureRmDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { Remove-AzureRmDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force }
}
```
