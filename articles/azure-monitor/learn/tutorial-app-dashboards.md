---
title: Azure Application Insights’ta özel panolar oluşturma | Microsoft Docs
description: Azure Application Insights’ı kullanarak özel KPI panoları oluşturma öğreticisi.
keywords: ''
services: application-insights
author: lgayhardt
ms.author: lagayhar
ms.date: 01/11/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 3abe0511200bf5828b485b15a4b8a512731c4ffa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540888"
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

1. Pano bölmeden **yeni Pano**.

   ![Yeni pano](media/tutorial-app-dashboards/1newdashboard.png)

1. Pano için bir ad yazın.
1. Panonuza ekleyebileceğiniz kutucuk çeşitleri için **Kutucuk Galerisi**’ne göz atın.  Galeriden kutucuk eklemenin yanı sıra Application Insights’taki grafikleri ve diğer görünümleri panoya doğrudan sabitleyebilirsiniz.
1. **Markdown** kutucuğunu bulun ve panonuza sürükleyin.  Bu kutucuk, panonuza açıklayıcı metinler eklemek için ideal olan markdown biçiminde metin eklemenize izin verir.
1. Metni kutucuğun özelliklerine ekleyin ve pano tuvalinizde yeniden boyutlandırın.
    
    ![Markdown kutucuğunu düzenleme](media/tutorial-app-dashboards/2dashboard-text.png)

1. Tıklayın **özelleştirme Bitti** kutucuğu özelleştirme modundan çıkmak için ekranın üst kısmındaki.

## <a name="add-health-overview"></a>Sistem durumuna genel bakış ekleme
Yalnızca statik metin içeren bir pano ilgi çekici olmadığından, uygulamanızla ilgili bilgileri göstermesi için Application Insights’tan bir kutucuk ekleyebilirsiniz.  Kutucuk Galerisi’nden Application Insights kutucukları ekleyebilir veya bunları Application Insights ekranlarından doğrudan sabitleyebilirsiniz.  Bu, bildiğiniz grafikleri ve görünümleri panonuza sabitlemeden önce yapılandırmanıza olanak tanır.  İlk olarak uygulamanız için sistem durumuna standart genel bakışı ekleyin.  Bu işlem yapılandırma gerektirmez ve panoda çok az özelleştirme yapmaya izin verir.


1. Seçin, **Application Insights** giriş ekranına kaynakta.
2. İçinde **genel bakış** bölmesinde, kutucuğu görüntülediğiniz en son panoya eklemek için Raptiye simgesine tıklayın.  

    ![Zaman çizelgesine genel bakışı sabitleme](media/tutorial-app-dashboards/3overview.png)
 
3. Sağ üst bölümde, kutucuk, panonuza sabitlendi bir bildirim görüntülenir. Tıklayın **panoya sabitlenmiş** bildirim panonuza geri dönün veya Pano bölmesini kullanın.
4. Bu kutucuk artık panonuza eklenir. Seçin **Düzenle** döşemenin konumlandırmasını değiştirmek için. ' A tıklayın ve BT istediğiniz konuma sürükleyin ve ardından **özelleştirme Bitti**. Artık panonuz yararlı bilgiler içeren bir kutucuğa sahip olur.

    ![Zaman çizelgesine genel bakış içeren pano](media/tutorial-app-dashboards/4dashboard-edit.png)



## <a name="add-custom-metric-chart"></a>Özel ölçüm grafiği ekleme
**Ölçümler** paneli, isteğe bağlı filtreler ve gruplandırma ile zaman içinde Application Insights tarafından toplanan bir ölçüm grafiği oluşturmanıza olanak tanır.  Application Insights’taki her öğe gibi bu grafiği de panoya ekleyebilirsiniz.  Bu işlem öncelikle birkaç basit özelleştirme gerektirir.

1. Seçin, **Application Insights** giriş ekranına kaynakta.
1. **Ölçümler**’i seçin.  
2. Boş bir grafik oluşturulur ve bir ölçüm eklemeniz istenir.  Grafiğe bir ölçüm ve isteğe bağlı olarak bir filtre ve gruplandırma ekleyin.  Aşağıdaki örnekte başarı ölçütüne göre gruplandırılmış sunucu isteklerinin sayısı gösterilmektedir.  Bu, başarılı ve başarısız isteklerin sürekli bir görünümünü sunar.

    ![Ölçüm ekleme](media/tutorial-app-dashboards/5sumserverrequests.png)

4. Seçin **panoya Sabitle** sağ. Bu, görünümü üzerinde çalıştığınız en son panoya ekler.

    ![Ölçüm grafiğini sabitleme](media/tutorial-app-dashboards/6sumserverrequests-pin.png)

3.  Sağ üst bölümde, kutucuk, panonuza sabitlendi bir bildirim görüntülenir. Tıklayın **panoya sabitlenmiş** bildirim panonuza geri dönün veya Pano dikey penceresini kullanın.

4. Bu kutucuk artık panonuza eklenir. Seçin **Düzenle** döşemenin konumlandırmasını değiştirmek için. ' A tıklayın ve BT istediğiniz konuma sürükleyin ve ardından **özelleştirme Bitti**.

    ![Ölçümleri içeren pano](media/tutorial-app-dashboards/7dashboard-edit2.png)

## <a name="add-analytics-query"></a>Analytics sorgusu ekleme
Azure Application Insights Analytics, Application Insights tarafından toplanan tüm verileri analiz etmeye yönelik zengin bir sorgu dili sağlar.  Grafikler ve diğer görünümlerde olduğu gibi, bir Analytics sorgusunun çıktısını panonuza ekleyebilirsiniz.   

Azure Applications Insights Analytics, ayrı bir hizmet olduğundan Analytics sorgusunu içermesi için panonuzu paylaşmanız gerekir. Bir Azure panosunu paylaştığınızda panoyu, diğer kullanıcıların ve kaynakların kullanabileceği bir Azure kaynağı olarak yayımlamış olursunuz.  

1. Pano ekranının üst kısmındaki **Paylaş** seçeneğine tıklayın.

    ![Panoyu yayımlama](media/tutorial-app-dashboards/8dashboard-share.png)

2. Panoyu paylaşmak için **Pano adını** aynı tutun ve **Abonelik Adı**’nı.  **Yayımla**’ta tıklayın.  Pano artık diğer hizmetler ve abonelikler tarafından kullanılabilir.  İsteğe bağlı olarak panoya erişebilecek belirli kullanıcıları tanımlayabilirsiniz.
1. Seçin, **Application Insights** giriş ekranına kaynakta.
2. Analytics portalını açmak için ekranın üst kısmındaki **Analytics** seçeneğine tıklayın.

    ![Analytics’i başlatma](media/tutorial-app-dashboards/9analytics.png)

3. En çok istekte bulunulan 10 sayfayı ve bunlara ait istek sayılarını döndüren şu sorguyu yazın:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Tıklayın **çalıştırma** sorgunun sonuçlarını doğrulamak için.
5. Raptiye simgesine tıklayın ve panonuzun adını seçin. Bu seçenekte bir pano seçmek zorunda kalmanızın nedeni, en son panonun kullanıldığı önceki adımların aksine Analytics konsolunun ayrı bir hizmet olmasıdır, paylaşılan mevcut panoların tümünden seçim yapmanız gerekir.

    ![Analytics sorgusunu sabitleme](media/tutorial-app-dashboards/10query.png)

5. Panoya dönmeden önce başka bir sorgu daha ekleyin, ancak bu sefer bir Analytics sorgusunu panoda görselleştirmenin farklı yollarını öğrenmek için sorguyu bir grafik olarak işleyin.  En çok istisnayı içeren ilk 10 işlemi özetleyen aşağıdaki sorguyu kullanın.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. **Grafik** seçeneğini belirleyin ve ardından çıktıyı görselleştirmek için seçimi **Halka** olarak değiştirin.

    ![Analytics grafiği](media/tutorial-app-dashboards/11querychart.png)

6. Grafiği panonuza sabitlemek için raptiye simgesine tıklayın ve bu sefer panonuza döndürmek için bağlantıyı seçin.
4. Sorguların sonuçları seçtiğini biçimde panonuza eklenir.  ' A tıklayın ve her konuma sürükleyin ve ardından **özelleştirme Bitti**.
5. Bunları açıklayıcı bir başlık vermeye her başlık çubuğunda kalem simgesini seçin.

    ![Analytics içeren pano](media/tutorial-app-dashboards/12edit-title.png)

5. Seçin **paylaşımı** değişikliklerinizi şimdi çeşitli grafikler ve görselleştirmeler Application ınsights'tan içeren panonuza yeniden yayımlamak için.


## <a name="next-steps"></a>Sonraki adımlar
Artık nasıl özel pano oluşturulacağını öğrendiniz. Bir örnek olay içeren diğer Application Insights belgelerine de göz atabilirsiniz.

> [!div class="nextstepaction"]
> [Derin tanılama](../../azure-monitor/app/devops.md)
