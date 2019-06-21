---
title: VM'ler (Önizleme) için Azure İzleyici ile Uygulama bağımlılıklarını görüntüleme | Microsoft Docs
description: Harita, VM'ler için Azure İzleyici özelliğidir. Otomatik olarak, Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini bulur ve hizmetler arasındaki iletişimi eşler. Bu makalede, çeşitli senaryolarda Haritası özelliğini kullanma hakkında ayrıntılar sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: f6273e9b6c7ed0c4685479976343497f01201b0b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206752"
---
# <a name="use-the-map-feature-of-azure-monitor-for-vms-preview-to-understand-application-components"></a>Azure İzleyici'nin eşleme özelliği, uygulama bileşenleri anlamak için Vm'leri (Önizleme) için kullanın.
VM'ler için Azure İzleyici'de, Windows ve Linux Azure veya kendi ortamında çalışan sanal makineleri (VM'ler) üzerinde bulunan uygulama bileşenleri görüntüleyebilirsiniz. Sanal makineleri iki yolla görebilirsiniz. Doğrudan bir VM'den bir haritayı görüntülemek veya Azure İzleyici'nın bileşenleri arasında VM grupları görmek için bir eşlemden görüntüleyin. Bu makalede, bu iki görüntüleme yöntemleri ve eşleme özelliğini nasıl kullanacağınızı anlamanıza yardımcı olur. 

VM'ler için Azure İzleyici yapılandırma hakkında daha fazla bilgi için bkz: [VM'ler için Azure İzleyici'ı etkinleştirme](vminsights-enable-overview.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="introduction-to-the-map-experience"></a>Harita deneyimi giriş
Harita deneyime girmeden önce nasıl sunar ve bilgi görselleştirir anlamanız gerekir. Doğrudan bir VM veya Azure İzleyici eşleme özelliğini seçmek ister, eşleme özelliğini tutarlı bir deneyim sunar. Tek fark, bir harita birden çok katmanlı uygulama veya kümenin tüm üyelerini gösterir, Azure İzleyici'den.

Eşleme özelliğini VM bağımlılıklara sahip işlemleri çalıştırırken bularak görselleştirir: 

- Sunucular arasında etkin ağ bağlantısı.
- Gelen ve giden bağlantı gecikme süresi.
- Belirtilen zaman aralığı boyunca tüm TCP bağlantılı mimarisi arasında bağlantı noktaları.  
 
Bir VM işlem ayrıntılarını ve VM ile iletişim kuran işlemleri görüntülemek için genişletin. İstemci grubu, VM'ye bağlanın, ön uç istemcilerin sayısını gösterir. Sunucu bağlantı noktası grupları, VM'nin bağlandığı arka uç sunucularının sayısını gösterir. Bu bağlantı noktası üzerinden sunucuları ayrıntılı listesini görmek için bir sunucu bağlantı noktası grubu genişletin.  

VM'yi seçtiğinizde **özellikleri** sağ bölmesinde sanal makinenin özelliklerini gösterir. Sistem bilgileri Azure VM ve bulunan bağlantılar özetleyen bir halka grafik özellikleri, işletim sistemi tarafından bildirilen özellikleri içerir. 

![Özellikler bölmesi](./media/vminsights-maps/properties-pane-01.png)

Bölmenin sağ tarafında seçin **günlüğü olaylarını** VM için Azure İzleyici gönderdiği veri listesi gösterilecek. Bu veriler, sorgulama için kullanılabilir.  Açmak için herhangi bir kayıt türü seçin **günlükleri** gördüğünüz söz konusu kayıt türü için sonuçları sayfası. Sanal Makineyi karşı filtrelenmiş bir önceden yapılandırılmış sorgu de görebilirsiniz.  

![Günlük olayları bölmesi](./media/vminsights-maps/properties-pane-logs-01.png)

Kapat **günlükleri** sayfasında ve geri dönüp **özellikleri** bölmesi. Burada, seçin **uyarılar** VM durumu ölçütlerini uyarıları görüntülemek için. Eşleme özelliği, seçili zaman aralığında sonra seçtiğiniz sunucu için uyarılar gösterilecek Azure uyarıları ile entegre olur. Sunucuyu geçerli uyarılar için bir simge görüntüler ve **makine uyarılar** bölmesi uyarılarını listeler. 

![Uyarılar bölmesinde](./media/vminsights-maps/properties-pane-alerts-01.png)

İlgili uyarıları görüntülemek eşleme özelliğini yapmak için belirli bir bilgisayar için geçerli bir uyarı kuralı oluşturun:

- Bilgisayar tarafından bir yan tümce grubu uyarılar içerir (örneğin, **bilgisayar aralığı 1 dakika**).
- Bir ölçüm uyarısında tabanı.

Azure uyarıları ve uyarı oluşturma kuralları hakkında daha fazla bilgi için bkz. [Azure İzleyici'de uyarılar birleşik](../../azure-monitor/platform/alerts-overview.md).

Sağ üst köşedeki **gösterge** seçenek harita üzerinde semboller ve rolleri tanımlar. Bir daha yakından bakış haritanızı ve onu etrafında taşımak için sağ alt köşedeki yakınlaştırma denetimlerini kullanın. Yakınlaştırma seviyesini ayarlayabilir ve sayfa boyutu haritayı sığdırmak.  

## <a name="connection-metrics"></a>Bağlantı ölçümü
**Bağlantıları** bölmesi standart ölçüm seçili VM bağlantısı için TCP bağlantı noktası üzerinden görüntüler. Ölçümler, yanıt süresi, istek başına dakika, trafiği hacmi ve bağlantılar içerir.  

![Bağlantılar bölmesinde ağ bağlantısı grafikleri](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Başarısız bağlantılar
Harita işlemlerini ve bilgisayarlar için başarısız bağlantılarını gösterir. Kesikli kırmızı bir çizgi bir işlem veya bağlantı noktası ulaşmak bir istemci sistem başarısız olduğunu gösterir. Bağımlılık Aracısı'nı kullanan sistemler için aracı üzerinde başarısız bağlantı denemesi bildirir. Eşleme özelliğini, bir işlem başarısız bir bağlantı kurmak için TCP yuvaları gözlemleyerek izler. Bu hata, bir güvenlik duvarı, istemci veya sunucu veya kullanılamayan bir uzak hizmeti hatalı yapılandırılması kadar neden olabilir.

![Harita üzerinde bağlantı başarısız](./media/vminsights-maps/map-group-failed-connection-01.png)

Başarısız bağlantılar yardımcı olabileceğini anlamak sorun giderme, geçişi doğrulama, güvenlik analiz edin ve hizmetin genel mimarisini anlama. Başarısız bağlantılar bazen zararsız, ancak bunlar genellikle bir soruna işaret. Bağlantılar, örneğin, bir yük devretme ortamında aniden ulaşılamaz hale geldiğinde veya iki uygulama katmanları bulut geçişten sonra birbirleri ile iletişim kuramıyor başarısız olabilir.

### <a name="client-groups"></a>İstemci grupları
Harita üzerinde istemci grupları eşlenen kullanarak makineye bağlanın ve istemci makineleri temsil eder. Tek bir istemci grubundaki istemcileri için bir tek bir işlem veya makine temsil eder.

![Harita üzerinde bir istemci grubu](./media/vminsights-maps/map-group-client-groups-01.png)

İzlenen istemciler ve bir istemci grubundaki sistemleri IP adreslerini görmek için grubu seçin. Grup içeriğini aşağıda görünür.  

![IP adresleri harita üzerinde istemci Grup listesi](./media/vminsights-maps/map-group-client-group-iplist-01.png)

İzlenen ve izlenmeyen istemcileri grubu içeriyorsa, istemcilere filtrelemek için grubun halka grafik uygun bölümünü seçebilirsiniz.

### <a name="server-port-groups"></a>Sunucu bağlantı noktası grupları
Sunucu bağlantı noktası gruplarını temsil eden sunucular üzerindeki bağlantı noktalarına gelen bağlantılara eşlenen makine. Grup sunucusu bağlantı noktası ve bağlantıları Bu bağlantı noktasına sahip sunucuların sayısını içerir. Tek tek sunucuların ve bağlantıları görmek için bir grup seçin. 

![Harita üzerinde bir sunucu bağlantı noktası grubu](./media/vminsights-maps/map-group-server-port-groups-01.png)  

İzlenen ve izlenmeyen sunucuları grubu içeriyorsa, grubun halka grafik sunucuları filtrelemek için uygun bölümünü seçebilirsiniz.

## <a name="view-a-map-from-a-vm"></a>Bir VM'den bir haritayı görüntülemek 

Doğrudan bir VM'den VM'ler için Azure İzleyici erişmek için:

1. Azure portalında **sanal makineler**. 
2. Listeden VM'yi seçin. İçinde **izleme** bölümünde, seçin **Insights (Önizleme)** .  
3. Seçin **harita** sekmesi.

Harita sanal makinenin bağımlılıklarını bularak görselleştirir işlem gruplarının ve belirtilen zaman aralığı üzerinde etkin ağ bağlantınız işlemleri çalıştırma.  

Varsayılan olarak, son 30 dakika eşlemeyi gösterir. Bağımlılıkları geçmişte nasıl baktığı görmek istiyorsanız, bir saate kadar geçmiş zaman aralıkları için sorgulayabilirsiniz. Sorguyu çalıştırmak için kullandığınız **TimeRange** sol üst köşedeki Seçici. Örneğin, bir olay sırasında veya bir değişiklik öncesinde durumunu görmek için bir sorgu çalıştırabilirsiniz.  

![Doğrudan VM eşlemesi genel bakış](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-a-map-from-a-virtual-machine-scale-set"></a>Sanal makine ölçek kümesinden bir haritayı görüntülemek

Azure İzleyici VM'ler için doğrudan bir sanal makine ölçek kümesinden erişmek için:

1. Azure portalında **sanal makine ölçek kümeleri**.
2. Listeden VM'yi seçin. Ardından **izleme** bölümünde, seçin **Insights (Önizleme)** .  
3. Seçin **harita** sekmesi.

Haritanın tüm örnekleri grubun bağımlılıkları yanı sıra bir grup düğümü olarak ölçek görselleştirir. Genişletilmiş düğüm, Ölçek kümesindeki örneklerin listeler. Aynı anda bu örnekler 10 arasında gezinebilirsiniz. 

Belirli bir örneği için bir harita yüklemek için önce bu örneği harita üzerinde seçin. Ardından **üç nokta** (...) düğmesini seçip **sunucu haritasını Yükle**. Görüntülenen haritada işlem gruplarının ve belirtilen zaman aralığı üzerinde etkin ağ bağlantınız işlemleri bakın. 

Varsayılan olarak, son 30 dakika eşlemeyi gösterir. Bağımlılıkları geçmişte nasıl baktığı görmek istiyorsanız, bir saate kadar geçmiş zaman aralıkları için sorgulayabilirsiniz. Sorguyu çalıştırmak için kullandığınız **TimeRange** Seçici. Örneğin, bir olay sırasında veya bir değişiklik öncesinde durumunu görmek için bir sorgu çalıştırabilirsiniz.

![Doğrudan VM eşlemesi genel bakış](./media/vminsights-maps/map-direct-vmss-01.png)

>[!NOTE]
>Belirli bir örneği için bir eşleme da erişebilirsiniz **örnekleri** görünümü için sanal makine ölçek kümesi. İçinde **ayarları** bölümüne gidin **örnekleri** > **Insights (Önizleme)** .

## <a name="view-a-map-from-azure-monitor"></a>Azure İzleyici bir eşlemden görüntüleyin
Azure İzleyici'de eşleme özelliğini Vm'lerinizi ve bunların bağımlılıklarını genel bir görünümünü sağlar. Azure İzleyicisi'ndeki harita özelliğe erişmek için:

1. Azure portalında **İzleyici**. 
2. İçinde **Insights** bölümünde, seçin **sanal makineler (Önizleme)** .
3. Seçin **harita** sekmesi.

   ![Azure İzleyici genel bakış haritasını birden çok VM](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Kullanarak bir çalışma alanı seçin **çalışma** sayfanın üstündeki Seçici. Birden fazla Log Analytics çalışma alanı varsa, kendisine rapor veren Vm'leri olan ve çözümle etkin çalışma alanını seçin. 

**Grubu** Seçici döndürür abonelikler, kaynak grupları [bilgisayar grupları](../../azure-monitor/platform/computer-groups.md)ve sanal makine ölçek kümeleri, seçilen çalışma alanına ilgili bilgisayar. Seçiminiz yalnızca eşleme özelliğini uygular ve performans veya sistem durumu aktarılmaz.

Varsayılan olarak, son 30 dakika eşlemeyi gösterir. Bağımlılıkları geçmişte nasıl baktığı görmek istiyorsanız, bir saate kadar geçmiş zaman aralıkları için sorgulayabilirsiniz. Sorguyu çalıştırmak için kullandığınız **TimeRange** Seçici. Örneğin, bir olay sırasında veya bir değişiklik öncesinde durumunu görmek için bir sorgu çalıştırabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar
- Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü Azure VM durumu](vminsights-health.md). 
- Performans sorunlarını tanımlamak için performans, denetleyin ve sanal makinelerinizin genel kullanımı anlamak, [performans durumunu görüntüleme için Azure İzleyici VM'ler için](vminsights-performance.md). 
