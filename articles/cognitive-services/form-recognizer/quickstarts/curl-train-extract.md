---
title: 'Hızlı Başlangıç: Bir modeli eğitmek ve cURL - Form tanıyıcı kullanarak form verilerini ayıklama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, modeli eğitmek ve formlardaki verileri ayıklamak için Form tanıyıcı REST API ile cURL kullanır.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: pafarley
ms.openlocfilehash: 1afe9239dcc3f5a24d2e950ec7b563bf53d1f04c
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143232"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-using-rest-api-with-curl"></a>Hızlı Başlangıç: Bir Form tanıyıcı modeli eğitmek ve REST API ile cURL kullanarak form verilerini ayıklama

Bu hızlı başlangıçta, eğitmek ve anahtar-değer çiftleri ve tabloları ayıklanacak forms puanlamak için Form tanıyıcının REST API ile cURL kullanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Form tanıyıcı sınırlı erişim Önizleme erişimi aldığınız. Önizleme erişim elde etmek için lütfen doldurun ve gönderme [Bilişsel Hizmetleri Form tanıyıcı erişim isteği](https://aka.ms/FormRecognizerRequestAccess) formu. 
* [cURL](https://curl.haxx.se/windows/)’niz olmalıdır.
* Form tanıyıcı için bir abonelik anahtarı olması gerekir. Bölümündeki yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Form tanıyıcının abone ve anahtarınızı alın.
* Minimum düzeyde beş forms aynı türde olmalıdır. Kullanabileceğiniz bir [örnek veri kümesini](https://go.microsoft.com/fwlink/?linkid=2090451) Bu Hızlı Başlangıç için.

## <a name="train-a-form-recognizer-model"></a>Bir Form tanıyıcı modeli eğitme

İlk olarak, bir eğitim veri kümesi gerekir. Verileri Azure Blob veya kendi yerel eğitim verilerini kullanabilirsiniz. En az beş örnek form (PDF belgeleri ve/veya görüntüleri), ana girdi verisi olarak aynı türü/yapısı olmalıdır. Alternatif olarak, tek bir boş form kullanabilirsiniz; "boş" sözcüğünü formun filename içerir

Azure Blob kapsayıcınızdaki belgeler kullanan bir Form tanıyıcı modeli eğitmek için çağrı **eğitme** aşağıdaki cURL komutu yürüterek API. Komutu çalıştırmadan önce aşağıdaki değişiklikleri yapın:

* Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Bu, formu tanıyıcı kaynak genel bakış sekmesinde bulabilirsiniz.
* Değiştirin `<SAS URL>` imzası (SAS) URL'si eğitim verileri bulunduğu bir Azure Blob Depolama kapsayıcısında paylaşılan erişim.  
* `<subscription key>` değerini abonelik anahtarınızla değiştirin.

```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/train" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \"<SAS URL>\"}"
```

Size gönderilecek bir `200 (Success)` aşağıdaki JSON çıkışını Yanıtla:

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

Not `"modelId"` değeri; için aşağıdaki adımları gerekir.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Anahtar-değer çiftleri ve tabloları formlardan ayıklayın

Ardından, bir belge çözümleyin ve anahtar-değer çiftleri ve tabloları buradan ayıklamak. Çağrı **Model - analiz** aşağıdaki cURL komutu yürüterek API. Komutu çalıştırmadan önce aşağıdaki değişiklikleri yapın:

* Değiştirin `<Endpoint>` Form tanıyıcı abonelik anahtarınızı aldığınız uç noktası ile. Form tanıyıcı kaynağınızı bulun **genel bakış** sekmesi.
* Değiştirin `<modelID>` modeli eğitmek, önceki adımda aldığınız model kimliği.
* Değiştirin `<path to your form>` formunuza dosya yolu.
* `<subscription key>` değerini abonelik anahtarınızla değiştirin.
* Değiştirin `<file type>` - desteklenen türler: pdf, görüntü/jpeg, görüntü/png dosya türüne sahip.

```bash
cURL cmd: curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/models/<modelID>/analyze" -H "Content-Type: multipart/form-data" -F "form=@<path to your form>;type=application/<file type>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür ve tablolar ve ayıklanan anahtar-değer çiftleri formdan temsil eder.

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

Bu kılavuzda, tanıyıcı REST API'leri ile cURL bir modeli eğitmek ve bir örnek durumda çalıştırmak için kullanılır. Ardından, daha fazla ayrıntılı Form tanıyıcı API'sini keşfetmek için başvuru belgelerine bakın.

> [!div class="nextstepaction"]
> [REST API başvuru belgeleri](https://aka.ms/form-recognizer/api)
