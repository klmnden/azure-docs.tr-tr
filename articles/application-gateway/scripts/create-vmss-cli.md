---
title: Azure CLI Betiği Örneği - Web trafiğini yönetme | Microsoft Docs
description: Azure CLI Betik Örneği - Bir uygulama ağ geçidi ve sanal makine ölçek kümesi ile web trafiğini yönetin.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: b398ff3b5f3e6fd2ea7d6e2544e7399646f4fa5c
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33200799"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Azure CLI kullanarak web trafiğini yönetme

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir uygulama ağ geçidi oluşturur. Ardından uygulama ağ geçidi web trafiğini yönetmek üzere yapılandırılabilir. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az_net) | Sanal ağ oluşturur. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Bir sanal ağda bir alt ağ oluşturur. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/public-ip#az_network_public_ip_create) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/application-gateway#az_application_gateway_create) | Uygulama ağ geçidi oluşturur. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Sanal makine ölçek kümesi oluşturur. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_show) | Uygulama ağ geçidinin genel IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama ağ geçidi CLI betiği örnekleri, [Azure Windows VM belgeleri](../cli-samples.md) içinde bulunabilir.
