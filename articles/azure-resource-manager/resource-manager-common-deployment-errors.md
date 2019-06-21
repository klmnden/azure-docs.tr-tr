---
title: Yaygın Azure dağıtım hatalarını giderme | Microsoft Docs
description: Sık karşılaşılan kaynakları Azure Resource Manager'ı kullanarak Azure'a dağıtırken çözümlemeyi açıklar.
tags: top-support-issue
author: tfitzmac
keywords: Dağıtım hatası, azure dağıtım azure'a dağıtma
ms.service: azure-resource-manager
ms.topic: troubleshooting
ms.date: 02/15/2019
ms.author: tomfitz
ms.openlocfilehash: fea7f77b1f4bcace23ad9164354c4f42e868869f
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206329"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme

Bu makalede bazı genel Azure dağıtım hatalarını açıklar ve hataları çözmek için bilgi sağlar. İçin dağıtım hatası hata kodu bulamazsa, bakın [Bul hata kodu](#find-error-code).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Risk azaltma | Daha fazla bilgi |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Depolama hesapları için adlandırma kısıtlamaları izleyin. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Kullanılabilir depolama hesabı özelliklerini denetleyin. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Küme veya bölge kullanılabilir kaynak yok veya istenen VM boyutu destekleyemez. Daha sonra isteği yeniden deneyin veya farklı bir VM boyutu isteyin. | [Linux için sağlama ve ayırma sorunları](../virtual-machines/linux/troubleshoot-deployment-new-vm.md), [Windows için sağlama ve ayırma sorunları](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) ve [ayırma hatalarını giderme](../virtual-machines/troubleshooting/allocation-failure.md)|
| AnotherOperationInProgress | Eş zamanlı işlemin tamamlanmasını bekleyin. | |
| AuthorizationFailed | Hesabınız veya hizmet sorumlusu dağıtımı tamamlamak için yeterli erişimi yok. Hesabınız için ait rolü ve dağıtım kapsamına erişim kontrol edin.<br><br>Gerekli kaynak sağlayıcısı kayıtlı değilse, bu hatayı alabilirsiniz. | [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)<br><br>[Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| BadRequest | Resource Manager tarafından beklenen eşleşmeyen dağıtım değerler gönderdiğiniz. Sorun giderme konusunda yardım için iç durum iletisini inceleyin. | [Şablon başvurusu](/azure/templates/) ve [desteklenen konumlar](resource-group-authoring-templates.md#resource-location) |
| Çakışma | Kaynağın geçerli durumda izin verilmeyen bir işlem istediği. Örneğin, disk yeniden boyutlandırması yalnızca bir VM oluşturulurken veya VM serbest bırakıldığında izin verilir. | |
| DeploymentActive | Eşzamanlı dağıtım tamamlamak için bu kaynak grubu için bekleyin. | |
| DeploymentFailed | Hatayı çözmek için gereken Ayrıntılar sağlamaz genel bir hata DeploymentFailed hatadır. Daha fazla bilgi sağlayan bir hata kodu için hata ayrıntılarına bakın. | [Hata kodu bulun](#find-error-code) |
| DeploymentQuotaExceeded | Her kaynak grubu 800 dağıtımlarının sınıra ulaştıysanız, artık gerekmeyen geçmişinden dağıtımları silin. İle geçmişinden girişleri silebilirsiniz [az grubu dağıtımı silin](/cli/azure/group/deployment#az-group-deployment-delete) için Azure CLI veya [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) PowerShell'de. Dağıtım geçmişinden giriş silme Dağıt kaynakları etkilemez. | |
| DnsRecordInUse | DNS kaydı adı benzersiz olmalıdır. Farklı bir ad girin veya varolan bir kaydı değiştirin. | |
| ImageNotFound | VM görüntü ayarlarını kontrol edin. |  |
| InUseSubnetCannotBeDeleted | Bir kaynak güncelleştirmeye çalışırken bu hatayı alabilirsiniz, ancak istek kaynak oluşturma ve silme ile işlenir. Tüm değişmez değerler belirttiğinizden emin olun. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Erişim için uygun Kiracı belirteci alın. Hesabınıza ait kiracıda yalnızca belirteç elde edebilirsiniz. | |
| InvalidContentLink | Kullanılamayan iç içe geçmiş bir şablon bağlamak büyük olasılıkla çalıştınız. Çifte denetim iç içe geçmiş şablon için sağlanan URI. Bir depolama hesabında bir şablon yoksa URI'sini erişilebilir olduğundan emin olun. Bir SAS belirteci geçmesi gerekebilir. | [Bağlı şablonlar](resource-group-linked-templates.md) |
| InvalidParameter | Bir kaynak için sağlanan değerlerden biri, beklenen değerle eşleşmiyor. Bu hata birçok farklı koşullar neden olabilir. Örneğin, bir parola yetersiz olabilir veya bir blob adı yanlış olabilir. Hangi değerin düzeltilmesi gereken belirlemek için hata iletisine bakın. | |
| InvalidRequestContent | Gerekli değerler dağıtım değerlerinizi ya da beklenen olmayan veya eksik değerler içerir. Kaynak türüne yönelik değerleri doğrulayın. | [Şablon başvurusu](/azure/templates/) |
| InvalidRequestFormat | Dağıtım yürütülürken hata ayıklama günlüğünü etkinleştirin ve istek içeriğini doğrulayın. | [Hata ayıklama günlüğü](#enable-debug-logging) |
| InvalidResourceNamespace | İçinde belirtilen kaynak ad alanı kontrol **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidResourceReference | Kaynak henüz mevcut değil veya yanlış olarak başvurulur. Bir bağımlılık eklemek gerekli olup olmadığını denetleyin. Doğrulayın kullanımınız **başvuru** işlevi senaryonuz için gerekli parametreleri içerir. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
| InvalidResourceType | Belirtilen onay kaynak türünü **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidSubscriptionRegistrationState | Aboneliğinizin kaynak sağlayıcısı ile kaydedin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| InvalidTemplate | Şablon söz dizimi hatalarını denetleyin. | [Geçersiz şablonunu Çözümle](resource-manager-invalid-template-errors.md) |
| InvalidTemplateCircularDependency | Gereksiz bağımlılıkları kaldırın. | [Döngüsel bağımlılıklar çözümleyin](resource-manager-invalid-template-errors.md#circular-dependency) |
| LinkedAuthorizationFailed | Hesabınız için dağıtıyorsanız kaynak grubu olarak aynı kiracıya ait olup olmadığını denetleyin. | |
| LinkedInvalidPropertyId | Kaynak Kimliği bir kaynak için doğru bir şekilde çözme değil. Abonelik kimliği, kaynak grubu adı, kaynak türü, (gerekirse) üst kaynak adı ve kaynak adı da dahil olmak üzere kaynak kimliği için gerekli tüm değerleri sağlayıp sağlamadığını kontrol edebilirsiniz. | |
| LocationRequired | Kaynağınız için bir konum sağlayın. | [Konum ayarlama](resource-group-authoring-templates.md#resource-location) |
| MismatchingResourceSegments | Kaynak adı ve türü parçaların doğru numarasına sahip olduğundan emin iç içe olun. | [Kaynak segmentleri çözümleyin](resource-manager-invalid-template-errors.md#incorrect-segment-lengths)
| MissingRegistrationForLocation | Kaynak sağlayıcısı kayıt durumu ve desteklenen konumlar denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Aboneliğinizin kaynak sağlayıcısı ile kaydedin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Kaynak sağlayıcısı kayıt durumu denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| NotFound | Üst kaynak ile paralel bir bağımlı kaynak dağıtmaya çalışıyor. Bir bağımlılık eklemeniz gerekip gerekmediğini denetleyin. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Dağıtım aboneliğe, kaynak grubu ya da bölge için kotayı aşan bir işlem çalışıyor. Mümkünse, dağıtımınızı kotaları içinde kalmak için gözden geçirin. Aksi takdirde, bir değişiklik kotanızı isteyen göz önünde bulundurun. | [Kotalar çözümleyin](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Alt kaynakları oluşturmadan önce üst kaynak bulunduğundan emin olun. | [Üst kaynak çözme](resource-manager-parent-resource-errors.md) |
| PasswordTooLong | Çok fazla karakter içeren bir parola seçmiş olabilirsiniz veya parola değerinizi güvenli bir dize için bir parametre olarak geçirerek önce dönüştürdükten. Şablon içeriyorsa bir **güvenli dize** parametresi için bir güvenli dizeye dönüştürün gerekmez. Parola değerini metin olarak belirtin. |  |
| PrivateIPAddressInReservedRange | Belirtilen IP adresi, Azure tarafından gereken bir adres aralığı içerir. Ayrılmış aralıktaki önlemek için IP adresini değiştirin. | [IP adresleri](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Belirtilen IP adresi alt ağ aralığının dışında. IP adresi alt ağ aralığında değiştirin. | [IP adresleri](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Bazı özellikleri, dağıtılan bir kaynakta olacak şekilde değiştirilemiyor. Bir kaynak güncelleştirirken, değişikliklerinizi izin verilen özelliklerini sınırlayın. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Dağıtım sırasında gerçekleştirmeye çalıştığınız eylem engelleyen bir kaynak İlkesi aboneliğinize dahildir. Eylem engelleyen ilke bulun. Mümkünse, ilkeden kısıtlamaları karşılamak için dağıtımınıza değiştirin. | [İlkeleri çözümleyin](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Ayrılmış bir ad içermeyen bir kaynak adı girin. | [Ayrılmış kaynak adları](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Silme işlemini tamamlamak bekleyin. | |
| ResourceGroupNotFound | Dağıtım için hedef kaynak grubu adını kontrol edin. Aboneliğinizde zaten mevcut olmalıdır. Abonelik Bağlamınızı denetleyin. | [Azure CLI](/cli/azure/account?#az-account-set) [PowerShell](/powershell/module/Az.Accounts/Set-AzContext) |
| ResourceNotFound | Dağıtımınız, çözümlenemeyen bir kaynağa başvuruyor. Doğrulayın kullanımınız **başvuru** işlevi senaryonuz için gerekli parametreleri içerir. | [Başvuruları çözümlemek](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Dağıtım için aboneliğe, kaynak grubu ya da bölge kotayı aştığınız kaynakları oluşturmak çalışıyor. Mümkünse, kotalar içinde kalmak için altyapınızı gözden geçirin. Aksi takdirde, bir değişiklik kotanızı isteyen göz önünde bulundurun. | [Kotalar çözümleyin](resource-manager-quota-errors.md) |
| SkuNotAvailable | Seçtiğiniz konum için kullanılabilir SKU (örneğin, VM boyutu) seçin. | [SKU çözümleyin](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Depolama hesabına benzersiz bir ad verin. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Depolama hesabına benzersiz bir ad verin. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Abonelik, kaynak grubu ve kullanmaya çalıştığınız depolama hesabının adını kontrol edin. | |
| SubnetsNotInSameVnet | Bir sanal makine yalnızca bir sanal ağ olabilir. Birden çok NIC dağıtırken, aynı sanal ağa ait oldukları emin olun. | [Birden çok NIC](../virtual-machines/windows/multiple-nics.md) |
| TemplateResourceCircularDependency | Gereksiz bağımlılıkları kaldırın. | [Döngüsel bağımlılıklar çözümleyin](resource-manager-invalid-template-errors.md#circular-dependency) |
| TooManyTargetResourceGroups | Kaynak grupları tek bir dağıtım için sayısını azaltın. | [Çapraz kaynak grubu dağıtımı](resource-manager-cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Hata kodu bulun

İki tür hata alabilirsiniz:

* doğrulama hataları
* dağıtım hataları

Doğrulama hataları dağıtım öncesinde saptanabilen senaryolardan kaynaklanır. Bunlar şablonunuzdaki söz dizimi hataları veya abonelik kotalarınızı aşabilecek kaynak dağıtımı denemeleri olabilir. Dağıtım hataları, dağıtım işlemi sırasında oluşan koşullardan kaynaklanır. Bu paralel olarak dağıtılan bir kaynağa erişme denemesi olabilir.

Her iki tür hata da dağıtım sorunlarını gidermek için kullanabileceğiniz bir hata kodu döndürür. Her iki tür hata da [etkinlik günlüğünde](resource-group-audit.md) görüntülenir. Öte yandan doğrulama hataları dağıtım geçmişinizde görüntülenmez çünkü dağıtım hiç başlatılmamıştır.

### <a name="validation-errors"></a>Doğrulama hataları

Portal üzerinden dağıtım yaparken değerlerinizi gönderdikten sonra doğrulama hatasını görürsünüz.

![Portal doğrulama hatası Göster](./media/resource-manager-common-deployment-errors/validation-error.png)

Ayrıntıları görmek için hatayı seçin. Aşağıdaki görüntüde gördüğünüz bir **InvalidTemplateDeployment** hata ve bir ilke belirten bir ileti dağıtım engellendi.

![Doğrulama ayrıntıları göster](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Dağıtım hataları

İşlem doğrulamayı geçer ama dağıtım sırasında başarısız olursa dağıtım hatası alırsınız.

PowerShell'le dağıtım hata kodlarını ve iletileri görmek için şunu kullanın:

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName exampledeployment -ResourceGroupName examplegroup).Properties.statusMessage
```

Azure CLI ile dağıtım hata kodlarını ve iletileri görmek için şunu kullanın:

```azurecli-interactive
az group deployment operation list --name exampledeployment -g examplegroup --query "[*].properties.statusMessage"
```

Portalda bildirimi seçin.

![bildirim hatası](./media/resource-manager-common-deployment-errors/notification.png)

Dağıtım hakkında daha fazla ayrıntı görürsünüz. Hata hakkında daha fazla bilgi bulmak için seçeneği belirtin.

![dağıtım başarısız oldu](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Hata iletisini ve hata kodlarını görürsünüz. İki hata kodu olduğuna dikkat edin. İlk hata kodu (**DeploymentFailed**), hataya çözmek için ihtiyacınız olan ayrıntıları sağlamayan genel bir hatadır. İkinci hata kodu (**StorageAccountNotFound**) ihtiyacınız olan ayrıntıları sağlar. 

![Hata ayrıntıları](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Hata ayıklama günlük kaydını etkinleştirme

Bazen istek ve yanıt neyin yanlış gittiğini öğrenin hakkında daha fazla bilgi gerekiyor. Dağıtım sırasında dağıtım sırasında günlüğe ek bilgiler isteyebilir. 

### <a name="powershell"></a>PowerShell

PowerShell'de ayarlamak **DeploymentDebugLogLevel** tüm parametre, obsah ResponseContent veya RequestContent.

```powershell
New-AzResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName examplegroup `
  -TemplateFile c:\Azure\Templates\storage.json `
  -DeploymentDebugLogLevel All
```

Aşağıdaki cmdlet ile içerik isteği inceleyin:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.request `
| ConvertTo-Json
```

Veya yanıt içeriği:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.response `
| ConvertTo-Json
```

Bu bilgiler, şablonda bir değeri yanlış ayarlanmış olup olmadığını belirlemenize yardımcı olabilir.

### <a name="azure-cli"></a>Azure CLI

Şu anda, Azure CLI, hata ayıklama günlüğünü açma desteklemez, ancak hata ayıklama günlüğünü alabilirsiniz.

Aşağıdaki komut ile dağıtım işlemlerini inceleyin:

```azurecli
az group deployment operation list \
  --resource-group examplegroup \
  --name exampledeployment
```

Aşağıdaki komutla içerik isteği inceleyin:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.request
```

Aşağıdaki komutla içerik yanıt inceleyin:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.response
```

### <a name="nested-template"></a>İç içe geçmiş şablon

İç içe geçmiş şablon için hata ayıklama bilgileri açmak için kullandığınız **debugSetting** öğesi.

```json
{
    "apiVersion": "2016-09-01",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "properties": {
        "mode": "Incremental",
        "templateLink": {
            "uri": "{template-uri}",
            "contentVersion": "1.0.0.0"
        },
        "debugSetting": {
           "detailLevel": "requestContent, responseContent"
        }
    }
}
```

## <a name="create-a-troubleshooting-template"></a>Sorun giderme şablonu oluşturma

Bazı durumlarda, en kolay yolu, şablonunuzu sorun giderme bölümlerini sınamaktır. Oluşturabileceğiniz basitleştirilmiş bir hata olduğuna inanıyorsanız parçası üzerinde odaklanmanıza olanak sağlayan şablon neden oluyor. Örneğin, bir kaynağa başvurma sırasında bir hata alıyorsunuz varsayalım. Tüm şablonu ile ilgilenen yerine, soruna bölümü döndüren bir şablon oluşturun. Şablon işlevleri doğru bir şekilde kullanarak doğru parametreleri geçirme ve beklediğiniz kaynak alma belirlemenize yardımcı olabilir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Ya da yanlışlıkla bağımlılıkları ayarlama ile ilgili olduğunu düşündüğünüz dağıtım hataları karşılaşılıyorsa varsayalım. Basitleştirilmiş şablonlara bölerek şablonunuzu test edin. İlk olarak yalnızca tek bir kaynak (örneğin, bir SQL Server) dağıtan bir şablon oluşturun. Bu kaynağı doğru tanımlı sahip olduğunuzdan emin olduğunuzda (SQL veritabanı gibi) bağlı bir kaynak ekleyin. Doğru kaynaklarla iki varsa, diğer bağımlı kaynaklar (örneğin, denetim ilkeleri) ekleyin. Her sınama dağıtımı arasında bağımlılıkları yeterince test emin olmak için kaynak grubunu silin.


## <a name="next-steps"></a>Sonraki adımlar

* Bir sorun giderme öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablon dağıtımları sorunlarını giderme](./resource-manager-tutorial-troubleshoot.md)
* Eylemler denetleme hakkında bilgi edinmek için [Resource Manager denetim işlemleri](resource-group-audit.md).
* Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md).
