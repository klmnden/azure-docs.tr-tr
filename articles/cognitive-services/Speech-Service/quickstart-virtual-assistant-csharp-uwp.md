---
title: 'Hızlı Başlangıç: (Önizleme), özel sesli öncelikli sanal asistan C# (UWP) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu makalede, oluşturduğunuz bir C# Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK) kullanarak evrensel Windows Platformu (UWP) uygulama. İstemci uygulamanızı doğrudan satır konuşma kanal kullanacak şekilde yapılandırılmış daha önce oluşturulmuş bir Bot Framework botu için bağlarsınız. Uygulama, konuşma SDK'sı NuGet paketi ve Microsoft Visual Studio 2017 ile oluşturulmuştur.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: e2b25875a0dff12bba32b033bca0c35394d407aa
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465637"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-uwp"></a>Hızlı Başlangıç: Ses öncelikli sanal asistan UWP konuşma SDK ile oluşturma

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma metin](quickstart-csharp-uwp.md) ve [konuşma çevirisi](quickstart-translate-speech-uwp.md).

Bu makalede, geliştireceksiniz bir C# kullanarak evrensel Windows Platformu (UWP) uygulama [Speech SDK'sı](speech-sdk.md). Programın istemci uygulamadan alınan bir ses öncelikli sanal asistan deneyimi etkinleştirmek için daha önce yazılmış ve yapılandırılmış bir bot bağlanır. Uygulama [Konuşma SDK'sı NuGet Paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 (herhangi bir sürüm) ile geliştirilmiştir.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* Yapılandırılmış önceden oluşturulmuş bir bot [doğrudan satır konuşma kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

    > [!NOTE]
    > Önizlemede, doğrudan satır konuşma kanal şu anda yalnızca destekleyen **westus2** bölge.

    > [!NOTE]
    > 30 günlük deneme için standart fiyatlandırma katmanı açıklanan [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) sınırlıdır **westus** (değil **westus2**) ve bu nedenle uyumlu değil doğrudan ile Satır konuşma. Ücretsiz ve standart katmanı **westus2** abonelikleri uyumludur.

## <a name="optional-get-started-fast"></a>İsteğe bağlı: Hızla kullanmaya başlayın

Bu hızlı başlangıçta, adım adım konuşma özellikli botunuzun bağlanmak için basit bir istemci uygulaması yapılacağını anlatmaktadır. Doğrudan içine dalmak tercih ederseniz, bu hızlı başlangıçta kullanılan tam, derleme için hazır kaynak kodu kullanılabilir [konuşma SDK örnekleri](https://aka.ms/csspeech/samples) altında `quickstart` klasör.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kod ekleyin

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcının XAML görünümünde tüm içeriğiyle aşağıda.

    ```xml
    <Page
        x:Class="helloworld.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:helloworld"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    
        <Grid>
            <StackPanel Orientation="Vertical" HorizontalAlignment="Center"  Margin="20,50,0,0" VerticalAlignment="Center" Width="800">
                <Button x:Name="EnableMicrophoneButton" Content="Enable Microphone"  Margin="0,0,10,0" Click="EnableMicrophone_ButtonClicked" Height="35"/>
                <Button x:Name="ListenButton" Content="Talk to your bot" Margin="0,10,10,0" Click="ListenButton_ButtonClicked" Height="35"/>
                <StackPanel x:Name="StatusPanel" Orientation="Vertical" RelativePanel.AlignBottomWithPanel="True" RelativePanel.AlignRightWithPanel="True" RelativePanel.AlignLeftWithPanel="True">
                    <TextBlock x:Name="StatusLabel" Margin="0,10,10,0" TextWrapping="Wrap" Text="Status:" FontSize="20"/>
                    <Border x:Name="StatusBorder" Margin="0,0,0,0">
                        <ScrollViewer VerticalScrollMode="Auto"  VerticalScrollBarVisibility="Auto" MaxHeight="200">
                            <!-- Use LiveSetting to enable screen readers to announce the status update. -->
                            <TextBlock x:Name="StatusBlock" FontWeight="Bold" AutomationProperties.LiveSetting="Assertive"
                    MaxWidth="{Binding ElementName=Splitter, Path=ActualWidth}" Margin="10,10,10,20" TextWrapping="Wrap"  />
                        </ScrollViewer>
                    </Border>
                </StackPanel>
            </StackPanel>
            <MediaElement x:Name="mediaElement"/>
        </Grid>
    </Page>
    ```

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs`. Altında gruplandırılmış bulabilirsiniz `MainPage.xaml`. İçeriğini aşağıdaki kodla değiştirin. Neleri kapsar Bu örnek aşağıda verilmiştir: 

    * Konuşma ve Speech.Dialog ad alanları için using deyimleri
    * Mikrofon erişimi, bir düğme işleyicisi için kablolu emin olmak için basit bir uygulama
    * İletileri ve hataları uygulamada sunmak için temel UI Yardımcıları
    * Daha sonra doldurulacak başlatma kod yolu için bir giriş noktası
    * (Akış desteği olmadan) geri metin okuma yürütmek için bir yardımcı
    * Daha sonra doldurulacak dinlemeye başla için boş düğme işleyicisi

    ```csharp
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.CognitiveServices.Speech.Dialog;
    using System;
    using System.Diagnostics;
    using System.IO;
    using System.Text;
    using Windows.Foundation;
    using Windows.Storage.Streams;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Media;

    namespace helloworld
    {
        public sealed partial class MainPage : Page
        {
            private SpeechBotConnector botConnector;

            private enum NotifyType
            {
                StatusMessage,
                ErrorMessage
            };

            public MainPage()
            {
                this.InitializeComponent();
            }

            private async void EnableMicrophone_ButtonClicked(object sender, RoutedEventArgs e)
            {
                bool isMicAvailable = true;
                try
                {
                    var mediaCapture = new Windows.Media.Capture.MediaCapture();
                    var settings = new Windows.Media.Capture.MediaCaptureInitializationSettings();
                    settings.StreamingCaptureMode = Windows.Media.Capture.StreamingCaptureMode.Audio;
                    await mediaCapture.InitializeAsync(settings);
                }
                catch (Exception)
                {
                    isMicAvailable = false;
                }
                if (!isMicAvailable)
                {
                    await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
                }
                else
                {
                    NotifyUser("Microphone was enabled", NotifyType.StatusMessage);
                }
            }

            private void NotifyUser(string strMessage, NotifyType type = NotifyType.StatusMessage)
            {
                // If called from the UI thread, then update immediately.
                // Otherwise, schedule a task on the UI thread to perform the update.
                if (Dispatcher.HasThreadAccess)
                {
                    UpdateStatus(strMessage, type);
                }
                else
                {
                    var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
                }
            }

            private void UpdateStatus(string strMessage, NotifyType type)
            {
                switch (type)
                {
                    case NotifyType.StatusMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Green);
                        break;
                    case NotifyType.ErrorMessage:
                        StatusBorder.Background = new SolidColorBrush(Windows.UI.Colors.Red);
                        break;
                }
                StatusBlock.Text += string.IsNullOrEmpty(StatusBlock.Text) ? strMessage : "\n" + strMessage;

                if (!string.IsNullOrEmpty(StatusBlock.Text))
                {
                    StatusBorder.Visibility = Visibility.Visible;
                    StatusPanel.Visibility = Visibility.Visible;
                }
                else
                {
                    StatusBorder.Visibility = Visibility.Collapsed;
                    StatusPanel.Visibility = Visibility.Collapsed;
                }
                // Raise an event if necessary to enable a screen reader to announce the status update.
                var peer = Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer.FromElement(StatusBlock);
                if (peer != null)
                {
                    peer.RaiseAutomationEvent(Windows.UI.Xaml.Automation.Peers.AutomationEvents.LiveRegionChanged);
                }
            }

            // Waits for accumulates all audio associated with a given PullAudioOutputStream and then plays it to the
            // MediaElement. Long spoken audio will create extra latency and a streaming playback solution (that plays
            // audio while it continues to be received) should be used -- see the samples for examples of this.
            private void SynchronouslyPlayActivityAudio(PullAudioOutputStream activityAudio)
            {
                var playbackStreamWithHeader = new MemoryStream();
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("RIFF"), 0, 4); // ChunkID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // ChunkSize: max
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("WAVE"), 0, 4); // Format
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("fmt "), 0, 4); // Subchunk1ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 4); // Subchunk1Size: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // AudioFormat: PCM
                playbackStreamWithHeader.Write(BitConverter.GetBytes(1), 0, 2); // NumChannels: mono
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16000), 0, 4); // SampleRate: 16kHz
                playbackStreamWithHeader.Write(BitConverter.GetBytes(32000), 0, 4); // ByteRate
                playbackStreamWithHeader.Write(BitConverter.GetBytes(2), 0, 2); // BlockAlign
                playbackStreamWithHeader.Write(BitConverter.GetBytes(16), 0, 2); // BitsPerSample: 16-bit
                playbackStreamWithHeader.Write(Encoding.ASCII.GetBytes("data"), 0, 4); // Subchunk2ID
                playbackStreamWithHeader.Write(BitConverter.GetBytes(UInt32.MaxValue), 0, 4); // Subchunk2Size

                byte[] pullBuffer = new byte[2056];

                uint lastRead = 0;
                do
                {
                    lastRead = activityAudio.Read(pullBuffer);
                    playbackStreamWithHeader.Write(pullBuffer, 0, (int)lastRead);
                }
                while (lastRead == pullBuffer.Length);

                var task = Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
                {
                    mediaElement.SetSource(playbackStreamWithHeader.AsRandomAccessStream(), "audio/wav");
                    mediaElement.Play();
                });
            }

            private void InitializeBotConnector()
            {
                // New code will go here
            }

            private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
            {
                // New code will go here
            }
        }
    }
    ```

1. Ardından, oluşturacağınız `SpeechBotConnector` abonelik bilgilerinizle. Yöntem gövdesi için aşağıdakileri ekleyin `InitializeBotConnector`, dizeleri değiştirme `YourChannelSecret`, `YourSpeechSubscriptionKey`, ve `YourServiceRegion` konuşma abonelik botunuza ilişkin kendi değerlerinizle ve [bölge](regions.md).

    > [!NOTE]
    > Önizlemede, doğrudan satır konuşma kanal şu anda yalnızca destekleyen **westus2** bölge.

    > [!NOTE]
    > Botunuzun yapılandırma ve kanal gizli dizi alma hakkında daha fazla bilgi için Bot Framework belgelerine bakın [doğrudan satır konuşma kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

    ```csharp
    // create a BotConnectorConfig by providing a bot secret key and Cognitive Services subscription key
    // the RecoLanguage property is optional (default en-US); note that only en-US is supported in Preview
    const string channelSecret = "YourChannelSecret";
    const string speechSubscriptionKey = "YourSpeechSubscriptionKey";
    const string region = "YourServiceRegion"; // note: this is assumed as westus2 for preview

    var botConnectorConfig = BotConnectorConfig.FromSecretKey(channelSecret, speechSubscriptionKey, region);
    botConnectorConfig.SetProperty(PropertyId.SpeechServiceConnection_RecoLanguage, "en-US");
    botConnector = new SpeechBotConnector(botConnectorConfig);
    ```

1. `SpeechBotConnector` bot etkinlikleriyle, konuşma tanıma sonuçları ve diğer bilgileri iletişim kurmak için çeşitli olayları kullanır. Aşağıdaki yöntem gövdesini sonuna ekleme, bu olayları için işleyiciler eklemek `InitializeBotConnector`.

    ```csharp
    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.ActivityReceived += async (sender, activityReceivedEventArgs) =>
    {
        NotifyUser($"Activity received, hasAudio={activityReceivedEventArgs.HasAudio} activity={activityReceivedEventArgs.Activity}");

        if (activityReceivedEventArgs.HasAudio)
        {
            SynchronouslyPlayActivityAudio(activityReceivedEventArgs.Audio);
        }
    };
    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.Canceled += (sender, canceledEventArgs) =>
    {
        NotifyUser($"Canceled, reason={canceledEventArgs.Reason}");
        if (canceledEventArgs.Reason == CancellationReason.Error)
        {
            NotifyUser($"Error: code={canceledEventArgs.ErrorCode}, details={canceledEventArgs.ErrorDetails}");
        }
    };
    // Recognizing (not 'Recognized') will provide the intermediate recognized text while an audio stream is being processed
    botConnector.Recognizing += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Recognizing! in-progress text={recognitionEventArgs.Result.Text}");
    };
    // Recognized (not 'Recognizing') will provide the final recognized text once audio capture is completed
    botConnector.Recognized += (sender, recognitionEventArgs) =>
    {
        NotifyUser($"Final speech-to-text result: '{recognitionEventArgs.Result.Text}'");
    };
    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.SessionStarted += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Now Listening! Session started, id={sessionEventArgs.SessionId}");
    };
    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.SessionStopped += (sender, sessionEventArgs) =>
    {
        NotifyUser($"Listening complete. Session ended, id={sessionEventArgs.SessionId}");
    };
    ```

1. Oluşturulan yapılandırmasını ve kayıtlı olay işleyicilerinin `SpeechBotConnector` artık yalnızca dinleme gerekiyor. Gövdesi için aşağıdakileri ekleyin `ListenButton_ButtonClicked` yönteminde `MainPage` sınıfı.

    ```csharp
    private async void ListenButton_ButtonClicked(object sender, RoutedEventArgs e)
    {
        if (botConnector == null)
        {
            InitializeBotConnector();
            // Optional step to speed up first interaction: if not called, connection happens automatically on first use
            var connectTask = botConnector.ConnectAsync();
        }

        try
        {
            // Start sending audio to your speech-enabled bot
            var listenTask = botConnector.ListenOnceAsync();

            // You can also send activities to your bot as JSON strings -- Microsoft.Bot.Schema can simplify this
            string speakActivity = @"{""type"":""message"",""text"":""Greeting Message"", ""speak"":""Hello there!""}";
            await botConnector.SendActivityAsync(speakActivity);

        }
        catch (Exception ex)
        {
            NotifyUser($"Exception: {ex.ToString()}", NotifyType.ErrorMessage);
        }
    }
    ```

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğunda, Visual Studio'dan seçin **derleme** > **Çözümü Derle**. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğunda, Visual Studio'dan seçin **hata ayıklama** > **hata ayıklamayı Başlat**, veya basın **F5**.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir pencere açılır. Uygulamanızda seçin **etkinleştirme mikrofon**ve açılır izin isteği kabul etmiş olursunuz.

    ![İzin isteğinin ekran görüntüsü](media/sdk/qs-csharp-uwp-10-access-prompt.png "Uygulamayı hata ayıklamada başlat")

1. Seçin **konuşmak için botunuzun**ve bir İngilizce deyim ya da cümle cihazınızın mikrofona. Konuşma doğrudan satır konuşma kanalına iletilmesini ve penceresinde görüntülenen metne transcribed.
QuickStart-cs-uwp-bot-Successful-Turn

    ![Ekran görüntüsü başarılı bot kapatma](media/voice-first-virtual-assistants/quickstart-cs-uwp-bot-successful-turn.png "başarılı bot Aç")

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Keşfedin C# github'da örnekleri](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
