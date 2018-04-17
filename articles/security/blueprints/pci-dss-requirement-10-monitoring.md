---
title: Azure ödeme işleme şeması gereksinimleri izleme-
description: PCI DSS gereksinim 10
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 293a1673-54bc-478c-9400-231074004eee
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 708c57c1d7b79d3fd3c129de9a7ce4099ab6ac36
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="monitoring-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için izleme gereksinimleri 
## <a name="pci-dss-requirement-10"></a>PCI DSS gereksinim 10

**İzleme ve tüm ağ kaynaklarına erişim ve kart sahibi veri izleme**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Günlüğe kaydetme mekanizmaları ve kullanıcı etkinliklerini takip etme imkanı önleme, algılama veya bir veri tehlikeye etkisini en aza önemlidir. Bir şeyler ters gittiğinde tüm ortamlar günlüklerinde varlığını kapsamlı izleme, uyarma ve analiz sağlar. Bir güvenlik açığı nedenini belirlemek çok imkansız, sistem etkinlik günlükleri olmadan zordur.

## <a name="pci-dss-requirement-101"></a>PCI DSS gereksinim 10.1

**10.1** sistem bileşenleri tek tek her kullanıcı için tüm erişim bağlamak için uygulama denetim izleri.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure erişimi yetkili personel yönetim ve tanılama araçları ile ilgili iş sorumluluk kısıtlar. Microsoft Azure en az ayrıcalık kurallara göre üretim ortamında kullanılan araçlar ayrıcalıklı erişimi sınırlandırır. Microsoft Azure kaydeder ve platform ortamında Microsoft Azure sistemi bileşenlerine tüm tek tek kullanıcı erişim günlüğünü tutar.<br /><br />Microsoft Azure platform bileşenleri (işletim sistemi, CloudNet, doku vb. dahil), oturum ve güvenlik olaylarını toplamak için yapılandırılır. Microsoft Azure platformu yönetici etkinliğinde günlüğe kaydedilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore ayrıntılı günlük kaydını ve tüm sistem, kullanıcı etkinliği (CHD dahil olmak üzere günlük) sahiptir. Daha fazla bilgi için bkz: [PCI Kılavuzu - günlük kaydı](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-102"></a>PCI DSS gereksinim 10.2

**10.2** gerçekleştir otomatik denetim izleri aşağıdaki olaylar yeniden oluşturmak tüm sistem bileşenleri için:
- **10.2.1** kart sahibi verilere tüm tek tek kullanıcı erişimi
- **10.2.2** kök veya yönetici ayrıcalıkları ile her kullanıcı tarafından gerçekleştirilen tüm eylemler
- **10.2.3** tüm denetim izleri erişimi
- **10.2.4** geçersiz mantıksal erişim deneme
- **10.2.5** kullanımını ve değişiklikleri tanımlama ve kimlik doğrulama mekanizmaları — dahil ancak bunlarla sınırlı olmamak üzere yeni hesapların oluşturulmasını ve ayrıcalıkların — ve tüm değişiklikleri, ekleme veya silme işlemleri köke sahip veya yönetici hesapları için ayrıcalıkları
- **10.2.6** başlatma, durdurma veya duraklatma denetim günlüklerini
- **10.2.7** oluşturma ve sistem düzeyindeki nesneleri silme

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure erişimi yetkili personel yönetim ve tanılama araçları ile ilgili iş sorumluluk kısıtlar. Microsoft Azure en az ayrıcalık kurallara göre üretim ortamında kullanılan araçlar ayrıcalıklı erişimi sınırlandırır. Microsoft Azure kaydeder ve platform ortamında Microsoft Azure sistemi bileşenlerine tüm tek tek kullanıcı erişim günlüğünü tutar.<br /><br />Microsoft Azure platform bileşenleri (işletim sistemi, CloudNet, doku vb. dahil), oturum ve güvenlik olaylarını toplamak için yapılandırılır. Microsoft Azure platformu yönetici etkinliğinde günlüğe kaydedilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore CHD günlükleri dahil olmak üzere tüm sistemi ve kullanıcı etkinliğini kapsamlı günlüğü vardır. Daha fazla bilgi için bkz: [PCI Kılavuzu - günlük kaydı](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-103"></a>PCI DSS gereksinim 10.3

**10.3** en az her olay için tüm sistem bileşenleri için aşağıdaki denetim izi girişlerini kaydedin:
- **10.3.1** kullanıcı kimliği
- **10.3.2** olay türü
- **10.3.3** tarih ve saat
- **10.3.4** başarı veya başarısızlık göstergesi
- **10.3.5** olay oluşturulma
- **10.3.6** kimliği veya etkilenen veri, sistem bileşeni veya kaynak adı

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure sunucuları eşitlemek için yordamlar kurulduğunda ve NTP Katman 1 saat sunucuları ile Microsoft Azure ortamında ağ aygıtlarını uydunuz Genel Konumlandırma Sistemi (GPS için) eşitlenir. Eşitleme beş dakikada bir otomatik olarak gerçekleştirilir. Microsoft Azure hizmet konakları düzgün eşitleme zamanı sağlamak için sorumludur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, kullanıcı kimliği, olay türü, tarih ve saat damgası, başarısız başarı olayları, olay kaynağı ve 10.3 denetim gerektirdiği kaynağın adını kaydeder.|



## <a name="pci-dss-requirement-104"></a>PCI DSS gereksinim 10.4

**10.4** zaman eşitleme teknolojisini kullanarak, tüm kritik sistem saatleri ve saatler eşitleyin ve aşağıdaki alınırken, dağıtma ve zaman depolamak için uygulanan emin olun. 
> [!NOTE]
> Zaman eşitleme teknolojisi ağ zaman Protokolü (NTP) örneğidir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure sunucuları eşitlemek için yordamlar kurulduğunda ve NTP Katman 1 saat sunucuları ile Microsoft Azure ortamında ağ aygıtlarını uydunuz Genel Konumlandırma Sistemi (GPS için) eşitlenir. Eşitleme beş dakikada bir otomatik olarak gerçekleştirilir. Microsoft Azure hizmet konakları düzgün eşitleme zamanı sağlamak için sorumludur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Zaman eşitleme PaaS hizmeti için Azure tarafından gerçekleştirilir.|



### <a name="pci-dss-requirement-1041"></a>PCI DSS gereksinim 10.4.1

**10.4.1** kritik sistemler doğru ve tutarlı süresi vardır.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.4](#pci-dss-requirement-10-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Zaman eşitleme PaaS hizmeti için Azure tarafından gerçekleştirilir.|



### <a name="pci-dss-requirement-1042"></a>PCI DSS gereksinim 10.4.2

**10.4.2** zaman veriler korunur.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.4](#pci-dss-requirement-10-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Zaman eşitleme PaaS hizmeti için Azure tarafından gerçekleştirilir.|



### <a name="pci-dss-requirement-1043"></a>PCI DSS gereksinim 10.4.3

**10.4.3** saat ayarlarını endüstri kabul zaman kaynağından alındı.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.4](#pci-dss-requirement-10-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Zaman eşitleme PaaS hizmeti için Azure tarafından gerçekleştirilir.|



## <a name="pci-dss-requirement-105"></a>PCI DSS gereksinim 10.5

**10.5** güvenli denetim izlerinin değiştirilemez böylece.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | FIM ve Kimlikler araçları Microsoft Azure ortamında uygulanır. Microsoft Azure EWS işletimsel ortamı içindeki olayların gerçek zamanlı analiz desteklemek için kullanır. Sistem tehlikeye atabilecek olayları hakkında gerçek zamanlı uyarılar yakın mAs ve AMAÇLAR oluşturur. <br /><br />Hizmet, kullanıcı ve güvenlik olaylarını (web sunucusu günlükleri, FTP sunucusu günlükleri vb.) etkin ve merkezi olarak korunur. Azure denetim günlükleri yetkili personel iş sorumluluklarını göre erişimi kısıtlar. Olay günlükleri, Azure güvenli arşivleme altyapı arşivlenmiş ve 180 gün için korunur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1051"></a>PCI DSS gereksinim 10.5.1

**10.5.1** denetim sınırı görüntülenmesi izlerinin iş ile ilgili bir gereksinimi olanlar için.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.5](#pci-dss-requirement-10-5). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1052"></a>PCI DSS gereksinim 10.5.2

**10.5.2** koruma denetim izi dosyaları yetkisiz değişiklikler.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.5](#pci-dss-requirement-10-5). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1053"></a>PCI DSS gereksinim 10.5.3

**10.5.3** derhal denetim izi dosyaları Merkezi günlük sunucu veya alter zordur medya için yedekleme.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.5](#pci-dss-requirement-10-5). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1054"></a>PCI DSS gereksinim 10.5.4

**10.5.4** yazma güvenli, merkezi, iç günlük sunucu veya medya cihaza dışa dönük teknolojileri için günlükleri.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.5](#pci-dss-requirement-10-5). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1055"></a>PCI DSS gereksinim 10.5.5

**10.5.5** mevcut günlük verileri (eklenmekte olan yeni veri uyarı oluşmamalıdır rağmen) uyarıları oluşturmadan değiştirilemez emin olmak için günlükleri dosyası bütünlüğü izleme veya değişiklik algılama yazılımı kullanın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.5](#pci-dss-requirement-10-5). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore Azure tüm öğelerin denetimi için sağlar. Yedekleme dış kaynak tarafından gerçekleştirilebilir [Azure Backup](https://azure.microsoft.com/services/backup/).|



## <a name="pci-dss-requirement-106"></a>PCI DSS gereksinim 10.6

**10.6** günlüklerini ve anormallikleri veya şüpheli etkinlik tanımlamak tüm sistem bileşenlerinin güvenlik olayları gözden geçirin.
 
> [!NOTE]
> Toplama, ayrıştırma ve araçları, bu gereksinimi karşılamak için kullanılabilir uyarı günlüğü.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | FIM ve Kimlikler araçları Microsoft Azure ortamında uygulanır. Microsoft Azure EWS işletimsel ortamı içindeki olayların gerçek zamanlı analiz desteklemek için kullanır. Sistem tehlikeye atabilecek olayları hakkında gerçek zamanlı uyarılar yakın mAs ve AMAÇLAR oluşturur. <br /><br />Hizmet, kullanıcı ve güvenlik olaylarını (web sunucusu günlükleri, FTP sunucusu günlükleri vb.) etkin ve merkezi olarak korunur. Azure denetim günlükleri yetkili personel iş sorumluluklarını göre erişimi kısıtlar. Olay günlükleri, Azure güvenli arşivleme altyapı arşivlenmiş ve 180 gün için korunur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanan [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) izlemek, rapor ve anormallikleri önlemek için. [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar.|



### <a name="pci-dss-requirement-1061"></a>PCI DSS gereksinim 10.6.1

**10.6.1** en az günlük aşağıdakileri gözden geçirin:
- Tüm güvenlik olayları
- Tüm sistem bileşenlerinin depolamak, işlem veya CHD ve/veya SAD iletme günlükleri
- Tüm kritik sistem bileşenlerinin günlükleri
- Tüm sunucuları ve güvenlik işlevleri (örneğin, güvenlik duvarları, izinsiz giriş algılama sistemleri/ihlal engelleme sistemleri (Kimlikleri/IP), kimlik doğrulama sunucuları, e-ticaret yeniden yönlendirme sunucuları ve benzeri) gerçekleştirmek sistem bileşenleri günlüğe kaydedilir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.6](#pci-dss-requirement-10-6). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanan [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) izlemek, rapor ve anormallikleri önlemek için. [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar.|



### <a name="pci-dss-requirement-1062"></a>PCI DSS gereksinim 10.6.2

**10.6.2** düzenli aralıklarla kuruluşunuzun ilkeleri ve risk yönetimi stratejilerine, kuruluşun yıllık risk değerlendirmesi tarafından belirlendiği şekilde göre diğer sistem bileşenlerini günlüklerini inceleyin.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 10.6](#pci-dss-requirement-10-6). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanan [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) izlemek, rapor ve anormallikleri önlemek için. [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar.|



### <a name="pci-dss-requirement-1063"></a>PCI DSS gereksinim 10.6.3

**10.6.3** özel durumlar ve gözden geçirme sürecinde tanımlanan anormallikleri izleyin.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | "Microsoft Azure" bölümüne bakın [gereksinim 10.6](#pci-dss-requirement-10-6). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanan [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) izlemek, rapor ve anormallikleri önlemek için. [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar.|



## <a name="pci-dss-requirement-107"></a>PCI DSS gereksinim 10.7

**10.7** en az üç ay hemen kullanılabilir çözümleme için (örneğin, çevrimiçi, arşivlenen veya yedekten geri yüklenebilen) ile en az bir yıl boyunca Beklet denetim izi geçmişi.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure denetim günlükleri kendi iç Portalı aracılığıyla hemen erişilebilir en son 3 ay ile bir yıl için korur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore ayrıntılı günlük kaydını ve tüm sistem, kullanıcı etkinliği (CHD dahil olmak üzere günlük) sahiptir. Daha fazla bilgi için bkz: [PCI Kılavuzu - günlük kaydı](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-108"></a>PCI DSS gereksinim 10.8

**10.8** **yalnızca hizmet sağlayıcıları için ek gereksinimi:** zamanında algılanması için bir işlem uygulayın ve kritik güvenlik denetim sistemleri dahil ancak bunlarla sınırlı olmamak başarısız, hata raporlama:
- Güvenlik Duvarları
- KİMLİKLERİ/IP'LERİ
- FIM
- Virüsten koruma
- Fiziksel erişim denetimi
- Mantıksal erişim denetimleri
- Denetim günlüğü mekanizmaları
- (Kullanılıyorsa) kesimleme denetimleri 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.



**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure EWS işletimsel ortamı içindeki olayların gerçek zamanlı analiz desteklemek için kullanır. Sistem tehlikeye atabilecek olayları hakkında gerçek zamanlı uyarılar yakın mAs ve AMAÇLAR oluşturur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore ayrıntılı günlük kaydını ve tüm sistem, kullanıcı etkinliği (CHD dahil olmak üzere günlük) sahiptir. Daha fazla bilgi için bkz: [PCI Kılavuzu - günlük kaydı](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-1081"></a>PCI DSS gereksinim 10.8.1

**10.8.1** **yalnızca hizmet sağlayıcıları için ek gereksinimi:** herhangi bir kritik güvenlik denetim hatalarına neden zamanında yanıt. Güvenlik denetimleri de hataları yanıt işlemler şunları içermelidir:
- Güvenlik işlevleri geri yükleme
- Tanımlama ve süresini (tarihi ve saati baştan sona) belgeleme güvenlik hata
- Tanımlama ve hata düzeltme adresi kök neden gerekli belgeleme ve kök neden dahil cause(s) belgeleme
- Tanımlama ve sırasında hata çıkan tüm güvenlik sorunlarını adresleme
- Daha fazla Eylemler sonucunda güvenlik hatası gerekip gerekmediğini belirlemek için bir risk değerlendirmesi gerçekleştirme
- Hatanın nedenini önlemek için denetimler uygulamak yinelenmeye - güvenlik denetimleri için izleme sürdürülüyor 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure EWS işletimsel ortamı içindeki olayların gerçek zamanlı analiz desteklemek için kullanır. Sistem tehlikeye atabilecek olayları hakkında gerçek zamanlı uyarılar yakın mAs ve AMAÇLAR oluşturur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore ayrıntılı günlük kaydını ve tüm sistem, kullanıcı etkinliği (CHD dahil olmak üzere günlük) sahiptir. Daha fazla bilgi için bkz: [PCI Kılavuzu - günlük kaydı](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-109"></a>PCI DSS gereksinim 10.9

**10.9** güvenlik ilkeleri ve tüm ağ kaynaklarına erişim ve kart sahibi veri izleme işletimsel yordamları, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanım örneği ve CHD nasıl yönetilir ve korumalı hakkında bir açıklama sağlar.|




