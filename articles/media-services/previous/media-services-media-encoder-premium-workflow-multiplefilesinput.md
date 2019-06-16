---
title: Birden çok giriş dosyaları ve bileşen özellikleri ile Premium Kodlayıcı - Azure | Microsoft Docs
description: Bu konu setRuntimeProperties birden fazla giriş dosyası kullanın ve Media Encoder Premium iş akışı medya işlemciye özel veri aktarmak için nasıl kullanılacağını açıklar.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: 608ca4bc3b58dd3c718d6239f90260154d2f6c3a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465550"
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Birden fazla giriş dosyaları ve bileşen özellikleri, Premium Kodlayıcı ile kullanma
## <a name="overview"></a>Genel Bakış
İçinde gerekebilir bileşen özelliklerinde özelleştirme senaryosu vardır, küçük listesi XML içeriğini belirtin veya bir görevle gönderdiğinizde, birden fazla giriş dosyası göndermek **Media Encoder Premium iş akışı** Medya işleyicisi. Bazı örnekler şunlardır:

* Metin, görüntü ve metin değeri (örneğin, geçerli tarihi), her giriş video için çalışma zamanında ayarlama planlamanızda.
* (Bir veya birden çok kaynak dosyalarını içeren veya içermeyen kırpmayı vb. belirtmek için.) küçük listesi XML özelleştirme.
* Video kodlanmış sırada bir logo resmi giriş videosu planlamanızda.
* Çoklu ses dili kodlaması.

İzin vermek için **Media Encoder Premium iş akışı** bilmeniz, görev oluşturabilir veya birden fazla giriş dosyası gönderdiğinizde iş akışında bazı özellikler değiştirmekte olduğunuz, içeren bir yapılandırma dizesi kullanmak zorunda  **setRuntimeProperties** ve/veya **transcodeSource**. Bu konuda bunların nasıl kullanılacağı açıklanmaktadır.

## <a name="configuration-string-syntax"></a>Yapılandırma dizesi söz dizimi
Kodlama görevi ayarlamak için yapılandırma dizesi şuna benzer bir XML belgesi kullanır:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

Aşağıdaki C# bir dosyadan XML yapılandırması okuyan kod doğrudan video dosya adıyla güncelleştirin ve göreve bir işlemle geçirir:

```csharp
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass to it the name of the 
// processor to use for the specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with the encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify the input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset to contain the results of the job. 
// This output is specified as AssetCreationOptions.None, which 
// means the output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Bileşen özelliklerini özelleştirme
### <a name="property-with-a-simple-value"></a>Basit bir değere sahip özellik
Bazı durumlarda, Media Encoder Premium iş akışı tarafından yürütülecek geçiyor iş akışı dosyası ile birlikte bileşen özelliği özelleştirmek kullanışlıdır.

Bir iş akışı, yer paylaşımları metni videolarınızı tasarlanmış ve metin (örneğin, geçerli tarihi) çalışma zamanında ayarlanmış olması beklenir varsayalım. Metin kodlama görevden katmana bileşenin metin özelliğinin yeni değeri olarak ayarlanacak göndererek bunu yapabilirsiniz. Bu mekanizma, bir bileşen (örneğin konum veya katmana rengini, bit hızı AVC Kodlayıcı, vb.) iş akışı, diğer özelliklerini değiştirmek için kullanabilirsiniz.

**setRuntimeProperties** iş akışının bileşenlerini özelliğinde geçersiz kılmak için kullanılır.

Örnek:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>XML değeri özelliği
XML değeri beklediği özellik ayarlamak için kullanarak Yalıt `<![CDATA[ and ]]>`.

Örnek:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Bir satır başı dönüş hemen sonrasına değil yerleştirdiğinizden emin olun `<![CDATA[`.

### <a name="propertypath-value"></a>propertyPath değerini
Önceki örneklerde olduğu propertyPath "/ medya dosya giriş/filename" veya "/ inactiveTimeout" veya "clipListXml".
Bu, genel olarak, bir bileşenin adını, sonra özelliğin adı olur. Yolu gibi daha fazla veya daha az düzeyde olabilir "/ primarySourceFile" (özellik iş akışı kökünde olduğundan) veya "/ Video işleme/grafik katmana/opaklık" (bir grupta yer paylaşımı olduğu için).    

Yol ve özellik adını kontrol etmek için hemen her bir özellik olan eylem düğmesini kullanın. Bu eylem düğmesine tıklayıp seçin **Düzenle**. Bu özelliğin ve hemen üstündeki ad alanı gerçek adı gösterir.

![Eylem/Düzenle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Özellik](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Birden fazla giriş dosyası
Her görev için gönderdiğiniz **Media Encoder Premium iş akışı** iki varlıklar gerektirir:

* İlki bir *iş akışı varlık* , bir iş akışı dosyası içerir. İş akışı dosyalarını kullanarak tasarlayabilirsiniz [iş akışı Tasarımcısı](media-services-workflow-designer.md).
* İkincisi olan bir *medya varlığını* kodlamak istediğiniz medya dosyaları içerir.

Ne zaman size göndererek birden çok medya dosyalarını **Media Encoder Premium iş akışı** Kodlayıcı, şu kısıtlamalar uygulanır:

* Tüm medya dosyalarını aynı olmalıdır *medya varlığını*. Birden çok medya varlıklarının kullanılması desteklenmiyor.
* Bu medya varlığı birincil dosya ayarlamanız gerekir (İdeal olarak, kodlayıcı işlemek için sorulan ana video dosyası budur).
* İçeren yapılandırma verilerini geçirmek gerekli olan **setRuntimeProperties** ve/veya **transcodeSource** işlemci öğesi.
  * **setRuntimeProperties** filename özelliği veya başka bir iş akışının bileşenlerini özelliğinde geçersiz kılmak için kullanılır.
  * **transcodeSource** küçük listesi XML içeriği belirtmek için kullanılır.

İş akışı bağlantıları:

* Bir veya birden çok medya dosyası girişini bileşenleri kullanır ve kullanmayı planlıyorsanız **setRuntimeProperties** dosya adını belirtin, ardından birincil dosya bileşen PIN kendisine bağlanmayan. Birincil dosya nesnesi ve medya dosyası girişlere arasında bağlantı olduğundan emin olun.
* Küçük resim listesi XML ve bir medya kaynağı bileşen kullanmayı tercih ederseniz, ardından, her ikisi de birbirine bağlayabilirsiniz.

![Medya dosyası girişini birincil kaynak dosyasından bağlantı yok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*SetRuntimeProperties filename özelliği ayarlamak için kullandığınız medya dosyası girişini bileşenleri için birincil dosyasından alınan bağlantı yoktur.*

![Kaynak listesi kırpmak için XML küçük listeden bağlantı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Küçük resim listesi XML medya kaynağına bağlanmak ve transcodeSource kullanın.*

### <a name="clip-list-xml-customization"></a>Küçük resim listesi XML özelleştirmesi
Kullanarak iş akışı çalışma zamanı en küçük listesi XML belirtebilirsiniz **transcodeSource** yapılandırmada XML dizesi. Bu iş akışında medya kaynağı bileşene bağlı küçük listesi XML PIN gerektirir.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

/PrimarySourceFile 'İfadeleri' kullanarak çıkış dosyalarının adı için bu özelliği kullanmak için belirtmek istediğiniz sonra bir özellik olarak küçük listesi XML geçirme öneririz *sonra* küçük zorunluluğundan /primarySourceFile özelliği Liste /primarySourceFile ayarı tarafından geçersiz kılınmış.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Ek çerçeve doğru kırpma ile:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a>Örnek 1: Görüntünün üzerine video yer paylaşımı

### <a name="presentation"></a>Sunum
Video kodlanmış sırada video girişteki bir logo resmi kaplama istediğiniz örneği göz önünde bulundurun. Bu örnekte, giriş videosunun "Microsoft_HoloLens_Possibilities_816p24.mp4" olarak adlandırılır ve logo "logo.png" olarak adlandırılır. Aşağıdaki adımları gerçekleştirmeniz gerekir:

* Bir iş akışı ile iş akışı dosyası oluşturmanız (aşağıdaki örneğe bakın).
* İki dosya içeren bir ortam varlık, oluşturun: Birincil dosya ve MyLogo.png MyInputVideo.mp4.
* Yukarıdaki giriş varlıklar ile Media Encoder Premium iş akışı medya işlemcisi bir görev gönderin ve aşağıdaki yapılandırma dizesi belirtin.

Yapılandırma:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Yukarıdaki örnekte, görüntü dosyasının adı, medya dosyası girişini bileşen ve primarySourceFile özelliği gönderilir. Logo dosyası adını grafik katmana bileşene bağlı başka bir medya dosyası girişini gönderilir.

> [!NOTE]
> Video dosyası adı primarySourceFile özelliğine gönderilir. İfadeler, örneğin kullanarak doğru çıkış dosyası adı oluşturmak için iş akışı içinde bu özelliği kullanmak için neden olmasıdır.

### <a name="step-by-step-workflow-creation"></a>Adım adım iş akışı oluşturma
İki dosyayı girdi olarak alır. bir iş akışı oluşturmak için adımlar şunlardır: video ve görüntü. Bu resim videonun üzerinde bulunacaktır.

Açık **iş akışı Tasarımcısı** seçip **dosya** > **yeni çalışma alanı** > **dönüştürme şema**.

Yeni iş akışı üç öğeleri gösterir:

* Birincil kaynak dosyası
* Küçük resim listesi XML'i
* Çıkış dosyası/varlık  

![Yeni kodlama iş akışı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Yeni kodlama iş akışı*

Giriş medya dosyasını kabul etmek için bir medya dosyası giriş bileşeni eklemeye başlayın. İş akışına bir bileşen eklemek için depo arama kutusuna aramanız ve istenen girişi Tasarımcı bölmesine sürükleyin.

Ardından, iş akışınızı tasarlamak için kullanılacak video dosyası ekleyin. Bunu yapmak için arka plan bölmesinde iş akışı Tasarımcısı'a tıklayın ve sağ özellik bölmesi birincil kaynak dosyası özelliği arayın. Klasör simgesine tıklayın ve uygun video dosyasını seçin.

![Birincil dosya kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Birincil dosya kaynağı*

Ardından, medya dosyası girişini bileşeni video dosyası belirtin.   

![Ortam giriş kaynağı dosyası](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Ortam giriş kaynağı dosyası*

Bu tamamlandıktan hemen sonra medya dosyası girişini bileşeni dosyasını inceleyin ve bu inceledi dosya yansıtmak için çıkış sabitleyicileri doldurmak.

Sonraki adım, bir "Video veri türü Rec.709 için renk alanını belirlemek için Updater"dır eklemektir. Bir "Video biçimi veri düzeni/Düzen türü ayarlanan Dönüştürücüsü" Ekle yapılandırılabilir düzlem =. Video akışı bu katmana bileşen kaynağı olarak uygulanabilecek bir biçime dönüştürür.

![Video veri türü güncelleştirici ve biçim dönüştürücü](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video veri türü güncelleştirici ve biçim dönüştürücü*

![Düzen türünü yapılandırılabilir düzlem =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Düzen yapılandırılabilir düzlem türüdür*

Ardından, bir Video yer paylaşımı bileşen eklemek ve medya dosyası giriş (sıkıştırılmamış) video PIN'i (sıkıştırılmamış) video PIN bağlanın.

Başka bir medya dosyası (logo dosyası yüklemek için) girişi, Ekle bu bileşene tıklayın ve "Medya dosyası girişi logosu için" yeniden adlandırın ve dosya özelliği (örneğin bir .png dosyası) bir görüntü seçin. Sıkıştırılmamış görüntüyü PIN'in katmana sıkıştırılmamış görüntü Sabitle bağlanın.

![Katmana bileşeni ve görüntü dosya kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Katmana bileşeni ve görüntü dosya kaynağı*

Video logosunu konumunu değiştirmek istiyorsanız (örneğin, video, sol üst köşesindeki dışına yüzde 10 konumlandırın isteyebileceğiniz), "El ile giriş" onay kutusunu temizleyin. Logo dosyası katman bileşenine sağlamak üzere bir medya dosyası girişini kullanarak çünkü bunu yapabilirsiniz.

![Yer paylaşımı konumu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Yer paylaşımı konumu*

H.264 video akışına kodlayın, AVC Video Kodlayıcısı ve AAC Kodlayıcı bileşenleri Tasarımcı yüzeyine ekleyin. PIN bağlanın.
AAC Kodlayıcısı kurma ayarlayın ve ses biçimi dönüştürme/önayarını seçin: 2.0 (L, R).

![Ses ve Video kodlayıcılar](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Ses ve Video kodlayıcılar*

Şimdi ekleyin **ISO Mpeg-4 çoğaltıcı** ve **dosya çıktısı** bileşenleri ve PIN'leri gösterildiği bağlanın.

![Çoğaltıcı MP4 ve dosya çıktısı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*Çoğaltıcı MP4 ve dosya çıktısı*

Çıkış dosyası adını ayarlamanız gerekir. Tıklayın **dosya çıktısı** bileşeni ve ifade için dosyayı düzenleme:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Dosya çıkış adı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Dosya çıkış adı*

Yerel olarak doğru şekilde çalıştığını denetlemek için iş akışı çalıştırabilirsiniz.

Tamamlandığında, Azure Media Services'da çalıştırabilirsiniz.

İlk olarak, Azure Media Services ile iki dosyada bir varlığı hazırlama: video dosyası ve logo. .NET veya REST API'yi kullanarak bunu yapabilirsiniz. Ayrıca Azure portalını kullanarak bunu yapabilirsiniz veya [Azure Media Services Gezgin](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Bu öğreticide, AMSE ile varlıkları yönetme işlemini göstermektedir. Dosya için bir varlık eklemek için iki yolu vardır:

* Bir yerel klasör oluşturun, iki dosya içine kopyalayın ve sürükleyip klasöre **varlık** sekmesi.
* Bir varlık olarak video dosyası yükleyin, varlık bilgilerini görüntülemek, dosyaları sekmesine gidin ve ek bir dosya (logosu) karşıya yükleyin.

> [!NOTE]
> Birincil dosya varlığı (ana video dosyası) ayarladığınızdan emin olun.

![AMSE varlık dosyaları](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*AMSE varlık dosyaları*

Varlık ve Premium Kodlayıcı ile kodlamak seçin. İş akışını karşıya yüklemek ve onu seçin.

İşlemci veri iletmek için düğmeye tıklayın ve çalışma zamanı özellikleri ayarlamak için aşağıdaki XML'i ekleyin:

![AMSE, Premium kodlayıcı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*AMSE, Premium kodlayıcı*

Sonra aşağıdaki XML verileri yapıştırın. Medya dosyası girişini ve primarySourceFile için video dosyası adını belirtmeniz gerekir. Çok logosu için dosya adı belirtin.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Oluşturun ve görevi çalıştırmak için .NET SDK'sı kullanıyorsanız, bu XML verileri yapılandırma dizesi olarak geçirilecek vardır.

```csharp
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

İş tamamlandıktan sonra çıktı varlığına MP4 dosyayı katmana görüntüler!

![Video yer paylaşımı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Video yer paylaşımı*

Örnek iş akışı'ndan indirebileceğiniz [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Örnek 2: Çoklu ses dili kodlama

Kodlama iş akışı kullanılabilir çoklu ses dili örneği [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Bu klasör, birden çok ses parçaları çoklu MP4 dosyaları varlıkla MXF dosyasına kodlamak için kullanılan bir örnek iş akışı içerir.

Bu iş akışı MXF dosyanın bir ses kaydı içerdiğini varsayar; Ek ses izleri ayrı ses dosyaları (WAV veya MP4...) geçirilmesi gerekir.

Kodlamak için bu adımları izleyin:

* Bir Media Services varlık MXF dosya ve ses dosyalarını (0 için 18 yaşında ses) ile oluşturun.
* MXF dosyanın bir birincil dosya olarak ayarlandığından emin olun.
* Bir iş ve Premium iş akışı Kodlayıcı işlemci kullanan bir görev oluşturun. Sağlanan iş akışı (MultiMP4-1080 p-19audio-v1.workflow) kullanın.
* Göreve setruntime.xml verisini geçirin (Azure Media Services Gezgini kullanıyorsanız, "xml verilerini iş akışına geçiş" düğmesini kullanın).
  * Lütfen doğru dosya adları ve dilleri etiket belirtmek üzere XML verileri güncelleştirin.
  * İş akışı ses 1 ses 18'e adlı ses bileşenlere sahiptir.
  * RFC5646 ilişkin dil etiketini desteklenir.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* Kodlanmış varlık çoklu dil ses parçalarını içerir ve bu parçaları Azure Media Player seçilebilir olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Premium Azure medya Hizmetleri kodlama ile tanışın](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Azure Media Services'da Premium Encoding kullanma](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Azure Media Services ile isteğe bağlı içerik kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Media Encoder Premium Workflow biçimleri ve kodlayıcıları](media-services-premium-workflow-encoder-formats.md)
* [Örnek iş akışı dosyaları](https://github.com/Azure/azure-media-services-samples)
* [Azure Media Services Gezgini aracı](https://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
