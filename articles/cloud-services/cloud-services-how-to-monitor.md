---
title: Bir bulut hizmeti izleme | Microsoft Docs
description: "Klasik Azure portalı kullanarak bulut hizmetlerini izleme öğrenin."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: c369b22cf068a473343b006eb1b06fdd350d31db
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-monitor-cloud-services"></a>Bulut Hizmetlerini İzleme
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

İzleyeceğiniz `key` Klasik Azure portalındaki bulut hizmetleriniz için performans ölçümleri. En az ve her hizmeti rolü için ayrıntılı izleme düzeyini ayarlayabilir ve izleme görüntüler özelleştirebilirsiniz. Ayrıntılı izleme verilerini dışında portalına erişebilmek için bir depolama hesabında depolanır. 

Klasik Azure portalındaki izleme görüntüler yüksek oranda yapılandırılabilir. Üzerinde ölçümleri listesinde izlemek istediğiniz ölçümleri seçebilirsiniz **İzleyici** sayfası ve seçim yapabileceğiniz üzerinde ölçümlerini çizmek için hangi ölçümleri grafikleri **İzleyici** sayfası ve Pano. 

## <a name="concepts"></a>Kavramlar
Varsayılan olarak, en az izleme rol örnekleri (sanal makineler) için ana bilgisayar işletim sisteminden toplanan performans sayaçlarını kullanarak yeni bir bulut hizmeti için sağlanır. CPU yüzdesi, verileri, veri çıkışı, Disk okuma performansı ve Disk yazma verimliliği için en az ölçümleri sınırlıdır. Ayrıntılı izleme yapılandırarak, sanal makinelerdeki (Rol örnekleri) performans verileri temel alan ek ölçümler alabilir. Ayrıntılı ölçümler uygulama işlemleri sırasında oluşabilecek sorunları daha yakından analizini sağlar.

Varsayılan olarak performans sayacı verilerini rol örneklerinden örneklenen ve rol örneğinden 3 dakika aralıklarla aktarılan. Ham performans sayacı verilerini ayrıntılı izleme etkinleştirdiğinizde, her rol örneği için ve her rol için rol örnekleri arasında 5 dakika, 1 saat ve 12 saat aralıklarla toplanır. Toplanan verileri 10 gün sonra temizlenir.

Ayrıntılı izleme etkinleştirdikten sonra toplanan izleme verilerini depolama hesabınız tablolarında depolanır. Ayrıntılı bir rol için izlemeyi etkinleştirmek için depolama hesabı bağlanan bir tanılama bağlantı dizesi yapılandırmanız gerekir. Farklı depolama hesapları için farklı roller kullanabilirsiniz.

Ayrıntılı izleme artar etkinleştirme depolama maliyetleriniz veri depolama, veri aktarımı ve depolama işlemleri ilgili. İzleme en az bir depolama hesabı gerektirmez. İzleme düzeyi için ayrıntılı ayarlasanız bile en az bir izleme düzeyinde kullanıma sunulan ölçümler için veri depolama hesabınızdaki depolanmaz.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Nasıl yapılır: Bulut Hizmetleri için izlemeyi Yapılandır
Azure Klasik Portalı'nda izleme ayrıntılı ya da en az yapılandırmak için aşağıdaki yordamları kullanın. 

### <a name="before-you-begin"></a>Başlamadan önce
* Oluşturma bir *Klasik* izleme verilerini depolamak için depolama hesabı. Farklı depolama hesapları için farklı roller kullanabilirsiniz. Daha fazla bilgi için bkz: [bir depolama hesabı oluşturmak nasıl](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Azure tanılama bulut hizmeti rollerinizi için etkinleştirin. Bkz: [tanılama bulut Hizmetleri için yapılandırma](cloud-services-dotnet-diagnostics.md).

Tanılama bağlantı dizesi rolü yapılandırmasında mevcut olduğundan emin olun. Azure Tanılama'yı etkinleştirmek ve rolü yapılandırmasında bir tanılama bağlantı dizesi içeren kadar ayrıntılı izlemesini etkinleştiremiyor.   

> [!NOTE]
> Azure SDK 2.5 hedefleyen projeler proje şablonu tanılama bağlantı dizesi otomatik olarak içermiyordu. Bu proje için rol yapılandırmasını tanılama bağlantı dizesi el ile eklemeniz gerekir.
> 
> 

**El ile rol yapılandırması için tanılama bağlantı dizesi eklemek için**

1. Bulut hizmeti projesini Visual Studio'da açın
2. Çift **rol** rolü seçin ve tasarımcı açmak için **ayarları** sekmesi
3. Adlı bir ayar için Ara **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Bu ayar mevcut değilse tıklayın **ayar Ekle** yapılandırmaya eklemek ve yeni ayar türünü Değiştir düğmesi **ConnectionString**
5. Bağlantı dizesi değeri ayarlanamıyor tıklayarak **...**  düğmesi. Bu depolama hesabını seçmenize olanak sağlayan bir iletişim kutusunu açar.
   
    ![Visual Studio ayarları](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Ayrıntılı veya minimum izleme düzeyini değiştirmek için
1. İçinde [Klasik Azure portalı](https://manage.windowsazure.com/), açık **yapılandırma** bulut hizmeti dağıtımı için sayfa.
2. İçinde **düzeyi**, tıklatın **ayrıntılı** veya **en az**. 
3. **Kaydet** düğmesine tıklayın.

Ayrıntılı izleme etkinleştirdikten sonra Klasik Azure portalındaki izleme verilerini bir saat içinde görmeye başlayacaksınız.

Ham performans sayacı verilerini ve toplanan izleme verilerini rolleri için dağıtım kimliği nitelenen tabloları depolama hesabında depolanır. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Nasıl yapılır: Bulut hizmeti ölçümleri için uyarılar almak
Bulut hizmetinizde ölçümleri izleme göre uyarıları alabilirsiniz. Üzerinde **Yönetim Hizmetleri** sayfasında Azure Klasik portal, size seçtiğiniz ölçüm belirttiğiniz bir değer ulaştığında bir uyarı tetiklemek için bir kural oluşturabilirsiniz. Uyarı tetiklendiğinde gönderilen e-posta seçebilirsiniz. Daha fazla bilgi için bkz: [nasıl yapılır: uyarı bildirimleri alma ve Azure uyarı kurallarını yönet](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Nasıl yapılır: ölçüm ölçümleri tablosuna Ekle
1. İçinde [Klasik Azure portalı](http://manage.windowsazure.com/), açık **İzleyici** bulut hizmeti için sayfa.
   
    Varsayılan olarak, ölçüm tablosunda kullanılabilir ölçüler kümesini görüntüler. Çizimde rol düzeyinde toplanan verilerle Bellek\Kullanılabilir MBayt performans sayacı için sınırlı bir bulut hizmeti için varsayılan ayrıntılı ölçümler gösterilmektedir. Kullanım **ölçüm Ekle** Klasik Azure portalında izlemek için ek toplama ve rol düzeyi ölçütleri seçin.
   
    ![Ayrıntılı görüntüleme](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. Ölçüm ölçümleri tablosuna eklemek için:
   
   1. Tıklatın **ölçüm Ekle** açmak için **ölçüm Seç**, aşağıda gösterilen.
      
       İlk kullanılabilir ölçüm, kullanılabilir seçenekleri göstermek için genişletilir. Her ölçüm için üst seçeneği tüm rolleri için toplanan izleme verilerini görüntüler. Ayrıca, verileri görüntülemek için bireysel rolleri seçebilirsiniz.
      
       ![Ölçüm Ekle](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. Görüntülenecek ölçümler seçmek için
      
      * İzleme seçeneklerini genişletmek için metriği tarafından aşağı oka tıklayın.
      * Görüntülemek istediğiniz her izleme seçeneğinin onay kutusunu seçin.
        
        En fazla 50 ölçümleri ölçümleri tabloda görüntüleyebilirsiniz.
        
        > [!TIP]
        > Ayrıntılı izlemede ölçümleri listesi ölçümleri düzinelerce içerebilir. Bir kaydırma çubuğu görüntülemek için iletişim kutusunun sağ tarafına getirin. Listeyi filtrelemek için arama simgesine tıklayın ve aşağıda gösterildiği gibi metin arama kutusuna girin.
        > 
        > 
        
        ![Ölçümleri arama ekleyin](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Ölçümleri seçtikten sonra Tamam'a (onay işareti) tıklayın.
   
    Seçilen ölçümleri, aşağıda gösterildiği gibi ölçümleri tablosuna eklenir.
   
    ![İzleyici ölçümleri](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. Ölçüm ölçümleri tablosundan silmek için onu seçin ve ardından ölçüm'ı tıklatın. **silmek ölçüm**. (Yalnızca gördüğünüz **silmek ölçüm** bir ölçüm seçili olduğunda,.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Özel ölçümleri ölçümleri tablosuna eklemek için
**Ayrıntılı** düzeyi izleme, portalda izleyebilirsiniz varsayılan ölçümleri listesini sağlar. Bunlara ek olarak, herhangi bir özel ölçümleri veya uygulamanızın portal üzerinden tarafından tanımlanan performans sayaçlarını izleyebilirsiniz.

Aşağıdaki adımlarda, açık olan varsayılmaktadır **ayrıntılı** düzeyi izleme ve toplama ve özel performans sayaçları aktarmak için uygulamanızın yapılandırılmamış olabilir. 

Özel performans sayaçları portalında görüntülenecek wad denetimi kapsayıcısı yapılandırmasında güncelleştirmeniz gerekir:

1. Wad denetimi kapsayıcısı blob tanılama depolama hesabınızı açın. Bunu yapmak için Visual Studio ya da başka bir Depolama Gezgini kullanabilirsiniz.
   
    ![Visual Studio Sunucu Gezgini](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Desen kullanarak blob yolu gidin **RoleName/Deploymentıd/RoleInstance** rol örneği için yapılandırma bulunamadı. 
   
    ![Visual Studio Depolama Gezgini](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Rol örneği için yapılandırma dosyasını indirin ve herhangi bir özel performans sayacı içerecek şekilde güncelleştirin. Örneğin İzleyicisi için *Disk Yazma Bayt/sn* için *C sürücüsü* altında aşağıdakileri ekleyin **PerformanceCounters\Subscriptions** düğümü
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Değişiklikleri kaydetmek ve yapılandırma dosyası blob içinde varolan bir dosyanın üzerine aynı konuma geri yükleyin.
5. Azure Klasik portalı yapılandırma ayrıntılı modda geç. Ayrıntılı modda zaten varsa, en az ve geri için ayrıntılı geçiş yapmak gerekir.
6. Özel performans sayacı şimdi kullanılabilir olması **ölçüm Ekle** iletişim kutusu. 

## <a name="how-to-customize-the-metrics-chart"></a>Nasıl yapılır: ölçümleri grafiği özelleştirme
1. Ölçümleri tablosunda ölçümleri grafiği çizmek için 6 ölçümleri seçin. Bir ölçü seçmek için onay kutusunun sol tarafında tıklayın. Ölçümleri grafikten bir ölçüm kaldırmak için ölçüm tablosunda onay kutusunu temizleyin.
   
    Ölçümleri tabloda ölçümleri seçtiğinizde, ölçümleri ölçümleri grafiğe eklenir. Dar bir görüntü üzerinde bir **n daha fazla** açılan listesinden görüntü sığmayan Metrik üstbilgileri içerir.
2. Göreli (her ölçüm için yalnızca son değer) ve mutlak değerleri (Y ekseni görüntülenen) görüntüleme arasında geçiş yapmak için göreli veya mutlak grafik üstündeki seçin.
   
    ![Göreli veya mutlak](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. Ölçümleri grafiği görüntüler zaman aralığını değiştirmek için grafik üstünde 1 saat, 24 saat veya 7 gün seçin.
   
    ![İzleyici görüntüleme süresi](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    Pano ölçümleri grafikte ölçümleri çizdirmek için yöntem farklıdır. Ölçümleri standart bir dizi kullanılabilir ve ölçümleri eklenir veya ölçüm üstbilgi seçerek kaldırılır.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Panoda ölçümleri grafiği özelleştirme
1. Bulut hizmeti için panosunu açın.
2. Eklemek veya grafikten ölçümleri kaldırın:
   
   * Yeni bir ölçümü çizmek için grafik üstbilgilerinde ölçümü için onay kutusunu seçin. Dar bir ekranda tarafından aşağı oka tıklayın  ***n* ?? Ölçümleri** grafik üstbilgi alanı görüntüleyemiyor ölçüm çizmek için.
   * Grafikte çizilen bir ölçüm silmek için kendi üstbilgisi tarafından onay kutusunu temizleyin.
   
3. Arasında geçiş **göreli** ve **mutlak** görüntüler.
4. 1 saat, 24 saat veya 7 günlük verileri görüntülemek için seçin.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Nasıl yapılır: izleme verilerini Klasik Azure portalı dışında ayrıntılı erişim
Ayrıntılı izleme verilerini her rol için belirlediğiniz depolama hesapları tablolarında depolanır. Her bulut hizmeti dağıtımı için rol için altı tablolar oluşturulur. İki tablo her biri için (5 dakika, 1 saat ve 12 saat) oluşturulur. Bu tablolar rol düzeyinde toplamalar depolar; başka bir tablo toplamaları rol örnekleri için depolar. 

Tablo adları aşağıdaki biçime sahiptir:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Burada:

* *Deploymentıd* bulut hizmeti dağıtımına atanmış GUID
* *aggregation_interval* 5 M, 1 H veya 12 H =
* rol düzeyinde toplamalar R =
* Rol örnekleri için toplamaları RI =

Örneğin, aşağıdaki tablolarda 1 saatlik aralıklarla toplanan ayrıntılı izleme verilerini saklayacağından:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
