---
title: Azure DNS özel bölgeler için senaryoları | Microsoft Docs
description: Azure DNS özel bölgeleri kullanmak için genel senaryolar genel bakış.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: de543913d4f8264fa8e5b3bca0c510c99c479cae
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32771880"
---
# <a name="azure-dns-private-zones-scenarios"></a>Azure DNS özel bölgeler senaryoları
Azure DNS özel bölgeler bir sanal ağ içinde de sanal ağlar arasında ad çözümlemesi sağlar. Bu makalede, bu özellik kullanılarak gerçekleştirilen bazı yaygın senaryolar arayın. 

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Senaryo: tek bir sanal ağa kapsamlı ad çözümlemesi
Bu senaryoda, sanal makineleri (VM'ler) dahil olmak üzere Azure kaynak sayısı, içerdiği Azure sanal ağında sahip. Belirli bir etki alanı adı (DNS bölgesi) aracılığıyla sanal ağ içinde kaynaklardan çözümlemek istediğiniz ve özel ve internet'ten erişilebilir olması için ad çözümlemesi gerekir. Ayrıca, sanal ağ içindeki VM'ler için DNS bölgesine bunları otomatik olarak kaydedilecek Azure gerekir. 

Bu senaryo, aşağıda belirtilmiştir. "A" adlı sanal ağ (VNETA VM1 ve VNETA VM2) iki VM içerir. Bunların her biri ilişkili özel IP vardır. Contoso.com adlı bir özel bölge oluşturun ve bu sanal ağ kayıt sanal ağı olarak bağla sonra Azure DNS otomatik olarak iki A kayıtlarını bölgede gösterilen şekilde oluşturur. Şimdi, VNETA VM2.contoso.com çözmek için VNETA-VM1 kaynaklı DNS sorgularını VNETA VM2 özel IP'si içeren bir DNS yanıtı alırsınız. Ayrıca, ters DNS sorgu (PTR) için özel VNETA-VM2 verilen IP VNETA VM1 (10.0.0.1), beklendiği gibi VNETA-VM1 adını içeren bir DNS yanıtı alırsınız. 

![Tek sanal ağ çözümleme](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Senaryo: Ad çözümlemesi sanal ağlar

Bu senaryo burada özel bölge birden çok sanal ağ ile ilişkilendirmeniz gerekir daha yaygın bir durumdur. Bu senaryo Hub ve bağlı bileşen modeli gibi mimarileri sığabilecek hangi birden çok diğer bağlı bileşen için sanal ağlar bir merkezi Hub sanal ağ bağlı olduğu. Merkezi Hub sanal ağ olarak özel bir bölgeye kayıt sanal ağa bağlanabilir ve bağlı bileşen sanal ağlar çözümleme sanal ağları olarak bağlanabilir. 

Aşağıdaki diyagramda bu senaryoyu basit bir sürümünü gösterir yalnızca iki sanal ağın - A ve b olduğu A kaydı sanal ağ olarak tasarlanmış ve B bir çözümleme sanal ağ olarak belirlenmiş. Ortak bir bölge contoso.com paylaşmak her iki sanal ağlar için hedeftir. Bölge oluşturulduğunda ve çözümleme ve sanal ağlar bölgeye bağlantılı kayıt Azure otomatik olarak ayarlanır sanal ağ A. Vm'lerden (VNETA VM1 ve VNETA VM2) için DNS kayıtlarını kaydetme Ayrıca el ile DNS kayıtlarını bölgeye VM'ler için çözüm sanal ağında B. ekleyebilirsiniz Bu kurulum ile aşağıdaki davranış ileriye ve geriye doğru DNS sorgularını gözlemlemek:
* VNETB VM1 çözümleme sanal ağındaki B VNETA-VM1.contoso.com için bir DNS sorgusu VNETA VM1 özel IP'si içeren bir DNS yanıtı alır.
* Geriye doğru DNS (PTR) sorgudan VNETB VM2 10.1.0.1 için çözüm sanal ağda B, FQDN VNETB VM1.contoso.com içeren bir DNS yanıtı alır. Ters DNS sorguları aynı sanal ağa kapsamındaki nedenidir. 
* VNETB VM3 10.0.0.1, çözümleme sanal ağda B, bir geriye doğru DNS (PTR) sorgusu NXDOMAIN alır. Ters DNS sorguları aynı sanal ağa yalnızca kapsamındaki nedenidir. 


![Birden çok sanal ağ çözümleri](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Senaryo: Bölme işlevi

Bu senaryoda aynı DNS bölgesi için istemci (Azure içinde veya out Internet), burada bulunur bağlı olarak farklı DNS çözümlemesi davranışını hayata geçirmek için istediğiniz kullanım örneği vardır. Örneğin, özel ve genel sürüm uygulamanızın farklı işlevler veya davranışa sahip olabilir, ancak iki sürümü de aynı etki alanı adını kullanmak istiyor. Bu senaryo Azure DNS ile aynı ada sahip bir özel bölge yanı sıra bir ortak DNS bölgesi oluşturarak gerçekleştirilmiş.

Aşağıdaki diyagramda, bu senaryo gösterilmektedir. Her iki özel IP olan iki VM (VNETA VM1 ve VNETA VM2) olan bir sanal ağa bir sahip ve genel IP'ler tahsis edilir. Contoso.com adlı bir ortak DNS bölgesi oluşturmak ve DNS kayıtları bölge içinde olarak bu VM'ler için genel IP'ler kaydedin. A kaydı sanal ağ olarak belirterek contoso.com olarak da adlandırılan bir özel DNS bölgesi oluşturabilir. Azure kayıtları olarak VM'ler için kendi özel IP'leri gösteren özel bölge içine otomatik olarak kaydeder.

Artık bir internet istemcisi VNETA VM1.contoso.com aramak için bir DNS sorgusu gönderdiğinde Azure ortak bölgeden genel IP kaydı döndürür. Başka bir sanal makineden aynı DNS sorgusu verilen varsa (örneğin: VNETA VM2) aynı sanal ağda bir Azure özel bölgeden özel IP kaydı döndürür. 

![Bölünmüş Brian çözümleme](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Sonraki adımlar
Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md).

Bilgi edinmek için nasıl [özel bir DNS bölgesi oluşturma](./private-dns-getstarted-powershell.md) Azure DNS'de.

Ziyaret ederek DNS bölgeleri ve kayıtlar hakkında bilgi edinin: [DNS bölgeleri ve genel bakış kayıtları](dns-zones-records.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

