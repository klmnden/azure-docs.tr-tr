---
title: "PowerApps bir işlevi çağırmak | Microsoft Docs"
description: "Özel bir bağlayıcı oluşturun ardından bu Bağlayıcısı'nı kullanarak bir işlevini çağırın."
services: functions
keywords: "Bulut uygulamaları, bulut Hizmetleri, PowerApps, iş süreçlerini iş uygulaması"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 1e262fde37b68bcfcee3c974deb91bd07965de19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-function-from-powerapps"></a>PowerApps’ten bir işlev çağırma
[PowerApps](https://powerapps.microsoft.com) platform geleneksel uygulama kodu olmadan uygulamaları oluşturmak iş uzmanları için tasarlanmıştır. Profesyonel geliştiricilere PowerApps uygulama oluşturucular teknik ayrıntıları koruma sırasında PowerApps, Windows'un yeteneklerini artırmak için Azure işlevleri kullanabilirsiniz.

Bu konuda bakım senaryo Rüzgar turbines için temel bir uygulama oluşturun. Bu konu içinde tanımlı bir işlevi çağırmak gösterilmiştir [işlevi için bir OpenAPI tanımı oluşturma](functions-openapi-definition.md). İşlev acil bir onarım Rüzgar Türbin üzerinde uygun maliyetli olup olmadığını belirler.

![PowerApps tamamlanan uygulama](media/functions-powerapps-scenario/finished-app.png)

Microsoft Flow aynı işlevi çağırma hakkında daha fazla bilgi için bkz: [Microsoft Flow bir işlevi çağırmak](functions-flow-scenario.md).

Bu konuda, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Örnek verileri Excel'de hazırlayın.
> * API tanımı verin.
> * API için bağlantı ekleyin.
> * Bir uygulama oluşturun ve veri kaynakları ekleyin.
> * Uygulamasında verileri görüntülemek için denetimleri ekleyin.
> * İşlevini çağırın ve verileri görüntülemek için denetimleri ekleyin.
> * Onarım uygun maliyetli olup olmadığını belirlemek için uygulamayı çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

+ Etkin bir [PowerApps hesap](https://powerapps.microsoft.com/tutorials/signup-for-powerapps.md) kimlik bilgileri Azure hesabınız olarak işaretli. 
+ Excel, uygulamanız için bir veri kaynağı olarak Excel kullanacağından.
+ Öğreticiyi tamamlamak [işlevi için bir OpenAPI tanımı oluşturma](functions-openapi-definition.md).


## <a name="prepare-sample-data-in-excel"></a>Excel'de örnek verileri hazırlama
Uygulama kullanma örnek verileri hazırlama tarafından başlayın. Aşağıdaki tabloda Excel'e kopyalayın. 

| Başlık      | Enlem  | Longtitude  | LastServiceDate | MaxOutput | ServiceRequired | EstimatedEffort | InspectionNotes                            |
|------------|-----------|-------------|-----------------|-----------|-----------------|-----------------|--------------------------------------------|
| Türbin 1  | 47.438401 | -121.383767 | 2/23/2017       | 2850      | Evet             | 6               | Bu ayın ikinci sorun budur.       |
| Türbin 4  | 47.433385 | -121.383767 | 5/8/2017        | 5400      | Evet             | 6               |                                            |
| Türbin 33 | 47.428229 | -121.404641 | 6/20/2017       | 2800      |                 |                 |                                            |
| Türbin 34 | 47.463637 | -121.358824 | 2/19/2017       | 2800      | Evet             | 7               |                                            |
| Türbin 46 | 47.471993 | -121.298949 | 3/2/2017        | 1200      |                 |                 |                                            |
| Türbin 47 | 47.484059 | -121.311171 | 8/2/2016        | 3350      |                 |                 |                                            |
| Türbin 55 | 47.438403 | -121.383767 | 10/2/2016       | 2400      | Evet             | 40               | Bunun için gelen bazı bölümleri sahibiz. |

1. Excel'de veri seçin ve **giriş** sekmesini tıklatın, **tablo biçiminde**.

    ![Tablo olarak biçimlendirmek](media/functions-powerapps-scenario/format-table.png)

1. Herhangi bir stil seçin ve tıklayın **Tamam**.

1. Seçiliyken tabloyla **tasarım** sekmesinde, girin `Turbines` için **tablo adı**.

    ![Tablo adı](media/functions-powerapps-scenario/table-name.png)

1. Excel çalışma kitabını kaydedin.

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>API için bir bağlantı Ekle
Özel API (özel bir bağlayıcı olarak da bilinir) PowerApps kullanılabilir, ancak bir uygulamada kullanabilmek için bir API için bağlantısı gerekir.

1. İçinde [web.powerapps.com](https://web.powerapps.com), tıklatın **bağlantıları**.

    ![PowerApps bağlantıları](media/functions-powerapps-scenario/powerapps-connections.png)

1. ' I tıklatın **yeni bağlantı**, aşağı kaydırarak **Türbin onarım** bağlayıcısını ve tıklatın.

    ![Yeni bağlantı](media/functions-powerapps-scenario/new-connection.png)

1. API anahtarını girin ve **oluşturma**.

    ![Bağlantı oluşturma](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Uygulamanızı başkalarıyla paylaşıyorsanız, çalışır veya uygulamanın kullandığı her kişi API'sine bağlanmak için API anahtarı da girmeniz gerekir. Bu davranış gelecekte değişebilir ve bu konuda, yansıtacak şekilde güncelleştireceğiz.

## <a name="create-an-app-and-add-data-sources"></a>Bir uygulama oluşturun ve veri kaynakları ekleyin
Artık PowerApps uygulaması oluşturma ve uygulama için veri kaynağı olarak Excel verilerini ve özel API eklemek hazırsınız.

1. İçinde [web.powerapps.com](https://web.powerapps.com), sol bölmede **yeni uygulama**.

1. Altında **boş uygulama**, tıklatın **telefon düzeni**.

    ![Tablet uygulaması oluşturma](media/functions-powerapps-scenario/create-phone-app.png)

    Uygulama için web PowerApps Studio'da açılır. Aşağıdaki resimde, PowerApps Studio farklı bölümlerini gösterir. Bu görüntü tamamlanan uygulama için durumundadır; Orta bölmede ilk boş bir ekran görürsünüz.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(1) sol gezinti çubuğunda**, hiyerarşik bir görünümü her ekranda tüm denetimlerin bakın

    **(2) orta bölmesinde**, üzerinde çalıştığınız ekran gösterir

    **(3) sağ bölmede**, burada düzeni ve veri kaynakları gibi seçeneklerini ayarlama

    **(4) özelliği** formüller uygulamak özellikleri seçin, açılan liste

    **(5) formül çubuğu**, uygulamanızın davranışını tanımlayan formüller (olduğu gibi Excel) eklediğiniz
    
    **(6) Şerit**, burada, denetimleri ekleme ve tasarım öğeleri özelleştirme

1. Excel dosyası veri kaynağı olarak ekleyin.

    1. Sağ bölmede üzerinde **veri** sekmesini tıklatın, **veri kaynağı Ekle**.

        ![Veri kaynağı ekleme](media/functions-powerapps-scenario/add-data-source.png)

    1. Tıklatın **uygulamanıza statik veri ekleme**.

        ![Veri kaynağı ekleme](media/functions-powerapps-scenario/add-static-data.png)

        Normalde okuma ve bir dış kaynaktan veri yazma, ancak bu bir örnek olduğundan Excel verilerini statik verileri olarak ekliyoruz.

    1. Kaydettiğiniz Excel dosyasına gidin, seçin **Turbines** Tablo öğesini tıklatıp **Bağlan**.

        ![Veri kaynağı ekleme](media/functions-powerapps-scenario/choose-table.png)

1. Özel API bir veri kaynağı olarak ekleyin.

    1. Üzerinde **veri** sekmesini tıklatın, **veri kaynağı Ekle**.

    1. Tıklatın **Türbin onarım**.

        ![Türbin onarım Bağlayıcısı](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Uygulamasında verileri görüntülemek için denetimler ekleme
Veri kaynakları uygulamada kullanılabilir, Türbin veri görüntüleyebilmeniz için uygulamanızın bir ekran ekleyin.

1. Üzerinde **giriş** sekmesini tıklatın, **yeni ekran** > **listesi ekran**.

    ![Liste ekranı](media/functions-powerapps-scenario/list-screen.png)

    PowerApps ekler içeren bir ekran bir *galeri* öğeleri ve arama, sıralama ve filtreleme etkinleştirmek diğer denetimleri görüntülemek için.

1. Başlık çubuğunu değiştirme `Turbine Repair`ve bu yüzden daha fazla denetim altındaki yer galeri yeniden boyutlandırın.

    ![Başlık değiştirmek ve galeri yeniden boyutlandırma](media/functions-powerapps-scenario/gallery-title.png)

1. Sağ bölmede seçiliyken Galerisi ile **veri** sekmesi, veri kaynağından değiştirme **CustomGallerySample** için **Turbines**.

    ![Veri Kaynağı Değiştir](media/functions-powerapps-scenario/change-data-source.png)

    Veri kümesi bir görüntü nedenle sonraki verileri daha iyi uyacak şekilde düzenini değiştirme içermiyor. 

1. Sağ bölmede hala değiştirme **düzeni** için **başlık, alt başlık ve gövde**.

    ![Galeri Düzeni Değiştir](media/functions-powerapps-scenario/change-layout.png)

1. Sağ bölmede son adım olarak, galeride görüntülenen alanları değiştirin.

    ![Galeri alanları değiştirin](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** LastServiceDate =
    + **Subtitle2** ServiceRequired =
    + **Başlık2** Title = 

1. Seçili Galerisi ile ayarlamak **TemplateFill** aşağıdaki formülü özelliğine: `If(ThisItem.IsSelected, Orange, White)`.

    ![Şablon dolgu formülü](media/functions-powerapps-scenario/formula-fill.png)

    Artık hangi galeri öğesi seçili görmek daha kolay olur.

    ![Seçilen öğe](media/functions-powerapps-scenario/selected-item.png)

1. Uygulama özgün ekranında gerek yoktur. Sol bölmede üzerine gelerek **ekran 1**, tıklatın **...** , ve **silmek**.

    ![Ekran Sil](media/functions-powerapps-scenario/delete-screen.png)

Diğer bir üretim uygulamasında genellikle yaptığınız biçimlendirme çok yoktur, ancak önemli bir bölümü açın işlevi çağırma bu senaryo için - taşıyacaksınız.

## <a name="add-controls-to-call-the-function-and-display-data"></a>İşlevini çağırın ve verileri görüntülemek için denetimler ekleme
Oluşturduğunuz işlevini çağırın ve döndürülen verileri görüntüleyen her Türbin özet verilerini görüntüler, böylece artık ekleme zamanı geldi denetimleri bir uygulama vardır. OpenAPI tanımı'nda ad biçimini temel işlevi erişim; Bu durumda olan `TurbineRepair.CalculateCosts()`.

1. Şerit üzerindeki **Ekle** sekmesini tıklatın, **düğmesini**. Aynı sekmede, ardından **etiketi**

    ![Düğme ve etiket Ekle](media/functions-powerapps-scenario/insert-controls.png)

1. Düğme ve etiket galeri sürükleyin ve etiket yeniden boyutlandırın. 

1. Düğme metni seçin ve ona değiştirin `Calculate costs`. Uygulama aşağıdaki görüntü gibi görünmelidir.

    ![Uygulama düğmesi](media/functions-powerapps-scenario/move-button-label.png)

1. Düğmesini seçin ve düğmenin için aşağıdaki formülü girin **OnSelect** özelliği.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Düğme tıklatıldığında ve seçili galeri öğesi varsa şunları yapar bu formülü yürütür bir **ServiceRequired** değerini `Yes`:

    + Temizler *koleksiyonu* `DetermineRepair` önceki çağrılarından verileri kaldırmak için. Bir tablo değişkeni koleksiyonudur.

    + Koleksiyona işlevini çağırarak döndürülen verileri atar `TurbineRepair.CalculateCosts()`. 
    
        Geçirilen değerleri işlevi gelen **EstimatedEffort** ve **MaxOutput** galeride seçili öğe için alanları. Galeride bu alanlar görüntülenmese ancak formüller hala kullanılabilir.

1. Etiketi seçin ve etiketin için aşağıdaki formülü girin **metin** özelliği.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Bu formülü kullanır `First()` ilk (ve yalnızca) satırının erişmek için işlevi `DetermineRepair` koleksiyonu. Ardından işlevi döndürür üç değerleri görüntüler: `message`, `costToFix`, ve `revenueOpportunity`. Uygulamayı ilk kez çalıştırılmadan önce bu değerleri boş.

    Tamamlanan uygulama aşağıdaki görüntü gibi görünmelidir.

    ![Tamamlanan uygulama Çalıştır önce](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>Uygulamayı çalıştırma
Tam bir uygulamanız! Şimdi çalıştırmak ve eylemde işlevi çağırır görmek için zaman yapılır.

1. PowerApps Studio sağ üst köşesinde çalışma düğmesini tıklatın: ![Çalıştır uygulama düğmesi](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Türbin değerini seçin `Yes` için **ServiceRequired**, ardından **maliyetlerini hesaplamak** düğmesi. Aşağıdaki görüntü gibi bir sonuç görmeniz gerekir.

    ![PowerApps tamamlanan uygulama](media/functions-powerapps-scenario/finished-app.png)

1. Ne her zaman işlevi tarafından döndürülen görmek için diğer turbines deneyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu konuda, öğrenilen nasıl yapılır:

> [!div class="checklist"]
> * Örnek verileri Excel'de hazırlayın.
> * API tanımı verin.
> * API için bağlantı ekleyin.
> * Bir uygulama oluşturun ve veri kaynakları ekleyin.
> * Uygulamasında verileri görüntülemek için denetimleri ekleyin.
> * İşlevini çağırın ve verileri görüntülemek için denetimler ekleme
> * Onarım uygun maliyetli olup olmadığını belirlemek için uygulamayı çalıştırın.

PowerApps hakkında daha fazla bilgi için bkz: [PowerApps giriş](https://powerapps.microsoft.com/tutorials/getting-started/).

Azure işlevleri kullanan diğer ilginç senaryoları hakkında bilgi edinmek için [Microsoft Flow bir işlevi çağırmak](functions-flow-scenario.md) ve [Azure Logic Apps ile tümleşen bir işlev oluşturun](functions-twitter-email.md).