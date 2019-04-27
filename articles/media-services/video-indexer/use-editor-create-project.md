---
title: Video Indexer Düzenleyicisi'ni projeleri oluşturmak için kullanın
titlesuffix: Azure Media Services
description: Bu konu, Video Indexer Düzenleyicisi'ni projeleri oluşturmak için nasıl kullanılacağını gösterir.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 04/02/2019
ms.author: juliako
ms.openlocfilehash: a9d6396cab560a201b98497e787af4b6c7c2dabb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60560929"
---
# <a name="use-the-video-indexer-editor-to-create-projects"></a>Video Indexer Düzenleyicisi'ni projeleri oluşturmak için kullanın

Video Indexer Web sitesi için ayrıntılı Öngörüler videolarınızın kullanmanıza olanak sağlar: doğru medya içeriği bulmak, ilgileniyorsanız ve sonuçları tamamen yeni bir proje oluşturmak için kullanmak bölümleri bulun. Oluşturulduktan sonra projenin kullanılabilir işlenen ve sağlanan Video Indexer'ı indirildi ve kendi düzenleme uygulamaları veya aşağı akış iş akışı kullanılacak.

Burada bu özellik yararlı bulabilir bazı senaryolar şunlardır: 

* Film oluşturma için tanıtımları vurgular.
* Eski klipleri video haber yayınları kullanıyor.
* Sosyal medya için daha kısa içeriği oluşturuluyor.

Bu makalede, sıfırdan bir proje oluşturma ve ayrıca bir video hesabınızdaki bir proje oluşturmak nasıl gösterir.

## <a name="create-new-project-and-manage-videos"></a>Yeni proje oluşturma ve videoları Yönet

1. [Video Indexer](https://www.videoindexer.ai/) web sitesine gidip oturum açın.
1. Seçin **projeleri** sekmesi. Projeleri önce oluşturduysanız, tüm diğer projelerinizi buraya görürsünüz.
1. Tıklayın **yeni proje oluştur**.  

    ![Yeni proje](./media/video-indexer-view-edit/new-project.png)
1. Kalem simgesine tıklayarak, projenize bir ad verin. "Adsız proje" ifadesini içeren metin, proje adıyla değiştirin ve üzerinde onay işaretine tıklayın.

    ![Yeni proje](./media/video-indexer-view-edit/new-project3.png)
    
### <a name="add-videos-to-the-project"></a>Projeye video ekleme

> [!NOTE]
> Şu anda projeleri yalnızca aynı dilde dizine videoları içerebilir. Tek bir dilde video seçtiğinizde, farklı bir dilde olan hesabınızda videoları ekleyemezsiniz.

1. Birlikte seçerek bu projede çalışmak istediğiniz video ekleme **video ekleme**.

    Hesabınızı ve "Metin, anahtar sözcükler veya görsel içerik arayın" ifadesini içeren bir arama kutusu, tüm videoları görürsünüz. Belirtilen kişinin, etiket, marka, anahtar sözcüğü veya transkriptine ve OCR'ye, oluşumunu video aramak için kullanılır.
    
    Örneğin, aşağıdaki görüntüde, "GitHub" bahsetmek videolar için arıyoruz.
    
    ![GitHub](./media/video-indexer-view-edit/github.png)

    Daha fazla seçerek sonuçlarınızı filtreleyebilirsiniz **sonuçlarını filtreleme**. Belirli bir kişiye içeren videolar göstermek veya yalnızca, video sonuçları görmek istediğinizi belirtmek için filtre bir belirli bir dilde veya belirli bir sahip. <br/> Sorgunuzun kapsamını da belirtebilirsiniz. Örneğin, "GitHub" OCR içinde arama yapmak istiyorsanız, seçin **görsel metin**.

    ![Filtre](./media/video-indexer-view-edit/visual-text.png)

    Sorgunuz için birden çok filtre katmanlayabilirsiniz. Kullanım **+** / **-** filtreleri Ekle/Kaldır düğmeleri. Kullanım **Filtreleri Temizle** tüm filtreler kaldırılacak.
1. Video eklemek, bunları seçin ve ardından **Ekle**.
1. Şimdi tüm seçtiğiniz videoları görürsünüz. Bu, projeniz için klipleri seçin olacak videolardır.

    Sürükleyip bırakarak veya liste menüsü düğmesini seçerek ve seçerek videoları sıralamayı yeniden düzenleyebilirsiniz **Aşağı Taşı** veya **Yukarı Taşı**. Liste menüsünden, aynı zamanda videonun bu projeden kaldırmak mümkün olmayacak. 

    ![Yeniden düzenleme](./media/video-indexer-view-edit/rearrange.png)
    
    Daha fazla video seçerek bu projeye herhangi bir zamanda ekleme seçeneğine sahip **video ekleme**. Bu gibi durumlarda, aynı video birden çok defa geçmelerine ayrıca projenize ekleyebilirsiniz. Bir video ve başka bir küçük bir küçük resim ve ilk video başka bir küçük resim göstermek istiyorsanız bunu yapmak isteyebilirsiniz. 

### <a name="select-clips-to-use-in-your-project"></a>Projenizde kullanılacak klipleri seçin

Her video sağ tarafındaki aşağı oka tıklayarak videonun (video klipleri) zaman damgalarına göre ınsights'ta açılır. 

1. Seçin **öngörüleri görüntüle** hangi ınsights özelleştirmek için bkz: ve, görmek istemediğiniz için kullanmanız gerekir. 

    ![İçgörüleri görüntüle](./media/video-indexer-view-edit/insights.png)
1. Belirli küçük resimleri için sorguları oluşturmak için "döküm, görsel metin, kişiler ve etiketleri arama" ifadesini içeren arama kutusunu kullanın.
1. Daha fazla aradığınız seçerek hangi Sahne üzerinde ayrıntılarını belirtmek için filtreleri ekleyin **filtre seçenekleri**.

    ![Filtre seçenekleri](./media/video-indexer-view-edit/filter-options.png)

    Örneğin, küçük resimleri ekranda olsa da Donovan Brown GitHub burada bahsedilen görmek isteyebilirsiniz. Bunun için "Kişiler" sahip "ekleme" bir filtre eklemek öngörü türü olarak gerekir. Ardından filtre için arama kutusuna "Donovan Brown" olarak yazmanız gerekir.
    
    ![Dahil Et](./media/video-indexer-view-edit/include.png)
    
    Burada GitHub belirtilen Donovan Brown olsa da klipleri isteyip istemediğinizi _değil_ ekranda yalnızca açılır menüyü kullanarak bir "dışlama" filtresine "içerir" filtresi değiştirirsiniz. 

1. Küçük resim eklemek istediğiniz segment seçerek projenize ekleyin. Parçadaki tekrar tıklayarak bu küçük işaretini kaldırabilirsiniz.
    
    Yanında video ve seçim listesi seçeneğine tıklayarak videonun tüm parçaları ekleyin **tüm segmentleri seçmek**. 

    ![Tümünü ekle](./media/video-indexer-view-edit/add-all.png)

    Tüm seçiminizi Seçimi Temizle seçerek temizleyebilirsiniz.

> [!TIP]
> Seçme ve resimlerinizi sıralama gibi sayfanın sağ tarafındaki player videoda önizleyebilirsiniz. 

![Önizleme](./media/video-indexer-view-edit/preview.png)

Projenizi seçerek değişiklikler yaptığınızda kaydetmeyi unutmayın **Kaydet proje**. 

### <a name="render-and-download-the-project"></a>Projeyi indirmek ve işleme

> [!NOTE]
> Hesapları Ücretli video Indexer için projenizi oluşturma kodlama sahiptir. Video Indexer deneme hesapları 5 saatlik işleme sınırlıdır.

1. İşiniz bittiğinde, projenizin kaydedildiğinden emin olun. Bu proje artık işleyebilirsiniz. Seçin **oluşturma ve indirme**. 

    ![Kaydet](./media/video-indexer-view-edit/save.png)

    Video ındexer'a bir dosya işlenir bildiren bir açılır pencere olur ve ardından indirme bağlantısı, e-posta Gönder olacaktır. Select devam edin. 
    
    Ayrıca, proje işlenen sayfanın en üstünde bir bildirim görürsünüz. İşlem tamamlandıktan sonra işlenen, projenin başarıyla işlenip yeni bir bildirim görürsünüz. Projeyi indirmek için bildirime tıklayın. Bu projenin mp4 biçimindeki indirir.

    ![İşleme tamamlandı](./media/video-indexer-view-edit/rendering-done.png)

1. Kaydedilen projelerden erişebileceğiniz **projeleri** sekmesi. 

    Bu proje öğesini seçerseniz, tüm öngörülerde ve zaman çizelgesi bu projenin bakın. Seçerseniz **Video Düzenleyicisi**, bu proje için düzenlemeleri devam edebilirsiniz. Düzenleme, ekleme veya video klipleri kaldırarak ya da projesini yeniden adlandırma içerir.

    ![Video düzenleyicisi](./media/video-indexer-view-edit/video-editor.png)
     
## <a name="create-a-project-from-your-video"></a>Videonuzdan proje oluşturma

Hesabınızda doğrudan bir video yeni bir proje oluşturabilirsiniz. 

1. Git **Kitaplığı** Video Indexer Web sitesinin sekmesi.
1. Projenizi oluşturmak için kullanmak istediğiniz videoya açın. Öngörüler ve zaman çizelgesi sayfasında seçin **Video Düzenleyicisi** düğmesi.

    Bu, yeni bir proje oluşturmak için kullandığınız aynı sayfasına götürür. Yeni Proje, daha önce düzenlemeye başladığını video zaman damgalı ınsights bölümleri bakın.

## <a name="see-also"></a>Ayrıca bkz.

[Video Indexer’a genel bakış](video-indexer-overview.md)

