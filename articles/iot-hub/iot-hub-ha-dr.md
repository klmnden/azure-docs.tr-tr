---
title: Azure IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma | Microsoft Docs
description: Yüksek oranda kullanılabilir Azure IOT çözümleri ile olağanüstü durum kurtarma özellikleri oluşturmanıza yardımcı olan Azure ve IOT hub'ı özelliklerini açıklar.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: rkmanda
ms.openlocfilehash: 22c53a85d46cbcb1ed9592d7373ea044e8910297
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248307"
---
# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IOT hub'ı yüksek kullanılabilirlik ve olağanüstü durum kurtarma
Esnek bir IOT uygulama doğrultusunda ilk adımı olarak çözüm, mimarları, geliştiriciler ve işletme sahipleri çözümleri oluşturmakta olduğunuz çalışma süresi amaçlarını tanımlamanız gerekir. Bu hedefleri, her senaryo için belirli iş hedeflerine göre öncelikli olarak tanımlanabilir. Bu bağlamda, makaleyi [Azure iş sürekliliği teknik rehberlik]( https://docs.microsoft.com/azure/architecture/resiliency/) iş sürekliliği ve olağanüstü durum kurtarma hakkında düşünmek yardımcı olmak için genel bir çerçeve açıklar. [Olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](_https://msdn.microsoft.com/library/dn251004.aspx) kağıt, yüksek kullanılabilirlik (HA) ve olağanüstü durum kurtarma (DR) elde etmek, Azure uygulamaları için stratejileri hakkında mimari rehberlik sağlar. 

Bu makalede özellikle IOT Hub hizmeti tarafından sunulan yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri açıklanmaktadır. Bu makalede ele alınan geniş alanlar 

- Bölge içi HA
- Bölgeler arası DR
- Bölgeler arası yüksek kullanılabilirlik elde edin

Çalışma süresi hedeflerini bağlı olarak, IOT çözümleri için tanımladığınız iş hedeflerinize hangi seçeneklerin en iyi karşılayacak özetlenen belirlemeniz gerekir. Bu HA/DR alternatifleri hiçbirini IOT çözümünüzü ekleme, gereksinimlerimi karşılamama dikkatli bir değerlendirmesini gerektirir:
- İhtiyaç duyduğunuz dayanıklılık düzeyi 
- Uygulama ve Bakım karmaşıklığı
- COGS etkisi


## <a name="intra-region-ha"></a>Bölge içi HA
IOT Hub hizmeti, neredeyse tüm hizmet katmanlarında fazlalıkları uygulayarak bölge içi HA sağlar. [SLA'sı IOT Hub hizmeti tarafından yayımlanan](_https://azure.microsoft.com/support/legal/sla/iot-hub) sağlayarak gerçekleştirilir bu fazlalıkları kullanımı. Ek bir iş, IOT çözümünün geliştiriciler tarafından bu HA özelliklerden yararlanmak için gereklidir. IOT hub'ı makul ölçüde yüksek çalışma süresi garantisi sunar ancak tüm dağıtılmış bilgi işlem platformu olarak ile hala geçici hataları beklenebilir. Yalnızca çözümlerinizi bir şirket içi çözümden buluta geçirme ile başlıyorsanız, odağınızı "hatalar arasındaki ortalama süreyi" en iyi duruma getirmesini kaydırılacak gerekiyor "ortalama kurtarma süresi". Diğer bir deyişle, geçici hataları karışımı bulutta çalışırken normal kabul edilip. Uygun [yeniden deneme ilkelerine](_https://channel9.msdn.com/Shows/Internet-of-Things-Show/Retry-logic-in-device-SDKs-for-Azure-IoT-Hub) geçici hatalarla uğraşmak için bir bulut uygulaması ile etkileşim kurma bileşenlerine yerleşik gerekir. 

> [!NOTE]
> Ayrıca bazı Azure Hizmetleri ile tümleştirerek bir bölgede kullanılabilen ek katman sağlar [kullanılabilirlik alanları (AZs)](_https://docs.microsoft.com/azure/availability-zones/az-overview). AZs IOT Hub hizmeti tarafından şu anda desteklenmemektedir.

## <a name="cross-region-dr"></a>Bölgeler arası DR
Bir veri merkezinde güç kesintileri veya fiziksel varlıklar içeren diğer hatalar nedeniyle genişletilmiş kesinti yaşandığında bazı nadir durumlar olabilir. Bu tür olaylar nadirdir sırasında hangi içi bölge yukarıda açıklanan HA özelliği her zaman yardımcı. IOT Hub gibi genişletilmiş kesintilere karşı kurtarılması için birden çok çözümü sunar. Böyle bir durum müşteriler kurtarma Seçenekler şunlardır: "Başlatılan Microsoft yük devretme" ve "el ile yük devretme". İkisi arasındaki temel fark, Microsoft eski başlatır ve kullanıcı ikinci başlatan ' dir. Ayrıca el ile yük devretme başlatılan Microsoft yük devretme seçeneğini karşılaştırıldığında bir düşük kurtarma süresi hedefi (RTO) sağlar. Her seçeneği ile sunulan belirli Rto'lar aşağıdaki bölümlerde ele alınmıştır. Birincil bölgede bir IOT hub'ın yük devretme gerçekleştirmek için bu seçeneklerden birini kullandı hub ilgili tam işlevsel olur [coğrafi eşleştirilmiş bir Azure bölgesine](_https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 


Aşağıdaki kurtarma noktası hedeflerine (RPO'lar) bu hem yük devretme seçenekleri sağlar:

| Veri türü | Kurtarma noktası hedeflerini (RPO) |
| --- | --- |
| Kimlik kayıt defteri |0-5 dakika veri kaybı |
| Cihaz ikizi verileri |0-5 dakika veri kaybı |
| Bulut-cihaz iletilerini ** |0-5 dakika veri kaybı |
| Üst ** ve cihaz işleri |0-5 dakika veri kaybı |
| Cihazdan buluta iletiler |Okunmamış iletileri kaybolur |
| İşlem izleme iletileri |Okunmamış iletileri kaybolur |
| Bulut-cihaz geri bildirim iletileri |Okunmamış iletileri kaybolur |

IOT hub'ı için yük devretme işlemi tamamlandıktan sonra el ile müdahale gerekmeden çalışmaya devam etmek için tüm cihaz ve arka uç uygulamaları işlemlerinden beklenmektedir.

> [!CAUTION]
> - Event Hub ile uyumlu ada ve uç IOT hub'ı yerleşik olayları uç nokta yük devretme sonrasında değiştirin. Telemetri iletilerini yerleşik uç noktadan olay hub'ı istemci veya olay işleyicisi ana bilgisayarı kullanılarak alındığında, size gereken [IOT hub bağlantı dizesini kullanmak](_https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin#read-from-the-built-in-endpoint) bağlantı kurmak için. Bu, arka uç uygulamalarınızı yük devretme sonrasında el ile müdahale gerekmeksizin çalışmaya devam etmesini sağlar. Event Hub ile uyumlu ada ve uç nokta arka uç uygulamanızın doğrudan kullanıyorsanız, uygulamanız tarafından yeniden yapılandırmanız gerekecektir [yeni Event Hub ile uyumlu ada ve uç nokta getirilirken](_https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin#read-from-the-built-in-endpoint) devam etmek için yük devretme sonrasında işlemler. 
>
> - Yük devretmeden sonra olaylar Event Grid yayılan bu Event Grid aboneliği kullanılabilir olmaya devam sürece önceden yapılandırdığınız aynı abonelikleri tüketilebilir.
>
> - ** Bulut-cihaz iletilerini ve üst işleri el ile yük devretme bu özelliğin Önizleme teklifi olarak bir parçası olarak kurtarılamaz.

### <a name="microsoft-initiated-failover"></a>Başlatılan Microsoft yük devretme
Başlatılan Microsoft yük devretme tüm IOT hub'ları bir etkilenen bölgelerine karşılık gelen coğrafi olarak eşleştirilen bölgeye yük devretmek için ender durumlarda Microsoft tarafından Uygula. Bu, varsayılan seçenek (kullanıcıların çevirmek için hiçbir yol) ve kullanıcı müdahalesi gerektirir. Microsoft, bu seçenek uygulanacak bir belirlenmesi yapmak için hakkını saklı tutar. Bu mekanizma, kullanıcının hub üzerinde başarısız olmadan önce kullanıcı onayı gerektirmez. Başlatılan Microsoft yük devretme 2-26 saat kurtarma süresi hedefi (RTO) sahiptir. Büyük RTO, Microsoft, bu bölgede etkilenen tüm müşteriler adına yük devretme işlemi gerçekleştirmeniz gerekir çünkü. Bir gün kabaca bir kapalı kalma süresi karşılayabilir daha az kritik bir IOT çözümü çalıştırıyorsanız, bir bağımlılık, IOT çözümünüzün genel olağanüstü durum kurtarma hedefleri karşılamak için bu seçeneği yapmanıza gerek normaldir. Çalışma zamanı için toplam süreyi bu işlem tetiklendikten sonra tam olarak işlevsel hale işlemlerinin "kurtarmak için zaman" bölümünde açıklanmıştır. 

### <a name="manual-failover-preview"></a>El ile yük devretme (Önizleme)

Bu başlatılan Microsoft yük devretme sağlar, iş çalışma süresi hedeflerinizi tarafından RTO tatmin edici değil yük devretme işlemini kendiniz tetiklemek için el ile yük devretme kullanmayı düşünmeniz gerekir. Bu seçenek kullanılarak RTO, herhangi bir yere 10 dakika olarak birkaç saat arasında olabilir. RTO, yükü devredilen IOT hub örneği üzerinde kayıtlı cihaz sayısını şu anda bir işlevdir. RTO yaklaşık 100.000 cihaz barındırma hub için 15 dakikalık ballpark içinde olmasını bekleyebilirsiniz. Çalışma zamanı için toplam süreyi bu işlem tetiklendikten sonra tam olarak işlevsel hale işlemlerinin "kurtarmak için zaman" bölümünde açıklanmıştır.

El ile yük devretme seçeneğini her zaman olup birincil bölgenin kesinti veya yaşanıyor bağımsız olarak kullanılabilir. Bu nedenle, planlanan yük devretmeler gerçekleştirmek için bu seçeneği potansiyel olarak kullanılabilir. Planlanan yük devretmeler bir kullanım örneği, düzenli aralıklarla yük devretme tatbikatlarının gerçekleştirmektir. Bir sözcük uyarı ancak planlı yük devretme işlemi sonuçlarını hub için bir kapalı kalma süresine RTO için bu seçeneği tarafından tanımlanan dönem için olan ve aynı zamanda da yukarıdaki RPO tablosu tarafından tanımlandığı şekilde bir veri kaybı ile sonuçlanır. Düzenli aralıklarla uçtan uca çözümlerinizi ve gerçek bir olağanüstü durum meydana geldiğinde çalışan yeteneğinizi güvenilirlik elde etmek için planlanan yük devretme seçeneğini kullanmak için IOT hub örneği bir test ayarı düşünebilirsiniz. 

> [!IMPORTANT]
> - Üretim ortamlarınızı kullanılmakta olan IOT hub'larında test tatbikatları gerçekleştirilmemelidir. 
>
> - El ile yük devretme mekanizması olarak kalıcı olarak hub'ınıza eşleştirilmiş Azure coğrafi bölgeler arasında geçirmek için kullanılmamalıdır. Bunun yapılması, eski birincil bölgeye bağlantılı cihazlar hub'ın karşı gerçekleştirilen işlemleri için daha yüksek bir gecikme süresiyle neden olur. 
>
> - El ile yük devretme, şu anda Önizleme aşamasındadır ve aşağıdaki Azure bölgelerinde kullanılabilir değil. Doğu ABD, Batı ABD, Kuzey Avrupa, Batı Avrupa, Brezilya Güney, Orta Güney ABD.

### <a name="failback"></a>Yeniden çalışma

Başarısız olan eski birincil bölgeye geri dönün, yük devretme eylem tetikleyerek gerçekleştirilebilir başka bir zaman. Özgün yük devretme işlemi, genişletilmiş kesinti özgün birincil bölgelerindeki kurtarılır yapıldıysa, bu konuma kesinti durumdan kurtardı sonra hub özgün konuma geri başarısız olduğunu önerilir. 

> [!IMPORTANT]
> - Kullanıcılar yalnızca 2 başarılı yük devretme ve her gün 2 başarılı bir yeniden çalışma işlemlerini gerçekleştirmek için izin verilir.
>
> - Yük devretme yedeklemek için yedekleme / yeniden çalışma işlemlerini izin verilmez. Kullanıcıların bu işlemler arasında 1 saat beklemeniz gerekecektir.

### <a name="time-to-recover"></a>Kurtarma süresi

FQDN (ve bu nedenle bağlantı dizesi) IOT hub'ı örneğinin aynı yük devretme sonrasında kaldığı sürece, temel alınan IP adresi değişir. Bu nedenle aşağıdaki işlevini kullanarak yük devretme işlemini tetiklendikten sonra tam olarak işlevsel olmasını, IOT hub'ı örneği karşı gerçekleştirilmekte olan çalışma zamanı işlemleri için toplam süreyi belirtilebilir.

Kurtarma süresi RTO = [10 dakika - el ile yük devretme için 2 saat | 2-26 saattir Microsoft yük devretme başlatılan] + DNS yayma gecikme + herhangi yenilemek için istemci uygulaması tarafından harcanan süre önbelleğe IOT hub'ı IP adresi. 

> [!IMPORTANT]
> IOT SDK'ları, IOT hub'ın IP adresi önbelleğe alma işlemi. Kullanıcı kodu SDK'ları ile arabirim IOT hub'ın IP adresini önbelleğe kaydetmelisiniz öneririz. 

## <a name="achieve-cross-region-ha"></a>Bölgeler arası yüksek kullanılabilirlik elde edin
İş çalışma süresi hedeflerinizi RTO tarafından Microsoft yük devretme başlatılana veya el ile yük devretme seçenekleri tatmin edici değil, cihaz başına otomatik bölgeler arası yük devretme mekanizması uygulayarak düşünmelisiniz. IOT çözümlerinde dağıtım topolojilerinin eksiksiz bir işlemden bu makalenin kapsamı dışında ' dir. Açıklanır makale *bölgesel yük devretme* yüksek kullanılabilirlik ve olağanüstü durum kurtarma amacıyla dağıtım modeli.

Bölgesel yük devretme modelinde, çözüm geri öncelikle tek bir veri merkezi konumda son çalışır. Bir IOT hub'ı ikincil ve arka uç farklı bir veri merkezi konumuna dağıtılır. IOT hub birincil bölgede kesinti alternatife veya birincil bölgeye CİHAZDAN ağ bağlantısı kesildi, ikincil bir hizmet uç noktası cihazları kullanın. Tek bir bölgede kaldığını yerine bölgeler arası yük devretme model uygulayarak çözüm kullanılabilirlik iyileştirebilir. 

Yüksek düzeyde, IOT Hub ile bölgesel yük devretme modeli uygulamak için aşağıdaki adımları uygulamanız gerekir:

* **İkincil bir IOT hub ve cihaz mantıksal yönlendirme**: Hizmet, birincil bölgedeki kesintiye uğrarsa, aygıtlar, ikincil bir bölgeye bağlanma başlatmanız gerekir. Durumu algılayan dahil çoğu Hizmetleri göz önünde bulundurulduğunda, bölgeler arası yük devretme işlemi tetiklemek çözüm Yöneticiler için yaygındır. İşlemin denetimini elde tutarak cihazlara, yeni uç nokta iletişim kurmak için en iyi yolu bunları düzenli olarak kontrol sahip olmaktır bir *concierge* geçerli etkin uç noktası için hizmet. Concierge hizmetini kopyalandığı ve erişilebilir kalmasını bir web uygulaması olabilir DNS-yeniden yönlendirme teknikleri kullanarak (örneğin, kullanarak [Azure Traffic Manager][Azure Traffic Manager]).

   > [!NOTE]
   > IOT hub hizmeti, bir Azure Traffic Manager'da desteklenen uç noktası türü değil. Önerilen concierge hizmetini API uç nokta durum araştırmasını uygulamak yaparak Azure traffic manager ile tümleştirmek için önerilir.

* **Kimlik kayıt defteri çoğaltma**: kullanılabilir olması için ikincil IOT hub'ı çözümüne bağlayabilirsiniz tüm cihaz kimlikleri içermelidir. Çözümün coğrafi olarak çoğaltılmış yedekleme cihaz kimliklerini tutmak ve ikincil IOT hub'ına cihazlar için etkin bir uç nokta geçmeden önce bunları yüklemek gerekir. IOT hub cihaz kimliği dışarı aktarma işlevi bu bağlamda yararlı olur. Daha fazla bilgi için [IOT Hub Geliştirici Kılavuzu - kimlik kayıt defteri][IoT Hub developer guide - identity registry].
* **Mantıksal birleştirme**: birincil bölge tekrar kullanılabilir duruma geldiğinde, tüm ikincil sitede oluşturulan verileri ve durum geçirilmelidir birincil bölgeye geri. Bu durum ve verileri, cihaz kimliklerini ve birincil IOT hub'ı ve birincil bölgedeki başka bir uygulamaya özgü depoları ile birleştirilmelidir uygulama meta verileri için çoğunlukla ilgilidir. Bu adım basitleştirmek için kere etkili olan işlemler kullanmalısınız. Idempotent işlemlerinin yan etkileri nihai tutarlı dağıtım olayların ve yinelemeleri ya da olayların sırası teslimini en aza indirin. Ayrıca, uygulama mantığı, olası tutarsızlıklar veya biraz güncel durumu tolerans tasarlanmalıdır. Bu durum, sistemin kurtarma noktası hedeflerini (RPO) tabanlı onarımı gereken ek süre nedeniyle ortaya çıkabilir.

## <a name="choose-the-right-hadr-option"></a>Doğru HA/DR seçeneği seçin
Bu makaledeki başvuru çerçevesi çalışır, çözümünüz için doğru seçeneği belirlemenize için kullanılabilir HA/DR seçeneklere özetini Heres

| HA/DR seçeneği | RTO | RPO | El ile müdahale gerektirir? | Uygulama karmaşıklığı | Ek ücret etkisi|
| --- | --- | --- | --- | --- | --- | --- |
| Başlatılan Microsoft yük devretme |2 - 26 saat|RPO tabloya bakın|Hayır|None|None
| El ile yük devretme |10 dakikalık - 2 saat|RPO tabloya bakın|Evet|Çok düşük. Bu işlem Portal tetiklemek yeterlidir.|None
| Çapraz bölge HA |< 1 dakika|Özel HA çözümünüzü çoğaltma sıklığına bağlıdır|Hayır|Yüksek|maliyeti, IOT hub'ı 1 x 1 > 

## <a name="next-steps"></a>Sonraki adımlar
Azure IOT Hub hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin:

* [IOT Hubs (eğitim) ile çalışmaya başlama][lnk-get-started]
* [Azure IOT Hub nedir?][What is Azure IoT Hub?]

[Disaster recovery and high availability for Azure applications]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure Traffic Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub developer guide - identity registry]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[What is Azure IoT Hub?]: about-iot-hub.md
