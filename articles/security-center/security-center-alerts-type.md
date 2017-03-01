---
title: "Azure Güvenlik Merkezi&quot;nde Türe göre güvenlik uyarıları | Microsoft Belgeleri"
description: "Bu belge Azure Güvenlik Merkezi&quot;nde mevcut olan güvenlik uyarı türünü anlamanıza yardımcı olur."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: b3e7b4bc-5ee0-4280-ad78-f49998675af1
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 5d51a5ef3387b4c00079547b0f44ffe1f96bd77c
ms.openlocfilehash: 9ee2d2ef7b21fab8cfc4a70561d612be7367d366


---
# <a name="security-alerts-by-type-in-azure-security-center"></a>Azure Güvenlik Merkezi’nde Türe Göre Güvenlik Uyarıları
Bu belge Azure Güvenlik Merkezi'nde mevcut olan farklı güvenlik uyarısı türlerini anlamanıza yardımcı olur. Uyarıların nasıl yönetileceği hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) konusunu okuyun.

> [!NOTE]
> Gelişmiş algılamaları etkinleştirmek için Azure Güvenlik Merkezi Standart sürümüne yükseltme yapın. 90 günlük ücretsiz deneme sürümü mevcuttur. Yükseltmek için [Güvenlik İlkesi](security-center-policies.md)’nde Fiyatlandırma Katmanı’nı seçin. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).
>
>

## <a name="what-type-of-alerts-are-available"></a>Hangi tür uyarılar mevcuttur?
Azure Güvenlik Merkezi sanal sonlandırma zincirinin aşamalarına uygun çeşitli uyarılar sağlar. Aşağıdaki şekilde bu aşamalardan bazılarıyla ilgili olan çeşitli uyarıların birtakım örnekleri verilmiştir.

![Sonlandırma zinciri](./media/security-center-alerts-type/security-center-alerts-type-fig1.png)

**Hedef ve Saldırı**

* Gelen RDP/SSH saldırıları
* Uygulama ve DDoS saldırıları (WAF ortakları)
* İzinsiz giriş algılaması (NG Güvenlik Duvarı ortakları)

**Yükleme ve Açıklardan Yararlanma**

* Bilinen kötü amaçlı yazılım imzaları (AM ortakları)
* Bellek içi kötü amaçlı yazılım ve açıklardan yararlanma denemeleri
* Şüpheli işlem yürütme
* Bulmayı önlemek için hileli hareketler
* Yana hareket
* İç keşif
* Şüpheli PowerShell etkinliği

**Yayınlama İhlali**  

* Bilinen bir kötü amaçlı IP ile iletişim (veri sızdırma veya komut ve denetim)
* Başka saldırılar yapmak için riskli kaynakları kullanma (giden bağlantı noktası tarama RDP/SSH deneme yanılma saldırıları ve istenmeyen posta)

Her bir aşamayla farklı saldırı türleri ilişkilendirilir ve farklı alt sistemler hedeflenir. Bu aşamalarda saldırıları ele almak için Güvenlik Merkezi’nde üç uyarı kategorisi bulunur:

* Sanal Makine Davranış Analizi (VMBA)
* Ağ Analizi
* Kaynak Analizi

## <a name="virtual-machine-behavioral-analysis"></a>Sanal makine davranış analizi
Azure Güvenlik Merkezi; sanal makine günlüklerinin analizine göre tehlike giren kaynakları belirlemek amacıyla davranış analizini kullanabilir (örneğin, İşlem Oluşturma Olayları, Oturum Açma Olayları, vb.). Ayrıca, yaygın bir kampanyanın kanıtını desteklemek üzere denetlenmesi gereken diğer sinyallerle bir bağlantı vardır.

> [!NOTE]
> Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.
>
>

### <a name="crash-analysis"></a>Kilitlenme analizi
Kilitlenme bellek dökümü analizi, geleneksel güvenlik çözümlerini atlatabilen karmaşık kötü amaçlı yazılımları algılamak için kullanılan bir yöntemdir. Kötü amaçlı yazılımların çeşitli türleri, diske hiçbir zaman yazmayarak veya diske yazılmış yazılım bileşenlerini şifreleyerek virüsten koruma ürünleri tarafından algılanma olasılığını azaltmaya çalışır. Bu durum, kötü amaçlı yazılımların geleneksel kötü amaçlı yazılımdan koruma yaklaşımlarıyla algılanmasını zor hale getirir. Ancak, bu tür kötü amaçlı yazılımlar çalışmak için bellekte iz bırakmak zorunda olduğundan bellek analizi kullanılarak algılanabilir.

Yazılım kilitlendiğinde bir kilitlenme dökümü kilitlenme sırasında belleğin bir kısmını yakalar. Kilitlenme durumu kötü amaçlı yazılımlardan, genel uygulama veya sistem sorunlarından kaynaklanabilir. Kilitlenme dökümündeki belleği analiz eden Güvenlik Merkezi, yazılımdaki açıklardan yararlanmak, gizli verilere erişmek ve tehlikeye giren bir makineye gizlice sızmak için kullanılan teknikleri algılayabilir. Analiz Güvenlik Merkezi arka ucu tarafından gerçekleştirildiği için bu özellik, ana bilgisayarların performansına en az etki ile sağlanır.

Aşağıdaki alanlar, aşağıda listelenen kilitlenme dökümü analiz uyarılarında ortak olarak görülür:

* DUMPFILE: Kilitlenme döküm dosyasının adı
* PROCESSNAME: Kilitlenen işlemin adı
* PROCESSVERSION: Kilitlenen işlemin sürümü

### <a name="shellcode-discovered"></a>Kabuk kodu bulundu
Kabuk Kodu, kötü amaçlı yazılım bir yazılım güvenlik açığından yararlandıktan sonra çalıştırılan yüktür. Bu uyarı, kilitlenme dökümü analizinin kötü amaçlı yükler tarafından yaygın olarak gerçekleştirilen davranışları sergileyen yürütülebilir kodlar algıladığını belirtir. Bu davranış kötü amaçlı olmayan yazılımlar tarafından gerçekleştiriliyor olabilir, ancak normal yazılım geliştirme uygulamaları için alışıldık bir davranış değildir.

Bu uyarı tarafından aşağıdaki ek alan sağlanır:

* ADDRESS: Kabuk kodunun bellekteki konumu

İşte bu tür bir uyarı örneği:

![Kabuk kodu uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig2.png)

### <a name="module-hijacking-discovered"></a>Modül ele geçirme bulundu
Windows, yazılımların ortak Windows sistem işlevselliğinden yararlanmasına izin vermek için Dinamik Bağlantı Kitaplıkları’nı (DLL’ler) kullanır. DLL Ele Geçirme, kötü amaçlı iş yüklerinin rastgele kodların yürütülebileceği belleğe yüklenmesi için kötü amaçlı yazılım tarafından DLL yükleme sırası değiştirildiğinde gerçekleşir. Bu uyarı, kilitlenme dökümü analizinin iki farklı yoldan yüklenmiş benzer adlı bir modül algıladığını ve yüklenen yollardan birinin ortak bir Windows sistemi ikili konumundan geldiğini gösterir.

Yasal yazılım geliştiricileri, Windows işletim sistemini veya Windows uygulamalarını izleme, genişletme gibi kötü amaçlı olmayan gerekçelerden dolayı nadiren DLL yükleme sırasını değiştirir. Azure Güvenlik Merkezi, DLL yükleme sırasında yapılan kötü amaçlı değişikliklerle zararsız olabilecek değişikliklerin birbirinden ayırt edilmesine yardımcı olmak için yüklenen bir modülün şüpheli bir profile uygun olup olmadığını denetler. Bu denetimin sonucu, uyarının “SIGNATURE” alanı tarafından gösterilir ve uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımlarında yansıtılır. Ele geçiren modülün diskteki kopyasının incelenmesi (örneğin, dosyaların dijital imzası doğrulanarak veya bir virüsten koruma taraması gerçekleştirilerek), ele geçiren modülünün yasal mı yoksa kötü amaçlı mı olduğuna ilişkin daha fazla bilgi sağlayabilir.

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki alanları sağlar:

* SIGNATURE: Ele geçiren modülün bir şüpheli davranış profiline uygun olup olmadığını gösterir
* HIJACKEDMODULE: Ele geçirilen Windows sistem modülünün adı
* HIJACKEDMODULEPATH: Ele geçirilen Windows sistem modülünün yolu
* HIJACKINGMODULEPATH: Ele geçiren modülün yolu

İşte bu tür bir uyarı örneği:

![Modül ele geçirme uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig3.png)

### <a name="masquerading-windows-module-detected"></a>Kendini gizleyen Windows modulü algılandı
Kötü amaçlı yazılım, “araya karışmak” ve kötü amaçlı yazılımın gerçek yapısını sistem yöneticilerinden saklamak amacıyla Windows sistem ikili dosyaları (örn. SVCHOST.EXE) veya modülleri (örn. NTDLL.DLL) için yaygın olarak kullanılan adları kullanabilir. Bu uyarı, kilitlenme dökümü analizinde kilitlenme dökümü dosyalarının Windows sistem modülü adlarını kullanan, ancak normal Windows modüllerine yönelik diğer kriterleri karşılamayan modüller içerdiğinin algılandığını gösterir. Kendini gizleyen modülün diskteki kopyasının çözümlenmesi, bu modülün yasal mı yoksa kötü amaçlı mı olduğu konusunda daha fazla bilgi sağlayabilir. Analiz şunları içerebilir:

* İlgili dosyanın yasal bir yazılım paketinin bir parçası olarak gönderildiğini doğrulayın
* Dosyanın dijital imzasını doğrulayın
* Dosya üzerinde virüsten koruma taraması çalıştırın

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki ek alanları sağlar:

* DETAILS: Modülün meta verilerinin geçerli olup olmadığını ve modülün bir sistem yolundan yüklenip yüklenmediğini açıklar.
* NAME: Kendini gizleyen Windows modülünün adı
* PATH: Kendini gizleyen Windows modülünün yolu.

Bu uyarı, modülün PE üst bilgisinden “CHECKSUM” ve “TIMESTAMP” gibi belirli alanları da ayıklar ve görüntüler. Bu alanlar yalnızca modülde varsa görüntülenir. Bu alanlarla ilgili ayrıntılı bilgi için bkz. [Microsoft PE ve COFF Belirtimi](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

İşte bu tür bir uyarı örneği:

![Kendini gizleyen Windows uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig4.png)

### <a name="modified-system-binary-discovered"></a>Değiştirilen sistem ikili dosyası bulundu
Kötü amaçlı yazılım, verilere gizlice erişmek veya güvenliği ihlal edilmiş bir sistemde kendini gizleyerek kalıcı olmak için çekirdek sistem ikili dosyalarını değiştirebilir. Bu uyarı, kilitlenme dökümü analizi tarafından bellekte veya diskte çekirdek Windows OS ikili dosyalarının değiştirildiğinin algılandığını gösterir.
Yasal uygulama geliştiricileri, Sapmalar veya uygulama uyumluluğu gibi kötü amaçlı olmayan nedenlerle bellekte sistem modüllerini nadiren değiştirir. Azure Güvenlik Merkezi, kötü amaçlı modüllerle yasal olabilecek modüllerin birbirinden ayırt edilmesine yardımcı olmak için değiştirilen modülün bir şüpheli profile uygun olup olmadığını denetler. Bu denetimin sonucu, uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımları tarafından gösterilir.

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki ek alanları sağlar:

* MODULENAME: Değiştirilen sistem ikili dosyasının adı
* MODULEVERSION: Değiştirilen sistem ikili dosyasının sürümü

İşte bu tür bir uyarı örneği:

![Sistem ikili dosyası uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig5.png)

### <a name="suspicious-process-executed"></a>Şüpheli işlem yürütüldü
Güvenlik Merkezi, hedef sanal makinede yürütülen şüpheli işlemi tanımlar ve bir uyarı tetikler. Algılama aşamasında belirli ad değil parametreye göre arama yapılır; bu nedenle, saldırgan yürütülebilir dosyayı yeniden adlandırsa bile Güvenlik Merkezi tarafından algılanabilir.

İşte bu tür bir uyarı örneği:

![Şüpheli işlem uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig6-new.png)

### <a name="multiple-domain-accounts-queried"></a>Birden fazla etki alanı hesabı sorgulandı
Güvenlik Merkezi, saldırganların ağ keşfi sırasında genellikle gerçekleştirdiği bir işlem olan etki alanı hesaplarını sorgulamaya yönelik birden fazla girişimi algılayabilir. Saldırganlar kullanıcıların kim olduğunu, etki alanı yönetici hesaplarının ne olduğunu, hangi bilgisayarların Etki Alanı Denetleyicileri olduğunu ve diğer etki alanlarıyla olası etki alanı güven ilişkisini belirlemek üzere etki alanını sorgulamak için bu teknikten yararlanabilir.

İşte bu tür bir uyarı örneği:

![Birden fazla etki alanı hesabı uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig7-new.png)

## <a name="network-analysis"></a>Ağ analizi
Güvenlik Merkezi ağ tehdidi algılaması, Azure IPFIX (İnternet Protokolü Akış Bilgileri Verme) trafiğinizden güvenlik verilerini otomatik olarak toplayarak çalışır. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder.

### <a name="suspicious-outgoing-traffic-detected"></a>Şüpheli giden trafik algılandı
Ağ cihazları diğer sistem türlerine büyük ölçüde benzer şekilde bulunabilir ve profili oluşturulabilir. Saldırganlar genellikle bağlantı noktası tarama / bağlantı noktası süpürme ile işe başlarlar. Aşağıdaki örnekte bir dış kaynağa karşı SSH deneme yanılma saldırıcı ya da bağlantı noktası süpürme saldırısı gerçekleştiriyor olabilecek bir sanal makineden şüpheli bir SSH trafiği vardır.

![Şüpheli giden trafik uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig8.png)

Bu uyarı bu saldırıyı başlatmak için kullanılan kaynağı, tehlikeye giren makineyi, algılama zamanını ve kullanılan protokol ile bağlantı noktasını tanımlamanızı sağlayan bilgiler verir. Bu dikey pencere ayrıca bu sorunu gidermek için kullanılabilecek bir düzeltme adımları listesi verir.

### <a name="network-communication-with-a-malicious-machine"></a>Kötü amaçlı bir makine ile ağ iletişimi
Microsoft tehdit bilgileri akışlarından yararlanan Azure Güvenlik Merkezi, kötü amaçlı IP adresleriyle iletişim kuran ve çoğunlukla bir komut ve denetim merkezi olan riskli makineleri algılayabilir. Bu örnekte Azure Güvenlik Merkezi iletişimin Pony Loader kötü amaçlı yazılımı ([Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF) olarak da bilinir) kullanılarak yapıldığını algılamıştır.

![ağ iletişimi uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig9.png)

Bu uyarı bu saldırıyı başlatmak için kullanılan kaynağı, saldırıya uğrayan kaynağı, maruz kalan IP’yi, saldıran IP’yi ve algılama zamanını tanımlamanızı sağlayan bilgiler verir.

> [!NOTE]
> Dinamik IP adresleri gizlilik amacıyla bu ekran görüntüsünden kaldırılmıştır.
>
>

### <a name="possible-outgoing-denial-of-service-attack-detected"></a>Olası giden hizmet reddi saldırısı algılandı
Bir sanal makineden kaynaklanan anormal ağ trafiği, Güvenlik Merkezi’nin olası bir hizmet reddi saldırı türü tetiklemesine yol açabilir.

İşte bu tür bir uyarı örneği:

![Giden DOS](./media/security-center-alerts-type/security-center-alerts-type-fig10-new.png)

## <a name="resource-analysis"></a>Kaynak analizi
Güvenlik Merkezi kaynak analizi, [Azure SQL Veritabanı Tehdidi Algılama](../sql-database/sql-database-threat-detection.md) özelliği ile tümleştirme gibi PaaS hizmetlerine odaklanır. Bu alanlardan elde edilen analiz sonuçlarına bağlı olarak, Güvenlik Merkezi kaynakla ilgili bir uyarı tetikler.

### <a name="potential-sql-injection"></a>Olası SQL ekleme
SQL ekleme, kötü amaçlı bir kodun daha sonra ayrıştırma ve yürütme amacıyla SQL Server örneğine geçirildiği dizelere eklendiği bir saldırıdır. SQL Server sözdizimsel açıdan geçerli olan aldığı tüm sorguları yürüttüğü için SQL deyimleri oluşturan her türlü yordam, ekleme güvenlik açıklarına karşı gözden geçirilmelidir. SQL Tehdit Algılama özelliği, Azure SQL Veritabanlarınızda gerçekleşebilecek şüpheli olayları belirlemek üzere machine learning, davranış analizi ve anormallik algılaması kullanır. Örneğin:

* Eski bir çalışan tarafından veritabanı erişimi denendi
* SQL ekleme saldırıları
* Evdeki bir kullanıcıdan üretim veritabanına olağan dışı erişim

![Olası SQL Ekleme uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig11.png)

Bu uyarı saldırıya uğrayan kaynağı, algılama zamanını, saldırının durumunu tanımlamanızı sağlayan ve ayrıca diğer araştırma adımlarının bağlantısını sunan bilgiler verir.

### <a name="vulnerability-to-sql-injection"></a>SQL Ekleme Güvenlik Açığı
Bir veritabanında SQL ekleme saldırılarına karşı olası bir güvenlik açığını belirtebilen bir uygulama hatası algılandığında bu uyarı tetiklenir.

![Olası SQL Ekleme uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig12-new.png)

### <a name="unusual-access-from-unfamiliar-location"></a>Tanınmayan konumdan olağan dışı erişim
Sunucuda son dönemde görülmemiş, tanınmayan bir IP adresinden erişim algılandığında bu uyarı tetiklenir.

![Olağan dışı erişim uyarısı](./media/security-center-alerts-type/security-center-alerts-type-fig13-new.png)

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi’ndeki farklı güvenlik uyarısı türleri hakkında bilgi edindiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
* [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Feb17_HO3-->


