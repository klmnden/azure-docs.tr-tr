---
title: Android öğretici için API Java yüz | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Algılamak ve İnsan yüzeyleri görüntüdeki çerçeve için Bilişsel hizmetler yüz API'sini kullanan basit bir Android uygulaması oluşturma.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 5164a261d482d0cca3842a973d2109b17999bd25
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355187"
---
# <a name="getting-started-with-face-api-in-java-for-android-tutorial"></a>Android öğretici için Java API yüzeyi ile Başlarken

Bu öğreticide, oluşturmak ve İnsan yüzeyleri görüntüdeki algılamak için yüz API çağıran basit bir Android uygulama geliştirmek bilgi edineceksiniz. Uygulama algıladığı yüzeyleri çerçeveleme tarafından sonucu gösterir.

![GettingStartedAndroid](../Images/android_getstarted2.1.PNG)

## <a name="preparation"></a> Hazırlama

Öğretici kullanmak için aşağıdaki önkoşullar gerekir:

- Android Studio ve yüklü SDK'sı
- Android cihaz (isteğe test etmek için).

## <a name="step1"></a>1. adım: Yüz API için abone olma ve aboneliği anahtarınızı alın

Herhangi bir yazıtipi API'yi kullanmadan önce yüz API'sine Microsoft Bilişsel hizmetler Portalı'nda abone olmak kaydolmanız gerekir. Bkz: [abonelikleri](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide birincil ve ikincil anahtar kullanılabilir.

## <a name="step2"></a>2. adım: uygulama çerçevesi oluşturma

Bu adımda, çekme ve görüntüyü görüntülemek için temel kullanıcı arabirimini uygulamak için bir Android uygulaması oluşturacaksınız. Yalnızca aşağıdaki yönergeleri izleyin: 

1. Android Studio’yu açın.
2. Dosya menüsünde tıklatın **yeni proje...**
3. Uygulama adı **MyFirstApp**, İleri'yi tıklatın. 

    ![GettingStartAndroidNewProject](../Images/AndroidNewProject.png)

4. Gerekli hedef platformu seçin ve İleri'yi tıklatın. 

    ![GettingStartAndroidNewProject2](../Images/AndroidNewProject2.png)

5. Seçin **temel etkinlik** ve İleri'yi tıklatın.
6. Etkinlik gibi adlandırın ve Son'u tıklatın. 

    ![GettingStartAndroidNewProject4](../Images/AndroidNewProject4.png)

7. Açık **activity_main.xml**, bu etkinliğin Düzen Düzenleyicisi görmeniz gerekir.
8. Metin kaynak dosyasını görüntülemek ve etkinlik düzeni şekilde düzenleyin:

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">
     
        <ImageView
            android:layout_width="match_parent"
            android:layout_height="fill_parent"
            android:id="@+id/imageView1"
            android:layout_above="@+id/button1" />
    
        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Browse"
            android:id="@+id/button1"
            android:layout_alignParentBottom="true" />
    </RelativeLayout>
    ```  

9. Açık **MainActivity.java** ve dosyanın başına aşağıdaki içeri aktarma yönergelerini ekleyin:

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
    ```
      
    İkincisi, sınıfı aşağıdaki gibi değiştirin:  
    
    ```java
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
                Intent gallIntent = new Intent(Intent.ACTION_GET_CONTENT);
                gallIntent.setType("image/*");
                startActivityForResult(Intent.createChooser(gallIntent, "Select Picture"), PICK_IMAGE);
            }
        });
         
        detectionProgressDialog = new ProgressDialog(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
                } catch (IOException e) {
                e.printStackTrace();
                }
        }
    }
    ```

Artık uygulamanızı Fotoğraf galerisinden göz atın ve aşağıdaki görüntü benzer penceresinde görüntüleyin:

![GettingStartAndroidUI](../Images/android_getstarted1.1.PNG)

## <a name="step3"></a>3. adım: yüz API'si istemci kitaplığı yapılandırma

HTTPS kullanarak çağıran API istekleri bulut yüz API'dir. Yüz API .NET platformu uygulamalarında kullanmanın daha kullanışlı bir yol, bir istemci kitaplığı web kapsülleyen istekleri de sağlanır. Bu örnekte, bizim iş basitleştirmek için istemci kitaplığını kullanın. 

İstemci Kitaplığı yapılandırmak için aşağıdaki yönergeleri izleyin: 

1. Üst düzey bulun **build.gradle** projenizin örnekte gösterilen Proje panelinden dosya. Olduğunu birkaç diğer Not **build.gradle** proje ağacı ve dosyaları açmanız üst düzey **build.gradle** ilk dosya.
2. Ekleme **mavenCentral()** projelerinizi depoları için. Jcenter() mavenCentral() bir alt kümesi olduğundan, Android Studio varsayılan deposudur jcenter() de kullanabilirsiniz.  

```
    allprojects {
        repositories {
            ...
            mavenCentral()
        }
    }
```

3. Açık **build.gradle** 'app' proje dosyasında.
4. Maven merkezi deposunda saklanır bizim istemci kitaplığı için bir bağımlılık ekleyin:

```
    dependencies {  
        ...  
        implementation 'com.microsoft.projectoxford:face:1.4.3'  
    }
```

5. Açık **MainActivity.java** 'app' proje ve ekleme aşağıdaki yönergeleri alma: 
    
    ```java
    import com.microsoft.projectoxford.face.*;  
    import com.microsoft.projectoxford.face.contract.*;  
    ```
    
   Ardından, sınıfında aşağıdaki kodu ekleyin:

    ```java
    private FaceServiceClient faceServiceClient = new FaceServiceRestClient("your API endpoint", "<Subscription Key>");
    ```

   İlk parametre yukarıdaki adım 1'de anahtarınızı atandığı API uç noktası ile değiştirin. Örneğin:
   
        https://eastus2.api.cognitive.microsoft.com/face/v1.0
   
   İkinci parametresi, 1. adımda elde ettiğiniz abonelik anahtarı ile değiştirin.
   
6. Dosyayı açmak **AndroidManifest.xml** 'app' projenizdeki. Şu öğenin alt öğesi olarak Ekle **bildirim** öğe:  

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />  
    ```

7. Artık uygulamanızı yüz API'sini çağırmak hazırsınız. 

## <a name="step4"></a>4. adım: yüzeyleri algılamak için resimler yükleyin

Yüz algılama en kolay çağırarak yoludur [yüz – algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) görüntü dosyasını doğrudan yükleyerek API. İstemci Kitaplığı kullanırken, bu zaman uyumsuz yöntem kullanılarak yapılabilir **DetectAsync** , **FaceServiceClient** sınıfı. Her döndürülen yüz isteğe bağlı yüz öznitelikleri bir dizi birleştirilmiş konumunu belirtmek için bir dikdörtgen içerir. Bu örnekte, biz yalnızca yüz konumunu almak gerekir. Burada bir yönteme eklemek ihtiyacımız **MainActivity** sınıfı yüz algılama için: 

```java

    // Detect faces by uploading face images
    // Frame faces after detection
    
    private void detectAndFrame(final Bitmap imageBitmap)
    {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        imageBitmap.compress(Bitmap.CompressFormat.JPEG, 100, outputStream);
        ByteArrayInputStream inputStream = 
            new ByteArrayInputStream(outputStream.toByteArray());
        AsyncTask<InputStream, String, Face[]> detectTask =
            new AsyncTask<InputStream, String, Face[]>() {
                @Override
                protected Face[] doInBackground(InputStream... params) {
                    try {
                        publishProgress("Detecting...");
                        Face[] result = faceServiceClient.detect(
                                params[0], 
                                true,         // returnFaceId
                                false,        // returnFaceLandmarks
                                null           // returnFaceAttributes: a string like "age, gender"
                /* If you want value of FaceAttributes, try adding 4th argument like below.
                            new FaceServiceClient.FaceAttributeType[] {
                    FaceServiceClient.FaceAttributeType.Age,
                    FaceServiceClient.FaceAttributeType.Gender }
                */              
                        );
                        if (result == null)
                        {
                            publishProgress("Detection Finished. Nothing detected");
                            return null;
                        }
                        publishProgress(
                                String.format("Detection Finished. %d face(s) detected",
                                        result.length));
                        return result;
                    } catch (Exception e) {
                        publishProgress("Detection failed");
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
```

## <a name="step5"></a>5. adım: İşareti görüntüde yüzler

Bu son adımda, yukarıdaki adımların tümünü birleştirmek ve görüntünün çerçeveleri ile algılanan yüzeyleri işaretleyin. İlk olarak, açık **MainActivity.java** ve dikdörtgen çizmek için bir yardımcı yöntemi ekleyin: 

```java
    private static Bitmap drawFaceRectanglesOnBitmap(Bitmap originalBitmap, Face[] faces) {
        Bitmap bitmap = originalBitmap.copy(Bitmap.Config.ARGB_8888, true);
        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(Color.RED);
        int stokeWidth = 2;
        paint.setStrokeWidth(stokeWidth);
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

Şimdi Yapılacaklar bölümlerinde son **detectAndFrame** yüzeyleri çerçeve ve durum raporu için yöntem.

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
        if (result == null) return;
        ImageView imageView = (ImageView)findViewById(R.id.imageView1);
        imageView.setImageBitmap(drawFaceRectanglesOnBitmap(imageBitmap, result));
        imageBitmap.recycle();
    }
```
 
Son olarak, bir çağrı ekleyin **detectAndFrame** yönteminden **onActivityResult** aşağıda gösterildiği gibi yöntemi. 

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == PICK_IMAGE && resultCode == RESULT_OK && data != null && data.getData() != null) {
            Uri uri = data.getData();
            try {
                Bitmap bitmap = MediaStore.Images.Media.getBitmap(getContentResolver(), uri);
                ImageView imageView = (ImageView) findViewById(R.id.imageView1);
                imageView.setImageBitmap(bitmap);
     
                // This is the new addition.
                // detectAndFrame(bitmap);
     
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
```

Bu uygulama ve bir yazıtipi içeren bir görüntü için Gözat'ı çalıştırın. Yanıt için API bulut izin vermek birkaç saniye bekleyin. Bundan sonra bir sonuç görüntüsüne benzer karşılaşırsınız: 

![GettingStartAndroid](../Images/android_getstarted2.1.PNG)

## <a name="summary"></a> Özet

Bu öğreticide yüz API'yi kullanarak için temel işlem öğrenilen ve resimleri yüz işaretleri görüntülemek için bir uygulama oluşturulur. Nasıl yapılır konuları için yüz API'si hakkında daha fazla bilgi için bakın ve [API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236). 

## <a name="related"></a> İlgili öğreticiler

- [CSharp öğreticide API yüzeyi ile çalışmaya başlama](FaceAPIinCSharpTutorial.md)
- [Python öğreticide API yüzeyi ile çalışmaya başlama](FaceAPIinPythonTutorial.md)
