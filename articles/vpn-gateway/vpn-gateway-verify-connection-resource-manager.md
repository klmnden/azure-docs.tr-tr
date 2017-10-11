---
title: "Bir VPN ağ geçidi bağlantısını doğrulama | Microsoft Docs"
description: "Bu makalede, bir sanal ağ VPN ağ geçidi bağlantı doğrulamak gösterilmiştir."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 7e3d1043-caa9-4472-96d3-832f4e2c91ee
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2017
ms.author: cherylmc
ms.openlocfilehash: b2d702ecdd5e1fca342e7c84c6e75339097f0bcd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="verify-a-vpn-gateway-connection"></a>Bir VPN ağ geçidi bağlantısını doğrulama

Bu makale VPN ağ geçidi bağlantısı Klasik ve Resource Manager dağıtım modelleri için doğrulama gösterilmiştir.

## <a name="azure-portal"></a>Azure portalına

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="powershell"></a>PowerShell

PowerShell kullanarak Resource Manager dağıtım modeli için bir VPN ağ geçidi bağlantısı doğrulamak için en son sürümünü yükleyin [Azure Resource Manager PowerShell cmdlet'lerini](/powershell/azure/overview).

[!INCLUDE [PowerShell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="azure-cli"></a>Azure CLI

Azure CLI kullanarak Resource Manager dağıtım modeli için bir VPN ağ geçidi bağlantısı doğrulamak için en son sürümünü yükleyin [CLI komutları](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0 veya üstü).

[!INCLUDE [CLI](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]


## <a name="azure-portal-classic"></a>Azure portal (Klasik)

[!INCLUDE [Azure portal](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]

## <a name="powershell-classic"></a>PowerShell (Klasik)

VPN ağ geçidi bağlantınızı PowerShell kullanarak Klasik dağıtım modeli için doğrulamak için Azure PowerShell cmdlet'lerinin en son sürümlerini yükleyin. İndirmek ve yüklemek mutlaka [Hizmet Yönetimi](https://docs.microsoft.com/powershell/azure/install-azure-ps?view=azuresmps-3.7.0) modülü. Klasik dağıtım modeli için oturum açmak için 'Add-AzureAccount' kullanın.

[!INCLUDE [Classic PowerShell](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağlarınıza sanal makineler ekleyebilirsiniz. Adımlar için bkz. [Sanal Makine Oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).