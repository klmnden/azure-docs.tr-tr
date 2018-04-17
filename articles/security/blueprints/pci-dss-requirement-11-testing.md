---
title: Azure ödeme işleme şeması gereksinimleri test-
description: PCI DSS gereksinim 11
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 9753853b-ad80-4be4-9416-2583b8fd2029
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: db9f1022ecb3b727f08bb6f232a8df55476e0755
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="testing-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için test gereksinimleri 
## <a name="pci-dss-requirement-11"></a>PCI DSS gereksinim 11

**Güvenlik sistemleri ve işlemleri düzenli olarak test**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Güvenlik açıkları kötü niyetli kişilere ve araştırmacılarının ve yeni yazılım tarafından eklenen tarafından sürekli olarak keşfedilen. Sistem bileşenleri, işlemler ve özel yazılım sık sık değişen bir ortamda yansıtacak şekilde güvenlik denetimleri devam emin olmak için test edilmelidir.

## <a name="pci-dss-requirement-111"></a>PCI DSS gereksinimi 11.1

**11.1** kablosuz erişim noktaları (802.11) varlığını test ve algılamak ve tüm tanımlamak için uygulama işlemleri yetkili ve kablosuz erişim noktaları üç aylık olarak yetkisiz.

> [!NOTE]
> İşlem sırasında kullanılan yöntemleri içerir, ancak sınırlı kablosuz ağ taramaları, sistem bileşenleri ve altyapı, ağ erişim denetimi (NAC) ya da kablosuz Kimlikleri/IP'leri fiziksel/mantıksal incelemeleri olup olmadığı.
Hangi yöntemlerin kullanılacağını, bunlar algılamak ve yetkili ve yetkisiz cihazları tanımlamak yeterli olmalıdır.


**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure olmayan izin vermek veya kablosuz bağlantıları Azure ağ ortamında izin verin. İç güvenlik ekiplerinden üç aylık olarak yanlış kablosuz sinyalleri için düzenli olarak tarar ve yanlış sinyalleri araştırılan kaldırılır ve. Müşteriler kablosuz teknolojisini Azure ortamında dağıtmak için izin verilmez. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Kablosuz ve SNMP çözümde uygulanmadı.|



### <a name="pci-dss-requirement-1111"></a>PCI DSS gereksinim 11.1.1

**11.1.1** belgelenen iş gerekçesinin dahil olmak üzere yetkili kablosuz erişim noktaları, bir stoğunu koruyun.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinimi 11.1](#pci-dss-requirement-11-1). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Kablosuz ve SNMP çözümde uygulanmadı.|



### <a name="pci-dss-requirement-1112"></a>PCI DSS gereksinim 11.1.2

**11.1.2** yetkisiz kablosuz erişim noktaları algılanan durumunda olay yanıtlama yordamları uygulayın.


**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinimi 11.1](#pci-dss-requirement-11-1). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Kablosuz ve SNMP çözümde uygulanmadı.|



## <a name="pci-dss-requirement-112"></a>PCI DSS gereksinim 11.2

**11.2** iç çalıştırın ve dış ağ güvenlik açığı taramaları en az üç aylık ve ağa (örneğin, yeni sistem bileşeni yüklemeleri, ağ topolojisini, güvenlik duvarı kuralı değişiklikler, ürün değişiklikleri önemli herhangi bir değişiklikten sonra yükseltmeler). 

> [!NOTE]
> Tüm sistemler taranan ve tüm uygun güvenlik açıkları ele göstermek üç aylık tarama işlemi için birden fazla tarama raporları birleştirilebilir. Ek belgeler düzeltilen olmayan güvenlik açıkları ele sürecinde doğrulamak için gerekebilir.
> İlk PCI DSS uygunluğu değil assessor 1 doğrularsa dört taramaları geçirme üç aylık dönem tamamlanması gereken) en son tarama sonuç geçirmenin tarama, 2) varlık ilkeleri ve üç aylık tarama gerektirme yordamları ve 3 belgelenen) tarama sonuçları not ettiğiniz güvenlik açıkları, bir re-scan(s) gösterildiği gibi düzeltildi. PCI DSS ilk gözden geçirdikten sonra sonraki yıl için dört taramaları geçirme üç aylık dönem oluşmuş gerekir.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure üç aylık iç ve dış güvenlik açığı taramaları gerçekleştirir. Taramaları tam personeli tarafından gerçekleştirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kalem test ve 'olduğundan' çaba güvenlik açığı taraması olmuştur. Kalem test sonuçlarını nmap veya pentest tools.com gibi ortak araçları kullanarak yinelenebilir. Kalem test sonuçlarını Etkilenme öğe ile yetersiz saldırı yüzeyini sağlar. Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) güvenlik açığı bilgileri ve düzeltme sağlar.|



### <a name="pci-dss-requirement-1121"></a>PCI DSS gereksinim 11.2.1

**11.2.1** üç aylık iç güvenlik açığı taramaları gerçekleştirin. Güvenlik açıklarına ve tüm "yüksek riskli" güvenlik açıkları varlığın Güvenlik Açığı (gereksinim 6.1) sıralaması uygun olarak çözümlenmiş doğrulamak için tekrar tarar gerçekleştirin. Taramaları tam personeli tarafından gerçekleştirilmelidir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure, kapsam altyapının güvenlik açıklarını taramalar gerçekleştirir. Microsoft Azure sunucu işletim sistemleri, veritabanları ve uygun güvenlik açığı tarama araçları olan ağ aygıtlarını tarama güvenlik açığı uygular. Azure web uygulamaları ile uygun endüstri çözümleri tarama taranır. Güvenlik Açığı taramaları bir üç aylık temelinde gerçekleştirilir.<br /><br />Tekrar tarar (gereksinimi 6.1 tanımlandığı gibi) tüm "riskli" güvenlik açıkları çözülene kadar tüm sistemler karşı gerektiği gibi gerçekleştirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kalem test ve 'olduğundan' çaba güvenlik açığı taraması olmuştur. Kalem test sonuçlarını nmap veya pentest tools.com gibi ortak araçları kullanarak yinelenebilir. Kalem test sonuçlarını Etkilenme öğe ile yetersiz saldırı yüzeyini sağlar. Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) güvenlik açığı bilgileri ve düzeltme sağlar.|



### <a name="pci-dss-requirement-1122"></a>PCI DSS gereksinim 11.2.2

**11.2.2** üç aylık dış güvenlik açığı taramaları, aracılığıyla bir onaylanmış tarama satıcı (ödeme kartı sektör güvenlik standartları Council tarafından (PCI SSC) onaylanan ASV) gerçekleştirin. Tekrar tarar, geçirme taramaları elde kadar gerektiğinde gerçekleştirin. 

> [!NOTE]
> Üç aylık dış güvenlik açığı taramaları tarafından bir onaylanmış tarama satıcı (ödeme kartı sektör güvenlik standartları Council tarafından (PCI SSC) onaylanan ASV), gerçekleştirilmesi gerekir.
> ASV Program tarama müşteri sorumlulukları, tarama hazırlık vb. için PCI SSC Web sitesinde yayınlanan Kılavuzu'na bakın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure dış taramalar Dışarıdan erişilebilen kapsam altyapının güvenlik açıklarını gerçekleştirir. Taramaları bir onaylanmış tarama satıcı (ASV tarafından) gerçekleştirilir.<br /><br />Microsoft Azure MSRC/OSSC aylık düzeltme eki bildirimlerine abone olur ve güvenlik açıkları için en az üç aylık tarar. Tanımlanan güvenlik açıkları değerlendirilir ve risk düzeyine göre belirlenen zaman çizelgesi başına düzeltilebilir.<br /><br />Microsoft Azure ortamı öncelikli bileşenlerinin karşı tarama her hedeflenen Çeyrek kapsamlı güvenlik açığı güvenlik açıklarını tanımlamak için gerçekleştirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, tanıtım müşterileri dağıtma olduğunda üç aylık dış güvenlik açığı yapmaktan sorumlu tarar ve bir onaylanmış tarama satıcı (ASV) kullanarak tüm PaaS örnekleri karşı kart sahibi veri ortamlarında (CDE) gerektiği gibi yeniden tarar Ödeme Kartı sektör güvenlik standartları Council tarafından onaylanmış.<br /><br />|



### <a name="pci-dss-requirement-1123"></a>PCI DSS gereksinim 11.2.3

**11.2.3** iç ve dış tarar ve, önemli bir değişiklikten sonra gerektiği gibi yeniden tarar gerçekleştirin. Taramaları tam personeli tarafından gerçekleştirilmelidir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Sonuçları katılımcılara bildirilir ve düzeltme kapatma aracılığıyla Azure güvenlik ekibi tarafından izlenir. Azure test sonuçlarını müşterilerle Gizlilik Sözleşmesi altında paylaşılabilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Müşteriler, üç aylık iç ve dış güvenlik açığı taramaları gerçekleştirmek için sorumlu ve bunların CDE tüm PaaS örnekleri karşı gerektiği gibi yeniden tarar. Kapsam içinde ortamında önemli değişikliklerden sonra taramaları gerçekleştirilmesi gerekir.<br /><br />Taramaları kuruluş bağımsızlığı ile bir ASV veya personeli tarafından gerçekleştirilmelidir.|



## <a name="pci-dss-requirement-113"></a>PCI DSS gereksinim 11,3 inç

**11,3 inç** bir Metodoloji sızma test etmek için aşağıdakileri içeren uygulayın:
- Üzerinde endüstri kabul sızma yaklaşımlar (örneğin, NIST SP800-115) sınama dayalı
- Kritik sistemleri ve tüm CDE çevre kapsamını içerir
- Hem içinde hem de ve ağ dışından sınama içerir
- Herhangi bir segment ve kapsam azaltma denetimleri doğrulamak için sınama içerir
- , En azından, uygulama katmanı sızma testlerinin ekleneceğini gereksinim 6.5 içinde listelenen güvenlik açıkları tanımlar
- İşletim sistemleri yanı sıra ağ işlevlerini destekleyen bileşenler dahil etmek için ağ katmanı sızma testlerini tanımlar
- Gözden geçirme ve göz önünde bulundurarak Tehditler ve güvenlik açıkları son 12 ay içinde yaşadı içerir
- Test sonuçları ve düzeltme etkinlikleri sonuçları sızma bekletme belirtir

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure hizmetlerini üçüncü taraf sızma kullanarak doğrular (açık Web uygulaması güvenlik Proje) OWASP alarak, ilk on TEPE sertifikalı sınayıcılar kullanma. Test sonuçları, denetlenen ve güvenlik uygulamaları için uyumluluğu sağlamak için düzenli olarak gözden risk kayıt aracılığıyla izlenir. <br /><br />Microsoft ayrıca kırmızı gruplama Microsoft tarafından yönetilen altyapı, hizmetler ve uygulamalar karşı kullanır. Hiçbir end müşteri verilerini kasıtlı olarak kırmızı ekip oluşturma ve canlı site sızma sınama sırasında yöneliktir. Testleri Microsoft Azure altyapı ve platformlar yanı sıra Microsoft'un kendi uygulamaları ve verileri karşı ' dir. Müşteri kiracılar, uygulamaları ve verileri Azure üzerinde barındırılan hiçbir zaman hedefler.<br /><br />Microsoft Azure, bir Sistem Değerlendirme planı geliştirin ve denetimleri değerlendirmesi gerçekleştirmek için bağımsız bir assessor işe. Denetimleri değerlendirmeleri yıllık gerçekleştirilir ve sonuçları ilgili tarafları bildirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kalem test ve 'olduğundan' çaba güvenlik açığı taraması olmuştur. Kalem test sonuçlarını nmap veya pentest tools.com gibi ortak araçları kullanarak yinelenebilir. Kalem test sonuçlarını Etkilenme öğe ile yetersiz saldırı yüzeyini sağlar. Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) güvenlik açığı bilgileri ve düzeltme sağlar.|



### <a name="pci-dss-requirement-1131"></a>PCI DSS gereksinim 11.3.1

**11.3.1** gerçekleştirme *dış* en az yıllık ve tüm önemli altyapı veya uygulama yükseltme veya değişiklik sonra test sızma (alt ağ, işletim sistemini yükseltme gibi eklendi ortam veya ortama eklenen bir web sunucusu).

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 11,3 inç](#pci-dss-requirement-11-3). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kalem test ve 'olduğundan' çaba güvenlik açığı taraması olmuştur. Kalem test sonuçlarını nmap veya pentest tools.com gibi ortak araçları kullanarak yinelenebilir. Kalem test sonuçlarını Etkilenme öğe ile yetersiz saldırı yüzeyini sağlar. Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) güvenlik açığı bilgileri ve düzeltme sağlar.|



### <a name="pci-dss-requirement-1132"></a>PCI DSS gereksinim 11.3.2

**11.3.2** gerçekleştirme *iç* en az yıllık ve tüm önemli altyapı veya uygulama yükseltme veya değişiklik sonra test sızma (alt ağ, işletim sistemini yükseltme gibi eklendi ortam veya ortama eklenen bir web sunucusu).

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure sınırını sızma sınamasını gerçekleştirmek için Microsoft Azure Sözleşmelerle bağımsız assessors. Kırmızı takım alıştırmaları da düzenli olarak yapılan ve sonuçları güvenlik geliştirmeler yapmak için kullanılır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kalem test ve 'olduğundan' çaba güvenlik açığı taraması olmuştur. Kalem test sonuçlarını nmap veya pentest tools.com gibi ortak araçları kullanarak yinelenebilir. Kalem test sonuçlarını Etkilenme öğe ile yetersiz saldırı yüzeyini sağlar. Ayrıca, [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) güvenlik açığı bilgileri ve düzeltme sağlar.|



### <a name="pci-dss-requirement-1133"></a>PCI DSS gereksinim 11.3.3

**11.3.3** sızma test sırasında bulunan Etkilenme güvenlik açıkları düzeltildi ve test düzeltmeleri doğrulamak için yinelenir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Yordamlar, bilinen güvenlik açıkları için Microsoft Azure platform bileşenleri izlemek için oluşturulmuştur. <br /><br /><br /><br />Azure üretim ortamının öncelikli bileşenlerinin karşı tarama her hedeflenen Çeyrek kapsamlı güvenlik açığı güvenlik açıklarını tanımlamak için gerçekleştirilir. Sonuçları katılımcılara bildirilir ve düzeltme kapatma aracılığıyla ekibi tarafından izlenir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations), sağladığınız tüm bekleyen sorunları Contoso Webstore demo CDE düzeltilen emin olmak için güvenlik açığı bilgileri ve düzeltme, kullanılmış.|



### <a name="pci-dss-requirement-1134"></a>PCI DSS gereksinim 11.3.4

**11.3.4** kesimleme diğer ağlarla CDE yalıtmak için kullanılırsa, en az yıllık ve kesimleme denetimleri/kesimleme yöntemleri işletimsel ve etkili, olduğunu doğrulamak için yöntem değişiklikleri sonra sızma sınamalar gerçekleştirmek ve CDE sistemlerinde tüm kapsam sistemlerinden yalıtma.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Yordamlar, bilinen güvenlik açıkları için Microsoft Azure platform bileşenleri izlemek için oluşturulmuştur. <br /><br /><br /><br />Azure üretim ortamının öncelikli bileşenlerinin karşı tarama her hedeflenen Çeyrek kapsamlı güvenlik açığı güvenlik açıklarını tanımlamak için gerçekleştirilir. Sonuçları katılımcılara bildirilir ve düzeltme kapatma aracılığıyla ekibi tarafından izlenir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations), sağladığınız tüm bekleyen sorunları Contoso Webstore demo CDE düzeltilen emin olmak için güvenlik açığı bilgileri ve düzeltme, kullanılmış.|



### <a name="pci-dss-requirement-11341"></a>PCI DSS gereksinim 11.3.4.1

**11.3.4.1** *yalnızca hizmet sağlayıcıları için ek gereksinimi:* kesimleme kullanılırsa, Segment üzerinde test sızma gerçekleştirerek PCI DSS kapsam denetimleri her en az altı ay sonra herhangi bir değişiklik onaylayın Segment denetimleri/yöntemleri.

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 11.3.4](#pci-dss-requirement-11-3-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations), sağladığınız tüm bekleyen sorunları Contoso Webstore demo CDE düzeltilen emin olmak için güvenlik açığı bilgileri ve düzeltme, kullanılmış.|



## <a name="pci-dss-requirement-114"></a>PCI DSS gereksinim 11.4

**11.4** algılamak ve/veya ağ içine yetkisiz erişimi önlemek için izinsiz giriş algılama ve/veya izinsiz giriş önleme tekniklerini kullanın. Kart sahibi veri ortamı çevre kart sahibi veri ortamında kritik noktalarda yanı sıra tüm trafiğini izlemek ve şüpheli güvenlik ihlalleri personel uyarır.
Tüm izinsiz giriş algılama ve önleme motorları, taban çizgileri ve imzaları güncel tutun.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure işlem ortamında olayların gerçek zamanlı analiz yürütür ve sistem tehlikeye atabilecek olayları hakkında gerçek zamanlı uyarılar yakın Kimlikleri sistemleri oluşturur. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore bir PaaS hizmetidir ve ağ izinsiz giriş algılama ve önleme Azure'un sorumluluğu bakın. [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) yetkisiz erişim uyarı ve düzeltme sağlar.|



## <a name="pci-dss-requirement-115"></a>PCI DSS gereksinim 11.5

**11.5** değişikliği algılama mekanizması (örneğin, dosya bütünlüğünü izleme araçları) uyarı personele (değişiklikler, eklemeler ve silmeler dahil) izinsiz değiştirilmesini kritik sistem dosyaları, yapılandırma dosyalarını veya içeriği yeniden dağıtma dosyaları; Kritik dosya karşılaştırmaları en az haftalık gerçekleştirmek için yazılımı ve yapılandırma. 

> [!NOTE]
> Değişikliği algılama amacıyla, kritik dosyaları genellikle düzenli olarak değiştirme, ancak değiştirilmesini bir sistemi güvenliğinin aşılması veya güvenliğinin aşılmasına riskini gelebilir olanlardır. Dosya bütünlüğü izleme ürünler gibi değişikliği algılama mekanizmaları genellikle ilgili işletim sistemi için kritik dosyaları ile önceden yapılandırılmış olarak sunulur. Özel uygulamalar için olanlar gibi diğer kritik dosyaları değerlendirilir ve varlık (diğer bir deyişle, ticari ya da hizmet sağlayıcısı) tarafından tanımlanmış.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure tutar ve olası değişiklikleri ve güvenliğini etkileyebilecek olaylar müşterileri veya bir çevrimiçi hizmet Pano üzerinden hizmetlerin kullanılabilirliğini bildirir. Microsoft Azure müşterilerin güvenlik yükümlülüklerin ve güvenlik taahhütleri yapılan değişiklikler Microsoft Azure Web sitesinde zamanında güncelleştirilir.<br /><br />Yükleme veya üretim ortamı için sınırlı olduğu Microsoft Azure ile ilgili yazılım değişiklikleri yönetim personeli yetkili ve yönetimi yordamları aşağıdaki şekilde değiştirin. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Günlük analizi kullanarak değişikliği algılama uygulanmıştır ve contoso Webstore demo bir PaaS hizmetidir. Daha fazla bilgi için bkz: [PCI Kılavuzu - önceden yönetim çözümleri](payment-processing-blueprint.md#management-solutions).<br /><br />|



### <a name="pci-dss-requirement-1151"></a>PCI DSS gereksinim 11.5.1

**11.5.1** değişikliği algılama çözümü tarafından oluşturulan tüm uyarıları yanıtlamak üzere bir işlem uygulayın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure izleme olay kuralları artan bir yüksek riskli işlemleri ve varlıklar için izleme düzeyini belirtin. Azure yönetilen ağ aygıtlarının tesis edilmiş güvenlik standartlarıyla uyumluluğu izlenir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore uyarılar değişiklikleri için günlük analizi uygulaması tarafından sağlanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - önceden yönetim çözümleri](payment-processing-blueprint.md#management-solutions).<br /><br /><br /><br />|



## <a name="pci-dss-requirement-116"></a>PCI DSS gereksinim 11.6

**11.6** güvenlik ilkeleri ve güvenlik izleme ve test için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore uyarılar değişiklikleri için günlük analizi uygulaması tarafından sağlanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - önceden yönetim çözümleri](payment-processing-blueprint.md#management-solutions).<br /><br /><br /><br />|




