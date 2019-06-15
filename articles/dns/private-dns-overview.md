---
title: Azure özel DNS nedir?
description: Barındırma hizmeti Microsoft Azure özel DNS genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: a8548b4972d5853f09630ae3e9ded05ed6fee32b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076412"
---
# <a name="what-is-azure-private-dns"></a>Azure özel DNS nedir?

> [!IMPORTANT]
> Azure özel DNS şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) bir hizmet adı, IP adresi.  Azure DNS bir barındırma DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. İnternet'e yönelik DNS etki alanlarınızı hizmetinin yanı sıra, Azure DNS özel DNS bölgelerini de destekler.

Azure özel DNS yönetmek ve özel bir DNS çözümü eklemek zorunda kalmadan bir sanal ağdaki etki alanı adlarını çözümlemek için güvenilir, güvenli DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, Azure tarafından sağlanan mevcut adların yerine kendi özel etki alanı adlarını kullanabilirsiniz. Özel etki alanı adlarını kullanarak, sanal ağ Mimarinizi en iyi karşılayacak şekilde uyarlamak için kuruluşunuzun gereksinimlerine yardımcı olur. Sanal makineler (VM) bir sanal ağdaki ve sanal ağlar arasında ad çözümleme sağlar. Ayrıca, bölge adlarını özel ve Genel DNS bölgelerinin adı paylaşmasına izin veren bir split-horizon görünümünde ile yapılandırabilirsiniz.

Özel bir DNS bölgesi sanal ağınızdan gelen kayıtları çözmek için sanal ağ ile bölgeye bağlamanız gerekir. Bağlı sanal ağlar tam erişimi vardır ve özel bölgesinde yayımlanan tüm DNS kayıtlarının çözebilirsiniz. Ayrıca, bir sanal ağ bağlantısını otomatik kayıt etkinleştirebilirsiniz. Bir sanal ağ bağlantısını otomatik kayıt etkinleştirirseniz, bu sanal ağ üzerindeki sanal makineler için DNS kayıtlarını özel bölgesinde kaydedilir. Otomatik kayıt etkin olduğunda, bir sanal makine oluşturulan her değiştiğinde Azure DNS de bölge kayıtları güncelleştirir, ' IP adresi ya da silinir.

![DNS'ye genel bakış](./media/private-dns-overview/scenario.png)

> [!NOTE]
> En iyi uygulama, kullanmayın bir *.local* özel DNS bölgeniz için etki alanı. Tüm işletim sistemleri bu destekler.

## <a name="benefits"></a>Avantajlar

Azure özel DNS aşağıdaki avantajları sağlar:

* **Özel DNS çözümler gereksinimini ortadan kaldırır**. Daha önce birçok müşterinin sanal ağlarında DNS bölgelerini yönetmek için özel DNS çözümler oluşturuldu. Artık DNS bölgeleri oluşturma ve özel DNS çözümlerini yönetme yükünden kaldırır yerel Azure altyapısını kullanarak da yönetebilirsiniz.

* **Tüm yaygın DNS kayıt türlerini kullanın**. Azure DNS, A, AAAA, CNAME, MX, PTR, SOA, SRV ve TXT kayıtlarını destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi**. Özel DNS kayıtlarınızı barındırma yanı sıra Azure belirtilen sanal ağlarda bulunan sanal makineler için ana bilgisayar adı kayıtlarını otomatik olarak korur. Bu senaryoda, gerek kalmadan özel DNS çözümler oluşturma veya uygulamaları değiştirmek için kullandığınız etki alanı adlarını iyileştirebilirsiniz.

* **Sanal ağlar arasında ana bilgisayar adı çözümlemesi**. Azure tarafından sağlanan ana bilgisayar adlarından farklı olarak, özel DNS bölgelerini sanal ağlar arasında paylaşılabilir. Bu özellik, sanal ağ eşlemesi gibi arası ağ ve hizmet bulma senaryolarını basitleştirir.

* **Alışık olduğunuz araçları ve kullanıcı deneyimi**. Öğrenme eğrisini azaltmak için bu hizmeti Azure DNS tanınmış Araçlar (Azure portalı, Azure PowerShell, Azure CLI, Azure Resource Manager şablonları ve REST API) kullanır.

* **Bölme DNS desteği**. Azure DNS ile aynı ada sahip bir sanal ağ içinde ve genel internet'ten farklı yanıtlardan çözümlenmesi bölgeler oluşturabilirsiniz. Bölme DNS için tipik bir senaryo, ayrılmış bir sanal ağınız içindeki kullanım için bir hizmet sürümünü sağlamaktır.

* **Tüm Azure bölgelerinde kullanılabilir**. Azure DNS özel bölgelerini özelliği, Azure genel bulutundaki tüm Azure bölgelerinde kullanılabilir.

## <a name="capabilities"></a>Özellikler

Azure DNS, aşağıdaki özellikleri sağlar:

* **Özel bir bölgeye otomatik kayıt etkin ile bağlı bir sanal ağdan sanal makinelerin otomatik kayıt**. Sanal makineler kayıtlı (eklendi) için özel IP adreslerine işaret eden bir kayıt olarak özel bölgeye olur. Bir sanal makinede etkin otomatik kayıt içeren bir sanal ağa bağlantı silindiğinde, Azure DNS bağlı özel bölgeden karşılık gelen DNS kaydını da otomatik olarak kaldırır.

* **İleriye doğru DNS çözümlemesi, özel bölgesiyle bağlantılı sanal ağlar desteklenir**. DNS çözümlemesi arası sanal ağ için olduğunu açık bağımlılığı olmayan sanal ağlar birbiriyle eşlenmiş gibi. Ancak, diğer senaryolar (örneğin, HTTP trafiğini) için sanal ağları eşleyebilme isteyebilirsiniz.

* **Geriye doğru DNS araması sanal ağ kapsamı içinde desteklenen**. Özel bir bölgesi için atanmış sanal ağ içindeki özel bir IP için ters DNS araması konak/kayıt adı ve bölge adı soneki içeren FQDN döndürür.

## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Azure DNS, aşağıdaki sınırlamalara sahiptir:

* Özel bölge başına yalnızca bir kayıt sanal ağı izin verilir.
* En fazla 10 çözümleme sanal ağları özel bölge başına izin verilir. Bu özellik genel kullanıma sunulduğunda, bu sınır kaldırılacak.
* Belirli bir sanal ağ için yalnızca bir özel bölge kayıt sanal ağı bağlanabilir.
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözümleme sanal ağı bağlanabilir. Bu özellik genel kullanıma sunulduğunda, bu sınır kaldırılacak.
* Kayıt sanal ağı belirtirseniz, özel bölgeye kaydedilen Vm'lerden söz konusu sanal ağ için DNS kayıtlarını görüntülenebilir veya Azure Powershell ve Azure CLI API'leri alınabilir değil. VM kayıtları gerçekten kaydedilir ve başarıyla çözülecektir.
* Kayıt sanal ağ özel IP alanı için yalnızca geriye doğru DNS çalışır.
* Ters DNS özel bölgesi (örneğin, bölge bağlantılı çözümleme sanal ağı özel bir sanal ağdaki bir sanal makine için bir özel IP) kayıtlı değilse bir özel IP döndürür için *internal.cloudapp.net* DNS son eki. Ancak bu sonek çözümlenebilir değil.
* Sanal ağ özel bir bölgeye kayıt veya çözümleme sanal ağı olarak bağlantı ilk kez tamamen boş olmalıdır. Ancak, sanal ağ boş ardından olabilir bir kayıt veya çözümleme sanal ağ özel diğer bölgelere gelecekteki bağlama.
* Şu anda koşullu iletme (örneğin, Azure ve şirket içi ağlar arasında çözüm etkinleştirme için) desteklenmiyor. Müşteriler, bu senaryo başka mekanizmalar aracılığıyla nasıl hayata geçirebilirsiniz hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).
* Yalnızca bağlı sanal ağ özel IP alanı için ters DNS çalışır
* Bağlı bir sanal ağ için özel bir IP için ters DNS "internal.cloudapp.net" sanal makine için varsayılan ön ek olarak döndürür. Özel bir IP ile varsayılan 2 FQDN'leri soneki döndürür için özel bir bölgeye otomatik kayıt etkin ile bağlı sanal ağlar için ters DNS *internal.cloudapp.net* ve başka bir özel bölge soneki.
* Koşullu iletme desteklenmez. Örneğin, Azure ve şirket içi ağlar arasındaki çözümleme etkinleştirmek için şunu yazın. Bu senaryo başka mekanizmalar kullanılarak nasıl olanak sağlayabileceğiniz öğrenin. Bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 


## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz: [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Sonraki adımlar

* Kullanarak Azure DNS özel bölgesi oluşturmayı öğrenin [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md).

* Bazı ortak hakkında okuyun [özel bölge senaryoları](./private-dns-scenarios.md) Azure DNS'te özel bölgeler ile gerçekleştirilebilecek.

* Yaygın sorular ve yanıtlar hakkında Azure DNS özel bölgeleri için de dahil olmak üzere belirli bir davranışı, belirli türde operasyonlar beklediğiniz, bakın [özel DNS SSS](./dns-faq-private.md).

* DNS bölgeleri ve kayıtları hakkında ederek bilgi [DNS bölgeleri ve kayıtları genel bakış](dns-zones-records.md).

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
