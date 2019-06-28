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
ms.openlocfilehash: f2cf65f9ee920b50af6242cee6b53cd07e53f0bc
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467028"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>Hızlı Başlangıç: Ses öncelikli sanal asistan konuşma SDK, Java oluşturma

Hızlı Başlangıçlar ücret karşılığında ayrıca [konuşma metin](quickstart-java-jre.md) ve [konuşma çevirisi](quickstart-translate-speech-java-jre.md).

Bu makalede, bir Java konsol uygulaması kullanarak oluşturduğunuz [Bilişsel hizmetler konuşma SDK'sı](speech-sdk.md). Uygulamayı doğrudan satır konuşma kanal kullanmak, bir ses İsteği Gönder ve ses yanıt etkinliğini (yapılandırılmışsa) iade için yapılandırılmış önceden yazılmış bir bot bağlanır. Uygulama konuşma SDK Maven paketini ve Windows, Ubuntu Linux veya macos'ta Eclipse Java IDE ile yerleşik olarak bulunur. 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: (64-bit) Windows, Ubuntu Linux 16.04/18.04 (64-bit) ve macOS 10.13 veya üzeri
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Konuşma Hizmetleri için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md) ya da üzerinde oluşturma [Azure portalında](https://portal.azure.com).
* Bot Framework 4.2 sürümü kullanılarak oluşturulan önceden yapılandırılmış bir bot veya üzeri. Bot, ses giriş almak için yeni "Satır konuşma doğrudan" kanala abone olmak gerekir.

    > [!NOTE]
    > Doğrudan satır okuma (Önizleme) şu anda konuşma Hizmetleri bölgelerin alt kümesinde kullanılabilir. Lütfen [ses öncelikli sanal Yardımcıları için desteklenen bölgelerin listesini](regions.md#Voice-first virtual assistants) ve kaynaklarınız bu bölgelerden birinde dağıtıldığı emin olun.

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

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Java projenize yeni bir boş sınıf eklemek için **Dosya** > **Yeni** > **Sınıf** seçeneklerini belirleyin.

1. **Yeni Java Sınıfı** penceresinde, **Paket** alanına **speechsdk.quickstart** ve **Ad** alanına da **Ana** girin.

   ![Yeni Java Sınıfı penceresinin ekran görüntüsü](media/sdk/qs-java-jre-06-create-main-java.png)

1. Yeni oluşturulan açın **ana** sınıfı ve içeriklerini `Main.java` şu başlayan kod dosyası.

    ```java
    package speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;
    import java.io.InputStream;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }

        private void playAudioStream(PullAudioOutputStream audio) {
            ActivityAudioStream stream = new ActivityAudioStream(audio);
            final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
            final AudioFormat format = new AudioFormat(
                    AudioFormat.Encoding.PCM_SIGNED,
                    audioFormat.getSamplesPerSecond(),
                    audioFormat.getBitsPerSample(),
                    audioFormat.getChannels(),
                    audioFormat.getFrameSize(),
                    audioFormat.getSamplesPerSecond(),
                    false);
            try {
                int bufferSize = format.getFrameSize();
                final byte[] data = new byte[bufferSize];

                SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
                SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
                line.open(format);

                if (line != null) {
                    line.start();
                    int nBytesRead = 0;
                    while (nBytesRead != -1) {
                        nBytesRead = stream.read(data);
                        if (nBytesRead != -1) {
                            line.write(data, 0, nBytesRead);
                        }
                    }
                    line.drain();
                    line.stop();
                    line.close();
                }
                stream.close();

            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }
    ```

1. İçinde **ana** yöntemi, ilk yapılandıracağınız, `DialogServiceConfig` ve oluşturmak için kullanmak bir `DialogServiceConnector` örneği. Bu, robotla etkileşim kurmak için doğrudan hat konuşma kanal bağlanır. Bir `AudioConfig` örneği ayrıca ses giriş kaynağını belirtmek için kullanılır. Bu örnekte, varsayılan mikrofon ile kullanılan `AudioConfig.fromDefaultMicrophoneInput()`.

    * Dize değiştirin `YourSubscriptionKey` sayfasından edinebilirsiniz abonelik anahtarınızı [burada](get-started.md).
    * Dize değiştirin `YourServiceRegion` ile [bölge](regions.md) aboneliğinizle ilişkili.
    * Dize değiştirin `YourChannelSecret` ile doğrudan hat konuşma kanal gizli anahtarı.

    > [!NOTE]
    > Doğrudan satır okuma (Önizleme) şu anda konuşma Hizmetleri bölgelerin alt kümesinde kullanılabilir. Lütfen [ses öncelikli sanal Yardımcıları için desteklenen bölgelerin listesini](regions.md#voice-first-virtual-assistants) ve kaynaklarınız bu bölgelerden birinde dağıtıldığı emin olun.

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // Your subscription key
    final String region = "YourServiceRegion"; // Your speech subscription service region. Note: only a subset of regions are currently supported
    final DialogServiceConfig botConfig = DialogServiceConfig.fromBotSecret(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a DialogServiceConnector instance
    final DialogServiceConnector connector = new DialogServiceConnector(botConfig, audioConfig);
    ```

1. `DialogServiceConnector` bot etkinlikleriyle, konuşma tanıma sonuçları ve diğer bilgileri iletişim kurmak için çeşitli olayları kullanır. Ardından bu olay dinleyicileri ekleyin.

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    connector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    connector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    connector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    connector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    connector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        connector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
    connector.activityReceived.addEventListener((o, activityEventArgs) -> {
        final String act = activityEventArgs.getActivity().serialize();
            log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
            if (activityEventArgs.hasAudio()) {
                playAudioStream(activityEventArgs.getAudio());
            }
        });
    ```

1. Connect `DialogServiceConnector` çağırarak doğrudan satır konuşmaya `connectAsync()` yöntemi. Botunuzun test etmek için çağırabilirsiniz `listenOnceAsync` ses girişi mikrofondan göndermek için yöntemi. Ayrıca, ayrıca kullanabileceğiniz `sendActivityAsync` seri hale getirilmiş bir dize olarak özel etkinlik göndermek için yöntemi. Bu özel Aktiviteler botunuzun konuşmada kullanacağı ek veriler sağlayabilir.

    ```java
    connector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    connector.listenOnceAsync();

    // connector.sendActivityAsync(...)
    ```

1. Değişiklikleri kaydedilsin `Main` dosya.

1. Yanıt kayıttan yürütme desteklemek için getAudio() işleme API bir kolayca için Java'ya InputStream döndürülen PullAudioOutputStream nesne dönüştüren ek bir sınıf ekleyeceksiniz. Bu ActivityAudioStream, "doğrudan satır konuşma kanalı" ses yanıttan işleyecek özel bir sınıftır. Bu işleme kayıttan yürütme için gerekli ses biçimi bilgileri getirmek için erişimciler sağlar: İçin seçme **dosya** > **yeni** > **sınıfı**.

1. İçinde **yeni Java sınıfı** penceresinde girin **speechsdk.quickstart** içine **paket** alan ve **ActivityAudioStream** içine **Adı** alan.

1. Yeni oluşturulan açın **ActivityAudioStream** sınıfı ve aşağıda sağlanan kod ile değiştirin.

    ```java
    package com.speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

    import java.io.IOException;
    import java.io.InputStream;


    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second. (16 kHz)
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format. (16 bits)
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample and the # channels
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

    ```

1. Değişiklikleri kaydedilsin `ActivityAudioStream` dosya.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.
Konsolu bir ileti "Deyin sorun" Bu noktada görüntüler, bir İngilizce ifade veya botunuzun anlayacaksınız cümle konuşurken. Konuşma botunuzun burada tanınır, doğrudan satır konuşma kanalı üzerinden için botunuzun tarafından işlenen aktarılır ve yanıt bir etkinlik döndürülür. Botunuzun yanıt olarak konuşma döndürürse, ses kullanarak yeniden yürütülür `AudioPlayer` sınıfı.

![Başarılı tanıma sonrası konsol çıktısının ekran görüntüsü](media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşma bir ses dosyasından okuma gibi ek örnekler Github'da kullanılabilir.

> [!div class="nextstepaction"]
> [Temel robot oluşturup](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses öncelikli sanal Yardımcıları](voice-first-virtual-assistants.md)
- [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
- [Özel Uyandırma sözcükler](speech-devices-sdk-create-kws.md)
- [Botunuz için doğrudan satır konuşma bağlanma](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)
