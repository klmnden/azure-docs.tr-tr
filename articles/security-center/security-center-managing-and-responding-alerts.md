<properties
   pageTitle="Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama | Microsoft Azure"
   description="Bu belge, güvenlik uyarılarını yönetmek ve yanıtlamak için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/07/2016"
   ms.author="yurid"/>

# Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi’ni kullanmanıza yardımcı olur.

## Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir. Azure Güvenlik Merkezi, sonlandırma zinciri desenleri ile hizalanan uyarıları da [Olaylar](security-center-incident.md) altında toplar. 

> [AZURE.NOTE] Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.


## Güvenlik ilkelerini yönetme

**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Azure Portal’ı açın ve her bir uyarı hakkında daha fazla ayrıntıya ulaşmak için aşağıdaki adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Güvenlik Merkezi'nde güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2.  Aşağıda gösterildiği gibi, uyarılar hakkında daha fazla ayrıntı içeren **Güvenlik uyarıları** dikey penceresini açmak için kutucuğa tıklayın.

    ![Güvenlik Merkezi'nde Güvenlik uyarıları dikey penceresi](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Bu dikey pencerenin alt bölümünde her bir uyarı için ayrıntılar bulunur. Sıralamak için hangi sütuna göre sıralamak istediğinizi belirtin. Her sütunun tanımı aşağıda verilmiştir:

- **Uyarı**: Uyarının kısa bir açıklaması.
- **Sayı**: Belirtilmiş türün belirli bir günde algılanan tüm uyarılarının listesi.
- **Algılayan**: Uyarıyı tetiklemekten sorumlu hizmet.
- **Tarih**: Olayın gerçekleştiği tarih
- **Durum**: Bu uyarı için geçerli durum. İki tür durum mevcuttur:
    - **Etkin**: Güvenlik uyarısı algılandı.
    - **Kapatıldı**: Güvenlik uyarısı kullanıcı tarafından kapatıldı. Bu durum genellikle, araştırılan ancak azaltılan ya da gerçek bir saldırı olarak düşünülmeyen uyarılar için kullanılır.

- **Önem derecesi**: Yüksek, orta veya düşük önem düzeyi.

### Uyarıları filtreleme

Tarihe, duruma ve önem derecesine göre uyarıları filtreleyebilirsiniz. Filtreleme uyarıları, güvenlik uyarıları gösterimi kapsamını daraltmanızın gerektiği senaryolar için faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

1. **Güvenlik Uyarıları** dikey penceresindeki **Filtreleme**'ye tıklayın. **Filtreleme** dikey penceresi açılır ve görmek istediğiniz tarih, durum ve önem derecesi değerlerini seçersiniz.

    ![Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-ga.png)

2.  Bir güvenlik uyarısını araştırdıktan sonra, uyarının ortamınız için hatalı bir pozitif sonuç olduğunu veya belirli bir kaynak için beklenen davranışı belirttiğini fark edebilirsiniz. Her durumda, bir güvenlik uyarısını uygulanamaz olarak belirlerseniz uyarıyı kapatabilir ve ardından görünümünüzden çıkarabilirsiniz. Bir güvenlik uyarısını kapatmanın iki yolu vardır. Uyarıya sağ tıklayın ve **Kapat**'ı seçin veya bir öğenin üzerine gelerek sağ tarafta görüntülenen üç noktaya tıklayın ve **Kapat**'a tıklayın. **Filtreleme**'ye tıklayıp **Kapatıldı**'yı seçerek kapatılan güvenlik uyarılarını görüntüleyebilirsiniz.

    ![Güvenlik Merkezi'nde uyarıları kapatma](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig4-ga.png)

### Güvenlik uyarılarını yanıtlama

Uyarıyı tetikleyen olay(lar) ve saldırıyı düzeltmek için (varsa) hangi adımları atmanız gerektiği hakkında daha fazla bilgi edinmek için bir güvenlik uyarısı seçin. Güvenlik uyarıları, türe ve tarihe göre gruplandırılır. Güvenlik uyarısına tıklandığında gruplanan uyarıların listesini içeren dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

Bu durumda, tetiklenen uyarılar şüpheli Uzak Masaüstü Protokolü (RDP) etkinliğine işaret eder. Birinci sütunda hangi kaynakların saldırıya uğradığı; ikinci sütunda kaynağın kaç kez saldırıya uğradığı; üçüncü sütunda saldırının zamanı; dördüncü sütunda uyarının durumu ve beşinci sütunda saldırının önem derecesi gösterilir. Bu bilgileri gözden geçirdikten sonra, saldırıya uğrayan kaynağa tıkladığınızda yeni bir dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarıları hakkında ne yapılacağına ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

Bu dikey pencerenin **Açıklama** alanında bu olay hakkında daha fazla ayrıntı bulabilirsiniz. Bu ek ayrıntılar, güvenlik uyarısını neyin tetiklediği, hedef kaynak, uygulanabilirse kaynak IP adresi ve düzeltmeye ilişkin öneriler hakkında öngörüler sunar.  Windows güvenlik olayı günlüklerinin tümü IP adresi içermediğinden, bazı durumlarda kaynak IP adresi boştur (yoktur).

> [AZURE.NOTE] Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir. Bazı durumlarda, önerilen düzeltmeyi uygulamak için diğer Azure işlevlerini kullanmak zorunda kalabilirsiniz. Örneğin, bu saldırıya ilişkin düzeltme, [ağ ACL'si](../virtual-network/virtual-networks-acl.md) veya [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) kuralı kullanarak bu saldırıyı gerçekleştiren IP adresini kara listeye almaktır.

## Türe göre güvenlik uyarıları
Şüpheli RDP etkinliği uyarısına erişmek için kullanılan adımların aynısı diğer uyarı türlerine erişmek için kullanılabilir. Güvenlik Merkezi’nde görebileceğiniz uyarıların bazı diğer örnekleri aşağıda verilmiştir:

### Olası SQL Ekleme
SQL ekleme, kötü amaçlı bir kodun daha sonra ayrıştırma ve yürütme amacıyla SQL Server örneğine geçirildiği dizelere eklendiği bir saldırıdır. SQL Server sözdizimsel açıdan geçerli olan aldığı tüm sorguları yürüttüğü için SQL deyimleri oluşturan her türlü yordam, ekleme güvenlik açıklarına karşı gözden geçirilmelidir. 

![SQL ekleme uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9.png) 

Bu uyarı saldırıya uğrayan kaynağı, algılama zamanını, saldırının durumunu tanımlamanızı sağlayan ve ayrıca diğer araştırma adımlarının bağlantısını sunan bilgiler verir.

### Şüpheli giden trafik algılandı

Ağ cihazları diğer sistem türlerine büyük ölçüde benzer şekilde bulunabilir ve profili oluşturulabilir. Saldırganlar genellikle bağlantı noktası tarama / bağlantı noktası süpürme ile işe başlarlar. Aşağıdaki örnekte bir dış kaynağa karşı SSH deneme yanılma saldırıcı ya da bağlantı noktası süpürme saldırısı gerçekleştiriyor olabilecek bir sanal makineden şüpheli bir SSH trafiği vardır. 

![Şüpheli giden trafik uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig-10.png)

Bu uyarı bu saldırıyı başlatmak için kullanılan kaynağı, tehlikeye giren makineyi, algılama zamanını ve kullanılan protokol ile bağlantı noktasını tanımlamanızı sağlayan bilgiler verir. Bu dikey pencere ayrıca bu sorunu gidermek için kullanılabilecek bir düzeltme adımları listesi verir.

### Kötü amaçlı bir makine ile ağ iletişimi
 
Microsoft tehdit bilgileri akışlarından yararlanan Azure Güvenlik Merkezi, kötü amaçlı IP adresleriyle iletişim kuran ve çoğunlukla bir komut ve denetim merkezi olan riskli makineleri algılayabilir. Bu örnekte Azure Güvenlik Merkezi iletişimin Pony Loader kötü amaçlı yazılımı ([Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF) olarak da bilinir) kullanılarak yapıldığını algılamıştır.

![Kötü amaçlı bir makine ile ağ iletişimi uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9-ga.png)

Bu uyarı bu saldırıyı başlatmak için kullanılan kaynağı, saldırıya uğrayan kaynağı, maruz kalan IP’yi, saldıran IP’yi ve algılama zamanını tanımlamanızı sağlayan bilgiler verir.

> [AZURE.NOTE] Dinamik IP adresleri gizlilik amacıyla bu ekran görüntüsünden kaldırılmıştır.

### Kabuk Kodu Bulundu 

Kabuk Kodu, kötü amaçlı yazılım bir yazılım güvenlik açığından yararlandıktan sonra çalıştırılan yüktür.  Bu uyarı, kilitlenme dökümü analizinin kötü amaçlı yükler tarafından yaygın olarak gerçekleştirilen davranışları sergileyen yürütülebilir kodlar algıladığını belirtir.  Bu davranış kötü amaçlı olmayan yazılımlar tarafından gerçekleştiriliyor olabilir, ancak normal yazılım geliştirme uygulamaları için alışıldık bir davranış değildir. 

Aşağıdaki alanlar tüm kilitlenme dökümü uyarılarında ortaktır:

- DUMPFILE: Kilitlenme döküm dosyasının adı 
- PROCESSNAME: Kilitlenen işlemin adı 
- PROCESSVERSION: Kilitlenen işlemin sürümü 

Bu uyarı tarafından aşağıdaki ek alan sağlanır:

- ADDRESS: Kabuk kodunun bellekteki konumu

İşte bu tür bir uyarı örneği:

![Kabuk kodu uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig10-ga.png)

### Kod Ekleme Bulundu

Kod ekleme, çalışmakta olan işlemlere veya iş parçacıklarına yürütülebilir modüllerin eklenmesidir.  Bu teknik, kötü amaçlı yazılımlar tarafından verilere erişmek, yazılımı gizlemek ya da kaldırılmasını önlemek (örn. kalıcılık) için kullanılır.  Bu uyarı, kilitlenme dökümü analizinin kilitlenme dökümüyle birlikte eklenen bir modül algıladığını gösterir.
 
Yasal yazılım geliştiricileri, var olan bir uygulama ya da işletim sistemi bileşenini değiştirme ya da genişletme gibi nadir durumlarda kötü amaçlı olmayan amaçlar için kod ekleme gerçekleştirir.  Azure Güvenlik Merkezi, kötü amaçlı olan ve olmayan eklenen modülleri birbirinden ayırt etmenize yardımcı olmak için eklenen modülün bir şüpheli davranış profiline uygun olup olmadığını denetler. Bu denetimin sonucu, uyarının “SIGNATURE” alanı tarafından gösterilir ve uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımlarında yansıtılır.  

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki ek alanları sağlar:

- ADDRESS: Eklenen modülün bellekteki konumu
- IMAGENAME: Eklenen modülün adı. Görüntüde görüntü adı sağlanmadıysa bu alanın boş olabileceğini unutmayın.
- SIGNATURE: Eklenen modülün bir şüpheli davranış profiline uyup uymadığını gösterir. Aşağıdaki tabloda örnek sonuçlar ve açıklamaları gösterilmektedir:

| **İmza değeri**                  | **Açıklama**                                                                                                   |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Şüpheli yansıtıcı yükleyici açığından yararlanma | Bu şüpheli davranış çoğunlukla eklenen kodun işletim sistemi yükleyicisinden bağımsız olarak yüklenmesiyle bağıntılıdır |
| Şüpheli eklenen kod açığından yararlanma          | Genellikle belleğe kod eklenmesiyle bağıntılı olan kötü amaçlılığı gösterir                                       |
| Şüpheli ekleme açığından yararlanma         | Genellikle belleğe eklenen kodun kullanımıyla bağıntılı olan kötü amaçlılığı gösterir                                   |
| Şüpheli eklenen hata ayıklayıcı açığından yararlanma | Genellikle bir hata ayıklayıcının algılanmasıyla veya aşılmasıyla bağıntılı olan kötü amaçlılığı gösterir                         |
| Şüpheli eklenen kodun uzaktan yürütülmesi açığından yararlanma   | Genellikle komut ve denetim (C2) senaryolarıyla bağıntılı olan kötü amaçlılığı gösterir                                 |

İşte bu tür bir uyarı örneği:

![Kod Ekleme Bulundu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig11-ga.png)

### Modül Ele Geçirme Bulundu

Windows, yazılımların ortak Windows sistem işlevselliğinden yararlanmasına izin vermek için Dinamik Bağlantı Kitaplıkları’nı (DLL’ler) kullanır.  DLL Ele Geçirme, kötü amaçlı iş yüklerinin rastgele kodların yürütülebileceği belleğe yüklenmesi için kötü amaçlı yazılım tarafından DLL yükleme sırası değiştirildiğinde gerçekleşir. Bu uyarı, kilitlenme dökümü analizinin iki farklı yoldan yüklenmiş benzer adlı bir modül algıladığını ve yüklenen yollardan birinin ortak bir Windows sistemi ikili konumundan geldiğini gösterir.

Yasal yazılım geliştiricileri, Windows işletim sistemini veya Windows uygulamalarını izleme, genişletme gibi kötü amaçlı olmayan gerekçelerden dolayı nadiren DLL yükleme sırasını değiştirir.  Azure Güvenlik Merkezi, DLL yükleme sırasında yapılan kötü amaçlı değişikliklerle zararsız olabilecek değişikliklerin birbirinden ayırt edilmesine yardımcı olmak için yüklenen bir modülün şüpheli bir profile uygun olup olmadığını denetler.   Bu denetimin sonucu, uyarının “SIGNATURE” alanı tarafından gösterilir ve uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımlarında yansıtılır.  Ele geçiren modülün diskteki kopyasının incelenmesi (örneğin, dosyaların dijital imzası doğrulanarak veya bir virüsten koruma taraması gerçekleştirilerek), ele geçiren modülünün yasal mı yoksa kötü amaçlı mı olduğuna ilişkin daha fazla bilgi sağlayabilir.

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki alanları sağlar:

- SIGNATURE: Ele geçiren modülün bir şüpheli davranış profiline uygun olup olmadığını gösterir
- HIJACKEDMODULE: Ele geçirilen Windows sistem modülünün adı
- HIJACKEDMODULEPATH: Ele geçirilen Windows sistem modülünün yolu
- HIJACKINGMODULEPATH: Ele geçiren modülün yolu 

İşte bu tür bir uyarı örneği:

![DLL Ele Geçirme uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig12-ga.png)

### Kendini gizleyen Windows Modulü Algılandı

Kötü amaçlı yazılım, “araya karışmak” ve kötü amaçlı yazılımın gerçek yapısını sistem yöneticilerinden saklamak amacıyla Windows sistem ikili dosyaları (örn. SVCHOST.EXE) veya modülleri (örn. NTDLL.DLL) için yaygın olarak kullanılan adları kullanabilir.  Bu uyarı, kilitlenme dökümü analizinde kilitlenme dökümü dosyalarının Windows sistem modülü adlarını kullanan, ancak normal Windows modüllerine yönelik diğer kriterleri karşılamayan modüller içerdiğinin algılandığını gösterir. Kendini gizleyen modülün diskteki kopyasının çözümlenmesi, bu modülün yasal mı yoksa kötü amaçlı mı olduğu konusunda daha fazla bilgi sağlayabilir. Analiz şunları içerebilir:

- İlgili dosyanın yasal bir yazılım paketinin bir parçası olarak gönderildiğini doğrulayın
- Dosyanın dijital imzasını doğrulayın 
- Dosya üzerinde virüsten koruma taraması çalıştırın

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki ek alanları sağlar:

- DETAILS: Modülün meta verilerinin geçerli olup olmadığını ve modülün bir sistem yolundan yüklenip yüklenmediğini açıklar.
- NAME: Kendini gizleyen Windows modülünün adı
- PATH: Kendini gizleyen Windows modülünün yolu.

Bu uyarı, modülün PE üst bilgisinden “CHECKSUM” ve “TIMESTAMP” gibi belirli alanları da ayıklar ve görüntüler.  Bu alanlar yalnızca modülde varsa görüntülenir. Bu alanlarla ilgili ayrıntılı bilgi için bkz. [Microsoft PE ve COFF Belirtimi](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

İşte bu tür bir uyarı örneği:

![Gizleme uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig13-ga.png)

### Değiştirilen Sistem İkili Dosyası Bulundu 

Kötü amaçlı yazılım, verilere gizlice erişmek veya güvenliği ihlal edilmiş bir sistemde kendini gizleyerek kalıcı olmak için çekirdek sistem ikili dosyalarını değiştirebilir.  Bu uyarı, kilitlenme dökümü analizi tarafından bellekte veya diskte çekirdek Windows OS ikili dosyalarının değiştirildiğinin algılandığını gösterir. 

Yasal uygulama geliştiricileri, Sapmalar veya uygulama uyumluluğu gibi kötü amaçlı olmayan nedenlerle bellekte sistem modüllerini nadiren değiştirir. Azure Güvenlik Merkezi, kötü amaçlı modüllerle yasal olabilecek modüllerin birbirinden ayırt edilmesine yardımcı olmak için değiştirilen modülün bir şüpheli profile uygun olup olmadığını denetler.   Bu denetimin sonucu, uyarının önem derecesi, uyarı açıklaması ve uyarı düzeltme adımları tarafından gösterilir. 

Bu uyarı, yukarıdaki “Kabuk Kodu Bulundu” bölümünde açıklanan ortak alanlara ek olarak aşağıdaki ek alanları sağlar:

- MODULENAME: Değiştirilen sistem ikili dosyasının adı 
- MODULEVERSION: Değiştirilen sistem ikili dosyasının sürümü

İşte bu tür bir uyarı örneği:

![Değiştirilen ikili dosya uyarısı](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig14-ga.png)


## Ayrıca bkz.

Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi'nde Güvenlik Olayını İşleme](security-center-incident.md)
- [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md)
- [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Aug16_HO4-->


