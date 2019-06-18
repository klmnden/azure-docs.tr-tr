---
title: Azure'daki kaynakları güncelleştirme yönetilen uygulamalar | Microsoft Docs
description: Yönetilen kaynaklar üzerinde çalışan açıklar yönetilen uygulama için bir Azure kaynak grubu.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 21f4e0aa339eb0c746f9b9b06f8aaada6c4d4b71
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61043467"
---
# <a name="work-with-resources-in-the-managed-resource-group-for-azure-managed-application"></a>Yönetilen kaynakları ile çalışmak için Azure kaynak grubu yönetilen uygulaması

Bu makalede, yönetilen bir uygulamanın bir parçası dağıtılan kaynakları güncelleştirme açıklar. Yönetilen bir uygulamanın yayımcısı yönetilen bir kaynak grubunda kaynaklarına erişimi vardır. Bu kaynakları güncelleştirmek için bir yönetilen uygulamayla ilişkili yönetilen kaynak grubunu bulun ve bu kaynak grubundaki bir kaynağa erişim gerekir.

Bu makalede, yönetilen bir uygulamada dağıttığınız varsayılmaktadır [yönetilen Web uygulaması (Iaas) ile Azure yönetim hizmetlerinin](https://github.com/Azure/azure-managedapp-samples/tree/master/samples/201-managed-web-app) örnek proje. Yönetilen uygulama içerdiğini bir **işler için standart_d1_v2** sanal makine. Bu yönetilen uygulamayı dağıtmadıysanız, bu makalede yönetilen kaynak grubu güncelleştirme adımları hakkında bilgi sahibi olmak için kullanmaya devam edebilirsiniz.

Aşağıdaki görüntüde, dağıtılmış yönetilen uygulamayı gösterir.

![Dağıtılan bir yönetilen uygulama](./media/update-managed-resources/deployed.png)

Bu makalede, Azure CLI'yı kullanın:

* Yönetilen uygulama tanımlayın
* Yönetilen kaynak grubu tanımlayın
* Yönetilen kaynak grubundaki sanal makine kaynakları tanımlayın
* VM boyutu (ya da daha küçük bir boyut kullanılmadı varsa veya daha fazla yükü desteklemeye daha büyük) değiştirme
* İzin verilen konumlar belirten yönetilen kaynak grubu için ilke atama

## <a name="get-managed-application-and-managed-resource-group"></a>Yönetilen uygulama ve yönetilen kaynak grubu Al

Yönetilen uygulamaların bir kaynak grubunda almak için kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')]"
```

Yönetilen kaynak grubu Kimliğini almak için kullanın:

```azurecli-interactive
az managedapp list --query "[?contains(resourceGroup,'DemoApp')].{ managedResourceGroup:managedResourceGroupId }"
```

## <a name="resize-vms-in-managed-resource-group"></a>Yönetilen kaynak grubundaki Vm'leri yeniden boyutlandırma

Yönetilen kaynak grubundaki sanal makineleri görmek için yönetilen kaynak grubunun adını sağlayın.

```azurecli-interactive
az vm list -g DemoApp6zkevchqk7sfq --query "[].{VMName:name,OSType:storageProfile.osDisk.osType,VMSize:hardwareProfile.vmSize}"
```

VM boyutlarını güncelleştirmek için kullanın:

```azurecli-interactive
az vm resize --size Standard_D2_v2 --ids $(az vm list -g DemoApp6zkevchqk7sfq --query "[].id" -o tsv)
```

İşlem tamamlandıktan sonra uygulama standart D2 v2 üzerinde çalıştığı doğrulayın.

![Standart D2 v2 kullanarak yönetilen uygulama](./media/update-managed-resources/upgraded.png)

## <a name="apply-policy-to-managed-resource-group"></a>İlke yönetilen kaynak grubuna uygulayabilirsiniz.

Atama ve yönetilen kaynak grubu bu kapsamda bir ilke alın. İlke **e56962a6-4747-49cd-b67b-bf8b01975c4c** izin verilen konumlar belirtmek için yerleşik bir ilkedir.

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

İzin verilen konumları görüntülemek için kullanın:

```azurecli-interactive
az policy assignment show --name locationAssignment --scope $managedGroup --query parameters.listofallowedLocations.value
```

İlke ataması, portalda görüntülenir.

![Görünüm ilke ataması](./media/update-managed-resources/assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Örnek projeler için bkz: [örnek projeler için Azure yönetilen uygulamalar](sample-projects.md).
