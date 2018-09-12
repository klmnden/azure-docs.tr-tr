---
title: 'Hızlı Başlangıç: Node.js kullanarak model değiştirme ve LUIS uygulamasını eğitme - Azure Bilişsel Hizmetler | Microsoft Docs'
description: Bu Node.js hızlı başlangıcında, bir Ev Otomasyonu uygulamasına örnek konuşmalar ekleyip uygulamayı eğitin. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: c6ad30340e0800e475af9a9fefa0af7df1769f6c
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43772564"
---
# <a name="quickstart-change-model-using-nodejs"></a>Hızlı Başlangıç: Node.js kullanarak model değiştirme

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Ön koşullar

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* En son [**Node.js**](https://nodejs.org/en/download/) ve NPM.
* Bu makale için NPM bağımlılıkları: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* [Visual Studio Code](https://code.visualstudio.com/).

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Hızlı başlangıç kodu oluşturma 

NPM bağımlılıklarını `add-utterances.js` adlı dosyaya ekleyin.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=8-11 "NPM Dependencies")]

LUIS sabitlerini dosyaya ekleyin. Aşağıdaki kodu kopyalayıp yazma anahtarı, uygulama kimliği ve sürüm kimliğini kendi değerlerinizle değiştirin.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=13-22 "LUIS key and IDs")]

Konuşmalarınızın bulunduğu yüklenecek dosyanın adını ve konumunu ekleyin. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=24-26 "Add upload file")]

`addUtterance` işlevi için yöntem ve nesne ekleyin.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=28-67 "Add configuration information for adding utterance")]

`train` işlevi için yöntem ve nesne ekleyin.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=69-101 "Add configuration information for training LUIS")]

HTTP çağrısı göndermek ve almak için `sendUtteranceToApi` işlevini ekleyin. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=103-119 "Send the HTTP request")]

Hangi eylemi seçeceğini belirleyen **ana** kodu ekleyin.

   [!code-javascript[Main function](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=121-143 "Main function")]

### <a name="install-dependencies"></a>Bağımlılıkları yükleme

Aşağıdaki metinle `package.json` dosyası oluşturun:

   [!code-json[Package.json](~/samples-luis/documentation-samples/quickstarts/change-model/node/package.json "Package.json")]

Komut satırında, package.json içeren dizinden NPM ile bağımlılıkları yükleyin: `npm install`.

## <a name="run-code"></a>Kodu çalıştırma

Uygulamayı Node.js ile komut satırından çalıştırın.

`npm start` komutunu çağırmak konuşmaları ekler, eğitir ve eğitim durumunu alır.

```CMD
> npm start 
```

Bu komut satırı add utterances API'sini çağırmanın sonuçlarını gösterir. 

[!include[Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıçla işiniz bittiğinde, bu hızlı başlangıçta oluşturulan tüm dosyaları kaldırın. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Programlama yoluyla bir LUIS uygulaması oluşturma](luis-tutorial-node-import-utterances-csv.md)