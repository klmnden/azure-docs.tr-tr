---
title: 'Öğretici: Görüntü işlemleri - Java'
titlesuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’de Görüntü İşleme API’sini kullanan temel bir Java Swing uygulamasını keşfedin. OCR gerçekleştirin, küçük resimler oluşturun ve bir görüntüdeki görsel özelliklerle çalışın.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.author: kefre
ms.custom: seodec18
ms.date: 09/21/2017
ms.openlocfilehash: 4f6af31ba6b04ddbecb7cb42cebe345b6af720ac
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201440"
---
# <a name="tutorial-computer-vision-api-java"></a>Öğretici: Bilgisayar görüntü işleme API'si Java

Bu öğreticide, Azure Bilişsel Hizmetler Görüntü İşleme REST API’sinin özellikleri gösterilmektedir.

Optik karakter tanıma (OCR) gerçekleştirmek, akıllı kırpılmış küçük resimler oluşturmak, ayrıca bir görüntüdeki yüzler gibi görsel özellikleri algılamak, kategorilere ayırmak, etiketlemek ve açıklamak için Görüntü İşleme REST API’sini kullanan bir Java Swing uygulamasını keşfedin. Bu örnek, analiz veya işleme için bir görüntü URL’si göndermenize olanak sağlar. Bu açık kaynak örneğini, Görüntü İşleme API’sini kullanmak amacıyla Java’da kendi uygulamanızı derlemek için şablon olarak kullanabilirsiniz.

Bu öğreticide, Görüntü İşleme’nin nasıl kullanılacağı ele alınacaktır:

> [!div class="checklist"]
> * Resim çözümleme
> * Bir görüntüdeki doğal ve yapay yer işaretlerini belirleme
> * Bir görüntüdeki ünlüyü belirleme
> * Bir görüntüden kaliteli küçük resim oluşturma
> * Bir görüntüdeki yazdırılan metni okuma
> * Bir görüntüdeki el yazısı metni okuma

Java Swing form uygulaması zaten yazılmıştır, ancak bir işlevselliğe sahip değildir. Bu öğreticide, uygulamanın işlevselliğini tamamlamak için Görüntü İşleme API'sine özgü kodu ekleyeceksiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğretici, NetBeans IDE kullanılarak geliştirilmiştir. Özellikle, [buradan indirebileceğiniz](https://netbeans.org/downloads/index.html) NetBeans’in **Java SE** sürümü.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Görüntü İşleme API’sine abone olma ve abonelik anahtarı alma 

Örneği oluşturmadan önce, Azure Bilişsel Hizmetler’in parçası olan Görüntü İşleme API’sine abone olmanız gerekir. Abonelik ve anahtar yönetimi ayrıntıları için bkz. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/). Bu öğreticide hem birincil hem de ikincil anahtarlar kullanılabilir. 

## <a name="acquire-the-incomplete-tutorial-project"></a>Tamamlanmamış öğretici projesini alma

### <a name="download-the-tutorial-project"></a>Öğretici projesini indirme

1. [Bilişsel Hizmetler Java Görüntü İşleme Öğreticisi](https://github.com/Azure-Samples/cognitive-services-java-computer-vision-tutorial) deposuna gidin.
1. **Kopyala veya indir** düğmesine tıklayın.
1. Öğretici projesinin .zip dosyasını indirmek için **ZIP İndir**’e tıklayın.

NetBeans, .zip dosyasından projeyi içeri aktardığından, .zip dosyasının içeriklerinin ayıklanması gerekmez.

### <a name="import-the-tutorial-project"></a>Öğretici projesini içeri aktarma

**cognitive-services-java-computer-vision-tutorial-master.zip** dosyasını NetBeans’e içeri aktarın.

1. NetBeans’te **Dosya** > **Projeyi İçeri Aktar** > **ZIP’ten...** seçeneklerine tıklayın. **ZIP’ten Projeleri İçeri Aktar** iletişim kutusu görüntülenir.
1. **ZIP Dosyası:** alanında **Gözat** düğmesine tıklayarak **cognitive-services-java-computer-vision-tutorial-master.zip** dosyasını bulun ve **Aç**’a tıklayın.
1. **ZIP’ten Projeleri İçeri Aktar** iletişim kutusundan **İçeri Aktar**’a tıklayın.
1. **Projeler** panelinde **ComputerVision** > **Kaynak Paketleri** > **&lt;varsayılan paket&gt;** seçeneklerini genişletin. 
   Bazı NetBeans sürümleri, **Kaynak Paketleri** > **&lt;varsayılan paket&gt;** yerine **src** öğesini kullanır. Bu durumda, **src** öğesini genişletin.
1. **MainFrame.java** seçeneğine çift tıklayarak dosyayı NetBeans düzenleyiciye yükleyin. **MainFrame.java** dosyasının **Tasarım** sekmesi görüntülenir.
1. Java kaynak kodunu görüntülemek için **Kaynak** sekmesine tıklayın.

### <a name="build-and-run-the-tutorial-project"></a>Öğretici projesini derleme ve çalıştırma

1. Öğretici uygulamasını derleyip çalıştırmak için **F6** tuşuna basın.

    Öğretici uygulamasında, bu özelliğe yönelik bölmeyi getirmek için bir sekmeye tıklayın. Düğmelerin boş yöntemleri vardır, bu nedenle herhangi bir şey yapmazlar.

    Pencerenin alt kısmında **Abonelik Anahtarı** ve **Abonelik Bölgesi** alanları yer alır. Bu alanlar, geçerli bir abonelik anahtarıyla ve o abonelik anahtarı için doğru bölgeyle doldurulmalıdır. Bir abonelik anahtarı almak için bkz. [Abonelikler](https://azure.microsoft.com/try/cognitive-services/). Bu bağlantıdaki ücretsiz deneme sürümünden aboneliğinizi aldıysanız varsayılan bölge olan **westcentralus**, abonelik anahtarlarınız için doğru bölgedir.

1. Öğretici uygulamasından çıkın.

## <a name="add-the-tutorial-code-to-the-project"></a>Öğretici kodunu projeye ekleme

Java Swing uygulaması altı sekme ile ayarlanır. Her sekme, Görüntü İşleme’nin farklı bir işlevini (analiz etme, OCR vb.) gösterir. Altı öğretici bölümünün birbirine bağımlılıkları yoktur, bu nedenle tek bir bölümü, altı bölümün tümünü veya bir alt kümesini ekleyebilirsiniz. Bölümleri herhangi bir sırayla da ekleyebilirsiniz.

### <a name="analyze-an-image"></a>Resim çözümleme

Görüntü İşleme’nin Analiz özelliği, bir görüntüyü 2.000’den fazla tanınabilir nesne, canlı varlık, sahne ve eylem açısından tarar. Analiz tamamlandıktan sonra Analiz işlevi, açıklayıcı etiketler, renk analizi, açıklamalı alt yazılar vb. ile görüntüyü açıklayan bir JSON nesnesi döndürür.

Öğretici uygulamasının Analiz özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**analyzeImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **AnalyzeImage** yöntemini çağırır. **AnalyzeImage** döndürüldüğünde yöntem, biçimlendirilmiş JSON yanıtını **Yanıt** metin alanında görüntüler, **JSONObject** öğesinden birinci açıklamalı alt yazıyı ayıklar ve açıklamalı alt yazıyı ve açıklamalı alt yazının doğruluğuna yönelik güvenilirlik düzeyini görüntüler.

Aşağıdaki kodu kopyalayıp **analyzeImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void analyzeImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL analyzeImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        analyzeImage.setIcon(new ImageIcon());
        analyzeCaptionLabel.setText("");
        analyzeResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            analyzeImageUrl = new URL(analyzeImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(analyzeImageUrl);
            scaleAndShowImage(bImage, analyzeImage);
        } catch(IOException e) {
            analyzeResponseTextArea.setText("Error loading Analyze image: " + e.getMessage());
            return;
        }
        
        // Analyze the image.
        JSONObject jsonObj = AnalyzeImage(analyzeImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        analyzeResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("description") && jsonObj.getJSONObject("description").has("captions")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("description").getJSONArray("captions").getJSONObject(0);
            
            if (jsonCaption.has("text") && jsonCaption.has("confidence")) {
                
                analyzeCaptionLabel.setText("Caption: " + jsonCaption.getString("text") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**AnalyzeImage** yöntemi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Yöntem, görüntüyü açıklayan bir **JSONObject** öğesini veya bir hata varsa **null** değerini döndürür.

**AnalyzeImage** yöntemini kopyalayıp **analyzeImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
    /**
     * Encapsulates the Microsoft Cognitive Services REST API call to analyze an image.
     * @param imageUrl: The string URL of the image to analyze.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject AnalyzeImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call for Analyze Image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseAnalyze;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color,Adult");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
 ```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. Analiz edilecek görüntünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="recognize-a-landmark"></a>Yer işaretini tanıma

Görüntü İşleme’nin Yer İşareti özelliği, bir görüntüyü dağ veya ünlü binalar gibi doğal ve yapay yer işaretleri açısından analiz eder. Analiz tamamlandıktan sonra Yer İşareti, görüntüde bulunan yer işaretlerini belirleyen bir JSON nesnesini döndürür.

Öğretici uygulamasının Yer İşareti özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**landmarkImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **LandmarkImage** yöntemini çağırır. **LandmarkImage** döndürüldüğünde yöntem, **Yanıt** metin alanında biçimlendirilmiş JSON yanıtını görüntüler, daha sonra **JSONObject** öğesinden ilk yer işareti adını ayıklar ve bunu yer işaretinin doğru şekilde belirlendiği güvenilirlik düzeyiyle birlikte pencerede görüntüler.

Aşağıdaki kodu kopyalayıp **landmarkImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void landmarkImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL landmarkImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        landmarkImage.setIcon(new ImageIcon());
        landmarkCaptionLabel.setText("");
        landmarkResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            landmarkImageUrl = new URL(landmarkImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(landmarkImageUrl);
            scaleAndShowImage(bImage, landmarkImage);
        } catch(IOException e) {
            landmarkResponseTextArea.setText("Error loading Landmark image: " + e.getMessage());
            return;
        }
        
        // Identify the landmark in the image.
        JSONObject jsonObj = LandmarkImage(landmarkImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        landmarkResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("result") && jsonObj.getJSONObject("result").has("landmarks")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("result").getJSONArray("landmarks").getJSONObject(0);
            
            if (jsonCaption.has("name") && jsonCaption.has("confidence")) {

                landmarkCaptionLabel.setText("Caption: " + jsonCaption.getString("name") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**LandmarkImage** yöntemi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Yöntem, görüntüde bulunan yer işaretlerini açıklayan bir **JSONObject** öğesini veya bir hata varsa **null** değerini döndürür.

**LandmarkImage** yöntemini kopyalayıp **landmarkImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to identify a landmark in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject LandmarkImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify a Landmark in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseLandmark;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. **Yer İşareti** sekmesine tıklayın, bir yer işareti görüntüsünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="recognize-celebrities"></a>Ünlüleri tanıma

Görüntü İşleme’nin Ünlüler özelliği bir görüntüyü ünlü kişiler açısından analiz eder. Analiz tamamlandıktan sonra Ünlüler, görüntüde bulunan Ünlüleri belirleyen bir JSON nesnesini döndürür.

Öğretici uygulamasının Ünlüler özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**celebritiesImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **CelebritiesImage** yöntemini çağırır. **CelebritiesImage** döndürüldüğünde yöntem, **Yanıt** metin alanında biçimlendirilmiş JSON yanıtını görüntüler, daha sonra **JSONObject** öğesinden ilk ünlü adını ayıklar ve adı ünlünün doğru şekilde belirlendiği güvenilirlik düzeyiyle birlikte pencerede görüntüler.

Aşağıdaki kodu kopyalayıp **celebritiesImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void celebritiesImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL celebritiesImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        celebritiesImage.setIcon(new ImageIcon());
        celebritiesCaptionLabel.setText("");
        celebritiesResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            celebritiesImageUrl = new URL(celebritiesImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(celebritiesImageUrl);
            scaleAndShowImage(bImage, celebritiesImage);
        } catch(IOException e) {
            celebritiesResponseTextArea.setText("Error loading Celebrity image: " + e.getMessage());
            return;
        }
        
        // Identify the celebrities in the image.
        JSONObject jsonObj = CelebritiesImage(celebritiesImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        celebritiesResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("result") && jsonObj.getJSONObject("result").has("celebrities")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("result").getJSONArray("celebrities").getJSONObject(0);
            
            if (jsonCaption.has("name") && jsonCaption.has("confidence")) {

                celebritiesCaptionLabel.setText("Caption: " + jsonCaption.getString("name") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**CelebritiesImage** yöntemi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Yöntem, görüntüde bulunan ünlüleri açıklayan bir **JSONObject** öğesini veya bir hata varsa **null** değerini döndürür.

**CelebritiesImage** yöntemini kopyalayıp **celebritiesImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to identify celebrities in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject CelebritiesImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify celebrities in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseCelebrities;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. **Ünlüler** sekmesine tıklayın, bir ünlü görüntüsünün URL’sini girin, ardından **Görüntüyü Analiz Et** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="intelligently-generate-a-thumbnail"></a>Akıllıca küçük resim oluşturma

Görüntü İşleme’nin Küçük Resim özelliği, bir görüntüden küçük resim oluşturur. **Akıllı Kırpma** özelliğini kullanarak Küçük Resim özelliği, estetik olarak daha çekici küçük resim görüntüleri oluşturmak için bir görüntüdeki ilgi alanını belirler ve küçük resmi bu alanda ortalar.

Öğretici uygulamasının Küçük Resim özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**thumbnailImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından küçük resmi oluşturmak için **getThumbnailImage** yöntemini çağırır. **getThumbnailImage** döndürüldüğünde yöntem, oluşturulan küçük resmi görüntüler.

Aşağıdaki kodu kopyalayıp **thumbnailImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void thumbnailImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL thumbnailImageUrl;
        JSONObject jsonError[] = new JSONObject[1];
        
        // Clear out the previous image, response, and thumbnail, if any.
        thumbnailSourceImage.setIcon(new ImageIcon());
        thumbnailResponseTextArea.setText("");
        thumbnailImage.setIcon(new ImageIcon());

        // Display the image specified in the text box.
        try {
            thumbnailImageUrl = new URL(thumbnailImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(thumbnailImageUrl);
            scaleAndShowImage(bImage, thumbnailSourceImage);
        } catch(IOException e) {
            thumbnailResponseTextArea.setText("Error loading image to thumbnail: " + e.getMessage());
            return;
        }
        
        // Get the thumbnail for the image.
        BufferedImage thumbnail = getThumbnailImage(thumbnailImageUrl.toString(), jsonError);
        
        // A non-null value indicates error.
        if (jsonError[0] != null) {
            // Format and display the JSON error.
            thumbnailResponseTextArea.setText(jsonError[0].toString(2));
            return;
        }
        
        // Display the thumbnail.
        if (thumbnail != null) {
            scaleAndShowImage(thumbnail, thumbnailImage);
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**getThumbnailImage** yöntemi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Yöntem, küçük resmi içeren bir **BufferedImage** öğesini veya bir hata varsa **null** değerini döndürür. **jsonError** dize dizisinin ilk öğesinde hata iletisi döndürülür.

Aşağıdaki **getThumbnailImage** yöntemini kopyalayıp **thumbnailImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to create a thumbnail for an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A BufferedImage containing the thumbnail, or null if a runtime error occurs. In the case
     * of an error, the error message will be returned in the first element of the jsonError string array.
     */
    private BufferedImage getThumbnailImage(String imageUrl, JSONObject[] jsonError) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify celebrities in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseThumbnail;
            URIBuilder uriBuilder = new URIBuilder(uriString);

            // Request parameters.
            uriBuilder.setParameter("width", "100");
            uriBuilder.setParameter("height", "150");
            uriBuilder.setParameter("smartCropping", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity requestEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(requestEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // Check for success.
            if (response.getStatusLine().getStatusCode() == 200)
            {
                // Return the thumbnail.
                return ImageIO.read(entity.getContent());
            }
            else
            {
                // Format and display the JSON error message.
                String jsonString = EntityUtils.toString(entity);
                jsonError[0] = new JSONObject(jsonString);
                return null;
            }
        }
        catch (Exception e)
        {
            String errorMessage = e.getMessage();
            System.out.println(errorMessage);
            jsonError[0] = new JSONObject(errorMessage);
            return null;
        }
    }
```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. **Küçük Resim** sekmesine tıklayın, bir görüntünün URL’sini girin, ardından **Küçük Resim Oluştur** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="read-printed-text-ocr"></a>Yazdırılan metni (OCR) okuma

Görüntü İşleme’nin Optik Karakter Tanıma (OCR) özelliği, yazdırılan metnin görüntüsünü analiz eder. Analiz tamamlandıktan sonra OCR, görüntüdeki metnin konumunu ve metni içeren bir JSON nesnesi döndürür.

Öğretici uygulamasının OCR özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**ocrImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **OcrImage** yöntemini çağırır. **OcrImage** döndürüldüğünde yöntem, algılanan metni **Yanıt** metin alanında biçimlendirilmiş JSON olarak görüntüler.

Aşağıdaki kodu kopyalayıp **ocrImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void ocrImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL ocrImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        ocrImage.setIcon(new ImageIcon());
        ocrResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            ocrImageUrl = new URL(ocrImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(ocrImageUrl);
            scaleAndShowImage(bImage, ocrImage);
        } catch(IOException e) {
            ocrResponseTextArea.setText("Error loading OCR image: " + e.getMessage());
            return;
        }
        
        // Read the text in the image.
        JSONObject jsonObj = OcrImage(ocrImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        ocrResponseTextArea.setText(jsonObj.toString(2));
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**OcrImage** yöntemi, bir görüntüyü analiz etmek için REST API çağrısını sarmalar. Yöntem, çağrıdan döndürülen JSON verilerinin **JSONObject** öğesini veya bir hata varsa **null** değerini döndürür.

Aşağıdaki **OcrImage** yöntemini kopyalayıp **ocrImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to read text in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject OcrImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to read text in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseOcr;
            URIBuilder uriBuilder = new URIBuilder(uriString);

            // Request parameters.
            uriBuilder.setParameter("language", "unk");
            uriBuilder.setParameter("detectOrientation ", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. **OCR** sekmesine tıklayın, yazdırılan metin görüntüsünün URL’sini girin, ardından **Görüntüyü Oku** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

### <a name="read-handwritten-text-handwriting-recognition"></a>El yazısı metni okuma (el yazısı tanıma)

Görüntü İşleme’nin El Yazısı Tanıma özelliği, el yazısı metnin görüntüsünü analiz eder. Analiz tamamlandıktan sonra El Yazısı Tanıma, görüntüdeki metnin konumunu ve metni içeren bir JSON nesnesi döndürür.

Öğretici uygulamasının El Yazısı Tanıma özelliğini tamamlamak için aşağıdaki adımları gerçekleştirin:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Form düğmesi için olay işleyicisi kodu ekleme

**handwritingImageButtonActionPerformed** olay işleyicisi yöntemi, formu temizler, URL’de belirtilen görüntüyü görüntüler, ardından görüntüyü analiz etmek için **HandwritingImage** yöntemini çağırır. **HandwritingImage** döndürüldüğünde yöntem, algılanan metni **Yanıt** metin alanında biçimlendirilmiş JSON olarak görüntüler.

Aşağıdaki kodu kopyalayıp **handwritingImageButtonActionPerformed** yöntemine yapıştırın.

> [!NOTE]
> NetBeans, yöntem tanımı satırına (```private void```) veya o yöntemin kapanış küme ayracına yapıştırma işlemi yapmanıza izin vermez. Kodu kopyalamak için, yöntem tanımı ile kapanış küme ayracı arasındaki satırları kopyalayıp yöntemin içeriklerinin üzerine yapıştırın.

```java
    private void handwritingImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL handwritingImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        handwritingImage.setIcon(new ImageIcon());
        handwritingResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            handwritingImageUrl = new URL(handwritingImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(handwritingImageUrl);
            scaleAndShowImage(bImage, handwritingImage);
        } catch(IOException e) {
            handwritingResponseTextArea.setText("Error loading Handwriting image: " + e.getMessage());
            return;
        }
        
        // Read the text in the image.
        JSONObject jsonObj = HandwritingImage(handwritingImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        handwritingResponseTextArea.setText(jsonObj.toString(2));
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>REST API çağrısı için sarmalayıcıyı ekleme

**HandwritingImage** yöntemi, bir görüntüyü analiz etmek için gereken iki REST API çağrısını sarmalar. El yazısı tanıma zaman alıcı bir işlem olduğundan iki adımlı bir işlem kullanılır. İlk çağrı, görüntüyü işlenmek üzere gönderir; ikinci çağrı, işleme tamamlandığında algılanan metni alır.

Metin alındıktan sonra **HandwritingImage** yöntemi, metni ve metnin konumlarını açıklayan bir **JSONObject** öğesini veya bir hata varsa **null** değerini döndürür.

Aşağıdaki **HandwritingImage** yöntemini kopyalayıp **handwritingImageButtonActionPerformed** yönteminin hemen altına yapıştırın.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to read handwritten text in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject HandwritingImage(String imageUrl) {
        try (CloseableHttpClient textClient = HttpClientBuilder.create().build();
             CloseableHttpClient resultClient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to read text in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseHandwriting;
            URIBuilder uriBuilder = new URIBuilder(uriString);
            
            // Request parameters.
            uriBuilder.setParameter("handwriting", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response.
            HttpResponse textResponse = textClient.execute(request);
            
            // Check for success.
            if (textResponse.getStatusLine().getStatusCode() != 202) {
                // An error occurred. Return the JSON error message.
                HttpEntity entity = textResponse.getEntity();
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            }
            
            String operationLocation = null;

            // The 'Operation-Location' in the response contains the URI to retrieve the recognized text.
            Header[] responseHeaders = textResponse.getAllHeaders();
            for(Header header : responseHeaders) {
                if(header.getName().equals("Operation-Location"))
                {
                    // This string is the URI where you can get the text recognition operation result.
                    operationLocation = header.getValue();
                    break;
                }
            }
            
            // NOTE: The response may not be immediately available. Handwriting recognition is an
            // async operation that can take a variable amount of time depending on the length
            // of the text you want to recognize. You may need to wait or retry this operation.
            //
            // This example checks once per second for ten seconds.
            
            JSONObject responseObj = null;
            int i = 0;
            do {
                // Wait one second.
                Thread.sleep(1000);
                
                // Check to see if the operation completed.
                HttpGet resultRequest = new HttpGet(operationLocation);
                resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());
                HttpResponse resultResponse = resultClient.execute(resultRequest);
                HttpEntity responseEntity = resultResponse.getEntity();
                if (responseEntity != null)
                {
                    // Get the JSON response.
                    String jsonString = EntityUtils.toString(responseEntity);
                    responseObj = new JSONObject(jsonString);
                }
            }
            while (i < 10 && responseObj != null &&
                    !responseObj.getString("status").equalsIgnoreCase("Succeeded"));
            
            // If the operation completed, return the JSON object.
            if (responseObj != null) {
                return responseObj;
            } else {
                // Return null for timeout error.
                System.out.println("Timeout error.");
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için **F6** tuşuna basın. Abonelik anahtarınızı **Abonelik Anahtarı** alanına girin ve **Abonelik Bölgesi**’nde doğru bölgeyi kullandığınızı doğrulayın. **El Yazısı Metni Oku** sekmesine tıklayın, el yazısı metin görüntüsünün URL’sini girin, ardından **Görüntüyü Oku** düğmesine tıklayarak bir görüntüyü analiz edip sonucu görün.

## <a name="next-steps"></a>Sonraki adımlar

- [Görüntü İşleme API’si C# Öğreticisi](CSharpTutorial.md)
- [Görüntü İşleme API'si Python Öğreticisi](PythonTutorial.md)
