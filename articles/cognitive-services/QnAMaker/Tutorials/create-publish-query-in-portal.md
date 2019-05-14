---
title: Oluşturun, yayımlayın, soru-cevap oluşturucu içinde yanıt
titleSuffix: Azure Cognitive Services
description: Genel bir web tabanlı SSS gelen soruları ve yanıtları ile yeni Bilgi Bankası oluşturun. Kaydedin, eğitmek ve Bilgi Bankası yayımlama. Bilgi Bankası yayımlandıktan sonra bir soru gönderin ve CURL komutu ile bir yanıt alırsınız. Ardından bir bot oluşturulabilir ve bot ile aynı soruyu test edin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a80a815d4a1a892b5258aef1c1fc7ef4ab881fe7
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65594156"
---
# <a name="tutorial-from-qna-maker-portal-create-a-knowledge-base"></a>Öğretici: Soru-cevap Oluşturucu Portalı'ndan Bilgi Bankası oluşturma

Genel bir web tabanlı SSS gelen soruları ve yanıtları ile yeni Bilgi Bankası oluşturun. Kaydedin, eğitmek ve Bilgi Bankası yayımlama. Bilgi Bankası yayımlandıktan sonra bir soru gönderin ve Curl komutu ile bir yanıt alırsınız. Ardından bir bot oluşturulabilir ve bot ile aynı soruyu test edin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Soru-Cevap Oluşturma portalında bilgi bankası oluşturma
> * Bilgi bankasını gözden geçirme, kaydetme ve eğitme
> * Bilgi bankasını yayımlama
> * Curl kullanarak bilgi bankasını sorgulama
> * Bir bot oluşturun
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
    |Microsoft Azure Directory Kimliği|_Microsoft Azure dizin kimliği_ Azure portalı ve soru-cevap Oluşturucu portalı oturum açmak için kullandığınız hesap ile ilişkilidir. |
    |Azure Aboneliği adı|Soru-Cevap Oluşturma kaynağını oluşturduğunuz fatura hesabıdır.|
    |Azure Soru-Cevap Oluşturma Hizmeti|Var olan Soru-Cevap Oluşturma kaynağınızdır.|

    ![KB oluşturma işleminin 2. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Bir sonraki adımda bilgi bankanıza `My Tutorial kb` adını verin.

    ![KB oluşturma işleminin 3. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Sonraki adımda bilgi bankanızı aşağıdaki ayarlarla yapılandırın:  

    |Ayar adı|Ayar değeri|Amaç|
    |--|--|--|
    |URL'si|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |URL'deki SSS içeriği, soru-cevap şeklinde biçimlendirilir. Soru-Cevap Oluşturma hizmeti bu biçimi yorumlayarak soruları ve cevaplarını ayıklayabilir.|
    |Dosya |_bu öğreticide kullanılmayacaktır_|Bu işlem sorular ve cevaplar için gerekli dosyaları karşıya yükler. |
    |Chit-chat personality (Konuşma kişiliği)|Kolay|Bu, kolay ve normal verir [kişilik](../Concepts/best-practices.md#chit-chat) genel sorular ve yanıtlar. Bu soruları ve cevapları daha sonra düzenleyebilirsiniz. |

    ![KB oluşturma işleminin 4. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Oluşturma işlemini tamamlamak için **Create your KB** (Bilgi bankanızı oluşturun) öğesini seçin.

    ![KB oluşturma işleminin 5. adımı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>Bilgi bankasını gözden geçirme, kaydetme ve eğitme

1. Soruları ve cevapları gözden geçirin. İlk sayfada, URL'den alınan sorular ve cevapları bulunur. 

    ![Kaydet ve eğit](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Tablonun en altından son soru-cevap sayfasını seçin. Bu sayfada Chit-chat personality (Konuşma kişiliği) ayarı ile birlikte gelen sorular ve cevapları bulunur. 

1. Sorular ve cevaplar listesini üstündeki araç çubuğundan seçin **görüntüleme seçenekleri** simgesini seçip **Göster meta verileri**. Bu, her bir soru ve yanıt için meta veri etiketleri gösterir. Sohbet Chit sorularınız **düzenleme: chit sohbet** meta verileri zaten ayarlanmış. Bu meta veriler seçilen yanıt yanı sıra istemci uygulamaya döndürülür. İstemci uygulama bir sohbet Robotu gibi ek işleme veya kullanıcı etkileşim belirlemek için bu filtre uygulanmış meta veri kullanabilirsiniz.

    ![! [Etiketler görünümü meta verileri] (.. / media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png#lightbox)

1. Üst menü çubuğundan **Save and train** (Kaydet ve eğit) öğesini seçin.

## <a name="publish-to-get-kb-endpoints"></a>Bilgi bankasını yayımlama ve uç noktaları alma

Üst menü çubuğundan **Publish** (Yayımla) düğmesini seçin. Yayımlama sayfası açıldığında **Cancel** (İptal) düğmesinin yanındaki **Publish** (Yayımla) düğmesini seçin.

![Yayımlama](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Bilgi bankası yayımlandıktan sonra uç nokta görüntülenir

![Yayımlama Sayfası uç noktası ayarları](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Bu kapatmayın **Yayımla** sayfasında, bu öğreticinin ilerleyen bölümlerinde bir bot oluşturulabilir için kullanacağı. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>SSS yanıt sorgulamak için Curl kullanma

1. **Curl** sekmesini seçin. 

    ![Curl komutu](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. **Curl** sekmesinin metnini kopyalayın ve Curl özellikli bir terminalde veya komut satırında yürütün. Yetkilendirme üst bilgisinin değeri `Endpoint` metni, bir boşluk ve anahtar değerini içerir.

1. `<Your question>` yerine `How large can my KB be?` yazın. Bu ifade `How large a knowledge base can I create?` sorusuna yakındır ancak tam olarak aynısı değildir. Soru-Cevap Oluşturma, doğal dil işleme süreçlerini kullanarak iki sorunun aynı olduğunu belirler.     

1. Curl komutunu yürütün ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Curl sorguya Chit sohbet yanıt için kullanın.

1. Curl özellikli terminalde değiştirin `How large can my KB be?` kullanıcıdan bir bot konuşma sonu ifadesiyle gibi `Thank you`.   

1. Curl komutunu yürütün ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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

## <a name="use-curl-to-query-for-the-default-answer"></a>Varsayılan yanıt sorgulamak için Curl kullanma

Soru-Cevap Oluşturma hizmeti, cevap veremediği sorular için varsayılan cevabı kullanır. Bu cevap Azure portalda yapılandırılır. 

1. Curl özellikli terminalde `Thank you` yerine `x` yazın. 

1. Curl komutunu yürütün ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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
    
    Soru-cevap Oluşturucu puanı, döndürülen `0`, hiçbir güven anlamına gelir, ancak aynı zamanda varsayılan yanıt döndürdü. 

## <a name="create-a-knowledge-base-bot"></a>Bilgi Bankası bot oluşturma

Daha fazla bilgi için [bu Bilgi Bankası ile Sohbet Robotu oluşturun](create-qna-bot.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası bot ile işiniz bittiğinde, kaynak grubunu kaldırma `my-tutorial-rg`bot işlemde oluşturulan tüm Azure kaynakları kaldırmak için.

Bilgi Bankası, soru-cevap Oluşturucu Portalı'nda ile işiniz bittiğinde seçin **My bilgi bankalarından**, Bilgi Bankası'ı seçin **My öğretici kb**, ardından söz konusu satırdaki sağ uçta Sil simgesini seçin.  

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [desteklenen veri kaynakları](../Concepts/data-sources-supported.md) destek dosya biçimleri hakkında daha fazla bilgi için. 

Genel konuşma [kişilikleri](../Concepts/best-practices.md#chit-chat) hakkında daha fazla bilgi edinin.

Varsayılan cevap hakkında daha fazla bilgi için bkz. [Eşleşme bulunamadı](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Bu Bilgi Bankası ile Sohbet Robotu oluşturun](create-qna-bot.md)