---
title: 'Hızlı Başlangıç: Okuma, Unity - konuşma Hizmetleri sentezlemek'
titleSuffix: Azure Cognitive Services
description: Unity ve Speech SDK'sı (Beta) Unity için bir metin okuma uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde, cihazınızın Konuşmacı gerçek zamanlı olarak metinden konuşmaya sentezlemek.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/26/2019
ms.author: yinhew
ms.openlocfilehash: 5240ea45097ce3c0ae7ccbc15a7f99b2f5990832
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467491"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity-beta"></a>Hızlı Başlangıç: Unity (Beta) Speech SDK'sı ile Konuşma sentezlemek

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma tanıma](quickstart-csharp-unity.md).

Kullanarak bir metin okuma uygulama oluşturmak için bu kılavuzu kullanın [Unity](https://unity3d.com/) ve Speech SDK'sı (Beta) Unity için.
İşiniz bittiğinde, cihazınızın Konuşmacı gerçek zamanlı olarak metinden konuşmaya sentezlemek.
Unity ile ilgili bilgi sahibi değilseniz, üzerinde çalışmanız önerilir [Unity kullanıcı kılavuzuna](https://docs.unity3d.com/Manual/UnityManual.html) uygulama geliştirme çalışmalarınızı başlatmadan önce.

> [!NOTE]
> Unity Speech SDK'sı şu anda beta sürümündedir.
> Bu, Windows Masaüstü (x86 ve x64) veya evrensel Windows Platformu (x86, x64, ARM/ARM64) ve Android (x86 ARM32/64) destekler.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Unity 2018.3 veya üzeri](https://store.unity.com/) ile [Unity 2019.1 UWP ARM64 için destek ekleme](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
     * ARM64 podporu androidu Pro [ARM64 ve ARM64 için Windows 10 SDK'sı için isteğe bağlı derleme araçları](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/) 
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz edinin](get-started.md).

## <a name="create-a-unity-project"></a>Unity proje oluşturma

* Unity başlatmak ve altında **projeleri** sekmesinde **yeni**.
* Belirtin **proje adı** olarak **csharp unity**, **şablon** olarak **3B** ve bir konum seçin.
  Ardından **proje oluştur**.
* Biraz zaman sonra Unity Düzenleyicisi penceresi açılır.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* Speech SDK'sı (Beta) Unity için Unity varlık paketini (.unitypackage) paketlenir.
  İndirdiği [burada](https://aka.ms/csspeech/unitypackage).
* Konuşma SDK'yı seçerek alma **varlıklar** > **paketini içeri aktar** > **özel paket**.
  Kullanıma [Unity belgeleri](https://docs.unity3d.com/Manual/AssetPackages.html) Ayrıntılar için.
* Dosya Seçici'de, yukarıda indirdiğiniz Speech SDK'sı .unitypackage dosyasını seçin.
* Tüm dosyaların seçildiğinden ve tıklayın olun **alma**:

  ![Unity konuşma SDK Unity varlık paketini içeri aktarılırken Düzenleyicisi'nin ekran görüntüsü](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>Kullanıcı Arabirimi ekleme

En az bir kullanıcı Arabirimi bizim sahneye giriş alanını sentezi, bir düğme tetikleyici konuşma sentezi için metin girin ve sonucu görüntülemek için bir metin alanı oluşan ekleriz.

* İçinde [hiyerarşi penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak sol taraftaki), Unity yeni proje ile oluşturulan bir örnek Sahne gösterilir.
* Tıklayın **Oluştur** düğmesini hiyerarşi penceresinin en üstünde ve **UI** > **giriş alanı**.
* Bu hiyerarşi penceresinde görebilirsiniz üç oyun nesneleri oluşturur: bir **giriş alanı** iç içe nesne içinde bir **tuval** nesnesi ve bir **olay sistemi** nesne.
* [Sahne görünüme gidin](https://docs.unity3d.com/Manual/SceneViewNavigation.html) tuval ve giriş alanı iyi bir açıyla alacak şekilde [Sahne görünümünde](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Tıklayın **giriş alanı** ayarlarını görüntülemek için hiyerarşi penceresinde nesne [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
* Ayarlama **Pos X** ve **Pos Y** özelliklerine **0**, giriş alanını tuvalin ortasında ortalanacak şekilde.
* Tıklayın **Oluştur** hiyerarşi penceresinin en üstünde düğmesini tekrar ve **UI** > **düğmesi** düğme oluşturmak için.
* Tıklayın **düğmesi** ayarlarını görüntülemek için hiyerarşi penceresinde nesne [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
* Ayarlama **Pos X** ve **Pos Y** özelliklerine **0** ve **-48**, ayarlayıp **genişliği** ve **Yükseklik** özelliklerine **160** ve **30** düğmesi ve giriş alanı çakışmadığından emin olmak için.
* Tıklayın **Oluştur** hiyerarşi penceresinin en üstünde düğmesini tekrar ve **UI** > **metin** bir metin alanı oluşturmak için.
* Tıklayın **metin** ayarlarını görüntülemek için hiyerarşi penceresinde nesne [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
* Ayarlama **Pos X** ve **Pos Y** özelliklerine **0** ve **80**, ayarlayıp **genişliği** ve  **Yükseklik** özelliklerine **320** ve **80** metni alanına ve giriş alanı çakışmadığından emin olmak için.
* Tıklayın **Oluştur** hiyerarşi penceresinin en üstünde düğmesini tekrar ve **ses** > **ses kaynağından** bir ses kaynağından oluşturmak için.

İşiniz bittiğinde, kullanıcı arabirimini bu ekran görüntüsüne benzer görünmelidir:

[![Unity Düzenleyicisi'nde Hızlı Başlangıç kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. İçinde [proje penceresi](https://docs.unity3d.com/Manual/ProjectView.html) tıklayın (varsayılan olarak sol alttaki) **Oluştur** düğmesine ve ardından  **C# betik**. Betik adı `HelloWorld`.

1. Çift tıklayarak betiğini düzenleyin.

   > [!NOTE]
   > Hangi Kod Düzenleyicisi altında başlatılacak yapılandırabileceğiniz **Düzenle** > **tercihleri**, bakın [Unity kullanıcı kılavuzuna](https://docs.unity3d.com/Manual/Preferences.html).

1. Tüm kodu aşağıdakiyle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Bulun ve dize değiştirin `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınız ile.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri komut dosyasına kaydedin.

1. Unity Editor geri, betiği bir bileşen olarak oyun nesnelerinizi biri olarak eklenmesi gerekir.

   * Tıklayarak **tuval** hiyerarşi penceresinde nesne. Bu ayar, Yukarı açar [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
   * Tıklayın **Bileşen Ekle** düğmesini Inspector penceresini ve ardından arama HelloWorld komut yukarıda oluşturun ve bunu ekleyin.
   * Hello World bileşen dört başlatılmamış özelliklere sahip olduğuna dikkat edin **çıkış metnini**, **giriş alanı**, **konuşmak düğmesi** ve **ses kaynak**, genel özelliklerini eşleşen `HelloWorld` sınıfı.
     Bunları ayarlamak wire için Nesne Seçicisi (özellik sağındaki küçük daire simgesi) tıklayın ve daha önce oluşturduğunuz metin ve düğme nesneleri seçin.

     > [!NOTE]
     > İç içe geçmiş metin nesnesine giriş alanı ve düğme de var. Emin olun, yanlışlıkla metin çıktısı için seçin (veya ad alanı, Karışıklığı önlemek için Inspector penceresinde kullanarak metin nesneleri yeniden adlandır).

## <a name="run-the-application-in-the-unity-editor"></a>Unity Düzenleyicisi'nde uygulamayı çalıştırın

* Tuşuna **Play** (Aşağıda, menü çubuğu) Unity Düzenleyicisi araç çubuğu düğmesini.

* Uygulamayı başlattıktan sonra girdi alanına metin girin ve düğmeye tıklayın. Metni konuşma hizmetlere iletilen ve konuşma tanıma, hoparlöründen için oluşturulan.

  [![Unity oyun penceresinde çalışan Hızlı Başlangıç ekran görüntüsü](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Denetleme [konsol penceresi](https://docs.unity3d.com/Manual/Console.html) hata ayıklama iletileri.

* Synthesizing konuşma işiniz bittiğinde tıklayın **Play** uygulamayı durdurmak için Unity Düzenleyicisi araç çubuğu düğmesi.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama bir tek başına uygulama Windows veya UWP uygulaması olarak Android için de dağıtılabilir.
Başvurmak bizim [örnek depoyu](https://aka.ms/csspeech/samples) csharp/hızlı başlangıç-unity klasöründe bu ek hedefler yapılandırmasını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Kayıt ses örnekleri](record-custom-voice-samples.md)
