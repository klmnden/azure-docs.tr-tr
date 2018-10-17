---
title: 'Öğretici: C# kullanarak Translator Metin Çevirisi için bir WPF uygulaması yazma'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, C# kullanarak bir WPF uygulaması oluşturacak ve metinleri çevirmek, desteklenen dillerin yerelleştirilmiş bir listesini almak ve daha fazlasını yapmak için Translator Metin Çevirisi API’sini kullanmayı öğreneceksiniz.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: tutorial
ms.date: 07/20/2018
ms.author: nolachar
ms.openlocfilehash: 97660985b275bbe4384acb3fc92be8aaa0b57881
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123972"
---
# <a name="tutorial-write-a-wpf-application-for-translator-text-using-c35"></a>Öğretici: Translator Metin Çevirisi için C&#35; kullanarak bir WPF uygulaması yazma

Bu öğreticide, Azure’da Microsoft Bilişsel Hizmetler’in bir parçası olan Translator Metin Çevirisi API'sini (V3) kullanarak etkileşimli bir metin çevirisi aracı oluşturacaksınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Hizmet tarafından desteklenen dillerin listesini alma
> * Kullanıcı tarafından girilen metnin bir dilden başka bir dile çevirisini yapma

Bu uygulama, Microsoft Bilişsel Hizmetler kapsamında sunulan hizmetlerden ikisiyle tümleştirme de sunar.

|||
|-|-|
|[Metin Analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|İsteğe bağlı olarak, çevrilecek metnin kaynak dilini otomatik olarak algılamak için kullanılır|
|[Bing Yazım Denetimi](https://azure.microsoft.com/services/cognitive-services/spell-check/)|İngilizce kaynak metinde, çevirinin daha doğru olmasını sağlamak amacıyla yazım hatalarını düzeltmek için kullanılır

![[Çalışan öğretici program]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Ön koşullar

Bu kodu Windows’da çalıştırmak için [Visual Studio 2017](https://www.visualstudio.com/downloads/)’ye ihtiyacınız olacak. (Ücretsiz Community Edition’ı kullanabilirsiniz.)

Ayrıca programda kullanılan üç Azure hizmeti için abonelik anahtarlarınızın olması gerekir. Azure panosundan Translator Metin Çevirisi hizmeti için bir anahtar alabilirsiniz. Ayda iki milyon karaktere kadar ücretsiz olarak çeviri yapmanıza olanak sağlayan ücretsiz bir fiyatlandırma katmanı bulunur.

Hem Metin Analizi hem de Bing Yazım Denetimi hizmetleri, [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/) bölümünde kaydolabileceğiniz ücretsiz denemeler sunar. Ayrıca Azure panosunu kullanarak iki hizmet için de abonelik oluşturabilirsiniz. Metin Analizi ücretsiz bir katmana sahiptir.

Bu öğretici için kaynak kod aşağıdadır. Abonelik anahtarlarınızın `MainWindow.xaml.cs` içinde kaynak koda `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY` değişkeni olarak kopyalanması gerekir.

> [!IMPORTANT]
> Metin Analizi hizmeti birden çok bölgede kullanılabilir. Bu öğretici kaynak kodunda, ücretsiz denemeler için kullanılan `westus` bölgesindedir. Başka bir bölgede aboneliğiniz varsa, bu URI’yi uygun şekilde güncelleştirin.

## <a name="source-code"></a>Kaynak kod

Bu Microsoft Translator metin çevirisi API’sinin kaynak kodudur. Uygulamayı çalıştırmak için, kaynak kodu Visual Studio’da yeni bir WPF projesinde uygun bir dosyaya yapıştırın.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Bu, uygulamanın işlevselliğini sağlayan arka plan kod dosyasıdır.

```csharp
using System;
using System.Windows;
using System.Net;
using System.Net.Http;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text;

// NOTE: Add assembly references to System.Runtime.Serialization, System.Web, and System.Web.Extensions.

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace MSTranslatorTextDemo
{
    /// <summary>
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string from
    /// one language to another. The languages are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected. English text is spell-checked.
    /// </summary>
    public partial class MainWindow : Window
    {
        // Translator text subscription key from Microsoft Azure dashboard
        const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

        public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
        const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
        const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

        private string[] languageCodes;     // array of language codes

        // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
        private SortedDictionary<string, string> languageCodesAndTitles =
            new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

        public MainWindow()
        {
            // at least show an error dialog if there's an unexpected error
            AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

            if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
                || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
                || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
            {
                MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                    "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                    "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
                System.Windows.Application.Current.Shutdown();
            }
            else
            {
                InitializeComponent();          // start the GUI
                GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
                PopulateLanguageMenus();        // fill the drop-down language lists
            }
        }

        // Global exception handler to display error message and exit
        private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
        {
            Exception e = (Exception)args.ExceptionObject;
            MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }

        // ***** POPULATE LANGUAGE MENUS
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

            // set default languages
            FromLanguageComboBox.SelectedItem = "Detect";
            ToLanguageComboBox.SelectedItem = "English";
        }

        // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
        private string DetectLanguage(string text)
        {
            string uri = TEXT_ANALYTICS_API_ENDPOINT + "languages?numberOfLanguagesToDetect=1";

            // create request to Text Analytics API
            HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_ANALYTICS_API_SUBSCRIPTION_KEY);
            detectLanguageWebRequest.Method = "POST";

            // create and send body of request
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            string jsonText = serializer.Serialize(text);

            string body = "{ \"documents\": [ { \"id\": \"0\", \"text\": " + jsonText + "} ] }";
            byte[] data = Encoding.UTF8.GetBytes(body);
            detectLanguageWebRequest.ContentLength = data.Length;

            using (var requestStream = detectLanguageWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);

            HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

            // read and parse JSON response
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);

            // fish out the detected language code
            var languageInfo = jsonResponse["documents"][0]["detectedLanguages"][0];
            if (languageInfo["score"] > (decimal)0.5)
                return languageInfo["iso6391Name"];
            else
                return "";
        }

        // ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
        private string CorrectSpelling(string text)
        {
            string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

            // create request to Bing Spell Check API
            HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", BING_SPELL_CHECK_API_SUBSCRIPTION_KEY);
            spellCheckWebRequest.Method = "POST";
            spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

            // create and send body of request
            string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
            byte[] data = Encoding.UTF8.GetBytes(body);
            spellCheckWebRequest.ContentLength = data.Length;
            using (var requestStream = spellCheckWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);
            HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

            // read and parse JSON response and get spelling corrections
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);
            var flaggedTokens = jsonResponse["flaggedTokens"];

            // construct sorted dictionary of corrections in reverse order in string (right to left)
            // so that making a correction can't affect later indexes
            var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
            for (int i = 0; i < flaggedTokens.Length; i++)
            {
                var correction = flaggedTokens[i];
                var suggestion = correction["suggestions"][0];  // consider only first suggestion
                if (suggestion["score"] > (decimal)0.7)         // take it only if highly confident
                    corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                        { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
            }

            // apply the corrections in order from right to left
            foreach (int i in corrections.Keys)
            {
                var oldtext = corrections[i][0];
                var newtext = corrections[i][1];

                // apply capitalization from original text to correction - all caps or initial caps
                if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
                else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

                text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
            }

            return text;
        }

        // ***** GET TRANSLATABLE LANGUAGE CODES
        private void GetLanguagesForTranslate()
        {
            // send request to get supported language codes
            string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
            WebRequest WebRequest = WebRequest.Create(uri);
            WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
            WebRequest.Headers.Add("Accept-Language", "en");
            WebResponse response = null;
            // read and parse the JSON response
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
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
                request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
                var translations = result[0]["translations"];
                var translation = translations[0]["text"];

                // Update the translation field
                TranslatedTextLabel.Content = translation;
            }
        }
    }
}
```

### <a name="mainwindowxaml"></a>MainWindow.xaml

Bu dosya bir uygulama için kullanıcı arabirimini, bir WPF formunu tanımlar. Formun kendinize ait bir sürümünü tanımlamak istiyorsanız, bu XAML’ye ihtiyacınız yoktur.

```xml
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
        <Label x:Name="TranslatedTextLabel" Content="Translation appears here" HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
    </Grid>
</Window>
```

## <a name="service-endpoints"></a>Hizmet uç noktaları

Microsoft Translator hizmeti, çeşitli çeviri işlevleri sağlayan birkaç uç noktaya sahiptir. Bu öğreticide kullanılanlar şunlardır:

|||
|-|-|
|`Languages`|Şu anda Translator Metin Çevirisi API’sinin diğer işlemleri tarafından desteklenen dillerin kümesini döndürür.|
|`Translate`|Kaynak metin, kaynak dil kodu ve hedef dil kodu belirtildiğinde, kaynak metnin hedef dile bir çevirisi döndürülür.|

## <a name="the-translation-app"></a>Çeviri uygulaması

Çeviri uygulamasının kullanıcı arabirimi Windows Presentation Foundation (WPF) kullanılarak oluşturulmuştur. Aşağıdaki adımları izleyerek Visual Studio’da yeni bir WPF projesi oluşturun.

* **Dosya** menüsünden, **Yeni > Proje**’yi seçin.
* Yeni Proje penceresinde, **Yüklenen > Şablonlar > Visual C#**’yi açın. Kullanılabilir proje şablonlarının bir listesi iletişim kutusunun ortasında görüntülenir.
* Proje şablonu listesinin üzerindeki açılır menüde **.NET Framework 4.5.2** öğesinin seçili olduğundan emin olun.
* Proje şablonu listesinde **WPF Uygulaması (.NET Framework)** seçeneğine tıklayın.
* İletişimin altındaki alanları kullanarak yeni projeyi ve bunu içeren çözümü adlandırın.
* Yeni projeyi ve çözümü oluşturmak için **Tamam**’a tıklayın.

![[Visual Studio’da yeni bir WPF uygulaması oluşturma]](media/translator-text-csharp-new-project.png)

Projenize aşağıdaki .NET çerçevesi derlemelerine başvurular ekleyin.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Ayrıca, projenize `Newtonsoft.Json` NuGet paketini yükleyin.

Şimdi `MainWindow.xaml` dosyasını Çözüm Gezgini’nde bulup açın. Başlangıçta boştur. Tamamlanmış kullanıcı arabirimi denetimlerin adları mavi ile belirtilerek şöyle görünmelidir. Denetimler kodda da göründüğü için, kullanıcı arabiriminizde bunlar için aynı adları kullanın.

![[Visual Studio tasarımcısında ana pencerenin açıklamalı görünümü]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> Bu öğreticinin kaynak kodu, bu formun XAML kaynağını içerir. Formu Visual Studio’da oluşturmak yerine projenize yapıştırabilirsiniz.

* `FromLanguageComboBox` *(ComboBox)* - Microsoft Translator tarafından metin çevirisi için algılanan dillerin bir listesini görüntüler. Kullanıcı çeviri yaptığı kaynak dili seçer.
* `ToLanguageComboBox` *(ComboBox)* - `FromComboBox` ile aynı dil listesini gösterir ancak kullanıcının çeviri yaptığı hedef dili seçmek için kullanılır.
* `TextToTranslate` *(TextBox)* - Kullanıcı çevrilecek metni buraya girer.
* `TranslateButton` *(Button)* - Kullanıcı metni çevirmek için bu düğmeye tıklar (veya Enter’a basar).
* `TranslatedTextLabel` *(Label)* - Kullanıcının metninin çevirisi burada görüntülenir.

Bu formun kendinize ait bir sürümünü oluşturuyorsanız, burada kullanılanın *tam olarak* aynısını yapmanıza gerek yoktur. Ancak dil adlarının kesilmesini önlemek için açılan menülerin yeterince geniş olduğundan emin olmanız gerekir.

## <a name="the-mainwindow-class"></a>MainWindow sınıfı

`MainWindow.xaml.cs` arka plan kod dosyası programın işlevini gerçekleştirmesi için kodların gittiği yerdir. İş iki kez gerçekleşir:

* Program başlatıldığında ve `MainWindow` örneklendirildiğinde, Translator ve API’leri kullanarak dil listesini alır ve bunlarla açılan menüleri doldurur. Bu görev her oturumun başında bir kez gerçekleştirilir.

* Kullanıcı **Çevir** düğmesine tıkladığında, kullanıcının dil seçimleri ile girdiği metin alınır ve çeviriyi gerçekleştirmek için `Translate` API’si çağrılır. Çeviriden önce metnin dilini belirlemek ve yazımını düzeltmek için diğer işlevler de çağrılabilir.

Sınıfın başlangıcına göz atın:

```csharp
public partial class MainWindow : Window
{
    // Translator text subscription key from Microsoft Azure dashboard
    const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

    public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
    const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
    const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

    private string[] languageCodes;     // array of language codes

    // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
    private SortedDictionary<string, string> languageCodesAndTitles =
        new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

    public MainWindow()
    {
        // at least show an error dialog if there's an unexpected error
        AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

        if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
            || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
            || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
        {
            MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }
        else
        {
            InitializeComponent();          // start the GUI
            GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
            PopulateLanguageMenus();        // fill the drop-down language lists
        }
    }
// more to come
}
```

Burada bildirilen iki üye değişken dillerimiz hakkında bilgileri tutar:

|||
|-|-|
|`languageCodes`<br>dize dizisi|Dil kodlarını önbelleğe alır. Translator hizmeti dilleri belirlemek için kısa kodlar kullanır (örneğin İngilizce için `en`).|
|`languageCodesAndTitles`<br>SortedDictionary|Kullanıcı arabirimindeki "kolay anlaşılır" adları, API’de kullanılan kısa kodlarla eşleştirir. Büyük küçük harf kullanımından bağımsız olarak alfabetik sırayla tutulur.|

Uygulama tarafından yürütülen ilk kod `MainWindow` oluşturucusudur. İlk olarak, genel hata işleyici olarak `HandleExceptions` yöntemini ayarlayın. Bu şekilde, bir özel durum işlenmezse en azından bir hata uyarısı görüntülenmiş olur.

Daha sonra, API abonelik anahtarlarının tam olarak 32 karakter uzunluğunda olduğundan emin olun. Değilse, bunun en olası nedeni *birinin* API anahtarını yapıştırmamış olmasıdır. Bu durumda, bir hata iletisi görüntüleyip çıkın. (Bu testin geçilmesi, anahtarların geçerli olduğu anlamına gelmez.)

En azından doğru uzunlukta anahtarlar varsa, `InitializeComponent()` çağrısı ana uygulama penceresini bularak, yükleyerek ve XAML açıklamasını örneklendirerek kullanıcı arabirimini başlatır.

Son olarak ise dil açılır menülerini ayarlayın. Bu görev aşağıdaki bölümlerde ayrıntılarıyla açıklanan üç ayrı yöntem çağrısı gerektirir.

## <a name="get-supported-languages"></a>Desteklenen dilleri alma

Microsoft Translator hizmeti bu makalenin yazılması sırasında toplam 61 dili desteklemektedir ve zaman zaman daha fazla dil eklenmektedir. Bu nedenle programınızda desteklenen dilleri doğrudan yazmamanız faydalı olabilir. Bunun yerine Translator hizmetine desteklediği dilleri sorun. Desteklenen bir dil, desteklenen herhangi bir dile çevrilebilir.

Desteklenen dillerin listesini almak için `Languages` API’sini çağırın.

`Languages` API’si isteğe bağlı bir GET sorgu parametresi olan *scope* alır. *scope* şu üç değerden birine sahip olabilir: `translation`, `transliteration` ve `dictionary`. Bu kod `translation` değerini kullanır.

`Languages` API’si ayrıca isteğe bağlı bir HTTP üst bilgisi olan `Accept-Language` alır. Bu üst bilginin değeri, desteklenen dillerin adlarının döndürüleceği dili belirler. Değer iyi biçimlendirilmiş bir BCP 47 dil etiketi olmalıdır. Kod, dil adlarını İngilizce olarak almak için `en` kullanır.

`Languages` API’si aşağıdakine benzer bir JSON yanıtı döndürür.

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
    },
...
}
```

Dil kodlarının (örneğin, `af`) ve dil adlarının (örneğin, `Afrikaans`) ayıklanabilmesi için, bu kod [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) NewtonSoft.Json yöntemini kullanır.

Bu arka plan bilgisi ile, dil kodları ve adlarını almak için aşağıdaki yöntemi oluşturun.

```csharp
private void GetLanguagesForTranslate()
{
    // send request to get supported language codes
    string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
    WebRequest WebRequest = WebRequest.Create(uri);
    WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
    WebRequest.Headers.Add("Accept-Language", "en");
    WebResponse response = null;
    // read and parse the JSON response
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
```

`GetLanguagesForTranslate()` ilk olarak HTTP isteğini oluşturur. `scope=translation` sorgu dizesi parametresi yalnızca metin çevirisi için desteklenen dilleri ister. Metin Çevirisi API’si abonelik anahtarı istek üst bilgilerine eklenir. Desteklenen dillerin İngilizce olarak döndürülmesi için `en` değerine sahip `Accept-Language` üst bilgisi eklenir.

İstek tamamlandıktan sonra, JSON yanıtı ayrıştırılır ve bir Sözlüğe dönüştürülür ve ardından dil kodları `languageCodes` üye değişkenine eklenir. Dil kodlarını ve kolay dil adlarını içeren anahtar/değer çiftleri döndürülür ve `languageCodesAndTitles` üye değişkenine eklenir. (Formdaki açılan menüler kolay adları gösterir ancak çeviriyi istemek için kodlar gereklidir.)

## <a name="populate-the-language-menus"></a>Dil menülerini doldurma

Kullanıcı arabiriminin çoğu XAML içinde tanımlanmıştır, bu nedenle ayarlamak için `InitializeComponent()` öğesini çağırmak dışında yapmanız gereken fazla işlem yoktur. Yapmanız gereken diğer tek şey kolay dil adlarını **Şu dilden çevir** ve **Şu dile çevir** açılır menülerine eklemektir. Bunu `PopulateLanguageMenus()` içinde yapabilirsiniz.

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

    // set default languages
    FromLanguageComboBox.SelectedItem = "Detect";
    ToLanguageComboBox.SelectedItem = "English";
}
```

Menüleri doldurmak, “kolay” ad olan her bir anahtarı iki menüye de ekleyerek `languageCodesAndTitles` sözlüğünü yinelemeyi içeren basit bir iştir. Menüleri doldurduktan sonra, varsayılan "hedef" ve "kaynak" diller **Algıla** (dili otomatik algılamak için) ve **İngilizce** olarak ayarlanır.

> [!TIP]
> Menüler için varsayılan bir seçim olmadan, kullanıcı önce bir "hedef" veya "kaynak" dil seçmeden **Çevir**’e tıklayabilir. Varsayılan değerler bu sorunla başa çıkma gereksinimini ortadan kaldırır.

`MainWindow` başlatıldığına ve kullanıcı arabirimi oluşturulduğuna göre, kod kullanıcı **Çevir** düğmesine basana kadar bekler.

## <a name="perform-translation"></a>Çeviri gerçekleştirme

Kullanıcı **Çevir**’e tıkladığında, WPF burada gösterilen `TranslateButton_Click()` olay işleyicisini çağırır.

```csharp
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
        request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
        request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();

        var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
        var translations = result[0]["translations"];
        var translation = translations[0]["text"];

        // Update the translation field
        TranslatedTextLabel.Content = translation;
    }
}
```

İlk adım, formdan "hedef" ve "kaynak" diller ile kullanıcının girdiği metni almaktır.

Kaynak dil **Algıla** olarak ayarlandıysa, metnin dilini belirlemek için `DetectLanguage()` öğesine bir çağrı yapın. Metin Translator API’sinin desteklemediği bir dilde olabilir (çevrilebilen dillerden çok daha fazla dil algılanır) veya Metin Analizi API’si metni algılamayabilir. Bu durumda kullanıcıya bunu bildirmek için bir ileti görüntüleyin ve çevirmeden dönün.

Kaynak dil İngilizce ise (belirtilerek veya algılanarak), metnin yazımını `CorrectSpelling()` ile denetleyin ve düzeltmeleri uygulayın. Düzeltilen metin tekrar giriş alanına yerleştirilir, böylece kullanıcı düzeltmenin yapıldığını anlar. (Kullanıcı yazım düzeltmesi uygulanmaması için çevrilen metnin başına kısa çizgi ekleyebilir.)

Kullanıcı herhangi bir metin girmediyse, veya “hedef” ve “kaynak” dil aynıysa, çeviriye gerek yoktur ve istekten kaçınılabilir.

Çeviri isteğini gerçekleştirmek için kod tanıdık görünmelidir: URI’yi oluşturun, istek oluşturun, gönderin ve yanıtı ayrıştırın. Metni görüntülemek için, `TranslatedTextLabel` denetimine gönderin.

Daha sonra, metni bir POST isteği gövdesinde serileştirilmiş bir JSON dizisi içinde `Translate` API’sine geçirin. JSON dizisi çevrilecek birden çok metin içerebilir ancak burada yalnızca bir tane gereklidir.

`X-ClientTraceId` adlı HTTP üst bilgisi isteğe bağlıdır. Değer, bir GUID olmalıdır. İstemci tarafından sağlanan iz kimliği bir şeyler beklendiği gibi gitmediğinde istekleri izlemek için yararlıdır. Ancak kullanışlı olması için, X-ClientTraceID değerinin istemci tarafından kaydedilmesi gerekir. İstemci izleme kimliği ve isteklerin tarihi Microsoft’un oluşabilecek sorunları tanılamasına yardımcı olabilir.

> [!NOTE]
> Bu öğretici Microsoft Translator hizmetine odaklanmaktadır, bu nedenle `DetectLanguage()` ve `CorrectSpelling()` yöntemleri ayrıntılı olarak ele alınmaz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Microsoft Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
