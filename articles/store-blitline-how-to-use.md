---
title: "Görüntü için Blitline kullanmak için işleme - Azure özellik nasıl Kılavuzu"
description: "Azure uygulama içinde resimleri işlemek için Blitline hizmetini kullanmayı öğrenin."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: cwatson
ms.openlocfilehash: 254af305592ebef755ccfcb3ae4367b27fb0fc4a
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Blitline Azure ve Azure Storage ile kullanma
Bu kılavuz, Blitline hizmetlerine erişmek nasıl ve Blitline iş göndermek nasıl anlatılmıştır.

## <a name="what-is-blitline"></a>Blitline nedir?
Blitline kurumsal düzey görüntü işleme kendiniz yapılandırmak için maliyet fiyat bir kısmı sağlayan hizmet işleme bulut tabanlı bir görüntü değil.

Görüntü işleme tekrar tekrar, genellikle her Web sitesi sıfırdan yeniden gerçekleştirildi olduğunu gerçeğidir. Biz bunları milyon kez çok oluşturuncaya çünkü Biz bu unutmayın. Bir belki de, zaman olduğunu karar gün biz yalnızca herkes için bunu. Hızlı ve verimli bir şekilde yapın ve iş herkes bu arada kaydetmek için bunu nasıl biliyoruz.

Daha fazla bilgi için bkz: [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Ne Blitline değil...
Blitline için yararlı nedir açıklamak için genellikle ne Blitline ileriye doğru taşımadan önce yapmaz tanımlamak daha kolay olur.

* Blitline resimleri karşıya yüklemek için HTML pencere öğeleri yok. Genel olarak veya kısıtlı izinle kullanılabilir Blitline ulaşması için kullanılabilir olması gerekir.
* Blitline yapmaz dinamik görüntü işleme, Aviary.com gibi
* Blitline görüntüyü karşıya kabul etmez, Blitline için doğrudan görüntülerinizi gönderemezsiniz. Azure Storage veya diğer yerler Blitline destekler itme ve sonra bunları getirmek nereye Blitline söyleyin.
* Blitline yüksek düzeyde paralel ve herhangi bir zaman uyumlu işlem yapmaz. Başka bir deyişle vermelidir bize bir postback_url ve biz, biz tamamladığınızda söyleyebilir işleniyor.

## <a name="create-a-blitline-account"></a>Blitline hesabı oluşturma
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Blitline işi oluşturma
Blitline JSON görüntüde almak istediğiniz eylemleri tanımlamak için kullanır. Bu JSON birkaç basit alanlarının oluşur.

En basit örnek aşağıdaki gibidir:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Burada "src" görüntü sürer JSON sahibiz "... boys.jpeg" ve 240 x 140 görüntüsünü yeniden boyutlandırın.

Uygulama Kimliği şeydir içinde bulabilirsiniz, **bağlantı bilgisi** veya **Yönet** Azure sekmesinde. İşlerini üzerinde Blitline çalıştırmanıza olanak sağlayan, gizli tanımlayıcısıdır.

"Kaydet" parametre biz işledikten sonra görüntüyü yerleştirmek istediğiniz hakkında bilgi tanımlar. Önemsiz bu durumda, biz tanımlanan henüz. Konum tanımlanmışsa Blitline, yerel olarak (ve geçici olarak) depolar benzersiz bulut konumda. Bu konuma Blitline yaptığınızda Blitline tarafından döndürülen JSON öğesinden almak mümkün olacaktır. "Görüntü" tanımlayıcısı gereklidir ve bu belirli tanımlamak için görüntü kaydedildiğinde size geri döndürülür.

Hakkında daha fazla bilgi bulabilirsiniz *işlevleri* burada destekliyoruz: <http://www.blitline.com/docs/functions>

Burada iş seçenekleri ile ilgili belgeler de bulabilirsiniz: <http://www.blitline.com/docs/api>

JSON olduktan sonra yapmanız gereken tek şey **POST** için`http://api.blitline.com/job`

Şunun gibi JSON geri alırsınız:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Bu, Blitline isteği aldı, onu bir işleme sırasına getirdi ve onu tamamlandığında görüntü adresinde bildirir: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_uygulama\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Azure depolama hesabınıza bir görüntü kaydetme
Bir Azure depolama hesabınız varsa, Azure kapsayıcıya işlenen görüntüleri anında Blitline kolayca olabilir. Bir "azure_destination" ekleyerek iletin Blitline izinlerini ve konumunu tanımlayın.

Örnek aşağıda verilmiştir:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


CAPITALIZED değerleri kendi değerlerinizle doldurarak, bu JSON http://api.blitline.com/job olarak gönderebilir ve "src" görüntü Bulanıklaştırma filtresi ile işlenen ve, Azure hedefe gönderilir.

### <a name="please-note"></a>Lütfen unutmayın:
SAS hedef dosyanın dahil olmak üzere tüm SAS url içermesi gerekir.

Örnek:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Ayrıca, Azure Storage belgeleri Blitline'nın en son sürümünü okuyabilirsiniz [burada](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Sonraki Adımlar
Tüm bizim diğer özellikleri hakkında bilgi için blitline.com ziyaret edin:

* Blitline API uç noktası belgeleri <http://www.blitline.com/docs/api>
* Blitline API işlevleri <http://www.blitline.com/docs/functions>
* Blitline API örnekleri <http://www.blitline.com/docs/examples>
* Üçüncü Nuget kitaplığı Kısım <http://nuget.org/packages/Blitline.Net>

