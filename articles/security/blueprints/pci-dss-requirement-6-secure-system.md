---
title: Azure ödeme işleme şeması - güvenli sistem gereksinimleri
description: PCI DSS gereksinim 6
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 79889fdb-52d2-42db-9117-6e2f33d501e0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 2f6f1bfc853f261eecf5357cef5d3e3d972781b1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33895439"
---
# <a name="secure-system-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için güvenli sistem gereksinimleri 
## <a name="pci-dss-requirement-6"></a>PCI DSS gereksinim 6

**Geliştirme ve güvenli sistemleri ve uygulamalar bakımının**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

İlkesiz kişiler güvenlik açıkları sistemleri ayrıcalıklı erişim kazanmak için kullanın. Çoğu güvenlik açıklarından sistemlerini yönetme varlıklar tarafından yüklenmelidir satıcı tarafından sağlanan güvenlik yamaları göre sabittir. Tüm sistemler kötü niyetli kişilere ve kötü amaçlı yazılım tarafından kart sahibi verilerin güvenliğinin aşılması ve kötüye kullanımlara karşı korumak için tüm uygun yazılım düzeltme eklerinin olması gerekir.

> [!NOTE]
> Uygun yazılım düzeltme eklerini değerlendirilir ve mevcut güvenlik yapılandırmaları ile çakışmaması düzeltme ekleri belirlemek için yeterince test bu düzeltme eklerini ' dir. Şirket içinde geliştirilen uygulamaları için standart sistem geliştirme işlemleri kullanarak hükümsüz kılınan ve güvenli teknikleri kodlama çağrılarındaki olabilir.

## <a name="pci-dss-requirement-61"></a>PCI DSS gereksinim 6.1

**6.1** güvenlik açığı bilgileri için kaynakları dışında tanınmış kullanarak güvenlik açıklarını tanımlamak için bir işlem oluşturun ve (örneğin, "Yüksek", "medium," veya "Düşük") için yeni bulunan güvenlik sıralaması riski atayın güvenlik açıkları.

> [!NOTE]
> Risk derecelendirmeleri olası etkisini değerlendirme yanı sıra endüstrideki en iyi uygulamaları üzerinde bağlı olmalıdır. Örneğin, güvenlik açıkları derecelendirme ölçütlerini göz önünde bulundurarak CVSS temel puanı ve/veya sınıflandırmaya göre satıcı ve/veya etkilenen sistemler türünü içerebilir. 
> 
> Güvenlik açıkları değerlendirmek ve risk derecelendirmeleri atama için kullanılan yöntemler kuruluşun ortamınıza ve risk değerlendirme stratejisi göre değişir. Risk derecelendirmeleri en azından bir ortama "yüksek riskli" olarak kabul tüm güvenlik açıklarını belirleme gerekir. "Bunlar etkisi kritik sistemler, ortamınıza uyaran bir tehdit teşkil ve/veya olası güvenliğinin aşılmasına neden olacaktır değilse ele risk derecelendirme yanı sıra güvenlik açıkları Kritik" olduğu düşünülebilir. Kritik sistemler örnekleri güvenlik sistemleri, genel kullanıma yönelik aygıtlar ve sistemler, veritabanları ve depolamak, işlem, diğer sistemler içerir veya kart sahibi veri aktaran olabilir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Yordamlar kurulan ve kapsam sınır hiper yönetici ana bilgisayarda güvenlik açıkları taramak için uygulanır. Güvenlik Açığı taraması, sunucu işletim sistemleri, veritabanları ve ağ aygıtlarını tarama araçlarının uygun güvenlik açığı ile gerçekleştirilir. Güvenlik Açığı taramaları en az üç aylık olarak gerçekleştirilir. Microsoft Azure sınırını sızma sınamasını gerçekleştirmek için Microsoft Azure Sözleşmelerle bağımsız assessors. Kırmızı takım alıştırmaları da düzenli olarak yapılan ve sonuçları güvenlik geliştirmeler yapmak için kullanılır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore WAF sahip bir uygulama ağ geçidi ve etkin OWASP ruleset kullanarak güvenlik açıkları riskini azaltır. Daha fazla bilgi için bkz: [PCI Kılavuzu - güvenlik açıkları riski azaltma](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-62"></a>PCI DSS gereksinim 6.2

**6.2** tüm sistem bileşenleri ve yazılım bilinen açıklarından uygun satıcı tarafından sağlanan güvenlik yamaları yükleyerek korunduğundan emin olun. Kritik güvenlik düzeltme eklerinin bir ay içinde sürümü yükleyin.

> [!NOTE]
> Kritik güvenlik yamaları gereksinim 6.1 içinde tanımlanan işlemi sıralaması risk göre tanımlanmalıdır.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Tüm ağ aygıtlarını sağlamak için Microsoft Azure sorumludur ve hiper yönetici işletim sistemi yazılım uygun satıcı tarafından sağlanan güvenlik yamaları yükleyerek bilinen açıklarından korunur. Bir müşteri hizmeti kullanmamayı istemedikçe, işletim sistemi düzeyinde güvenlik açıkları engelledi ve zamanında düzeltilen emin olmak için bir düzeltme eki yönetimi işleminin bulunmaktadır. Üretim sunucularında, düzeltme eki uyumluluk aylık olarak doğrulamak için taranır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore bir PaaS hizmet çözümüdür. Azure tüm hizmet düzeltme eklerini bakım sağlar.|



## <a name="pci-dss-requirement-63"></a>PCI DSS gereksinim 6.3

**6.3** (web tabanlı yönetim uygulamaları dahil olmak üzere) güvenli bir şekilde, aşağıdaki gibi dahili ve harici yazılım uygulamaları geliştirin:
- PCI DSS uygun olarak (örneğin, güvenli kimlik doğrulaması ve günlüğe kaydetme)
- Endüstri standartları ve/veya en iyi uygulamalar hakkında
- Bilgi güvenliği ve yazılım geliştirme yaşam döngüsü boyunca ekleme 

> [!NOTE]
> Bu, tüm dahili olarak geliştirilen yanı sıra bespoke yazılım veya bir üçüncü taraf tarafından geliştirilen özel yazılım için geçerlidir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure uygulamalarının ve uç noktaları uygun olarak DSS gereksinimleri olan Microsoft güvenlik geliştirme yaşam döngüsü (SDL) yönteme göre geliştirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore CHD korumak için endüstrideki en iyi uygulamaları izlemek için tasarlanmıştır. Dağıtım Kılavuzu güvenlik meassures ayrıntılarını sağlar ve günlük kaydı etkindir.|



### <a name="pci-dss-requirement-631"></a>PCI DSS gereksinim 6.3.1

**6.3.1** uygulamaları etkin hale veya müşterilere serbest önce geliştirme, test ve/veya özel uygulama hesapları, kullanıcı kimliklerini ve parolaları kaldırın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Son güvenlik gözden geçirme (FSR) varsayılan olarak, bir atanmış güvenlik yalnızca üretime hazır uygulamalar yayımlanan emin olmak için Advisor tarafından Azure geliştirme ekibi dışında üretim dağıtımından önce önemli sürümler için gerçekleştirilir. Bu son gözden bir parçası olarak, tüm hesapları test ve test verileri kaldırılmış güvence altına. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, yalıtılmış ve günlüğe kaydedilen hazırlama bir hizmet sunar. Her ağ katmanı ayrılmış bir ağ güvenlik grubu [NSG] vardır. Daha fazla bilgi için bkz: [PCI kılavuzu - ağ güvenlik grupları](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-632"></a>PCI DSS gereksinim 6.3.2

**6.3.2** üretim veya müşteriler için en azından aşağıdakileri içermesi için Güvenlik Açığı (el ile veya otomatik işlemleri kullanarak) kodlama olası belirlemek amacıyla serbest bırakmak için özel kod öncesinde gözden geçirin:
- Kod değişiklikleri kişiler güvenli uygulamalar kodlama ve kod gözden geçirme teknikleri hakkında bilgili ve kaynak kod yazar dışındaki kişiler tarafından incelenen.
- Kod gözden geçirme kod göre güvenli kodlama yönergeleri geliştirilen emin olun.
- Gerekli düzeltmeleri serbest bırakmak için uygulanan önceki olan
- Kod gözden geçirme sonuçları gözden geçirdikten ve yönetim yayımlamayı önceki Onaylandı 

> [!NOTE]
> Bu gereksinim kod gözden geçirme için tüm özel kod (iç ve genel kullanıma yönelik), sistem geliştirme yaşam döngüsü bir parçası olarak uygulanır. 
>
> Kod gözden geçirme bilgili iç personel veya üçüncü taraflar tarafından gerçekleştirilmesi. Genel kullanıma yönelik web uygulamaları da adresi devam eden tehditleri ve uygulama sonra güvenlik açıkları için ek denetimler PCI DSS gereksinim 6.6 tanımlanan tabidir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure uygulamalarının ve uç noktaları uygun olarak Microsoft Security Development Lifecycle (SDL) Metodoloji geliştirilir. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, yalıtılmış ve günlüğe kaydedilen hazırlama bir hizmet sunar. Her ağ katmanı ayrılmış bir ağ güvenlik grubu [NSG] vardır. Daha fazla bilgi için bkz: [PCI kılavuzu - ağ güvenlik grupları](payment-processing-blueprint.md#network-security-groups).|



## <a name="pci-dss-requirement-64"></a>PCI DSS gereksinim 6.4

**6.4** izleyin, sistem bileşenlerine denetim işlemleri ve tüm değişiklikleri için yordamlar değiştirin. İşlemler (bkz. gereksinimleri için 6.4.1 6.4.6) aşağıdaki eklemeniz gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft bu bilgileri güvenlik yazılım geliştirme ilgili güvenlik konuları Sistem başlangıcından SDLC tümleştirilmesini gerekir NIST kılavuzu izler. Microsoft SDL güvenlik uygulamalarının sürekli tümleştirme sağlar:<ul><li>Erken tanımlama ve güvenlik açıkları ve yapılandırma hataları azaltma</li><li>Gerekli güvenlik denetimleri tarafından neden zorluklar kodlama olası yazılım tanıma</li><li>Paylaşılan güvenlik hizmetleri ve güvenlik tutumunu kanıtlanmış yöntem ve teknikler aracılığıyla artıran güvenlik en iyi yöntemler araçları kullanılmasını tanımlaması</li><li>Microsoft'un zaten kapsamlı risk yönetimi programının zorlama</li></ul>Microsoft Azure değişiklik oluşturulmuş ve yayın yönetimi işlemleri dahil olmak üzere önemli değişiklikler uyarlamasını denetlemek için:<ul><li>Tanımlama ve planlı değişikliği belgeleri</li><li>Ürün planlama sırasında tanımlaması iş hedeflerini, öncelikleri ve senaryoları</li><li>Özellik/bileşen tasarım belirtimi</li><li>Önceden tanımlanmış ölçüt/onay-genel risk/etkisi değerlendirmek için listesini esas alan işletimsel hazırlık gözden geçirme</li><li>Test, yetkilendirme ve değişiklik Yönetimi giriş/çıkış ölçütlere uygun şekilde ortamları geliştirme (Geliştirme), INT (tümleştirme test), aşama (üretim öncesi) ve üretim (üretim) göre. Müşteriler, Microsoft Azure üzerinde barındırılan kendi uygulamalarında sorumludur.</li></ul> |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore gösteri, yalıtılmış ve günlüğe kaydedilen hazırlama bir hizmet sunar. <br /><br />Her ağ katmanı ayrılmış bir ağ güvenlik grubu [NSG] vardır. Daha fazla bilgi için bkz: [PCI kılavuzu - ağ güvenlik grupları](payment-processing-blueprint.md#network-security-groups).<br /><br />Operations Management Suite kullanarak değişiklikleri günlüğe kaydedilir ve Runbook günlükleri toplamak için kullanılır. Günlük analizi değişiklikleri ayrıntılı günlük kaydını sağlar. Değişiklikleri gözden ve doğruluk doğrulandı. Daha ayrıntılı yönergeler için bkz [PCI Kılavuzu - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-641"></a>PCI DSS gereksinim 6.4.1

**6.4.1** ayrı üretim ortamlarından geliştirme ve test ortamları ve erişim denetimleri ile ayırma uygulayın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, yalıtılmış ve günlüğe kaydedilen hazırlama bir hizmet sunar. Her ağ katmanı ayrılmış bir ağ güvenlik grubu [NSG] vardır. Daha fazla bilgi için bkz: [PCI kılavuzu - ağ güvenlik grupları](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-642"></a>PCI DSS gereksinim 6.4.2

**6.4.2** , görevlerin ayrılmasını geliştirme ve test ve üretim ortamlarını arasında

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, yalıtılmış ve günlüğe kaydedilen hazırlama bir hizmet sunar. Her ağ katmanı ayrılmış bir ağ güvenlik grubu [NSG] vardır. Daha fazla bilgi için bkz: [PCI kılavuzu - ağ güvenlik grupları](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-643"></a>PCI DSS gereksinim 6.4.3

**6.4.3** üretim verileri (dinamik planlar) test veya geliştirme için kullanılmaz.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore birincil hesap numarası (PAN) verileri Canlı Hayır sahiptir.|



### <a name="pci-dss-requirement-644"></a>PCI DSS gereksinim 6.4.4

**6.4.4** test verileri ve sistem etkin hale gelir veya üretime geçmeden önce sistem bileşenleri hesaplardan kaldırılması.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore bir test hesabınız yok.|



### <a name="pci-dss-requirement-645"></a>PCI DSS gereksinim 6.4.5

**6.4.5** değişiklik denetim yordamları güvenlik düzeltme eklerinin ve yazılım değişiklikleri uygulanması için aşağıdakileri içerir:
- **6.5.4.1** etkisi belgeleri.
- **6.5.4.2** Documented onay yetkili tarafların değiştirin.
- **6.5.4.3** değişikliği sistem güvenliğini olumsuz etkilemez doğrulamak için sınama işlevselliği.
- **6.5.4.4** geri çekme yordamlar.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore bir PaaS hizmet çözümüdür. Ve tüm hizmet düzeltme eklerini bakım Azure sağlar.|



### <a name="pci-dss-requirement-646"></a>PCI DSS gereksinim 6.4.6

**6.4.6** önemli bir değişiklik tamamlandıktan sonra tüm yeni veya değiştirilmiş sistemleri ve ağlar ve uygun şekilde güncelleştirilmiş belgeleri tüm ilgili PCI DSS gereksinimleri uygulanması gerekir.

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore bir PaaS hizmet çözümüdür. Ve tüm hizmet düzeltme eklerini bakım Azure sağlar.|



## <a name="pci-dss-requirement-65"></a>PCI DSS gereksinim 6.5

**6.5** yazılım geliştirme süreçleri güvenlik açıkları gibi kodlama ortak adres:
- Ortak kodlama güvenlik açıkları önlemek de dahil olmak üzere en az yıllık güncel güvenli kodlama teknikleri, geliştiricilerin eğitmek.
- Güvenli kodlama yönergeleri tabanlı uygulamalar geliştirin.

> [!NOTE]
> PCI DSS bu sürümü yayımlandığında 6.5.10 aracılığıyla 6.5.1 adresinde listelenmiş güvenlik açıkları endüstri en iyi yöntemlerle geçerli. Ancak, güvenlik açığı yönetimi için endüstrideki en iyi uygulamaları (örneğin, CWE üst 25, güvenli sertifika kodlama, vb. SANS OWASP Kılavuzu) güncelleştirildikçe geçerli en iyi uygulamalar için bu gereksinimleri kullanılması gerekir. 
> 
> [!NOTE]
> Gereksinimleri 6.5.1 6.5.6 Aşağıda, tüm uygulamalar için (iç veya dış) geçerlidir. Gereksinimleri 6.5.7 6.5.10 Aşağıda, web uygulamaları ve uygulama arabirimleri (iç veya dış) için geçerlidir. 

- **6.5.1** ekleme açıkları, özellikle SQL ekleme. Ayrıca, işletim sistemi komut ekleme, LDAP ve XPath ekleme açıkları yanı sıra diğer ekleme açıkları düşünün.
- **6.5.2** arabellek taşmaları
- **6.5.3** güvenli şifreleme depolama
- **6.5.4** güvenli iletişimler
- **6.5.5** hatalı hata işleme
- **6.5.6** (PCI DSS gereksinim 6.1 tanımlandığı gibi) güvenlik açığı tanımlama işleminde tanımlanan tüm "yüksek riskli" güvenlik açıkları
- **6.5.7** siteler arası komut dosyası (XSS)
- **6.5.8** hatalı erişim denetimi (örneğin, güvenli olmayan doğrudan nesne başvuruları, URL erişim, dizin geçişi ve işlevleri için kullanıcı erişimini kısıtlamak için hata kısıtlamak için hatası)
- **6.5.9** siteler arası istek sahtekarlığı (CSRF)
- **6.5.10** kimlik doğrulama ve oturum yönetimi bozuk

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | İçin "Microsoft Azure" bölümüne bakın. [gereksinim 6.4](#pci-dss-requirement-6-4). |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore demo güvenli geliştirme yöntemleri göstermek için güvenli geliştirme, DFD ve tehdit modeli için kılavuzluk sağlar.|



## <a name="pci-dss-requirement-66"></a>PCI DSS gereksinim 6.6

**6.6** genel kullanıma yönelik web uygulamaları için yeni tehditler ve güvenlik açıkları düzenli olarak adres ve bu uygulamaları bilinen saldırıları aşağıdaki yöntemlerden birini tarafından korunmasını emin olun:

- El ile veya otomatik uygulama güvenlik açığı güvenlik değerlendirme araçları veya yöntemleri, üzerinden genel kullanıma yönelik web uygulamaları en az yıllık sonra herhangi bir değişiklik gözden geçirme 

   > [!NOTE]
   > Bu değerlendirme için gereksinim 11.2 gerçekleştirilen güvenlik açığı taramaları aynı değil. 

- Algılar ve web tabanlı saldırılara (örneğin, bir web uygulaması güvenlik duvarı) engeller otomatik bir teknik çözüm sürekli tüm trafiği denetlemek için genel kullanıma yönelik web uygulamaları önünde yükleniyor.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Uygulamaları üretim ortamına dağıtılmadan önce Microsoft Azure genel kullanıma yönelik web uygulamaları kendi SDL işleminin bir parçası sınar. Ayrıca, Microsoft tüm olası güvenlik açıklarını algılamak için tüm genel kullanıma yönelik web uygulamalarında üretim düzenli olarak tarar. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Başvuru çözüm WAF sahip bir uygulama ağ geçidi ve etkin OWASP ruleset kullanarak güvenlik açıkları riskini azaltır. Daha fazla bilgi için bkz: [PCI Kılavuzu - güvenlik açıkları riski azaltma](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-67"></a>PCI DSS gereksinim 6.7

**6.7** güvenlik ilkeleri ve işletimsel yordamlarına geliştirmenin ve korumanın güvenli sistemler ve uygulamalar, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Başvuru çözüm WAF sahip bir uygulama ağ geçidi ve etkin OWASP ruleset kullanarak güvenlik açıkları riskini azaltır. Daha fazla bilgi için bkz: [PCI Kılavuzu - güvenlik açıkları riski azaltma](payment-processing-blueprint.md#application-gateway).|




