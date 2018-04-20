---
title: BT Hizmet Yönetimi Bağlayıcısı Azure günlük analizi | Microsoft Docs
description: Bu çözüm merkezi olarak izlemek ve ITSM yönetmek için nasıl kullanılacağı hakkında bilgi Azure günlük analizi çalışma öğeleri ve hızlı bir şekilde tüm sorunları giderin ve bu makalede BT Hizmet Yönetimi Bağlayıcısı (ITSMC) genel bakış sağlar.
services: log-analytics
documentationcenter: ''
author: JYOTHIRMAISURI
manager: riyazp
editor: ''
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: c39cf464a7e838fecf7ebd4a3cbb08612388a5fa
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı'nı kullanarak ITSM araçları Azure connect

![BT Hizmet Yönetimi Bağlayıcısı simgesi](./media/log-analytics-itsmc/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı'nı (ITSMC) Azure ve desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmet bağlamanıza olanak sağlar.

Günlük analizi ve Azure İzleyicisi gibi Azure hizmetlerini algılamak, çözümlemek ve Azure ve Azure olmayan kaynakları ile ilgili sorunları gidermek için araçlar sağlar. Ancak, genellikle bir sorunla ilgili iş öğeleri bir ITSM ürün/hizmetini bulunur. ITSM bağlayıcı sorunları daha hızlı çözmenize yardımcı olmak için Azure ve ITSM araçları arasında çift yönlü bağlantı sağlar.

ITSMC aşağıdaki ITSM araçları ile bağlantılarını destekler:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

ITSMC ile şunları yapabilirsiniz

-  Azure, uyarılar (ölçüm uyarılar, etkinlik günlüğü uyarıları ve günlük analizi uyarılarını) temelinde ITSM aracında iş öğelerini oluşturabilir.
-  İsteğe bağlı olarak, olay eşitleyin ve istek verileri Azure günlük analizi çalışma alanına ITSM aracından değiştirin.


Aşağıdaki adımları kullanarak ITSM Bağlayıcısı'nı kullanarak başlatabilirsiniz:

1.  [ITSM bağlayıcı çözüme ekleyin](#adding-the-it-service-management-connector-solution)
2.  [Bir ITSM bağlantısı oluşturun](#creating-an-itsm-connection)
3.  [Bağlantısını kullan](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Hizmet Yönetimi Bağlayıcısı çözümü BT ekleme

Bir bağlantı oluşturmadan önce ITSM bağlayıcı çözüm eklemeniz gerekir.

1.  Azure portalında tıklatın **+ yeni** simgesi.

    ![Azure yeni kaynak](./media/log-analytics-itsmc/azure-add-new-resource.png)

2.  Arama **BT Hizmet Yönetimi Bağlayıcısı** tıklatın ve Market **oluşturma**.

    ![ITSMC çözümü ekleyin](./media/log-analytics-itsmc/add-itsmc-solution.png)

3.  İçinde **OMS çalışma** bölümünde, çözümü yüklemek istediğiniz Azure günlük analizi çalışma alanı seçin.
4.  İçinde **OMS çalışma alanı ayarları** bölümünde, istediğiniz çözüm kaynak oluşturmak için kaynak grubu seçin.

    ![ITSMC çalışma](./media/log-analytics-itsmc/itsmc-solution-workspace.png)

5.  **Oluştur**’a tıklayın.

Çözüm kaynak dağıtıldığında en üstünde bir bildirim görüntülenir pencerenin sağ.


## <a name="creating-an-itsm--connection"></a>Bir ITSM bağlantı oluşturma

Çözüm yükledikten sonra bir bağlantı oluşturabilirsiniz.

Bir bağlantı oluşturmak için ITSM bağlayıcı çözümden bağlantıya izin vermek için ITSM aracınızı prep gerekecektir.  

Bağlanmakta olduğunuz ITSM ürün bağlı olarak, aşağıdaki adımları kullanın:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

ITSM araçlarınızı prepped sonra bir bağlantı oluşturmak için aşağıdaki adımları izleyin:

1.  Git **tüm kaynakları**, Ara **ServiceDesk(YourWorkspaceName)**.
2.  Altında **çalışma veri kaynakları** sol bölmede **ITSM bağlantıları**.
    ![ITSM bağlantıları](./media/log-analytics-itsmc/itsm-connections.png)

    Bu sayfa bağlantıların listesini görüntüler.
3.  Tıklatın **Bağlantı Ekle**.

    ![ITSM Bağlantısı Ekle](./media/log-analytics-itsmc/add-new-itsm-connection.png)

4.  Bölümünde açıklandığı gibi bağlantı ayarlarını belirtin [ITSM ürünler/hizmetler Makalenizi ile ITSMC bağlantı yapılandırılırken](log-analytics-itsmc-connections.md).

    > [!NOTE]

    > Varsayılan olarak, ITSMC 24 saatte bir kez bağlantının yapılandırma verileri yeniler. Hemen tüm düzenlemeler veya şablon için bağlantının verileri yenilemek için bağlantınızı yanında görüntülenen "Yenile" düğmesini tıklatın yaptığınız güncelleştirir.

    ![Bağlantı yenileme](./media/log-analytics-itsmc/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Çözümü kullanma
   ITSM bağlayıcı çözümünü kullanarak, iş öğelerini Azure uyarılar, günlük analizi uyarılarını ve günlük analizi günlük kayıtlarını oluşturabilirsiniz.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarıları ITSM iş öğeleri oluşturma

Oluşturulan ITSM bağlantınızı olduktan sonra iş öğeleri kullanarak Azure uyarılar, temelinde ITSM aracınızı oluşturabileceğiniz **ITSM eylem** içinde **Eylem grupları**.

Eylem grupları Azure uyarılarınızı eylemleri tetikleyen, modüler ve yeniden kullanılabilir bir yolunu sağlar. Ölçüm uyarılar, etkinlik günlüğü uyarıları ve Azure Portal'da Azure günlük analizi uyarılarını eylem gruplarını kullanabilirsiniz.

Aşağıdaki yordamı kullanın:

1. Azure portalında tıklatın **İzleyici**.
2. Sol bölmede **Eylem grupları**. **Eylem Grup Ekle** penceresi görüntülenir.

    ![Eylem Grupları](media/log-analytics-itsmc/action-groups.png)

3. Sağlamak **adı** ve **kısaad** eylem grubunuz için. Seçin **kaynak grubu** ve **abonelik** eylem grubu oluşturmak istediğiniz.

    ![Eylem grupları ayrıntısı](media/log-analytics-itsmc/action-groups-details.png)

4. Eylemler listesinde **ITSM** için aşağı açılır menüden **eylem türü**. Sağlayan bir **adı** tıklatın ve eylemi için **Düzenle ayrıntıları**.
5. Seçin **abonelik** günlük analizi çalışma alanınız bulunduğu. Seçin **bağlantı** , çalışma alanı adından (ITSM bağlayıcı adı). Örneğin, "MyITSMMConnector(MyWorkspace)."

    ![ITSM eylemi ayrıntıları](./media/log-analytics-itsmc/itsm-action-details.png)

6. Seçin **iş öğesi** açılır menüsünden türü.
   ITSM ürününüzü tarafından gerekli alanları doldurun veya varolan bir şablonu kullanmak üzere seçin.
7. **Tamam**’a tıklayın.

Azure uyarı kuralı oluşturma/düzenleme yaparken ITSM eylemi olan bir eylem grubu kullanın. Uyarıyı tetikleyen, iş öğesi oluşturulan/ITSM aracı güncelleştirilmiştir.

>[!NOTE]

> ITSM eylemi fiyatlandırma hakkında daha fazla bilgi için bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/) Eylem grupları için.


## <a name="create-itsm-work-items-from-log-analytics-alerts"></a>Günlük analizi uyarılardan ITSM iş öğeleri oluşturma

Uyarı kuralları, aşağıdaki yordamı kullanarak iş öğelerini ITSM aracında oluşturmak için Azure günlük analizi portalında yapılandırabilirsiniz.

1. Gelen **günlük arama** penceresinde verileri görüntülemek için bir günlük arama sorgusunu çalıştırın. Sorgu sonuçları, iş öğeleri için kaynaktır.
2. İçinde **günlük arama**, tıklatın **uyarı** açmak için **uyarı kuralı Ekle** sayfası.

    ![Günlük analizi ekranı](./media/log-analytics-itsmc/itsmc-work-items-for-azure-alerts.png)

3. Üzerinde **uyarı kuralı Ekle** penceresinde sağlamak için gerekli ayrıntıları **adı**, **önem**, **arama sorgusu**, ve **uyarı Ölçüt** (zaman penceresi/ölçü ölçüm).
4. Seçin **Evet** için **ITSM Eylemler**.
5. ITSM bağlantınızı seçin **seçin bağlantı** listesi.
6. Gerekli ayrıntıları belirtin.
7. Bu uyarının her günlük girişinin için ayrı iş öğesi oluşturmak için seçin **her günlük girişinin için tek tek iş öğeleri oluşturma** onay kutusu.

    Veya

    Bu onay kutusunu günlük girişlerini bu uyarı altında herhangi bir sayıda için yalnızca bir iş öğesi oluşturmak için seçili bırakın.

7. **Kaydet**’e tıklayın.

Altında oluşturulan günlük analizi uyarı görüntüleyebilirsiniz **ayarlar > Uyarılar**. Belirtilen uyarının koşulu karşılandığında karşılık gelen ITSM bağlantı çalışma öğeleri oluşturulur.


## <a name="create-itsm-work-items-from-log-analytics-log-records"></a>Günlük analizi günlük kayıtlarından ITSM iş öğeleri oluşturma

İş öğelerini bağlı ITSM kaynaklardan doğrudan bir günlük kaydı oluşturabilirsiniz. Bu bağlantının düzgün çalışıp çalışmadığını sınamak için kullanılabilir.


1. Gelen **günlük arama**, gerekli verileri arama, ayrıntı seçin ve tıklatın **oluşturma çalışma öğesini**.

    **ITSM iş öğesi oluşturma** penceresi görüntülenir:

    ![Günlük analizi ekranı](media/log-analytics-itsmc/itsmc-work-items-from-azure-logs.png)

2.   Aşağıdaki ayrıntıları ekleyin:

  - **İş öğesi başlık**: iş öğesi için başlık.
  - **İş öğesi tanımı**: yeni çalışma öğesi için bir açıklama.
  - **Bilgisayar etkilenen**: Bu günlük verileri nerede bulundu bilgisayarın adı.
  - **Bağlantıyı seçin**: Bu iş öğesi oluşturmak istediğiniz ITSM bağlantı.
  - **İş öğesi**: iş öğesi türü.

3. Bir olay için var olan bir iş öğesi şablonunu kullanmak için tıklatın **Evet** altında **oluşturmak iş öğesi şablona dayalı** seçeneğini ve ardından **oluşturma**.

    Veya

    Tıklatın **Hayır** özelleştirilmiş değerlerinizi sağlamak istiyorsanız.

4. Uygun değerleri sağlayın **kişi türündeki**, **etkisi**, **aciliyet**, **kategori**, ve **alt kategori** metin kutuları ve ardından **oluşturma**.


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Görselleştirme ve olayı çözümlemek ve istek verileri değiştirme

Bağlantı kurma ayarlarken yapılandırmanızı temel alarak, ITSM bağlayıcı en fazla 120 gün olay ve değişiklik isteği verileri eşitleyebilirsiniz. Bu veri günlük kaydı şeması sağlanan [sonraki bölümde](#additional-information).

Olay ve değişiklik isteği verilerini çözümde ITSM bağlayıcı Panoyu kullanarak canlandırılabilir.

![Günlük analizi ekranı](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

Pano ayrıca bağlantıları ile ilgili tüm sorunları çözümlemek için bir başlangıç noktası olarak kullanılabilecek bağlayıcı durumu hakkında bilgiler sağlar.

Ayrıca, etkilenen bilgisayarların hizmet Haritası çözümünde karşı eşitlenen olaylar görselleştirebilirsiniz.

Hizmet eşlemesi otomatik olarak sistemlerde, Windows ve Linux uygulama bileşenleri bulur ve Hizmetleri arasındaki iletişimi eşler. Bunları – Kritik hizmetler sunan birbirine bağlı sistemler olarak düşündüğünüz sunucularınızı görüntülemenize izin verir. Bir aracı yüklemesini dışında hiçbir yapılandırma TCP bağlı mimarisiyle arasında bağlantı noktaları gerekli ve hizmet Haritası sunucuları, işlemleri arasındaki bağlantıları gösterir. [Daha fazla bilgi edinin](../operations-management-suite/operations-management-suite-service-map.md).

Hizmet eşlemesi çözümü kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi ITSM çözümlerinde oluşturulan hizmet Masası öğeleri görüntüleyebilirsiniz:

![Günlük analizi ekranı](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)

Daha fazla bilgi: [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md)


## <a name="additional-information"></a>Ek bilgiler

### <a name="data-synced-from-itsm-product"></a>Veri ITSM üründen eşitlendi
Olaylar ve değişiklik istekleri ITSM ürününüzü bağlantının yapılandırmasına bağlı olarak günlük analizi çalışma alanınıza eşitlenir.

Aşağıdaki bilgiler ITSMC tarafından toplanan veri örnekleri gösterilmektedir:

> [!NOTE]

> İş öğesi türüne bağlı olarak günlük analizi içeri **ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **olaylar**  
ServiceDeskWorkItemType_s="Incident"

**Alanları**

- ServiceDeskConnectionName
- Hizmet Masası kimliği
- Durum
- Aciliyet
- Etki
- Öncelik
- Önem Yükseltme
- Oluşturan
- Çözümleyen
- Tarafından kapatıldı
- Kaynak
- Atanan
- Kategori
- Başlık
- Açıklama
- Oluşturma Tarihi
- Kapatılma tarihi
- Çözümlenme tarihi
- Son Değişiklik Tarihi
- Bilgisayar


**İş öğesi:** **değişiklik istekleri**

ServiceDeskWorkItemType_s="ChangeRequest"

**Alanları**
- ServiceDeskConnectionName
- Hizmet Masası kimliği
- Oluşturan
- Tarafından kapatıldı
- Kaynak
- Atanan
- Başlık
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
- Bilgisayar

## <a name="output-data-for-a-servicenow-incident"></a>ServiceNow olay için çıktı verileri

| Günlük analizi alan | ServiceNow alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Tarafından açılmış |
| ResolvedBy_s | Çözen:|
| ClosedBy_s  | Tarafından kapatıldı |
| Source_s| İlgili kişi türü |
| AssignedTo_s | Atanan:  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Opened |
| ClosedDate_t| kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>Çıktı verileri bir ServiceNow için değişiklik isteği

| Log Analytics | ServieNow alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteği gönderen: |
| ClosedBy_s | Tarafından kapatıldı |
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
| Bilgisayar  | Yapılandırma öğesi |


## <a name="troubleshoot-itsm-connections"></a>ITSM bağlantı sorunlarını giderme
1.  Bağlantılı kaynağın kullanıcı Arabirimi ile gelen bağlantı başarısız olursa bir **bağlantı kaydetmede hata** iletisi, aşağıdaki adımları uygulayın:
- ServiceNow, Cherwell ve Provance bağlantıları için  
           -doğru girdiğiniz kullanıcı adı, parola, istemci kimliği ve istemci parolası bağlantıların her biri için emin olun.  
           -karşılık gelen ITSM üründe bağlantıyı kurmak için yeterli ayrıcalıklara sahip olmadığını denetleyin.  
- Service Manager bağlantılarında  
           -Web uygulaması başarıyla dağıtılır ve karma bağlantı oluşturulur emin olun. Şirket içi Service Manager makineyle bağlantı kuran başarıyla doğrulamak için Web uygulaması URL'si yapma belgelerindeki ayrıntılı olarak ziyaret [karma bağlantı](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).  

2.  ServiceNow verileri için günlük analizi eşitlenmedi, örneği değil uykuda ServiceNow emin olun. ServiceNow geliştirme örnekleri bazen boştayken uzun bir süre için uyku moduna gidin. Aksi takdirde, sorunu bildirin.
3.  OMS uyarıları yangın ancak iş öğeleri ITSM üründe oluşturulmamış veya yapılandırma öğeleri oluşturulan/iş öğeleri veya herhangi diğer genel bilgi için aşağıdaki konumlarda aramak için bağlı değil:
 -  ITSMC: Çözüm bağlantıları/iş öğeleri/bilgisayarların vb. özetini gösterir. Döşeme gösteren tıklatın **bağlayıcı durumu**, size aldığı **günlük arama** ilgili sorgu. Günlük kayıtları LogType_S ile daha fazla bilgi için hata olarak arayın.
 - **Arama oturum** sayfa: doğrudan sorgu kullanarak hataları ve ilgili bilgileri görüntüleyin `*`ServiceDeskLog_CL`*`.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Service Manager Web uygulama dağıtım sorunlarını gider
1.  Herhangi bir sorun olması durumunda web uygulama dağıtımı, kaynakları oluşturun/dağıtmak için belirtilen abonelik yeterli izinlere sahip olun.
2.  Alırsanız bir **"Nesne başvurusu ayarlı değil bir nesne örneğine"** çalıştırdığınızda hata [betik](log-analytics-itsmc-service-manager-script.md), altında geçerli değerler girdiğinizden emin olun **Kullanıcı Yapılandırması** bölümü .
3.  Service bus geçiş ad alanı oluşturma başarısız olursa, gereken kaynak sağlayıcısı abonelikte kayıtlı olduğundan emin olun. El ile kayıtlı değil, hizmet veri yolu geçişi ad Azure portalından oluşturun. Bunu sırasında da oluşturabilirsiniz [karma bağlantı oluşturma](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) Azure portalından.


## <a name="contact-us"></a>Bizimle iletişim kurun

Tüm sorgular veya BT Hizmet Yönetimi Bağlayıcısı geribildirim için adresinden bize başvurun [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar
[BT Hizmet Yönetimi Bağlayıcısı için ITSM ürünler/hizmetler eklemek](log-analytics-itsmc-connections.md).
