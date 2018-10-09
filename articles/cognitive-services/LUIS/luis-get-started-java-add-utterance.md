---
title: Java Hızlı Başlangıç - modeli değiştirme ve LUIS uygulamasını eğitme
titleSuffix: Azure Cognitive Services
description: Bu Java hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 8500dfdd639b4f1de46a0b79b87a8564d338b6ab
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47042033"
---
# <a name="quickstart-change-model-using-java"></a>Hızlı Başlangıç: Java kullanarak model değiştirme 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Quickstart prerequisites for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html)  (Java Geliştirme Seti, Standart Sürüm)
* [Google GSON JSON kitaplığı](https://github.com/google/gson).

[!INCLUDE [Quickstart note about code repository](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma

1. Java bağımlılıklarını `AddUtterances.java` adlı dosyaya ekleyin.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=23-26 "Java Dependencies")]

2. `AddUtterances` sınıfını oluşturun. Bu sınıf sonraki tüm kod parçacıklarını içerir.

    ```Java
    public class AddUtterances {
        // Insert code here
    }
    ```

3. LUIS sabitlerini sınıfa ekleyin. Aşağıdaki kodu kopyalayıp yazma anahtarı, uygulama kimliği ve sürüm kimliğini kendi değerlerinizle değiştirin.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=33-44 "LUIS-based IDs")]

4. LUIS API’sine AddUtterances sınıfını çağırmak için yöntemi ekleyin. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=46-168 "HTTP request to LUIS")]

5. LUIS API'sinden HTTP yanıtı için kullanılacak metodu AddUtterances sınıfına ekleyin.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=170-202 "HTTP response from LUIS")]

6. AddUtterances sınıfına özel durum işleme ekleyin. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=205-243 "Exception Handling")]

7. AddUtterances sınıfına temel işlevi ekleyin.

   [!code-java[Add main function](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=245-278 "Add main function")]

## <a name="build-code"></a>Kod derleme

AddUtterance öğesini bağımlılıklarla derleme

```CMD
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

## <a name="run-code"></a>Kodu çalıştırma
Bağımsız değişken olmadan `AddUtterance` çağrısını yaptığınızda uygulamaya LUIS konuşmaları eklenir ancak eğitim gerçekleştirilmez.

```CMD
> java -classpath .;gson-2.8.2.jar AddUtterances
```

Bu komut satırı add utterances API'sini çağırmanın sonuçlarını gösterir. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md) 