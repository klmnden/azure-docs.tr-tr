---
title: Azure Log analytics'te verileri maliyetini yönetme | Microsoft Docs
description: Fiyatlandırma planı değiştirmek ve Azure Log Analytics çalışma alanınız için veri hacmi ve saklama ilkesini yönetme hakkında bilgi edinin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: be64d299ac61a47dd3c44ee2e422abd09785189e
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "53002727"
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Veri hacmi ve saklama Log analytics'te kontrol ederek maliyet yönetme

> [!NOTE]
> Bu makalede, maliyetlerinizi Log analytics'te veri saklama süresi ayarlayarak denetlemek nasıl açıklar.  İlgili bilgiler için aşağıdaki makalelere göz atın.
> - [Log analytics'te veri kullanımını çözümleme](manage-cost-storage.md) analiz ve veri kullanımınızı uyarı açıklar.
> - [Kullanım ve Tahmini maliyetler izleme](../../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) çoklu Azure İzleme özelliklerini farklı fiyatlandırma modelleri için tahmini maliyetleri ve kullanım görüntülemeyi açıklar. Ayrıca, uygulamanızın fiyatlandırma modelinin değiştirilmesi nasıl açıklar.

Log Analytics ölçek ve Destek toplama, dizin oluşturma ve kuruluşunuzda oldukça büyük miktardaki verileri günde herhangi bir kaynaktan depolamak üzere tasarlanmış veya Azure'da dağıtılır.  Bu, kuruluşunuz için birincil bir sürücü olabilir, ancak hesaplıdır sonuçta temel alınan sürücüsüdür. Önemli bir Log Analytics çalışma alanı maliyetini yalnızca, toplanan veri hacmine dayalı değilse anlamak bu amaçla için Ayrıca seçilen plan üzerinde bağımlı olduğu ve ne kadar süreyle, bağlı kaynaklardan oluşturulan verileri depolamak seçtiğiniz.  

Bu makalede nasıl proaktif bir şekilde veri hacmi ve depolama büyüme izleyebilir ve bu ilişkili maliyetleri denetleyebilirsiniz sınırlarını tanımlamak inceleyin. 

Veri maliyetine aşağıdaki faktörlere bağlı olarak önemli ölçüde olabilir: 

- Sistemleri, altyapı bileşenlerini, bulut kaynakları, toplama vb. sayısı 
- İleti kuyrukları, günlükleri, olaylar, güvenlikle ilgili verileri veya performans ölçümlerini gibi bir kaynak tarafından oluşturulan veri türü 
- Bu kaynaklar tarafından oluşturulan ve çalışma alanı için alınan veri hacmi 
- Dönem veri çalışma alanında tutulur.  
- Etkin yönetim çözümlerinin sayısını, veri kaynağı ve toplama sıklığı 

> [!NOTE]
> Bir tahmin ne kadar veri topladığı sağladığı gibi her çözüm için belgelerine başvurun.   

Bir kurumsal anlaşması olan müşteriler 1 Temmuz 2018'den önce imzalanmış veya zaten bir Log Analytics çalışma alanı bir abonelikte oluşturan kişi, yine de erişiminiz *ücretsiz* planı. Mevcut bir EA kaydına için aboneliğinize bağlı değildir, *ücretsiz* katmanı kullanılabilir değil, bir çalışma alanı 2 Nisan 2018'den sonra yeni bir abonelik oluşturduğunuzda.  7 gün bekletme için sınırlı veri *ücretsiz* katmanı.  İçin *tek başına* veya *Ücretli* katmanı, toplanan veriler kullanılabilir son 31 gün için. *Ücretsiz* katmanda 500 MB günlük alımı sınırı vardır ve tutarlı bir şekilde toplu izin miktarları aşan fark ederseniz, bu sınırı aşan miktarda veri toplamak için ücretli bir plana çalışma alanınızı değiştirebilirsiniz. 

> [!NOTE]
> Bir daha uzun bekletme süreleri Ücretli katmanı seçmek seçerseniz ücretleri uygulanır. Plan türünüzü istediğiniz zaman ve fiyatlandırma hakkında daha fazla bilgi için değiştirme, bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/). 

Veri hacmi sınırlı ve Yardım iki şekilde maliyetlerinizi denetim vardır, bu günlük üst sınırını ve veri saklama.  

## <a name="review-estimated-cost"></a>Tahmini maliyet gözden geçirin
Son kullanım modellerini temel maliyetleri ne olduğunu anlamak kolay, büyük olasılıkla günlük analizi sağlar.  Bunu yapmak için aşağıdaki adımları gerçekleştirin.  

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure portal](media/manage-cost-storage/azure-portal-01.png)<br><br>  
3. Log Analytics abonelikleri bölmesinde, çalışma alanınızı seçin ve ardından **kullanım ve Tahmini maliyetler** sol bölmesinden.<br><br> ![Kullanım ve Tahmini maliyetler sayfasını](media/manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Buradan veri hacminiz ay için gözden geçirebilirsiniz. Bu, Log Analytics çalışma alanınızda saklanır ve alınan tüm verileri içerir.  Tıklayın **kullanım ayrıntılarını** veri kaynağı, bilgisayarlar ve teklifi volume Trend bilgilerle kullanım panosunu görüntülemek için sayfanın üst. Görüntüleme ve günlük üst limit ayarlayabilir veya bekletme süresini değiştirmek için tıklayın **veri hacmi Yönetimi**.
 
Log Analytics ücretleri Azure faturanızı eklenir. Azure faturalandırma bölümünde Azure portal'ın veya fatura ayrıntılarını görebilirsiniz [Azure fatura portalı](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Günlük sınır
Azure portalı ve, Log Analytics çalışma alanı oluşturma seçtiğinizde *ücretsiz* planı ayarlanır bir günlük sınır 500 MB. Diğer fiyatlandırma planları için bir sınır yoktur. Günlük üst sınır yapılandırın ve çalışma alanınız için günlük alımı sınırlayabilirsiniz, ancak hedef günlük limite ulaşılmadan olmamalıdır dikkatli kullanın.  Aksi takdirde, diğer Azure Hizmetleri ve çözümleri olan işlevselliği güncel verileri çalışma alanında kullanılabilir olan bağımlı etkileyebilir günün geri kalanında verileri kaybedersiniz.  Sonuç olarak, BT Hizmetleri destekleyen kaynakların sistem durumu koşullarını etkilendiğinde yeteneğinizi inceleyin ve almak için sizi uyarır.  Günlük üst sınırınızı içinde veya basit çalışma alanınız için planlanmayan ücretleri sınırlandırmak istediğinizde, yönetilen kaynaklardaki veri hacmindeki beklenmeyen artış yönetmek ve için bir yol olarak kullanılmak üzere tasarlanmıştır.  

Günlük sınıra ulaşıldığında, Faturalanabilir veri türlerinin günlük geri kalanı için durdurur. Seçili Log Analytics çalışma alanı için sayfanın üstündeki bir uyarı başlık görünür ve bir işlemi olay gönderilir *işlemi* altında tablo **LogManagement** kategorisi. Veri toplama sürdürür altında sıfırlama zaman tanımlandıktan sonra *günlük sınır ayarlanacak*. Günlük veri sınırına ulaşıldığında bildirmek için yapılandırılmış. Bu işlem olayı temel alan bir uyarı kuralı tanımlayan öneririz. 

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlamak için hangi günlük veri sınırınızın tanımlayın 
Gözden geçirme [Log Analytics kullanımı ve Tahmini maliyetler](../../log-analytics/log-analytics-usage.md) veri alma eğilimi ve tanımlamak için günlük hacim üst sınırını ne olduğunu anlamak için. Sınıra ulaşıldıktan sonra kaynaklarınızı izleyin mümkün olmayacaktır beri dikkatlice değerlendirilmelidir. 

### <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmini yönetme 
Log Analytics günlük içe alma veri hacmi yönetmek için bir sınır yapılandırma aşağıdaki adımları açıklanmaktadır.  

1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. Üzerinde **kullanım ve Tahmini maliyetler** sayfasında seçilen çalışma alanı için **veri hacmi Yönetimi** sayfanın üst. 
5. Günlük üst sınır olan **OFF** varsayılan olarak – tıklayın **ON** etkinleştirin ve ardından veri birimi sınırı GB/gün.<br><br> ![Log Analytics'e veri sınırını yapılandırın](media/manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Sınırına olduğunda uyar
Veri sınırı eşiğine karşılandığında size görsel bir ipucu Azure portalında mevcut olsa da bu davranış mutlaka Acil dikkat gerektiren işletimsel sorunları nasıl yönettiğiniz için hizalayın değil.  Bir uyarı bildirimine almak, Azure İzleyici'de yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi için bkz. [oluşturun, görüntüleyin ve Uyarıları yönetmek nasıl](../../monitoring-and-diagnostics/alert-metric.md).      

Başlamanıza yardımcı olmak için uyarı için önerilen ayarları şunlardır:

* Hedef: Log Analytics kaynağınızı seçin.
* Ölçütleri: 
   * Sinyal adı: özel günlük araması
   * Arama sorgusu: işlemi | Ayrıntı 'Altındaysa' sahip olduğu
   * Temel: sonuç sayısı
   * Koşul: Büyüktür
   * Eşik: 0
   * Dönem: 5 (dakika)
   * Sıklık: 5 (dakika)
* Uyarı kuralı adı: Günlük veri sınırına ulaşıldı
* Önem derecesi: Uyarı (önem derecesi 1)

Uyarı tanımlanır ve sınıra ulaşıldıktan sonra bir uyarı tetiklenir ve eylem grubunda tanımlanan yanıt gerçekleştirir. Aracılığıyla e-posta veya metin iletileriyle ekibinizi bilgilendirin veya Web kancaları, Otomasyon runbook'ları kullanarak işlemleri otomatik hale getirmek veya [dış bir ITSM çözümüyle tümleştirme](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Veri saklama süresini değiştirme 
Aşağıdaki adımları ne kadar günlük verileri çalışma alanınızda tarafından tutulur yapılandırma açıklanmaktadır.
 
1. Çalışma alanınızın sayfasında, soldaki bölmeden **Kullanım ve tahmini maliyetler**’i seçin.
2. **Kullanım ve tahmini maliyetler** sayfasının üst kısmındaki **Veri hacmi yönetimi**'ni seçin.
5. Bölmede artırın veya gün sayısını azaltın, ardından kaydırıcıyı **Tamam**.  Kullanıyorsanız *ücretsiz* katmanı, veri bekletme süresini değiştirmek mümkün olmayacaktır ve bu ayarı denetlemek için ücretli katmana yükseltmeniz gerekir.<br><br> ![Çalışma alanı veri saklama ayarını değiştirme](media/manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Sorun giderme
**Soru**: sorunlarını nasıl giderebilirim, Log Analytics, artık veri topluyor? 
**Yanıt**: üzerinde ücretsiz fiyatlandırma katmanı ve bir günde 500 MB veri göndermiş, günün geri kalanı için veri toplamayı durdurur. Günlük sınıra ulaşılması Log Analytics Veri toplamayı durdurur ya da veri eksik gibi görünüyor yaygın bir nedenidir.  
Log Analytics'e veri toplamayı başlatır ve durdurur ' % s'olay türü işlemi oluşturur.  
Aramada, günlük sınırınıza ulaşmanız ve eksik veriler olmadığını denetlemek için aşağıdaki sorguyu çalıştırın: işlemi | Burada OperationCategory 'Veri toplama durumu' ==   
Ne zaman OperationStatus uyarı bir veri toplamayı durdurur. Veri toplama başladığında OperationStatus başarılı oldu.  
Aşağıdaki tabloda veri toplamayı durdurur nedenleri açıklanır ve veri koleksiyonu devam bir önerilen eylem:  

|Neden koleksiyonu durdurur| Çözüm| 
|-----------------------|---------|
|Ücretsiz veri günlük sınırına<sup>1</sup>|Koleksiyon otomatik olarak yeniden başlatmak için sonraki güne kadar bekleyin veya Ücretli fiyatlandırma katmanı olarak değiştirme.|
|Veri Birim Yönetimi'nde tanımlanan günlük sınırına ulaşıldı|Koleksiyon otomatik olarak yeniden başlatmak için sonraki güne kadar bekleyin veya açıklanan günlük veri birimi sınırı artırmak [en fazla günlük veri hacmini yönetme](#manage-the-maximum-daily-volume)|
|Azure aboneliği askıya alınma durumuna nedeniyle oluşturulur.<br> Ücretsiz deneme sürümü sona erdi<br> Azure pass süresi doldu<br> Aylık harcama sınırına (örneğin bir MSDN veya Visual Studio abonelik üzerinde)|Ücretli aboneliğe dönüştürme<br> Sınırı kaldırın veya sınır sıfırlar kadar bekleyin|

<sup>1</sup> çalışma alanınız ücretsiz fiyatlandırma katmanında ise, 500 MB günlük veri hizmetine gönderebilirsiniz. Günlük sınıra ulaştığında, sonraki güne kadar veri toplamayı durdurur. Veri toplama durdurulduğu sırada gönderilen veriler, dizinlenmeyen ve arama için kullanılabilir değil. Veri koleksiyonu devam ettiğinde, yalnızca yeni gönderilen veriler için işleme gerçekleşir. 

Log Analytics'e UTC saatini kullanır. Sıfırlama zaman, veri alma aynı anda tüm tavan çalışma başlangıç önlemek için çalışma alanları arasında değişir. Çalışma alanı günlük sınırına ulaşırsa, sıfırlama zaman içinde tanımlanan sonra işleme sürdürür **günlük sınır ayarlanacak**.<br><br> ![UTC saat dilimine log Analytics'e sınırla](media/manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Soru**: nasıl bildirim veri toplama durduğunda? 
**Yanıt**: açıklanan adımları kullanın *oluşturma günlük veri üst sınırında* ekleme Uyarı kurallarına Eylemler yapılandırmanız bir e-posta, Web kancasını veya runbook veri toplamayı durdurur bildirilmesini sağlamak için uyarı ve izleme adımları bölümünde açıklanan adımları kullanın Uyarı kuralı için eylem. 

## <a name="next-steps"></a>Sonraki adımlar  

Hangi kaynakları ve farklı türde kullanım ve maliyeti yönetmenize yardımcı olmak için gönderilen veri gönderme ne kadar veri toplandığı belirlemek için bkz: [Log analytics'te veri kullanımını çözümleme](../../log-analytics/log-analytics-usage.md).
