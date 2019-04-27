---
title: BT Hizmet Yönetimi Bağlayıcısı Azure Log analytics'te | Microsoft Docs
description: Bu makale BT Hizmet Yönetimi Bağlayıcısı (ITSMC) genel bakış sağlar ve bu çözümü merkezi olarak izlemeye ve ITSM yönetmek için nasıl kullanılacağı hakkında bilgi Azure Log Analytics'te iş öğeleri ve sorunları hızla çözün.
services: log-analytics
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.openlocfilehash: abbd26779cefaf52c6f2247a5d27db25f280c930
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60395875"
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Azure BT Hizmet Yönetimi Bağlayıcısı'nı kullanarak ITSM araçlara bağlayın

![BT Hizmet Yönetimi Bağlayıcısı simgesi](media/itsmc-overview/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı'nı (ITSMC), Azure ve desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmet bağlanmanızı sağlar.

Log Analytics ve Azure İzleyici gibi Azure hizmetlerinin algılayın, çözümleyin ve Azure ve Azure dışı kaynaklar ile ilgili sorunları gidermek için araçlar sağlar. Ancak, genellikle bir sorunla ilgili iş öğelerini bir ITSM ürün/hizmetini yer alır. ITSM Bağlayıcısı sorunlarını daha hızlı çözmenize yardımcı olması için Azure ve ITSM araçları arasında çift yönlü bağlantı sağlar.

Bağlantılar aşağıdaki ITSM araçları ile ITSMC destekler:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

ITSMC ile şunları yapabilirsiniz:

-  ITSM aracına, Azure, uyarılara (ölçüm Uyarısı, etkinlik günlüğü uyarılarına ve Log Analytics uyarılarını) bağlı iş öğeleri oluşturun.
-  İsteğe bağlı olarak, olayınız eşitleme ve istek verileri bir Azure Log Analytics çalışma alanına ITSM aracınızın değiştirin.


ITSM Bağlayıcısı aşağıdaki adımları kullanarak kullanmaya başlayabilirsiniz:

1.  [ITSM Bağlayıcısı çözümü ekleme](#adding-the-it-service-management-connector-solution)
2.  Bir ITSM bağlantısı oluşturun
3.  [Bağlantı kullan](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Hizmet Yönetimi bağlayıcı çözümü BT ekleme

Bir bağlantı oluşturmadan önce ITSM Bağlayıcısı çözümü eklemeniz gerekir.

1. Azure portalında **+ yeni** simgesi.

   ![Yeni bir Azure kaynak](media/itsmc-overview/azure-add-new-resource.png)

2. Arama **BT Hizmet Yönetimi Bağlayıcısı** Market tıklayıp **Oluştur**.

   ![ITSMC çözümü ekleme](media/itsmc-overview/add-itsmc-solution.png)

3. İçinde **OMS çalışma alanı** bölümünde, istediğiniz çözümü yüklemek için Azure Log Analytics çalışma alanını seçin.
   >[!NOTE]
   >Azure İzleyici sürekli geçiş Microsoft Operations Management Suite (OMS) gelen bir parçası olarak, OMS çalışma alanları artık için Log Analytics çalışma alanları bilinir.
4. İçinde **OMS çalışma alanı ayarlarını** bölümünde, istediğiniz çözüm kaynağının oluşturulacağı kaynak grubu seçin.

   ![ITSMC çalışma](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Azure İzleyici sürekli geçiş Microsoft Operations Management Suite (OMS) gelen bir parçası olarak, OMS çalışma alanları artık için Log Analytics çalışma alanları bilinir.

5. **Oluştur**’a tıklayın.

Çözüm kaynak dağıtıldığında en üstünde bir bildirim görüntülenir pencerenin sağ.


## <a name="creating-an-itsm--connection"></a>ITSM bağlantısı oluşturma

Çözüm yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bir bağlantı oluşturmak için ITSM Bağlayıcısı çözümünden bağlantısına izin vermek için bir ITSM aracına hazırlık gerekir.  

Bağlanmakta olduğunuz ITSM ürün bağlı olarak, aşağıdaki adımları kullanın:

- [System Center Service Manager (SCSM)](../../azure-monitor/platform/itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](../../azure-monitor/platform/itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](../../azure-monitor/platform/itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](../../azure-monitor/platform/itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

ITSM araçlarınıza prepped sonra bir bağlantı oluşturmak için aşağıdaki adımları izleyin:

1. Git **tüm kaynakları**, Aranan **ServiceDesk(YourWorkspaceName)**.
2. Altında **çalışma alanı veri kaynakları** sol bölmesinden **ITSM bağlantıları**.
   ![ITSM bağlantıları](media/itsmc-overview/itsm-connections.png)

   Bu sayfa, bağlantıların listesini görüntüler.
3. **Bağlantı Ekle**'ye tıklayın.

   ![ITSM Bağlantısı Ekle](media/itsmc-overview/add-new-itsm-connection.png)

4. Bağlantı ayarlarını anlatıldığı gibi belirtin [ITSM ürünler/hizmetler Makalenizi ITSMC bağlantı yapılandırılırken](../../azure-monitor/platform/itsmc-connections.md).

   > [!NOTE]
   > 
   > Varsayılan olarak, ITSMC bağlantının yapılandırma verileri 24 saatte bir kez yenilenir. Herhangi bir düzenleme veya şablon güncelleştirmelerinin için yaptığınız instantly, bağlantının verileri yenilemek için şuna tıklayın **eşitleme** , bağlantının dikey penceresinde düğmesi.

   ![Bağlantıyı yenileme](media/itsmc-overview/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Çözümü kullanma
   ITSM Bağlayıcısı çözümü kullanarak, iş öğelerini Azure uyarılar, Log Analytics uyarılarını ve Log Analytics günlük kayıtlarını oluşturabilirsiniz.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarılarından ITSM iş öğeleri oluşturma

ITSM bağlantınız oluşturulan aldıktan sonra iş öğelerini kullanarak Azure uyarıları üzerinde temel alan, bir ITSM aracına oluşturabileceğiniz **ITSM eylem** içinde **Eylem grupları**.

Eylem grupları Azure uyarıları eylemleri tetikleyen, modüler ve yeniden kullanılabilir bir yol sağlar. Ölçüm Uyarısı, etkinlik günlüğü uyarılarına ve Azure Portal'da Azure Log Analytics uyarılarını ile Eylem grupları kullanabilirsiniz.

Aşağıdaki yordamı kullanın:

1. Azure portalında **İzleyici**.
2. Sol bölmede **Eylem grupları**. **Eylem grubu Ekle** penceresi görüntülenir.

    ![Eylem Grupları](media/itsmc-overview/action-groups.png)

3. Sağlamak **adı** ve **ShortName** eylem grubunuz için. Seçin **kaynak grubu** ve **abonelik** istediğiniz eylem grubunuzu oluşturmak.

    ![Eylem grupları ayrıntıları](media/itsmc-overview/action-groups-details.png)

4. Eylemler listesinde **ITSM** açılan menüsünden **eylem türü**. Sağlayan bir **adı** tıklatın ve eylemi için **Ayrıntıları Düzenle**.
5. Seçin **abonelik** Log Analytics çalışma alanınızın bulunduğu. Seçin **bağlantı** , çalışma alanı adından (ITSM Bağlayıcısı adı). Örneğin, "MyITSMMConnector(MyWorkspace)."

    ![ITSM eylem ayrıntıları](media/itsmc-overview/itsm-action-details.png)

6. Seçin **iş öğesi** aşağı açılan menüden türü.
   ITSM ürününüzü gerekli alanları doldurun veya mevcut bir şablonu kullanmak bu seçeneği seçin.
7. **Tamam** düğmesine tıklayın.

Azure bir uyarı kuralı oluşturma veya düzenleme, bir ITSM eylemi olan bir eylem grubu kullanın. Uyarı tetiklendiğinde ITSM Aracı'nda oluşturulan/güncelleştirilen iş öğesi.

> [!NOTE]
> 
> ITSM eylemi fiyatlandırması hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/) Eylem grupları için.


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Görselleştirme ve olayı çözümlemek ve istek verileri değiştirme

Bağlantı kurma ayarlarken yapılandırmanızı temel alarak, ITSM Bağlayıcısı, en fazla 120 gün olay ve değişiklik isteği veri eşitleyebilirsiniz. Bu veriler için günlük kaydı şema sağlanan [sonraki bölümde](#additional-information).

Olay ve değişiklik isteği verileri, ITSM Bağlayıcısı panoyu çözümde kullanarak görselleştirilebilir.

![Log Analytics ekranı](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Pano bağlantıları ile ilgili tüm sorunları analiz etmek için bir başlangıç noktası olarak kullanılabilecek Bağlayıcısı durumu hakkında bilgi de sağlar.

Etkilenen bilgisayarlar, hizmet eşlemesi çözümünü içinde karşı eşitlenen olayları da görselleştirebilirsiniz.

Hizmet eşlemesi, otomatik olarak Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini bulur ve hizmetler arasındaki iletişimi eşler. Bunları – kritik hizmetleri sunan birbirine sistemleri düşündüğünüz sunucularınızın görüntülemenize olanak sağlar. Hizmet eşlemesi, işlemler, sunucular arasında bağlantılar gösterir ve bağlantı noktalarını yapılandırma TCP bağlantılı mimarisiyle arasında bir aracı yüklemesini dışındaki gerekli. [Daha fazla bilgi edinin](../../azure-monitor/insights/service-map.md).

Hizmet eşlemesi çözümünü kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi ITSM çözümleriyle oluşturulan hizmet Masası öğeleri görüntüleyebilirsiniz:

![Log Analytics ekranı](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

Daha fazla bilgi: [Hizmet Eşlemesi](../../azure-monitor/insights/service-map.md)


## <a name="additional-information"></a>Ek bilgiler

### <a name="data-synced-from-itsm-product"></a>ITSM üründen eşitlenmiş verileri
Olaylar ve değişiklik istekleri, ITSM üründen Log Analytics çalışma alanınıza bağlantının yapılandırmasına göre eşitlenir.

ITSMC tarafından toplanan veri örnekleri aşağıdaki bilgileri gösterir:

> [!NOTE]
> 
> İş öğesi türüne bağlı olarak içeri Log Analytics'e **ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **Olaylar**  
ServiceDeskWorkItemType_s="Incident"

**Alanları**

- ServiceDeskConnectionName
- Hizmet Masası Kimliği
- Durum
- Aciliyet
- Etki
- Öncelik
- Önem Yükseltme
- Oluşturan
- Çözümleyen
- Kapatma tarihi
- Kaynak
- Atanan
- Kategori
- Unvan
- Açıklama
- Oluşturma Tarihi
- Kapatılma tarihi
- Çözüm Tarihi
- Son Değişiklik Tarihi
- Computer


**İş öğesi:** **Değişiklik istekleri**

ServiceDeskWorkItemType_s="ChangeRequest"

**Alanları**
- ServiceDeskConnectionName
- Hizmet Masası Kimliği
- Oluşturan
- Kapatma tarihi
- Kaynak
- Atanan
- Unvan
- Tür
- Kategori
- Durum
- Önem Yükseltme
- Çakışma durumu
- Aciliyet
- Öncelik
- Risk
- Etki
- Atanan
- Oluşturma Tarihi
- Kapatılma tarihi
- Son Değişiklik Tarihi
- İstenen tarih
- Planlanan başlangıç tarihi
- Planlanan bitiş tarihi
- İş başlangıç tarihi
- İş bitiş tarihi
- Açıklama
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Çıktı verileri için ServiceNow olayı

| Log Analytics alan | ServiceNow alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Tarafından açılmış |
| ResolvedBy_s | Çözen:|
| ClosedBy_s  | Kapatma tarihi |
| Source_s| Kişi türü |
| AssignedTo_s | Atanan:  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Açıldı |
| ClosedDate_t| Kapalı|
| ResolvedDate_t|Çözümlendi|
| Computer  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>Çıktı verilerini bir ServiceNow için değişiklik isteği

| Log Analytics | ServiceNow alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteği gönderen: |
| ClosedBy_s | Kapatma tarihi |
| AssignedTo_s | Atanan:  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Risk|
| Impact_s| Etki|
| RequestedDate_t  | Tarihe göre istendi |
| ClosedDate_t | Kapatılma tarihi |
| PlannedStartDate_t  |     Planlanan başlangıç tarihi |
| PlannedEndDate_t  |   Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Açıklama |
| Computer  | Yapılandırma öğesi |


## <a name="troubleshoot-itsm-connections"></a>ITSM bağlantılarında sorun giderme
1. Bağlantılı kaynağın kullanıcı arabiriminden ile bağlantı başarısız olursa bir **bağlantı kaydetme hatası** iletisi, aşağıdaki adımları uygulayın:
   - Servicenow'ı ve Cherwell Provance bağlantıları  
   - doğru kullanıcı adı, parola, istemci Kimliğini ve istemci gizli anahtarı bağlantıların her biri için girdiğiniz emin olun.  
   - karşılık gelen ITSM ürününde bağlantı kurmak için yeterli ayrıcalıklara sahip olup olmadığını denetleyin.  
   - Service Manager bağlantıları için  
   - Web uygulaması başarıyla dağıtılır ve karma bağlantı oluşturuldu emin olun. Şirket içi Service Manager makineyle bağlantı kurulan başarıyla doğrulamak için Web uygulaması URL'si için yapma belgelerinde açıklandığı ziyaret [karma bağlantı](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection).  

2. ServiceNow verileri Log Analytics'e eşitlenmediğinden, ServiceNow örneği değil uyku emin olun. Servicenow'ı geliştirme örnekleri, bazen boştayken uzun bir süre için uyku moduna geçer. Aksi takdirde, sorunu bildirin.
3. Log Analytics uyarılarını yangın ancak iş öğeleri ITSM ürününde oluşturulmamış veya yapılandırma öğeleri oluşturulan/herhangi diğer genel bilgi için aşağıdaki konumlarda bakın veya iş öğelerine bağlı değildir:
   -  ITSMC: Çözüm öğeleri bağlantıları/iş/vb. bilgisayarların özetini gösterir. Kutucuk gösteren tıklayın **Bağlayıcısı durumu**, size aldığı **günlük araması** ile ilgili sorgu. Günlük kayıtları LogType_S ile daha fazla bilgi için hata olarak arayın.
   - **Günlük arama** sayfası: doğrudan sorgu kullanarak hataları ve ilgili bilgileri görüntüleyin `*`ServiceDeskLog_CL`*`.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Service Manager Web uygulama dağıtım sorunlarını giderme
1.  Web uygulaması dağıtımı ile ilgili olması durumunda, kaynakları oluşturun/dağıtmak için belirtilen abonelikte yeterli izinlere sahip olun.
2.  Alırsanız bir **"Nesnesinin bir nesnenin örneğine ayarlı değil başvuru"** çalıştırdığınızda hata [betik](itsmc-service-manager-script.md), altında geçerli değerler girdiğinizden emin olun **Kullanıcı Yapılandırması** bölümü .
3.  Service bus geçiş ad alanı oluşturmak başarısız olursa, abonelikte gereken kaynak sağlayıcısı kayıtlı olduğundan emin olun. Kayıtlı değil, aynı zamanda Azure portalından el ile service bus geçiş ad alanı oluşturun. Bunu çalışırken de oluşturabilirsiniz [karma bağlantı oluşturma](../../azure-monitor/platform/itsmc-connections.md#configure-the-hybrid-connection) Azure portalından.


## <a name="contact-us"></a>Bizimle iletişim kurun

Tüm sorguları veya BT Hizmet Yönetimi Bağlayıcısı geribildirim için bizimle iletişime geçin [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar
[BT Hizmet Yönetimi Bağlayıcısı için ITSM ürün veya hizmetlerden ekleme](../../azure-monitor/platform/itsmc-connections.md).
