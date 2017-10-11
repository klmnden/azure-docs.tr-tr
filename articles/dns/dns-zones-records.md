---
title: "DNS bölgeleri ve kayıtları genel bakış - Azure DNS | Microsoft Docs"
description: "DNS bölgeleri ve Microsoft Azure DNS kayıtlarını barındırmak için destek genel bakış."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: jonatul
ms.openlocfilehash: 5818986c939c464a364c52ab31225e15130ab30e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-dns-zones-and-records"></a>DNS bölgeleri ve kayıtları'na genel bakış

Bu sayfa, etki alanları, DNS bölgeleri ve DNS kayıtlarını ve kayıt kümelerini ve Azure DNS'de nasıl desteklenen anahtar kavramlarını açıklar.

## <a name="domain-names"></a>Etki alanı adları

Etki Alanı Adı Sistemi, bir etki alanları hiyerarşisidir. Hiyerarşi, adı yalnızca "**.**" olan "kök" etki alanından başlar.  Bunun altında "com", "net", "org", "uk" veya "jp" gibi en üst düzey etki alanları bulunur.  Bunların altında "org.uk" veya "co.jp" gibi ikinci düzey etki alanları bulunur. DNS hiyerarşisindeki etki alanları genel olarak, dünya genelindeki DNS ad sunucuları tarafından barındırılan dağıtılır.

Bir etki alanı adı kayıt "contoso.com" gibi bir etki alanı adı satın almanızı sağlayan bir kuruluş olan.  Bir etki alanı adı satın Örneğin, şirketinizin web sitesi için 'www.contoso.com' adı doğrudan olanak tanıyan DNS hiyerarşi o adla denetim hakkı verir. Kayıt şirketi ana sizin adınıza kendi ad sunucuları, etki alanı veya diğer ad sunucularını belirtmenizi sağlar.

Azure DNS, etki alanınızın barındırmak için kullanabileceğiniz bir genel dağıtılmış, yüksek kullanılabilirlik adı sunucu altyapısı sağlar. Azure DNS'de etki alanlarınızı barındırarak, diğer Azure hizmetleriyle DNS kayıtlarınızı aynı kimlik bilgilerini, API'leri, Araçlar, faturalama ve Destek ile yönetebilirsiniz.

Azure DNS, etki alanı adlarını satın alma şu anda desteklemiyor. Bir etki alanı adı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt kullanmanız gerekebilir. Kayıt, genellikle küçük bir yıllık ücret ister. Etki alanları için DNS kayıtlarını Yönetimi sonra Azure DNS'de barındırılabilir. Bkz: [bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md) Ayrıntılar için.

## <a name="dns-zones"></a>DNS bölgeleri

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>DNS kayıtları

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Yaşam süresi

Yaşam süresi veya TTL, yeniden sorgulanmadan önce istemci tarafından ne süreyle önbelleğe alınacağını belirtir. Yukarıdaki örnekte, TTL 3600 saniye veya 1 saat değil.

Bu kayıt kümesindeki tüm kayıtlar için aynı değeri kullanılmak üzere Azure DNS'de TTL her kayıt için kayıt kümesi için belirtilmiş.  Herhangi bir TTL değeri 1 ile 2.147.483.647 saniye arasında bir değer belirtebilirsiniz.

### <a name="wildcard-records"></a>Joker karakter kayıtları

Azure DNS [joker kayıtlarını](https://en.wikipedia.org/wiki/Wildcard_DNS_record) destekler. (Bir joker karakter olmayan kayıt kümesinden daha yakın bir eşleşme olmadıkça) joker kayıtlarını yanıt adla eşleşen herhangi bir sorgu olarak döndürülür. Azure DNS joker karakter kaydı kümeleri NS ve SOA dışında tüm kayıt türleri için destekler.

Joker karakter kaydı kümesi oluşturmak için kayıt kümesi adını kullanın '\*'. Alternatif olarak, aynı zamanda bir adla kullanabilirsiniz '\*'olarak kendi en solundaki etiketle, örneğin,'\*.foo'.

### <a name="cname-records"></a>CNAME kayıtları

CNAME kaydı kümeleri aynı ada sahip diğer kayıt kümeleriyle birlikte olamaz. Örneğin, aynı anda göreli adına sahip 'www' kümesi göreli adı 'www' ile bir CNAME kaydı ve bir A kaydı oluşturamazsınız.

Çünkü bölge tepesinde (ad = ' @') her zaman içeren bölge oluşturulduğunda oluşturulan kümeleri NS ve SOA kaydı, bölgenin tepesinde ayarlamak bir CNAME kaydı oluşturamazsınız.

Bu kısıtlamalar DNS standartları kaynaklıdır ve Azure DNS sınırlamaları değildir.

### <a name="ns-records"></a>NS kayıtları

NS kayıt kümesinde bölge tepesinde (ad ' @') her DNS bölge ile otomatik olarak oluşturulur ve bölge silindiğinde otomatik olarak silinir (ayrı olarak silinemez).

Bu kayıt kümesi bölgeye atanan Azure DNS ad sunucularının adlarını içerir. Birden fazla DNS sağlayıcınız ile birlikte barındırma etki alanlarını destekleyecek şekilde bu NS kaydının sunucularına ayarlayın ek ad ekleyebilirsiniz. Bu kayıt kümesi için meta verileri ve TTL de değiştirebilirsiniz. Ancak, kaldırmak veya önceden doldurulmuş haldedir Azure DNS ad sunucuları değiştirin. 

Bu bölge tepesinde yalnızca NS kayıt kümesi için geçerli olduğunu unutmayın. (Alt bölgelere temsilci seçmek için kullanıldığı şekilde), bu bölgedeki diğer NS kayıt kümelerini oluşturulan, değiştirilebilir ve kısıtlama silinir.

### <a name="soa-records"></a>SOA kayıtları

Her bölgenin tepesinde bir SOA kayıt kümesini otomatik olarak oluşturulan (ad = ' @') ve bölge silindiğinde otomatik olarak silinir.  SOA kayıtları oluşturulamıyor veya ayrı olarak silinir.

SOA kaydı Azure DNS tarafından sağlanan birincil sunucu adı başvurmak üzere önceden yapılandırılmıştır 'ana' özellik dışında tüm özelliklerini değiştirebilirsiniz.

### <a name="spf-records"></a>SPF kayıtlarının

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>SRV kayıtları

[SRV kayıtları](https://en.wikipedia.org/wiki/SRV_record) çeşitli hizmetler tarafından sunucu konumlarını belirtmek için kullanılır. Bir SRV kaydı Azure DNS'de belirtirken:

* *Hizmet* ve *Protokolü* alt çizgi ile önek kayıt kümesi adı, bir parçası olarak belirtilmesi gerekir.  Örneğin, '\_SIP.\_ TCP.Name'.  Bölge tepesinde bir kaydı için belirtmek için gerek yoktur ' @' kayıt adı yalnızca hizmet ve protokol, örneğin kullanma '\_SIP.\_ TCP'.
* *Öncelik*, *ağırlık*, *bağlantı noktası*, ve *hedef* kayıt kümesindeki her bir kaydı parametrelerinin olarak belirtilir.

### <a name="txt-records"></a>TXT kayıtları

TXT kayıtlarının rastgele metin dizelerini etki alanı adlarını eşleştirmek için kullanılır. E-posta yapılandırması için özellikle gibi ilgili birden çok uygulamalarda kullanılır [gönderen ilke Çerçevesi'ı (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) ve [DomainKeys tanımlanan posta (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

DNS standartları her biri en fazla 254 karakter uzunluğunda olabilir, birden çok dizeyi içeren tek bir TXT kaydı izin verir. Birden çok dizenin kullanıldığı durumlarda, bunlar istemciler tarafından birleştirilmiş ve tek bir dize kabul edilir.

Azure DNS REST API'si çağrılırken, her bir TXT dize ayrı olarak belirtmeniz gerekir.  Azure Portalı'nı kullanırken, PowerShell veya CLI arabirimleri 254 karakter parçaya gerekirse, otomatik olarak bölünür kayıt başına tek bir dize belirtmeniz gerekir.

Bir DNS kaydı birden çok dizelerde TXT kayıt kümesi içinde birden çok TXT kayıt ile karıştırılmamalıdır.  TXT kayıt kümesi birden fazla kayıt içerebilir *her biri* birden çok dizenin içerebilir.  Azure DNS, toplam dize uzunluğu en çok 1024 karakter (Birleşik ayarlanmış tüm kayıtları arasında) her bir TXT kaydı destekler.

## <a name="tags-and-metadata"></a>Etiketleri ve meta veriler

### <a name="tags"></a>Etiketler

Etiketleri ad-değer çiftlerinin listesini ve Azure Resource Manager tarafından kaynakları etiketlemek üzere kullanılır.  Azure Resource Manager Azure faturasını filtrelenmiş görünümlerini etkinleştirmek için etiketleri kullanır ve ayrıca etiketleri gereklidir ilkesi ayarlamanıza olanak sağlar. Etiketler hakkında daha fazla bilgi için bkz. [Etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).

Azure DNS, DNS bölge kaynakları Azure Resource Manager etiketleri kullanarak destekler.  Alternatif 'meta verileri' desteklenir gibi DNS kaydı aşağıda açıklandığı gibi ayarlar ancak DNS kayıt kümelerini üzerinde etiketleri desteklemez.

### <a name="metadata"></a>Meta Veriler

Kayıt kümesi etiketleri alternatif olarak, Azure DNS kayıt kümelerini 'meta verileri' kullanarak açıklanmasını destekler.  Benzer şekilde etiketleri, meta verileri, ad-değer çiftleri her bir kayıt kümesi ile ilişkilendirmek sağlar.  Bu örneğin kayıt her kayıt kümesi amacı yararlı olabilir.  Etiketler, aksine meta verileri Azure faturasını filtre uygulanmış bir görünümünü sağlamak için kullanılamaz ve bir Azure Resource Manager ilkesinde belirtilemez.

## <a name="etags"></a>Etag'ler

İki kişinin veya iki işlemler aynı anda bir DNS kaydı değiştirmeye çalıştığınızda varsayalım. Hangisinin WINS? Ve başkaları tarafından oluşturulan değişikliklerin üzerine kazanan bilir?

Azure DNS Etag'ler aynı kaynak eşzamanlı değişiklikleri güvenli bir şekilde işlemek için kullanır. Etag'ler ayrı [Azure Resource Manager 'Etiketleri'](#tags). Her DNS kaynak (bölge veya kayıt kümesi) ilişkili bir ETag değerine sahip. Bir kaynak alınır olduğunda, kendi Etag de alınır. Bir kaynak güncelleştirirken Azure DNS, doğrulayabilmeniz için Etag sunucu eşleşmeleri ile Etag geri geçirmek seçebilirsiniz. Yeniden Etag bir kaynağa her bir güncelleştirme sonuçları olduğundan, bir Etag uyuşmazlığı eşzamanlı bir değişiklik oluştuğunu gösterir. Etag'ler da yeni bir kaynak oluştururken kaynak zaten yoksa emin olmak için kullanılabilir.

Varsayılan olarak, Azure DNS PowerShell Etag'ler bölgelere eşzamanlı değişiklikleri engellemek ve kayıt kümeleri için kullanır. İsteğe bağlı *-üzerine* anahtar Etag denetimlerini gizlemek için kullanılabilir, her eşzamanlı durumda oluşan değişikliklerin üzerine yazılır.

Azure DNS REST API düzeyinde Etag'ler HTTP üstbilgileri kullanılarak belirtilir.  Davranışlarını aşağıdaki tabloda verilmiştir:

| Üstbilgi | Davranışı |
| --- | --- |
| None |PUT (Etag denetimleri) her zaman başarılı |
| IF-match<etag> |PUT yalnızca kaynak var ve Etag eşleşiyorsa başarılı |
| IF-match * |Kaynak varsa PUT yalnızca başarılı |
| IF-none-match * |Kaynak yoksa, PUT yalnızca başarılı |


## <a name="limits"></a>Sınırlar

Azure DNS kullanarak aşağıdaki varsayılan sınırları uygulayın:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Azure DNS kullanmaya başlamak için bilgi nasıl [bir DNS bölgesi oluşturma](dns-getstarted-create-dnszone-portal.md) ve [DNS kayıtlarını oluşturun](dns-getstarted-create-recordset-portal.md).
* Varolan bir DNS bölgesi geçirmek için bilgi nasıl [içeri ve dışarı aktarma bir DNS bölge dosyasına](dns-import-export.md).
