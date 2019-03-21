---
title: Azure stack'teki IDN'ler anlama | Microsoft Docs
description: IDN'ler özelliklerini ve yeteneklerini Azure Stack anlama
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: scottnap
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: ab867af76821f90c6a87c08d42affdef8192e201
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58258148"
---
# <a name="introducing-idns-for-azure-stack"></a>Azure Stack için iDNS ile tanışın

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

IDN'ler olan dış DNS adlarını çözümlemek sağlayan bir Azure Stack ağ özelliği (örneğin, https:\//www.bing.com.) Ayrıca, dahili sanal ağ adları kaydetmenize olanak sağlar. Bunun yapılması, Vm'leri aynı sanal ağda IP adresi yerine adına göre çözebilirsiniz. Bu yaklaşım, özel DNS sunucusu girdileri sağlama gereksinimini kaldırır. DNS hakkında daha fazla bilgi için bkz. [Azure DNS'ye genel bakış](https://docs.microsoft.com/azure/dns/dns-overview).

## <a name="what-does-idns-do"></a>IDN'ler ne yapar?

Azure stack'teki IDN'ler ile özel DNS sunucusu girdileri belirtmenize gerek kalmadan aşağıdaki özellikleri alın:

- Kiracı İş yükleri için DNS ad çözümleme hizmetleri paylaşılan.
- Ad çözümlemesi ve Kiracı sanal ağ içindeki DNS kaydı için yetkili DNS hizmeti.
- Kiracının sanal makineleri Internet ad çözümlemesi için özyinelemeli DNS hizmeti. Kiracılar artık Internet adlarını (örneğin, www.bing.com.) çözümlemek için DNS girişleri özel belirtmeniz gerekir

Yine de kendi DNS sunucunuzu getirin ve özel DNS sunucuları kullanın. Ancak, IDN'ler kullanarak, Internet DNS adlarını çözemez ve aynı sanal ağdaki diğer Vm'lere bağlanmak, özel DNS girişlerini oluşturmanız gerekmez.

## <a name="what-doesnt-idns-do"></a>IDN'ler değil nedir?

Hangi IDN'ler yapmanıza olanak sağlar olduğundan sanal ağ dışında çözümlenebilir bir adı bir DNS kaydı oluşturun.

Azure'da genel IP adresi ile ilişkili bir DNS ad etiketi belirtme seçeneğiniz vardır. Etiket (ön ek) seçebilirsiniz, ancak Azure genel IP adresini oluşturduğunuz bölgeye göre soneki seçer.

![Bir DNS ad etiketi örneği](media/azure-stack-understanding-dns-in-tp2/image3.png)

Önceki görüntüde gösterildiği gibi Azure bir "A" kaydı DNS bölge altında belirtilen DNS ad etiketi için oluşturacaksınız **westus.cloudapp.azure.com**. Önek ve sonek oluşturmak için birleştirilmiş bir [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN), çözümlenen öğesinden herhangi bir genel Internet'te.

Aşağıdaki işlemleri yapamaz için azure Stack için iç ad kaydı, yalnızca IDN'ler destekler:

- Var olan barındırılan DNS bölgesi (örneğin, local.azurestack.external.) altında bir DNS kaydı oluşturma
- (Örneğin, Contoso.com) bir DNS bölgesi oluşturma
- Kendi özel DNS bölgesi altında bir kayıt oluşturun.
- Etki alanı adları satın destekler.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack'te DNS kullanma](azure-stack-dns.md)
