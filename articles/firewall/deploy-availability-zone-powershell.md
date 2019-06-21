---
title: Azure güvenlik duvarı Azure PowerShell kullanarak kullanılabilirlik alanları ile dağıtma
description: Bu makalede, bir Azure güvenlik duvarı Azure PowerShell ile kullanılabilirlik alanları ile dağıtmayı öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 06/20/2019
ms.author: victorh
ms.openlocfilehash: 2fa6a62a28a1536da83994cb07b7c5fa5d7bda9f
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276918"
---
# <a name="deploy-an-azure-firewall-with-availability-zones-using-azure-powershell"></a>Bir Azure güvenlik duvarı Azure PowerShell kullanarak kullanılabilirlik alanları ile dağıtma

> [!IMPORTANT]
> Kullanılabilirlik alanları ile Azure Güvenlik Duvarı şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure güvenlik duvarı, yüksek kullanılabilirlik için birden fazla kullanılabilirlik yaymasına izin dağıtımı sırasında yapılandırılabilir.

Bu özellik aşağıdaki senaryolar sağlar:

- % 99,99 çalışma süresi kullanılabilirliğini artırabilirsiniz. Daha fazla bilgi için bkz: Azure Güvenlik Duvarı [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). İki veya daha fazla kullanılabilirlik seçildiğinde % 99,99 çalışma süresi SLA sunulur.
- Azure güvenlik duvarı yalnızca yakınlığı nedeniyle için belirli bir bölgesine hizmeti standart % 99,95 oranında SLA kullanarak da ilişkilendirebilirsiniz.

Azure güvenlik duvarı kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Azure güvenlik duvarı nedir?](overview.md)

Aşağıdaki Azure PowerShell örneği, kullanılabilirlik alanları ile bir Azure güvenlik duvarının nasıl dağıtılacağı gösterilmektedir.

## <a name="create-a-firewall-with-availability-zones"></a>Kullanılabilirlik alanları ile güvenlik duvarı oluşturma

Bu örnek, bölge 1, 2 ve 3 bir güvenlik duvarı oluşturur.

Belirli bir bölge yok, standart genel IP adresinin oluşturulduğu sırada belirtilir. Bu, varsayılan olarak bölgesel olarak yedekli bir IP adresi oluşturur. Standart genel IP adresleri, tek bir bölge veya tüm bölgeler yapılandırılabilir.

2 bölgesinde bir Güvenlik Duvarı'nda Bölge 1 ve bir IP adresi bulunamayacağından bilmesi önemlidir. Ancak, yakınlık amacıyla aynı tek bölgesinde bir Güvenlik Duvarı'nda Bölge 1 ve tüm bölgelerde IP adresi veya bir güvenlik duvarı ve bir IP adresi olabilir.

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

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1)
  -Zone 1,2,3
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure güvenlik duvarı günlüklerini izleyin](./tutorial-diagnostics.md)
