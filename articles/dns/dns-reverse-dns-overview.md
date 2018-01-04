---
title: "Ters Azure DNS'ye genel bakış | Microsoft Docs"
description: "Nasıl geriye doğru DNS çalışır ve Azure'da nasıl kullanılabileceğini öğrenin"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 08f4f4aca20efad8f51ebc9ca8c6df8de8d0d4c7
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="overview-of-reverse-dns-and-support-in-azure"></a>Geriye doğru DNS ve Azure desteği'na genel bakış

Bu makalede nasıl geriye doğru DNS works genel bir bakış ve Azure'da desteklenen geriye doğru DNS senaryoları sağlar.

## <a name="what-is-reverse-dns"></a>Geriye doğru DNS nedir?

Geleneksel DNS kayıtları bir DNS adı (örneğin, 'www.contoso.com') bir IP adresi (örneğin, 64.4.6.100) için ana kadar eşlemeyi etkinleştirin.  Geriye doğru DNS adı ('www.contoso.com') için geri bir IP adresi (64.4.6.100) çeviri sağlar.

Geriye doğru DNS kayıtları çeşitli durumlar kullanılır. Örneğin, geriye doğru DNS kayıtlarını yaygın olarak istenmeyen e-posta gönderen bir e-posta iletisinin doğrulayarak mücadele kullanılır.  Alıcı posta sunucusuna gönderen sunucunun IP adresinin ters DNS kaydı alır ve bu ana kaynak etki alanından e-posta göndermek için yetkili olup olmadığını doğrular. 

## <a name="how-reverse-dns-works"></a>Nasıl geriye doğru DNS çalışır

Geriye doğru DNS kayıtlarını 'ARPA' bölgeleri bilinen özel DNS bölgeleri barındırılır.  Bu bölgeler ayrı bir DNS hiyerarşi paralel "contoso.com" gibi etki alanı barındırma normal hiyerarşisi oluşturur.

Örneğin, DNS kaydı 'www.contoso.com' name 'www' ile bir 'A' DNS kaydı bölge "contoso.com" kullanılarak uygulanır.  Bu durumda 64.4.6.100 karşılık gelen IP adresi bu A kaydı işaret ediyor.  Geriye doğru arama, ayrı ayrı '100' bölgesinde '6.4.64.in-addr.arpa' (IP adresleri ARPA bölgeleri ters gerektiğini unutmayın.) adlı bir 'PTR' kayıt kullanılarak uygulanır  Doğru şekilde yapılandırılmışsa bu PTR kaydı 'www.contoso.com' adı işaret eder.

Bir kuruluş IP adres bloğu atandığında, bunlar aynı zamanda ilgili ARPA bölgeyi yönetme hakkı alın. Azure tarafından kullanılan IP adres blokları karşılık gelen ARPA bölgeleri barındırılan ve Microsoft tarafından yönetilir. ISS'niz kendi IP adreslerini ARPA bölgenin sizin için barındırabilir veya bir DNS hizmeti Azure DNS gibi tercih ettiğiniz ARPA bölgesinde barındırmak izin verebilir.

> [!NOTE]
> İleriye doğru DNS araması ve geriye doğru DNS araması ayrı, paralel DNS hiyerarşileri uygulanır. Geriye doğru arama 'www.contoso.com' için olan **değil** "contoso.com" bölgesinde barındırılan, bunun yerine, karşılık gelen IP adres bloğu için ARPA bölgede barındırılan. Ayrı bölgeleri için IPv4 ve IPv6 adres bloklarını kullanılır.

### <a name="ipv4"></a>IPv4

Bir IPv4 geriye doğru arama bölgesi adı şu biçimde olmalıdır: `<IPv4 network prefix in reverse order>.in-addr.arpa`.

Örneğin, ana bilgisayar kayıtları geriye doğru arama bölgesine 192.0.2.0/24 önekini olan IP'leri konaklarla için oluştururken, bölge adı öneki (192.0.2) adresinin yalıtma (2.0.192) sırayı ters çevirme ve sonek ekleme tarafından oluşturulacak `.in-addr.arpa`.

|Alt ağ sınıfı|Ağ öneki  |Ters ağ öneki  |Standart soneki  |Geriye doğru arama bölgesi adı |
|-------|----------------|------------|-----------------|---------------------------|
|Sınıf A|203.0.0.0/8     | 203        | .in-addr.arpa   | `203.in-addr.arpa`        |
|B sınıfı|198.51.0.0/16   | 51.198     | .in-addr.arpa   | `51.198.in-addr.arpa`     |
|C sınıfı|192.0.2.0/24    | 2.0.192    | .in-addr.arpa   | `2.0.192.in-addr.arpa`    |

### <a name="classless-ipv4-delegation"></a>Sınıfsız IPv4 temsilci seçme

Bazı durumlarda, kuruluş için ayrılan IP aralığı C sınıfı küçüktür (/ 24) aralık. Bu durumda, IP aralığı bir bölge sınırı içinde üzerinde kapsamında değilse `.in-addr.arpa` bölge hiyerarşisini ve bu nedenle bir alt bölge olarak temsil edilemez.

Bunun yerine, farklı bir mekanizma, tek tek geriye doğru arama (PTR) kayıtlarının denetim adanmış bir DNS bölgesi aktarmak için kullanılır. Bu mekanizma her IP aralığı için bir alt bölge atar ve ardından her IP adresi aralığı, CNAME kayıtları kullanarak bu alt bölge için ayrı ayrı eşler.

Örneğin, bir kuruluşun kendi ISS tarafından IP aralığı 192.0.2.128/26 verilir varsayalım. 192.0.2.191 192.0.2.128 bu 64 IP adreslerini gösterir. Bu aralık için ters DNS şu şekilde gerçekleştirilir:
- Kuruluş 128 26.2.0.192.in addr.arpa adlı bir geriye doğru arama bölgesi oluşturur. ' 128-26' öneki C sınıfı kuruluşunuzda atanan ağ kesimini temsil eder (/ 24) aralık.
- ISS C sınıfı üst bölgeden yukarıdaki bölgenin DNS temsilcisi ayarlamak için NS kayıtları oluşturur. Ayrıca CNAME kayıtları üst (C sınıfı) geriye doğru arama bölgesi kuruluş tarafından oluşturulan yeni bölge için IP aralığı her IP adresi eşleme oluşturur:

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
- Kuruluş kendi alt bölgedeki tek tek PTR kayıtlarını yönetir.

```
$ORIGIN 128-26.2.0.192.in-addr.arpa
; PTR records for each UIP address. Names match CNAME targets in parent zone
129      PTR    www.contoso.com
130      PTR    mail.contoso.com
131      PTR    partners.contoso.com
; etc
```
Geriye doğru arama PTR kaydı için IP adresi '192.0.2.129' sorgular için '129.2.0.192.in-addr.arpa' adlı. Bu sorgu, alt bölgedeki PTR kaydı için üst bölgedeki CNAME aracılığıyla çözümler.

### <a name="ipv6"></a>IPv6

Bir IPv6 geriye doğru arama bölgesi adı şu biçimde olmalıdır:`<IPv6 network prefix in reverse order>.ip6.arpa`

Örneğin. Ana bilgisayar kayıtları geriye doğru arama bölgesine konakları için IP'leri ile oluşturduğunuz olduğunda 2001:db8:1000:abdc:: / 64 öneki, bölge adı oluşturulacaktır adresinin ağ öneki yalıtarak (2001:db8:abdc::). Sonraki kaldırmak için IPv6 öneki genişletin [sıfır sıkıştırma](https://technet.microsoft.com/library/cc781672(v=ws.10).aspx), IPv6 adresi öneki kısaltmak için kullandıysanız (2001:0db8:abdc:0000::). Bir süre ters ağ öneki oluşturmak için her bir onaltılık sayıyı önek bölücüyü kullanarak sırasını ters (`0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2`) ve soneki eklemek `.ip6.arpa`.


|Ağ öneki  |Genişletilmiş ve ters ağ öneki |Standart soneki |Geriye doğru arama bölgesi adı  |
|---------|---------|---------|---------|
|2001:db8:abdc:: / 64    | 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2        | . ip6.arpa        | `0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa`       |
|2001:db8:1000:9102:: / 64    | 2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2        | . ip6.arpa        | `2.0.1.9.0.0.0.1.8.b.d.0.1.0.0.2.ip6.arpa`        |


## <a name="azure-support-for-reverse-dns"></a>Geriye doğru DNS Azure desteği

Azure DNS geriye doğru şekilde ilgili iki ayrı senaryoları destekler:

**IP adres bloğu ile karşılık gelen geriye doğru arama bölgesini barındıran.**
Azure DNS için kullanılan olabilir [, geriye doğru arama bölgeleri barındıran ve her geriye doğru DNS araması PTR kayıtlarını yönetme](dns-reverse-dns-hosting.md), IPv4 ve IPv6 için.  (ARPA) geriye doğru arama bölgesi oluşturma, temsilci seçmeyi ayarlama ayarlama ve PTR kayıtlarını yapılandırma işlemi normal DNS bölgeleri ile aynıdır.  Yalnızca farklılıklar şunlardır: temsilci DNS kayıt yerine ISS yapılandırılması gerekir ve yalnızca PTR kayıt türü kullanılmalıdır.

**Geriye doğru DNS kaydı Azure Hizmetinize atanmış IP adresi için yapılandırın.** Azure olanak tanır [Azure hizmetiniz için ayrılan IP adresleri için geriye doğru arama yapılandırma](dns-reverse-dns-for-azure-services.md).  Bu geriye doğru arama Azure tarafından bir PTR kaydı karşılık gelen ARPA bölgede olarak yapılandırılır.  Azure tarafından kullanılan tüm IP aralıklarını karşılık gelen bu ARPA bölgeleri Microsoft tarafından barındırılan

## <a name="next-steps"></a>Sonraki adımlar

Geriye doğru DNS hakkında daha fazla bilgi için bkz: [wikipedia'da ters DNS araması](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Bilgi edinmek için nasıl [ISS atanan IP aralığınızı Azure DNS geriye doğru arama bölgesini barındırmak](dns-reverse-dns-for-azure-services.md).
<br>
Bilgi edinmek için nasıl [, Azure Hizmetleri için ters DNS kayıtlarını yönetme](dns-reverse-dns-for-azure-services.md).

