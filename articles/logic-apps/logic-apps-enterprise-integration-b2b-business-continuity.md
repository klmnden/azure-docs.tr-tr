---
title: B2B tümleştirme hesabı - Azure Logic Apps için olağanüstü durum kurtarma | Microsoft Docs
description: Azure Logic Apps'te bölgeler arası Olağanüstü Durum Kurtarmaya Hazırlanma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.date: 04/10/2017
ms.openlocfilehash: ac29ef7f0599cc41924ba1a5a00e46b0292e7e9b
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967750"
---
# <a name="cross-region-disaster-recovery-for-b2b-integration-accounts-in-azure-logic-apps"></a>Azure Logic Apps B2B tümleştirme hesapları için bölgeler arası olağanüstü durum kurtarma

B2B iş yükleri, sipariş ve faturalar gibi para işlemleri içerir. Bir olağanüstü durum olayı sırasında bir iş için kritik için hızlı bir şekilde iş düzeyinde SLA'ları karşılamak için Kurtarma ile bunların iş ortakları varılmış. Bu makalede, bir iş süreklilik planı B2B iş yükleri için yapı gösterilmiştir. 

* Olağanüstü Durum Kurtarma Hazırlık 
* İkincil bölgeye bir olağanüstü durum olayı sırasında yük devretme 
* Birincil bölgeye bir olağanüstü durum olayından sonra geri döner

## <a name="disaster-recovery-readiness"></a>Olağanüstü Durum Kurtarma Hazırlık  

1. İkincil bir bölgeye tanımlamak ve oluşturma bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ikincil bölgedeki.

2. İş ortakları, şemalar ve çalıştırma durumu ikincil bölgeye tümleştirme hesabına çoğaltılması gereken yere gerekli ileti akışları için anlaşmaları ekleyin.

   > [!TIP]
   > Bölgeler arasında tümleştirme hesabı yapı'nın adlandırma kuralı tutarlılık olduğundan emin olun. 

3. Birincil bölgede çalıştırma durumu çıkarmak için ikincil bölge'de bir mantıksal uygulama oluşturun. 

   Bu mantıksal uygulama olmalıdır bir *tetikleyici* ve *eylem*. 
   Tetikleyici birincil bölgeye tümleştirme hesabı ile bağlanmanız gerekir ve eylem ikincil bölgeye tümleştirme hesabına bağlanması. 
   Zaman aralığına dayalı, tetikleyici çalıştırma durum tablosu birincil bölgeye yoklar ve varsa yeni kayıtlarını çeker. Eylem, bunları ikincil bölgeye tümleştirme hesabına güncelleştirir. 
   Bu, artımlı çalışma zamanı durumu birincil bölgeden ikincil bölgeye almak için yardımcı olur.

4. Logic Apps tümleştirme hesabı'nda iş sürekliliği, B2B protokolleri - X12, AS2 ve EDIFACT tabanlı desteklemek için tasarlanmıştır. Ayrıntılı adımlar bulmak için ilgili bağlantıları seçin.

5. İkincil bir bölgede tüm birincil bölgeye kaynakları çok dağıtmak için önerilir. 

   Azure SQL veritabanı veya Azure Cosmos DB, Azure Service Bus ve Azure Event Hubs Mesajlaşma, Azure API Management ve Azure Logic Apps özelliği, Azure App Service için kullanılan birincil bölgeye kaynaklar içerir.   

6. Birincil bir bölgeden ikincil bir bölgeye bağlantı kurun. Bir birincil bölgede çalıştırma durumu çıkarmak için bir ikincil bölge'de bir mantıksal uygulama oluşturun. 

   Mantıksal uygulama bir tetikleyici ve eylem olmalıdır. 
   Tetikleyici, bir birincil bölge tümleştirme hesabına bağlanmanız gerekir. 
   Eylemi bir ikincil bölgeye tümleştirme hesabına bağlanmanız gerekir. 
   Zaman aralığına dayalı, tetikleyici çalıştırma durum tablosu birincil bölgeye yoklar ve varsa yeni kayıtlarını çeker. 
   Eylem, bunları bir ikincil bölgeye tümleştirme hesabına güncelleştirir. 
   Bu işlem, birincil bölgeden ikincil bölgeye artımlı çalışma zamanının durumunu almak yardımcı olur.

Logic Apps tümleştirme hesabı'nda iş sürekliliği B2B protokolleri X12, AS2 ve EDIFACT dayalı destek sağlar. X12 ve AS2 kullanma hakkında ayrıntılı adımlar için bkz. [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) ve [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) bu makaledeki.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>İkincil bir bölgeye üzerinden bir olağanüstü durum olay sırasında başarısız oluyor

Olağanüstü durum olayı sırasında birincil bölge için iş sürekliliği, trafiği ikincil bölgeye kullanılabilir olmadığında. Bir ikincil bölgeye yardımcı bir RPO'ya/RTO'ya karşılamak için hızla işlevleri kurtarmak için iş ortaklarının tarafından üzerinde anlaşmaya varılan. Ayrıca bir bölgesinden başka bir bölgeye yük devretmek için çabalarınızı en aza indirir. 

Denetim numaraları birincil bir bölgeden ikincil bölgeye kopyalanırken beklenen bir gecikme yoktur. Yinelenen oluşturulan denetim numaraları iş ortakları için olağanüstü durum olayı sırasında gönderilmesini önlemek için Denetim numaralarını kullanarak ikincil bölgeye sözleşmelerde artırmak için önerilir [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.logicapp/set-azurermintegrationaccountgeneratedicn?view=azurermps-6.13.0).

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Bir birincil bölge sonrası olağanüstü durum olayına geri döner

Uygun olduğunda birincil bölgeye geri dönmesi için bu adımları izleyin:

1. İkincil bölgedeki iş ortaklarından iletileri kabul etmeye durdurun.  

2. Tüm birincil bölgeye anlaşmalar için oluşturulan denetim numaralarını kullanarak Artır [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.logicapp/set-azurermintegrationaccountgeneratedicn?view=azurermps-6.13.0).  

3. Birincil bölgeye trafiği ikincil bölgeye ait.

4. Birincil bölgede çalıştırma durumu çekmek için ikincil bölgede oluşturulan mantıksal uygulama etkinleştirildiğinden emin olun.

## <a name="x12"></a>X12 

X 12 belgeleri EDI için iş sürekliliği denetim numaralarını dayanır:

> [!TIP]
> Ayrıca [X12 Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/201-logic-app-b2b-disaster-recovery-replication/) mantıksal uygulamalar oluşturmak için. Birincil ve ikincil tümleştirme hesabı oluşturmayı, şablonu kullanmak için önkoşullardır. Şablon iki logic apps, alınan denetim numaraları için bir tane ve oluşturulan denetim numaraları için başka oluşturulmasına yardımcı olur. Tetikleyici birincil tümleştirme hesabı ve eylem ikincil tümleştirme hesabına bağlanma logic apps'te, kendi Tetikleyicileri ve eylemleri oluşturulur.

**Önkoşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek için yinelenen onay ayarları içinde X12 seçin anlaşması ayarlarını alır.

![Yinelenen onay ayarlarını seçin](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bir bölgede.    

2. Arama **X12**seçip **denetim numarası değiştirildiğinde X12-**.   

   ![X12 arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   Tetikleyici bir tümleştirme hesabı için bir bağlantı kurmak ister. 
   Tetikleyici, bir birincil bölge tümleştirme hesabına bağlanması gerekir.

3. Bir bağlantı adı girin, seçin, *birincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**.   

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. **Denetim numarası eşitlemesini başlatma tarihi ve saati** ayardır isteğe bağlı. **Sıklığı** ayarlanabilir **gün**, **saat**, **dakika**, veya **ikinci** zaman aralığı.   

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Seçin **yeni adım** > **Eylem Ekle**.

   ![Yeni adım, bir eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Arama **X12**seçip **X12-ekleme veya güncelleştirme denetim numaraları**.   

   ![Denetim numaralarını ekleyin veya güncelleştirin](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Bir eylem bir ikincil bölgeye tümleştirme hesabına bağlanacak seçin **Bağlantıyı Değiştir** > **yeni bağlantı Ekle** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**. 

   ![İkincil bölgeye tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Ham girişleri için sağ üst köşedeki simgeye tıklayarak geçin.

   ![Ham girişleri için geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Gövde dinamik içerik seçiciden seçin ve mantıksal uygulamayı kaydedin.

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Zaman aralığına dayalı, tetikleyici alınan birincil bölgeye denetim numarası tablo yoklar ve yeni kayıtlarını çeker. 
   Eylem kayıtları ikincil bölge'tümleştirme hesabında güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **atlandı**.   

   ![Denetim numarası tablo](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Artımlı çalışma zamanı durumu zaman aralığına dayalı, birincil bir bölgeden ikincil bölgeye çoğaltır. Olağanüstü durum olayı sırasında iş sürekliliği için ikincil bölgede kullanılabilir, doğrudan trafiği birincil bölgeye değilse. 

## <a name="edifact"></a>EDIFACT 

EDI EDIFACT belgelerini için iş sürekliliği denetim numaralarını temel alır.

**Önkoşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek için EDIFACT anlaşmanın alma ayarları'nda yinelenen onay ayarlarını seçin.

![Yinelenen onay ayarlarını seçin](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bir bölgede.    

2. Arama **EDIFACT**seçip **denetim numarası değiştirildiğinde EDIFACT -**.

   ![EDIFACT arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   Tetikleyici bir tümleştirme hesabı için bir bağlantı kurmak ister. 
   Tetikleyici, bir birincil bölge tümleştirme hesabına bağlanması gerekir. 

3. Bir bağlantı adı girin, seçin, *birincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**.    

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. **Denetim numarası eşitlemesini başlatma tarihi ve saati** ayardır isteğe bağlı. **Sıklığı** ayarlanabilir **gün**, **saat**, **dakika**, veya **ikinci** zaman aralığı.    

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Seçin **yeni adım** > **Eylem Ekle**.    

   ![Yeni adım, bir eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Arama **EDIFACT**seçip **EDIFACT - ekleme veya güncelleştirme denetim numaraları**.   

   ![Denetim numaralarını ekleyin veya güncelleştirin](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Bir eylem bir ikincil bölgeye tümleştirme hesabına bağlanacak seçin **Bağlantıyı Değiştir** > **yeni bağlantı Ekle** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**.

   ![İkincil bölgeye tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Ham girişleri için sağ üst köşedeki simgeye tıklayarak geçin.

   ![Ham girişleri için geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Gövde dinamik içerik seçiciden seçin ve mantıksal uygulamayı kaydedin.   

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Zaman aralığına dayalı, tetikleyici alınan birincil bölgeye denetim numarası tablo yoklar ve yeni kayıtlarını çeker.
   Eylem kayıtları için ikincil bölgeye tümleştirme hesabı güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **atlandı**.

   ![Denetim numarası tablo](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Artımlı çalışma zamanı durumu zaman aralığına dayalı, birincil bir bölgeden ikincil bölgeye çoğaltır. Olağanüstü durum olayı sırasında iş sürekliliği için ikincil bölgede kullanılabilir, doğrudan trafiği birincil bölgeye değilse. 

## <a name="as2"></a>AS2 

AS2 protokolü kullanan belgeler için iş sürekliliği, ileti kimliği ve MIC değerini temel alır.

> [!TIP]
> Ayrıca [AS2 Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/pull/3302) mantıksal uygulamalar oluşturmak için. Birincil ve ikincil tümleştirme hesabı oluşturmayı, şablonu kullanmak için önkoşullardır. Şablon, bir tetikleyici ve eylem sahip mantıksal uygulama oluşturma yardımcı olur. Mantıksal uygulama, bir birincil tümleştirme hesabı ve bir ikincil tümleştirme hesabı için bir eylem için bir tetikleyici ile bir bağlantı oluşturur.

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bölgedeki.  

2. Arama **AS2**seçip **AS2 - zaman bir MIC değeri oluşturulduğunda**.   

   ![AS2 arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Bir tetikleyici bir tümleştirme hesabı için bir bağlantı kurmak ister. 
   Tetikleyici, bir birincil bölge tümleştirme hesabına bağlanması gerekir. 
   
3. Bir bağlantı adı girin, seçin, *birincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**.

   ![Birincil bölge tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. **MIC değeri eşitlemesinin başlatılacağı tarih ve saat** ayardır isteğe bağlı. **Sıklığı** ayarlanabilir **gün**, **saat**, **dakika**, veya **ikinci** zaman aralığı.   

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Seçin **yeni adım** > **Eylem Ekle**.  

   ![Yeni adım, bir eylem ekleme](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Arama **AS2**seçip **AS2 - ekleme veya güncelleştirme MIC içeriği**.  

   ![MIC ekleme veya güncelleştirme](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Bir eylem ikincil tümleştirme hesabı'na bağlanmak için seçin **Bağlantıyı Değiştir** > **yeni bağlantı Ekle** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölgeye tümleştirme hesabı* listesinden seçip **Oluştur**.

   ![İkincil bölgeye tümleştirme hesabı adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Ham girişleri için sağ üst köşedeki simgeye tıklayarak geçin.

   ![Ham girişleri için geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Gövde dinamik içerik seçiciden seçin ve mantıksal uygulamayı kaydedin.   

   ![Dinamik içerik](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Zaman aralığına dayalı, tetikleyici birincil bölgeye tablo yoklar ve yeni kayıtlarını çeker. Eylem, bunları ikincil bölgeye tümleştirme hesabı'na güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **atlandı**.  

   ![Birincil bölge tablo](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Artımlı çalışma zamanı durumu zaman aralığına dayalı, birincil bölgeden ikincil bölgeye çoğaltır. Olağanüstü durum olayı sırasında iş sürekliliği için ikincil bölgede kullanılabilir, doğrudan trafiği birincil bölgeye değilse. 

## <a name="next-steps"></a>Sonraki adımlar

[B2B iletilerini izleme](logic-apps-monitor-b2b-message.md)

