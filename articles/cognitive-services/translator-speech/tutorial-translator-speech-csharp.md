---
title: Translator Konuşma Öğreticisi (C#) | Microsoft Docs
titleSuffix: Cognitive Services
description: Translator konuşma tanıma hizmeti gerçek zamanlı metni çevirmek için kullanmayı öğrenin.
services: cognitive-services
author: v-jerkin
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.devlang: csharp
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jerkin
ms.openlocfilehash: 010ad8b5ceeaf046c8d361ff352e6058154a482d
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "41987564"
---
# <a name="tutorial-microsoft-translator-wpf-application-in-c"></a>Öğreticisi: C# Microsoft Translator WPF uygulaması

Microsoft Translator konuşma çevirisi hizmeti olan Microsoft Azure Bilişsel Hizmetler'in bir parçası kullanan bir etkileşimli konuşma çevirisi araç turu öğreticidir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İstek hizmeti tarafından desteklenen dillerin listesi
> * Hizmetine aktarmanıza ve ses yakalama
> * Alma ve konuşma çevirisi, metin olarak görüntüleme
> * İsteğe bağlı olarak bir çeviri (metin okuma) sürümünü konuşulan Yürüt

Bu uygulama için bir Visual Studio çözüm dosyası [github'da](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, Visual Studio 2017 Community Edition dahil olmak üzere, herhangi bir sürümü gerekir. 

Visual Studio çözümünü uygulama için bir yükleyici de oluşturur. Gereksinim duyduğunuz [WiX Toolset](http://wixtoolset.org/) ve [WiX Toolset Visual Studio Uzantısı](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) bu işlevselliği desteklemek için.

Ayrıca, Microsoft Azure panodan edinebilirsiniz Translator konuşma çevirisi hizmeti için bir abonelik anahtarı gerekir. Ücretsiz fiyatlandırma katmanı kullanılabilir 10 saate kadar her ay ücretsiz olarak konuşma Çevir olanak tanır. Bu katman, Bu öğretici için yeterlidir.

Üçüncü taraf [JSON.Net Kitaplığı](https://www.newtonsoft.com/json) (Newtonsoft) de gereklidir. Visual Studio seçenekleri iki paketi geri yükle onay etkinse bu derleme tarafından NuGet otomatik olarak yüklenir.

## <a name="trying-the-translation-app"></a>Çeviri App'i denediğiniz

Microsoft konuşma çevirmeni çözüm açılırken sonra (`SpeechTranslator.sln`) Visual Studio'da oluşturmak ve uygulamayı başlatmak için F5 tuşuna basın.  Programın ana penceresi görüntülenir.

![[Konuşma çevirmeni ana pencere]](media/speech-translator-main-window.png)

İlk çalıştırılmasında seçin **hesap ayarları** gelen **ayarları** burada gösterilen penceresini açmak için menü.

![[Konuşma çevirmeni ana pencere]](media/speech-translator-settings-window.png)

Bu pencerede, Microsoft Translator konuşma çevirisi abonelik anahtarınızı yapıştırın ve ardından tıklayın **kaydedin.** Anahtarınızı çalışmaları arasında kaydedilir.

Ses giriş ve çıkış cihazları ve Kimden ve dilleri istediğiniz tekrar ana penceresinde, seçin. Çeviri yalnızca sesi duymak istiyorsanız emin **TTS** (metin okuma) seçeneği denetlenir. Konuşurken, etkinleştirme kurgusal kısmi çevirileri yazarken görmek istiyorsanız **kısmi sonuçlar** seçeneği.

Son olarak, tıklayın **Başlat** çeviri başlatmak için. Çevrilmiş ve tanınan metin ile penceresinde görünür çeviri izlemek istediğiniz bir şey varsayalım. TTS seçeneği etkinleştirilirse, ayrıca çeviri dinleyin.

## <a name="obtaining-supported-languages"></a>Desteklenen diller edinme

Bu yazma sırasında Microsoft Translator hizmeti, metin çevirisi için birden fazla beş düzine dilleri destekler. Daha az sayıda dil, konuşma çevirisi için desteklenir. Gibi diller için her iki döküm (konuşma tanıma) ve metin okuma çıkışı, sentezi için desteği gerektirir.

Diğer bir deyişle, konuşma çevirisi için kaynak dili döküm için desteklenen olmalıdır. Çıkış dil herhangi bir metin sonucu istediğiniz varsayılarak, metin çevirisi için desteklenen dilleri olabilir. Konuşma çıkışındaki istiyorsanız, yalnızca metin okuma için desteklenen bir dile çevirebilir.

Microsoft zaman zaman yeni diller için destek ekleyebilirsiniz. Bu nedenle, sabit gereken bilgisine sahip, uygulamanızda desteklenen diller. Bunun yerine, Translator konuşma tanıma API'si, desteklenen diller, çalışma zamanında almanızı sağlayan bir dilleri uç noktası sağlar. Bir veya daha fazla dil listesini almayı tercih edebilirsiniz: 

| | |
|-|-|
|`speech`|Konuşma transkripsiyonu için desteklenen diller. Konuşma çevirisi için kaynak dilleri olabilir.|
|`text`|Metni metin çevirisi için desteklenen diller. Metin çıktısı kullanıldığında, hedef diller konuşma çevirisi için olabilir.|
|`tts`|Konuşma sentezi için desteklenen sesi, her biri belirli bir dil ile ilişkilendirilmiş. Metin okuma kullanıldığında, hedef diller konuşma çevirisi için olabilir. Belirli bir dile göre birden fazla ses desteklenmiyor olabilir.|

Dilleri uç nokta bir abonelik anahtarı gerektirmez ve kullanımı kotanız sayılmaz. Kendi URI `https://dev.microsofttranslator.com/languages` ve sonuçları JSON biçiminde döndürür.

Yöntem `UpdateLanguageSettingsAsync()` içinde `MainWindow.xaml.cs`gösterilen burada dilleri uç nokta desteklenen dillerin listesini almak için çağırır. 

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

Bu yöntem ilk üç tüm diller listesini isteyen dilleri uç noktasına bir HTTP isteği oluşturur (`text`, `speech`, ve `tts`).

İsteğin dilleri uç noktası kullanan `Accept-Languages` dilleri adlarını temsil dil belirlemek için başlığı. Örneğin, "Almanca" "Deutsch" Almanca ve İspanyolca "Alemán" ve dillerin listesi şeklinde adlandırılır için İngilizce konuşmacıları bilinen dil bu farklılıkları yansıtır. Bu başlığı için sistemin varsayılan dil kullanılır.

Sonra istek gönderildi ve alındı, JSON yanıtı, yanıt iç veri yapılarını ayrıştırılır. Bu yapılar, daha sonra gelen, dil ve dil menüleri oluşturmak için kullanılır. 

Kullanıcı tarafından seçilen dil kullanılabilir sesi bağımlı olduğundan, henüz ses menüsünü ayarlamak mümkün değildir. Bunun yerine, her dil için kullanılabilir seslerini daha sonra kullanmak üzere depolanır. `ToLanguage_SelectionChanged` İşleyici (aynı kaynak dosyasındaki) çağırarak ses menü daha sonra güncelleştirmeleri `UpdateVoiceComboBox()` olduğunda kullanıcı için bir dil seçer. 

Kullanıcı önce uygulamayı çalıştırılmamışsa eğlencelik için bir dil için rastgele seçilir. (Menü ayarlarını oturumları arasında depolanır.)

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama

Azure abonelik anahtarınızı değeri olarak üst bilgisinde göndermek için gereken Microsoft Translator konuşma çevirisi hizmeti için kimlik doğrulaması `Ocp-Apim-Subscription-Key` bağlantı isteği.

## <a name="translation-overview"></a>Çeviri genel bakış

Çevirme API (WebSockets uç nokta `wss://dev.microsofttranslator.com/speech/translate`) kabul çevrilemeyen ses monophonic, 16 kHz, 16 bit WAVE biçiminde imzalı. Hizmet, hem tanınan ve çevrilmiş metni içeren bir veya daha fazla JSON yanıtlarını döndürür. Ses dosyası metin okuma istenmişse gönderilir.

Kullanıcı girişi mikrofon/Dosya menüsünü kullanarak ses kaynağından seçer. Ses (örneğin, bir mikrofon) ses cihazından veya gelen bir `.WAV` dosya.

Yöntem `StartListening_Click` kullanıcı Başlat düğmesine tıkladığında çağrılır. Bu olay işleyicisi, çağıran `Connect()` ses hizmeti API uç noktaya gönderme işlemi başlatmak için. `Connect()` Yöntemi aşağıdaki görevleri gerçekleştirir:


> [!div class="checklist"]
> * Kullanıcı ayarları ana pencereden almak ve bunları doğrulayabilirsiniz
> * Başlatma ses giriş ve çıkış akışları
> * Çağırma `ConnectAsync()` gerisini işlemek için

`ConnectAsync()`, aşağıdaki işlerini sırayla işler:

> [!div class="checklist"]
> * Üst bilgisindeki Azure abonelik anahtarı ile kimlik doğrulaması `Ocp-Apim-Subscription-Key`
> * Oluşturma bir `SpeechClient` örneği (bulunan `SpeechClient.cs`) hizmetiyle iletişim kurmak için
> * Başlatma `TextMessageDecoder` ve `BinaryMessageDecoder` örnekleri (bkz `SpeechResponseDecoder.cs`) yanıtlarını işlemek için
> * Ses aracılığıyla gönderme `SpeechClient` Translator konuşma çevirisi hizmeti örneği
> * Alma ve çeviri sonuçlarını işleme

Sorumluluk `SpeechClient` daha az olan:

> [!div class="checklist"]
> * Translator konuşma çevirisi hizmeti bir WebSocket bağlantısı kurma
> * Ses verisi gönderip yuva aracılığıyla yanıtlar

## <a name="a-closer-look"></a>Daha yakından bakın

Bunu şimdi uygulamanın parçaları birlikte çeviri isteği gerçekleştirmek için nasıl daha anlaşılır olması gerekir. İlgili bölümlerin üzerinde odaklanarak, bazı kod bir göz atalım.

Kısmi bir hali aşağıdadır `Connect()` ses akışları ayarı gösterilmektedir:

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

Önemli bir kısmı `Connect()` oluşturulmasını içeren bir `SpeechClientOptions` örneği (bkz `SpeechClientOptions.cs`) çeviri seçeneklerini tutmak için. Seçenekler (örneğin, kimlik doğrulama anahtarı ve ana bilgisayar adı) hizmetine bağlanmak için gereken bilgileri ve çeviri için kullanılan özellikler içerir. HTTP parametreleri tarafından kullanıma sunulan ve üstbilgi alanlarını buraya alanları eşleyin [Translator konuşma çevirisi API'sine](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference).

`Connect()` Ayrıca, oluşturur ve ses giriş cihazını başlatır (değişken `sampleProvider`) çevrilemeyen konuşma kaynağı olarak görev yapar. Bu, bir mikrofon gibi Giriş bir donanım cihazı veya WAVE ses veriler içeren bir dosya cihazdır.

İşte `ConnectAsync()` başlatan yöntem `speechClient` sınıfı ve metin ve ikili yanıtları hizmetinden işlemek için anonim işlevler bağlar.

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

Kimlik doğrulandıktan sonra yöntemi oluşturur `SpeechClient` örneği. `SpeechClient` Sınıfı (içinde `SpeechClient.cs`) giriş ikili ve metin verilerini sonra olay işleyicilerini çağırır. Ek işleyicileri bağlantısı hata verdiğinde veya bağlantısı kesildiğinde çağrılır.

Ses TTS etkin olduğunda hizmet tarafından gönderilen (metin okuma çıkış) ikili verilerdir. Metin kısmi veya tam bir çeviri konuşulan metnin verilerdir. Yöntemi bu iletileri işlemek için işlevlerini kancaları başlatıldıktan sonra bu nedenle: penceresinde görüntüleyerek daha sonra kayıttan yürütmek ve metin depolayarak ses.

## <a name="next-steps"></a>Sonraki adımlar

Bu kod örneği, Translator konuşma tanıma API'si kullanımı gösteren zengin bir uygulamadır. Bu nedenle, anlamak için hareketli parçadan adil bir dizi vardır. En önemli bitleri öğrendiniz. Geri kalan için Visual Studio'da birkaç kesme noktaları ayarlamak ve çeviri işleminde size kılavuzluk için eğitici olabilir. Örnek uygulamayı anladığınızda, Translator konuşma çevirisi hizmeti uygulamalarınızda kullanmaya donatılmış.

> [!div class="nextstepaction"]
> [Microsoft Translator konuşma tanıma API'si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
