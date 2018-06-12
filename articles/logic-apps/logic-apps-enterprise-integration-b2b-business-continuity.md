---
title: B2B tümleştirme hesabı - Azure mantıksal uygulamaları için olağanüstü durum kurtarma | Microsoft Docs
description: Logic Apps B2B olağanüstü durum kurtarma
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: jeconnoc
editor: ''
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 65c7262916219a74dcd6bdab487306b5bd5f709f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299105"
---
# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B çapraz bölge olağanüstü durum kurtarma

B2B iş yükleri siparişler ve faturalar gibi para işlemleri içerir. Bir olağanüstü sırasında bir iş için kritik için hızlı bir şekilde iş düzeyi SLA karşılamak için kurtarma kendi iş ortaklarıyla üzerinde anlaşmaya varılan. Bu makalede, B2B iş yükleri için iş devamlılığı plan oluşturma gösterilmiştir. 

* Olağanüstü Durum Kurtarma Hazırlık 
* İkincil bölge'ye üzerinden bir olağanüstü durum olay sırasında başarısız oluyor 
* Birincil bölge için bir olağanüstü durum olayından sonra geri dönüş

## <a name="disaster-recovery-readiness"></a>Olağanüstü Durum Kurtarma Hazırlık  

1. Bir ikincil bölge tanımlayabilir ve oluşturabilirsiniz bir [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ikincil bölge içinde.

2. İş ortakları, şemalar ve çalışma durumu burada ikincil bölge tümleştirme hesabına çoğaltılması gerekiyor gerekli ileti akışları için anlaşmaları ekleyin.

   > [!TIP]
   > Tutarlılık tümleştirme hesap yapı'nın Adlandırma kuralında bölgeler arasında olduğundan emin olun. 

3. Çalışma durumu birincil bölgesinden çıkarmak için ikincil bölge'de bir mantıksal uygulama oluşturun. 

   Bu mantıksal uygulama olmalıdır bir *tetikleyici* ve bir *eylem*. 
   Tetikleyici birincil bölge tümleştirme hesabınıza bağlanmasını ve eylem ikincil bölge tümleştirme hesabına bağlanmanız gerekir. 
   Zaman aralığına dayalı, tetikleyici durum tablosu çalıştırmak birincil bölge yoklar ve yeni kayıtlar varsa çeker. Eylem bunları ikincil bölge tümleştirme hesabını güncelleştirir. 
   Bu ikincil bölge'ye birincil bölgesinden artımlı çalışma zamanı durumunu almak için yardımcı olur.

4. Logic Apps tümleştirme hesaptaki iş sürekliliği, B2B protokolleri - X12, AS2 ve EDIFACT temel destekleyecek şekilde tasarlanmıştır. Ayrıntılı adımlar bulmak için ilgili bağlantıyı seçin.

5. İkincil bir bölgede tüm birincil bölge kaynakları çok dağıtmak için önerilir. 

   Azure SQL Database veya Azure Cosmos DB, Azure Service Bus ve Azure olay hub'ın Mesajlaşma, Azure API Management ve Azure App Service'teki Azure Logic Apps özelliği için kullanılan birincil bölge kaynaklar içerir.   

6. Bir ikincil bölge'bir birincil bölge arasında bağlantı kurun. Çalışma durumu birincil bölgesinden çıkarmak için bir ikincil bölge'de bir mantıksal uygulama oluşturun. 

   Mantıksal uygulama tetikleyici ve eylem olması gerekir. 
   Tetikleyici bir birincil bölge tümleştirme hesabına bağlanmanız gerekir. 
   Eylemi bir ikincil bölge tümleştirme hesabına bağlanmanız gerekir. 
   Zaman aralığına dayalı, tetikleyici durum tablosu çalıştırmak birincil bölge yoklar ve yeni kayıtlar varsa çeker. 
   Eylem bunları bir ikincil bölge tümleştirme hesabına güncelleştirir. 
   Bu işlem artımlı çalışma zamanı durumunu birincil bölgesinden ikincil bölge'ye almak için yardımcı olur.

Logic Apps tümleştirme hesabında iş sürekliliği B2B protokolleri X12, AS2 ve EDIFACT göre desteği sağlar. X12 ve AS2 kullanma hakkında ayrıntılı adımlar için bkz: [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) ve [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) bu makalede.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Bir ikincil bölge'ye üzerinden bir olağanüstü durum olay sırasında başarısız oluyor

Olağanüstü durum olay sırasında birincil bölge iş sürekliliği, ikincil bölge doğrudan trafiği için kullanılabilir olmadığında. Bir ikincil bölge yardımcı bir RPO'ya/RTO'ya hızlı bir şekilde karşılamak için işlevleri kurtarmak için iş ortakları tarafından üzerinde anlaşmaya varılan. Ayrıca bir bölgesinden başka bir bölgeye yük devri çalışmalarını en aza indirir. 

Denetim numaraları birincil bölgesinden bir ikincil bölge'ye kopyalanırken beklenen bir gecikme süresi yok. Yinelenen oluşturulan denetim numaraları ortaklarınıza olağanüstü durum olayı sırasında gönderilmesini önlemek için denetim numaraları kullanarak ikincil bölge sözleşmelerde Artır önerilir [PowerShell cmdlet'leri](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Bir birincil bölge sonrası olağanüstü için geri dönüş

Kullanılabilir olduğunda bir birincil bölge için geri dönüş için şu adımları izleyin:

1. İkincil bölge'de ortakları gelen iletileri kabul durdurun.  

2. Tüm birincil bölge anlaşmalar için oluşturulan denetim numaraları kullanarak Artır [PowerShell cmdlet'leri](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. İkincil bölge doğrudan trafiğinden birincil bölge için.

4. Birincil bölgesinden çalışma durumu çekmek için ikincil bölgede oluşturulan mantıksal uygulama etkin olup olmadığını denetleyin.

## <a name="x12"></a>X12 

EDI iş sürekliliği X 12 belgeler üzerinde denetim numaraları dayanır:

> [!TIP]
> Aynı zamanda [X12 hızlı başlatma şablonunu](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) logic apps oluşturmak için. Oluşturma birincil ve ikincil tümleştirme şablonu kullanmak için önkoşulları hesaplarıdır. Şablon iki logic apps, alınan denetim numaraları için bir ve oluşturulan denetim numaraları için başka bir oluşturmanıza yardım eder. İlgili tetikleyiciler ve Eylemler tetikleyici birincil tümleştirme hesabını ve eylem ikincil tümleştirme hesabına bağlanma logic apps oluşturulur.

**Önkoşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek üzere X12 yinelenen onay ayarlarını Seç anlaşmanın alma ayarı.

![Yinelenen onay ayarlarını Seç](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bir bölgede.    

2. Arama **X12**seçip **bir denetim numarasını değiştirildiğinde X12-**.   

   ![X12 arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn1.png)

   Tetikleyici bir tümleştirme hesap bağlantı kurmak ister. 
   Tetikleyici bir birincil bölge tümleştirme hesabına bağlanması gerekir.

3. Bir bağlantı adı girin, seçin, *birincil bölge tümleştirme hesabını* listeden seçin **Oluştur**.   

   ![Birincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn2.png)

4. **Denetim sayı Eşitlemeyi Başlat DateTime** ayardır isteğe bağlıdır. **Sıklığı** ayarlanabilir **gün**, **saat**, **Minute**, veya **ikinci** bir aralık ile.   

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

5. Seçin **yeni adım** > **Eylem Ekle**.

   ![Yeni adım, bir eylem Ekle](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

6. Arama **X12**seçip **X12-ekleme veya güncelleştirme denetim numaraları**.   

   ![Denetim numaralarını ekleyin veya güncelleştirin](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn5.png)

7. Bir eylem bir ikincil bölge tümleştirme hesabına bağlanmak üzere seçin **değiştirmek bağlantı** > **yeni bağlantı ekleme** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölge tümleştirme hesabını* listeden seçin **Oluştur**. 

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

8. Ham girişleri sağ üst köşesinde simgesine tıklayarak geçin.

   ![Ham girişleri geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12rawinputs.png)

9. Dinamik içerik seçiciden gövde seçin ve mantıksal uygulama kaydedin.

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn7.png)

   Zaman aralığına dayalı, tetikleyici alınan birincil bölge denetim sayı tablosunu yoklar ve yeni kayıtlar çeker. 
   Eylem kayıtları ikincil bölge tümleştirme hesabındaki güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **Atlanan**.   

   ![Denetim sayı tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Zaman aralığına dayalı, artımlı çalışma zamanı durumu bir ikincil bölge'ye birincil bölgesinden çoğaltır. Olağanüstü durum olay sırasında birincil bölge ikincil bölge iş sürekliliği için kullanılabilir, doğrudan trafiği değilse. 

## <a name="edifact"></a>EDIFACT 

İş sürekliliği EDI EDIFACT belgeler için Denetim numaralarında temel alır.

**Önkoşullar**

Gelen iletiler için olağanüstü durum kurtarmayı etkinleştirmek üzere EDIFACT anlaşmanızın alma ayarlarında yinelenen onay ayarlarını seçin.

![Yinelenen onay ayarlarını Seç](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactdupcheck.png)  

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bir bölgede.    

2. Arama **EDIFACT**seçip **bir denetim numarasını değiştirildiğinde EDIFACT -**.

   ![EDIFACT arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/edifactcn1.png)

   Tetikleyici bir tümleştirme hesap bağlantı kurmak ister. 
   Tetikleyici bir birincil bölge tümleştirme hesabına bağlanması gerekir. 

3. Bir bağlantı adı girin, seçin, *birincil bölge tümleştirme hesabını* listeden seçin **Oluştur**.    

   ![Birincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN2.png)

4. **Denetim sayı Eşitlemeyi Başlat DateTime** ayardır isteğe bağlıdır. **Sıklığı** ayarlanabilir **gün**, **saat**, **Minute**, veya **ikinci** bir aralık ile.    

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn3.png)

6. Seçin **yeni adım** > **Eylem Ekle**.    

   ![Yeni adım, bir eylem Ekle](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn4.png)

7. Arama **EDIFACT**seçip **EDIFACT - ekleme veya güncelleştirme denetim numaraları**.   

   ![Denetim numaralarını ekleyin veya güncelleştirin](./media/logic-apps-enterprise-integration-b2b-business-continuity/EdifactChooseAction.png)

8. Bir eylem bir ikincil bölge tümleştirme hesabına bağlanmak üzere seçin **değiştirmek bağlantı** > **yeni bağlantı ekleme** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölge tümleştirme hesabını* listeden seçin **Oluştur**.

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12cn6.png)

9. Ham girişleri sağ üst köşesinde simgesine tıklayarak geçin.

   ![Ham girişleri geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/Edifactrawinputs.png)

10. Dinamik içerik seçiciden gövde seçin ve mantıksal uygulama kaydedin.   

   ![Dinamik içerik alanları](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12CN7.png)

   Zaman aralığına dayalı, tetikleyici alınan birincil bölge denetim sayı tablosunu yoklar ve yeni kayıtlar çeker.
   Eylem kayıtları için ikincil bölge tümleştirme hesabını güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **Atlanan**.

   ![Denetim sayı tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/x12recevicedcn8.png)

Zaman aralığına dayalı, artımlı çalışma zamanı durumu bir ikincil bölge'ye birincil bölgesinden çoğaltır. Olağanüstü durum olay sırasında birincil bölge ikincil bölge iş sürekliliği için kullanılabilir, doğrudan trafiği değilse. 

## <a name="as2"></a>AS2 

İş sürekliliği AS2 protokolünü kullanan belgeler için ileti kimliği ve MIC değeri temel alır.

> [!TIP]
> Aynı zamanda [AS2 Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/pull/3302) logic apps oluşturmak için. Oluşturma birincil ve ikincil tümleştirme şablonu kullanmak için önkoşulları hesaplarıdır. Şablonu bir tetikleyici ve eylem sahip bir mantıksal uygulama oluşturma yardımcı olur. Mantıksal uygulama birincil tümleştirme hesabı ve bir ikincil tümleştirme hesabı için bir eylem için tetikleyiciden bir bağlantı oluşturur.

1. Oluşturma bir [mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md) ikincil bölge içinde.  

2. Arama **AS2**seçip **AS2 - olduğunda bir MIC değer oluşturulur**.   

   ![AS2 arayın](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid1.png)

   Bir tetikleyici bir tümleştirme hesap bağlantı kurmak ister. 
   Tetikleyici bir birincil bölge tümleştirme hesabına bağlanması gerekir. 
   
3. Bir bağlantı adı girin, seçin, *birincil bölge tümleştirme hesabını* listeden seçin **Oluştur**.

   ![Birincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid2.png)

4. **MIC değeri Eşitlemeyi Başlat DateTime** ayardır isteğe bağlıdır. **Sıklığı** ayarlanabilir **gün**, **saat**, **Minute**, veya **ikinci** bir aralık ile.   

   ![DateTime ve sıklığı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid3.png)

5. Seçin **yeni adım** > **Eylem Ekle**.  

   ![Yeni adım, bir eylem Ekle](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid4.png)

6. Arama **AS2**seçip **AS2 - MIC içeriği güncelleştirmek ya da eklemek**.  

   ![MIC ekleme veya güncelleştirme](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid5.png)

7. Bir eylem bir ikincil tümleştirme hesabına bağlanmak üzere seçin **değiştirmek bağlantı** > **yeni bağlantı ekleme** kullanılabilir tümleştirme hesapları listesi. Bir bağlantı adı girin, seçin, *ikincil bölge tümleştirme hesabını* listeden seçin **Oluştur**.

   ![İkincil bölge tümleştirme hesap adı](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid6.png)

8. Ham girişleri sağ üst köşesinde simgesine tıklayarak geçin.

   ![Ham girişleri geçiş](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2rawinputs.png)

9. Dinamik içerik seçiciden gövde seçin ve mantıksal uygulama kaydedin.   

   ![Dinamik içerik](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid7.png)

   Zaman aralığına dayalı, tetikleyici birincil bölge tablo yoklar ve yeni kayıtlar çeker. Eylem bunları ikincil bölge tümleştirme hesabına güncelleştirir. 
   Herhangi bir güncelleştirme varsa, tetikleyici durumu olarak görünür **Atlanan**.  

   ![Birincil bölge tablosu](./media/logic-apps-enterprise-integration-b2b-business-continuity/as2messageid8.png)

Zaman aralığına dayalı, artımlı çalışma zamanı durumu ikincil bölge'ye birincil bölgesinden çoğaltır. Olağanüstü durum olay sırasında birincil bölge ikincil bölge iş sürekliliği için kullanılabilir, doğrudan trafiği değilse. 

## <a name="next-steps"></a>Sonraki adımlar

[B2B iletilerini izleme](logic-apps-monitor-b2b-message.md)

