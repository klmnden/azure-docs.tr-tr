---
title: Özel Konuşma Tanıma Hizmeti ile dil modeli oluşturma - Microsoft Bilişsel Hizmetler
description: Microsoft Bilişsel Hizmetler'de Özel Konuşma Tanıma Hizmeti ile dil modeli oluşturmayı öğrenin.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 97659bf38b6d06464eee37a33e87d0c528cdcd37
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126947"
---
# <a name="tutorial-create-a-custom-language-model"></a>Öğretici: Özel dil modeli oluşturma

Bu belgede özel bir dil modeli oluşturacak ve ardından bunu Microsoft'un son teknoloji konuşma modelleriyle birlikte kullanarak uygulamanıza sesli etkileşim ekleyeceksiniz.

Bu belgede aşağıdakileri nasıl gerçekleştireceğiniz açıklanmaktadır:
> [!div class="checklist"]
> * Verileri hazırlama
> * Dil veri kümesini içeri aktarma
> * Özel dil modelini oluşturma

Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/try/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

[Cognitive Services Subscriptions](https://customspeech.ai/Subscriptions) (Bilişsel Hizmetler Abonelikleri) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

**Connect existing subscription** (Var olan aboneliğe bağlan) düğmesine tıklayarak Azure portalında oluşturulmuş olan bir Özel Konuşma Tanıma Hizmeti aboneliğine bağlanabilirsiniz.

Özel Konuşma Tanıma Hizmeti aboneliği oluşturma hakkında bilgi için [Kullanmaya Başlama](get-started.md) sayfasına bakın.

## <a name="prepare-the-data"></a>Verileri hazırlama

Uygulamanız için özel bir dil modeli oluşturmak üzere sisteme bir dizi örnek konuşma eklemeniz gerekir. Örneğin:

*   "The patient has had urticaria for the past week." (Hasta geçen hafta kurdeşen döktü.)
*   "The patient had a well-healed herniorrhaphy scar." (Hastanın fıtık ameliyatı yarası güzel bir şekilde iyileşmiş.)

Cümlelerin tam veya dilbilgisi açısından doğru olması gerekmez ve bu cümlelerin sistemin dağıtımda karşılaşması beklenen konuşma girişini tam olarak yansıtması gerekir. Bu örnekler kullanıcıların uygulamanızda gerçekleştireceği görevlerin stilini ve içeriğini yansıtmalıdır.

Dil modeli verileri UTF-8 BOM ile yazılmalıdır. Metin dosyasının her satırında bir örnek (cümle, konuşma veya sorgu) bulunmalıdır.

Bazı terimlerin ağırlığının (öneminin) daha fazla olmasını isterseniz o terimle ilgili birden fazla konuşmayı verilerinize ekleyebilirsiniz. 

Dil verilerinin ana gereksinimleri aşağıdaki tabloda özetlenmiştir.

| Özellik | Değer |
|----------|-------|
| Metin Kodlaması | UTF-8 BOM|
| Satır başına konuşma sayısı | 1 |
| En Büyük Dosya Boyutu | 1,5 GB |
| Açıklamalar | karakterleri dörtten fazla tekrarlamaktan kaçının, örneğin: "aaaaa"|
| Açıklamalar | "\t" gibi özel karakterler veya [Unicode karakter tablosunda](http://www.utf8-chartable.de/) U+00A1 üzerindeki UTF-8 karakterleri kullanılamaz|
| Açıklamalar | Bir URI'yi telaffuz etmenin tek bir yöntemi olmadığından URI'ler de reddedilir|

Metin içeri aktarıldığında sistem tarafından işlenebilmesi için normalleştirilmiş metin haline getirilir. Ancak veriler yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Dil verilerinizi hazırlarken uygun dili belirlemek için [Transkripsiyon yönergelerine](prepare-transcription.md) bakın.

## <a name="language-support"></a>Dil desteği

Özel **Konuşmayı Metne Dönüştürme** dil modellerinde aşağıdaki diller desteklenir.

[Desteklenen dillerin](supported-languages.md) tam listesine ulaşmak için tıklayın

## <a name="import-the-language-data-set"></a>Dil veri kümesini içeri aktarma

"Language Datasets" (Dil Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır.

Kendi oluşturduğunuz dil veri kümesini içeri aktarmaya hazır olduğunuzda [Konuşma Tanıma Hizmeti Portalı](https://customspeech.ai)'nda oturum açın.  Ardından üst şeritteki “Custom Speech” (Özel Konuşma) açılan menüsüne tıklayıp “Adaptation Data” (Uyarlama Verileri) öğesini seçin. Konuşma Tanıma Hizmeti'ne ilk kez veri yüklemek istediğinizde “Datasets” (Veri Kümeleri) adlı boş bir tablo görürsünüz.

Yeni veri kümesini içeri aktarmak için "Language Datasets" (Dil Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır. Name (Ad) ve Description (Açıklama) alanlarına ileride veri kümesini tanımlamanıza yardımcı olacak bilgiler girin ve yerel ayarı seçin. Ardından “Choose File” (Dosya Seç) düğmesini kullanarak dil verileri metin dosyasını bulun. Daha sonra “Import” (İçeri Aktar) öğesine tıklayarak veri kümesinin yüklenmesini sağlayın. Veri kümesinin boyutuna bağlı olarak içeri aktarma işlemi birkaç dakika sürebilir.

![Deneme](media/stt/speech-language-datasets-import.png)

İçeri aktarma işlemi tamamlandığında dil verileri tablosuna dönecek ve dil verileri kümenizi gösteren bir giriş göreceksiniz. Bu kümeye benzersiz tanıtıcı (GUID) atanmış olduğuna dikkat edin. Ayrıca verilerin geçerli durumunu gösteren bir durum alanı da bulunur. Durum işlenmek üzere kuyruğa alınmış olduğunda “Waiting” (Beklemede), doğrulama sürecinde “Processing” (İşleniyor), kullanıma hazır olduğunda ise “Complete” (Tamamlandı) olur. Veri doğrulama, dosyadaki metinler üzerinde bir dizi denetim gerçekleştirir ve veri metinlerinde normalleştirme yapar.

Durum “Complete” (Tamamlandı) olduğunda “View Report” (Raporu Görüntüle) öğesine tıklayarak dil verileri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte iki konuşma, uygun olmayan karakterlerin kullanılması nedeniyle doğrulama adımında başarısız olmuştur (bu veri kümesinde birincide satırda iki SEKME karakteri, ikincide ASCII yazdırılabilir karakter kümesinde bulunmayan birçok karakter mevcuttur, üçüncü satır ise boştur).

![Deneme](media/stt/speech-language-datasets-report.png)

Durumu “Complete” (Tamamlandı) olan dil veri kümeleri özel bir dil modeli oluşturmak için kullanılabilir.

![Deneme](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Özel dil modeli oluşturma

Dil verileriniz hazır duruma geldikten sonra özel dil modeli oluşturma işlemini başlatmak için “Menu” (Menü) açılan menüsünden “Language Models” (Dil Modelleri) öğesine tıklayın. Bu sayfada var olan özel dil modellerinizin bulunduğu “Language Models” (Dil Modelleri) adlı bir tablo yer alır. Henüz hiç özel dil modeli oluşturmadıysanız tablo boş olacaktır. Geçerli yerel ayar, tabloda ilgili veri girişinin yanında gösterilir.

Eylem gerçekleştirilmeden önce uygun dil ayarının seçilmesi gerekir. Geçerli yerel ayar tüm veri, model ve dağıtım sayfalarında tablo başlığında belirtilir. Yerel ayarı değiştirmek için tablo başlığının adında bulunan “Change Locale” (Yerel Ayarı Değiştir) düğmesine tıklayın. Bu durumda yerel ayar onay sayfası açılır. Tabloya geri dönmek için "OK" (Tamam) düğmesine tıklayın.

"Create Language Model" (Dil Modeli Oluştur) sayfasında kullanılan veri kümesi gibi bu modelle ilgili bilgileri takip etmenize yardımcı olacak "Name" (Ad) ve "Description" (Açıklama) bilgileri girin. Ardından açılan menüden “Base Language Model” (Temel Dil Modeli) girişini seçin. Bu model, özelleştirme işlemlerinizin başlangıç noktası olacaktır. İki temel dil modelinden birini seçebilirsiniz. Search and Dictation modeli; komutlar, arama sorguları veya dikte gibi uygulamaya yönlendirilen konuşmalar için uygundur. Conversational model, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezlerinde veya toplantılarda kullanılır. "Universal" adlı yeni bir model de genel kullanıma sunulmuştur. Universal, tüm senaryoları kapsamayı hedeflemektedir ve ileride Search and Dictation ile Conversational modellerinin yerini alacaktır.

5.  Aşağıdaki örnekte temel dil modelini belirledikten sonra “Language Data” (Dil Verileri) açılan menüsünü kullanarak özelleştirme için kullanmak istediğiniz dil veri kümesini seçin

![Deneme](media/stt/speech-language-models-create2.png)

Akustik model oluşturma adımında olduğu gibi işleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin çevrimdışı testini gerçekleştirebilirsiniz. Model değerlendirme için akustik veri kümesi gerekir.

Dil modelinizin çevrimdışı testini gerçekleştirmek için “Offline Testing” (Çevrimdışı Test) öğesinin yanındaki onay kutusunu işaretleyin. Ardından açılan menünden bir akustik model seçin. Henüz bir özel akustik model oluşturmadıysanız menüde yalnızca Microsoft temel akustik modelleri gösterilir. Konuşmaya dayalı LM temel modeli seçmeniz durumunda burada konuşmaya dayalı AM kullanmanız gerekir. Arama ve dikte LM modeli kullanmanız halinde arama ve dikte AM modeli seçmeniz gerekir.

Son olarak değerlendirmeyi gerçekleştirmek için kullanmak istediğiniz akustik veri kümesini seçin.

İşlemeye başlamaya hazır olduğunuzda “Create” (Oluştur) düğmesine basarak dil modeli tablosuna gidebilirsiniz. Tabloda bu modele karşılık gelen yeni bir giriş olacaktır. Durum alanı, modelin durumunu gösterir ve “Waiting” (Beklemede), “Processing” (İşleniyor) ve “Complete” (Tamamlandı) gibi farklı aşamaları yansıtır.

Model “Complete” (Tamamlandı) durumuna ulaştıktan sonra bir uç noktaya dağıtılabilir. “View Result” (Sonucu Görüntüle) öğesine tıkladığınızda varsa çevrimdışı testin sonuçları gösterilir.

Herhangi bir noktada Modelin "Name" (Ad) veya "Description" (Açıklama) bilgilerini değiştirmek isterseniz dil modelleri tablosunun ilgili satırındaki “Edit” (Düzenle) bağlantısını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [C# dilinde konuşmayı algılama](quickstart-csharp-dotnet-windows.md)
- [Git Örnek Verileri](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
