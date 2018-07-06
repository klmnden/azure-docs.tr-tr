---
title: Hızlı Başlangıç'ı Microsoft soru-cevap Oluşturucu API'si (V4) - Azure Bilişsel hizmetler için Git | Microsoft Docs
description: Microsoft Translator metin çevirisi API'si, Azure üzerinde Microsoft Bilişsel hizmetler kullanarak hızlı bir şekilde yardımcı olmak için bilgi ve kod örnekleri get başlayın.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jaswel
ms.openlocfilehash: d68ff91d74a6c43c6d492aa07e916a842b50e96f
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867598"
---
# <a name="quickstart-for-microsoft-qna-maker-api-with-go"></a>Hızlı Başlangıç için Microsoft soru-cevap Oluşturucu API'si ile Git 
<a name="HOLTop"></a>

Bu makalede nasıl kullanılacağını gösterir [Microsoft soru-cevap Oluşturucu API'si](../Overview/overview.md) aşağıdakileri yapmak için Go ile.

- [Yeni Bilgi Bankası oluşturun.](#Create)
- [Var olan bir Bilgi Bankası güncelleştirin.](#Update)
- [Oluşturulacak veya güncelleştirilecek bir Bilgi Bankası isteğinin durumunu alın.](#Status)
- [Var olan bir Bilgi Bankası yayımlama.](#Publish)
- [Var olan bir Bilgi Bankası içerikleri değiştirin.](#Replace)
- [Bilgi Bankası içerikleri indirin.](#GetQnA)
- [Bilgi Bankası'nı kullanarak bir soru yanıtlarını alın.](#GetAnswers)
- [Bilgi Bankası hakkında bilgi alın.](#GetKB)
- [Belirtilen kullanıcıya ait tüm bilgi bankalarından hakkında bilgi alın.](#GetKBsByUser)
- [Bilgi Bankası silin.](#Delete)
- [Geçerli uç nokta anahtarlarını alın.](#GetKeys)
- [Geçerli uç nokta anahtarları yeniden oluştur.](#PutKeys)
- [Word değişiklikleri geçerli kümesini alın.](#GetAlterations)
- [Word değişiklikleri geçerli kümesini değiştirin.](#PutAlterations)

## <a name="prerequisites"></a>Önkoşullar

İhtiyacınız olacak [1.10.1 Git](https://golang.org/dl/) bu kodu çalıştırmak için.

Olmalıdır bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ile **Microsoft soru-cevap Oluşturucu API'si**. Ücretli aboneliğe anahtarından gerekir, [Azure panosuna](https://portal.azure.com/#create/Microsoft.CognitiveServices).

<a name="Create"></a>

## <a name="create-knowledge-base"></a>Bilgi Bankası oluşturma

Aşağıdaki kod temel kullanarak yeni bir Bilgi Bankası oluşturur [Oluştur](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "time"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/create"

type Response struct {
    Headers map[string][]string
    Body    string
}

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func post(uri string, content string) Response {
    req, _ := http.NewRequest("POST", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return Response {response.Header, string(body)}
}

func get(uri string) Response {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return Response {response.Header, string(body)}
}

var req string = `{
  "name": "QnA Maker FAQ",
  "qnaList": [
    {
      "id": 0,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    }
  ],
  "urls": [
    "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq"
  ],
  "files": []
}`;

func create_kb(uri string, req string) (string, string) {
    fmt.Println("Calling " + uri + ".")
    result := post(uri, req)
    return result.Headers["Location"][0], result.Body
}

func check_status(uri string) (string, string) {
    fmt.Println("Calling " + uri + ".")
    result := get(uri)
    if retry, success := result.Headers["Retry-After"]; success {
        return retry[0], result.Body
    } else {
// If the response headers did not include a Retry-After value, default to 30 seconds.
        return "30", result.Body
    }
}

func main() {
    var uri = host + service + method
    operation, body := create_kb(uri, req)
    fmt.Printf(body + "\n")
    var done bool = false
    for done == false {
        uri := host + service + operation
        wait, status := check_status(uri)
        fmt.Println(status)
        var status_obj map[string]interface{}
        json.Unmarshal([]byte(status), &status_obj)
        state := status_obj["operationState"]
// If the operation isn't finished, wait and query again.
        if state == "Running" || state == "NotStarted" {
            fmt.Printf ("Waiting " + wait + " seconds...")
            sec, _ := strconv.Atoi(wait)
            time.Sleep (time.Duration(sec) * time.Second)
        } else {
            done = true
        }
    }
}
```

**Bilgi Bankası yanıt oluşturma**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Running",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:30Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:52:30Z",
  "lastActionTimestamp": "2018-04-13T01:52:46Z",
  "resourceLocation": "/knowledgebases/b0288f33-27b9-4258-a304-8b9f63427dad",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "e88b5b23-e9ab-47fe-87dd-3affc2fb10f3"
}
```

[Başa dön](#HOLTop)

<a name="Update"></a>

## <a name="update-knowledge-base"></a>Bilgi Bankası güncelleştirme

Aşağıdaki kodu kullanarak temel bir mevcut bilgi güncelleştirmeleri [güncelleştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
    "time"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

type Response struct {
    Headers map[string][]string
    Body    string
}

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func patch(uri string, content string) Response {
    req, _ := http.NewRequest("PATCH", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return Response {response.Header, string(body)}
}

func get(uri string) Response {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return Response {response.Header, string(body)}
}

var req string = `{
  'add': {
    'qnaList': [
      {
        'id': 1,
        'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle',
        'source': 'Custom Editorial',
        'questions': [
          'How can I change the default message from QnA Maker?'
        ],
        'metadata': []
      }
    ],
    'urls': [
      'https://docs.microsoft.com/en-us/azure/cognitive-services/Emotion/FAQ'
    ]
  },
  'update' : {
    'name' : 'New KB Name'
  },
  'delete': {
    'ids': [
      0
    ]
  }
}`;

func update_kb (uri string, req string) (string, string) {
    fmt.Println("Calling " + uri + ".")
    result := patch(uri, req)
    return result.Headers["Location"][0], result.Body
}

func check_status(uri string) (string, string) {
    fmt.Println("Calling " + uri + ".")
    result := get(uri)
    if retry, success := result.Headers["Retry-After"]; success {
        return retry[0], result.Body
    } else {
// If the response headers did not include a Retry-After value, default to 30 seconds.
        return "30", result.Body
    }
}

func main() {
    var uri = host + service + method + kb
    operation, body := update_kb (uri, req)
    fmt.Printf(body + "\n")
    var done bool = false
    for done == false {
        uri := host + service + operation
        wait, status := check_status(uri)
        fmt.Println(status)
        var status_obj map[string]interface{}
        json.Unmarshal([]byte(status), &status_obj)
        state := status_obj["operationState"]
// If the operation isn't finished, wait and query again.
        if state == "Running" || state == "NotStarted" {
            fmt.Printf ("Waiting " + wait + " seconds...")
            sec, _ := strconv.Atoi(wait)
            time.Sleep (time.Duration(sec) * time.Second)
        } else {
            done = true
        }
    }
}
```

**Bilgi Bankası yanıtı güncelleştirme**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:48Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:50Z",
  "resourceLocation": "/knowledgebases/140a46f3-b248-4f1b-9349-614bfd6e5563",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
Press any key to continue.
```

[Başa dön](#HOLTop)

<a name="Status"></a>

## <a name="get-request-status"></a>İstek durumunu Al

Çağırabilirsiniz [işlemi](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails) oluşturmak ya da bir Bilgi Bankası güncelleştirme isteği durumunu denetlemek için yöntemi. Bu yöntem nasıl kullanıldığını görmek için örnek kod için bkz: [Oluştur](#Create) veya [güncelleştirme](#Update) yöntemi.

[Başa dön](#HOLTop)

<a name="Publish"></a>

## <a name="publish-knowledge-base"></a>Bilgi Bankası yayımlama

Aşağıdaki kodu kullanarak temel bir mevcut bilgi yayımlar [Yayımla](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func post(uri string, content string) string {
    req, _ := http.NewRequest("POST", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    if(response.StatusCode == 204) {
        return "{'result' : 'Success.'}"
    } else {
        return string(body)
    }
}

func publish(uri string, req string) string {
    fmt.Println("Calling " + uri + ".")
    return post(uri, req)
}

func main() {
    var uri = host + service + method + kb
    body := publish(uri, "")
    fmt.Printf(body + "\n")

}
```

**Bilgi Bankası yanıtı yayımlama**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="Replace"></a>

## <a name="replace-knowledge-base"></a>Bilgi Bankası değiştirin

Aşağıdaki kodu kullanarak, temel belirtilen bilgi içeriğini değiştirir [değiştirin](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_publish) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func put(uri string, content string) string {
    req, _ := http.NewRequest("PUT", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    if(response.StatusCode == 204) {
        return "{'result' : 'Success.'}"
    } else {
        return string(body)
    }
}

var req string = `{
  'qnaList': [
    {
      'id': 0,
      'answer': 'You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600',
      'source': 'Custom Editorial',
      'questions': [
        'How do I programmatically update my Knowledge Base?'
      ],
      'metadata': [
        {
          'name': 'category',
          'value': 'api'
        }
      ]
    }
  ]
}`;

func replace_kb (uri string, req string) (string) {
    fmt.Println("Calling " + uri + ".")
    return put(uri, req)
}

func main() {
    var uri = host + service + method + kb
    body := replace_kb (uri, req)
    fmt.Printf(body + "\n")
}
```

**Bilgi Bankası yanıt değiştirin**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
    "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetQnA"></a>

## <a name="download-the-contents-of-a-knowledge-base"></a>Bilgi Bankası içeriğini indir

Aşağıdaki kod temel kullanarak belirtilen bilgi içeriğini indirir [indirme Bilgi Bankası](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_download) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

// NOTE: Replace this with "test" or "prod".
var env string = "test";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/%s/%s/qna"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func get(uri string) string {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    return string(body)
}

func getQnA(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return get(uri)
}

func main() {
    var method_with_id = fmt.Sprintf(method, kb, env)
    var uri = host + service + method_with_id
    body := getQnA(uri)
    fmt.Printf(body + "\n")
}
```

**Bilgi Bankası yanıt indirin**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "qnaDocuments": [
    {
      "id": 1,
      "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
      "source": "Custom Editorial",
      "questions": [
        "How do I programmatically update my Knowledge Base?"
      ],
      "metadata": [
        {
          "name": "category",
          "value": "api"
        }
      ]
    },
    {
      "id": 2,
      "answer": "QnA Maker provides an FAQ data source that you can query from your bot or application. Although developers will find this useful, content owners will especially benefit from this tool. QnA Maker is a completely no-code way of managing the content that powers your bot or application.",
      "source": "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
      "questions": [
        "Who is the target audience for the QnA Maker tool?"
      ],
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetAnswers"></a>

## <a name="get-answers-to-a-question-by-using-a-knowledge-base"></a>Bilgi Bankası'nı kullanarak bir soru yanıtlar alın

Aşağıdaki kodu kullanarak, belirtilen Bilgi Bankası'nı kullanarak bir sorusunun yanıtlarını alır **yanıtları oluşturmak** yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
1. Aşağıda sağlanan kod ekleyin.
1. Değiştirin `host` soru-cevap Oluşturucu aboneliğiniz için Web sitesi adı değeri. Daha fazla bilgi için [soru-cevap Oluşturucu hizmeti oluşturma](../How-To/set-up-qnamaker-service-azure.md).
1. Değiştirin `endpoint_key` aboneliğiniz için geçerli uç nokta anahtar ile değeri. Bu abonelik anahtarınız ile aynı olmadığını unutmayın. Uç nokta anahtarlarınızı kullanarak alabileceğiniz [uç nokta anahtarları alma](#GetKeys) yöntemi.
1. Değiştirin `kb` yanıtları sorgulamak istediğiniz Bilgi Bankası kimliği değeri. Bu Bilgi Bankası gerekir zaten yayınlandı kullanarak Not [Yayımla](#Publish) yöntemi.
1. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// NOTE: Replace this with a valid host name.
var host string = "ENTER HOST HERE";

// NOTE: Replace this with a valid endpoint key.
// This is not your subscription key.
// To get your endpoint keys, call the GET /endpointkeys method.
var endpoint_key string = "ENTER KEY HERE";

// NOTE: Replace this with a valid knowledge base ID.
// Make sure you have published the knowledge base with the
// POST /knowledgebases/{knowledge base ID} method.
var kb string = "ENTER KB ID HERE";

var method string = "/qnamaker/knowledgebases/" + kb + "/generateanswer"

func post(uri string, content string) string {
    req, _ := http.NewRequest("POST", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Authorization", "EndpointKey " + endpoint_key)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return string(body)
}

var req string = `{
    'question': 'Is the QnA Maker Service free?',
    'top': 3
}`;

func get_answers(uri string, req string) string {
    fmt.Println("Calling " + uri + ".")
    return post(uri, req)
}

func main() {
    var uri = host + method
    body := get_answers(uri, req)
    fmt.Printf(body + "\n")
}
```

**Yanıtlar yanıt alın**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "answers": [
    {
      "questions": [
        "Can I use BitLocker with the Volume Shadow Copy Service?"
      ],
      "answer": "Yes. However, shadow copies made prior to enabling BitLocker will be automatically deleted when BitLocker is enabled on software-encrypted drives. If you are using a hardware encrypted drive, the shadow copies are retained.",
      "score": 17.3,
      "id": 62,
      "source": "https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-frequently-asked-questions",
      "metadata": []
    },
...
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKB"></a>

## <a name="get-information-about-a-knowledge-base"></a>Bilgi Bankası hakkında bilgi edinin

Aşağıdaki kodu belirtilen bilgi hakkındaki bilgileri kullanarak, temel alır [Bilgi Bankası ayrıntıları alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasedetails) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func get(uri string) string {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    return string(body)
}

func getKB(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return get(uri)
}

func main() {
    var uri = host + service + method + kb
    body := getKB(uri)
    fmt.Printf(body + "\n")
}
```

**Bilgi Bankası ayrıntıları yanıt alın**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
  "hostName": "https://qna-docs.azurewebsites.net",
  "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
  "lastChangedTimestamp": "2018-04-12T22:58:01Z",
  "name": "QnA Maker FAQ",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "urls": [
    "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq"
  ],
  "sources": [
    "Custom Editorial"
  ]
}
```

[Başa dön](#HOLTop)

<a name="GetKBsByUser"></a>

## <a name="get-all-knowledge-bases-for-a-user"></a>Bir kullanıcının tüm bilgi bankalarından alır

Aşağıdaki kod, belirtilen bir kullanıcı için tüm bilgi bankalarından hakkında bilgi alır kullanarak [alma bilgi bankaları için kullanıcı](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasesforuser) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func get(uri string) string {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    return string(body)
}

func getKBs(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return get(uri)
}

func main() {
    var uri = host + service + method
    body := getKBs(uri)
    fmt.Printf(body + "\n")
}
```

**Bilgi bankaları için kullanıcı yanıtı alın**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "knowledgebases": [
    {
      "id": "081c32a7-bd05-4982-9d74-07ac736df0ac",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T11:51:58Z",
      "lastChangedTimestamp": "2018-04-12T11:51:58Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [],
      "sources": []
    },
    {
      "id": "140a46f3-b248-4f1b-9349-614bfd6e5563",
      "hostName": "https://qna-docs.azurewebsites.net",
      "lastAccessedTimestamp": "2018-04-12T22:58:01Z",
      "lastChangedTimestamp": "2018-04-12T22:58:01Z",
      "name": "QnA Maker FAQ",
      "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
      "urls": [
        "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
        "https://docs.microsoft.com/en-us/bot-framework/resources-bot-framework-faq"
      ],
      "sources": [
        "Custom Editorial"
      ]
    },
...
  ]
}
Press any key to continue.
```

[Başa dön](#HOLTop)

<a name="Delete"></a>

## <a name="delete-a-knowledge-base"></a>Bilgi Bankası Sil

Aşağıdaki kodu kullanarak, temel belirtilen bilgi siler [Sil Bilgi Bankası](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_delete) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with a valid knowledge base ID.
var kb string = "ENTER ID HERE";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/knowledgebases/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func delete(uri string) string {
    req, _ := http.NewRequest("DELETE", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    if(response.StatusCode == 204) {
        return "{'result' : 'Success.'}"
    } else {
        return string(body)
    }
}

func deleteKB(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return delete(uri)
}

func main() {
    var uri = host + service + method + kb
    body := deleteKB(uri)
    fmt.Printf(body + "\n")

}
```

**Bilgi Bankası yanıtı Sil**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

<a name="GetKeys"></a>

## <a name="get-endpoint-keys"></a>Uç nokta anahtarları alma

Aşağıdaki kodu kullanarak geçerli uç nokta anahtarlarını alır [uç nokta anahtarları alma](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_getendpointkeys) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/endpointkeys/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func get(uri string) string {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    return string(body)
}

func getEndpointKeys(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return get(uri)
}

func main() {
    var uri = host + service + method
    body := getEndpointKeys(uri)
    fmt.Printf(body + "\n")
}
```

**Uç nokta anahtarları yanıt alın**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="PutKeys"></a>

## <a name="refresh-endpoint-keys"></a>Uç nokta anahtarlarını yenileme

Aşağıdaki kodu kullanarak geçerli uç nokta anahtarlarını yeniden oluşturur [uç nokta anahtarlarını yenileme](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_refreshendpointkeys) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

// NOTE: Replace this with "PrimaryKey" or "SecondaryKey."
var key_type string = "PrimaryKey";

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/endpointkeys/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func patch(uri string, content string) string {
    req, _ := http.NewRequest("PATCH", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    return string(body)
}

func refresh (uri string, req string) string {
    fmt.Println("Calling " + uri + ".")
    return patch(uri, req)
}

func main() {
    var uri = host + service + method + key_type
    body := refresh (uri, "")
    fmt.Printf(body + "\n")
}
```

**Uç nokta anahtarları yanıt Yenile**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "primaryEndpointKey": "ac110bdc-34b7-4b1c-b9cd-b05f9a6001f3",
  "secondaryEndpointKey": "1b4ed14e-614f-444a-9f3d-9347f45a9206"
}
```

[Başa dön](#HOLTop)

<a name="GetAlterations"></a>

## <a name="get-word-alterations"></a>Word değişiklikleri Al

Aşağıdaki kodu kullanarak geçerli sözcüğü değişiklikleri alır [indirme değişiklikleri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fc) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/alterations/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func get(uri string) string {
    req, _ := http.NewRequest("GET", uri, nil)
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)
    return string(body)
}

func getAlterations(uri string) string {
    fmt.Println("Calling " + uri + ".")
    return get(uri)
}

func main() {
    var uri = host + service + method
    body := getAlterations(uri)
    fmt.Printf(body + "\n")
}
```

**Word değişiklikleri yanıt alın**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "wordAlterations": [
    {
      "alterations": [
        "botframework",
        "bot frame work"
      ]
    }
  ]
}
```

[Başa dön](#HOLTop)

<a name="PutAlterations"></a>

## <a name="replace-word-alterations"></a>Word değişiklikleri değiştirin

Aşağıdaki kodu kullanarak geçerli sözcüğü değişiklikleri değiştirir [değiştirin değişiklikleri](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) yöntemi.

1. Sık kullandığınız IDE'de yeni bir Git projesi oluşturun.
2. Aşağıda sağlanan kod ekleyin.
3. Değiştirin `key` aboneliğiniz için geçerli bir erişim anahtarı ile değeri.
4. Programı çalıştırın.

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strconv"
)

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
var subscriptionKey string = "ENTER KEY HERE"

var host string = "https://westus.api.cognitive.microsoft.com"
var service string = "/qnamaker/v4.0"
var method string = "/alterations/"

func pretty_print(content string) string {
    var obj map[string]interface{}
    json.Unmarshal([]byte(content), &obj)
    result, _ := json.MarshalIndent(obj, "", "  ")
    return string(result)
}

func put(uri string, content string) string {
    req, _ := http.NewRequest("PUT", uri, bytes.NewBuffer([]byte(content)))
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Content-Length", strconv.Itoa(len(content)))
    client := &http.Client{}
    response, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer response.Body.Close()
    body, _ := ioutil.ReadAll(response.Body)

    if(response.StatusCode == 204) {
        return "{'result' : 'Success.'}"
    } else {
        return string(body)
    }
}

var req string = `{
  'wordAlterations': [
    {
      'alterations': [
        'botframework',
        'bot frame work'
      ]
    }
  ]
}`;

func putAlterations (uri string, req string) (string) {
    fmt.Println("Calling " + uri + ".")
    return put(uri, req)
}

func main() {
    var uri = host + service + method
    body := putAlterations (uri, req)
    fmt.Printf(body + "\n")
}
```

**Word değişiklikleri yanıt değiştirin**

Başarılı yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür: 

```json
{
  "result": "Success."
}
```

[Başa dön](#HOLTop)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)

## <a name="see-also"></a>Ayrıca bkz. 

[Soru-cevap Oluşturucu genel bakış](../Overview/overview.md)
