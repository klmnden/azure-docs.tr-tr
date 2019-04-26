---
title: Azure ters DNS'ye genel bakış | Microsoft Docs
description: Nasıl geriye doğru DNS çalışır ve Azure'da nasıl kullanılabileceğini öğrenin
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: victorh
ms.openlocfilehash: 9d3a62ec1c9ede1f25f2b53f800642a792b3aa28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60192991"
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Ters DNS ve Azure desteği'na genel bakış

Bu makalede, Azure'da desteklenen geriye doğru DNS senaryoları ve nasıl geriye doğru DNS works genel bakış verilmektedir.

## <a name="what-is-reverse-dns"></a>Ters DNS nedir?

Geleneksel DNS kayıtları bir DNS adı (örneğin, 'www.contoso.com') bir IP adresi (örneğin, 64.4.6.100) için ana kadar eşlemeyi etkinleştirin.  Ters DNS çeviri geri bir adı ('www.contoso.com') için bir IP adresi (64.4.6.100) sağlar.

Ters DNS kayıtlarını çeşitli durumlarda kullanılır. Örneğin, ters DNS kayıtlarını yaygın istenmeyen e-posta gönderen bir e-posta iletisinin doğrulayarak mücadele içinde kullanılır.  Alıcı e-posta sunucusuna gönderen sunucusunun IP adresinin ters DNS kaydı alır ve barındıran kaynak etki alanından e-posta göndermek için yetkili olup olmadığını doğrular. 

## <a name="how-reverse-dns-works"></a>Nasıl geriye doğru DNS çalışır

Ters DNS kayıtlarını 'ARPA' bölgeleri bilinen özel DNS bölgeleri içinde barındırılır.  Bu bölgeler, 'contoso.com' gibi etki alanı barındırma normal hiyerarşi ile paralel ayrı bir DNS hiyerarşi oluşturur.

Örneğin, DNS kayıt 'www.contoso.com', 'contoso.com' bölgesindeki "www" adı ile bir DNS 'A' kaydı kullanılarak uygulanır.  Bu bir kayıt, bu durumda 64.4.6.100 karşılık gelen IP adresine işaret eder.  Geriye doğru arama, '100' bölgesinde '6.4.64.in-addr.arpa' (IP adresleri ARPA bölgeleri ters gerektiğini unutmayın.) adlı bir 'PTR' kaydı kullanılarak ayrı ayrı uygulanır  Doğru bir şekilde yapılandırılmışsa bu PTR kaydı adı "www.contoso.com" işaret eder.

Bunlar ayrıca bir kuruluş bir IP adres bloğu atandığında, karşılık gelen ARPA bölgesini yönetme hakkı edinin. Azure tarafından kullanılan IP adres blokları karşılık gelen ARPA bölgeleri barındırılan ve Microsoft tarafından yönetilir. ISS ARPA bölgesini kendi IP adresleri için sizin için barındırabilir veya Azure DNS gibi tercih ettiğiniz bir DNS hizmetinde ARPA bölgesini barındırmanıza olanak tanıyabilir.

> [!NOTE]
> İleriye doğru DNS araması ve geriye doğru DNS araması ayrı, paralel DNS hiyerarşileri uygulanır. "Www.contoso.com" için geriye doğru arama olup **değil** "contoso.com" bölgesinde barındırılan, bunun yerine, karşılık gelen IP adresi bloğunun ARPA bölgesini barındırılır. Ayrı bölgeler için IPv4 ve IPv6 adres bloklarını kullanılır.

### <a name="ipv4"></a>IPv4

Bir IPv4 geriye doğru arama bölgesi adı şu biçimde olmalıdır: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Örneğin, ana bilgisayar kayıtları bir geriye doğru arama bölgesine IP'leri ile 192.0.2.0/24 önekini konaklar için oluştururken, bölge adı ağ ön eki (192.0.2) adresinin yalıtma ve sonra ters sırada (2.0.192) ve sonekiekleyerekoluşturulacak`.in-addr.arpa`.

|Alt ağ sınıfı|Ağ ön eki  |Ters çevrilen ağ ön eki  |Standart soneki  |Geriye doğru arama bölgesi adı |
|-------|----------------|------------|-----------------|---------------------------|
|Sınıf A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|B sınıfı|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|C sınıfı|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Sınıfsız IPv4 temsilci seçme

Bazı durumlarda, bir kuruluş için ayrılan IP aralığını bir sınıf C küçüktür (/ 24) aralığı. Bu durumda, IP aralığını bir bölge sınırı içinde üzerinde düşmüyorsa `.in-addr.arpa` bölge hiyerarşisi ve bu nedenle bir alt bölge temsil edilemez.

Bunun yerine, farklı bir mekanizma, bir özel DNS bölgesi için bireysel geriye doğru arama (PTR) kayıtlarının denetim aktarmak için kullanılır. Bu mekanizma her IP aralığı için bir alt bölge atar ve ardından her IP adresi aralığı, CNAME kayıtları kullanarak bu alt bölge için ayrı ayrı eşler.

Örneğin, bir kuruluşun kendi ISP'ye göre IP aralığı 192.0.2.128/26 verilir varsayalım. Bu 64 IP adresleri arasında 192.0.2.191 için 192.0.2.128 temsil eder. Bu aralık için ters DNS şu şekilde gerçekleştirilir:
- Kuruluş 128 26.2.0.192.in addr.arpa adlı bir geriye doğru arama bölgesi oluşturur. ' 128-26' öneki, kuruluş sınıfı C içinde atanan ağ kesimini temsil eder (/ 24) aralığı.
- ISS sınıfı C üst bölgeden yukarıdaki bölgesi için DNS temsilcisini ayarlamak için NS kayıtları oluşturur. Ayrıca CNAME kayıtları üst (sınıfı C) geriye doğru arama bölgesi ' her IP adresi, IP aralığı kuruluş tarafından oluşturulan yeni bölge eşleme oluşturur:

```
$ORIGIN 2.0.192.in-addr.arpa
; Delegate child zone
128-26    NS       <name server 1 for 128-26.2.0.192.in-addr.arpa>
128-26    NS       <name server 2 for 128-26.2.0.192.in-addr.arpa>
; CNAME records for each IP address
129       CNAME    129.128-26.2.0.192.in-addr.arpa
130       CNAME    130.128-26.2.0.192.in-addr.arpa
131       CNAME    131.128-26.2.0.192.in-addr.arpa
; etc
```
- Kuruluş, kendi alt bölge içerisindeki tek PTR kayıtları yönetir.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
Geriye doğru arama bir PTR kaydı için IP adresi '192.0.2.129' sorgular için '129.2.0.192.in-addr.arpa' adlı. Üst bölgeden alt bölgeye PTR kaydı CNAME aracılığıyla bu sorguyu çözümler.

### <a name="ipv6"></a>IPv6

Bir IPv6 geriye doğru arama bölgesi adı şu biçimde olmalıdır: `<IPv6 network prefix in reverse order>.ip6.arpa`

Örneğin. Konaklar için ana bilgisayar kayıtları için bir geriye doğru arama bölgesi ile IP'ler, oluşturma olduğunda içinde 2001:db8:1000:abdc:: / 64 ön eki, bölge adı oluşturulacaktır adresi öneki yalıtarak (2001:db8:abdc::). Sonraki kaldırmak için IPv6 öneki genişletin [sıfır sıkıştırma](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), IPv6 adres ön eki kısaltmak için kullanıldıysa (2001:0db8:abdc:0000::). Ters çevrilen ağ ön eki oluşturmak için her ön ek, onaltılık sayı arasında sınırlayıcı olarak nokta kullanarak sırada (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) ve soneki eklemek `.ip6.arpa`.


|Ağ ön eki  |Genişletilmiş ve ters ağ ön eki |Standart soneki |Geriye doğru arama bölgesi adı  |
|---------|---------|---------|---------|
|2001:db8:abdc::/64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | .ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102::/64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | .ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Ters DNS için Azure desteği

Azure, ters DNS ile ilgili iki ayrı senaryoları destekler:

**Karşılık gelen, IP adresi bloğuna geriye doğru arama bölgesi barındırma.**
Azure DNS için kullanılabilir [, geriye doğru arama bölgeleri barındırma ve her geriye doğru DNS araması için PTR kayıtlarını yönetme](dns-reverse-dns-hosting.md), IPv4 ve IPv6 için.  (ARPA) geriye doğru arama bölgesi oluşturma, temsilci seçmeyi ayarlama ayarlama ve PTR kayıtlarını yapılandırma işlemi normal DNS bölgeleri ile aynıdır.  Tek fark, temsilci DNS kayıt şirketinizde yerine ISS'niz yapılandırılmalıdır ve yalnızca PTR kaydı türü kullanılmalıdır vardır.

**Ters DNS kaydı, Azure hizmetine atanmış IP adresi için yapılandırın.** Azure, sağlar [yapılandırmak için Azure service ayrılan IP adresleri için geriye doğru arama](dns-reverse-dns-for-azure-services.md).  Bu geriye doğru arama, Azure tarafından karşılık gelen ARPA bölgesini bir PTR kaydı olarak yapılandırılır.  Microsoft tarafından barındırılan, Azure tarafından kullanılan tüm IP aralıklarını karşılık gelen bu ARPA bölgeleri

## <a name="next-steps"></a>Sonraki adımlar

Ters DNS hakkında daha fazla bilgi için bkz. [Wikipedia geriye doğru DNS araması](https://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [barındırmak için ISS atanmış IP aralığınızı Azure DNS'de geriye doğru arama bölgesi](dns-reverse-dns-for-azure-services.md).
<br>
Bilgi edinmek için nasıl [, Azure Hizmetleri için ters DNS kayıtlarını yönetme](dns-reverse-dns-for-azure-services.md).

