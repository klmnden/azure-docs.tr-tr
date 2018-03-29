---
title: Performans İzleyicisi'ni Özelliği Azure günlük analizi Ağ Performansı İzleyicisi çözümünde | Microsoft Docs
description: Performans İzleyicisi yetenek ağ Performans İzleyicisi'nde, ağınızdaki çeşitli noktaları arasında ağ bağlantısı izlemenize yardımcı olur. Bulut dağıtımları ve şirket içi konumlara izleyebilirsiniz birden fazla veri merkezleri ve şube ofisleri ve kritik çok katmanlı uygulama veya mikro.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 65497548d0b8066627be25520c28d39491918d09
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>Ağ Performans İzleyicisi çözüm: performans izleme

Performans İzleyicisi'ni özelliği [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) izlemenizi sağlar, ağınızdaki çeşitli noktaları arasında bağlantı ağ. Bulut dağıtımları ve şirket içi konumlara izleyebilirsiniz birden fazla veri merkezleri ve şube ofisleri ve kritik çok katmanlı uygulama veya mikro. Performans izleme ile kullanıcılarınız şikayetçi önce ağ sorunları algılayabilir. Anahtar avantajları şunlardır: 

- Kaybı ve gecikme çeşitli alt ağlar arasında izlemek ve Uyarıları ayarlayın.
- Ağdaki tüm yolları (yedekli yollar dahil) izleyin.
- Çoğaltma zor olan geçici ve zaman içinde nokta ağ sorunları giderin.
- İçin performans sorumlu olduğu ağ belirli kesiminde belirler.
- SNMP gerek kalmadan ağ durumunu izleyin.


![Ağ Performansı İzleyicisi](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Yapılandırma
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md)seçip **yapılandırma**.

![Ağ Performans İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Yeni ağ oluşturma

Ağ Performans İzleyicisi'nde bir ağ, alt ağlar için mantıksal bir kapsayıcısıdır. İhtiyaçlarınıza göre ağ altyapınızın izleme düzenlemenize yardımcı olur. Bir kolay adla bir ağ oluşturun ve alt ağlarını iş mantığınızı göre ekleyin. Örneğin, Londra adlı bir ağ oluşturun ve tüm alt Londra veri merkezinizde ekleyin. Veya adlı bir ağ oluşturabilirsiniz *ContosoFrontEnd* ve bu ağa, uygulamanızın ön uç hizmet Contoso adlı tüm alt ağlar ekleyin. Çözüm, ortamınızda bulunan tüm alt ağlar içeren bir varsayılan ağ otomatik olarak oluşturur. 

Bir ağ oluşturduğunuzda, bir alt ağ ekleyin. Ardından bu alt ağ varsayılan ağdan kaldırılır. Bir ağ silerseniz, tüm alt varsayılan ağa otomatik olarak döndürülür. Varsayılan ağ herhangi bir kullanıcı tarafından tanımlanan ağ içinde yer alan olmayan tüm alt ağlar için kapsayıcı görevi görür. Düzenleyemez veya varsayılan ağ silin. Her zaman, sistemde kalır. Gereksinim duyduğunuz kadar çok özel ağlar oluşturabilirsiniz. Çoğu durumda, alt ağlar, kuruluşunuzda birden fazla ağ düzenlenir. İş mantığınızın için alt ağlar gruplandırmak için bir veya daha fazla ağ oluşturun.

Yeni bir ağ oluşturmak için:


1. Seçin **ağlar** sekmesi.
1. Seçin **Ekle ağ**ve ardından ağ adı ve açıklama girin. 
2. Bir veya daha fazla alt ağ seçin ve ardından **Ekle**. 
3. Seçin **kaydetmek** yapılandırmayı kaydetmek için. 


### <a name="create-monitoring-rules"></a>İzleme kuralları oluşturma 

Performans İzleyicisi iki ağ arasında veya iki alt ağlar arasında ağ bağlantıları performans eşiği aşıldığında sistem durumu olayları oluşturur. Sistem bu eşikler otomatik olarak bulabilir. Özel eşikler sağlar. Sistem, kayıp ya da ağ veya alt ağ herhangi bir çifti arasındaki gecikme süresi ihlallerini bağlantılar her bir sistem durumu olayı oluşturan varsayılan bir kural sistem öğrenilen eşiği otomatik olarak oluşturur. Bu işlem, ağ altyapınızın tüm izleme kurallarını açıkça oluşturmadınız kadar izleme çözüm yardımcı olur. Varsayılan kural etkinleştirilirse, tüm düğümlere yapay işlemler izleme için etkinleştirilmiş tüm diğer düğümlere gönderin. Varsayılan kural küçük ağlarla yararlıdır. Örneği burada bir mikro hizmet çalıştıran sunucuların küçük bir sayıya sahip ve tüm sunucuların bağlantınız birbirlerine emin olmak istediğiniz bir senaryodur.

>[!NOTE]
> Varsayılan kural devre dışı bırakın ve özellikle çok sayıda düğümü izleme için kullandığınız büyük ağlar ile özel izleme kurallarını oluşturun öneririz. Özel İzleme kurallarını çözüm ve ağınızı izleme düzenlemenize yardım tarafından oluşturulan trafiğin azaltabilir.

İş mantığınızın göre izleme kurallarını oluşturun. Genel merkez iki office sitelere ağ bağlantısını performansını izlemek istiyorsanız bir örnektir. Office site1 O1 ağındaki tüm alt ağlar grup. Sonra office site2 O2 ağındaki tüm alt gruplayın. Son olarak, Genel merkez ağ H. oluşturmak iki izleme kurallarında--O1 H arasındaki tek ve diğer O2 H. arasındaki tüm alt ağlar Grup 

Özel İzleme kurallarını oluşturmak için:

1. Seçin **Kuralı Ekle** üzerinde **İzleyici** sekmesini tıklatın ve kural ad ve açıklama girin.
2. Listelerden izlemek için ağ veya alt ağ bağlantıları çifti seçin. 
3. Ağ aşağı açılan listesinden istediğiniz ağlarla içeren ağ seçin. Ardından ağlarla ilgili alt ağ aşağı açılan listeden seçin. Bir ağ bağlantısı tüm alt ağlar izlemek istiyorsanız seçin **tüm alt ağlar**. Benzer şekilde, istediğiniz diğer ağlarla seçin. Yaptığınız seçimlerin gelen belirli alt ağ bağlantıları için izleme dışlamak için seçin **eklemek özel durum**. 
4. ICMP ve TCP arasında yapay işlemleri yürütmek için protokolleri seçin. 
5. Öğelerin sistem durumu olayları oluşturmak istemiyorsanız, Temizle seçtiğiniz **bağlantılarında sistem durumu izleme etkinleştirmek bu kural tarafından kapsanan**. 
6. İzleme koşulları seçin. Sistem durumu olayı oluşturma için özel eşikler ayarlamak için eşik değerleri girin. Koşul değeri seçili ağ veya alt ağ çifti için seçilen eşiğini aştığında sistem durumu olayı oluşturulur. 
7. Seçin **kaydetmek** yapılandırmayı kaydetmek için. 

Bir izleme kuralı kaydettikten sonra bu kural uyarı yönetimi ile seçerek tümleştirebilirsiniz **oluşturma uyarı**. Bir uyarı kuralı arama sorgusu ile birlikte otomatik olarak oluşturulur. Diğer gerekli parametreleri otomatik olarak doldurulur. Bir uyarı kuralı kullanarak, Ağ Performansı İzleyicisi içinde var olan uyarılar ek olarak, e-posta tabanlı uyarılar alabilir. Uyarılar ayrıca eylemlerden runbook'larla tetikleyebilir veya Web kancalarını kullanarak mevcut hizmet yönetimi çözümleriyle tümleştirilebilir. Seçin **yönetmek uyarı** uyarı ayarlarını düzenlemek için. 

Şimdi, daha fazla Performans İzleyicisi kuralları oluşturun veya özellikten yararlanabilmek için çözüm panosuna taşıyın.

### <a name="choose-the-protocol"></a>Protokol seçin

Ağ Performansı İzleyicisi yapay işlemler paket kaybı ve bağlantı gecikme süresi gibi ağ performans ölçümlerini hesaplamak için kullanır. Bu kavram daha iyi anlamak için bağlı bir ağ performansı İzleyicisi Aracısı göz önünde bulundurun. bir ağ bağlantısı ucunu. Bu Ağ Performansı İzleyicisi Aracısı başka bir ağ sonuna bağlı ikinci bir ağ performansı İzleyicisi Aracısı araştırma paketleri gönderir. İkinci aracı yanıt paketleri ile yanıtlar. Bu işlem birkaç kez yineler. Yanıtlar ve her yanıt almak için geçen süre sayısını ölçme, ilk ağ performansı İzleyicisi Aracısı bağlantı gecikme değerlendirir ve paket bırakma. 

Biçim, boyutu ve bu paketlerin dizisini izleme kurallarını oluştururken seçtiğiniz protokolü tarafından belirlenir. Paketlerin protokolünü temel, yönlendiriciler ve anahtarlar gibi ara ağ aygıtlarını bu paketleri farklı işlem. Sonuç olarak, Protokolü tercih ettiğiniz sonuçları doğruluğunu etkiler. Protokol seçiminiz Ayrıca Ağ Performansı İzleyicisi çözümü dağıttıktan sonra herhangi bir el ile adımlar atmanız gerekir olup olmadığını belirler. 

Ağ Performansı İzleyicisi yapay işlemleri yürütmek ICMP ve TCP protokollerini arasında seçim sunar. Yapay işlem kuralı oluşturduğunuzda, ICMP seçerseniz, Ağ Performansı İzleyicisi aracıları ağ gecikme süresi ve paket kaybı hesaplamak için ICMP YANKI iletisi kullanın. ICMP YANKI geleneksel ping yardımcı programı tarafından gönderilen aynı iletiyi kullanır. Protokol olarak TCP'yi kullandığınızda, Ağ Performansı İzleyicisi aracıları ağ üzerinden TCP Eşitlemeye paketleri gönderir. Bu adım bir TCP el sıkışma tamamlama tarafından izlenir ve bağlantı RST paketlerini kullanarak kaldırılır. 

Bir protokol seçmeden önce aşağıdaki bilgileri göz önünde bulundurun: 

* **Birden çok ağ yollarını bulma.** Birden çok yol keşfederken TCP daha doğru olduğunu ve her alt ağda daha az aracıları gerekiyor. Örneğin, TCP kullanan bir veya iki aracılar alt ağlar arasındaki tüm yedekli yollar bulabilir. Benzer sonuçlar elde etmek için ICMP kullanan birkaç aracılar gerekir. Rotaların iki alt ağlar arasında bir sayı varsa, ICMP, kullanmadan, birden çok kaynak veya hedef alt ağdaki 5N aracıları gerekir.

* **Sonuçları doğruluğunu.** Yönlendiriciler ve anahtarlar ICMP YANKI paketleri TCP paketleri karşılaştırıldığında daha düşük öncelikli atamak eğilimindedir. Ağ aygıtlarını yoğun olarak yüklendiğinde, belirli durumlarda daha yakından TCP tarafından alınan veri kaybı ve gecikme uygulamalar tarafından yaşadı yansıtır. Bu uygulama trafiğini çoğunu akışlar için TCP üzerinden oluşur. Böyle durumlarda, ICMP TCP'ye kıyasla daha az doğru sonuçlar sağlar. 

* **Güvenlik duvarı yapılandırması.** TCP protokolü, bir hedef bağlantı noktasına gönderilen TCP paket gerektirir. Ağ Performansı İzleyicisi aracıları tarafından kullanılan varsayılan bağlantı noktası 8084 ' dir. Aracıları yapılandırdığınızda, bağlantı noktasını değiştirebilirsiniz. Ağ güvenlik duvarları veya ağ güvenlik grubu (NSG) kuralları (azure'da) bağlantı noktası üzerinde trafiğe izin verdiğinden emin olun. Aracıları yüklü bilgisayarda yerel güvenlik duvarı Bu bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırıldığından emin olmanız gerekir. Windows çalıştıran bilgisayarlarınızda güvenlik duvarı kurallarını yapılandırmak için PowerShell komut dosyalarını kullanabilirsiniz, ancak ağ güvenlik duvarını elle yapılandırmanız gerekir. Buna karşılık, ICMP bir bağlantı noktası kullanarak çalışmaz. Çoğu Kurumsal senaryoda, ICMP trafiği güvenlik duvarları üzerinden ping yardımcı programı gibi ağ tanılama araçları kullanmanıza izin verilir. Başka bir makineden ping, güvenlik duvarları el ile yapılandırmak zorunda kalmadan ICMP protokolünü kullanabilirsiniz.

>[!NOTE] 
> Bazı güvenlik duvarları çok sayıda olayları, güvenlik, bilgi ve olay yönetim sisteminde sonuçları aktarım için yol açabilecek ICMP engelleyebilir. Seçtiğiniz protokolü bir ağ güvenlik duvarı veya NSG tarafından engellenen olmadığından emin olun. Aksi takdirde, Ağ Performansı İzleyicisi ağ kesimine izleyemez. İzleme için TCP kullanmanızı öneririz. ICMP burada TCP, zaman gibi kullanamazsınız senaryolarda kullanın: 
>
> - TCP ham yuva Windows istemcileri izin verilmediğinden istemci tabanlı Windows düğümleri kullanın.
> - Ağ güvenlik duvarı veya NSG TCP engeller.
> - Protokol geçiş yapmak nasıl çıkılacağını bilmiyoruz.

Dağıtım sırasında ICMP kullanmayı seçerseniz, varsayılan kuralı izleme düzenleyerek TCP için herhangi bir zamanda geçiş yapabilirsiniz.

1. Git **ağ performansı** > **İzleyici** > **yapılandırma**   >  **İzleyici**. Ardından **varsayılan kuralı**. 
2. Kaydırma **Protokolü** bölümünde ve kullanmak istediğiniz protokolü seçin. 
3. Seçin **kaydetmek** ayarı uygulamak için. 

Varsayılan kural belirli bir protokol kullanıyor olsa da, farklı bir protokol yeni kurallar oluşturabilirsiniz. Burada bazı kurallar ICMP ve diğerleri TCP kullanmak kuralları bir karışımını da oluşturabilirsiniz. 

## <a name="walkthrough"></a>Kılavuz 

Şimdi kök nedeni bir sistem durumu olayı için basit bir araştırmasını bakın.

Çözüm panosunda bir sistem durumu olayı, bir ağ bağlantısı sağlıksız olduğunu gösterir. Sorunu araştırmaya seçin **ağ bağlantıları izlenmekte olan** döşeme.

Ayrıntıya sayfasını gösteren **DMZ2 DMZ1** sağlıksız ağ bağlantısı. Seçin **görüntülemek alt ağ bağlantıları** bu ağ bağlantısı için. 


İçindeki tüm alt ağ bağlantıları ayrıntıya sayfası gösterir **DMZ2 DMZ1** ağ bağlantısı. Her iki alt ağ bağlantıları için gecikme ağ bağlantısı sağlıksız yapar eşiği aşıldığında. Her iki alt ağ bağlantıları gecikme eğilimleri de görebilirsiniz. Saat seçimini kontrol gerekli zaman aralığını odaklanmaya grafikte kullanın. Gecikme süresi, yoğun ulaşıldığında günün saatini görebilirsiniz. Sorunu araştırmaya daha sonra bu zaman aralığı için günlükleri arayın. Seçin **görüntülemek düğüm bağlantıları** daha fazla ayrıntıya için. 
 
 ![Alt ağ bağlantıları sayfası](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Önceki sayfaya benzer, belirli alt ağ bağlantısı için detaya sayfa onun bağlı düğüm bağlantıları listeler. Benzer eylemler gerçekleştirebilir önceki adımda yaptığınız gibi burada. Seçin **görünüm topoloji** iki düğüm arasında topolojisini görüntülemek için. 
 
 ![Düğüm bağlantılar sayfası](media/log-analytics-network-performance-monitor/node-links.png) 

İki seçili düğümler arasındaki tüm yolları topoloji haritasında çizilir. Topoloji harita üzerinde iki düğüm arasında yolların atlama atlamalı topoloji görselleştirebilirsiniz. Kaç yollar iki düğüm arasında ne mevcut NET bir resim sunan yolları veri paketlerinin alın. Ağ performans sorunları kırmızı olarak gösterilir. Hatalı ağ bağlantısı ya da hatalı ağ aygıtı bulmak için topoloji haritasında kırmızı öğeler arayın. 

 ![Topoloji Haritası ile topoloji Panosu](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

Kaybı, gecikme ve her yolunda durak gözden geçirebilirsiniz **eylemi** bölmesi. Scrollbar sağlıksız yollarının ayrıntılarını görüntülemek için kullanın. Yalnızca seçilen yollar için topoloji çizilen sağlıksız atlama ile yolları seçmek için filtreleri kullanın. İçinde veya dışında topoloji Haritası yakınlaştırma için fare tekerleğinin kullanın. 

Aşağıdaki görüntüde, ağ belirli bölümüne sorunlu alanları kök nedenini kırmızı yolları ve atlama görünür. Bir düğüm FQDN ve IP adresini içeren düğüm özelliklerini ortaya çıkarmak için topoloji haritasında seçin. Bir atlama seçerek atlamanın IP adresini gösterir. 
 
![Seçili düğüm özellikleriyle topoloji Haritası](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
