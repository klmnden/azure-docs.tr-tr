---
title: Performans izleme özelliğini Azure Log analytics'te Ağ Performansı İzleyicisi çözümü | Microsoft Docs
description: Ağ Performans İzleyicisi'nde Performans İzleyicisi özelliği çeşitli noktalarında ağınız arasında ağ bağlantısı izlemenize yardımcı olur. Bulut dağıtımları ve şirket içi konumlara izleyebilirsiniz birden çok veri merkezleri ve şube ofislerinde ve görev açısından kritik çok katmanlı uygulamalar veya mikro hizmetler.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: absha
ms.openlocfilehash: bb99689409ddff311e556250083b99842bc59927
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65963523"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Performans izleme çözümü ağ: Performans izleme

Performans İzleyicisi özelliği [Ağ Performansı İzleyicisi](network-performance-monitor.md) çeşitli noktalarında ağınız genelinde izlemenizi ağ bağlantısı. Bulut dağıtımları ve şirket içi konumlara izleyebilirsiniz birden çok veri merkezleri ve şube ofislerinde ve görev açısından kritik çok katmanlı uygulamalar veya mikro hizmetler. Kullanıcılarınız şikayet etmeden önce Performans İzleyicisi ile ağ sorunlarını algılayabilir. Başlıca avantajları şunlardır: 

- Çeşitli alt ağlar kaybı ve gecikme süresi izleme ve uyarılar ayarlayın.
- Ağdaki tüm yolları (yedekli yolları dahil) izleyin.
- Çoğaltması zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme.
- Ağdaki performansın düşmesine neden olan segmenti belirleme.
- SNMP'ye gerek duymadan ağın sistem durumunu izleyin.


![Ağ Performansı İzleyicisi](media/network-performance-monitor-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Yapılandırma
Yapılandırma için Ağ Performansı İzleyicisi'ni açmak için açık [Ağ Performansı İzleyicisi çözüm](network-performance-monitor.md)seçip **yapılandırma**.

![Ağ Performansı İzleyicisi'ni yapılandırma](media/network-performance-monitor-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Yeni ağlar oluşturma

Ağ Performans İzleyicisi'nde bir ağ, alt ağlar için mantıksal bir kapsayıcıdır. Ağ altyapınızın ihtiyaçlarınıza göre izleme düzenlemenize yardımcı olur. Kolay bir ada sahip bir ağı oluşturma ve iş mantığınızı göre alt ağları ekleyin. Örneğin, Londra adlı bir ağ oluşturun ve tüm alt ağları Londra veri Merkezinize ekleyin. Ya da adlı bir ağ oluşturabilirsiniz *ContosoFrontEnd* ve uygulamanızın ön uç hizmet Contoso adlı tüm alt ağlar bu ağa ekleyin. Çözüm, ortamınızda bulunan tüm alt ağları içeren bir varsayılan ağ otomatik olarak oluşturur. 

Bir ağ oluşturduğunuzda, bir alt ağ ekleyin. Ardından bu alt varsayılan ağdan kaldırılır. Bir ağ silerseniz, tüm alt ağlar varsayılan ağa otomatik olarak döndürülür. Varsayılan ağ herhangi bir kullanıcı tarafından tanımlanan ağ içinde yer alan olmayan alt ağlar için kapsayıcı görevi görür. Düzenleyemez veya varsayılan ağ silin. Her zaman, sistemde kalır. Gereksinim duyduğunuz kadar çok özel ağlar oluşturabilirsiniz. Çoğu durumda, alt ağlar, kuruluşunuzda birden fazla ağ düzenlenir. İş mantığınızı için alt grubuna bir veya daha fazla ağ oluşturun.

Yeni bir ağ oluşturmak için:


1. Seçin **ağları** sekmesi.
1. Seçin **Ekle ağ**ve ardından ağ adı ve açıklama girin. 
2. Bir veya daha fazla alt ağ seçin ve ardından **Ekle**. 
3. Seçin **Kaydet** yapılandırmayı kaydetmek için. 


### <a name="create-monitoring-rules"></a>İzleme kuralları oluşturma 

Performans İzleyicisi, ağ bağlantılarının iki ağ arasında veya iki alt ağlar arasında performans eşiği aşıldığında sistem durumu olaylarını oluşturur. Sistem, bu eşikleri otomatik olarak bilgi edinebilirsiniz. Özel eşikler de sağlayabilirsiniz. Sistem, sistem öğrenilen eşiği ihlallerini kayıp veya gecikme süresi çiftinde ağ veya alt ağ arasında bağlantılar her bir sistem durumu olayı oluşturan bir varsayılan kuralı otomatik olarak oluşturur. Bu işlem, tüm izleme kurallarını açıkça oluşturmadınız kadar ağ altyapınızı izleme çözümü yardımcı olur. Varsayılan kural etkin olduğunda tüm düğümleri yapay işlemler izleme için etkinleştirilmiş tüm diğer düğümlere gönderin. Varsayılan kural küçük ağlarla yararlı olur. Burada bir mikro hizmet çalıştıran sunucuların küçük bir sayı olması ve tüm sunucuları birbirlerine sahip olduğunuzdan emin olun, istediğiniz bir senaryo buna bir örnektir.

>[!NOTE]
> Varsayılan kuralı devre dışı bırak ve özellikle çok sayıda düğümü izlemek için kullandığınız büyük ağlar ile özel izleme kuralları oluşturma öneririz. Özel İzleme kuralları ağınızı izleme düzenlemenize yardımcı ve çözüm tarafından oluşturulan trafiğin azaltabilir.

İş mantığınızı göre izleme kuralları oluşturun. Ağ bağlantısı iki office sitelerin genel merkeze performansını izlemek istiyorsanız bir örnektir. Office site1 O1 ağındaki tüm alt ağların gruplandırın. Ardından office site2 O2 ağındaki tüm alt ağların gruplandırın. Son olarak, tüm alt ağların merkezi ağ h oluşturma iki izleme kuralları--O1 H arasındaki ve diğeri O2 h arasındaki Grup 

Özel İzleme kuralları oluşturmak için:

1. Seçin **Kuralı Ekle** üzerinde **İzleyici** sekmesini tıklatın ve kural adı ve açıklama girin.
2. Çiftinin listelerden izlemek için ağ veya alt ağ bağlantıları'nı seçin. 
3. İçeren alt ağlar, ağ aşağı açılan listeden istediğiniz ağı seçin. Daha sonra alt ağlar, karşılık gelen alt ağ aşağı açılan listeden seçin. Bir ağ bağlantısı tüm alt ağlar izlemek isteyip istemediğinizi seçin **tüm alt ağlar**. Benzer şekilde, istediğiniz diğer alt ağlar'ı seçin. Yaptığınız seçimleri belirli bir alt ağ bağlantıları için izleme dışlanacak seçin **Ekle özel durum**. 
4. ICMP arasında TCP protokolleri yapay işlemleri yürütmek için seçin. 
5. Öğeler için sistem durumu olayları oluşturmak istemiyorsanız, Temizle seçtiğiniz **bu kural tarafından kapsadığı bağlantılarda sistem durumu izlemeyi etkinleştir**. 
6. İzleme koşulları seçin. Sistem durumu olayı oluşturma için özel eşikler ayarlamak için eşik değerleri girin. Koşul değeri seçilen ağ veya alt ağ çifti için seçilen eşiğini aştığında sistem durumu olayı oluşturulur. 
7. Seçin **Kaydet** yapılandırmayı kaydetmek için. 

Bir izleme kuralını kaydettikten sonra bu kural uyarı yönetimi ile seçerek tümleştirebilirsiniz **oluşturma uyarı**. Arama sorgusuyla bir uyarı kuralı oluşturulur. Diğer gerekli parametreleri otomatik olarak doldurulur. Bir uyarı kuralı kullanarak, Ağ Performansı İzleyicisi içinde'var olan uyarılar ek olarak, e-posta tabanlı uyarılar alabilirsiniz. Uyarılar ayrıca runbook'ları ile düzeltici eylemleri tetikleyebilirsiniz veya Web kancalarını kullanarak var olan hizmet yönetim çözümleriyle tümleştirilebilir. Seçin **yönetme uyarı** uyarı ayarlarını düzenlemek için. 

Artık daha fazla Performans İzleyicisi kuralları oluşturun veya çözüm panosuna özelliğini kullanarak taşıyabilirsiniz.

### <a name="choose-the-protocol"></a>Protokol seçin

Ağ Performansı İzleyicisi, paket kaybı ve bağlantı gecikme süresi gibi ağ performans ölçümlerini hesaplamak için yapay işlemler kullanır. Bu kavramı daha iyi anlamak için bağlı bir ağ performansı İzleyicisi Aracısı göz önünde bulundurun. tek bir ağ bağlantısı sonu. Bu Ağ Performansı İzleyicisi Aracısı başka bir ağ sonuna bağlı ikinci bir ağ performansı İzleyicisi aracı araştırma paketleri gönderir. İkinci aracı yanıt paketleri ile yanıtlar. Bu işlemi birkaç kez yineler. Yanıtlar ve her yanıt almak için geçen süre sayısını ölçme, ilk ağ performansı İzleyicisi aracı bağlantı gecikmesi değerlendirir ve paket bırakılır. 

Biçim, boyutu ve bu paketleri bir dizi izleme kuralları oluşturduğunuzda seçtiğiniz protokolü tarafından belirlenir. Paketlerin protokolüne dayalı, Orta ağ cihazları, yönlendiriciler ve anahtarlar gibi bu paketleri farklı işleyebilir. Sonuç olarak, Protokolü seçtiğiniz sonuçları doğruluğunu etkiler. Ağ Performansı İzleyicisi çözümü dağıttıktan sonra el ile herhangi bir adım atmasına Protokolü seçtiğiniz de belirler. 

Ağ Performansı İzleyicisi, yürütme yapay işlemler için ICMP ve TCP protokollerini arasında seçim yapma olanağı sunar. Yapay işlem kuralı oluşturduğunuzda, ICMP seçerseniz, Ağ Performansı İzleyicisi aracıları paket kaybı ve ağ gecikmesini hesaplamak için ICMP YANKI iletileri kullanın. ICMP YANKI geleneksel ping yardımcı programı tarafından gönderilen iletinin kullanır. Protokol olarak TCP kullandığınızda, Ağ Performansı İzleyicisi aracıları TCP SYN paketlerin ağ üzerinden göndermek. Bu adım bir TCP el sıkışması tamamlama tarafından izlenir ve bağlantı lk paketlerini kullanarak kaldırılır. 

Bir protokol seçin önce aşağıdaki bilgileri göz önünde bulundurun: 

* **Birden çok ağ yollarını bulma.** Birden çok yol keşfederken TCP daha doğru olmasını ve daha az aracıları her alt ağ gerekir. Örneğin, TCP kullanan bir veya iki aracılar, alt ağlar arasındaki tüm yedekli yollar bulabilir. Benzer sonuçlar elde etmek için ICMP kullanan bazı aracılar ihtiyacınız vardır. ICMP, yolların iki alt ağlar arasında bir sayı varsa kullanarak, birden çok kaynak veya hedef alt ağdaki 5N aracıları gerekir.

* **Sonuçları doğruluğunu.** ICMP YANKI paketleri TCP paketleri kıyasla daha düşük bir öncelik atamak için yönlendiricileri ve anahtarlarını eğilimindedir. Ağ aygıtlarını yoğun olarak yüklendiğinde, belirli durumlarda, daha yakından TCP tarafından alınan veri kaybı ve uygulamalar tarafından karşılaşılan gecikme sürelerini yansıtır. Bu uygulama trafiği çoğunu akışlar için TCP üzerinden oluşur. Böyle durumlarda, ICMP TCP'ye kıyasla daha az doğru sonuçlar sağlar. 

* **Güvenlik duvarı yapılandırması.** TCP protokolü TCP paketleri bir hedef bağlantı noktasına gönderilir gerektirir. Ağ Performansı İzleyicisi Aracılar tarafından kullanılan varsayılan bağlantı noktası 8084 ' dir. Aracıları yapılandırdığınızda, bağlantı noktasını değiştirebilirsiniz. Ağ güvenlik duvarları veya ağ güvenlik grubu (NSG) kuralları (azure'da) bağlantı noktası üzerinde trafiğe izin verdiğinden emin olun. Aracıların yüklü olduğu bilgisayarlarda yerel güvenlik duvarının Bu bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırıldığından emin emin olmanız gerekir. Windows çalıştıran bilgisayarlarda güvenlik duvarı kurallarını yapılandırmak için PowerShell komut dosyalarını kullanabilirsiniz, ancak ağ güvenlik duvarını elle yapılandırmanız gerekir. Buna karşılık, bir bağlantı noktası kullanarak ICMP çalışmaz. Çoğu Kurumsal senaryolarda ICMP trafiğine, ping yardımcı programı gibi Ağ Tanılama Araçları'nı kullanmanıza izin vermek için güvenlik duvarlarından izin verilir. Başka bir makineden ping komutu gönderebilir, güvenlik duvarları el ile yapılandırmak zorunda kalmadan ICMP protokolünü kullanabilirsiniz.

>[!NOTE] 
> Bazı güvenlik duvarları, çok sayıda olayları güvenlik bilgileri ve Olay yönetimi sisteminizde sonuçları yeniden iletim için yol açabilecek ICMP engelleyebilir. Seçtiğiniz protokolü bir ağ güvenlik duvarı veya NSG tarafından engellenen olmadığından emin olun. Aksi takdirde, Ağ Performansı İzleyicisi, ağ kesimine izleyemez. İzleme için TCP kullanmanızı öneririz. ICMP, burada, TCP gibi ne zaman kullanamazsınız senaryolarda kullanın: 
>
> - Windows istemcisi tabanlı düğümler, TCP ham yuva Windows istemcileri izin verilmediğinden kullanın.
> - Ağ güvenlik duvarınız veya NSG TCP engeller.
> - Protokol geçiş bilmiyorum.

Dağıtım sırasında ICMP kullanmayı seçerseniz, varsayılan kuralı izleme düzenleyerek dilediğiniz zaman TCP'ye geçiş yapabilirsiniz.

1. Git **ağ performansı** > **İzleyici** > **yapılandırma**   >  **İzleyici**. Ardından **varsayılan kural**. 
2. Kaydırma **Protokolü** bölümünde ve kullanmak istediğiniz protokolü seçin. 
3. Seçin **Kaydet** ayarı uygulamak için. 

Varsayılan kural belirli bir protokol kullansa bile, farklı bir protokol yeni kurallar oluşturabilirsiniz. Burada bazı kurallar ICMP kullanın ve TCP diğerlerinde kuralları bir karışımını bile oluşturabilirsiniz. 

## <a name="walkthrough"></a>Kılavuz 

Artık basit bir sistem durumu olayı kök nedenini araştırmasını bakın.

Çözüm panosunda sistem durumu olayı, bir ağ bağlantısı kötü durumda olduğunu gösterir. Sorunu araştırmaya seçin **ağ bağlantıları izlenmekte olan** Döşe.

Detaya gitme sayfasını gösteren **DMZ2 DMZ1** sağlıksız ağ bağlantısı. Seçin **alt ağ bağlantılarını görüntüle** bu ağ bağlantısı. 


Detaya gitme sayfanın tüm alt ağ bağlantıları gösterir **DMZ2 DMZ1** ağ bağlantısı. Her iki alt ağ bağlantıları için gecikme süresi ağ bağlantısını sağlıksız hale eşiği aşıldı. Her iki alt ağ bağlantıları, gecikme süresi eğilimleri de görebilirsiniz. Zaman seçimi denetlemek gerekli zaman aralığına odaklanmak için grafikteki kullanın. En yüksek gecikme süresi dolduğunda günün saatini görebilirsiniz. Sorunu araştırmaya daha sonra bu zaman aralığı için günlükleri arayın. Seçin **düğüm bağlantılarını görüntüle** daha fazla detaya gitmek için. 
 
 ![Alt ağ bağlantıları sayfası](media/network-performance-monitor-performance-monitor/subnetwork-links.png) 

Önceki sayfaya benzer, belirli bir alt ağ bağlantısı için detaya gitme sayfası onun bağlı düğüm bağlantıları listeler. Benzer eylemleri gerçekleştirmek önceki adımda yaptığınız gibi buradan. Seçin **topolojisini görüntüleme** iki düğüm arasındaki topolojiyi görüntülemek için. 
 
 ![Düğüm bağlantılar sayfası](media/network-performance-monitor-performance-monitor/node-links.png) 

Seçilen iki düğüm arasındaki tüm yolları topoloji Haritası çizilir. Atlama atlama düzeyinde topolojisini yollarının topolojisi haritası üzerinde iki düğüm arasında görselleştirebilirsiniz. İki düğüm arasında ne kaç yollar mevcut NET bir resim sunan yolları veri paketlerinin yararlanın. Ağ performans sorunlarını kırmızı renkte gösterilir. Hatalı ağ bağlantısı ya da hatalı ağ cihazı bulmak için topoloji harita üzerinde kırmızı öğesini arayın. 

 ![Topoloji Haritası ile topoloji Panosu](media/network-performance-monitor-performance-monitor/topology-dashboard.png) 

Kaybı, gecikme süresi ve her yolunda durak sayısını inceleyebilirsiniz **eylem** bölmesi. Sağlıksız yollar ayrıntılarını görüntülemek için kaydırma çubuğunu kullanın. Sağlıksız atlama ile yolları seçin, böylece yalnızca seçilen yollar için topoloji çizilen için filtreleri kullanın. İçine veya dışına topoloji Haritası yakınlaştırma için fare tekerleğini kullanın. 

Aşağıdaki görüntüde, belirli ağ bölümüne sorunlu alanları kök nedenini kırmızı yolları ve atlama görünür. Topoloji haritasında FQDN ve IP adresini içeren düğümünün özellikleri açığa çıkarmak için bir düğüm seçin. Bir atlama seçildiğinde, atlama IP adresi gösterilir. 
 
![Seçili düğümü özellikleri ile bir topoloji Haritası](media/network-performance-monitor-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](../../azure-monitor/log-query/log-query-overview.md) ayrıntılı ağ performansı verileri kayıtları görüntülemek için.
