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
ms.openlocfilehash: d3c955d7be0e7e6d45751c0e685bad498e524d94
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
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

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Örnek projeler için bkz: [Azure örnek projelerine yönetilen uygulamaları](sample-projects.md).
