---
title: Değerler - Azure Logic Apps kaydetmek için değişkenler oluşturun | Microsoft Docs
description: Kaydetme ve Azure Logic Apps içinde değişkenleri oluşturarak değerleri yönetme
services: logic-apps
author: ecfan
manager: cfowler
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: cc464edf416a2b036d84e65e05810104d2706041
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655598"
---
# <a name="create-variables-for-saving-and-managing-values-in-azure-logic-apps"></a>Kaydetme ve Azure Logic Apps değerlerde yönetmek için değişkenleri oluşturma

Bu makalede nasıl depolamak ve değişkenleri oluşturarak mantıksal uygulamanızı değerlerle çalışma gösterilmektedir. Örneğin, değişkenleri, bir döngü çalışan sayısı saymak yardımcı olabilir. Ne zaman bir dizi yineleme ya da belirli bir öğe için bir dizi denetimi, her bir dizi öğesi için dizin numarasını başvurmak için bir değişkeni kullanabilirsiniz. 

Tamsayı, kayan nokta, boolean, dize, dizi ve nesne gibi veri türleri için değişkenleri oluşturabilirsiniz. Bir değişken oluşturduktan sonra Örneğin diğer görevleri gerçekleştirebilirsiniz:

* Alın veya değişken değerinin başvuru.
* Değişkeni sabit bir değer olarak da bilinen azaltabileceğinden *artırma* ve *azaltma*.
* Farklı bir değer değişkenine atayın.
* INSERT veya *sona* değişkenin değeri olarak en son ne zaman bir dize veya dizi.

Değişkenleri var ve kendilerini oluşturan yalnızca mantığı uygulama örneği içinde geneldir. Ayrıca, herhangi bir mantıksal uygulama örneğinin içine döngü yineleme arasında kalır. Bir değişken başvururken belirteci olarak bir eylemin çıkış başvurmak için her zamanki gibi olan eylemin adı değil, değişkenin adını kullanın.

Henüz bir Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede izlemek için gereksinim duyduğunuz öğeleri şunlardır:

* Bir değişken oluşturmak istediğiniz mantıksal uygulama 

  Logic apps yeniyseniz, gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* A [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak 

  Oluşturma ve değişkenlerle çalışma Eylemler eklemeden önce mantıksal uygulamanızı tetikleyici ile başlamalıdır.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Değişken başlat

Bir değişken oluşturun ve kendi veri türü ve ilk değer - tüm mantıksal uygulamanızı bir eylemde içinde bildirin. Yalnızca genel düzeyde, kapsamları, koşullar ve döngüler içinde olmayan değişkenleri bildirebilirsiniz. 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portal</a> veya Visual Studio, mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnek, Azure portalı ve bir mantıksal uygulama ile varolan bir tetikleyicinin kullanır.

2. Bir değişkeni eklemek istediğiniz adımı altında mantıksal uygulamanızı aşağıdaki adımlardan birini tamamlayın: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem Ekle](./media/logic-apps-create-variables-store-values/add-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı (+) görünmesi bağlanan oku farenizi taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna "değişkenleri", filtre olarak girin. Eylemler listesinden **değişkenleri - Initialize değişkeni**.

   ![Bir eylem seçin](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

4. Değişkeniniz için bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer |  Açıklama |
   |----------|----------|-------|--------------|
   | Ad | Evet | <*değişken adı*> | Artırmak bir değişken adı | 
   | Tür | Evet | <*değişken türü*> | Değişken veri türü | 
   | Değer | Hayır | <*Başlangıç değeri*> | Değişkeninizin ilk değeri <p><p>**İpucu**: Başlangıç değeri, değişken için her zaman bilmesi isteğe bağlı olsa da bu değer en iyi uygulama olarak ayarlayın. | 
   ||||| 

   ![Değişken başlat](./media/logic-apps-create-variables-store-values/initialize-variable.png)

5. Şimdi, istediğiniz eylemleri ekleme devam edin. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **Initialize değişkeni** JavaScript nesne gösterimi (JSON) biçiminde, mantıksal uygulama tanımını içinde eylem görüntülenir:

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

Diğer bir değişken türleri için örnekler şunlardır:

*Dize değişkeni*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Boolean değişkeni*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Dizi tamsayılı*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Dizi dizeler*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>Değişken değeri Al

Almak veya değişkenin içeriği başvurmak için de kullanabilirsiniz [variables() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#variables) mantığı Uygulama Tasarımcısı'nı ve kod görünümü Düzenleyicisi.
Bir değişken başvururken belirteci olarak bir eylemin çıkış başvurmak için her zamanki gibi olan eylemin adı değil, değişkenin adını kullanın. 

Örneğin, bu deyim öğeleri dizisi değişkeninden alır [bu makalede daha önce oluşturduğunuz](#append-value) kullanarak **variables()** işlevi. **String()** işlevi değişkenin içeriği dize biçiminde döndürür: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Artış değişkeni 

Artırmak için veya *artırma* değişkeni sabit bir değer tarafından eklemek **değişkenleri - artışı değişkeni** mantığı uygulamanıza eylem. Bu eylem yalnızca tamsayı ve kayan nokta değişkenleri ile çalışır.

1. Mantıksal Uygulama Tasarımcısı'nda mevcut bir değişken artırmak istediğiniz adımı altında seçin **yeni adım** > **Eylem Ekle**. 

   Örneğin, bu mantıksal uygulama zaten bir tetikleyici ve bir değişkeni oluşturan bir eylem içeriyor. Bu nedenle, bu adımları altında yeni bir eylem ekleyin:

   ![Eylem Ekle](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Var olan adımları arasında bir eylem eklemek için artı (+) görünmesi bağlanan oku farenizi taşıyın. Artı işaretini seçin ve ardından **Eylem Ekle**.

2. Arama kutusuna "artışı değişkeni", filtre olarak girin. Eylemler listesinde **değişkenleri - artışı değişkeni**.

   !["Artışı değişkeni" eylemini seçin](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

3. Değişkeniniz artırma için bu bilgileri sağlayın:

   | Özellik | Gerekli | Değer |  Açıklama |
   |----------|----------|-------|--------------|
   | Ad | Evet | <*değişken adı*> | Artırmak bir değişken adı | 
   | Değer | Hayır | <*artış değeri*> | Değişkeni artırma için kullanılan değer. Varsayılan değer biridir. <p><p>**İpucu**: değişkeniniz artırma söz konusu değeri her zaman bilmesi isteğe bağlı olsa da bu değer en iyi uygulama olarak ayarlayın. | 
   |||| 

   Örneğin: 
   
   ![Artış değeri örneği](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

4. Tasarımcı araç çubuğunda bitirdiniz seçin **kaydetmek**. 

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **artırma değişkeni** JSON biçiminde, mantıksal uygulama tanımını içinde eylem görüntülenir:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Örnek: döngü sayacı oluşturun

Değişkenler, bir döngü çalışan sayısı sayım için yaygın olarak kullanılır. Bu örnek nasıl oluşturmak ve bir e-posta ekleri sayılarını bir döngü oluşturarak değişkenleri bu görev için kullanacağınız gösterir.

1. Azure portalında boş mantıksal uygulama oluşturma. Yeni e-posta ve eklerin denetleyen bir tetikleyici ekleyin. 

   Bu örnek için Office 365 Outlook tetikleyici kullanır **yeni bir e-posta geldiğinde**. 
   Yalnızca e-posta ekleri olduğunda tetiklenecek Bu tetikleyici ayarlayabilirsiniz.
   Ancak, yeni e-posta ekleri gibi Outlook.com Bağlayıcısı'nı denetler bağlayıcısını kullanabilirsiniz.

2. Tetikleyici seçin **Gelişmiş Seçenekleri Göster**. Ekler için denetleyin ve bu ekleri mantığı uygulamanızın akışına geçirmek tetiklemesini sağlamak için seçin **Evet** bu özellikler için:
   
   * **Eki Var** 
   * **Ekleri Dahil Et** 

   ![Denetlemek ve ekleri içerir](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

3. Ekleme [ **Initialize değişkeni** eylem](#create-variable). Adlı bir tamsayı değişken oluşturma **sayısı** değeri sıfır ile başlatın.

   !["Başlatma değişkeni" için Eylem Ekle](./media/logic-apps-create-variables-store-values/initialize-variable.png)

4. Her ek arasında geçiş yapmak için ekleme bir *her* seçerek döngü **yeni adım** > **daha fazla** > **Ekle bir heriçin**.

   !["İçin her" döngü ekleme](./media/logic-apps-create-variables-store-values/add-loop.png)

5. İçinde bir döngüde tıklatın **bir çıktı önceki adımları seçin** kutusu. Dinamik içerik listesi göründüğünde, seçin **ekleri**. 

   !["Ekler"i seçin](./media/logic-apps-create-variables-store-values/select-attachments.png)

   **Ekleri** alan döngü tetikleyici çıktısından e-posta ekleri olan bir dizi geçirir.

6. "İçin"her döngüde seçin **Eylem Ekle**. 

   !["Eylem" Ekle](./media/logic-apps-create-variables-store-values/add-action-2.png)

7. Arama kutusuna "artışı değişkeni", filtre olarak girin. Eylemler listesinden **değişkenleri - artışı değişkeni**.

   > [!NOTE]
   > Emin olun **artırma değişkeni** eylem döngünün içinde görüntülenir. Eylemin döngü dışında görünürse, eylem döngüye sürükleyin.

8. İçinde **artırma değişkeni** eylem, gelen **adı** listesinde **sayısı** değişkeni. 

   !["Count" değişken seçin](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

9. Döngü altında ek sayısı gönderen herhangi bir eylem ekleyin. Değeri, eylemde dahil **sayısı** değişken, örneğin: 

   ![Sonuçları gönderdiği Eylem Ekle](./media/logic-apps-create-variables-store-values/send-email-results.png)

10. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulamanızı etkin değil mantığı uygulama menünüzde kaldığınızda **genel bakış**. Araç çubuğunda seçin **etkinleştirmek**. 

2. Mantıksal Uygulama Tasarımcısı araç çubuğundaki seçin **çalıştırmak**. Bu adım, mantıksal uygulamanızı el ile başlar.

3. Bu örnekte, kullandığınız e-posta hesabı için bir veya daha fazla ek içeren bir e-posta gönderin.

   Bu adım oluşturur ve mantığı uygulamanızın iş akışı için bir örneğini çalıştıran mantığı uygulamanın tetikleyici ateşlenir.
   Sonuç olarak, mantıksal uygulama, bir ileti veya ek sayısı, gönderilen e-postada gösterir e-posta gönderir.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, "her biri için" döngü ile görünümünü işte **artırma değişkeni** JSON biçiminde, logic app tanımı içinde eylem.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Değişkeni azalt

Azaltmak için veya *azaltma* değişkeni sabit bir değer tarafından için adımları izleyin [bir değişken artırma](#increment-value) bulmak ve seçmek dışında **değişkenleri - azaltma değişkeni**eylem yerine. Bu eylem yalnızca tamsayı ve kayan nokta değişkenleri ile çalışır.

Özelliklerini işte **azaltma değişkeni** eylem:

| Özellik | Gerekli | Değer |  Açıklama |
|----------|----------|-------|--------------|
| Ad | Evet | <*değişken adı*> | Düşürmek bir değişken adı | 
| Değer | Hayır | <*artış değeri*> | Azaltma değişkeni için değer. Varsayılan değer biridir. <p><p>**İpucu**: azaltma için özel değer her zaman değişkeniniz bilmesi isteğe bağlı olsa da bu değer en iyi uygulama olarak ayarlayın. | 
||||| 

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **azaltma değişkeni** eylem JSON biçiminde, mantıksal uygulama tanımını içinde görüntülenir.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```


<a name="assign-value"></a>

## <a name="set-variable"></a>Değişken ayarla

Mevcut bir değişken için farklı bir değer atamak için adımları izleyin [bir değişken artırma](#increment-value) dışındaki doğrulayın: 

1. Bulmak ve seçmek **değişkenleri - değişken Ayarla** eylem yerine. 

2. Değişken adı ve atamak istediğiniz değeri belirtin. Yeni değer ve değişken aynı veri türüne sahip olmalıdır.
Bu eylem bir varsayılan değeri yoktur çünkü gerekli bir değerdir. 

Özelliklerini işte **değişken Ayarla** eylem:

| Özellik | Gerekli | Değer |  Açıklama | 
|----------|----------|-------|--------------| 
| Ad | Evet | <*değişken adı*> | Değiştirmek bir değişken adı | 
| Değer | Evet | <*Yeni değer*> | Değişkeni atamak istediğiniz değer. Aynı veri türüne sahip olmalıdır. | 
||||| 

> [!NOTE]
> Artırma veya azaltma değişkenleri değilseniz, döngüler içinde değişkenlerini değiştirme *olabilir* döngüler paralel veya eşzamanlı olarak, varsayılan olarak çalıştığı için beklenmeyen sonuçlar oluşturun. Bu durumlarda, sıralı olarak çalıştırmak için döngü ayarlamayı deneyin. Örneğin, döngü içinde değişken değeri başvuru ve başlangıç ve bitiş Bu döngü örneğinin aynı değerinde beklediğiniz istediğinizde, döngü nasıl çalışacağını değiştirmek için aşağıdaki adımları izleyin: 
>
> 1. Döngünün sağ üst köşesinde, üç nokta (...) düğmesini seçin ve ardından **ayarları**.
> 
> 2. Altında **eşzamanlılık denetimi**, değiştirme **geçersiz kılma varsayılan** ayarını **üzerinde**.
>
> 3. Sürükleme **paralellik derecesi** kaydırıcıyı **1**.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **değişken Ayarla** eylem JSON biçiminde, mantıksal uygulama tanımını içinde görüntülenir. Bu örnekte "Count" değişkenin geçerli değeri başka bir değer olarak değiştirir. 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Değişkeni ekleme

Dizeler veya diziler depolama değişkenleri ekleyebilirsiniz veya *ekleme* bir değişkenin değeri olarak bu dizeler veya diziler son öğenin. İçin adımları izleyebilirsiniz [bir değişken artırma](#increment-value) dışında bunun yerine aşağıdaki adımları izleyin: 

1. Bulma ve, değişken bir dize veya dizi olduğuna bağlı olarak bu eylemlerden birini seçin: 

  * **Değişkenleri - dize değişkenine ekleme**
  * **Değişkenleri - dizi değişkeni ekleme** 

2. Dize veya dizinin son öğesi olarak eklenecek değeri belirtin. Bu değer gereklidir. 

Özelliklerini işte **sonuna...**  eylemler:

| Özellik | Gerekli | Değer |  Açıklama | 
|----------|----------|-------|--------------| 
| Ad | Evet | <*değişken adı*> | Değiştirmek bir değişken adı | 
| Değer | Evet | <*Append değeri*> | Herhangi bir tür olan eklemek istediğiniz değer | 
|||||  

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **dizi değişkeni Ekle** eylem JSON biçiminde, mantıksal uygulama tanımını içinde görüntülenir.
Bu örnek, bir dizi değişkeni oluşturur ve son öğesi dizi olarak başka bir değer ekler. Sonuç kümenizi, bu diziye içeren güncelleştirilmiş bir değişkenidir: `[1,2,3,"red"]` 

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcılar](../connectors/apis-list.md)
