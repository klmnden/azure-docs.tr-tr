---
title: Bir VPN Gateway bağlantısını doğrulama | Microsoft Docs
description: Bu makalede bir sanal ağ VPN ağ geçidi bağlantısını doğrulama gösterilmektedir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: 037c1c7dd73f668bd8ad95568743b223b1e11c79
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38706006"
---
# <a name="verify-a-vpn-gateway-connection"></a>Bir VPN Gateway bağlantısını doğrulama

Bu makalede, hem Klasik hem de Resource Manager dağıtım modeli için bir VPN ağ geçidi bağlantısını doğrulama işlemini göstermektedir.

## <a name="azure-portal"></a>Azure portalına

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

PowerShell kullanarak Resource Manager dağıtım modeli için bir VPN ağ geçidi bağlantısını doğrulama için en son sürümünü yükleyin. [Azure Resource Manager PowerShell cmdlet'lerinin](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak Resource Manager dağıtım modeli için bir VPN ağ geçidi bağlantısını doğrulama için en son sürümünü yükleyin. [CLI komutları](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 veya üzeri).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure portalını (Klasik)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (Klasik)

PowerShell kullanarak Klasik dağıtım modeli için VPN ağ geçidi bağlantınızı doğrulamak için Azure PowerShell cmdlet'lerinin en son sürümlerini yükleyin. İndirme ve yükleme yapılmasını [Hizmet Yönetimi](https://docs.microsoft.com/en-us/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets) modülü. Klasik dağıtım modeli için oturum açma 'Add-AzureAccount' kullanın.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).