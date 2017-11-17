---
title: "Azure ödeme işleme şeması - fiziksel erişimi gereksinimleri"
description: PCI DSS gereksinim 9
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 91595a69-e9ce-4f9c-8388-10224165d9c0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 89f7b20a130e988bfe4964d50ae97de788ca4623
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="physical-access-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için fiziksel erişimi gereksinimleri 
## <a name="pci-dss-requirement-9"></a>PCI DSS gereksinim 9

**Kart sahibi veri fiziksel erişimi kısıtlama**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Tüm fiziksel veri aygıtları veya veri erişimi ve sistemleri veya hardcopies kaldırmak için Fırsat kişiler için sağlar ve uygun şekilde kısıtlanması gerektiğini, ev kart sahibi veri veya sistemleri ile erişim. Gereksinim 9 amaçları doğrultusunda, tam zamanlı ve zamanlı çalışanlar, geçici çalışanlar, Yükleniciler ve varlığın şirket içinde fiziksel olarak danışmanlar "yerinde personel" ifade eder. Bir satıcı, herhangi bir yerinde personeli, hizmet çalışanları veya herkes, genellikle fazla bir gün gibi kısa bir süre için tesis girmek için konuk "Ziyaretçi" başvurur. "Ortam" tüm kağıt ve kart sahibi verileri içeren elektronik medya başvuruyor.

## <a name="pci-dss-requirement-91"></a>PCI DSS gereksinim 9.1

**9.1** kart sahibi veri ortamında sistemler için fiziksel erişimi sınırı ve İzleyici uygun tesis giriş denetimlerini kullanın.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure, uygulama, zorlama ve fiziksel erişim güvenlik veri merkezleri için izleme sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-911"></a>PCI DSS gereksinim 9.1.1

**9.1.1** video kamera veya erişim denetimi mekanizmaları (veya her ikisi de) hassas alanlara tek tek fiziksel erişimi izlemek için kullanın. Toplanan verileri gözden geçirin ve diğer girişleri ile ilişkilendirilmesi. En az üç ay için Aksi takdirde yasalar tarafından kısıtlanmış sürece depolar.

> [!NOTE]
> "Hassas alanlara" herhangi bir veri merkezi, sunucu odasına veya depolamak, işlem ya da kart sahibi veri aktaran sistemler barındıran herhangi bir alan anlamındadır. Bu, burada yalnızca satış noktası Terminal mevcut bir perakende deposundaki kasiyer alanları gibi genel kullanıma yönelik alanları dışlar.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure, uygulama, zorlama ve izleme CCTV ve veri merkezlerinin biyometrik erişim denetimi mekanizmaları sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-912"></a>PCI DSS gereksinim 9.1.2

**9.1.2** uygulamak için genel olarak erişilebilir ağ girişlerine erişimi kısıtlamak için fiziksel ve/veya mantıksal kontrol eder. 

Örneğin, ağ bulunan ortak alanları girişlerine ve alanları ziyaretçileri erişilebilir devre dışı ve ağ erişimi açıkça yetkilendirildiğinde yalnızca etkinleştirilmiş olmalıdır. Alternatif olarak, işlemleri ziyaretçileri adresindeki her zaman alanlarda etkin ağ girişlerine escorted emin olmak için uygulanabilir.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure platformu içinde hiçbir genel olarak erişilebilir ağ girişlerine vardır. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-913"></a>PCI DSS gereksinim 9.1.3

**9.1.3** kablosuz erişim noktaları, ağ geçitleri, el aygıtları, ağ/iletişimleri donanım ve telekomünikasyon satırları için fiziksel erişimi sınırlayın.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ağ donanımına sıkı bir şekilde erişim listeleri tarafından denetlenen fiziksel erişim, birden çok form kimlik doğrulaması, giriş ve iş gereksinimini fiziksel engelleri donanımına erişim için onaylanması gerekir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



## <a name="pci-dss-requirement-92"></a>PCI DSS gereksinim 9.2

**9.2** dahil etmek için yerinde personel ve ziyaretçileri arasında ayrımlar yordamlara geliştirin:
- Yerinde personel ve ziyaretçileri (örneğin, atama rozetleri) tanımlama
- Erişim gereksinimleri için değişiklikler
- İptal etme veya yerinde personel ve süresi dolan ziyaretçi kimliği (örneğin, kimliği rozetleri) sonlandırılıyor.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure, uygulama, zorlama ve veri merkezleri ziyaret eden fiziksel erişimi güvenliği ve çalışan veya yüklenici kimliği izleme sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



## <a name="pci-dss-requirement-93"></a>PCI DSS gereksinim 9.3

**9,3** fiziksel erişimi için hassas alanlara yerinde personel aşağıdaki şekilde denetleyebilirsiniz:
- Erişim yetkisi ve tek tek iş işlevine bağlı.
- Erişim sonlandırıldığında hemen iptal edilir ve anahtarları, erişim kartları, vb. gibi tüm fiziksel erişimi mekanizmaları döndürülen ya da devre dışı.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Erişim kimlik doğrulamalarını Microsoft veri merkezleri için en az ayrıcalık prensibi tabanlı veri merkezi ekibi tarafından onaylanan bir yetkili erişimi listesi kullanılarak denetlenir. Erişim denetim listesini gözden, doğrulandı ve üç aylık güncelleştirildi.<br /><br />Çevre ağ geçitleri, elektronik erişim rozet okuyucular, biyometrik okuyucular, ADAM-tuzakları/portalları ve koruma geçişi aygıtları geri gibi Microsoft Azure veri merkezlerinde fiziksel erişim aygıtları kullanın. Erişim rozet aygıtları sürekli olarak izlenir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



## <a name="pci-dss-requirement-94"></a>PCI DSS gereksinim 9,4

**9,4** tanımlamak ve ziyaretçileri yetkilendirmek için yordamları uygulayın. Yordamlar şunları içermelidir.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Zorunlu önceden onaylanmış teslimler bilgi işleme tesis fiziksel olarak yalıtılmış ve yetkili personel tarafından izlenen bir güvenli yükleme bay alındığı için Microsoft Azure sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-941"></a>PCI DSS gereksinim 9.4.1

**9.4.1** ziyaretçileri girmeden önce yetkili ve her zaman, nerede kart sahibi veri işlenen veya tutulan alanları, escorted.


**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Zorunlu önceden onaylanmış teslimler bilgi işleme tesis fiziksel olarak yalıtılmış ve yetkili personel tarafından izlenen bir güvenli yükleme bay alındığı için Microsoft Azure sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-942"></a>PCI DSS gereksinim 9.4.2

**9.4.2** ziyaretçileri tanımlanır ve verilen bir gösterge veya diğer kimliği, zaman aşımına uğrar ve, yerinde personel ziyaretçilerden görünür şekilde ayırır.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Veri Merkezi erişim önceden onaylanmış ve yetkili ziyaretçileri varış noktasında fiziksel güvenlik iade ve geçerli bir kimlik kanıtı girişi önce sağlamak için gereklidir. Rozetleri açıkça çalışanları belirtin. Yükleniciler ve ziyaretçileri ayrılma tesis öğesinden sonra surrendered gerekir geçici rozetleri alırsınız. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-943"></a>PCI DSS gereksinim 9.4.3

**9.4.3** ziyaretçileri rozet veya kimlik tesis çıkmadan önce veya sona erme tarihine surrender istenir.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Ziyaretçilerin rozetleri ayrılma herhangi bir Microsoft tesisine öğesinden sonra surrender için gereklidir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-944"></a>PCI DSS gereksinim 9.4.4

**9.4.4** ziyaretçi günlük ziyaretçi etkinliği tesis yanı sıra bilgisayar odaları ve kart sahibi verileri nerede depolanır veya aktarılan veri merkezlerine fiziksel denetim izi korumak için kullanılır.
Ziyaretçi adı, gösterilen kesin ve oturum fiziksel erişimi yetkilendirme yerinde personel belge.
En az üç ay için bu günlük, aksi takdirde yasalar tarafından kısıtlanmış sürece korur.

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ziyaretçi günlük ziyaretçi etkinliği tesis yanı sıra bilgisayar odaları ve kart sahibi veri nerede depolanır veya aktarılan veri merkezlerine fiziksel denetim izi olarak sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



## <a name="pci-dss-requirement-95"></a>PCI DSS gereksinim 9.5

**9.5** tüm medya fiziksel olarak güvenli.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-951"></a>PCI DSS gereksinim 9.5.1

**9.5.1** güvenli bir konuma, tercihen bir site dışındaki özelliği, bir alternatif veya yedekleme sitesi gibi veya ticari depolama tesisi depolamak medya yedekler. Konumun güvenliği en az yıllık gözden geçirin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-96"></a>PCI DSS gereksinim 9.6

**9.6** medya, aşağıdakiler de dahil olmak üzere herhangi bir türde iç veya dış dağıtım üzerinde katı denetimi korumak.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-961"></a>PCI DSS gereksinim 9.6.1'e

**9.6.1'e** verilerin duyarlılığına belirlenebilir şekilde ortamı Sınıflandır.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-962"></a>PCI DSS gereksinim 9.6.2

**9.6.2** medyayı güvenli courier veya doğru izlenebilir başka bir teslim yöntemi tarafından gönderin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-963"></a>PCI DSS gereksinim 9.6.3'e

**9.6.3'e** olun Yönetimi (medya kişilere dağıtıldığında dahil olmak üzere) güvenli alanından taşınmış tüm medya onaylar.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-97"></a>PCI DSS gereksinim 9.7

**9.7** depolama ve erişilebilirlik medya üzerinde katı denetimi korumak.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-971"></a>PCI DSS gereksinim 9.7.1

**9.7.1** düzgün stok günlüklerin tüm ortamının bakımını yapmak ve medya envanterleri en az yıllık yürütün.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-98"></a>PCI DSS gereksinim 9.8

**9.8** artık iş veya yasal nedenlerle için aşağıdaki gibi gerektiğinde medya yok.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-981"></a>PCI DSS gereksinim 9.8.1

**9.8.1** parçalara ayır, incinerate veya böylece kart sahibi veri yeniden kopyalama sabit malzemeleri pulp. Güvenli Depolama kapsayıcıları yok edilmesi gereken malzemeler için kullanılır.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm verileri Azure SQL veritabanında depolar. PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-982"></a>PCI DSS gereksinim 9.8.2

**9.8.2** işlemek elektronik medya üzerindeki kart sahibi veriler kurtarılamaz kart sahibi veri yeniden böylece.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Veri yok etme teknikleri yoksa abonelikler, depolama, sanal makineler veya veritabanları mı yok veri nesnesi türüne bağlı olarak farklılık gösterir. Microsoft Azure çok kiracılı ortamı dikkatli dikkat emin olmak için gerçekleştirilecek bir müşterinin veri ya da "sızıntısı" başka bir müşteri'nin veri içine veya müşteri verileri, (, çoğu durumda, müşteri de dahil olmak üzere hiçbir müşterinin sildiğinde izin verilmiyor kimin bir kez veri ait) silinen bu verilere erişebilir.<br /><br />Microsoft Azure, bu verileri sağlamaya asıl sorunu adres medya temizleme 800 88 yönergeleri kasıtsız olarak kullanıma NIST izler. Bu yönergeleri elektronik ve fiziksel temizleme kapsar. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tamamen dağıtımı sırasında kullanılan kaynak grubunu silerek silinebilir.|



## <a name="pci-dss-requirement-99"></a>PCI DSS gereksinim 9.9

**9.9** ödeme kartı verileri izinsiz ve değiştirme kartından doğrudan fiziksel etkileşim aracılığıyla yakalama cihazları koruma.

> [!NOTE]
> Bu gereksinimleri satış noktasında kartı mevcut işlemleri (diğer bir deyişle, kart geçirme veya DIP) kullanılan kartı okuma aygıtları için geçerlidir. Bu gereksinim, bilgisayar klavyeler ve POS keypads gibi el ile anahtarı girişi bileşenlerine uygulamak üzere tasarlanmamıştır. 

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm sistem değişiklikleri oturum OMS kullanır.<br /><br />[Operations Management Suite (OMS)](/azure/operations-management-suite/) değişiklikleri ayrıntılı günlük kaydını sağlar. Değişiklikleri gözden ve doğruluk doğrulandı. Daha ayrıntılı yönergeler için bkz [PCI Kılavuzu - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-991"></a>PCI DSS gereksinim 9.9.1

**9.9.1** cihazları güncel bir listesini korumak. Liste şunları içermelidir:
- Yapın, cihaz modeli
- Cihazın (örneğin, site veya aygıt bulunduğu tesis adresi) konumu
- Cihaz seri numarası veya başka bir yöntem benzersiz kimlik

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore başvuru mimarisi ve dağıtım belgelerinde kullanılan tüm hizmetlerin bir listesini sağlar.|



### <a name="pci-dss-requirement-992"></a>PCI DSS gereksinim 9.9.2

**9.9.2** düzenli aralıklarla (örneğin, cihazlara kartı skimmers toplama) oynama ya da değiştirme algılamak için cihaz yüzeyleri inceleyebilir (örneğin, seri numarası veya başka bir aygıt denetleyerek doğrulamak için özelliklere olmayan takas ile bir sahte aygıt).

> [!NOTE]
> Bir aygıt değiştirilmiş veya olabilir yerine, işaretlerini beklenmeyen ekler veya aygıt, eksik veya değiştirilen güvenlik etiketleri, bozuk veya farklı renkli büyük/küçük harf veya seri numarası veya başka değişiklikler takılı kabloları örnekler Dış işaretler.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-993"></a>PCI DSS gereksinim 9.9.3

**9.9.3** denenen değiştirilmesine veya aygıtları değiştirilmesini unutmayın personeli için eğitim sağlayın. Eğitim şunları içermelidir:
- Bunları değiştirmek veya aygıtlarla ilgili sorunları giderme için erişim vermeden önce onarım veya bakım personel olmasını istenmesini tüm üçüncü taraf kişi kimliğini doğrulayın.
- Değil yüklemek, değiştirin veya doğrulaması olmadan cihazları döndürür.
- Cihazları (örneğin, çıkarın veya aygıtları'nı açmak için bilinmeyen kişiler tarafından çalışır) şüpheli davranışa farkında olun.
- Rapor şüpheli davranış ve cihaz değiştirilmesine veya değiştirme (örneğin, için bir yönetici veya güvenlik yetkilisi) uygun personele belirtileri.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|



## <a name="pci-dss-requirement-910"></a>PCI DSS gereksinim 9.10

**9.10** güvenlik ilkeleri ve işletimsel yordamlarına fiziksel kart sahibi verilere erişimi kısıtlayarak, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil.|




