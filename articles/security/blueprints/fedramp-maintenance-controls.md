---
title: "FedRAMP Azure şeması Otomasyonu - bakım"
description: "Web uygulamaları FedRAMP - bakım için"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 53acae01-bea9-4570-a9bc-734ff65262ba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: a0546f6e10b04bbfdb5b02e5c0bbe6d907c76e72
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="maintenance-ma"></a>Bakım (MA)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-ma-1"></a>NIST 800 53 denetim MA-1

#### <a name="system-maintenance-policy-and-procedures"></a>Sistem Bakımı ilke ve yordamları

**MA-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt, koordinasyonu sistem bakım İlkesi Kurumsal varlıklar ve uyumluluk arasında; ve ilişkili sistem bakımı denetimleri ve Sistem Bakımı ilke uygulaması kolaylaştırmak için yordamlar; gözden geçirir ve geçerli sistem bakımı İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve Bakım yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde Sistem Bakımı ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-2a"></a>NIST 800 53 denetim MA-2.a

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.a** kuruluş zamanlar, gerçekleştirir, belgeler ve Bakım ve onarım bilgileri sistem bileşenlere üreticisi veya Satıcı belirtimleri ve/veya kuruluş gereksinimlerine göre kayıtlarını gözden geçirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için denetimli bakım sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-2b"></a>NIST 800 53 denetim MA-2.b

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.b** kuruluş onaylar ve tüm bakım etkinlikleri sitesinde gerçekleştirilen olup olmadığını izler veya uzaktan ve olup ekipman sitesinde hizmet veya başka bir konuma kaldırıldı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için denetimli bakım sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-2c"></a>NIST 800 53 denetim MA-2.c

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.c** kuruluş gerektiren [atama: kuruluş tarafından tanımlanan personel ya da roller] açıkça bilgi sistemi veya şirket dışı Bakım veya onarım kuruluş tesislerinde sistem bileşenlerini kaldırılmasını onaylayın.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure'un bu özellik gerekir (örneğin, ağ aygıtı veya sunucu) varlıklar aktarımı site dışı gerektiren sahip açık varlık sahibinin onayı. |


 ## <a name="nist-800-53-control-ma-2d"></a>NIST 800 53 denetim MA-2.d

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.d** kuruluşun şirket dışı Bakım veya onarım kuruluş tesislerinde kaldırma önce ilişkili medya tüm bilgileri kaldırmak için donanım temizler.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure'nın varlık koruma standart varlıklar site dışı aktarımı için gerekli önlemleri işleme varlık tanımlar. Varlık koruma standart veri depolama varlıklarını datacenter çıkmadan önce temizlenmiş/temizlendi NIST SP 800-88, medya Temizleme için yönergeleri ile tutarlı şekilde olmasını gerektirir. |


 ## <a name="nist-800-53-control-ma-2e"></a>NIST 800 53 denetim MA-2.e

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.e** Kuruluş düzgün Bakım veya onarım eylemleri aşağıdaki denetimleri hala çalıştığını doğrulamak için tüm olası etkilenen güvenlik denetimleri denetler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için denetimli bakım sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-2f"></a>NIST 800 53 denetim MA-2.f

#### <a name="controlled-maintenance"></a>Denetimli bakım

**MA 2.f** kuruluş içerir [atama: Bakım ilgili bilgiler kuruluş tarafından tanımlanan] kuruluş bakım kayıtlarında.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için denetimli bakım sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-2-2a"></a>NIST 800 53 denetim MA-2 (2) bir

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Bakım denetlenen | Otomatik bakım etkinlikleri

**MA-2 (2) bir** kuruluş zamanlama, kuralları, belge Bakım ve onarım otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bakım etkinliklerini otomatikleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-2-2b"></a>NIST 800 53 denetim MA-2 (2) .b

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Bakım denetlenen | Otomatik bakım etkinlikleri

**MA-2 (2) .b** güncel, doğru kuruluş üretir ve tam tüm bakım ve onarım eylemlerin istenen kayıtları, zamanlanan, işlemde ve tamamlandı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bakım etkinliklerini otomatikleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-3"></a>NIST 800 53 denetim MA-3

#### <a name="maintenance-tools"></a>Bakım araçları

**MA-3** kuruluş onaylar, denetimleri ve bilgi Sistem Bakımı araçları izler.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure bakım tamamlamak için çeşitli araçlar kullanır. Araçlar onaylanmış, denetlenen ve Microsoft Azure ile korunan yazılım bakımı değiştirmek ve yayın işlem. <br /> Veri Merkezi içinde kullanım için onaylı bakım araçlarını envanterini Site Hizmetleri ekibi tutar (MA 3 bakın). Bakım personel sağlanan bakım araçlarını kullanmak üzere yönlendirilir. Veri Merkezi Yönetimi onayına datacenter tarafından sağlanmayan araçları kullanmak için gereklidir. Fiziksel el Araçları (tornavidalar, ayarlı anahtarlar, vb.), bu denetimden dışındadır. <br /> Her tesis kısıtlı fiziksel Kilitle kutusunu veya fluke ether kapsamları, fluke fiber kanal sınayıcılar, Ethernet toners vb. gibi özelleştirilmiş bakım Araçları'nın depolama için erişim kontrol yeri içerir. Site hizmetlerini takım tüm araçlar durumunu doğrulamak için rutin Envanter denetimleri de gerçekleştirir. Kutusunu veya bakım depolama odası kilitlemek için erişim durumunda bir araştırma kullanılabilir erişim rozet okuyucu günlükler izlenir. |


 ### <a name="nist-800-53-control-ma-3-1"></a>NIST 800 53 denetim MA-3 (1)

#### <a name="maintenance-tools--inspect-tools"></a>Bakım araçlarını | Araçlar inceleyin.

**MA-3 (1)** yanlış ya da yetkisiz değişiklikler için bakım personeli tarafından tesisi gerçekleştirilen bakım araçlarını kuruluş inceler.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure'nın Site Hizmetleri ekibi veri merkezi içinde kullanım için onaylı bakım araçlarını envanterini tutar (daha fazla ayrıntı için bkz: MA 3). Bakım personel sağlanan bakım araçlarını kullanmak üzere yönlendirilir. DCM onay datacenter tarafından sağlanmayan araçları kullanmak için gereklidir. |


 ### <a name="nist-800-53-control-ma-3-2"></a>NIST 800 53 denetim MA-3 (2)

#### <a name="maintenance-tools--inspect-media"></a>Bakım araçlarını | Medya inceleyin.

**MA-3 (2)** medya bilgileri sistemde kullanılmadan önce kuruluş medya tanılama içeren ve test programlar için kötü amaçlı kod denetler.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure mobil bilgisayar veya veri merkezi yönetimi onayı olmadan Microsoft Azure veri merkezleri, üretim ortamında depolama medyası kullanımını engeller. Microsoft Azure veri merkezleri üretim ortamında kullanılan kişisel ait medya yasaktır. <br /> Microsoft Azure Microsoft Azure veri merkezleri üretim ortamında kullanılan önce dizüstü incelemek için bir işlem uyguladı. Güvenlik görevlileri dizüstü bilgisayarların, dizüstü yapılması sahip ve İnceleme geçirilen doğrulamak için üretim ortamında kullanmadan personel sınama eğitilmiş olup. |


 ### <a name="nist-800-53-control-ma-3-3"></a>NIST 800 53 denetim MA-3 (3)

#### <a name="maintenance-tools--prevent-unauthorized-removal"></a>Bakım araçlarını | Yetkisiz kaldırma engelle

**MA-3 (3)** kuruluş hiçbir donanım üzerinde bulunan kuruluş bilgilerini; temizleme veya yok etme olduğunu doğrulayarak kuruluş bilgilerini içeren bakım ekipman yetkisiz kaldırılmasını engeller ekipman; tesis içinde donanım korur; veya bir muafiyet alma [atama: kuruluş tarafından tanımlanan personel ya da roller] açıkça tesis gelen ekipmanının kaldırma yetkisi verme.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure tesis içinde korunur ve kaldırılmıyor datacenter belirli bakım araçlarını kullanır. Her tesis fluke ether kapsamları, vb. fluke fiber kanal sınayıcılar, Ethernet toners bakım araçlarını depolayan bir kısıtlı fiziksel kilit kutusu veya depolama yer içerir. Erişim bakım araçlarını yetkisiz erişimi önlemek için DCAT kilit kutusu veya depolama odada için denetlenir. <br /> Kuruluş bilgilerini bakım sırasında MA 4 denetimlerinde korunur. Kuruluş bilgilere erişmek için kullanıcı hesapları ve Doğrulayıcı ayrıcalıklı gerekir. |


 ## <a name="nist-800-53-control-ma-4a"></a>NIST 800 53 denetim MA-4.a

#### <a name="nonlocal-maintenance"></a>Yerel olmayan bakım

**MA 4.a** kuruluş onaylar ve yerel olmayan Bakım ve tanılama etkinliklerini izler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan işletim sistemlerinde yerel olmayan bakım gerçekleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-4b"></a>NIST 800 53 denetim MA-4.b

#### <a name="nonlocal-maintenance"></a>Yerel olmayan bakım

**MA 4.b** kuruluş yerel olmayan bakım kullanılmasına izin verir ve tanılama araçları yalnızca olarak kuruluş ilkesi ile tutarlı ve bilgi sistemi güvenlik planı belgelenen.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan işletim sistemlerinde yerel olmayan bakım gerçekleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-4c"></a>NIST 800 53 denetim MA-4.c

#### <a name="nonlocal-maintenance"></a>Yerel olmayan bakım

**MA 4.c** kuruluş güçlü Doğrulayıcı olmayan Bakım ve tanılama oturumları kurulmasını içinde kullanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan işletim sistemlerinde yerel olmayan bakım gerçekleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-4d"></a>NIST 800 53 denetim MA-4.d

#### <a name="nonlocal-maintenance"></a>Yerel olmayan bakım

**MA 4.d** kuruluş yerel olmayan Bakım ve tanılama etkinliklerin kayıtları korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan işletim sistemlerinde yerel olmayan bakım gerçekleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-4e"></a>NIST 800 53 denetim MA-4.e

#### <a name="nonlocal-maintenance"></a>Yerel olmayan bakım

**MA 4.e** Yerel olmayan bakım tamamlandığında, kuruluşun oturum ve ağ bağlantılarını sonlandırır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan işletim sistemlerinde yerel olmayan bakım gerçekleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-4-2"></a>NIST 800 53 denetim MA-4 (2)

#### <a name="nonlocal-maintenance--document-nonlocal-maintenance"></a>Yerel olmayan bakım | Belge olmayan bakım

**MA-4 (2)** bilgi sistemi, ilkeleri ve yordamlar oluşturulması ve yerel olmayan Bakım ve tanılama bağlantıları kullanımı için güvenlik planlama kuruluş belgelerde.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri müşteri tarafından dağıtılan işletim sistemleri için güvenlik planında yerel olmayan bakım belgelemesinden sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-4-3"></a>NIST 800 53 denetim MA-4 (3)

#### <a name="nonlocal-maintenance--comparable-security--sanitization"></a>Yerel olmayan bakım | Karşılaştırılabilir güvenlik / temizleme

**MA-4 (3)** yerel olmayan Bakım ve tanılama Hizmetleri karşılaştırılabilir verilen; sistemde uygulanan özelliği için bir güvenlik özelliği uygulayan veya kaldıran bir bilgi sisteminden gerçekleştirilmesi kuruluşun gerektirir yerel olmayan Bakım veya tanı Hizmetleri önce bilgi sisteminden verilecek bileşen bileşeni (açısından kuruluş bilgilerini) kuruluş tesis ve hizmet sonra kaldırılmadan önce temizler gerçekleştirilen, inceler ve bilgi sistemi bileşenine yeniden bağlanmadan önce bileşen (açısından, kötü amaçlı yazılım) temizler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri karşılaştırılabilir güvenliği sahip bir bilgi sisteminden müşteri tarafından dağıtılan işletim sistemlerinin tüm yerel olmayan bakım gerçekleştirmek için sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-4-6"></a>NIST 800 53 denetim MA-4 (6)

#### <a name="nonlocal-maintenance--cryptographic-protection"></a>Yerel olmayan bakım | Şifreleme koruma

**MA-4 (6)** bilgileri sistem bütünlüğü ve gizliliği yerel olmayan Bakım ve tanılama iletişimleri korumak için şifreleme mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, şifreleme mekanizmasıyla uygulamak için yerel olmayan Bakım ve müşteri tarafından dağıtılan işletim sistemlerinin tanılama gerçekleştirirken sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-5a"></a>NIST 800 53 denetim MA-5.a

#### <a name="maintenance-personnel"></a>Bakım personel

**MA 5.a** kuruluş bakım personel yetkilendirme için bir işlem oluşturur ve yetkili bakım kuruluşlar veya personel listesini tutar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Yetkilendirme ve escort Müşteri'nin kuruluş düzeyinde Sistem Bakımı personel işlemleri bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-5b"></a>NIST 800 53 denetim MA-5.b

#### <a name="maintenance-personnel"></a>Bakım personel

**MA 5.b** escorted personel bilgi sistemde bakım gerçekleştirme erişim kimlik doğrulamalarını gerekli sahip kuruluş sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Yetkilendirme ve escort Müşteri'nin kuruluş düzeyinde Sistem Bakımı personel işlemleri bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-5c"></a>NIST 800 53 denetim MA-5.c

#### <a name="maintenance-personnel"></a>Bakım personel

**MA 5.c** gerekli erişim kimlik doğrulamalarını ve gerekli erişim yetkilerini sahip olmayan personeli bakım etkinliklerini denetlemek için teknik yetki ile kuruluş personel kuruluş belirler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Yetkilendirme ve escort Müşteri'nin kuruluş düzeyinde Sistem Bakımı personel işlemleri bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-5-1a"></a>NIST 800 53 denetim MA-5 (1) bir

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Bakım personel | Uygun erişim olmadan kişiler

**MA-5 (1) bir** kuruluş eksikliği uygun güvenlik düzeylerinin veya gerekli olan aşağıdaki gereksinimleri bakım personeli içeren ABD kullanıcılarının olmayan bakım personelinin kullanmak için yordamlar uygular erişim kimlik doğrulamalarını, düzeylerinin veya onayları escorted ve Bakım ve tanılama bilgileri sistem etkinliklerini performansını sırasında tam olarak temizlenir, onaylanan kuruluş personeli tarafından denetlenen resmi erişimine sahip uygun erişim kimlik doğrulamalarını ve bu teknik olarak tam; başlatma Bakım veya tanılama etkinlikleri personeli tarafından önce kimin erişim kimlik doğrulamalarını, düzeylerinin veya resmi erişim onaylar, depolama bileşenleri bilgi sistemi içinde ayıklanır tüm volatile bilgileri ve tüm gerekli sahip değil kalıcı depolama medyası kaldırıldı veya fiziksel olarak sistemden bağlantısı kesilen ve güvenli.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Yetkilendirme ve escort Müşteri'nin kuruluş düzeyinde Sistem Bakımı personel işlemleri bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ma-5-1b"></a>NIST 800 53 denetim MA-5 (1) .b

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Bakım personel | Uygun erişim olmadan kişiler

**MA-5 (1) .b** kuruluş geliştirir ve Implements alternatif güvenlik önlemleri bilgileri sistem bileşeni ayıklanır, kaldırılamıyor veya sistemden bağlantısı kesildi durumunda.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Yetkilendirme ve escort Müşteri'nin kuruluş düzeyinde Sistem Bakımı personel işlemleri bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ma-6"></a>NIST 800 53 denetim MA-6

#### <a name="timely-maintenance"></a>Zamanında bakım

**MA-6** kuruluş bakım için destek ve/veya yedek bölümleri edinir [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] içinde [atama: kuruluş tanımlı süre] hata.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteriler fiziksel tüm sistem kaynakları Azure veri merkezlerinde erişiminiz yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure veri merkezleri, veri merkezi işlemlerinin yanı sıra kritik veri merkezi altyapı sistemleri desteklemek için yerleşik bakım personel korur. Takımlar kritik güvenlik ve teknoloji sistem bileşenleri tanımladınız, bunlar korumak için yerinde ödemek zorunda kalmamasını sağlar. Kritik sistemler N + 1 yapılandırmasında tasarlanmıştır ve Hizmetleri dayanıklı olacak şekilde tasarlanmıştır. Bu, bir hizmet kesintisi veya Yedek planı durum durumunda kurtarma hedeflerinizi karşılamak veri merkezi yönetimi ekibi sağlar. Önemli bilgileri Sistem Hizmetleri veri merkezleri birinde bir olay nedeniyle hizmetindeki kesintiyi önlemek için birden fazla veri merkezi gelen sağlanır. Müşteri uygulamalar, artıklık ve dayanıklılık sağlamak üzere birden çok veri merkezi dağıtmak için sorumludur. |
