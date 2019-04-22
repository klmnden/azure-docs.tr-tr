---
title: Sürekli tümleştirme ve teslim Azure Data factory'de | Microsoft Docs
description: Data Factory işlem hatları (geliştirme, test ve üretim) bir ortamdan diğerine taşımak için sürekli tümleştirme ve teslim kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: gauravmalhot
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: gamal
manager: craigg
ms.openlocfilehash: 2edd4e28a0dd67be3c06159bce2e968d681b7f70
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58905265"
---
# <a name="continuous-integration-and-delivery-cicd-in-azure-data-factory"></a>Sürekli tümleştirme ve teslim (CI/CD) Azure Data factory'de

Sürekli tümleştirme, sistemi, yapılan her değişikliği testin uygulamadır, otomatik olarak ve mümkün olduğunca erken kod tabanı. Sürekli teslim, test sırasında sürekli tümleştirme olur ve değişiklikler bir hazırlık veya üretim sistemine gönderim izler.

Azure Data Factory için sürekli tümleştirme ve teslim taşıma Data Factory işlem hatları (geliştirme, test ve üretim) bir ortamdan diğerine gösterir. Sürekli tümleştirme ve teslim yapmak için Azure Resource Manager şablonları ile tümleştirme Data Factory kullanıcı Arabirimi kullanabilirsiniz. Data Factory kullanıcı arabirimini seçtiğinizde bir Resource Manager şablonu oluşturabilir **ARM şablonu** seçenekleri. Seçtiğinizde, **dışarı ARM şablonu**, portalda Resource Manager şablonu için veri fabrikasını ve tüm bağlantı dizeleri ve diğer parametreleri içeren bir yapılandırma dosyası oluşturur. Ardından, bir yapılandırma dosyası her bir ortamda (geliştirme, test ve üretim) oluşturmanız gerekir. Ana Resource Manager şablon dosyası tüm ortamlar için aynı kalır.

Dokuz dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-resource-manager-template-for-each-environment"></a>Her ortam için bir Resource Manager şablonu oluşturma
Seçin **dışarı ARM şablonu** geliştirme ortamında veri fabrikanızın Resource Manager şablonunu dışarı aktarmak için.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

Ardından, veri fabrikası test ve üretim veri fabrikası gidin ve seçin **alma ARM şablonu**.

![](media/continuous-integration-deployment/continuous-integration-image2.png)

Bu eylem, Azure portalında nerede dışarı aktarılan şablonu içeri aktarabilirsiniz alır. Seçin **düzenleyicide kendi şablonunuzu oluşturun** ardından **yük dosyası** ve oluşturulan Resource Manager şablonu seçin. Data factory ve ayarları sağlayın ve tüm işlem hattının üretim ortamınıza içeri aktarılır.

![](media/continuous-integration-deployment/continuous-integration-image3.png)

![](media/continuous-integration-deployment/continuous-integration-image4.png)

Seçin **yük dosyası** dışarı aktarılan Resource Manager şablonu seçin ve tüm yapılandırma değerleri (örneğin, bağlı hizmetler) girin.

![](media/continuous-integration-deployment/continuous-integration-image5.png)

**Bağlantı dizeleri**. Bağlantı dizeleri tek tek bağlayıcılar hakkında makaleler oluşturmak için gerekli bilgileri bulabilirsiniz. Örneğin, Azure SQL veritabanı için bkz. [veri kopyalama için veya Azure SQL veritabanı'ndan Azure Data Factory kullanarak](connector-azure-sql-database.md). -Bağlı bir hizmet için doğru bağlantı dizesini doğrulamak için örneğin - da kaynak kod görünümü Data Factory kullanıcı Arabiriminde açabilirsiniz. Kod Görünümü'nde, ancak bağlantı dizesinin parolanızı veya hesabınızı anahtar bölümünü kaldırılır. Kod görünümü açmak için aşağıdaki ekran görüntüsünde vurgulanmış simgesini seçin.

![Bağlantı dizesini görmek için kod görünümü Aç](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="continuous-integration-lifecycle"></a>Sürekli Tümleştirme yaşam döngüsü
Sürekli tümleştirme ve teslim kullanabileceğiniz için tüm yaşam döngüsüne İşte Azure depoları Git tümleştirmesi Data Factory kullanıcı arabiriminde etkinleştirdikten sonra:

1.  Azure tüm geliştiriciler işlem hatları, veri kümeleri ve diğerleri gibi Data Factory kaynaklarını yazabilirsiniz depoları ile geliştirme veri fabrikası oluşturun.

1.  Geliştiriciler, işlem hatları gibi kaynakları daha sonra değiştirebilirsiniz. Bunlar, değişiklikler gibi seçebilirsiniz **hata ayıklama** işlem hattı en son değişikliklerle nasıl çalıştığını görmek için.

1.  Geliştiriciler kendi değişikliklerden memnunsanız sonra bunlar ana dal (veya işbirliği dal) eşleri tarafından gözden yaptıkları değişiklikleri almak için kendi daldan bir çekme isteği oluşturabilirsiniz.

1.  Değişiklikleri ana dalınızda sonra bunlar seçerek geliştirme fabrikasına yayımlayabilirsiniz **Yayımla**.

1.  Takım test Fabrika ve üretim fabrikası değişiklikleri yükseltmek hazır olduğunda, Canlı geliştirme Data Factory, ana dal yedekler durumunda, Resource Manager şablonu ana daldan gelen veya diğer herhangi bir dala dışarı aktarabilirsiniz.

1.  Dışarı aktarılan Resource Manager şablonu test Fabrika hem de üretim fabrikası için farklı parametre dosyaları ile dağıtılabilir.

## <a name="automate-continuous-integration-with-azure-pipelines-releases"></a>Azure işlem hatları sürümleri ile sürekli tümleştirme otomatikleştirin

Veri Fabrikası birden çok ortama dağıtımı otomatik hale getirmek için bir Azure işlem hatları yayını Ayarla için adımlar aşağıda verilmiştir.

![Azure işlem hatları ile sürekli tümleştirme diyagramı](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Gereksinimler

-   Bir Azure aboneliğine bağlı Team Foundation Server veya Azure depoları kullanarak [*Azure Resource Manager hizmet uç noktası*](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

-   Data Factory ile Azure depoları Git tümleştirmesi.

-   Bir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) gizli dizileri içeren.

### <a name="set-up-an-azure-pipelines-release"></a>Bir Azure işlem hatları yayını Ayarla

1.  Data Factory ile yapılandırılmış bir olarak aynı projede Azure depo sayfanıza gidin.

1.  Üst menü çubuğunda **Azure işlem hatları** &gt; **yayınlar** &gt; **Oluştur yayın tanımı**.

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Seçin **boş işlem** şablonu.

1.  Ortamınızın adını girin.

1.  Git yapıt ekleyin ve Data Factory ile yapılandırılmış aynı depoda'ni seçin. Seçin `adf_publish` varsayılan en son sürümüyle varsayılan dal olarak.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Bir Azure Resource Manager dağıtım görevi ekleyin:

    a.  Yeni görev oluşturun, arama **Azure kaynak grubu dağıtımı**ve bunu ekleyin.

    b.  Dağıtım görevi, abonelik, kaynak grubu ve Data Factory hedef konumu seçin ve gerekirse, kimlik bilgilerini sağlayın.

    c.  Seçin **kaynak grubu oluşturma veya güncelleştirme** eylem.

    d.  Seçin **...** içinde **şablon** alan. Resource Manager şablonu bulun (*ARMTemplateForFactory.json*) portalında Yayımla eylemini tarafından oluşturuldu. Bu dosyada bir klasörü arayın `<FactoryName>` , `adf_publish` dal.

    e.  Parametre dosyasını için aynı işlevi görür. Bir kopya oluşturduğunuz bağlı olarak doğru dosyayı seçin veya varsayılan dosya kullandığınız *ARMTemplateParametersForFactory.json*.

    f.  Seçin **...** yanındaki **şablon parametrelerini geçersiz kıl** alan ve Data Factory hedef bilgileri doldurun. Şu biçimde gizli dizi için aynı adı için anahtar kasasından gelen kimlik bilgilerini kullanın: parolanın adı olduğu varsayılırsa `cred1`, girin `"$(cred1)"` (tırnak işaretleri arasında olmalıdır).

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

    g. Seçin **artımlı** dağıtım modu.

    > [!WARNING]
    > Seçerseniz **tam** kaynakları mevcut dağıtım modu, silinmiş olabilir, Resource Manager şablonunda tanımlı değil hedef kaynak grubundaki tüm kaynakları dahil olmak üzere.

1.  Sürüm ardışık kaydedin.

1.  Bu yayın ardışık düzeni yeni bir yayın oluşturun.

    ![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="optional---get-the-secrets-from-azure-key-vault"></a>İsteğe bağlı: Azure Key Vault'tan gizli dizileri Al

Bir Azure Resource Manager şablonunda geçirmek için gizli dizileri varsa, Azure anahtar kasası Azure işlem hatlarını sürümde kullanmanızı öneririz.

Gizli dizileri işlemek için iki yolu vardır:

1.  Parolaları parametre dosyasına ekleyin. Daha fazla bilgi için bkz. [dağıtım sırasında güvenli bir parametre geçirmek için Azure anahtar kasası kullanım](../azure-resource-manager/resource-manager-keyvault-parameter.md).

    -   Yayımlama dala karşıya parametreleri dosyasının bir kopyasını oluşturun ve aşağıdaki biçime sahip anahtar kasasından almak istediğiniz parametre değerlerini ayarlayın:

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

    -   Bu yöntemi kullandığınızda, gizli anahtar kasasından otomatik olarak aktarılır.

    -   Parametre dosyasını yayımlama dalında de olması gerekir.

1.  Ekleme bir [Azure anahtar kasası görev](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) Azure Resource Manager önceki bölümde açıklanan dağıtımdan önce:

    -   Seçin **görevleri** sekmesinde, yeni bir görev oluşturma, arama **Azure anahtar kasası** ve ekleyin.

    -   Key Vault görevde, anahtar kasasını oluşturduğunuz aboneliği seçin, sağlamanız gerekiyorsa, kimlik ve anahtar Kasası'nı seçin.

    ![](media/continuous-integration-deployment/continuous-integration-image8.png)

### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Azure işlem hatları aracısına izin ver
Azure Key Vault görev fIntegration çalışma zamanı saati erişim reddedildi hatası ile başarısız olabilir. Yayın günlükleri indirmek ve bulun `.ps1` Azure işlem hatları aracıya izin vermek için komut dosyası. Komutu doğrudan çalıştırabilirsiniz veya asıl kimliği dosyasından kopyalayın ve erişim ilkesi, Azure portalında el ile ekleyin. (*Alma* ve *listesi* olan gerekli en düşük izinleri).

### <a name="update-active-triggers"></a>Güncelleştirme etkin Tetikleyicileri
Etkin Tetikleyicileri güncelleştirmeye çalışırsanız, dağıtım başarısız olabilir. Etkin Tetikleyicileri güncelleştirmek için el ile sonlandırmasına ve bunları dağıtımdan sonra başlatmak gerekir. Aşağıdaki örnekte gösterildiği gibi bu amaç için bir Azure Powershell görev ekleyebilirsiniz:

1.  Yayın görevleri sekmede arama **Azure Powershell** ve ekleyin.

1.  Seçin **Azure Resource Manager** bağlantı olarak yazın ve aboneliğinizi seçin.

1.  Seçin **satır içi betik** betik olarak yazın ve ardından kodunuzu sağlayın. Aşağıdaki örnek, Tetikleyiciler durdurur:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

Benzer adımları izleyin ve benzer bir kod kullanın (ile `Start-AzDataFactoryV2Trigger` işlevi) Tetikleyiciler dağıtımdan sonra yeniden başlatmak için.

> [!IMPORTANT]
> Sürekli tümleştirme ve dağıtım senaryoları, tümleştirme çalışma zamanı türü farklı ortamlar genelinde aynı olmalıdır. Örneğin, bir *şirket içinde barındırılan* Integration Runtime (IR) geliştirme ortamında, aynı IR türde olmalıdır *şirket içinde barındırılan* test ve üretim gibi diğer ortamlarda da. Tümleştirme çalışma zamanları arasında birden çok aşama paylaşıyorsanız, benzer şekilde, tümleştirme çalışma zamanları olarak yapılandırmak kullandığınız *bağlı şirket içinde barındırılan* , geliştirme, test ve üretim gibi tüm ortamlarda.

## <a name="sample-deployment-template"></a>Örnek dağıtım şablonu

Azure işlem hatlarında aktarabileceğiniz bir örnek dağıtım şablonu aşağıda verilmiştir.

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

## <a name="sample-script-to-stop-and-restart-triggers-and-clean-up"></a>Durdur ve Tetikleyicileri yeniden başlatın ve temizlemek için betik örnek

Dağıtımdan önce Tetikleyicileri durdurmak ve Tetikleyicileri daha sonra yeniden başlatmak için bir örnek betiği aşağıda verilmiştir. Betik ayrıca kaldırılmış olan kaynakları silmek için kod içerir. Azure PowerShell'in en son sürümünü yüklemek için bkz [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder,
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start Active triggers - After cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Özel Parametreler ile Resource Manager şablonu kullanma

GIT modunda olduğunda, şablonu ve sabit kodlanmış özellikler Parametreleştirilen özellikleri ayarlamak için Resource Manager şablonunuzu varsayılan özellikleri geçersiz kılabilirsiniz. Bu senaryolarda varsayılan Parametreleştirme şablonu geçersiz kılmak isteyebilirsiniz:

* Otomatik CI/CD kullanın ve Resource Manager dağıtım sırasında bazı özelliklerini değiştirmek istediğiniz, ancak varsayılan olarak parametreli özellikler değildir.
* Fabrikanızı parametreleri (256) izin verilen en fazla olduğundan varsayılan Resource Manager şablonu geçersiz kadar büyüktür.

Bu koşullar altında varsayılan Parametreleştirme şablonu geçersiz kılmak için adlı bir dosya oluşturun. *arm şablonu parametreleri definition.json* deposunun kök klasörüne. Dosya adı tam olarak eşleşmelidir. Data Factory, bu dosyayı okuma, şu anda Azure Data Factory Portalı'nda bulunan hangi daldan, yalnızca işbirliği dalından dener. Oluşturma veya test edebileceğiniz değişikliklerinizi kullanarak özel bir dal dosyasından Düzenle **dışarı ARM şablonu** kullanıcı arabiriminde. Ardından, dosyayı işbirliği dala birleştirebilirsiniz. Dosya bulunamazsa, varsayılan şablon kullanılır.


### <a name="syntax-of-a-custom-parameters-file"></a>Özel Parametreler dosyasının söz dizimi

Özel parametre dosyasını yazarken kullanmak için bazı yönergeler aşağıda verilmiştir. Her varlık türü için bir bölüm dosya oluşur: Tetikleyici, işlem hattı, linkedservice, dataset, integrationruntime ve benzeri.
* İlgili varlık türü altında özellik yolu girin.
* Bir özellik adı ayarlandığında '\*'', (yalnızca aşağı ilk düzeyi, yinelemeli olarak) altındaki tüm özellikleri parametreleştirmek istediğiniz belirtin. Bu özel durumların de sağlayabilirsiniz.
* Bir özelliğin değerini bir dize olarak ayarladığınızda, özellik parametreleştirmek istediğiniz gösterir. Biçimini kullanın `<action>:<name>:<stype>`.
   *  `<action>` şu karakterlerden biri olabilir:
      * `=` yol geçerli değer parametresi için varsayılan değer olarak tutun.
      * `-` yol parametresi için varsayılan değer tutma.
      * `|` Azure Key vault'tan bir gizli bağlantı dizelerini veya anahtarlarının için özel bir durumdur.
   * `<name>` parametrenin adıdır. Boş ise, özelliğin adını alır. Değer ile başlıyorsa bir `-` karakter adı kısalttık. Örneğin, `AzureStorage1_properties_typeProperties_connectionString` kısaltılmıştır `AzureStorage1_connectionString`.
   * `<stype>` parametre türüdür. Varsa `<stype>` olan boş, varsayılan türü olduğu `string`. Desteklenen değerleri: `string`, `bool`, `number`, `object`, ve `securestring`.
* Bir dizi tanım dosyasında belirttiğinizde, şablonda eşleşen özellik dizisi olduğunu belirtir. Veri Fabrikası dizi içindeki tüm nesneler dizisi Integration Runtime nesnesinde belirtilen tanımı kullanarak gezinir. Dize, ikinci nesne, her yineleme için parametre adı olarak kullanılan özelliğin adı olur.
* Kaynak örneği için belirli bir tanımı mümkün değildir. Herhangi bir tanımı bu türdeki tüm kaynakları için geçerlidir.
* Varsayılan olarak, Key Vault gizli dizileri ve bağlantı dizeleri, anahtarları ve belirteçleri, gibi güvenli dizeler gibi tüm güvenli dizeleri Parametreleştirilen.
 
## <a name="sample-parameterization-template"></a>Örnek Parametreleştirme şablonu

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```

### <a name="explanation"></a>Açıklama:

#### <a name="pipelines"></a>İşlem hatları
    
* Etkinlikler/typeProperties/waitTimeInSeconds yolu herhangi bir özelliği parametreli. Buna herhangi bir etkinliği adlı bir kod düzeyinde özelliğine sahip bir işlem hattı `waitTimeInSeconds` (örneğin, `Wait` etkinliği) varsayılan ada sahip bir sayı olarak parametreli. Ancak, Resource Manager şablonunda bir varsayılan değere sahip olmaz. Bu, Resource Manager dağıtım sırasında bir zorunlu giriş olacaktır.
* Benzer şekilde, bir özelliğin çağırılır `headers` (örneğin, bir `Web` etkinliği) türüyle parametreli `object` (JObject). Kaynak fabrikası ile aynı değer bir varsayılan değer var.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Yalnızca özellikler ve yol altındaki tüm özellikleri `typeProperties` , ilgili varsayılan değerlerinde Parametreleştirilen. Örneğin, bugünün şema itibaren iki özellik yok altında **IntegrationRuntimes** türü özellikleri: `computeProperties` ve `ssisProperties`. Her iki özellik türleri ile ilgili varsayılan değerler ve türler (nesne) oluşturulur.

#### <a name="triggers"></a>Tetikleyiciler

* Altında `typeProperties`, parametreli iki özellikler. İlki `maxConcurrency`, varsayılan değerine sahip olacak şekilde belirtilen ve türü olacaktır `string`. Varsayılan parametre adını taşıyan `<entityName>_properties_typeProperties_maxConcurrency`.
* `recurrence` Özelliği de parametreli. Bunun altında varsayılan değerleri ve parametre adları içeren bir dize olarak parametre haline getirilip için o düzeydeki tüm özellikleri belirtilir. Bir özel durum `interval` sayı türü olarak parametreli ve parametre adıyla sonekine sahip özellik `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. Benzer şekilde, `freq` özelliği bir dize ve dize olarak parametreli. Ancak, `freq` özelliği bir varsayılan değer parametreli. Ad kısalttık ve soneki. Örneğin, `<entityName>_freq`.

#### <a name="linkedservices"></a>linkedServices

* Bağlı hizmetler, benzersiz. Bağlı hizmetleri ve veri kümeleri olası birkaç türde olabileceğinden, türe özgü özelleştirme sağlayabilir. Örneğin, tüm türdeki hizmetlerin bağlı diyebilirsiniz `AzureDataLakeStore`, belirli bir şablon uygulanmış ve diğer tüm olacaktır (aracılığıyla \*) farklı bir şablon uygulanır.
* Önceki örnekte `connectionString` özellik parametreli olarak bir `securestring` değeri varsayılan değere sahip olmaz ve kısaltılmış parametre adları ile sonekine sahip olacaktır `connectionString`.
* Özellik `secretAccessKey`, ancak özelleştirmede bir `AzureKeyVaultSecret` (örneğin, bir `AmazonS3` bağlı hizmet). Bu nedenle, otomatik olarak bir Azure Key Vault gizli parametreli ve kaynak fabrikada yapılandırılır anahtar kasasından getirildi. Ayrıca kendi key vault parametreleştirebilirsiniz.

#### <a name="datasets"></a>Veri kümeleri

* Türe özgü özelleştirme veri kümeleri için kullanılabilir olsa bile yapılandırma açıkça zorunda kalmadan sağlanabilir bir \*-düzeyi yapılandırma. Yukarıdaki örnekte, tüm veri kümesi özellikleri altında `typeProperties` Parametreleştirilen.

Varsayılan Parametreleştirme şablonu değiştirebilirsiniz, ancak bu geçerli bir şablonudur. Bu, yeniden oluşturmanız gerekir ve mevcut parameterizations kaybetmek istemiyorsanız, bir ek özellik parametre olarak, aynı zamanda eklemek istediğinizde yararlı olacaktır.


```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

**Örnek**: Bir Databricks etkileşimli küme Kimliğinden (Databricks bağlı hizmeti) için parametre dosyasını ekleyin:

```
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```


## <a name="linked-resource-manager-templates"></a>Bağlantılı Resource Manager şablonları

Sürekli tümleştirmeyi ve dağıtım (CI/CD), veri fabrikaları için ayarladıysanız, fabrikanızı büyük büyüdükçe, Resource Manager şablonu sınırları, kaynaklar veya kaynak en fazla yüke sayısı gibi yaşadığınız, gözlemleyin Yöneticisi şablonu. Data Factory, tam bir Resource Manager şablonu için bir Fabrika oluşturma yanı sıra bu gibi senaryolar için bağlantılı Resource Manager şablonları artık oluşturur. Sonuç olarak, belirtilen sınırları çalışmasını önlemek için birkaç dosyalarına ayrılmış tüm fabrikanızın yükü sahip olursunuz.

Yapılandırılan Git varsa, bağlı şablonların oluşturulur ve tüm Resource Manager şablonları ile birlikte kaydedilmiş `adf_publish` adlı yeni bir klasör altında bir dal `linkedTemplates`.

![Bağlantılı Resource Manager Şablonları klasörü](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Bağlantılı Resource Manager şablonları, genellikle bir ana şablon ve asıl bağlı alt şablonları kümesi bulunur. Üst şablonun adlı `ArmTemplate_master.json`, ve desen ile alt şablonları adlı `ArmTemplate_0.json`, `ArmTemplate_1.json`ve benzeri. Bağlı şablonlar kullanarak tam Resource Manager şablonu kullanarak kaydırmak için CI/CD göreviniz işaret edecek şekilde güncelleştirme `ArmTemplate_master.json` işaret yerine `ArmTemplateForFactory.json` (diğer bir deyişle, tam Resource Manager şablonu). Resource Manager, dağıtım sırasında Azure tarafından erişilebilmelerini böylece bağlı şablonların bir depolama hesabına veri yükleme gerektirir. Daha fazla bilgi için bkz. [VSTS ile bağlantılı ARM şablonlarını dağıtma](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/).

Önce ve sonra dağıtım görevi, CI/CD işlem hattı, Data Factory komut eklemeyi unutmayın.

Yapılandırılan Git yoksa, bağlı şablonların aracılığıyla erişilebilir **dışarı ARM şablonu** hareketi.

## <a name="best-practices-for-cicd"></a>CI/CD için en iyi uygulamalar

Git Tümleştirmesi ile veri fabrikanızı kullanıyorsanız ve değişikliklerinizi geliştirme, Test ve sonra üretime taşır. bir CI/CD işlem hattı varsa aşağıdaki en iyi öneririz:

-   **Git tümleştirmesi**. Yalnızca geliştirme veri fabrikanıza Git tümleştirmesiyle yapılandırmak için gereklidir. Değişiklikleri Test ve üretim için CI/CD dağıtılır ve bunlar Git tümleştirmesi olması gerekmez.

-   **Data Factory CI/CD komut**. CI/CD, Resource Manager dağıtım adımı önce tetikleyiciler ve farklı türde bir Fabrika temizleme durdurma gibi şeyler ilgileniriz gerekir. Kullanmanızı öneririz [bu betik](#sample-script-to-stop-and-restart-triggers-and-clean-up) gibi tüm bu işlemler üstlenir. Dağıtımdan önce bir kez ve tamamladıktan sonra uygun bayrakları kullanarak betiği çalıştırın.

-   **Tümleştirme çalışma zamanları ve Paylaşım**. Tümleştirme çalışma zamanları daha az sıklıkta değişiklik yapmak ve CI/CD tüm aşamaları boyunca benzer altyapısal bileşenleri, data factory'de biridir. Sonuç olarak, Data Factory tümleştirme çalışma zamanları, CI/CD tüm aşamaları boyunca aynı türde ve aynı ada sahip olmasını bekliyor. Tümleştirme çalışma zamanları - Örneğin, şirket içinde barındırılan tümleştirme çalışma zamanları - tüm aşamaları paylaşmak istiyorsanız paylaşmak için bir paylaşılan tümleştirme çalışma zamanları içeren için Üçlü bir factory'de şirket içinde barındırılan IR barındırarak yoludur. Ardından, bunları geliştirme/Test/üretim bağlı IR türü olarak kullanabilirsiniz.

-   **Anahtar kasası**. Önerilen kullandığınızda Azure anahtar kasası tabanlı bağlı hizmetler, geliştirme/Test/Prod için büyük olasılıkla ayrı anahtar kasalarını tutarak kendi avantajları bir düzey daha fazla sürebilir. Ayrıca bunların her biri için ayrı izin düzeylerini yapılandırabilirsiniz. Takım üyelerinizin üretim gizli dizileri izinlerine sahip olmasını istemeyebilirsiniz. Ayrıca, tüm aşamaları aynı gizli adların tutmanızı öneririz. Aynı adları tutarsanız, değiştirilmesi gereken tek şey, Resource Manager şablon parametrelerinden biri olan anahtar kasası adı olduğundan, Resource Manager şablonları CI/CD arasında değiştirmeniz gerekmez.

## <a name="unsupported-features"></a>Desteklenmeyen özellikler

-   Data factory varlıklarını birbirine bağımlı olduğundan tek tek kaynakları yayımlanamıyor. Örneğin, işlem hatları Tetikleyiciler bağlıdır, işlem hatları, veri kümeleri ve diğer işlem hatları, vb. üzerinde bağlıdır. Değişen bağımlılıkları izleme zordur. El ile yayımlamak için gereken kaynakları seçmek mümkün ise, yalnızca bir alt kümesinin tamamını yayımladıktan sonra şeyler beklenmeyen davranışa neden, değişikliklerin çekme mümkün olacaktır.

-   Özel dallardan yayımlanamıyor.

-   Bitbucket projelerde barındıramaz.
