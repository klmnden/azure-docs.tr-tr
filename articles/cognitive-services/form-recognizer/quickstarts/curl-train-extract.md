---
title: 'Hızlı Başlangıç: Bir modeli eğitmek ve cURL - Form tanıyıcı kullanarak form verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, modeli eğitmek ve formlardaki verileri ayıklamak için Form tanıyıcı REST API ile cURL kullanacaksınız.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 3bfffc94bc11f9da2336d6edaeb96bf2e471c4ce
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67602610"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve REST API ile cURL kullanarak form verileri ayıklayın

Bu hızlı başlangıçta, eğitmek ve anahtar-değer çiftleri ve tabloları ayıklanacak forms puanlamak için cURL ile Azure Form tanıyıcı REST API kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu hızlı başlangıcı tamamlamak için şunlara sahip olmalısınız:
- Form tanıyıcı sınırlı erişim önizlemesine erişebilirsiniz. Önizleme erişim elde etmek için doldurun ve gönderme [Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu.
- [cURL](https://curl.haxx.se/windows/) yüklü.
- En az beş forms aynı türde bir dizi. Modeli eğitmek için bu verileri kullanır. Kullanabileceğiniz bir [örnek veri kümesini](https://go.microsoft.com/fwlink/?linkid=2090451) Bu Hızlı Başlangıç için. Verileri Azure Blob Depolama hesabı kök dizinine yükleyin.

## <a name="create-a-form-recognizer-resource"></a>Form tanıyıcı kaynak oluştur

[!INCLUDE [create resource](../includes/create-resource.md)]

## <a name="train-a-form-recognizer-model"></a>Bir Form tanıyıcı modeli eğitme

İlk olarak, bir Azure depolama blobu eğitim veri kümesi gerekir. En az beş doldurulmuş forms (PDF belgeleri ve/veya görüntüleri), ana girdi verisi olarak aynı türü/yapısı olması gerekir. Ya da tek bir boş formda doldurulmuş iki formlarla kullanabilirsiniz. "Boş" sözcüğünü içerecek şekilde formun boş dosya adı gerekiyor Bkz: [bir eğitim veri kümesi için özel bir model derleme](../build-training-data-set.md) ipuçları ve eğitim verilerinizi bir araya getirilmesi için Seçenekler.

Azure blob kapsayıcınızdaki belgelerle bir Form tanıyıcı modeli eğitmek için çağrı **eğitme** aşağıdaki cURL komutunu çalıştırarak API. Komutu çalıştırmadan önce şu değişiklikleri yapın:

1. Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Form tanıyıcı kaynağınızda bulabilirsiniz **genel bakış** sekmesi.
1. Değiştirin `<subscription key>` önceki adımda kopyaladığınız abonelik anahtarı.
1. Değiştirin `<SAS URL>` Azure Blob Depolama kapsayıcısı paylaşılan erişim imzası (SAS) URL'si. SAS URL'sini alın, Microsoft Azure Depolama Gezgini'ni açın, kapsayıcınızın sağ tıklatın ve seçin için **Get paylaşılan erişim imzası**. Emin **okuma** ve **listesi** izinleri denetlenir ve tıklayın **Oluştur**. Sonra da değeri kopyalayın **URL** bölümü. Form olması gereken: `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.

```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/train" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Size gönderilecektir bir `200 (Success)` aşağıdaki JSON çıkışını Yanıtla:

```json
{
  "modelId": "59e2185e-ab80-4640-aebc-f3653442617b",
  "trainingDocuments": [
    {
      "documentName": "Invoice_1.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_2.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_3.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_4.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_5.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    }
  ],
  "errors": []
}
```

Not `"modelId"` değeri. Aşağıdaki adımlarda gerekir.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Anahtar-değer çiftleri ve tabloları formlardan ayıklayın

Ardından, bir belge çözümleyin ve anahtar-değer çiftleri ve tabloları buradan ayıklamak. Çağrı **Model - analiz** aşağıdaki cURL komutu çalıştırarak API. Komutu çalıştırmadan önce şu değişiklikleri yapın:

1. Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Form tanıyıcı kaynağınızda bulabilirsiniz **genel bakış** sekmesi.
1. Değiştirin `<modelID>` önceki bölümde aldığınız model kimliği.
1. Değiştirin `<path to your form>` ile formunuzu (örneğin, C:\temp\file.pdf) dosyasının yolu.
1. Değiştirin `<file type>` dosya türüne sahip. Desteklenen türler: `application/pdf`, `image/jpeg`, `image/png`.
1. `<subscription key>` değerini abonelik anahtarınızla değiştirin.


```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/models/<modelID>/analyze" -H "Content-Type: multipart/form-data" -F "form=@\"<path to your form>\";type=<file type>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı yanıtı JSON biçiminde döndürülür. Formdan ayıklanan tablo ve anahtar-değer çiftleri temsil eder:

```bash
{
  "status": "success",
  "pages": [
    {
      "number": 1,
      "height": 792,
      "width": 612,
      "clusterId": 0,
      "keyValuePairs": [
        {
          "key": [
            {
              "text": "Address:",
              "boundingBox": [
                57.4,
                683.1,
                100.5,
                683.1,
                100.5,
                673.7,
                57.4,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "1 Redmond way Suite",
              "boundingBox": [
                57.4,
                671.3,
                154.8,
                671.3,
                154.8,
                659.2,
                57.4,
                659.2
              ],
              "confidence": 0.86
            },
            {
              "text": "6000 Redmond, WA",
              "boundingBox": [
                57.4,
                657.1,
                146.9,
                657.1,
                146.9,
                645.5,
                57.4,
                645.5
              ],
              "confidence": 0.86
            },
            {
              "text": "99243",
              "boundingBox": [
                57.4,
                643.4,
                85,
                643.4,
                85,
                632.3,
                57.4,
                632.3
              ],
              "confidence": 0.86
            }
          ]
        },
        {
          "key": [
            {
              "text": "Invoice For:",
              "boundingBox": [
                316.1,
                683.1,
                368.2,
                683.1,
                368.2,
                673.7,
                316.1,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "Microsoft",
              "boundingBox": [
                374,
                687.9,
                418.8,
                687.9,
                418.8,
                673.7,
                374,
                673.7
              ],
              "confidence": 1
            },
            {
              "text": "1020 Enterprise Way",
              "boundingBox": [
                373.9,
                673.5,
                471.3,
                673.5,
                471.3,
                659.2,
                373.9,
                659.2
              ],
              "confidence": 1
            },
            {
              "text": "Sunnayvale, CA 87659",
              "boundingBox": [
                373.8,
                659,
                479.4,
                659,
                479.4,
                645.5,
                373.8,
                645.5
              ],
              "confidence": 1
            }
          ]
        }
      ],
      "tables": [
        {
          "id": "table_0",
          "columns": [
            {
              "header": [
                {
                  "text": "Invoice Number",
                  "boundingBox": [
                    38.5,
                    585.2,
                    113.4,
                    585.2,
                    113.4,
                    575.8,
                    38.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "34278587",
                    "boundingBox": [
                      38.5,
                      547.3,
                      82.8,
                      547.3,
                      82.8,
                      537,
                      38.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Date",
                  "boundingBox": [
                    139.7,
                    585.2,
                    198.5,
                    585.2,
                    198.5,
                    575.8,
                    139.7,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/18/2017",
                    "boundingBox": [
                      139.7,
                      546.8,
                      184,
                      546.8,
                      184,
                      537,
                      139.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Due Date",
                  "boundingBox": [
                    240.5,
                    585.2,
                    321,
                    585.2,
                    321,
                    575.8,
                    240.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/24/2017",
                    "boundingBox": [
                      240.5,
                      546.8,
                      284.8,
                      546.8,
                      284.8,
                      537,
                      240.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Charges",
                  "boundingBox": [
                    341.3,
                    585.2,
                    381.2,
                    585.2,
                    381.2,
                    575.8,
                    341.3,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "$56,651.49",
                    "boundingBox": [
                      387.6,
                      546.4,
                      437.5,
                      546.4,
                      437.5,
                      537,
                      387.6,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "VAT ID",
                  "boundingBox": [
                    442.1,
                    590,
                    474.8,
                    590,
                    474.8,
                    575.8,
                    442.1,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "PT",
                    "boundingBox": [
                      447.7,
                      550.6,
                      460.4,
                      550.6,
                      460.4,
                      537,
                      447.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            }
          ]
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Form tanıyıcı REST API ile cURL bir modeli eğitmek ve bir örnek senaryosunda çalıştırmak için kullanılır. Ardından, daha fazla ayrıntılı Form tanıyıcı API'sini keşfetmek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://aka.ms/form-recognizer/api)
