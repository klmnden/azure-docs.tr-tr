---
title: Oluşturun, yayımlayın ve soru-cevap oluşturucu içinde yanıt
titleSuffix: Azure Cognitive Services
description: Genel bir web tabanlı SSS gelen soruları ve yanıtları ile yeni Bilgi Bankası oluşturun. Kaydedin, eğitmek ve Bilgi Bankası yayımlama. Bilgi Bankası yayımlandıktan sonra soru gönderebilir ve cURL komutu ile bir yanıt alırsınız. Ardından bir bot oluşturulabilir ve bot ile aynı soruyu test edin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a13e0cb0e594571344b16d007ef13475b384b73d
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693006"
---
# <a name="tutorial-from-the-qna-maker-portal-create-a-knowledge-base"></a>Öğretici: Soru-cevap Oluşturucu Portalı'ndan Bilgi Bankası oluşturma

Genel bir web tabanlı SSS gelen soruları ve yanıtları ile yeni Bilgi Bankası oluşturun. Kaydedin, eğitmek ve Bilgi Bankası yayımlama. Bilgi Bankası yayımlandıktan sonra soru gönderebilir ve cURL komutu ile bir yanıt alırsınız. Ardından bir bot oluşturulabilir ve bot ile aynı soruyu test edin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz: 

> [!div class="checklist"]
> * Soru-cevap Oluşturucu Portalı'nda bir Bilgi Bankası oluşturun.
> * Gözden geçirin, kaydetme ve Bilgi Bankası eğitme.
> * Bilgi Bankası yayımlama.
> * Bilgi Bankası'nı sorgulamak için cURL kullanın.
> * Bir bot oluşturulabilir.
 

> [!NOTE]
> Bu öğreticide programlı sürümü eksiksiz bir çözüm ile kullanıma hazır [ **Azure-Samples/bilişsel-services-qnamaker-csharp** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici için var olan bir [Soru-Cevap Oluşturma hizmetini](../How-To/set-up-qnamaker-service-azure.md) kullanmanız gerekir. 

## <a name="create-a-knowledge-base"></a>Bilgi bankası oluşturma 

1. [Soru-Cevap Oluşturma](https://www.qnamaker.ai) portalında oturum açın. 

1. Üstteki menüden **Create a knowledge base** (Bilgi bankası oluştur) öğesini seçin.

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Çünkü, varolan bir soru-cevap Oluşturucu hizmetini kullanacak olan ilk adımı atlayın. 

1. Mevcut ayarlarınızı seçin:  

    |Ayar|Amaç|
    |--|--|
    |Microsoft Azure dizin kimliği|Bu kimliği, Azure portalı ve soru-cevap Oluşturucu portalı oturum açmak için kullandığınız hesap ile ilişkilidir. |
    |Azure Aboneliği adı|Soru-cevap Oluşturucu kaynağın oluşturulduğu Faturalama hesabı.|
    |Azure Soru-Cevap Oluşturma Hizmeti|Var olan Soru-Cevap Oluşturma kaynağınızdır.|

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Bilgi Bankası adınızı girin `My Tutorial kb`.

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Aşağıdaki ayarlarla bilgi bankanızı doldurun:  

    |Ayar adı|Ayar değeri|Amaç|
    |--|--|--|
    |URL'si|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |URL'deki SSS içeriği, soru-cevap şeklinde biçimlendirilir. Soru-Cevap Oluşturma hizmeti bu biçimi yorumlayarak soruları ve cevaplarını ayıklayabilir.|
    |Dosya |_bu öğreticide kullanılmayacaktır_|Bu işlem sorular ve cevaplar için gerekli dosyaları karşıya yükler. |
    |Chit-chat personality (Konuşma kişiliği)|Kolay|Bu, kolay ve normal verir [kişilik](../Concepts/best-practices.md#chit-chat) genel sorular ve yanıtlar. Bu soruları ve cevapları daha sonra düzenleyebilirsiniz. |

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Oluşturma işlemini tamamlamak için **Create your KB** (Bilgi bankanızı oluşturun) öğesini seçin.

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-save-and-train-the-knowledge-base"></a>Bilgi bankasını gözden geçirme, kaydetme ve eğitme

1. Soruları ve cevapları gözden geçirin. İlk sayfada, URL'den alınan sorular ve cevapları bulunur. 

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Tablonun en altından son soru-cevap sayfasını seçin. Bu sayfada Chit-chat personality (Konuşma kişiliği) ayarı ile birlikte gelen sorular ve cevapları bulunur. 

1. Sorular ve cevaplar listesini üstündeki araç çubuğundan seçin **görüntüleme seçenekleri** simgesine tıklayın ve ardından **Göster meta verileri**. Bu, her bir soru ve yanıt için meta veri etiketleri gösterir. Sohbet Chit sorularınız **düzenleme: chit sohbet** meta verileri zaten ayarlanmış. Bu meta veriler seçilen yanıt yanı sıra istemci uygulamaya döndürülür. İstemci uygulama bir sohbet Robotu gibi ek işleme veya kullanıcı etkileşim belirlemek için bu filtre uygulanmış meta veri kullanabilirsiniz.

    ![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Üst menü çubuğundan **Save and train** (Kaydet ve eğit) öğesini seçin.

## <a name="publish-to-get-knowledge-base-endpoints"></a>Bilgi Bankası uç noktalarını alma yayımlama

Üst menü çubuğundan **Publish** (Yayımla) düğmesini seçin. Yayımlama sayfasında **Yayımla**'yı seçin.

![Ekran görüntüsü, soru-cevap Oluşturucu portalı](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Bilgi Bankası yayımlandıktan sonra uç nokta görüntülenir.

![Uç nokta ayarları görüntüsü](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Bu kapatmayın **Yayımla** sayfası. Bir bot oluşturmak için öğreticinin sonraki bölümlerinde, ihtiyacınız. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>SSS yanıt sorgu için cURL kullanın

1. **Curl** sekmesini seçin. 

    ![Ekran görüntüsü, Curl sekmesi](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Metni kopyalayın **Curl** sekme ve uygulamayı cURL özellikli, terminal veya komut satırı. Yetkilendirme üst bilginin değeri metin içeren `Endpoint`, sonunda boşluk ve anahtarı ile.

1. `<Your question>` yerine `How large can my KB be?` yazın. Bu ifade `How large a knowledge base can I create?` sorusuna yakındır ancak tam olarak aynısı değildir. Soru-Cevap Oluşturma, doğal dil işleme süreçlerini kullanarak iki sorunun aynı olduğunu belirler.     

1. CURL komutu çalıştırın ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Chit sohbet yanıt için sorgu için cURL kullanın

1. CURL özellikli terminalde değiştirin `How large can my KB be?` kullanıcıdan bir bot konuşma sonu ifadesiyle gibi `Thank you`.   

1. CURL komutu çalıştırın ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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

    `Thank you` sorusu bir genel konuşma sorusuyla tam olarak eşleştiği için Soru-Cevap Oluşturma tam olarak emindir ve 100 puan döndürür. Soru-cevap Oluşturucu, ayrıca ilgili tüm soruları yanı sıra Chit sohbet meta veri etiketi bilgileri içeren bir meta veri özelliği döndürdü.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Varsayılan yanıt sorgu için cURL kullanın

Soru-cevap Oluşturucu hakkında emin değil herhangi bir soru varsayılan yanıtını alır. Bu cevap Azure portalda yapılandırılır. 

1. CURL özellikli terminalde değiştirin `Thank you` ile `x`. 

1. CURL komutu çalıştırın ve puan ve yanıt dahil olmak üzere JSON yanıtı alırsınız. 

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
    
    Soru-cevap Oluşturucu puanı, döndürülen `0`, hiçbir güven anlamına gelir. Ayrıca, varsayılan yanıt döndürdü. 

## <a name="create-a-knowledge-base-bot"></a>Bilgi Bankası bot oluşturma

Daha fazla bilgi için [bu Bilgi Bankası ile Sohbet Robotu oluşturun](create-qna-bot.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi Bankası bot ile işiniz bittiğinde, kaynak grubunu kaldırma `my-tutorial-rg`bot işlemde oluşturulan tüm Azure kaynakları kaldırmak için.

Bilgi Bankası, soru-cevap Oluşturucu Portalı'nda ile işiniz bittiğinde seçin **My bilgi bankalarından**. Bilgi Bankası seçip **My öğretici kb**, en sağdaki söz konusu satırdaki Sil simgesini seçin.  

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen dosya biçimleri hakkında daha fazla bilgi için bkz. [Desteklenen veri kaynakları](../Concepts/data-sources-supported.md). 

Genel konuşma [kişilikleri](../Concepts/best-practices.md#chit-chat) hakkında daha fazla bilgi edinin.

Varsayılan cevap hakkında daha fazla bilgi için bkz. [Eşleşme bulunamadı](../Concepts/confidence-score.md#no-match-found). 

> [!div class="nextstepaction"]
> [Bu Bilgi Bankası ile Sohbet Robotu oluşturun](create-qna-bot.md)