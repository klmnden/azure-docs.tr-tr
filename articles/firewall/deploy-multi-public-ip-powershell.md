---
title: Azure PowerShell kullanarak birden çok genel IP adresi ile Azure Güvenlik Duvarı'nı dağıtma
description: Bu makalede, Azure PowerShell kullanarak birden çok genel IP adresi ile bir Azure güvenlik duvarı dağıtmayı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 06/15/2019
ms.author: victorh
ms.openlocfilehash: a7dd35212a573fc3e94dadea4365f150122e1b5a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276749"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Azure PowerShell kullanarak birden çok genel IP adresi ile bir Azure Güvenlik Duvarı'nı dağıtma

> [!IMPORTANT]
> Azure güvenlik duvarı ile birden çok genel IP adresleri şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir Azure güvenlik duvarı en fazla 600 genel IP adresi ile dağıtabilirsiniz.

Bu özellik aşağıdaki senaryolar sağlar:

- **Dnat'ı** -standart bağlantı noktası birden fazla arka uç sunucularınızın çevirebilir. Örneğin, iki genel IP adresi varsa, her iki IP adresleri için TCP bağlantı noktası 3389 (RDP) çevirebilir.
- **SNAT** -ek bağlantı noktalarını, giden SNAT bağlantıları, bağlantı noktası tükenmesi SNAT olasılığını azaltmak için kullanılabilir. Şu anda Azure güvenlik duvarı, rastgele bir bağlantı için kullanılacak kaynak genel IP adresi seçer. Tüm aşağı akış filtreleme ağınız üzerinde varsa, tüm genel IP adresleri, güvenlik duvarı ile ilişkili izin vermeniz gerekir.

Aşağıdaki Azure PowerShell örnekleri nasıl ekleyin, kaldırın ve genel IP adresleri Azure için güvenlik duvarını gösterir.

> [!NOTE]
> Çalışan bir Güvenlik Duvarı'na bir genel IP adresi ekleyip, dnat'ı kurallarını kullanarak var olan bir gelen bağlantı 40-120 saniye çalışmayabilir. Bu özellik genel önizlemeye ilişkin bir sınırlama budur.

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Mevcut bir Güvenlik Duvarı'na bir genel IP adresi Ekle

Bu örnekte, genel IP adresini *azFwPublicIp1* güvenlik duvarı bağlı olarak.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Genel bir IP adresi mevcut bir Güvenlik Duvarı'ndan kaldırma

Bu örnekte, genel IP adresini *azFwPublicIp1* Güvenlik Duvarı'ndan ayrılmış olarak.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>Sahip iki veya daha fazla genel IP adreslerini güvenlik duvarı oluşturma

Bu örnek, sanal ağa bağlı bir güvenlik duvarı oluşturur. *vnet* iki genel IP adreslerine sahip.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
