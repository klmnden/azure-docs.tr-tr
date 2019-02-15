---
title: Azure DNS diğer ad kayıtlarını genel bakış
description: Microsoft Azure DNS diğer ad kayıtlarını desteği genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 52653252df3efd3e12fa974ed82cd2557eee93d0
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301256"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS diğer ad kayıtlarını genel bakış

Azure DNS diğer ad kayıtlarını DNS kayıt kümesi üzerinde nitelikleri ' dir. Bunlar, diğer Azure kaynaklarından DNS bölgenizi içinde başvurabilirsiniz. Örneğin, bir A kaydı yerine bir Azure genel IP adresine başvuruyor bir diğer ad kayıt kümesi oluşturabilirsiniz. Azure genel IP'LERİNDEN diğer ad kaydı kümesi puanları, hizmet örneği dinamik adres. Sonuç olarak, diğer ad kayıt kümesi sorunsuz bir şekilde kendi DNS çözümlemesi sırasında güncelleştirir.

Bir diğer ad kayıt kümesi, aşağıdaki kayıt türlerinin bir Azure DNS bölgesindeki desteklenir: 

- A 
- AAAA 
- CNAME 

> [!NOTE]
> Diğer ad kayıtlarını A veya AAAA kayıt türleri için Azure Traffic Manager için yalnızca dış uç nokta türleri için desteklenir. Dış Traffic Manager'da uç noktaları için uygun şekilde IPv4 veya IPv6 adresi sağlamanız gerekir. İdeal olarak, statik IP adresini kullanın.

## <a name="capabilities"></a>Özellikler

- **Bir genel IP kaynağı için bir DNS A/AAAA kayıt kümesi gelin.** A/AAAA kayıt kümesi oluşturma ve bir genel IP kaynağına işaret edecek bir diğer ad kayıt kümesi kolaylaştırır.

- **Traffic Manager profili için bir DNS A/AAAA/CNAME kayıt kümesi gelin.** Bir DNS CNAME kayıt kümesi için bir Traffic Manager profilinin CNAME işaret edebilir. Contoso.trafficmanager.net buna bir örnektir. Şimdi de dış uç noktalardan gelen bir A veya AAAA kaydı DNS bölgenizi kümesi olan bir Traffic Manager profiline işaret edebilir.

   > [!NOTE]
   > Diğer ad kayıtlarını A veya AAAA kayıt türleri için Traffic Manager için yalnızca dış uç nokta türleri için desteklenir. Dış Traffic Manager'da uç noktaları için uygun şekilde IPv4 veya IPv6 adresi sağlamanız gerekir. İdeal olarak, statik IP adresini kullanın.
   
- **Aynı bölgede başka bir DNS kayıt kümesi gelin.** Diğer ad kayıtları aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesi aynı türde başka bir CNAME kayıt kümesi için bir diğer ad olabilir. Bu düzenleme, diğer adlar olmasını bazı kayıt kümelerini ve bazı diğer olmayan istiyorsanız kullanışlıdır.

## <a name="scenarios"></a>Senaryolar
Diğer ad kayıtlarını için bazı yaygın senaryolar vardır.

### <a name="prevent-dangling-dns-records"></a>DNS kayıtlarını Sallantıdaki engelle
 Azure DNS bölgelerini barındırmasını diğer ad kayıtlarını yakından yaşam döngüsünü Azure kaynakları izlemek için kullanılabilir. Kaynaklar, genel bir IP veya Traffic Manager profili içerir. Geleneksel DNS kayıtları ile ortak bir sorunu kayıtları sarkan. Özellikle A ile bu sorun oluşur/AAAA veya CNAME kayıt türleri. 

Hedef IP veya CNAME artık mevcut değilse, geleneksel bir DNS bölgesi kaydıyla, DNS bölge kaydı bilmez. Sonuç olarak, kayıt el ile güncelleştirilmelidir. Bazı Kurumlarda, bu el ile güncelleştirme zaman içinde gerçekleşebilir değil. Rolleri ve ilişkili izin düzeylerini ayrımı nedeniyle sorunlu da olabilir.

Örneğin, bir rol bir uygulamaya ait bir CNAME veya IP adresini silmek yetkisine sahip olabilir. Ancak bu hedefe işaret eden DNS kaydı güncelleştirmek için yeterli yetkiniz yok. Gecikme süresi arasında IP veya CNAME silinir ve kendisine işaret eden DNS kaydı kaldırıldığında gerçekleşir. Bu gecikme süresi, kesinti kullanıcılar için durum neden.

Diğer ad kayıtlarını bu senaryoyla ilişkili neden olduğu karmaşadan kurtulun. Bunlar Sallantıdaki başvuruları önlemeye yardımcı olun. Örneğin, bir diğer ad kaydı genel bir IP ya da Traffic Manager profili işaret edecek şekilde yetkili bir DNS kaydı'ı alın. Temel alınan kaynaklarla silinirse, aynı anda DNS diğer ad kaydı kaldırılır. Bu işlem, kullanıcıların hiç kesinti etkilese emin olur.

### <a name="update-dns-zones-automatically-when-application-ips-change"></a>Uygulama IP'ler değiştirdiğinizde, DNS bölgelerini otomatik olarak güncelleştirin

Bu senaryo, Öncekine benzer. Belki de bir uygulama taşınır veya temel sanal makine yeniden başlatılır. Bir diğer ad kaydı daha sonra otomatik olarak IP adresi için temel alınan genel IP kaynağı değiştiğinde güncelleştirir. Bu, kullanıcılar eski IP adresine sahip başka bir uygulamaya yönlendiren, olası güvenlik risklerini ortadan kaldırır.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Konak yük dengeli uygulamalarda bölgenin tepesindeki

DNS protokolü, bir A veya AAAA kaydı bölgenin tepesindeki dışında herhangi bir şeyi atamayı engeller. Örneğin: contoso.com. Bu kısıtlama, yük dengeli uygulamalarda arkasında Traffic Manager sahip uygulama sahipleri için bir sorunu gösterir. Traffic Manager profilinde bölgenin tepesinde kaydını işaret etmek mümkün değildir. Sonuç olarak, uygulama sahipleri, geçici bir çözüm kullanmanız gerekir. Uygulama katmanındaki bir yeniden yönlendirme başka bir etki bölge tepesinde yeniden yönlendirmeniz gerekir. Contoso.com yeniden yönlendirme www.contoso.com için buna bir örnektir. Bu düzenleme tek bir yeniden yönlendirme işlevine yönelik bir hata noktası sunar.

Diğer ad kayıtlarını ile bu sorun artık yok. Artık uygulama sahipleri kendi bölge tepesinde kayıt dış uç noktaları olan bir Traffic Manager profiline işaret edebilir. Uygulama sahibi kendi DNS bölgesi içinde başka bir etki alanı için kullanılan aynı Traffic Manager profilini işaret edebilir. Örneğin, contoso.com ve www.contoso.com aynı Traffic Manager profiline işaret edebilir. Traffic Manager profilini yapılandırılmış yalnızca harici son noktaları olduğu sürece bu durum geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Öğretici: Bir Azure genel IP adresini belirtmek için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Bir diğer ad kaydı Apex etki alanı adları ile Traffic Manager'ı destekleyecek şekilde yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
