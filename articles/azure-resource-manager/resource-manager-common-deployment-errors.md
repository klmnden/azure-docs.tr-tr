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
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2ebb469289afc36b08c90ae9839f5bdba41cd90b
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme

Bu makalede karşılaşabilir ve hataları gidermek için bilgi sağlayan bazı yaygın bir Azure dağıtım hatalar açıklanır. Dağıtım hatası için hata kodu bulamazsa, bakın [Bul hata kodu](#find-error-code).

## <a name="error-codes"></a>Hata kodları

| Hata kodu | Risk azaltma | Daha fazla bilgi |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Depolama hesapları için adlandırma kısıtlamaları izleyin. | [Depolama hesabı adı çözümlenemedi](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Kullanılabilir depolama hesabı özellikleri denetleyin. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AnotherOperationInProgress | Eşzamanlı işlemin tamamlanmasını bekleyin. | |
| AuthorizationFailed | Hizmet sorumlusu veya hesabınızı dağıtımını tamamlamak için yeterli erişimi yok. Hesabınıza ait olduğu rol ve dağıtım kapsamın erişimini denetleyin. | [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md) |
| BadRequest | Kaynak Yöneticisi tarafından beklenen eşleşmeyen dağıtım değerlerini gönderdi. Sorun giderme konusunda yardım için iç durum iletisini kontrol edin. | [Şablon başvurusu](/azure/templates/) ve [desteklenen konumlar](resource-manager-template-location.md) |
| Çakışma | Kaynağın geçerli durumunda izin verilmiyor bir işlem istiyor. Örneğin, disk yeniden boyutlandırmaya yalnızca VM oluşturulurken veya VM serbest bırakıldığında izin verilir. | |
| DeploymentActive | Eşzamanlı dağıtım tamamlamak için bu kaynak grubu için bekleyin. | |
| DnsRecordInUse | DNS kayıt adı benzersiz olmalıdır. Farklı bir ad sağlayın ya da varolan bir kaydı değiştirin. | |
| ImageNotFound | VM görüntü ayarlarını kontrol edin. | [Linux resim sorun giderme](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) ve [sorun giderme Windows görüntüleri](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| InUseSubnetCannotBeDeleted | Bu hata bir kaynak güncelleştirme girişimi sırasında karşılaşabileceğiniz, ancak istek kaynağını oluşturma ve silme ile işlenir. Tüm değişmez değerler belirttiğinizden emin olun. | [Güncelleştirme kaynağı](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Uygun bir kiracı için erişim belirteci alın. Belirteç, hesabınıza ait kiracısındaki yalnızca alabilir. | |
| InvalidContentLink | Büyük olasılıkla kullanılamıyor iç içe geçmiş bir şablon bağlantı denediniz. İç içe geçmiş şablon için sağladığınız URI kontrol edin. Şablon bir depolama hesabı varsa, URI erişilebilir olduğundan emin olun. Bir SAS belirteci geçmesi gerekebilir. | [Bağlı şablonları](resource-group-linked-templates.md) |
| InvalidParameter | Bir kaynak için sağlanan değerlerden biri, beklenen değerle eşleşmiyor. Bu hata, birçok farklı koşulları neden olabilir. Örneğin, bir parola yetersiz olabilir veya bir blob adı yanlış olabilir. Hangi değerin düzeltilmesi gerektiğini hata iletisine bakın. | |
| InvalidRequestContent | Dağıtım değerlerinizi ya da değil beklenmeyen veya eksik değerler değerleri gereklidir. Kaynak türü için değerleri onaylayın. | [Şablon başvurusu](/azure/templates/) |
| InvalidRequestFormat | Dağıtım yürütürken hata ayıklama günlüğünü etkinleştirin ve istek içeriğini doğrulayın. | [Hata ayıklama günlüğü](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | Belirttiğiniz kaynak ad alanı denetleyin **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidResourceReference | Kaynak henüz yok veya yanlış başvuruluyor. Bir bağımlılık eklemeniz gerekip gerekmediğini denetleyin. Doğrulayın kullanımınız **başvuru** işlevi senaryonuz için gerekli parametreler içerir. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
| InvalidResourceType | Belirtilen onay kaynak türünü **türü** özelliği. | [Şablon başvurusu](/azure/templates/) |
| InvalidTemplate | Şablon söz dizimi hataları denetleyin. | [Geçersiz şablon çözümleyin](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Hesabınızı dağıttığınız kaynak grubu olarak aynı kiracıya ait olup olmadığını denetleyin. | |
| LinkedInvalidPropertyId | Kaynak Kimliği bir kaynak için doğru biçimde çözümleyemiyor değil. Abonelik kimliği, kaynak grubu adı, kaynak türü, (gerekirse) üst kaynak adı ve kaynak adı dahil olmak üzere kaynak kimliği için gerekli tüm değerleri sağlayın denetleyin. | |
| LocationRequired | Kaynağınız için bir konum sağlar. | [Konum ayarlama](resource-manager-template-location.md) |
| MissingRegistrationForLocation | Kaynak sağlayıcı kayıt durumu ve desteklenen konumlardan denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Aboneliğinizi kaynak sağlayıcısı ile kaydedin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Kaynak sağlayıcı kayıt durumunu denetleyin. | [Kayıt çözümleyin](resource-manager-register-provider-errors.md) |
| notFound | Bağımlı bir kaynak paralel bir üst kaynakla dağıtmak çalışıyor. Bir bağımlılık ekleme gerekip gerekmediğini denetleyin. | [Bağımlılıklarını Çözümle](resource-manager-not-found-errors.md) |
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

Dağıtım sırasında bir hatayla karşılaştığında Resource Manager hata kodu döndürür. Portal, PowerShell veya Azure CLI aracılığıyla hata iletisi görebilirsiniz. Dış hata iletisi, sorun giderme için çok genel olabilir. Hata hakkında ayrıntılı bilgi içeren iç ileti arayın. Daha fazla bilgi için bkz: [hata kodu belirlemek](resource-manager-troubleshoot-tips.md#determine-error-code).

## <a name="next-steps"></a>Sonraki adımlar
* Eylemler denetleme hakkında bilgi edinmek için [denetim işlemleri Resource Manager ile](resource-group-audit.md).
* Dağıtım sırasında hataları belirlemek için Eylemler hakkında bilgi edinmek için [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
