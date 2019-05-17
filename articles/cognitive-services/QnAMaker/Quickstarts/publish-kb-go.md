---
title: Bilgi Bankası, REST, Git ile yayımlama
titleSuffix: QnA Maker - Azure Cognitive Services
description: Bu Git (REST) tabanlı hızlı test edilmiş Bilgi Bankası en son sürümünü temsil eden yayımlanan Bilgi Bankası adanmış bir Azure Search dizinine iter, Bilgi Bankası yayımlama aracılığıyla size yol gösterir. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 5c0d3c5d33d41ddd01b9d0c0ccf4f468d52f6a9e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790794"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-go"></a>Hızlı Başlangıç: Soru-cevap Oluşturucu Go kullanarak Bilgi Bankası yayımlama

REST tabanlı bu hızlı başlangıçta, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi bankanızın son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda ya da sohbet botunuzda çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* [Go 1.10.1](https://golang.org/dl/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 

* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Bilgi Bankası henüz sahip değilseniz, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni Bilgi Bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE] 
> Eksiksiz bir çözüm dosyaları kullanılabilir [ **Azure-Samples/bilişsel-services-qnamaker-go** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-a-go-file"></a>Bir Git dosyası oluşturun

Adlı yeni bir dosya oluşturun ve açın VSCode `publish-kb.go`.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `publish-kb.go` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=3-7 "Add the required dependencies")]

## <a name="create-the-main-function"></a>Main işlevi oluşturma

Gereken bağımlılıklardan sonra aşağıdaki sınıfı ekleyin:

```Go
package main

func main() {

}
```

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

İçinde **ana**


 işlev, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=16-20 "Add the required constants")]

## <a name="add-post-request-to-publish-kb"></a>KB yayımlamak için POST isteğini ekleme

Sonra gerekli sabitleri, Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si bir HTTPS isteği yapar ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-go[Add a POST request to publish KB](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to publish KB")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Dosyayı derlemek için aşağıdaki komutu girin. Komut istemi başarılı bir derleme için herhangi bir bilgi döndürmez.

```bash
go build publish-kb.go
```

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. KB yayımlayın ve ardından başarı veya hata 204 yazdırmak için soru-cevap Oluşturucu API'si isteği gönderir.

```bash
./publish-kb
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra ihtiyacınız [yanıt oluşturmak için uç nokta URL'si](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
