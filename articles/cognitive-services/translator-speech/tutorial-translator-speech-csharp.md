---
title: "Öğretici: Translator konuşma çevirisi API'siC#"
titleSuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'sini kullanarak gerçek zamanlı metin çevirisi yapın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: tutorial
ms.date: 3/5/2018
ms.author: swmachan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a3853dd810182948e12b578c33b8cb91bef4b1cf
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445576"
---
# <a name="tutorial-translator-speech-application-in-c"></a>Öğretici: Translator konuşma uygulamaC#

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Bu öğreticide, Azure Bilişsel Hizmetleri'nin bir parçası olan Translator Konuşma Çevirisi API'sini kullanan etkileşimli konuşma çevirisi aracı geliştirme adımları gösterilmektedir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Hizmet tarafından desteklenen dillerin listesini isteme
> * Ses yakalama ve hizmete iletme
> * Konuşmanın çevirisini metin olarak alma ve görüntüleme
> * İsteğe bağlı olarak çevirinin sözlü (metin okuma) sürümünü oynatma

Bu uygulamanın Visual Studio çözüm dosyası [GitHub'da mevcuttur](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, Visual Studio Community sürümü dahil olmak üzere 2019'ın herhangi bir sürümü gerekir.

Visual Studio çözümü, uygulama için bir yükleyici de derler. Bu işlevi kullanabilmek için [WiX Toolset](http://wixtoolset.org/) ve [WiX Toolset Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) uygulamalarına ihtiyacınız vardır.

Ayrıca Microsoft Azure panosundan alabileceğiniz Translator Konuşma Çevirisi hizmeti abonelik anahtarı da gereklidir. Ayda 10 saat konuşmaya kadar ücretsiz olarak çeviri yapmanıza olanak sağlayan ücretsiz bir fiyatlandırma katmanı bulunur. Bu katman bu öğretici için yeterli olacaktır.

Üçüncü taraf [JSON.NET kitaplığı](https://www.newtonsoft.com/json) (Newtonsoft) de gereklidir. Bu bütünleştirilmiş kod, Visual Studio seçeneklerinde Paket Geri Yükleme onay kutularının her ikisi de işaretli olduğunda NuGet tarafından otomatik olarak yüklenir.

## <a name="trying-the-translation-app"></a>Çeviri uygulamasını deneme

Konuşma Çevirisi çözümünü (`SpeechTranslator.sln`) Visual Studio'da açtıktan sonra F5 tuşuna basarak uygulamayı derleyin ve çalıştırın.  Programın ana penceresi görüntülenir.

![[Konuşma Çevirmeni ana penceresi]](media/speech-translator-main-window.png)

Programı ilk kez çalıştırdıktan sonra **Settings** (Ayarlar) menüsünden **Account Settings**'i (Hesap Ayarları) seçerek burada gösterilen pencereyi açın.

![[Konuşma Çevirmeni ana penceresi]](media/speech-translator-settings-window.png)

Translator Konuşma Çevirisi abonelik anahtarınızı bu pencereye yapıştırıp **Save**'e (Kaydet) tıklayın. Anahtarınız kaydedilir ve programı yeniden çalıştırdığınızda aynı anahtar kullanılır.

Ana pencereye geri dönüp kullanmak istediğiniz ses giriş ve çıkış cihazlarının yanı sıra Kaynak ve Hedef dilleri seçin. Çeviriyi duymak istiyorsanız **TTS** (metin okuma) seçeneğinin işaretlenmiş olduğundan emin olun. Konuşurken olası kısmi çevirileri görmek istiyorsanız **Partial Results** (Kısmi Sonuçlar) seçeneğini etkinleştirin.

Son olarak **Start**'a (Başlat) tıklayarak çeviriyi başlatın. Çevrilmesini istediğiniz bir şeyler söyleyip pencerede metnin tanınmasını ve çevirinin görünmesini izleyin. TTS seçeneğini etkinleştirdiyseniz çeviriyi duyarsınız.

## <a name="obtaining-supported-languages"></a>Desteklenen dilleri alma

Bu öğreticinin oluşturulduğu an itibarıyla Translator Konuşma Çevirisi hizmetinin metin çevirisi için desteklediği dil sayısı beş düzinenin üzerindedir. Konuşma çevirisi için desteklenen dil sayısı bunun biraz daha altındadır. Bu diller için hem transkripsiyon (konuşma tanıma) hem de metin okuma çıkışı için birleştirme desteği gerekir.

Başka bir deyişle konuşma çevirisi için kaynak dilin transkripsiyon desteğine sahip olması gerekir. Sonucu metin halinde almak istediğinizi düşünürsek çıkış dili metin çevirisi için desteklenen dillerden herhangi biri olabilir. Sonucu konuşma olarak almak istiyorsanız yalnızca metin okuma desteğine sahip bir dile çeviri yapabilirsiniz.

Microsoft belirli dönemlerde yeni diller için destek ekleyebilir. Bu nedenle desteklenen dilleri uygulamanıza doğrudan yazmamanız gerekir. Bunun yerine Translator Konuşma Çevirisi API'si tarafından sunulan Languages uç noktasını kullanarak desteklenen dilleri çalışma zamanına alabilirsiniz. Bir veya daha fazla dil listesi alabilirsiniz:

| | |
|-|-|
|`speech`|Konuşma transkripsiyonu için desteklenen diller. Konuşma çevirisi için kaynak dil olabilir.|
|`text`|Metin çevirisi için desteklenen diller. Metin çıktısının kullanıldığı konuşma çevirisi için hedef dil olabilir.|
|`tts`|Her biri belirli bir dille ilişkilendirilmiş olan konuşma birleştirme için desteklenen sesler. Metin okumanın kullanıldığı konuşma çevirisi için hedef dil olabilir. Belirli bir dil birden fazla ses desteğine sahip olabilir.|

Languages uç noktası abonelik anahtarı gerektirmez ve kullanımı kotanıza dahil edilmez. URI'si `https://dev.microsofttranslator.com/languages` şeklindedir ve sonuçlarını JSON biçiminde döndürür.

Burada gösterilen `MainWindow.xaml.cs` içindeki `UpdateLanguageSettingsAsync()` metodu, desteklenen dillerin listesini almak için Languages uç noktasını çağırır.

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

Bu metot öncelikle Languages uç noktasına bir HTTP isteği oluşturur ve üç dil listesini ister (`text`, `speech` ve `tts`).

Languages uç noktası, dil adlarının temsil edildiği dili belirlemek için isteğin `Accept-Languages` üst bilgisini kullanır. Örneğin Türkçe konuşanların "Almanca" olarak bildiği dil Almanca dilinde "Deutsch", İspanyolca dilinde ise "Alemán" şeklinde yazılır ve dil listesi bu farklılıkları gösterir. Bu üst bilgi için sistemin varsayılan dili kullanılır.

İstek gönderildikten ve JSON yanıtı alındıktan sonra yanıt iç veri yapılarına ayrıştırılır. Ardından bu yapılar Kaynak Dil ve Hedef Dil menülerini oluşturmak için kullanılır.

Kullanılabilir sesler kullanıcı tarafından belirlenen Hedef Dile bağlı olduğundan bu noktada Ses menüsünü ayarlamak mümkün değildir. Bunun yerine her dil için kullanılabilir durumdaki sesler daha sonra kullanılmak üzere depolanır. `ToLanguage_SelectionChanged` işleci (aynı kaynak dosyasındaki) daha sonra kullanıcı bir Hedef Dil seçtiğinde `UpdateVoiceComboBox()` çağrısı yaparak Ses menüsünü güncelleştirir.

Kullanıcı uygulamayı önceden çalıştırmamışsa rastgele bir Hedef Dil seçilir. (Oturumlardaki menü ayarları kaydedilir.)

## <a name="authenticating-requests"></a>İsteklerin kimliğini doğrulama

Microsoft Translator Konuşma Çevirisi hizmetinde kimlik doğrulaması gerçekleştirmek için Azure abonelik anahtarınızı bağlantı isteği üst bilgisinde `Ocp-Apim-Subscription-Key` parametresinin değeri olarak göndermeniz gerekir.

## <a name="translation-overview"></a>Çeviriye genel bakış

Çeviri API'si (WebSockets uç noktası `wss://dev.microsofttranslator.com/speech/translate`) çeviri için monofonik, 16 kHz, 16 bit imzalı WAVE biçimindeki sesleri kabul eder. Hizmet tanınan ve çevrilen metni içeren bir veya daha fazla JSON yanıtı döndürür. Metin okuma istenmişse bir ses dosyası gönderilir.

Kullanıcı Mikrofon/Dosya Girişi menüsünü kullanarak ses kaynağını seçer. Ses bir ses cihazından (mikrofon gibi) veya bir `.WAV` dosyasından alınabilir.

Kullanıcı Başlat düğmesine tıkladığında `StartListening_Click` metodu çağrılır. Olay işleyicisi bunun üzerine `Connect()` çağrısı yaparak sesi hizmet API uç noktasına gönderme işlemini başlatır. `Connect()` metodu şu görevleri gerçekleştirir:


> [!div class="checklist"]
> * Ana pencereden kullanıcı ayarlarını alma ve bunları doğrulama
> * Ses giriş ve çıkış akışlarını başlatma
> * İşin geri kalanını yapması için `ConnectAsync()` metodunu çağırma

`ConnectAsync()` metodu da şu görevleri gerçekleştirir:

> [!div class="checklist"]
> * `Ocp-Apim-Subscription-Key` üst bilgisindeki Azure Abonelik anahtarıyla kimlik doğrulama
> * Hizmetle iletişim kurmak için bir `SpeechClient` örneği (`SpeechClient.cs` içinde bulunur) oluşturma
> * Yanıtları işlemek için `TextMessageDecoder` ve `BinaryMessageDecoder` örneklerini başlatma (bkz. `SpeechResponseDecoder.cs`)
> * Sesi `SpeechClient` örneği aracılığıyla Translator Konuşma Çevirisi hizmetine gönderme
> * Çevirinin sonuçlarını alma ve işleme

`SpeechClient` öğesinin sorumlulukları daha azdır:

> [!div class="checklist"]
> * Translator Konuşma Çevirisi hizmetine bir WebSocket bağlantısı kurma
> * Ses verilerini gönderme ve yuva aracılığıyla yanıt alma

## <a name="a-closer-look"></a>Daha yakından bakın

Artık uygulamanın parçalarının çeviri isteğini gerçekleştirmek için birlikte nasıl çalıştığı konusunda daha fazla bilgiye sahipsiniz. Şimdi biraz kod inceleyelim ve ilgili bölümlere odaklanalım.

`Connect()` bölümünün ses akışlarının ayarlanmasını gösteren bölümü aşağıda verilmiştir:

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

`Connect()` metodunun bir bölümünde çeviri seçeneklerini barındıran bir `SpeechClientOptions` örneği (bkz. `SpeechClientOptions.cs`) oluşturulur. Seçenekler hizmete bağlanmak için gerekli bilgileri (kimlik doğrulama anahtarı ve ana bilgisayar adı gibi) ve çeviri için kullanılan özellikleri içerir. Buradaki alanlar [Translator Konuşma Çevirisi API'si](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference) tarafından sunulan üst bilgi alanları ve HTTP parametreleriyle eşleşir.

`Connect()` ayrıca çevrilecek konuşmanın kaynağı olarak görev yapan ses girişi cihazını (`sampleProvider` değişkeni) oluşturur ve başlatır. Bu cihaz mikrofon gibi bir donanım giriş cihazı veya WAVE ses verileri içeren bir dosyadır.

Burada `speechClient` sınıfını başlatan ve hizmetten gelen metni ve ikili dosya yanıtlarını işlemek için anonim işlevler atayan `ConnectAsync()` metodu verilmiştir.

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

Kimlik doğrulama sonrasında metot `SpeechClient` örneğini başlatır. `SpeechClient` sınıfı (`SpeechClient.cs` içindeki) ikili dosya ve metin verileri alındıktan sonra olay işleyicilerini çağırır. Bağlantı başarısız olduğunda veya kesildiğinde ek işleyiciler çağrılır.

İkili veriler TTS etkinleştirildiğinde hizmet tarafından gönderilen seslerdir (metin okuma çıkışı). Metin verileri konuşulan metnin kısmi veya tam çevirisini içerir. Bu nedenle başlatma işleminin ardından metot şu iletileri işlemek için işlevler ayarlar: sesi daha sonra kayıttan yürütmek üzere kaydederek ve metni pencerede görüntüleyerek.

## <a name="next-steps"></a>Sonraki adımlar

Bu kod örneği Translator Konuşma Çevirisi API'sinin kullanımını gösteren zengin özellikli bir uygulamadır. Bu nedenle anlaşılması gereken birçok farklı parça vardır. Burada en önemli parçaları öğrendiniz. Geri kalanı için Visual Studio'da birkaç kesme noktası oluşturabilir ve çeviri sürecini adım adım inceleyebilirsiniz. Örnek uygulamayı kavradıktan sonra Translator Konuşma Çevirisi hizmetini kendi uygulamalarınızda kullanmaya başlayabilirsiniz.

> [!div class="nextstepaction"]
> [Microsoft Translator Konuşma Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
