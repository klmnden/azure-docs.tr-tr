---
title: Grubu durumu - Azure Logic Apps temel eylemleri kapsamları ekleyin | Microsoft Docs
description: Grup eylem durumu Azure Logic Apps içinde temel iş akışı eylemleri Çalıştırma kapsamı oluşturma
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.date: 03/05/2018
ms.topic: article
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 1258175eb3d28d39be8be08498ba8d2e0998aa43
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35298823"
---
# <a name="create-scopes-that-run-workflow-actions-based-on-group-status-in-azure-logic-apps"></a>Azure mantıksal uygulamaları Grup durumu temel iş akışı eylemleri çalıştırmak kapsamları oluşturun

Başka bir grup işlemlerin başarılı veya başarısız sonra eylemleri çalıştırmak için bu eylemleri içinde grup bir *kapsam*. Bu yapı, mantıksal grup olarak Eylemler düzenlemek, o grubun durumunu değerlendirmek ve kapsamın durum üzerinde temel eylemleri gerçekleştirmek istediğiniz yararlıdır. Çalıştırma kapsamı içindeki tüm eylemler tamamladıktan sonra kapsam Ayrıca kendi durumlarını alır. Örneğin, uygulamak istediğiniz zaman kapsamlarını kullanabilirsiniz [özel durumu ve hata işleme](../logic-apps/logic-apps-exception-handling.md#scopes). 

Bir kapsamın durumu denetlemek için bir mantığı belirlemek için kullandığınız aynı ölçütleri kullanabilirsiniz uygulamaları durumu "Başarılı", "Başarısız", "İptal edildi" vb. gibi çalıştırın. Kapsamın tüm eylemler başarılı olduğunda, varsayılan olarak, kapsamın durumu "Başarılı" olarak işaretlenmiş Ancak kapsamı içinde herhangi bir işlem başarısız olduğunda veya iptal edildi, kapsamın durumu "Başarısız" olarak işaretlenmiş Kapsamlar üzerinde sınırları için bkz: [sınırları ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md). 

Örneğin, belirli eylemleri ve kapsamın durumunu denetlemek için bir koşul çalıştırmak için bir kapsam kullandığı bir üst düzey mantıksal uygulama aşağıdadır. Kapsamdaki herhangi bir eylem başarısız veya beklenmedik şekilde sona, kapsam sırasıyla "Başarısız" veya "İptal edildi" işaretlenir ve mantıksal uygulama bir "Kapsam başarısız oldu" iletisi gönderir. Tüm kapsamlı eylemleri başarılı olursa, mantıksal uygulama "Kapsam başarılı" iletisi gönderir.

!["Zamanlama – Recurrence" tetikleyecek](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örnek takip etmek için bu öğeler gerekir:

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcıdan gelen e-posta hesabı. Bu örnek, Outlook.com kullanır. Farklı bir sağlayıcı kullanırsanız, genel akış aynı kalır, ancak UI farklı görünür.

* Bing Haritalar anahtarı. Bu anahtarı almak için bkz: <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">Bing Haritalar anahtarı alma</a>.

* Hakkındaki temel bilgileri [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-sample-logic-app"></a>Örnek mantıksal uygulama oluşturma

Böylece bir kapsam daha sonra ekleyebilirsiniz ilk olarak, bu örnek mantıksal uygulama oluşturun:

![Örnek mantıksal uygulama oluşturma](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* A **çizelgesi - yineleme** , belirttiğiniz bir zaman aralığında Bing Haritalar hizmetini denetler tetikleyici
* A **Bing Haritalar - Get yol** iki konum arasında seyahat süresi denetleyen eylemin
* Seyahat belirtilen seyahat zaman aralığını aşan olup olmadığını denetleyen bir koşul deyimi
* Belirtilen zamanınızı geçerli seyahat bundan e-posta gönderen bir eylem aşıyor

Herhangi bir zamanda mantıksal uygulamanızı kaydetme, genellikle çalışmalarınızı kaydedin.

1. Oturum <a href="https://portal.azure.com" target="_blank">Azure portal</a>, henüz yapmadıysanız. Boş bir mantıksal uygulama oluşturma.

2. Ekleme **çizelgesi - yinelenme** bu ayarlarla tetikleyici: **aralığı** = "1" ve **sıklığı** "Dakika" =

   !["Zamanlama – Recurrence" tetikleyecek](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Görsel olarak görünümünüzü basitleştirmek ve her eylemin ayrıntıları Tasarımcısı'nda gizlemek için bu adımları ilerlemeyi olarak her eylemin şekli daraltın.

3. Ekleme **Bing Haritalar - Get yol** eylem. 

   1. Bing Haritalar bağlantı yoksa, bir bağlantı oluşturmak için sorulur.

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Bağlantı Adı** | BingMapsConnection | Bağlantınıza bir ad verin. | 
      | **API Anahtarı** | <*your-Bing-Maps-key*> | Daha önce aldığınız Bing Haritalar anahtarını girin. | 
      ||||  

   2. Ayarlama, **Get yol** bu görüntüyü aşağıdaki tabloda gösterildiği gibi eylem:

      !["Bing Haritalar - Get yol" ayarlamak eylemi](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Bu parametreler hakkında daha fazla bilgi için bkz. [Rota hesaplama](https://msdn.microsoft.com/library/ff701717.aspx).

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Güzergah noktası 1** | <*Başlat*> | Rotanın kaynak girin. | 
      | **Güzergah noktası 2** | <*Bitiş*> | Rotanın hedef girin. | 
      | **Kaçının** | None | Otoyollar gibi rotada tolls, önlemek üzere öğeleri girin. Olası değerler için bkz: [bir rota hesaplamak](https://msdn.microsoft.com/library/ff701717.aspx). | 
      | **İyileştir** | timeWithTraffic | Uzaklık, süresiyle geçerli trafik bilgileri vb. gibi rota en iyi duruma getirmek için bir parametre seçin. Bu örnekte bu değeri kullanır: "timeWithTraffic" | 
      | **Mesafe birimi** | <*your-preference*> | Rota hesaplamak için uzaklık birimi girin. Bu örnekte bu değeri kullanır: "Mil" | 
      | **Seyahat modu** | Sürüş | Seyahat modu için yol girin. Bu örnekte bu değer "Driving" kullanır. | 
      | **Toplu Ulaşım Tarih-Saati** | None | Yalnızca aktarım modu için geçerlidir. | 
      | **Geçiş türü tarih türü** | None | Yalnızca aktarım modu için geçerlidir. | 
      ||||  

4. Geçerli seyahat trafiği ile belirtilen bir zaman aralığını aşan olup olmadığını denetle için bir koşul ekleyin. Bu örnekte, bu görüntüyü altındaki adımları izleyin:

   ![Koşul oluşturma](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Bu açıklama koşuluyla yeniden adlandırın: **trafiğin belirtilen süreden daha zamanında**

   2. Parametre listesinden **seyahat süresi trafiği** saniye alan. 

   3. Bu işleç karşılaştırma işleci için seçin: **büyüktür:**

   4. Karşılaştırma değeri için girin **600**, olduğu saniye ve 10 dakika ile eşdeğerdir.

5. Koşulunun içinde **true ise** dal, e-posta sağlayıcınız için bir "e-posta Gönder" eylemini ekleyin. Bu eylem ayrıntılarıyla bu görüntüyü altındaki tabloda gösterildiği gibi ayarlayın:

   !["True ise" Ekle "bir e-posta Gönder" eylemi için şube](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. İçin **için** alanında, test amacıyla e-posta adresinizi girin.

   2. İçin **konu** alanında, bu metin girin:

      ```Time to leave: Traffic more than 10 minutes```

   3. İçin **gövde** alan, bir boşluk bu metin girin: 

      ```Travel time: ```

      İmleci görünür ancak **gövde** alan, dinamik içerik listesi kalır açık bu noktada kullanılabilir herhangi bir parametre seçebilirsiniz.

   4. Dinamik içerik listesinde **İfade**’yi seçin.

   5. Bulmak ve seçmek **div ()** işlevi.

   6. İmleci işlevin parantez içinde olsa da, seçin **dinamik içerik** ekleyebilirsiniz böylece **trafiği süresi trafiği** parametresi sonraki.

   7. Altında **Get yol** dinamik parametreyi listeden seçin **trafiği süresi trafiği** alan.

      !["Trafik süresi trafiği" seçin](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   8. JSON biçimine alan çözümler sonra eklemek bir **virgülle** (```,```) sayının ```60``` böylece değeri Dönüştür **trafiği süresi trafiği** dakika saniyeye gelen. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      İfadeniz şimdi aşağıdaki gibi görünür:

      ![Bitiş ifade](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   9. Seçtiğinizden emin olun **Tamam** tamamladığınızda.

  10. İfade çözümler sonra bu metni başında boşluk ile ekleyin: ``` minutes```
  
      **Gövde** alan şimdi aşağıdaki gibi görünür:

      ![Tamamlandı "Body" alanı](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)

6. Mantıksal uygulamanızı kaydedin.

Ardından, böylece belirli eylemleri gruplamak ve bunların durumunu değerlendirmek kapsam ekleyin.

## <a name="add-a-scope"></a>"Kapsam" ekle

1. Henüz yapmadıysanız, mantıksal uygulamanızı mantığı Uygulama Tasarımcısı'nda açın. 

2. Bir kapsam istediğiniz iş akışı konumunda ekleyin. Örneğin:

   * Mantıksal uygulama iş akışı içinde varolan adımlar arasındaki bir kapsam eklemek için oku kapsamı eklemek istediğiniz işaretçiyi. 
   Seçin **artı** (**+**) > **bir kapsam eklemek**.

     !["Kapsam" ekle](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

     Mantıksal uygulamanızı sonundaki akışınıza sonuna bir kapsam eklemek istediğinizde belirleyin **+ yeni adım** > **... Daha fazla** > **bir kapsam eklemek**.

3. Şimdi adımlar ekleyebilir veya kapsam içinde çalıştırmak istediğiniz varolan adımları sürükleyin. Bu örnekte, bu eylemleri kapsam içine sürükleyin:
      
   * **Rota Al**
   * **Trafiğin belirtilen süreden daha zamanında**, her ikisini de içeren **true** ve **false** dalları

   Mantıksal uygulamanız şimdi aşağıdaki gibi görünür:

   ![Eklenen kapsamı](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

4. Kapsam altında kapsamın durumu denetleyen bir koşul ekleyin. Bu açıklama koşuluyla yeniden adlandırın: **kapsam başarısız olursa**

   ![Kapsam durumunu denetlemek için koşul Ekle](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
5. Kapsamın durumu eşit olup olmadığını denetler bu deyim derleme `Failed` veya `Aborted`.

   ![Kapsamın durumunu denetler ifade ekleyin](./media/logic-apps-control-flow-run-steps-group-scopes/build-expression-check-scope-status.png)

   Ya da bu ifade metin olarak girmeyi seçerseniz **Gelişmiş modda Düzenle**.

   ```@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')```

6. İçinde **true ise** ve **false ise** dal, ekleyebilir, gerçekleştirmek istediğiniz eylemleri Örneğin, e-posta veya bir ileti gönderin.

   ![Kapsamın durumunu denetler ifade ekleyin](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

7. Mantıksal uygulamanızı kaydedin.

Tamamlanmış mantıksal uygulamanızı şimdi genişletilmiş tüm şekiller ile aşağıdaki gibi görünür:

![Tamamlanmış mantıksal uygulama kapsamlı](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Çalışmanızı test

Tasarımcı araç çubuğunda seçin **çalıştırmak**. Tüm kapsamlı eylemleri başarılı olursa, "Kapsam başarılı oldu" iletisi alıyorum. Kapsamlı eylemleri işinde başarılı olursa, "Kapsam başarısız oldu" iletisini alırsınız. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON tanımı

Kod görünümünde çalışıyorsanız, kapsam yapısı mantığı uygulamanızın JSON tanımında yerine tanımlayabilirsiniz. Örneğin, tetikleyici ve önceki mantıksal uygulama eylemler için JSON tanımını şöyledir:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    },
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
            "Body": "Scope failed",
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
              "Body": "None",
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
    "expression": "@equals('@result(''Scope'')[0][''status'']', 'Failed, Aborted')",
    "runAfter": {
      "Scope": [
        "Succeeded"
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
      "If_traffic_time_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'], 60)} minutes",
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
        "expression": "@greater(body('Get_route')?['travelDurationTraffic'], 600)",
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Gönderme veya özellikleri ve öneriler oylamak için ziyaret [Azure Logic Apps kullanıcı geri bildirim sitesi](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Sonraki adımlar

* [Bir koşula göre (koşullu deyimler) adımları çalıştırın](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlerini (switch deyimleri) temel alan adımları çalıştırın](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Çalıştırma ve (döngüler) arasındaki adımları yineleyin](../logic-apps/logic-apps-control-flow-loops.md)
* [Çalıştırmak veya paralel adımları (dal) birleştirme](../logic-apps/logic-apps-control-flow-branches.md)
