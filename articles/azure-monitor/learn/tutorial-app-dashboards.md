---
title: Azure Application Insights’ta özel panolar oluşturma | Microsoft Docs
description: Azure Application Insights’ı kullanarak özel KPI panoları oluşturma öğreticisi.
keywords: ''
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: e008a53ce71a55b344dfc3a76bdb4ae39b954c46
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104888"
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Azure Application Insights’ı kullanarak özel KPI panoları oluşturma

Azure portalında, her biri farklı kaynak gruplarında ve aboneliklerde yer alan çeşitli Azure kaynaklarındaki verilerin görselleştirildiği kutucular içeren birden çok pano oluşturabilirsiniz.  Uygulamanızın durumunu ve performansını tam olarak gösteren özel panolar oluşturmak için, Azure Application Insights’daki farklı grafikleri ve görünümleri sabitleyebilirsiniz.  Bu öğretici, Azure Application Insights’taki çeşitli veri ve görselleştirme türlerini içeren özel bir pano oluşturma konusunda size rehberlik etmektedir.  Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure’da özel bir pano oluşturma
> * Kutucuk Galerisinden kutucuk ekleme
> * Application Insights’taki standart ölçümleri panoya ekleme 
> * Özel bir Application Insights ölçüm grafiğini panoya ekleme
> * Analytics sorgusunun sonuçlarını panoya ekleme 



## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Azure’a .NET uygulaması dağıtma ve [Application Insights SDK’sını etkinleştirme](../../azure-monitor/app/asp-net.md). 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-new-dashboard"></a>Yeni pano oluşturma
Tek bir pano çeşitli uygulamalardan, kaynak gruplarından ve aboneliklerden gelen kaynakları içerebilir.  Uygulamanız için yeni bir pano oluşturarak öğreticiyi kullanamaya başlayın.  

2.  Portalın ana ekranında **Yeni pano**’yu seçin.

    ![Yeni pano](media/tutorial-app-dashboards/new-dashboard.png)

3. Pano için bir ad yazın.
4. Panonuza ekleyebileceğiniz kutucuk çeşitleri için **Kutucuk Galerisi**’ne göz atın.  Galeriden kutucuk eklemenin yanı sıra Application Insights’taki grafikleri ve diğer görünümleri panoya doğrudan sabitleyebilirsiniz.
5. **Markdown** kutucuğunu bulun ve panonuza sürükleyin.  Bu kutucuk, panonuza açıklayıcı metinler eklemek için ideal olan markdown biçiminde metin eklemenize izin verir.
6. Metni kutucuğun özelliklerine ekleyin ve pano tuvalinizde yeniden boyutlandırın.
    
    ![Markdown kutucuğunu düzenleme](media/tutorial-app-dashboards/edit-markdown.png)

6. Kutucuk özelleştirme modundan çıkmak için ekranın üst kısmındaki **Özelleştirme bitti** seçeneğine ve ardından değişikliklerinizi kaydetmek için **Değişiklikleri yayımla**’ya tıklayın.

    ![Markdown kutucuğu içeren pano](media/tutorial-app-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Sistem durumuna genel bakış ekleme
Yalnızca statik metin içeren bir pano ilgi çekici olmadığından, uygulamanızla ilgili bilgileri göstermesi için Application Insights’tan bir kutucuk ekleyebilirsiniz.  Kutucuk Galerisi’nden Application Insights kutucukları ekleyebilir veya bunları Application Insights ekranlarından doğrudan sabitleyebilirsiniz.  Bu, bildiğiniz grafikleri ve görünümleri panonuza sabitlemeden önce yapılandırmanıza olanak tanır.  İlk olarak uygulamanız için sistem durumuna standart genel bakışı ekleyin.  Bu işlem yapılandırma gerektirmez ve panoda çok az özelleştirme yapmaya izin verir.


1. Azure menüsünden **Application Insights**’ı seçin ve ardından uygulamanızı seçin.
2. **Zaman çizelgesine genel bakış** bölümünde bağlam menüsünü seçin ve **Panoya sabitle** seçeneğine tıklayın.  Bu, kutucuğu görüntülediğiniz en son panoya ekler.  

    ![Zaman çizelgesine genel bakışı sabitleme](media/tutorial-app-dashboards/pin-overview-timeline.png)
 
3. Panonuza dönmek için ekranın üst kısımdaki **Panoyu görüntüle** seçeneğine tıklayın.
4. Zaman çizelgesine genel bakış panonuza eklenir.  Öğeye tıklayıp istediğiniz yere sürükleyin ve sonra **Özelleştirme bitti** ve **Değişiklikleri yayımla** seçeneklerine tıklayın.  Artık panonuz yararlı bilgiler içeren bir kutucuğa sahip olur.

    ![Zaman çizelgesine genel bakış içeren pano](media/tutorial-app-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Özel ölçüm grafiği ekleme
**Ölçümler** paneli, isteğe bağlı filtreler ve gruplandırma ile zaman içinde Application Insights tarafından toplanan bir ölçüm grafiği oluşturmanıza olanak tanır.  Application Insights’taki her öğe gibi bu grafiği de panoya ekleyebilirsiniz.  Bu işlem öncelikle birkaç basit özelleştirme gerektirir.

1. Azure menüsünden **Application Insights**’ı seçin ve ardından uygulamanızı seçin.
1. **Ölçümler**’i seçin.  
2. Boş bir grafik oluşturulur ve bir ölçüm eklemeniz istenir.  Grafiğe bir ölçüm ve isteğe bağlı olarak bir filtre ve gruplandırma ekleyin.  Aşağıdaki örnekte başarı ölçütüne göre gruplandırılmış sunucu isteklerinin sayısı gösterilmektedir.  Bu, başarılı ve başarısız isteklerin sürekli bir görünümünü sunar.

    ![Ölçüm ekleme](media/tutorial-app-dashboards/metrics-chart.png)

4. Grafiğin bağlam menüsünü seçin ve ardından **Panoya sabitle** seçeneğini belirleyin.  Bu, görünümü üzerinde çalıştığınız en son panoya ekler.

    ![Ölçüm grafiğini sabitleme](media/tutorial-app-dashboards/pin-metrics-chart.png)

3. Panonuza dönmek için ekranın üst kısımdaki **Panoyu görüntüle** seçeneğine tıklayın.

4. Zaman Çizelgesi Ölçümleri Grafiği, panonuza eklenir. Öğeye tıklayıp istediğiniz yere sürükleyin ve sonra **Özelleştirme bitti** ve **Değişiklikleri yayımla** seçeneklerine tıklayın. 

    ![Ölçümleri içeren pano](media/tutorial-app-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Ölçüm Gezgini
**Ölçüm Gezgini**, Ölçümler’e benzer, ancak panoya eklenirken çok daha fazla özelleştirme yapmanıza izin verir.  Ölçümlerinizin grafiğini oluşturmak için kullandığınız seçenek belirli tercihlerinize ve gereksinimlerinize bağlıdır.

1. Azure menüsünden **Application Insights**’ı seçin ve ardından uygulamanızı seçin.
1. **Ölçüm Gezgini**’ni seçin. 
2. Grafiği düzenlemek için tıklayıp bir veya daha fazla ölçüm ve isteğe bağlı olarak ayrıntılı bir yapılandırmayı seçin.  Bu örnekte, ortalama sayfa yanıt süresini izleyen bir çizgi grafik görüntülenmektedir.
3. Grafiği panonuza eklemek için sağ üstteki raptiye simgesine tıklayın ve ardından istediğiniz konuma sürükleyin.

    ![Ölçüm Gezgini](media/tutorial-app-dashboards/metrics-explorer.png)

4. Ölçüm Gezgini kutucuğu panoya eklendikten sonra daha fazla özelleştirme yapmanıza izin verir.  Kutucuğa sağ tıklayın ve özel bir kutucuk eklemek için **Kutucuğu düzenle**’yi seçin.  İstiyorsanız başka özelleştirmeler de gerçekleştirebilirsiniz.

    ![Ölçüm gezginini içeren pano](media/tutorial-app-dashboards/dashboard-04a.png)

5. Ölçüm Gezgini artık panonuza eklenir.

    ![Ölçüm gezginini içeren pano](media/tutorial-app-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Analytics sorgusu ekleme
Azure Application Insights Analytics, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlar.  Grafikler ve diğer görünümlerde olduğu gibi, bir Analytics sorgusunun çıktısını panonuza ekleyebilirsiniz.   

Azure Applications Insights Analytics, ayrı bir hizmet olduğundan Analytics sorgusunu içermesi için panonuzu paylaşmanız gerekir.  Bir Azure panosunu paylaştığınızda panoyu, diğer kullanıcıların ve kaynakların kullanabileceği bir Azure kaynağı olarak yayımlamış olursunuz.  

1. Pano ekranının üst kısmındaki **Paylaş** seçeneğine tıklayın.

    ![Panoyu yayımlama](media/tutorial-app-dashboards/publish-dashboard.png)

2. Panoyu paylaşmak için **Pano adını** aynı tutun ve **Abonelik Adı**’nı.  **Yayımla**’ta tıklayın.  Pano artık diğer hizmetler ve abonelikler tarafından kullanılabilir.  İsteğe bağlı olarak panoya erişebilecek belirli kullanıcıları tanımlayabilirsiniz.
1. Azure menüsünden **Application Insights**’ı seçin ve ardından uygulamanızı seçin.
2. Analytics portalını açmak için ekranın üst kısmındaki **Analytics** seçeneğine tıklayın.

    ![Analytics’i başlatma](media/tutorial-app-dashboards/start-analytics.png)

3. En çok istekte bulunulan 10 sayfayı ve bunlara ait istek sayılarını döndüren şu sorguyu yazın:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Sorgunun sonuçlarını doğrulamak için **Git**’e tıklayın.
5. Raptiye simgesine tıklayın ve panonuzun adını seçin.  Bu seçenekte bir pano seçmek zorunda kalmanızın nedeni, en son panonun kullanıldığı önceki adımların aksine Analytics konsolunun ayrı bir hizmet olmasıdır, paylaşılan mevcut panoların tümünden seçim yapmanız gerekir.

    ![Analytics sorgusunu sabitleme](media/tutorial-app-dashboards/analytics-pin.png)

5. Panoya dönmeden önce başka bir sorgu daha ekleyin, ancak bu sefer bir Analytics sorgusunu panoda görselleştirmenin farklı yollarını öğrenmek için sorguyu bir grafik olarak işleyin.  En çok istisnayı içeren ilk 10 işlemi özetleyen aşağıdaki sorguyu kullanın.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. **Grafik** seçeneğini belirleyin ve ardından çıktıyı görselleştirmek için seçimi **Halka** olarak değiştirin.

    ![Analytics grafiği](media/tutorial-app-dashboards/analytics-chart.png)

6. Grafiği panonuza sabitlemek için raptiye simgesine tıklayın ve bu sefer panonuza döndürmek için bağlantıyı seçin.
4. Sorguların sonuçları seçtiğini biçimde panonuza eklenir.  Sorgulara tıklayıp istediğiniz konuma sürükleyin ve ardından **Düzenleme bitti**’ye tıklayın.
5. Her bir kutucuğa sağ tıklayın ve açıklayıcı bir başlık eklemek için **Başlığı Düzenle** seçeneğini belirleyin.

    ![Analytics içeren pano](media/tutorial-app-dashboards/dashboard-05.png)

5. Application Insights’a ait çeşitli grafikler ve görselleştirmeler içeren panonuzdaki değişiklikleri işlemek için **Değişiklikleri yayımla**’ya tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Artık nasıl özel pano oluşturulacağını öğrendiniz. Bir örnek olay içeren diğer Application Insights belgelerine de göz atabilirsiniz.

> [!div class="nextstepaction"]
> [Derin tanılama](../../azure-monitor/app/devops.md)
