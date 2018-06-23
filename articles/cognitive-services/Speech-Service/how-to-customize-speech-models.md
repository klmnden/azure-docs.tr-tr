---
title: Konuşma metin modelleri için özelleştirme | Microsoft Docs
description: Konuşma tanıma için metin modelleri konuşma özelleştirerek geliştirin.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 380c585f57737c0aa4ec99303c52d4567500b5f4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354154"
---
# <a name="customize-speech-to-text-models-using-speech-service"></a>Konuşma hizmetini kullanarak "Metne konuşma" modelleri özelleştirme

Daha iyi konuşma tanıma sonuçları elde yardımcı olmak için konuşma hizmeti tarafından kullanılan üç modeli özelleştirmenizi sağlar **metin konuşma** API. Modeller, otomatik olarak sağladığınız örneği verilerden eğitilmiş olup.

| Model | Örnek veriler | Amaç |
|-------|---------------|---------|
| Akustik modeli      | Konuşma metin | Belirli sözcükler ile ilişkilendirilmiş sesleri (Fonem) özelleştirin. Yeni Vurgu veya konuşarak ortamı, vb. dialect eğitmek. |
| Dil modeli      | Metin | Hizmet ve bunların içinde utterances nasıl kullanıldığı bilinen sözcükler özelleştirin. Teknik terimler, yerel yer adları vb. ekleyin. |
| Söyleniş | Metin | Sorunlu sözcükler, bileşimlerini ve kısaltmalar tanımayı geliştirmek. Örneğin, "threepio bkz:" "C3PO" kabul edilecek eşleme |

Yeni modelleri oluşturduktan sonra bir veya daha fazla yukarıdaki amaçları için modelinizin kullanan özel bir uç noktası oluşturun. Kullanmak, örneğin, isterseniz bir temel modeli konuşma hizmeti tarafından özel akustik modeli ve standart dil modeli sağlanan de seçebilirsiniz. Standart uç noktası yerine özel uç noktası sonra geri KALAN istekleri için kullanır. Her bitiş noktasıyla ilişkili bir sahip *dağıtım kimliği* böylece konuşma SDK ile birlikte kullanılabilir.

Tüm modelleri özelleştirmesini aracılığıyla yapılır [özel konuşma portal](https://www.cris.ai/).

## <a name="language-support"></a>Dil desteği

Aşağıdaki diller için özel desteklenir **metin konuşma** dil modeller.

| Kod | Dil |
|-|-|
| tr-TR | İngilizce (ABD) 
| zh-CN | Çince 
| SP SP | İspanyolca (İspanya) 
| fr-FR | Fransızca (Fransa) 
| it-IT | İtalyanca 
| de-DE | Almanca 
| pt-BR | Portekizce (Brezilya)
| ru-RU | Rusça
| JP JP | Japonca
| ar-ÖRN | Arapça (Mısır)

Özel akustik modelleri yalnızca ABD İngilizcesi (en-US) destekler. Ancak, Çince akustik veri kümelerini Çince dil modelleri test etmek için içeri aktarılabilir.

Özel telaffuz şu anda yalnızca ABD İngilizcesi (en-US) ve Almanca (de-DE) destekler.

## <a name="prepare-data-sets"></a>Veri kümeleri hazırlama

Her türde bir model biraz farklı veri ve biçimlendirme, burada açıklandığı gibi gerektirir.

| Model | Sunduğunuz hizmetler      |
|-------|-----------------------|
| Akustik | Tam utterances ses dosyaları içeren bir ZIP dosyası ve bu dosyaların transcriptions içeren bir metin dosyası. Her satır dosyanın adını dosya, bir sekme (ASCII 9) ve metin oluşmalıdır.|
| Dil | Satır başına bir utterance içeren bir metin dosyası. |
| Söyleniş | Her satırda bir telaffuz ipucu içeren bir metin dosyası. Her ipucu bir sekme (ASCII 9) ve ardından bir görünen (bir sözcük veya kısaltması) ve konuşma formunda (istenen telaffuz) olur.  |

Metin dosyaları izlemelidir [metin transcription yönergeleri](prepare-transcription.md) modelinin dil için.

## <a name="prepare-audio-files"></a>Ses dosyalarını hazırlama

Ses dosyalarını akustik modelleri için kaydedilen temsili bir konumda çeşitli temsilcisi kullanıcılar tarafından (bir Konuşmacı tanıma iyileştirmek için amacınız olmadığı sürece), kullanıcılarınızın olması için benzer bir mikrofon kullanarak. Tüm ses örnekleri gerekli biçimi bu tabloda açıklanmıştır.

| Özellik | Gerekli değer |
|----------|------|
Dosya biçimi | RIFF (WAV)
Örnekleme hızı | 8000 Hz veya 16.000 Hz
Kanallar | 1 (mono)
Örnek Biçim | PCM, 16 bit tam sayı
Dosya süresi | 0,1 ve 60 saniye arasında
Sessizlik collar | 0,1 saniye
Arşiv biçimi | zip
En fazla arşiv boyutu | 2 GB

Eğitim varsa bir Fabrika veya bir araba gibi gürültülü arka plan çalışması için bir model birkaç saniye tipik arka plan gürültü başında veya sonunda bazı örnekleri içerir. Yalnızca gürültü örnekleri dahil etmeyin.

## <a name="upload-data-sets"></a>Veri kümeleri karşıya yükle

Özel bir model oluşturmak için önce verilerinizi karşıya yükleyin, sonra Eğitim işlemini başlatmak.

1.  Oturum [özel konuşma portal](https://www.cris.ai/).

1.  Veri kümesi oluşturmak için istediğiniz seçin **özel konuşma** menüsü - dil modelleri için dil verileri veya telaffuz akustik modelleri için uyarlama verileri. Var olan veri kümeleri (varsa) bu tür bir listesi görüntülenir.

1. Tıklayarak dili seçin **yerel ayarını Değiştir**.

1.  Tıklatın **alma yeni** ve bir ad ve yeni veri kümesi için bir açıklama belirtin.

1. Hazırladığınız veri dosyalarını seçin.

1. ' I tıklatın **alma** verileri yüklemek ve doğrulama başlayın. Tüm dosyaların doğru biçimde olduğunu doğrulama sağlar. Doğrulama birkaç dakika sürebilir.

## <a name="create-a-model"></a>Bir model oluşturma

 Veri kümenizi doğrulandıktan sonra model gibi eğitim yapabilirsiniz.

> [!NOTE]
> Söyleniş veri Eğitilecek gerekmez.

1. Gelen oluşturmakta modeli seçin **özel konuşma** menüsü, ardından **yeni oluştur.**

1. Model için yerel ayarları seçin.

1. Yeni modeliniz için temel bir modeli seçin. Seçiminizi temel model için model, veri kümenizdeki tüm veriler için bir geri dönüş olarak hizmet veren yanı sıra kullanılabilecek tanıma modları belirler.

1.  Model oluşturulacak olduğu veri kümesi seçin. Bir veri kümesi herhangi bir sayıda modelleri kullanılabilir.

1. Tıklatın **oluşturma** yeni model eğitim başlamak için.

## <a name="test-a-model"></a>Test bir modeli

Model oluşturma işleminin bir parçası olarak modelinizi akustik bir veri kümesine karşı test edebilirsiniz. Yeni model veri kümesinin ses dosyaları ve ilgili metin karşı test sonuçları konuşma tanımak için kullanılır. En iyi sonuçlar için model oluşturmak için kullanılan olandan farklı bir akustik veri kümesi kullanın.

## <a name="custom-endpoint"></a>Özel uç nokta

Özel akustik modelleri veya dil modelleri oluşturduktan sonra bunları özel olarak dağıtabilirsiniz **metin konuşma** uç noktası. Bu çağrı yapmak için yalnızca bir uç nokta oluşturulan hesabı izin.

Bir uç nokta oluşturmak için seçtiğiniz **dağıtımları** gelen **özel konuşma** sayfanın üstündeki menü. **Dağıtımları** sayfa tüm oluşturduysanız geçerli özel uç noktaları, bir tablo içeriyor. Tıklatın **Yeni Oluştur** yeni bir uç noktası oluşturulamadı.

Kullanmak istediğiniz modelleri seçin **akustik modeli** ve **dil modeli** listeler. Kullanılabilir seçenekler, her zaman temel Microsoft modelleri ekleyin. Değil konuşma modelleri ile arama karışımı ve modelleri dikte, sınırlar kullanılabilir dil modeller, bu nedenle bir akustik modeli seçme ve tersi. Herhangi bir sayıda uç noktalar aynı modelleri kullanabilir.

Tıklatın **oluşturma** modelleri seçme sonra. Yeni uç noktanızı sağlamak için 30 dakika kadar sürebilir.

Uç noktanız hazır olduğunda seçin **dağıtımları** URI ve dağıtım kimliğini görmek için tablo Özel uç noktaları ile kullanabileceğiniz [Rest API](rest-apis.md#speech-to-text) ve [konuşma SDK](speech-sdk.md). [Kod örnekleri](samples.md) metin uç nokta için özel bir konuşma kullanma örneği içerir.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi Al](https://azure.microsoft.com/try/cognitive-services/)
- [C# Konuşma tanımasını nasıl](quickstart-csharp-windows.md)
