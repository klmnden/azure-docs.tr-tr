---
title: PowerShell ile Azure Data factory'de bir paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma | Microsoft Docs
description: Tümleştirme çalışma zamanının birden çok veri fabrikaları erişebilmesi için Azure Data Factory'de bir paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: abnarain
ms.openlocfilehash: f038510c20e70c9d6b9dc8e396d9a15beb7270ca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66155145"
---
# <a name="create-a-shared-self-hosted-integration-runtime-in-azure-data-factory-with-powershell"></a>PowerShell ile Azure Data factory'de bir paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma

Bu adım adım kılavuz, Azure PowerShell kullanarak Azure Data Factory'de bir paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma işlemini göstermektedir. Ardından, başka bir veri fabrikasında paylaşılan şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz: 

1. Veri fabrikası oluşturma. 
1. Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
1. Şirket içinde barındırılan tümleştirme çalışma zamanının, diğer veri fabrikaları ile paylaşın.
1. Bir bağlı tümleştirme çalışma zamanı oluşturun.
1. Paylaşımı iptal edin.

## <a name="prerequisites"></a>Önkoşullar 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/). 

- **Azure PowerShell**. Bölümündeki yönergeleri [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). Diğer veri fabrikaları ile paylaşılabilen bir şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmak için bir betik çalıştırmak için PowerShell kullanın. 

> [!NOTE]  
> Data Factory kullanılabildiği şu anda Azure bölgelerinin listesi için üzerinde ilgilendiğiniz bölgeleri seçin [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory).

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Windows PowerShell Tümleşik Komut Dosyası Ortamı’nı (ISE) başlatın.

1. Değişkenleri oluşturun. Aşağıdaki betiği kopyalayıp yeniden açın. Değişkenleri aşağıdaki gibi değiştirin **SubscriptionName** ve **ResourceGroupName**, gerçek değerleriyle: 

    ```powershell
    # If input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$". 
    $SubscriptionName = "[Azure subscription name]" 
    $ResourceGroupName = "[Azure resource group name]" 
    $DataFactoryLocation = "EastUS" 

    # Shared Self-hosted integration runtime information. This is a Data Factory compute resource for running any activities 
    # Data factory name. Must be globally unique 
    $SharedDataFactoryName = "[Shared Data factory name]" 
    $SharedIntegrationRuntimeName = "[Shared Integration Runtime Name]" 
    $SharedIntegrationRuntimeDescription = "[Description for Shared Integration Runtime]"

    # Linked integration runtime information. This is a Data Factory compute resource for running any activities
    # Data factory name. Must be globally unique
    $LinkedDataFactoryName = "[Linked Data factory name]"
    $LinkedIntegrationRuntimeName = "[Linked Integration Runtime Name]"
    $LinkedIntegrationRuntimeDescription = "[Description for Linked Integration Runtime]"
    ```

1. Oturum açın ve bir abonelik seçin. Açıp Azure aboneliğinizi seçmek için betiğe aşağıdaki kodu ekleyin:

    ```powershell
    Connect-AzAccount
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

1. Bir kaynak grubu ve veri fabrikası oluşturun.

    > [!NOTE]  
    > Bu adım isteğe bağlıdır. Veri Fabrikası zaten varsa bu adımı atlayın. 

    Oluşturma bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) kullanarak [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnekte adlı bir kaynak grubu oluşturur `myResourceGroup` WestEurope konumda: 

    ```powershell
    New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
    ```

    Veri fabrikası oluşturmak için aşağıdaki komutu çalıştırın: 

    ```powershell
    Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                             -Location $DataFactoryLocation `
                             -Name $SharedDataFactoryName
    ```

## <a name="create-a-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma

> [!NOTE]  
> Bu adım isteğe bağlıdır. Diğer veri fabrikaları ile paylaşmak istediğiniz şirket içinde barındırılan tümleştirme çalışma zamanı zaten varsa bu adımı atlayın.

Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
$SharedIR = Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Type SelfHosted `
    -Description $SharedIntegrationRuntimeDescription
```

### <a name="get-the-integration-runtime-authentication-key-and-register-a-node"></a>Tümleştirme çalışma zamanı kimlik doğrulama anahtarı almak ve bir düğüm kaydetme

Şirket içinde barındırılan tümleştirme çalışma zamanı için kimlik doğrulama anahtarı almak için aşağıdaki komutu çalıştırın:

```powershell
Get-AzDataFactoryV2IntegrationRuntimeKey `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName
```

Bu şirket içinde barındırılan tümleştirme çalışma zamanı için kimlik doğrulama anahtarı yanıtı içerir. Integration runtime düğümü kaydettiğinizde bu anahtarı kullanırsınız.

### <a name="install-and-register-the-self-hosted-integration-runtime"></a>Yükleme ve şirket içinde barındırılan tümleştirme çalışma zamanını kaydetme

1. Şirket içinde barındırılan tümleştirme çalışma zamanı Yükleyicisi'nden indirin [Azure Data Factory Integration Runtime](https://aka.ms/dmg).

2. Şirket içinde barındırılan tümleştirme yerel bir bilgisayara yüklemek için yükleyiciyi çalıştırın.

3. Yeni şirket içinde barındırılan tümleştirme, önceki adımda aldığınız kimlik doğrulama anahtarı ile kaydedin.

## <a name="share-the-self-hosted-integration-runtime-with-another-data-factory"></a>Başka bir data factory ile şirket içinde barındırılan tümleştirme çalışma zamanı paylaşın

### <a name="create-another-data-factory"></a>Başka bir veri fabrikası oluşturma

> [!NOTE]  
> Bu adım isteğe bağlıdır. Paylaşmak istediğiniz bir veri fabrikası zaten varsa bu adımı atlayın.

```powershell
$factory = Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
    -Location $DataFactoryLocation `
    -Name $LinkedDataFactoryName
```
### <a name="grant-permission"></a>İzin ver

Oluşturulan ve kaydettirilen şirket içinde barındırılan tümleştirme çalışma zamanı erişmesi gereken veri fabrikasına izni verin.

> [!IMPORTANT]  
> Bu adımı atlayın değil!

```powershell
New-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId ` #MSI of the Data Factory with which it needs to be shared
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' ` #This is the Contributor role
    -Scope $SharedIR.Id
```

## <a name="create-a-linked-self-hosted-integration-runtime"></a>Bağlı şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma

Bir bağlı şirket içinde barındırılan tümleştirme çalışma zamanı oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
Set-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $LinkedDataFactoryName `
    -Name $LinkedIntegrationRuntimeName `
    -Type SelfHosted `
    -SharedIntegrationRuntimeResourceId $SharedIR.Id `
    -Description $LinkedIntegrationRuntimeDescription
```

Artık bu bağlantılı tümleştirme çalışma zamanı içinde herhangi bir bağlı hizmeti kullanabilirsiniz. Bağlantılı tümleştirme çalışma zamanı paylaşılan tümleştirme çalışma zamanı etkinlik çalıştıracak şekilde kullanır.

## <a name="revoke-integration-runtime-sharing-from-a-data-factory"></a>Paylaşımı bir data factory'deki tümleştirme çalışma zamanı iptal et

Bir data factory paylaşılan tümleştirme çalışma zamanı tarafından erişim iptal etmek için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzRoleAssignment `
    -ObjectId $factory.Identity.PrincipalId `
    -RoleDefinitionId 'b24988ac-6180-42a0-ab88-20f7382dd24c' `
    -Scope $SharedIR.Id
```

Mevcut bağlantılı tümleştirme çalışma zamanını kaldırmak için paylaşılan tümleştirme çalışma zamanının karşı aşağıdaki komutu çalıştırın:

```powershell
Remove-AzDataFactoryV2IntegrationRuntime `
    -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $SharedDataFactoryName `
    -Name $SharedIntegrationRuntimeName `
    -Links `
    -LinkedDataFactoryName $LinkedDataFactoryName
```

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [Azure Data factory'deki tümleştirme çalışma zamanı kavramları](https://docs.microsoft.com/azure/data-factory/concepts-integration-runtime).

- Bilgi edinmek için nasıl [Azure portalında bir şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).
