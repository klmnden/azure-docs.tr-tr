---
title: Azure yığınında anlama IDN'ler | Microsoft Docs
description: IDN'ler özellikleri ve yetenekleri Azure yığınında anlama
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: scottnap
ms.openlocfilehash: 9123160f42adea57c28dff265bd5b5dbbcbb7918
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724267"
---
# <a name="introducing-idns-for-azure-stack"></a>IDN'ler için Azure yığınına Tanıtımı

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

IDN'ler özelliğidir dış DNS adlarını çözemez olanak tanıyan bir Azure yığın ağ (örneğin, http://www.bing.com.) de iç sanal ağ adları kaydetmenizi sağlar. Bunun yapılması, sanal makineleri aynı sanal ağda IP adresi yerine adına göre çözebilirsiniz. Bu yaklaşım, özel DNS sunucusu girdileri sağlamak için gereksinimini ortadan kaldırır. DNS hakkında daha fazla bilgi için bkz: [Azure DNS'ye genel bakış](https://docs.microsoft.com/en-us/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>IDN'ler ne yapar?

Azure yığınında IDN'ler ile özel DNS sunucusu girdileri belirtmek zorunda kalmadan aşağıdaki özellikleri alın:

- Kiracı İş yükleri için DNS ad çözümleme hizmetleri paylaşılan.
- Ad çözümlemesi ve Kiracı sanal ağ içinde DNS kaydı için yetkili DNS hizmeti.
- Kiracı VM'ler Internet adlarından çözümlenmesi için yinelemeli DNS hizmeti. Kiracılar artık Internet adları (örneğin, www.bing.com.) çözümlemek için özel DNS girişlerini belirtmeniz gerekir

Hala kendi DNS getirin ve özel DNS sunucuları kullanın. Ancak, IDN'ler kullanarak Internet DNS adlarını çözemez ve diğer Vm'lerle aynı sanal ağda bağlanmak, özel DNS girişlerini oluşturmanız gerekmez.

## <a name="what-doesnt-idns-do"></a>IDN'ler değil nedir?

Hangi IDN'ler yapmanıza olanak sağlar olduğundan sanal ağ dışında çözümlenebilir bir adı için bir DNS kaydı oluşturun.

Azure'da, bir ortak IP adresi ile ilişkili bir DNS ad etiketi belirtme seçeneğiniz vardır. Etiket (önek) seçebilirsiniz, ancak Azure genel IP adresi oluşturma bölgeye göre soneki seçer.

![Bir DNS ad etiketi örneği](media/azure-stack-understanding-dns-in-tp2/image3.png)

Önceki görüntüde gösterildiği gibi Azure bir "A" kaydı DNS bölge altında belirtilen DNS ad etiketi için oluşturacak **westus.cloudapp.azure.com**. Önek ve sonek oluşturmak için birleştirilmiş bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN), çözülmüş gelen herhangi bir yere genel Internet'te.

Aşağıdakileri yapamazsınız şekilde azure yığın IDN'ler dahili ad kaydı için yalnızca destekler:

- Varolan barındırılan bir DNS bölgesi (örneğin, local.azurestack.external.) altında bir DNS kaydı oluşturun
- (Örneğin, Contoso.com.) bir DNS bölgesi oluşturma
- Kendi özel DNS bölgesi altında bir kayıt oluşturun.
- Etki alanı adları satın destekler.

## <a name="next-steps"></a>Sonraki adımlar

[DNS kullanarak Azure yığını](azure-stack-dns.md)
