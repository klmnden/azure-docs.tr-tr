---
title: Azure Sentinel hızlı başlangıç - Azure Gözcü önizlemesi ile çalışmaya başlama | Microsoft Docs
description: Azure Sentinel hızlı başlangıç - Azure Gözcü ile çalışmaya başlama
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5a4ae93c-d648-41fb-8fb8-96a025d2f73e
ms.service: sentinel
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: a80c4db1b81dd2bc0b223a2781e28ccb1f5ba68e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60805729"
---
# <a name="quickstart-get-started-with-azure-sentinel-preview"></a>Hızlı Başlangıç: Gözcü Azure Önizleme kullanmaya başlama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Bu hızlı başlangıçta nasıl hızlı bir şekilde görüntüleyebilir ve Azure Gözcü kullanarak ortamınızda neler olduğunu izlemek öğreneceksiniz. Veri kaynaklarınızı Azure Gözcü için bağlandıktan sonra böylece tüm bağlı veri kaynakları arasında neler olduğunu bilmeniz, anında Görselleştirme ve veri analizi alın. Azure Sentinel Araçları zaten kullanılabilir tablolar yanı sıra Azure'nın tüm gücünden sağlayan panolar ve günlükleri ve sorgular için analytics ile sağlamak için yerleşik grafikler sağlar. Yerleşik panoları kullanabilir veya yeni bir pano, karalama veya mevcut bir panoya göre kolayca oluşturun. 

## <a name="get-visualization"></a>Görselleştirme Al

Görselleştirme ve analiz ortamınızda olup bitenleri almak için ilk olarak, kuruluşunuzun güvenlik duruşunu hakkında bir fikir edinmek için genel bakış Panosu'nu göz atın. İçinden oluşturuldukları ham verilere ulaşmak için bu kutucukların her öğeye tıklayabilirsiniz. Kumu ve gözden geçirin ve araştırmak için sahip olduğunuz uyarıların sayısını en aza indirmenize yardımcı olmak için Azure Gözcü fusion teknik servis taleplerine uyarılar ilişkilendirmek için kullanır. **Çalışmaları** birlikte, araştırmak ve çözmek eyleme dönüştürülebilir bir olay oluşturma ilgili uyarıları gruplarıdır.

- Azure portalında Azure Gözcü seçin ve sonra izlemek istediğiniz çalışma alanını seçin.

  ![Azure Sentinel genel bakış](./media/qs-get-visibility/overview.png)

- Üstteki araç çubuğunda, seçili zaman aralığında toplanan aldığınız kaç tane olay bildirir ve bunu önceki 24 saate karşılaştırır. Araç bu olayları belirtir (küçük sayı temsil değişimini son 24 saat) olan uyarılar tetiklenir ve bu olayları için ne kadar sürüyor, açık ve kapalı olduğunu size bildirir. Gücündeki büyük artış olmadığını bakın veya bırak olayların sayısını denetleyin. Bir bırakma varsa, bağlantı Azure Gözcü için raporlama durdurulmuş olabilir. Bir artış olduğunda, şüpheli bir sorun meydana gelmiş olabilir. Yeni uyarılar olup olmadığını denetleyin.

   ![Azure Sentinel Huni](./media/qs-get-visibility/funnel.png)

Genel Bakış sayfası ana gövdesi, çalışma alanınızın güvenlik durumuna bir bakışta hakkında bilgi sağlar:

- **Olayları ve Uyarıları zaman içinde**: Olayları ve bu olayların oluşturulan kaç uyarı sayısını listeler. Varsa olağan dışı bir şey burada olayları bir depo bulunmamaktadır, ancak uyarılar görmüyor uyarılar de - görmelisiniz, olağan dışı bir depo görürseniz endişeye neden olabilir.

- **Olası kötü amaçlı olayları**: Kötü amaçlı olduğu bilinen kaynaklardan gelen trafiği algılandığında, Azure Gözcü haritada uyarır. Turuncu görürseniz, gelen trafiği olduğu: birisi bir bilinen kötü amaçlı IP adresinden kuruluşunuz erişmeye çalışıyor. Giden (kırmızı) etkinlik görürseniz, ağ üzerinden veri bilinen kötü amaçlı IP adresine, kuruluşunuzun dışında akıtılan anlamına gelir.

   ![Azure Sentinel harita](./media/qs-get-visibility/map.png)


- **Son çalışmaları**: Son çalışmalarınızı, önem derecesine ve örneği ile ilişkili uyarıların sayısını görüntülemek için. Belirli bir uyarı türünde ani yoğun olarak görürseniz, şu anda çalışan etkin bir saldırı olduğunu gelebilir. Azure ATP 20 Pass--hash olayları ani bir tepe varsa, örneğin, birisinin şu anda, saldırı çalıştığının mümkündür.

- **Veri kaynağı anomalileri**: Microsoft'un veri analistleri, sürekli anormallikleri için veri kaynaklarından alınan verileri arama modelleri oluşturuldu. Anomalileri yoksa hiçbir şey görüntülenmez. Algılanan anomalileri, derin gereken ne olduğunu görmek için bunları inceleyin. Örneğin, Azure etkinlik artış tıklayın. Tıklayabilirsiniz **grafik** depo gerçekleştiğinde görebilir ve daha sonra depo nedenini görmek için ilgili süre içinde gerçekleşen etkinlikler için filtre.

   ![Azure Sentinel harita](./media/qs-get-visibility/anomolies.png)

## Yerleşik panolarını kullanın<a name="dashboards"></a>

Yerleşik panolar, bu hizmetleri oluşturulan olaylar tümleşik verilerden ayrıntılı izin vermek için bağlı veri kaynakları derinlerine sağlar. Azure kimliği, Azure etkinlik olayları ve Windows olaylarını verilerden sunuculardan birinci taraf uyarıları, güvenlik duvarı trafik günlüklerini, Office 365 ve Windows üzerinde temel güvensiz protokolleri dahil olmak üzere herhangi bir üçüncü taraf olabilen şirket içinde yerleşik panoları içerir olaylar.

1. Altında **ayarları**seçin **panolar**. Altında **yüklü**, yüklü tüm panolarınıza görebilirsiniz. Altında **tüm** yüklenmeye hazır yerleşik panoları tüm Galerisi görebilirsiniz. 
2. Arama tam listesi ve açıklaması görmek belirli bir Pano için her bir sunar. 
3. Varsayılmıştır başlamak için Azure AD'yi kullanın ve Azure Gözcü ile çalışan, en az aşağıdaki panolar yüklemeniz önerilir:
   - **Azure AD**: Birindeki veya her birini kullanın:
       - **Azure AD oturum açma işlemleri** anomalileri olup olmadığını görmek için zaman içinde oturum açma işlemleri analiz eder. Bu pano, olağan dışı bir şey olursa, bir bakışta fark, böylece uygulamalar, cihazlar ve konumları, başarısız oturum açma işlemleri sağlar. Birden çok başarısız oturum açma işlemleri dikkat edin. 
       - **Azure AD denetim günlükleri** kullanıcılar değişiklikleri gibi yönetici etkinlikleri analiz eder (Ekle, Kaldır, vb.), grup oluşturma ve değişiklikler.  

   - Güvenlik duvarınızı için bir Pano ekleyin. Örneğin, Palo Alto Pano ekleyin. Pano, çözümler, güvenlik duvarı veri ve tehdit olayları arasındaki bağıntıları sağlayan güvenlik duvarı trafiğiniz ve şüpheli olayları varlıklar arasında vurgular. Panolar trafiğiniz eğilimler hakkında bilgi sağlar ve incelemek ve sonuçlara filtre olanak tanır. 

      ![PAL Alto Panosu](./media/qs-get-visibility/palo-alto-week-query.png)


Ana sorguda düzenleyerek panolar özelleştirebilirsiniz ![düğmesi](./media/qs-get-visibility/edit-query-button.png). Düğmeyi tıklatabilirsiniz ![düğmesi](./media/qs-get-visibility/go-to-la-button.png) gitmek için [var. sorguyu düzenlemek için Log Analytics](../azure-monitor/log-query/get-started-portal.md), nokta (...) seçin ve seçin ve **kutucuk verilerini Özelleştir**, sağlar Ana zaman filtresini Düzenle veya özel kutucuklar panodan kaldırmak için.

Sorgular ile çalışma hakkında daha fazla bilgi için bkz. [Öğreticisi: Log analytics'te görsel verileri](../azure-monitor/learn/tutorial-logs-dashboards.md)

### <a name="add-a-new-tile"></a>Yeni bir kutucuk Ekle

Yeni bir kutucuk eklemek istiyorsanız, mevcut bir panoya için oluşturduğunuz bir veya bir Azure Gözcü yerleşik Pano ekleyebilirsiniz. 
1. Log Analytics'te bulunan yönergeleri kullanarak kutucuk oluşturma [Öğreticisi: Görsel verileri Log analytics'te](../azure-monitor/learn/tutorial-logs-dashboards.md). 
2. Kutucuğu altında oluşturulduktan sonra **PIN**, kutucuğu görünmesini istediğiniz panoyu seçin.

## <a name="create-new-dashboards"></a>Yeni panolar oluşturun
Yerleşik bir pano, yeni panonuz için temel olarak kullanmak ya da sıfırdan yeni bir pano oluşturun.

1. Sıfırdan yeni bir Pano oluşturmak için seçin **panolar** ardından **+ yeni Pano**.
2. Pano içinde oluşturduğunuz aboneliği seçin ve açıklayıcı bir ad verin. Her bir pano gibi başka bir Azure kaynağıdır ve rolleri (RBAC) tanımlamak için ve kimin erişebileceğini sınırı atayabilirsiniz. 
3. Panolarınızdaki görselleştirmeleri için görünmesini sağlamak için paylaşabilmesi sahip. Tıklayın **paylaşımı** ardından **kullanıcıları yönetme**. 
 
1. Kullanım **denetleyin erişim** ve **rol atamaları** diğer Azure kaynakları için yaptığınız gibi. Daha fazla bilgi için [RBAC kullanarak paylaşım Azure panoları](../azure-portal/azure-portal-dashboard-share-access.md).


## <a name="new-dashboard-examples"></a>Yeni Pano örnekleri

Aşağıdaki örnek sorguda, hafta eğilimleri trafik karşılaştırmanızı sağlar. Hangi cihaz satıcısı ve veri kaynağı, sorguyu çalıştırmak kolayca geçiş yapabilirsiniz. Bu örnekte Windows SecurityEvent, üzerinde başka bir güvenlik duvarı AzureActivity veya CommonSecurityLog çalışacak şekilde geçiş yapabilirsiniz.

     |where DeviceVendor = = "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Birden çok kaynaktan alınan verileri içeren bir sorgu oluşturmak isteyebilirsiniz. Yeni oluşturduğunuz yeni kullanıcılar ve kullanıcı oluşturma, 24 saat içinde rol ataması değişiklik başlatıldığını görmek için Azure oturumu denetimleri için Azure Active Directory denetim günlüklerini bakan bir sorgu oluşturabilirsiniz. Bu şüpheli etkinliğin Bu panodaki gösterilir:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

Veriler ve aradıklarını bakarak kişinin rolüne dayalı farklı Pano oluşturabilirsiniz. Örneğin, güvenlik duvarı verileri içeren ağ yöneticiniz için bir Pano oluşturabilirsiniz. Her gün gözden geçirmek istediğiniz şeyleri olup ne sıklıkta, onları, aramak istediğinize bağlı panolar oluşturabilirsiniz ve diğerleri için kullanmak istediğiniz öğeleri saatte bir denetle, örneğin, Azure AD oturum açma işlemlerinin her saat için anomali aramak için bakmak isteyebilirsiniz Es. 

## <a name="create-new-detections"></a>Yeni algılamalar oluşturma

Üzerinde algılamalar oluşturmak [Azure Gözcü için bağlı veri kaynakları](connect-data-sources.md) kuruluşunuzdaki tehditleri araştırmak için.

Yeni bir algılama oluşturduğunuzda, bağlı veri kaynaklarına uyarlanmış Microsoft Güvenlik Araştırmacıları tarafından hazırlanmış yerleşik algılamalar yararlanın.

1. [GitHub topluluğuna](https://github.com/Azure/Azure-Sentinel/tree/master/Detections) Git **Algılamalar** klasör ve ilgili klasörleri seçin.
   ![ilgili klasörleri](./media/qs-get-visibility/detection-folders.png)
 
3.  Git **Analytics** sekmenize **ekleme**.
   ![Log Analytics'te kuralı oluşturma](./media/qs-get-visibility/query-params.png)

3.  Tüm parametreler kuralı kopyalamak ve tıklayın **Oluştur**.
   ![Uyarı kuralı oluşturma](./media/qs-get-visibility/create-alert-rule.png)

 
## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Gözcü kullanmaya başlama öğrendiniz. Öğreticiye devam [tehditleri algılamak nasıl](tutorial-detect-threats.md).
> [!div class="nextstepaction"]
> [Tehditleri algılamak](tutorial-detect-threats.md) tehditleri verdiğiniz yanıtları otomatik hale getirmek için.

