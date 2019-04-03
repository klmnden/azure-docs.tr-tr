---
title: Oluşturun, yayımlayın, soru-cevap oluşturucu içinde yanıt
titleSuffix: Azure Cognitive Services
description: Bu portal tabanlı öğretici, program aracılığıyla bilgi bankası oluşturup yayımlama ve bunu kullanarak soruları cevaplama adımlarını göstermektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 12/17/2018
ms.author: diberry
ms.openlocfilehash: 6f79614e4b1ec660d2ec5c8aee40924908cf8f5c
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58884134"
---
# <a name="tutorial-create-a-knowledge-base-then-answer-question-via-the-qna-maker-portal"></a>Öğretici: Bilgi Bankası sonra soruyu yanıtlama soru-cevap Oluşturucu portalı üzerinden oluşturma

Bu öğretici, bilgi bankası oluşturup yayımlama ve bunu kullanarak soruları cevaplama adımlarını göstermektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Soru-Cevap Oluşturma portalında bilgi bankası oluşturma
> * Bilgi bankasını gözden geçirme, kaydetme ve eğitme
> * Bilgi bankasını yayımlama
> * Curl kullanarak bilgi bankasını sorgulama
> 
> [!NOTE]
> Bu öğreticide programlı sürümü eksiksiz bir çözüm ile kullanıma hazır [ **Azure-Samples/bilişsel-services-qnamaker-csharp** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için var olan bir [Soru-Cevap Oluşturma hizmetini](../How-To/set-up-qnamaker-service-azure.md) kullanmanız gerekir. 

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma 

1. [Soru-Cevap Oluşturma](https://www.qnamaker.ai) portalında oturum açın. 

1. Üstteki menüden **Create a knowledge base** (Bilgi bankası oluştur) öğesini seçin.

    ![KB oluşturma işleminin 1. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Var olan Soru-Cevap Oluşturma hizmetini kullanacağınız için ilk adımı atlayabilirsiniz. 

1. Bir sonraki adımda var olan ayarlarınızı seçin:  

    |Ayar|Amaç|
    |--|--|
    |Microsoft Azure Directory Kimliği|_Microsoft Azure Directory Kimliğiniz_, Azure portal ve Soru-Cevap Oluşturma portalında oturum açmak için kullandığınız hesapla ilişkilendirilmiştir. |
    |Azure Aboneliği adı|Soru-Cevap Oluşturma kaynağını oluşturduğunuz fatura hesabıdır.|
    |Azure Soru-Cevap Oluşturma Hizmeti|Var olan Soru-Cevap Oluşturma kaynağınızdır.|

    ![KB oluşturma işleminin 2. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Bir sonraki adımda bilgi bankanıza `My Tutorial kb` adını verin.

    ![KB oluşturma işleminin 3. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Sonraki adımda bilgi bankanızı aşağıdaki ayarlarla yapılandırın:  

    |Ayar adı|Ayar değeri|Amaç|
    |--|--|--|
    |URL'si|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |URL'deki SSS içeriği, soru-cevap şeklinde biçimlendirilir. Soru-Cevap Oluşturma hizmeti bu biçimi yorumlayarak soruları ve cevaplarını ayıklayabilir.|
    |Dosya |_Bu öğreticide kullanılan değil_|Bu işlem sorular ve cevaplar için gerekli dosyaları karşıya yükler. |
    |Chit-chat personality (Konuşma kişiliği)|The friend (Arkadaş)|Bu ayar yaygın sorulara ve cevaplara arkadaş canlısı ve günlük kullanım dilini kullanan bir kişilik ekler. Bu soruları ve cevapları daha sonra düzenleyebilirsiniz. |

    ![KB oluşturma işleminin 4. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Oluşturma işlemini tamamlamak için **Create your KB** (Bilgi bankanızı oluşturun) öğesini seçin.

    ![KB oluşturma işleminin 5. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>Bilgi bankasını gözden geçirme, kaydetme ve eğitme

1. Soruları ve cevapları gözden geçirin. İlk sayfada, URL'den alınan sorular ve cevapları bulunur. 

    ![Kaydet ve eğit](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Tablonun en altından son soru-cevap sayfasını seçin. Bu sayfada Chit-chat personality (Konuşma kişiliği) ayarı ile birlikte gelen sorular ve cevapları bulunur. 

1. Sorular ve cevaplar listesini üstündeki araç çubuğundan meta verileri simgesini seçin. Bu, her bir soru ve yanıt için meta veri etiketleri gösterir. Sohbet Chit sorularınız **düzenleme: chit sohbet** meta verileri zaten ayarlanmış. Bu meta veriler seçilen yanıt yanı sıra istemci uygulamaya döndürülür. İstemci uygulama bir sohbet Robotu gibi ek işleme veya kullanıcı etkileşim belirlemek için bu filtre uygulanmış meta veri kullanabilirsiniz.

    ![Meta veri etiketleri görüntüleme](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Üst menü çubuğundan **Save and train** (Kaydet ve eğit) öğesini seçin.

## <a name="publish-to-get-kb-endpoints"></a>Bilgi bankasını yayımlama ve uç noktaları alma

Üst menü çubuğundan **Publish** (Yayımla) düğmesini seçin. Yayımlama sayfası açıldığında **Cancel** (İptal) düğmesinin yanındaki **Publish** (Yayımla) düğmesini seçin.

![Yayımlama](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Bilgi bankası yayımlandıktan sonra uç nokta görüntülenir

![Yayımlama Sayfası uç noktası ayarları](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

## <a name="use-curl-to-query-for-an-faq-answer"></a>Curl kullanarak SSS cevabı sorgulama

1. **Curl** sekmesini seçin. 

    ![Curl komutu](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. **Curl** sekmesinin metnini kopyalayın ve Curl özellikli bir terminalde veya komut satırında yürütün. Yetkilendirme üst bilgisinin değeri `Endpoint` metni, bir boşluk ve anahtar değerini içerir.

1. `<Your question>` yerine `How large can my KB be?` yazın. Bu ifade `How large a knowledge base can I create?` sorusuna yakındır ancak tam olarak aynısı değildir. Soru-Cevap Oluşturma, doğal dil işleme süreçlerini kullanarak iki sorunun aynı olduğunu belirler.     

1. CURL komutunu yürütün ve puanla cevabı içeren JSON cevabını alın. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    Soru-Cevap Oluşturma, %42,81 puanla cevaptan emin sayılır.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Curl kullanarak genel konuşma sorgulama

1. Curl özellikli bir terminalde `How large can my KB be?` yerine `Thank you` gibi kullanıcının bot sohbetini sonlandırmak için kullandığı bir ifade yazın.   

1. CURL komutunu yürütün ve puanla cevabı içeren JSON cevabını alın. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    `Thank you` sorusu bir genel konuşma sorusuyla tam olarak eşleştiği için Soru-Cevap Oluşturma tam olarak emindir ve 100 puan döndürür. Soru-cevap Oluşturucu, ayrıca tüm ilgili sorular ve bunun yanı sıra Chit sohbet meta veri etiketi bilgileri içeren bir meta veri özelliği döndürdü.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Curl kullanarak varsayılan cevap sorgulama

Soru-Cevap Oluşturma hizmeti, cevap veremediği sorular için varsayılan cevabı kullanır. Bu cevap Azure portalda yapılandırılır. 

1. Curl özellikli terminalde `Thank you` yerine `x` yazın. 

1. CURL komutunu yürütün ve puanla cevabı içeren JSON cevabını alın. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    Soru-Cevap Oluşturma hizmeti sıfır güven ifade eden 0 puanının yanı sıra varsayılan cevabı döndürür. 

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz. [Desteklenen veri kaynakları](../Concepts/data-sources-supported.md). 

Genel konuşma [kişilikleri](../Concepts/best-practices.md#chit-chat) hakkında daha fazla bilgi edinin.

Varsayılan cevap hakkında daha fazla bilgi için bkz. [Eşleşme bulunamadı](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Bilgi Bankası kavramları](../Concepts/knowledge-base.md)