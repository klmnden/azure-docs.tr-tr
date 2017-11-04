---
title: "Azure Application Insights kullanarak performans sorunlarını tanılama | Microsoft Docs"
description: "Azure Application Insights'ı kullanarak uygulamanızdaki performans sorunlarını tanılamak ve Bul Öğreticisi."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0edec15c7f14ee5338555b03700b7be32c3a1023
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Azure Application Insights ile performans sorunlarını tanılamak ve Bul

Azure Application Insights telemetri işlemi ve performansını çözümlemek amacıyla uygulamanızdan toplar.  Bu bilgileri oluşabilecek bir sorunu belirlemek için veya etkisi kullanıcıların çoğunun misiniz uygulama geliştirmeleri belirlemek için kullanabilirsiniz.  Bu öğretici, uygulamanızın hem sunucu bileşenleri ve istemci perspektif performansını analiz etme işleminde size gösterir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Sunucu tarafı işlemlerinin performansını tanımlayın
> * Yavaş performans kök nedenini belirlemek için sunucu işlemleri Çözümle
> * En yavaş istemci-tarafı işlemleri tanımlayın
> * Sorgu dili kullanarak sayfa görünümleri ayrıntılarını Çözümle


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme
- .NET uygulaması azure'a dağıtma ve [Application Insights SDK'sı etkinleştirmek](app-insights-asp-net.md).
- [Application Insights profil oluşturucu etkinleştirmek](app-insights-profiler.md#installation) uygulamanız için.

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Yavaş sunucu işlemleri tanımlayın
Application Insights, uygulamanızda farklı işlemler için performans ayrıntıları toplar.  Bu işlemler uzun süreli belirleyerek olası sorunları tanılamak veya en iyi uygulama genel performansını artırmak için devam eden geliştirme hedefleyin.

1. Seçin **Application Insights** ve aboneliğinizi seçin.  
1. Açmak için **performans** ya da seçin panel **performans** altında **Araştır** tıklayın veya menü **sunucu yanıt süresi** grafiği .

    ![Performans](media/app-insights-tutorial-performance/performance.png)

2. **Performans** paneli uygulaması için her işlemi sayısı ve ortalama süresini gösterir.  Çoğu kullanıcıları etkileyen bu işlemleri tanımlamak için bu bilgileri kullanın. Bu örnekte, **müşteriler/ayrıntıları alma** ve **GET Home/Index** kendi görece yüksek süre ve çağrı sayısı nedeniyle araştırmak için büyük olasılıkla bir aday değildir.  Diğer işlemleri daha yüksek bir süre olabilir ancak kendi geliştirme etkisini en az olacak şekilde nadiren, adı veriliyordu.  

    ![Performans paneli](media/app-insights-tutorial-performance/performance-blade.png)

3. Grafik şu anda tüm işlemleri ortalama süresi zamanla gösterir.  Sizin de grafiğe sabitlemenin ilgileniyor işlemleri ekleyin.  Bu araştırma değerinde bazı yükselmeleri gösterir.  Bu, daha fazla zaman penceresi grafik azaltarak yalıtma.

    ![PIN işlemleri](media/app-insights-tutorial-performance/pin-operations.png)

4.  Sağ tarafta kendi performansını paneli görüntülemek için bir işlem'ı tıklatın. Bu farklı istekleri için süreleri gösterir.  Kullanıcılar, yaklaşık yarım saniye performanslarının yavaş, böylece penceresi istekleri 500 milisaniye azaltmak genellikle unutmayın.  

    ![Süre dağıtım](media/app-insights-tutorial-performance/duration-distribution.png)

5.  Bu örnekte, çok sayıda istekleri işlemek için saniyenin kaplayan görebilirsiniz. Tıklayarak, bu işlem ayrıntılarını görebilirsiniz **işlem ayrıntıları**.

    ![İşlem ayrıntıları](media/app-insights-tutorial-performance/operation-details.png)

6.  Topladığınız bilgileri şu ana kadar yalnızca yavaş performans yoktur, ancak kök nedeni almak için çok az mu onaylar.  **Profil oluşturucu** bu işlemi ve her adımı için gereken süreyi çalıştırılan gerçek bir kod göstererek yardımcı olur. Profil Oluşturucu düzenli aralıklarla çalışan bu yana bazı işlemler izleme olmayabilir.  Zaman içinde daha fazla işlem izlemeleri olması gerekir.  İşlem için profil oluşturucu başlatmak için tıklatın **profil oluşturucu izlemeleri**.
5.  Genel işlem süresi için temel nedeni tanılamanıza için izleme olayları tek tek her işlem için gösterir.  En uzun süresi olan üst örneklerinden birini tıklatın.
6.  Tıklatın **Göster etkin yolunuzda** belirli yolun toplam işlem süresi için en çok katkıda olayların vurgulamak için.  Bu örnekte, en yavaş çağrısı kaynaklı olduğunu görebilirsiniz *FabrikamFiberAzureStorage.GetStorageTableData* yöntemi. Çoğu zaman alır parçasıdır *CloudTable.CreateIfNotExist* yöntemi. İşlevi çağrıldığında her zaman bu kod satırı yürütülürse, gereksiz ağ çağrısı ve CPU kaynak kullanılır. Kodunuzu düzeltmek için en iyi yalnızca için bir kez yürütme bazı başlangıç yönteminde bu satırı yerleştirilecek yoludur. 

    ![Profil Oluşturucu ayrıntıları](media/app-insights-tutorial-performance/profiler-details.png)

7.  **Performans İpucu** ekranın üstünde bekleme nedeniyle aşırı aralıktır değerlendirme destekler.  Tıklatın **bekleyen** farklı türlerdeki olayların yorumlanması hakkında belgeler için bağlantı.

    ![Performans İpucu](media/app-insights-tutorial-performance/performance-tip.png)

8.  Daha fazla çözümleme için tıklattığınız **karşıdan .etl izleme** izlemede Visual Studio'ya karşıdan yüklemek için.

## <a name="use-analytics-data-for-server"></a>Sunucusu için analiz verileri kullanma
Uygulama Öngörüler Analytics Application Insights tarafından toplanan tüm verileri çözümlemek sağlayan zengin bir sorgu dili sağlar.  Bu istek ve performans verilerini üzerinde derin çözümleme gerçekleştirmek için kullanabilirsiniz.

1. İşlemi ayrıntı Masası'na dönün ve analizi düğmesini tıklatın.

    ![Analytics düğmesi](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Uygulama Öngörüler Analytics her panelinde görünüm için bir sorgu ile açılır.  Olan veya gereksinimleriniz için değiştirmek gibi bu sorguları çalıştırabilirsiniz.  İlk sorgu zaman içinde bu işlem süresince gösterir.

    ![Analiz](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Yavaş istemci işlemleri tanımlayın
Application Insights iyileştirmek için sunucu işlemleri tanımlamaya ek olarak, istemci tarayıcıları açısından çözümleyebilirsiniz.  Bu istemci bileşenleri için olası geliştirmeleri tanımlamak ve hatta farklı tarayıcılar veya farklı konumlarda sorunları belirlemenize yardımcı olabilir.

1. Seçin **tarayıcı** altında **Araştır** Özet tarayıcıyı açın.  Bu, çeşitli telemetries uygulamanızın tarayıcı perspektifinden görsel özetini sağlar.

    ![Tarayıcı özeti](media/app-insights-tutorial-performance/browser-summary.png)

2.  Ekranı aşağı kaydırarak **yavaş sayfalarımı nelerdir?**.  Bu istemcilerin yüklemek en uzun zaman almış, uygulamanızdaki sayfaların listesini gösterir.  Kullanıcıya en önemli etkiye sahip bu sayfaları önceliğini belirlemek için bu bilgileri kullanın.
3.  Açmak için sayfaları birini tıklatın **sayfa görünümü** paneli.  Örnekte, **/FabrikamProd** sayfasını aşırı bir ortalama süre gösteren.  **Sayfa görünümü** paneli farklı süre aralıkları dökümünü dahil olmak üzere bu sayfayı hakkında ayrıntılar sağlar.

    ![Sayfa görünümü](media/app-insights-tutorial-performance/page-view.png)

4.  Bu istekler ayrıntılarını incelemek için en yüksek süre'yi tıklatın.  Tarayıcı ve konumunu türünü de dahil olmak üzere sayfa isteyen istemci ayrıntılarını görüntülemek için ayrı istek'ye tıklayın.  Bu bilgiler performans sorunları ile ilgili belirli istemci türlerini olup olmadığını belirlemenize yardımcı olabilir.

    ![İstek Ayrıntıları](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>İstemci için analiz verileri kullanın
Sunucu performansını toplanan verileri gibi Application Insights tüm istemci verilerini analiz kullanarak derin çözümleme için kullanılabilir hale getirir.

1. Tarayıcıya Özet dönün ve analizi simgesine tıklayın.

    ![Analytics simgesi](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Uygulama Öngörüler Analytics her panelinde görünüm için bir sorgu ile açılır. İlk sorguyu farklı sayfa görünümlerini süresince zamanla gösterilir.

    ![Analiz](media/app-insights-tutorial-performance/client-analytics.png)

3.  Akıllı Tanılama verileri benzersiz düzenleri tanımlar uygulaması Öngörüler Analytics özelliğidir.  Çizgi grafikte akıllı tanılama nokta tıkladığınızda, aynı sorgu anomali neden olan kayıtların çalışır.  Aşırı süresi neden olan bu sayfa görünümlerini özelliklerini tanımlayabilmeniz kayıtların ayrıntılarını sorgu Açıklama bölümünde gösterilir.

    ![Akıllı tanılama](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Sonraki adımlar
Çalışma zamanı özel durumları tanımlamak nasıl öğrendiğinize göre yanıt hatalarına uyarılar oluşturma hakkında bilgi edinmek için sonraki öğretici için ilerleyin.

> [!div class="nextstepaction"]
> [Üzerinde uygulama sistem durumu Uyarısı](app-insights-tutorial-alert.md)
