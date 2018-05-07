---
title: Azure güvenliği ve uyumluluğu - FedRAMP Web uygulamaları Otomasyon - fiziksel ve ortam koruma şeması
description: FedRAMP Web uygulamaları Otomasyon - fiziksel ve ortam koruma
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 0bf8349b-450d-413c-a535-6f7b80b82781
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: 792b9da0f4e5ec73c39f56a6e4805cf3c37133c4
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="physical-and-environmental-protection-pe"></a>Fiziksel ve ortam koruma (PE)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-pe-1"></a>NIST 800 53 denetim PE-1

#### <a name="physical-and-environmental-protection-policy-and-procedures"></a>Fiziksel ve ortam koruma ilke ve yordamlar

**PE-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim fiziksel ve ortam koruma İlkesi Taahhüt, Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve fiziksel ve ortam koruma ilkesini ve ilişkili fiziksel ve ortam koruma denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli fiziksel ve ortam koruma İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve fiziksel ve ortam koruma yordamlar [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde fiziksel ve ortam koruma ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-pe-2a"></a>NIST 800 53 denetim PE-2.a

#### <a name="physical-access-authorizations"></a>Fiziksel erişim kimlik doğrulamalarını

**PE 2.a** kuruluş geliştirir, onaylar ve kişiler bilgi sistemi bulunduğu tesis yetkili erişimi olan bir listesini tutar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Bir Microsoft Veri merkezinde fiziksel erişim veri merkezi erişim aracını kullanarak veri merkezi yönetimi (DCM) ekibi tarafından onaylanması gerekir. Erişim atamalarını olacağı erişim otomatik olarak kaldırılır ve reapproved gereken bir bitiş tarihi gerektirir. Ayrıca, erişim artık gerekli olduğunda, veri merkezi güvenlik görevlileri veya yönetim el ile sonlandırma erişim istemek için. |


 ## <a name="nist-800-53-control-pe-2b"></a>NIST 800 53 denetim PE-2.b

#### <a name="physical-access-authorizations"></a>Fiziksel erişim kimlik doğrulamalarını

**PE 2.b** kuruluş tesis erişimi için yetkilendirme kimlik bilgilerini sorunları.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Veri Merkezi erişim belirli bir veri merkezine yetkili erişimi olan tüm personel listeleme yetkili kaynak aracıdır. Aracı merkezinin fiziksel güvenlik erişimi denetim aygıtları ile bağlantılı ve DCM ekibi tarafından onaylanan erişim düzeyleri göre erişim yetkisi verir. Erişim düzeyleri aracında bir kullanıcının Microsoft rozet veya datacenter denetim odası yönetici (CR) tarafından atanan bir geçici erişim rozet verilen atanır. Erişim düzeyleri DCM ekibi tarafından onaylanır. Fiziksel rozetleri atanan kimlik bilgilerine ek olarak bazı alanlar veri merkezinin kayıt kullanıcının biyometrik verilerin (el geometrisi veya parmak izi) gerektirir. |


 ## <a name="nist-800-53-control-pe-2c"></a>NIST 800 53 denetim PE-2.c

#### <a name="physical-access-authorizations"></a>Fiziksel erişim kimlik doğrulamalarını

**PE 2.c** kuruluşun kişiler tarafından yetkili tesis erişim ayrıntılı erişim listesi incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Açıklanan erişim iptali ek a, Microsoft Azure bölümünde incelemeler yetkili Azure veri merkezleri için erişim listelerini her 90 günde gerektiği bireysel erişim Kaldır/güncelleştirmek için. |


 ## <a name="nist-800-53-control-pe-2d"></a>NIST 800 53 denetim PE-2.d

#### <a name="physical-access-authorizations"></a>Fiziksel erişim kimlik doğrulamalarını

**PE 2.d** erişim artık gerekli olmadığında kuruluşun kişiler tesis erişimi listesinden kaldırır.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Erişim atama bitiş tarihi ulaşıldığında Microsoft Azure erişim otomatik olarak kaldırır. Erişim artık gerekli olmadığında, veri merkezi güvenlik görevlileri veya yönetim el ile veri merkezi erişim aracı Access'te sonlandırılması ister. Ayrıca, Microsoft Azure bölümü C'de açıklanan erişim listesini gözden geçirme sonucunda bulunan tüm gereksiz erişim kimlik doğrulamalarını kaldırır. |


 ## <a name="nist-800-53-control-pe-3a"></a>NIST 800 53 denetim PE-3.a

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.a** adresindeki fiziksel erişim yetkilerini kuruluş zorlar [atama: kuruluş tarafından tanımlanan giriş/çıkış noktaları tesisine bilgi sistemi bulunduğu] vermeden önce bireysel erişim yetkilerini doğrulayarak tesisine erişim; ve tesis kullanarak giriş/çıkış denetleme [seçimi (bir veya daha fazla): [atama: kuruluş tarafından tanımlanan fiziksel erişim denetim sistemleri/aygıtları]; koruyucuları].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure 7 x 24 personel kullanarak Azure veri merkezleri, uyarılar, Kameralı, çok faktörlü kimlik doğrulama ve Turnike sistemi portal cihazlara fiziksel erişim kimlik doğrulamalarını tüm fiziksel erişim noktaları zorlar. |


 ## <a name="nist-800-53-control-pe-3b"></a>NIST 800 53 denetim PE-3.b

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.b** kuruluş fiziksel erişim denetim günlükleri tutar [atama: kuruluş tarafından tanımlanan giriş/çıkış noktaları].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Azure veri merkezi tesisler için tüm erişimler günlüğe ve denetlendi. |


 ## <a name="nist-800-53-control-pe-3c"></a>NIST 800 53 denetim PE-3.c

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.c** kuruluş sağlar [atama: kuruluş tanımlanan güvenlik önlemlerinin] resmi olarak genel olarak erişilebilir olarak belirlenmiş tesis içindeki alanlara erişimi denetleme.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Azure veri merkezlerinde genel olarak erişilebilir olarak belirlenmiş alanları içermez. |


 ## <a name="nist-800-53-control-pe-3d"></a>NIST 800 53 denetim PE-3.d

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.d** kuruluş ziyaretçileri escorts ve izleyicileri ziyaretçi etkinliği [atama: ziyaretçi escorts gerektiren ve izleme kuruluşunuz tarafından tanımlanan koşullar].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. (Bkz PE-2) veri merkezine erişim onaylanmasını tüm ziyaretçiler kendi rozetleri veya diğer görsel işaret (örneğin, renkli rozetleri) aracılığıyla 'Yalnızca Escort' belirlenmiş ve her zaman kendi escorts ile kalması gerekir. Escorted ziyaretçileri kendilerine verilen tüm erişim düzeylerine sahip değil ve yalnızca kendi escorts erişimi izler. Escorts kendi ziyaretçi merkezinde tüm etkinliklerini izler. |


 ## <a name="nist-800-53-control-pe-3e"></a>NIST 800 53 denetim PE-3.e

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.e** Kuruluş anahtarları, birleşimleri ve diğer fiziksel erişim aygıtları güvenliğini sağlar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Fiziksel anahtarlar ve geçici erişim rozetleri güvenlik işlemlerini Merkezi (SOC) içinde güvenlidir. Güvenlik görevlileri aracılığıyla 24 x 7 ' dir. Anahtarları belirli personel kişinin erişim rozet fiziksel anahtara eşleştirerek teslim alınır. Anahtar envanterleri her kaydırma sırasında yürütülür ve şirket dışı gerçekleştirilecek anahtarlara izin verilmez. |


 ## <a name="nist-800-53-control-pe-3f"></a>NIST 800 53 denetim PE-3.f

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.f** kuruluş envanterleri [atama: kuruluş tarafından tanımlanan fiziksel erişim aygıtları] her [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Fiziksel erişim aygıtları Azure veri merkezleri içinde en az bir yıllık temelinde envantere eklenen. Birden çok kez bir günde her shift stoğa alınan anahtarları ve geçici erişim işaretler. Erişim rozet okuyucular ve benzer erişim aygıtları burada durumu sürekli olarak temsil fiziksel güvenlik sistemine bağlanır. |


 ## <a name="nist-800-53-control-pe-3g"></a>NIST 800 53 denetim PE-3.g

#### <a name="physical-access-control"></a>Fiziksel erişim denetimi

**PE 3.g** birleşimleri ve anahtarları kuruluş değişiklikler [atama: kuruluş tarafından tanımlanan sıklığı] ve/veya anahtarları kaybolur birleşimleri tehlikeye veya kişiler aktarılan veya sonlandırıldı.  

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri erişim rozet zaman durumlarda uygulama yordamlarını sahip anahtar kaybolur veya bir kişinin sonlandırıldı veya transfer. Sonlandırma veya aktarım durumunda kişinin erişim hemen sistemden kaldırılır ve bunların erişim rozet kaldırıldı. |


 ### <a name="nist-800-53-control-pe-3-1"></a>NIST 800 53 denetim PE-3 (1)

#### <a name="physical-access-control--information-system-access"></a>Fiziksel erişim denetimi | Bilgi sistem erişimi

**PE-3 (1)** fiziksel erişim kimlik doğrulamalarını olanağı için fiziksel erişim denetimi ek bilgileri sisteme kuruluş zorlar [atama: bir veya daha fazla bileşenlerini içeren kuruluş tarafından tanımlanan fiziksel alanları bilgi sistem].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Daha fazla Microsoft Azure kısıtlayan elektronik erişim denetimi, biyometrik cihazlar gibi çeşitli güvenlik mekanizmaları aracılığıyla kritik sistemleri (örn., colocations, kritik ortamlar, MDF odaları, vb.) içeren Microsoft veri merkezleri içinde alanlarına ve Anti-passback denetler. Azure colocations erişimi DCAT erişim diğer veri merkezi erişim bölümlerinin daha ayrı, daha yüksek bir düzeyde olarak verilmiştir. Ayrıca, tüm Azure FTE'ın ve Azure kamu colocations erişimi satıcıları için resmi olarak Microsoft'un bulut filtreleme geçer ve öncesinde ABD vatandaşlığa benzer doğrulama ortam erişim yetkisi gereklidir. Daha fazla Azure kamu colocations bulut filtreleme ilişkin ayrıntılar için PS-03 bölümüne bakın. |


 ## <a name="nist-800-53-control-pe-4"></a>NIST 800 53 denetim PE-4

#### <a name="access-control-for-transmission-medium"></a>Aktarım ortamı için erişim denetimi

**PE 4** kuruluş fiziksel erişimi kontrol [atama: kuruluş tarafından tanımlanan bilgileri sistem dağıtım ve aktarım satırları] kullanarak kuruluş tesis içinde [atama: kuruluş tanımlanan güvenlik önlemlerinin] .

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure erişim denetimi tasarım ve yapı ana dağıtım çerçeve (MDF) odaları ve colocations bilgi sistem dağıtım ve aktarım satırları yanlışlıkla zarar korumak için üzerinden aktarım ortamı için uyguladı, kesintisi ve fiziksel oynama. Erişim için MDF odaları ve colocations iki Etmenli kimlik doğrulamasının (erişim rozet Biyometri) gerektirir. Bu erişim yalnızca yetkili personel (bkz PE-2, PE 3) kısıtlı olmasını sağlar. MDF içinde deneyiminizi, kesintisi ve metal conduits, kilitli rafları, kafesleri bulunur veya kablo Tepsisi kullanımı ile fiziksel oynama iletim ve dağıtım satırları korunur. |


 ## <a name="nist-800-53-control-pe-5"></a>NIST 800 53 denetim PE 5

#### <a name="access-control-for-output-devices"></a>Çıkış aygıtları için erişim denetimi

**PE 5** kuruluş bilgileri sistem çıkış aygıtları yetkisiz kişiler çıkış ele geçirmesini önlemek için fiziksel erişimi kontrol eder.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri için Azure varlıklar kalıcı olarak bağlı çıkış aygıtları (izleyicileri, yazıcılar, ses aygıtları, vb.) yok veya Azure paylaşılan varlıkları. Çıkış aygıtları olmamasından ek olarak, güvenlik görevlileri kilitleniyor kapıları ve güvenli bölmeler gibi öğeleri denetleme birden çok kez vardiya tesisi fiziksel izlenecek yollar gerçekleştirin. Veri Merkezi erişim erişim kimlik doğrulamalarını onaylanmasını kişilere sınırlıdır. Colocations iki öğeli kimlik doğrulamasını (erişim rozet ve Biyometri) erişim gerektirir. |


 ## <a name="nist-800-53-control-pe-6a"></a>NIST 800 53 denetim PE-6.a

#### <a name="monitoring-physical-access"></a>Fiziksel erişim izleme

**PE 6.a** kuruluş bilgileri sistem bulunduğu algılamak ve fiziksel güvenlik olaylarını yanıtlama tesis fiziksel erişim izler.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Fiziksel erişim güvenlik aygıtları ve işlemleri veri merkezleri, uygulama tarafından izlenir. 7 x 24 elektronik erişim denetimi, uyarı ve video sistemleri yanı sıra 24 x 7 site güvenlik patrols tesis işe son verme ve izleme örnekler. Bir denetim yer yönetici, veri merkezindeki fiziksel erişim izleme sağlamak için her zaman SOC bulunur. |


 ## <a name="nist-800-53-control-pe-6b"></a>NIST 800 53 denetim PE-6.b

#### <a name="monitoring-physical-access"></a>Fiziksel erişim izleme

**PE 6.b** kuruluş fiziksel erişim günlüklerini gözden geçirir [atama: kuruluş tarafından tanımlanan sıklığı] ve geçişi sırasında [atama: kuruluş tarafından tanımlanan olayları ya da olayların olası göstergeleri].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Fiziksel erişim günlüklerini sürekli olarak gözden ve sonraki araştırma gözden geçirilmek üzere saklanır. |


 ## <a name="nist-800-53-control-pe-6c"></a>NIST 800 53 denetim PE-6.c

#### <a name="monitoring-physical-access"></a>Fiziksel erişim izleme

**PE 6.c** kuruluş incelemeler ve kuruluş olay yanıtlama özelliğine sahip araştırmalar sonuçları düzenler. 

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Veri Merkezi içinde oluşan güvenlik olayları güvenlik ekibi tarafından belgelenmiştir. Güvenlik ekibi, olay oluştuktan sonra güvenlik olay ayrıntılarını yakalama raporlar oluşturur. <br /> Kamu bildirim gerektiren olaylar için devlet dairesi müşteri, BİZE CERT ve FedRAMP içinde ABD-CERT yönergeleri (IR 6'ya bakın) bilgilendirmek için ana uygulama sağlayıcısı (örn., O365) ile Microsoft Azure güvenlik ekibine koordine. |


 ### <a name="nist-800-53-control-pe-6-1"></a>NIST 800 53 denetim PE-6 (1)

#### <a name="monitoring-physical-access--intrusion-alarms--surveillance-equipment"></a>Fiziksel erişim izleme | Yetkisiz erişim alarmlar / gözetleme donanım

**PE-6 (1)** kuruluş fiziksel yetkisiz erişim alarmlar ve izleme sistemine donanım izler.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. 7 x 24 yerinde güvenlik yanı sıra (kiralanmış ve tam olarak yönetilen) Microsoft Azure veri merkezleri alarm sistemleri ve CCTV izleme de kullanır. Alarmlar izlenen ve isteğe yanıt SOC. 7 24 x denetim odası yönetici tayin Bir yanıt durum sırasında denetim odası yönetici kameralar Yanıtlayıcı vermek için araştırılan olay alanı kullanır. gerçek zamanlı bilgiler. |


 ### <a name="nist-800-53-control-pe-6-4"></a>NIST 800 53 denetim PE-6 (4)

#### <a name="monitoring-physical-access--monitoring-physical-access-to-information-systems"></a>Fiziksel erişim izleme | Bilgi Systems izleme fiziksel erişim

**PE-6 (4)** kuruluş izler fiziksel erişim olanağı izlenmesini ek olarak bilgi sisteme fiziksel erişim [atama: bir veya daha fazla bileşen bilgileri içeren kuruluş tarafından tanımlanan fiziksel alanları Sistem].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri merkezleri içinde bilgi systems yanı sıra tesis fiziksel erişim izler. Tüm Microsoft online services donanımına fiziksel erişim burada izlenen konumlarda veri merkezleri içinde yerleştirilir. Tüm MDF odaları ve birlikte bulundurma, erişim denetimi, uyarılar ve video tarafından korunur. |


 ## <a name="nist-800-53-control-pe-8a"></a>NIST 800 53 denetim PE-8.a

#### <a name="visitor-access-records"></a>Ziyaretçi erişim kayıtları

**PE 8.a** kuruluş bilgileri sistem bulunduğu için tesis ziyaretçi erişim kayıtları tutar [atama: kuruluş tanımlı süre].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Veri Merkezi erişim kayıtları veri merkezi erişim aracı onaylanan istekleri biçiminde saklanır. PE-3'te açıklandığı gibi ziyaretçilere her zaman escorted gereklidir. Escort kişinin erişim veri merkezi içinde günlüğe kaydedilir ve gelecekteki gözden geçirme ziyaretçi için gerekli bağıntılı durumunda. |


 ## <a name="nist-800-53-control-pe-8b"></a>NIST 800 53 denetim PE-8.b

#### <a name="visitor-access-records"></a>Ziyaretçi erişim kayıtları

**PE 8.b** kuruluş ziyaretçi erişim kayıtları incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Onaylanan erişim isteği ziyaretçilerin erişim kayıtlarını kimliklerini kimliğine kamu bir form karşı doğrulanır zaman gözden erişebilir veya Microsoft rozet verilen. PE-3'te açıklandığı gibi ancak veri merkezindeki her zaman ziyaretçilere escorted. |


 ### <a name="nist-800-53-control-pe-8-1"></a>NIST 800 53 denetim PE-8 (1)

#### <a name="visitor-access-records--automated-records-maintenance--review"></a>Ziyaretçi erişim kayıtları | Kayıtları bakım otomatik / gözden geçirin

**PE-8 (1)** kuruluş Bakımı ve gözden geçirilmesi ziyaretçi erişim kayıtları kolaylaştırmak için otomatik mekanizmaları uygular.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri merkezi erişim onaylanan DCAT istekleri biçiminde DCAT kayıtlarında tutar. DCAT istekleri yalnızca DCM ekibi tarafından onaylanabilir. Erişim düzeyleri veri merkezi içinde atanan ve DCAT içinde yönetilir. Veri Merkezi erişim, üç aylık gözden geçirilir. Azure veri merkezleri tüm erişimi DCAT kaydedilir ve gelecekteki olası araştırmalar için kullanılabilir. Ziyaretçilere her zaman escorted gereklidir. Veri Merkezi içinde escort'ın erişim izleme sistemi ve gelecekteki gözden geçirme ziyaretçi için gerekli bağıntılı varsa uyarı içinde günlüğe kaydedilir. Ziyaretçi erişimine sürekli olarak atanan escort ve CCTV ve sistem izleme uyarısı aracılığıyla denetim odası yönetici tarafından gözden geçiriliyor. Ziyaretçilerin erişimle sağlanmayan ve her zaman kendi escorts tarafından bilgisiyle birlikte olmalıdır. |


 ## <a name="nist-800-53-control-pe-9"></a>NIST 800 53 denetim PE-9

#### <a name="power-equipment-and-cabling"></a>Güç ekipman ve kablo

**PE 9** kuruluş güç donanım ve güç hasar ve yok etme kablo için bilgi sistemi korur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure koruyucu alanları ve uygun kablolarını etiketleme sağlar. Microsoft Azure altyapı ekipman, örneğin, kablo, elektrik satırları ve yedekleme oluşturucuları hırsızlık, yangın, patlayıcılar, Duman, su, toz gibi çevresel riskleri korunacak mühendislik ortamlarda yerleştirilmelidir, Titreşim, deprem, zararlı chemicals, elektrik girişim, güç kesintileri, elektrik kesilmelerinden (ani). Tüm taşınabilir Çevrimiçi Hizmetler varlıklar (örneğin, bölmeler, sunucuları, ağ aygıtları) kilitli veya hırsızlığı veya hareketini hasarı karşı koruma sağlamak için yerinde fastened gerekir. Herhangi bir Microsoft Azure ortamın içinde güç ve bilgileri sistem kablo uygun şekilde etiketli ve kişiler tarafından ele veya hasar karşı korumalı. Güç ve bilgi sistem kabloları önlemek için bir ortamı içindeki tüm noktalarda birbirinden ayrılır. |


 ## <a name="nist-800-53-control-pe-10a"></a>NIST 800 53 denetim PE-10.a

#### <a name="emergency-shutoff"></a>Acil Durum kesici

**PE 10.a** kuruluş kapatma bilgileri sistem ya da tek sistem bileşenleri acil durumlarda kapatılıyor yeteneği sağlar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure, yerel güvenlik kodu gerektirdiği gibi veri merkezi içinde konumlarda Acil Durum güç kapalı (EPO) düğmeleri yükledi. Bazı Microsoft yönetilen Azure veri merkezlerinde, veri merkezi tasarım artık EPO düğmeleri gerektirir. |


 ## <a name="nist-800-53-control-pe-10b"></a>NIST 800 53 denetim PE-10.b

#### <a name="emergency-shutoff"></a>Acil Durum kesici

**PE 10.b** Acil Durum kesici anahtarları ya da cihaz kuruluş yerleştirir [atama: kuruluş tarafından tanımlanan konum bilgileri sistem veya sistem bileşeni tarafından] personeli için güvenli ve kolay erişim sağlamak için.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. EPO düğmeleri etkinleştirme acil durumlarda izin vermek üzere stratejik yerleştirilir. EPO düğmeleri colocations manned tesis işlemi merkezleri (FOCs) veya yerel güvenlik kodu gerektirdiği yerleştirilebilir. |


 ## <a name="nist-800-53-control-pe-10c"></a>NIST 800 53 denetim PE-10.c

#### <a name="emergency-shutoff"></a>Acil Durum kesici

**PE 10.c** kuruluş Acil Durum güç kesici özelliği yetkisiz etkinleştirmeden korur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Yanlışlıkla etkinleştirme önlemek için EPO düğmeleri koruyucu kasası, çift etkinleştirme gerektiren veya olabilir işitsel bir alarm etkinleştirme önce bir uyarı olarak kullanma. Ayrıca, EPO düğmeler Kameralı altında bulunur. |


 ## <a name="nist-800-53-control-pe-11"></a>NIST 800 53 denetim PE-11

#### <a name="emergency-power"></a>Acil Durum güç

**PE-11** kuruluş kolaylaştırmak için kısa vadeli bir kesintisiz güç kaynağı sağlar [seçimi (bir veya daha fazla): bilgi sistemi kapanmasını; uzun vadeli alternatif güç bilgi sisteme geçişin] durumunda bir Birincil güç kaynağı kaybı.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezi ekipman koruyarak Acil Durum güç uyguladı ve bir kesintisiz güç ile devreler sağlayan oluşturucuları çevrimiçi olamıyor kadar güç sağlamak için kısa vadeli bir güç kaynağı (UPS) sistem sağlayın. |


 ### <a name="nist-800-53-control-pe-11-1"></a>NIST 800 53 denetim PE-11 (1)

#### <a name="emergency-power--long-term-alternate-power-supply---minimal-operational-capability"></a>Acil Durum güç | Uzun vadeli alternatif güç kaynağı - en az işlem özelliği

**PE-11 (1)** en düşük düzeyde koruma yeteneğine bilgileri sistem birincil güç kaynağı genişletilmiş bir kaybı durumunda işletimsel yetenek gerekli uzun vadeli bir alternatif güç kaynağı kuruluş sağlar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure işletimsel yetenek birincil güç kaynağı genişletilmiş bir kaybı olduğunda oluşur en az gerekli sağlayabilecek bilgi sistemi için uzun vadeli bir alternatif güç kaynağı uyguladı. Güç başarısız veya kabul edilebilir voltaj düzeyine bırakır, kesintisiz güç kaynağı (UPS) sistemleri anında ilkenin etkisini gösterip ve güç yük üzerinde gerçekleştirin. Bu sunucular oluşturucuları görevi üstlenebilir kadar çalıştırmak için yeterli güç sağlar. Acil Durum oluşturucuları genişletilmiş kesintiler ve planlı bakım için yedek güç sağlar ve veri merkezi doğal afet durumunda yerinde yakıt yedekleri birlikte çalışabilir. Azure PLACEGHOLDER Oluşturucu bizim veri merkezleri çoğunu adresindeki tutar. Yedekleme oluşturucuları kılavuz kararlılık sağlamak üzere gerektiğinde kullanılır veya olağandışı onarım ve bize bizim veri merkezleri güç kılavuz kapalı yapılacak gerektiren bakım durumları. |


 ## <a name="nist-800-53-control-pe-12"></a>NIST 800 53 denetim PE-12

#### <a name="emergency-lighting"></a>Acil Durum aydınlatma

**PE-12** kuruluş kullanır ve güç kaybı veya kesintisi durumunda etkinleştirir ve, kapsayan Acil durum bilgileri sistem çıkar için otomatik Acil Durum ışık ve Tahliye yollarını tesis içinde tutar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri (kiralanmış ve tam olarak yönetilen) Acil Durum aydınlatma ayrılmış devreler üzerinde Acil Durum aydınlatma yedeklenen yükünü UPS ve oluşturucu sistemlerinin (bkz PE-11) biçiminde uygulayın. Otomatik Acil Durum aydınlatma boyunca tüm Tahliye yollar, Acil Durum çıkar ve Ulusal Güvenlik ve koruma ilişkilendirme (NFPA) ömrü güvenlik kodu uygun olarak colocations içinde uygulanır. Yardımcı güç durumunda durumunda, Acil Durum aydınlatma UPS ve oluşturucu sistemleri tarafından sağlanan güç otomatik olarak geçer. Microsoft Azure veri merkezleri içinde Acil Durum aydınlatma sistemleri uygun çalışma sırada kalmasını sağlamak için düzenli bakım geçer. |


 ## <a name="nist-800-53-control-pe-13"></a>NIST 800 53 denetim PE-13

#### <a name="fire-protection"></a>Yangın koruma

**PE-13** kuruluş uygular ve yangın gizleme ve algılama aygıtları/bağımsız enerji kaynak tarafından desteklenen işletim sistemleri bilgi sistemi için korur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure, Microsoft Azure veri merkezleri yangın algılama ve yangın gizleme sistemler yükleyerek yangın koruma uyguladı. <br /> Microsoft Azure veri merkezleri sağlam yangın algılama mekanizması uygular. Microsoft Azure yangın koruma yaklaşım photoelectric Duman kat aşağıda ve tavan yüklü olduğu yangın koruma fıskiyesi sistemiyle tümleşiktir algılayıcılar kullanımını içerir. Ayrıca, hangi Uzaktan izleme Xtralis VESDA (çok erken Duman algılama Apparatus) sistemlerinde her birlikte bulundurma vardır. VESDA, birden çok değerli alanları yüksek oranda duyarlı hava örnekleme sistemlerinden birimleridir. VESDA birimleri gerçek yangın algılama alarm önce bir araştırma yanıtı izin verir. <br /> 'İstasyon pull' Yangın alarmı kutuları el ile yangın uyarı bildirimi için veri merkezleri boyunca yüklenir. Yangın extinguishers veri merkezlerinde bulunan düzgün Denetlenmekte, hizmet ve yıllık olarak etiketlenir. Güvenlik personel tüm alanlara birden çok kez patrols her shift. Veri Merkezi personelinin tüm yangın izleme gereksinimlerinin karşılandığından emin günlük bir site gözden geçirme gerçekleştirin. <br /> Elektrik hassas donanımların içeren alanlar (colocations, MDFs, vb.) çift kilidi ön eylem (Kuru kanal) fıskiyesi sistemleri tarafından korunur. Kuru kanal fıskiyeleri hem bir fıskiyesi baş etkinleştirme (nedeniyle ısı) hem de su serbest bırakmak için Duman algılama gerektiren iki aşamalı bir ön eylem sistem ' dir. Fıskiyesi baş etkinleştirme ile su doldurmak kanallar sağlayan hava baskısı kanallar de serbest bırakır. Duman veya Isı algılayıcısı de etkinleştirildiğinde su yayımlanır. <br /> Veri Merkezi UPS ve oluşturucu sistemleri için bir yedek güç kaynağı sağlama yangın algılama/gizleme ve Acil Durum aydınlatma sistemleri kablolu. |


 ### <a name="nist-800-53-control-pe-13-1"></a>NIST 800 53 denetim PE-13 (1)

#### <a name="fire-protection--detection-devices--systems"></a>Yangın koruma | Algılama aygıtları / sistemleri

**PE-13 (1)** kullanan kuruluş algılama aygıtları/otomatik olarak etkinleştirmeyi ve bildirim sistemleri için bilgi sistemi yangın [atama: kuruluş tarafından tanımlanan personel ya da roller] ve [atama: kuruluş tarafından tanımlanan Acil Durum Yanıtlayıcı] yangın durumunda.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure kullanan algılama aygıtları/otomatik olarak etkinleştirmeyi ve veri merkezi personelinin Acil Durum yanıtlayıcılarını yangın durumunda birlikte bildirim sistemleri için bilgi sistemi kov. Yangın algılama mekanizmalardan biri herhangi bulundurma etkinleştirilir gelmesi durumunda, yerel yangın departmanı Yangın alarmı sistem üzerinden otomatik olarak bildirilir. Ayrıca, yangın koruma ve yangın algılama sistemleri yerel tesis ve güvenlik personel bildiren güvenlik sistemine bağlıdır. |


 ### <a name="nist-800-53-control-pe-13-2"></a>NIST 800 53 denetim PE-13 (2)

#### <a name="fire-protection--suppression-devices--systems"></a>Yangın koruma | Gizleme aygıtları / sistemleri

**PE-13 (2)** kullanan kuruluş gizleme aygıtları/sistemler için herhangi bir etkinleştirme otomatik bildirim sağlamak bilgileri sistem için yangın [atama: kuruluş tarafından tanımlanan personel ya da roller] ve [atama: Acil Durum yanıtlayıcılarını] kuruluş tanımlı.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Yangın gizleme sistemlerinden birini datacenter etkinleştirilir gelmesi durumunda, yerel yangın departmanı Yangın alarmı sistem üzerinden otomatik olarak bildirilir. Ayrıca, yangın koruma ve yangın algılama sistemleri yerel tesis ve güvenlik personel bildiren güvenlik sistemine bağlıdır. |


 ### <a name="nist-800-53-control-pe-13-3"></a>NIST 800 53 denetim PE-13 (3)

#### <a name="fire-protection--automatic-fire-suppression"></a>Yangın koruma | Otomatik yangın gizleme

**PE-13 (3)** tesis üzerinde sürekli bir şekilde aracılığıyla değil, kuruluşunuzun bilgi sistem için bir otomatik yangın gizleme özelliği kullanır.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri aracılığıyla 24 x 7 ' dir. Yangın alarmı durum algılandığında yangın gizleme sistemleri el ile müdahalesi olmadan otomatik olarak göster. |


 ## <a name="nist-800-53-control-pe-14a"></a>NIST 800 53 denetim PE-14.a

#### <a name="temperature-and-humidity-controls"></a>Sıcaklık ve nem denetimleri

**PE 14.a** kuruluş bilgileri sistem bulunduğu adresindeki tesis içindeki sıcaklık ve nem düzeyleri tutar [atama: kuruluş tarafından tanımlanan kabul edilebilir düzeyleri].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure, American topluluğu ısıtma, Refrigerating ve Air-conditioning mühendisleri (ASHRAE) yönergelerine uygun olarak sıcaklık ve nem düzeyleri tutar. Sıcaklık ve nem düzeyleri merkezinin yapı yönetim sistemi (BMS) tarafından sürekli olarak izlenir. |


 ## <a name="nist-800-53-control-pe-14b"></a>NIST 800 53 denetim PE-14.b

#### <a name="temperature-and-humidity-controls"></a>Sıcaklık ve nem denetimleri

**PE 14.b** kuruluş sıcaklık ve nem düzeyleri izler [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri sıcaklık ve nem düzeyleri yapı yönetim sistemi (BMS) tarafından sürekli olarak izlenir. Herhangi bir uyarı noktası aşıldı önce bunlar sıcaklık ve nem veri merkezi içinde yönetebilmeniz için veri merkezi personelinin gelen tesis işlemlerini Merkezi (FOC), BMS izleyin. |


 ### <a name="nist-800-53-control-pe-14-2"></a>NIST 800 53 denetim PE-14 (2)

#### <a name="temperature-and-humidity-controls--monitoring-with-alarms--notifications"></a>Sıcaklık ve nem denetimleri | Alarmlar ile izleme / bildirimleri

**PE-14 (2)** kuruluş kullanır sıcaklık ve nem izleme, bir uyarı veya bildirim değişiklikleri zararlı personel veya ekipman sağlar.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure veri merkezleri sıcaklık ve nem düzeyleri yapı yönetim sistemi (BMS) tarafından sürekli olarak izlenir. Herhangi bir uyarı noktası aşıldı önce bunlar sıcaklık ve nem veri merkezi içinde yönetebilmeniz için veri merkezi personelinin gelen tesis işlemlerini Merkezi (FOC), BMS izleyin. |


 ## <a name="nist-800-53-control-pe-15"></a>NIST 800 53 denetim PE-15

#### <a name="water-damage-protection"></a>Su hasara karşı koruma

**PE-15** anahtar personel erişilebilir, düzgün çalışma ve bilinen ana kesici veya yalıtım vanalar sağlayarak su sızıntısına karşı kaynaklanan hasarlardan bilgi sistemi kuruluş korur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure su sızıntısı (örn., hava işleyicileri birimleri) riskini alanlarda su/sızıntısı algılama sağlar. Yangın gizleme sistemleri izlenen sızıntısı algılama uyarıları da vardır. Su/sızıntısı algılama sistem tesis uyarı ve bildirim sistemiyle tümleşiktir. Veri merkezleri fıskiyesi sistemlerinde bölgesi. Veri Merkezi personelinin su kesici vanalar ve konumlarını kullanımını gerektiren acil durum yordamlarla biliyorsunuzdur. Fıskiyesi yükseltici seçeneği ayrı ayrı veya ağ geçidi vanalar aracılığıyla bir grup olarak kapatılması sahipsiniz. Kritik alandaki tüm fıskiyeleri iki formları etkinleştirme akışı başlatılmadan önce gerektiren çift kilidi ön eylem türü fıskiyeleri ' dir. Fıskiyesi sistem baskısı izlenen ve su sızıntısına karşı uyarıda. |


 ### <a name="nist-800-53-control-pe-15-1"></a>NIST 800 53 denetim PE-15 (1)

#### <a name="water-damage-protection--automation-support"></a>Su hasara karşı koruma | Otomasyon desteği

**PE-15 (1)** kuruluş bilgileri sistem ve Uyarıları kapsamına su varolup olmadığını algılamak için otomatik mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri merkezlerinde su varolup olmadığını algılamak için otomatik mekanizmaları uygular ve veri merkezi personelinin uyarır. Azure su sızıntısı (örn., hava işleyicileri birimleri) riskini alanlarda su/sızıntısı algılama sağlar. Yangın gizleme sistemleri izlenen sızıntısı algılama uyarıları da vardır. Su/sızıntısı algılama sistem tesis uyarı ve bildirim sistemiyle tümleşiktir. Fıskiyesi sistem baskısı izlenen ve su sızıntısına karşı uyarıda. |


 ## <a name="nist-800-53-control-pe-16"></a>NIST 800 53 denetim PE-16

#### <a name="delivery-and-removal"></a>Teslim ve kaldırma

**PE-16** kuruluş yetkilendirir, izler ve denetimleri [atama: kuruluş tanımlı türler bilgileri sistem bileşenlerinin] girme ve tesis çıkma ve bu öğelerden kayıtları korur.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bu gereksinim müşteriler adına uygular. Microsoft Azure girin ve veri merkezi çıkış izin verilen, katı zorlama uygular. Tüm sistem bileşenleri/varlıklar, varlık yönetimi aracı veritabanında izlenir. |


 ## <a name="nist-800-53-control-pe-17a"></a>NIST 800 53 denetim PE-17.a

#### <a name="alternate-work-site"></a>Alternatif iş Site

**PE 17.a** kuruluş kullanır [atama: kuruluş tarafından tanımlanan güvenlik denetimleri] alternatif iş sitelerdeki.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde alternatif iş site sağlarken bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-pe-17b"></a>NIST 800 53 denetim PE-17.b

#### <a name="alternate-work-site"></a>Alternatif iş Site

**PE 17.b** kuruluş uygun olarak değerlendirir, güvenlik verimliliğini alternatif iş sitelerde denetler.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde alternatif iş site sağlarken bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-pe-17c"></a>NIST 800 53 denetim PE-17.c

#### <a name="alternate-work-site"></a>Alternatif iş Site

**PE 17.c** kuruluşun güvenlik olaylarına veya sorunları durumunda bilgi güvenliği personeli ile iletişim kurmak çalışanlar için bir yol sağlar.

**Sorumlulukları:** `Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde alternatif iş site sağlarken bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-pe-18"></a>NIST 800 53 denetim PE-18

#### <a name="location-of-information-system-components"></a>Konum bilgileri sistem bileşenleri

**PE 18** kuruluş bilgileri sistem bileşenleri olası zarar en aza indirmek için tesis içinde konumlandırır [atama: kuruluş tarafından tanımlanan fiziksel ve ortam tehlikesi] ve yetkisiz için Fırsat en aza indirmek için erişim.

**Sorumlulukları:** `Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Azure bilgileri sistem bileşenleri denetimin konumunu karşılamak üzere stratejik datacenter tasarım yaklaşımı uygular. Tüm Microsoft online services donanım hırsızlığı, yangın, patlayıcılar, Duman, su, toz, Titreşim, deprem, zararlı chemicals, elektrik girişim gibi çevresel riskleri korunacak mühendislik konumlarda yerleştirildiğinde, güç kesintileri, elektrik kesilmelerinden (ani) ve Radyasyonu. Tesis ve altyapı çevresel riskleri karşı koruma için Sismik destek oluştur uyguladık. Tüm MDF odaları ve birlikte bulundurma, erişim denetimi, uyarılar ve video tarafından korunur. Tesis ayrıca güvenlik görevlileri 7 x 24 tarafından patrolled. Tüm taşınabilir Azure varlıklar kilitli veya hırsızlığı veya hareketini hasarı karşı koruma sağlamak için yerinde fastened. |


