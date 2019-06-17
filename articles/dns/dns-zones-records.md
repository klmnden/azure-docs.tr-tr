---
title: DNS bölgelerini ve kayıtlarını genel bakış - Azure DNS | Microsoft Docs
description: DNS bölgeleri ve kayıtları Microsoft Azure DNS'de barındırma desteği genel bakış.
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: victorh
ms.openlocfilehash: 2b9c8f1bb7407dd36623fd8ad68f9489172a1caf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712228"
---
# <a name="overview-of-dns-zones-and-records"></a>DNS bölgeleri ve kayıtları'na genel bakış

Bu sayfa, etki alanları, DNS bölgeleri ve DNS kayıtlarını ve kayıt kümeleri ve Azure DNS'de nasıl desteklenen anahtar kavramlarını açıklar.

## <a name="domain-names"></a>Etki alanı adları

Etki Alanı Adı Sistemi, bir etki alanları hiyerarşisidir. Hiyerarşi, adı yalnızca " **.** " olan "kök" etki alanından başlar.  Bunun altında "com", "net", "org", "uk" veya "jp" gibi en üst düzey etki alanları bulunur.  Bunların altında "org.uk" veya "co.jp" gibi ikinci düzey etki alanları bulunur. DNS hiyerarşisindeki etki alanları genel olarak, dünya genelindeki DNS ad sunucuları tarafından barındırılan dağıtılır.

Bir etki alanı adı kayıt şirketi 'contoso.com' gibi bir etki alanı adı satın almanıza olanak tanıyan bir kuruluştur.  Bir etki alanı adı satın alma, örneğin, şirketinizin web sitesi için 'www.contoso.com' adını doğrudan olanak sağlayan DNS hiyerarşisindeki o adla kontrol etme sunar. Kayıt şirketi etki alanında kendi ad sunucularını sizin adınıza barındırmak veya alternatif ad sunucularını belirtmenizi sağlar.

Azure DNS, etki alanınız barındırmak için kullanabileceğiniz bir Global olarak dağıtılmış, yüksek kullanılabilirlik adı sunucu altyapısı sağlar. Azure DNS etki alanlarınızı barındırarak diğer Azure hizmetlerinde kullandığınız kimlik bilgilerini, API'leri, araçları, faturalandırma ve Destek ile DNS kayıtlarınızı yönetebilirsiniz.

Azure DNS, etki alanı adları satın alımını şu anda desteklemiyor. Bir etki alanı adı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt şirketi kullanmanız gerekir. Kayıt şirketi, genelde küçük bir yıllık ücret uygular. Etki alanlarını Azure DNS'de DNS kayıtlarını yönetimi için ardından barındırılabilir. Ayrıntılar için bkz. [Azure DNS'ye bir etki alanı devretme](dns-domain-delegation.md).

## <a name="dns-zones"></a>DNS bölgeleri

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS kayıtları

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Yaşam süresi

Yaşam süresi veya TTL, ne kadar süreyle her kaydı istemciler tarafından yeniden önce önbelleğe alınacağını belirtir. Yukarıdaki örnekte, TTL 3600 saniye veya 1 saat var.

Bu nedenle bu kayıt kümesindeki tüm kayıtlar için aynı değeri kullanılır Azure DNS'de TTL her kayıt için değil, kayıt kümesi için belirtilir.  1 ile 2.147.483.647 saniye arasında herhangi bir TTL değer belirtebilirsiniz.

### <a name="wildcard-records"></a>Joker karakter kayıtları

Azure DNS [joker kayıtlarını](https://en.wikipedia.org/wiki/Wildcard_DNS_record) destekler. (Bir joker karakter olmayan kayıt kümesinden daha yakın bir eşleşme olmadıkça) joker kayıtlarını yanıt eşleşen bir ada sahip herhangi bir sorgu olarak döndürülür. Azure DNS, joker karakter kaydı kümeleri NS ve SOA dışında tüm kayıt türlerine ait destekler.

Joker karakter kaydı kümesi oluşturmak için kayıt kümesi adını kullanın. '\*'. Alternatif olarak, bir ad ile de kullanabileceğiniz '\*'en soldaki etiket olarak, örneğin,'\*.foo'.

### <a name="caa-records"></a>CAA kayıt

CAA kayıtlarını etki sahipleri, hangi sertifika yetkilileri (CA), etki alanı için sertifikalar yetkisi olduğunu belirtmek izin verin. Bu, bazı durumlarda yanlış sertifika önlemek CA'ları sağlar. CAA kayıtlarını üç özelliklere sahiptir:
* **Bayrakları**: 0 ve 255 başına özel bir anlamı olan kritik bayrağı temsil etmek için kullanılan arasında bir tamsayı budur [RFC](https://tools.ietf.org/html/rfc6844#section-3)
* **Etiket**: ASCII dizisi aşağıdakilerden biri olabilir:
    * **sorunu**: (tüm türleri için) sertifikaları vermek için izin verilen CA belirtmek istiyorsanız bunu kullanın.
    * **issuewild**: (yalnızca joker certs) sertifikaları vermek için izin verilen CA belirtmek istiyorsanız bunu kullanın.
    * **iodef**: bir e-posta adresi veya ana bilgisayar adı için CA'lara yetkisiz sertifika sorunu istekleri bildirebilir belirtin
* **Değer**: seçilen özel etiket değeri

### <a name="cname-records"></a>CNAME kayıtları

CNAME kaydı kümeleri aynı ada sahip diğer kayıt kümeleriyle birlikte olamaz. Örneğin, aynı zamanda "www" göreli adı ile bir CNAME kaydı kümesi göreli adı "www" ve bir A kaydı oluşturamazsınız.

Çünkü bölge tepesinde (ad = '\@') her zaman içeren kümesi, bölge oluşturulduğunda oluşturulan NS ve SOA kaydı, CNAME kaydı bölgenin tepesinde kümesi oluşturulamıyor.

Bu kısıtlamalar DNS standartları kaynaklıdır ve Azure DNS sınırlamaları değildir.

### <a name="ns-records"></a>NS kayıtlarını

NS kayıt kümesi bölgenin tepesindeki (adı '\@') her DNS bölge ile otomatik olarak oluşturulur ve bölge silindiğinde otomatik olarak silinen (ayrı olarak silinemez).

Bu kayıt kümesi bölgesi için atanmış Azure DNS ad sunucularının adlarını içerir. Ek ad sunucuları bu NS kayıt için birden fazla DNS sağlayıcısı ile ortak barındırma etki alanlarını destekleyecek şekilde ayarlama, ekleyebilirsiniz. Bu kayıt kümesi için meta veriler ve TTL de değiştirebilirsiniz. Ancak, kaldırın veya önceden doldurulmuş Azure DNS ad sunucularını değiştirin. 

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerlidir. (Alt bölgeleri temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümeleri oluşturulabilir, değiştirilebilir ve kısıtlama silindi.

### <a name="soa-records"></a>SOA kayıtları

Her bölgenin tepesinde bir SOA kaydı kümesi otomatik olarak oluşturulan (ad = '\@') ve bölge silindiğinde otomatik olarak silinir.  SOA kayıtları oluşturulamıyor veya ayrı olarak silindi.

SOA kaydı, Azure DNS tarafından sağlanan birincil sunucu adının başvurmak için önceden yapılandırılmış olan 'host' özelliği dışında tüm özelliklerini değiştirebilirsiniz.

### <a name="spf-records"></a>SPF kayıtlarının

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV kayıtları

[SRV kayıtlarını](https://en.wikipedia.org/wiki/SRV_record) çeşitli hizmetler tarafından sunucusu konumları belirtmek için kullanılır. Azure DNS'de bir SRV kaydı belirtirken:

* *Hizmet* ve *Protokolü* kayıt kümesi adının alt çizgi ile önek olarak kullanılan bir parçası olarak belirtilmesi gerekir.  Örneğin, '\_SIP.\_ TCP.Name'.  İçin bir kayıt bölgenin tepesindeki belirtmek için gerek yoktur '\@'kayıt adı, yalnızca hizmet ve protokol, örneğin Kullan'\_SIP.\_ TCP'.
* *Öncelik*, *ağırlık*, *bağlantı noktası*, ve *hedef* kayıt kümesindeki her bir kaydın parametreler olarak belirtilir.

### <a name="txt-records"></a>TXT kayıtlarının

TXT kayıtlarının, rastgele metin dizeleri için etki alanı adlarını eşleştirmek için kullanılır. E-posta yapılandırması için özellikle gibi ilgili birden çok uygulamada kullanılan [gönderen ilke Çerçevesi'ı (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) ve [DomainKeys tanımlanan posta (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

DNS standartları, her biri en fazla 254 karakter uzunluğunda olabilir. birden çok dizeyi içeren tek bir TXT kaydı izin verir. Birden çok dizeyi kullanıldığı durumlarda, bunlar istemciler tarafından birleştirilmiş ve tek bir dize olarak kabul edilir.

Azure DNS REST API'si çağrılırken, her bir TXT dizenin ayrı olarak belirtmeniz gerekir.  Azure portalını kullanarak, PowerShell veya CLI arabirimleri 254 karakter parçaya gerekirse otomatik olarak bölünür kayıt başına tek bir dize belirtmeniz gerekir.

DNS kaydındaki birden çok dizeyi birden çok TXT kayıtlarının TXT kayıt kümesindeki ile karıştırılmamalıdır.  Birden çok kayıt içeren bir TXT kayıt kümesi *her biri* birden çok dizeyi içerebilir.  Azure DNS bir toplam dize uzunluğu en fazla 1024 karakter (Birleşik ayarlanmış tüm kayıtları arasında) her bir TXT kaydı destekler.

## <a name="tags-and-metadata"></a>Etiketleri ve meta verileri

### <a name="tags"></a>Tags

Etiketleri ad-değer çiftlerinin listesini ve Azure Resource Manager tarafından kaynakları etiketlemek üzere kullanılır.  Azure Resource Manager Azure faturanızı filtrelenmiş görünümlerini etkinleştirmek için etiketler kullanır ve ayrıca bir ilke etiketleri gerekli ayarlamanıza imkan sağlar. Etiketler hakkında daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).

Azure DNS, DNS bölgesi kaynakları Azure Resource Manager etiketleri kullanarak destekler.  'Metadata' alternatif desteklenir gibi DNS kaydının aşağıda açıklandığı gibi belirler ancak DNS kayıt kümeleri üzerinde etiketleri desteklememektedir.

### <a name="metadata"></a>Meta Veriler

Kayıt kümesi etiketleri alternatif, Azure DNS kayıt kümelerini 'metadata' kullanarak yorumlama destekler.  Benzer şekilde etiketler, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar.  Bu örneğin amacı, her bir kayıt kümesi kayda yararlı olabilir.  Etiketleri farklı meta verileri Azure faturanızı filtrelenmiş bir görünümünü sağlamak için kullanılamaz ve bir Azure Resource Manager ilkesinde belirtilemez.

## <a name="etags"></a>Etag'ler

Aynı anda bir DNS kaydı değiştirmek iki kişinin veya iki işlem ile deneyin varsayalım. Hangisinin kazandı? Ve kazanan başkaları tarafından oluşturulan değişikliklerin üzerine bilir?

Azure DNS, aynı kaynağa eş zamanlı değişiklikleri güvenli bir şekilde işlemek için Etag'ler kullanır. Etag'ler ayrı [Azure Resource Manager 'Etiketleri'](#tags). Her DNS kaynak (bölge ya da kayıt kümesi), kendisiyle ilişkili bir Etag'e sahiptir. Bir kaynak alınan her dosyanın etag değeri de alınır. Bir kaynak güncelleştirirken, Azure DNS, doğrulayabilmeniz için Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Bir kaynağa her bir güncelleştirme yeniden oluşturuluyor Etag sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik meydana geldiğini gösterir. Etag'ler de yeni bir kaynak oluşturulurken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, Azure DNS PowerShell bölgelere eş zamanlı değişiklikleri engellemek ve kaydı kümeleri için Etag'ler kullanır. İsteğe bağlı *-üzerine* anahtar Etag denetimlerini gizlemek için kullanılabilir, eş zamanlı her durumda, gerçekleşen değişikliklerin üzerine yazılır.

Azure DNS REST API düzeyinde Etag'ler HTTP üst bilgilerini kullanarak belirtilir.  Davranışları aşağıdaki tabloda verilmiştir:

| Üstbilgi | Davranış |
| --- | --- |
| None |PUT (herhangi bir Etag denetimi) her zaman başarılı |
| IF-match \<etag > |PUT yalnızca kaynak var ve Etag eşleşiyorsa başarılı |
| IF-match * |Kaynak yoksa PUT yalnızca başarılı |
| IF-none-match * |Kaynak yoksa PUT yalnızca başarılı |


## <a name="limits"></a>Limits

Azure DNS kullanırken aşağıdaki varsayılan sınırlar geçerlidir:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS kullanmaya başlamak için bilgi nasıl [DNS bölgesi oluşturma](dns-getstarted-create-dnszone-portal.md) ve [DNS kayıtları oluşturma](dns-getstarted-create-recordset-portal.md).
* Mevcut bir DNS bölgesini geçirmeyi öğrenin nasıl [DNS bölge dosyasını içeri ve dışarı](dns-import-export.md).
