---
title: Azure DNS diğer ad kayıtlarını genel bakış
description: Microsoft Azure DNS diğer ad kayıtlarını desteği genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 05a6a1700de2b8b334b40d9efd9b8d79e9e7241f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957593"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS diğer ad kayıtlarını genel bakış

Azure DNS diğer ad kayıtlarını nitelik diğer Azure kaynaklarından DNS bölgenizi içinde başvurmak izin veren bir DNS kayıt kümesi üzerinde ' dir. Örneğin, bir A kaydı yerine bir Azure genel IP adresine başvuran bir diğer ad kayıt kümesi oluşturabilirsiniz. Diğer ad kayıt dinamik olarak bir Azure genel IP adresi hizmet örneğine işaret olduğundan, diğer ad kayıt kümesi kendisini sorunsuz bir şekilde DNS çözümlemesi sırasında güncelleştirir.

Aşağıdaki kayıt türlerinin bir Azure DNS bölgesindeki bir diğer ad kayıt kümesi desteklenir: A, AAAA ve CNAME. 

> [!NOTE]
> Diğer ad kayıtlarını A veya AAAA kayıt türleri için Traffic Manager için yalnızca dış uç nokta türleri için desteklenir. IPv4 veya IPv6 adresi (İdeal olarak statik IP) dış Traffic Manager'da uç noktaları için uygun şekilde sağlamanız gerekir.

## <a name="capabilities"></a>Özellikler

- **Bir DNS A/AAAA kayıt kümesi için bir genel IP kaynağı işaret**. A/AAAA kayıt kümesi oluşturma ve bir genel IP kaynağına işaret edecek bir diğer ad kayıt kümesi hale getirebilirsiniz.
- **Traffic Manager profili için bir DNS A/AAAA/CNAME kayıt kümesi noktası**. Traffic Manager profilinin CNAME için işaret edecek şekilde işaretleyebilmesine ek olarak (örneğin: contoso.trafficmanager.net) bir DNS CNAME kayıt kümesinden, şimdi de, DNS'de bir A veya AAAA kayıt kümesinden dış uç noktaları olan bir Traffic Manager profili işaret edebilir bölge.
   > [!NOTE]
   > Diğer ad kayıtlarını A veya AAAA kayıt türleri için Traffic Manager için yalnızca dış uç nokta türleri için desteklenir. IPv4 veya IPv6 adresi (İdeal olarak statik IP) dış Traffic Manager'da uç noktaları için uygun şekilde sağlamanız gerekir.
- **Aynı bölgede başka bir DNS kayıt kümesi işaret**. Diğer ad kayıtlarını aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesi aynı türde başka bir CNAME kayıt kümesi için bir diğer ad olması olabilir. Bu özellik, diğer adlar olabilir. bazı kayıt kümelerini ve bazı davranış açısından diğer adları olarak istiyorsanız kullanışlıdır.

## <a name="scenarios"></a>Senaryolar
Diğer ad kayıtlarını için bazı yaygın senaryolar şunlardır:

### <a name="prevent-dangling-dns-records"></a>DNS kayıtlarını Sallantıdaki engelle
Gelen Azure DNS bölgelerini barındırmasını diğer ad kayıtlarını yakın bir genel IP ve Traffic Manager profili gibi Azure kaynaklarının yaşam döngüsü izlemek için kullanılabilir. Geleneksel DNS kayıtları ile yaygın sorunlardan biri olan "Sallantıdaki kayıtlarla", özellikle A/AAAA veya CNAME kayıt türleri. 

Geleneksel bir DNS bölgesi kaydını hedef IP veya CNAME artık mevcut değilse, DNS bölge kaydı bu durumu kendi lehine olanağıyla sahiptir ve el ile güncelleştirilmesi gerekiyor. Bazı kuruluşlarda el ile bu güncelleştirme, zaman içinde olmayabilir veya rolleri ve ilişkili izin düzeylerini ayrımı nedeniyle sorunlara neden olabilir.

Örneğin, bir uygulamaya ait bir CNAME veya IP adresini silmek için yetkiye sahip rolü bu hedefe işaret eden DNS kaydı güncelleştirmek için yeterli yetkiye sahip olmayabilirsiniz. Sonuç olarak, IP veya CNAME silindiğinde ve onu işaret kaldırılmış, kesinti son kullanıcılar için olası neden DNS kaydı arasında bir gecikme olabilir.

Diğer ad kayıtlarını, bu senaryoyla ilişkili karmaşıklığı ortadan kaldırın ve Sallantıdaki böyle başvurular önlemeye yardımcı olur. Bir DNS kaydı için genel bir IP ya da Traffic Manager profili işaret etmek için bir diğer ad kaydı nitelenmiş ve temel alınan kaynaklarla silinirse, DNS diğer ad kaydı aynı anda hem de kaldırılır. Bu, son kullanıcılar, hiç kesinti etkilese sağlar.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Uygulama IP'ler değiştirdiğinizde, DNS bölgelerini otomatik olarak güncelleştirin

Benzer şekilde, önceki senaryoda bir uygulama veya temel sanal makine yeniden başlatılırsa, bir diğer ad kaydı temel alınan genel IP kaynağı için IP adresinin değiştiği zaman otomatik olarak güncelleştirilir. Son kullanıcılar eski IP adresine sahip başka bir uygulamaya yönlendirilir, olası güvenlik risklerini önleyebilirsiniz.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Konak yük dengeli küme uygulamaları bölgenin tepesindeki

DNS protokolü bölgenin tepesindeki bir A veya AAAA kaydı dışında herhangi bir şeyi atamayı engeller (örnek: contoso.com). Traffic Manager profilinde bölgenin tepesinde kaydını işaret edecek şekilde olası olmadığından bu sunar uygulamaların arkasında bir Traffic Manager yük sahip uygulama sahipleri için sorun dengeli. Sonuç olarak, uygulama sahipleri, geçici bir çözüm kullanmak için zorlanan. Örneğin, başka bir etki alanına (www.contoso.com contoso.com'da) bölge tepesinde yönlendirmek için uygulama katmanında bir yeniden yönlendirme. Bu, tek bir yeniden yönlendirme işlevsellik bakımından hata noktası sunar.

Diğer ad kayıtlarını ile bu sorun artık yok. Uygulama sahibi kendi bölge tepesinde kayıt dış uç noktaları olan bir Traffic Manager profiline şimdi işaret edebilir. Bu özellik ile uygulama sahipleri kendi DNS bölgesi içinde başka bir etki alanı için kullanılan aynı Traffic Manager profilini işaret edebilir. Örneğin, yapılandırılmış yalnızca dış uç noktaları Traffic Manager profiline sahip olduğu sürece contoso.com ve www.contoso.com her ikisi de aynı Traffic Manager profiline işaret edebilir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Öğretici: Azure genel IP adresine başvurmak için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: bir diğer ad kaydı apex etki alanı adları ile Traffic Manager'ı destekleyecek şekilde yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
