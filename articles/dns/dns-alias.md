---
title: Azure DNS diğer ad kayıtlarını genel bakış
description: Microsoft Azure DNS diğer ad kayıtlarını desteği genel bakış.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/7/2019
ms.author: victorh
ms.openlocfilehash: 5dfc00b1193117c22ba1c763bb0e75d9c4712222
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275748"
---
# <a name="azure-dns-alias-records-overview"></a>Azure DNS diğer ad kayıtlarını genel bakış

Azure DNS diğer ad kayıtlarını DNS kayıt kümesi üzerinde nitelikleri ' dir. Bunlar, diğer Azure kaynaklarından DNS bölgenizi içinde başvurabilirsiniz. Örneğin, bir A kaydı yerine bir Azure genel IP adresine başvuruyor bir diğer ad kayıt kümesi oluşturabilirsiniz. Azure genel IP'LERİNDEN diğer ad kaydı kümesi puanları, hizmet örneği dinamik adres. Sonuç olarak, diğer ad kayıt kümesi sorunsuz bir şekilde kendi DNS çözümlemesi sırasında güncelleştirir.

Bir diğer ad kayıt kümesi, aşağıdaki kayıt türlerinin bir Azure DNS bölgesindeki desteklenir: 

- A
- AAAA
- CNAME

> [!NOTE]
> İşaret etmek için bir diğer ad kaydı A veya AAAA kayıt türleri için kullanmak istiyorsanız bir [Azure Traffic Manager profilini](../traffic-manager/quickstart-create-traffic-manager-profile.md) , Traffic Manager profilini yalnızca olduğundan emin olun [dış uç noktalar](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints). Dış Traffic Manager'da uç noktaları için IPv4 veya IPv6 adresi sağlamanız gerekir. Uç noktaların tam etki alanı adlarını (FQDN) kullanamazsınız. İdeal olarak, statik IP adresleri kullanın.

## <a name="capabilities"></a>Özellikler

- **Bir genel IP kaynağı için bir DNS A/AAAA kayıt kümesi gelin.** A/AAAA kayıt kümesi oluşturma ve bir genel IP kaynağına işaret edecek bir diğer ad kayıt kümesi kolaylaştırır. Genel IP adresini değiştirir veya DNS kümesi değişiklikleri otomatik olarak kaydedin. silindi. DNS sarkan yanlış IP adreslerine işaret kayıtları engellendi.

- **Traffic Manager profili için bir DNS A/AAAA/CNAME kayıt kümesi gelin.** A/AAAA oluşturabilir veya CNAME kaydı kümesi ve bir Traffic Manager profiline işaret etmek için diğer ad kayıtlarını kullanın. Bir bölgenin tepesindeki trafiği yönlendirmek gerektiğinde geleneksel CNAME kayıtları bir bölge tepesinde için desteklenmeyen gibi özellikle yararlıdır. Örneğin, Traffic Manager profilinizin myprofile.trafficmanager.net ve iş DNS bölgenizi contoso.com varsayalım. Tür A/AAAA contoso.com (bölge tepesinde) için bir diğer ad kaydı kümesi oluşturma ve myprofile.trafficmanager.net için gelin.
- **Bir Azure Content Delivery Network (CDN) uç noktası**. Azure depolama ve Azure CDN kullanarak statik Web sitesi oluşturduğunuzda, bu yararlıdır.
- **Aynı bölgede başka bir DNS kayıt kümesi gelin.** Diğer ad kayıtları aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesi bir diğer ad başka bir CNAME kaydı kümesi olabilir. Bu düzenleme, diğer adlar olmasını bazı kayıt kümelerini ve bazı diğer olmayan istiyorsanız kullanışlıdır.

## <a name="scenarios"></a>Senaryolar

Diğer ad kayıtlarını için bazı yaygın senaryolar vardır.

### <a name="prevent-dangling-dns-records"></a>DNS kayıtlarını Sallantıdaki engelle

Geleneksel DNS kayıtları ile ortak bir sorunu kayıtları sarkan. IP adresi değişiklikleri yansıtacak şekilde güncelleştirilmedi Örneğin, DNS kayıtları. Özellikle ile bir sorun/AAAA veya CNAME kayıt türleri.

Hedef IP veya CNAME artık mevcut değilse, geleneksel bir DNS bölgesi kaydıyla ilişkili DNS kaydını el ile güncelleştirilmesi gerekir. Bazı Kurumlarda, el ile güncelleştirme işlemi sorunları nedeniyle zaman veya roller ve ilişkili izin düzeylerini ayrımı gerçekleşebilir değil. Örneğin, bir rol bir uygulamaya ait bir CNAME veya IP adresini silmek yetkisine sahip olabilir. Ancak bu hedefe işaret eden DNS kaydı güncelleştirmek için yeterli yetkiniz yok. DNS kaydını güncelleştirme bir gecikme kesinti kullanıcılar için olası neden olabilir.

Diğer ad kayıtlarını sıkı bir şekilde bir Azure kaynağı ile bir DNS kaydı yaşam döngüsünü eşleyerek Sallantıdaki başvuruları engelleyin. Örneğin, bir diğer ad kaydı bir genel IP adresi veya bir Traffic Manager profili işaret edecek şekilde yetkili bir DNS kaydı göz önünde bulundurun. DNS diğer ad kaydı, temel alınan kaynaklarla silerseniz, boş bir kayıt kümesi haline gelir. Artık, silinen kaynağa başvuruyor.

### <a name="update-dns-record-set-automatically-when-application-ip-addresses-change"></a>Uygulama IP adresleri değiştiğinde DNS kayıt kümesi otomatik olarak güncelleştir

Bu senaryo, Öncekine benzer. Belki de bir uygulama taşınır veya temel sanal makine yeniden başlatılır. Bir diğer ad kaydı daha sonra otomatik olarak IP adresi için temel alınan genel IP kaynağı değiştiğinde güncelleştirir. Bu, kullanıcılar eski genel IP adresi atanmış olan başka bir uygulamaya yönlendiren, olası güvenlik risklerini ortadan kaldırır.

### <a name="host-load-balanced-applications-at-the-zone-apex"></a>Konak yük dengeli uygulamalarda bölgenin tepesindeki

DNS protokolü, bölge tepesinde CNAME kayıtları atanmasını engeller. Örneğin; etki alanı contoso.com ise CNAME kayıtlarını somelabel.contoso.com oluşturabilirsiniz; ancak CNAME contoso.com için oluşturamazsınız.
Bu kısıtlama sahip yük dengeli uygulamalarda uygulama sahipleri için sorun oluşturur ardında [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Bir Traffic Manager profili kullanarak bir CNAME kaydı oluşturulmasını gerektirdiğinden, bölge tepesinde Traffic Manager profilinde işaret etmek mümkün değildir.

Bu sorun, diğer ad kayıtlarını kullanan çözülür. CNAME kayıtları aksine bölgenin tepesinde diğer ad kayıtlarını oluşturulur ve uygulama sahipleri kendi bölge tepesinde kayıt dış uç noktaları olan bir Traffic Manager profiline işaret edecek şekilde kullanabilirsiniz. Uygulama sahibi kendi DNS bölgesi içinde başka bir etki alanı için kullanılan aynı Traffic Manager profilini gelin.

Örneğin, contoso.com ve www\.contoso.com aynı Traffic Manager profiline işaret edebilir. Diğer ad kayıtlarını ile Azure Traffic Manager profillerini kullanma hakkında daha fazla bilgi edinmek için sonraki adımlar bölümüne bakın.

### <a name="point-zone-apex-to-azure-cdn-endpoints"></a>Bölge tepesinde Azure CDN uç noktası

Yalnızca bir Traffic Manager profili gibi diğer ad kayıtlarını, DNS bölge tepesinde Azure CDN uç noktası için de kullanabilirsiniz. Azure depolama ve Azure CDN kullanarak statik Web sitesi oluşturduğunuzda, bu yararlıdır. Web sitesi daha sonra DNS adı "www" eklenmesini olmadan da erişebilirsiniz.

Örneğin, www.contoso.com, statik Web sitesi ise, kullanıcılarınızın sitenizi gerek kalmadan contoso.com DNS adına www önüne eklediğinizden kullanarak erişebilirsiniz.

Daha önce açıklandığı gibi bölge tepesinde CNAME kayıtları desteklenmez. Bu nedenle, bir CNAME kaydı, contoso.com CDN uç noktanıza işaret edecek şekilde kullanamazsınız. Bunun yerine, bölge tepesinde doğrudan CDN uç noktası için bir diğer ad kaydı kullanabilirsiniz.

> [!NOTE]
> Bölge tepesinde için CDN uç noktası için Azure CDN Akamai işaret eden şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Diğer ad kayıtları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Öğretici: Bir Azure genel IP adresini belirtmek için bir diğer ad kaydı yapılandırma](tutorial-alias-pip.md)
- [Öğretici: Bir diğer ad kaydı Apex etki alanı adları ile Traffic Manager'ı destekleyecek şekilde yapılandırma](tutorial-alias-tm.md)
- [DNS SSS](https://docs.microsoft.com/azure/dns/dns-faq#alias-records)
