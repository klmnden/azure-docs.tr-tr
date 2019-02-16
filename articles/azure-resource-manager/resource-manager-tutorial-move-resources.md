---
title: Azure kaynakları için başka bir kaynak grubu taşıma | Microsoft Docs
description: Azure kaynaklarını bir kaynak grubu başka bir kaynak grubuna veya bir abonelik başka bir aboneliğe taşımayı öğreneceksiniz.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/19/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: cf1894a218af35459e0d0dc432c5813169856cca
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267709"
---
# <a name="tutorial-move-azure-resources-to-another-resource-group-or-subscription"></a>Öğretici: Azure kaynakları başka bir kaynak grubuna veya aboneliğe taşıma

Azure kaynaklarını bir kaynak grubundan başka bir kaynak grubuna taşımayı öğreneceksiniz. Ayrıca, Azure kaynaklarını bir Azure aboneliğine ait başka bir Azure aboneliğine taşıyabilirsiniz. Bu öğreticide, iki kaynak grubu ve bir depolama hesabı dağıtmak için bir resource manager şablonu kullanın. Ardından, depolama hesabını bir kaynak grubundan diğerine taşıyın.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Kaynakları hazırlayın.
> * Kaynak taşınabilir doğrulayın.
> * Kaynak taşımadan önce Yapılacaklar listesi.
> * Taşıma işlemini doğrulayın.
> * Kaynak taşıma.
> * Kaynakları temizleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prepare-the-resources"></a>Kaynakları hazırla

Bir şablon oluşturulmuş ve için yerleştirilmiş bir [paylaşılan depolama hesabı](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json). İki kaynak grubu ve bir depolama hesabı için şablonu tanımlar. Şablonu dağıtırken, bir proje adı sağlamanız gerekir. Proje adı benzersiz kaynak adları oluşturmak için kullanılır.  Aşağıdaki JSON şablonu ayıklanır:

```json
"variables": {
  "resourceGroupSource": {
    "name": "[concat(parameters('projectName'), 'rg1')]",
    "location": "eastus"
  },
  "resourceGroupDestination": {
    "name": "[concat(parameters('projectName'), 'rg2')]",
    "location": "westus"      
  },
  "storageAccount": {
    "name": "[concat(parameters('projectName'), 'store')]",
    "location": "eastus"
  }
},
```

Json'da tanımlanan konumları dikkat edin, Doğu ABD ve Batı ABD iki kaynak gruplarını bulunur. Depolama hesabı, Doğu ABD bölgesinde bulunur. Bir kaynağı farklı bir konum ile başka bir kaynak grubuna taşıdığınızda, taşıma işlemi kaynak konumunu değiştirmez.

Seçin **deneyin** Cloud Shell'i açın ve sonra Cloud shell içinde PowerShell betiğini çalıştırın:

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
New-AzDeployment `
    -Name $projectname `
    -Location "centralus" `
    -TemplateUri "https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json" `
    -projectName $projectName
```

Betik başarıyla tamamlanana kadar bekleyin ve ardından açın [Azure portalında](https://portal.azure.com)ve kaynak grupları ve depolama hesabını beklendiği gibi dağıtılır doğrulayın.

> [!NOTE]
> Bu dağıtım, şablon iki kaynak grubu tanımladığından, abonelik düzeyinde dağıtım olarak kabul edilir. Portal şablon dağıtımı, abonelik düzeyinde dağıtımları desteklemiyor. Bu nedenle Azure PowerShell, bu öğreticide kullanılır. Azure CLI, abonelik düzeyinde dağıtımları da destekler. Bkz: [oluşturma kaynak grubu ve bir Azure aboneliği için kaynak](./deploy-to-subscription.md).

## <a name="verify-the-resource-can-be-moved"></a>Kaynak taşınabilir doğrulayın

Tüm kaynaklar taşınabilir. Bu öğreticide kullanılan resource taşınabilir depolama hesabıdır. Bir kaynak taşınıp taşınamayacağını doğrulamak için bkz: [taşınabilir Hizmetleri](./resource-group-move-resources.md#services-that-can-be-moved).

## <a name="checklist-before-moving-resources"></a>Kaynakları taşımadan önce Yapılacaklar listesi

Bu adım, bunu bitti olarak Bu öğretici için isteğe bağlıdır.

Bir kaynağı taşımadan önce yapmanız gereken bazı önemli adımlar vardır. Bkz: [kaynakları taşımadan önce Yapılacaklar listesi](./resource-group-move-resources.md#checklist-before-moving-resources).

## <a name="validate-the-move"></a>Taşıma doğrula

Taşıma doğrulama, bitti olarak Bu öğretici için isteğe bağlıdır.

Doğrulama taşıma işlemi kaynakları taşımadan taşıma senaryonuza sınamanızı sağlar. Bu işlem, taşıma başarılı olur belirlemek için kullanın. Daha fazla bilgi için [taşıma doğrulama](./resource-group-move-resources.md#validate-move).

## <a name="move-the-resource"></a>Kaynak taşıma

Depolama hesabının kaynak kaynak grubu (rg1) içinde olduğundan, aşağıdaki PowerShell komutunu çalıştırın betik taşıma kaynak hedef kaynak grubu (rg2). Aynı proje adı, kaynakları dağıtırken kullanılan kullandığınızdan emin olun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
$resourceGroupSource = $projectName + "rg1"
$resourceGroupDestination = $projectName + "rg2"
$storageAccountName = $projectName + "store"

$storageAccount = Get-AzResource -ResourceGroupName $resourceGroupSource -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $resourceGroupDestination -ResourceId $storageAccount.ResourceId
```

Açık [Azure portalında](https://portal.azure.com), depolama hesabı başka bir kaynak grubuna taşındı ve ayrıca depolama hesabı konumu Doğu ABD hala olduğunu doğrulayın.

Kaynakları taşırken, hem kaynak grubunun hem de hedef grubu işlem sırasında kilitlenir. Yazma ve silme işlemleri taşıma işlemi tamamlanana kadar kaynak gruplarında engellenir. Bu kilit ekleyemez, güncelleştirme veya kaynak gruplarındaki kaynakları silin, ancak kaynakları dondurulmuş gelmez anlamına gelir. Örneğin, bir SQL Server ve veritabanı yeni bir kaynak grubuna taşırsanız, veritabanı kullanan bir uygulama kapalı kalma süresi olmadan karşılaşır. Bunu hala okuyabilir ve veritabanına yazma.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak kaynak grubu adı seçin.  
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.
5. Hedef kaynak grubu adı seçin.  
6. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir depolama hesabı, bir kaynak grubundan başka bir kaynak grubuna taşımak öğrendiniz. Şu ana kadar bir depolama hesabı veya birden çok depolama hesabı örnekleri ile ilgili. Bir sonraki öğreticide, birden fazla kaynağa ve birden çok kaynak türüne sahip bir şablon geliştireceksiniz. Bazı kaynakların bağımlı kaynakları vardır.

> [!div class="nextstepaction"]
> [Bağımlı kaynaklar oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
