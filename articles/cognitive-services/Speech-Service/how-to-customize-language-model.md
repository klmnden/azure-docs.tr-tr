---
title: 'Öğretici: Konuşma hizmeti sayesinde bir dil modeli oluşturma'
titlesuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti ile bir dil modeli oluşturmayı öğrenin. Bu özel dil modeli uygulamanıza sesli etkileşim eklemek için mevcut durumu resim konuşma modelleri Microsoft ile birlikte kullanın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 8276b86df2dc1bc90fc07da262aa0979f7562619
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60996083"
---
# <a name="tutorial-create-a-custom-language-model"></a>Öğretici: Özel dil modeli oluşturma

Bu belgede, özel bir dil modeli oluşturacaksınız. Ardından bu özel dil modelini Microsoft'un son teknoloji konuşma modelleriyle birlikte kullanarak uygulamanıza sesli etkileşim ekleyeceksiniz.

Bu belgede aşağıdakileri nasıl gerçekleştireceğiniz açıklanmaktadır:
> [!div class="checklist"]
> * Verileri hazırlama
> * Dil veri kümesini içeri aktarma
> * Özel dil modelini oluşturma

Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/try/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Bilişsel Hizmetler Abonelikler](https://customspeech.ai/Subscriptions) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

**Var olan aboneliğe bağlan** düğmesine tıklayarak Azure portalda oluşturulmuş olan bir Konuşma Hizmetleri aboneliğine bağlanabilirsiniz.

Konuşma Tanıma Hizmetleri aboneliği oluşturma hakkında bilgi için [Kullanmaya başlama](get-started.md) sayfasına bakın.

## <a name="prepare-the-data"></a>Verileri hazırlama

Uygulamanıza özel bir dil modeli oluşturmak için sisteme bir dizi örnek konuşma eklemeniz gerekir. Örneğin:

*   "The patient has had urticaria for the past week." (Hasta geçen hafta kurdeşen döktü.)
*   "The patient had a well-healed herniorrhaphy scar." (Hastanın fıtık ameliyatı yarası güzel bir şekilde iyileşmiş.)

Cümlelerin tam veya dilbilgisi açısından doğru olması gerekmez ama bu cümlelerin sistemin dağıtımda karşılaşması beklenen konuşma girişini tam olarak yansıtması gerekir. Bu örnekler kullanıcıların uygulamanızda gerçekleştireceği görevlerin stilini ve içeriğini yansıtmalıdır.

Dil modeli verileri UTF-8 BOM ile yazılmalıdır. Metin dosyasının her satırında bir örnek (cümle, konuşma veya sorgu) bulunmalıdır.

Bazı terimlerin ağırlığının (öneminin) daha fazla olmasını isterseniz, verilerinize o terimleri içeren birden fazla konuşma ekleyebilirsiniz.

Dil verilerinin ana gereksinimleri aşağıdaki tabloda özetlenmiştir.

| Özellik | Değer |
|----------|-------|
| Metin kodlaması | UTF-8 BOM|
| Satır başına konuşma sayısı | 1 |
| En büyük dosya boyutu | 1,5 GB |
| Açıklamalar | Karakterleri dörtten fazla kez tekrarlamaktan kaçının; örneğin, 'aaaaa'|
| Açıklamalar | "\t" gibi özel karakterler veya [Unicode karakter tablosunda](https://www.utf8-chartable.de/) U+00A1'in üzerindeki UTF-8 karakterleri kullanılamaz|
| Açıklamalar | Bir URI'yi telaffuz etmenin tek bir yöntemi olmadığından URI'ler de reddedilir|

Metin içeri aktarıldığında sistem tarafından işlenebilmesi için normalleştirilmiş metin haline getirilir. Ancak veriler yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Dil verilerinizi hazırlarken uygun dili belirlemek için [transkripsiyon yönergelerine](prepare-transcription.md) bakın.

## <a name="language-support"></a>Dil desteği

Özel [Konuşmayı Metne Dönüştürme](language-support.md#text-to-speech) dil modellerinde **desteklenen dillerin** tam listesine bakın.



## <a name="import-the-language-data-set"></a>Dil veri kümesini içeri aktarma

**Language Datasets** (Dil Veri Kümeleri) satırındaki **Import** (İçeri Aktar) düğmesini seçtiğinizde sitede yeni veri kümesi yükleme sayfası açılır.

Kendi oluşturduğunuz dil veri kümesini içeri aktarmaya hazır olduğunuzda [Konuşma Tanıma Hizmetleri portalında](https://customspeech.ai) oturum açın. İlk olarak, üst şeritteki **Custom Speech** (Özel Konuşma) açılan menüsünü seçin. Ardından **Adaptation Data** (Uyarlama Verileri) öğesini seçin. Konuşma Tanıma Hizmetleri'ne ilk kez veri yüklemek istediğinizde **Datasets** (Veri Kümeleri) adlı boş bir tablo görürsünüz.

Yeni veri kümesini içeri aktarmak için, **Language Datasets** (Dil Veri Kümeleri) satırındaki **Import** (İçeri Aktar) düğmesini seçin. Site, yeni veri kümesinin karşıya yüklenmesi için bir sayfa görüntüler. **Name** (Ad) ve **Description** (Açıklama) alanlarına ileride veri kümesini tanımlamanıza yardımcı olacak bilgiler girin ve yerel ayarı seçin.

Ardından **Choose File** (Dosya Seç) düğmesini kullanarak dil verileri metin dosyasını bulun. Bundan sonra **Import** (İçeri Aktar) öğesine tıklayın; veri kümesi karşıya yüklenecektir. Veri kümesinin boyutuna bağlı olarak içeri aktarma işlemi birkaç dakika sürebilir.

![Deneme](media/stt/speech-language-datasets-import.png)

İçeri aktarma tamamlandıktan sonra, dil verilerinde dil verileri kümenize karşılık gelen bir girdi olacaktır. Bu kümeye benzersiz tanımlayıcı (GUID) atanmış olduğuna dikkat edin. Ayrıca verilerin geçerli durumunu gösteren bir durum alanı da bulunur. Veriler işlenmek üzere kuyruğa alınmış olduğunda durum **Waiting** (Beklemede), doğrulama sürecinde **Processing** (İşleniyor), kullanıma hazır olduğunda ise **Complete** (Tamamlandı) olur. Veri doğrulama, dosyadaki metinler üzerinde bir dizi denetim gerçekleştirir. Verilerde biraz metin normalleştirmesi de yapar.

Durum **Complete** (Tamamlandı) olduğunda **View Report** (Raporu Görüntüle) öğesini seçerek dil verileri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte, iki örnek yanlış karakterler nedeniyle doğrulamadan geçememiştir. (Bu veri kümesinde, ilk satırın iki sekme karakteri, ikinci satırın ASCII yazdırılabilir karakter kümesinde yer almayan çeşitli karakterler vardı ve üçüncü satır da boştu).

![Deneme](media/stt/speech-language-datasets-report.png)

Durumu **Complete** (Tamamlandı) olan dil veri kümeleri özel bir dil modeli oluşturmak için kullanılabilir.

![Deneme](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Özel dil modeli oluşturma

Dil verileriniz hazır duruma geldikten sonra özel dil modeli oluşturma işlemini başlatmak için **Menu** (Menü) açılan menüsünde **Language Models** (Dil Modelleri) öğesini seçin. Bu sayfada, mevcut özel dil modellerinizi içeren **Language Models** (Dil Modelleri) adlı bir tablo vardır. Henüz hiç özel dil modeli oluşturmadıysanız tablo boş olacaktır. Geçerli yerel ayar, tabloda ilgili veri girişinin yanında gösterilir.

Eylem gerçekleştirilmeden önce uygun dil ayarının seçilmesi gerekir. Geçerli yerel ayar tüm veri, model ve dağıtım sayfalarında tablo başlığında belirtilir. Yerel ayarı değiştirmek için, tablo başlığının altında yer alan **Change Locale** (Yerel Ayarı Değiştir) düğmesini seçin.  Bu düğme size yerel ayar onay sayfasına götürür. Tabloya dönmek için **OK** (Tamam) düğmesini seçin.

Create Language Model (Dil Modeli Oluştur) sayfasında kullanılan veri kümesi gibi bu modelle ilgili bilgileri izlemenize yardımcı olacak **Name** (Ad) ve **Description** (Açıklama) bilgilerini girin. Ardından açılan menüden **Base Language Model** (Temel Dil Modeli) öğesini seçin. Bu model, özelleştirme işlemlerinizin başlangıç noktasıdır.

İki temel dil modelinden birini seçebilirsiniz. Search and Dictation modeli; komutlar, arama sorguları veya dikte gibi bir uygulamaya yönlendirilen konuşmalar için uygundur. Conversational modeli, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezlerinde veya toplantılarda kullanılır. "Universal" adlı yeni bir model de genel kullanıma sunulmuştur. Universal, tüm senaryoları kapsamayı hedeflemektedir ve ileride Search and Dictation ile Conversational modellerinin yerini alacaktır.

Aşağıdaki örnekte gösterildiği gibi, temel dil modelini belirledikten sonra **Dil Verileri** açılan menüsünü kullanarak özelleştirme için kullanmak istediğiniz dil veri kümesini seçin.

![Deneme](media/stt/speech-language-models-create2.png)

Akustik model oluşturma adımında olduğu gibi işleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin çevrimdışı testini yapabilirsiniz. Model değerlendirme için akustik veri kümesi gerekir.

Dil modelinizin çevrimdışı testini yapmak için **Çevrimdışı Test** öğesinin yanındaki onay kutusunu işaretleyin. Ardından açılan menünden bir akustik model seçin. Henüz bir özel akustik model oluşturmadıysanız menüde yalnızca Microsoft temel akustik modelleri gösterilir. Konuşmaya dayalı LM temel modeli seçtiyseniz burada konuşmaya dayalı AM kullanmanız gerekir. Arama ve dikte LM modeli kullanıyorsanız arama ve dikte AM modeli seçmeniz gerekir.

Son olarak değerlendirmeyi yapmak için kullanmak istediğiniz akustik veri kümesini seçin.

İşlemi başlatmaya hazır olduğunuzda **Oluştur**'u seçin. Bundan sonra dil modelleri tablosunu göreceksiniz. Tabloda bu modele karşılık gelen yeni bir giriş olacaktır. Durum alanı, modelin durumunu gösterir ve **Waiting** (Beklemede), **Processing** (İşleniyor) ve **Complete** (Tamamlandı) gibi farklı aşamaları yansıtır.

Model **Complete** (Tamamlandı) durumuna ulaştıktan sonra bir uç noktaya dağıtılabilir. **View Result** (Sonucu Görüntüle) öğesini seçtiğinizde, çevrimdışı testin (testi yaptıysanız) sonuçları gösterilir.

Herhangi bir noktada Modelin **Name** (Ad) veya **Description** (Açıklama) bilgisini değiştirmek isterseniz dil modelleri tablosunun ilgili satırındaki **Edit** (Düzenle) bağlantısını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# dilinde konuşmayı algılama](quickstart-csharp-dotnet-windows.md)
- [Git örnek verileri](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
