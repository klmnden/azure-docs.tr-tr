---
title: Azure Application Insights ile Hızlı Başlangıç | Microsoft Docs
description: Application Insights ile izleme için bir ASP.NET Core Web uygulaması hızlı bir şekilde ayarlamak için yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 03/29/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 6deb3ac0869ba7d8dfb41cc6fac8c30fb035b5a6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64572716"
---
# <a name="start-monitoring-your-aspnet-core-web-application"></a>ASP.NET Core Web Uygulamanızı İzlemeye Başlama

Azure Application Insights ile web uygulamanızı kullanılabilirlik, performans ve kullanım bakımından kolayca izleyebilirsiniz. Ayrıca, bir kullanıcının bildirmesini beklemeden uygulamanızdaki hataları hızlıca tanımlayıp tespit edebilirsiniz. 

Bu hızlı başlangıçta, Application Insights SDK'sını var olan bir ASP.NET Core web uygulamasına eklerken size kılavuzluk eder. Application Insights Visual Studio kullanıma alma bu yapılandırma hakkında bilgi edinmek için [makale](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core-no-visualstudio).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
  - ASP.NET ve web geliştirme
  - Azure geliştirme
- [.NET Core 2.0 SDK yükleme](https://www.microsoft.com/net/core)
- Bir Azure Aboneliği ve var olan bir .NET Core web uygulaması gerekir.

Bir ASP.NET Core web uygulaması yoksa, adım adım kılavuzunu kullanabilirsiniz [bir ASP.NET Core uygulaması oluşturma ve Application Insights ekleyin.](../../azure-monitor/app/asp-net-core.md)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Application Insights, şirket içinde veya bulutta çalışmasından bağımsız olarak İnternet’e bağlı herhangi bir uygulamadan telemetri verilerini toplayabilir. Bu verileri görüntülemeyi başlatmak için aşağıdaki adımları kullanın.

1. **Kaynak oluştur** > **Geliştirici araçları** > **Application Insights** seçeneğini belirleyin.

   ![Application Insights Kaynağı ekleme](./media/dotnetcore-quick-start/1createresourceappinsight.png)

    Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Uygulama Türü** | ASP.NET web uygulaması | İzlemekte olduğunuz uygulamanın türü |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="configure-app-insights-sdk"></a>App Insights SDK’sını Yapılandırma

1. ASP.NET Core Web Uygulaması **projenizi** Visual Studio’da açın > **Çözüm Gezgini**’nde AppName öğesne sağ tıklayın > **Ekle** > **Application Insights Telemetrisi**’ni seçin.

    ![Application Insights Telemetrisi ekleme](./media/dotnetcore-quick-start/2vsaddappinsights.png)

2. Tıklayın **Başlarken** düğmesi

3. Hesabı ve aboneliği seçin > seçin **var olan kaynak** Azure portalında oluşturduğunuz > tıklatın **kaydetme**.

4. Uygulamanızı başlatmak için **Hata Ayıkla** > **Hata Ayıklamadan Başlat** (Ctrl+F5) öğesini seçin

    ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/3debug.png)

> [!NOTE]
> Verilerin portalda görünmesi 3-5 dakika sürer. Bu uygulama düşük trafikli bir test uygulaması ise, çoğu ölçümün yalnızca etkin istek veya işlem olduğunda yakalandığını aklınızda bulundurun.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Application ınsights'ı yeniden **genel bakış** seçerek Azure Portalı'nda sayfayı **giriş** ve mevcut durumda yürütülen hakkında ayrıntıları görüntülemek için önceden oluşturduğunuz kaynağın altında yeni bir kaynak seçin uygulama.

   ![Application Insights’a Genel Bakış Menüsü](./media/dotnetcore-quick-start/4overview.png)

2. Uygulama bileşenleriniz arasındaki bağımlılık ilişkilerinin görsel düzeni için **Uygulama haritası**’na tıklayın. Her bileşen yük, performans, hatalar ve uyarılar gibi KPI'leri gösterir.

   ![Uygulama Eşlemesi](./media/dotnetcore-quick-start/5appmap.png)

3. Tıklayarak **uygulama analizi** simgesi ![Uygulama Haritası simgesi](./media/dotnetcore-quick-start/006.png) **analytics'te görüntüle**. Bu işlem, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlayan **Application Insights Analizi**’ni açar. Bu örnekte, istek sayısını grafik olarak işleyen bir sorgu oluşturulur. Diğer verileri çözümlemek için kendi sorgularınızı yazabilirsiniz.

   ![Belirli bir süre içindeki kullanıcı isteklerinin analiz grafiği](./media/dotnetcore-quick-start/6analytics.png)

4. Geri dönüp **genel bakış** sayfasında ve KPI panoları inceleyin.  Bu pano, gelen istek sayısı, bu isteklerin süresi ve oluşan hatalar dahil olmak üzere uygulamanızın sistem durumu hakkında istatistikler sağlar. 

   ![Sistem Durumuna Genel Bakış zaman çizelgesi grafikleri](./media/dotnetcore-quick-start/7kpidashboards.png)

5. Sol tıklayın üzerinde **ölçümleri**. Sistem durumunu ve kaynak kullanımını araştırmak için ölçüm Gezgini'ni kullanın. **Yeni grafik ekle**’ye tıklayarak ek özel görünümler oluşturabilir veya **Düzenle**’yi seçerek mevcut grafik türlerini, yüksekliğini, renk paletini, gruplandırmaları ve ölçümleri değiştirebilirsiniz. Örneğin, ortalama tarayıcı sayfa yükleme süresi "Tarayıcı sayfa yükleme süresi" seçerek ölçümleri açılır ve "Ortalama" günlüklerden tutun gösteren bir grafik yapabilirsiniz. Azure ölçüm Gezgini ziyaret hakkında daha fazla bilgi edinmek için [Azure ölçüm Gezgini ile çalışmaya başlama](../../azure-monitor/platform/metrics-getting-started.md).

     ![Ölçümleri sekmesi: Ortalama tarayıcı sayfa yükleme zamanı grafiği](./media/dotnetcore-quick-start/8metrics.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
İşiniz bittiğinde test, kaynak grubunu silebilirsiniz ve tüm ilgili kaynakları. İçin aşağıdaki adımları izleyin.

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**’na tıklayın ve ardından **myResourceGroup**’a tıklayın.
2. Kaynak grubu sayfanızda, **Sil**’e tıklayın, metin kutusuna **myResourceGroup** yazın ve ardından **Sil**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çalışma zamanı özel durumlarını bulma ve tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-runtime-exceptions)
