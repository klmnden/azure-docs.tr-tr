---
title: 'Hızlı Başlangıç: C# kullanarak model değiştirme ve LUIS uygulamasını eğitme - Azure Bilişsel Hizmetler | Microsoft Docs'
description: Bu C# hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: 0c631fe281587c86f26643367aead14683b699df
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160924"
---
# <a name="quickstart-change-model-using-c"></a>Hızlı Başlangıç: C# kullanarak model değiştirme

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* C# programlama dilini yükleyin.
* [JsonFormatterPlus](https://www.nuget.org/packages/JsonFormatterPlus) ve [CommandLine](https://www.nuget.org/packages/CommandLineParser/) NuGet paketleri

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma 

Visual Studio'da .Net Framework kullanarak yeni bir **Windows Klasik Masaüstü Konsolu** uygulaması oluşturun. 

![Visual Studio proje türü](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

### <a name="add-the-systemweb-dependency"></a>System.Web bağımlılığını ekleyin

Visual Studio projesinde **System.Web** gerekir. Çözüm Gezgini’nde, **Başvurular**’a sağ tıklayın ve **Başvuru Ekle**’yi seçin.

![System.web başvurusunu ekleyin](./media/luis-quickstart-cs-add-utterance/system.web.png)

### <a name="add-other-dependencies"></a>Diğer bağımlılıkları ekleyin

Visual Studio projesi için **JsonFormatterPlus** ve **CommandLineParser** gerekir. Çözüm Gezgini'nde **Başvurular**'a sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesini seçin. İki paketi de arayın ve ekleyin. 

![3. taraf bağımlılıkları ekleme](./media/luis-quickstart-cs-add-utterance/add-dependencies.png)


### <a name="write-the-c-code"></a>C# kodunu yazma
**Program.cs** dosyasının şu şekilde olması gerekir:

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp3
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Bağımlılıkları ekleyin.

   [!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=1-11 "Add the dependencies")]


LUIS kimliklerini ve dizelerini **Program** sınıfına ekleyin.

   [!code-csharp[Add the LUIS IDs and strings](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=19-30&dedent=8 "Add the LUIS IDs and strings")]

**Program** sınıfına iletilen komut satırı parametrelerini yönetmek için sınıfı ekleyin.

   [!code-csharp[Add class to manage command line parameters.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=32-46 "Add class to manage command-line parameters.")]

**Program** sınıfına GET isteği metodunu ekleyin.

   [!code-csharp[Add the GET request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=49-59 "Add the GET request.")]


**Program** sınıfına POST isteği metodunu ekleyin. 

   [!code-csharp[Add the POST request.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=60-76 "Add the POST request.")]

**Program** sınıfına dosyadan örnek konuşmaları ekleyin.

   [!code-csharp[Add example utterances from file.
](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=77-86 "Add example utterances from file.
")]

Değişiklikler modele uygulandıktan sonra modeli eğitin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[After the changes are applied to the model, train the model.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=87-96 "After the changes are applied to the model, train the model.")]

Eğitim hemen tamamlanmayabilir, ilerleme durumunu kontrol edin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[Training may not complete immediately, check status to verify training is complete.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=97-103 "Training may not complete immediately, check status to verify training is complete.")]

Komut satırı bağımsız değişkenlerini yönetmek için ana kodu ekleyin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[To manage command line arguments, add the main code.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=104-137 "To manage command-line arguments, add the main code.")]

### <a name="copy-utterancesjson-to-output-directory"></a>utterances.json dosyasını çıkış dizinine kopyalayın

Çözüm Gezgini'nde `utterances.json` öğesine sağ tıklayıp **Özellikler**'i seçin. Özellikler penceresinde `Content` öğesinin **Derleme Eylemi** ve `Copy Always` öğesinin **Çıkış Dizinine Kopyala** seçeneğini işaretleyin.  

![JSON dosyasını içerik olarak işaretleme](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="build-code"></a>Kod derleme

Kodu Visual Studio’da derleyin. 

## <a name="run-code"></a>Kodu çalıştırma

Projenin /bin/Debug dizininde uygulamayı komut satırından çalıştırın. 

```CMD
ConsoleApp\bin\Debug> ConsoleApp1.exe --add utterances.json --train --status
```

Bu komut satırı add utterances API'sini çağırmanın sonuçlarını gösterir. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md) 