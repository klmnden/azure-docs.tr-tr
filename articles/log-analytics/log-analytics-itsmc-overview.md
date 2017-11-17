---
title: "BT Hizmet Yönetimi Bağlayıcısı Azure günlük analizi | Microsoft Docs"
description: "Bu çözüm merkezi olarak izlemek ve ITSM yönetmek için nasıl kullanılacağı hakkında bilgi OMS günlük analizi çalışma öğeleri ve hızlı bir şekilde tüm sorunları giderin ve bu makalede BT Hizmet Yönetimi Bağlayıcısı (ITSMC) genel bakış sağlar."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: bd384255b3c46b3ae88b1269ab26e0ddaa6f6e77
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>ITSM iş öğelerini BT Hizmet Yönetimi Bağlayıcısı (Önizleme) kullanarak merkezi olarak yönetme

![BT Hizmet Yönetimi Bağlayıcısı simgesi](./media/log-analytics-itsmc/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı'nı (ITSMC) desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmet ve günlük analizi arasında çift yönlü tümleştirme sağlar.  Bu bağlantı günlük analizi uyarılar, günlük kayıtlarını veya Azure uyarıları göre ITSM üründeki olaylar, uyarılar ya da olaylar oluşturabilirsiniz. Bağlayıcı da olaylar gibi verileri içe aktaran ve OMS günlük analizi ITSM üründen değişiklik istekleri.

ITSMC ile şunları yapabilirsiniz:

  - Olay yönetimi uygulamalarınızı tercih ettiğiniz ITSM aracında işletimsel uyarıları tümleştirebilirsiniz.
    - İş öğeleri (örneğin, uyarı, olay, olay) içinde ITSM OMS uyarılardan ve günlük arama aracılığıyla oluşturun.
    - Azure etkinlik günlüğü uyarılarınızı Eylem grupları ITSM eylemde üzerinden bağlı iş öğeleri oluşturun.

  - İzleme, günlük ve kuruluşunuzda kullanılan hizmet yönetimi verileri birleştirin.
    - Olay ilişkilendirmek ve istek verileri ile ilgili günlük verilerini günlük analizi çalışma alanındaki tooling, ITSM değiştirin.   
    - Olaylara genel bakış için üst düzey panoları görüntülemek için değişiklik istekleri ve etkilenen sistemleri.
    - Hizmet Yönetimi veri almak için günlük analizi sorgular yazarsınız.

## <a name="adding-the-it-service-management-connector-solution"></a>Hizmet Yönetimi Bağlayıcısı çözümü BT ekleme

Açıklanan işlemi kullanarak, günlük analizi çalışma alanı BT Hizmet Yönetimi Bağlayıcısı çözüm eklemek [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

Çözümleri Galerisi'nde gördüğünüz gibi ITSMC döşeme şöyledir:

![Bağlayıcı döşeme](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Başarılı ayrıca sonra BT Hizmet Yönetimi Bağlayıcısı altında görürsünüz **OMS** > **ayarları** > **bağlı kaynakları.**

![Bağlı ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Varsayılan olarak, ITSMC 24 saatte bir kez bağlantının verileri yeniler. Hemen tüm düzenlemeler veya şablon için bağlantının verileri yenilemek için bağlantınızı yanında görüntülenen "Yenile" düğmesini tıklatın yaptığınız güncelleştirir.

 ![ITSMC Yenile](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-itsmc-connection-with-your-itsm-productsservices"></a>ITSM ürünler/hizmetlerinizi ITSMC bağlantısını yapılandırma

ITSMC destekler bağlanmasını **System Center Service Manager**, **ServiceNow**, **Provance**, ve **Cherwell**.

Aşağıdaki yordamları uygun şekilde sizin için kullanın:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Çözümü kullanma

Bağlayıcısını yapılandırdıktan sonra bağlı ITSM ürün/hizmetinden veri toplamayı başlatır. Olaylar ve değişiklik istekleri ITSM Ürün/hizmet sayısına bağlı olarak, ilk eşitleme birkaç dakika içinde tamamlanması.

> [!NOTE]
> - ITSM ürün ITSMC çözümü tarafından alınan veri türü günlük kayıtları olarak günlük analizi görünür **ServiceDesk_CL**.
> - Günlük kaydı içeren adında bir alan **ServiceDeskWorkItemType_s**, olay veya değişiklik isteği, iki tür ITSM ürün içeri veri olduğu.

## <a name="data-synced-from-itsm-product"></a>Veri ITSM üründen eşitlendi
Olaylar ve değişiklik istekleri, ITSM ürün için günlük analizi çalışma alanınız eşitlenir.
Aşağıdaki bilgiler ITSMC tarafından toplanan veri örnekleri gösterilmektedir:

> [!NOTE]

> İş öğesi türüne bağlı olarak günlük analizi içeri **ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **olaylar**  
ServiceDeskWorkItemType_s "Olay" =

**Alanları**

- ServiceDeskConnectionName
- Hizmet Masası kimliği
- Durum
- Aciliyet
- Etki
- Öncelik
- Yükseltme
- Oluşturan
- Çözümleyen
- Tarafından kapatıldı
- Kaynak
- Atanan
- Kategori
- Başlık
- Açıklama
- Oluşturulma tarihi
- Kapatılma tarihi
- Çözümlenme tarihi
- Son değiştirilme tarihi
- Bilgisayar


**İş öğesi:** **değişiklik istekleri**

ServiceDeskWorkItemType_s "ChangeRequest" =

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
- Yükseltme
- Çakışma durumu
- Aciliyet
- Öncelik
- Riski
- Etki
- Atanan
- Oluşturulma tarihi
- Kapatılma tarihi
- Son değiştirilme tarihi
- İstenen tarih
- Planlanan başlangıç tarihi
- Planlanan bitiş tarihi
- İş başlangıç tarihi
- İş bitiş tarihi
- Açıklama
- Bilgisayar

## <a name="output-data-for-a-servicenow-incident"></a>ServiceNow olay için çıktı verileri

| OMS alan | ITSM alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| IncidentState_s | Durum |
| Urgency_s |Aciliyet |
| Impact_s |Etki|
| Priority_s | Öncelik |
| CreatedBy_s | Tarafından açılmış |
| ResolvedBy_s | Çözümleyen|
| ClosedBy_s  | Tarafından kapatıldı |
| Source_s| İlgili kişi türü |
| AssignedTo_s | Atanan  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Açıldı |
| ClosedDate_t| Kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>Çıktı verileri bir ServiceNow için değişiklik isteği

| OMS alan | ITSM alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | İsteği gönderen: |
| ClosedBy_s | Tarafından kapatıldı |
| AssignedTo_s | Atanan  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Riski|
| Impact_s| Etki|
| RequestedDate_t  | Tarihe göre istendi |
| ClosedDate_t | Kapatılma tarihi |
| PlannedStartDate_t  |     Planlanan başlangıç tarihi |
| PlannedEndDate_t  |   Planlanan bitiş tarihi |
| WorkStartDate_t  | Gerçek başlangıç tarihi |
| WorkEndDate_t | Gerçek bitiş tarihi|
| Description_s | Açıklama |
| Bilgisayar  | Yapılandırma öğesi |

**Örnek günlük analizi ekran ITSM verileri için:**

![Günlük analizi ekranı](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="itsmc-integration-with-other-oms-solutions"></a>Diğer OMS çözümleri ile ITSMC tümleştirme

ITSM bağlayıcı şu anda hizmet Haritası çözümüyle tümleştirmeyi destekler.

Hizmet eşlemesi otomatik olarak sistemlerde, Windows ve Linux uygulama bileşenleri bulur ve Hizmetleri arasındaki iletişimi eşler. Bunları – Kritik hizmetler sunan birbirine bağlı sistemler olarak düşündüğünüz sunucularınızı görüntülemenize izin verir. Bir aracı yüklemesini dışında hiçbir yapılandırma TCP bağlı mimarisiyle arasında bağlantı noktaları gerekli ve hizmet Haritası sunucuları, işlemleri arasındaki bağlantıları gösterir.

Daha fazla bilgi: [hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md).

Ayrıca hizmet Haritası çözümü kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi ITSM çözümlerinde oluşturulan hizmet Masası öğeleri görüntüleyebilirsiniz:

![ServiceMap tümleştirme](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>OMS uyarılar için ITSM iş öğeleri oluşturma

Yerinde ITSMC çözümüyle OMS bağlı ITSM aracınızı iş öğelerini oluşturma tetiklemek üzere uyarılar yapılandırabilirsiniz. Aşağıdaki yordamı kullanın:

1. Gelen **günlük arama** penceresinde verileri görüntülemek için bir günlük arama sorgusunu çalıştırın. Sorgu sonuçları, iş öğeleri için kaynaktır.
2. İçinde **günlük arama**, tıklatın **uyarı** açmak için **uyarı kuralı Ekle** sayfası.

    ![Günlük analizi ekranı](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Üzerinde **uyarı kuralı Ekle** penceresinde sağlamak için gerekli ayrıntıları **adı**, **önem**, **arama sorgusu**, ve **uyarı Ölçüt** (zaman penceresi/ölçü ölçüm).
4. Seçin **Evet** için **ITSM Eylemler**.
5. ITSM bağlantınızı seçin **seçin bağlantı** listesi.
6. Gerekli ayrıntıları belirtin.
7. Bu uyarının her günlük girişinin için ayrı iş öğesi oluşturmak için seçin **her günlük girişinin için tek tek iş öğeleri oluşturma** onay kutusu.

    Veya

    Bu onay kutusunu günlük girişlerini bu uyarı altında herhangi bir sayıda için yalnızca bir iş öğesi oluşturmak için seçili bırakın.

7. **Kaydet** düğmesine tıklayın.

Oluşturduğunuz OMS uyarı altında görülebilir **ayarları**>**uyarıları**. Belirtilen uyarının koşulu karşılandığında karşılık gelen ITSM bağlantı çalışma öğeleri oluşturulur.

## <a name="create-itsm-work-items-from-oms-logs"></a>OMS günlüklerinden ITSM iş öğeleri oluşturma

İş öğelerini bağlı ITSM kaynaklardan doğrudan bir günlük kaydı oluşturabilirsiniz. Aşağıdaki yordamı kullanın:

1. Gelen **günlük arama**, gerekli verileri arama, ayrıntı seçin ve tıklatın **oluşturma çalışma öğesini**.

    **ITSM iş öğesi oluşturma** penceresi görüntülenir:

    ![Günlük analizi ekranı](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

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

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarıları ITSM iş öğeleri oluşturma

ITSMC Eylem grupları ile tümleşiktir.

[Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) Azure uyarılarınızı eylemleri tetikleyen, modüler ve yeniden kullanılabilir bir yolunu sağlar. Eylem gruplarında ITSM eylemini kullanarak ITSM ürününüzde ITSM bağlayıcı çözüm mevcut bir bağlantı olan iş öğeleri oluşturabilirsiniz.

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
7. **Tamam** düğmesine tıklayın.

Azure uyarı kuralı oluşturma/düzenleme yaparken ITSM eylemi olan bir eylem grubu kullanın. Uyarıyı tetikleyen iş öğesi ITSM aracında oluşturulur.

>[!NOTE]

> Şu anda, etkinlik günlüğü uyarıları ITSM eylem desteği yalnızca, diğer Azure uyarıları bu desteklemez.


## <a name="troubleshoot-itsm-connections-in-oms"></a>OMS ITSM bağlantı sorunlarını giderme
1.  Bağlantılı kaynağın kullanıcı Arabirimi ile gelen bağlantı başarısız olursa bir **bağlantı kaydetmede hata** iletisi, aşağıdaki adımları uygulayın:
 - ServiceNow, Cherwell ve Provance bağlantıları için-doğru girdiğiniz kullanıcı adı, parola, istemci kimliği ve istemci parolası bağlantıların her biri için emin olun.
        -karşılık gelen ITSM üründe bağlantıyı kurmak için yeterli ayrıcalıklara sahip olmadığını denetleyin.
 - Service Manager bağlantılarında-Web uygulaması başarıyla dağıtılır ve karma bağlantı oluşturulur emin olun. Şirket içi Service Manager makineyle bağlantı kuran başarıyla doğrulamak için Web uygulaması URL'si yapma belgelerindeki ayrıntılı olarak ziyaret [karma bağlantı](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  ServiceNow verileri için günlük analizi eşitlenmedi, örneği değil uykuda ServiceNow emin olun. ServiceNow geliştirme örnekleri bazen boştayken uzun bir süre için uyku moduna gidin. Aksi takdirde, sorunu bildirin.
3.  OMS uyarıları yangın ancak iş öğeleri ITSM üründe oluşturulmamış veya yapılandırma öğeleri oluşturulan/iş öğeleri veya herhangi diğer genel bilgi için aşağıdaki konumlarda aramak için bağlı değil:
 -  ITSMC: Çözüm bağlantıları/iş öğeleri/bilgisayarların vb. özetini gösterir. Döşeme gösteren tıklatın **bağlayıcı durumu**, size aldığı **günlük arama** ilgili sorgu. Günlük kayıtları LogType_S ile daha fazla bilgi için hata olarak arayın.
 - **Arama oturum** sayfa: doğrudan sorgu kullanarak hataları ve ilgili bilgileri görüntüleyin *türü ServiceDeskLog_CL =*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Service Manager Web uygulama dağıtım sorunlarını gider
1.  Herhangi bir sorun olması durumunda web uygulama dağıtımı, kaynakları oluşturun/dağıtmak için belirtilen abonelik yeterli izinlere sahip olun.
2.  Alırsanız bir **"Nesne başvurusu ayarlı değil bir nesne örneğine"** çalıştırdığınızda hata [betik](log-analytics-itsmc-service-manager-script.md), altında geçerli değerler girdiğinizden emin olun **Kullanıcı Yapılandırması** bölümü .
3.  Service bus geçiş ad alanı oluşturma başarısız olursa, gereken kaynak sağlayıcısı abonelikte kayıtlı olduğundan emin olun. El ile kayıtlı değil, hizmet veri yolu geçişi ad Azure portalından oluşturun. Bunu sırasında da oluşturabilirsiniz [karma bağlantı oluşturma](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) Azure portalından.


## <a name="contact-us"></a>Bizimle iletişim kurun

Tüm sorgular veya BT Hizmet Yönetimi Bağlayıcısı geribildirim için adresinden bize başvurun [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar
[BT Hizmet Yönetimi Bağlayıcısı için ITSM ürünler/hizmetler eklemek](log-analytics-itsmc-connections.md).
