---
title: Azure İzleyici (Önizleme) VM'ler için sık sorulan sorular | Microsoft Docs
description: VM'ler (Önizleme) için Azure İzleyici, sistem durumu ve performans izlemesi Azure VM'nin işletim sistemini bir araya getiren Azure bir çözümdür. Otomatik olarak uygulama bileşenleri ve bağımlılıkları diğer kaynaklarla bulur ve bunların arasındaki iletişimi eşler. Bu makalede, sık sorulan sorular yanıtlanmaktadır.
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
ms.openlocfilehash: f5865cf72f413db49e70a08305de54aff955607b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53075242"
---
# <a name="azure-monitor-for-vms-preview-faq"></a>Azure İzleyici VM'ler (Önizleme) ile ilgili SSS
Bu makalede, sanal makineler için Azure İzleyici hakkında sık sorulan soruların yanıtları. Çözümü hakkında ek sorularınız varsa, Git [Azure tartışma forumumuzdan](https://feedback.azure.com/forums/34192--general-feedback) ve sorularınızı gönderin. Sık sorulan sorular, hızla ve kolayca bulunabilir için bunları bu makaleye ekleriz.

## <a name="can-i-deploy-vms-to-an-existing-workspace"></a>Vm'leri mevcut bir çalışma alanına dağıtabilir miyim?
Sanal makinelerinizi bir Log Analytics çalışma alanınıza bağlıysa, VM'ler için bunları Azure İzleyici dağıttığınızda bu çalışma alanını kullanmaya devam edebilir. Çalışma alanı "Önkoşullar" bölümünde listelenen desteklenen bölgelerin birinde bulunmalıdır [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md#prerequisites).

Dağıtım sırasında çalışma alanı için performans sayaçlarını yapılandırıyoruz. Bu eylem bilgilerini toplamaya başlamak için çalışma alanına rapor verileri görüntüleyen Vm'leri ve analiz VM'ler için Azure İzleyici'de sağlar. Sonuç olarak, seçilen çalışma alanına bağlı olan sanal makinelerin tüm performans verileri görürsünüz. Sistem durumu ve harita özellikleri, dağıtım için belirttiğiniz sanal makineler için etkinleştirilir.

Hangi performans sayaçlarını etkinleştirilmiş daha fazla bilgi için bkz. [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).

## <a name="can-i-deploy-vms-to-a-new-workspace"></a>VM'ler için yeni bir çalışma alanı dağıtabilir miyim? 
Sanal makinelerinizi şu anda mevcut bir Log Analytics çalışma alanına bağlı değilseniz, verilerinizi depolamak için yeni bir çalışma alanı oluşturmanız gerekir. Otomatik olarak yapılandırarak tek bir VM için Azure İzleyici VM'ler için Azure portalında bir tane oluşturabilirsiniz.

Betik tabanlı bir yöntemini kullanmayı tercih ederseniz bkz [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md). 

## <a name="what-can-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>Sanal Makinem zaten mevcut bir çalışma alanına raporlama olursa ne yapabilirim?
Sanal makinelerinizden zaten veri topluyorsanız, zaten bunları mevcut bir Log Analytics çalışma alanına rapor verilerine yapılandırmış olabilir. Bu çalışma alanı, desteklenen bölgelerden birinde olduğu sürece, Azure İzleyici önceden mevcut olan bu çalışma alanı için VM'ler için etkinleştirebilirsiniz. Etkin bir şekilde ek bölgeler desteklemek için çalışıyoruz.

>[!NOTE]
>Bunları VM'ler için Azure İzleyici dağıtmayı seçtiğiniz kullanılıp kullanılmadığını belirten çalışma alanına rapor veren tüm sanal makineleri etkiler çalışma alanı için performans sayaçlarını yapılandırıyoruz. Performans sayaçları için bir çalışma alanı nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz: "Yapılandırma performans sayaçları" bölümünü [Log analytics'te Windows ve Linux performans veri kaynakları](../../azure-monitor/platform/data-sources-performance-counters.md). Azure İzleyici için yapılandırılmış VM'ler için sayaçları hakkında daha fazla bilgi için bkz: [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md). 

## <a name="why-did-my-vm-deployment-fail"></a>Neden VM dağıtımım başarısız oldu?
Azure portalında bir Azure VM'den dağıttığınızda aşağıdaki olaylar gerçekleşir:

* Varsayılan Log Analytics çalışma alanı, seçenek seçilen oluşturulur.
* Performans sayaçları, seçilen çalışma alanı için yapılandırılır. Bu adım başarısız olursa, bazı performans grafikleri ve tabloları dağıttığınız sanal makine için verileri görüntülemez. "PowerShell ile etkinleştir" kısmında belgelenen PowerShell betiğini çalıştırarak bu sorunu düzeltebilirsiniz [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md#enable-with-powershell).
* Log Analytics aracısını zorunludur, Azure Vm'leri üzerinde bir VM uzantısıyla yüklenir. 
* Zorunlu Vm'leri harita bağımlılık aracısı için Azure İzleyici Azure Vm'leri üzerinde bir uzantı ile yüklenir. 
* Gerekirse sağlığı özelliğini destekleyen azure İzleyici bileşenleri yapılandırılır ve VM için rapor sistem durumu verilerini yapılandırılır.

Dağıtım sırasında her önceki adımın durumunu denetlemek ve için portalda bildirim durumu dönersiniz. Çalışma alanını ve aracı yüklemesini yapılandırmasını, genellikle 5-10 dakika sürer. Azure portalında görüntüleme izleme ve sistem durumu verileri, bir ek 5-10 dakika sürebilir. 

Dağıtım başlattınız VM dağıtılması gerektiğini belirten bir ileti görürseniz, en fazla 30 dakika işlemi tamamlamak sanal makine için izin verin. 

## <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>Sanal Makinem için performans grafikleri bazı veya tüm veriler göremiyorum
Performans verileri disk tablonun veya performans grafiklerini görüntülenmiyorsa, çalışma alanında, performans sayaçları yapılandırılmamış olabilir. Bu sorunu çözmek için "PowerShell ile etkinleştir" kısmında belgelenen PowerShell betiğini Çalıştır [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md#enable-with-powershell).

## <a name="how-is-the-azure-monitor-for-vms-map-feature-different-from-service-map"></a>Vm'leri Haritası özelliği için Azure İzleyici hizmeti eşlemesinden farklı mı?
Hizmet eşlemesinde Vm'leri eşleme özelliğini Azure İzleyicisi'ni temel alır, ancak aşağıdaki farklar vardır:

* Azure İzleyici altında yer alan VM'ler için VM bölmesinden ve Azure İzleyici'den harita görünümünü erişebilirsiniz.
* Eşleme bağlantıları artık tıklanabilir ve yan panelde bağlantı ölçüm verileri görüntüler.
* Yeni bir API, haritalar daha iyi desteklemek için daha karmaşık eşlemeleri oluşturmak için kullanılır.
* İzlenen Vm'leri olan istemci grubu düğümünü ve halka grafik izlenmeyen için izlenen sanal makineler oranını gösterir. Grup genişletildiğinde ayrıca makineler listesini filtreleyebilirsiniz.
* İzlenen sanal makineler artık sunucu bağlantı noktası grubu düğümlerin ve izlenen için izlenmeyen makineler oranını halka grafiği görüntüler. Grup genişletildiğinde ayrıca makineler listesini filtreleyebilirsiniz.
* Harita Stil Uygulama Haritası Azure Application ınsights'tan daha tutarlı olması için güncelleştirildi.
* Yan paneller güncelleştirildi ancak hizmet eşlemesinde desteklendi tümleştirmeler kümesini henüz mevcut değil: güncelleştirme yönetimi, değişiklik izleme, güvenlik ve hizmet Masası. 
* Harita grup ve makineleri seçme seçeneği güncelleştirildi. Şimdi, abonelik, kaynak grupları, Azure sanal makine ölçek kümeleri ve bulut hizmetleri de destekler.
* Vm'leri Haritası özelliği için Azure İzleyici'de yeni hizmet eşlemesi makine grup oluşturamazsınız. 

## <a name="why-do-my-performance-charts-show-dotted-lines"></a>Neden benim performans grafiklerini noktalı satırları göster?

Performans grafiklerini noktalı çizgiler için birkaç nedenden Kesiksiz çizgi yerine görüntülenir:
* Veri toplama boşluk olabilir. 

* 60 saniyede veri örnekleme için varsayılan ayardır. Grafik bir dar zaman aralığını seçin ve, örnekleme sıklığı grafikte kullanılan kova boyutu küçüktür ise noktalı satırları görebilirsiniz. Diyelim ki, 10 dakikalık bir örnekleme sıklığı seçtiğiniz ve grafikteki her bir demete 5 dakikadır. Bu durumda, görüntülemek için daha geniş bir zaman aralığı seçerek nokta yerine düz çizgiler olarak görüntülenecek grafik satırları neden olmaz.

## <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Grupları, sanal makineleri için Azure İzleyici ile desteklenir?
Evet, abonelik, kaynak grubu, alan gruplar görüntülenecek vm'lerden bilgileri toplarız bağımlılık aracısını yükledikten sonra sanal makine ölçek kümeleri ve bulut Hizmetleri. Hizmet eşlemesi kullanarak ve makine gruplarını oluşturdunuz, bu grupları da görüntülenir. Görüntülemekte olduğunuz için çalışma alanı oluşturduysanız bilgisayar grupları da grupları filtre görünür. 

## <a name="how-can-i-display-the-details-about-whats-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Ayrıntıları nasıl görüntüleyebilirim 95. yüzdebirlik önünü açıyor hakkında ve toplam performans grafiklerini satır?
Varsayılan olarak, listenin 95. yüzdebirlik seçilen ölçüm için için en yüksek değer olan Vm'leri gösterecek şekilde sıralanır. Bir özel durum **kullanılabilir bellek** beşinci yüzdelik dilim değeri en düşük makinelerle gösteren grafik. Açmak için grafiğe seçin **en iyi N listesi** uygun ölçüm seçili görünüm.

## <a name="how-does-the-map-feature-handle-duplicate-ips-across-various-virtual-networks-and-subnets"></a>Nasıl eşleme özelliğini çeşitli sanal ağlar ve alt ağlar arasında yinelenen IP'ler işliyor?
Ya da sanal makineleri kullanarak IP aralıklarını çoğaltma, veya alt ağlar ve sanal ağlar arasında Azure sanal makine ölçek kümeleri, Azure İzleyici Vm'leri Haritası özelliği için yanlış bilgileri görüntülemek neden. Biz bu sorunun farkındadır ve deneyimini geliştirmek için seçenekleri inceliyoruz.

## <a name="does-the-map-feature-support-ipv6"></a>IPv6 eşleme özelliğini destekliyor mu?
Harita özelliği şu anda yalnızca IPv4 destekler ve IPv6 için destek inceliyoruz. IPv6 tünel IPv4 destekliyoruz.

## <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-why-is-the-map-difficult-to-view"></a>Neden bir kaynak grubu veya diğer büyük grup için bir harita yükleyebilirim, haritada görüntülemek zor mu?
Büyük ve karmaşık yapılandırmalar işlemek için eşleme özelliğini geliştirdik olsa da, bir harita, birçok düğüm, bağlantılar ve bir küme olarak çalışan düğümleri olabilir farkında olun. Ölçeklenebilirliği artırmak için destek geliştirmek devam etmek için kararlıyız.  

## <a name="why-does-the-network-chart-on-the-performance-tab-look-different-from-the-network-chart-on-the-azure-vm-overview-page"></a>Performans sekmesinde ağ grafik neden Azure VM genel bakış sayfasında ağ planından farklı görünüyor?

Bir Azure VM için genel bakış sayfasında, konağın Konuk VM etkinliğinde ölçü temel grafik görüntüler. Azure VM genel bakış sayfasında ağ grafik faturalandırılır ağ trafiğini görüntüler. Bu görüntü, sanal ağlar arasında trafik içermez. Azure İzleyici VM'ler için VM Konuk verileri temel alır ve tüm TCP/IP ağ grafik görüntüler için gösterilen grafikler ve veri, trafiği olan gelen ve giden sanal ağlar arasında trafiği de dahil olmak üzere, bu VM'ye.

## <a name="what-are-the-limitations-of-the-log-analytics-free-pricing-plan"></a>Log Analytics fiyatlandırma planınız ücretsiz sınırlamaları nelerdir?
Azure İzleyici ile bir Log Analytics çalışma alanı kullanarak yapılandırdığınız varsa *ücretsiz* fiyatlandırma katmanı, Azure İzleyici Vm'leri Haritası özelliği için çalışma alanına bağlanan yalnızca beş makine destekler. 

Örneğin, beş Vm'leri boş bir çalışma alanına bağlı olan varsayalım. Bir sanal makine bağlantısını kesmek ve yeni bir tane daha sonra bağlanmak, yeni VM izlenen ve harita ekranında değil. Yeni VM açtığınızda bu senaryoda, kullanılacak istenir **şimdi deneyin** seçeneğini işaretleyip **Insights (Önizleme)** sol bölmeden bile sanal makinede yüklendikten sonra. Ancak, VM için Azure İzleyici VM'ler için dağıtılan olmayan, normalde olduğu gibi seçeneklerle istenmez. 

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinelerinizi izlemeyi etkinleştirme yöntemlerini ve gereksinimleri hakkında bilgilere [VM'ler (Önizleme) için Azure İzleyici'ı Dağıtma](vminsights-onboard.md).
