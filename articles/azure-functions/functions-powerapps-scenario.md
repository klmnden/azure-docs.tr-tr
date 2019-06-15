---
title: Powerapps'ten bir işlev çağırma | Microsoft Docs
description: Özel bağlayıcı oluşturma, ardından bu bağlayıcıyı kullanarak bir işlev çağırın.
services: functions
keywords: Bulut uygulamaları, bulut Hizmetleri, PowerApps, iş süreçleri, iş kolu uygulaması
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: glenga
ms.reviewer: sunayv
ms.custom: ''
ms.openlocfilehash: 26f6502f63b39d3f1ecf8dfeb09c8df4daa63b68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65786134"
---
# <a name="call-a-function-from-powerapps"></a>PowerApps’ten bir işlev çağırma
[PowerApps](https://powerapps.microsoft.com) platform, geleneksel uygulama kodu olmadan uygulamalar oluşturmak, iş uzmanları için tasarlanmıştır. Profesyonel Geliştiriciler, Azure işlevleri, PowerApps uygulama derleyicileri from teknik ayrıntıları koruma sırasında PowerApps, yeteneklerini genişletmek için kullanabilirsiniz.

Bu konuda bakım senaryosunda Rüzgar turbines için temel bir uygulama oluşturun. Bu konu içinde tanımlanan işlevinin nasıl çağrıldığını gösterir [bir işlev için Openapı tanımı oluşturma](functions-openapi-definition.md). İşlevi, bir Rüzgar türbini Acil onarımın uygun maliyetli olup olmadığını belirler.

![Powerapps'teki tamamlanmış uygulama](media/functions-powerapps-scenario/finished-app.png)

Microsoft Flow aynı işlev çağırma hakkında daha fazla bilgi için bkz: [Microsoft Flow bir işlevi çağırmayı](functions-flow-scenario.md).

Bu konu başlığında, şunların nasıl yapılır:

> [!div class="checklist"]
> * Örnek verileri Excel'de hazırlayın.
> * Bir API tanımını dışarı aktarın.
> * API için bir bağlantı ekleyin.
> * Bir uygulama oluşturun ve veri kaynaklarını ekleyin.
> * Uygulamasında verileri görüntülemek için denetimler ekleme.
> * İşlev çağrısı ve verileri görüntülemek için denetimler ekleme.
> * Bir onarım uygun maliyetli olup olmadığını belirlemek için uygulamayı çalıştırın.

[!INCLUDE [functions-openapi-note](../../includes/functions-openapi-note.md)]

## <a name="prerequisites"></a>Önkoşullar

+ Etkin bir [PowerApps hesabı](https://docs.microsoft.com/powerapps/maker/signup-for-powerapps) Azure hesabınızla oturum açma kimlik ile. 
+ Excel ve [Excel örnek dosyası](https://procsi.blob.core.windows.net/docs/turbine-data.xlsx) , uygulamanız için bir veri kaynağı olarak kullanır.
+ Bu öğreticiyi tamamlamak [bir işlev için Openapı tanımı oluşturma](functions-openapi-definition.md).

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>API'ye bağlantı ekleme
Powerapps'te özel API'yi (aynı zamanda özel bağlayıcı olarak da bilinir) kullanılabilir, ancak bir uygulamada kullanabilmeniz için önce bir API'ye bağlantısı gerekir.

1. İçinde [web.powerapps.com](https://web.powerapps.com), tıklayın **bağlantıları**.

    ![PowerApps bağlantıları](media/functions-powerapps-scenario/powerapps-connections.png)

1. Tıklayın **yeni bağlantı**, ekranı aşağı kaydırarak **Türbin onarımı** bağlayıcısını bulup tıklayın.

    ![Yeni bağlantı](media/functions-powerapps-scenario/new-connection.png)

1. API anahtarını girin ve tıklayın **Oluştur**.

    ![Bağlantı oluşturma](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Uygulamanızı başkalarıyla paylaştığınızda, çalışıyor veya uygulamayı kullanan her kişi API'sine bağlanmak için API anahtarı da girmeniz gerekir. Bu davranış gelecekte değişebilir ve, değişimi yansıtmak için bu konuyu güncelleştireceğiz.

## <a name="create-an-app-and-add-data-sources"></a>Bir uygulama oluşturun ve veri kaynaklarını ekleyin
Artık Powerapps'te uygulama oluşturma ve uygulama için veri kaynağı olarak Excel verilerini ve özel API'yi eklemek hazırsınız.

1. [web.powerapps.com](https://web.powerapps.com)’da, **Sıfırdan başlayın** > ![Telefon uygulaması simgesi](media/functions-powerapps-scenario/icon-phone-app.png) (telefon) > **Bu uygulamayı yap**’ı seçin.

    ![Sıfırdan - phone uygulaması Başlat](media/functions-powerapps-scenario/create-phone-app.png)

    Uygulamayı web için PowerApps Studio içinde açılır. Aşağıdaki görüntüde, PowerApps Studio farklı kısımlarını gösterir.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(A) sol gezinti çubuğunda**, her bir ekrandaki denetimlerin hiyerarşik bir görünümü bakın

    **(B) Ortadaki bölmeden**, üzerinde çalıştığınız ekranın gösterildiği

    **(C) sağ bölmesinde**, Düzen ve veri kaynakları gibi seçenekleri ayarladığınız yerdir

    **(D) özelliği** aşağı açılan listesinde ilgili formüllerin uygulanacağı özellikleri seçtiğiniz

    **(E) formül çubuğuna**, uygulama davranışını belirleyen formülleri (Excel'de olduğu gibi) eklediğiniz yerdir
    
    **(F) Şerit**, burada, denetimleri eklediğiniz ve tasarım öğelerini özelleştirdiğiniz

1. Excel dosyasını veri kaynağı olarak ekleyin.

    Alacağınız veriler aşağıdaki gibi görünür:

    ![Excel verilerini içeri aktarmak için](media/functions-powerapps-scenario/excel-table.png)

    1. Uygulama tuvali üzerinde **veriye bağlan**’ı seçin.

    1. Üzerinde **veri** panelinde, tıklayın **uygulamanıza statik veriler ekleyin**.

        ![Veri kaynağı ekleme](media/functions-powerapps-scenario/add-static-data.png)

        Normalde okuma ve bir dış kaynaktan veri yazma, ancak bu bir örnek olduğundan statik verileri olarak Excel verilerini ekliyoruz.

    1. Kaydettiğiniz Excel dosyasına gidin, seçin **Turbines** tablosuna sağ tıklayıp tıklayın **Connect**.

        ![Veri kaynağı ekleme](media/functions-powerapps-scenario/choose-table.png)


1. Özel API'yi bir veri kaynağı olarak ekleyin.

    1. Üzerinde **veri** panelinde, tıklayın **veri kaynağı Ekle**.

    1. Tıklayın **Türbin onarımı**.

        ![Türbin onarımı Bağlayıcısı](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Uygulamasında verileri görüntülemek için denetimler ekleme
Mevcut veri kaynakları, türbinin verileri görüntülemek için uygulamanıza bir ekran ekleyin.

1. Üzerinde **giriş** sekmesinde **yeni ekran** > **liste ekranı**.

    ![Liste ekranı](media/functions-powerapps-scenario/list-screen.png)

    PowerApps içeren bir ekran ekler bir *galeri* öğeleri ve arama, sıralama ve filtreleme sağlayan diğer denetimleri görüntülenecek.

1. İçin başlık çubuğunu değiştirme `Turbine Repair`ve böylelikle daha fazla denetim altındaki yer galeriyi yeniden boyutlandırın.

    ![Başlığı değiştirmek ve galeriyi yeniden boyutlandırın](media/functions-powerapps-scenario/gallery-title.png)

1. Galeri seçildiğinde, sağ bölmede altında ile **özellikleri**, tıklayın **CustomGallerySample**.

    ![Veri kaynağını Değiştir](media/functions-powerapps-scenario/change-data-source.png)

1. İçinde **veri** paneli, select **Turbines** listeden.

    ![Veri kaynağı seçme](media/functions-powerapps-scenario/select-data-source.png)

    Veri kümesi bu nedenle sonraki verileri daha iyi uyum sağlamak için düzeni değiştirin bir görüntü içermiyor. 

1. Hala **veri** panelinde, değiştirmek **Düzen** için **başlık, alt başlık ve gövde**.

    ![Galeri düzenini değiştirme](media/functions-powerapps-scenario/change-layout.png)

1. Son adımda olarak **veri** panelinde, galeride görüntülenen alanları değiştirin.

    ![Galeri alanları değiştirme](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** LastServiceDate =
    + **Subtitle2** ServiceRequired =
    + **Title2** başlık = 

1. Galeri seçildiğinde ayarlama **TemplateFill** özelliği şu formül olarak ayarlayın: `If(ThisItem.IsSelected, Orange, White)`.

    ![Şablon dolgusu formülü](media/functions-powerapps-scenario/formula-fill.png)

    Artık hangi galeri öğesinin seçili olup daha kolay olur.

    ![Seçili öğe](media/functions-powerapps-scenario/selected-item.png)

1. Özgün ekran gerekmez. Sol bölmede, üzerine **Screen1**, tıklayın **...** , ve **Sil**.

    ![Ekranı Sil](media/functions-powerapps-scenario/delete-screen.png)

1. Tıklayın **dosya**ve uygulama adı. Tıklayın **Kaydet** sol taraftaki menüde, ardından **Kaydet** sağ alt köşesinde.

Birçok tipik bir üretim uygulamasında yaptığınız diğer biçimlendirme, ancak bu senaryo için işleve çağrı - önemli kısımdan geçeceğiz.

## <a name="add-controls-to-call-the-function-and-display-data"></a>İşlev çağrısı ve verileri görüntülemek için denetimler ekleme
Oluşturduğunuz işlevini çağırır ve döndürülen verileri görüntüleyen her türbinin için Özet verileri görüntüler, şimdi ekleme zamanı geldi denetleyen bir uygulama vardır. Size Openapı tanımına adı biçimini temel işlev erişim; Bu durumda sahip `TurbineRepair.CalculateCosts()`.

1. Şerit üzerinde **Ekle** sekmesinde **düğmesi**. Aynı sekmede ardından **etiketi**

    ![Düğme ve etiket Ekle](media/functions-powerapps-scenario/insert-controls.png)

1. Düğme ve etiketi galerinin sürükleyin ve etiketi yeniden boyutlandırın. 

1. Düğme metnini seçin ve bunu değiştirmek `Calculate costs`. Uygulama şu resimdeki gibi görünmelidir.

    ![Uygulama düğmesi](media/functions-powerapps-scenario/move-button-label.png)

1. Düğmeyi seçin ve düğme için şu formülü girin **OnSelect** özelliği.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Bu formül düğmeye tıkladı ve seçili galeri öğesi varsa aşağıdakileri yapar yürütür bir **ServiceRequired** değerini `Yes`:

    + Temizler *koleksiyon* `DetermineRepair` önceki çağrılarından verileri kaldırmak için. Bir koleksiyon tablosal bir değişkendir.

    + İşleve çağrı döndürülen veri koleksiyonuna atar `TurbineRepair.CalculateCosts()`. 
    
        Geçirilen değerlerin işlevi geldiğini **EstimatedEffort** ve **MaxOutput** galeride seçilmiş öğe için alanları. Bu alanlar, galeride görüntülenmez, ancak bunlara formüllerde kullanmak hala erişilebilir.

1. Etiketi seçin ve etiketin için şu formülü girin **metin** özelliği.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Bu formülü kullanır `First()` ilk (ve tek) satırının erişmek için işlevi `DetermineRepair` koleksiyonu. Ardından işlevinin döndürdüğü üç değerden görüntüler: `message`, `costToFix`, ve `revenueOpportunity`. Uygulamayı ilk kez çalıştırılmadan önce bu değerleri boştur.

    Tamamlanmış uygulama şu resimdeki gibi görünmelidir.

    ![Çalıştırma önce tamamlanmış uygulama](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>Uygulamayı çalıştırma
Sahip olduğunuz bütün bir uygulama! Artık çalıştırın ve eylem işlev çağrıları görmek için zamanı geldi.

1. PowerApps Studio sağ alt köşesinde, çalıştırma düğmesine tıklayın: ![Uygulama Çalıştırma düğmesine](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Değerini Türbin seçin `Yes` için **ServiceRequired**, ardından **maliyetleri hesaplamak** düğmesi. Aşağıdaki görüntü gibi bir sonuç görmeniz gerekir.

    ![Powerapps'teki tamamlanmış uygulama](media/functions-powerapps-scenario/finished-app.png)

1. Hangi her işlevi tarafından döndürülen görmek için diğer turbines deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu konu başlığında, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Örnek verileri Excel'de hazırlayın.
> * Bir API tanımını dışarı aktarın.
> * API için bir bağlantı ekleyin.
> * Bir uygulama oluşturun ve veri kaynaklarını ekleyin.
> * Uygulamasında verileri görüntülemek için denetimler ekleme.
> * İşlev çağrısı ve verileri görüntülemek için denetimler ekleme
> * Bir onarım uygun maliyetli olup olmadığını belirlemek için uygulamayı çalıştırın.

PowerApps hakkında daha fazla bilgi için bkz: [Powerapps'e giriş](https://powerapps.microsoft.com/tutorials/getting-started/).

Azure işlevleri'ni kullanan diğer ilginç senaryoları hakkında bilgi edinmek için [Microsoft Flow bir işlevi çağırmayı](functions-flow-scenario.md) ve [Azure Logic Apps ile tümleşen bir işlev oluşturma](functions-twitter-email.md).
