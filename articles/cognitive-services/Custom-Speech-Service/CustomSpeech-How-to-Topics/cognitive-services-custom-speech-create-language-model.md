---
title: 'Öğretici: Özel Konuşma Tanıma Hizmeti ile dil modeli oluşturma - Microsoft Bilişsel Hizmetler | Microsoft Docs'
description: Bu öğreticide Microsoft Bilişsel Hizmetler'de Özel Konuşma Tanıma Hizmeti ile dil modeli oluşturmayı öğreneceksiniz.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: nitinme
ms.openlocfilehash: fe486cb31aa77cf130ca852d022543d1400c6470
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862420"
---
# <a name="tutorial-create-a-custom-language-model"></a>Öğretici: Özel dil modeli oluşturma

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Bu öğreticide kullanıcıların bir uygulamada söylemesini veya yazmasını beklediğiniz metin sorguları veya konuşmalar için özel bir dil modeli oluşturacaksınız. Ardından bu özel dil modelini Microsoft'un son teknoloji konuşma modelleriyle birlikte kullanarak uygulamanıza sesli etkileşim ekleyeceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Verileri hazırlama
> * Dil veri kümesini içeri aktarma
> * Özel dil modelini oluşturma

Bilişsel Hizmetler hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://cris.ai) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

[Cognitive Services Subscriptions](https://cris.ai/Subscriptions) (Bilişsel Hizmetler Abonelikleri) sayfasını açarak Bilişsel Hizmetler hesabınızın bir aboneliğe bağlanmış olduğundan emin olun.

Herhangi bir abonelik listelenmiyorsa **Get free subscription** (Ücretsiz abonelik oluştur) düğmesine tıklayarak Bilişsel Hizmetler'in sizin için bir hesap oluşturmasını sağlayabilirsiniz. Alternatif olarak **Connect existing subscription** (Var olan aboneliğe bağlan) düğmesine tıklayarak Azure portalında oluşturulmuş olan bir Özel Arama Hizmeti aboneliğine bağlanabilirsiniz.

Azure portalında Özel Arama Hizmeti aboneliği oluşturma hakkında bilgi için bkz. [Azure portalında Bilişsel Hizmetler API'si hesabı oluşturma](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Verileri hazırlama

Uygulamanız için özel bir dil modeli oluşturmak üzere sisteme bir dizi örnek konuşma eklemeniz gerekir. Örneğin:

*   "He has had urticaria for the past week." (Geçen hafta kurdeşen döktü.)
*   "The patient had a well-healed herniorrhaphy scar." (Hastanın fıtık ameliyatı yarası güzel bir şekilde iyileşmiş.)

Cümlelerin tam veya dilbilgisi açısından doğru olması gerekmez ve bu cümlelerin sistemin dağıtımda karşılaşacağını beklediğiniz konuşma girişini tam olarak yansıtması gerekir. Bu örnekler kullanıcıların uygulamanızda gerçekleştireceği görevlerin stilini ve içeriğini yansıtmalıdır.

Dil modeli verileri, yerel ayara bağlı olarak US-ASCII veya UTF-8 kodlamasıyla düz metin dosyasında yazılmalıdır. en-US yerel ayarında iki kodlama da desteklenir. zh-CN yerel ayarında yalnızca UTF-8 kodlaması desteklenir (BOM isteğe bağlıdır). Metin dosyasının her satırında bir örnek (cümle, konuşma veya sorgu) bulunmalıdır.

Bazı cümlelerin ağırlığının daha fazlasını isterseniz verilerinize birkaç kez ekleyebilirsiniz. Tekrarları 10-100 aralığında tutmanız önerilir. 100'e göre normalleştirirseniz cümlelerin ağırlığını bu ölçekte kolayca belirleyebilirsiniz.

Dil verilerinin ana gereksinimleri aşağıdaki tabloda özetlenmiştir.

| Özellik | Değer |
|----------|-------|
| Metin Kodlaması | en-US: ABD ACSII veya UTF-8 ya da zh-CN: UTF-8|
| Satır başına konuşma sayısı | 1 |
| En Büyük Dosya Boyutu | 200 MB |
| Açıklamalar | karakterleri 4'ten fazla tekrarlamaktan kaçının, örneğin: "aaaaa"|
| Açıklamalar | "\t" gibi özel karakterler veya [Unicode karakter tablosunda](http://www.utf8-chartable.de/) U+00A1 üzerindeki UTF-8 karakterleri kullanılamaz|
| Açıklamalar | Bir URI'yi telaffuz etmenin tek bir yöntemi olmadığından URI'ler de reddedilir|

Metin içeri aktarıldığında sistem tarafından işlenebilmesi için normalleştirilmiş metin haline getirilir. Ancak veriler yüklenmeden _önce_ kullanıcı tarafından gerçekleştirilmesi gereken bazı önemli normalleştirme adımları vardır. Dil verilerinizi hazırlarken uygun dili belirlemek için [Transkripsiyon yönergelerine](cognitive-services-custom-speech-transcription-guidelines.md) bakın.

## <a name="import-the-language-data-set"></a>Dil veri kümesini içeri aktarma

"Acoustic Datasets" (Akustik Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır.

Kendi oluşturduğunuz dil veri kümesini içeri aktarmaya hazır olduğunuzda [Özel Konuşma Tanıma Hizmeti Portalı](https://cris.ai)'nda oturum açın.  Ardından üst şeritteki “Custom Speech” (Özel Konuşma) açılan menüsüne tıklayıp “Adaptation Data” (Uyarlama Verileri) öğesini seçin. Özel Konuşma Tanıma Hizmeti'ne ilk kez veri yüklüyorsanız “Datasets” (Veri Kümeleri) adlı boş bir tablo görürsünüz.

Yeni veri kümesini içeri aktarmak için "Language Datasets" (Dil Veri Kümeleri) satırındaki "Import" (İçeri Aktar) düğmesine tıkladığınızda sitede yeni veri kümesi yükleme sayfası açılır. Name (Ad) ve Description (Açıklama) alanlarına ileride veri kümesini tanımlamanıza yardımcı olacak bilgiler girin. Ardından “Choose File” (Dosya Seç) düğmesini kullanarak dil verileri metin dosyasını bulun. Daha sonra “Import” (İçeri Aktar) öğesine tıklayarak veri kümesinin yüklenmesini sağlayın. Veri kümesinin boyutuna bağlı olarak bu işlem birkaç dakika sürebilir.

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-import.png)

İçeri aktarma işlemi tamamlandığında dil verileri tablosuna dönecek ve dil verileri kümenizi gösteren bir giriş göreceksiniz. Bu kümeye benzersiz tanıtıcı (GUID) atanmış olduğuna dikkat edin. Ayrıca verilerin geçerli durumunu gösteren bir durum alanı da bulunur. Durum işlenmek üzere kuyruğa alınmış olduğunda “Waiting” (Beklemede), doğrulama sürecinde “Processing” (İşleniyor), kullanıma hazır olduğunda ise “Complete” (Tamamlandı) olur. Veri doğrulama, dosyadaki metinler üzerinde bir dizi denetim gerçekleştirir ve veri metinlerinde normalleştirme yapar.

Durum “Complete” (Tamamlandı) olduğunda “View Report” (Raporu Görüntüle) öğesine tıklayarak dil verileri doğrulama raporunu görebilirsiniz. Doğrulama adımında başarılı ve başarısız olan konuşma sayısının yanı sıra başarısız olan konuşmalar hakkında ayrıntılı bilgiler gösterilir. Aşağıdaki örnekte iki konuşma, uygun olmayan karakterlerin kullanılması nedeniyle doğrulama adımında başarısız olmuştur (bu veri kümesinde birincide iki ifade, ikisinde de ASCII yazdırılabilir karakter kümesinde bulunmayan birçok karakter mevcuttur).

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-report.png)

Durumu “Complete” (Tamamlandı) olan dil veri kümeleri özel bir dil modeli oluşturmak için kullanılabilir.

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Özel dil modeli oluşturma

Dil verileriniz hazır duruma geldikten sonra özel dil modeli oluşturma işlemini başlatmak için “Menu” (Menü) açılan menüsünden “Language Models” (Dil Modelleri) öğesine tıklayın. Bu sayfada var olan özel dil modellerinizin bulunduğu “Language Models” (Dil Modelleri) adlı bir tablo yer alır. Henüz hiç özel dil modeli oluşturmadıysanız tablo boş olacaktır. Geçerli yerel ayar, tablo başlığında gösterilir. Farklı bir dil için dil modeli oluşturmak isterseniz “Change Locale” (Yerel Ayarı Değiştir) öğesine tıklayın. Desteklenen diller hakkında daha fazla bilgi için [Dil Ayarını Değiştirme](cognitive-services-custom-speech-change-locale.md) bölümüne bakın. Yeni bir model oluşturmak için tablo başlığının altındaki “Create New” (Yeni Oluştur) bağlantısına tıklayın.

"Create Language Model" (Dil Modeli Oluştur) sayfasında kullanılan veri kümesi gibi bu modelle ilgili bilgileri takip etmenize yardımcı olacak "Name" (Ad) ve "Description" (Açıklama) bilgileri girin. Ardından açılan menüden “Base Language Model” (Temel Dil Modeli) girişini seçin. Bu model, özelleştirme işlemlerinizin başlangıç noktası olacaktır. İki temel dil modelinden birini seçebilirsiniz. _Microsoft Search and Dictation LM_; komutlar, arama sorguları veya dikte gibi uygulamaya yönlendirilen konuşmalar için uygundur. _Microsoft Conversational LM_, günlük konuşma tarzındaki konuşmaları tanımak için uygundur. Bu konuşma türü genelde başka bir kişiye hitaben yapılır ve çağrı merkezlerinde veya toplantılarda kullanılır.

Temel dil modelini belirledikten sonra “Language Data” (Dil Verileri) açılan menüsünü kullanarak özelleştirme için kullanmak istediğiniz dil veri kümesini seçin

![Deneme](../../../media/cognitive-services/custom-speech-service/custom-speech-language-models-create2.png)

Akustik model oluşturma adımında olduğu gibi işleme tamamlandıktan sonra isteğe bağlı olarak yeni modelinizin çevrimdışı testini gerçekleştirebilirsiniz. Konuşmayı metne dönüştürme performansı değerlendirileceğinden çevrimdışı test sırasında akustik veri kümesi kullanılması gerekir.

Dil modelinizin çevrimdışı testini gerçekleştirmek için “Offline Testing” (Çevrimdışı Test) öğesinin yanındaki onay kutusunu işaretleyin. Ardından açılan menünden bir akustik model seçin. Henüz bir özel akustik model oluşturmadıysanız menüde yalnızca Microsoft temel akustik modelleri gösterilir. Konuşmaya dayalı LM temel modeli seçmeniz durumunda burada konuşmaya dayalı AM kullanmanız gerekir. Arama ve dikte LM modeli kullanmanız halinde arama ve dikte AM modeli seçmeniz gerekir.

Son olarak değerlendirmeyi gerçekleştirmek için kullanmak istediğiniz akustik veri kümesini seçin.

İşlemi başlatmaya hazır olduğunuzda “Create” (Oluştur) düğmesine basın. Dil modeli tablosu açılır. Tabloda bu modele karşılık gelen yeni bir giriş olacaktır. Durum alanı, modelin durumunu gösterir ve “Waiting” (Beklemede), “Processing” (İşleniyor) ve “Complete” (Tamamlandı) gibi farklı aşamaları yansıtır.

Model “Complete” (Tamamlandı) durumuna ulaştıktan sonra bir uç noktaya dağıtılabilir. “View Result” (Sonucu Görüntüle) öğesine tıkladığınızda varsa çevrimdışı testin sonuçları gösterilir.

Herhangi bir noktada Modelin "Name" (Ad) veya "Description" (Açıklama) bilgilerini değiştirmek isterseniz dil modelleri tablosunun ilgili satırındaki “Edit” (Düzenle) bağlantısını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide metinle kullanmak üzere özel bir dil modeli geliştirdiniz. Ses dosyaları ve transkripsiyonlarla kullanmak üzere özel bir akustik model oluşturmak için akustik model oluşturma öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Özel akustik model oluşturma](cognitive-services-custom-speech-create-acoustic-model.md)
