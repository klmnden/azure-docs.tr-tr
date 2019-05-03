---
title: 'Hızlı Başlangıç: Özel ses öncelikli sanal asistan (Önizleme), Java (Windows, Linux) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir Java konsol uygulamasında Bilişsel hizmetler konuşma Yazılım Geliştirme Seti (SDK) kullanmayı öğreneceksiniz. Önceden oluşturulmuş bir Bot Framework botu doğrudan satır konuşma kanal kullanın ve ses öncelikli sanal asistan deneyimini etkinleştirmek üzere yapılandırılmış istemci uygulamanıza nasıl bağlanacağını öğreneceksiniz.
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: bidishac
ms.openlocfilehash: 4e9010bed54d0b2a7cb1a95b9e01e5ba02ea9fd5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027022"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Hızlı Başlangıç: Ses öncelikli sanal asistan konuşma SDK, Java oluşturma

Bu makalede, bir Java konsol uygulaması kullanarak oluşturduğunuz [Bilişsel hizmetler konuşma SDK'sı](speech-sdk.md). Uygulamayı doğrudan satır konuşma kanal kullanmak, bir ses İsteği Gönder ve ses yanıt etkinliğini (yapılandırılmışsa) iade için yapılandırılmış önceden yazılmış bir bot bağlanır. Uygulama konuşma SDK Maven paketini ve Windows, Ubuntu Linux veya macos'ta Eclipse Java IDE ile yerleşik olarak bulunur. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: (64-bit) Windows, Ubuntu Linux 16.04/18.04 (64-bit) ve macOS 10.13 veya üzeri
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* Bot Framework 4.2 sürümü kullanılarak oluşturulan önceden yapılandırılmış bir bot veya üzeri. Bot, ses giriş almak için yeni "Satır konuşma doğrudan" kanala abone olmak gerekir.

    > [!NOTE]
    > Önizlemede, doğrudan satır konuşma kanal şu anda yalnızca destekleyen **westus2** bölge.

    > [!NOTE]
    > 30 günlük deneme için standart fiyatlandırma katmanı açıklanan [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) sınırlıdır **westus** (değil **westus2**) ve bu nedenle uyumlu değil doğrudan ile Satır konuşma. Ücretsiz ve standart katmanı **westus2** abonelikleri uyumludur.

Ubuntu 16.04/18.04 çalıştırıyorsanız, Eclipse başlatmadan önce bu bağımlılıkların yüklü olduğundan emin olun:

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

Windows (64-bit) çalıştırıyorsanız, Microsoft Visual yüklediğinizden emin olun C++ platformunuz için yeniden dağıtılabilir:
* [Microsoft Visual C++ için Visual Studio 2017 yeniden dağıtılabilir'i indirin](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>İsteğe bağlı: Hızla kullanmaya başlayın

Bu hızlı başlangıçta, adım adım konuşma özellikli botunuzun bağlanmak için basit bir istemci uygulaması yapılacağını anlatmaktadır. Doğrudan içine dalmak tercih ederseniz, bu hızlı başlangıçta kullanılan tam, derleme için hazır kaynak kodu kullanılabilir [konuşma SDK örnekleri](https://aka.ms/csspeech/samples) altında `quickstart` klasör.

## <a name="create-and-configure-project"></a>Proje oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

Ayrıca, günlük kaydını etkinleştirmek için güncelleştirmesi **pom.xml** aşağıdaki bağımlılık dahil edilecek dosyası.

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>Örnek kod ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. Yeni oluşturulan açın **ana** sınıfı ve içeriklerini `Main.java` şu başlayan kod dosyası.

    ```java
    package speechsdk.quickstart;

    import java.io.IOException;
    import java.io.PipedOutputStream;
    import java.util.HashMap;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import com.microsoft.cognitiveservices.speech.ResultReason;
    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }
    }
    ```

1. İçinde **ana** yöntemi, ilk yapılandıracağınız, `BotConnectorConfig` ve oluşturmak için kullanmak bir `SpeechBotConnector` örneği. Bu, robotla etkileşim kurmak için doğrudan hat konuşma kanal bağlanır. Bir `AudioConfig` örneği ayrıca ses giriş kaynağını belirtmek için kullanılır. Bu örnekte, varsayılan mikrofon ile kullanılan `AudioConfig.fromDefaultMicrophoneInput()`.

    * Dize değiştirin `YourSubscriptionKey` sayfasından edinebilirsiniz abonelik anahtarınızı [burada](get-started.md).
    * Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili.
    * Dize değiştirin `YourChannelSecret` ile doğrudan hat konuşma kanal gizli anahtarı.

    > [!NOTE]
    > Önizlemede, doğrudan satır konuşma kanal şu anda yalnızca destekleyen **westus2** bölge.

    > [!NOTE]
    > 30 günlük deneme için standart fiyatlandırma katmanı açıklanan [konuşma Hizmetleri ücretsiz olarak deneyin](get-started.md) sınırlıdır **westus** (değil **westus2**) ve bu nedenle uyumlu değil doğrudan ile Satır konuşma. Ücretsiz ve standart katmanı **westus2** abonelikleri uyumludur.

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // your subscription key
    final String region = "YourServiceRegion"; // Your service region. Currently assumed to be westus2
    final BotConnectorConfig botConnectorConfig = BotConnectorConfig.fromSecretKey(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a SpeechjBotConnector instance
    final SpeechBotConnector botConnector = new SpeechBotConnector(botConnectorConfig, audioConfig);
    ```

1. `SpeechBotConnector` bot etkinlikleriyle, konuşma tanıma sonuçları ve diğer bilgileri iletişim kurmak için çeşitli olayları kullanır. Ardından bu olay dinleyicileri ekleyin.

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    botConnector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    botConnector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        botConnector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        String act = activityEventArgs.getActivity().serialize();
        log.info("Received activity: {}", act);
    });
    ```

1. Connect `SpeechBotConnector` çağırarak doğrudan satır konuşmaya `connectAsync()` yöntemi. Botunuzun test etmek için çağırabilirsiniz `listenOnceAsync` ses girişi mikrofondan göndermek için yöntemi. Ayrıca, ayrıca kullanabileceğiniz `sendActivityAsync` seri hale getirilmiş bir dize olarak özel etkinlik göndermek için yöntemi. Bu özel Aktiviteler botunuzun konuşmada kullanacağı ek veriler sağlayabilir.

    ```java
    botConnector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    botConnector.listenOnceAsync();

    // botConnector.sendActivityAsync(...)
    ```

1. Değişiklikleri kaydedilsin `Main` dosya.

1. Yanıt kayıttan yürütme desteklemek için ses desteklemek için yardımcı program yöntemleri içeren ek bir sınıf ekleyeceksiniz. Ses etkinleştirmek için başka bir yeni boş sınıf Java projenize ekleyin: seçin **dosya** > **yeni** > **sınıfı**.

1. İçinde **yeni Java sınıfı** penceresinde girin **speechsdk.quickstart** içine **paket** alan ve **AudioPlayer** içine**Adı** alan.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. Yeni oluşturulan açın **AudioPlayer** sınıfı ve aşağıda sağlanan kod ile değiştirin.

    ```java
    import static javax.sound.sampled.AudioFormat.Encoding.PCM_SIGNED;

    import java.io.InputStream;
    import java.io.PipedInputStream;
    import java.io.PipedOutputStream;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    import java.util.concurrent.atomic.AtomicBoolean;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;


    public class AudioPlayer {

        public static final int SAMPLE_RATE = 16000; // 16Hz sampling rate
        public static final int SAMPLE_SIZE_IN_BITS = 16; // 16 bit PCM
        public static final int CHANNELS = 1; // Use Mono / Single channel

        public static final int FRAME_RATE = 16000;
        public static final int FRAME_SIZE = 2;

        private static final Logger log = LoggerFactory.getLogger(AudioPlayer.class);
        private AtomicBoolean isPlaying = new AtomicBoolean(false);
        private ExecutorService executorService = Executors.newSingleThreadExecutor();

        public boolean isPlaying() {
            return isPlaying.get();
        }

        public void stopPlaying() {
            isPlaying.set(false);
        }

        public void play(final PipedOutputStream pipedOutputStream) {
            // The current audio supported by the Microsoft Bot framework ~ 16-bit PCM encoding, 16KHz sampling rate.
            final AudioFormat defaultFormat = new AudioFormat(PCM_SIGNED, SAMPLE_RATE, SAMPLE_SIZE_IN_BITS, CHANNELS, FRAME_SIZE, FRAME_RATE, false);
            try {
                final PipedInputStream inputStream = new PipedInputStream(pipedOutputStream);

                executorService.submit(() -> {
                    try {
                        isPlaying.set(true);
                        play(inputStream, defaultFormat);
                        inputStream.close();
                    } catch (Exception e) {
                        log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
                    }
                });
            } catch (Exception e) {
                log.error("Exception thrown during playback. Message: {}", e.getMessage(), e);
            }
        }

        private void play(final InputStream inputStream, final AudioFormat targetFormat) throws Exception {
            final byte[] buffer = new byte[1024];
            final DataLine.Info info = new DataLine.Info(SourceDataLine.class, targetFormat);
            final SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
            line.open();
            if (line != null) {
                line.start();
                int bytesRead = 0;
                while (bytesRead != -1) {
                    bytesRead = inputStream.read(buffer, 0, buffer.length);
                    if (bytesRead != -1) {
                        line.write(buffer, 0, bytesRead);
                    }
                }
                line.drain();
                line.stop();
                line.close();
            }
        }
    }
    ```

1. Değişiklikleri kaydedilsin `AudioPlayer` dosya.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.
Konsolu bir ileti "Deyin sorun" Bu noktada görüntüler, bir İngilizce ifade veya botunuzun anlayacaksınız cümle konuşurken. Konuşma botunuzun burada tanınır, doğrudan satır konuşma kanalı üzerinden için botunuzun tarafından işlenen aktarılır ve yanıt bir etkinlik döndürülür. Botunuzun yanıt olarak konuşma döndürürse, ses kullanarak yeniden yürütülür `AudioPlayer` sınıfı.

![Başarılı tanıma sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasından okuma gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı Başlangıç: Java (Windows, Linux) konuşma Çevir](quickstart-translate-speech-java-jre.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
