---
title: Destekleniyor - veri kaynakları soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/25/2018
ms.author: tulasim
ms.openlocfilehash: 29e894b0666b37d32f36b016603fda408e9d2746
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161072"
---
# <a name="data-sources-for-qna-maker-content"></a>Veri kaynakları için soru-cevap Oluşturucu içeriği

Soru-cevap Oluşturucu SSS, ürün kılavuzlarını, kılavuzlar, destek belgeleri ve web sayfaları, PDF dosyaları ya da Word MS doc dosyalarını depolanan ilkeleri gibi yarı yapılandırılmış içeriği otomatik olarak soru-cevap çiftlerini ayıklar. İçeriği, yapılandırılmış soru-cevap içerik dosyalarından Bilgi Bankası'na da eklenebilir. 

Aşağıdaki tabloda, soru-cevap Oluşturucu tarafından desteklenen içeriği ve dosya biçimlerini türlerini özetler.

|Kaynak Türü|İçerik Türü| Örnekler|
|--|--|--|
|URL'si|Sık sorulan sorular (düz, bölümleri veya ile konuları giriş sayfası)|[Düz SSS](https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs), [bağlantılarla SSS](https://www.microsoft.com/software-download/faq), [konuları giriş sayfası ile ilgili SSS](https://support.microsoft.com/products/windows?os=windows-10)|
|PDF / DOC|Yapılandırılmış bir soru-cevap, vb. SSS sayfaları, ürün el ile broşürler, inceleme, El İlanı ilke ve Destek Kılavuzu.|[QnA.doc yapılandırılmış](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Bot%20Service%20Sample%20FAQ.docx), [örnek Ürün Manual.pdf](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf), [örnek yarı structured.doc](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx), [beyaz paper.pdf örneği](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-stack-wortmann-bring-the-power-of-the-public-cloud-into-your-data-center/Azure_Stack_Wortmann_Bring_the_Power_of_the_Public_Cloud_into_Your_Data_Center.pdf)|
|Excel|Yapılandırılmış soru-cevap dosyası (RTF dahil olmak üzere HTML desteği)|[Soru-cevap FAQ.xls örneği](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/QnA%20Maker%20Sample%20FAQ.xlsx)|
|TXT/TSV|Yapılandırılmış soru-cevap dosyası|[Örnek chit-chat.tsv](https://raw.githubusercontent.com/Microsoft/BotBuilder-PersonalityChat/master/CSharp/Datasets/Queries_Responses_Friendly_QnAMaker.tsv)|

## <a name="faq-urls"></a>SSS URL'leri

Soru-cevap Oluşturucu SSS web sayfaları 3 farklı formlarda destekleyebilir: düz SSS sayfaları, SSS sayfaları bağlantılarla birlikte, SSS sayfaları konuları giriş sayfası.

### <a name="plain-faq-pages"></a>Düz SSS sayfaları

Bu SSS sayfası, yanıtları aynı sayfada sorular hemen izleyin, en yaygın türüdür. 

Düz bir SSS sayfasının bir örneği aşağıda verilmiştir:

![Düz SSS sayfası](../media/qnamaker-concepts-datasources/plain-faq.png) 

 
### <a name="faq-pages-with-links"></a>Bağlantılar hakkında SSS sayfaları 

Bu tür bir SSS sayfasını, sorular birlikte toplanır ve aynı sayfa farklı bölümlerde veya farklı sayfalardaki olan yanıtları bağlanır.

SSS sayfası aynı sayfada bölümlerde bağlantılarla birlikte bir örnek aşağıda verilmiştir:

 ![Bölüm bağlantı SSS sayfasını](../media/qnamaker-concepts-datasources/sectionlink-faq.png) 


### <a name="faq-pages-with-a-topics-homepage"></a>SSS sayfaları konuları giriş sayfası

Bu tür bir SSS, her konunun farklı sayfasında, ilgili Bankalarıyla bağlantı olduğu konuları ile bir giriş sayfası vardır. Burada, soru-cevap Oluşturucu ilgili sorular ve yanıtlar ayıklamak için tüm bağlı sayfalarda gezinir.

Aşağıdaki konular giriş sayfası bağlantıları için farklı sayfalara SSS bölümlerinde sahip olduğu bir SSS sayfasında örneğidir. 

 ![Ayrıntılı bağlantı SSS sayfası](../media/qnamaker-concepts-datasources/topics-faq.png) 


## <a name="pdf-doc-files"></a>PDF / DOC dosyaları

Soru-cevap Oluşturucu, yarı yapılandırılmış bir PDF veya belge dosyanın içeriğini işlemek ve Bankalarıyla dönüştürün. İyi ayıklanabileceği iyi bir dosya olduğu içerik bazı yapılandırılmış biçimde düzenlenmiştir ve iyi tanımlanmış bölümlerinde gösterilen olan biridir. Aşağıdaki bölümlerde daha fazla alt bölümleri veya alt konuları bölünebilir. Ayıklama, en iyi açık bir yapı hiyerarşik başlıkları olan belgeler üzerinde çalışır.

Soru-cevap Oluşturucu, numaralandırma, renkler vb. bölüm ve alt bölümleri ve yazı tipi boyutu, yazı tipi stili gibi görsel ipuçları dayalı dosya ilişkilerini tanımlar. Yarı yapılandırılmış PDF veya belge dosyaları kılavuzları, SSS, yönergeleri, ilkeleri, broşürler, el ilanları ve birçok diğer dosya türlerini olabilir. Bu dosyaların bazı örnek türleri aşağıda verilmiştir.

### <a name="product-manuals"></a>Ürün kılavuzlarını

El ile genellikle bir ürünle birlikte verilen yönergeleri malzeme oluşur. Bunu ayarlamak, kullanma, korumanıza ve ürünle ilgili sorunları giderme kullanıcıya yardımcı olur. Soru-cevap Oluşturucu el ile işlediğinde, başlıklar ve başlıklar olarak sorular ve yanıtlar olarak sonraki içeriği ayıklar. Bir örnek görmek [burada](http://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

El ile bir dizin sayfası ve hiyerarşik içerik ilişkin bir örnek aşağıda verilmiştir

 ![Ürün el ile örnek](../media/qnamaker-concepts-datasources/product-manual.png) 

> [!NOTE]
> Ayıklama içeriğini ve/veya dizin sayfası ve açık bir yapı hiyerarşik başlıklara sahip bir tablosu kılavuzları en iyi şekilde çalışır.

### <a name="brochures-guidelines-papers-and-other-files"></a>Broşürler, kılavuzlar, incelemeler ve diğer dosyaları

Açık bir yapı ve düzeni olması kaydıyla, QA çiftleri oluşturmak için ayrıca diğer türlerde belgeleri işlenebilir. Bunlar: broşür yönergeleri, raporları, incelemeler, bilimsel incelemeler, ilkeleri, kitaplar, vb. beyaz. Bir örnek görmek [burada](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Dizin olmadan, yarı yapılandırılmış bir belge örneği aşağıdadır:

 ![Azure Blob Depolama yarı yapılandırılmış belge](../media/qnamaker-concepts-datasources/semi-structured-doc.png) 

### <a name="structured-qna-document"></a>Yapılandırılmış soru-cevap belge

Belge dosyaları içinde yapılandırılmış soru-yanıt biçimi değişen sorular biçiminde ve her satırda, her satırda bir soruya yanıt, yanıt aşağıdaki satırda, aşağıda gösterildiği gibi ardından: 

```
Question1

Answer1

Question2

Answer2
```

Yapılandırılmış bir soru-cevap word belgesinin bir örnek aşağıda verilmiştir:

 ![Yapılandırılmış soru-cevap belge](../media/qnamaker-concepts-datasources/structured-qna-doc.png) 

## <a name="structured-txt-tsv-and-xls-files"></a>Yapılandırılmış *TXT*, *TSV* ve *XLS* dosyaları

Biçiminde Bankalarıyla yapılandırılmış *.txt*, *.tsv* veya *.xls* dosyalar da karşıya yüklenebilir oluşturmak veya bir Bilgi Bankası büyütmek için soru-cevap Oluşturucu.  Bu düz metin olabilir veya RTF veya HTML içeriği sahip olabilir. 

| Soru  | Yanıt  | Meta Veriler                |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Kaynak dosyadaki ek sütunlar göz ardı edilir.

Yapılandırılmış bir soru-cevap örneği aşağıdadır *.xls* dosyasıyla HTML içeriği:

 ![Yapılandırılmış bir soru-cevap excel](../media/qnamaker-concepts-datasources/structured-qna-xls.png)

## <a name="structured-data-format-through-import"></a>İçeri aktarma ile yapılandırılmış veri biçimi

Bilgi Bankası içeri aktarma, var olan bir Bilgi Bankası içeriğini değiştirir. İçeri aktarma, veri kaynağı bilgilerini içeren bir yapılandırılmış .tsv dosyası gerektirir. Bu bilgiler soru-cevap soru-cevap çiftlerini gruplandırabilir ve bunları belirli bir veri kaynağı için öznitelik oluşturucu yardımcı olur.

| Soru  | Yanıt  | Kaynak| Meta Veriler                |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Düzenleme|    `Key:Value`       |

## <a name="editorially-add-to-knowledge-base"></a>Bilgi Bankası'na bilgi bankanızı düzenleyerek Ekle

Bilgi Bankası doldurmak için önceden var olan içerik yoksa, soru-cevap Oluşturucu bilgi temel Bankalarıyla bilgi bankanızı düzenleyerek ekleyebilirsiniz. Bilgi bankanızı güncelleştirmeyi öğrenebilirsiniz [burada](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir soru-cevap Oluşturucu hizmetini ayarlama](../How-To/set-up-qnamaker-service-azure.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)
