---
title: Azure günlük analizi veri maliyetini yönetme | Microsoft Docs
description: Fiyatlandırma planı değiştirmek ve Azure günlük analizi çalışma alanınız için veri birimi ve bekletme ilkelerini yönetme hakkında bilgi edinin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: magoedte
ms.openlocfilehash: 0e4c4c9e950610526a29e02d70827a1279d9686a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Veri birimi ve günlük analizi bekletmeyi denetleyerek maliyet yönetme
Günlük analizi ölçek ve Destek toplama, dizin oluşturma ve herhangi bir kaynaktan veri günde oldukça büyük miktardaki kuruluşunuzda depolamak üzere tasarlanmış veya Azure'da dağıtılabilir.  Bu, kuruluşunuz için birincil bir sürücü olabilir, ancak maliyet verimliliği sonuçta temel sürücüsüdür. Kendi önemli bir günlük Analytisc çalışma maliyetini toplanan, veri biriminde yalnızca dayanmayan anlamak bu amaçla da seçilen plan bağlı olduğu ve ne kadar süreyle bağlı kaynaklarınızdan oluşturulan veri depolamak seçtiğiniz.  

Bu makalede nasıl proaktif olarak veri birimi ve Depolama Büyümesi izleyebilir ve bu ilişkili maliyetler denetlemek için sınırları tanımlamak inceleyin. 

Veri maliyetini aşağıdaki faktörlere bağlı olarak önemli olabilir: 

- Sistemler, altyapı bileşenlerini, bulut kaynakları, gelen topluyorsunuz vb. sayısı 
- İleti kuyrukları, günlükleri, olay, güvenlik ile ilgili veriler veya performans ölçümleri gibi kaynak tarafından oluşturulan veri türü 
- Bu kaynaklar tarafından oluşturulan ve çalışma alanı'na alınan verilerin hacmi 
- Çalışma alanında dönem veriler korunur  
- Etkin yönetim çözümleri sayısı, veri kaynağı ve toplama sıklığı 

> [!NOTE]
> Ne kadar veri topladığı tahmini sağladığından her çözümünü içeren belgelere bakın.   

Kullanıyorsanız *serbest* planı, verilerin 7 gün bekletme için sınırlı. İçin *tek başına* veya *Ödendi* katmanı, toplanan verileri kullanılabilir son 31 gün için. *Serbest* planına sahip 500 MB günlük alım sınır ve birim izin tutarlar tutarlı bir şekilde aşması fark ederseniz, bu sınırı aşan veri toplamak için ücretli bir plana çalışma alanınızı değiştirebilirsiniz. 

> [!NOTE]
> Ücretli katmanı için uzun bir bekletme dönemi seçmek seçerseniz ücretleri uygulanır. Herhangi bir zamanda ve fiyatlandırma hakkında daha fazla bilgi için plan türünü değiştirmek, bkz: [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/log-analytics/). 

Veri hacmi sınırlı ve yardımcı iki yolla maliyetinizi denetlemek, günlük sınır ve veri bekletme bunlar.  

## <a name="review-estimated-cost"></a>Tahmini maliyet gözden geçirin
Maliyetler ne olduğunu anlamak kolaydır, büyük olasılıkla günlük analizi yaptığı son kullanım düzenlerini esas alarak.  Bunu yapmak için aşağıdaki adımları gerçekleştirin.  

1. [Azure portal](http://portal.azure.com) oturum açın. 
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. Günlük analizi abonelikleri bölmesinde çalışma alanınızı seçin ve ardından **kullanım ve tahmini maliyetleri** sol bölmesinden.<br><br> ![Kullanım ve tahmini maliyetleri sayfası](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Buradan, ay için veri biriminiz gözden geçirebilirsiniz. Alınan ve günlük analizi çalışma alanınızda korunan tüm verileri içerir.  Tıklatın **kullanım ayrıntılarını** veri birimi eğilimlerini kaynak, bilgisayarlar ve sunumu bilgilerle kullanım panoyu görüntülemek için sayfanın en üstünden. Görüntülemek ve günlük bir sınır ayarlamak için ya da bekletme süresini değiştirmek için tıklatın **veri birimi Yönetimi**.
 
Günlük analizi ücretler Azure faturanızı eklenir. Azure faturalama bölümü altında Azure portalının ya da buna fatura ayrıntılarını görebilirsiniz [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Günlük sınır
Azure portalı ve, günlük analizi çalışma alanı oluşturma seçtiğinizde *serbest* planı gün başına 500 MB ayarlanır. Bir fiyatlandırma planları için bir sınır yoktur. Günlük sınır yapılandırmak ve günlük alımı için çalışma alanınızda sınırlamak ancak amacınız günlük sınırına olmamalıdır dikkatli kullanın.  Aksi takdirde, diğer Azure Hizmetleri ve çözümler, işlevselliği güncel verilerin çalışma alanında kullanılabilir olmasıyla bağımlı etkileyebilir gün geri kalanı için verileri kaybedersiniz.  Sonuç olarak, BT Hizmetleri destekleyen kaynak sistem durumu koşullarını etkilenen zaman inceleyin ve alma becerinizi uyarır.  Günlük sınır veri birimi beklenmeyen artış yönetilen kaynaklarınızdan yönetmek ve sınırınızı içinde veya planlanmamış ücretleri çalışma alanınız için yalnızca sınırlama getirmek istediğinizde kalmak için bir yol olarak kullanılmak üzere tasarlanmıştır.  

Günlük sınıra ulaşıldığında Faturalanabilir veri türleri koleksiyonunu gün geri kalanı için durdurur. Seçilen günlük analizi çalışma alanı için sayfanın üst arasında bir uyarı başlığı görüntülenir ve bir işlemi olay gönderilir *işlemi* altında tablo **LogManagement** kategorisi. Veri toplama sürdürür altında sıfırlama süresine tanımlandıktan sonra *konumundaki günlük sınır ayarlanacak*. Bu işlemi olaya göre günlük veri sınırına ulaşıldığında bildirmek için yapılandırılmış bir uyarı kuralı tanımlama öneririz. 

### <a name="identify-what-daily-data-limit-to-define"></a>Tanımlamak için hangi günlük veri sınırı tanımlayın 
Gözden geçirme [günlük analizi kullanımını ve tahmini maliyetleri](log-analytics-usage.md) veri alım eğilim ve tanımlamak için günlük birimi cap ne olduğunu anlamak için. Sınıra ulaşıldıktan sonra kaynaklarınızı izlemek açamazsınız bu yana, dikkatle düşünülmelidir. 

### <a name="manage-the-maximum-daily-data-volume"></a>Maksimum günlük veri hacmi yönetme 
Aşağıdaki adımlarda günlük analizi günde alma veri hacmi yönetmek için bir sınır yapılandırma konusunda açıklanmıştır.  

1. Alanınızdan, seçin **kullanım ve tahmini maliyetleri** sol bölmeden.
2. Üzerinde **kullanım ve tahmini maliyetleri** sayfasında seçilen çalışma alanı için **veri birimi Yönetimi** sayfasının üstten. 
5. Günlük sınır olan **OFF** tıklatın – varsayılan olarak **ON** için etkinleştirmek ve veri birimi sınırı GB/gün ayarlayın.<br><br> ![Günlük analizi veri sınırı yapılandırın](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Sınırına olduğunda uyar
Veri sınırı eşiğine ulaşıldığında biz görsel bir ipucu Azure portalında sunarken, bu davranış mutlaka Acil dikkat gerektiren işletimsel sorunlar yönetme hizalayın değil.  Bir uyarı bildirimi almak için Azure İzleyicisi'nde yeni bir uyarı kuralı oluşturabilirsiniz.  Daha fazla bilgi için bkz: [oluşturmak, uyarıları görüntülemek ve yönetmek nasıl](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).      

Başlamanıza yardımcı olmak için uyarı için önerilen ayarları şunlardır:

* Hedef: günlük analizi kaynak seçin
* Ölçüt: 
   * Sinyal adı: özel günlük arama
   * Arama sorgusu: işlemi | Ayrıntı 'OverQuota' sahip olduğu
   * Temel: sonuç sayısı
   * Koşul: Büyüktür
   * Eşik: 0
   * Dönem: 5 (dakika)
   * Sıklık: 5 (dakika)
* Uyarı kuralı adı: Günlük veri sınırına ulaşıldı
* Önem derecesi: Uyarı (önem düzeyi 1)

Uyarı tanımlanır ve sınıra ulaşıldıktan sonra bir uyarı tetiklenir ve eylem grubunda tanımlanan yanıtlama gerçekleştirir. Ekibinizin e-posta ve metin iletileri üzerinden bildirim veya Web kancalarını, Otomasyon runbook'ları kullanarak eylemleri otomatikleştirmek veya [bir dış ITSM çözümü ile tümleştirme](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Veri saklama süresi değiştirme 
Aşağıdaki adımlarda ne kadar günlük verileri alanınızdaki tarafından tutuluyor yapılandırma konusunda açıklanmıştır.
 
1. Alanınızdan, seçin **kullanım ve tahmini maliyetleri** sol bölmeden.
2. Üzerinde **kullanım ve tahmini maliyetleri** sayfasında, **veri birimi Yönetimi** sayfasının üstten.
5. Bölmesinde, artırın veya gün sayısını azaltın ve ardından kaydırıcıyı **Tamam**.  Kullanıyorsanız *ücretsiz* katmanı, veri saklama süresi değiştirmek mümkün olmayacak ve bu ayarı denetlemek için ücretli katmanına yükseltme yapmanız gerekir.<br><br> ![Çalışma alanı veri saklama ayarını değiştirme](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Sorun giderme
**Soru**: nasıl giderebilirim günlük analizi artık veri toplama varsa? 
**Yanıt**: üzerinde ücretsiz fiyatlandırma katmanı ve bir günde birden fazla 500 MB veri göndermiş, gün geri kalanı için veri toplamayı durdurur. Günlük sınıra ulaşması günlük analizi veri toplamayı durdurur ya da veri eksik gibi görünüyor yaygın bir nedenidir.  
Günlük analizi veri toplamayı başlatır ve durdurur işlemi türündeki bir olay oluşturur.  
Aramada, günlük sınıra ulaşması ve veriler eksik, denetlemek için aşağıdaki sorguyu çalıştırın: işlemi | Burada OperationCategory 'Veri toplama durumu' ==   
Ne zaman OperationStatus uyarı bir veri toplamayı durdurur. Veri toplama başladığında OperationStatus başarılı oldu.  
Aşağıdaki tabloda veri toplamayı durdurur nedenleri açıklanmaktadır ve veri toplama sürdürmek için önerilen eylem:  

|Neden koleksiyonu durdurur| Çözüm| 
|-----------------------|---------|
|Boş veri günlük sınırına<sup>1</sup>|Koleksiyonun otomatik olarak yeniden başlatmak sonraki güne kadar bekleyin veya Ücretli bir fiyatlandırma katmanı değiştirin.|
|Veri Birimi Yönetimi'nde tanımladığınız günlük sınırına ulaşıldı|Koleksiyonun otomatik olarak yeniden başlatmak sonraki güne kadar bekleyin veya açıklanan günlük veri birimi sınırını artırın [maksimum günlük veri hacmi yönetme](#manage-the-maximum-daily-volume)|
|Azure aboneliği nedeniyle askıya alınmış durumda olduğundan:<br> Ücretsiz deneme sürümü sona erdi<br> Azure geçiş süresi<br> Aylık harcama sınırı (örneğin bir MSDN veya Visual Studio abonelikte) ulaşıldı|Ücretli bir aboneliğe Dönüştür<br> Sınırı kaldırın veya sınırı sıfırlar kadar bekleyin|

<sup>1</sup> çalışma alanınızı ücretsiz fiyatlandırma katmanını ise, hizmete 500 MB günde veri göndermeye sınırlı. Günlük sınıra ulaştığınızda, sonraki güne kadar veri toplamayı durdurur. Veri toplama durdurulduğunda gönderilen veriler dizinli değil ve arama amacıyla kullanılamıyor. Veri toplama devam ettiğinde işleme yalnızca yeni gönderilen verileri için oluşur. 

Günlük analizi UTC saati kullanır. Aynı anda veri alma tüm çalışma alanları tutulabilir başlangıç önlemek için çalışma alanları arasında sıfırlama süresine değişir. Çalışma Günlük sınıra ulaştığında, sıfırlama süresine tanımlanan sonra işleme devam ettirir **konumundaki günlük sınır ayarlanacak**.<br><br> ![Günlük analizi UTC saat dilimi sınırla](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Soru**: nasıl bildirim veri toplama durduğunda? 
**Yanıt**: açıklanan adımları kullanın *oluşturma günlük veri cap* adımları kullanın açıklanan adımları izleyin ve veri toplama durduğunda bildirim almak için uyarı ekleme eylemleri uyarı kuralları için bir e-posta, Web kancası veya runbook yapılandırmak Eylem için uyarı kuralı. 

## <a name="next-steps"></a>Sonraki adımlar  

Hangi kaynakları ve kullanım ve maliyet yönetmenize yardımcı olmak için gönderilen veri farklı türdeki gönderiyorsunuz ne kadar veri toplanır belirlemek için bkz: [günlük analizi veri kullanımı analiz](log-analytics-usage.md).