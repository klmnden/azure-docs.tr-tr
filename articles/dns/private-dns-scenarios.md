---
title: Azure DNS özel bölgeleri için senaryolar
description: Azure DNS özel bölgeleri kullanmak için yaygın senaryolara genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 03/15/2018
ms.author: victorh
ms.openlocfilehash: 409595febded7b242eae876ebb2cb35ae4999e5e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60686861"
---
# <a name="azure-dns-private-zones-scenarios"></a>Azure DNS özel bölgeleri senaryoları
Azure DNS özel bölgeleri, bir sanal ağ içindeki de sanal ağlar arasında ad çözümleme sağlar. Bu makalede, bu özellik kullanılarak gerçekleştirilen bazı yaygın senaryolar bakacağız. 

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Senaryo: Tek bir sanal ağa kapsamlı ad çözümlemesi
Bu senaryoda, azure'da sanal makineler (VM'ler) dahil olmak üzere Azure kaynaklarını bir dizi içinde olan bir sanal ağ sahip. Bir özel etki alanı adı (DNS bölgesi) aracılığıyla bir sanal ağdaki kaynaklar çözümlemek istediğiniz ve ad çözümlemesinin özel ve internet'ten erişilebilir olması gerekir. Ayrıca, sanal ağ içindeki VM'ler için Azure DNS bölgesine bunları otomatik olarak kaydedilecek gerekir. 

Bu senaryo aşağıda belirtilmiştir. Sanal ağ "A" adlı iki sanal makine (VM1 SANALAĞA ve SANALAĞA VM2) içerir. Bunların her biri ilişkili özel IP'ler vardır. Contoso.com adlı bir özel bölge oluşturmanız ve bu sanal ağın kayıt sanal ağı olarak bağlantı sonra Azure DNS otomatik olarak iki A kaydı bölgede gösterildiği şekilde oluşturur. Şimdi, SANALAĞA-VM1, SANALAĞA VM2.contoso.com çözümlemek için DNS sorgularını, SANALAĞA VM2 özel IP'si içeren bir DNS yanıtı alırsınız. Ayrıca, SANALAĞA-VM1'in (10.0.0.1) özel SANALAĞA VM2 verilen IP'si için ters DNS sorgusu (PTR) beklendiği gibi-VM1, SANALAĞA adını içeren bir DNS yanıtı alırsınız. 

![Tek bir sanal ağ çözümleme](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Senaryo: Sanal ağlar arasında ad çözümleme

Bu senaryo bir özel bölge birden çok sanal ağlarla ilişkilendirme gerek duyduğunuz senaryolara daha yaygın bir durumdur. Bu senaryo gibi Hub-and-Spoke modelini mimarileri sığabilen hangi birden çok diğer uca sanal ağlara bağlı merkezi Hub sanal ağındaki olduğu. Özel bir bölgeye kayıt sanal ağı olarak merkezi Hub sanal ağa bağlanabilir ve bağlı sanal ağlar çözümleme sanal ağları bağlanabilir. 

Aşağıdaki diyagramda bu senaryonun basit bir sürümü gösterir. yalnızca iki sanal ağ - A ve b olduğu Bir kayıt sanal ağı tasarlanmış ve B çözümleme sanal ağı belirlenmiş. Amaç, ortak bir bölge contoso.com paylaşmak her iki sanal ağ için ' dir. Bölgesi oluşturulduğuna ve kayıt sanal ağları bölgeye bağlanır ve çözüm, Azure otomatik olarak sanal ağ a Vm'lerden (SANALAĞA VM1 ve SANALAĞA VM2) için DNS kayıtlarını kaydetme Ayrıca el ile DNS kayıtlarını bölgeye VM'ler için çözümleme sanal ağ b ekleyebilirsiniz Bu kurulum ile ileriye ve geriye doğru DNS sorguları için aşağıdaki davranış gözlemleyeceksiniz:
* Bir DNS sorgusuna SANALAĞB VM1'den SANALAĞA-VM1.contoso.com için çözümleme sanal ağı B, SANALAĞA VM1 özel IP'si içeren bir DNS yanıtı alırsınız.
* Ters DNS (PTR) sorgudan SANALAĞB VM2 için 10.1.0.1, çözümleme sanal ağ B, FQDN SANALAĞB VM1.contoso.com içeren bir DNS yanıtı alırsınız. Ters DNS sorguları aynı sanal ağa kapsamındaki nedenidir. 
* Ters DNS (PTR) sorgudan SANALAĞB VM3 10.0.0.1, çözümleme sanal ağ B NXDOMAIN alırsınız. Ters DNS sorgular yalnızca aynı sanal ağa kapsamındaki nedenidir. 


![Birden çok sanal ağ çözümleri](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Senaryo: Split-Horizon işlevi

Bu senaryoda, aynı DNS bölgesi için farklı DNS çözümleme davranışı, istemci (Azure içinde veya internet üzerinde), burada yer alan bağlı olarak gerçeğe dönüştürmek için istediğiniz bir kullanım örneği vardır. Örneğin, uygulamanızın farklı işlevler veya davranışı özel ve genel bir sürümü olabilir, ancak her iki sürüm de aynı etki alanı adını kullanmak istiyorsunuz. Bu senaryo Azure DNS ile aynı ada sahip bir özel bölge yanı sıra, Genel DNS bölgesi oluşturma tarafından gerçekleştirilmiş.

Aşağıdaki diyagramda, bu senaryo gösterilmektedir. Her iki özel IP'ler sahip iki sanal makine (VM1 SANALAĞA ve SANALAĞA VM2) olan bir sanal ağa bir sahip ve genel IP'ler tahsis edilir. Contoso.com adlı bir genel DNS bölgesi oluşturur ve genel IP'ler bölge içindeki DNS kayıt olarak bu VM'ler için kaydolun. Ayrıca bir kayıt sanal ağı belirtilmesi contoso.com adlı bir özel DNS bölgesi oluşturabilir. Azure bir kayıt olarak Vm'leri özel Ip'lerini işaret eden özel bölge içinde otomatik olarak kaydeder.

Artık bir internet istemcisi SANALAĞA VM1.contoso.com aramak için bir DNS sorgusu gönderdiğinde, Azure genel bölgeye genel IP kaydını döndürür. Başka bir VM'den aynı DNS sorgusu verilen varsa (örneğin: VNETA-VM2) aynı sanal ağdaki bir Azure özel bölgeye özel IP kaydını döndürür. 

![Bölünmüş Brian çözümleme](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Sonraki adımlar
Özel DNS bölgeleri hakkında daha fazla bilgi için bkz. [Özel etki alanları için Azure DNS'i kullanma](private-dns-overview.md).

Bilgi edinmek için nasıl [özel bir DNS bölgesi oluşturma](./private-dns-getstarted-powershell.md) Azure DNS'de.

Ziyaret ederek, DNS bölgeleri ve kayıtları hakkında bilgi edinin: [DNS bölgeleri ve kayıtları genel bakış](dns-zones-records.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

