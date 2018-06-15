---
title: Birden çok giriş dosyaları ve Premium Kodlayıcı - Azure ile bileşen özellikleri | Microsoft Docs
description: Bu konu setRuntimeProperties birden fazla girdi dosyası kullanın ve Medya Kodlayıcısı Premium iş akışı medya işlemciyi özel veri geçirmek için nasıl kullanılacağını açıklar.
services: media-services
documentationcenter: ''
author: xpouyat
manager: cfowler
editor: ''
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: 66aec76e5af399e1909446b8ddf7a79aa1384d52
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788903"
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Premium Kodlayıcı ile birden fazla giriş dosyaları ve bileşen özellikleri kullanma
## <a name="overview"></a>Genel Bakış
İçinde ihtiyacınız olabilecek bileşeni özellikleri özelleştirmek senaryo vardır küçük listesi XML içeriğini belirtin veya bir görev ile gönderdiğinizde birden çok giriş dosyaları göndermek **Medya Kodlayıcısı Premium iş akışı** medya işlemcisi. Bazı örnekler şunlardır:

* Video ve metin değeri (örneğin, geçerli tarihten başlayarak) her giriş video için çalışma zamanında ayarlama metin üst üste getirme.
* (Bir veya daha çok kaynak dosyaları ile veya olmadan kırpma, vb. belirtmek için.) küçük liste XML özelleştirme.
* Video kodlanmış sırada video giriş logo görüntüsü üst üste getirme.
* Birden çok ses dil kodlaması.

İzin vermek için **Medya Kodlayıcısı Premium iş akışı** bilmeniz görevi oluşturmak veya birden çok giriş dosyaları gönderdiğinizde iş akışında bazı özellikler değiştiriyorsunuz, içeren bir yapılandırma dizesi kullanmak zorunda  **setRuntimeProperties** ve/veya **transcodeSource**. Bu konu, bunların nasıl kullanılacağını açıklar.

## <a name="configuration-string-syntax"></a>Yapılandırma dizesi sözdizimi
Kodlama görevde ayarlamak için yapılandırma dizesi şuna benzer bir XML belgesi kullanır:

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

Bir dosyadan XML yapılandırması okuyan, sağa video dosya adıyla güncelleştirin ve bir iş görevi geçirir C# kod aşağıda verilmiştir:

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
### <a name="property-with-a-simple-value"></a>Basit bir değere sahip özelliği
Bazı durumlarda, Medya Kodlayıcısı Premium iş akışı tarafından yürütülecek giderek iş akışı dosyası ile birlikte bir bileşen özelliği özelleştirmek kullanışlıdır.

Bir iş akışı videolarınızı üzerinde yer paylaşımları metni tasarlanmış ve metin (örneğin, geçerli tarih) çalışma zamanında ayarlanmış olması beklenir varsayalım. Metin kodlama görevden katmana bileşeninin metin özelliği için yeni değer olarak ayarlanacak göndererek bunu yapabilirsiniz. Bu mekanizma (örneğin, konum veya katmana rengini, bit hızı AVC Kodlayıcı, vb.) iş akışında bir bileşenin diğer özelliklerini değiştirmek için kullanabilirsiniz.

**setRuntimeProperties** iş akışı bileşenlerinin özelliğinde geçersiz kılmak için kullanılır.

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
XML değeri bekler bir özelliği ayarlamak için kullanarak kapsülleyen `<![CDATA[ and ]]>`.

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
> Bir satır başı dönüş hemen sonra değil yerleştirdiğinizden emin olun `<![CDATA[`.

### <a name="propertypath-value"></a>propertyPath değeri
Önceki örneklerde propertyPath oluştu "/ medya dosya giriş/dosya adı" veya "/ inactiveTimeout" veya "clipListXml".
Bu, genel olarak, bileşen adı, ardından özelliğinin adı var. Yolun daha fazla veya daha az düzeyleri gibi olabilir "/ primarySourceFile" (özellik iş akışı kökünde olduğundan) veya "/ Video işleme/grafiği katmana/opaklık" (bir grupta yer paylaşımı olduğu için).    

Yol ve özellik adını denetlemek için hemen her özelliğin eylem düğmesi kullanın. Bu eylem düğmesini tıklatın ve seçin **Düzenle**. Bu özelliğin ve hemen üstündeki ad gerçek adı gösterir.

![Eylem/Düzenle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Özellik](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Birden çok giriş dosyaları
Her görev için gönderme **Medya Kodlayıcısı Premium iş akışı** iki varlıklar gerektirir:

* İlk sağlayıcıdır bir *iş akışı varlık* bir iş akışı dosyası içerir. İş akışı dosyalarını kullanarak tasarlayabilirsiniz [iş akışı Tasarımcısı](media-services-workflow-designer.md).
* İkincisi olan bir *medya varlık* kodlamak istediğiniz medya dosyaları içerir.

Ne zaman size göndererek birden çok medya dosyalarını **Medya Kodlayıcısı Premium iş akışı** Kodlayıcı, aşağıdaki kısıtlamalar geçerlidir:

* Tüm medya dosyaları aynı olmalıdır *medya varlık*. Birden çok medya varlıkları kullanma desteklenmiyor.
* Bu medya varlık birincil dosya ayarlamanız gerekir (İdeal olarak, kodlayıcı işlemek için sorulan ana video dosyası budur).
* İçeren yapılandırma verilerini geçirmek gerekli olan **setRuntimeProperties** ve/veya **transcodeSource** işlemci öğesi.
  * **setRuntimeProperties** filename özelliği veya başka bir iş akışı bileşenlerinin özelliğinde geçersiz kılmak için kullanılır.
  * **transcodeSource** küçük liste XML içeriği belirtmek için kullanılır.

Bağlantıları iş akışı:

* Bir veya birkaç ortam dosyası girişi bileşenleri kullanır ve kullanmayı planlıyorsanız **setRuntimeProperties** dosya adı belirtmek için daha sonra birincil dosya bileşen PIN onlara bağlamayın. Birincil dosya nesnesi ve medya dosyası Input(s) arasında bağlantı olduğundan emin olun.
* Küçük resim listesi XML ve bir medya kaynağı bileşen kullanmayı tercih ederseniz, ardından, her ikisi de birbirine bağlayabilirsiniz.

![Medya dosyası giriş birincil kaynak dosyadan bağlantı yok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Filename özelliği ayarlamak için setRuntimeProperties kullanırsanız, ortam dosyası girişi bileşenleri birincil dosyasından bağlantı yoktur.*

![Kaynak listesi küçük XML küçük listeden bağlantı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Medya kaynağı için küçük liste XML bağlanabilir ve transcodeSource kullanabilirsiniz.*

### <a name="clip-list-xml-customization"></a>Liste XML özelleştirmesi Kırp
Kullanarak iş akışı içinde çalışma zamanında küçük liste XML belirtebilirsiniz **transcodeSource** yapılandırmada XML dizesi. Bu iş akışının medya kaynağı bileşeni bağlanması için küçük liste XML PIN gerektirir.

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

/PrimarySourceFile 'İfadelerin' kullanarak çıkış dosyalarının ad vermek için bu özelliği kullanmak için belirtmek istediğiniz sonra küçük liste XML bir özellik olarak geçirme öneririz *sonra* küçük zorunda kalmamak için /primarySourceFile özelliği Liste /primarySourceFile ayarıyla geçersiz.

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

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a>Örnek 1: bir görüntü video en üstünde yer paylaşımı

### <a name="presentation"></a>Sunum
Video kodlanmış sırada logo görüntüsü video girişleri kaplama istediğiniz bir örneği göz önünde bulundurun. Bu örnekte, giriş video "Microsoft_HoloLens_Possibilities_816p24.mp4" olarak adlandırılır ve logo "logo.png" olarak adlandırılır. Aşağıdaki adımları gerçekleştirmeniz gerekir:

* Bir iş akışı varlık ile iş akışı dosyası oluşturun (aşağıdaki örneğe bakın).
* İki dosya içeren bir medya varlık, oluşturma: MyInputVideo.mp4 MyLogo.png ve birincil dosya olarak.
* Yukarıdaki giriş varlıklar Medya Kodlayıcısı Premium iş akışı medya işlemcisi bir görev gönderin ve aşağıdaki yapılandırma dizesi belirtin.

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

Yukarıdaki örnekte, video dosyası adını medya dosyası giriş bileşen ve primarySourceFile özelliği gönderilir. Logo dosyası adını grafik katmana bileşene bağlı başka bir ortam dosyası girişi gönderilir.

> [!NOTE]
> Video dosyası adı primarySourceFile özelliğine gönderilir. Bunun nedeni bu özellik iş akışı içinde ifadeler, örneğin kullanarak doğru çıktı dosyası adı oluşturmak için kullanmaktır.

### <a name="step-by-step-workflow-creation"></a>Adım adım iş akışı oluşturma
İki dosya girdi olarak alır bir iş akışı oluşturmak için adımlar şunlardır: bir video ve görüntü. Görüntünün video üstünde bulunacaktır.

Açık **iş akışı Tasarımcısı** seçip **dosya** > **yeni çalışma alanı** > **kodlamasını şeması**.

Yeni bir iş akışı üç öğeleri gösterir:

* Birincil kaynak dosyası
* Küçük resim listesi XML'i
* Çıkış dosyası/varlık  

![Yeni bir kodlama iş akışı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Yeni bir kodlama iş akışı*

Giriş medya dosyası kabul etmek için bir medya dosyası giriş bileşeni ekleyerek başlayın. Bir bileşenin iş akışına eklemek için deposu arama kutusuna aramanız ve istenen girişi Tasarımcı bölmesine sürükleyin.

Ardından, iş akışınızı tasarlamak için kullanılacak video dosyası ekleyin. Bunu yapmak için iş akışı Tasarımcısı'nda arka plan bölmesine tıklayın ve sağ taraftaki özellik bölmesinde birincil kaynak dosya özellikte arayın. Klasör simgesine tıklayın ve uygun video dosyası seçin.

![Birincil dosya kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Birincil dosya kaynağı*

Ardından, ortam dosyası girişi bileşeni video dosyası belirtin.   

![Medya dosya giriş kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Medya dosya giriş kaynağı*

Bu yapılır hemen medya dosyası giriş bileşen dosyasını inceleyin ve onu Denetlenmekte dosya yansıtacak şekilde kendi çıktı pini doldurun.

Sonraki adım, bir "Video veri türü Rec.709 için renk alanını belirlemek için güncelleştirici" eklemektir. Bir "Video biçimi, veri düzeni/düzenini türüne ayarlanır dönüştürücü" ekleme yapılandırılabilir düzlemsel =. Video akışına bu katmana bileşen kaynağı olarak gerçekleştirilecek bir biçime dönüştürür.

![Video veri türü güncelleştirici ve biçimine dönüştürücü](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video veri türü güncelleştirici ve biçimine dönüştürücü*

![Düzen türü yapılandırılabilir düzlemsel =](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Düzen yapılandırılabilir düzlemsel türüdür*

Ardından, bir Video yer paylaşımı bileşen ekleyin ve medya dosyası giriş (sıkıştırılmamış) video PIN'i (sıkıştırılmamış) video PIN bağlanın.

Başka bir ortam dosyası (logo dosyası yüklemek için) girişi, eklemek bu bileşeni tıklatın ve "Medya dosyası giriş logosu için" yeniden adlandırın ve dosya özelliği (örneğin bir .png dosyası) bir görüntü seçin. Sıkıştırılmamış görüntü PIN katmana sıkıştırılmamış görüntü Sabitle bağlayın.

![Bir katmana bileşeni ve görüntü dosyası kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Bir katmana bileşeni ve görüntü dosyası kaynağı*

Video logosunu konumunu değiştirmek istiyorsanız (örneğin, video, sol üst köşesinde dışına yüzde 10 konumlandırmak istediğiniz), "El ile giriş" onay kutusunu temizleyin. Logo dosyası katmana bileşenine sağlamak için bir medya dosyası girişi kullanıldığı için bunu yapabilirsiniz.

![Bir katmana konumu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Bir katmana konumu*

H.264 video akışına kodlayın Tasarımcı yüzeyine AVC Video Kodlayıcısı ve AAC Kodlayıcı bileşenleri ekleyin. PIN bağlayın.
AAC Kodlayıcısı kurma ayarlamak ve ses biçimi dönüştürme/hazır seçin: 2.0 (M, R).

![Ses ve görüntü Kodlayıcıları](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Ses ve görüntü Kodlayıcıları*

Şimdi ekleyin **ISO Mpeg-4 çoğaltıcı** ve **dosya çıktısı** bileşenleri ve PIN'leri gösterildiği gibi bağlanın.

![Çoğaltıcı MP4 ve dosya çıktısı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*Çoğaltıcı MP4 ve dosya çıktısı*

Çıktı dosyası adını ayarlamanız gerekir. Tıklatın **dosya çıktısı** bileşeni ve düzenleme ifade dosya için:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Dosya çıktı adı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Dosya çıktı adı*

Yerel olarak düzgün çalışıp çalışmadığını denetlemek için iş akışı çalıştırabilirsiniz.

Sona erdikten sonra Azure Media Services çalıştırabilirsiniz.

İlk olarak, bir varlıkla Azure Media Services iki dosyada hazırlayın: video dosyası ve logosu. .NET veya REST API'yi kullanarak bunu yapabilirsiniz. Ayrıca Azure portalını kullanarak bunu yapabilirsiniz veya [Azure Media Services Gezgini](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Bu öğretici AMSE ile varlıklarını yönetme gösterir. Dosya için bir varlık eklemek için iki yolu vardır:

* Yerel bir klasör oluşturun, iki dosya içinde kopyalayın ve sürükleme ve bırakma klasörüne **varlık** sekmesi.
* Bir varlık olarak video dosyası yükleyin, varlık bilgilerini görüntülemek, dosyaları sekmesine gidin ve bir ek (logosu) dosyasını karşıya yükleyin.

> [!NOTE]
> Varlık (ana video dosyası) bir birincil dosya ayarladığınızdan emin olun.

![AMSE varlık dosyaları](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*AMSE varlık dosyaları*

Varlık ve Premium Encoder ile kodlamak seçin. İş akışı karşıya yükleyin ve onu seçin.

İşlemciyi veri iletmek için düğmesini tıklatın ve çalışma zamanı özelliklerini ayarlamak için aşağıdaki XML ekleyin:

![Premium Kodlayıcı AMSE içinde](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Kodlayıcı AMSE içinde*

Aşağıdaki XML verileri yapıştırabilirsiniz. Ortam dosyası girişi ve primarySourceFile için video dosyası adını belirtmeniz gerekir. Logo dosyası adı çok belirtin.

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

Oluşturma ve görevi çalıştırmak için .NET SDK'sı kullanıyorsanız yapılandırma dizesi olarak geçirilecek bu XML verileri içeriyor.

```csharp
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

İş tamamlandığında, çıkış varlık MP4 dosyasındaki katmana görüntüler!

![Video yer paylaşımı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Video yer paylaşımı*

Örnek iş akışını yükleyebilirsiniz [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Örnek 2: Birden çok ses dil kodlaması

Kodlama workfkow bulunan birden çok ses dili örneği [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Bu klasör, birden çok ses izleri çoklu MP4 dosyaları varlıkla MXF dosyasına kodlanması için kullanılan bir örnek iş akışı içerir.

Bu iş akışı MXF dosyasının bir ses izleme içerdiğini varsayar; Ek ses izleri ayrı ses dosyaları (WAV veya MP4...) geçirilmesi gerekir.

Kodlamak için lütfen şu adımları izleyin:

* Bir Media Services varlık MXF dosyası ve ses dosyaları (0 için 18 ses dosyaları) ile oluşturun.
* MXF dosya birincil bir dosya olarak ayarlandığından emin olun.
* Bir işi ve Premium iş akışı Kodlayıcı işlemci kullanarak bir görev oluşturun. Sağlanan iş akışını (MultiMP4-1080 p-19audio-v1.workflow) kullanın.
* Görevi setruntime.xml veri iletmek (Azure Media Services Gezgini kullanırsanız, "xml veri akışına iletmek" düğmesini kullanın).
  * Lütfen doğru dosya adları ve dilleri etiket belirtmek üzere XML verileri güncelleştirin.
  * İş akışı ses 18 ses 1 adlı ses bileşenleri içerir.
  * RFC5646 dil etiketi için desteklenir.

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

* Kodlanmış varlık çoklu dil ses izleri içerir ve bu parçaları Azure Media Player seçilebilir olması gerekir.

## <a name="see-also"></a>Ayrıca bkz.
* [Premium Azure Media Services kodlama Tanıtımı](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Premium kodlama Azure Media Services ile kullanma](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Azure Media Services ile isteğe bağlı içerik kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Medya Kodlayıcısı Premium iş akışı biçimleri ve codec bileşenleri](media-services-premium-workflow-encoder-formats.md)
* [Örnek iş akışı dosyaları](https://github.com/Azure/azure-media-services-samples)
* [Azure Media Services Gezgini aracı](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
