---
title: Azure İzleyici (Önizleme) VM'ler için sık sorulan sorular | Microsoft Docs
description: VM'ler (Önizleme) için Azure İzleyici sistem durumunu ve uygulama bileşenleri ve diğer kaynaklarla ilgili bağımlılıkları otomatik olarak keşfetme yanı sıra Azure VM'nin işletim sistemi izleme performansını birleştirir ve iletişimi eşleyen bir Azure çözümüdür Bunlar arasında. Bu makalede, sık sorulan soruları yanıtlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2018
ms.author: magoedte
ms.openlocfilehash: 124488d13d1d303743900ab1de41768633e4f6e0
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715727"
---
# <a name="azure-monitor-for-vms-preview-frequently-asked-questions"></a>Azure İzleyici (Önizleme) VM'ler için sık sorulan sorular
Bu Microsoft FAQ VM'ler için Azure İzleyici hakkında sık sorulan soruların bir listesidir. Çözümü hakkında ek sorularınız varsa, Git [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) ve sorularınızı gönderin. Sık sorulan bir soru, böylece hızla ve kolayca bulunabilir, bu makaleye ekleriz.

## <a name="can-i-onboard-to-an-existing-workspace"></a>Alabilirim yerleşik mevcut bir çalışma alanına?
Sanal makinelerinizi bir Log Analytics çalışma alanınıza bağlıysa, ekleme, sağlanan VM'ler için Azure İzleyici için listelenen desteklenen bölgelerden birinde olduğunda, o çalışma uygulamasını kullanmaya devam edebilir [burada](vminsights-onboard.md#prerequisites).

Ne zaman ekleme, tüm VM'lerin VM'ler için görüntüleme ve analiz Azure İzleyici'de, bu bilgileri toplamaya başlamak için çalışma alanına raporlama verilerini neden olacak bir çalışma alanı için performans sayaçları yapılandırıyoruz.  Sonuç olarak, seçilen çalışma alanına bağlı sanal makinelerin tüm performans verileri görürsünüz.  Sistem durumu ve harita özellikleri, yalnızca belirttiğiniz için yerleşik VM'ler için etkinleştirilir.

Hangi performans sayaçlarını etkinleştirilmiş daha fazla bilgi için müşterilerimize [ekleme](vminsights-onboard.md) makalesi.

## <a name="can-i-onboard-to-a-new-workspace"></a>Alabilirim eklemek için yeni bir çalışma alanı? 
Sanal makinelerinizi şu anda mevcut bir Log Analytics çalışma alanına bağlı değil, verilerinizi depolamak için yeni bir çalışma alanı oluşturmanız gerekir.  Yeni bir varsayılan çalışma alanı oluşturma, otomatik olarak tek bir Azure VM için Azure İzleyici VM'ler için Azure Portalı aracılığıyla yapılandırırsanız yapılır.

Betik tabanlı bir yöntemini kullanmayı tercih ederseniz, bu adımları ele alınmaktadır [ekleme](vminsights-onboard.md) makalesi. 

## <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Sanal Makinem zaten mevcut bir çalışma alanına raporlama varsa ne yapmalıyım?
Zaten sanal makinelerinizden veri topluyorsanız, zaten mevcut bir Log Analytics çalışma alanına rapor verilerine yapılandırmadığınız.  Bu çalışma alanı, desteklenen bölgelerden birinde olduğu sürece, Azure İzleyici önceden mevcut olan bu çalışma alanına VM'ler için etkinleştirebilirsiniz.  Zaten kullanmakta olduğunuz çalışma alanı, desteklenen bölgelerden birinde değilse, şu anda sanal makineleri için Azure İzleyici ekleneceği mümkün olmayacaktır.  Etkin bir şekilde ek bölgeler desteklemek için çalışıyoruz.

>[!NOTE]
>Çalışma alanına rapor veren tüm sanal makineler eklemek için bunları Azure İzleyici VM'ler için seçtiğiniz olup olmadığını etkileyen çalışma alanı için performans sayaçlarını yapılandırıyoruz. Performans sayaçları için bir çalışma alanı nasıl yapılandırılacağı hakkında ayrıntılı bilgi için bkz bizim [belgeleri](../../log-analytics/log-analytics-data-sources-performance-counters.md). VM'ler için Azure İzleyici için yapılandırılmış sayaçları hakkında daha fazla bilgi için bkz bizim [ekleme belgeleri](vminsights-onboard.md#performance-counters-enabled).  

## <a name="why-did-my-vm-fail-to-onboard"></a>Neden sanal Makinem ekleme başarısız oldu?
Azure portalında bir Azure VM'den ekleme, aşağıdaki adımları olduğunda:

* Varsayılan Log Analytics çalışma alanı, seçenek seçilen oluşturulur.
* Performans sayaçları, seçilen çalışma alanı için yapılandırılır. Bu adım başarısız olursa, bazı performans grafikleri ve tabloları veri VM için gösteren olmayan olduğunu fark, eklenmedi. Belgelenen PowerShell betiğini çalıştırarak bunu düzeltebilirsiniz [burada](vminsights-onboard.md#enable-with-powershell).
* Log Analytics aracısını gerekli belirlenir bir VM uzantısı kullanarak Azure Vm'lerinde yüklenir.  
* Azure İzleyici Vm'leri harita bağımlılık aracısı için gerekli olduğunu belirledi bir uzantısını kullanarak Azure Vm'lerinde yüklenir.  
* Gerekirse sağlığı özelliğini destekleyen azure İzleyici bileşenlerini yapılandırılır ve VM için rapor sistem durumu verilerini yapılandırılır.

Ekleme işlemi sırasında size her bir portalda bildirim durumuna döndürmek için yukarıdaki durum olup olmadığını denetleyin.  Çalışma alanını ve aracı yüklemesini yapılandırmasını, genellikle 5-10 dakika sürer.  Görüntüleme izleme ve sistem durumu verileri portalında, bir ek 5-10 dakika sürebilir.  

Onboarding başlattınız yapılacak VM gerektiğini belirten bir ileti görürseniz, en fazla 30 dakika işlemi tamamlamak sanal makine için izin verir. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Sanal Makinem için performans grafikleri bazı veya tüm veriler göremiyorum
Disk tablodaki veya performans grafiklerini bazı performans verileri görmüyorsanız, ardından, performans sayaçları çalışma alanındaki yapılandırılmamış olabilir. Çözmek için aşağıdaki komutu çalıştırın. [PowerShell Betiği](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Nasıl Vm'leri Haritası özelliği için Azure İzleyici hizmeti eşlemesinden fark nedir?
Vm'leri Haritası özelliği için Azure İzleyici, hizmet eşlemesinde bağlıdır, ancak aşağıdaki farklılıklar vardır:

* Harita görünümünü Azure İzleyici altında yer alan VM'ler için VM dikey penceresinden ve Azure İzleyicisi'nden erişilebilir.
* Eşleme bağlantıları artık tıklanabilir öğelerdir ve yan panelinde seçili bağlantı için bağlantı ölçüm verilerinin bir görünümünü görüntüler.
* Haritalar daha iyi desteklemek için daha karmaşık eşlemeleri oluşturmak için kullanılan yeni bir API yoktur.
* İzlenen Vm'leri artık istemci grubu düğümü dahil edilir ve halka grafik grubunda izlenen ve izlenmeyen sanal makineler oranını gösterir.  Ayrıca, Grup genişletildiğinde makineler listesine filtre uygulamak için de kullanılabilir.
* İzlenen sanal makineler artık sunucu bağlantı noktası grubu düğümler dahil edilir ve halka grafik grubunda izlenen ve izlenmeyen makineler oranını gösterir.  Ayrıca, Grup genişletildiğinde makineler listesine filtre uygulamak için de kullanılabilir.
* Harita stil uygulama anlayışları'ndan Uygulama Haritası ile daha tutarlı olacak şekilde güncelleştirildi.
* Yan paneller güncelleştirildi, ancak henüz eksiksiz bir listesi, hizmet eşlemesi - güncelleştirme yönetimi, değişiklik izleme, güvenlik ve hizmet Masası desteklenen tümleştirme 's yoktur. 
* Harita grup ve makineleri seçme seçeneği güncelleştirildi ve artık abonelik, kaynak grupları, Azure sanal makine ölçek kümeleri ve bulut Hizmetleri destekler.
* Vm'leri Haritası özelliği için Azure İzleyici'de yeni hizmet eşlemesi makine grup oluşturamazsınız.  

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Neden benim performans grafiklerini noktalı satırları göster?

Bazı nedenlerden dolayı ortaya çıkabilir.  Durumlarda veri toplama boşluk olduğu şu satırları noktalı olarak kullanılırlar.  Veri örnekleme sıklığı etkin performans sayaçları için değiştirdiyseniz (varsayılan ayar 60 saniyede verileri toplamak için değer) örnekleme sıklığı, grafik bir dar zaman aralığını seçin ve noktalı grafik satırlarında görebilirsiniz küçüktür Grafikte kullanılan demet boyutuna (örneğin, örnekleme sıklığı 10 dakikada bir ve grafikteki her bir demete 5 dakikadır).  Görüntülemek için daha geniş bir zaman aralığı seçerek nokta yerine düz çizgiler olarak bu durumda görüntülenecek grafik satırları neden olmaz.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Grupları, sanal makineleri için Azure İzleyici ile desteklenir?
Evet, abonelik, kaynak grubu, alan gruplar görüntülenecek vm'lerden bilgileri toplarız bağımlılık aracısını yükledikten sonra sanal makine ölçek kümeleri ve bulut Hizmetleri.  Hizmet eşlemesi kullanarak ve makine gruplarını oluşturdunuz, bunlar da gösterilir.  Görüntülemekte olduğunuz için çalışma alanı oluşturduysanız bilgisayar grupları da grupları filtre görünür. 

## <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Nasıl ayrıntılarını görebilirim 95. yüzdebirlik yönlendirmek için performans grafikleri ve toplam satır?
Varsayılan olarak, listenin 95. yüzdebirlik seçilen ölçüm için en yüksek değeri en düşük değerini 5. yüzdebirlik makinelerle gösteren kullanılabilir bellek grafiğin dışında olan Vm'leri gösterecek şekilde sıralanır.  Grafikte tıklayarak açılır **en iyi N listesi** uygun ölçüm seçili görünüm.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>Nasıl eşleme özelliğini farklı sanal ağlar ve alt ağlar arasında yinelenen IP'ler işliyor?
IP aralıklarını çoğaltıyorsanız ile Vm'leri ya da Azure sanal makine ölçek kümeleri alt ağlar ve sanal ağlar arasında VM yanlış bilgileri görüntülemek harita Azure İzleyici neden olabilir. Bu bilinen bir sorundur ve bu deneyimi geliştirmek için seçenekleri inceliyoruz.

## <a name="does-map-feature-support-ipv6"></a>Özellik desteği IPv6 eşleşiyor mu?
Harita özelliği şu anda yalnızca IPv4 destekler ve IPv6 için destek inceliyoruz. IPv6 tünel IPv4 destekliyoruz.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Bir eşleme bir kaynak grubu veya diğer büyük grup için yük haritada görüntülemek zordur
Büyük ve karmaşık yapılandırmalar ele eşlemek için geliştirmeler yaptık karşın, bir harita çok fazla düğüm kümesi olarak çalışmaya düğümleri ve bağlantıları olabilir unutmayın.  Ölçeklenebilirliği artırmak için destek geliştirmek devam etmek için kararlıyız.   

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>Performans sekmesinde ağ grafik neden Azure VM genel bakış sayfasında ağ grafik farklı görünüyor?

Bir Azure VM için genel bakış sayfasında, konağın Konuk VM etkinliğinde ölçü temel grafik görüntüler.  Azure VM genel ağ grafik için faturalandırılır, ağ trafiği yalnızca görüntüler.  Bu, sanal ağlar arası trafik içermez.  VM'ler için Azure İzleyici için gösterilen grafikler ve veri Konuk VM verilerden dayanır ve ağ grafik gelen ve giden sanal ağlar arası dahil olmak üzere, bu VM'ye tüm TCP/IP'yi trafiğini gösterir.

## <a name="are-their-limitations-if-i-am-on-the-log-analytics-free-pricing-plan"></a>Log Analytics'i ücretsiz fiyatlandırma planına müşterisiysem, kendi sınırlaması var mı?
Log Analytics çalışma alanını kullanma ile Azure İzleyici yapılandırdıysanız *ücretsiz* Vm'leri eşleme özelliğini yalnızca beş bağlı makine destekleyeceği fiyatlandırma katmanı, Azure İzleyici, çalışma alanına bağlı. Boş bir çalışma alanına bağlı beş Vm'leriniz varsa, Vm'lerden birinin bağlantısını kesip daha sonra yeni bir VM bağlanmak yeni VM izlenen değil ve harita sayfada yansıtılır.  

Bu koşul altında ile istenir **şimdi deneyin** seçeneği VM açıp seçin **Insights (Önizleme)** sol bölmesinden sonra bile, sanal makinede yüklü.  Ancak, bu VM VM'ler için Azure İzleyici'na eklediğinizden yokmuş normal olarak ortaya çıkabilecek gibi seçeneklerle istenmez. 

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [sanal makineler için yerleşik Azure İzleyici](vminsights-onboard.md) gereksinimleri ve yöntemler, sanal makinelerin izlemeyi etkinleştirmek için anlamak için.
