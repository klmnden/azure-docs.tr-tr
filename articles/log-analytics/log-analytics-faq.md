---
title: Günlük analizi ile ilgili SSS | Microsoft Docs
description: Azure günlük analizi hizmeti ile ilgili sık sorulan soruların yanıtları.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: 22da58df653b31c46145ebbbd1f6f6a26b0e9f29
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="log-analytics-faq"></a>Log Analytics SSS
Bu Microsoft FAQ Microsoft Azure Log Analytics hakkında sık sorulan soruların listesidir. Günlük analizi hakkında ek herhangi bir sorunuz varsa, Git [tartışma Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="general"></a>Genel

### <a name="q-does-log-analytics-use-the-same-agent-as-azure-security-center"></a>Q. Günlük analizi aynı aracı Azure Güvenlik Merkezi kullanıyor mu?

A. Erken Haziran 2017 toplamak ve verilerini depolamak için Microsoft Monitoring Agent'ı kullanarak Azure Güvenlik Merkezi başlamıştır. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi platformu geçiş ile ilgili SSS](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-the-ad-and-sql-assessment-solutions"></a>Q. Hangi denetimleri AD tarafından yapılır ve SQL değerlendirmesi çözümleri?

A. Aşağıdaki sorgu, şu anda gerçekleştirilen tüm denetimler açıklamasını gösterir:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Sonuçları sonra daha fazla inceleme için Excel'e aktarılabilir.

### <a name="q-why-do-i-see-something-different-than-oms-in-the-system-center-operations-manager-console"></a>System Center Operations Manager konsolunda OMS farklı bir şey neden görüyor musunuz?

A: hangi güncelleştirme toplaması Operations, bulunduğunuz Yöneticisi'nin üzerinde bağlı olarak, bir düğüm için görebilirsiniz *System Center Advisor*, *operasyonel Öngörüler*, veya *günlük analizi*.

Metin dizesini güncelleştirmeye *OMS* el ile içeri aktarılacak gereken bir yönetim paketinde bulunur. Geçerli metin ve işlevsellik görmek için son System Center Operations Manager güncelleştirme toplaması KB makalesinde yönergeleri izleyin ve konsolu yenileyin.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>S: günlük analizi şirket içi sürümü var mı?

A: No. Günlük analizi, işler ve büyük miktarlarda veri depolayan bir ölçeklenebilir bulut hizmetidir. 

### <a name="q-how-do-i-troubleshoot-if-log-analytics-is-no-longer-collecting-data"></a>Q. Günlük analizi artık veri toplama, nasıl giderebilirim?

Y: için bir abonelik ve açık olan 2 Nisan 2018 önce oluşturulan bir çalışma alanı *serbest* fiyatlandırma katmanı, bir gün içinde birden fazla 500 MB veri gönderirse, günlük geri kalanı için veri toplamayı durdurur. Günlük sınıra ulaşması günlük analizi veri toplamayı durdurur ya da veri eksik gibi görünüyor yaygın bir nedenidir.  

Günlük analizi oluşturur olay türü *sinyal* ve veri toplama vermediğinde belirlemek için kullanılabilir. 

Aramada, günlük sınıra ulaşması ve eksik veri varsa denetlemek için aşağıdaki sorguyu çalıştırın: `Heartbeat | summarize max(TimeGenerated)`

Belirli bir bilgisayarı denetlemek için aşağıdaki sorguyu çalıştırın: `Heartbeat | where Computer=="contosovm" | summarize max(TimeGenerated)`

Veri toplama durduğunda seçili zaman aralığına bağlı döndürülen kayıt görmezsiniz.   

Aşağıdaki tabloda veri toplamayı durdurur nedenleri açıklanmaktadır ve veri toplama sürdürmek için önerilen eylem:

| Neden veri toplamayı durdurur                       | Veri toplama sürdürmek için |
| -------------------------------------------------- | ----------------  |
| Boş veri sınırına<sup>1</sup>       | Koleksiyonun otomatik olarak yeniden başlatmak aşağıdaki aya kadar bekleyin veya<br> Ücretli bir fiyatlandırma katmanı değiştirme |
| Azure aboneliği nedeniyle askıya alınmış durumda olduğundan: <br> Ücretsiz deneme sürümü sona erdi <br> Azure geçiş süresi <br> Aylık harcama sınırı (örneğin bir MSDN veya Visual Studio abonelikte) ulaşıldı                          | Ücretli bir aboneliğe Dönüştür <br> Ücretli bir aboneliğe Dönüştür <br> Sınırı kaldırın veya sınırı sıfırlar kadar bekleyin |

<sup>1</sup> çalışma alanınızı açıksa *serbest* fiyatlandırma katmanı, hizmete 500 MB günde veri göndermeye sınırlı. Günlük sınıra ulaştığınızda, sonraki güne kadar veri toplamayı durdurur. Veri toplama durdurulduğunda gönderilen veriler dizinli değil ve arama amacıyla kullanılamıyor. Veri toplama çıktığında işleme gönderilen yalnızca yeni verileri oluşur. 

Günlük analizi UTC saati kullanır ve her gün UTC gece yarısı başlar. Çalışma alanı günlük sınıra ulaşırsa, ilk günün sonraki UTC sırasında işlemeyi sürdürür.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>Q. Veri toplama durduğunda nasıl bildirim?

Y: açıklanan adımları kullanın [bir uyarı kuralı oluştur](log-analytics-alerts-creating.md#create-an-alert-rule) veri toplama durduğunda bildirilmesi.

Veri toplama durduğunda için uyarı oluşturulurken ayarlayın:
- **Ad** için *veri toplama durdu*
- **Önem derecesi**: *Uyarı*
- **Arama sorgusu**: `Heartbeat | summarize LastCall = max(TimeGenerated) by Computer | where LastCall < ago(15m)`
- **Zaman penceresi** için *30 dakika*.
- **Uyarı sıklığı** her *on* dakika.
- **Şuna bağlı olarak uyarı oluştur**: *sonuç sayısı*
- **Sonuç sayısı**: *Şundan büyüktür: 0*

Sorgu sonuçları döndürdüğünde yalnızca 15 dakikadan fazla bir süre için eksik sinyal varsa bu uyarı ateşlenir.  Uyarı kuralı olarak bir e-postayı, web kancasını veya runbook eylemini yapılandırmak için, [Uyarı kurallarına eylemler ekleme](log-analytics-alerts-actions.md) başlığı altında açıklanan adımları kullanın.

## <a name="configuration"></a>Yapılandırma
### <a name="q-can-i-change-the-name-of-the-tableblob-container-used-to-read-from-azure-diagnostics-wad"></a>Q. Azure tanılama (WAD) okumak için kullanılan tablo/blob kapsayıcısının adını değiştirebilir miyim?

A. Hayır, şu anda rasgele tablolar veya Azure depolama kapsayıcıları okuma mümkün değildir.

### <a name="q-what-ip-addresses-does-the-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-to-the-log-analytics-service"></a>Q. Hangi IP adresleri günlük analizi hizmeti kullanıyor? Duvarım yalnızca günlük analizi hizmetine trafiğine izin verdiğini nasıl emin olursunuz?

A. Günlük analizi hizmeti, Azure üzerinde oluşturulmuştur. Günlük analizi IP adresleri olan [Microsoft Azure veri merkezi IP aralıkları](http://www.microsoft.com/download/details.aspx?id=41653).

Hizmet dağıtımları yaptığınız gibi günlük analizi hizmeti gerçek IP adreslerini değiştirin. DNS adları, güvenlik duvarından geçmesine izin belgelenmiştir [sistem gereksinimleri](log-analytics-concept-hybrid.md#prerequisites).

### <a name="q-i-use-expressroute-for-connecting-to-azure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>Q. I ExpressRoute Azure'a bağlanmak için kullanın. My günlük analizi trafik ExpressRoute bağlantım kullanıyor mu?

A. ExpressRoute trafiği farklı türleri açıklanmaktadır [ExpressRoute belgeleri](../expressroute/expressroute-faqs.md#supported-services).

Günlük analizi için trafiği genel eşliği expressroute bağlantı hattı kullanır.

### <a name="q-is-there-a-simple-and-easy-way-to-move-an-existing-log-analytics-workspace-to-another-log-analytics-workspaceazure-subscription"></a>Q. Varolan bir günlük analizi çalışma alanını başka bir günlük analizi çalışma alanı/Azure aboneliği taşımak için basit ve kolay bir yolu var mı?

A. `Move-AzureRmResource` Cmdlet günlük analizi çalışma alanı ve aynı zamanda bir Otomasyon hesabı bir Azure aboneliğinden diğerine taşımanıza olanak tanır. Daha fazla bilgi için bkz: [taşıma AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Bu değişiklik ayrıca Azure portalında yapılabilir.

Verilerini bir günlük analizi çalışma alanından diğerine taşımak veya günlük analizi veri depolanan bölgeyi değiştirin.

### <a name="q-how-do-i-add-log-analytics-to-system-center-operations-manager"></a>S: System Center Operations Manager için nasıl günlük analizi eklensin mi?

A: en son güncelleştirme paketini güncelleştirme ve yönetim paketlerini içeri Operations Manager için günlük analizi bağlanmanıza olanak sağlar.

>[!NOTE]
>Günlük analizi Operations Manager bağlantısı, yalnızca System Center Operations Manager 2012 SP1 ve sonraki sürümleri için kullanılabilir.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-to-communicate-with-log-analytics"></a>S: bir aracı günlük analizi ile iletişim kuramayacak nasıl onaylayabilirsiniz?

Y: aracı OMS ile iletişim kurabildiğinden emin olun için şu adrese gidin: Denetim Masası, güvenlik ve ayarları, **Microsoft İzleme Aracısı**.

Altında **Azure günlük analizi (OMS)** sekmesinde, yeşil bir onay işareti için bakın. Yeşil onay işareti simgesi aracı Azure hizmetiyle iletişim kuramıyor olduğunu doğrular.

Sarı bir uyarı simgesi aracı günlük analizi ile iletişim sorunları yaşıyor anlamına gelir. Ortak nedenlerinden biri, Microsoft İzleme Aracısı Hizmeti durdu ' dir. Hizmet Denetimi Yöneticisi hizmeti yeniden başlatın.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>S: günlük analizi ile iletişim kurmasını bir aracı nasıl Durdur?

Y: System Center Operations Manager'da, bilgisayar OMS yönetilen bilgisayarlara listeden kaldırın. Operations Manager Aracısı'nın yapılandırması artık günlük analizi raporu güncelleştirir. Günlük analizi için doğrudan bağlı aracılar için bunları aracılığıyla iletişim kurmasını durdurabilirsiniz: Denetim Masası, güvenlik ve ayarları, **Microsoft İzleme Aracısı**.
Altında **Azure günlük analizi (OMS)**, listelenen tüm çalışma alanlarını kaldırın.

### <a name="q-why-am-i-getting-an-error-when-i-try-to-move-my-workspace-from-one-azure-subscription-to-another"></a>Çalışma Alanım bir Azure aboneliğinden diğerine taşımak çalıştığımda neden bir hata alıyorum?

A: Azure portalı kullanıyorsanız, yalnızca çalışma alanına taşımaya seçili olduğundan emin olun. Çalışma alanı taşındıktan sonra otomatik olarak taşır--çözümleri seçmeyin. 

Her iki Azure aboneliklerini izninizin olduğundan emin olun.

## <a name="agent-data"></a>Aracı verileri
### <a name="q-how-much-data-can-i-send-through-the-agent-to-log-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>Q. Ne kadar veri için günlük analizi ile aracıyı gönderebilirim? Yüksek miktarda verinin müşteri başına var mı?
A. Ücretsiz planı çalışma alanı bir günlük sınır 500 MB'lık ayarlar. Standart ve premium planları sınır karşıya veri miktarına sahip. Otomatik ölçek tanıtıcı birim kadar üzere tasarlanmış bir bulut hizmeti olarak günlük analizi günde terabayt olsa bile, müşteriden – geliyor.

Günlük analizi aracı, küçük bir yer olan emin olmak için tasarlanmıştır. Veri birimi etkinleştirmeniz çözümleri göre değişir. Veri birimi ile ilgili ayrıntılı bilgiler bulmak ve çözümde tarafından dökümünü görmek [kullanım](log-analytics-usage.md) sayfası.

Daha fazla bilgi için okuduğunuz bir [müşteri blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) OMS Aracısı'nın kaynak kullanımı (ayak izini) değerlendirdikten sonra bunların sonuçları gösteriliyor.

### <a name="q-how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-log-analytics"></a>Q. Ne kadar ağ bant genişliği için günlük analizi veri gönderirken, Microsoft Yönetim Aracısı (MMA) tarafından kullanılıyor?

A. Bant genişliği gönderilen veri miktarını üzerinde bir işlevdir. Ağ üzerinden gönderilen veri sıkıştırılır.

### <a name="q-how-much-data-is-sent-per-agent"></a>Q. Ne kadar veri her aracı gönderilir?

A. Her aracı için gönderilen veri miktarına bağlıdır:

* Etkinleştirdiğiniz çözümleri
* Günlükleri ve toplanmakta olan performans sayaçlarının sayısı
* Günlüklerine veri hacmi

Ücretsiz fiyatlandırma katmanı, birkaç yerleşik sunuculara ve ölçer tipik veri hacmi iyi bir yoludur. Genel kullanım gösterilir [kullanım](log-analytics-usage.md) sayfası.

WireData Aracısı'nı çalıştırabilir ve bilgisayarlar için ne kadar veri gönderilen görmek için aşağıdaki sorguyu kullanın:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük Analytics ile çalışmaya başlama](log-analytics-get-started.md) günlük analizi hakkında daha fazla bilgi ve dakika cinsinden başlamak ve çalıştırmak için.
