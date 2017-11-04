---
title: "Azure Application Insights'ta özel panolar oluşturun | Microsoft Docs"
description: "Azure Application Insights kullanarak özel KPI panoları oluşturmak için Öğreticisi."
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0d2f98ca2fb39289b2916ddd24590924856507d6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Azure Application Insights kullanarak özel KPI panolar oluşturun

Birden çok panolar Azure portalında, farklı kaynak grupları ve abonelikler arasında birden çok Azure kaynaklardan veri görselleştirme her INCLUDE döşeme oluşturabilirsiniz.  Farklı grafiklerin ve sistem eksiksiz bir görünümünü ve uygulamanızın performansını sağlama özel panolar oluşturmak için Azure Application Insights görünümlerinden sabitleyebilirsiniz.  Bu öğreticide, verileri ve görselleştirmeleri Azure Application Insights gelen birden çok türde içeren özel bir Pano oluşturulması açıklanmaktadır.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure'da özel bir pano oluşturun
> * Döşeme Galeriden bir kutucuğu ekleyin
> * Standart ölçümleri Application Insights'ta için Pano ekleyin. 
> * Pano için özel bir ölçüm grafik Application Insights ekleyin
> * Analytics sorgu sonuçları için Pano ekleyin 



## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

- .NET uygulaması azure'a dağıtma ve [Application Insights SDK'sı etkinleştirmek](app-insights-asp-net.md). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
Oturum açtığınızda Azure portalında [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-new-dashboard"></a>Yeni bir pano oluşturun
Tek bir panoya birden çok uygulama, kaynak grupları ve abonelikleri kaynaklardan içerebilir.  Uygulamanız için yeni bir Pano oluşturarak başlangıç Öğreticisi.  

2.  Portalın ana ekranında seçin **yeni Pano**.

    ![Yeni Pano](media/app-insights-tutorial-dashboards/new-dashboard.png)

3. Pano için bir ad yazın.
4. Bakın **döşeme galeri** panonuza ekleyebilirsiniz döşeme çeşitli.  Galeriden döşeme ekleme yanı sıra grafikler ve diğer görünümlerden doğrudan Application Insights panoya sabitleyebilirsiniz.
5. Bulun **Markdown** döşeme ve panonuz açın sürükleyin.  Bu kutucuğu panonuza açıklayıcı metin eklemek için ideal olan markdown'da biçimlendirilmiş metin eklemenize olanak sağlar.
6. Metin döşemesinin özellikleri ve Pano tuvalde yeniden boyutlandırın.
    
    ![Markdown döşeme Düzenle](media/app-insights-tutorial-dashboards/edit-markdown.png)

6. Tıklatın **özelleştirme Bitti** döşeme özelleştirme modundan çıkmak için ekranın üstünde ve ardından **yayımlama değişiklikleri** yaptığınız değişiklikleri kaydetmek için.

    ![Markdown döşeme içeren Pano](media/app-insights-tutorial-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Sistem durumu genel bakış ekleme
Yalnızca statik metin içeren bir Pano çok ilginç değilse, artık uygulamanız hakkındaki bilgileri görüntülemek için Application Insights'den bir kutucuğu ekleyin.  Application Insights döşeme döşeme Galeriden ekleyebilir veya doğrudan Application Insights ekranlarından sabitleyebilirsiniz.  Bu grafikler ve zaten panonuza sabitlemeden önce farkları bilmiyorsanız görünümleri yapılandırmanıza olanak sağlar.  Uygulamanız için standart sistem genel bakış ekleyerek başlayın.  Bu yapılandırma gerektirir ve en az özelleştirme panosunda izin verir.


1. Seçin **Application Insights** Azure menüsünde ve ardından uygulamanızı seçin.
2. İçinde **genel bakış zaman çizelgesi**, bağlam menüsünü seçin ve tıklatın **panoya Sabitle**.  Bu kutucuğu görüntülemekte olduğunuz son panoya ekler.  

    ![PIN genel bakış zaman çizelgesi](media/app-insights-tutorial-dashboards/pin-overview-timeline.png)
 
3. Ekranın üstündeki **görünüm Pano** panonuza döndürülecek.
4. Genel Bakış zaman çizelgesi şimdi panonuza eklenir.  Tıklayın ve yerine sürükleyin ve ardından **özelleştirme Bitti** ve **yayımlama değişiklikleri**.  Panonuz artık bazı yararlı bilgiler içeren bir kutucuğa sahiptir.

    ![Genel Bakış zaman çizelgesi içeren Pano](media/app-insights-tutorial-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Özel ölçüm grafik ekleyin
**Ölçümleri** paneli, isteğe bağlı filtreler ve gruplandırma Application Insights tarafından zaman içerisinde toplanır ölçüm grafik olanak sağlar.  Şey gibi Application Insights'ta, Pano için bu grafik ekleyebilirsiniz.  Bu biraz özelleştirme ilk yapmanızı gerektirir.

1. Seçin **Application Insights** Azure menüsünde ve ardından uygulamanızı seçin.
1. Seçin **ölçümleri**.  
2. Boş bir grafik zaten oluşturulur ve bir ölçüm eklemeniz istenir.  Grafiğe bir ölçüm ekleyin ve isteğe bağlı olarak bir filtre ve bir gruplandırma ekleyin.  Aşağıdaki örnek başarı göre gruplandırılmış sunucu istek sayısını gösterir.  Bu başarılı ve başarısız istekler çalışan bir görünümünü sağlar.

    ![Ölçüm Ekle](media/app-insights-tutorial-dashboards/metrics-chart.png)

4. Bağlam menüsünü seçin ve grafik **panoya Sabitle**.  Bu görünüm çalıştığınız son panoya ekler.

    ![PIN ölçüm grafik](media/app-insights-tutorial-dashboards/pin-metrics-chart.png)

3. Ekranın üstündeki **görünüm Pano** panonuza döndürülecek.

4. Genel Bakış zaman çizelgesi şimdi panonuza eklenir.  Tıklayın ve yerine sürükleyin ve ardından **özelleştirme Bitti** ve ardından **yayımlama değişiklikleri**. 

    ![Pano ölçümlerle](media/app-insights-tutorial-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Ölçüm Gezgini
**Ölçüm Gezgini** panoya eklendiğinde önemli ölçüde daha fazla özelleştirme sağlar ancak ölçümlere benzer.  Ölçümlerinizi graph için kullanacağınız, belirli tercih ve gereksinimlerine bağlıdır.

1. Seçin **Application Insights** Azure menüsünde ve ardından uygulamanızı seçin.
1. Seçin **ölçüm Gezgini**. 
2. Bir grafiği düzenlemek ve bir veya daha fazla ölçümleri ve isteğe bağlı olarak ayrıntılı bir yapılandırma seçmek için tıklatın.  Örneğin, ortalama sayfa yanıt süresi izleme bir çizgi grafiği görüntüler.
3. Üst sabitleme simgesine tıklayarak grafiği panonuza ekleyin ve yerine sürükleyin hakkı.

    ![Ölçüm Gezgini](media/app-insights-tutorial-dashboards/metrics-explorer.png)

4. Panoya eklendiğinde ölçüm Gezgini döşeme daha fazla özelleştirme sağlar.  Sağ tıklayın döşeme ve select **Düzenle başlık** özel başlık eklemek için.  Bir tane istiyorsanız, diğer özelleştirmeler.

    ![Ölçüm Gezgini ile Panosu](media/app-insights-tutorial-dashboards/dashboard-04a.png)

5. Şimdi panonuza eklenen ölçüm Gezgini grafik var.

    ![Ölçüm Gezgini ile Panosu](media/app-insights-tutorial-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Analytics sorgu ekleme
Azure uygulama Öngörüler Analytics Application Insights tüm verileri çözümlemek sağlayan zengin bir sorgu dili toplanan sağlar.  Grafikler ve diğer görünümlere yalnızca gibi panonuza Analytics sorgusu çıktısını ekleyebilirsiniz.   

Azure uygulamaları Öngörüler Analytics ayrı bir hizmet olduğundan, Pano Analytics sorgusu içerecek şekilde için paylaşmanız gerekir.  Bir Azure Pano paylaştığınızda, diğer kullanıcılar ve kaynaklar kullanabilmesi bir Azure kaynağı olarak yayımlayın.  

1. Pano ekranı üstündeki **paylaşımı**.

    ![Pano yayımlama](media/app-insights-tutorial-dashboards/publish-dashboard.png)

2. Tutmak **Pano adı** aynı ve select **abonelik adı** panoyu paylaşmak için.  **Yayımla**’ta tıklayın.  Pano artık diğer hizmetler ve abonelikler için kullanılabilir.  Pano erişimi olması gereken belirli kullanıcılar isteğe bağlı olarak tanımlayabilirsiniz.
1. Seçin **Application Insights** Azure menüsünde ve ardından uygulamanızı seçin.
2. Tıklatın **Analytics** Analytics Portalı'nı açmak için ekranın üstünde.

    ![Başlangıç analizi](media/app-insights-tutorial-dashboards/start-analytics.png)

3. En çok istenen 10 sayfaları ve bunların istek sayısını döndürür aşağıdaki sorguyu yazın:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Tıklatın **Git** sorgu sonuçlarını doğrulamak için.
5. PIN simgesine tıklayın ve panonuz adını seçin.  Analytics konsolunda ayrı bir hizmet olduğundan ve tüm kullanılabilir paylaşılan panolar seçmek gereken bu seçeneği, son Pano kullanıldığı önceki adımları farklı olarak bir Pano seçin olduğunu nedenidir.

    ![PIN Analytics sorgusu](media/app-insights-tutorial-dashboards/analytics-pin.png)

5. Panoya geri dönün önce başka bir sorguya ekleyin, ancak bu işleme zamanı, bir grafik olarak bir Pano Analytics sorguda görselleştirmek için farklı yollar görmeleri.  Çoğu istisnalar üst 10 işlemleri özetlenmektedir aşağıdaki sorguyu başlayın.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Seçin **grafik** ve sonra değiştirirsiniz bir **halka** çıkış görselleştirmek için.

    ![Analytics grafik](media/app-insights-tutorial-dashboards/analytics-chart.png)

6. Grafiği panonuza sabitlemek için PIN simgesine tıklayın ve bu süre, panoya dönmek için bağlantıyı seçin.
4. Sorguların sonuçlarını şimdi panonuza seçtiğiniz biçimde eklenir.  Tıklayın ve her konuma sürükleyin ve ardından **düzenleme Bitti**.
5. Sağ döşemeleri seçin ve her tıklayın **Düzenle başlık** açıklayıcı bir başlık vermek için.

    ![Pano Analytics ile](media/app-insights-tutorial-dashboards/dashboard-05.png)

5. Tıklatın **yayımlama değişiklikleri** şimdi çeşitli grafikler ve Application Insights gelen görselleştirmeleri içeren panonuza değişiklikleri uygulamak için.


## <a name="next-steps"></a>Sonraki adımlar
Özel panoları oluşturmayı öğrendiğinize göre bir örnek olay incelemesi dahil Application Insights belgeleri bekleyen bir görünüme sahiptir.

> [!div class="nextstepaction"]
> [Ayrıntılı tanılama](app-insights-devops.md)