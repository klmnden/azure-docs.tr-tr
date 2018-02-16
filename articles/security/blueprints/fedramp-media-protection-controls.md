---
title: "Azure güvenliği ve uyumluluğu - FedRAMP Web uygulamaları Otomasyon - medya koruma şeması"
description: "FedRAMP Web uygulamaları Otomasyon - medya koruma"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: fe86fd92-ef6b-4d17-a4a2-de6796d251d0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 37812c2f7ee79685f9014a7999b4355e649ca6e1
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="media-protection-mp"></a>Medya koruma (MP)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-mp-1"></a>NIST 800 53 denetim MP-1

#### <a name="media-protection-policy-and-procedures"></a>Medya koruma ilke ve yordamlar

**MP-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt, koordinasyonu bir medya koruma İlkesi Kurumsal varlıklar ve uyumluluk arasında; ve media koruma ilkesini ve ilişkili medya koruma denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli medya koruma İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve medya koruma yordamlar [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde medya koruma ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-mp-2"></a>NIST 800 53 denetim MP-2

#### <a name="media-access"></a>Media Access

**MP-2** kuruluş erişimi kısıtlayan [atama: dijital ve/veya sayısal olmayan medya türleri kuruluşunuz tarafından tanımlanan] için [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure medya erişim Microsoft Güvenlik İlkesi uygulaması aracılığıyla uyguladı. Bir dijital medyayı mantıksal erişimi Active Directory Grup İlkesi nesneleri (GPO'lar AD) ve güvenlik grupları aracılığıyla denetlenir. Tüm ortamlar için fiziksel erişim veri merkezi erişim işlem tarafından kısıtlanıyor. Erişim verilere erişmek için yasal işletme amacı olan kişiler sınırlıdır. Yerinde veri merkezi erişim denetimleri hakkında daha fazla ayrıntı için lütfen PE-3, fiziksel erişim denetimi, bakın. Varlık koruma standart gizlilik, bütünlük ve kullanılabilirlik Microsoft Azure veri merkezleri içinde bilgi varlıklarını korumak üzere gerekli korumaları tanımlar. |


 ## <a name="nist-800-53-control-mp-3a"></a>NIST 800 53 denetim MP-3.a

#### <a name="media-marking"></a>Media Marking

**MP 3.a** kuruluş uyarılar ve bilgiler ilgili güvenlik işaretler (varsa) işleme dağıtım kısıtlamaları belirten bilgi sistem ortamı işaretler.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure işaretler varlıklar bir HBI, MBI veya LBI Microsoft veri merkezleri içinde farklı güvenlik düzeylerini ve önlemleri işleme gerektiren (yüksek, Orta veya düşük iş etkisi) belirtimi. Varlık sahipleri, bir Microsoft Veri Merkezi içinde depolanan varlıklarına sınıflandırmak için gereklidir. Varlık sınıflandırması standart ve varlık koruma standart için daha fazla bilgi için bakın. |


 ## <a name="nist-800-53-control-mp-3b"></a>NIST 800 53 denetim MP-3.b

#### <a name="media-marking"></a>Media Marking

**MP 3.b** kuruluş zorunluluğundan [atama: kuruluşunuz tarafından tanımlanan bilgileri sistem medya türlerini] içinde medya kaldığı sürece gelen işaretleme [atama: kuruluş tarafından tanımlanan denetlenen alanları].

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure varlıklarına bir varlık sınıflandırması ile atamak varlık sahipleri gerektirir ve hiçbir varlıklar bu gereksinimden dışındadır. Microsoft veri merkezi ortamında, sunucular, ağ aygıtlarına ve manyetik bant varlıklar bakın. Başka bir dijital medyayı gibi USB flash/sürücüleri, harici/taşınabilir sabit sürücüler, Flash veya CD/DVD kullanılmaz. Sayısal olmayan ortam veri merkezinde kullanılmaz. |


 ## <a name="nist-800-53-control-mp-4a"></a>NIST 800 53 denetim MP-4.a

#### <a name="media-storage"></a>Medya depolama

**MP 4.a** kuruluş fiziksel olarak denetler ve güvenli bir şekilde depolar [atama: dijital ve/veya sayısal olmayan medya türleri kuruluşunuz tarafından tanımlanan] içinde [atama: kuruluş tarafından tanımlanan denetlenen alanları].

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bir dijital medyayı varlıklar, fiziksel olarak ve güvenli bir şekilde datacenter birlikte bulundurma odaları içinde depolanır. Microsoft veri merkezlerine fiziksel erişim denetimleri (erişim rozet, Biyometri; fiziksel erişim denetimi hakkında daha fazla ayrıntı için bkz: PE 3) ve Kameralı çok katmanlı güvenli depolama sağlamak için kullanıyor. Sunucular, ağ aygıtlarına ve manyetik bant yedeklemesi için kullanılan dijital medya içerir. Sayısal olmayan ortam veri merkezi ortamında kullanılmaz. |


 ## <a name="nist-800-53-control-mp-4b"></a>NIST 800 53 denetim MP-4.b

#### <a name="media-storage"></a>Medya depolama

**MP 4.b** ortamı yok veya ayıklanmış kullanarak onaylanmış ekipman, teknikleri ve yordamları kadar bilgi sistem ortam kuruluş korur.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Varlık ömrü boyunca, Microsoft datacenter colocations fiziksel erişim denetimi (PE-3) ve mantıksal erişim denetimleri (IA-2) aracılığıyla Microsoft Azure bir dijital medyayı varlıklar korunur. Microsoft Azure varlıklar temizlendi, temizlenmiş veya varlıklar elden önce NIST SP 800 88 ile tutarlı yöntemleriyle yok. Varlık yok etme için Microsoft Azure yerinde varlık yok etme Hizmetleri kullanır. |


 ## <a name="nist-800-53-control-mp-5a"></a>NIST 800 53 denetim MP-5.a

#### <a name="media-transport"></a>Medya Aktarım

**MP 5.a** kuruluş korur ve denetimleri [atama: kuruluş tarafından tanımlanan bilgileri sistem medya türlerini] kullanarak denetimli alanları dışında aktarım sırasında [atama: kuruluş tanımlanan güvenlik önlemlerinin].

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft veri merkezlerine adresindeki Microsoft Azure dijital medyayı sunucuları, ağ aygıtlarını ve manyetik yedekleme bantlarını ve diskler, uygun olduğunda oluşur. Microsoft veri merkezlerine sayısal olmayan ortam kullanmayın. Microsoft Veri Merkezi dışında taşınan medyayı korumak için üç yöntem kullanır: 1) güvenli aktarım, 2) şifreleme 3) temizlemeyi temizlemek veya yok. |


 ## <a name="nist-800-53-control-mp-5b"></a>NIST 800 53 denetim MP-5.b

#### <a name="media-transport"></a>Medya Aktarım

**MP 5.b** denetimli alanları dışında aktarım sırasında bilgi sistem ortam sorumluluk kuruluş korur.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure NIST SP 800 88 Kılavuzu kullanımıyla veri merkezi bırakarak varlıklar için sorumluluk korur: tutarlı temizleme/temizleme, varlık yok etme, şifreleme, doğru envantere, izleme ve gözetim zinciri koruma Aktarım sırasında. |


 ## <a name="nist-800-53-control-mp-5c"></a>NIST 800 53 denetim MP-5.c

#### <a name="media-transport"></a>Medya Aktarım

**MP 5.c** kuruluş bilgileri sistem Medya Aktarım ile ilişkili etkinlikler belgeleri.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure Aktarım, izleme ve gözetim zinciri koruma Aktarım, varlık temizleme/temizleme, varlık yok etme, varlıklar ve stok doğrulama taşıma sonrasında alınması sırasında önce Envanter kayıtlarının tutar. |


 ## <a name="nist-800-53-control-mp-5d"></a>NIST 800 53 denetim MP-5.d

#### <a name="media-transport"></a>Medya Aktarım

**MP 5.d** yetkili personel bilgileri sistem Medya Aktarım ile ilişkili etkinlikler kuruluş kısıtlar.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure varlık taşıma etkinliklerinin gözetim zinciri koruma aracılığıyla yetkili personel kısıtlar. Kilitleri, kullanımını kanıtını mühürlerin değiştirmesine ve varlık envanterleri doğrulanmasını gerektiren yalnızca yetkili personel varlık taşıma söz konusu sağlar. |


 ### <a name="nist-800-53-control-mp-5-4"></a>NIST 800 53 denetim MP-5 (4)

#### <a name="media-transport--cryptographic-protection"></a>Medya Aktarım | Şifreleme koruma

**MP-5 (4)** bilgi sistemi denetimli alanları dışında aktarım sırasında dijital medyada depolanan bilgilerin bütünlüğünü ve gizliliğini korumak için şifreleme mekanizmaları uygular.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure kullanan veri koruma hizmeti (DPS), şifreleme yönetmek için bir FIPS 140-2 Düzey 3 doğrulanmış şifreleme Modülü (cert #1694) kullanarak ve HSM (cert #1178) manyetik bantlardaki AES 256 bit şifrelenmiş verilerin güvenliğini sağlamak için anahtarları. |


 ## <a name="nist-800-53-control-mp-6a"></a>NIST 800 53 denetim MP-6.a

#### <a name="media-sanitization"></a>Medya temizleme

**MP 6.a** kuruluş temizler [atama: kuruluş tarafından tanımlanan bilgileri sistem ortam] kuruluş denetimi dışında elden önce yayın veya yayın yeniden kullanmak için [atama: kuruluş tarafından tanımlanan temizleme teknikleri ve yordamları] geçerli federal ve kuruluş standartları ve ilkeleri uygun olarak.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure medya yeniden veya elden önce temizleme 800-88, yönergeleri cleansed temizlendi/Microsoft onaylı Azure araçlarını kullanarak olmasını Microsoft Azure veri merkezi ortamında ve NIST SP ile tutarlı şekilde bir dijital medyayı gerektirir . Sayısal olmayan ortam veri merkezi ortamında Microsoft Azure tarafından kullanılmaz. |


 ## <a name="nist-800-53-control-mp-6b"></a>NIST 800 53 denetim MP-6.b

#### <a name="media-sanitization"></a>Medya temizleme

**MP 6.b** kuruluş gücü ve bütünlüğü güvenlik kategorisi veya bilgilerin sınıflandırılması orantılı temizleme mekanizmalarını kullanır.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri NIST SP 800 88 ile tutarlı şekilde ve hangi varlık Microsoft Azure varlık sınıflandırması ile orantılı temizlemeyi/Temizleme için veri silinme birimleri ve işlemleri kullanır. Yok etme gerekliliği varlıklar için Microsoft Azure yerinde varlık yok etme Hizmetleri kullanır. |


 ### <a name="nist-800-53-control-mp-6-1"></a>NIST 800 53 denetim MP-6 (1)

#### <a name="media-sanitization--review--approve--track--document--verify"></a>Medya temizleme | Gözden geçirin / Onayla / izlemek / belge / doğrulayın

**MP-6 (1)** kuruluş inceler, onaylar, izler, belgeler ve medya temizleme ve elden Eylemler doğrular.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure medya temizleme yordamları yönergeye uygun olarak varlık sınıflandırması standart ve varlık koruma standart için NIST SP 800 88 uyguladı. Tüm manyetik ya da elektronik medya cleansed/temizlendi aşağıdaki NIST SP 800 88 belirtimlerine uygun şekilde kendi Azure varlık sınıflandırması tarafından. Azure veri silinme birimleri aşırı Protokolü çözümler (EPS) gelen kullanır. EPS yazılım NIST SP temizleme ve temizleme ve güvenli silinme için 800 88 gereksinimlerini destekler. |


 ### <a name="nist-800-53-control-mp-6-2"></a>NIST 800 53 denetim MP-6 (2)

#### <a name="media-sanitization--equipment-testing"></a>Medya temizleme | Donanım test etme

**MP-6 (2)** kuruluş testleri temizleme ekipman ve yordamlar [atama: kuruluş tarafından tanımlanan sıklığı] hedeflenen temizleme elde olduğunu doğrulamak için.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri NIST SP 800 88 ile tutarlı şekilde temizlemeyi/Temizleme için veri silinme birimleri ve işlemleri kullanır. 180 günde DC'leri işlemler Microsoft Azure veri silinme birimleri ve işlem silinme için sınar. Test, DC'LERİN işlemleri doğrular hedeflenen temizleme adli analiz verileri veri silinme birimlerle ayıklanır olduğunu onaylamak için test edilmiş sabit disk sürücülerinin aracılığıyla elde olduğunu |


 ### <a name="nist-800-53-control-mp-6-3"></a>NIST 800 53 denetim MP-6 (3)

#### <a name="media-sanitization--nondestructive-techniques"></a>Medya temizleme | Dönüşlü teknikleri

**MP-6 (3)** kuruluş aşağıdaki durumlarda bilgi sistemi cihazlara bağlanma önce taşınabilir depolama aygıtlarını dönüşlü temizleme teknikleri uygulandığı: [atama: kuruluş tarafından tanımlanan koşullar] taşınabilir depolama aygıtlarını temizleme gerektirme.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure, Azure veri merkezleri taşınabilir depolama aygıtlarda kötü amaçlı yazılım tarafından kamu ortamının bulaşma önlemek için Araçlar ve veri merkezi hizmetleri çalıştırma kitap çıkarılabilir medya güvenlik yordamda izleyin sağlar. Yordam, aşağıdaki eylemleri kamu ortamında kullanmadan önce USB sürücüleri ile alınması belirtir: <br /> (1) USB sürücüleri üretici veya satıcı, ilk kullanmadan önce veya farklı bir aracı yeniden kullanılıyor sürücüleri ilk satın alınan biçimi. <br /> (2) kamu belirlenmiş bir alanında kötü amaçlı yazılım için sürücü alanına çıkarmadan önce kullanılacak herhangi bir USB sürücüye tarayın. <br /> (3) sonra kamu belirlenmiş bir alanda bir sürücü biçimi kullanılarak sürücü alanı ayrılmadan önce. <br /> Araçlar ve çıkarılabilir medya güvenlik yordamı da gerektirir tüm kayıp, atılan, çalınan veya yanlış yerleştirilmiş flash sürücüler hiçbir zaman Azure veri merkezleri içinde yeniden kullanılmaya ancak bunun yerine olmaları kataloğa ve yok etme. |


 ## <a name="nist-800-53-control-mp-7"></a>NIST 800 53 denetim MP-7

#### <a name="media-use"></a>Medya kullanma

**MP-7** kuruluş [seçim: kısıtlar; yasaklar] kullanımını [atama: kuruluş tarafından tanımlanan bilgileri sistem medya türlerini] üzerinde [atama: kuruluş tarafından tanımlanan bilgileri sistemleri veya sistem bileşenleri] kullanarak [atama: Kuruluşunuz tarafından tanımlanan güvenlik önlemleri].

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Hiçbir varlıklar bu gereksinimden muaf tutulan ve Microsoft Azure varlıklarına bir varlık sınıflandırması ile atamak varlık sahipleri gerektirir. Microsoft Azure veri merkezi ortamında, sunucuları ve ağ aygıtlarını varlıklar bakın. USB flash/Flash sürücüleri gibi başka bir dijital medyayı belirli ilkeleri ve bu cihazların nasıl yönetileceğini yöneten yordamları tarafından yönetilir. CD/DVD kullanılmaz. Sayısal olmayan ortam veri merkezinde kullanılmaz. Microsoft Azure veri merkezi ortamlarında bir dijital medyayı CCTV kapsamı aracılığıyla izlenen 24 x 7 kullanımdır. Daha fazla ayrıntı için lütfen PE-06 bakın. |


 ### <a name="nist-800-53-control-mp-7-1"></a>NIST 800 53 denetim MP-7 (1)

#### <a name="media-use--prohibit-use-without-owner"></a>Medya kullanma | Sahibi kullanmadan engelle

**MP-7 (1)** cihazlara tanımlanabilen sahip olduğunda kuruluş taşınabilir depolama aygıtlarının kullanımını kuruluş bilgilerini sistemlerinde yasaklar.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri kontrollü ortam yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft, açıkça çıkarılabilir medya yordamı ve DC'leri araçları aracılığıyla veri Merkezi Yönetimi tarafından onaylanan bir ortama yazılabilir, çıkarılabilir medya kısıtlar. Microsoft veri merkezi iş ve düzenlemelere uygun şekilde belgeye belirtildiği gibi herhangi bir üretim alanda, kişisel ait veya kişisel sahibi olan medya yasaktır. |
