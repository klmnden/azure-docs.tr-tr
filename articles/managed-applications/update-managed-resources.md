---
title: "Azure'daki kaynakları güncelleştirme yönetilen uygulamalar | Microsoft Docs"
description: "Yönetilen kaynaklar üzerinde çalışmak açıklar uygulama yönetilen bir Azure kaynak grubu."
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 59dce2fe7d91cc80f991e5ff298be7757ae19ef4
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Yönetilen kaynaklarla çalışmak için Azure kaynak grubu yönetilen uygulama

Bu makalede, yönetilen bir uygulamaya bir parçası dağıtılan kaynakları güncelleştirmek açıklar. Yönetilen bir uygulamaya yayımcı olarak yönetilen kaynak grubunda kaynaklarına erişimi vardır. Bu kaynakları güncelleştirmek için yönetilen bir uygulama ile ilişkili yönetilen kaynak grup bulunamıyor ve kaynak grubunda kaynağa erişim gerekir.

Bu makalede, yönetilen bir uygulamada dağıttığınız varsayılmaktadır [yönetilen Web uygulaması (Iaas) Azure Yönetim Hizmetleri ile](https://github.com/Azure/azure-managedapp-samples/tree/master/samples/201-managed-web-app) örnek proje. Yönetilen uygulama içeren bir **Standard_D1_v2** sanal makine. Bu yönetilen uygulamayı dağıtmadıysanız, bu makalede bir yönetilen kaynak grubu güncelleştirme adımları hakkında bilgi sahibi olmak için kullanmaya devam edebilirsiniz.

Aşağıdaki resimde, dağıtılan bir yönetilen uygulama gösterir.

![Dağıtılan bir yönetilen uygulama](./media/update-managed-resources/deployed.png)

Bu makalede, Azure CLI kullanın:

* Yönetilen uygulama tanımlayın
* Yönetilen kaynak grubunu tanımlayın
* Yönetilen kaynak grubundaki sanal makine kaynaklar tanımlayın
* VM boyutu (ya da kullanılan değil, küçük bir boyut veya daha fazla yük desteklemek için daha büyük) değiştirme
* İzin verilen konumların belirtir yönetilen kaynak grubu için bir ilke atama

## <a name="get-managed-application-and-managed-resource-group"></a>Yönetilen uygulama ve yönetilen kaynak grubunu Al

Yönetilen uygulamaların bir kaynak grubunda almak için kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

Yönetilen kaynak grubunun kimliği almak için kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Yönetilen kaynak grubunda sanal makineleri yeniden boyutlandırma

Yönetilen kaynak grubundaki sanal makineleri görmek için yönetilen kaynak grubunun adını sağlayın.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

VM boyutlarını güncelleştirmek için kullanın:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

İşlem tamamlandıktan sonra uygulama standart D2 v2 üzerinde çalıştığını doğrulayın.

![Standart D2 v2 kullanarak yönetilen uygulama](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>Yönetilen kaynak grup ilkesini uygula

Yönetilen kaynak grubu ve atama bu kapsamda bir ilke alın. İlke **e56962a6-4747-49cd-b67b-bf8b01975c4c** izin verilen konumların belirtmek için yerleşik bir ilkedir.

```azurecli-interactive
managedGroup=$(az managedapp show --name <app-name> --resource-group DemoApp --query managedResourceGroupId --output tsv)

az policy assignment create --name locationAssignment --policy e56962a6-4747-49cd-b67b-bf8b01975c4c --scope $managedGroup --params '{
                            "listofallowedLocations": {
                                "value": [
                                    "northeurope",
                                    "westeurope"
                                ]
                            }
                        }'
```

İzin verilen konumların görmek için kullanın:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

İlke ataması portalda görüntülenir.

![Görünüm ilke ataması](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [yönetilen uygulama genel bakış](overview.md).
* Örnek projeler için bkz: [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).
* Azure Marketi'nde yayımlama yönetilen uygulamalar hakkında daha fazla bilgi için bkz: [yönetilen Market uygulamalarda](publish-marketplace-app.md).