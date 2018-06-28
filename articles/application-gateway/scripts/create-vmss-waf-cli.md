---
title: Azure CLI Betiği Örneği - Web trafiğini kısıtlama | Microsoft Docs
description: Azure CLI Betik Örneği - Bir web uygulaması güvenlik duvarı ve trafiği kısıtlamak için OWASP kurallarını kullanan bir sanal makine ölçek kümesi ile bir uygulama ağ geçidi oluşturun.
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
ms.openlocfilehash: 7b63da2e21735da78d2e6b3c244dc2e80a48aab5
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750264"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Azure CLI kullanarak web trafiğini kısıtlama

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturur. Web uygulaması güvenlik duvarı, OWASP kurallarına göre web trafiğini kısıtlar. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

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
| [az network public-ip create](https://docs.microsoft.com/en-us/cli/azure/network/public-ip?view=azure-cli-latest) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [az network application-gateway create](https://docs.microsoft.com/en-us/cli/azure/network/application-gateway?view=azure-cli-latest) | Uygulama ağ geçidi oluşturur. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Sanal makine ölçek kümesi oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Bir depolama hesabı oluşturur. |
| [az monitor diagnostic-settings create](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) | Bir depolama hesabı oluşturur. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_show) | Uygulama ağ geçidinin genel IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama ağ geçidi CLI betiği örnekleri, [Azure uygulama ağ geçidi belgeleri](../cli-samples.md) içinde bulunabilir.
