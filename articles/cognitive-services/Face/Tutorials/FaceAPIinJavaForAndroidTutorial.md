---
title: 'Öğretici: Android SDK ile görüntüdeki yüzleri algılama ve çerçeveleme'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir görüntüdeki yüzleri algılamak ve çerçevelemek için Yüz Tanıma API’sini kullanan basit bir Android uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 07/12/2018
ms.author: pafarley
ms.openlocfilehash: 99b2734745df722f45443b5347ae6dd054c8aa31
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957046"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için Android uygulaması oluşturma

Bu öğreticide, resimdeki insan yüzlerini algılamak için Yüz Tanıma hizmeti Java sınıf kitaplığını kullanan basit bir Android uygulaması oluşturursunuz. Uygulama, seçilen her resmi algılanan yüzler dikdörtgen bir çerçeve içine alınmış olarak gösterir. Örnek kodun tamamı GitHub'da [Android'de resimdeki yüzleri algılama ve çerçeve içine alma](https://github.com/Azure-Samples/cognitive-services-face-android-sample).

![Yüzleri kırmızı dikdörtgenle çerçeve içine alınmış bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - Android uygulaması oluşturma
> - Yüz Tanıma hizmeti istemci kitaplığını yükleme
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme

## <a name="prerequisites"></a>Ön koşullar

- Örneği çalıştırmanız için bir abonelik anahtarınız olmalıdır. [Bilişsel Hizmetleri Deneme](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz deneme abonelik anahtarları alabilirsiniz.
- [Android Studio](https://developer.android.com/studio/) minimum SDK 22 ile (Yüz Tanıma istemci kitaplığı için gerekir).
- Maven'den [com.microsoft.projectoxford:face:1.4.3](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.microsoft.projectoxford%22) Yüz Tanıma istemci kitaplığı. Paketi indirmek gerekli değildir. Yükleme yönergeleri aşağıda verilmiştir.

## <a name="create-the-project"></a>Proje oluşturma

Aşağıdaki adımları izleyerek Android uygulama projenizi oluşturun:

1. Android Studio’yu açın. Bu öğreticide Android Studio 3.1 kullanılır.
1. **Yeni Android Studio projesi başlat**'ı seçin.
1. **Android Projesi Oluştur** ekranında, gerekirse varsayılan alanları değiştirin ve sonra da **İleri**'ye tıklayın.
1. **Hedef Android Cihazları** ekranında, aşağı açılan seçiciyi kullanarak **API 22** veya üstünü seçin, sonra da **İleri**'ye tıklayın.
1. **Boş Etkinlik** öğesini seçin ve **İleri**'ye tıklayın.
1. **Geriye Dönük Uyumluluk** öğesinin işaretini kaldırın ve **Son**'a tıklayın.

## <a name="create-the-ui-for-selecting-and-displaying-the-image"></a>Resmi seçmek ve görüntülemek için kullanıcı arabirimi oluşturma

*Activity_main.xml* dosyasını açın; Yerleşim Düzenleyicisi'ni görüyor olmalısınız. **Metin** sekmesini seçin ve içeriği aşağıdaki kodla değiştirin.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:id="@+id/imageView1"
        android:layout_above="@+id/button1"
        android:contentDescription="Image with faces to analyze"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Browse for face image"
        android:id="@+id/button1"
        android:layout_alignParentBottom="true"/>
</RelativeLayout>
```

*MainActivity.java*'yı açın ve ilk `package` deyimi dışındaki her şeyi aşağıdaki kodla değiştirin.

Kod, olay işleyicisinde kullanıcının resim seçmesini sağlamak için yeni bir etkinlik başlatan `Button` öğesini ayarlar. Bir kez seçildikten sonra, resim `ImageView` içinde görüntülenir.

```java
import java.io.*;
import android.app.*;
import android.content.*;
import android.net.*;
import android.os.*;
import android.view.*;
import android.graphics.*;
import android.widget.*;
import android.provider.*;

public class MainActivity extends Activity {
    private final int PICK_IMAGE = 1;
    private ProgressDialog detectionProgressDialog;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            Button button1 = (Button)findViewById(R.id.button1);
            button1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
                intent.setType("image/*");
                startActivityForResult(Intent.createChooser(
                        intent, "Select Picture"), PICK_IMAGE);
            }
        });

        detectionProgressDialog = new ProgressDialog(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                        getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);

                // Uncomment
                //detectAndFrame(bitmap);
                } catch (IOException e) {
                    e.printStackTrace();
                }
        }
    }
}
```

Artık uygulamanız bir fotoğraf bulabilir ve aşağıdaki görüntüde gösterildiği gibi bunu pencerede görüntüler.

![Yüzler içeren bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted1.1.PNG)

## <a name="configure-the-face-client-library"></a>Yüz Tanıma istemci kitaplığını yapılandırma

Yüz Tanıma API'si, HTTPS isteklerini kullanarak çağırabileceğiniz bir bulut API'sidir. Bu öğreticide, çalışmanızı basitleştirmek için bu web isteklerini kapsülleyen Yüz Tanıma istemci kitaplığı kullanılır.

**Proje** bölmesinde, aşağı açılan seçiciyi kullanarak **Android**'i seçin. **Gradle Scripts**'i genişletin, sonra *build.gradle (Modül: uygulama)* öğesini açın.

Aşağıdaki ekran görüntüsünde gösterildiği gibi Yüz Tanıma istemci kitaplığı `com.microsoft.projectoxford:face:1.4.3` için bir bağımlılık ekleyin, sonra da **Şimdi Eşitle**'ye tıklayın.

![Uygulama build.gradle dosyasının Android Studio ekran görüntüsü](../Images/face-tut-java-gradle.png)

**MainActivity.java**'yı açın ve aşağıdaki içeri aktarma yönergelerini ekleyin:

```java
import com.microsoft.projectoxford.face.*;
import com.microsoft.projectoxford.face.contract.*;
```

## <a name="add-the-face-client-library-code"></a>Yüz Tanıma istemci kitaplığı kodunu ekleme

Aşağıdaki kodu `MainActivity` sınıfına, `onCreate` yönteminin üst kısmına ekleyin:

```java
private final String apiEndpoint = "<API endpoint>";
private final String subscriptionKey = "<Subscription Key>";

private final FaceServiceClient faceServiceClient =
        new FaceServiceRestClient(apiEndpoint, subscriptionKey);
```

`<API endpoint>` değerini anahtarınıza atanmış olan API uç noktasıyla değiştirin. Ücretsiz deneme abonelik anahtarları **westcentralus** bölgesinde oluşturulur. Dolayısıyla, ücretsiz bir deneme abonelik anahtarı kullanıyorsanız, deyim aşağıdaki gibi olabilir:

```java
apiEndpoint = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
```

`<Subscription Key>` değerini abonelik anahtarınızla değiştirin. Örnek:

```java
subscriptionKey = "0123456789abcdef0123456789ABCDEF"
```

**Proje** bölmesinde **uygulamayı**, sonra da **bildirimleri** genişletin ve *AndroidManifest.xml* dosyasını açın.

Aşağıdaki öğeyi `manifest` öğesinin doğrudan alt öğesi olarak ekleyin:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Projenizi derleyip hataları denetleyin. Artık Yüz Tanıma hizmetini çağırmaya hazırsınız.

## <a name="upload-an-image-to-detect-faces"></a>Yüzlerin algılanması için resmi karşıya yükleme

Yüzleri algılamanın en kolay yolu `FaceServiceClient.detect` yöntemini çağırmaktır. Bu yöntem [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API yöntemini sarmalar ve bir `Face` dizisi döndürür.

Döndürülen her `Face`, konumunu göstermek için bir dikdörtgen ile birlikte bir dizi isteğe bağlı yüz özniteliği içerir. Bu örnekte, yalnızca yüzlerin konumları gereklidir.

Hata oluşursa, bir `AlertDialog` temel nedeni görüntüler.

Aşağıdaki yöntemleri `MainActivity` sınıfına ekleyin.

```java
// Detect faces by uploading a face image.
// Frame faces after detection.
private void detectAndFrame(final Bitmap imageBitmap) {
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
    ByteArrayInputStream inputStream =
            new ByteArrayInputStream(outputStream.toByteArray());

    AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                String exceptionMessage = "";

                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0],
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null          // returnFaceAttributes:
                                /* new FaceServiceClient.FaceAttributeType[] {
                                    FaceServiceClient.FaceAttributeType.Age,
                                    FaceServiceClient.FaceAttributeType.Gender }
                                */
                        );
                        if (result == null){
                            publishProgress(
                                    "Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(String.format(
                                "Detection Finished. %d face(s) detected",
                                result.length));
                        return result;
                    } catch (Exception e) {
                        exceptionMessage = String.format(
                                "Detection failed: %s", e.getMessage());
                        return null;
                    }
                }

                @Override
                protected void onPreExecute() {
                    //TODO: show progress dialog
                }
                @Override
                protected void onProgressUpdate(String... progress) {
                    //TODO: update progress
                }
                @Override
                protected void onPostExecute(Face[] result) {
                    //TODO: update face frames
                }
            };

    detectTask.execute(inputStream);
}

private void showError(String message) {
    new AlertDialog.Builder(this)
    .setTitle("Error")
    .setMessage(message)
    .setPositiveButton("OK", new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int id) {
        }})
    .create().show();
}
```

## <a name="frame-faces-in-the-image"></a>Resimdeki yüzleri çerçeve içine alma

Aşağıdaki yardımcı yöntemi `MainActivity` sınıfına ekleyin. Bu yöntem, algılanan her yüzün çevresine bir dikdörtgen çizer.

```java
private static Bitmap drawFaceRectanglesOnBitmap(
        Bitmap originalBitmap, Face[] faces) {
    Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
    Canvas canvas = new Canvas(bitmap);
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(Color.RED);
    paint.setStrokeWidth(10);
    if (faces != null) {
        for (Face face : faces) {
            FaceRectangle faceRectangle = face.faceRectangle;
            canvas.drawRect(
                    faceRectangle.left,
                    faceRectangle.top,
                    faceRectangle.left + faceRectangle.width,
                    faceRectangle.top + faceRectangle.height,
                    paint);
        }
    }
    return bitmap;
}
```

`detectAndFrame` yönteminde `TODO` açıklamalarıyla gösterilen `AsyncTask` yöntemlerini tamamlayın. Başarılı olduğunda, seçilen resim `ImageView` içinde çerçeve içine alınmış yüzlerle görüntülenir.

```java
@Override
protected void onPreExecute() {
    detectionProgressDialog.show();
}
@Override
protected void onProgressUpdate(String... progress) {
    detectionProgressDialog.setMessage(progress[0]);
}
@Override
protected void onPostExecute(Face[] result) {
    detectionProgressDialog.dismiss();
    if(!exceptionMessage.equals("")){
        showError(exceptionMessage);
    }
    if (result == null) return;
    ImageView imageView = findViewById(R.id.imageView1);
    imageView.setImageBitmap(
            drawFaceRectanglesOnBitmap(imageBitmap, result));
    imageBitmap.recycle();
}
```

Son olarak, `onActivityResult` yönteminde `detectAndFrame` yöntemine yapılan çağrıyı aşağıda gösterildiği gibi açıklama olmaktan çıkarın.

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == PICK_IMAGE && resultCode == RESULT_OK &&
                data != null && data.getData() != null) {
        Uri uri = data.getData();
        try {
            Bitmap bitmap = MediaStore.Images.Media.getBitmap(
                    getContentResolver(), uri);
            ImageView imageView = findViewById(R.id.imageView1);
            imageView.setImageBitmap(bitmap);

            // Uncomment
            detectAndFrame(bitmap);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüzlerin yer aldığı bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Bundan sonra, aşağıdaki resme benzer bir sonuç elde edersiniz:

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a>Özet

Bu öğreticide, Yüz Tanıma hizmetini kullanmanın temel sürecini öğrendiniz ve resimde çerçeve içine almak yüzleri görüntüleyen bir uygulama oluşturdunuz.

## <a name="next-steps"></a>Sonraki adımlar

Yüz işaretlerini algılama ve kullanma hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

Yüzleri algılamak için kullanılan Yüz Tanıma API'lerini ve poz, cinsiyet, yaş, kafa duruşu, sakal, bıyık ve gözlükler gibi bunların özniteliklerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).