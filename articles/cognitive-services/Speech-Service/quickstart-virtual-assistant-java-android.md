---
title: 'Hızlı Başlangıç: Özel ses öncelikli sanal asistan (Önizleme), Java (Android) - konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Android Speech SDK'sı kullanarak Java'da bir ses öncelikli sanal asistan uygulaması oluşturmayı öğrenin
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: travisw
ms.openlocfilehash: c62402faa1995e1e992c8251ed87160a8f33d3a7
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67602738"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-in-java-on-android-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak bir ses öncelikli sanal asistan Android üzerinde Java oluşturun

Bir hızlı başlangıç için de kullanılabilir olan [konuşma metin](quickstart-java-android.md).

Bu makalede, Android kullanarak Java ile bir ses öncelikli sanal asistan oluşturacaksınız [Speech SDK'sı](speech-sdk.md). Bu uygulama, önceden yazılmış ve yapılandırılmış bir bot bağlanacağı [doğrudan satır konuşma kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech). Ardından bir ses isteği göndermek için robot ve bir ses etkin yanıt etkinliği sunar.

Bu uygulama, konuşma SDK Maven paketini ve Android Studio 3.3 ile oluşturulmuştur. Konuşma SDK’sı şu anda 32/64 bit ARM işlemcilerine sahip Android cihazlarıyla ve Intel x86/x64 uyumlu işlemcilerle uyumludur.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

* Konuşma Hizmetleri için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md) ya da üzerinde oluşturma [Azure portalında](https://portal.azure.com).
* Yapılandırılmış önceden oluşturulmuş bir bot [doğrudan satır konuşma kanal](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
* [Android Studio](https://developer.android.com/studio/) v3.3 veya üzeri

    > [!NOTE]
    > Doğrudan satır okuma (Önizleme) şu anda konuşma Hizmetleri bölgelerin alt kümesinde kullanılabilir. Lütfen [ses öncelikli sanal Yardımcıları için desteklenen bölgelerin listesini](regions.md#Voice-first virtual assistants) ve kaynaklarınız bu bölgelerden birinde dağıtıldığı emin olun.

## <a name="create-and-configure-a-project"></a>Projeyi oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-user-interface"></a>Kullanıcı arabirimi oluşturma

Bu bölümde, uygulama için bir temel kullanıcı arabirimi (UI) oluşturacağız. Ana etkinlik açarak başlayalım: `activity_main.xml`. Temel Şablon uygulamanın adını, bir başlık çubuğuyla içerir ve bir `TextView` "Hello world!" iletisi.

Ardından, içeriği değiştirin `activity_main.xml` aşağıdaki kod ile:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:onClick="onBotButtonClicked"
        android:text="Talk to your bot" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Recognition Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/recoText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="  \n(Recognition goes here)\n" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Activity Data"
        android:textSize="18dp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/activityText"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="vertical"
        android:text="  \n(Activities go here)\n" />

   </LinearLayout>
   ```

Bu XML botunuzun ile etkileşim kurmak için basit bir kullanıcı Arabirimi tanımlar.

* `button` Öğesi etkileşim başlatır ve çağıran `onBotButtonClicked` tıklandığında yöntemi.
* `recoText` Öğesi botunuz için konuşma olarak konuşma metin sonuçları görüntüler.
* `activityText` Öğesi en son botunuzun Bot Framework etkinliği için JSON yükü görüntülenir.

Metin ve grafik temsilini kullanıcı Arabirimi artık şöyle görünmelidir:

![](media/sdk/qs-java-android-assistant-designer-ui.png)

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Açık `MainActivity.java`ve içeriğini aşağıdaki kodla değiştirin:

   ```java
    package samples.speech.cognitiveservices.microsoft.com;

    import android.media.AudioFormat;
    import android.media.AudioManager;
    import android.media.AudioTrack;
    import android.support.v4.app.ActivityCompat;
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import android.text.method.ScrollingMovementMethod;
    import android.view.View;
    import android.widget.TextView;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConfig;
    import com.microsoft.cognitiveservices.speech.dialog.DialogServiceConnector;

    import org.json.JSONException;
    import org.json.JSONObject;

    import static android.Manifest.permission.*;

    public class MainActivity extends AppCompatActivity {
        // Replace below with your bot's own Direct Line Speech channel secret
        private static String channelSecret = "YourChannelSecret";
        // Replace below with your own speech subscription key
        private static String speechSubscriptionKey = "YourSpeechSubscriptionKey";
        // Replace below with your own speech service region (note: only a subset of regions are currently supported)
        private static String serviceRegion = "YourSpeechServiceRegion";

        private DialogServiceConnector connector;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            TextView recoText = (TextView) this.findViewById(R.id.recoText);
            TextView activityText = (TextView) this.findViewById(R.id.activityText);
            recoText.setMovementMethod(new ScrollingMovementMethod());
            activityText.setMovementMethod(new ScrollingMovementMethod());

            // Note: we need to request permissions for audio input and network access
            int requestCode = 5; // unique code for the permission request
            ActivityCompat.requestPermissions(MainActivity.this, new String[]{RECORD_AUDIO, INTERNET}, requestCode);
        }

        public void onBotButtonClicked(View v) {
            // Recreate the DialogServiceConnector on each button press, ensuring that the existing one is closed
            if (connector != null) {
                connector.close();
                connector = null;
            }

            // Create the DialogServiceConnector from the channel and speech subscription information
            DialogServiceConfig config = DialogServiceConfig.fromBotSecret(channelSecret, speechSubscriptionKey, serviceRegion);
            connector = new DialogServiceConnector(config, AudioConfig.fromDefaultMicrophoneInput());

            // Optional step: preemptively connect to reduce first interaction latency
            connector.connectAsync();

            // Register the DialogServiceConnector's event listeners
            registerEventListeners();

            // Begin sending audio to your bot
            connector.listenOnceAsync();
        }

        private void registerEventListeners() {
            TextView recoText = (TextView) this.findViewById(R.id.recoText); // 'recoText' is the ID of your text view
            TextView activityText = (TextView) this.findViewById(R.id.activityText); // 'activityText' is the ID of your text view

            // Recognizing will provide the intermediate recognized text while an audio stream is being processed
            connector.recognizing.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognizing: " + recoArgs.getResult().getText());
            });

            // Recognized will provide the final recognized text once audio capture is completed
            connector.recognized.addEventListener((o, recoArgs) -> {
                recoText.setText("  Recognized: " + recoArgs.getResult().getText());
            });

            // SessionStarted will notify when audio begins flowing to the service for a turn
            connector.sessionStarted.addEventListener((o, sessionArgs) -> {
                recoText.setText("Listening...");
            });

            // SessionStopped will notify when a turn is complete and it's safe to begin listening again
            connector.sessionStopped.addEventListener((o, sessionArgs) -> {
            });

            // Canceled will be signaled when a turn is aborted or experiences an error condition
            connector.canceled.addEventListener((o, canceledArgs) -> {
                recoText.setText("Canceled (" + canceledArgs.getReason().toString() + ") error details: {}" + canceledArgs.getErrorDetails());
                connector.disconnectAsync();
            });

            // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
            connector.activityReceived.addEventListener((o, activityArgs) -> {
                try {
                    // Here we use JSONObject only to "pretty print" the condensed Activity JSON
                    String rawActivity = activityArgs.getActivity().serialize();
                    String formattedActivity = new JSONObject(rawActivity).toString(2);
                    activityText.setText(formattedActivity);
                } catch (JSONException e) {
                    activityText.setText("Couldn't format activity text: " + e.getMessage());
                }

                if (activityArgs.hasAudio()) {
                    // Text-to-speech audio associated with the activity is 16 kHz 16-bit mono PCM data
                    final int sampleRate = 16000;
                    int bufferSize = AudioTrack.getMinBufferSize(sampleRate, AudioFormat.CHANNEL_OUT_MONO, AudioFormat.ENCODING_PCM_16BIT);

                    AudioTrack track = new AudioTrack(
                            AudioManager.STREAM_MUSIC,
                            sampleRate,
                            AudioFormat.CHANNEL_OUT_MONO,
                            AudioFormat.ENCODING_PCM_16BIT,
                            bufferSize,
                            AudioTrack.MODE_STREAM);

                    track.play();

                    PullAudioOutputStream stream = activityArgs.getAudio();

                    // Audio is streamed as it becomes available. Play it as it arrives.
                    byte[] buffer = new byte[bufferSize];
                    long bytesRead = 0;

                    do {
                        bytesRead = stream.read(buffer);
                        track.write(buffer, 0, (int) bytesRead);
                    } while (bytesRead == bufferSize);

                    track.release();
                }
            });
        }
    }
   ```

   * `onCreate` Yöntem, mikrofon ve internet izinleri isteyen kod içerir.

   * `onBotButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Bir düğme basma tek etkileşim ("Aç") ile botunuza tetikler.

   * `registerEventListeners` Yöntemi tarafından kullanılan olayları gösterir `DialogServiceConnector` ve temel işlemenin gelen etkinlikleri.

1. Aynı dosyada, yapılandırma dizeleri kaynaklarınızı eşleşecek şekilde değiştirin:

    * Değiştirin `YourChannelSecret` botunuza ilişkin doğrudan satır konuşma kanal gizli dizi ile.

    * `YourSpeechSubscriptionKey` değerini abonelik anahtarınızla değiştirin.

    * Değiştirin `YourServiceRegion` ile [bölge](regions.md) doğrudan satır Konuşma ile desteklenen yalnızca bir alt bölgelerin konuşma Hizmetleri aboneliğinizle ilişkili. Daha fazla bilgi için [bölgeleri](regions.md#voice-first-virtual-assistants).

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Cihazda [geliştirme modunu ve USB hata ayıklamasını](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun.

1. Uygulamayı derlemek için Ctrl+F9 tuşlarına basın veya menü çubuğundan **Derle** > **Proje Yap**'ı seçin.

1. Uygulamayı başlatmak için Shift+F10 tuşlarına basın veya **Çalıştır** >  **'uygulama' çalıştır**'ı seçin.

1. Görüntülenen dağıtım hedefi penceresinde Android cihazınızı seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](media/sdk/qs-java-android-12-deploy.png)

Uygulama ve onun etkinlik başlattıktan sonra botunuzun için konuşma başlatmak için düğmeye tıklayın. Transcribed metin olarak konuşan ve en son etkinlik sahip olduğunuz, robot alınan, alındığında görünür görünür. Botunuzun konuşulan yanıtlar sağlamak için yapılandırılmışsa, Konuşmayı metne dönüştürme otomatik olarak yürütülür.

![Android uygulamasının ekran görüntüsü](media/sdk/qs-java-android-assistant-completed-turn.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Temel robot oluşturup](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)

## <a name="see-also"></a>Ayrıca bkz.
- [Ses öncelikli sanal Yardımcıları](voice-first-virtual-assistants.md)
- [Bir konuşma Hizmetleri abonelik anahtarı ücretsiz olarak edinin](get-started.md)
- [Özel Uyandırma sözcükler](speech-devices-sdk-create-kws.md)
- [Botunuz için doğrudan satır konuşma bağlanma](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)
- [GitHub üzerinde Java örnekleri keşfedin](https://aka.ms/csspeech/samples)
