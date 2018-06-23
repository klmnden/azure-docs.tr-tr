---
title: Çevirici Konuşma Öğreticisi (C#) | Microsoft Docs
titleSuffix: Cognitive Services
description: Gerçek zamanlı metin çevirmek için Çeviricisi konuşma hizmetini kullanmayı öğrenin.
services: cognitive-services
author: v-jerkin
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.devlang: csharp
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jerkin
ms.openlocfilehash: e82c5c5ccfa6b7de8a9ec111140dad1a40ad44f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352288"
---
# <a name="tutorial-microsoft-translator-wpf-application-in-c"></a>Öğreticisi: C# Microsoft Translator WPF uygulaması

Bu öğretici Microsoft Çeviricisi konuşma çeviri hizmeti, Microsoft Azure Bilişsel Hizmetleri'nde parçası kullanan bir etkileşimli konuşma çeviri araç turu ' dir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İstek hizmeti tarafından desteklenen dillerin listesi
> * Ses yakalamak ve hizmete iletme
> * Alma ve çevirileri konuşma metin olarak görüntüleme
> * İsteğe bağlı olarak bir konuşma (metin okuma) sürüm çeviri Yürüt

Bu uygulama için bir Visual Studio çözümü dosya [github'da](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için Visual Studio 2017 Community Edition dahil olmak üzere, herhangi bir sürümünü gerekir. 

Visual Studio çözümü, ayrıca uygulama için bir yükleyici oluşturur. Gereksinim duyduğunuz [WiX Toolset](http://wixtoolset.org/) ve [WiX Toolset Visual Studio Uzantısı](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) bu işlevleri desteklemek için.

Ayrıca Microsoft Azure panodan edinebilirsiniz Çeviricisi konuşma hizmeti için bir abonelik anahtarı gerekir. Ücretsiz fiyatlandırma katmanını kullanılabilir en fazla 10 saat başına ücret ödemeden aylık konuşma Çevir izin verir. Bu katman, Bu öğretici için yeterlidir.

Üçüncü taraf [JSON.Net Kitaplığı](https://www.newtonsoft.com/json) (Newtonsoft) de gereklidir. Her iki paketi geri yüklemesi onay kutularını Visual Studio Seçenekleri'nde etkinleştirilmişse, bu derleme NuGet tarafından otomatik olarak yüklenir.

## <a name="trying-the-translation-app"></a>Çeviri uygulaması çalışıyor

Microsoft konuşma Çeviricisi çözüm açılıyor sonra (`SpeechTranslator.sln`) Visual Studio'da derleme ve uygulamayı başlatmak için F5 tuşuna basın.  Programın ana penceresi görüntülenir.

![[Konuşma Çeviricisi ana penceresi]](media/speech-translator-main-window.png)

İlk çalıştırılmasında seçin **hesap ayarlarını** gelen **ayarları** menü burada gösterilen penceresini açın.

![[Konuşma Çeviricisi ana penceresi]](media/speech-translator-settings-window.png)

Microsoft Çeviricisi konuşma abonelik anahtarınızı bu penceresine yapıştırın ve ardından **kaydedin.** Anahtarınızı çalışmaları arasında kaydedilir.

Tekrar ana penceresinde ses giriş ve çıkış aygıtları kullanın ve Kimden ve dilleri seçin. Çeviri ses duymak istiyorsanız emin olun **TTS** (metin okuma) seçeneği denetlenir. Seslendir, etkinleştirme kurgusal kısmi çevirileri yazarken görmek istiyorsanız **kısmi sonuçlar** seçeneği.

Son olarak, tıklatın **Başlat** çeviri başlamak için. Çevrilen ve tanınan metni ve penceresinde görünür çevirisi izlemek istediğiniz bir şeyler söyleyin. TTS seçeneği etkinleştirilirse, ayrıca çeviri duyarsınız.

## <a name="obtaining-supported-languages"></a>Desteklenen diller alma

Bu yazma sırasında Microsoft Translator hizmeti metin çevirisi için birden fazla beş düzine dilleri destekler. Küçük sayıda dildeki konuşma çevirisi için desteklenir. Bu tür diller hem transcription (konuşma tanıma) için ve metin okuma çıktı, birleştirici desteği gerektirir.

Diğer bir deyişle, konuşma çevirisi için kaynak dili bir transcription için desteklenen olması gerekir. Çıktı dil herhangi bir metin sonuç istediğiniz varsayılarak metin çevirisi için desteklenen dilleri olabilir. Konuşma çıktı istiyorsanız, yalnızca okuma için desteklenen bir dil içine çevirebilir.

Microsoft zaman zaman yeni diller için destek ekleyebilir. Bu nedenle, sabit gerekir, uygulamanızda desteklenen diller bilgisi. Bunun yerine, çalışma zamanında desteklenen diller almanıza olanak tanır dilleri uç nokta Çeviricisi konuşma API sağlar. Bir veya daha fazla dilleri listesini almayı tercih edebilirsiniz: 

| | |
|-|-|
|`speech`|Konuşma transcription için desteklenen diller. Konuşma çeviri için kaynak dilleri olabilir.|
|`text`|Metni metin çevirisi için desteklenen diller. Metin çıktısı kullanıldığında konuşma Çeviri hedef dillerini olabilir.|
|`tts`|Konuşma Birleştirici için desteklenen sesi, her biri belirli bir dil ile ilişkilendirilmiş. Metin okuma kullanıldığında konuşma Çeviri hedef dillerini olabilir. Belirli bir dile göre birden fazla ses desteklenmiyor olabilir.|

Dilleri uç noktası bir abonelik anahtarı gerektirmez ve kullanım kotanız karşı sayılmaz. Kendi URI `https://dev.microsofttranslator.com/languages` ve sonuçları JSON biçiminde döndürür.

Yöntem `UpdateLanguageSettingsAsync()` içinde `MainWindow.xaml.cs`gösterilen Burada, desteklenen dillerin listesini almak için dilleri endpoint çağırır. 

```csharp
private async Task UpdateLanguageSettingsAsync()
{
    Uri baseUri = new Uri("https://" + baseUrl);
    string fullUriString = "/Languages?api-version=1.0&scope=text,speech,tts";
    if (MenuItem_Experimental.IsChecked) fullUriString += "&flight=experimental";            
    Uri fullUri = new Uri(baseUri, fullUriString);

    using (HttpClient client = new HttpClient()) //'client' is the var - using statement ensures the dispose method is used even after an exception.
    {
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, fullUri);

        // get language names for current UI culture:
        request.Headers.Add("Accept-Language", CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);

        // add a client-side trace Id. In case of issues, one can contact support and provide this:
        //string traceId = "SpeechTranslator" + Guid.NewGuid().ToString();
        //request.Headers.Add("X-ClientTraceId", traceId);
        //Debug.Print("TraceId: {0}", traceId);

        client.Timeout = TimeSpan.FromMilliseconds(10000);
        HttpResponseMessage response = await client.SendAsync(request); //make the async call to the web using the client var and passing the built up URI
        response.EnsureSuccessStatusCode(); //causes exception if the return is false

        Debug.Print("Request Id returned: {0}", GetRequestId(response));

        //create dictionaries to hold the language specific data
        spokenLanguages = new Dictionary<string, string>();
        fromLanguages = new Dictionary<string, string>();
        textLanguages = new Dictionary<string, string>();
        isLTR = new Dictionary<string, bool>();
        voices = new Dictionary<string, List<TTsDetail>>();

        JObject jResponse = JObject.Parse(await response.Content.ReadAsStringAsync()); //get the json from the async call with the response var created above, parse it and put it in a var called jResponse - JObject is a newton class

        //Gather the set of TTS voices
        foreach (JProperty jTts in jResponse["tts"])
        {
            JObject ttsDetails = (JObject)jTts.Value;

            string code = jTts.Name;
            string language = ttsDetails["language"].ToString();
            string displayName = ttsDetails["displayName"].ToString();
            string gender = ttsDetails["gender"].ToString();

            if (!voices.ContainsKey(language)) //check dictionary for a specific key value
            {
                voices.Add(language, new List<TTsDetail>()); //add to the dictionary the locale key and a ttsDetail object
            }

            voices[language].Add(new TTsDetail() { Code = code, DisplayName = string.Format("{0} ({1})", displayName, gender) });
        }

        // Gather the set of speech translation languages
        foreach (JProperty jSpeech in jResponse["speech"])
        {
            JObject languageDetails = (JObject)jSpeech.Value;
            string code = jSpeech.Name;
            string simplecode = languageDetails["language"].ToString();
            string displayName = languageDetails["name"].ToString();
            spokenLanguages.Add(code, displayName);
            fromLanguages.Add(code,simplecode);
        }

        spokenLanguages = spokenLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        FromLanguage.Items.Clear();
        foreach (var language in spokenLanguages)
        {
            FromLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key});
        }

        // Gather the set of text translation languages
        foreach (JProperty jText in jResponse["text"])
        {
            JObject languageDetails = (JObject)jText.Value;
            string code = jText.Name;
            string displayName = languageDetails["name"].ToString();
            textLanguages.Add(code, displayName);

            string direction = languageDetails["dir"].ToString().ToLowerInvariant();
            bool LTR = true;
            if (direction.ToLowerInvariant() == "rtl") LTR = false;
            isLTR.Add(code, LTR);
        }

        textLanguages = textLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        ToLanguage.Items.Clear();
        foreach (var language in textLanguages)
        {
            ToLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key });
        }

        if (Properties.Settings.Default.FromLanguageIndex >= 0) FromLanguage.SelectedIndex = Properties.Settings.Default.FromLanguageIndex;
        else
        {
            for(int i=0; i < FromLanguage.Items.Count; ++i)
            {
                ComboBoxItem item = (ComboBoxItem)FromLanguage.Items[i];
                if(CultureInfo.CurrentUICulture.Name.Equals((string)item.Tag, StringComparison.OrdinalIgnoreCase))
                {
                    FromLanguage.SelectedIndex = i;
                }
            }
        }
        if (Properties.Settings.Default.ToLanguageIndex >= 0) ToLanguage.SelectedIndex = Properties.Settings.Default.ToLanguageIndex;
        else
        {
            Random rnd = new Random(DateTime.Now.Millisecond);
            ToLanguage.SelectedIndex = (rnd.Next() % textLanguages.Count);
        }
    }
}
```

Bu yöntem ilk üç tüm dilleri listesini isteyen dilleri uç noktasına bir HTTP isteği oluşturur (`text`, `speech`, ve `tts`).

İsteğin dilleri uç nokta kullanır `Accept-Languages` dilleri adlarını temsil dilini belirlemek için üstbilgi. Örneğin, "Almanca" "Deutsch" Almanca ve İspanyolca "Alemán" ve dillerin listesini şeklinde adlandırılır İngilizce konuşmacılar bilinen dil Bu farklılıklar yansıtır. Sistemin varsayılan dil bu başlığı için kullanılır.

Sonra istek gönderildi ve alındı, JSON yanıtı yanıtı iç veri yapılarını ayrıştırılır. Bu yapıları, daha sonra gelen dil ve dil menüleri oluşturmak için kullanılır. 

Kullanıcı tarafından seçilen dil kullanılabilir sesi bağımlı olduğundan, henüz sesli menüsü Ayarla mümkün değildir. Bunun yerine, her dil için kullanılabilir seslerini daha sonra kullanılmak üzere depolanır. `ToLanguage_SelectionChanged` İşleyicisinde (aynı kaynak dosyası) çağırarak sesli menü daha sonra güncelleştirmeleri `UpdateVoiceComboBox()` zaman kullanıcının seçtiği bir dili. 

Kullanıcı önce uygulamayı çalıştırılmamışsa yalnızca eğlenceli için bir dil rastgele seçilir. (Menü ayarlarını oturumlar arasında depolanır.)

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama

Azure abonelik anahtarınızı değeri üst bilgi göndermesine gereken Microsoft Çeviricisi konuşma hizmete yönelik kimlik doğrulama için `Ocp-Apim-Subscription-Key` bağlantı isteği.

## <a name="translation-overview"></a>Çeviri genel bakış

Çevir API (WebSockets endpoint `wss://dev.microsofttranslator.com/speech/translate`) kabul çevrilecek ses monophonic, 16 kHz, 16 bit dalga biçimi imzalı. Hizmet, hem tanınan ve çevrilmiş metin içeren bir veya daha fazla JSON yanıtlarını döndürür. Metin okuma istenirse bir ses dosyası gönderilir.

Kullanıcı mikrofon/dosya giriş menüsünü kullanarak ses kaynağı seçer. Ses (örneğin, bir mikrofon) ses bir aygıttan veya gelebilir bir `.WAV` dosyası.

Yöntem `StartListening_Click` kullanıcı Başlat düğmesine tıkladığında çağrılır. Bu olay işleyicisi sırayla çağırır `Connect()` ses hizmeti API uç noktasına gönderme işlemini başlatmak için. `Connect()` Yöntemi aşağıdaki görevleri gerçekleştirir:


> [!div class="checklist"]
> * Onları doğrulamak ve ana penceresinden kullanıcı ayarlarını alma
> * Ses giriş başlatma ve çıkış akışları
> * Çağırma `ConnectAsync()` gerisini işlemek için

`ConnectAsync()`, aşağıdaki işlerinden sırayla işler:

> [!div class="checklist"]
> * Azure abonelik anahtarı üstbilgisinde ile kimlik doğrulaması `Ocp-Apim-Subscription-Key`
> * Oluşturma bir `SpeechClient` örneği (bulunan `SpeechClient.cs`) hizmetiyle iletişim kurmak için
> * Başlatma `TextMessageDecoder` ve `BinaryMessageDecoder` örnekleri (bkz: `SpeechResponseDecoder.cs`) yanıtları işlemek için
> * Ses aracılığıyla gönderme `SpeechClient` Çeviricisi konuşma hizmet örneği
> * Alma ve çeviri sonuçlarını işleme

Sorumluluklarını `SpeechClient` daha az şunlardır:

> [!div class="checklist"]
> * Çevirici konuşma hizmetine WebSocket bağlantı kuruluyor
> * Ses veri göndermek ve yanıtları yuva aracılığıyla alma

## <a name="a-closer-look"></a>Daha ayrıntılı bir bakış

Bunu şimdi nasıl uygulama bölümlerinin çeviri isteği gerçekleştirmek için birlikte çalışan daha anlaşılır olması gerekir. İlgili bölümleri odaklanan biraz kod bir göz atalım.

Kısmi bir sürümünü işte `Connect()` ses akışları ayarı gösterir:

```csharp
private void Connect()
{
    if (this.currentState != UiState.ReadyToConnect) return;

    Stopwatch watch = Stopwatch.StartNew();
    UpdateUiState(UiState.Connecting);

    // Omitted: code to validate UI settings

    string tag = ((ComboBoxItem)Mic.SelectedItem).Tag as string;
    string audioFileInputPath = null;
    if (tag == "File")
    {
        audioFileInputPath = this.AudioFileInput.Text;
        foreach (string currFile in audioFileInputPath.Split('|'))
        {
            if (!File.Exists(currFile))
            {
                SetMessage(String.Format($"Invalid audio source: selected file {currFile} does not exist."), "", MessageKind.Error);
                UpdateUiState(UiState.ReadyToConnect);
                return;
            }
        }
    }
    bool shouldSuspendInputAudioDuringTTS = this.CutInputAudioCheckBox.IsChecked.HasValue ? this.CutInputAudioCheckBox.IsChecked.Value : false;

    correlationId = Guid.NewGuid().ToString("D").Split('-')[0].ToUpperInvariant();

    // Setup speech translation client options
    SpeechClientOptions options;

    string voicename = "";
    if (this.Voice.SelectedItem != null)
    {
        voicename = ((ComboBoxItem)this.Voice.SelectedItem).Tag.ToString();
    }
    options = new SpeechTranslateClientOptions()
    {
        TranslateFrom = ((ComboBoxItem)this.FromLanguage.SelectedItem).Tag.ToString(),
        TranslateTo = ((ComboBoxItem)this.ToLanguage.SelectedItem).Tag.ToString(),
        Voice = voicename,
    };
    
    options.Hostname = baseUrl;
    options.AuthHeaderKey = "Authorization";
    options.AuthHeaderValue = ""; // set later in ConnectAsync.
    options.ClientAppId = new Guid("EA66703D-90A8-436B-9BD6-7A2707A2AD99");
    options.CorrelationId = this.correlationId;
    options.Features = GetFeatures().ToString().Replace(" ", "");
    options.Profanity = ((SpeechClient.ProfanityFilter)Enum.Parse(typeof(SpeechClient.ProfanityFilter), GetProfanityLevel(), true)).ToString();
    options.Experimental = MenuItem_Experimental.IsChecked;

    // Setup player and recorder but don't start them yet.
    WaveFormat waveFormat = new WaveFormat(16000, 16, 1);

    // WaveProvider for incoming TTS
    // We use a rather large BufferDuration because we need to be able to hold an entire utterance.
    // TTS audio is received in bursts (faster than real-time).
    textToSpeechBytes = 0;
    playerTextToSpeechWaveProvider = new BufferedWaveProvider(waveFormat);
    playerTextToSpeechWaveProvider.BufferDuration = TimeSpan.FromMinutes(5);

    ISampleProvider sampleProvider = null;
    if (audioFileInputPath != null)
    {
        // Setup mixing of audio from input file and from TTS
        playerAudioInputWaveProvider = new BufferedWaveProvider(waveFormat);
        var srce1 = new Pcm16BitToSampleProvider(playerTextToSpeechWaveProvider);
        var srce2 = new Pcm16BitToSampleProvider(playerAudioInputWaveProvider);
        var mixer = new MixingSampleProvider(srce1.WaveFormat);
        mixer.AddMixerInput(srce1);
        mixer.AddMixerInput(srce2);
        sampleProvider = mixer;
    }
    else
    {
        recorder = new WaveIn();
        recorder.DeviceNumber = (int)((ComboBoxItem)Mic.SelectedItem).Tag;
        recorder.WaveFormat = waveFormat;
        recorder.DataAvailable += OnRecorderDataAvailable;
        sampleProvider = playerTextToSpeechWaveProvider.ToSampleProvider();
    }

    if (!batchMode)
    {
        player = new WaveOut();
        player.DeviceNumber = (int)((ComboBoxItem)Speaker.SelectedItem).Tag;
        player.Init(sampleProvider);
    }

    this.audioBytesSent = 0;

    string logAudioFileName = null;
    if (LogSentAudio.IsChecked|| LogReceivedAudio.IsChecked)
    {
        string logAudioPath = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), Properties.Settings.Default.OutputDirectory);
        try
        {
            Directory.CreateDirectory(logAudioPath);
        }
        catch
        {
            this.AddItemToLog(string.Format("Could not create folder {0}", logAudioPath));
        }

        if (LogSentAudio.IsChecked)
        {
            logAudioFileName = System.IO.Path.Combine(logAudioPath, string.Format("audiosent_{0}.wav", this.correlationId));
        }

        if (LogReceivedAudio.IsChecked)
        {
            string fmt = System.IO.Path.Combine(logAudioPath, string.Format("audiotts_{0}_{{0}}.wav", this.correlationId));
            this.audioReceived = new BinaryMessageDecoder(fmt);
        }
    }
}
```

Önemli bir bölümü `Connect()` oluşturulmasını içerir bir `SpeechClientOptions` örneği (bkz: `SpeechClientOptions.cs`) çeviri için seçenekleri tutmak için. Seçenekler (örneğin, kimlik doğrulama anahtarı ve ana bilgisayar adı) hizmetine bağlanmak için gereken bilgiler ve çevirisi kullanılan özellikler içerir. Üstbilgi alanları ve HTTP parametreleri tarafından sunulan alanları buraya Eşle [Çeviricisi konuşma API](http://docs.microsofttranslator.com/speech-translate.html).

`Connect()` Ayrıca, oluşturur ve ses giriş cihazlarını başlatır (değişken `sampleProvider`) çevrilecek konuşma kaynağı olarak görev yapar. Bu cihaz bir giriş donanım mikrofon gibi ya da WAVE ses verilerini içeren bir dosya değil.

Burada `ConnectAsync()` başlatır yöntemi `speechClient` sınıfı ve metin ve ikili yanıtları hizmetinden işlemek için anonim işlevleri kancaları.

```csharp
private async Task ConnectAsync(SpeechClientOptions options, bool suspendInputAudioDuringTTS)
{
    await ADMAuthenticate(options);
    
    TextMessageDecoder textDecoder;
    
    s2smtClient = new SpeechClient((SpeechTranslateClientOptions)options, CancellationToken.None);
    
    s2smtClient.OnBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnEndOfBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnTextData += (c, a) => { textDecoder.AppendData(a); lastReceivedPacketTick = DateTime.Now.Ticks; };
    s2smtClient.OnEndOfTextData += (c, a) =>
    {
        textDecoder.AppendData(a);
        lastReceivedPacketTick = DateTime.Now.Ticks;
        textDecoder
            .Decode()
            .ContinueWith(t =>
            {
                if (t.IsFaulted)
                {
                    Log(t.Exception, "E: Failed to decode incoming text message.");
                }
                else
                {
                    object msg = t.Result;
                    if (msg.GetType() == typeof(FinalResultMessage))
                    {
                        // omitted: code to process final binary result
                    }
                    if (msg.GetType() == typeof(PartialResultMessage))
                    {
                        // omitted: code to process partial binary result
                    }
                }
            });
    };
    s2smtClient.Failed += (c, ex) =>
    {
        Log(ex, "E: SpeechTranslation client reported an error.");
    };
    s2smtClient.Disconnected += (c, ea) =>
    {
        SafeInvoke(() =>
        {
            // We only care to react to server disconnect when our state is Connected. 
            if (currentState == UiState.Connected)
            {
                Log("E: Connection has been lost.");
                Log($"E: Errors (if any): \n{string.Join("\n", s2smtClient.Errors)}");
                Disconnect();
            }
        });
    };
    await s2smtClient.Connect();
}
```

Kimlik doğrulandıktan sonra yöntem oluşturur `SpeechClient` örneği. `SpeechClient` Sınıfı (içinde `SpeechClient.cs`) olay işleyicileri giriş ikili ve metin veri üzerine çağırır. Bağlantı hata verdiğinde veya keser ek işleyicileri çağrılır.

İkili veri TTS etkinleştirildiğinde hizmeti tarafından gönderilen ses (metin okuma çıktı) var. Metin kısmi veya tam bir çeviri konuşma metin verilerdir. Yöntemi bu iletileri işlemek için işlevlerini kancalarını başlatmasını sonra şekilde: penceresinde görüntüleyerek sonraki kayıttan yürütme ve metin depolayarak ses.

## <a name="next-steps"></a>Sonraki adımlar

Bu kod örneği, Çevirici konuşma API kullanımını gösteren zengin bir uygulamadır. Bu nedenle, Orta birkaç anlamak için taşıma bölümleri vardır. En önemli BITS ile gitti. Kalan için Visual Studio birkaç kesme noktalarını ayarlayın ve çeviri işleminde size kılavuzluk için eğitici olabilir. Örnek uygulama anladığınızda, kendi uygulamalarında Çeviricisi konuşma hizmetini kullanmak için donatılmış.

> [!div class="nextstepaction"]
> [Microsoft Çeviricisi konuşma API Başvurusu](http://docs.microsofttranslator.com/speech-translate.html)
