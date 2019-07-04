---
title: Azure Özel DNS nedir?
description: Barındırma hizmeti Microsoft Azure özel DNS genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: overview
ms.date: 6/12/2019
ms.author: victorh
ms.openlocfilehash: aedace031eaedf2709993b5185979e8777821759
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444832"
---
# <a name="what-is-azure-private-dns"></a>Azure Özel DNS nedir?

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

## <a name="known-issues"></a>Bilinen sorunlar
Bilinen hataları ve sorunları Önizleme sürümünde aşağıdaki öğeler şunlardır:
* Özel bir DNS bölgesi için bağlı bir sanal ağ silerseniz, özel DNS bölgesi bağlantılar silmez. Sanal ağ ile aynı adı ve kaynak grubu oluşturmanız ve tüm özel DNS bölgesi için bağlantısını yeniden denemek bağlantı başarısız olur. Bu sorunu geçici olarak çözmek için farklı bir kaynak grubunda veya farklı bir adla aynı kaynak grubunda sanal ağ oluşturun.
* Bir sanal ağ başka bir kaynak grubuna veya aboneliğe taşıma, özel DNS bölgesi için bağlantıları güncelleştirmez. Taşınan sanal ağ için ad çözümlemesi çalışmaya, ancak özel DNS bölgenizi, sanal ağ bağlantıları'nı görüntülediğinizde, sanal ağın eski ARM kimlikleri görürsünüz devam eder.
* Şu anda barındırılan BAE Kuzey BAE Orta, Güney Afrika Batı, Güney Afrika Kuzey, Doğu Kanada, Fransa Güney bağlı sanal ağlar başarısız olabilir ve aralıklı DNS çözümlemesi sorunları görebilirsiniz. 


## <a name="other-considerations"></a>Dikkat edilecek diğer noktalar

Azure DNS, aşağıdaki sınırlamalara sahiptir:

* VM DNS kayıtlarının otomatik kayıt etkinse, gibi belirli bir sanal ağ için yalnızca bir özel bölge bağlanabilir. Ancak, tek bir DNS bölgesi için birden çok sanal ağı bağlayabilirsiniz.
* Yalnızca bağlı sanal ağ özel IP alanı için ters DNS çalışır
* Bağlı bir sanal ağ için özel bir IP için ters DNS "internal.cloudapp.net" sanal makine için varsayılan ön ek olarak döndürür. Özel bir IP ile varsayılan 2 FQDN'leri soneki döndürür için özel bir bölgeye otomatik kayıt etkin ile bağlı sanal ağlar için ters DNS *internal.cloudapp.net* ve başka bir özel bölge soneki.
* Koşullu iletme, şu anda yerel olarak desteklenmiyor. Azure ve şirket içi ağlar arasında çözüm sağlamak için. Bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)
 
## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz: [Azure DNS fiyatlandırma](https://azure.microsoft.com/pricing/details/dns/).

## <a name="next-steps"></a>Sonraki adımlar

* Kullanarak Azure DNS özel bölgesi oluşturmayı öğrenin [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md).

* Bazı ortak hakkında okuyun [özel bölge senaryoları](./private-dns-scenarios.md) Azure DNS'te özel bölgeler ile gerçekleştirilebilecek.

* Yaygın sorular ve yanıtlar hakkında Azure DNS özel bölgeleri için de dahil olmak üzere belirli bir davranışı, belirli türde operasyonlar beklediğiniz, bakın [özel DNS SSS](./dns-faq-private.md).

* DNS bölgeleri ve kayıtları hakkında ederek bilgi [DNS bölgeleri ve kayıtları genel bakış](dns-zones-records.md).

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
