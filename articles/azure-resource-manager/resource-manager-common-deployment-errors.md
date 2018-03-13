---
title: "Ortak Azure dağıtım hatalarını giderme | Microsoft Docs"
description: "Azure Kaynak Yöneticisi'ni kullanarak Azure kaynakları dağıttığınızda sık karşılaşılan hataların nasıl çözüleceği açıklanmaktadır."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "Dağıtım hatası, azure dağıtım azure'a dağıtma"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 2cf31b32e02923aa573d5586b8ca24bf30b7d97b
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme

Bu makalede karşılaşabilir ve hataları gidermek için bilgi sağlayan bazı yaygın bir Azure dağıtım hatalar açıklanır. Dağıtım hatası için hata kodu bulamazsa, bakın [Bul hata kodu](#find-error-code).

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Risk azaltma | Daha fazla bilgi |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Depolama hesapları için adlandırma kısıtlamaları izleyin. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Kullanılabilir depolama hesabı özellikleri denetleyin. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Küme veya bölgede kullanılabilir kaynak yok veya istenen VM boyutu destekleyemez. İsteği daha sonra yeniden deneyin veya farklı bir VM boyutu isteyin. | [Linux için sağlama ve ayırma sorunları](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) ve [Windows için sağlama ve ayırma sorunları](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| AnotherOperationInProgress | Eşzamanlı işlemin tamamlanmasını bekleyin. | |
| AuthorizationFailed | Hizmet sorumlusu veya hesabınızı dağıtımını tamamlamak için yeterli erişimi yok. Hesabınıza ait olduğu rol ve dağıtım kapsamın erişimini denetleyin. | [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md) |
| BadRequest | Kaynak Yöneticisi tarafından beklenen eşleşmeyen dağıtım değerlerini gönderdi. Sorun giderme konusunda yardım için iç durum iletisini kontrol edin. | [Şablon başvurusu](/azure/templates/) ve [desteklenen konumlar](resource-manager-templates-resources.md#location) |
| Çakışma | Kaynağın geçerli durumunda izin verilmiyor bir işlem istiyor. Örneğin, disk yeniden boyutlandırmaya yalnızca VM oluşturulurken veya VM serbest bırakıldığında izin verilir. | |
| DeploymentActive | Eşzamanlı dağıtım tamamlamak için bu kaynak grubu için bekleyin. | |
| DeploymentFailed | DeploymentFailed hata hatayı çözmek için gereken ayrıntıları sağlamaz genel bir hatadır. Daha fazla bilgi sağlayan bir hata kodu için hata ayrıntıları bakın. | [Hata kodu bulun](#find-error-code) |
| DnsRecordInUse | DNS kayıt adı benzersiz olmalıdır. Farklı bir ad sağlayın ya da varolan bir kaydı değiştirin. | |
| ImageNotFound | VM görüntü ayarlarını kontrol edin. |  |
| InUseSubnetCannotBeDeleted | Bu hata bir kaynak güncelleştirme girişimi sırasında karşılaşabileceğiniz, ancak istek kaynağını oluşturma ve silme ile işlenir. Tüm değişmez değerler belirttiğinizden emin olun. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Uygun bir kiracı için erişim belirteci alın. Belirteç, hesabınıza ait kiracısındaki yalnızca alabilir. | |
| InvalidContentLink | Büyük olasılıkla kullanılamıyor iç içe geçmiş bir şablon bağlantı denediniz. İç içe geçmiş şablon için sağladığınız URI kontrol edin. Şablon bir depolama hesabı varsa, URI erişilebilir olduğundan emin olun. Bir SAS belirteci geçmesi gerekebilir. | [bağlı şablonları](resource-group-linked-templates.md) |
| InvalidParameter | Bir kaynak için sağlanan değerlerden biri, beklenen değerle eşleşmiyor. Bu hata, birçok farklı koşulları neden olabilir. Örneğin, bir parola yetersiz olabilir veya bir blob adı yanlış olabilir. Hangi değerin düzeltilmesi gerektiğini hata iletisine bakın. | |
| InvalidRequestContent | Dağıtım değerlerinizi ya da değil beklenmeyen veya eksik değerler değerleri gereklidir. Kaynak türü için değerleri onaylayın. | [Şablon başvurusu](/azure/templates/) |
| InvalidRequestFormat | Dağıtım yürütürken hata ayıklama günlüğünü etkinleştirin ve istek içeriğini doğrulayın. | [Hata ayıklama günlüğü](#enable-debug-logging) |
| InvalidResourceNamespace | Belirttiğiniz kaynak ad alanı denetleyin **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidResourceReference | Kaynak henüz yok veya yanlış başvuruluyor. Bir bağımlılık eklemeniz gerekip gerekmediğini denetleyin. Doğrulayın kullanımınız **başvuru** işlevi senaryonuz için gerekli parametreler içerir. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
| InvalidResourceType | Belirtilen onay kaynak türünü **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidTemplate | Şablon söz dizimi hataları denetleyin. | [Geçersiz şablon çözümleyin](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Hesabınızı dağıttığınız kaynak grubu olarak aynı kiracıya ait olup olmadığını denetleyin. | |
| LinkedInvalidPropertyId | Kaynak Kimliği bir kaynak için doğru biçimde çözümleyemiyor değil. Abonelik kimliği, kaynak grubu adı, kaynak türü, (gerekirse) üst kaynak adı ve kaynak adı dahil olmak üzere kaynak kimliği için gerekli tüm değerleri sağlayın denetleyin. | |
| LocationRequired | Kaynağınız için bir konum sağlar. | [Konum ayarlama](resource-manager-templates-resources.md#location) |
| MissingRegistrationForLocation | Kaynak sağlayıcı kayıt durumu ve desteklenen konumlardan denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Aboneliğinizi kaynak sağlayıcısı ile kaydedin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Kaynak sağlayıcı kayıt durumunu denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| Bulunamadı | Bağımlı bir kaynak paralel bir üst kaynakla dağıtmak çalışıyor. Bir bağımlılık ekleme gerekip gerekmediğini denetleyin. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
| OperationNotAllowed | Dağıtım abonelik, kaynak grubu veya bölge için kotayı aşan bir işlem çalışıyor. Mümkünse, dağıtımınızı kotaları içinde kalmak için gözden geçirin. Aksi takdirde, kotalar bir değişiklik isteğinde göz önünde bulundurun. | [Kotalar çözümleyin](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Alt kaynakları oluşturmadan önce bir üst kaynak bulunduğundan emin olun. | [Üst kaynak çözümleyin](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | Belirtilen IP adresi, Azure tarafından gerekli bir adres aralığı içerir. Ayrılmış aralık önlemek için IP adresini değiştirin. | [IP adresleri](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Belirtilen IP adresi alt ağ aralığının dışında. Alt ağ aralığında için IP adresini değiştirin. | [IP adresleri](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Dağıtılan bir kaynakta bazı özellikleri değiştirilemez. Bir kaynak güncelleştirirken değişikliklerinizi izin verilen özelliklerini sınırlayın. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Aboneliğiniz dağıtımı sırasında gerçekleştirmeye eylem önleyen bir kaynak ilke içerir. Eylem engeller ilkeyi bulun. Mümkünse, ilkeden sınırlamaları karşılamak üzere, dağıtımınızı değiştirin. | [İlkeleri çözümleyin](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Ayrılmış bir ad içermeyen bir kaynak adı belirtin. | [Ayrılmış kaynak adları](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Tamamlamak silme işlemi için bekleyin. | |
| ResourceGroupNotFound | Dağıtımı için hedef kaynak grubu adını kontrol edin. Önceden, aboneliğinizde var olmalıdır. Abonelik bağlamını kontrol edin. | [Azure CLI](/cli/azure/account?#az_account_set) [PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | Dağıtımınız, çözümlenemeyen bir kaynağa başvuruyor. Doğrulayın kullanımınız **başvuru** işlevi senaryonuz için gerekli olan parametreleri içerir. | [Başvuruları çözümlenemedi](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | Dağıtım kotasına abonelik, kaynak grubu veya bölge için kaynak oluşturmak çalışıyor. Mümkünse, kotalar içinde kalmak için altyapınızı gözden geçirin. Aksi takdirde, kotalar bir değişiklik isteğinde göz önünde bulundurun. | [Kotalar çözümleyin](resource-manager-quota-errors.md) |
| SkuNotAvailable | Seçtiğiniz konum için kullanılabilir olan SKU (örneğin, VM boyutu) seçin. | [SKU çözümleyin](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Depolama hesabı için benzersiz bir ad sağlayın. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Depolama hesabı için benzersiz bir ad sağlayın. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Abonelik, kaynak grubu ve kullanmaya çalıştığınız depolama hesabının adını kontrol edin. | |
| SubnetsNotInSameVnet | Bir sanal makine yalnızca tek bir sanal ağa sahip olabilir. Birden çok NIC dağıtırken aynı sanal ağa ait oldukları emin olun. | [Birden çok NIC](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>Hata kodu bulun

İki tür alabileceğiniz hataları vardır:

* Doğrulama hataları
* Dağıtım hataları

Dağıtım öncesinde belirlenebilir senaryolardan doğrulama hataları ortaya çıkar. Sözdizimi hataları şablonunuzu veya abonelik kotalarında aşacak kaynakları dağıtılmaya çalışılırken içerirler. Dağıtım işlemi sırasında ortaya koşullar uygulamadaki dağıtım hataları kaynaklanır. Paralel olarak dağıtılan bir kaynağa erişmeye içerirler.

Her iki tür hataları dağıtım sorunlarını giderme için kullandığınız bir hata kodunu döndürür. Her iki tür hataları görünür [etkinlik günlüğü](resource-group-audit.md). Ancak, dağıtım hiçbir zaman başlamadı çünkü doğrulama hataları dağıtım geçmişiniz görünmez.

### <a name="validation-errors"></a>Doğrulama hataları

Portalı aracılığıyla dağıtırken değerlerinizi gönderdikten sonra bir doğrulama hatası bakın.

![Portal doğrulama hatasını göster](./media/resource-manager-common-deployment-errors/validation-error.png)

Daha fazla ayrıntı için iletiyi seçin. Aşağıdaki resimde gördüğünüz bir **InvalidTemplateDeployment** hata ve bir ilke belirten bir ileti dağıtım engellendi.

![Doğrulama ayrıntıları göster](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Dağıtım hataları

İşlemi doğrulama başarılı olur, ancak dağıtım sırasında başarısız olduğunda, bildirimler hataya bakın. Bildirim seçin.

![bildirim hatası](./media/resource-manager-common-deployment-errors/notification.png)

Dağıtım hakkında daha fazla ayrıntı bakın. Hata hakkında daha fazla bilgi bulmak için bu seçeneği belirleyin.

![dağıtım başarısız](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Hata iletisi ve hata kodları bakın. İki hata kodları dikkat edin. İlk hata kodu (**DeploymentFailed**) hatayı çözmek için gereken ayrıntıları sağlamaz genel bir hatadır. İkinci hata kodunu (**StorageAccountNotFound**), gereksinim ayrıntıları sağlar. 

![hata ayrıntıları](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Hata ayıklama günlük kaydını etkinleştir

Bazen istek ve yanıt nelerin yanlış gittiğini öğrenmek için hakkında daha fazla bilgi gerekiyor. PowerShell veya Azure CLI kullanarak bir dağıtım sırasında günlüğe ek bilgiler isteyebilir.

- PowerShell

   PowerShell'de ayarlamak **DeploymentDebugLogLevel** tüm parametresi, ResponseContent veya RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Aşağıdaki cmdlet ile içerik isteği inceleyin:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Veya, ile içerik yanıtı:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Bu bilgiler şablondaki değeri yanlış ayarlanmış olup olmadığını belirlemenize yardımcı olabilir.

- Azure CLI

   Aşağıdaki komutla dağıtım işlemlerini inceleyin:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- İç içe geçmiş şablonu

   İç içe geçmiş bir şablon için hata ayıklama bilgileri oturum açmak için kullandığınız **debugSetting** öğesi.

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

Bazı durumlarda, en kolay yolu, şablonunuzu gidermek için bazı bölümleri test etmektir. Oluşturabileceğiniz basitleştirilmiş bir hata olduğunu düşündüğünüz parçası üzerinde odaklanmanıza olanak tanıyan şablon neden olan. Örneğin, bir kaynak başvururken bir hata alıyorsunuz varsayalım. Tüm şablon postalarla yerine, soruna parçası döndürür bir şablon oluşturun. Şablon işlevleri doğru kullanarak sağ parametrelerinde geçtiğiniz ve beklediğiniz kaynak alınıyor belirlemenize yardımcı olabilir.

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

Ya da yanlış bağımlılıkları ayarlamak üzere ilgili düşünüyorsanız dağıtım hataları karşılaşıyoruz varsayalım. Basitleştirilmiş şablonlara bölerek şablonunuzu test edin. İlk olarak, yalnızca tek bir kaynak (bir SQL sunucu gibi) dağıtan bir şablon oluşturun. Bu kaynağı doğru tanımlı sahip olduğunuzdan emin olduğunuzda (SQL veritabanı gibi) bağımlı bir kaynak ekleyin. Doğru tanımlanmadı iki kaynaklarla varsa, bağımlı diğer kaynaklar (örneğin, denetim ilkeleri) ekleyin. Her sınama dağıtımı arasında yeterli bağımlılıkları sınama emin olmak için kaynak grubunu silin.


## <a name="next-steps"></a>Sonraki adımlar
* Eylemler denetleme hakkında bilgi edinmek için [denetim işlemleri Resource Manager ile](resource-group-audit.md).
* Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
