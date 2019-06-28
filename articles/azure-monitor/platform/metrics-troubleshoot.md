---
title: Azure İzleyici ölçüm grafikleri sorunlarını giderme
description: Oluşturma, özelleştirme veya ölçüm grafikleri yorumlama sorunlarını giderme
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 73ef5cc00b5154dbdbc92911d17740c7d13038ec
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341973"
---
# <a name="troubleshooting-metrics-charts"></a>Ölçüm grafikleri sorunlarını giderme

Oluşturma, özelleştirme veya Azure ölçüm Gezgini'nde grafikleri yorumlama konusunda sorun yaşarsanız bu makaleyi kullanın. Ölçümler için yeni başladıysanız, hakkında bilgi edinin [ölçüm Gezgini ile çalışmaya başlama](metrics-getting-started.md) ve [ölçüm Gezgini özelliklerinin Gelişmiş](metrics-charts.md). Ayrıca bkz [örnekler](metric-chart-samples.md) yapılandırılmış ölçüm grafikleri.

## <a name="cant-find-your-resource-to-select-it"></a>Seçmek için kaynak bulunamıyor

Üzerinde tıkladığınız **bir kaynak seçin** düğmesi, ancak kaynak Seçici iletişim kutusunu kaynağınızda göremiyorum.

**Çözüm:** Ölçüm Gezgini kullanılabilir kaynakları listeleme önce Abonelikleriniz ve kaynak gruplarınız seçmenizi gerektirir. Kaynağınızı göremiyorsanız:

1. Doğru aboneliği seçtiğinizden emin olun **abonelik** açılır. Aboneliğiniz listede yoksa, tıklayarak **dizin + Abonelik ayarları** ve kaynağınız ile bir abonelik ekleyin.

1. Doğru kaynak grubunu seçtiğinizden emin olun.
    > [!WARNING]
    > Ölçüm Gezgini, ilk kez açtığınızda en iyi performans için **kaynak grubu** açılan sahip önceden seçilen kaynak grubu yok. Herhangi bir kaynağa görebilmek için önce en az bir grubu seçmeniz gerekir.

## <a name="chart-shows-no-data"></a>Grafik veri gösterir

Bazen grafikleri, doğru kaynakları ve ölçümleri belirledikten sonra hiçbir veri gösterebilir. Bu davranış, çeşitli nedenlerden kaynaklanabilir:

### <a name="microsoftinsights-resource-provider-isnt-registered-for-your-subscription"></a>Microsoft.Insights kaynak sağlayıcısı, aboneliğiniz için kayıtlı değil

Ölçümleri keşfederken gerektirir *Microsoft.Insights* kaynak sağlayıcısının aboneliğinize kayıtlı. Çoğu durumda, (diğer bir deyişle, bir uyarı kuralını yapılandırın, herhangi bir kaynak için tanılama ayarları özelleştirme veya bir otomatik ölçeklendirme kuralını yapılandırın sonra), otomatik olarak kaydedilir. Microsoft.Insights kaynak sağlayıcısı kayıtlı değilse, el ile açıklanan adımları uygulayarak kaydetmelisiniz [Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/resource-manager-supported-services.md).

**Çözüm:** Açık **abonelikleri**, **kaynak sağlayıcıları** sekmesini tıklatıp doğrulayın *Microsoft.Insights* aboneliğiniz için kayıtlı.

### <a name="you-dont-have-sufficient-access-rights-to-your-resource"></a>Kaynağınız için yeterli erişim haklarına sahip değilsiniz

Azure'da ölçümlerine erişim tarafından denetlenen [rol tabanlı erişim denetimi (RBAC)](../../role-based-access-control/overview.md). Bir üyesi olmanız gerekir [izleme okuyucusu](../../role-based-access-control/built-in-roles.md#monitoring-reader), [izleme katkıda bulunanı](../../role-based-access-control/built-in-roles.md#monitoring-contributor), veya [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) ölçümleri herhangi bir kaynak için keşfetmek için.

**Çözüm:** Ölçümleri keşfederken kaynak için yeterli izinleri olduğundan emin olun.

### <a name="your-resource-didnt-emit-metrics-during-the-selected-time-range"></a>Seçilen zaman aralığında kaynak ölçümleri yayma olmadı

Bazı kaynaklar, sürekli olarak kendi ölçümleri yayma yok. Örneğin, Azure ölçümleri durdurulmuş sanal makineler için toplamaz. Yalnızca bazı koşullar meydana geldiğinde, diğer kaynakları kendi ölçümleri yayabilir. Örneğin, bir hareket işleme zamanı gösteren bir ölçüm en az bir işlem gerektirir. Seçili zaman aralığında hiçbir işlem varsa, grafik doğal olarak boş olur. Ayrıca, azure'da ölçümleri çoğunu dakikada toplanır, ancak bazı kısıtlamalar var. daha az sıklıkta toplanan. Keşfetmeye çalıştığınız ölçümü hakkında daha fazla bilgi almak için ölçüm belgelerine bakın.

**Çözüm:** Grafiğin zaman geniş bir değiştirin. "Son 30 güne ait" başlayabilir daha büyük bir zaman ayrıntı düzeyi kullanarak (veya "otomatik zaman ayrıntı düzeyi" seçeneği bağlı olan).

### <a name="you-picked-a-time-range-greater-than-30-days"></a>30 günden fazla bir zaman aralığı Çekildi

[Azure, çoğu ölçümün 93 gün boyunca saklanır](data-platform-metrics.md#retention-of-metrics). Ancak, yalnızca en fazla 30 gün herhangi tek bir grafiğe verileri için sorgulama yapabilirsiniz. Bu sınırlama geçerli değildir [günlük tabanlı ölçümler](../app/pre-aggregated-metrics-log-metrics.md#log-based-metrics).

**Çözüm:** Boş bir grafik görürsünüz ya da grafiğiniz yalnızca ölçüm verileri bir parçası görüntüler arasında Başlat - ve son-tarih saat Seçici içinde fark 30 gün aralığı aşmadığını doğrulayın.

### <a name="all-metric-values-were-outside-of-the-locked-y-axis-range"></a>Kilitli y ekseni aralığı dışında tüm ölçüm değerleri olan

Tarafından [grafik y ekseni sınırlarını kilitleme](metrics-charts.md#lock-boundaries-of-chart-y-axis), grafik çizgisi gösterme grafik görüntüleme alanını yanlışlıkla yapabilirsiniz. Örneğin, y ekseni aralığı %0 ve % 50 arasında kilitli ve ölçüm % 100 sabit bir değer varsa satırın her zaman grafiğin boş görünür yapmadan görünür alanının dışında işlenir.

**Çözüm:** Grafiğin y ekseni sınırlarını ölçüm değerleri aralığı dışında kilitlenmez doğrulayın. Y ekseni sınırlarını kilitliyse, geçici olarak ölçüm değerleri grafik aralığın dışında kalan yoksa emin olmak için bunları sıfırlamak isteyebilirsiniz. Y ekseni aralığını kilitleme grafiklerle için otomatik ayrıntı düzeyi ile önerilmez **toplam**, **min**, ve **max** toplama değerleri ile değişeceği tarayıcı penceresini yeniden boyutlandırabilir veya başka bir ekran çözünürlüğünü giderek ayrıntı düzeyi. Geçiş ayrıntı düzeyi, grafik boş görüntü alanının bırakabilir.

### <a name="you-are-looking-at-a-guest-os-metric-but-didnt-enable-azure-diagnostic-extension"></a>Bir konuk işletim sistemi ölçüm aranıyor, ancak Azure tanılama uzantısını etkinleştirme fırsatınız

Koleksiyonu **konuk işletim sistemi** ölçümleri kullanarak etkinleştirme ya da Azure tanılama uzantısı yapılandırma gerektirir **tanılama ayarları** kaynağınızın paneli.

**Çözüm:** Azure tanılama uzantısı etkin ancak ölçümlerinizi bkz hala bulamıyorsanız, konusunda özetlenen adımları izleyin [Azure tanılama uzantısını sorun giderme kılavuzu](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal). Ayrıca sorun giderme adımları için bkz: [konuk işletim sistemi ad alanı ve ölçümleri seçemezsiniz](metrics-troubleshoot.md#cannot-pick-guest-os-namespace-and-metrics)

## <a name="error-retrieving-data-message-on-dashboard"></a>Panoda "veriler alınırken hata oluştu" iletisi

Daha sonra kullanım dışı ve Azure'dan kaldırılan ölçülü panonuz oluşturulduğunda bu sorun oluşabilir. Açık durumda olduğunu doğrulamak için **ölçümleri** kaynak ve onay ölçüm Seçici kullanılabilir ölçümleri sekmesi. Ölçüm gösterilmezse, ölçüm Azure'dan kaldırıldı. Genellikle, bir ölçümü kullanım dışı olduğunda, kaynak durumu benzer bir perspektif sağlar, daha yeni bir ölçüm yoktur.

**Çözüm:** Başarısız olan kutucuk Panoda grafiğiniz için alternatif bir ölçüm seçerek güncelleştirin. Yapabilecekleriniz [Azure Hizmetleri için kullanılabilir ölçümlerin bir listesini gözden geçirin](metrics-supported.md).

## <a name="chart-shows-dashed-line"></a>Kesik çizgi grafik gösterilir

Azure ölçüm grafikleri kesikli satır türü olduğunu eksik değerlerin (diğer adıyla "null değer") arasında iki bilinen zaman dilimi veri noktaları belirtmek için kullanın. Örneğin, "1 dakika" zaman ayrıntı düzeyi seçtiğiniz zaman seçicide, ancak ölçüm 07:26, 07:27 raporlandı 07:29-07:30 (ikinci ve üçüncü veri noktaları arasındaki dakika boşluk unutmayın), ardından kesikli çizgiye 07:27 ve 07:29 bağlanır ve düz bir çizgi bağlanır diğer tüm veri noktaları. Ölçüm kullandığında sıfıra kesikli çizgiye düşme **sayısı** ve **toplam** toplama. İçin **ortalama**, **min** veya **max** toplamalar, kesikli çizgiye yakın iki bilinen bir veri noktası bağlanır. Ayrıca, veri grafik en sağdaki veya sol tarafta eksik olduğunda kesikli çizgiye eksik veri noktasının yönü genişletir.
  ![Ölçüm görüntüsü](./media/metrics-troubleshoot/missing-data-point-line-chart.png)

**Çözüm:** Bu davranış tasarım gereğidir. Veri noktaları tanımlamak için yararlıdır. Çizgi grafik, yüksek yoğunluklu ölçüm eğilimleri görselleştirmek için üstün bir seçenektir ancak seyrek değerleri Ölçümleriyle için zaman dilimi değerlerle corelating özellikle önemli olduğu durumlarda yorumlamak zor olabilir. Bu grafiklerin okuma kesikli çizgiye kolaylaştırır, ancak grafik hala belirsiz ise farklı bir grafik türü ile kullanarak ölçümleri görüntüleme göz önünde bulundurun. Bir değer olduğunda bir nokta Görselleştirme ve veri atlanıyor tamamen değeri eksik olduğunda noktası yalnızca örneğin, aynı Ölçüm dağınık çizim grafiği açıkça tarafından her zaman çizgisi gösterir: ![ölçüm görüntüsü](./media/metrics-troubleshoot/missing-data-point-scatter-chart.png)

   > [!NOTE]
   > Bir çizgi grafik, ölçüm için hala tercih ederseniz, konumda fare işaretçisinin veri noktası vurgulayarak zaman ayrıntı düzeyi değerlendirmek için grafik üzerinde fareyi hareket yardımcı olabilir.

## <a name="chart-shows-unexpected-drop-in-values"></a>Grafik beklenmeyen bırakma değerleri gösterir.

Çoğu durumda, bir yanlış anlama grafikte gösterilen veri ölçüm değerleri algılanan düşüş olur. Bir düşüş toplamları tarafından misled ya da son ölçüm veri noktaları henüz alınan veya henüz Azure tarafından işlenen çünkü grafik son dakika zaman gösterir sayar. Ölçümler işlem gecikme süresi, hizmete bağlı olarak birkaç dakika aralığında olabilir. 1 veya 5 dakikalık bir ayrıntı düzeyi ile yeni bir zaman aralığı gösteren grafikler için bir bırakma değerin son birkaç dakikadan daha belirgin hale gelir: ![ölçüm görüntüsü](./media/metrics-troubleshoot/drop-in-values.png)

**Çözüm:** Bu davranış tasarım gereğidir. Bunu aldığımız hemen sonra veriler gösteriliyor olsa bile verileri faydalı olduğunu düşünüyoruz *kısmi* veya *tamamlanmamış*. Bunun yapılması, daha kısa süre içinde önemli sonuç yapmak ve araştırma hemen başlatmak sağlar. Örneğin, hata sayısını gösteren bir ölçüm için bir kısmi değer X görmeye yapıldığını en az bildiren bir verilen dakika hatalarında X. Sorun hemen incelemeye başlayın yerine, tam olarak önemli olmayabilir bu dakika üzerinde gerçekleşen hataları sayısını görmek için bekleyin. Grafik, veri kümesinin tamamını aldığımız ancak o zaman daha yeni bir dakika yeni eksik veri noktalarından gösterebilir sonra güncelleştirilir.

## <a name="cannot-pick-guest-os-namespace-and-metrics"></a>Konuk işletim sistemi ad alanı ve ölçümleri seçemezsiniz

Sanal makineler ve sanal makine ölçek kümeleri ölçümleri iki kategorisi vardır: **Sanal makine konağı** Azure barındırma ortamı tarafından toplanan Ölçümler ve **konuk işletim sistemi** tarafından toplanan ölçümler [İzleme Aracısı](agents-overview.md) , sanal makineler üzerinde çalışan. İzleme Aracısı'nı etkinleştirerek yükleme [Azure tanılama uzantısı](diagnostics-extension-overview.md).

Varsayılan olarak, konuk işletim sistemi ölçümleri arasından seçim Azure depolama hesabında depolanan **tanılama ayarları** kaynağınızın sekmesi. Toplanan ölçümler konuk işletim sistemi olmayan veya ölçüm Gezgini bunları erişemez, yalnızca göreceksiniz **sanal makine konağı** ölçüm ad alanı:

![Ölçüm görüntüsü](./media/metrics-troubleshoot/cannot-pick-guest-os-namespace.png)

**Çözüm:** Görmüyorsanız **konuk işletim sistemi** ad alanı ve ölçüm Gezgini'nde ölçüm:

1. Onaylayın [Azure tanılama uzantısı](diagnostics-extension-overview.md) etkinleştirilmişse ve ölçümleri toplamak üzere yapılandırılmış.
    > [!WARNING]
    > Kullanamazsınız [Log Analytics aracısını](agents-overview.md#log-analytics-agent) (Microsoft Monitoring Agent ya da "MMA" da bilinir) göndermek için **konuk işletim sistemi** depolama hesabına.

1. Depolama hesabı güvenlik duvarı tarafından korunmuyor doğrulayın.

1. Kullanım [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) ölçümleri depolama hesabına akar doğrulamak için. Toplanan ölçümler olmayan Eğer [Azure tanılama uzantısını sorun giderme kılavuzu](diagnostics-extension-troubleshooting.md#metric-data-doesnt-appear-in-the-azure-portal).

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Gezgini ile çalışmaya başlama hakkında bilgi edinin](metrics-getting-started.md)
* [Ölçüm Gezgini'nde gelişmiş özellikler hakkında bilgi edinin](metrics-charts.md)
* [Azure Hizmetleri için kullanılabilir ölçümleri bakın](metrics-supported.md)
* [Yapılandırılmış grafiklerin örneklerine bakın](metric-chart-samples.md)
