---
title: Özel etki alanları için Azure DNS kullanabilir | Microsoft Docs
description: Microsoft Azure üzerinde hizmet barındırma özel DNS genel bakış.
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
ms.openlocfilehash: 0ee3b18b7f874c4f6b7b2c9c559aa7e393ad7d8d
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700613"
---
# <a name="use-azure-dns-for-private-domains"></a>Özel etki alanları için Azure DNS kullanabilir
Etki alanı adı sistemi ya da DNS, çevirmek için sorumludur (veya çözme) IP adresi için bir hizmet adı. DNS etki alanı için bir barındırma hizmeti, Microsoft Azure altyapı kullanarak Azure DNS ad çözümlemesi sağlar. İnternet'e yönelik DNS etki alanı'ı desteklemenin yanında Azure DNS şimdi ayrıca özel DNS etki alanı bir önizleme özelliği olarak destekler. 
 
Azure DNS, yönetmek ve özel DNS çözüm eklemek zorunda kalmadan sanal bir ağ etki alanı adları çözümlemek için bir güvenilir, güvenli DNS hizmeti sağlar. Özel DNS bölgeleri kullanarak, günümüzün Azure tarafından sağlanan adları yerine kendi özel etki alanı adlarını kullanabilirsiniz. Özel etki alanı adlarını kullanarak, sanal ağ Mimarinizi en iyi karşılayacak şekilde uyarlamak için kuruluşunuzun gereksinimlerine yardımcı olur. Sanal makineler (VM'ler) sanal ağlar arasında ve sanal ağ için ad çözümlemesi sağlar. Ayrıca, özel ve ortak bir DNS bölgesi aynı adı paylaşan olanak veren bir bölme görünümü ile bölgeleri adları yapılandırabilirsiniz.

![DNS'ye genel bakış](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Avantajlar

Azure DNS, aşağıdaki avantajları sağlar:

* **Özel DNS çözümler gereksinimini ortadan kaldırır**. Daha önce sanal ağlarındaki DNS bölgeleri yönetmek için özel DNS çözümler birçok müşteri oluşturuldu. DNS bölge Yönetimi artık oluşturmak ve özel DNS çözümleri yönetmek yükünü kaldırır yerel Azure altyapısını kullanarak da gerçekleştirebilirsiniz.

* **Tüm yaygın DNS kayıt türlerini kullanmak**. Azure DNS A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ve TXT kaydı destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi**. Özel DNS kayıtlarınızı barındırma yanı sıra, Azure belirtilen sanal ağ içindeki VM'ler için ana bilgisayar kayıtları otomatik olarak bulundurur. Bu senaryoda, gerek kalmadan özel DNS çözümler oluşturmak veya uygulamaları değiştirmek için kullandığınız etki alanı adları iyileştirebilirsiniz.

* **Ana bilgisayar adı çözümlemesi sanal ağlar arasında**. Azure tarafından sağlanan ana bilgisayar adları farklı olarak, özel DNS bölgeleri sanal ağlar arasında paylaşılabilir. Bu özellik, sanal ağ eşlemesi gibi arası ağ ve hizmet bulma senaryolar basitleştirir.

* **Tanıdık Araçlar ve kullanıcı deneyimi**. Öğrenme eğrisi azaltmak için bu yeni sunum tanınmış Azure DNS araçları (PowerShell, Azure Resource Manager şablonları ve REST API) kullanır.

* **Yatay bölme DNS desteği**. Azure DNS ile aynı ada sahip bir sanal ağ içinde ve ortak internet'ten farklı yanıtlardan çözümlemek bölgeler oluşturabilirsiniz. Yatay bölme DNS tipik bir senaryo, ayrılmış bir sanal ağınız içinde kullanmak için bir hizmet sürümü sağlamaktır.

* **Tüm Azure bölgeleri bulunan**. Azure DNS özel bölgeler özelliği tüm Azure bölgeleri Azure genel bulutunda kullanılabilir. 


## <a name="capabilities"></a>Özellikler

Azure DNS aşağıdaki yetenekleri sağlar:
 
* **Özel bir bölgeye kayıt sanal ağı olarak bağlı tek bir sanal ağdaki sanal makinelerin otomatik kayıt**. Sanal makine kayıtlı (eklenir) için kendi özel IP işaret eden bir kayıt olarak özel bölge yok. Bir sanal makine, sanal ağ silinir kayıt Azure karşılık gelen DNS da otomatik olarak kaldırır. bağlantılı özel bölgeden kayıt. 

  > [!NOTE]
  > Varsayılan olarak, kayıt sanal ağlar herhangi birinden kaydı sanal ağ içindeki sanal makinelerin DNS çözümlemesi bölge karşı çalışır herkese açık çözümleme sanal ağlar da görür. 

* **İleri DNS çözümlemesi özel bölgesine çözümleme sanal ağları olarak bağlı olan sanal ağlar üzerinden desteklenir**. DNS çözümlemesi arası sanal ağ için olduğunu hiçbir açık bağımlılık sanal ağlar birbirleri ile eşlenen gibi. Ancak, müşteriler sanal ağları (örneğin, HTTP trafiği) diğer senaryolar için eş isteyebilirsiniz.

* **Geriye doğru DNS araması sanal ağ kapsamında desteklenen**. Özel bir bölgeye atanmış sanal ağ içinde özel bir IP için geriye doğru DNS araması bölge adı son eki olarak yanı sıra konak/kayıt adı içerir FQDN döndürür. 


## <a name="limitations"></a>Sınırlamalar

Azure DNS aşağıdaki sınırlamalara tabi şöyledir:

* Yalnızca bir kayıt sanal ağ özel bölge izin verilir.
* En fazla 10 çözümleme sanal ağlar özel bölge izin verilir.
* Belirli bir sanal ağ için yalnızca bir özel bölge kaydı sanal ağ olarak bağlanabilir.
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözüm sanal ağ olarak bağlanabilir.
* Kayıt sanal ağ belirtilirse, bu sanal ağdan özel bölgesine kayıtlı olan VM'ler için DNS kayıtlarını görüntülenebilir veya Azure Powershell ve Azure CLI API'leri alınabilir değildir, ancak VM kayıtları gerçekten kaydedilir ve olur başarılı bir şekilde çözümleyin.
* Özel IP alanı kaydı sanal ağ için yalnızca geriye doğru DNS çalışır.
* Özel bölgesinde (örneğin, bir sanal makine bir çözümleme sanal ağ olarak özel bölge bağlı bir sanal ağ için özel bir IP) kayıtlı değil özel bir IP döndürür DNS geriye doğru *internal.cloudapp.net* DNS son eki. Ancak, bu soneki çözümlenebilir değil. 
* Sanal ağ boş olması gerekir (yani, VM kayıt yok) olduğunda, başlangıçta (diğer bir deyişle, ilk kez) özel bir bölgeye kayıt ya da çözümleme bir sanal ağ bağlantıları. Ancak, sanal ağ sonra boş olabilir kayıt ya da çözümleme sanal ağı olarak, diğer özel bölgeler için gelecekteki bağlama. 
* Şu anda koşullu iletme (örneğin, Azure ve OnPrem ağları arasında çözümleme etkinleştirmek için) desteklenmiyor. Müşteriler bu senaryo başka mekanizmalar aracılığıyla nasıl sağlarsınız hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Ortak hakkında sorular ve yanıtlar Azure DNS'de özel bölgeler için belirli bir DNS kaydı ve çözümleme davranışı dahil olmak üzere, belirli türde operasyonlar beklediğiniz, bkz: [SSS](./dns-faq.md#private-dns).  


## <a name="pricing"></a>Fiyatlandırma

Özel DNS bölgelerini özelliği genel Önizleme sırasında ücretsizdir. Genel kullanılabilirlik sırasında bu özellik, bir kullanım tabanlı fiyatlandırma modeli, mevcut Azure DNS benzer sunumu sunar. 


## <a name="next-steps"></a>Sonraki adımlar

Azure DNS'de kullanarak özel bir bölge oluşturmayı öğrenin [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md).

Bazı ortak hakkında okuyun [özel bölge senaryoları](./private-dns-scenarios.md) , gerçekleşen Azure DNS'de özel bölgeler ile.

Ortak hakkında sorular ve yanıtlar Azure DNS'de özel bölgeler için dahil olmak üzere belirli bir davranışı, belirli türde operasyonlar beklediğiniz, bkz: [SSS](./dns-faq.md#private-dns). 

Ziyaret ederek DNS bölgeleri ve kayıtlar hakkında bilgi edinin [DNS bölgeleri ve genel bakış kayıtları](dns-zones-records.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin. 

