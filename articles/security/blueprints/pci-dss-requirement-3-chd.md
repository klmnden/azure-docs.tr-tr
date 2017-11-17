---
title: "Azure ödeme işleme şeması - CHD gereksinimleri"
description: PCI DSS gereksinim 3
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 9736f7c8-c632-4b86-8b8a-6e4f45c1a7aa
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 356599cbe1e4e1948a5ec16d0d504835fa7dcd43
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="chd-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için CHD gereksinimleri
## <a name="pci-dss-requirement-3"></a>PCI DSS gereksinim 3

**Saklı kart sahibi verileri koruma**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Şifreleme, kesme, maskeleme, gibi koruma yöntemleri ve karma, kart sahibi veri koruması kritik bileşenleridir. Bir saldırgan tarafından diğer güvenlik denetimlerini bozar ve uygun olan şifreleme anahtarları olmadan şifrelenmiş verilere erişim kazanır veriler okunamaz ve bu kişiye kullanılamaz olur. Depolanan verileri korumanın etkin başka yöntemler de olası risk azaltma fırsatları olarak düşünülmelidir. Örneğin, riski en aza indirmek için yöntemleri kesinlikle gerekli, kesilmesiyle kart sahibi veri tam PAN varsa gerekmediği sürece ve e-posta ve anlık gibi son kullanıcı Mesajlaşma teknolojilerini kullanarak değil gönderen korumasız planlar değil depolanmasını kart sahibi veri içerir ileti.
Lütfen PCI DSS ve PA-DSS terimler sözlüğü, kısaltmalar ve kısaltmalar için "güçlü şifreleme" diğer PCI DSS terimleri ve tanımları bakın.

## <a name="pci-dss-requirement-31"></a>PCI DSS gereksinim 3.1

**3.1** Koru kart sahibi veri depolama alanına veri saklama ve elden ilkeleri, yordamları ve tüm kart sahibi (CHD) veri depolama için en az şunlar işlemleri uygulayarak en az:
- Yasal, yasal ve iş gereksinimleri için gerekli olan veri depolama miktarını ve bekletme süresini, sınırlama
- Kart sahibi veri belirli bekletme gereksinimleri
- Artık gerektiğinde verilerin güvenli silme işlemleri
- Tanımlama ve güvenli bir şekilde silmek için bir üç aylık işlem tanımlanan bekletme aşan kart sahibi veriler.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure, silinmek üzere belirtilen müşteri verilerini 800 88 uyumlu protokoller, güvenli elden ilkelerinde belirtilen NIST kullanarak güvenli bir şekilde kullanımdan alınmış sağlamak için sorumludur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore silmeyin veya depolanan tüm CHD yok. Tüm veriler ancak şifreli ve Hayır birincil hesap numarası (PAN) verileri depolanır.<br /><br />|



## <a name="pci-dss-requirement-32"></a>PCI DSS gereksinim 3.2

**3.2** hassas kimlik doğrulama verileri yetkilendirme sonrasında (şifrelenmiş olsa bile) depolamayın. Hassas kimlik doğrulama verileri alınırsa, tüm verileri Yetkilendirme işlemi tamamlandıktan sonra kurtarılamaz hale. 
- Bir iş gerekçesinin yoktur ve 
- Veriler güvenli bir şekilde depolanır.
Hassas kimlik doğrulama verileri aşağıdaki gereksinimleri 3.2.1 3.2.3 aracılığıyla bildirdi veri içerir:


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore silmeyin veya depolanan tüm CHD yok; Örnek verileri yalnızca tanıtım amacıyla depolanır. Ancak, tüm verileri numarası (PAN) veriler şifrelenmiş ve Hayır birincil hesabı gerekir.<br /><br />|



### <a name="pci-dss-requirement-321"></a>PCI DSS gereksinim 3.2.1

**3.2.1** (Başlangıç yonga üzerinde veya başka bir yerde bulunan bir karta, eşdeğer verileri arkasında bulunan manyetik bant) herhangi bir parçayı tam içeriğini yetkilendirme sonrasında depolamayın. Bu verileri tam izleme alternatif olarak adlandırılır, izlemek, 1 izlemek, 2 ve manyetik bant veri izleyen. 

> [!NOTE]
> İş normal seyri içinde aşağıdaki veri öğeleri manyetik bant korunabilmesi gerekebilir: 
> - Kart sahibinin'ın adı 
> - Birincil hesap numarası (PAN) 
> - Sona erme tarihi 
> - Servis kodu 
>
> Riski en aza indirmek için iş için gerektiği şekilde yalnızca bu veri öğeleri depolayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore herhangi CHD tam içeriğini depolamaz.|



### <a name="pci-dss-requirement-322"></a>PCI DSS gereksinim 3.2.2

**3.2.2** kart doğrulama kodu veya değeri (ön veya kart değil mevcut işlemleri doğrulamak için kullanılan bir ödeme kartı arkasında basılıdır üç basamaklı veya dört basamaklı bir sayı) yetkilendirme sonrasında depolamayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore kart doğrulama örnekleri dahil tüm verileri şifreler.|



### <a name="pci-dss-requirement-323"></a>PCI DSS gereksinim 3.2.3

**3.2.3** kişisel kimlik numarası (PIN) veya şifrelenmiş PIN blok yetkilendirme sonrasında depolamayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore PIN bilgilerini depolamaz.|



## <a name="pci-dss-requirement-33"></a>PCI DSS gereksinim 3.3

**3.3** maskesi görüntülendiğinde PAN (ilk altı ve son dört hanesi görüntülenecek basamak sayısı üst sınırı olan), yasal bir işletme ile yalnızca personel tam PAN'ın görebilirsiniz şekilde 

> [!NOTE]
> Bu gereksinim daha sıkı gereksinimlerine kart sahibi veri görüntüler için yerinde yerini almaz — örneğin, yasal veya ödeme kartı marka gereksinimleri satış noktası (POS) girişler için.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore saydam veri şifreleme, her zaman şifreli sütunları ve dinamik veri maskeleme kullanarak birincil hesap numarası (PAN) maskeleri. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-34"></a>PCI DSS gereksinim 3.4

**3.4** PAN okunamaz depolanır (taşınabilir dijital medyayı, yedekleme ortamı ve günlükleri dahil) her yerden aşağıdaki yaklaşımlardan birini kullanarak oluştur:
- Güçlü şifrelemeye dayalı tek yönlü karma (karma tüm PAN olmalıdır)
- Kesme (karma PAN kesilmiş kesimi değiştirmek için kullanılamaz)
- Dizin belirteçleri ve klavye takımı (klavye takımı güvenli şekilde depolanması gerekir)
- Güçlü şifreleme ile ilişkili anahtar yönetimi işlemler ve yordamlar. 

> [!NOTE]
> Kötü niyetli bir kullanıcının bir PAN'ın kesilmiş ve karma sürümü erişiminiz varsa özgün PAN verileri yeniden oluşturmak için görece Önemsiz çaba olduğu Burada karma ve kesilmiş bir varlığın ortamında aynı PAN sürümleri mevcutsa, ek denetimler karma ve kesilmiş sürümleri özgün PAN'ın yeniden oluşturmak için ilişkilendirilemez emin olmak için karşılanması gereken

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm kredi kartı verileri şifreler ve CHD alınmasını engelleyen anahtarlarını yönetmek için Azure anahtar kasası kullanır.<br /><br />Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-341"></a>PCI DSS gereksinim 3.4.1

**3.4.1** disk şifreleme (dosya ya da sütun düzeyi veritabanı şifreleme yerine) kullanılıyorsa, mantıksal erişimi ayrı ayrı ve yerel işletim sistemi kimlik doğrulaması ve erişim denetimi mekanizmaları bağımsız olarak yönetilmelidir (örneğin, değil Yerel kullanıcı hesabı veritabanları veya genel ağ oturum açma kimlik bilgilerini kullanarak). Şifre çözme anahtarlarını kullanıcı hesapları ile ilişkilendirilmemiş olması gerekir. 

> [!NOTE]
> Bu gereksinim, diğer tüm PCI DSS şifreleme ve anahtar yönetimi gereksinimlerini ek olarak uygulanır.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore depolanan tüm verileri şifreler ve DevOps işlevler için ayrıcalıklı yükselmesini engellemek için trafiği ayırır.<br /><br />Uygulama hizmeti ortamı güvenli ve kilitli olarak vardır herhangi bir DevOps sürümleri veya Kudu kullanarak bir Web uygulaması izleme yeteneği gibi gerekli olabilecek değişiklikleri izin vermek için bir mekanizma olması gerekir.<br /><br />Bir sanal makine aşağıdaki yapılandırmaları olan bir jumpbox (savunma ana bilgisayarı) olarak oluşturulur:<br /><br /><ul><li>[Kötü amaçlı yazılımdan koruma uzantısı](/azure/security/azure-security-antimalware)</li><li>[OMS izleme uzantısı](/azure/virtual-machines/virtual-machines-windows-extensions-oms)</li><li>[VM tanılama uzantısını](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)</li><li>[BitLocker şifreli Disk](/azure/security/azure-security-disk-encryption)</li></ul>Azure anahtar kasası kullanarak Azure kamu, PCI DSS ve HIPAA gereksinimleri ile hizalar.|



## <a name="pci-dss-requirement-35"></a>PCI DSS gereksinim 3.5

**3.5** saklı kart sahibi verilerin açığa çıkması ve kötüye karşı güvenliğini sağlamak için kullanılan anahtarları korumak için belge ve uygulama yordamları. 

> [!NOTE]
> Bu gereksinim saklı kart sahibi verileri şifrelemek için kullanılan anahtarları için geçerlidir ve anahtarı şifrelemek için de geçerlidir veri şifreleme anahtarlarınızı korumak için kullanılan anahtarları — bu anahtar şifreleme anahtarları veri şifreleme anahtarı olarak en az sağlam olması gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Microsoft Azure müşteri anahtar kasalarını birbirinden mantıksal olarak yalıtılmış ve mantıksal anahtar kasası hizmetindeki Yönetim düzeyi yalıtılmış sağlar. Anahtar kasası, Microsoft Müşteri'nin anahtar kasası konumu erişim yok şekilde tasarlanmıştır. <br /><br />Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenlik modülleri (HSM'ler) kullanılarak Microsoft Azure tarafından korunur.<br /><br />Microsoft Azure anahtar kasasında depolanan bir anahtar, başka bir anahtarını korumak için kullanılabilir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore göstermek ve CHD demo korumak için korumalı bir anahtar çözümü dağıtmak için belgeler sağlar.|



### <a name="pci-dss-requirement-351"></a>PCI DSS gereksinim 3.5.1

**3.5.1** *yalnızca hizmet sağlayıcıları için ek gereksinimi:* belgelenen açıklamasını içerir şifreleme mimarisinin Koru:
- Tüm algoritmalar, protokolleri ve anahtar gücü ve bitiş tarihi dahil olmak üzere, kart sahibi veri koruması için kullanılan anahtarları ayrıntıları
- Her anahtar için anahtar kullanım açıklaması
- Tüm Hsm'leri ve anahtar yönetimi için kullanılan diğer SCDs sayımı 

> [!NOTE]
> Bu gereksinim, bir gereksinim haline geldikten sonra en iyi 31 Ocak 2018 kadar uygulamadır.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Microsoft Azure müşteri anahtar kasalarını birbirinden mantıksal olarak yalıtılmış ve mantıksal anahtar kasası hizmetindeki Yönetim düzeyi yalıtılmış sağlar. Anahtar kasası, Microsoft Müşteri'nin anahtar kasası konumu erişim yok şekilde tasarlanmıştır. <br /><br />Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenlik modülleri (HSM'ler) kullanılarak Microsoft Azure tarafından korunur.<br /><br />Microsoft Azure anahtar kasasında depolanan bir anahtar, başka bir anahtarını korumak için kullanılabilir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore göstermek ve CHD demo korumak için korumalı bir anahtar çözümü dağıtmak için belgeler sağlar.|



### <a name="pci-dss-requirement-352"></a>PCI DSS gereksinim 3.5.2'de

**3.5.2'de** koruyucuları gerekli en az sayıda şifreleme anahtarları kısıtlayın.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtar kasası ayrıntılı erişim ilkeleri destekler, bu belirli varlıklar için belirli işlemleri gerçekleştirmek için belirli işlevlere erişim vermek bir anahtar kasası sahibi izin verin. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore anahtar yönetimi için tek bir kullanıcı hesabı yalıtılmış (yönetici ##@contosowebstore.com).|



### <a name="pci-dss-requirement-353"></a>PCI DSS gereksinim 3.5.3

**3.5.3** her zaman bir (veya daha fazla) aşağıdaki biçimlerden birini kart sahibi veri şifreleme/şifre çözme için kullanılan gizli ve özel anahtarları depolamak:
- Veri şifreleme anahtarı olarak en az sağlam ve ayrı ayrı veri şifreleme anahtarı saklanan anahtar şifreleme anahtar ile şifrelenmiş
- İçindeki bir güvenli şifreleme aygıt (örneğin, bir (ana) donanım güvenlik modülü (HSM) veya NK onaylı etkileşim noktası aygıt)
- En az iki aracılığıyla anahtar bileşenler veya anahtar paylaşımları bir endüstri uygun olarak yöntemi kabul edilen 

> [!NOTE]
> Bu ortak anahtarları bu formların birinde depolanması gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtarlar müşteri tarafından tanımlanan belirli anahtar kasalarını depolanır.<br /><br />Anahtar kasası aynı anda hem de genel olarak birden çok uygulama tarafından bir anahtarı kopyalayın ve birden fazla konumda depolamak için gereken azaltır erişilebilir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-354"></a>PCI DSS gereksinim 3.5.4

**3.5.4** şifreleme anahtarları en az olası konumlarda saklayın.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtarlar müşteri tarafından tanımlanan belirli anahtar kasalarını depolanır. <br /><br />Anahtar kasası aynı anda hem de genel olarak birden çok uygulama tarafından bir anahtarı kopyalayın ve birden fazla konumda depolamak için gereken azaltır erişilebilir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



## <a name="pci-dss-requirement-36"></a>PCI DSS gereksinim 3.6

**3.6** tam olarak belge ve tüm anahtar yönetimi işlemlerini ve aşağıdakiler dahil kart sahibi veri şifreleme için kullanılan şifreleme anahtarları için yordamları uygulayın. 

> [!NOTE]
> Anahtar Yönetimi için çok sayıda endüstri standartları http://csrc.nist.gov bulunabilir NIST dahil olmak üzere çeşitli kaynaklardan kullanılabilir.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-361"></a>PCI DSS gereksinim 3.6.1

**3.6.1** güçlü şifreleme anahtarları oluşturma

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:** <br /><br />Anahtarları anahtar kasasına oluştururken, Müşteri'nin belirtimleri başına anahtarları oluşturmak için Azure sorumludur. Anahtarları bir HSM kullanarak oluşturulur. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-362"></a>PCI DSS gereksinim 3.6.2

**3.6.2** güvenli şifreleme anahtar dağıtımı

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Getir kendi anahtarı (BYOK) aracı müşteri anahtarını kapsar ve belirli bir Azure aboneliği ile bağlantılı bir özel güvenlik kasası hedefler. Anahtar, belirtilen bölgede tanımlanan aboneliğin anahtar kasasına yalnızca alınabilir. Bu işlem donanım üreticisi tarafından sağlanan şifreleme yordamları kullanır. Müşteriler aktarımı başarılı bir bildirim alırsınız. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-363"></a>PCI DSS gereksinim 3.6.3

**3.6.3** güvenli şifreleme anahtarı depolama alanı

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtarları HSM'ler depolanır ve donanım üreticisinin şifreleme güvenlik kullanarak güvenlidir. Meta veri anahtarlar her anahtar Kasası'na benzersiz şifrelenmiş bir duruma Azure Storage'da depolanır. <br /><br /> |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-364"></a>PCI DSS gereksinim 3.6.4

**3.6.4** kendi cryptoperiod sonuna (örneğin, tanımlı bir süre geçtikten sonra ve/veya belirli bir miktarda şifreli metin belirli bir anahtar tarafından üretilen sonra) ulaştınız anahtarları için şifreleme anahtarı değişiklikleri, ilişkili tarafından tanımlanan uygulamanın satıcısına veya anahtar sahibi ve temel alınarak endüstri en iyi yöntemler ve yönergeleri (örneğin, NIST özel yayını 800-57).

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtar kasası güncelleştirmeye veya müşteri tarafından tanımlandığı anahtarlarını alma işlevselliği destekler. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-365"></a>PCI DSS gereksinim 3.6.5

**3.6.5** devre dışı bırakma ya da (örneğin, bir çalışan ayrılma bilgisine sahip bir düz metin anahtarı anahtar bütünlüğü zayıflar zaman gerekli görüldüğü gibi anahtarların değiştirme (örneğin, arşivleme, yok etme ve/veya iptal) bileşeni) veya anahtarları şüpheli tehlikeye biri. 

> [!NOTE]
> Devre dışı bırakılan veya korunabilmesi için şifreleme anahtarları gereksinimi yerine, bu anahtarları güvenli bir şekilde (örneğin, bir anahtar şifreleme anahtarı kullanarak) arşivlenmiş gerekir. Arşivlenmiş şifreleme anahtarları yalnızca şifre çözme/doğrulama amacıyla kullanılmalıdır.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtar kasası devre dışı bırak veya Değiştir, müşteri tarafından tanımlandığı şekilde işlevselliği destekler. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-366"></a>PCI DSS gereksinim 3.6.6

**3.6.6** el ile düz metin şifreleme anahtarı yönetim işlemlerini kullandıysanız, bu işlemlerin bölünmüş bilgi ve çift denetimini kullanarak yönetiliyor olması gerekir. 

> [!NOTE]
> El ile anahtar yönetim işlemlerini örnekleri dahil ancak bunlarla sınırlı değildir: anahtar oluşturma, iletim, yükleme, depolama ve yok etme.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-367"></a>PCI DSS gereksinim 3.6.7

**3.6.7** önlemek için şifreleme anahtarlarının yetkisiz değiştirme.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | **Anahtar kasası kullanan müşteriler için:**<br /><br />Anahtar kasalarını mantıksal olarak ayrılır ve geçici dizin yetkilendirmesi desteklemez. Sonuç olarak, yetkisiz değiştirme engellenir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-368"></a>PCI DSS gereksinim 3.6.8

**3.6.8** anlamak ve, anahtar koruyucusu sorumluluklarını kabul resmi olarak onaylamak şifreleme anahtar koruyucuları gereksinimini.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore anahtar yönetimi için tek bir kullanıcı hesabı yalıtılmış (yönetici ##@contosowebstore.com).|



## <a name="pci-dss-requirement-37"></a>PCI DSS gereksinim 3.7

**3.7** güvenlik ilkeleri ve saklı kart sahibi verileri korumak için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore tüm anahtar yönetimi için Azure anahtar kasası kullanır. Daha fazla bilgi için bkz: [PCI Kılavuzu - şifreleme](payment-processing-blueprint.md#encryption-and-secrets-management).|




