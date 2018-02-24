---
title: "Performans İzleyicisi'ni Özelliği Azure günlük analizi Ağ Performansı İzleyicisi çözümünde | Microsoft Docs"
description: "Ağ Performans İzleyicisi'nde Performans İzleyicisi yetenek ağınızda, bulut dağıtımları ve şirket içi konumları, birden çok veri merkezi ve şube ofisleri gibi kritik görev çeşitli noktaları arasında ağ bağlantısı izlemenize yardımcı olur çok katmanlı uygulamalar/mikro-hizmetler."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: a90ab3bc857b704d9d94daf96d17611844abb1a6
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="network-performance-monitor-solution---performance-monitoring"></a>Performans İzleyicisi çözüm - ağ performansını izleme

Performans İzleyicisi'ni özelliği [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) izlemenizi ağ bağlantısı çeşitli sağlar işaret bulut dağıtımları ve şirket içi konumlar gibi ağınızda birden çok veri merkezleri ve Şube ofislerinde, görev kritik çok katmanlı uygulamalar/mikro-hizmetler. Performans izleme ile kullanıcılarınız şikayetçi önce ağ sorunları algılayabilir. Anahtar avantajları şunlardır: 

- Çeşitli alt ağları ve Uyarıları Ayarla kaybı ve gecikme izleme 
- Ağdaki tüm yolları (yedekli yollar dahil) izleme 
- Çoğaltma zor olan geçici & zaman içinde nokta ağ sorunlarını giderme 
- İçin performans sorumlu olduğu ağ belirli kesiminde belirleme 
- SNMP gerek kalmadan ağ durumunu izleyin 


![Ağ Performansı İzleyicisi](media/log-analytics-network-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>Yapılandırma
Ağ Performansı İzleyicisi Yapılandırması'nı açmak için açın [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) tıklatıp **yapılandırma** düğmesi.

![Ağ Performans İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>Yeni ağ oluşturma

NPM bir ağda, alt ağlar için mantıksal bir kapsayıcısıdır. Ağ altyapınızın izleme gereksinimlerinize göre izleme düzenlemenize yardımcı olur. Bir kolay adla bir ağ oluşturun ve alt ağlarını iş mantığınızı göre ekleyin. Örneğin, Londra adlı bir ağ oluşturun ve tüm alt ağlar, Londra veri merkezinizde eklemek veya adlandırılmış bir ağ *ContosoFrontEnd* ve tüm alt ağlar bu ağa Contoso adlı, uygulamanızın ön uç hizmet veren ekleyin. Çözüm, ortamınızda bulunan tüm alt ağlar içeren bir varsayılan ağ otomatik olarak oluşturur. Her bir ağ oluşturduğunuzda, bir alt ağ ekleyin ve bu alt ağ varsayılan ağdan kaldırılır. Bir ağ silerseniz, tüm alt varsayılan ağa otomatik olarak döndürülür. Bu nedenle, varsayılan ağ herhangi bir kullanıcı tarafından tanımlanan ağ içinde yer almayan tüm alt ağlar için kapsayıcı görevi görür. Düzenleyemez veya varsayılan ağ silin. Her zaman, sistemde kalır. Ancak, gereksinim duyduğunuz kadar çok özel ağlar oluşturabilirsiniz. Çoğu durumda, alt ağlar, kuruluşunuzda birden fazla ağ düzenlenir ve iş mantığınızı için alt grup için bir veya daha fazla ağlar oluşturmanız gerekir.

Yeni bir ağ oluşturmak için:


1. Tıklayın **ağlar sekmesini**.
1. Tıklatın **Ekle ağ** ve ağ adını ve açıklamasını yazın. 
2. Bir veya daha fazla alt ağ seçin ve ardından **Ekle**. 
3. tıklatın **kaydetmek** yapılandırmayı kaydetmek için. 


### <a name="create-monitoring-rules"></a>İzleme kuralları oluşturma 

Performans İzleyicisi iki ağ arasında veya iki alt ağlar arasında ağ bağlantıları performans eşiği aşıldığında sistem durumu olayları oluşturur. Bu eşikler sistem tarafından otomatik olarak öğrenilebilecek veya özel eşikler sağlayabilir. Sistem, kayıp veya herhangi bir alt ağ/ağ çifti arasında gecikme ihlallerini bağlantılar her bir sistem durumu olayı oluşturan varsayılan bir kural sistem öğrenilen eşiği otomatik olarak oluşturur. Bu, tüm izleme kurallarını açıkça oluşturmadınız kadar ağ altyapınızı izlemek çözüm yardımcı olur. Varsa **varsayılan kural** olan etkinse, tüm düğümlere yapay işlemler izleme için etkinleştirilmiş olan tüm diğer düğümlere gönderin. Varsayılan kural küçük ağlar ile Örneğin, burada bir mikro hizmet çalıştıran sunucuların küçük bir sayıya sahip ve tüm sunucuların bağlantınız birbirlerine emin olmak istediğiniz bir senaryo yararlıdır. 

>[!NOTE]
> Devre dışı bırakmanızı önerilen **varsayılan kural** ve burada kullandığınız çok sayıda düğümü izleme için özellikle büyük ağlar ile özel izleme kurallarını oluşturun. Bu çözüm ve ağınızı izleme düzenlemenize yardım tarafından oluşturulan trafiğin azaltır. 

İş mantığınızın göre izleme kurallarını oluşturun. Headquarter iki office sitelere ağ bağlantısını performansını izlemek isterseniz, örneğin, ardından office site1 O1 ağındaki tüm alt ağlar, office site2 O2 ağındaki tüm alt ağlar ve headquarter H. ağındaki tüm alt ağlar gruplandırma Kuralları-O1 ve H ve diğer O2 H. arasındaki arasında bir izleme 2 oluşturma 

Özel İzleme kurallarını oluşturmak için:

1. Tıklatın **Kuralı Ekle** içinde **İzleyici** sekmesinde ve kural ad ve açıklama girin. 
2. Listelerden izlemek için ağ veya alt ağ bağlantıları çifti seçin. 
3. İlk ilgi ilk alt ağ/s yer alan ağ ağ aşağı açılır listeden seçtikten sonra alt ağ/s karşılık gelen alt ağ aşağı açılır listeden seçin. Seçin **tüm alt ağlar** bir ağ bağlantısı tüm alt ağlar izlemek istiyorsanız. Benzer şekilde, diğer alt ağ/s ilgi seçin. Ve tıklayabilirsiniz **özel durum ekleyin** yaptığınız seçimden belirli alt ağ bağlantıları için izleme dışlanacak. 
4. ICMP ve TCP arasında yapay işlemler yürütme için protokol seçin. 
5. Seçtiğiniz, ardından öğeleri için sistem durumu olayları Temizle oluşturmak istemiyorsanız, **bu kural tarafından kapsanan bağlantılarında sistem durumu izlemeyi etkinleştir**. 
6. İzleme koşulları seçin. Eşik değerleri yazarak, sistem olay oluşturma için özel eşikler ayarlayabilirsiniz. Seçilen ağ/alt ağ çifti için seçilen eşiğin üstünde koşul değerini gider olduğunda, bir sistem durumu olayı oluşturulur. 
7. tıklatın **kaydetmek** yapılandırmayı kaydetmek için. 

Bir izleme kuralı kaydettikten sonra bu kural uyarı yönetimi ile tıklayarak tümleştirebilirsiniz **oluşturma uyarı**. Bir uyarı kuralı arama sorgusu ve diğer gerekli ile otomatik olarak oluşturulan otomatik olarak doldurulmuş parametreler. Bir uyarı kuralı kullanarak NPM içinde var olan uyarılar ek olarak e-posta tabanlı uyarılar alabilir. Uyarılar ayrıca eylemlerden runbook'larla tetikleyebilir veya Web kancalarını kullanarak mevcut hizmet yönetimi çözümleriyle tümleştirilebilir. Tıklayabilirsiniz **yönetmek uyarı** uyarı ayarlarını düzenlemek için. 

Artık daha fazla Performans İzleyicisi kurallar oluşturabilir veya çözüm panosuna özelliği kullanarak başlangıcına Taşı 

### <a name="choose-the-protocol"></a>Protokol seçin

Ağ Performans İzleyicisi'ni (NPM) yapay işlemler paket kaybı ve bağlantı gecikme süresi gibi ağ performans ölçümlerini hesaplamak için kullanır. Bu daha iyi anlamak için bağlı bir NPM aracı göz önünde bulundurun. bir ağ bağlantısı ucunu. Bu NPM aracı başka bir ağ sonuna bağlı ikinci bir NPM aracı araştırma paketleri gönderir. İkinci aracı yanıt paketleri ile yanıtlar. Bu işlem birkaç kez yinelenir. Yanıtlar ve her yanıt almak için geçen süre sayısını ölçme, birinci NPM aracı bağlantı gecikme değerlendirir ve paket bırakma. 

Biçim, boyutu ve bu paketlerin dizisini izleme kurallarını oluştururken seçtiğiniz protokolü tarafından belirlenir. Paketlerin protokolünü temel, ara ağ aygıtlarını (yönlendiriciler, anahtarlar vb.) bu paketleri farklı işlem. Sonuç olarak, Protokolü tercih ettiğiniz sonuçları doğruluğunu etkiler. Ve protokolü tercih ettiğiniz de NPM çözümü dağıttıktan sonra herhangi bir el ile adımlar atmanız gerekir olup olmadığını belirler. 

NPM yapay işlemleri yürütmek ICMP ve TCP protokollerini arasında seçim sunar. Yapay işlem kuralı oluşturduğunuzda, ICMP seçerseniz, NPM aracıları ağ gecikme süresi ve paket kaybı hesaplamak için ICMP YANKI iletisi kullanın. ICMP YANKI geleneksel Ping yardımcı programı tarafından gönderilen aynı iletiyi kullanır. Protokol olarak TCP'yi kullandığınızda NPM aracıları ağ üzerinden TCP Eşitlemeye paket gönderin. Bu bir TCP anlaşması tarafından izlenir tamamlama ve RST paketlerini kullanarak bağlantı kaldırılıyor. 

Kullanılacak protokol seçmeden önce aşağıdaki bilgileri göz önünde bulundurun: 

**Birden çok ağ yollarını keşfetme.**  Birden çok yol keşfederken TCP daha doğru olduğunu ve her alt ağda daha az aracıları gerekiyor. Örneğin, TCP kullanarak bir veya iki aracılarını alt ağlar arasındaki tüm yedekli yollar bulabilir. Ancak, benzer sonuçlar elde etmek için ICMP kullanarak birkaç aracılarını gerekir. ICMP, iki alt ağlar arasındaki yolları sayısı varsa kullanılarak, birden çok kaynak veya hedef alt ağdaki 5N aracıları gerekir. 

**Sonuçları doğruluğunu.** Yönlendiriciler ve anahtarlar ICMP YANKI paketleri TCP paketleri karşılaştırıldığında daha düşük öncelikli atamak eğilimindedir. Ağ aygıtlarını yoğun olarak yüklendiğinde, belirli durumlarda daha yakından TCP tarafından alınan veri kaybı ve gecikme uygulamalar tarafından yaşadı yansıtır. Bu uygulama trafiğini çoğunu akışlar için TCP üzerinden oluşur. Böyle durumlarda, ICMP TCP'ye kıyasla daha az doğru sonuçlar sağlar. 

**Güvenlik duvarı yapılandırması.** TCP protokolü, bir hedef bağlantı noktasına gönderilen TCP paket gerektirir. Aracıları yapılandırdığınızda bu değiştirebilirsiniz ancak NPM aracıları tarafından kullanılan varsayılan bağlantı noktası 8084, ' dir. Bu nedenle, ağ güvenlik duvarları veya NSG kuralları (azure'da) bağlantı noktası üzerinde trafiğe izin emin olmak için gerekir. Aracıları yüklü bilgisayarda yerel güvenlik duvarı Bu bağlantı noktası üzerinde trafiğe izin verecek şekilde yapılandırıldığından emin olmanız gerekir. Ağ güvenlik duvarı el ile yapılandırmanız gerekiyor ancak Windows çalıştıran bilgisayarlarınızda güvenlik duvarı kurallarını yapılandırmak için PowerShell komut dosyalarını kullanabilirsiniz. Buna karşılık, ICMP bağlantı noktası kullanılarak çalışmaz. Çoğu Kurumsal senaryoda, ICMP trafiği güvenlik duvarları üzerinden Ping yardımcı programı gibi ağ tanılama araçları kullanmanıza izin verilir. Başka bir makineden Ping, bu nedenle, daha sonra ICMP Protokolü güvenlik duvarları el ile yapılandırmak zorunda kalmadan kullanabilirsiniz. 

>[!NOTE] 
> Bazı güvenlik duvarları çok fazla sayıda olayı, güvenlik, bilgi ve olay yönetim sisteminde kaynaklanan aktarım açabilir ICMP engelleyebilir. Seçtiğiniz Protokolü engellenmemiş olduğundan emin olun ağ güvenlik duvarı/NSG, tarafından aksi NPM ağ kesimine izleyebilirsiniz değil. Bu nedenle, izleme için TCP kullanan önerilir. Burada, TCP, gibi ne zaman kullanmanız mümkün olmayan senaryolarda ICMP kullanmanız gerekir: 
>
> - TCP ham yuva Windows istemcisinde izin verilmiyor beri Windows istemci tabanlı düğümleri, kullanmakta olduğunuz
> - Ağ güvenlik duvarı/NSG TCP engeller
> - Protokol geçiş yapma 

Dağıtım sırasında ICMP kullanmayı seçerseniz, varsayılan kuralı izleme düzenleyerek TCP için herhangi bir zamanda geçiş yapabilirsiniz:

1. Gidin **ağ performansı** > **İzleyici** > **yapılandırma**   >  **İzleyici** ve ardından **varsayılan kuralı**. 
2. Kaydırma **Protokolü** bölümünde ve kullanmak istediğiniz protokolü seçin. 
3. Tıklatın **kaydetmek** ayarı uygulamak için. 

Varsayılan kural belirli bir protokol kullanıyor olsa bile, farklı bir protokol yeni kurallar oluşturabilirsiniz. Burada bazı kuralların ICMP kullanır ve başka bir TCP kullandığı kuralları bir karışımını da oluşturabilirsiniz. 

## <a name="walkthrough"></a>Kılavuz 

Performans İzleyicisi hakkında okuduğunuza göre kök nedeni bir sistem durumu olayı için basit bir araştırmasını bakalım.  

Çözüm panosunda, fark sistem durumu olayı – bir ağ bağlantısı sağlam değil. Sorunu araştırmanıza ve tıklayın karar **ağ bağlantıları izlenmekte olan** döşeme.

Ayrıntılı sayfasında, bu gözlemlemek **DMZ2 DMZ1** sağlıksız ağ bağlantısı. Tıklatılan **görüntülemek alt ağ bağlantıları** bu ağ bağlantısı için bağlantı. 


İçindeki tüm alt ağ bağlantıları ayrıntıya sayfası gösterir **DMZ2 DMZ1** ağ bağlantısı. Her iki alt ağ bağlantıları için ağ bağlantısı bozan eşik gecikmesi taşını dikkat edin. Her iki alt ağ bağlantıları gecikme eğilimleri de görebilirsiniz. Saat seçimini kullanabilirsiniz gerekli zaman aralığına odaklanmak için grafiği denetiminde. Gecikme süresi, yoğun zaman ulaştı günün saatini görebilirsiniz. Sorunu araştırmaya bu zaman aralığı için günlükleri daha sonra arama yapabilirsiniz. Tıklatın **görüntülemek düğüm bağlantıları** daha fazla ayrıntıya için. 
 
 ![Ağ bağlantıları](media/log-analytics-network-performance-monitor/subnetwork-links.png) 

Önceki sayfaya benzer, belirli alt ağ bağlantısı için detaya sayfa onun bağlı düğüm bağlantıları listeler. Benzer eylemler gerçekleştirebilir önceki adımda yaptığınız gibi burada. Tıklatın **görünüm topoloji** iki düğüm arasında topolojisini görüntülemek için. 
 
 ![Düğüm bağlantıları](media/log-analytics-network-performance-monitor/node-links.png) 

İki seçili düğümler arasındaki tüm yolları topoloji haritasında çizilir. Topoloji harita üzerinde iki düğüm arasında yolların atlama atlamalı topoloji görselleştirebilirsiniz. Kaç yollar iki düğüm arasında ne mevcut NET bir resim sunan veri paketlerinin yapmakta yolları. Ağ performans sorunları kırmızı renk işaretlenir. Hatalı ağ bağlantısı veya hatalı ağ aygıtı topoloji haritasında kırmızı renkli öğeler arayarak bulabilirsiniz. 

 ![Topoloji Panosu](media/log-analytics-network-performance-monitor/topology-dashboard.png) 

İçinde kaybı, gecikme ve her yolundaki durak sayısını incelenebilecek **eylemi** bölmesi. Scrollbar sağlıksız bu yollar ayrıntılarını görüntülemek için kullanın. Yalnızca seçilen yollar için topoloji çizilen sağlıksız atlama ile yolları seçmek için filtreleri kullanın. İçinde veya dışında topoloji Haritası yakınlaştırma için fare tekerleğinin kullanabilirsiniz. 

İçindeki görüntü açıkça ağ belirli bölümüne sorunlu alanları kök nedenini yolları ve kırmızı renk atlama bakarak görebilirsiniz. Bir topoloji Haritası düğümünde tıklayarak FQDN dahil olmak üzere düğümü özelliklerini gösterir ve IP adresi. Atlama tıklatarak atlamanın IP adresi gösterir. 
 
![Topoloji Panosu](media/log-analytics-network-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>Sonraki adımlar
* [Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı veri kayıtları görüntülemek için.
