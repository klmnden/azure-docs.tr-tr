---
title: "PowerShell ile Azure çözümleri yönetmek | Microsoft Docs"
description: "Azure PowerShell ve Resource Manager kaynaklarınızı yönetmek için kullanın."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 10/06/2017
ms.author: tomfitz
ms.openlocfilehash: ae5ccb83a0088cb7c9668f18620b74f9f3f1e9b0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Azure PowerShell ve Resource Manager ile kaynakları yönetme

Bu makalede, Azure PowerShell ve Azure Resource Manager çözümlerinizi yönetmek öğrenin. Resource Manager ile bilmiyorsanız bkz [Resource Manager'a genel bakış](resource-group-overview.md). Bu makalede, yönetim görevlerine odaklanır. Yapacaklarınız:

1. Kaynak grubu oluşturma
2. Kaynak kaynak grubuna ekleyin
3. Bir etiket için kaynak ekleme
4. Adlarını veya etiket değerlerine göre kaynaklarını sorgulama
5. Uygulama ve kaynak üzerinde bir kilit kaldırın
6. Bir kaynak grubu Sil

Bu makalede, Resource Manager şablonu aboneliğinize dağıtma göstermez. Bu bilgi için bkz: [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Azure PowerShell ile çalışmaya başlama

Azure PowerShell yüklü değilse bkz [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

Azure PowerShell geçmişte yüklü, ancak bu kısa süre önce güncelleştirdiniz değil, en son sürümünü yüklemeyi göz önünde bulundurun. Sürümü yüklemek için kullanılan aynı yöntemi güncelleştirebilirsiniz. Örneğin, Web Platformu yükleyicisi kullandıysanız, yeniden başlatın ve bir güncelleştirme için bakın.

Azure kaynakları modülü sürümünüz denetlemek için aşağıdaki cmdlet'i kullanın:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Bu makale, 3.3.0 sürümü için güncelleştirilmiştir. Önceki bir sürümü varsa, deneyiminiz bu makalede gösterilen adımlar eşleşmeyebilir. Bu sürümde cmdlet'ler hakkında daha fazla belgeler için bkz: [AzureRM.Resources Modülü](/powershell/module/azurerm.resources).

## <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açın
Çözümünüzü üzerinde çalışmaya başlamadan önce hesabınızda oturum açmalısınız.

Azure hesabınızda oturum açmak için kullandığınız **Login-AzureRmAccount** cmdlet'i.

```powershell
Login-AzureRmAccount
```

Bu cmdlet, Azure hesabınıza ilişkin oturum açma kimlik bilgilerinizi girmenizi ister. Oturum açtıktan sonra, Azure PowerShell'de kullanabilmeniz için hesap ayarlarınızı indirir.

Cmdlet hesabınızı ve görevler için kullanılacak aboneliği hakkında bilgi döndürür.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Birden fazla aboneliğiniz varsa, farklı bir aboneliğe geçebilirsiniz. İlk olarak, hesabınız için tüm abonelikleri görelim.

```powershell
Get-AzureRmSubscription
```

Etkin ve devre dışı abonelikler döndürür.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

Farklı bir aboneliğe geçiş yapmak için abonelik ile ad **Set-AzureRmContext** cmdlet'i.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Herhangi bir kaynağa aboneliğinize dağıtmadan önce kaynakları içeren bir kaynak grubu oluşturmanız gerekir.

Bir kaynak grubu oluşturmak için kullanın **New-AzureRmResourceGroup** cmdlet'i. Komut kullanır **adı** parametresini kullanarak kaynak grubu için bir ad belirtin ve **konumu** konumunu belirtmek için parametre.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

Çıktı aşağıdaki biçimdedir:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Kaynak grubu daha sonra alması gerekiyorsa, aşağıdaki cmdlet'i kullanın:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

Tüm kaynak grupları, aboneliğinizde almak için bir ad belirtmeyin:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-to-a-resource-group"></a>Kaynakları bir kaynak grubuna Ekle

Bir kaynak kaynak grubuna eklemek için kullanabileceğiniz **yeni AzureRmResource** cmdlet veya kaynak türü için belirli bir cmdlet oluşturmakta (gibi **New-AzureRmStorageAccount**). Yeni kaynak için gerekli olan özellikler için parametreler içerdiğinden, bir kaynak türü için belirli bir cmdlet'i kullanması daha kolay. Kullanılacak **yeni AzureRmResource**, bunlar için istenmeden ayarlanacak tüm özellikler bilmeniz gerekir.

Ancak, yeni kaynak bir Resource Manager şablonu mevcut olmadığından kaynak cmdlet'leri aracılığıyla ekleme gelecekteki karışıklığa neden olabilir. Microsoft Azure, çözümünüz için altyapıyı Resource Manager şablonunda tanımlama önerir. Şablonlar, güvenilir ve art arda çözümünüzü dağıtmak etkinleştirir. Bu makale için bir PowerShell cmdlet'i ile depolama hesabı oluşturma, ancak daha sonra kaynak grubundan bir şablon oluştur.

Aşağıdaki cmdlet'i bir depolama hesabı oluşturur. Örnekte gösterilen adı kullanmak yerine, depolama hesabı için benzersiz bir ad sağlayın. Ad uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca rakamlar ve küçük harfler kullanın. Örnekte gösterilen adı kullanırsanız, bu ad zaten kullanımda olduğundan, bir hata alırsınız.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Daha sonra bu kaynak alması gerekiyorsa, aşağıdaki cmdlet'i kullanın:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Etiket ekleme

Etiketler kaynaklarınızı farklı özelliklere göre düzenlemek etkinleştirin. Örneğin, bazı kaynaklar aynı departmana ait farklı kaynak gruplarında olabilir. Aynı kategoriye ait olarak işaretlemek için bu kaynakları departmanı etiketini ve değer uygulayabilirsiniz. Veya, bir kaynak bir üretim veya test ortamında kullanılıp kullanılmadığını işaretleyebilirsiniz. Bu makalede, yalnızca bir kaynak etiketleri geçerli, ancak ortamınızda büyük olasılıkla tüm kaynaklarınıza etiketler uygulamak için anlamlıdır.

Aşağıdaki cmdlet'i iki etiketleri depolama hesabınıza uygular:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Etiketler tek bir nesne olarak güncelleştirilir. Etiketleri içeren bir kaynak için bir etiket eklemek için önce varolan etiketleri alın. Yeni etiket varolan etiketleri içeren nesneyi eklemek ve tüm etiketleri kaynağa yeniden uygulayın.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Kaynakları Ara

Kullanım **Bul AzureRmResource** farklı arama koşulu için kaynakları almak üzere.

* Bir kaynak tarafından adını almak için sağlamanız **ResourceNameContains** parametre:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* Tüm kaynaklar bir kaynak grubunda almak için sağlayın **ResourceGroupNameContains** parametre:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* Bir etiket adı ve değeri içeren tüm kaynakları almak için sağlayan **TagName** ve **TagValue** Parametreler:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* Belirli bir kaynak türü ile tüm kaynaklar sağlar **ResourceType** parametre:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="get-resource-id"></a>Kaynak Kimliği alma

Komutların çoğu kaynak kimliği, parametre olarak alın. Bir kaynak ve bir değişkeni deposunda Kimliği almak için kullanın:

```powershell
$webappID = (Get-AzureRmResource -ResourceGroupName exampleGroup -ResourceName exampleSite).ResourceId
```

## <a name="lock-a-resource"></a>Bir kaynak Kilitle

Kritik bir kaynak yanlışlıkla silinmiş veya değiştirilmiş emin olmak gerektiğinde bir kilidi kaynağı uygulayın. Ya da belirtebilirsiniz bir **CanNotDelete** veya **salt okunur**.

Oluşturmak veya yönetim kilitleri silmek için erişimi olmalıdır `Microsoft.Authorization/*` veya `Microsoft.Authorization/locks/*` eylemler. Yerleşik roller bu eylemleri yalnızca sahip ve kullanıcı erişimi Yöneticisi verilir.

Kilit uygulamak için aşağıdaki cmdlet'i kullanın:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Önceki örnekte kilitli kaynak kilit kaldırılana kadar silinemez. Kilidi kaldırmak için kullanın:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Ayar kilitleri hakkında daha fazla bilgi için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Kaynakları veya kaynak grubunu Kaldır
Bir kaynak veya kaynak grubu kaldırabilirsiniz. Bir kaynak grubu kaldırdığınızda, bu kaynak grubu içindeki tüm kaynaklar da kaldırın.

* Bir kaynak kaynak grubundan silmek için kullanın **Kaldır AzureRmResource** cmdlet'i. Bu cmdlet kaynağı siler, ancak kaynak grubu silmez.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* Bir kaynak grubu ve tüm kaynaklarını silmek için kullanın **Remove-AzureRmResourceGroup** cmdlet'i.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

Her iki cmdlet'leri için kaynak veya kaynak grubunu kaldırmak istediğinizden onaylamanız istenir. İşlem başarıyla kaynağı veya kaynak grubunu silip silmediğini, döndürür **doğru**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Azure Automation ile Kaynak Yöneticisi komut dosyalarını çalıştır

Bu makale Azure PowerShell ile kaynaklarınızı temel işlemleri gerçekleştirmek nasıl gösterir. Daha gelişmiş yönetim senaryoları için tipik olarak bir komut dosyası oluşturabilir ve bu komut dosyası gerektiğinde veya bir zamanlamaya göre yeniden istediğiniz. [Azure Otomasyonu](../automation/automation-intro.md) sık otomatikleştirmek bir yol kullanılan Azure çözümleri yönetmek komut dosyaları sağlar.

Aşağıdaki konular Azure Otomasyonu, Resource Manager ve PowerShell etkili bir şekilde yönetim görevlerini gerçekleştirmek için nasıl kullanılacağını gösterir:

- Bir runbook oluşturma hakkında daha fazla bilgi için bkz: [ilk PowerShell runbook'um](../automation/automation-first-runbook-textual-powershell.md).
- Komut dosyaları galerileri ile çalışma hakkında daha fazla bilgi için bkz [Azure Otomasyonu Runbook ve modül galerileri](../automation/automation-runbook-gallery.md).
- Başlatma ve durdurma sanal makineleri runbook'lar için bkz: [Azure otomasyonu senaryosu: Azure VM başlatma ve kapatma için bir zamanlama oluşturmak için etiketler kullanarak JSON biçimli](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Başlatma ve durdurma sanal makineleri yoğun olmayan saatlerde runbook'lar için bkz: [Başlat/Durdur VM'ler Otomasyon yoğun olmayan saatlerde çözümde sırasında](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablonları dağıtma hakkında bilgi edinmek için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).
* Yeni bir kaynak grubu mevcut kaynakları taşıyabilirsiniz. Örnekler için bkz: [yeni kaynak grubuna veya aboneliğe taşıma kaynaklara](resource-group-move-resources.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

