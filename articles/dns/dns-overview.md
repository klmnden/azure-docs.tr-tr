---
title: "Azure DNS genel bakış | Microsoft Docs"
description: "DNS barındırma hizmeti Microsoft Azure ile ilgili genel bakış. Microsoft Azure etki alanınızda barındırır."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: f255fd9621ff90bfbb3ad193faa64495acf7ecd7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="azure-dns-overview"></a>Azure DNS'ye genel bakış

Etki alanı adı sistemi ya da DNS, çevirmek için sorumludur (veya çözme) bir Web sitesi ya da hizmet adı IP adresine. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Azure DNS şimdi özel DNS etki alanı da destekler. Daha fazla bilgi için bkz: [kullanarak Azure DNS özel etki alanları için](private-dns-overview.md).

![DNS'ye genel bakış](./media/dns-overview/scenario.png)

## <a name="features"></a>Özellikler

* **Güvenilirlik ve performans** -Azure DNS'de DNS etki alanı, Azure'nın genel DNS ad sunucuları ağda barındırılır. Azure DNS, böylece her DNS sorgusu en yakın kullanılabilir DNS sunucusu tarafından yanıtlanan her noktaya yayın ağ kullanır. Bu, etki alanınız için yüksek kullanılabilirlik ve hızlı performans sağlar.

* **Sorunsuz tümleştirme** -Azure DNS hizmeti, dış kaynaklarınız için DNS sağlamak için kullanılan ve Azure hizmetlerinizi için DNS kayıtlarını yönetmek için kullanılır. Azure DNS, Azure portalında tümleşiktir ve aynı kimlik bilgileri, faturalama ve destek sözleşmesi, diğer Azure Hizmetleri olarak kullanır.

* **Güvenlik** -Azure DNS hizmeti, Azure Resource Manager dayanır. Bu nedenle, rol tabanlı erişim denetimi, denetim günlüklerini ve kaynak kilitleme gibi Resource Manager özelliklerinden yararlanır. Etki alanları ve kayıtları Azure portalı, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI yönetilebilir. Otomatik DNS Yönetimi gerektiren uygulamalar, REST API ve SDK aracılığıyla hizmeti ile tümleştirebilirsiniz.

Azure DNS, etki alanı adlarını satın alma şu anda desteklemiyor. Etki alanı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt kullanmanız gerekir. Kayıt, genellikle küçük bir yıllık ücret ister. Etki alanları için DNS kayıtlarını Yönetimi sonra Azure DNS'de barındırılabilir. Bkz: [bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md) Ayrıntılar için.

## <a name="pricing"></a>Fiyatlandırma

DNS faturalama Azure ve DNS sorgularının sayısı tarafından barındırılan DNS bölgeleri sayısını temel alır. Ziyaret fiyatlandırma hakkında daha fazla bilgi için [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>SSS

Azure DNS hakkında sık sorulan sorular için bkz: [Azure DNS ile ilgili SSS](dns-faq.md).

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret ederek DNS bölgeleri ve kayıtlar hakkında bilgi edinin: [DNS bölgeleri ve genel bakış kayıtları](dns-zones-records.md).

Bilgi edinmek için nasıl [bir DNS bölgesi oluşturma](./dns-getstarted-create-dnszone-portal.md) Azure DNS'de.

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

