---
title: Değiştirmek için uygulama eğitmek,C#
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu C# hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 04/08/2019
ms.author: diberry
ms.openlocfilehash: e9f8d274d81cdefbf9dfb41708cd537b2d60471a
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59273473"
---
# <a name="quickstart-change-model-using-c"></a>Hızlı Başlangıç: Modeli kullanarak değiştirinC#

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-change-model-intro-para.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* C# programlama dilini yükleyin.
* [JsonFormatterPlus](https://www.nuget.org/packages/JsonFormatterPlus) ve [CommandLine](https://www.nuget.org/packages/CommandLineParser/) NuGet paketleri

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma 

Visual Studio'da yeni bir oluşturma **Windows Klasik Masaüstü Konsolu** .NET Framework kullanarak uygulama. Projeyi adlandırın `ConsoleApp1`.

![Visual Studio proje türü](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

### <a name="add-the-systemweb-dependency"></a>System.Web bağımlılığını ekleyin

Visual Studio projesinde **System.Web** gerekir. Çözüm Gezgini'nde sağ **başvuruları** seçip **Başvuru Ekle** derlemeleri bölümünden.

![System.web başvurusunu ekleyin](./media/luis-quickstart-cs-add-utterance/system.web.png)

### <a name="add-other-dependencies"></a>Diğer bağımlılıkları ekleyin

Visual Studio projesi için **JsonFormatterPlus** ve **CommandLineParser** gerekir. Çözüm Gezgini'nde **Başvurular**'a sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesini seçin. Göz atın ve her iki paketlerin bir bölümünü ekleyin. 

![3. taraf bağımlılıkları ekleme](./media/luis-quickstart-cs-add-utterance/add-dependencies.png)


### <a name="write-the-c-code"></a>C# kodunu yazma
**Program.cs** dosyasının şu şekilde olması gerekir:

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Bağımlılıkları olan güncelleştirin:

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

   [!code-csharp[Add example utterances from file.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=77-86 "Add example utterances from file.")]

Değişiklikler modele uygulandıktan sonra modeli eğitin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[After the changes are applied to the model, train the model.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=87-96 "After the changes are applied to the model, train the model.")]

Eğitim hemen tamamlanmayabilir, ilerleme durumunu kontrol edin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[Training may not complete immediately, check status to verify training is complete.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=97-103 "Training may not complete immediately, check status to verify training is complete.")]

Komut satırı bağımsız değişkenlerini yönetmek için ana kodu ekleyin. **Program** sınıfına yöntemi ekleyin.

   [!code-csharp[To manage command line arguments, add the main code.](~/samples-luis/documentation-samples/quickstarts/change-model/csharp/ConsoleApp1/Program.cs?range=104-137 "To manage command-line arguments, add the main code.")]

### <a name="copy-utterancesjson-to-output-directory"></a>utterances.json dosyasını çıkış dizinine kopyalayın

Çözüm Gezgini'nde ekleme `utterances.json` Solution Explorer'ın Proje adına sağ tıklayarak seçip **Ekle**, ardından seçerek **varolan öğe**. Seçin `utterances.json` dosya. Bu dosyayı projeye ekler. Ardından çıktı yönü eklenmesi gerekir. Sağ `utterances.json` seçip **özellikleri**. Özellikler penceresinde `Content` öğesinin **Derleme Eylemi** ve `Copy Always` öğesinin **Çıkış Dizinine Kopyala** seçeneğini işaretleyin.  

![JSON dosyasını içerik olarak işaretleme](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="build-code"></a>Kod derleme

Kodu Visual Studio’da derleyin. 

## <a name="run-code"></a>Kodu çalıştırma

Projenin /bin/Debug dizininde uygulamayı komut satırından çalıştırın. 

```console
ConsoleApp1.exe --add utterances.json --train --status
```

Bu komut satırı add utterances API'sini çağırmanın sonuçlarını gösterir. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md) 
