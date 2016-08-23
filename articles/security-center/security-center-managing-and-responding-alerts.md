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
   ms.date="07/21/2016"
   ms.author="yurid"/>

# Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi’ni kullanmanıza yardımcı olur.

## Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için Azure kaynaklarınızdan, ağınızdan ve güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

> [AZURE.NOTE] Güvenlik Merkezi algılama özelliklerinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Algılama Özellikleri](security-center-detection-capabilities.md) konusunu okuyun.

Microsoft güvenlik araştırmacıları, dünya genelinde sürekli ortaya çıkan tehditleri çözümler (tüketiciler, kurumsal ürünler ve çevrimiçi hizmetlerinde görülen yeni saldırı desenleri ve eğilimleri dahil). Sonuç olarak Güvenlik Merkezi, yeni güvenlik açıkları ve açıklardan yararlanma keşfedildiğinde kendi algılama algoritmalarını güncelleştirebilir. Böylece müşterilerin gelişen tehditlere ayak uydurmalarına yardımcı olur. Güvenlik Merkezi'nin algılayabileceği bazı tehdit türlerine şunlar örnektir:

- **Ağ verileri üzerinden deneme yanılmayı algılama**: Uygulamalarınıza ilişkin genel ağ trafiği desenlerini anlayan Machine Learning modellerini kullanır, geçerli kullanıcılar yerine hatalı aktörlerin yürüttüğü erişim girişimlerini daha verimli bir şekilde algılamayı etkinleştirir.
- **Uç nokta verileri üzerinden deneme yanılmayı algılama**: Makine günlüklerinin çözümlemesini temel alarak başarılı ve başarısız denemeler arasında ayrım sağlar.
- **Kötü amaçlı IP'lerle iletişim kuran VM'ler**: Ağ trafiğini Microsoft küresel tehdit zekasıyla karşılaştırır, Komuta ve Kontrol (C & C) sunucularıyla iletişim kuran, gizliliği tehlikeye giren makineleri bulur (veya tam tersi yönde işlem yapar).
- **Gizliliği tehlikeye giren VM'ler**: Makine günlükleri ve diğer sinyaller arasındaki bağıntının davranışsal analizine dayanarak, büyük olasılıkla makinenin gizliliğinin tehlikeye girmesi ve istismarı sonucu gerçekleşmiş anormal olayları tanımlar.

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


## Ayrıca bkz.

Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md)
- [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.



<!--HONumber=Aug16_HO1-->


