---
title: 'Hızlı Başlangıç: Küçük resim - REST, Ruby oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Ruby ile Görüntü İşleme API'sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: fbac507ce904227aebd235323da32f3a4d216075
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59994444"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-ruby-in-computer-vision"></a>Hızlı Başlangıç: Görüntü işleme Ruby ve REST API kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API’sini kullanarak bir görüntüden küçük resim oluşturacaksınız. [Küçük Resim Alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yöntemi ile bir görüntünün küçük resmini alabilirsiniz. Giriş görüntüsünün en boy oranından farklı olabilen bir yükseklik ve genişlik belirtirsiniz. Görüntü işleme, akıllı bir şekilde ilgi belirlemek ve söz konusu bölgeyi temel alan kırpma koordinatları oluşturmak için akıllı kırpma kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- [Ruby](https://www.ruby-lang.org/en/downloads/) 2.4.x veya sonraki bir sürüm yüklenmiş olmalıdır.
- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir metin düzenleyicisine kopyalayın.
1. Gerektiğinde kodda aşağıdaki değişiklikleri yapın:
    1. `<Subscription Key>` değerini abonelik anahtarınızla değiştirin.
    1. Gerekirse `https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze` değerini, abonelik anahtarlarınızı aldığınız Azure bölgesindeki [Küçük Resim Al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yönteminin uç nokta URL’si ile değiştirin.
    1. İsteğe bağlı olarak `https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\` değerini, küçük resmini oluşturmak istediğiniz başka bir görüntünün URL’si ile değiştirin.
1. Kodu, `.rb` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-thumbnail.rb`.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `ruby` komutunu kullanın. Örneğin, `ruby get-thumbnail.rb`.

```ruby
require 'net/http'

# You must use the same location in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from westus,
# replace "westcentralus" in the URL below with "westus".
uri = URI('https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail')
uri.query = URI.encode_www_form({
    # Request parameters
    'width' => '100',
    'height' => '100',
    'smartCropping' => 'true'
})

request = Net::HTTP::Post.new(uri.request_uri)

# Request headers
# Replace <Subscription Key> with your valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '<Subscription Key>'
request['Content-Type'] = 'application/json'

request.body =
    "{\"url\": \"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/" +
    "Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

#puts response.body
```

## <a name="examine-the-response"></a>Yanıtı inceleme

Küçük resmin görüntü verilerini temsil eden ikili veri halinde başarılı bir yanıt döndürülür. İstek başarısız olursa, yanıt konsol penceresinde görüntülenir. Başarısız isteğin yanıtı, neyin yanlış gittiğini belirlemeye yardımcı olması için bir hata kodu ve bir ileti içerir.

## <a name="next-steps"></a>Sonraki adımlar

Görüntü analiz etmek, ünlüleri ve yer işaretlerini algılamak, küçük resim oluşturmak ve yazdırılan ve el yazısı metinleri ayıklamak için kullanılan Görüntü İşleme API’sini keşfedin. Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console) konusuna bakın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API’sini keşfetme](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
