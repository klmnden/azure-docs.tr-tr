---
title: Alan grubu durumu - Azure Logic Apps eylemleri çalıştırmak kapsamları ekleyin | Microsoft Docs
description: Grup eylem durumu Azure Logic apps'te temel iş akışı eylemleri Çalıştırma kapsamı oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.reviewer: klam, LADocs
ms.date: 10/03/2018
ms.topic: article
ms.openlocfilehash: 48fb2d14cd4cf99510fff88b25b9ae45814a92a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60685555"
---
# <a name="run-actions-based-on-group-status-with-scopes-in-azure-logic-apps"></a>Azure Logic apps'te kapsamları Grup durumuyla göre eylemleri çalıştırma

Başka bir grup eylemlerin başarılı veya başarısız sonra eylemleri çalıştırmak için bu eylemlerin içinde grubunda bir *kapsam*. Bu yapı, mantıksal grup olarak eylemleri düzenlemek, bu grubun durumunu değerlendirmek ve kapsamın durumuna dayalı eylemleri gerçekleştirmek istediğinizde yararlıdır. Bir kapsam içindeki tüm eylemleri çalıştırma işlemini tamamladıktan sonra kapsam, ayrıca kendi durumlarını alır. Örneğin, uygulamak istediğiniz zaman kapsamlarını kullanabilirsiniz [özel durum ve hata işleme](../logic-apps/logic-apps-exception-handling.md#scopes). 

Bir kapsamın durumu denetlemek için bir mantıksal belirlemek için kullandığınız aynı kriteri kullanabilirsiniz uygulamaların durumu "Başarılı", "Başarısız", "İptal edildi" ve benzeri gibi çalıştırın. Kapsamın tüm eylemleri başarılı olduğunda, varsayılan olarak, kapsamın durumu "Başarılı" olarak işaretlenmiş Ancak kapsamdaki herhangi bir işlem başarısız olduğunda veya iptal edildi, kapsamın durumu "Başarısız" olarak işaretlenmiş Kapsamlar hakkında daha fazla limitleri için bkz [limitler ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

Örneğin, belirli eylemleri ve kapsamın durumu kontrol etme koşulu çalıştırmak için bir kapsam kullanan bir üst düzey mantıksal uygulama aşağıdadır. Kapsamdaki herhangi bir eylem başarısız veya beklenmedik şekilde sona, kapsamı sırasıyla "Başarısız" veya "İptal edildi" işaretlenir ve mantıksal uygulama bir "Kapsam başarısız oldu" iletisi gönderir. Kapsamı belirli eylemlerin tümü başarısız olursa mantıksal uygulama bir "Kapsam başarılı oldu" iletisi gönderir.

!['Zamanlama – yinelenme"tetikleyicisini ' ayarlayın](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örnek takip etmek için bu öğeler gerekir:

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcısından bir e-posta hesabı. Bu örnek, Outlook.com kullanır. Farklı bir sağlayıcı kullanıyorsanız genel akışı aynı kalır, ancak kullanıcı Arabirimi farklı görünür.

* Bing Haritalar anahtarını. Bu anahtarı almak için bkz: <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">Bing Haritalar anahtarını alma</a>.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Örnek mantıksal uygulama oluşturma

İlk olarak, daha sonra bir kapsam ekleyebilirsiniz, böylece bu örnek mantıksal uygulama oluşturun:

![Örnek mantıksal uygulama oluşturma](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* A **zamanlama - yinelenme** Bing Haritalar hizmetini denetler, belirttiğiniz bir zaman aralığında tetikleyicisi
* A **Bing Haritalar - rota Al** iki konum arasında seyahat süresini denetleyen eylemin
* Seyahat süresi, belirtilen seyahat süresini aşan olup olmadığını denetleyen bir koşul deyimi
* Bu geçerli seyahat süresini e-posta gönderen bir eylem, belirtilen zaman aşıyor

Mantıksal uygulamanızı kaydetmek istediğiniz zaman, genellikle çalışmalarınızı kaydedin.

1. Oturum <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, henüz yapmadıysanız. Boş bir mantıksal uygulama oluşturma.

1. Ekleme **zamanlama - yinelenme** şu ayarlarla tetikleyici: **Aralığı** = "1" ve **sıklığı** "Minute" =

   !['Zamanlama – yinelenme"tetikleyicisini ' ayarlayın](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Görsel olarak görünümünüzü basitleştirin ve her eylemin ayrıntıları Tasarımcısı'nda gizlemek için bu adımları ilerlemeyi olarak her eylemin şekli daraltın.

1. Ekleme **Bing Haritalar - rota Al** eylem. 

   1. Bing Haritalar bağlantınız yoksa bir bağlantı oluşturmanız istenir.

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Bağlantı Adı** | BingMapsConnection | Bağlantınıza bir ad verin. | 
      | **API Anahtarı** | <*your-Bing-Maps-key*> | Daha önce aldığınız Bing Haritalar anahtarını girin. | 
      ||||  

   1. Ayarlama, **rota Al** bu görüntünün altındaki tabloda gösterildiği gibi bir eylem:

      !["Bing Haritalar - rota Al" ayarlamak eylemi](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Bu parametreler hakkında daha fazla bilgi için bkz. [Rota hesaplama](https://msdn.microsoft.com/library/ff701717.aspx).

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Güzergah noktası 1** | <*Başlangıç*> | Rotanızın girin. | 
      | **Güzergah noktası 2** | <*end*> | Rotanızın hedefi girin. | 
      | **Kaçının** | None | Ücretli geçişler, Otoyollar gibi yönlendiricilerin önlemek ve benzeri öğeleri girin. Olası değerler için bkz. [rota hesaplama](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **İyileştir** | timeWithTraffic | Uzaklık, zaman ile geçerli trafik bilgileri vb. gibi Rotanızı iyileştirmeye yönelik bir parametre seçin. Bu örnekte bu değer: "timeWithTraffic" | 
      | **Mesafe birimi** | <*your-preference*> | Hesaplamak rotanız için mesafe birimi girin. Bu örnekte, bu değeri kullanır: "Mil" | 
      | **Seyahat modu** | Sürüş | Rotanız için seyahat modunu girin. Bu örnekte bu değer "Driving" kullanır. | 
      | **Toplu Ulaşım Tarih-Saati** | None | Yalnızca toplu ulaşım modu için geçerlidir. | 
      | **Aktarım türü tarih türü** | None | Yalnızca toplu ulaşım modu için geçerlidir. | 
      ||||  

1. [Koşul Ekle](../logic-apps/logic-apps-control-flow-conditional-statement.md) geçerli seyahat süresi trafik ile belirli bir süre aşıyor olup olmadığını denetler. 
   Bu örnekte, aşağıdaki adımları izleyin:

   1. Koşulu şu açıklama ile yeniden adlandırın: **Trafik zaman belirtilen süreden fazlaysa**

   1. En soldaki sütunda içine tıklayın **bir değer seçin** dinamik içerik listesinde görünecek şekilde kutusu. Bu listeden **seyahat süresi trafik** saniyeler içinde alanı. 

      ![Koşul derleme](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Orta kutusunda şu işleci seçin: **büyüktür**

   1. En sağdaki sütunda eşdeğer 10 dakika ve saniye içinde bu karşılaştırma değeri girin: **600**

      İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

      ![Tamamlanmış koşul](./media/logic-apps-control-flow-run-steps-group-scopes/finished-condition.png)

1. İçinde **doğruysa** dal, e-posta sağlayıcınız için bir "e-posta Gönder" eylemini ekleyin. 
   Bu eylem, bu görüntünün altındaki adımları izleyerek ayarlama:

   !["True ise", "e-posta Gönder" eylemini ekleme dal](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. İçinde **için** test amacıyla e-posta adresinizi girin.

   1. İçinde **konu** alan, şu metni girin:

      ```Time to leave: Traffic more than 10 minutes```

   1. İçinde **gövdesi** bir boşluk koyarak şu metni girin: 

      ```Travel time:```

      İmlecinizi görünür ancak **gövdesi** alan, dinamik içerik listesinden kalır açık bu noktada kullanılabilir herhangi bir parametre seçebilirsiniz.

   1. Dinamik içerik listesinde **İfade**’yi seçin.

   1. Bulmak ve seçmek **div()** işlevi. 
      İmlecinizi, işlevin parantez içinde yerleştirin.

   1. İmlecinizi bir işlevin parantez içinde olsa da, seçin **dinamik içerik** dinamik içerik listesini görüntüleyin. 
   
   1. Gelen **rota Al** bölümünden **trafiği süresi trafik** alan.

      !["Trafiği süresi trafik" seçin](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   1. JSON biçimine alan çözümlendikten sonra ekleme bir **virgülle** (```,```) bir sayı ```60``` değeri dönüştürme böylece **trafiği süresi trafik** saniyelerden dakikalara. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      Şimdi ifadeniz şu örnekteki gibi görünür:

      ![İfadeyi tamamlayın](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   1. İşiniz bittiğinde seçin **Tamam**.

   <!-- markdownlint-disable MD038 -->
   1. İfade çözümlendikten sonra başında boşluk koyarak şu metni ekleyin: ``` minutes```
  
       **Gövdesi** alan artık şu örnekteki gibi görünür:

       ![Tamamlanmış "Gövde" alanı](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)
   <!-- markdownlint-enable MD038 -->

1. Mantıksal uygulamanızı kaydedin.

Ardından, böylece belirli eylemleri grup ve bunların durumunu değerlendirmek kapsam ekleyin.

## <a name="add-a-scope"></a>"Kapsam" ekle

1. Henüz yapmadıysanız, mantıksal uygulamanızı Logic Apps Tasarımcısı'nda açın. 

1. Bir kapsam, istediğiniz iş akışı konumunda ekleyin. Örneğin, bir kapsamı arasında var olan mantıksal uygulama iş akışı adımları eklemek için aşağıdaki adımları izleyin: 

   1. İmlecinizi, kapsamı eklemek istediğiniz okun üzerine getirin. 
   Seçin **artı** (**+**) > **Eylem Ekle**.

      !["Kapsam" ekle](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

   1. Arama kutusuna filtreniz olarak "scope" girin. 
   Seçin **kapsam** eylem.

## <a name="add-steps-to-scope"></a>Kapsama adımlar ekleyin

1. Artık adımlar eklemeniz veya kapsam içinde çalıştırmak istediğiniz var olan adımları sürükleyin. Bu örnekte, bu eylemleri kapsamına sürükleyin:
      
   * **Rota Al**
   * **Trafik zaman belirtilen süreden fazlaysa**, her ikisini de içeren **true** ve **false** dallar

   Mantıksal uygulamanız artık şu örnekteki gibi görünür:

   ![Kapsam eklendi](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

1. Kapsam altında kapsamın durumu denetleyen bir koşul ekleyin. Koşulu şu açıklama ile yeniden adlandırın: **Kapsam başarısız olduysa**

   ![Kapsam durumunu denetlemek için koşul Ekle](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
1. Koşul kapsamın durumu "Başarısız" veya "İptal edildi" eşit olup olmadığını denetleyin. Bu ifadeler ekleyin. 

   1. Başka bir satır eklemek için **Ekle**. 

   1. Dinamik içerik listesinde görünecek şekilde her satırda sol kutusunun içine tıklayın. 
   Dinamik içerik listesinden seçin **ifade**. Düzenleme kutusuna şu ifadeyi girin ve ardından **Tamam**: 
   
      `result('Scope')[0]['status']`

      ![Kapsamın durumu denetleyen bir ifade ekleyin](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status.png)

   1. Her iki satır seçin **eşittir** işleci olarak. 
   
   1. Karşılaştırma değerlerinin ilk satırını girin `Failed`. 
   İkinci satırda girin `Aborted`. 

      İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

      ![Kapsamın durumu denetleyen bir ifade ekleyin](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status-finished.png)

      Şimdi koşulun ayarlamak `runAfter` koşul kapsamı durumunu denetler ve eşleşen eylemi çalıştıran özelliğini daha sonraki adımlarda tanımlayın.

   1. Üzerinde **kapsam başarısız olduysa** koşul öğesini **üç nokta** (...) düğmesini ve ardından **sonrasında çalıştırmayı Yapılandır**.

      !['RunAfter' özelliği'ni yapılandırma](./media/logic-apps-control-flow-run-steps-group-scopes/configure-run-after.png)

   1. Bu kapsam durumlarını seçin: **başarılı**, **başarısız oldu**, **atlandı**, ve **zaman aşımına uğradı**

      ![Kapsam durumlarını seçin](./media/logic-apps-control-flow-run-steps-group-scopes/select-run-after-statuses.png)

   1. İşiniz bittiğinde **Bitti**'yi seçin. 
   Koşul artık bir "bilgi" simgesi gösterir.

1. İçinde **doğruysa** ve **false ise** dallar ekleme gerçekleştirmek istediğiniz eylemleri her kapsam duruma göre örneğin, bir e-posta veya ileti gönderin.

   ![Kapsam durumuna göre gerçekleştirilecek eylemler ekleyin](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

1. Mantıksal uygulamanızı kaydedin.

Tamamlanmış mantıksal uygulamanız artık şu örnekteki gibi görünür:

![Tamamlanmış mantıksal uygulama kapsamlı](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>İşinizi test etme

Tasarımcı araç çubuğunda **çalıştırma**. Kapsamı belirli eylemlerin tümü başarılı olursa, "Kapsam başarılı oldu" iletisi alıyorum. Kapsamı belirlenmiş eylemleri yoksa başarılı olursa, "Kapsam başarısız oldu" iletisi alıyorum. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON tanımı

Kod Görünümü'nde çalışıyorsanız, mantıksal uygulamanızın JSON tanımında bir kapsama yapısı yerine tanımlayabilirsiniz. Örneğin, tetikleyici ve Eylemler önceki mantıksal uygulama için JSON tanımı şöyledir:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    }
  }
}
```

```json
"actions": {
  "If_scope_failed": {
    "type": "If",
    "actions": {
      "Scope_failed": {
        "type": "ApiConnection",
        "inputs": {
          "body": {
            "Body": "Scope failed. Scope status: @{result('Scope')[0]['status']}",
            "Subject": "Scope failed",
            "To": "<your-email@domain.com>"
          },
          "host": {
            "connection": {
              "name": "@parameters('$connections')['outlook']['connectionId']"
            }
          },
          "method": "post",
          "path": "/Mail"
        },
        "runAfter": {}
      }
    },
    "else": {
      "actions": {
        "Scope_succeded": {
          "type": "ApiConnection",
          "inputs": {
            "body": {
              "Body": "Scope succeeded. Scope status: @{result('Scope')[0]['status']}",
              "Subject": "Scope succeeded",
              "To": "<your-email@domain.com>"
            },
            "host": {
              "connection": {
               "name": "@parameters('$connections')['outlook']['connectionId']"
              }
            },
            "method": "post",
            "path": "/Mail"
          },
          "runAfter": {}
        }
      }
    },
    "expression": {
      "or": [ 
         {
            "equals": [ 
              "@result('Scope')[0]['status']", 
              "Failed"
            ]
         },
         {
            "equals": [
               "@result('Scope')[0]['status']", 
               "Aborted"
            ]
         } 
      ]
    },
    "runAfter": {
      "Scope": [
        "Failed",
        "Skipped",
        "Succeeded",
        "TimedOut"
      ]
    }
  },
  "Scope": {
    "type": "Scope",
    "actions": {
      "Get_route": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['bingmaps']['connectionId']"
            }
          },
          "method": "get",
          "path": "/REST/V1/Routes/Driving",
          "queries": {
            "distanceUnit": "Mile",
            "optimize": "timeWithTraffic",
            "travelMode": "Driving",
            "wp.0": "<start>",
            "wp.1": "<end>"
          }
        },
        "runAfter": {}
      },
      "If_traffic_time_is_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'],60)} minutes",
                 "Subject": "Time to leave: Traffic more than 10 minutes",
                 "To": "<your-email@domain.com>"
              },
              "host": {
                "connection": {
                   "name": "@parameters('$connections')['outlook']['connectionId']"
                }
              },
              "method": "post",
              "path": "/Mail"
            },
            "runAfter": {}
          }
        },
        "expression": {
          "and" : [
            {
               "greater": [ 
                  "@body('Get_route')?['travelDurationTraffic']", 
                  600
               ]
            }
          ]
        },
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderin veya özellikleri ve önerileri oylamak için şurayı ziyaret edin [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere (switch deyimleri) adımlarını çalıştırmayı](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve yineleme adımları (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırın veya paralel adımları (dallar) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
