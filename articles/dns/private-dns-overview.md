---
title: Azure DNS özel etki alanları için kullanın. | Microsoft Docs
description: Barındırma hizmeti Microsoft Azure özel DNS genel bakış.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: victorh
ms.openlocfilehash: 2ab7070a4cf46dae543af8d3e1d688e12ec1eb2a
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173651"
---
# <a name="use-azure-dns-for-private-domains"></a>Özel etki alanları için Azure DNS kullanma
DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) bir hizmet adı, IP adresi. Bir barındırma hizmeti DNS etki alanları için Microsoft Azure altyapısı kullanarak Azure DNS ad çözümlemesi sağlar. İnternet'e yönelik DNS etki alanlarınızı hizmetinin yanı sıra, Azure DNS artık ayrıca özel DNS etki alanı bir önizleme özelliği olarak destekler. 
 
Azure DNS, yönetmek ve özel bir DNS çözümü ekleme gerek kalmadan bir sanal ağdaki etki alanı adlarını çözümlemek için güvenilir, güvenli DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, Azure tarafından sağlanan mevcut adların yerine kendi özel etki alanı adlarını kullanabilirsiniz. Özel etki alanı adlarını kullanarak, sanal ağ Mimarinizi en iyi karşılayacak şekilde uyarlamak için kuruluşunuzun gereksinimlerine yardımcı olur. Sanal makineler (VM) bir sanal ağdaki ve sanal ağlar arasında ad çözümleme sağlar. Ayrıca, bölge adlarını özel ve Genel DNS bölgelerinin aynı adı paylaşmasını sağlayan ve split-horizon görünümünde ile yapılandırabilirsiniz.

![DNS'ye genel bakış](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Avantajlar

Azure DNS aşağıdaki avantajları sağlar:

* **Özel DNS çözümler gereksinimini ortadan kaldırır**. Daha önce birçok müşterinin sanal ağlarında DNS bölgelerini yönetmek için özel DNS çözümler oluşturuldu. DNS bölgesi Yönetimi artık oluşturma ve özel DNS çözümlerini yönetme yükünden kaldırır yerel Azure altyapısını kullanarak da gerçekleştirebilirsiniz.

* **Tüm yaygın DNS kayıt türlerini kullanın**. Azure DNS, A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ve TXT kayıtlarını destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi**. Özel DNS kayıtlarınızı barındırma yanı sıra Azure belirtilen sanal ağlarda bulunan sanal makineler için ana bilgisayar adı kayıtlarını otomatik olarak korur. Bu senaryoda, gerek kalmadan özel DNS çözümler oluşturma veya uygulamaları değiştirmek için kullandığınız etki alanı adlarını iyileştirebilirsiniz.

* **Sanal ağlar arasında ana bilgisayar adı çözümlemesi**. Azure tarafından sağlanan ana bilgisayar adlarından farklı olarak, özel DNS bölgelerini sanal ağlar arasında paylaşılabilir. Bu özellik, sanal ağ eşlemesi gibi arası ağ ve hizmet bulma senaryolarını basitleştirir.

* **Alışık olduğunuz araçları ve kullanıcı deneyimi**. Öğrenme eğrisini azaltmak için bu yeni teklif tanınmış Azure DNS araçları (PowerShell, Azure Resource Manager şablonları ve REST API) kullanır.

* **Bölme DNS desteği**. Azure DNS ile aynı ada sahip bir sanal ağ içinde ve genel internet'ten farklı yanıtlardan çözümlenmesi bölgeler oluşturabilirsiniz. Bölme DNS için tipik bir senaryo, ayrılmış bir sanal ağınız içindeki kullanım için bir hizmet sürümünü sağlamaktır.

* **Tüm Azure bölgelerinde kullanılabilir**. Azure DNS özel bölgelerini özelliği, Azure genel bulutundaki tüm Azure bölgelerinde kullanılabilir. 


## <a name="capabilities"></a>Özellikler

Azure DNS, aşağıdaki özellikleri sağlar:
 
* **Kayıt sanal ağı olarak özel bir bölgeye bağlı tek bir sanal ağ üzerinden sanal makinelerin otomatik kayıt**. Sanal makineler kayıtlı (eklendi) özel Ip'lerini işaret eden bir kayıt olarak özel bölgeye olur. Bir sanal makine, sanal ağ silindiğinden bir kayıt karşılık gelen DNS Azure da otomatik olarak kaldırır. kayıt bağlı özel bölge. 

  > [!NOTE]
  > Varsayılan olarak, kayıt sanal ağları kayıt sanal ağdaki sanal makinelerin herhangi DNS çözümlemesi bölge karşı çalışan anlamında çözümleme sanal ağları de görür. 

* **İleriye doğru DNS çözümlemesi, özel bölgesiyle bağlantılı çözümleme sanal ağları sanal ağlar desteklenir**. DNS çözümlemesi arası sanal ağ için olduğunu açık bağımlılığı olmayan sanal ağlar birbiriyle eşlenmiş gibi. Bununla birlikte, müşteriler (örneğin, HTTP trafiğini) diğer senaryolar için sanal ağları eşleyebilme isteyebilirsiniz.

* **Geriye doğru DNS araması sanal ağ kapsamı içinde desteklenen**. Özel bir bölgesi için atanmış sanal ağ içindeki özel bir IP için ters DNS Arama soneki bölge adı yanı sıra konak/kayıt adını içeren FQDN döndürür. 


## <a name="limitations"></a>Sınırlamalar

Azure DNS, aşağıdaki sınırlamalara tabi şöyledir:

* Özel bölge başına yalnızca bir kayıt sanal ağı izin verilir.
* En fazla 10 çözümleme sanal ağları özel bölge başına izin verilir.
* Belirli bir sanal ağ için yalnızca bir özel bölge kayıt sanal ağı bağlanabilir.
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözümleme sanal ağı bağlanabilir.
* Kayıt sanal ağı belirtilmediği takdirde, özel bölgeye kaydedilen Vm'lerden söz konusu sanal ağ için DNS kayıtlarını görüntülenebilir veya Azure Powershell ve Azure CLI API'leri alınabilir değildir, ancak VM kayıtları gerçekten kaydedilir ve olur başarılı bir şekilde çözün.
* Kayıt sanal ağ özel IP alanı için yalnızca geriye doğru DNS çalışır.
* Ters DNS özel bölgesi (örneğin, bölge bağlantılı çözümleme sanal ağı özel bir sanal ağdaki bir sanal makine için bir özel IP) içinde kayıtlı değil özel bir IP döndürür *internal.cloudapp.net* DNS son eki. Ancak bu sonek çözümlenebilir değil. 
* Sanal ağ boş olması gerekir (diğer bir deyişle, VM kayıt yok) olduğunda, başlangıçta (diğer bir deyişle, ilk kez) bağlantılar, özel bir bölgeye kayıt veya çözümleme sanal ağı olarak. Ancak, sanal ağ boş ardından olabilir bir kayıt veya çözümleme sanal ağ özel diğer bölgelere gelecekteki bağlama. 
* Şu anda koşullu iletme (örneğin, Azure ve şirket içi ağlar arasındaki çözümleme etkinleştirmek için) desteklenmiyor. Müşteriler, bu senaryo başka mekanizmalar aracılığıyla nasıl hayata geçirebilirsiniz hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

Yaygın sorular ve yanıtlar hakkında Azure DNS özel bölgeleri için belirli bir DNS kaydı ve çözümleme davranışı dahil olmak üzere, belirli türde operasyonlar beklediğiniz, bakın [SSS](./dns-faq.md#private-dns).  


## <a name="pricing"></a>Fiyatlandırma

Özel DNS bölgeleri özelliği genel Önizleme süresince ücretsizdir. Genel kullanıma sunulduğunda özellik sunan bir kullanım tabanlı fiyatlandırma modeli, var olan Azure DNS için benzer sunacaktır. 


## <a name="next-steps"></a>Sonraki adımlar

Kullanarak Azure DNS özel bölgesi oluşturmayı öğrenin [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md).

Bazı ortak hakkında okuyun [özel bölge senaryoları](./private-dns-scenarios.md) Azure DNS'te özel bölgeler ile gerçekleştirilebilecek.

Yaygın sorular ve yanıtlar hakkında Azure DNS özel bölgeleri için de dahil olmak üzere belirli bir davranışı, belirli türde operasyonlar beklediğiniz, bakın [SSS](./dns-faq.md#private-dns). 

DNS bölgeleri ve kayıtları hakkında ederek bilgi [DNS bölgeleri ve kayıtları genel bakış](dns-zones-records.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin. 

