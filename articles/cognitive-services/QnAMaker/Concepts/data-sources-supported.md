---
title: Destekleniyor - veri kaynakları soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/26/2019
ms.author: tulasim
ms.openlocfilehash: 8fcc3ea8340a8645a1983eebb4a619904f884a19
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578637"
---
# <a name="data-sources-for-qna-maker-content"></a>Veri kaynakları için soru-cevap Oluşturucu içeriği

Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir. 

Aşağıdaki tabloda, soru-cevap Oluşturucu tarafından desteklenen içeriği ve dosya biçimlerini türlerini özetler.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL'si|SSS<br> (Düz, bölümleri veya konuları giriş sayfası ile)<br>Destek sayfaları <br> (Sorun giderme makaleleri vb. tek sayfalı nasıl yapılır makaleleri,.)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), <br>[SSS bağlantılarla](https://www.microsoft.com/software-download/faq),<br> [Konuları giriş sayfası ile ilgili SSS](https://www.microsoft.com/Licensing/servicecenter/Help/Faq.aspx)<br>[Destek makalesi](https://docs.microsoft.com/azure/cognitive-services/qnamaker/concepts/best-practices)|
|PDF / DOC|Sık sorulan sorular<br> Ürün el ile<br> Broşür<br> Kağıt<br> El İlanı İlkesi<br> Destek Kılavuzu<br> Yapılandırılmış soru-cevap<br> VS.|[QnA.doc yapılandırılmış](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Örnek Ürün Manual.pdf](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf),<br> [Örnek Yarı structured.doc](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/semi-structured.docx),<br> [Beyaz paper.pdf örneği](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/white-paper.pdf)|
|Excel|Yapılandırılmış soru-cevap dosyası<br> (RTF dahil olmak üzere HTML desteği)|[Soru-cevap FAQ.xls örneği](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Yapılandırılmış soru-cevap dosyası|[Örnek chit-chat.tsv](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Scenario_Responses_Friendly.tsv)|

## <a name="data-source-locations"></a>Veri kaynağı konumları

Çoğu veri kaynağı konumlarınız genel URL veya kimlik doğrulaması gerektirmeyen dosyaları sağlamanız gerekir. 

[SharePoint veri kaynağı konumları](../How-to/add-sharepoint-datasources.md) kimliği doğrulanmış dosyaları sağlamanıza izin verilir. SharePoint kaynakları, dosyalar, web sayfaları olmalıdır. 

Kimliği doğrulanmış bir dosyaya veya URL'ye sahip alternatif bir seçenek varsa, dosyayı yerel bilgisayarınıza kimliği doğrulanmış siteden indirmek için yerel dosya eklersiniz bilgisayar için bilgi bankasına. 

## <a name="faq-urls"></a>SSS URL'leri

Soru-cevap Oluşturucu SSS web sayfaları 3 farklı biçimlerde destekler: Düz SSS sayfaları, SSS sayfaları bağlantılarla birlikte, SSS sayfaları konuları giriş sayfası.

### <a name="plain-faq-pages"></a>Düz SSS sayfaları

Bu SSS sayfası, yanıtları aynı sayfada sorular hemen izleyin, en yaygın türüdür. 

Düz bir SSS sayfasının bir örneği aşağıda verilmiştir:

![Bilgi Bankası için düz SSS sayfası örneği](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Bağlantılar hakkında SSS sayfaları 

Bu tür bir SSS sayfasını, sorular birlikte toplanır ve aynı sayfa farklı bölümlerde veya farklı sayfalardaki olan yanıtları bağlanır.

SSS sayfası aynı sayfada bölümlerde bağlantılarla birlikte bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için bölüm bağlantı SSS sayfası örneği](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>SSS sayfaları konuları giriş sayfası

Bu tür bir SSS, her konunun farklı sayfasında, ilgili Bankalarıyla bağlantı olduğu konuları ile bir giriş sayfası vardır. Burada, soru-cevap Oluşturucu ilgili sorular ve yanıtlar ayıklamak için tüm bağlı sayfalarda gezinir.

Aşağıdaki konular giriş sayfası bağlantıları için farklı sayfalara SSS bölümlerinde sahip olduğu bir SSS sayfasında örneğidir. 

 ![Bilgi Bankası için ayrıntılı bağlantı SSS sayfası örneği](../media/qnamaker-concepts-datasources/topics-faq.png) 


### <a name="support-urls"></a>Destek URL'si

Soru-cevap Oluşturucu, belirli bir görevi gerçekleştirme, belirli bir sorunu tanılamak ve çözmek nasıl ve verilen bir işlem için en iyi uygulamalar nelerdir açıklayan web makaleleri gibi yarı yapılandırılmış Destek web sayfaları işleyebilir. Ayıklama açık bir yapı hiyerarşik başlıkları olan içeriği, en iyi şekilde çalışır.

> [!NOTE]
> Destek makaleleri için ayıklama yeni bir özelliktir ve erken aşamalarında. Bu da yapılandırılmış ve karmaşık üstbilgi/altbilgi içermeyen basit sayfaları, en iyi şekilde çalışır.

![Soru-cevap Oluşturucu ayıklama açık bir yapı hiyerarşik başlıklarla Burada sunulan yarı yapılandırılmış web sayfalarından destekler](../media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)


## <a name="pdf-doc-files"></a>PDF / DOC dosyaları

Soru-cevap Oluşturucu, yarı yapılandırılmış bir PDF veya belge dosyanın içeriğini işlemek ve Bankalarıyla dönüştürün. İyi ayıklanabileceği iyi bir dosya olduğu içerik bazı yapılandırılmış biçimde düzenlenmiştir ve iyi tanımlanmış bölümlerinde gösterilen olan biridir. Aşağıdaki bölümlerde daha fazla alt bölümleri veya alt konuları bölünebilir. Ayıklama, en iyi açık bir yapı hiyerarşik başlıkları olan belgeler üzerinde çalışır.

Soru-cevap Oluşturucu, numaralandırma, renkler vb. bölüm ve alt bölümleri ve yazı tipi boyutu, yazı tipi stili gibi görsel ipuçları dayalı dosya ilişkilerini tanımlar. Yarı yapılandırılmış PDF veya belge dosyaları kılavuzları, SSS, yönergeleri, ilkeleri, broşürler, el ilanları ve birçok diğer dosya türlerini olabilir. Bu dosyaların bazı örnek türleri aşağıda verilmiştir.

### <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

El ile bir dizin sayfası ve hiyerarşik içerik ilişkin bir örnek aşağıda verilmiştir

 ![Ürün el ile Bilgi Bankası Örneğin](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.

### <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, incelemeler ve diğer dosyaları

Açık bir yapı ve düzeni olması kaydıyla, QA çiftleri oluşturmak için ayrıca diğer türlerde belgeleri işlenebilir. Bunlar: Broşürler, yönergeleri, raporlar, teknik incelemeler, bilimsel incelemeler, ilkeleri, kitaplar, vb. Bir örnek görmek [burada](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Dizin olmadan, yarı yapılandırılmış bir belge örneği aşağıdadır:

 ![Azure Blob Depolama yarı yapılandırılmış belge](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Yapılandırılmış soru-cevap belge

Belge dosyaları içinde yapılandırılmış soru-yanıt biçimi değişen sorular biçiminde ve her satırda, her satırda bir soruya yanıt, yanıt aşağıdaki satırda, aşağıda gösterildiği gibi ardından: 

```text
Question1

Answer1

Question2

Answer2
```

Yapılandırılmış bir soru-cevap word belgesinin bir örnek aşağıda verilmiştir:

 ![Bilgi Bankası için yapılandırılmış soru-cevap belge örneği](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Yapılandırılmış *TXT*, *TSV* ve *XLS* dosyaları

Biçiminde Bankalarıyla yapılandırılmış *.txt*, *.tsv* veya *.xls* dosyalar da karşıya yüklenebilir oluşturmak veya bir Bilgi Bankası büyütmek için soru-cevap Oluşturucu.  Bu düz metin olabilir veya RTF veya HTML içeriği sahip olabilir. 

| Soru  | Yanıt  | Meta veri (1 anahtarı: 1 değeri) |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar göz ardı edilir.

Yapılandırılmış bir soru-cevap örneği aşağıdadır *.xls* dosyasıyla HTML içeriği:

 ![Yapılandırılmış bir soru-cevap, örnek bir Bilgi Bankası için excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi

Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler soru-cevap soru-cevap çiftlerini gruplandırabilir ve bunları belirli bir veri kaynağı için öznitelik oluşturucu yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta veri (1 anahtarı: 1 değeri) |          
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Bilgi Bankası'na bilgi bankanızı düzenleyerek Ekle

Bilgi Bankası doldurmak için önceden var olan içerik yoksa, soru-cevap Oluşturucu bilgi temel Bankalarıyla bilgi bankanızı düzenleyerek ekleyebilirsiniz. Bilgi bankanızı güncelleştirmeyi öğrenebilirsiniz [burada](../How-To/edit-knowledge-base.md).

## <a name="formatting-considerations"></a>Biçimlendirme konuları

URL veya içeri aktardıktan sonra Markdown dönüştürülür ve bu biçimde depolanır. Dönüştürme işlemini doğru şekilde dosyalarınızı ve URL'leri bağlantılar dönüştürme değil, sorular ve cevaplar üzerindeki düzenleyebilir **Düzenle** sayfası. 

|Biçimlendir|Amaç|
|--|--|
|`\n\n`| Yeni satır|
|`\n*`|Madde işaretinde sıralı bir listesi için|

## <a name="editing-your-knowledge-base-locally"></a>Bilgi Bankası yerel olarak düzenleme

Bilgi Bankası oluşturulduktan sonra Bilgi Bankası metinde düzenlemeleri yapın önerilir [soru-cevap Oluşturucu portalı](https://qnamaker.ai), verme ve yerel dosyaları aracılığıyla yeniden içe aktarılması yerine. Ancak, bir Bilgi Bankası yerel olarak düzenlemeniz gerekir zamanlar olabilir. 

Bilgi Bankası'ndaki dışarı **ayarları** sayfasında, ardından Microsoft Excel ile Bilgi Bankası'nı düzenleyin. Uygulama, tam olarak TSV uyumlu olduğundan, dışarı aktarılan TSV dosyasını düzenlemek için başka bir uygulama kullanmayı seçerseniz, sözdizimi hataları neden olabilir. Microsoft Excel'in TSV dosyalar genellikle tüm biçimlendirme hataları açmadığınızdan. 

Düzenlemelerinizi tamamladıktan sonra TSV dosyasını yeniden içeri aktarın **ayarları** sayfası. Bu tamamen geçerli Bilgi Bankası içeri aktarılan Bankası ile değiştirir. 

## <a name="testing-your-markdown"></a>Markdown'ınız test etme

Kullanım **[CommonMark](https://commonmark.org/help/tutorial/index.html)** Markdown'ınız doğrulamak için öğretici. Öğreticiyi sahip bir **deneyin** hızlı kopyala/yapıştır doğrulama özelliği. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir soru-cevap Oluşturucu hizmetini ayarlama](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
