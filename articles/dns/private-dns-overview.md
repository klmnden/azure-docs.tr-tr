---
title: Özel etki alanları için Azure DNS kullanarak | Microsoft Docs
description: Özel DNS barındırma hizmeti Microsoft Azure ile ilgili genel bakış.
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
ms.openlocfilehash: 1c805819a22d26e650d13b0e41ebac00c4e52d91
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="using-azure-dns-for-private-domains"></a>Azure DNS için özel etki alanlarını kullanma
Etki alanı adı sistemi ya da DNS, çevirmek için sorumludur (veya çözme) IP adresi için bir hizmet adı. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir.  İnternet'e yönelik DNS etki alanı yanı sıra Azure DNS şimdi ayrıca özel DNS etki alanı bir önizleme özelliği olarak destekler.  
 
Azure DNS, yönetmek ve özel DNS çözüm eklemek zorunda kalmadan sanal bir ağ etki alanı adları çözümlemek için bir güvenilir, güvenli DNS hizmeti sağlar. Özel DNS bölgeleri günümüzün Azure tarafından sağlanan adları yerine kendi özel etki alanı adlarını kullanmanızı sağlar.  Özel etki alanı adlarını kullanarak, sanal ağ Mimarinizi en iyi karşılayacak şekilde uyarlamak için kuruluşunuzun gereksinimlerine yardımcı olur. Bu ad çözümlemesi VM'ler için sanal ağlar arasında ve sanal ağ sağlar. Ayrıca, özel ve ortak bir DNS bölgesi aynı adı paylaşan izin vererek bir bölme görünümü ile - bölgeleri adları yapılandırabilirsiniz.

![DNS'ye genel bakış](./media/private-dns-overview/scenario.png)

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="benefits"></a>Avantajlar

* **Özel DNS çözümler gereksinimini ortadan kaldırır.** Daha önce sanal ağlarındaki DNS bölgeleri yönetmek için özel DNS çözümler birçok müşteri oluşturuldu.  DNS bölge Yönetimi artık yapılabilir oluşturma & özel DNS çözümleri yönetmek yükünü kaldırır Azure'nın yerel altyapısını kullanarak.

* **Tüm yaygın DNS kayıt türlerini kullanın.**  Azure DNS A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ve TXT kaydı destekler.

* **Otomatik ana bilgisayar adı kayıt yönetimi.** Özel DNS kayıtlarınızı barındırma yanı sıra, Azure belirtilen sanal ağ içindeki VM'ler için ana bilgisayar kayıtları otomatik olarak bulundurur.  Bu, gerek kalmadan özel DNS çözümler oluşturmak veya uygulama değiştirmek için kullandığınız etki alanı adları en iyi duruma getirme sağlar.

* **Ana bilgisayar adı çözümlemesi sanal ağlar arasında.** Azure tarafından sağlanan ana bilgisayar adları farklı olarak, özel DNS bölgeleri sanal ağlar arasında paylaşılabilir.  Bu özellik, sanal ağ eşlemesi gibi arası ağ ve hizmet bulma senaryolar basitleştirir.

* **Tanıdık Araçlar ve kullanıcı deneyimi.** Öğrenme eğrisi azaltmak için bu yeni sunum zaten tanınmış Azure DNS araçları (PowerShell, Resource Manager şablonları, REST API) kullanır.

* **Yatay bölme DNS desteği.** Azure DNS bölgeleri farklı yanıtlardan bir sanal ağ içinde ve ortak Internet'ten çözümlemek aynı ada sahip oluşturmanıza olanak sağlar.  Yatay bölme DNS tipik bir senaryo, ayrılmış bir sanal ağınız içinde kullanmak için bir hizmet sürümü sağlamaktır.

* **Tüm Azure bölgeleri'nde kullanılabilir.** Azure DNS özel bölgeler tüm Azure bölgeleri Azure genel bulutunda kullanılabilir. 


## <a name="capabilities"></a>Özellikler 
* Özel bir bölgeye kayıt sanal ağı olarak bağlı tek bir sanal ağ sanal makinelerden otomatik kaydı. Sanal makineler kayıtlı (eklenir) özel bölgesine kendi özel IP'leri işaret eden bir kayıt olarak olacaktır. Ayrıca, bir sanal makine, sanal ağ silinir kayıt Azure da otomatik olarak ilgili DNS kaydı bağlantılı özel bölgesi'nden kaldırır. Bölgeye göre DNS çözümlemesi herhangi birinden kaydı sanal ağ içindeki sanal makinelerin çalışır, ayrıca varsayılan olarak sanal ağlar kayıt çözümleme sanal ağları olarak hareket unutmayın. 
* Özel bölgesine çözümleme sanal ağları olarak bağlı olan sanal ağlar arasında desteklenen DNS çözümlemesi iletin. DNS çözümlemesi arası sanal ağ için açık bir bağımlılık yoktur sanal ağlar birbirleri ile eşlenen. Ancak, müşteriler sanal ağları diğer senaryolar için eş isteyebilir (örneğin: HTTP trafiği).
* Geriye doğru DNS araması VNET kapsamı içinde desteklenir. Özel bölgeye atanan sanal ağ içindeki özel IP için geriye doğru DNS Arama soneki olarak bölge adı yanı sıra konak/kayıt adı içerir FQDN döndürür. 


## <a name="limitations"></a>Sınırlamalar
* Özel bölge başına 1 kaydı sanal ağ
* Özel bölge başına fazla 10 çözümleme sanal ağlar
* Belirli bir sanal ağdaki tek bir özel bölgesine bir kayıt sanal ağ olarak bağlanabilir
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözüm sanal ağ olarak bağlanabilir.
* Kayıt sanal ağ belirtilirse, bu sanal ağdan özel bölgesine kayıtlı olan VM'ler için DNS kayıtlarını görüntülenebilir veya Powershell/CLI/API'leri gelen alınabilir olmaz, ancak VM kayıtları gerçekten kaydedilir ve çözümler başarıyla
* Geriye doğru DNS kaydı sanal ağında özel IP alanı için yalnızca çalışır
* Özel bölgesinde kayıtlı olmayan bir özel IP için DNS geriye doğru (örn: özel bir bölge çözümleme sanal ağa bağlı bir sanal ağ bir sanal makine için özel IP) "internal.cloudapp.net" DNS son eki olarak ancak döndürür bu soneki çözümlenebilir olmaz.   
* Sanal ağ boş olması gerekir (yani VM kayıt yok) ilk defa (yani ilk kez) özel bölgeye kayıt ya da çözümleme sanal ağı bağlama. Ancak, sanal ağ sonra boş olabilir kayıt ya da çözümleme sanal ağı olarak, diğer özel bölgeler için gelecekteki bağlama. 
* Şu anda koşullu iletme, örneğin Azure ve OnPrem ağları arasında çözümleme etkinleştirmek için desteklenmiyor. Müşteriler bu senaryo başka mekanizmalar aracılığıyla nasıl sağlarsınız hakkında daha fazla belgeleri için lütfen bkz [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

Ayrıca, aynı zamanda okumaya öneririz [SSS](./dns-faq.md#private-dns) bazı ortak sorular ve yanıtlar özel bölgelerinde Azure DNS'de için belirli bir DNS kaydı ve çözümleme davranışı dahil olmak üzere belirli türde operasyonlar bekleyebilirsiniz. 


## <a name="pricing"></a>Fiyatlandırma

Özel DNS bölgeleri genel Önizleme sırasında ücretsiz. Genel kullanılabilirlik sırasında bu özellik bir kullanım tabanlı fiyatlandırma modelini teklifi mevcut Azure DNS için benzer kullanır. 


## <a name="next-steps"></a>Sonraki adımlar

Azure DNS kullanarak özel bir bölge oluşturmayı öğrenin [PowerShell](./private-dns-getstarted-powershell.md) veya [CLI](./private-dns-getstarted-cli.md).

Azure DNS’te Özel Bölgeler ile gerçekleştirilebilecek bazı genel senaryolara ([Özel Bölge senaryoları](./private-dns-scenarios.md)) göz atın.

Üzerinde okuma [SSS](./dns-faq.md#private-dns) bazı ortak sorular ve yanıtlar özel bölgelerinde Azure DNS'de için de dahil olmak üzere belirli bir davranışı belirli türde operasyonlar bekleyebilirsiniz. 

Ziyaret ederek DNS bölgeleri ve kayıtlar hakkında bilgi edinin: [DNS bölgeleri ve genel bakış kayıtları](dns-zones-records.md).

Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.

