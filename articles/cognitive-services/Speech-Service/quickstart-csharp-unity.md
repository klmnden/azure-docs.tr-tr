---
title: 'Hızlı Başlangıç: Unity - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Unity ve Speech SDK'sı için Unity (Beta) ile bir konuşma metin uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: 3cedfaf1ae16c17026314fc24dbdc7bb11494caf
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020949"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Hızlı Başlangıç: Konuşma SDK'sı (Beta) Unity için konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Kullanarak bir konuşma metin uygulaması oluşturmak için bu kılavuzu kullanın [Unity](https://unity3d.com/) ve Speech SDK'sı (Beta) Unity için.
İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
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
* Bilgisayarınızın mikrofon erişimi.

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

En az bir kullanıcı Arabirimi bizim sahneye, konuşma tanıma ve sonucu görüntülemek için bir metin alanı tetiklemek için bir düğmeye oluşan ekleriz.

* İçinde [hiyerarşi penceresinde](https://docs.unity3d.com/Manual/Hierarchy.html) (varsayılan olarak sol taraftaki), Unity yeni proje ile oluşturulan bir örnek Sahne gösterilir.
* Tıklayın **Oluştur** düğmesini hiyerarşi penceresinin en üstünde ve **UI** > **düğmesi**.
* Bu hiyerarşi penceresinde görebilirsiniz üç oyun nesneleri oluşturur: bir **düğmesi** iç içe nesne içinde bir **tuval** nesnesi ve bir **olay sistemi** nesne.
* [Sahne görünüme gidin](https://docs.unity3d.com/Manual/SceneViewNavigation.html) tuval ve düğmeye iyi bir açıyla alacak şekilde [Sahne görünümünde](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Tıklayın **düğmesi** ayarlarını görüntülemek için hiyerarşi penceresinde nesne [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
* Ayarlama **Pos X** ve **Pos Y** özelliklerine **0**, düğmeyi ortasında tuval ortalanacak şekilde.
* Tıklayın **Oluştur** hiyerarşi penceresinin en üstünde düğmesini tekrar ve **UI** > **metin** bir metin alanı oluşturmak için.
* Tıklayın **metin** ayarlarını görüntülemek için hiyerarşi penceresinde nesne [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
* Ayarlama **Pos X** ve **Pos Y** özelliklerine **0** ve **120**, ayarlayıp **genişliği** ve **Yükseklik** özelliklerine **240** ve **120** metin alanı ve düğme çakışmadığından emin olmak için.

İşiniz bittiğinde, kullanıcı arabirimini bu ekran görüntüsüne benzer görünmelidir:

[![Unity Düzenleyicisi'nde Hızlı Başlangıç kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. İçinde [proje penceresi](https://docs.unity3d.com/Manual/ProjectView.html) tıklayın (varsayılan olarak sol alttaki) **Oluştur** düğmesine ve ardından  **C# betik**. Betik adı `HelloWorld`.

1. Çift tıklayarak betiğini düzenleyin.

   > [!NOTE]
   > Hangi Kod Düzenleyicisi altında başlatılacak yapılandırabileceğiniz **Düzenle** > **tercihleri**, bakın [Unity kullanıcı kılavuzuna](https://docs.unity3d.com/Manual/Preferences.html).

1. Tüm kodu aşağıdakiyle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Bulun ve dize değiştirin `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınız ile.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Değişiklikleri komut dosyasına kaydedin.

1. Unity Editor geri, betiği bir bileşen olarak oyun nesnelerinizi biri olarak eklenmesi gerekir.

   * Tıklayarak **tuval** hiyerarşi penceresinde nesne. Bu ayar, Yukarı açar [denetçisi penceresi](https://docs.unity3d.com/Manual/UsingTheInspector.html) (varsayılan sağ).
   * Tıklayın **Bileşen Ekle** düğmesini Inspector penceresini ve ardından arama HelloWorld komut yukarıda oluşturun ve bunu ekleyin.
   * Hello World bileşen iki başlatılmamış özelliklere sahip olduğuna dikkat edin **çıkış metnini** ve **Başlat düğmesini tanıma**, genel özelliklerini eşleşen `HelloWorld` sınıfı.
     Bunları ayarlamak wire için Nesne Seçicisi (özellik sağındaki küçük daire simgesi) tıklayın ve daha önce oluşturduğunuz metin ve düğme nesneleri seçin.

     > [!NOTE]
     > Düğme, ayrıca iç içe geçmiş metin nesnesine sahiptir. Yanlışlıkla bu metin çıktısı (veya ad alanı, Karışıklığı önlemek için Inspector penceresinde kullanarak metin nesnelerinin birini yeniden adlandırın) seçtiğinizden değil emin olun.

## <a name="run-the-application-in-the-unity-editor"></a>Unity Düzenleyicisi'nde uygulamayı çalıştırın

* Tuşuna **Play** (Aşağıda, menü çubuğu) Unity Düzenleyicisi araç çubuğu düğmesini.

* Uygulamayı başlattıktan sonra düğmesine tıklayın ve bir İngilizce deyim ya da cümle bilgisayarınızın mikrofona. Konuşma konuşma hizmetlere iletilen ve transcribed pencerede görünen metin.

  [![Unity oyun penceresinde çalışan Hızlı Başlangıç ekran görüntüsü](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-expanded.png#lightbox)

* Denetleme [konsol penceresi](https://docs.unity3d.com/Manual/Console.html) hata ayıklama iletileri.

* Konuşma tanıma işiniz bittiğinde tıklayın **Play** uygulamayı durdurmak için Unity Düzenleyicisi araç çubuğu düğmesi.

## <a name="additional-options-to-run-this-application"></a>Bu uygulamayı çalıştırmak için ek seçenekler

Bu uygulama bir tek başına uygulama Windows veya UWP uygulaması olarak Android için de dağıtılabilir.
Başvurmak bizim [örnek depoyu](https://aka.ms/csspeech/samples) csharp/hızlı başlangıç-unity klasöründe bu ek hedefler yapılandırmasını açıklar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
