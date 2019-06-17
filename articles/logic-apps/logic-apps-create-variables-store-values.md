---
title: Değerleri - Azure Logic Apps kaydetmek için değişkenler oluşturun | Microsoft Docs
description: Kaydetme ve Azure Logic Apps'te değişkenleri oluşturarak değerleri yönetme
services: logic-apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.topic: article
ms.date: 05/30/2018
ms.service: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: e525e5584e4835b0f2b73203c818c3f799b77cf5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61004594"
---
# <a name="create-variables-for-saving-and-managing-values-in-azure-logic-apps"></a>Kaydetme ve Azure Logic Apps değerleri yönetmek için değişkenleri oluşturma

Bu makale, depolamak ve değerleri ile mantıksal uygulamanızda değişkenleri oluşturarak iş nasıl gösterir. Örneğin, değişkenleri, bir döngü çalışan sayısını sayma yardımcı olabilir. Ne zaman bir dizi yineleme veya belirli bir öğe için bir dizi denetimi, dizideki tüm öğeler için dizin numarasını başvurmak için bir değişken kullanabilirsiniz. 

Tamsayı, kayan noktalı sayı, Boole, dize, dizi ve nesne gibi veri türleri için değişkenler oluşturabilirsiniz. Bir değişken oluşturduktan sonra Örneğin diğer görevleri gerçekleştirebilirsiniz:

* GET veya değişkenin değeri başvurusu.
* Artırma veya değişkeni sabit bir değer olarak da bilinen azaltma *artışı* ve *azaltma*.
* Değişkenine farklı bir değer atayın.
* Ekleme veya *ekleme* değişkenin değeri olarak bir dize veya dizideki son zaman.

Değişkenleri, mevcut ve kendilerini oluşturan yalnızca mantıksal uygulama örneği içinde geneldir. Ayrıca, bir mantıksal uygulama örneği içindeki herhangi bir döngü yinelemesi arasında kalıcı. Bir değişken başvururken değişkenin adı belirteci, bir eylemin çıkışlarına başvuruyor için her zamanki şekilde değil eylemin adı olarak kullanın. 

> [!IMPORTANT]
> Varsayılan olarak, Döngülerde "Foreach" döngüsünü paralel olarak çalıştırın. Döngülerde değişkenleri kullandığınızda döngünün [sırayla](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop) değişkenleri tahmin edilebilir sonuçlar döndürülmesi için. 

Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede takip etmek için gereksinim duyduğunuz öğeleri şunlardır:

* Bir değişken oluşturmak için istediğiniz mantıksal uygulama 

  Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* A [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak 

  Mantıksal uygulamanızı, oluşturmak ve değişkenlerle çalışmak için Eylemler ekleyebilmeniz için önce bir tetikleyici ile başlamalıdır.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Değişken başlat

Bir değişken oluşturun ve kendi veri türüne ve ilk değer - tüm mantıksal uygulamanızda bir eylem içinde bildirin. Yalnızca genel düzeyde, kapsamları, koşullar ve döngüler içinde değil değişkenleri bildirebilirsiniz. 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnek, Azure portalı ve bir mantıksal uygulama ile var olan bir tetikleyici kullanır.

2. Mantıksal uygulamanızda bir değişkeni eklemek istediğiniz adımı altında aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-create-variables-store-values/add-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "değişkenler" girin. Eylem listesinden seçim **değişkenler - değişken Başlat**.

   ![Eylem seçin](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

4. Değişkeninizin bu bilgileri sağlayın:

   | Özellik | Gereklidir | Value |  Açıklama |
   |----------|----------|-------|--------------|
   | Name | Evet | <*değişken adı*> | Artış değişkeni adı | 
   | Type | Evet | <*değişken türü*> | Değişken için veri türü | 
   | Value | Hayır | <*Başlangıç değeri*> | Değişkeninizin ilk değeri <p><p>**İpucu**: Başlangıç değeri her zaman değişkeninizin bilmesi isteğe bağlı olsa da, bu değeri en iyi uygulama ayarlayın. | 
   ||||| 

   ![Değişken başlat](./media/logic-apps-create-variables-store-values/initialize-variable.png)

5. Şimdi, istediğiniz eylemleri ekleme devam edin. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **değişken Başlat** eylem görünür JavaScript nesne gösterimi (JSON) biçiminde, mantıksal uygulama tanımı içinde:

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

*String değişkeni*

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

*İle tamsayı dizisi*

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

*Dizeler içeren dizi*

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

## <a name="get-the-variables-value"></a>Değişken değerini alın

Almak veya değişkenin içeriklerini başvurmak için de kullanabilirsiniz [variables() işlevi](../logic-apps/workflow-definition-language-functions-reference.md#variables) mantıksal Uygulama Tasarımcısı ve kod görünümü Düzenleyicisi.
Bir değişken başvururken değişkenin adı belirteci, bir eylemin çıkışlarına başvuruyor için her zamanki şekilde değil eylemin adı olarak kullanın. 

Örneğin, bu ifade öğeleri dizisi değişkeninden alır [bu makalede daha önce oluşturduğunuz](#append-value) kullanarak **variables()** işlevi. **String()** işlevi, değişkenin içeriklerini dize biçiminde döndürür: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Artış değişkeni 

Artırmak için veya *artışı* sabit bir değere göre değişken Ekle **değişkenler - değişken artırma** mantıksal uygulamanız için eylem. Bu eylem yalnızca tamsayı ve kayan değişkenler ile çalışır.

1. Logic Apps Tasarımcısı'nda mevcut bir değişken artırmak için istediğiniz adımı altında seçin **yeni adım** > **Eylem Ekle**. 

   Örneğin, bu mantıksal uygulama zaten bir tetikleyici ve bir değişken oluşturan eylem bulunur. Bu nedenle, bu adımları altında yeni bir eylem ekleyin:

   ![Eylem ekle](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Var olan adımlar arasında bir eylem eklemek için artı işaretini (+) gözükmesi farenizi bağlanan okun üzerine taşıyın. Artı işaretini seçin ve ardından **Eylem Ekle**.

2. Arama kutusuna filtreniz olarak "artış değişkeni" girin. Eylemler listesinde **değişkenler - değişken artırma**.

   !["Artış değişkeni" eylemini seçin](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

3. Değişkeninizin artırma için bu bilgileri sağlayın:

   | Özellik | Gereklidir | Value |  Açıklama |
   |----------|----------|-------|--------------|
   | Name | Evet | <*değişken adı*> | Artış değişkeni adı | 
   | Value | Hayır | <*değeri Artır*> | Değişken değerini artırmak için kullanılan değer. Varsayılan değer biridir. <p><p>**İpucu**: Değişkeninizin artırma için söz konusu değeri her zaman haberdar olmak için isteğe bağlı olsa da, bu değeri en iyi uygulama ayarlayın. | 
   |||| 

   Örneğin: 
   
   ![Artış değeri örneği](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

4. Tasarımcı araç çubuğunda, işiniz bittiğinde seçin **Kaydet**. 

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **artış değişkeni** eylem görünür JSON biçimindedir, mantıksal uygulama tanımı içinde:

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

## <a name="example-create-loop-counter"></a>Örnek: Döngü sayacı oluşturun

Değişkenleri, bir döngü çalışan sayısını sayma için yaygın olarak kullanılır. Bu örnekte, nasıl oluşturabileceğinizi ve e-posta eklerini sayan bir döngü oluşturarak bu görev için değişkenleri kullanma gösterilmektedir.

1. Azure portalında boş bir mantıksal uygulama oluşturun. Yeni e-posta ve ekleri için denetleyen bir tetikleyici ekleme. 

   Bu örnekte Office 365 Outlook tetikleyicisi **yeni bir e-posta geldiğinde**. 
   Bu tetikleyici yalnızca e-posta ek içerdiğinde ateşlenmesine ayarlayabilirsiniz.
   Ancak, yeni e-postalar Outlook.com Bağlayıcısı gibi ekleriyle denetleyen bir bağlayıcıyı kullanabilirsiniz.

2. Tetikleyici seçin **Gelişmiş Seçenekleri Göster**. Tetikleyici için ekleri denetleyin ve bu eklerin mantıksal uygulamanızın iş akışınıza aktarmak için seçin **Evet** bu özellikler için:
   
   * **Eki Var** 
   * **Ekleri Dahil Et** 

   ![Arayın ve ekleri dahil et](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

3. Ekleme [ **değişken Başlat** eylem](#create-variable). Adlı bir tamsayı değişkeni oluşturma **sayısı** sıfır ile başlangıç değeri.

   !["Değişken Başlat" eylemini ekleme](./media/logic-apps-create-variables-store-values/initialize-variable.png)

4. Her ek geçiş yapmak için ekleme bir *her* seçerek döngü **yeni adım** > **daha fazla** > **Ekle bir heriçin**.

   ![Bir "for each" döngüsü ekleyin](./media/logic-apps-create-variables-store-values/add-loop.png)

5. Döngüde içine tıklayın **önceki adımlardan bir çıkış seçin** kutusu. Dinamik içerik listesi göründüğünde seçin **ekleri**. 

   !["Ekler"i seçin](./media/logic-apps-create-variables-store-values/select-attachments.png)

   **Ekleri** alan döngünüzü tetikleyicinin çıktısından e-posta ekleri olan bir dizi iletir.

6. "For each" döngüsü seçin **Eylem Ekle**. 

   !["Eylem Ekle"'i seçin](./media/logic-apps-create-variables-store-values/add-action-2.png)

7. Arama kutusuna filtreniz olarak "artış değişkeni" girin. Eylem listesinden seçim **değişkenler - değişken artırma**.

   > [!NOTE]
   > Emin **artış değişkeni** eylemi döngü içinde görüntülenir. Eylemi döngü dışına görünüyorsa, döngüye eylem sürükleyin.

8. İçinde **artış değişkeni** eylem gelen **adı** listesinden **sayısı** değişkeni. 

   !["Count" değişken seçin](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

9. Döngü altında ek sayısı gönderen herhangi bir eylem ekleyin. Değeri dahil **sayısı** değişken, örneğin: 

   ![Sonuçları gönderen bir eylem ekleme](./media/logic-apps-create-variables-store-values/send-email-results.png)

10. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

1. Mantıksal uygulamanızı etkinleştirilmemişse, mantıksal uygulama menüsünde seçin **genel bakış**. Araç çubuğunda **etkinleştirme**. 

2. Mantıksal Uygulama Tasarımcısı araç çubuğunda **çalıştırma**. Bu adım, mantıksal uygulamanızı el ile başlatır.

3. Bu örnekte kullandığınız e-posta hesabı için bir veya daha fazla ek içeren bir e-posta gönderin.

   Bu adım, oluşturur ve mantıksal uygulamanızın iş akışı için bir örneğini çalıştıran mantıksal uygulamanın tetikleyicisi tetikler.
   Sonuç olarak, mantıksal uygulama, ileti veya ek sayısı, gönderilen e-postada gösteren bir e-posta gönderir.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, "for each" döngüsü ile görünümünü işte **artış değişkeni** eylemi JSON biçimindedir, mantıksal uygulama tanımınızı içinde.

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

Azaltmak için veya *azaltma* sabit değerli bir değişken için adımları izleyin [bir değişken artırma](#increment-value) bulun ve seçin **değişkenler - azaltma değişkeni**eylemi yerine. Bu eylem yalnızca tamsayı ve kayan değişkenler ile çalışır.

Özellikleri şunlardır **azaltma değişkeni** eylem:

| Özellik | Gereklidir | Value |  Açıklama |
|----------|----------|-------|--------------|
| Name | Evet | <*değişken adı*> | Azaltma değişkeni adı | 
| Value | Hayır | <*değeri Artır*> | Azaltma değişkeni için değer. Varsayılan değer biridir. <p><p>**İpucu**: Azaltma özel değeri, değişken her zaman bilmesi isteğe bağlı olsa da, bu değeri en iyi uygulama ayarlayın. | 
||||| 

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **azaltma değişkeni** JSON biçimindedir, mantıksal uygulama tanımınızı içinde eylem görünür.

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

Mevcut bir değişken için farklı bir değer atamak için adımları izleyin. [bir değişken artırma](#increment-value) , hariç: 

1. Bulmak ve seçmek **değişkenler - değişken Ayarla** eylemi yerine. 

2. Değişken adı ve atamak istediğiniz değerini belirtin. Hem yeni değeri hem de değişken aynı veri türüne sahip olmalıdır.
Bu eylem bir varsayılan değer olmadığı için gerekli bir değerdir. 

Özellikleri şunlardır **değişken Ayarla** eylem:

| Özellik | Gereklidir | Value |  Açıklama | 
|----------|----------|-------|--------------| 
| Name | Evet | <*değişken adı*> | Değiştirmek değişken adı | 
| Value | Evet | <*Yeni değer*> | Değişken atamak istediğiniz değer. Aynı veri türüne sahip olmalıdır. | 
||||| 

> [!NOTE]
> Artırma veya azaltma değişkenleri olduğunuz sürece, döngü içinde değişkenleri değiştirme *olabilir* döngüler paralel veya aynı anda, varsayılan olarak çalıştığı için beklenmeyen sonuçlar oluşturabilir. Bu durumlarda, sırayla çalışması döngünüzü ayarlamayı deneyin. Örneğin, döngü içinde değişken değeri başvurusu ve başlangıç ve bitiş Bu döngü örneğinin aynı değerde beklediğiniz istediğinizde, döngünün nasıl çalıştığını değiştirmek üzere aşağıdaki adımları izleyin: 
>
> 1. Döngünün sağ üst köşedeki üç nokta (...) düğmesini seçin ve ardından **ayarları**.
> 
> 2. Altında **eşzamanlılık denetimi**, değiştirme **Varsayılanı geçersiz kıl** ayarını **üzerinde**.
>
> 3. Sürükleme **paralellik derecesi** kaydırıcısını **1**.

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **değişken Ayarla** JSON biçimindedir, mantıksal uygulama tanımınızı içinde eylem görünür. Bu örnek, "Sayı" değişkenin geçerli değeri için başka bir değerle değiştirir. 

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

## <a name="append-to-variable"></a>Değişkenine Ekle

Dizeler veya diziler depolamak için değişkenleri, eklediğiniz veya *ekleme* son öğesi bu dizeler veya diziler olarak bir değişkenin değeri. İçin adımları takip edebilirsiniz [bir değişken artırma](#increment-value) dışında bunun yerine aşağıdaki adımları izleyin: 

1. Bulup, değişkeni bir dize veya bir dizi olduğuna bağlı olarak aşağıdaki eylemlerden birini seçin: 

   * **Değişkenler - dize değişkenine Ekle**
   * **Değişkenler - dizi değişkenine Ekle** 

2. Son öğe dizisi veya dize olarak eklenecek değeri belirtin. 
   Bu değer gereklidir. 

Özellikleri şunlardır **ekleyin...**  eylemler:

| Özellik | Gereklidir | Value |  Açıklama | 
|----------|----------|-------|--------------| 
| Name | Evet | <*değişken adı*> | Değiştirmek değişken adı | 
| Value | Evet | <*değer ekleme*> | Herhangi bir tür olabilen, eklemek istediğiniz değer | 
|||||  

Kod Görünümü düzenleyicisine Tasarımcısı'ndan geçiş yapıyorsanız, işte yol **dizi değişkenine Ekle** JSON biçimindedir, mantıksal uygulama tanımınızı içinde eylem görünür.
Bu örnek, bir dizi değişkenini oluşturur ve son öğe dizisi olarak başka bir değer ekler. Sonuç bu dizi içeren güncelleştirilmiş bir değişkendir: `[1,2,3,"red"]` 

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
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
