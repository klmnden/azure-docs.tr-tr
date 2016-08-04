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
   ms.date="04/22/2016"
   ms.author="yurid"/>

# Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama
Bu belge, güvenlik uyarılarını yönetmeniz ve yanıtlamanız için Azure Güvenlik Merkezi işlevlerini kullanmanıza yardımcı olur.

> [AZURE.NOTE] Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir.

## Azure Güvenlik Merkezi nedir?
 Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## Güvenlik uyarıları nedir?
Güvenlik Merkezi, gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için güvenlik duvarlarından, tümleşik kötü amaçlı yazılımdan koruma yazılımından, ağdan ve Azure kaynaklarınızdan günlük verilerini otomatik olarak toplar, çözümler ve tümleştirir. Tümleşik iş ortağı çözümlerinden gelen uyarılar dahil olmak üzere öncelikli güvenlik uyarıları listesi, hızlı araştırmanız gereken bilgiler ile saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Microsoft güvenlik araştırmacıları, dünya genelinde sürekli ortaya çıkan tehditleri çözümler (tüketiciler, kurumsal ürünler ve çevrimiçi hizmetlerinde görülen yeni saldırı desenleri ve eğilimleri dahil). Sonuç olarak Güvenlik Merkezi, yeni güvenlik açıkları ve açıklardan yararlanma keşfedildiğinde kendi algılama algoritmalarını güncelleştirebilir. Böylece müşterilerin gelişen tehditlere ayak uydurmalarına yardımcı olur. Güvenlik Merkezi'nin algılayabileceği bazı tehdit türlerine şunlar örnektir:

- **Ağ verileri üzerinden deneme yanılmayı algılama**: Uygulamalarınıza ilişkin genel ağ trafiği desenlerini anlayan Machine Learning modellerini kullanır, geçerli kullanıcılar yerine hatalı aktörlerin yürüttüğü erişim girişimlerini daha verimli bir şekilde algılamayı etkinleştirir.
- **Uç nokta verileri üzerinden deneme yanılmayı algılama**: Makine günlüklerinin çözümlemesini temel alarak başarılı ve başarısız denemeler arasında ayrım sağlar.
- **Kötü amaçlı IP'lerle iletişim kuran VM'ler**: Ağ trafiğini Microsoft küresel tehdit zekasıyla karşılaştırır, Komuta ve Kontrol (C & C) sunucularıyla iletişim kuran, gizliliği tehlikeye giren makineleri bulur (veya tam tersi yönde işlem yapar).
- **Gizliliği tehlikeye giren VM'ler**: Makine günlükleri ve diğer sinyaller arasındaki bağıntının davranışsal analizine dayanarak, büyük olasılıkla makinenin gizliliğinin tehlikeye girmesi ve istismarı sonucu gerçekleşmiş anormal olayları tanımlar.

## Güvenlik ilkelerini yönetme

**Güvenlik uyarıları** kutucuğuna bakarak mevcut uyarılarınızı gözden geçirebilirsiniz. Uyarılar hakkında daha fazla ayrıntıya ulaşmak için şu adımları izleyin:

1. Güvenlik Merkezi panosunda **Güvenlik uyarıları** kutucuğunu görürsünüz.

    ![Azure Güvenlik Merkezi'nde Güvenlik uyarıları kutucuğu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-new.png)

2.  Aşağıda gösterildiği gibi, uyarılar hakkında daha fazla ayrıntı içeren **Güvenlik uyarıları** dikey penceresini açmak için kutucuğa tıklayın.

    ![Azure Güvenlik Merkezi'nde Güvenlik uyarıları dikey penceresi](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-new.png)

Bu dikey pencerenin alt bölümünde her bir uyarı için ayrıntılar bulunur. Sıralamak için hangi sütuna göre sıralamak istediğinizi belirtin. Her sütunun tanımı aşağıda verilmiştir:

- **Uyarı**: Uyarının kısa bir açıklaması.
- **Sayı**: Belirtilmiş türün belirli bir günde algılanan tüm uyarılarının listesi.
- **Algılayan**: Uyarıyı tetiklemekten sorumlu hizmet.
- **Tarih**: Olayın gerçekleştiği tarih
- **Durum**: Bu uyarı için geçerli durum. Üç tür durum vardır:
    - **Etkin**: Güvenlik uyarısı algılandı.
    - **Kapatıldı**: Güvenlik uyarısı kullanıcı tarafından kapatıldı. Bu durum genellikle, araştırılan ancak azaltılan ya da gerçek bir saldırı olarak düşünülmeyen uyarılar için kullanılır.

- **Önem derecesi**: Yüksek, orta veya düşük önem düzeyi.

Tarihe, duruma ve önem derecesine göre uyarıları filtreleyebilirsiniz. Filtreleme uyarıları, güvenlik uyarıları gösterimi kapsamını daraltmanızın gerektiği senaryolar için faydalı olabilir. Örneğin, sistemde olası bir ihlali araştırdığınız için son 24 saatte oluşan güvenlik uyarılarını ele almak isteyebilirsiniz.

1. **Güvenlik Uyarıları** dikey penceresindeki **Filtreleme**'ye tıklayın. **Filtreleme** dikey penceresi açılır ve görmek istediğiniz tarih, durum ve önem derecesi değerlerini seçersiniz.

    ![Azure Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-new.png)

2.  Bir güvenlik uyarısını araştırdıktan sonra, uyarının ortamınız için hatalı bir pozitif sonuç olduğunu veya belirli bir kaynak için beklenen davranışı belirttiğini fark edebilirsiniz. Her durumda, bir güvenlik uyarısını uygulanamaz olarak belirlerseniz uyarıyı kapatabilir ve ardından görünümünüzden çıkarabilirsiniz. Bir güvenlik uyarısını kapatmanın iki yolu vardır. Uyarıya sağ tıklayın ve **Kapat**'ı seçin veya bir öğenin üzerine gelerek sağ tarafta görüntülenen üç noktaya tıklayın ve **Kapat**'a tıklayın. **Filtreleme**'ye tıklayıp **Kapatıldı**'yı seçerek kapatılan güvenlik uyarılarını görüntüleyebilirsiniz.

    ![Azure Güvenlik Merkezi'nde uyarıları filtreleme](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-new.png)

### Güvenlik uyarılarını yanıtlama
Uyarıyı tetikleyen olay(lar) ve saldırıyı düzeltmek için (varsa) hangi adımları atmanız gerektiği hakkında daha fazla bilgi edinmek için bir güvenlik uyarısı seçin. Güvenlik uyarıları, türe ve tarihe göre gruplandırılır. Güvenlik uyarısına tıklandığında gruplanan uyarıların listesini içeren dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarılarını yanıtlama](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig7.png)

Bu durumda, tetiklenen uyarılar şüpheli Uzak Masaüstü Protokolü (RDP) etkinliğine işaret eder. İlk sütun hangi kaynakların saldırıya uğradığını; ikinci sütun bu saldırının algılandığı zamanı, üçüncü sütun uyarının durumunu; dördüncü sütun ise saldırının önem derecesini gösterir.  Bu bilgileri gözden geçirdikten sonra, saldırıya uğrayan kaynağa tıklayın. Aşağıdaki örnekte gösterildiği gibi bundan sonra yapmanız gerekenler hakkında daha fazla öneri içeren yeni bir dikey pencere açılır.

![Azure Güvenlik Merkezi'nde güvenlik uyarıları hakkında ne yapılacağına ilişkin öneriler](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig8-1.png)

Bu dikey pencerenin **Uyarı** alanında bu olay hakkında daha fazla ayrıntı bulabilirsiniz. Bu ek ayrıntılar, güvenlik uyarısını neyin tetiklediği, hedef kaynak, uygulanabilirse kaynak IP adresi ve düzeltmeye ilişkin öneriler hakkında öngörüler sunar.  Windows güvenlik olayı günlüklerinin tümü IP adresi içermediğinden, bazı durumlarda kaynak IP adresi boştur (yoktur).

> [AZURE.NOTE] Güvenlik Merkezi tarafından önerilen düzeltme, güvenlik uyarısına göre farklılık gösterir. Bazı durumlarda, önerilen düzeltmeyi uygulamak için diğer Azure işlevlerini kullanmak zorunda kalabilirsiniz. Örneğin, bu saldırıya ilişkin düzeltme, [ağ ACL'si](../virtual-network/virtual-networks-acl.md) veya [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) kuralı kullanarak bu saldırıyı gerçekleştiren IP adresini kara listeye almaktır.


## Sonraki adımlar
Bu belgede, Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

- [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
- [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz



<!----HONumber=Jun16_HO2-->


