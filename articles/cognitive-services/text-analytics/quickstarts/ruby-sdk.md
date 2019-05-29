---
title: "Hızlı Başlangıç: Metin analizi Bilişsel Ruby SDK'sını kullanarak hizmet çağrısı"
titleSuffix: Azure Cognitive Services
description: Hızlı bir şekilde yardımcı olması için alma bilgileri ve kod örnekleri, Azure Bilişsel hizmetler metin analizi API'sini kullanarak başlayın.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/08/2019
ms.author: tasharm
ms.openlocfilehash: 688887826fa803b616ca737bc8558aa17ed80e37
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297767"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-ruby-sdk"></a>Hızlı Başlangıç: Ruby SDK'sını kullanarak metin analizi hizmeti çağırma

<a name="HOLTop"></a>


Bu hızlı başlangıçta, Ruby için metin analizi SDK'sı ile dil incelemeye başlamak için kullanın. Sırada [metin analizi](//go.microsoft.com/fwlink/?LinkID=759711) çoğu programlama dilleri ile uyumlu REST API, SDK hizmeti uygulamalarınızla tümleştirmek için kolay bir yol sağlar. Bu örnek için kaynak kodu bulunabilir [GitHub](https://github.com/Azure-Samples/cognitive-services-ruby-sdk-samples/blob/master/samples/text_analytics.rb).

API'lerle ilgili teknik bilgiler için [API tanımları](//go.microsoft.com/fwlink/?LinkID=759346) sayfasını inceleyin.

## <a name="prerequisites"></a>Önkoşullar

* [Ruby 2.5.5 veya üzeri](https://www.ruby-lang.org/)
* Metin analizi [Ruby SDK](https://rubygems.org/gems/azure_cognitiveservices_textanalytics)
 
[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

Ayrıca kayıt sırasında oluşturulan [uç nokta ve erişim anahtarı](../How-tos/text-analytics-how-to-access-key.md) değerlerine de sahip olmanız gerekir. 

<a name="RubyProject"></a>

## <a name="create-a-ruby-project-and-install-the-sdk"></a>Ruby proje oluşturma ve SDK'sını yükleyin

1. Yeni bir ruby projesi oluşturun ve adlı yeni bir dosya ekleyin `Gemfile`.
2. Metin analizi SDK ekleyerek projeye eklemek için kod aşağıda `Gemfile`.

    ```ruby
    source 'https://rubygems.org'
    gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
    ```

## <a name="create-a-text-analytics-client"></a>Bir metin analytics istemcisi oluşturma

1. Adlı yeni bir dosya oluşturun `TextAnalyticsExamples.rb` tercih ettiğiniz düzenleyiciyi veya IDE. Metin analizi SDK'sını alın.

2. Bir kimlik bilgileri nesnesi, metin analizi istemci tarafından kullanılır. İle oluşturma `CognitiveServicesCredentials.new()` ve abonelik anahtarınızı geçirme.

3. İstemci, doğru metin analizi uç noktanız ile oluşturun.

    ```ruby
    require 'azure_cognitiveservices_textanalytics'
    
    include Azure::CognitiveServices::TextAnalytics::V2_1::Models
    
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new("enter key here")
    # Replace 'westus' with the correct region for your Text Analytics subscription
    endpoint = String.new("https://westus.api.cognitive.microsoft.com/")
    
    textAnalyticsClient =
        Azure::TextAnalytics::Profiles::Latest::Client.new({
            credentials: credentials
        })
    textAnalyticsClient.endpoint = endpoint
    ```

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Metin analizi SDK'sı veya API'si kullanarak, bir metin kayıt kümesi üzerinde yaklaşım analizi gerçekleştirebilirsiniz. Aşağıdaki örnek, çeşitli belgelerin duyarlılığı puanlarını görüntüler.

1. Adlı yeni bir işlev oluşturma `SentimentAnalysisExample()` yukarıda bir parametre olarak oluşturulan metin analizi istemci alır.

2. Bir dizi `MultiLanguageInput` Analiz edilecek nesneleri. Bir dil ve her nesne için metin ekleyin. Kimliği, herhangi bir değer olabilir.

    ```ruby
    def SentimentAnalysisExample(client)
      # The documents to be analyzed. Add the language of the document. The ID can be any value.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'en'
      input_1.text = 'I had the best day of my life.'
    
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'en'
      input_2.text = 'This was a waste of my time. The speaker put me to sleep.'
    
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'es'
      input_3.text = 'No tengo dinero ni nada que dar...'
    
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'it'
      input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
    ```

3. Aynı işlevin içinde belgeleri bir liste olarak birleştirir. Ekleyin `documents` alanını bir `MultiLanguageBatchInput` nesne. 

4. İstemcinin çağrı `sentiment()` işleviyle `MultiLanguageBatchInput` belgeleri göndermek için bir parametre olarak nesne. Hiçbir sonuç döndürmezse, adımları yazdırın.
    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.sentiment(
          multi_language_batch_input: input_documents
      )
      
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        puts '===== SENTIMENT ANALYSIS ====='
        result.documents.each do |document|
          puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
        end
      end
    end
    ```

5. Çağrı `SentimentAnalysisExample()` işlevi.

    ```ruby
    SentimentAnalysisExample(textAnalyticsClient)
    ```

### <a name="output"></a>Çıktı

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Dil algılama

Metin analizi hizmeti, çok sayıda dil ve yerel ayarlar arasında bir metin belgesi dili algılayabilir. Aşağıdaki örnek, birkaç belge yazılmış dili görüntüler.

1. Adlı yeni bir işlev oluşturma `DetectLanguageExample()` yukarıda bir parametre olarak oluşturulan metin analizi istemci alır. 

2. Bir dizi `LanguageInput` Analiz edilecek nesneleri. Bir dil ve her nesne için metin ekleyin. Kimliği, herhangi bir değer olabilir.

    ```ruby
    def DetectLanguageExample(client)
       # The documents to be analyzed.
       language_input_1 = LanguageInput.new
       language_input_1.id = '1'
       language_input_1.text = 'This is a document written in English.'
    
       language_input_2 = LanguageInput.new
       language_input_2.id = '2'
       language_input_2.text = 'Este es un document escrito en Español..'
    
       language_input_3 = LanguageInput.new
       language_input_3.id = '3'
       language_input_3.text = '这是一个用中文写的文件'
    ```

3. Aynı işlevin içinde belgeleri bir liste olarak birleştirir. Ekleyin `documents` alanını bir `LanguageBatchInput` nesne. 

4. İstemcinin çağrı `detect_language()` işleviyle `LanguageBatchInput` belgeleri göndermek için bir parametre olarak nesne. Hiçbir sonuç döndürmezse, adımları yazdırın.
    ```ruby
       input_documents = LanguageBatchInput.new
       input_documents.documents = [language_input_1, language_input_2, language_input_3]
    
    
       result = client.detect_language(
           language_batch_input: input_documents
       )
    
       if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
         puts '===== LANGUAGE DETECTION ====='
         result.documents.each do |document|
           puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
         end
       else
         puts 'No results data..'
       end
     end
    ```

5. İşlev çağrısı `DetectLanguageExample`

    ```ruby
    DetectLanguageExample(textAnalyticsClient)
    ```

### <a name="output"></a>Çıktı

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Varlık tanıma

Metin analizi hizmeti, ayırmak ve farklı varlıklarda (kişiler, yerler ve öğeleri) metin belgeleri ayıklayın. Aşağıdaki örnek, birkaç örnek belgelerde bulunan varlıkları görüntüler.

1. Adlı yeni bir işlev oluşturma `Recognize_Entities()` yukarıda bir parametre olarak oluşturulan metin analizi istemci alır.

2. Bir dizi `MultiLanguageInput` Analiz edilecek nesneleri. Bir dil ve her nesne için metin ekleyin. Kimliği, herhangi bir değer olabilir.

    ```ruby
      def RecognizeEntitiesExample(client)
        # The documents to be analyzed.
        input_1 = MultiLanguageInput.new
        input_1.id = '1'
        input_1.language = 'en'
        input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'
    
        input_2 = MultiLanguageInput.new
        input_2.id = '2'
        input_2.language = 'es'
        input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'
    ```

3. Aynı işlevin içinde belgeleri bir liste olarak birleştirir. Ekleyin `documents` alanını bir `MultiLanguageBatchInput` nesne. 

4. İstemcinin çağrı `entities()` işleviyle `MultiLanguageBatchInput` belgeleri göndermek için bir parametre olarak nesne. Hiçbir sonuç döndürmezse, adımları yazdırın.

    ```ruby
        input_documents =  MultiLanguageBatchInput.new
        input_documents.documents = [input_1, input_2]
    
        result = client.entities(
            multi_language_batch_input: input_documents
        )
    
        if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
          puts '===== ENTITY RECOGNITION ====='
          result.documents.each do |document|
            puts "Document ID: #{document.id}"
              document.entities.each do |entity|
                puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
                entity.matches.each do |match|
                  puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
                end
                puts
              end
          end
        else
          puts 'No results data..'
        end
      end
    ```

5. İşlev çağrısı `RecognizeEntitiesExample`
    ```ruby
    RecognizeEntitiesExample(textAnalyticsClient)
    ```

### <a name="output"></a>Çıktı

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Anahtar tümcecik ayıklama

Metin analizi hizmeti, anahtar tümcecikleri cümle içinde ayıklayabilir. Aşağıdaki örnek, birden çok dilde birkaç örnek belge bulunan varlıkları görüntüler.

1. Adlı yeni bir işlev oluşturma `KeyPhraseExtractionExample()` yukarıda bir parametre olarak oluşturulan metin analizi istemci alır.

2. Bir dizi `MultiLanguageInput` Analiz edilecek nesneleri. Bir dil ve her nesne için metin ekleyin. Kimliği, herhangi bir değer olabilir.

    ```ruby
    def KeyPhraseExtractionExample(client)
      # The documents to be analyzed.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'ja'
      input_1.text = '猫は幸せ'
  
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'de'
      input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'
  
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'en'
      input_3.text = 'My cat is stiff as a rock.'
  
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'es'
      input_4.text = 'A mi me encanta el fútbol!'
      ```

3. Aynı işlevin içinde belgeleri bir liste olarak birleştirir. Ekleyin `documents` alanını bir `MultiLanguageBatchInput` nesne. 

4. İstemcinin çağrı `key_phrases()` işleviyle `MultiLanguageBatchInput` belgeleri göndermek için bir parametre olarak nesne. Hiçbir sonuç döndürmezse, adımları yazdırın.

    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.key_phrases(
          multi_language_batch_input: input_documents
      )
    
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        result.documents.each do |document|
          puts "Document Id: #{document.id}"
          puts '  Key Phrases'
          document.key_phrases.each do |key_phrase|
            puts "    #{key_phrase}"
          end
        end
      else
        puts 'No results data..'
      end
    end
    ```

5. İşlev çağrısı `KeyPhraseExtractionExample`

    ```ruby
    KeyPhraseExtractionExample(textAnalyticsClient)
    ```

### <a name="output"></a>Çıktı

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile Metin Analizi](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Ayrıca bkz.

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)
