---
title: "Avanced Medya Kodlayıcısı Premium iş akışı öğreticileri"
description: "Bu belge Medya Kodlayıcısı Premium akışıyla Gelişmiş görevlerin nasıl gerçekleştirileceği ve ayrıca iş akışı Tasarımcısı ile karmaşık iş akışları oluşturmak nasıl gösteren izlenecek yollar içeriyor."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 565497bd5a35e3c4d69d29512307cf3ca2364bdd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Gelişmiş Medya Kodlayıcısı Premium iş akışı öğreticileri
## <a name="overview"></a>Genel Bakış
Bu belge iş akışlarıyla özelleştirmeyi Göster izlenecek yollar içeriyor **iş akışı Tasarımcısı**. Gerçek iş akışı dosyalarını bulabilirsiniz [burada](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>İÇİNDEKİLER
Aşağıdaki konular ele alınmaktadır:

* [Tek bit hızlı MP4 MXF kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Yeni bir iş akışı başlatma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Medya dosyası girişi kullanma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Medya akışlarının inceleniyor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Bir video Kodlayıcısı için ekleniyor. MP4 dosyası oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Ses akışı kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Ses ve Video akışları bir MP4 kapsayıcıya çoğullama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [MP4 dosyası yazılıyor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Çıktı dosyasından bir Media Services varlık oluşturma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Tamamlanmış iş akışı yerel olarak test etme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [MXF MP4s - multibitrate kodlama etkin dinamik paketleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Bir veya daha fazla ek MP4 çıktı ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Dosya çıkış adları yapılandırma](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Ayrı bir ses izi ekleme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [Ekleme. ISM SMIL dosyası](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [MP4 - Gelişmiş şeması multibitrate MXF kodlama](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Geliştirmek için iş akışı genel bakış](#workflow-overview-to-enhance)
  * [Dosya adlandırma kuralları](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [İş akışı kök üzerine yayımlama bileşeni özellikleri](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Çıktı dosyası adları yayımlanan özellik değerlerine dayanan oluşturulmasını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Küçük resimleri multibitrate MP4 çıktı ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Küçük resim eklemek için iş akışı genel bakış](#workflow-overview-to-add-thumbnails-to)
  * [JPG kodlama ekleme](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Renk alanını dönüştürme postalarla](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Küçük resimlerin yazma](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Bir iş akışında hataları algılama](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Tamamlanmış iş akışı](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Zamana bağlı kırpma multibitrate MP4 çıktı](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Kırpma için eklemeye başlamak için iş akışı genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Akış Kırpıcıyı kullanma](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Tamamlanmış iş akışı](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Komut dosyalı bileşen Tanıtımı](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Bir iş akışı içinde komut dosyası: Merhaba Dünya](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [Çerçeve tabanlı kırpma multibitrate MP4 çıktı](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Kırpma için eklemeye başlamak için ayrıntılı genel bakış](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [XML küçük resim listesi kullanma](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [Komut dosyası bir bileşenin küçük listesini değiştirme](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [ClippingEnabled kolaylık özellik ekleme](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>Tek bit hızlı MP4 MXF kodlama
Bu kılavuzda tek bit hızlı oluşturacağız. MP4 dosyası AAC-HE ile kodlanmış ses öğesinden bir. MXF giriş dosyası.

### <a id="MXF_to_MP4_start_new"></a>Yeni bir iş akışı başlatma
İş Akışı Tasarımcısı'nı açın ve "Dosyası"-"Yeni çalışma alanı"-"kodlamasını şeması" seçin

Yeni bir iş akışı 3 öğeleri gösterir:

* Birincil kaynak dosyası
* Küçük resim listesi XML'i
* Çıkış dosyası/varlık  

![Yeni bir kodlama iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Yeni bir kodlama iş akışı*

### <a id="MXF_to_MP4_with_file_input"></a>Medya dosyası girişi kullanma
Bizim giriş medya dosyası kabul etmek için bir medya dosyası giriş bileşeni ekleme ile başlar. Bir bileşenin iş akışına eklemek için deposu arama kutusuna aramanız ve istenen girişi Tasarımcı bölmesine sürükleyin. Medya dosyası için giriş yapmak ve birincil kaynak dosya bileşen Filename giriş PIN medya dosyası giriş bağlanın.

![Bağlı ortam giriş dosyası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Bağlı ortam giriş dosyası*

Başka bir çok yapabiliriz önce biz ilk iş akışı Tasarımcısı biz bizim iş akışıyla tasarlamak için kullanmak istediğiniz hangi örnek dosyası belirtmek gerekir. Bunu yapmak için tasarımcı bölmesinde arka plan tıklayın ve sağ taraftaki özellik bölmesinde birincil kaynak dosya özellikte arayın. Klasör simgesine tıklayın ve iş akışı ile test etmek istediğiniz dosyayı seçin. Bu yapılır hemen medya dosyası giriş bileşen dosyasını inceleyin ve onu Denetlenmekte dosya yansıtacak şekilde kendi çıktı pini doldurun.

![Doldurulan ortam giriş dosyası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Doldurulan ortam giriş dosyası*

Bu hangi giriş ile birlikte çalışmak isteriz onu bildirmez belirtir ancak henüz kodlanmış çıktı için nereye. Benzer şekilde nasıl birincil kaynak dosyası yapılandırıldı, hemen altındaki çıkış klasörü değişken özelliği şimdi yapılandırın.

![Yapılandırılan giriş ve çıkış özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Yapılandırılan giriş ve çıkış özellikleri*

### <a id="MXF_to_MP4_streams"></a>Medya akışlarının inceleniyor
Genellikle akış gibi akışları akışı görüntülenme şeklini bilmeniz istenen. İş akışı içinde herhangi bir noktada bir akış incelemek için yalnızca bir çıkış veya giriş PIN bileşenleri hiçbirinde tıklatın. Bu durumda, bizim medya dosyası girişten gelen sıkıştırılmamış Video Çıkış PIN tıklayarak deneyin. Bir iletişim kutusu giden video incelemek için veren açılır.

![Sıkıştırılmamış Video Çıkış PIN inceleniyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Sıkıştırılmamış Video Çıkış PIN inceleniyor*

Örneğimizde, bize örneğin biz 4 saniyede 24 kare konumundaki 1920 x 1080 giriş ile ilgilenen olduğunu söyler: 2:2 örnekleme neredeyse 2 dakikalık video.

### <a id="MXF_to_MP4_file_generation"></a>Bir video Kodlayıcısı için ekleniyor. MP4 dosyası oluşturma
Şimdi unutmayın, sıkıştırılmamış bir Video ve birden çok sıkıştırılmamış ses çıkış PIN bizim ortam dosyası girişi üzerinde kullanılabilir. Gelen videoyu kodlamak için kodlama bileşeni - bu durumda oluşturmak için ihtiyacımız var. Mp4 dosyaları.

H.264 video akışına kodlayın Tasarımcı yüzeyine AVC Video Kodlayıcısı bileşen ekleyin. Bu bileşen uncompress video akışına giriş olarak alır ve bir AVC sıkıştırılmış video akışı kendi çıktı PIN sunar.

![Bağlantısız AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Bağlantısız AVC kodlayıcı*

Özellikleri tam olarak kodlama nasıl gerçekleştiğini belirleyin. Şimdi daha önemli ayarlardan bazıları bakma vardır:

* Çıktı genişlik ve yükseklik çıktı: Bu kodlanmış video çözünürlüğünü belirler. Örneğimizde 640 x 360 ile edelim
* Kare hızı: geçiş için ayarlandığında, kaynak kare hızı yalnızca benimseyeceği, ancak bu geçersiz kılma mümkündür. Böyle kare hızı dönüştürme değil hareket-dengelendi olduğunu unutmayın.
* Profil ve düzeyi: Bunlar AVC profil ve düzeyini belirler. Rahat farklı düzeylerde ve profilleri hakkında daha fazla bilgi almak için AVC Video Kodlayıcısı bileşen soru işareti simgesine tıklayın ve her düzeyleri hakkında daha fazla ayrıntı Yardım sayfası gösterilir. Bizim örnek için 3.2 (varsayılan) düzeyinde ana profille edelim.
* Denetim modu, bit hızı (kbps) oranı: biz opt Senaryomuzda 1200 KB/sn ile çıkış sabit hızı (CBR) için
* : Video bu H.264 akışa (görüntü geliştirmek için bir kod çözücü tarafından kullanılan ancak doğru çözecek temel olabilir yan bilgileri) yazılmış VUI (Video kullanılabilirlik bilgileri) hakkında biçimdedir:
* NTSC (ABD veya Japonya, 30 fps kullanma için tipik)
* PAL (25 fps kullanarak Avrupa için tipik)
* GOP boyutu modu: 2 saniye kapalı GOPs ile bir anahtar aralığı ile bizim amacıyla sabit GOP boyutu yapılandıracağız. Bu uyumluluk dinamik paketleme Azure Media Services ile sağlar sağlar.

Bizim AVC Kodlayıcı akış için medya dosyası giriş bileşeninden sıkıştırılmamış Video Çıkış PIN AVC kodlayıcıdan sıkıştırılmamış Video giriş PIN bağlayın.

![Bağlı AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Bağlı AVC ana kodlayıcı*

### <a id="MXF_to_MP4_audio"></a>Ses akışı kodlama
Bu noktada, biz video kodlanmıştır ancak özgün sıkıştırılmamış ses akışı hala sıkıştırılmış gerekiyor. Bunun için biz AAC AAC Kodlayıcı (Dolby) bileşeni tarafından kodlama ile gidersiniz. İş akışına ekleyin.

![Bağlantısız AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Bağlantısız AAC kodlayıcı*

Bir uyumsuzluk şimdi: büyük olasılıkla medya dosyası giriş iki farklı sıkıştırılmamış ses akışı kullanılabilir olsa yalnızca bir tek sıkıştırılmamış ses giriş PIN AAC kodlayıcıdan olduğu: sol ses kanal, diğeri sağ için. (Surround sesle ele alma, 6 kanalları olmasıdır.) Bu nedenle doğrudan ses ortam dosyası girişi kaynağından AAC ses Kodlayıcı bağlamak mümkün değil. AAC bileşen sözde "araya eklemeli" ses akışı bekliyor: sol ve sağ kanallar birbirleri ile araya eklemeli sahip tek bir akış. Bizim kaynak medya dosyasından biliyoruz verdikten sonra hangi ses izleri kaynağındaki hangi konumuna biz sol ve sağ için doğru atanan Konuşmacı konumlarına olan araya eklemeli gibi ses akış oluşturabilirsiniz demektir.

Önce bir oluşturulan bir araya eklemeli akışı gerekli kaynak ses kanaldan isteyeceksiniz. Ses akışı ayırıcı bileşen bu bize işleyecek. İş akışını Ekle ve ses çıkış medya dosyası girişten içine bağlanın.

![Ses akışı ayırıcı bağlı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Ses akışı ayırıcı bağlı*

Bir araya eklemeli ses akışını sahibiz, biz hala sola veya sağa Konuşmacı konumlara atamak istediğiniz yeri belirtin alamadık. Bu belirtmek için Konuşmacı konumu Assigner yararlanabilirsiniz.

![Konuşmacı konumu Assigner ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Konuşmacı konumu Assigner ekleme*

Konuşmacı konumu Assigner kullanmak için bir kodlayıcı önceden filtresinden "Özel" ve "2.0 (M, R)" adlı kanal hazır olan stereo giriş akış yapılandırın. (Bu 1 kanal için sol Konuşmacı konumu ve sağ Konuşmacı konumu kanalına 2 atayacaktır.)

Konuşmacı konumu Assigner çıkışına AAC Kodlayıcı girişine bağlayın. Ardından, bir "2.0 ile (M, R)" çalışacak biçimde AAC Kodlayıcı söyleyin kanal stereo ses giriş olarak çalışılabilecek bilmesi için hazır.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Ses ve Video akışları bir MP4 kapsayıcıya çoğullama
Bizim AVC belirli bir ses akışını kodlanmış video akışına ve bizim AAC kodlanmış, biz içine yakalamak için bir. MP4 kapsayıcı. Farklı akışları tek bir karıştırma işleminin "çoğullama" (veya "muxing") adı verilir. Bu durumda biz ses ve video akışları tek bir tutarlı Interleaving. MP4 paketi. Bu düzenler bileşen bir. MP4 kapsayıcı ISO MPEG-4 çoğaltıcı adı verilir. Tasarımcı yüzeyine ekleyin ve AVC Video Kodlayıcısı ve AAC Kodlayıcı girdilerinden için bağlanın.

![Bağlı MPEG4 çoğaltıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Bağlı MPEG4 çoğaltıcı*

### <a id="MXF_to_MP4_writing_mp4"></a>MP4 dosyası yazılıyor
Dosya çıktısı bileşeni bir çıktı dosyası yazılırken kullanılır. Çıktısını yazılmış Biz bu ISO MPEG-4 çoğaltıcı çıkışına bağlanabileceği diske. Bunu yapmak için yazma giriş PIN dosya çıktısı için kapsayıcı (MPEG-4) çıkış PIN bağlayın.

![Dosya çıktısı bağlı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Dosya çıktısı bağlı*

Kullanılacak dosya adı dosya özelliği tarafından belirlenir. Bu özellik sabit kodlanmış belirli bir değere olabilirler, ancak büyük olasılıkla bir bunun yerine bir ifade yoluyla ayarlamak istediğiniz.

İş akışı çıktısı otomatik olarak belirlemek için dosya adı bir ifade özelliğinden, düğme (yanındaki klasör simgesine) dosya adının yanındaki'ı tıklatın. Açılan menüden "İfadesi" ardından seçin. Bu ifade Düzenleyicisi çıkarır. Düzenleyici içeriğini ilk temizleyin.

![Boş ifade Düzenleyicisi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Boş ifade Düzenleyicisi*

Bir veya daha fazla değişkenlerle karma herhangi bir değişmez değer girmesini ifade Düzenleyicisi sağlar. Değişkenleri dolar işareti başlatın. $ Tuşuna basın, Düzenleyici bir seçenek kullanılabilir değişkenlere sahip bir açılan kutu gösterecektir. Çıktı dizini değişken ve temel giriş dosyası adı değişkeni bileşimini örneğimizde kullanacağız:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![İfade Düzenleyicisi çıkışı dolu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*İfade Düzenleyicisi çıkışı dolu*

> [!NOTE]
> Görmek için çıkış dosyasını görmek kodlama Azure, işinizin ifade Düzenleyicisi'nde bir değer belirtmeniz gerekir.
>
>

Tamam basarsa tarafından ifade onayladığınızda özellik penceresi hangi dosya özelliği çözümler bu anda değerin için Önizleme.

![Çıktı dizini dosyası ifadesini çözümler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Çıktı dizini dosyası ifadesini çözümler*

### <a id="MXF_to_MP4_asset_from_output"></a>Çıktı dosyasından bir Media Services varlık oluşturma
Biz bir MP4 çıktı dosyası yazılmış olsa da, biz yine bu dosyayı media services bu iş akışı yürütmenin sonucu olarak oluşturan çıkış varlığına ait olduğunu belirtmek gerekir. Bu amaçla, iş akışı tuval üzerinde çıktı dosyası/varlık düğümü kullanılır. Bu düğüm tüm gelen dosyalarıyla sonuç Azure Media Services varlık parçası hale getirir.

Dosya çıktısı bileşen iş akışı tamamlamak için çıktı dosyası/varlık bileşenine bağlayın.

![Tamamlanmış iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Tamamlanmış iş akışı*

### <a id="MXF_to_MP4_test"></a>Tamamlanmış iş akışı yerel olarak test etme
İş akışı yerel olarak test etmek için üst araç çubuğunda YÜRÜT düğmesine basın. İş akışı yürütme tamamlandığında, yapılandırılan çıkış klasöründe oluşturulan çıktıyı inceleyin. MXF giriş kaynağı dosyasından kodlanan tamamlanmış MP4 çıktı dosyası görürsünüz.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Dinamik paketleme etkin MP4 - multibitrate MXF kodlama
Bu kılavuzda Çoklu bit hızlı MP4 dosyaları kümesini kodlanmış AAC ile tek bir ses oluşturacağız. MXF giriş dosyası.

Ne zaman Çoklu bit hızlı varlık çıkış birlikte kullanmak için Azure Media Services, birden çok GOP hizalı MP4 dosyaları her farklı bit hızı ve çözüm oluşturulması gerekecek tarafından sunulan dinamik paketleme özelliklerle istendiğini. Bunu yapmak için [kodlama MXF tek bit hızlı MP4 içine](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) izlenecek bize iyi bir başlangıç noktası sağlar.

![İş akışı başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*İş akışı başlatma*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Bir veya daha fazla ek MP4 çıktı ekleme
Sonuçta elde edilen bizim Azure Media Services varlık her MP4 dosyasında farklı bit hızı ve çözüm destekleyecektir. Bir veya daha fazla MP4 çıktı dosyaları akışına ekleyelim.

Aynı ayarlarla oluşturulan tüm bizim video Kodlayıcıları sahibiz emin olmak için zaten varolan AVC Video Kodlayıcısı yinelenen ve başka bir birleşimi çözünürlük ve bit hızı (960 x 540 birini 2,5 MB/sn, saniye başına 25 çerçeveler adresindeki ekleyelim yapılandırmak en uygun ). Var olan Kodlayıcı çoğaltmak için kopyalama yapıştırın, Tasarımcı yüzeyine.

Medya dosyası giriş sıkıştırılmamış Video Çıkış PIN bizim yeni AVC bileşen halinde bağlayın.

![Bağlı ikinci AVC kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Bağlı ikinci AVC kodlayıcı*

Şimdi 960 x 540 2,5 Mbps hızında çıktısını almak yeni bizim AVC kodlayıcı yapılandırması uyarlayın. (Çıktı özellikleri "genişliğini", "Çıktı yükseklik" ve "Bit hızı (kbps)" Bu için kullanın.)

Verilen sonuç varlık Azure Media Services dinamik paketleme ile birlikte kullanmak istiyoruz, akış uç noktası bu MP4 dosyalarını tam olarak başka bir şekilde hizalanır HLS/parçalanmış MP4/DASH parçaları oluşturma yeteneğine sahip olması gerekir, farklı bit arasında geçiş istemcileri tek sorunsuz bir sürekli video ve ses deneyim alın. Gerçekleşen yapmak için her iki AVC kodlayıcılar resimlerin ("grubu") GOP özelliklerinde boyutu hem MP4 dosyaları tarafından yapılan 2 saniye ayarlanır, emin olmak ihtiyacımız var:

* Sabit GOP boyutuna GOP boyutu modunu ayarlama ve
* Anahtar çerçeve aralığı iki saniye.
* Ayrıca tüm GOP's emin olmak için GOP kapalı GOP IDR denetimine duran olmadan kendi bağımlılıkları

Bizim iş akışı anlamak kullanışlı hale getirmek için ilk AVC Kodlayıcı yeniden adlandırma "AVC Video Kodlayıcısı 640 x 360 1200kbps" ve ikinci AVC Kodlayıcı "AVC Video Kodlayıcısı 960 x 540 2500 kbps".

İkinci bir ISO MPEG-4 çoğaltıcı ve ikinci bir dosya çıktısı. Şimdi ekleyin. Çoğaltıcı yeni AVC kodlayıcıya bağlanmak ve çıktısını dosya çıktısı yönlendirilmiş emin olun. Sonra da AAC ses Kodlayıcı çıkışı çoğaltıcı ait yeni giriş bağlanır. Dosya çıktısı sırayla sonra oluşturulacak Media Services varlık eklemek için çıktı dosyası/varlık düğüme bağlanabilir.

![Bağlı ikinci Karıştırıcı ve dosya çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Bağlı ikinci Karıştırıcı ve dosya çıktısı*

Azure Media Services dinamik paketleme ile uyumluluk için GOP sayısı veya süresi çoğaltıcı 's öbek modunu yapılandırmak ve GOPs öbek başına 1 olarak ayarlayın. (Bu varsayılan olmalıdır.)

![Karıştırıcı öbek modları](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Karıştırıcı öbek modları*

Not: Varlık çıktı eklemek istediğiniz diğer bit hızı ve çözüm birleşimleri için bu işlemi tekrarlayın isteyebilirsiniz.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Dosya çıkış adları yapılandırma
Birden fazla tek çıkış varlığına eklenen dosya sahibiz. Bu, her ve çıkış dosyalarının dosya adları birbirinden farklı olduğundan emin olun ve belki de ile ilgilenen dosya adından açık olacak şekilde bile dosya adlandırma kuralını uygula gerek sağlar.

Çıkış dosya adlandırma Tasarımcısı'nda ifadeler ile denetlenebilir. Dosya çıktısı bileşenlerden biri için özellik bölmesini açın ve dosya özelliği için ifade Düzenleyicisi'ni açın. Bizim ilk çıkış dosyası ifadesini yapılandırılmış (gelen gitmek için öğretici bkz [tek bit hızlı MP4 çıktı MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Bu bizim filename iki değişken tarafından belirlenir anlamına gelir: yazmak için çıktı dizini ve kaynak dosya temel adı. Eski iş akışı kök bir özellik olarak sunulan ve ikincisi gelen dosya tarafından belirlenir. Çıktı dizini yerel testte kullanmak olduğunu unutmayın; Azure Media Services bulut tabanlı media işlemcisi tarafından iş akışı çalıştırıldığında bu özellik iş akışı altyapısı tarafından geçersiz kılınacaktır.
Her iki bizim çıktı dosyalarını adlandırma tutarlı çıkış vermek için ilk dosya ifadesine adlandırma değiştirin:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

İkincisi için:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Her iki MP4 çıktı dosyaları düzgün şekilde oluşturulan emin olmak için bir ara testi yürütün.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Ayrı bir ses izi ekleme
Biz bizim MP4 çıktı dosyalarıyla gitmek için bir .ism dosyası oluştururken daha sonra anlatıldığı gibi biz de yalnızca ses MP4 dosyası ses parçası bizim Uyarlamalı akış için gerektirir. Bu dosyayı oluşturmak için ek Karıştırıcı (ISO-MPEG-4 çoğaltıcı) eklemeniz ve AAC Kodlayıcı'nın çıkış PIN izleme 1 için kendi giriş PIN ile bağlanın.

![Ses karıştırıcı eklendi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Ses karıştırıcı eklendi*

Üçüncü Karıştırıcı giden akış çıkışı ve dosya ifade olarak adlandırma yapılandırma dosyası çıkış bileşeni oluşturun:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Ses karıştırıcı dosya çıktısı oluşturma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Ses karıştırıcı dosya çıktısı oluşturma*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Ekleme. ISM SMIL dosyası
Dinamik paketleme hem MP4 dosyaları (ve yalnızca ses MP4) bizim Media Services varlık ile birlikte çalışmak de bir bildirim dosyası ihtiyacımız ("SMIL" dosyası olarak da bilinir: eşitlenmiş multimedya tümleştirme dil). Bu dosya için Azure Media Services dinamik paketleme ve bu ses akış için dikkate alınması gereken hangi MP4 dosyaları kullanılabilir gösterir. Kümesiyle tek bir ses akışını MP4'ın için tipik bir bildirim dosyası şuna benzer:

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
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

Switch deyimi, her tek tek MP4 video dosyaları ve bu da bir (veya daha fazla) ses dosyası başvuruları yalnızca ses içeren bir MP4 için ek olarak bir başvuru içinde .ism dosyası içerir.

Bizim MP4'ın kümesi için bildirim dosyası oluşturma "AMS bildirim Yazan" adında bir bileşen ile yapılabilir. Kullanmak için yüzeyine sürükleyin ve "Yazma tamamlandı" çıktı pini üç dosya çıktısı bileşenlerini AMS bildirim yazan girdisi bağlanın. Ardından çıktı dosyası/varlık için bildirim AMS yazıcı çıkışına bağlayın emin olun.

Bizim diğer bileşenlerle dosya çıktı olarak, bir ifade .ism dosyası çıkış adıyla yapılandırın:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Bizim tamamlanmış iş akışı benzer altında:

![Tamamlanmış MXF multibitrate MP4 akışına](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Tamamlanmış MXF multibitrate MP4 akışına*

## <a id="MXF_to__multibitrate_MP4"></a>MP4 - Gelişmiş şeması multibitrate MXF kodlama
İçinde [önceki iş akışı Kılavuzu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) nasıl tek bir MXF giriş varlık bir çıktı varlığa Çoklu bit hızlı MP4 dosyaları, bir yalnızca ses MP4 dosyası ve Azure Media Services ile birlikte kullanmak için bir bildirim dosyası ile dönüştürülebilir gördük dinamik paketleme.

Bu kılavuzda nasıl bazı yönleri geliştirilebilen ve daha kullanışlı hale gösterir.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Geliştirmek için iş akışı genel bakış
![Geliştirmek için Multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Geliştirmek için Multibitrate MP4 iş akışı*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Dosya adlandırma kuralları
Önceki iş akışında çıktı dosyası adları oluşturmak için temel olarak basit bir ifade belirtildi. Bazı çoğaltma yine de sunuyoruz: tüm tek tek çıktı dosyası bileşenleri gibi ifade belirtilen.

Örneğin, bizim dosya çıktı bileşen ilk video dosyası için bu ifade ile yapılandırılır:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

İkinci çıktısı için sırada video, biz gibi bir ifadeye sahip:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Hata daha az yatkın ve biz Bu çoğaltma bazılarını kaldırın ve şeyleri daha yapılandırılabilir yerine yaparsanız daha kullanışlı temizleyici olmaz mıydı? Luckily geçebiliriz: özel özellikler bizim iş akışı kökünde oluşturma olanağı ile birlikte tasarımcının ifade özellikleri bize kolaylık eklenen bir katmanı verecektir.

Biz filename yapılandırma tek tek MP4 dosyaları bit sürücü varsayalım. Bu bit biz bir merkezi bir yerde (kökü verdiğimiz grafiğinin), burada yapılandırmak için erişmeleri ve sürücü dosya adı oluşturma yapılandırmak için hedeflenir. AVC kodlayıcılar uğradıysa hem kökünden de erişilebilir hale Bunu yapmak için şu iki AVC kodlayıcılar bit hızı özelliğinden bizim iş akışı kökünde yayımlayarak başlatın. (İki farklı noktayı kaçırmadığınızdan görüntülenen olsa bile, yalnızca bir temel alınan değer yoktur.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>İş akışı kök üzerine yayımlama bileşeni özellikleri
İlk AVC Kodlayıcı açın, bit hızı (kbps) özelliğine gidin ve yayımlama aşağı açılır listeden seçin.

![Bit hızı özelliği yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Bit hızı özelliği yayımlama*

Bizim iş akışı grafik kökünde yayımlamak için Yayımla iletişim yayımlanan adını "video1bitrate" ve "Video 1 bit hızı" okunabilir görünen adını yapılandırın. Özel bir yapılandırma grup adı "Bit akış" olarak adlandırılan ve yayımlama basın.

![Bit hızı özelliği yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Bit hızı özelliği için yayımlama iletişim kutusu*

Aynı ikinci AVC Kodlayıcı bit hızı özelliği için yineleyin ve aynı özel grup "Akış bit" ile "Video 2 hızı" görünen adını "video2bitrate" olarak adlandırın.

Biz şimdi iş akışı kök özelliklerini inceleme, görünmesini bizim Özel grup iki yayımlanan özellikleriyle göreceğiz. Her ikisi de kendi ilgili AVC Kodlayıcı hızı değerini yansıtma.

![İş akışı kök yayımlanan bit hızı özellik](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Biz kodu veya bir ifade bu özelliklere erişmek istediğiniz zaman, biz bunu şu şekilde yapabilirsiniz:

* Satır içi kodundan hemen kökü altındaki bir bileşenin: node.getPropertyAsString('../video1bitrate', null)
* bir ifade içinde: ${ROOT_video1bitrate}

Şimdi de bizim ses izleme hızı üzerindeki yayımlayarak "Akış bit" grubu tamamlayın. İçinde AAC Kodlayıcı özellikleri, bit hızı ayarı arayın ve Yayımla yanında aşağı açılan listeden seçin. Grafik adı "audio1bitrate" ile kökünde yayımlama ve görünen ad "Ses 1 bit hızı" bizim özel gruptaki "Akış bit".

![Ses bit hızı için yayımlama iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Ses bit hızı için yayımlama iletişim kutusu*

![Sonuçta elde edilen video ve ses özellik kök](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Sonuçta elde edilen video ve ses özellik kök*

Bu üç hiçbirini değiştirme de yeniden yapılandırır değerleri ve bunlar bağlı ile ilgili bileşenler değerlerine değiştirir unutmayın (ve bir yayımlandığı yerlerde).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Çıktı dosyası adları yayımlanan özellik değerlerine dayanan oluşturulmasını
Cmdlet'e kod yerine bizim oluşturulan dosya adları, biz şimdi biz yalnızca grafik kök yayımlanan bit hızı özellikleri güvenemeyeceklerini dosya çıktısı bileşenlerinden her biri üzerinde bizim filename ifade değiştirebilirsiniz. Bizim ilk dosya çıktı ile başlayarak, dosya özelliği bulun ve bu gibi ifadeyi düzenleyin:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Bu ifadede farklı parametreler erişilen ve ifade penceresinde klavyedeki dolar işareti basarsa tarafından girdiniz. Kullanılabilir parametrelerden biri, daha önce yayımlanan bizim video1bitrate özelliğidir.

![Bir ifade içinde Parametreler erişme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Bir ifade içinde Parametreler erişme*

Bizim ikinci video dosyası çıktısı için aynı işlemi yapın:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

ve yalnızca ses dosyası çıktısı için:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Biz artık herhangi bir ses veya video dosyaları için bit hızı değiştirirseniz, ilgili kodlayıcıyı yeniden yapılandırılması ve bit hızı tabanlı bir dosya adı kuralı tüm otomatik olarak kullanılacaktır.

## <a id="thumbnails_to__multibitrate_MP4"></a>Küçük resimleri multibitrate MP4 çıktı ekleme
Üreten bir iş akışından başlangıç [giriş MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz şimdi küçük resimleri çıktıya ekleyerek içine arayacaktır.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Küçük resim eklemek için iş akışı genel bakış
![Başlangıç için Multibitrate MP4 iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Başlangıç için Multibitrate MP4 iş akışı*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG kodlama ekleme
Bizim küçük resim oluşturma Kalp çıkış JPG dosyaları mümkün JPG Kodlayıcı bileşen olacaktır.

![JPG kodlayıcı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG kodlayıcı*

Ancak doğrudan sunduğumuz sıkıştırılmamış Video akışı medya dosyası girişten JPG Kodlayıcı bağlanamıyoruz. Bunun yerine, tek tek çerçeveler verilmesini bekliyor. Bu, bir Video çerçeve kapısı bileşen ile yapabiliriz.

![Bir çerçeve kapısı JPG kodlayıcıya bağlanma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Bir çerçeve kapısı JPG kodlayıcıya bağlanma*

Çerçeve ağ geçidi çok fazla sayıda saniyede veya çerçeveler geçirmek video bir çerçeve sağlar. Aralık ve hangi böyle ile uzaklığı saat özelliklerinde yapılandırılabilir.

![Video çerçeve kapısı özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video çerçeve kapısı özellikleri*

Küçük resim dakikada modu süresi (saniye) ve aralığı 60 ayarlanarak oluşturalım.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Renk alanını dönüştürme postalarla
Her iki sıkıştırılmamış Video PIN'ler çerçeve kapısı ve ortam dosyası girişi şimdi bağlanabilir mantıksal görünüyor, ancak biz bunu yapmayı tercih ediyorsanız size bir uyarı alır.

![Giriş rengi alan hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Giriş rengi alan hatası*

Hangi renkli bilgileri bizim özgün ham sıkıştırılmamış video akışında, bizim MXF gelen temsil edilen şekilde JPG Kodlayıcı bekleniyor öğesinden farklı olmasıdır. Daha açık belirtmek gerekirse bir sözde "renk aralığı" "RGB" veya "Gri" içinde akış beklenir. Başka bir deyişle, Video çerçeve ağ geçidi'nin gelen video akışına kendi renk alanını önce uygulanan bir dönüştürme olması gerekir.

İş akışı renk alanı Dönüştürücüsü - Intel sürükleyin ve bizim çerçeve ağ geçidine bağlayın.

![Bir renk alanı Dönüştürücüsü bağlanma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Bir renk alanı Dönüştürücüsü bağlanma*

Özellikler penceresinde önceden ayarlanmış listeden BGR 24 girişi seçin.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Küçük resimlerin yazma
Bizim MP4 video farklıdır, JPG Kodlayıcı bileşen birden fazla dosya çıktı. Bu işlem için bir Sahne arama JPG dosya yazıcısı bileşeni kullanılabilir: gelen JPG küçük resimleri ele ve bunları çıkışı, farklı bir sayı sonekine her dosya yazar. (Genellikle, gelen ve küçük çizilmiş akışında saniye/birim sayısını belirten sayı.)

![Sahne arama JPG dosyası yazan Tanıtımı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Sahne arama JPG dosyası yazan Tanıtımı*

Çıkış klasörü yolu özelliğini ifade ile yapılandırma: ${ROOT_outputWriteDirectory}

ve dosya adı önekini özelliğiyle:

    ${ROOT_sourceFileBaseName}_thumb_

Önek, küçük resim dosyaları nasıl adlı belirler. Bunlar olan akış parmak uzakta konumu belirten bir sayı sonekine.

![Sahne arama JPG dosya yazıcı özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Sahne arama JPG dosya yazıcı özellikleri*

Sahne arama JPG dosyası yazan çıktı dosyası/varlık düğüme bağlayın.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Bir iş akışında hataları algılama
Renk alanı dönüştürücü giriş sıkıştırılmamış bir ham video çıkışına bağlayın. Şimdi iş akışı için yerel testi gerçekleştirin. İş akışı aniden yürütme durdurun ve bir hata ile karşılaştı bileşen kırmızı çerçevesinde belirtmek iyi bir fırsat vardır:

![Renk alanı dönüştürücü hatası](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Renk alanı dönüştürücü hatası*

Kodlama girişimi neden yenilikleri görmek için renk alanını dönüştürücü bileşeninin köşe başarısız sağ üst küçük kırmızı "E" simgesine tıklayın.

![Renk alanı dönüştürücü hata iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Renk alanı dönüştürücü hata iletişim kutusu*

Gördüğünüz gibi için renk alanını dönüştürücü standart gelen renk alanını istenen bizim YUV dönüştürme RGB için rec601 olmak zorundadır çıkışı, etkinleştirir. Görünüşe göre bizim akışı onun rec601 göstermez. (Rec 601 aralıklı analog video sinyalleri dijital video formunda kodlama standardıdır. 720 aydınlatma örnekleri ve her satırda 360 chrominance örnekleri kapsayan etkin bir bölge belirtir. Sistem kodlama renk YCbCr 4 bilinir: 2:2.)

Bu sorunu gidermek için şu rec601 içerikle ilgilenme bizim akışın meta veriler üzerinde belirtmek. Bunu yapmak için şu bizim ham kaynak ve renk alanı dönüştürme bileşeni Between gireceğiniz bir Video veri türü güncelleştirici bileşeni kullanacağız. Bu veri türü güncelleştirici belirli bir video verileri el ile güncelleştirme için tür özellikleri sağlar. Bir renk alanı standart "Rec 601" olarak göstermek için yapılandırın. Bu, henüz tanımlanmış hiçbir renk alanını ise "Rec 601" renk alanını olan akış etiketlemek Video veri türü güncelleştirici neden olur. (Geçersiz kılma onay kutusu işaretli değilse, var olan tüm meta veriler geçersiz kılmaz.)

![Veri türü güncelleştirici renk alanı standardına güncelleştiriliyor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Veri türü güncelleştirici renk alanı standardına güncelleştiriliyor*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Tamamlanmış iş akışı
Şimdi bizim bizim iş akışı tamamlandı, başka bir sınama geçirmek görmek için Çalıştır.

![Küçük resimler ile çoklu mp4 çıktı için tamamlanmış iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Küçük resimler ile çoklu mp4 çıktı için tamamlanmış iş akışı*

## <a id="time_based_trim"></a>Zamana bağlı kırpma multibitrate MP4 çıktı
Üreten bir iş akışından başlangıç [giriş MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz artık kaynak videonun zaman damgalarını kırpma içine arayacaktır.

### <a id="time_based_trim_start"></a>Kırpma için eklemeye başlamak için iş akışı genel bakış
![Kırpma için eklemek için iş akışı başlatma](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Kırpma için eklemek için iş akışı başlatma*

### <a id="time_based_trim_use_stream_trimmer"></a>Akış Kırpıcıyı kullanma
Başlangıç ve bitiş bilgilerini (saniye, dakika,...) zamanlama temel bir giriş akışının kırpmaya akış Kırpıcıyı bileşeni sağlar. Kırpıcıyı tabanlı çerçeve kırpma desteklemez.

![Akış Ayarlayıcısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Akış Ayarlayıcısı*

AVC Kodlayıcıları ve Konuşmacı konumu assigner medya dosyası girdisi doğrudan bağlama yerine, biz bu Between akış Kırpıcıyı put. (Bir video sinyali ve bir araya eklemeli ses sinyal.)

![Akış Kırpıcıyı arasında yerleştirme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Akış Kırpıcıyı arasında yerleştirme*

Şimdi biz yalnızca video ve ses ve video 60 saniye 15 saniye arasında işleyecek şekilde Kırpıcıyı yapılandırın.

Video akışı Kırpıcıyı özelliklerine gidin ve başlangıç saati (15 sn) ve bitiş zamanı (60s) özelliklerini yapılandırın. Hem bizim ses ve video Kırpıcıyı her zaman aynı başlangıç ve bitiş değerleri için yapılandırıldığından emin olun, biz bu iş akışının kök yayınlanır.

![Başlangıç saati akış Kırpıcıyı özelliğinden yayımlama](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Başlangıç saati akış Kırpıcıyı özelliğinden yayımlama*

![Yayımlama özelliği iletişim kutusu için başlangıç saati](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Yayımlama özelliği iletişim kutusu için başlangıç saati*

![Bitiş saati Yayımlama özelliği iletişim kutusu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Bitiş saati Yayımlama özelliği iletişim kutusu*

Biz şimdi bizim iş akışı kökündeki inceleyin, her iki özellik düzgün görüntülenen ve yapılandırılabilir buradan olacaktır.

![Kök kullanılabilen yayımlanan Özellikler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Kök kullanılabilen yayımlanan Özellikler*

Şimdi ses Kırpıcıyı kırpma Özellikleri'ni açın ve bizim iş akışı kökünde yayımlanan özelliklerine başvuran bir ifade ile başlangıç ve bitiş zamanları yapılandırın.

Kırpma ses için başlangıç saati:

    ${ROOT_TrimmingStartTime}

ve bitiş saati:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Tamamlanmış iş akışı
![Tamamlanmış iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Tamamlanmış iş akışı*

## <a id="scripting"></a>Komut dosyalı bileşen Tanıtımı
Komut dosyası bileşenleri bizim iş akışı yürütme aşamalarında rasgele komut dosyaları yürütebilir. Çalıştırılabilir, dört farklı betikleri vardır her birinin belirli özelliklere ve iş akışı yaşam döngüsü kendi yerinde:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

Komut dosyası bileşen belgeleri daha ayrıntılı olarak her Yukarıdakilerin gider. İçinde [aşağıdaki bölümde](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), **realizeScript** komut dosyası bileşen iş akışı başlatıldığında cliplist xml kolay bir şekilde oluşturmak için kullanılır. Bu komut dosyası, yalnızca bir kez kaydının çevriminin olur Bileşen Kurulumu sırasında çağrılır.

### <a id="scripting_hello_world"></a>Bir iş akışı içinde komut dosyası: Merhaba Dünya
Bir komut dosyasıyla bileşenini Tasarımcı yüzeyine sürükleyin ve bunu (örneğin, "SetClipListXML") yeniden adlandırın.

![Bir komut dosyası bileşeni ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Bir komut dosyası bileşeni ekleme*

Komut dosyası bileşen özelliklerini incelediğinizde, dört farklı komut türlerine gösterilen, farklı bir komut dosyası her yapılandırılabilir olacaktır.

![Komut dosyası bileşeni özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komut dosyası bileşeni özellikleri*

ProcessInputScript temizleyin ve realizeScript Düzenleyicisi'ni açın. Şimdi biz yukarı ve komut dosyası başlamaya hazırsınız.

Komut dosyaları Groovy, Java ile uyumluluk korur Java platform için dinamik olarak derlenmiş bir komut dosyası dili yazılır. Aslında, çoğu Java kodu geçerli Modaya uygun koddur.

Basit hello world Modaya uygun betik bizim realizeScript bağlamında yazalım. Aşağıdaki Düzenleyicisi'nde girin:

    node.log("hello world");

Şimdi yerel test çalışması yürütün. Bu çalışma sonrasında (aracılığıyla Script bileşen Sistem sekmesinde) günlükleri özelliğini inceleyin.

![Hello world günlük çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world günlük çıktısı*

Biz içindeki komut dosyası bileşeni ya da geçerli bizim "düğümün", günlük yöntemi diyoruz düğüm nesnesi başvurur. Her bileşen, bu nedenle çıktı günlüğü verileri, sistem sekmesi üzerinden kullanılabilir özelliğine sahiptir. Bu durumda, "hello world" değişmez dize değeri çıktı. İşte bu komut gerçekte yaptıklarını üzerinde Insight ile sağlayan çok hata ayıklama aracı olarak kanıtlayabilirsiniz anlamak önemlidir.

Komut dosyası ortamımızda içinde biz de özelliklerine erişimi diğer bileşenleri vardır. Şunu deneyin:

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

Bizim günlük penceresinde bize gösterilir:

![Düğüm yollarını erişmek için bir günlük çıktısı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Düğüm yollarını erişmek için bir günlük çıktısı*

## <a id="frame_based_trim"></a>Çerçeve tabanlı kırpma multibitrate MP4 çıktı
Üreten bir iş akışından başlangıç [giriş MXF multibitrate MP4 çıktısını](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), biz artık kaynak videonun çerçeve sayar kırpma içine arayacaktır.

### <a id="frame_based_trim_start"></a>Kırpma için eklemeye başlamak için ayrıntılı genel bakış
![Kırpma için eklemeye başlamak için iş akışı](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Kırpma için eklemeye başlamak için iş akışı*

### <a id="frame_based_trim_clip_list"></a>XML küçük resim listesi kullanma
Tüm önceki iş akışı eğitimlerine bizim video giriş kaynağı olarak medya dosyası giriş bileşen kullandık. Bu belirli bir senaryo için yine de biz küçük listesi kaynak bileşen yerine kullanırsınız. Bu çalışma tercih edilen yol olmamalıdır unutmayın; Bunu yapmak için gerçek bir neden olduğunda kaynak küçük listesi yalnızca kullanın (de olduğu gibi burada yapmadan talebi aşağıda küçük listesi kırpma özelliklerinin kullanılmasına).

Bizim ortam dosyası girişi küçük listesi kaynağına geçmek için küçük liste kaynak bileşen tasarım yüzeyine sürükleyin ve iş akışı Tasarımcısı'nın küçük listesi XML düğümü küçük liste XML PIN bağlayın. Bu çıktı PIN'ler, küçük resim listesi kaynağıyla video giriş göre bizim doldurmanız gerekir. Şimdi gelen sıkıştırılmamış Video ve sıkıştırılmamış ses PIN'ler Bağlan ilgili AVC kodlayıcılar ve ses akışı ayırıcı kaynak küçük listesi. Şimdi ortam dosyası girişi kaldırın.

![Ortam dosyası girişi ile kaynak küçük listesi değiştirildi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Ortam dosyası girişi ile kaynak küçük listesi değiştirildi*

Küçük resim listesi kaynak bileşen "Küçük listesi XML" kendi giriş olarak alır. Yerel olarak test etmek için kaynak dosyası seçerken bu küçük resim listesi xml otomatik-sizin için doldurulur.

![Otomatik olarak doldurulmuş küçük listesi XML özelliği](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Otomatik olarak doldurulmuş küçük listesi XML özelliği*

Xml için biraz daha yakından bakarak bu nasıl gibi görünüyor.

![Küçük resim listesi iletişim kutusunda Düzenle](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Küçük resim listesi iletişim kutusunda Düzenle*

Bu küçük resim listesi xml yeteneklerini ancak yansıtmaz. Her iki video ve ses kaynak altında böyle bir "Kırpma" öğesi eklemek için sahip olduğumuz bir seçenek verilmiştir:

![Kırpma öğesi küçük listesine ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Kırpma öğesi küçük listesine ekleme*

Bu gibi küçük liste xml değiştirin ve yerel testi gerçekleştirmek, video görürsünüz doğru edilmiş videoda 10 ile 20 saniye arasında kırpılır.

Yerel çalıştırma ancak bunu ne olur aykırı, bu çok aynı cliplist xml Azure Media Services çalıştıran bir iş akışında uygulandığında aynı etkiye sahip olmaz. Azure Premium Kodlayıcı başladığında cliplist xml yeniden kodlama işinin verilen giriş dosyasını temel alarak her zaman oluşturulur. Bu, biz xml yapmak herhangi bir değişiklik ne yazık ki geçersiz anlamına gelir.

Bir kodlama iş başlatıldığında silmeden cliplist xml sayaç için biz bunu anında yalnızca bizim iş akışı başladıktan sonra yeniden oluşturabilirsiniz. Bu tür özel eylemler bir "komut dosyası Component" adlı aracılığıyla alınabilir. Daha fazla bilgi için bkz: [Script bileşeni Tanıtımı](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Bir komut dosyasıyla bileşenini Tasarımcı yüzeyine sürükleyin ve "SetClipListXML" yeniden adlandırın.

![Bir komut dosyası bileşeni ekleme](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Bir komut dosyası bileşeni ekleme*

Komut dosyası bileşen özelliklerini incelediğinizde, dört farklı komut türlerine gösterilen, farklı bir komut dosyası her yapılandırılabilir olacaktır.

![Komut dosyası bileşeni özellikleri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komut dosyası bileşeni özellikleri*

### <a id="frame_based_trim_modify_clip_list"></a>Komut dosyası bir bileşenin küçük listesini değiştirme
İş akışı başlatma sırasında oluşturulan cliplist xml yeniden yazma önce biz cliplist xml özellik ve içeriğini erişiminiz olması gerekir. Biz bunu şu şekilde yapabilirsiniz:

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Günlüğe kaydedilmesini gelen küçük resim listesi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Günlüğe kaydedilmesini gelen küçük resim listesi*

Öncelikle hangi noktası video kırpma istiyoruz hangi noktasından belirlemek için bir yol gerekir. Bu iş akışının daha az teknik kullanıcıya kullanışlı hale getirmek grafik alanının kök dizinine iki özellik yayımlayın. Bunu yapmak için tasarımcı yüzeyine sağ tıklayın ve "Özellik Ekle" seçin:

* İlk özellik: türü "ClippingTimeStart": "zaman kodu"
* İkinci özelliği: türü "ClippingTimeEnd": "zaman kodu"

![Özellik iletişim için kırpma başlangıç saati ekleyin](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Özellik iletişim için kırpma başlangıç saati ekleyin*

![İş akışı kökünde zaman özellik kırpma yayımlanan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*İş akışı kökünde zaman özellik kırpma yayımlanan*

Her iki özellik için uygun bir değere yapılandırın:

![Kırpma Başlangıç yapılandırmak ve Özellikler bitiş](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Kırpma Başlangıç yapılandırmak ve Özellikler bitiş*

Şimdi, bizim komut dosyası içinden Biz bu gibi her iki özellik erişebilirsiniz:

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Başlangıç ve bitiş kırpma, gösteren Günlüğü penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Başlangıç ve bitiş kırpma, gösteren Günlüğü penceresi*

Şimdi zaman kodu dizeleri daha kullanışlı form, basit bir normal ifade kullanarak kullanılacak ayrıştırılamıyor:

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

![Ayrıştırılmış zaman kodu çıkış Günlüğü penceresi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Ayrıştırılmış zaman kodu çıkış Günlüğü penceresi*

Bu bilgilerle elinizdeki, biz şimdi cliplist xml film istenen çerçeve doğru kırpma için başlangıç ve bitiş zamanları yansıtacak şekilde değiştirebilirsiniz.

![Kırpma öğeler eklemek için komut dosyası kodu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kırpma öğeler eklemek için komut dosyası kodu*

Bu normal dize düzenleme işlemleri gerçekleştirilir. Sonuçta elde edilen değiştirilmiş küçük listesi xml geri clipListXML özelliğine iş akışı kökünde "setProperty" yöntemle yazılır. Başka bir test çalıştırdıktan sonra Günlüğü penceresi bize göstermeniz:

![Sonuçta elde edilen küçük listesi günlüğü](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Sonuçta elde edilen küçük listesi günlüğü*

Nasıl video ve ses akışları kırpılmış görmek için bir test çalıştırması yapın. Kesme noktaları için farklı değerlere sahip birden fazla test çalıştırması gerçekleştirirsiniz gibi bu hesaba ancak alınmayacak olduğunu fark edeceksiniz! Bunun nedeni, Azure çalışma zamanı aksine Tasarımcı her çalıştırılışında cliplist xml kılmaz ' dir. İlk kez ve çıkış noktaları, ayarladığınız yalnızca, tüm diğer durumlarda, bizim koruma yan tümcesi dönüştürmek xml neden olacak bu anlamına gelir (varsa (clipListXML.indexOf ("<trim>") -1 ==)) iş akışı başka bir kesim öğe ekleme engeller olduğunda zaten var. mevcut bir.

Bizim iş akışı yerel olarak test etmek uygun hale getirmek için en iyi biz kırpma öğesi zaten mevcut değilse inceler bazı ev tutma kodu ekleyin. Bu durumda, biz xml yeni değerlerle değiştirerek devam etmeden önce kaldırabilirsiniz. Düz dize işlemeleri kullanmak yerine gerçek xml nesne modeli aracılığıyla ayrıştırma bunu büyük olasılıkla daha güvenli olacaktır.

Bu tür kodu ancak ekleyebilmeniz için önce biz ilk bizim betik başlangıcında içeri aktarma deyimlerini sayısı eklemeniz gerekir:

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

Bundan sonra gerekli temizleme kod ekleyebilirsiniz:

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

Bu kod, kırpma öğeleri cliplist xml eklediğimiz noktasından hemen yukarıda gider.

Bu noktada, biz çalıştırabilir ve her zamankinden uygulanan değişiklikler yaparken istediğiniz kadar süresi gibi bizim iş akışı değiştirebilirsiniz.    

### <a id="frame_based_trim_clippingenabled_prop"></a>ClippingEnabled kolaylık özellik ekleme
Şimdi, her zaman gerçekleşecek şekilde kırpma istemeyebilirsiniz gibi bizim iş akışı kesme / kırpma etkinleştirmek istiyoruz olup olmadığını belirten uygun bir Boole bayrağı ekleyerek Son'u tıklatın.

Gibi daha önce kök bizim iş akışının "ClippingEnabled" adlı yeni bir özellik türü "BOOLEAN değerini" yayımlayın.

![Kırpma etkinleştirmek için bir özellik yayımlanan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Kırpma etkinleştirmek için bir özellik yayımlanan*

İle basit koruma yan tümcesi biz kırpma gerekiyorsa denetleyebilir ve bizim küçük listesi şekilde veya değiştirilmesi gerekip gerekmediğine karar verebilirsiniz.

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <a id="code"></a>Tam kod
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


## <a name="also-see"></a>Ayrıca bkz.
[Premium Azure Media Services kodlama Tanıtımı](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Premium Azure Media Services kodlama kullanma](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Azure medya hizmeti ile isteğe bağlı içerik kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)

[Media Encoder Premium İş Akışı Biçimleri ve Kod Çözücüleri](media-services-premium-workflow-encoder-formats.md)

[Örnek iş akışı dosyaları](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Gezgini aracı](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
