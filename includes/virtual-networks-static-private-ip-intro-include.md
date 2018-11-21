---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 5c1caf87f32ddd827b85263ee634d3e15d821124
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52272696"
---
Iaas sanal makineleri (VM'ler) ve PaaS rolü örnekleri bir sanal ağdaki belirttiğiniz, bağlı oldukları alt ağın dayanarak bir aralıktan otomatik olarak özel bir IP adresi alır. Bunlar kullanımdan kadar bu adres VM'ler ve rol örnekleri tarafından korunur. Bir VM veya rol örneği, PowerShell, Azure CLI veya Azure portalında durdurarak yetkisini alma. Bu gibi durumlarda, VM veya rol örneği yeniden başlatıldıktan sonra kullanılabilir bir IP adresi aynı daha önce sahip olmayabilir Azure altyapısından alırsınız. Konuk işletim sisteminden VM veya rol örneğindeki kapatırsanız, önceki IP adresini tutar.  

Bazı durumlarda, bir statik IP adresi, örneğin, sanal makinenizin çalıştırılacağı DNS veya etki alanı denetleyicisidir sağlamak için bir VM veya rol örneğindeki istersiniz. Statik özel IP adresi ayarlayarak bunu yapabilirsiniz.

