---
title: "Öğretici: WPF ile bir çeviri uygulaması oluşturma C# -Translator metin çevirisi API'si"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, metin çevirisi, dil algılama ve tek bir abonelik anahtarı ile yazım Bilişsel hizmet API'lerini kullanan bir Windows Presentation Foundation (WPF) uygulaması oluşturacaksınız. Bu alıştırmada Translator metin çevirisi API'si ve Bing yazım denetimi API'si özelliklerinin nasıl kullanılacağını gösterir.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: b300c40b4a9c832a0df87f7cfc6e6a9558d766f6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448221"
---
# <a name="tutorial-create-a-translation-app-with-wpf"></a>Öğretici: WPF ile bir çeviri uygulaması oluşturma

Bu öğreticide, oluşturacağınız bir [Windows Presentation Foundation (WPF)](https://docs.microsoft.com/visualstudio/designers/getting-started-with-wpf?view=vs-2017) tek bir abonelik anahtarı ile Azure Bilişsel hizmetler metin çevirisi, dil algılama ve yazım denetimini kullanan uygulama. Özellikle, uygulamanızı Translator metin API'lerini çağıracak ve [Bing yazım denetimi](https://azure.microsoft.com/services/cognitive-services/spell-check/).

WPF nedir? Bu masaüstü istemci uygulamalar oluşturan bir kullanıcı Arabirimi bir çerçevedir. WPF geliştirme platformu, çok sayıda uygulama modeli, kaynaklar, denetimler, grafik, düzen, veri bağlama, belgeleri ve güvenlik dahil olmak üzere uygulama geliştirme özellikleri destekler. .NET Framework'ün bir alt kümesi olan şekilde, daha önce uygulamaları .NET Framework ile ASP.NET veya Windows Forms kullanarak oluşturduysanız, bir programlama deneyimi hakkında bilgi sahibi olmanız. WPF Genişletilebilir uygulama biçimlendirme dili (bildirim temelli bir model programlamada, uygulaması sağlamak için biz gelecek bölümde gözden geçireceğiz XAML) kullanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio'da bir WPF projesi oluşturma
> * Derlemeler ve NuGet paketlerini projenize ekleyin.
> * XAML ile uygulamanızın kullanıcı Arabirimi oluşturma
> * Dilleri Al, metni çevirmek ve kaynak dili algılama için Translator Text API kullanın
> * Bing yazım denetimi API'si girişinizi doğrulayın ve çeviri doğruluğunu artırmak için kullanın
> * WPF uygulamanızı çalıştırma

### <a name="cognitive-services-used-in-this-tutorial"></a>Bu öğreticide kullanılan bilişsel hizmetler

Bu liste, bu öğreticide kullanılan Bilişsel hizmetleri içerir. Her özellik için API Başvurusu göz atmak için bağlantıyı izleyin.

| Hizmet | Özellik | Açıklama |
|---------|---------|-------------|
| Translator Metin Çevirisi | [Dilleri Al](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) | Tam metin çevirileri için desteklenen dillerin listesini alın. |
| Translator Metin Çevirisi | [Çevir](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate) | Metin, 60'tan fazla dillere çevirme. |
| Translator Metin Çevirisi | [Algılama](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect) | Giriş metni dili algılayın. Güvenilirlik puanı algılama içerir. |
| Bing Yazım Denetimi | [Yazım denetimi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference) | Çeviri doğruluğunu artırmak için yazım hatalarını düzeltin. |

## <a name="prerequisites"></a>Önkoşullar

Devam şunlar gerekir:

* Bir Azure Bilişsel hizmetler abonelik. [Bilişsel hizmetler anahtarı alma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#multi-service-subscription).
* Windows makine
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) -topluluk veya kuruluş

> [!NOTE]
> Abonelik, Bu öğretici için Batı ABD bölgesinde oluşturmanızı öneririz. Aksi takdirde, bu alıştırmada çalışırken uç noktaları ve kod bölgelerde değiştirmek gerekir.  

## <a name="create-a-wpf-app-in-visual-studio"></a>Visual Studio'da bir WPF uygulaması oluşturma

Yapmamız gereken ilk şey, bizim projesini Visual Studio'da ayarlanır.

1. Visual Studio'yu açın. Ardından **Dosya > Yeni > Proje**.
2. Sol bölmede, bulun ve seçin **Visual C#** . Ardından, **WPF uygulaması (.NET Framework)** merkezi panelinde.
   ![Visual Studio'da bir WPF uygulaması oluşturma](media/create-wpf-project-visual-studio.png)
3. Framework sürümünü ayarlamak, projenizi adlandırın **.NET Framework 4.5.2 veya üzeri**, ardından **Tamam**.
4. Projeniz oluşturuldu. İki sekme açık olduğunu fark edeceksiniz: `MainWindow.xaml` ve `MainWindow.xaml.cs`. Bu öğretici boyunca kod için bu iki dosyayı ekleyeceğiz. Uygulamanın kullanıcı arabirimi için ilk; İkincisi için Translator metin çevirisi ve Bing yazım denetimi bizim çağrıları.
   ![Ortamınızı gözden geçirin](media/blank-wpf-project.png)

Projemizin JSON ayrıştırma gibi ek işlevler için derlemeler ve bir NuGet paketi eklemek için sonraki bölümde ekleyeceğiz.

## <a name="add-references-and-nuget-packages-to-your-project"></a>Başvurular ve NuGet paketlerini projenize ekleyin.

.NET Framework derlemelerinin ve NuGet Paket Yöneticisi'ni kullanarak yükleyeceğiz NewtonSoft.Json, birkaç Projemizin gerektirir.

### <a name="add-net-framework-assemblies"></a>.NET Framework derlemeleri ekleyin

Derlemeleri bizim proje nesnelerini seri hale getrime ve ve HTTP isteklerini ve yanıtlarını yönetmek için ekleyelim.

1. Projenizi Visual Studio Çözüm Gezgini'nde (sağ panelde) bulun. Projenize sağ tıklayın ve ardından **Ekle > başvurusu...** , açan **başvuru Yöneticisi**.
   ![Derleme başvuruları ekleme](media/add-assemblies-sample.png)
2. Derlemeler sekmesi, başvuru kullanılabilen tüm .NET Framework derlemelerini listeler. Arama çubuğuna için ekranın sağ üst kısımdaki bu başvuruları için arama yapın ve bunları projenize eklemek için kullanın:
   * [System.Runtime.Serialization](https://docs.microsoft.com/dotnet/api/system.runtime.serialization)
   * [System.Web](https://docs.microsoft.com/dotnet/api/system.web)
   * [System.Web.Extensions](https://docs.microsoft.com/dotnet/api/system.web)
3. Bu başvuruları projenize ekledikten sonra tıklayabilirsiniz **Tamam** kapatmak için **başvuru Yöneticisi**.

> [!NOTE]
> Derleme başvuruları hakkında daha fazla bilgi edinmek istiyorsanız bkz [nasıl yapılır: Başvuru Yöneticisi'ni kullanarak başvuru ekleyip](https://docs.microsoft.com/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager?view=vs-2017).

### <a name="install-newtonsoftjson"></a>NewtonSoft.Json yükleyin

Uygulamamızı NewtonSoft.Json JSON nesneleri seri durumdan çıkarılacak kullanır. Paketi yüklemek için bu yönergeleri izleyin.

1. Visual Studio Çözüm Gezgini'nde projenizi bulun ve projenize sağ tıklayın. Seçin **NuGet paketlerini Yönet...** .
2. Bulun ve seçin **Gözat** sekmesi.
3. Tür [NewtonSoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) Arama çubuğuna.
   ![Bulma ve NewtonSoft.Json yükleme](media/add-nuget-packages.png)
4. Paketi seçin ve tıklayın **yükleme**.
5. Yükleme tamamlandığında sekmesini kapatın.

## <a name="create-a-wpf-form-using-xaml"></a>XAML kullanarak WPF formu oluşturma

Uygulamanızı kullanmak için bir kullanıcı arabirimi ihtiyacınız olacak. XAML kullanarak, kullanıcıların giriş ve çeviri dilleri seçin, çevirmek için metin girin izin veren bir form oluşturacağız ve çeviri çıktıyı görüntüler.

Şimdi ne oluşturmakta olduğumuz bir göz atalım.

![WPF XAML kullanıcı arabirimi](media/translator-text-csharp-xaml.png)

Kullanıcı interfacer şu bileşenleri içerir:

| Ad | Tür | Açıklama |
|------|------|-------------|
| `FromLanguageComboBox` | ComboBox | Tarafından Microsoft Translator metin çevirisi için desteklenen dillerin listesini görüntüler. Kullanıcı çeviri yaptığı kaynak dili seçer. |
| `ToLanguageComboBox` | ComboBox | Dil olarak aynı listesini görüntüler `FromComboBox`, ancak kullanıcı çevirmek için bir dil seçmek için kullanılır. |
| `TextToTranslate` | TextBox | Çevrilecek metin girmesini sağlar. |
| `TranslateButton` | Düğme | Metni çevirmek için bu düğmeyi kullanın. |
| `TranslatedTextLabel` | Etiket | Çeviri görüntüler. |
| `DetectedLanguageLabel` | Etiket | Algılanan dilin Çevrilecek metin görüntüler (`TextToTranslate`). |

> [!NOTE]
> XAML kaynak kodunu kullanarak bu formu oluşturuyoruz, ancak Visual Studio düzenleyicisinde ile form oluşturabilirsiniz.

Projemizin için kod ekleyelim.

1. Visual Studio'da sekmesini seçin `MainWindow.xaml`.
2. Bu kod projenize kopyalayın ve kaydedin.
   ```xaml
   <Window x:Class="MSTranslatorTextDemo.MainWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           xmlns:local="clr-namespace:MSTranslatorTextDemo"
           mc:Ignorable="d"
           Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
       <Grid>
           <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
           <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
           <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
           <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

           <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
           <ComboBox x:Name="ToLanguageComboBox"
                   HorizontalAlignment="Left"
                   Margin="306,88,0,0"
                   VerticalAlignment="Top"
                   Width="175" FontSize="14" TabIndex="2">

           </ComboBox>
           <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
           <ComboBox x:Name="FromLanguageComboBox"
               HorizontalAlignment="Left"
               Margin="42,88,0,0"
               VerticalAlignment="Top"
               Width="175" FontSize="14" TabIndex="1"/>
           <Label x:Name="TranslatedTextLabel" Content="Translation is displayed here." HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
           <Label x:Name="DetectedLanguageLabel" Content="Autodetected language is displayed here." HorizontalAlignment="Left" Margin="39,288,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="84" BorderThickness="0"/>
       </Grid>
   </Window>
   ```
3. Artık Visual Studio kullanıcı arabiriminde uygulamanın önizlemesini görmeniz gerekir. Yukarıdaki resimde benzemelidir.

İşte bu kadar formunuz hazırdır. Şimdi metin çevirisi ve Bing yazım denetimi kullanmak için biraz kod yazalım.

> [!NOTE]
> Bu formu ince veya kendinizinkini oluşturun çekinmeyin.

## <a name="create-your-app"></a>Uygulamanızı oluşturma

`MainWindow.xaml.cs` uygulamamızı denetleyen kodu içerir. Bizim açılan menüler doldurmak için ve birkaç Translator metin çevirisi ve Bing yazım denetimi tarafından kullanıma sunulan API çağırmak için kodu eklemek için sonraki birkaç bölümde ekleyeceğiz.

* Program başladığında ve `MainWindow` örneği `Languages` Translator Text API yöntemi, almak ve bizim dil seçimi açılan listelerini doldurmak için çağrılır. Bu, her oturum, başında bir kez gerçekleşir.
* Zaman **çevir** düğmesine tıklandığında, kullanıcının dili seçimi ve metin alınır, yazım denetimi, girişi üzerinde gerçekleştirilir ve kullanıcı için algılanan dilin ve çeviri görüntülenir.
  * `Translate` Metni çevirmek için Translator Text API yöntemi çağrıldığında `TextToTranslate`. Bu çağrı yöntemlerine `to` ve `from` açılan menüleri kullanarak seçilen diller.
  * `Detect` Translator Text API yöntemi, metin dili belirlemek için çağrılır `TextToTranslate`.
  * Bing yazım denetimi doğrulamak için kullanılan `TextToTranslate` ve yazım hataları ayarlayın.

Tüm Projemizin saklanmış olduğu içinde `MainWindow : Window` sınıfı. Abonelik anahtarınızı ayarlayın, Translator metin çevirisi ve Bing yazım denetimi için uç noktalar bildirmek ve uygulamayı başlatmak için kod ekleyerek başlayalım.

1. Visual Studio'da sekmesini seçin `MainWindow.xaml.cs`.
2. Önceden doldurulmuş değiştirin `using` deyimlerini aşağıdaki.  
   ```csharp
   using System;
   using System.Windows;
   using System.Net;
   using System.Net.Http;
   using System.IO;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using Newtonsoft.Json;
   ```
3. Bulun `MainWindow : Window` sınıfı ve şu kodla değiştirin:
   ```csharp
   {
       // This sample uses the Cognitive Services subscription key for all services. To learn more about
       // authentication options, see: https://docs.microsoft.com/azure/cognitive-services/authentication.
       const string COGNITIVE_SERVICES_KEY = "YOUR_COG_SERVICES_KEY";
       // Endpoints for Translator Text and Bing Spell Check
       public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api- version=3.0";
       const string BING_SPELL_CHECK_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/bing/v7.0/spellcheck/";
       // An array of language codes
       private string[] languageCodes;

       // Dictionary to map language codes from friendly name (sorted case-insensitively on language name)
       private SortedDictionary<string, string> languageCodesAndTitles =
           new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

       // Global exception handler to display error message and exit
       private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
       {
           Exception e = (Exception)args.ExceptionObject;
           MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
           System.Windows.app.Current.Shutdown();
       }
       // MainWindow constructor
       public MainWindow()
       {
           // Display a message if unexpected error is encountered
           AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

           if (COGNITIVE_SERVICES_KEY.Length != 32)
           {
               MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                   "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                   "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
               System.Windows.app.Current.Shutdown();
           }
           else
           {
               // Start GUI
               InitializeComponent();
               // Get languages for drop-downs
               GetLanguagesForTranslate();
               // Populate drop-downs with values from GetLanguagesForTranslate
               PopulateLanguageMenus();
           }
       }
   // NOTE:
   // In the following sections, we'll add code below this.
   }
   ```
   1. Bilişsel hizmetler abonelik anahtarınızı ekleyin ve kaydedin.

Bu kod bloğunda biz çevirisi kullanılabilir diller hakkında bilgi içeren iki üye değişkenleri bildirdikten:

| Değişken | Tür | Açıklama |
|----------|------|-------------|
|`languageCodes` | dize dizisi |C dil kodlarını aches. Translator hizmeti dilleri belirlemek için kısa kodlar kullanır (örneğin İngilizce için `en`). |
|`languageCodesAndTitles` | Sıralanmış sözlük | Kullanıcı arabirimindeki "kolay anlaşılır" adları, API’de kullanılan kısa kodlarla eşleştirir. Büyük küçük harf kullanımından bağımsız olarak alfabetik sırayla tutulur. |

Ardından, içinde `MainWindow` oluşturucusu, şu hata ile işleme eklediğiniz `HandleExceptions`. Bu, bir özel durum işlenmez varsa bir uyarı temin edilmesini sağlar. Ardından sağlanan abonelik anahtarı 32 karakter uzunluğunda olup olmadığını onaylamak için denetimi gerçekleştirin. Anahtar / 32 karakterden daha uzun altındaysa bir hata oluşturulur.

En az doğru uzunluğu anahtarları varsa `InitializeComponent()` çağrı bulma, yükleme ve ana uygulama penceresini XAML açıklaması örnekleme sıralı kullanıcı arabirimini alır.

Son olarak, uygulama kullanıcı arabirimi için aşağı açılır menüler doldurmak için çeviri dilleri alınacak yöntemleri çağırmak için kod ekledik. Merak etmeyin, biz bu çağrılar arkasındaki kodda yakında elde edersiniz.

## <a name="get-supported-languages"></a>Desteklenen dilleri alma

Translator metin çevirisi API'si şu anda, 60'tan fazla dilleri de destekler. Zaman içinde yeni dil desteği eklenecektir olduğundan, runbook'a kod uygulamanızı dil listesinde yerine Translator metin çevirisi tarafından kullanıma sunulan diller kaynak çağırma öneririz.

Bu bölümde, oluşturacağız bir `GET` kullanılabilir dillerin bir listesi için çeviri istediğimizi belirten dilleri kaynağına isteği.

> [!NOTE]
> Dil kaynağı dil desteği aşağıdaki sorgu parametrelerinin filtrelemenize olanak sağlar: Harf çevirisi, sözlük ve çeviri. Daha fazla bilgi için [API Başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

Size daha fazla ilerlemeden, örnek çıktı dilleri kaynak çağrısı için bir göz atalım:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    }
    // Additional languages are provided in the full JSON output.
}
```

Bu çıktıdan, biz dil kodu ayıklayabilir ve `name` belirli bir dili. JSON nesnesi seri durumdan çıkarılacak NewtonSoft.Json uygulamamızı kullanır ([`JsonConvert.DeserializeObject`](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm)).

Burada şu son bölümde kaldığı yukarı çekme, uygulamamız için desteklenen diller almak için bir yöntem ekleyelim.

1. Visual Studio'da açmak için sekmesinde `MainWindow.xaml.cs`.
2. Bu kod projenize ekleyin:
   ```csharp
   // ***** GET TRANSLATABLE LANGUAGE CODES
   private void GetLanguagesForTranslate()
   {
       // Send request to get supported language codes
       string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
       WebRequest WebRequest = WebRequest.Create(uri);
       WebRequest.Headers.Add("Accept-Language", "en");
       WebResponse response = null;
       // Read and parse the JSON response
       response = WebRequest.GetResponse();
       using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
       {
           var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
           var languages = result["translation"];

           languageCodes = languages.Keys.ToArray();
           foreach (var kv in languages)
           {
               languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
           }
       }
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

`GetLanguagesForTranslate()` Yöntemi bir HTTP GET isteği oluşturur ve `scope=translation` çeviri için desteklenen diller isteği kapsamını sınırlamak için kullanılan sorgu dizesi parametresi. Desteklenen dillerin İngilizce olarak döndürülmesi için `en` değerine sahip `Accept-Language` üst bilgisi eklenir.

JSON yanıtı ayrıştırılır ve bir sözlüğe dönüştürülür. Dil kodları eklenen sonra `languageCodes` üye değişkeni. Dil kodlarını ve kolay dil adlarını içeren anahtar/değer çiftleri döndürülür ve `languageCodesAndTitles` üye değişkenine eklenir. Form açılır menüleri kolay adlarını görüntülemek, ancak kodları çeviri istemek için gereklidir.

## <a name="populate-language-drop-down-menus"></a>Dil açılan menüler Doldur

Kullanıcı arabirimi çok yanı sıra arama ayarlamak için yapmanız gerekmez, XAML kullanarak tanımlanan `InitializeComponent()`. Yapmanız gereken tek şey kolay dil adlarına eklemektir **gelen çevir** ve **için çevir** açılan menüler, bu işlem ile `PopulateLanguageMenus()` yöntemi.

1. Visual Studio'da açmak için sekmesinde `MainWindow.xaml.cs`.
2. Bu kod, projenize eklemek `GetLanguagesForTranslate()` yöntemi:
   ```csharp
   private void PopulateLanguageMenus()
   {
       // Add option to automatically detect the source language
       FromLanguageComboBox.Items.Add("Detect");

       int count = languageCodesAndTitles.Count;
       foreach (string menuItem in languageCodesAndTitles.Keys)
       {
           FromLanguageComboBox.Items.Add(menuItem);
           ToLanguageComboBox.Items.Add(menuItem);
       }

       // Set default languages
       FromLanguageComboBox.SelectedItem = "Detect";
       ToLanguageComboBox.SelectedItem = "English";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Bu yöntem üzerinden yinelenir `languageCodesAndTitles` sözlük ve her iki menülere her anahtar ekler. Menüler doldurulduktan sonra gelen ve dilleri varsayılan ayarlanmış **Algıla** ve **İngilizce** sırasıyla.

> [!TIP]
> Menüler için varsayılan bir seçim olmadan, kullanıcı önce bir "hedef" veya "kaynak" dil seçmeden **Çevir**’e tıklayabilir. Varsayılan değerler bu sorunla başa çıkma gereksinimini ortadan kaldırır.

Şimdi `MainWindow` başlatıldı ve oluşturulan kullanıcı arabiriminin kadar bu kod çalıştırılmaz **çevir** düğmesine tıklandığında.

## <a name="detect-language-of-source-text"></a>Metnin kaynak dilini algılar

Kaynak metni (metin alanımıza girilen metin) dili algılamak için yöntemi oluşturmak için yapacağız şimdi Translator metin çevirisi API'si kullanarak. Bu istek tarafından döndürülen değer müşterilerimizin çeviri isteği daha sonra kullanılır.

1. Visual Studio'da açmak için sekmesinde `MainWindow.xaml.cs`.
2. Bu kod, projenize eklemek `PopulateLanguageMenus()` yöntemi:
   ```csharp
   // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
   private string DetectLanguage(string text)
   {
       string detectUri = string.Format(TEXT_TRANSLATION_API_ENDPOINT ,"detect");

       // Create request to Detect languages with Translator Text
       HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(detectUri);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
       detectLanguageWebRequest.ContentType = "application/json; charset=utf-8";
       detectLanguageWebRequest.Method = "POST";

       // Send request
       var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
       string jsonText = serializer.Serialize(text);

       string body = "[{ \"Text\": " + jsonText + " }]";
       byte[] data = Encoding.UTF8.GetBytes(body);

       detectLanguageWebRequest.ContentLength = data.Length;

       using (var requestStream = detectLanguageWebRequest.GetRequestStream())
           requestStream.Write(data, 0, data.Length);

       HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

       // Read and parse JSON response
       var responseStream = response.GetResponseStream();
       var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
       dynamic jsonResponse = serializer.DeserializeObject(jsonString);

       // Fish out the detected language code
       var languageInfo = jsonResponse[0];
       if (languageInfo["score"] > (decimal)0.5)
       {
           DetectedLanguageLabel.Content = languageInfo["language"];
           return languageInfo["language"];
       }
       else
           return "Unable to confidently detect input language.";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Bu yöntemi bir HTTP oluşturur `POST` Algıla kaynak isteği. Tek bir bağımsız değişken alan `text`, hangi geçirilen boyunca istek gövdesi olarak. Daha sonra biz çeviri isteği, kullanıcı Arabirimimizi girilen metni oluşturduğumuzda dil algılama için bu yönteme geçirilir.

Ayrıca, bu yöntem, yanıtın güvenilirlik puanı değerlendirir. Puan büyükse `0.5`, algılanan dilin bizim kullanıcı arabiriminde görüntülenir.

## <a name="spell-check-the-source-text"></a>Yazım denetimi kaynak metni

Yazım için bir yöntem oluşturma oluşturacağız şimdi Bing yazım denetimi API'sini kullanarak müşterilerimize kaynak metin denetleyin. Bu, size geri doğru çevirileri sunduğu Translator metin API'si elde edersiniz sağlar. Kaynak metin düzeltmeleri boyunca müşterilerimizin çeviri geçirilir ne zaman istek **çevir** düğmesine tıklandığında.

1. Visual Studio'da açmak için sekmesinde `MainWindow.xaml.cs`.
2. Bu kod, projenize eklemek `DetectLanguage()` yöntemi:

```csharp
// ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
private string CorrectSpelling(string text)
{
    string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

    // Create a request to Bing Spell Check API
    HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
    spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
    spellCheckWebRequest.Method = "POST";
    spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

    // Create and send the request
    string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
    byte[] data = Encoding.UTF8.GetBytes(body);
    spellCheckWebRequest.ContentLength = data.Length;
    using (var requestStream = spellCheckWebRequest.GetRequestStream())
        requestStream.Write(data, 0, data.Length);
    HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

    // Read and parse the JSON response; get spelling corrections
    var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
    var responseStream = response.GetResponseStream();
    var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
    dynamic jsonResponse = serializer.DeserializeObject(jsonString);
    var flaggedTokens = jsonResponse["flaggedTokens"];

    // Construct sorted dictionary of corrections in reverse order (right to left)
    // This ensures that changes don't impact later indexes
    var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
    for (int i = 0; i < flaggedTokens.Length; i++)
    {
        var correction = flaggedTokens[i];
        var suggestion = correction["suggestions"][0];  // Consider only first suggestion
        if (suggestion["score"] > (decimal)0.7)         // Take it only if highly confident
            corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
    }

    // Apply spelling corrections, in order, from right to left
    foreach (int i in corrections.Keys)
    {
        var oldtext = corrections[i][0];
        var newtext = corrections[i][1];

        // Apply capitalization from original text to correction - all caps or initial caps
        if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
        else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

        text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
    }
    return text;
}
// NOTE:
// In the following sections, we'll add code below this.
```

## <a name="translate-text-on-click"></a>' E tıklayın, metin çevirme

Gerçekleştirmeniz gereken son oluşturan bir yöntem ise zaman çağrılır **çevir** kullanıcı arabirimimizi düğmesine tıklandığında.

1. Visual Studio'da açmak için sekmesinde `MainWindow.xaml.cs`.
2. Bu kod, projenize eklemek `CorrectSpelling()` yöntemi ve kaydedin:  
   ```csharp
   // ***** PERFORM TRANSLATION ON BUTTON CLICK
   private async void TranslateButton_Click(object sender, EventArgs e)
   {
       string textToTranslate = TextToTranslate.Text.Trim();

       string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
       string fromLanguageCode;

       // auto-detect source language if requested
       if (fromLanguage == "Detect")
       {
           fromLanguageCode = DetectLanguage(textToTranslate);
           if (!languageCodes.Contains(fromLanguageCode))
           {
               MessageBox.Show("The source language could not be detected automatically " +
                   "or is not supported for translation.", "Language detection failed",
                   MessageBoxButton.OK, MessageBoxImage.Error);
               return;
           }
       }
       else
           fromLanguageCode = languageCodesAndTitles[fromLanguage];

       string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

       // spell-check the source text if the source language is English
       if (fromLanguageCode == "en")
       {
           if (textToTranslate.StartsWith("-"))    // don't spell check in this case
               textToTranslate = textToTranslate.Substring(1);
           else
           {
               textToTranslate = CorrectSpelling(textToTranslate);
               TextToTranslate.Text = textToTranslate;     // put corrected text into input field
           }
       }
       // handle null operations: no text or same source/target languages
       if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
       {
           TranslatedTextLabel.Content = textToTranslate;
           return;
       }

       // send HTTP request to perform the translation
       string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
       string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

       System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
       var requestBody = JsonConvert.SerializeObject(body);

       using (var client = new HttpClient())
       using (var request = new HttpRequestMessage())
       {
           request.Method = HttpMethod.Post;
           request.RequestUri = new Uri(uri);
           request.Content = new StringContent(requestBody, Encoding.UTF8, "app/json");
           request.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
           request.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
           request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

           var response = await client.SendAsync(request);
           var responseBody = await response.Content.ReadAsStringAsync();

           var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
           var translation = result[0]["translations"][0]["text"];

           // Update the translation field
           TranslatedTextLabel.Content = translation;
       }
   }
   ```

İlk adım almaktır "Kimden" ve "için" diller ve kullanıcının bizim forma girdiği metin. Kaynak dili ayarlanırsa **Algıla**, `DetectLanguage()` kaynak metnin dilini belirlemek için çağrılır. Metin çevirmeni API'si desteklemeyen bir dilde olabilir. Bu durumda, kullanıcıyı bilgilendirmeniz ve metin çevirme olmadan döndürmek için bir ileti görüntüler.

Kaynak dil İngilizce ise (belirtilerek veya algılanarak), metnin yazımını `CorrectSpelling()` ile denetleyin ve düzeltmeleri uygulayın. Kullanıcı bir düzeltme yapıldı görebilmesi için düzeltilmiş metin metin alanına eklenir.

Kod metni çevirmek için tanıdık gelecektir: derleme URİ'si, bir istek oluşturun, gönderin ve yanıtı ayrıştıramadı. Birden fazla nesne çevirisi için JSON dizisi içerebilir, ancak uygulamamızı yalnızca bir gerektirir.

Başarılı bir isteği sonra `TranslatedTextLabel.Content` ile değiştirilir `translation`, çevrilmiş metni görüntülemek için kullanıcı arabirimi güncelleştirir.

## <a name="run-your-wpf-app"></a>WPF uygulamanızı çalıştırma

İşte bu kadar WPF kullanılarak oluşturulan bir çalışma çeviri uygulamanız olur. Uygulamanızı çalıştırmak için tıklayın **Başlat** Visual Studio'daki düğmesi.

## <a name="source-code"></a>Kaynak kod

Bu proje için kaynak kodu Github'da kullanılabilir.

* [Kaynak kodu keşfedin](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp-Tutorial)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Microsoft Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
