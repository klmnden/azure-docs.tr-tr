---
title: Gelişmiş Media Encoder Premium iş akışı öğreticileri
description: Bu belge, Media Encoder Premium iş akışı ile gelişmiş görevler gerçekleştirme ve ayrıca iş akışı Tasarımcısı ile karmaşık iş akışları oluşturma işlemini gösteren izlenecek yolları içerir.
services: media-services
documentationcenter: ''
author: xstof
manager: femila
editor: ''
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: d227e3618c138e6661cc4be7caa2b9a3ba1af3f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61242047"
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Gelişmiş Media Encoder Premium iş akışı öğreticileri
## <a name="overview"></a>Genel Bakış
Bu belge ile iş akışlarını özelleştirme işlemini gösteren izlenecek yollar içeriyor **iş akışı Tasarımcısı**. Gerçek iş akışı dosyalarını bulabilirsiniz [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>İÇİNDEKİLER
Aşağıdaki konular ele alınmaktadır:

* [Tek bit hızlı MP4 MXF kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Yeni bir iş akışı başlatma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Medya dosyası girişini kullanarak](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Medya akışlarının inceleniyor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Bir video Kodlayıcısı için ekleniyor. MP4 dosyası oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Ses akışı kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Ses ve Video akışları bir MP4 kapsayıcıya çoğullama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [MP4 dosyası yazılıyor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Çıkış dosyasından bir medya Hizmetleri varlık oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Tamamlanan iş akışı yerel olarak test etme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [MP4 - multibitrate MXF kodlama etkin dinamik paketleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Bir veya daha fazla ek MP4 çıkışları ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Çıkış dosya adları yapılandırma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Ayrı bir ses izi ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * ["ISM" SMIL dosyası ekleniyor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [MP4 - Gelişmiş şema multibitrate MXF kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * Geliştirmek için iş akışı genel bakış
  * [Dosya adlandırma kuralları](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [İş akışı kök üzerine yayımlama bileşeni özellikleri](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Çıktı dosyası adları yayımlanan özellik değerlerine dayanan oluşturulmasını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Küçük resimleri için multibitrate MP4 çıktısına ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * Küçük resimleri eklemek için iş akışı genel bakış
  * [JPG kodlama ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Renk alanı dönüştürme uğraşmanızı](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Küçük yazma](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Bir iş akışında hataları algılama](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Tamamlanan iş akışı](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Zamana bağlı kesme multibitrate MP4 çıkış](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Kırpma için eklemeye başlamak için iş akışı genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Stream Ayarlayıcısı kullanma](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Tamamlanan iş akışı](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Betikleştirilmiş bileşeni ile tanışın](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Bir iş akışı içinde betik: Merhaba Dünya](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Çerçeve tabanlı kesme multibitrate MP4 çıkış](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Kırpma için eklemeye başlamak için şema genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Küçük resim listesi XML kullanma](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Bir komut dosyası bileşeninden küçük listesini değiştirme](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [ClippingEnabled kolaylık özellik ekleme](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>Tek bit hızlı MP4 MXF kodlama
Bu bölüm, tek bit hızlı oluşturma işlemini gösterir. AAC-HE dosyasıyla MP4 kodlanmış gelen bir. MXF giriş dosyası.

### <a id="MXF_to_MP4_start_new"></a>Yeni bir iş akışı başlatma
İş Akışı Tasarımcısı'nı açın ve dosyayı seçin > Yeni çalışma alanı > dönüştürme şema

Yeni iş akışı üç öğeleri gösterir:

* Birincil kaynak dosyası
* Küçük resim listesi XML'i
* Çıkış dosyası/varlık  

![Yeni kodlama iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Yeni kodlama iş akışı*

### <a id="MXF_to_MP4_with_file_input"></a>Medya dosyası girişini kullanarak
Giriş medya dosyasını kabul etmek için bir medya dosyası giriş bileşeni ekleme ile başlar. İş akışına bir bileşen eklemek için depo arama kutusuna aramanız ve istenen girişi Tasarımcı bölmesine sürükleyin. Eylem medya dosyasını giriş için yineleyin ve birincil kaynak dosyası bileşen için dosya adı giriş PIN medya dosyası giriş'e bağlanın.

![Bağlı ortam giriş dosyası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Bağlı ortam giriş dosyası*

Başlangıçta, özel bir iş akışını tasarlarken kullanmak için bir uygun örnek dosyasını belirleyin. Bunu yapmak için tasarımcı bölmesi arka plan tıklayın ve sağ özellik bölmesi birincil kaynak dosyası özelliği arayın. Klasör simgesine tıklayın ve iş akışı test etmek istediğiniz dosyayı seçin. Medya dosyası girişini bileşeni, dosyayı inceler ve bu inceledi örnek dosyası ayrıntılarını yansıtmak için çıkış sabitleyicileri doldurur.

![Doldurulmuş ortam giriş dosyası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Doldurulmuş ortam giriş dosyası*

Giriş doldurduktan kodlama ayarları çıkışını ayarlamak için sonraki adım olan. Benzer şekilde nasıl birincil kaynak dosyası yapılandırıldı, hemen altındaki çıkış klasörü değişken özelliği şimdi yapılandırın.

![Yapılandırılmış giriş ve çıkış özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Yapılandırılmış giriş ve çıkış özellikleri*

### <a id="MXF_to_MP4_streams"></a>Medya akışlarının inceleniyor
Genellikle akışı akan nasıl akış gibi göründüğünü bilmeniz istenen. İş akışındaki herhangi bir noktada bir akışı incelemek için bir çıkış veya giriş PIN'i bileşenleri tıklamanız yeterlidir. Bu durumda, medya dosyası girdisinden sıkıştırılmamış Video Çıkış PIN tıklayarak deneyin. Giden videoyu inceleyin izin veren açılır iletişim kutusu.

![Sıkıştırılmamış Video Çıkış PIN inceleniyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Sıkıştırılmamış Video Çıkış PIN inceleniyor*

Bu durumda, videoyu 24 çerçeveler saniyede 4'te, bir 1920 x 1080 girişi içerdiğini gösterir: 2:2 örnekleme neredeyse 2 dakikalık video için.

### <a id="MXF_to_MP4_file_generation"></a>Bir video Kodlayıcısı için ekleniyor. MP4 dosyası oluşturma
Şimdi sıkıştırılmamış bir Video ve PIN'leri kullanılabilir birden çok sıkıştırılmamış ses çıkış üzerinde medya dosyası girişini kullanın. Gelen video kodlamak için bir kodlama bileşen iş akışına -, bu durumda oluşturmak için eklenmesi gerekir. Mp4 dosyaları.

H.264 video akışına kodlayın, AVC Video Kodlayıcı bileşeni Tasarımcı yüzeyine ekleyin. Bu bileşen Sıkıştırılmışı Aç video akışı girdi olarak alır ve bir AVC sıkıştırılmış video akışı çıkış PIN'ini sunar.

![Bağlantısız AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Bağlantısız AVC kodlayıcı*

Özellikleri tam olarak kodlama nasıl olacağını belirler. Şimdi bazı önemli ayarları vardır:

* Çıkış genişlik ve yükseklik çıkış: video kodlanmış çözünürlüğünü belirler. Bu durumda, 640 x 360 iyi bir ayardır.
* Geçiş için kare oranı: ayarlandığında, kaynak kare hızı yalnızca benimseyeceği, ancak bunu geçersiz kılmak mümkündür. Böyle bir kare hızını dönüştürme hareket-tazmin değil.
* Profil ve düzeyi: AVC profili ve düzeyini belirler. Rahatça profilleri ve farklı düzeyleri hakkında daha fazla bilgi almak için AVC Video Kodlayıcı bileşeni soru işareti simgesine tıklayın ve yardım sayfasına düzeylerinin her biri hakkında daha fazla ayrıntı gösterilir. Bu örnekte, 3.2 (varsayılan) düzeyinde ana profilini kullanın.
* Denetim modu, bit hızı (kbps) oranı: Bu senaryoda 1200 KB/sn çıkış sabit hızı (CBR) için iyileştirilmiş
* Görüntü biçimi: (görüntü geliştirmek için bir kod çözücü tarafından kullanılan ancak doğru bir şekilde çözmek için gerekli olabilecek tarafı bilgileri) H.264 akışa yazılan VUI (Video kullanılabilirlik bilgileri) hakkında bilgi sağlar:
* NTSC (ABD veya Japonya, 30 fps kullanma için tipik)
* PAL (25 fps kullanarak Avrupa için tipik)
* GOP boyutu modu: Sabit GOP boyutu amaçlarımız kapalı GOPs ile 2 saniyelik anahtar aralığına ayarlayın. 2 saniye ayarını sağlayan dinamik paketleme Azure Media Services ile uyumluluk sağlar.

AVC kodlayıcıdan akışı için medya dosyası giriş bileşeninden sıkıştırılmamış Video Çıkış PIN AVC kodlayıcıdan sıkıştırılmamış Video giriş PIN bağlayın.

![Bağlı AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Bağlı AVC ana kodlayıcı*

### <a id="MXF_to_MP4_audio"></a>Ses akışı kodlama
Bu noktada, özgün sıkıştırılmamış ses akışı hala sıkıştırılmış olması gerekir. Ses akışı Sıkıştırma akışına bir AAC Kodlayıcı (Dolby) bileşeni ekleyin.

![Bağlantısız AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Bağlantısız AAC kodlayıcı*

Artık bir uyumsuzluk: büyük olasılıkla bu medya dosyası giriş iki farklı sıkıştırılmamış ses akışları kullanılabilir olacaktır ancak yalnızca bir tek sıkıştırılmamış ses giriş PIN AAC kodlayıcıdan olduğu: bir ses kanal sol ve sağ için. (Saran ses ile işlem yapılırken, altı kanalları değil.) Bu nedenle ses medya dosyası girişini kaynaktan AAC ses Kodlayıcı doğrudan bağlanmak mümkün değildir. AAC bileşen sözde "araya eklemeli" ses akışı bekliyor: sol hem birbiriyle aralıklı doğru kanallara sahip tek bir akış. Bir kez bizim kaynak medya dosyasından ses parçaları kaynak hangi konumda, biz sola ve sağa doğru atanan Konuşmacı konumlar ile araya eklemeli gibi ses akışı oluşturabilirsiniz olduğunu biliyoruz.

İlk olarak, bir gerekli kaynak ses kanaldan bir araya eklemeli akışı oluşturmak istiyor. Ses Stream ayırıcı bileşeni bizim için işler. İş akışını Ekle ve ses çıkış medya dosyası girişini içine bağlayın.

![Ses Stream ayırıcı bağlı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Ses Stream ayırıcı bağlı*

Araya eklemeli bir ses akışı sahibiz, biz yine de sola veya sağa Konuşmacı konumlarına atamak istediğiniz yeri belirtin alamadık. Bu belirtmek için Konuşmacı konumu atayan yararlanabilirsiniz.

![Konuşmacı konumu ataması ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Konuşmacı konumu ataması ekleme*

Konuşmacı konumu atayan kullanmak için stereo bir giriş akışı ile bir kodlayıcı önceden filtresinden "Özel" ve "2.0 (L, R)." adlı kanal önceden yapılandırın. (Bu sol Konuşmacı konumu 1 kanal ve doğru Konuşmacı konumu kanalına 2 atar.)

Konuşmacı konumu atayan çıktısını AAC Kodlayıcı girişine bağlayın. Ardından, bir "2.0 ile (L, R)" çalışmaya AAC Kodlayıcı söyleyin kanal stereo ses giriş olarak çalışılabilecek anlaması için hazır.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Ses ve Video akışları bir MP4 kapsayıcıya çoğullama
Her ikisini de kopyalayıp yakalayarak kullanabiliriz, bizim AVC video kodlanmış akışı ve bizim AAC ses akışı olarak kodlanmış bir. MP4 kapsayıcısı. Tek bir farklı akış karıştırma işleminin "çoğullama" (veya "muxing") adı verilir. Bu durumda, biz ses ve video akışları tutarlı tek bir Interleaving. MP4 paketi. Bu koordinatları bileşen bir. MP4 kapsayıcı ISO MPEG-4 çoğaltıcı çağrılır. Tasarımcı yüzeyine ekleyin ve hem AVC Video Kodlayıcısı ve AAC Kodlayıcı girişleri için bağlanın.

![Çoğaltıcı bağlı MPEG4](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Çoğaltıcı bağlı MPEG4*

### <a id="MXF_to_MP4_writing_mp4"></a>MP4 dosyası yazılıyor
Dosya çıktısı bileşeni, bir çıkış dosyası yazılırken kullanılır. Çıktısını yazılan, böylece Biz bu ISO MPEG-4 çoğaltıcı çıkışına bağlanabilir diske. Bunu yapmak için yazma giriş PIN dosya çıkış için kapsayıcı (MPEG-4) çıktı PIN bağlanın.

![Bağlı dosya çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Bağlı dosya çıktısı*

Kullanılan dosya adı dosya özelliği tarafından belirlenir. Bu özellik belirli bir değere sabit kodlanmış olabilir, ancak bunun yerine bir ifade ayarlamak büyük olasılıkla bir istiyor.

İş akışı çıktısı otomatik olarak belirlemek için dosya adı bir ifade özelliğinden, dosya adı (klasör simgesi) yanındaki yanındaki düğmeye tıklayın. "İfadesi" seçip aşağı açılan menüsünden Bu ifade Düzenleyicisi ' getirir. Düzenleyici içeriği önce temizleyin.

![Boş ifade Düzenleyicisi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Boş ifade Düzenleyicisi*

İfade Düzenleyicisi, bir veya daha fazla değişken ile karışık herhangi bir değişmez değer girmenizi sağlar. Değişkenleri, bir dolar işareti ile başlayın. $ Anahtar isabet gibi Düzenleyicisi kullanılabilir değişkenlerin bir seçenek içeren bir açılan kutusu gösterir. Çıkış dizini değişkeni ve temel bir giriş dosyası adı değişkeni örneğimizde kullanacağız:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![İfade düzenleyicisini dolu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*İfade düzenleyicisini dolu*

> [!NOTE]
> Kodlama işinin azure'da bir çıktı dosyasını görmek için bir değer ifade Düzenleyicisi'nde sağlamanız gerekir.
>
>

İfade ok tuşlarına basarak onayladığınızda, özellik penceresi, hangi dosya özelliği çözümler bu anda değerin önizlemeleri sağlanır.

![Çıkış dizini dosya ifade çözümleniyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Çıkış dizini dosya ifade çözümleniyor*

### <a id="MXF_to_MP4_asset_from_output"></a>Çıkış dosyasından bir medya Hizmetleri varlık oluşturma
Biz bir MP4 çıktı dosyası yazmış, ancak yine de bu dosyayı hangi medya Hizmetleri'nin ürettiği bu iş akışı yürütmenin sonucu olarak çıktı varlığına ait olduğunu belirtmek ihtiyacımız var. Bu, iş akışı tuvalde çıkış dosyası/varlık düğümü kullanılır. Bu düğüm tüm gelen dosyalarına elde edilen Azure Media Services varlık bir parçası olun.

Dosya çıktısı bileşen iş akışının tamamlanması çıkış dosyası/varlık bileşenine bağlayın.

![Tamamlanan iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Tamamlanan iş akışı*

### <a id="MXF_to_MP4_test"></a>Tamamlanan iş akışı yerel olarak test etme
İş akışı yerel olarak test etmek için üst araç çubuğunda yürütme düğmesine basın. İş akışı çalıştırma bittiğinde, yapılandırılmış çıktı klasöründe oluşturulan çıktıyı inceleyin. MXF giriş kaynak dosyasından kodlanan tamamlanmış MP4 çıkış dosyası görürsünüz.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Dinamik paketleme MXF MP4 - multibitrate kodlama etkin
Bu izlenecek yolda tek bir ses kodlanmış AAC ile Çoklu bit hızı MP4 dosyaları kümesini oluşturur. MXF giriş dosyası.

Ne zaman Çoklu bit hızına sahip bir varlık çıkış birlikte kullanım için birden çok GOP hizalı MP4 dosyaları her farklı bit hızı ve çözüm oluşturulması gerekecek olan Azure Media Services tarafından sunulan dinamik paketleme özelliklerle istenir. Bunu yapmak için [kodlama MXF tek bit hızlı MP4'içine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) izlenecek bize iyi bir başlangıç noktası sağlar.

![İş akışı başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*İş akışı başlatma*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Bir veya daha fazla ek MP4 çıkışları ekleme
Her sonuç bizim Azure Media Services varlık MP4 dosyasında farklı bit hızı ve çözüm destekler. Bir veya daha fazla MP4 çıkış dosyası iş akışına ekleyelim.

Aynı ayarlarla oluşturulmuş video kodlayıcılar tüm müşterilerimize sunuyoruz emin olmak için zaten mevcut AVC Video Kodlayıcısı yinelenen ve başka bir birleşimi çözünürlük ve bit hızı (960 x 540 birini 2,5 MB/sn 25 saniyedeki en ekleyelim yapılandırmak en uygun olduğu ). Var olan Kodlayıcı çoğaltmak için kopyalama yapıştırın, Tasarımcı yüzeyinde.

Medya dosyası girişini sıkıştırılmamış Video Çıkış PIN kodunu yeni bizim AVC bileşene dönüştürerek bağlanın.

![Bağlı ikinci AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Bağlı ikinci AVC kodlayıcı*

Şimdi yeni AVC kodlayıcımız 960 x 540 2,5 MB/sn hızında çıktı yapılandırmasını uyarlayın. (Kendi çıkış özellikleri "width", "Çıkış yükseklik" ve "Hızı (kbps)" için bunu kullanın.)

Verilen sonuç varlık Azure Media Services dinamik paketleme ile birlikte kullanmak istiyoruz, akış uç noktasını bu MP4 dosyalarını tam olarak diğer bir şekilde hizalı olup HLS/parçalanmış MP4/DASH parçaları oluşturma yeteneğine sahip olması gerekir, farklı bit hızlarına dönüştürme arasında geçiş istemciler bir tek kesintisiz sürekli video ve ses denetimi yaşayın. Gerçekleşen hale getirmek için biz özelliklerinde hem AVC kodlayıcılar GOP resim ("group"), her iki MP4 dosyaları için boyut yapılabilir 2 saniye ayarlandığını, emin olmanız gerekir:

* Sabit GOP boyutuna GOP boyutu modunu ayarlama ve
* Anahtar çerçeve aralığı iki saniye.
* Ayrıca tüm GOPs emin olmak için GOP kapalı GOP IDR denetime duran olmadan kendi bağımlılıkları

Bu iş akışını anlamak daha kolay hale getirmek için ilk AVC kodlayıcıya yeniden adlandır "AVC Video Kodlayıcısı 640 x 360 1200 kbps" ve ikinci AVC Kodlayıcı "AVC Video Kodlayıcısı 960 x 540 2500 KB/sn."

Şimdi ikinci bir ISO MPEG-4 çoğaltıcı ve ikinci bir dosya çıktısı ekleyin. Çoğaltıcı yeni AVC kodlayıcıya bağlanın ve çıktısını dosya çıkışa yönlendirilir emin olun. Sonra da AAC ses Kodlayıcı çıkışı çoğaltıcı ait yeni giriş bağlanır. Dosya çıktısı sırayla sonra Oluşturulacak Medya Hizmetleri varlık eklemek için çıkış dosyası/varlık düğümüne bağlanabilir.

![İkinci Karıştırıcı ve bağlı dosya çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*İkinci Karıştırıcı ve bağlı dosya çıktısı*

Azure Media Services dinamik paketleme ile uyumluluk için GOP sayısı veya süresi çoğaltıcı 's öbek modunu yapılandırın ve GOPs öbek başına 1 olarak ayarlayın. (Bu varsayılan olmalıdır.)

![Karıştırıcı öbek modları](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Karıştırıcı öbek modları*

Not: Varlık çıkışı eklemek istediğiniz diğer bit hızı ve çözüm birleşimleri için bu işlemi tekrarlayın isteyebilirsiniz.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Çıkış dosya adları yapılandırma
Birden fazla tek çıktı varlığına eklenen dosya sahibiz. Bu, her çıkış dosyalarının dosya adlarını birbirinden farklı olduğundan emin olun ve belki de ile ilgilenen dosya adından açık olacak şekilde bile bir dosya adlandırma kuralı uygulamak için bir gereksinim sağlar.

Çıkış dosya adlandırma Tasarımcı ifadelerinde aracılığıyla denetlenebilir. Dosya çıktısı bileşenlerden biri için özellik bölmesini açın ve dosya özelliği için ifade düzenleyicisini açın. Bizim ilk çıktı dosyası ifadesini yapılandırılan (gitmek için bkz [tek bit hızlı MP4 çıktısına için MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Bu bizim dosya adı iki değişkeni tarafından belirlenir anlamına gelir: yazmak için çıktı dizini ve kaynak dosyası taban adı. Eski iş akışı kök üzerindeki bir özellik olarak kullanıma sunulmuştur ve ikincisi gelen dosyası tarafından belirlenir. Çıkış dizini, yerel olarak test etmek için kullandığınız olur; Azure Media Services bulut tabanlı medya işleyicisi ile iş akışı yürütülürken bu özellik iş akışı altyapısı tarafından geçersiz kılınır.
Müşterilerimize hem Çıkış dosyalarını adlandırma tutarlı çıkış vermek için ilk dosya ifade adlandırma değiştirin:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

ve ikinci:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Her iki MP4 çıktı dosyaları düzgün şekilde oluşturulan emin olmak için bir ara testi yürütün.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Ayrı bir ses izi ekleme
Size sunduğumuz MP4 çıktı dosyaları ile Git bir .ism dosyası oluşturduğunuzda daha sonra anlatıldığı gibi biz de yalnızca ses bir MP4 dosyası ses kaydının bizim Uyarlamalı akış için gerektirir. Bu dosyayı oluşturmak için ek bir Karıştırıcı (ISO-MPEG-4 çoğaltıcı) iş akışını Ekle ve AAC Kodlayıcısı'nın çıkış PIN giriş PIN'ini İzle 1 için bağlanın.

![Eklenen ses karıştırıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Eklenen ses karıştırıcı*

Üçüncü Karıştırıcı giden akıştan çıktı ve dosya ifade olarak adlandırma yapılandırma dosyasını çıkış bileşen oluşturun:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Ses karıştırıcı çıkış dosyası oluşturma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Ses karıştırıcı çıkış dosyası oluşturma*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Ekleme. ISM SMIL dosyası
Dinamik paketleme hem MP4 dosyaları (ve yalnızca ses MP4) bizim Media Services ile birlikte çalışmak de bir bildirim dosyası ihtiyacımız (bir "SMIL" dosyası olarak da adlandırılır: Multimedya tümleştirme dil eşitlenen). Bu dosya, Azure Media Services dinamik paketleme ve bu ses akışı için dikkate alınması gereken hangi MP4 dosyaları kullanılabilir gösterir. Tipik bir bildirim dosyası için bir dizi MP4'ın tek bir ses akışı ile şöyle görünür:

```xml
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="https://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>
```

Tek MP4 video dosyaları ve bu da bir (veya daha fazla) ses dosyası başvuruları için yalnızca ses içeren bir MP4 yanı sıra her bir başvuru switch deyimi içinde .ism dosyası içerir.

Bizim dizi MP4'ın bildirim dosyasını oluşturma "AMS bildirimi yazıcı." adlı bir bileşen yapılabilir Bunu kullanmak için yüzeyine sürükleyin ve "Yazma tamamlandı" çıkış sabitleyicileri üç dosya çıktısı bileşenlerden AMS bildirim yazıcı girişi bağlanın. Sonra bildirim AMS yazan çıktısını çıkış dosyası/varlığına bağlanma emin olun.

Diğer dosya çıkış bileşenlerimiz gibi .ism dosyası çıktı adı bir ifade ile yapılandırın:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Bizim tamamlanmış iş akışının nasıl göründüğü aşağıda:

![Tamamlanmış MXF multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Tamamlanmış MXF multibitrate MP4 iş akışı*

## <a id="MXF_to__multibitrate_MP4"></a>MP4 - Gelişmiş şema multibitrate MXF kodlama
İçinde [önceki iş akışı izlenecek](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) nasıl tek bir MXF giriş varlığı bir çıkış varlığa Çoklu bit hızına sahip MP4 dosyalarının yalnızca ses bir MP4 dosyasını ve Azure medya ile kullanılmak üzere bir bildirim dosyası ile dönüştürülebilir gördük Dinamik paketleme Hizmetleri.

Bu izlenecek yol, nasıl bazı yönlerini geliştirilebilen ve daha uygun hale gösterir.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Geliştirmek için iş akışı genel bakış
![Geliştirmek için Multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Geliştirmek için Multibitrate MP4 iş akışı*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Dosya adlandırma kuralları
Önceki iş akışında, çıkış dosyası adı oluşturmak için temel olarak basit bir ifade belirtildi. Bazı çoğaltma yine de sahibiz: belirtilen tüm bireysel çıkış dosyası bileşenleri gibi ifade.

Örneğin, bizim ilk video dosyası için dosya çıkış bileşen bu ifadesiyle birlikte yapılandırılır:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

İkinci çıkışı için sırada video, gibi ifade sunuyoruz:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Biz bu çoğaltma bazılarını kaldırın ve şeyler bunun yerine daha yapılandırılabilir olun temizleyici daha az hata yapmaya açık ve daha kullanışlı olmaz mıydı? Neyse ki gerçekleştirebiliriz: özel özellikler, iş akışı kökünde oluşturma olanağı ile birlikte tasarımcının ifade özellikleri kolaylık, ek bir katmanı sağlar.

Biz filename yapılandırma tek bir MP4 dosyaları bit hızlarına dönüştürme sürücü varsayalım. Bu bit hızlarında biz burada yapılandırmak için erişmeleri ve sürücü dosyası adı oluşturma (kökündeki bizim grafik), tek bir merkezi yerde yapılandırmak için hedeflenir. AVC kodlayıcılarda itibaren iki kökünden de erişilebilir hale Bunu yapmak için şu iki AVC kodlayıcılar hızı özelliğinden iş, kök yayımlayarak başlatın. (İki farklı etmektedir görüntülenen olsa bile, yalnızca bir temel alınan değer yoktur.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>İş akışı kök üzerine yayımlama bileşeni özellikleri
İlk AVC Kodlayıcı açın, bit hızı (kbps) özelliğine gidin ve Yayımla açılan listeden seçin.

![Yayımlama hızı özelliği](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Yayımlama hızı özelliği*

Bizim iş akışı grafiği kök yayımlamak için Yayımla iletişim yayımlanan adını "video1bitrate" ve "Video 1 bit hızı" okunabilir görünen adını yapılandırın. Özel bir yapılandırma grup adı "Akış bit hızlarına dönüştürme" olarak adlandırılan ve yayımlama basın.

![Yayımlama hızı özelliği](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Bit hızı özelliği için yayımlama iletişim kutusu*

Aynı ikinci AVC Kodlayıcı hızı özelliği için yineleyin ve aynı özel grup "Akış bit hızlarına dönüştürme" bir görünen ad "Video 2 hızı" ile "video2bitrate" olarak adlandırın.

Biz, artık iş akışı kök özellikleri incelemek, görünmesini grubumuz özel iki yayımlanmış özelliklere sahip göreceğiz. Her ikisi de, ilgili AVC Kodlayıcı hızı değerini yansıtan.

![İş akışı kök yayımlanan hızı Özellikler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Size kodu veya bir ifade bu özelliklere erişmek istediğinizde, bunu gibi bunu yapabilirsiniz:

* Satır içi kod sağ kökünün altındaki bir bileşenin: node.getPropertyAsString('.. / video1bitrate', null)
* bir ifade içinde: ${ROOT_video1bitrate}

Şimdi de bizim ses hızı üzerindeki yayımlayarak "Akış bit hızlarına dönüştürme" grubu tamamlayın. AAC Kodlayıcı özelliklerini içinde hızı ayarı arayın ve yanındaki açılır listeden Yayımla'yı seçin. Grafik adı "audio1bitrate" ile kök yayımlama ve özel grubumuz "Akış bit hızlarına dönüştürme" içinde "1 hızına sahip ses" adını görüntüler.

![Ses hızı için yayımlama iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Ses hızı için yayımlama iletişim kutusu*

![Kök ortaya çıkan video ve Ses Özellikler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Kök ortaya çıkan video ve Ses Özellikler*

Ayrıca yeniden yapılandırır değerler ve bunların bağlı ile ilgili bileşenleri değerlerini değiştirir herhangi birine değiştirme (ve bir yayımlandığı yerlerde).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Çıktı dosyası adları yayımlanan özellik değerlerine dayanan oluşturulmasını
Runbook'a kod yerine bizim oluşturulan dosya adları şimdi biz grafik kök yayımlanan hızı özellikleri etmenin dosya çıktısı bileşenlerinin her bizim dosya adı ifade Değiştirebiliriz. Bizim ilk dosya çıktısı ile başlayarak, dosya özelliğini bulun ve şöyle ifadeyi düzenleyin:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Bu ifadede farklı parametreler, erişilebilir ve ifade penceresinde klavyedeki dolar işareti tuşlarına basarak girdiniz. Kullanılabilir parametrelerin biri daha önce yayımlanan bizim video1bitrate özelliğidir.

![Bir ifade içinde parametrelerine erişim](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Bir ifade içinde parametrelerine erişim*

İkinci videomuza dosya çıktısı için de aynısını yapın:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

ve yalnızca ses dosyası çıktısı için:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Artık herhangi bir video veya ses dosyaları için bit hızı değiştirirsek ilgili Kodlayıcı yapılandırılacak ve bit hızı tabanlı bir dosya adı kuralı tüm otomatik olarak kullanılacaktır.

## <a id="thumbnails_to__multibitrate_MP4"></a>Küçük resimleri için multibitrate MP4 çıktısına ekleme
Oluşturan bir iş akışından başlangıç [Giriş bir MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz artık çıktı küçük resim eklemek içine arayacaktır.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Küçük resimleri eklemek için iş akışı genel bakış
![Başlangıç Multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Başlangıç Multibitrate MP4 iş akışı*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG kodlama ekleme
Bizim küçük resim oluşturma yaklaşımının JPG Kodlayıcı bileşeni, çıkış JPG dosyaları mümkün olacaktır.

![JPG kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG Encoder*

Ancak doğrudan bizim sıkıştırılmamış Video akış medya dosyası girişini JPG Kodlayıcı bağlanamıyoruz. Bunun yerine, tek tek çerçeve verilmesini bekler. Bu, Video çerçeve kapı bileşeni yapabiliriz.

![JPG kodlayıcıya bir çerçeve kapısı bağlanma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*JPG kodlayıcıya bir çerçeve kapısı bağlanma*

Çerçeve andan itibaren çok fazla saniyede veya çerçeveleri geçirilecek video bir çerçeve sağlar. Aralığı ve saat, böyle ile özelliklerinde yapılandırılabilir.

![Video çerçeve ağ geçidi özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video çerçeve ağ geçidi özellikleri*

Bir küçük resim dakikada modu süresi (saniye) ve 60 aralığı ayarlayarak oluşturalım.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Renk alanı dönüştürme uğraşmanızı
Çerçeve ağ geçidi ve medya dosyası girişini hem sıkıştırılmamış Video iğne artık bağlı olarak mantıksal görünüyor, ancak biz bunu yapmayı tercih ediyorsanız şu uyarı elde edersiniz.

![Giriş rengi alan hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Giriş rengi alan hatası*

Hangi renkte bilgileri bizim özgün ham sıkıştırılmamış video akış bizim MXF gelen gösteriliş şeklini JPG Kodlayıcı bekleniyor öğesinden farklı olmasıdır. Özellikle, bir sözde "renk aralığı" "RGB" veya "Gri" içindeki akış bekleniyor. Bu Video çerçeve ağ geçidi'nin gelen video akışı, renk alanı ile ilgili ilk uygulanan bir dönüştürme sahip olması gerektiğini anlamına gelir.

İş akışı renk alanı dönüştürücü - Intel sürükleyin ve bizim çerçeve ağ geçidine bağlayın.

![Renk alanı Dönüştürücüsü bağlanma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Renk alanı Dönüştürücüsü bağlanma*

Özellikler penceresinde BGR 24 giriş hazır listeden seçin.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Küçük yazma
Bizim MP4 video farklıdır, birden fazla dosya JPG Kodlayıcı bileşeni çıkarır. Sahne arama JPG dosyası yazıcısı bileşeni bu işlemel için kullanılabilir: gelen JPG küçük resimleri alır ve bunları, farklı bir sayıyla soneki tüm dosya yazar. (Genellikle öğesinden küçük çizilmiş stream'de saniye/birim sayısını gösteren sayı.)

![Sahne arama JPG dosyası yazıcısı ile tanışın](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Sahne arama JPG dosyası yazıcısı ile tanışın*

Çıkış klasörü yolu özelliği ifade ile yapılandırma: ${ROOT_outputWriteDirectory}

ve dosya adı ön eki özelliği:

    ${ROOT_sourceFileBaseName}_thumb_

Ön ek, küçük resim dosyalarını nasıl adlı belirler. Bunlar, akışta başparmak konumu gösteren bir sayı ile soneki.

![Sahne arama JPG dosyası yazıcısı özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Sahne arama JPG dosyası yazıcısı özellikleri*

Sahne arama JPG dosyası yazıcısı çıktı dosyası/varlık düğümüne bağlanın.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Bir iş akışında hataları algılama
Renk alanı dönüştürücü girişi sıkıştırılmamış bir ham video çıkışına bağlayın. Artık iş akışı için yerel bir test gerçekleştirin. İş akışı aniden yürütülmesini durdurur ve bir kırmızı anahat bir hatayla karşılaştı bileşende ile belirtmek iyi bir fırsat vardır:

![Renk alanı dönüştürücü hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Renk alanı dönüştürücü hatası*

Başarısız köşe kodlama denemesi nedeni yenilikleri görmek için renk alanı dönüştürücü bileşenin sağ üst az red "E" simgesine tıklayın.

![Renk alanı dönüştürücü hata iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Renk alanı dönüştürücü hata iletişim kutusu*

Gördüğünüz gibi standart renk alanı dönüştürücü gelen renk alanını RGB istenen bizim YUV dönüştürülmesi için rec601 olmasını sahip, kullanıma açar. Görünüşe göre bizim stream kendi rec601 göstermiyor. (Rec 601 dijital video formunda aralıklı analog video sinyalleri kodlama standardıdır. Bu, 720 aydınlatma örnekleri ve 360 chrominance örnekleri her satıra bir etkin bölgeyi belirtir. Sistem kodlama renk YCbCr 4 bilinir: 2:2.)

Bu sorunu gidermek için biz size rec601 içerikle ilgilenme bizim akışın meta veriler belirtmek. Bunu yapmak için biz bizim ham kaynağı ve renk alanı dönüştürme bileşen arasında giriyorum bir Video veri türü güncelleştirici bileşeni kullanacağız. Bu veri türü güncelleştirici için el ile güncelleştirme video belirli veri türü özellikleri sağlar. Bir renk alanı standart "Rec 601" birini belirtmek için yapılandırın. Bu, hiçbir renk alanı henüz tanımlanmış ise "Rec 601" renk alanını stream'le etiketlemek Video veri türü güncelleştirici neden olur. (Geçersiz kılma onay kutusunu seçili sürece, var olan tüm meta verilere geçersiz kılmaz.)

![Veri türü güncelleştirici alanı standart renk güncelleştiriliyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Veri türü güncelleştirici alanı standart renk güncelleştiriliyor*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Tamamlanan iş akışı
İş tamamlandıktan sonra başka bir test yapmak arabimini görmek için Çalıştır.

![Çoklu mp4 çıktısına küçük resimleri ile tamamlanan iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Çoklu mp4 çıktısına küçük resimleri ile tamamlanan iş akışı*

## <a id="time_based_trim"></a>Zamana bağlı kesme multibitrate MP4 çıkış
Oluşturan bir iş akışından başlangıç [Giriş bir MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz artık kaynak videonun zaman damgalarını kırpma içine arayacaktır.

### <a id="time_based_trim_start"></a>Kırpma için eklemeye başlamak için iş akışı genel bakış
![Kırpma için eklemek için iş akışı başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Kırpma için eklemek için iş akışı başlatma*

### <a id="time_based_trim_use_stream_trimmer"></a>Stream Ayarlayıcısı kullanma
Stream Ayarlayıcısı bileşen şekilde başlangıç ve bitiş zamanlama bilgilerini (saniye, dakika,...) hakkında temel bir giriş akışı, kırpmanıza olanak sağlar. Çerçeve tabanlı kesme Ayarlayıcısı desteklemez.

![Stream Ayarlayıcısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stream Ayarlayıcısı*

AVC kodlayıcılar ve Konuşmacı konumu alanlarınız için medya dosyası girişini doğrudan bağlama yerine, ki bunlar arasında akış Ayarlayıcısı giriyorum. (Bir video sinyali ve bir araya eklemeli ses sinyal.)

![Stream Ayarlayıcısı arasında yerleştirin](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Ayarlayıcısı arasında yerleştirin*

Böylece biz yalnızca video ve ses 15 saniyede 60 saniye içinde video arasındaki işleyecek Ayarlayıcısı yapılandıralım.

Video Stream Ayarlayıcısı özelliklerine gidin ve hem başlangıç saati'ı yapılandırın (15 s) ve bitiş zamanı (60 s) özellikleri. Hem bizim ses ve video Ayarlayıcısı her zaman aynı başlangıç ve bitiş değerleri için yapılandırıldığından emin olmak için Biz bu iş akışının kök yayımlayın.

![Başlangıç saati Stream Ayarlayıcısı özelliğinden yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Başlangıç saati Stream Ayarlayıcısı özelliğinden yayımlama*

![Yayımlama için başlangıç saati özelliği iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Yayımlama için başlangıç saati özelliği iletişim kutusu*

![Yayımlama için bitiş saati özelliği iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Yayımlama için bitiş saati özelliği iletişim kutusu*

Biz, artık iş kök inceleyin, her iki özellik buradan düzgünce görüntülenir ve yapılandırılabilir.

![Kök kullanılabilen yayımlanan Özellikler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Kök kullanılabilen yayımlanan Özellikler*

Şimdi ses Ayarlayıcısı kırpmayı özelliklerini açın ve iş kökünde yayımlanmış özelliklere başvuran bir ifadeyle hem başlangıç ve bitiş zamanlarını yapılandırın.

Kırpma ses için başlangıç saati:

    ${ROOT_TrimmingStartTime}

ve kendi bitiş zamanı:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Tamamlanan iş akışı
![Tamamlanan iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Tamamlanan iş akışı*

## <a id="scripting"></a>Betikleştirilmiş bileşeni ile tanışın
Komut dosyalı bileşenleri, iş yürütme aşamaları sırasında rastgele betikleri çalıştırabilirsiniz. Yürütülebilecek, dört farklı betik vardır her belirli özellikleri ve kendi yerinde iş akışı yaşam döngüsü:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

Belgeleri Script bileşenin her biri Yukarıdakilerden için daha ayrıntılı olarak gider. İçinde [aşağıdaki bölümde](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), **realizeScript** betik bileşen iş akışı başladığında hareket halindeyken cliplist xml oluşturmak için kullanılır. Bu betik, yaşam döngüsü içinde yalnızca bir kez gerçekleşir Bileşen Kurulumu sırasında çağrılır.

### <a id="scripting_hello_world"></a>Bir iş akışı içinde betik: Merhaba Dünya
Bir komut dosyası bileşeni Tasarımcı yüzeyine sürükleyin ve adlandırın (örneğin, "SetClipListXML").

![Betikleştirilmiş bileşeni ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Betikleştirilmiş bileşeni ekleme*

Komut dosyası bileşen özelliklerini incelediğinizde, dört farklı komut türlerine gösterilen, farklı bir betik her yapılandırılabilir olacaktır.

![Betikleştirilmiş bileşeni özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Betikleştirilmiş bileşeni özellikleri*

ProcessInputScript temizleyin ve realizeScript için düzenleyicisini açın. Şimdi biz Kurulum ve hazırlık betik oluşturmaya başlamak hazırsınız.

Betikler, Groovy, Java ile uyumluluğu koruyan Java platformu için dinamik olarak derlenmiş bir betik oluşturma dili yazılır. Aslında, çoğu Java kodu geçerli Groovy kodudur.

Basit bir hello world groovy betik bizim realizeScript bağlamında yazalım. Düzenleyicide aşağıdakileri girin:

    node.log("hello world");

Şimdi yerel test çalıştırması yürütün. Bu, sonrasında çalıştırma, günlükleri özelliği (aracılığıyla Script bileşenin Sistem sekmesinde) inceleyin.

![Hello world günlük çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world günlük çıktısı*

"Düğüm", geçerli ya da biz içinde komut bileşeni, günlük yöntemi diyoruz düğüm nesnesi başvurur. Her bileşen, bu nedenle çıktı günlüğü verileri, sistem sekmesi kullanılabilir için özelliğine sahiptir. Bu durumda, biz "hello world." dize sabit değeri çıktı İşte bu komut aslında yaptığı hakkında öngörü sağlayan her hata ayıklama aracı olmasını kanıtlayabilirsiniz anlamak önemlidir.

Gelen kodlama ortamımızda içinde de özelliklerine erişimi diğer bileşenlere bağlı sahibiz. Şunu deneyin:

```java
    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up to other nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);
```

Bizim Günlüğü penceresini bize aşağıda gösterilmiştir:

![Düğüm yolları erişmek için günlük çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Düğüm yolları erişmek için günlük çıktısı*

## <a id="frame_based_trim"></a>Çerçeve tabanlı kesme multibitrate MP4 çıkış
Oluşturan bir iş akışından başlangıç [Giriş bir MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz artık çerçeve sayılarına dayalı kaynak görüntü kırpma içine arayacaktır.

### <a id="frame_based_trim_start"></a>Kırpma için eklemeye başlamak için şema genel bakış
![Kırpma için eklemeye başlamak için iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Kırpma için eklemeye başlamak için iş akışı*

### <a id="frame_based_trim_clip_list"></a>Küçük resim listesi XML kullanma
Önceki tüm iş akışı öğreticilerde, bizim video giriş kaynağı olarak medya dosyası giriş bileşen kullandık. Bu belirli bir senaryo için küçük resim listesi kaynak bileşen yerine kullanacağız. Bu çalışma, tercih edilen yol olmamalıdır; Bunu yapmak için gerçek bir neden olduğunda yalnızca küçük kaynak listesi kullanın (aşağıdaki örnekte, burada yapmak ister küçük listesi kırpmayı özelliklerinin).

Bizim medya dosyası girişini küçük listesi kaynağına geçmek için küçük listesi kaynak bileşen tasarım yüzeyine sürükleyin ve iş akışı Tasarımcısı için küçük resim listesi XML düğümü küçük listesi XML PIN bağlanın. Bu, bizim giriş videosu göre küçük kaynak listesi çıkış sabitleyicileri ile doldurur. Artık sıkıştırılmamış Video ve ses sıkıştırılmamış PIN, ilgili AVC kodlayıcılar ve ses Stream ayırıcı küçük resim listesi kaynağından bağlanın. Şimdi, medya dosyası girişini kaldırın.

![Medya dosyası girişini ile küçük kaynak listesi değiştirildi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Medya dosyası girişini ile küçük kaynak listesi değiştirildi*

Küçük resim listesi kaynak bileşen, girdi olarak "küçük liste XML." alır Yerel olarak test etmek için kaynak dosya seçerken bu küçük resim listesi xml sizin için otomatik olarak doldurulur.

![Küçük resim listesi XML özelliği otomatik olarak doldurulur](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Küçük resim listesi XML özelliği otomatik olarak doldurulur*

Xml için biraz daha yakından baktığımızda bu nasıl gibi görünüyor.

![Küçük resim listesi iletişim kutusunda Düzenle](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Küçük resim listesi iletişim kutusunda Düzenle*

Bu küçük resim listesi xml yeteneklerini ancak yansıtmaz. Her iki video ve ses kaynak altında bunun gibi bir "Kırpma" öğesi eklemek için bir seçenek sunuyoruz verilmiştir:

![Küçük listesine bir kesim öğesi ekleniyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Küçük listesine bir kesim öğesi ekleniyor*

Bunun üzerinde gibi küçük listesi XML'i değiştirin ve yerel bir test çalıştırması gerçekleştirmek, videoyu görürsünüz doğru videoda 10 ile 20 saniye arasında kırpılır.

Yerel çalıştırma ancak bunu yaptığınızda ne aykırı, bu aynı cliplist xml Azure Media Services'da çalışan bir iş akışında uygulandığında aynı etkiye sahip olmaz. Azure Premium Kodlayıcı başladığında cliplist xml yeniden kodlama işinin verilen giriş dosyasının temel alarak her zaman oluşturulur. Bu xml yaptığımız değişikliklerin ne yazık ki yazılırdı anlamına gelir.

Bir kodlama işi başladığında silmeden cliplist xml sayaç için biz bunu çalışma sırasında yalnızca iş başladıktan sonra yeniden oluşturabilirsiniz. Bu tür özel eylemler bir "Betikleştirilmiş bileşeni." adlı aracılığıyla alınabilir. Daha fazla bilgi için [Script bileşeni ile tanışın](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Komut dosyası bir bileşenin Tasarımcı yüzeyine sürükleyin ve bunu "SetClipListXML" Yeniden Adlandır

![Betikleştirilmiş bileşeni ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Betikleştirilmiş bileşeni ekleme*

Komut dosyası bileşen özelliklerini incelediğinizde, dört farklı komut türlerine gösterilen, her bir betik için yapılandırılabilir.

![Betikleştirilmiş bileşeni özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Betikleştirilmiş bileşeni özellikleri*

### <a id="frame_based_trim_modify_clip_list"></a>Bir komut dosyası bileşeninden küçük listesini değiştirme
Biz iş akışı başlatma sırasında oluşturulan cliplist xml yazabilirsiniz önce biz cliplist xml özellik ve içeriğini erişiminiz olması gerekir. Biz bunu şu şekilde yapabilirsiniz:

```java
    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);
```

![Gelen küçük listesi günlüğe kaydedildi.](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Gelen küçük listesi günlüğe kaydedildi.*

Öncelikle bu noktaya kadar Videoyu Kırp istiyoruz hangi noktasından belirlemek için bir yol gerekir. Bu iş akışının daha az teknik kullanıcıya uygun hale getirmek için iki özellik grafiğin kök yayımlayın. Bunu yapmak için tasarımcı yüzeyine sağ tıklayın ve "Özelliği Ekle"'i seçin:

* İlk özellik: "Tür"ClippingTimeStart: "ZAMAN"KODU
* İkinci özellik: "Tür"ClippingTimeEnd: "ZAMAN"KODU

![Kırpma için başlangıç saati Ekle özelliği iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Kırpma için başlangıç saati Ekle özelliği iletişim kutusu*

![İş akışı kökünde zaman özellikler kırpma yayımlandı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*İş akışı kökünde zaman özellikler kırpma yayımlandı*

Her iki özellik için uygun bir değere yapılandırın:

![Kırpma Başlangıç yapılandırmak ve özellikleri bitiş](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Kırpma Başlangıç yapılandırmak ve özellikleri bitiş*

Şimdi, betiğimizi içinde Biz bu gibi her iki özellik erişebilirsiniz:

```java
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);
```

![Başındaki ve sonundaki kırpma gösteren günlük penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Başındaki ve sonundaki kırpma gösteren günlük penceresi*

Şimdi zaman kodu dizeleri daha kullanışlı form, basit bir normal ifade kullanılırken kullanılacak ayrıştırılamıyor:

```java
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);
```

![Ayrıştırılmış zaman kodu çıkışı ile Günlüğü penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Ayrıştırılmış zaman kodu çıkışı ile Günlüğü penceresi*

Bu bilgilerle eldeki biz şimdi cliplist xml filmin istenen çerçeve doğru kırpma için başlangıç ve bitiş zamanları yansıtacak şekilde değiştirebilirsiniz.

![Kırpma öğeleri eklemek için betik kodu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kırpma öğeleri eklemek için betik kodu*

Bu, normal dize işleme işlemleri yapıldı. Elde edilen değiştirilmiş küçük listesi xml geri clipListXML özelliği iş akışı kökünde "setProperty" yöntemiyle yazılır. Başka bir test çalıştırdıktan sonra günlük penceresini bize gösterebilir:

![Küçük listesinde günlüğe kaydetme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Küçük listesinde günlüğe kaydetme*

Nasıl video ve ses akışları kırpılarak görmek için bir test çalıştırması yaparsınız. Kesme noktaları için farklı değerlerle birden fazla test çalıştırması yaparsınız gibi bu hesaba ancak alınmayacak olduğunu fark edeceksiniz! Bunun nedeni, Azure çalışma zamanı aksine Tasarımcısı, her çalıştırma cliplist xml kılmaz ' dir. Yalnızca ilk kez, giriş ve çıkış noktaları, ayarladığınız tüm diğer durumlarda, bizim guard yan tümcesi dönüştürülecek xml neden olacak bu anlamına gelir (varsa (`clipListXML.indexOf("<trim>") == -1`)) iş akışı zaten mevcutsa mevcut bir kesim başka bir öğeye eklemesini önler.

İş yerel olarak test etmek kullanışlı hale getirmek için en iyi biz bir kesim öğesi zaten mevcut değilse inceler bazı ev tutma kodu ekleyin. Bu durumda, biz xml yeni değerlerle değiştirerek devam etmeden önce kaldırabilirsiniz. Düz dize işlemeleri kullanmak yerine, ayrıştırma gerçek xml nesne modeli aracılığıyla Bunu yapmak büyük olasılıkla daha güvenlidir.

İçeri aktarma deyimlerini sayısı betiğimizi başlangıcında eklemeniz gerekir, ancak bu tür kod ekleyebilmeniz için önce:

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;
```

Bundan sonra gerekli temizleme kodu ekleyebilirsiniz:

```java
    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();
```

Bu kod, kırpma öğeleri cliplist xml ekleriz noktasından hemen üstündeki geçer.

Bu noktada, biz çalıştırın ve iş kadar hiç olmadığı kadar uygulanan değişiklikler yaparken istediğiniz zaman olarak değiştirin.    

### <a id="frame_based_trim_clippingenabled_prop"></a>ClippingEnabled kolaylık özellik ekleme
Şimdi, her zaman gerçekleşecek kırpma istemeyebilirsiniz gibi iş devre dışı kesme / kırpma sağlamak istiyoruz olup olmadığını belirten kullanışlı bir Boole bayrağı ekleyerek tamamlayın.

Önceki örneklerde olduğu gibi "ClippingEnabled" adlı iş kök yeni bir özellik türü "Boole" yayımlayın.

![Kırpma etkinleştirmek için bir özellik yayımlandı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Kırpma etkinleştirmek için bir özellik yayımlandı*

İle basit guard yan biz kırpmayı gerekli olup olmadığını denetleyebilir ve küçük listemize şekilde veya değiştirilmesi gerekip gerekmediğine karar verebilirsiniz.

```java
    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }
```

### <a id="code"></a>Tam kod

```java
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse the end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from the clip list xml by parsing the xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements to cliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }
```

## <a name="also-see"></a>Ayrıca bkz:
[Premium Azure medya Hizmetleri kodlama ile tanışın](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Premium Azure medya Hizmetleri kodlama kullanma](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Azure medya Hizmetleri ile isteğe bağlı içerik kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)

[Media Encoder Premium İş Akışı Biçimleri ve Kod Çözücüleri](media-services-premium-workflow-encoder-formats.md)

[Örnek iş akışı dosyaları](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Gezgini aracı](https://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
