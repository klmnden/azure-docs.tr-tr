---
title: Azure DNS temsilcisine genel bakış
description: Etki alanı temsilcisi seçiminin nasıl değiştirileceğini ve etki alanı barındırma sağlamak üzere Azure DNS ad sunucularının nasıl kullanılacağını anlayın.
services: dns
author: WenJason
ms.service: dns
origin.date: 2/19/2019
ms.date: 03/04/2019
ms.author: v-jay
ms.openlocfilehash: 70c1c1ab0dd5d1998054cf0c68325022803dff06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60563155"
---
# <a name="delegation-of-dns-zones-with-azure-dns"></a>Azure DNS ile DNS bölgelerinin temsilciliği

Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Bir etki alanının DNS sorgularının Azure DNS'ye erişmesi için, etki alanının Azure DNS'ye üst etki alanından devredilmiş olması gerekir. Azure DNS'nin etki alanı kayıt şirketi olmadığını unutmayın. Bu makalede, etki alanı temsilcisi seçmenin nasıl çalıştığı ve etki alanlarının Azure DNS'ye nasıl devredildiği açıklanmaktadır.

## <a name="how-dns-delegation-works"></a>DNS temsilcisi seçme nasıl çalışır?

### <a name="domains-and-zones"></a>Etki alanları ve bölgeler

Etki Alanı Adı Sistemi, bir etki alanları hiyerarşisidir. Hiyerarşi, adı yalnızca "**.**" olan "kök" etki alanından başlar.  Bunun altında "com", "net", "org", "uk" veya "jp" gibi en üst düzey etki alanları bulunur.  Bu üst düzey etki alanlarının altında ‘org.uk’ veya ‘co.jp’ gibi ikinci düzey etki alanları bulunur.  Etki alanları bu hiyerarşi sıralamasıyla devam eder. DNS hiyerarşisindeki etki alanları, ayrı DNS bölgeleri kullanılarak barındırılır. Bu bölgeler global olarak dağıtılır ve dünya genelindeki DNS ad sunucuları tarafından barındırılır.

**DNS bölgesi** - Etki alanı, Etki Alanı Adı Sistemi'nde yer alan "contoso.com" gibi benzersiz bir addır. DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Örneğin ‘contoso.com’ etki alanı, ‘mail.contoso.com’ (bir posta sunucusu için) ve ‘www.contoso.com’ (bir web sitesi için) gibi birden fazla DNS kaydını içerebilir.

**Etki alanı kayıt şirketi** - Etki alanı kayıt şirketi, İnternet etki alanı adlarını sağlayabilen bir şirkettir. Bu şirketler kullanmak istediğiniz İnternet etki alanının kullanılabilir olup olmadığını doğrular ve bu etki alanını satın almanızı sağlar. Etki alanı adı kaydedildikten sonra, etki alanı adının yasal sahibi olursunuz. Bir İnternet etki alanına zaten sahipseniz Azure DNS'ye devretmek için geçerli etki alanı kayıt şirketini kullanırsınız.

Akredite bir etki alanı kayıt hakkında daha fazla bilgi için bkz: [ICANN-Accredited kaydedicilerin](https://www.icann.org/registrar-reports/accredited-list.html).

### <a name="resolution-and-delegation"></a>Çözümleme ve temsilci seçme

İki tür DNS sunucusu bulunur:

* *Yetkili* DNS sunucusu DNS bölgelerini barındırır. Bu sunucu, yalnızca bu bölgelerdeki kayıtlar için DNS sorgularını yanıtlar.
* *Özyinelemeli* DNS sunucusu DNS bölgelerini barındırmaz. Bu sunucu, tüm DNS sorgularını yanıtlamak için yetkili DNS sunucularını çağırarak ihtiyacı olan verileri toplar.

Azure DNS, bir yetkili DNS hizmeti sağlar.  Bir özyinelemeli DNS hizmeti sağlamaz. Azure’daki Cloud Services ve Sanal Makineler, Azure altyapısının bir parçası olarak ayrıca sağlanan, özyinelemeli bir DNS hizmetini kullanacak şekilde otomatik olarak yapılandırılmıştır. Bu DNS ayarlarını değiştirme hakkında bilgi edinmek için bkz. [Azure’da Ad Çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

Bilgisayarlar veya mobil cihazlarda bulunan DNS istemcileri, istemci uygulamaları için gereken tüm DNS sorgularını gerçekleştirmek için genellikle özyinelemeli bir DNS sunucusu çağırır.

Özyinelemeli bir DNS sunucusu "www.contoso.com" gibi bir DNS kaydı için bir sorguyu aldığında öncelikle "contoso.com" etki alanı için bölgeyi barındıran ad sunucusunu bulması gerekir. Ad sunucusunu bulmak için kök adı sunucularından başlar ve buradan ‘com’ bölgesini barındıran ad sunucularını bulur. Ardından, "contoso.com" bölgesini barındıran ad sunucularını bulmak için "com" ad sunucularını sorgular.  Son olarak, bu ad sunucularını "www.contoso.com" için sorgulayabilir.

Bu yordam DNS adını çözümleme olarak adlandırılır. Net olarak ifade etmek gerekirse DNS çözümlemesi CNAME'leri izleme gibi ek adımları içerir ancak DNS temsilcisi seçmenin nasıl çalıştığını anlamak için bu önemli değildir.

Bir üst bölge, bir alt bölgenin ad sunucularına nasıl "işaret eder"? Bunu, NS kaydı olarak adlandırılan (NS "ad sunucusu" anlamına gelir) özel bir DNS kaydı türünü kullanarak yapar. Örneğin, kök bölgesi "com" için NS kayıtları içerir ve "com" bölgesi için ad sunucularını gösterir. Buna karşılık "com" bölgesi, "contoso.com" bölgesi için ad sunucularını gösteren "contoso.com"un NS kayıtlarını içerir. Bir üst bölge içindeki bir alt bölge için NS kayıtlarının ayarlamasına etki alanını devretme adı verilir.

Aşağıdaki resimde örnek bir DNS sorgusu gösterilir. Contoso.net ve partners.contoso.net, Azure DNS bölgeleridir.

![Dns-nameserver](./media/dns-domain-delegation/image1.png)

1. İstemci, yerel DNS sunucusundan `www.partners.contoso.net` ister.
2. Yerel DNS sunucusunda kayıt yoktur, dolayısıyla kendi kök ad sunucusundan istekte bulunur.
3. Kök ad sunucusunda kayıt yoktur, ama `.net` ad sunucusunun adresini bilir ve bu adresi DNS sunucusuna sağlar
4. Yerel DNS sunucusu isteği gönderir `.net` ad sunucusu.
5. `.net` Ad sunucusu kaydı yok ancak adresini bilir `contoso.net` ad sunucusu. Bu durumda, Azure DNS'de barındırılan DNS bölgesi için ad sunucusu adresi ile yanıt verir.
6. Yerel DNS sunucusu için ad sunucusu isteği gönderir `contoso.net` Azure DNS'de barındırılan bir bölge.
7. Bölge `contoso.net` kayıt yoktur ancak için ad sunucusu bilir `partners.contoso.net` ve adresiyle yanıt verir. Bu durumda, Azure DNS'de barındırılan bir DNS bölgesi olur.
8. Yerel DNS sunucusu için ad sunucusu isteği gönderir `partners.contoso.net` bölge.
9. `partners.contoso.net` Bölgesi A kaydı ve IP adresiyle yanıt verir.
10. Yerel DNS sunucusunun IP adresini istemciye sağlar.
11. İstemci `www.partners.contoso.net` web sitesine bağlanır.

Her temsilci seçimi aslında NS kayıtlarının iki kopyasını içerir, bunlardan biri üst bölgede bulunup alt bölgeyi işaret ederken diğeri de alt bölgede yer alır. "Contoso.net" bölgesi, "contoso.net"e ait NS kayıtlarını içerir ("net"teki NS kayıtlarına ek olarak). Bu kayıtlar yetkili NS kayıtları olarak adlandırılır ve alt bölgenin tepesinde durur.

## <a name="next-steps"></a>Sonraki adımlar

[Etki alanınızı Azure DNS'e atamayı](dns-delegate-domain-azure-dns.md) öğrenin

