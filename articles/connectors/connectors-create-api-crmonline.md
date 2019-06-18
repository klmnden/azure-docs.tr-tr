---
title: Dynamics 365 - Azure Logic Apps için Bağlan
description: Oluşturma ve Dynamics 365 (çevrimiçi) REST API ve Azure Logic Apps ile kayıtlarını yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: Mattp123
ms.author: matp
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 08/18/2018
tags: connectors
ms.openlocfilehash: b81efba0ce860bea5fd68dd99ce52980e6816b7e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60313806"
---
# <a name="manage-dynamics-365-records-with-azure-logic-apps"></a>Azure Logic Apps ile Dynamics 365 kayıtlarını yönetme

Azure Logic Apps ve Dynamics 365 Bağlayıcısı ile otomatik görevler ve Dynamics 365 kayıtlarınızda dayalı iş akışları oluşturabilirsiniz. Kayıt, güncelleştirme öğeleri, dönüş kayıtları ve Dynamics 365 hesabınızda daha fazla iş akışlarınızı oluşturabilirsiniz. Çıkış diğer eylemler için kullanılabilir hale getirmek ve Dynamics 365 yanıtları alma logic apps eylemleri dahil edebilirsiniz. Örneğin, Dynamics 365 içinde bir öğe güncelleştirildiğinde, Office 365'i kullanarak bir e-posta gönderebilirsiniz.

Bu makalede, Dynamics 365 içinde yeni bir müşteri adayı kayıt oluşturulduğunda Dynamics 365 içinde bir görev oluşturan bir mantıksal uygulama nasıl oluşturabileceğinizi gösterir.
Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* A [Dynamics 365 hesabı](https://dynamics.microsoft.com)

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Dynamics 365 hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir Dynamics 365 tetikleyicisi ile birlikte mantıksal uygulamanızı başlatmak için gereken bir [boş mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-dynamics-365-trigger"></a>Dynamics 365 tetikleyicisi ekleyin

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

İlk olarak, yeni bir müşteri adayı kaydı Dynamics 365 içinde göründüğünde tetiklenen bir Dynamics 365 tetikleyici ekleyin.

1. İçinde [Azure portalında](https://portal.azure.com), Logic Apps Tasarımcısı'nda boş mantıksal uygulamanızı açın, açık değilse.

1. Arama kutusuna filtreniz olarak "Dynamics 365" girin. Bu örnekte, tetikleyici listesi altında şu tetikleyiciyi seçin: **Bir kayıt oluşturulduğunda**

   ![Tetikleyici seçin](./media/connectors-create-api-crmonline/select-dynamics-365-trigger.png)

1. Dynamics 365 için oturum açmanız istenirse, şimdi oturum açın.

1. Bu tetikleyici ayrıntıları sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Kuruluş adı** | Evet | Dynamics 365 örneğine izleme, örneğin "Contoso" kuruluşunuzun adı |
   | **Varlık adı** | Evet | İzlemek, varlığın adı Örneğin, "Müşteri adayı" | 
   | **Sıklık** | Evet | Tetikleyici için ilgili güncelleştirmeler denetlenirken aralıklarıyla kullanılacak zaman birimi |
   | **Aralık** | Evet | Saniye, dakika, saat, gün, hafta veya sonraki denetim önce geçen ay sayısı |
   ||| 

   ![Tetikleyici ayrıntıları](./media/connectors-create-api-crmonline/trigger-details.png)

## <a name="add-dynamics-365-action"></a>Dynamics 365 Eylem Ekle

Şimdi yeni müşteri adayı kaydı için bir görev kayıt oluşturur Dynamics 365 eylemi ekleyin.

1. Tetikleyicinizin altında seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "Dynamics 365" girin. Eylem listesinden şu eylemi seçin: **Yeni bir kayıt oluşturun**

   ![Eylem seçin](./media/connectors-create-api-crmonline/select-action.png)

1. Bu eylem ayrıntıları sağlayın:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Kuruluş adı** | Evet | Tetikleyicinize aynı örneğinde olması gerekmez, kaydı oluşturmasını istediğiniz Dynamics 365 örneği, "Contoso" ancak bu örnekte'tür. |
   | **Varlık adı** | Evet | Bu gibi bir durumda kaydı oluşturmak istediğiniz varlığı "Görevleri" |
   | | |

   ![Eylem ayrıntıları](./media/connectors-create-api-crmonline/action-details.png)

1. Zaman **konu** kutusu, eylem görünür içine tıklayın **konu** dinamik içerik listesinde görünecek şekilde kutusu. Bu listeden yeni müşteri adayı kaydıyla ilişkili görev kayıt dahil edilecek alan değerleri seçin:

   | Alan | Açıklama |
   |-------|-------------|
   | **Soyadı** | Soyadı birincil ilgili kişi kaydı olarak sağlama |
   | **Konu** | Kayıt müşteri adayı için açıklayıcı bir ad |
   | | |

   ![Görev kayıt ayrıntıları](./media/connectors-create-api-crmonline/create-record-details.png)

1. Tasarımcı araç çubuğunda **Kaydet** mantıksal uygulamanız için. 

1. Mantıksal Uygulama Tasarımcısı araç çubuğundan el ile başlatmak için **çalıştırma**.

   ![Mantıksal uygulamanızı çalıştırma](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

1. Şimdi mantıksal uygulamanızın iş akışı tetikleyebilirsiniz. Bu nedenle Dynamics 365 içinde bir müşteri adayı kaydı oluşturun.

## <a name="add-filter-or-query"></a>Filtre veya Sorgu Ekle

Dynamics 365 eylem verileri nasıl filtreleme yapılacağını belirtmek için **Gelişmiş Seçenekleri Göster** bu uygulamada. Bir filtre veya sıralama'nı, ardından sorgu tarafından ekleyebilirsiniz.
Örneğin, bir filtre sorgusu yalnızca etkin hesapları alın ve bu kayıtların hesap adına göre sıralamak için kullanabilirsiniz. Bu görev için şu adımları izleyin:

1. Altında **filtre sorgusu**, bu OData filtre sorgusunu girin: `statuscode eq 1`

2. Altında **Order By**, dinamik içerik listesi göründüğünde, seçin **hesap adı**. 

   ![Filtre ve sıralama belirtin](./media/connectors-create-api-crmonline/advanced-options.png)

Bu Dynamics 365 müşteri katılımı Web API'sini sistem sorgu seçenekleri daha fazla bilgi için bkz:

* [$filter](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#filter-results)
* [$orderby](https://docs.microsoft.com/dynamics365/customer-engagement/developer/webapi/query-data-web-api#order-results)

### <a name="best-practices-for-advanced-options"></a>Gelişmiş seçenekleri için en iyi uygulamalar

Bir eylem veya tetikleyici bir alan için değer belirttiğinizde, el ile bir değer girin veya dinamik içerik listesinden değeri seçin değerinin veri türü alan türüyle eşleşmelidir.

Bu tablo, bazı alan türleri ve değerleri için gerekli veri türlerini açıklar.

| Alan türü | Gerekli veri türü | Açıklama | 
|------------|--------------------|-------------|
| Metin alanları | Tek metin satırı | Bu alanlar, tek satırlık bir metin veya metin türü olan dinamik içerik gerektirir. <p><p>*Örnek alanları*: **Açıklama** ve **kategorisi** | 
| Tamsayı alanları | Tam sayı | Bazı alanlar tamsayı veya tamsayı türünde dinamik içerik gerektirir. <p><p>*Örnek alanları*: **Tamamlanma** ve **süresi** | 
| Tarih alanları | Tarih ve saat | Bazı alanlar, bir tarihi gg/aa/yyyy biçiminde veya tarih türü olan dinamik içerik gerektirir. <p><p>*Örnek alanları*: **Oluşturulma tarihi**, **başlangıç tarihi**, **gerçek başlangıç**, **gerçek bitiş**, ve **son tarih** | 
| Bir kayıt kimliği ve arama gerektiren alanlar yazın | Birincil anahtar | Başka bir varlık kaydına başvuran bazı alanlar, hem bir kayıt kimliği ve arama türünü gerektirir. | 
||||

Bu alan türlerine ek olarak, Dynamics 365 tetikleyiciler ve Eylemler hem bir kayıt kimliği ve arama türü gerektiren alanlara örnek aşağıda verilmiştir. Bu gereksinim, dinamik listeden seçtiğiniz değerler çalışmaz anlamına gelir.

| Alan | Açıklama |
|-------|-------------|
| **Sahip** | Bir geçerli kullanıcı kimliği olması gerekir veya takım kaydı kimliği |
| **Sahip türü** | Aşağıdakilerden biri olması gereken **sistem kullanıcıları** veya **takımlar**. |
| **İlgili** | Hesap kimliği gibi bir geçerli kayıt kimliği olması gerekir veya ilgili kişi kaydı kimliği |
| **İlgili türü** | Bir arama türü gibi olmalıdır **hesapları** veya **kişiler**. |
| **Müşteri** | Hesap kimliği gibi bir geçerli kayıt kimliği olması gerekir veya ilgili kişi kaydı kimliği |
| **Müşteri türü** | Arama türü gibi olmalıdır **hesapları** veya **kişiler**. |
|||

Bu örnekte, eylem adlı **yeni kayıt oluşturma** yeni bir görev kayıt oluşturur:

![Görev kayıt kayıt kimliği ve arama türleri oluşturma](./media/connectors-create-api-crmonline/create-record-advanced.png)

Bu eylem görev kaydın belirli bir kullanıcı kimliği veya takım kaydı kimliği, kayıt kimliği temel atar **sahibi** alan ve arama yazın **sahip türü** alan:

![Sahip kayıt kimliği ve arama türü](./media/connectors-create-api-crmonline/owner-record-id-and-lookup-type.png)

Bu eylem ayrıca kaydı kimliği eklenir, ile ilişkili hesap kaydı ekler **ilgili** alan ve arama yazın **ilgili türü** alan:

![İlgili kayıt kimliği ve arama türü](./media/connectors-create-api-crmonline/regarding-record-id-lookup-type-account.png)

## <a name="find-record-id"></a>Kayıt Kimliğini bulma

Bir kaydın Kimliğini bulmak için aşağıdaki adımları izleyin:

1. Dynamics 365 içinde bir hesap kaydı gibi bir kayıt açın.

2. Eylemler araç çubuğunda, aşağıdaki adımlardan birini seçin:

   * Seçin **büyütme**. ![açılan kayıt](./media/connectors-create-api-crmonline/popout-record.png) 
   * Seçin **e-posta ile bağlantı** tam URL'yi varsayılan e-posta programınıza kopyalamak için.

   Kayıt Kimliği URL arasında görünür `%7b` ve `%7d` karakter kodlama:

   ![Kayıt Kimliğini bulma](./media/connectors-create-api-crmonline/find-record-ID.png)

## <a name="troubleshoot-failed-runs"></a>Başarısız çalışma sorunlarını giderme

Bulmak ve mantıksal uygulamanızda başarısız adımları gözden geçirmek için mantıksal uygulamanızın çalıştırma geçmişi, durumu, girişler, çıkışlar ve benzeri görüntüleyebilirsiniz.

1. Azure portalında mantıksal uygulamanızın ana menüsündeki seçin **genel bakış**. İçinde **çalıştırma geçmişi** mantıksal uygulamanız için tüm çalışma durumları gösterir, bölümünde, daha fazla bilgi için başarısız bir çalıştırmayı seçin.

   ![Logic app çalıştırma durumu](./media/connectors-create-api-crmonline/run-history.png)

1. Daha fazla ayrıntı görüntülemek için başarısız olan bir adım genişletin.

   ![Başarısız adımı genişletin](./media/connectors-create-api-crmonline/expand-failed-step.png)

1. Girişler gibi adımının ayrıntılarını gözden geçirin ve hata ardındaki yardımcı olan çıktıları bulun.

   ![Başarısız adımı - giriş ve çıkışları](./media/connectors-create-api-crmonline/expand-failed-step-inputs-outputs.png)

Logic apps sorunlarını giderme hakkında daha fazla bilgi için bkz. [mantıksal uygulama hatalarını tanılama](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları, bağlayıcının Openapı'nin açıklandığı gibi teknik ayrıntılar için (önceki adıyla Swagger) dosyası, bkz: [bağlayıcının başvuru sayfası](/connectors/dynamicscrmonline/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
