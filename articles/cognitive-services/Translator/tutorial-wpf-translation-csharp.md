---
title: C#, Azure Bilişsel hizmetler ile Microsoft Çeviricisi WPF uygulama yazmak nasıl | Microsoft Docs
description: Çevirici metin hizmeti metin çevirme, desteklenen diller ve daha fazlasını yerelleştirilmiş listesini almak için nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.devlang: csharp
ms.topic: article
ms.date: 10/25/2017
ms.author: v-jansko
ms.openlocfilehash: fb58fd087de09561a0ea930748562e595d3dde1c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352601"
---
# <a name="how-to-write-a-microsoft-translator-wpf-application-in-c"></a>Microsoft Çeviricisi WPF uygulaması C# dilinde yazma

Bu öğreticide, biz Microsoft Çeviricisi metin API (V3) bir parçası olan Microsoft Bilişsel hizmetler Azure kullanarak bir etkileşimli metin çeviri aracı yapı. Bilgi edineceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Hizmet tarafından desteklenen diller için kısa kodlarının listesini isteği
> * Bu dil kodlarını karşılık gelen yerelleştirilmiş dil adlarının bir listesini alma
> * Kullanıcı tarafından girilen metin çevrilmiş sürümü bir dilden diğerine alın

Bu uygulamayı Ayrıca iki diğer Microsoft Hizmetleri ile tümleştirme Bilişsel sahiptir.

|||
|-|-|
|[Metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Çevrilecek metnin kaynağı dilini isteğe bağlı olarak otomatik olarak algılamak için kullanılan|
|[Bing yazım denetimi](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Çeviri daha doğru şekilde yazım hatalarını gidermek için kullanılan İngilizce kaynak metin

![[Öğretici program çalıştıran]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici yapmak için Visual Studio 2017 Community Edition dahil olmak üzere, herhangi bir sürümünü gerekir.

Ayrıca programda kullanılan üç Azure Hizmetleri için Abonelik anahtarları gerekir. Azure panodan Çeviricisi metin hizmeti için bir anahtar alabilirsiniz. Ücretsiz fiyatlandırma katmanını kullanılabilir ücretsiz başına en fazla iki milyon karakter Çevir izin verir.

Metin analizi hem Bing yazım denetimi Hizmetleri için oturum üzerinde teklif ücretsiz deneme [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/). Ayrıca Azure Pano aracılığıyla ya da hizmet için bir abonelik oluşturabilir. Metin analizi ücretsiz bir katman vardır.

Bu öğretici için kaynak kod aşağıda verilmiştir. Abonelik anahtarlarınızı kaynak koda değişkenleri olarak kopyalanmalıdır `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY`ve benzeri içinde `MainWindow.xaml.cs`.

> [!IMPORTANT]
> Metin analizi hizmeti birden çok bölgede kullanılabilir. URI bizim öğretici kaynak kodunda yer `westus` bölgedir bölge kullanılan ücretsiz deneme. Başka bir bölgede bir aboneliğiniz varsa, bu URI gerektiği gibi güncelleştirin.

## <a name="source-code"></a>Kaynak kod

Microsoft Translator metin API kaynak kodudur. Uygulamayı çalıştırmak için Visual Studio'da yeni bir WPF projesi uygun dosyasında kaynak kodunu kopyalayın.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Bu uygulamanın işlevselliğini sağlayan arka plan kodu dosyasıdır.

```cs
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
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string
    /// one language to another. The langauges are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected.  English text is spell-checked.
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
            // at least show an error dialog when we get an unexpected error
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

            // read and and parse JSON response
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

Bu dosya, uygulama, bir WPF formu için kullanıcı arabirimi tanımlar. Formun kendi sürümünün tasarlanmasına istiyorsanız, bu XAML gerekmez.

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

Microsoft Translator hizmeti çeşitli parçaları çeviri işlevselliği sağlayan çok sayıda uç noktalar vardır. Bu öğreticide kullanırız olanlardır:

|||
|-|-|
|`Languages`|Şu anda diğer Çeviricisi metin API işlemleri tarafından desteklenen diller kümesini döndürür.|
|`Translate`|Kaynak metni verilen, kaynağı dil kodu ve bir hedef dil kodu döndürür kaynak metnin çevirisi hedef dile.|

## <a name="the-translation-app"></a>Çeviri uygulama

Bizim Çeviricisi uygulamanın kullanıcı arabirimi, Windows Presentation Foundation (WPF) kullanılarak oluşturulur. Aşağıdaki adımları izleyerek Visual Studio'da yeni bir WPF projesi oluşturun.

* Gelen **dosya** menüsünde seçin **yeni > Proje**.
* Yeni Proje penceresinin açık **yüklü > şablonları > Visual C#**. Kullanılabilir proje şablonları listesi iletişim Center'da görüntülenir.
* Emin olun **.NET Framework 4.5.2** proje şablonu listesi yukarıdaki açılır menüde seçilir.
* Tıklatın **WPF uygulaması (.NET Framework)** proje şablonu listesinde.
* İletişim kutusunun alt kısmındaki alanlarını kullanarak, yeni projeyi ve içerdiği çözüm adı.
* Tıklatın **Tamam** yeni proje ve çözüm oluşturmak için.

![[Visual Studio'da yeni bir WPF uygulaması oluşturma]](media/translator-text-csharp-new-project.png)

Aşağıdaki .NET framework derlemelerini başvurular projenize ekleyin.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Ayrıca, NuGet paket yüklemesi `Newtonsoft.Json` projenize.

Şimdi Bul `MainWindow.xaml` dosya Çözüm Gezgini'nde ve açın. Başlangıçta boş olur. İşte bitti kullanıcı arabirimi, mavi denetimlerinde adlarıyla açıklamalı nasıl göründüğünü. Ayrıca kodda göründükleri beri denetimleri için aynı adları, kullanıcı arabiriminde kullanın.

![[Visual Studio Tasarımcısı'nda ana penceresinin açıklamalı görünümü]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> XAML kaynağı için bu formu Bu öğretici için kaynak kodunu içerir. Formun Visual Studio'da derleme yerine projenize yapıştırın.

* `FromLanguageComboBox` *(ComboBox)*  -Metin çeviri Microsoft Translator tarafından desteklenen dillerin listesini görüntüler. Kullanıcı, gelen çevirme dil seçer.
* `ToLanguageComboBox` *(ComboBox)*  -Diller olarak aynı listesini görüntüler `FromComboBox`, kullanıcı tercüme ederek dil seçmek için kullanılır, ancak.
* `TextToTranslate` *(Metin)*  -Kullanıcı burada Çevrilecek metin girer.
* `TranslateButton` *(Düğme)*  -Kullanıcı bu düğmeye tıklar (veya Enter tuşuna bastığında) metin çevrilemedi.
* `TranslatedTextLabel` *(Etiketli)*  -Kullanıcının metin çevirisi burada görünür.

Bu form kendi sürümü yapıyorsanız, bunu yapmak gerekli değildir *tam olarak* bizim ister. Ancak dil aşağı açılan listeler bir dil adı kapalı kesme önlemek için geniş olduğundan emin olun.

## <a name="the-mainwindow-class"></a>MainWindow sınıfı

Arka plan kodu dosyanın `MainWindow.xaml.cs` ne yaptığını yapmak program yapar kodu nereye gider olduğu. Çalışma sırasında iki kez olur:

* Program başladığında otomatik olarak Başlat Zaman `MainWindow` olan örneği, Çeviricisi'nın kullanarak dil listesi alıyoruz `GetLanguagesForTranslate` ve `GetLanguageNames` API'leri ve onlarla bizim aşağı açılır menüler doldurmak. Bu görev bir kez, her oturumunun başlangıcında gerçekleştirilir.

* Kullanıcı tıkladığında **çevir** düğmesini alıyoruz kullanıcının dil seçimleri ve bunlar girilen metin. Diyoruz sonra `Translate` API çevirileri gerçekleştirebilir. Biz de metin dili belirlemeye ve yazılmadığını çeviri önce düzeltmek için diğer işlevleri çağırabilir.

Bizim sınıfı başlamadan adresindeki nasıl bakalım:

```cs
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
        // at least show an error dialog when we get an unexpected error
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

Bildirilen iki üye değişkenleri burada bizim diller hakkında bilgi tutun:

|||
|-|-|
|`languageCodes`<br>dize dizisi|Dil kodlarını önbelleğe alır. Çevirici hizmeti kısa kodları gibi kullanır `en` dilleri tanımlamak için İngilizce '.|
|`languageCodesAndTitles`<br>SortedDictionary|MAPS API çağrısında kullanılan kısa kodları kullanıcı arabirimindeki "kolay" adları dön. Alfabetik olarak çalışması için bakmadan sıralanmış tutulur.|

Uygulamamız tarafından yürütülen ilk kodu `MainWindow` Oluşturucusu. İlk olarak, biz yöntemi ayarlamak `HandleExceptions` genel hata işleyici olarak. Herhangi bir özel durumun işlenip değil, bu şekilde, biz en az bir hata uyarı alırsınız.

Ardından, biz API Abonelik anahtarları tüm tam olarak 32 karakter uzunluğunda olduğundan emin olmak için kontrol edin. Yoksa, olan nedeni büyük olasılıkla *birisi* kendi API anahtarları (tsk) yapıştırılan kurmadı. Bu durumda, biz hata iletisi görüntüler ve bail. (Bu test geçirme değil anlamına anahtarları geçerli Elbette.)

Biz en az sağ uzunluğu anahtarları varsa `InitializeComponent()` çağrısı bulma, yükleme ve ana uygulama penceresi XAML açıklaması başlatmasını çalışırken kullanıcı arabirimini alır.

Son olarak, dil aşağı açılır menüler ayarlayın. Bu görev, üç ayrı bir yöntem çağrıları gerektirir. Bu yöntemler aşağıdaki bölümlerde ayrıntılı olarak ele gidin.

## <a name="get-supported-languages"></a>Desteklenen diller Al

Bu yazma 61 dil toplam Microsoft Translator hizmeti destekler ve daha fazla zaman zaman eklenebilir. Bu nedenle onu sahip en iyi olmayan sabit kodlu için programınızdaki desteklenen diller. Bunun yerine, Çevirici hizmet destekliyorsa hangi dilde isteyin. Desteklenen her dil için herhangi bir desteklenen dilde çevrilebilir.

Çağrı `Languages` API listesini almak için desteklenen diller.

`Languages` API gereken isteğe bağlı bir GET sorgu parametresi *kapsam*. *Kapsam* üç değerlerden biri olabilir: `translation`, `transliteration`, ve `dictionary`. Değer kullanacağız `translation`.

`Languages` API Ayrıca isteğe bağlı bir HTTP üstbilgi alan `Accept-Language`. Bu üstbilgi değerini desteklenen dillerin adlarını döndürülen dilini belirler. Değer doğru biçimlendirilmiş BCP 47 dil etiket olmalıdır. Değer kullanacağız `en` İngilizce dil adları alınamıyor.

`Languages` API şuna benzeyen bir JSON yanıtı döndürür.

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

Biz dil kodlarını ayıklamak istediğiniz (örneğin, `af`) ve dil adlarını (örneğin, `Afrikaans`). NewtonSoft.Json yöntemi kullanırız [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) Bunu yapmak için.

Bu arka plan bilgiyle dil kodlarını ve adlarını almak için aşağıdaki yöntemi oluşturun.

```cs
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

`GetLanguagesForTranslate()` ilk HTTP isteğini oluşturur. `scope=translation` Sorgu dizesi parametresi belirtir metin çevirisi için desteklenen dilleri istiyoruz. Bizim istek üstbilgileri metin çeviri API abonelik anahtarı ekleriz. Ayrıca eklediğimiz `Accept-Language` üstbilgi değeri ile `en` İngilizce döndürülen desteklenen diller istiyoruz belirtmek için.

İstek tamamlandıktan sonra biz JSON yanıtı ayrıştırılamadı ve bir sözlüğe dönüştürün. İçin dil kodlarını eklediğimiz `languageCodes` üye değişkeni. Biz sonra onlara ekleyin ve dil kodlarını ve kolay dil adlarını içeren anahtar/değer çiftleri döngü `languageCodesAndTitles` üye değişkeni. (Bizim formunda aşağı açılır menüler kolay adlarını görüntülemek, ancak biz çeviri isteği için kodlarını gerekir.)

## <a name="populate-the-language-menus"></a>Dil menüleri doldurma

Bizim kullanıcı arabirimi çoğunu biz çok çağrı yanı sıra ayarlamak için gerçekleştirmeniz gereken şekilde XAML içinde tanımlanan `InitializeComponent()`. İhtiyacımız yapmak için yalnızca diğer hangi yapılır Kime ve aşağı açılan listeler, kolay dil adlarını eklemek şeydir `PopulateLanguageMenus()`.

```cs
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

Menüleri doldurma olan basit sağlasa da, üzerinde yineleme `languageCodesAndTitles` sözlük ve "kolay" adı olan her anahtar hem menülere ekleme. Menüleri doldurduktan sonra varsayılan dili (otomatik dil algılama için) Algıla için gelen ve giden ayarlarız ve İngilizce.

> [!TIP]
> Biz bizim menüleri için varsayılan seçimi ayarlamazsanız, kullanıcı tıklatabilirsiniz **çevir** dilden veya bir Kime seçme olmadan. Varsayılanları Bu sorun dağıtılacak gereksinimini ortadan kaldırır.

Şimdi `MainWindow` kullanıcı arabirimi oluşturma başlatıldı. Denetim yeniden kadar kullanıcı tıklama almadım **çevir** düğmesi.

## <a name="perform-translation"></a>Çeviri gerçekleştir

Kullanıcı tıkladığında **çevir**, WPF çağırır `TranslateButton_Click()` burada gösterilen olay işleyicisi.

```cs
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

Burada, ilk Kime alıyoruz ve dillerden metnin yanı sıra kullanıcı, formdan girmiştir.

Kaynak dili için Algıla ayarlarsanız diyoruz `DetectLanguage()` metnin dilini belirlemek için. Metin olabilir Çeviricisi API'ları içermeyen bir dilde destek (çok daha fazla dil algılanan çevrilebilir daha) ya da metin Analytics API algılaması kuramamış olabilir. Bu durumda, kullanıcı bildirin ve çevirme olmadan dönmek için bir ileti görüntüler.

Varsa kaynak dili İngilizce (belirtilen veya algılanan) olup, biz metinle yazım `CorrectSpelling()` ve değişiklikleri uygulayın. Düzeltme yapıldığı kullanıcı bilmesi için düzeltilen metin girdi alanına basa. (Kullanıcı yazım düzeltme gizlemek için tireyle çevrildiğini metin önünde.)

Kullanıcının herhangi bir metin girilmezse veya Kime ve dillerden aynı çeviri gereklidir. Bu durumda istekte kaçının.

Çeviri isteği gerçekleştirmek için kod tanıdık gelecektir. Biz URI oluşturmak, bir istek oluşturun, göndermek ve yanıtı ayrıştırılamadı. Metni görüntülemek için içinde depolarız `TranslatedTextLabel` denetim.

Biz metne geçirmek `Translate` bir POST isteğinin gövdesinde serileştirilmiş bir JSON dizisindeki API. JSON dizisinin birden çok parça çevirmek için metin içerebilir, ancak burada yalnızca bir eklediğimiz.

Adlı bir HTTP üstbilgisi `X-ClientTraceId` isteğe bağlıdır. Değer bir GUID olmalıdır. Öğeleri beklendiği gibi çalışmıyor istemci tarafından sağlanan izleme kimliği izleme istekleri için faydalıdır. Ancak, kullanışlı olması için X ClientTraceID değerini istemci tarafından kaydedilmesi gerekir. Bir istemci izleme kimliği ve istekleri tarihini meydana gelebilecek sorunları tanılamak Microsoft yardımcı olabilir.

> [!NOTE]
> Kapak yok şekilde Bu öğretici Microsoft Translator hizmette odaklanır `DetectLanguage()` ve `CorrectSpelling()` ayrıntılı yöntemleri. Metin analizi ve Bing yazım denetimi Hizmetleri XML yerine JSON yanıtları sağlayın ve metin analizi isteğini JSON olarak da biçimlendirilmiş olması gerekir. Bu özellikleri zaten gördük yöntemleri çoğu kod farkları hesap.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Microsoft Çeviricisi metin API Başvurusu](http://docs.microsofttranslator.com/text-translate.html)
