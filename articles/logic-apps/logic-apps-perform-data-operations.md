---
title: Veri - Azure Logic Apps işlemleri | Microsoft Docs
description: Dönüştürme, yönetmek ve veri çıkışlarını ve biçimlerde Azure Logic Apps ile düzenleme
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.topic: article
ms.date: 07/30/2018
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 93c24f88fcd6a002493933ef71c5c80bd2ff8c10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62120188"
---
# <a name="perform-data-operations-in-azure-logic-apps"></a>Azure Logic Apps'te veri işlemleri

Bu makalede logic apps eylemleri bu görevler ve daha fazlasını ekleyerek verilerle nasıl çalışabileceğini gösterir:

* Dizilerden tabloları oluşturun.
* Bir koşula göre diğer dizilerden diziler oluşturun.
* Bu özellikler, iş akışınızı kolayca kullanabilmeniz için JavaScript nesne gösterimi (JSON) nesne özelliklerinden kullanıcı dostu belirteçleri oluşturun.

Burada istediğiniz eylemi bulamazsanız, birçok çeşitli gözatma deneyin [veri işleme işlevleri](../logic-apps/workflow-definition-language-functions-reference.md) , Logic Apps sağlar. 

Bu tablolar düzenlenir ve kullanma işlemleri üzerinde işlemleri çalışır, ancak her açıklama alfabetik olarak görünür kaynak veri türlerine göre özetler.

**Dizi eylemleri** 

Bu Eylemler, dizileri verilerle çalışmanıza yardımcı olur.

| Eylem | Açıklama | 
|--------|-------------| 
| [**CSV tablosu oluşturma**](#create-csv-table-action) | Bir diziden bir virgülle ayrılmış değer (CSV) tablo oluşturun. | 
| [**HTML tablosu oluşturma**](#create-html-table-action) | Bir diziden bir HTML tablosu oluşturun. | 
| [**Diziyi filtreleme**](#filter-array-action) | Belirtilen filtre veya koşul göre bir dizi bir dizi alt kümesi oluşturun. | 
| [**Katılın**](#join-action) | Bir dizideki tüm öğeler bir dize oluşturmak ve her öğeyi belirtilen karakteriyle ayırın. | 
| [**Seçin**](#select-action) | Belirtilen özelliklerinden farklı bir dizideki tüm öğeler için bir dizi oluşturun. | 
||| 

**JSON eylemleri**

Bu Eylemler, JavaScript nesne gösterimi (JSON) biçiminde verilerle çalışmanıza yardımcı olur.

| Eylem | Açıklama | 
|--------|-------------| 
| [**Oluşturan**](#compose-action) | Bir ileti veya dize, çeşitli veri türleri olan birden çok girişler oluşturun. Bu dize daha sonra tek bir giriş yerine tekrar tekrar aynı girişleri girme da kullanabilirsiniz. Örneğin, tek bir JSON ileti çeşitli girişler oluşturabilirsiniz. | 
| [**JSON Ayrıştır**](#parse-json-action) | Mantıksal uygulamalarınızı daha kolay özelliklerini kullanabilmeniz için özellikleri için kullanıcı dostu veri belirteçlerini JSON'da içerik oluşturun. | 
||| 

Daha karmaşık JSON dönüştürmeleri oluşturmak için bkz [şablonları Liquid ile JSON dönüştürmeleri Gelişmiş gerçekleştirme](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md).

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki örnekleri izlemek için bu öğeler gerekir:

* Azure aboneliği. Henüz Azure aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* İşlemi verilerle çalışmak için gerek duyduğunuz mantıksal uygulama 

  Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: İlk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* A [tetikleyici](../logic-apps/logic-apps-overview.md#logic-app-concepts) mantıksal uygulamanızı ilk adımı olarak 

  Veri işlemleri yalnızca eylem olarak kullanılabilir, böylece kullanabilmeniz için önce bu Eylemler, mantıksal uygulamanızın bir tetikleyici ile başlayın ve istediğiniz çıkışları oluşturmak için gereken diğer tüm eylemler içerir.

<a name="compose-action"></a>

## <a name="compose-action"></a>Oluştur eylemi

Bir JSON nesnesinde birden çok giriş gibi tek bir çıktı oluşturmak için kullanabileceğiniz **veri işlemleri - Oluştur** eylem. Tamsayılar, Boole, diziler, JSON nesnelerini ve herhangi bir yerel, Azure Logic Apps destekler; Örneğin, ikili ve XML türü gibi çeşitli türlerde girişlerinizi olabilir. Çıkış sonra izleyen Eylemler daha sonra kullanabileceğiniz **Compose** eylem. **Compose** eylem de kaydedebilir, mantıksal uygulamanızın iş akışı derleme sırasında tekrar tekrar aynı girişleri girmesini. 

Örneğin, bir JSON ileti kişi adlarını ve soyadlarını saklayan string değişkeni ve Halk yaş bulunduran bir tamsayı değişkeni gibi birden fazla değişken oluşturabilirsiniz. Burada, **Compose** eylem kabul eder, bu girişler:

`{ "age": <ageVar>, "fullName": "<lastNameVar>, <firstNameVar>" }`

Bu çıktıyı oluşturur:

`{"age":35,"fullName":"Owens,Sophie"}`

Bir örnek denemek için Logic Apps Tasarımcısı'nı kullanarak aşağıdaki adımları izleyin. Ya da kod görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **Compose** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - Compose](../logic-apps/logic-apps-data-operations-code-samples.md#compose-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve birkaç **değişken Başlat** eylemler. 
   Bu eylemler iki dize değişkeni ve bir tam sayı değişkeni oluşturmak için ayarlanır. Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-compose-action.png)

2. Çıktı oluşturmak istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-compose-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "Oluştur" girin. Eylem listesinden şu eylemi seçin: **Oluştur**

   !["Oluştur" eylemini seçin](./media/logic-apps-perform-data-operations/select-compose-action.png)

4. İçinde **girişleri** kutusunda, istediğiniz çıktı oluşturmak için girişleri sağlayın. 

   Bu örnekte, içine tıkladığınızda **girişleri** kutusu, dinamik içerik listesi görüntülenir daha önce oluşturulan değişkenleri seçebilmeniz için:

   ![Girişleri oluşturmak için işaretleyin](./media/logic-apps-perform-data-operations/configure-compose-action.png)

   Aşağıda, tamamlanan örnek **Compose** eylem: 

   !["Oluştur" eylemi tamamlandı](./media/logic-apps-perform-data-operations/finished-compose-action.png)

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [Oluştur eylemi](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action). 

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **Compose** eylemi beklenen sonuçları oluşturur, kendinize çıktısını içeren bir bildirim göndermek **Compose** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **Compose** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında, altında **Compose** seçme eylemini **çıkış**. 

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve içerir **çıkış** e-postanın gövdesi ve konu alanları:

   !["E-posta Gönder" eylemini "Çıkış" alanları](./media/logic-apps-perform-data-operations/send-email-compose-action.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Oluştur" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/compose-email-results.png)

<a name="create-csv-table-action"></a>

## <a name="create-csv-table-action"></a>CSV tablosu oluşturma eylemini

Özelliklerini ve değerlerini JavaScript nesne gösterimi (JSON) nesnelerinden bir dizi içeren bir virgülle ayrılmış değer (CSV) tabloda oluşturmak için kullanın **veri işlemleri - CSV tablosu oluştur** eylem. Ardından ortaya çıkan tabloda izleyen eylemlerde kullanabiliriz **CSV tablosu oluştur** eylem. 

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **CSV tablosu oluştur** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - CSV tablosu oluştur](../logic-apps/logic-apps-data-operations-code-samples.md#create-csv-table-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Eylem ilk değeri JSON biçiminde bazı özelliklerini ve değerlerini içeren bir dizidir bir değişken oluşturmak için ayarlanır. 
   Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. CSV tablosu oluşturma için istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna "filtreniz olarak csv tablosu oluştur" girin. Eylem listesinden şu eylemi seçin: **CSV tablosu oluşturma**

   ![Seçin "oluşturma CSV tablosu" eylemi](./media/logic-apps-perform-data-operations/select-create-csv-table-action.png)

4. İçinde **gelen** kutusunda, dizi veya ifade tablo oluşturmak için istediğiniz sağlayın. 

   Bu örnekte, içine tıkladığınızda **gelen** kutusu, dinamik içerik listesi görüntülenir önceden oluşturulmuş değişkeni seçebilmeniz için:

   ![CSV tablosu oluşturma için çıkış dizisi seçin](./media/logic-apps-perform-data-operations/configure-create-csv-table-action.png)

   Aşağıda, tamamlanan örnek **CSV tablosu oluştur** eylem: 

   ![Tamamlandı "oluşturma CSV tablosu" eylemi](./media/logic-apps-perform-data-operations/finished-create-csv-table-action.png)

   Bu eylem, varsayılan olarak, dizi öğeleri temelinde sütunları otomatik olarak oluşturur. 
   Sütun üst bilgilerini ve değerlerini el ile oluşturmak için seçin **Gelişmiş Seçenekleri Göster**. 
   Yalnızca özel değerler sağlamak için değiştirmeniz **sütunları** için **özel**. 
   Özel sütun üst bilgilerini çok sağlamak için değiştirme **üst bilgileri ekleyin** için **Evet**. 

   > [!TIP]
   > Girdi olarak bu özellikleri seçebilmeniz için JSON nesneleri özellikler için kullanıcı dostu belirteçleri oluşturmak için kullanın [JSON Ayrıştır](#parse-json-action) çağırmadan önce **CSV tablosu oluştur** eylem.

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [tablo eylem](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **CSV tablosu oluştur** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **CSV tablosu oluştur** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **CSV tablosu oluştur** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında, altında **CSV tablosu oluştur** seçme eylemini **çıkış**. 

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve içerir **çıkış** alanındaki e-postanın gövdesi:

   !["E-posta Gönder" eylemini "Çıkış" alanları](./media/logic-apps-perform-data-operations/send-email-create-csv-table-action.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   ![E-posta ile "Create CSV tablosu" eylem sonuçları](./media/logic-apps-perform-data-operations/create-csv-table-email-results.png)

<a name="create-html-table-action"></a>

## <a name="create-html-table-action"></a>HTML tablosu oluşturma eylemini

Bir dizi içinde özelliklerini ve değerlerini nesnelerinden JavaScript nesne gösterimi (JSON) sahip bir HTML tablosu oluşturmak için kullanın **veri işlemleri - HTML tablosu oluştur** eylem. Ardından ortaya çıkan tabloda izleyen eylemlerde kullanabiliriz **HTML tablosu oluştur** eylem.

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **HTML tablosu oluştur** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - HTML tablosu oluştur](../logic-apps/logic-apps-data-operations-code-samples.md#create-html-table-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Eylem ilk değeri JSON biçiminde bazı özelliklerini ve değerlerini içeren bir dizidir bir değişken oluşturmak için ayarlanır. 
   Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-create-table-action.png)

2. Bir HTML tablosu oluşturmak istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-create-table-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna "filtreniz olarak html tablosu oluştur" girin. Eylem listesinden şu eylemi seçin: **HTML tablosu oluşturma**

   !["Oluştur HTML tablosu" eylemini seçin](./media/logic-apps-perform-data-operations/select-create-html-table-action.png)

4. İçinde **gelen** kutusunda, dizi veya ifade tablo oluşturmak için istediğiniz sağlayın. 

   Bu örnekte, içine tıkladığınızda **gelen** kutusu, dinamik içerik listesi görüntülenir önceden oluşturulmuş değişkeni seçebilmeniz için:

   ![HTML tablosu oluşturma için çıkış dizisi seçin](./media/logic-apps-perform-data-operations/configure-create-html-table-action.png)

   Aşağıda, tamamlanan örnek **HTML tablosu oluştur** eylem: 

   !["Oluştur HTML tablo" eylemi tamamlandı](./media/logic-apps-perform-data-operations/finished-create-html-table-action.png)

   Bu eylem, varsayılan olarak, dizi öğeleri temelinde sütunları otomatik olarak oluşturur. 
   Sütun üst bilgilerini ve değerlerini el ile oluşturmak için seçin **Gelişmiş Seçenekleri Göster**. 
   Yalnızca özel değerler sağlamak için değiştirmeniz **sütunları** için **özel**. 
   Özel sütun üst bilgilerini çok sağlamak için değiştirme **üst bilgileri ekleyin** için **Evet**. 

   > [!TIP]
   > Girdi olarak bu özellikleri seçebilmeniz için JSON nesneleri özellikler için kullanıcı dostu belirteçleri oluşturmak için kullanın [JSON Ayrıştır](#parse-json-action) çağırmadan önce **HTML tablosu oluştur** eylem.

5. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [tablo eylem](../logic-apps/logic-apps-workflow-actions-triggers.md#table-action).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **HTML tablosu oluştur** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **HTML tablosu oluştur** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **HTML tablosu oluştur** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında, altında **HTML tablosu oluştur** seçme eylemini **çıkış**. 

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve içerir **çıkış** alanındaki e-postanın gövdesi:

   !["E-posta Gönder" eylemini "Çıkış" alanları](./media/logic-apps-perform-data-operations/send-email-create-html-table-action.png)
   
   > [!NOTE]
   > Bir e-posta eylemi çıktısını HTML tablosu dahil olmak üzere, ayarladığınız emin **HTML'dir** özelliğini **Evet** seçenekleri Gelişmiş e-postada eylem. Bu şekilde, e-posta eylemi HTML tablosu doğru bir şekilde biçimlendirir.

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Oluştur HTML tablo" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/create-html-table-email-results.png)

<a name="filter-array-action"></a>

## <a name="filter-array-action"></a>Diziyi filtreleme eylemini

Mevcut bir diziden belirli ölçütlere uyan öğeleri, daha küçük bir dizi oluşturmak için kullanmak **veri işlemleri - diziyi Filtrele** eylem. Filtrelenmiş bir dizi sonra izleyen Eylemler daha sonra kullanabileceğiniz **filtre dizisi** eylem. 

> [!NOTE]
> Koşulunuzu içinde kullandığınız herhangi bir filtre metin büyük/küçük harfe duyarlıdır. Ayrıca, bu eylem biçimini veya bileşenleri dizideki öğelerin değiştiremezsiniz. 
> 
> Dizi çıkışı kullanmak eylemler için **filtre dizisi** eylem, bu eylemlerin diziler girdi olarak kabul etmesi gereken veya çıkış dizisi uyumlu başka bir biçime dönüştürmek sahip. 

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **filtre dizisi** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - diziyi Filtrele](../logic-apps/logic-apps-data-operations-code-samples.md#filter-array-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Eylemi içeren bazı örnek tamsayılar dizisi ilk değeri olan bir değişken oluşturmak için ayarlanır. Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   > [!NOTE]
   > Bu örnekte basit tamsayı dizisi kullansa da, bu eylem, burada filtreleyebilirsiniz diziler nesnelerin özellikleri ve değerleri temel alan bir JSON nesnesi için kullanışlıdır.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-filter-array-action.png)

2. Filtrelenmiş bir dizi oluşturmak istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-filter-array-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "filtre dizisi" girin. Eylem listesinden şu eylemi seçin: **Diziyi filtreleme**

   !["Diziyi filtreleme" eylemini seçin](./media/logic-apps-perform-data-operations/select-filter-array-action.png)

4. İçinde **gelen** kutusunda, dizi veya ifade filtrelemek istediğiniz sağlayın. 

   Bu örnekte, içine tıkladığınızda **gelen** kutusu, dinamik içerik listesi görüntülenir önceden oluşturulmuş değişkeni seçebilmeniz için:

   ![Filtrelenmiş bir dizi oluşturmak için dizi çıkış seçin](./media/logic-apps-perform-data-operations/configure-filter-array-action.png)

5. Koşul için karşılaştırma işleci karşılaştırmak için dizi öğeleri belirtin ve karşılaştırma değeri belirtin.

   Bu örnekte **item()** dizideki her öğe erişmek için işlevi **filtre dizisi** eylem aramaları dizi değeri öğeleri için 1'den büyük:
   
   !["Diziyi filtreleme" eylemi tamamlandı](./media/logic-apps-perform-data-operations/finished-filter-array-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [sorgu eylemi](../logic-apps/logic-apps-workflow-actions-triggers.md#query-action).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **filtre dizisi** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **filtre dizisi** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **filtre dizisi** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında seçin **ifade**. Dizi çıkışını almak için **filtre dizisi** eylemi içeren şu ifadeyi girin **filtre dizisi** eylem adı:

   ```
   @actionBody('Filter_array')
   ```

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve çıkışları içeren **actionBody('Filter_array')** e-postanın gövdesi içindeki ifade:

   ![Eylem içinde "e-posta Gönder" eylemini çıkarır](./media/logic-apps-perform-data-operations/send-email-filter-array-action.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Filtre dizisi" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/filter-array-email-results.png)

<a name="join-action"></a>

## <a name="join-action"></a>Eylem katılın

Bir dizideki tüm öğeler içeren bir dize oluşturmak ve belirli bir ayırıcı karakteri ile bu öğeleri ayırmak için kullanın **veri işlemleri - Birleştir** eylem. Dize sonra izleyen Eylemler daha sonra kullanabileceğiniz **katılın** eylem.

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **katılın** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - katılın](../logic-apps/logic-apps-data-operations-code-samples.md#join-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Bu eylem bazı örnek tamsayı olan bir dizi ilk değeri olan bir değişken oluşturmak için ayarlanır. 
   Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-join-action.png)

2. Bir diziden dizesi oluşturmak istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-join-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "join" girin. Eylem listesinden şu eylemi seçin: **Birleştir**

   !["Veri işlemleri – Birleştir" eylemini seçin](./media/logic-apps-perform-data-operations/select-join-action.png)

4. İçinde **gelen** kutusunda, bir dize olarak katılmasını istediğiniz öğeleri içeren bir dizi sağlayın. 

   İçine tıkladığınızda bu örneğin **gelen** kutusunda, önceden oluşturulmuş değişkeni seçebilmeniz için görüntülenen dinamik içerik listesi:  

   ![Dizi çıkışını bir dize oluşturmak için seçin](./media/logic-apps-perform-data-operations/configure-join-action.png)

5. İçinde **şununla Birleştir** kutusunda, istediğiniz her dizi öğesi ayırmak için karakteri girin. 

   Bu örnek, iki nokta üst üste (:) ayırıcı olarak kullanır.

   ![Ayırıcı karakter girin](./media/logic-apps-perform-data-operations/finished-join-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [katılın eylem](../logic-apps/logic-apps-workflow-actions-triggers.md#join-action).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **katılın** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **katılın** eylem. 

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **katılın** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında, altında **katılın** seçme eylemini **çıkış**. 

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve içerir **çıkış** alanındaki e-postanın gövdesi:

   !["E-posta Gönder" eylemini "Çıkış" alanları](./media/logic-apps-perform-data-operations/send-email-join-action.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Join" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/join-email-results.png)

<a name="parse-json-action"></a>

## <a name="parse-json-action"></a>Eylem JSON Ayrıştır

Başvuru veya erişim özellikleri, JavaScript nesne gösterimi (JSON), kullanıcı dostu alanlar veya bu özellikler için belirteçleri kullanarak oluşturabilirsiniz, içerik **veri işlemleri - JSON Ayrıştır** eylem.
Bu şekilde mantıksal uygulamanız için girişler belirttiğinizde dinamik içerik listesinden bu özellikleri seçebilirsiniz. Bu eylem için bir JSON şeması sağlamak veya bir JSON şema örnek JSON içeriği ya da yük oluşturmak.

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **JSON Ayrıştır** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı : [Veri işlem kod örnekleri - JSON Ayrıştır](../logic-apps/logic-apps-data-operations-code-samples.md#parse-json-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Eylem ilk değeri değişken özelliklerini ve değerlerini içeren bir JSON nesnesi oluşturmak için ayarlanır. 
   Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-parse-json-action.png)

2. JSON içeriği ayrıştırılamıyor istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-parse-json-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna "filtreniz olarak json Ayrıştır" girin. Eylem listesinden şu eylemi seçin: **JSON Ayrıştırma**

   !["JSON Ayrıştır" eylemini seçin](./media/logic-apps-perform-data-operations/select-parse-json-action.png)

4. İçinde **içeriği** kutusunda, istediğiniz ayrıştırmak için JSON içeriği sağlayın. 

   Bu örnekte, içine tıkladığınızda **içeriği** kutusu, dinamik içerik listesi görüntülenir önceden oluşturulmuş değişkeni seçebilmeniz için:

   ![JSON Ayrıştır eylemi için JSON nesnesi seçin](./media/logic-apps-perform-data-operations/configure-parse-json-action.png)

5. Ayrıştırma JSON içeriği tanımlayan JSON şemasını girin. 

   Bu örnekte, JSON şema aşağıdadır:

   ![Ayrıştırılacak istediğiniz bir JSON nesnesi için JSON şemasını belirtin](./media/logic-apps-perform-data-operations/provide-schema-parse-json-action.png)

   Şema yoksa, bu şema JSON içeriği oluşturabilir veya *yükü*, ayrıştırma. 
   
   1. İçinde **JSON Ayrıştır** seçme eylemini **şema oluşturmak için örnek yük kullanma**.

   2. Altında **girin veya yapıştırın örnek JSON yükü**, JSON içeriği belirtin ve ardından **Bitti**.

      ![Şema oluşturmak için JSON içeriği girin](./media/logic-apps-perform-data-operations/generate-schema-parse-json-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [JSON Ayrıştır eylemi](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **JSON Ayrıştır** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **JSON Ayrıştır** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **JSON Ayrıştır** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında, altında **JSON Ayrıştır** eylemi, artık özelliklerini ayrıştırılmış JSON içeriği seçebilirsiniz.

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve içerir **FirstName**, **LastName**, ve **e-posta** alanlar e-posta gövdesi:

   !["E-posta Gönder" eylemini JSON özellikleri](./media/logic-apps-perform-data-operations/send-email-parse-json-action.png)

   Tamamlanmış bir e-posta eylemi şu şekildedir:

   ![Tamamlanmış bir e-posta eylemi](./media/logic-apps-perform-data-operations/send-email-parse-json-action-2.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Join" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/parse-json-email-results.png)

<a name="select-action"></a>

## <a name="select-action"></a>Eylem seçin

Mevcut bir dizideki değerleri oluşturulmuş JSON nesneleri olan bir dizi oluşturmak için kullanmak **veri işlemleri - Seç** eylem. Örneğin, her JSON nesnesi içermesi gereken özellikleri ve bu özellikleri için kaynak dizideki değerleri eşlemeyle ilgili bilgi belirterek bir tamsayı dizisinde her değer için bir JSON nesnesi oluşturabilirsiniz. Ve JSON nesneleri bileşenlerin değiştirebilirsiniz ancak çıkış dizisi her zaman kaynak diziden aynı sayıda öğe içeriyor.

> [!NOTE]
> Dizi çıkışı kullanmak eylemler için **seçin** eylem, bu eylemlerin diziler girdi olarak kabul etmesi gereken veya çıkış dizisi uyumlu başka bir biçime dönüştürmek sahip. 

Kod Görünümü Düzenleyicisi'nde çalışma tercih ederseniz, örnek kopyalayabilirsiniz **seçin** ve **değişken Başlat** mantıksal uygulamanız bu makaledeki eylem tanımlarını temel aldığı iş akışı tanımı: [Veri işlem kod örnekleri - seçin](../logic-apps/logic-apps-data-operations-code-samples.md#select-action-example) 

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a> veya Visual Studio, Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın. 

   Bu örnekte Azure portalında ve bir mantıksal uygulama ile bir **yinelenme** tetikleyici ve bir **değişken Başlat** eylem. 
   Eylemi içeren bazı örnek tamsayılar dizisi ilk değeri olan bir değişken oluşturmak için ayarlanır. 
   Daha sonra mantıksal uygulamanızı test ettiğinizde, tetikleyiciyi harekete geçirmek beklemenize gerek kalmadan uygulamanızı el ile çalıştırabilirsiniz.

   ![Başlangıç örnek mantıksal uygulama](./media/logic-apps-perform-data-operations/sample-starting-logic-app-select-action.png)

2. Dizi oluşturmak istediğiniz mantıksal uygulamanızda, aşağıdaki adımlardan birini izleyin: 

   * Son adım altında bir eylem eklemek için **yeni adım** > **Eylem Ekle**.

     ![Eylem ekle](./media/logic-apps-perform-data-operations/add-select-action.png)

   * Adımlar arasındaki bir eylem eklemek için artı işaretini (+) görünecek şekilde farenizi bağlanan okun üzerine taşıyın. 
   Artı işaretini seçin ve ardından **Eylem Ekle**.

3. Arama kutusuna filtreniz olarak "seçin" girin. Eylem listesinden şu eylemi seçin: **Seç**

   !["Seç" eylemini seçin](./media/logic-apps-perform-data-operations/select-select-action.png)

4. İçinde **gelen** kutusunda, kullanmak istediğiniz kaynak dizisi belirtin.

   Bu örnekte, içine tıkladığınızda **gelen** kutusu, dinamik içerik listesi görüntülenir önceden oluşturulmuş değişkeni seçebilmeniz için:

   ![Kaynak dizi için seçme eylemini seçin](./media/logic-apps-perform-data-operations/configure-select-action.png)

5. İçinde **harita** kutunun sol sütunu, kaynak dizideki her bir değer atamak istediğiniz özellik adı sağlayın. Sağ sütunda özelliği atamak istediğiniz değeri temsil eden bir ifade belirtin.

   Bu örnek, kullanarak her bir tamsayı dizisi değeri atamak özellik adı olarak "Product_ID" belirtir. **item()** işlevi dizideki tüm öğeler erişen bir ifade. 

   ![JSON nesnesi özelliği ve oluşturmak istediğiniz dizi değerlerini belirtin](./media/logic-apps-perform-data-operations/configure-select-action-2.png)

   Tamamlanan eylem şu şekildedir:

   ![Seçme eylemini tamamlandı](./media/logic-apps-perform-data-operations/finished-select-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

Bu eylem, temel alınan iş akışı tanımı hakkında daha fazla bilgi için bkz. [seçin eylem](../logic-apps/logic-apps-workflow-actions-triggers.md).

### <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test edin

Onaylamak için olup olmadığını **seçin** eylemi beklenen sonuçları oluşturur, kendiniz çıktısını içeren bildirim gönderme **seçin** eylem.

1. Mantıksal uygulamanızı sonuçlardan gönderebileceğiniz bir eylem ekleme **seçin** eylem.

2. İçinde bu eylemi, sonuçların görünmesini istediğiniz herhangi bir yere tıklayın. Dinamik içerik listesi açıldığında seçin **ifade**. Dizi çıkışını almak için **seçin** eylemi içeren şu ifadeyi girin **seçin** eylem adı:

   ```
   @actionBody('Select')
   ```

   Bu örnekte **Office 365 Outlook - e-posta Gönder** eylem ve çıkışları içeren **actionBody('Select')** e-postanın gövdesi içindeki ifade:

   ![Eylem içinde "e-posta Gönder" eylemini çıkarır](./media/logic-apps-perform-data-operations/send-email-select-action.png)

3. Şimdi mantıksal uygulamanızı el ile çalıştırın. Tasarımcı araç çubuğunda **çalıştırma**. 

   Kullandığınız e-posta bağlayıcı bağlı olarak, aldığınız sonuçları şunlardır:

   !["Seçin" eylemi sonuçları ile e-posta](./media/logic-apps-perform-data-operations/select-email-results.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
