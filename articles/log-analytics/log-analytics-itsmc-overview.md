---
title: "BT Hizmet Yönetimi Bağlayıcısı Azure günlük analizi | Microsoft Docs"
description: "Tüm sorunların hızla çözülmesine ve BT Hizmet Yönetimi Bağlayıcısı merkezi olarak izlemek ve Azure günlük analizi ITSM iş öğelerini yönetmek için kullanın."
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
ms.openlocfilehash: 411d6103852cbf534d3c420d5ea7b2146df5164e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>ITSM iş öğelerini BT Hizmet Yönetimi Bağlayıcısı (Önizleme) kullanarak merkezi olarak yönetme

![BT Hizmet Yönetimi Bağlayıcısı simgesi](./media/log-analytics-itsmc/itsmc-symbol.png)

BT Hizmet Yönetimi Bağlayıcısı, desteklenen bir BT Hizmet Yönetimi (ITSM) ürün/hizmet ve günlük analizi arasında çift yönlü tümleştirme sağlar.  Bu bağlantı günlük analizi uyarıları veya günlük kayıtlarını göre ITSM üründeki olaylar, uyarılar ya da olaylar oluşturabilirsiniz. Bağlayıcı da olaylar gibi verileri içe aktaran ve OMS günlük analizi ITSM üründen değişiklik istekleri.

BT Hizmet Yönetimi Bağlayıcısı ile şunları yapabilirsiniz:

  - Olay yönetimi uygulamalarınızı tercih ettiğiniz ITSM aracında ile işletimsel uyarı tümleştirin.
    - İş öğeleri (örneğin, uyarı, olay, olay) içinde ITSM OMS uyarılardan ve günlük arama aracılığıyla oluşturun.
    - Azure etkinlik günlüğü uyarılarınızı Eylem grupları ITSM eylemde üzerinden bağlı iş öğeleri oluşturun. 
  
  - İzleme, günlük ve kuruluşunuzda kullanılan hizmet yönetimi verileri birleştirin.
    - Olay ilişkilendirmek ve istek verileri ile ilgili günlük verilerini günlük analizi çalışma alanındaki tooling, ITSM değiştirin.   
    - Olaylara genel bakış için üst düzey panoları görüntülemek için değişiklik istekleri ve etkilenen sistemleri.
    - Hizmet Yönetimi veri almak için günlük analizi sorgular yazarsınız.
      
## <a name="adding-the-it-service-management-connector-solution"></a>Hizmet Yönetimi Bağlayıcısı çözümü BT ekleme

Açıklanan işlemi kullanarak, günlük analizi çalışma alanı BT Hizmet Yönetimi Bağlayıcısı çözüm eklemek [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).

BT Hizmet Yönetimi Bağlayıcısı çözümleri Galerisi'nde gördüğünüz gibi döşeme:

![Bağlayıcı döşeme](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Başarılı ayrıca sonra BT Hizmet Yönetimi Bağlayıcısı altında görürsünüz **OMS** > **ayarları** > **bağlı kaynakları.**

![Bağlı ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Varsayılan olarak, BT Hizmet Yönetimi Bağlayıcısı her 24 saatte bağlantının verileri yeniler. Hemen tüm düzenlemeler veya şablon için bağlantının verileri yenilemek için bağlantınızı yanında görüntülenen Yenile düğmesini tıklatın yaptığınız güncelleştirir.

 ![ITSMC Yenile](./media/log-analytics-itsmc/itsmc-connection-refresh.png)


## <a name="configuring-the-connection-with-your-itsm-software"></a>Bağlantıyı ITSM yazılımınızda yapılandırma

BT Hizmet Yönetimi Bağlayıcısı çözümünü destekler bağlanmasını **System Center Service Manager**, **ServiceNow**, **Provance**, ve **Cherwell**. Bağlantınızı ile yapılandırma

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Çözümü kullanma

BT Hizmet Yönetimi Bağlayıcısı ITSM yazılım ayrıntılarla yapılandırdıktan sonra bağlayıcı bağlı ITSM ürün/hizmetinden veri toplamayı başlatır. Olaylar ve değişiklik istekleri ITSM Ürün/hizmet sayısına bağlı olarak, ilk eşitleme birkaç dakika içinde tamamlanması. 

> [!NOTE]
> - ITSM üründen BT Hizmet Yönetimi Bağlayıcısı çözümü tarafından alınan veri türü günlük kayıtları olarak günlük analizi görünür **ServiceDesk_CL**.
> - Günlük kaydı içeren adında bir alan **ServiceDeskWorkItemType_s**, olay veya değişiklik isteği, iki tür ITSM ürün içeri veri olduğu

## <a name="data-synced-from-itsm-product"></a>Veri ITSM üründen eşitlendi
Olaylar ve değişiklik istekleri, ITSM ürün için günlük analizi çalışma alanınız eşitlenir. Aşağıdaki bilgiler BT Hizmet Yönetimi Bağlayıcısı tarafından toplanan veri örnekleri gösterilmektedir:

> [!NOTE]
> İş öğesi türüne bağlı olarak günlük analizi içeri **ServiceDesk_CL** aşağıdaki alanları içerir:

**İş öğesi:** **olaylar**  
ServiceDeskWorkItemType_s "Olay" =

**Alanları**

- ServiceDeskConnectionName
- Hizmet Masası kimliği
- Durum
- Aciliyet
- Etkisi
- Öncelik
- Yükseltme
- Tarafından oluşturulan
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
- Tarafından oluşturulan
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
- Etkisi
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
| Impact_s |Etkisi|
| Priority_s | Öncelik |
| CreatedBy_s | Tarafından açılmış |
| ResolvedBy_s | Çözümleyen|
| ClosedBy_s  | Tarafından kapatıldı |
| Source_s| İlgili kişi türü |
| AssignedTo_s | Atanan  |
| Category_s | Kategori |
| Title_s|  Kısa açıklama |
| Description_s|  Notlar |
| CreatedDate_t|  Açılan |
| ClosedDate_t| Kapalı|
| ResolvedDate_t|Çözümlendi|
| Bilgisayar  | Yapılandırma öğesi |

## <a name="output-data-for-a-servicenow-change-request"></a>Çıktı verileri bir ServiceNow için değişiklik isteği

| OMS alan | ITSM alan |
|:--- |:--- |
| ServiceDeskId_s| Sayı |
| CreatedBy_s | Tarafından istenen |
| ClosedBy_s | Tarafından kapatıldı |
| AssignedTo_s | Atanan  |
| Title_s|  Kısa açıklama |
| Type_s|  Tür |
| Category_s|  Kategori |
| CRState_s|  Durum|
| Urgency_s|  Aciliyet |
| Priority_s| Öncelik|
| Risk_s| Riski|
| Impact_s| Etkisi|
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

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>BT Hizmet Yönetimi Bağlayıcısı – diğer OMS çözümleri ile tümleştirme

BT Hizmet Yönetimi Bağlayıcısı şu anda hizmet Haritası çözümüyle tümleştirmeyi destekler.

Hizmet eşlemesi otomatik olarak sistemlerde, Windows ve Linux uygulama bileşenleri bulur ve Hizmetleri arasındaki iletişimi eşler. Bunları – Kritik hizmetler sunan birbirine bağlı sistemler olarak düşündüğünüz sunucularınızı görüntülemenize izin verir. Bir aracı yüklemesini dışında hiçbir yapılandırma TCP bağlı mimarisiyle arasında bağlantı noktaları gerekli ve hizmet Haritası sunucuları, işlemleri arasındaki bağlantıları gösterir. Daha fazla bilgi: [hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md).

Ayrıca hizmet Haritası çözümü kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi ITSM çözümlerinde oluşturulan hizmet Masası öğeleri görüntüleyebilirsiniz:

![ServiceMap tümleştirme](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>OMS uyarılar için ITSM iş öğeleri oluşturma

Yerinde ITSM bağlayıcı çözümüyle OMS bağlı ITSM aracınızı iş öğelerini oluşturma gibi tetiklemek üzere uyarılar yapılandırabilirsiniz:

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

Ayrıca iş öğelerini bağlı ITSM kaynaklardan doğrudan bir günlük kaydı gibi oluşturabilirsiniz:

1. Gelen **günlük arama**, gerekli verileri arama, ayrıntı seçin ve tıklatın **oluşturma çalışma öğesini**.

    **Oluşturma ITSM iş öğesi** penceresi görüntülenir:

    ![Günlük analizi ekranı](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Aşağıdaki ayrıntıları ekleyin:

  - **İş öğesi başlık**: iş öğesi için başlık.
  - **İş öğesi tanımı**: yeni çalışma öğesi için bir açıklama.
  - **Bilgisayar etkilenen**: Bu günlük verileri nerede bulundu bilgisayarın adı.
  - **Bağlantıyı seçin**: Bu iş öğesi oluşturmak istediğiniz ITSM bağlantı.
  - **İş öğesi**: iş öğesi türü.

3. Bir olay için var olan bir iş öğesi şablonunu kullanmak için tıklatın **Evet** altında **Generate iş öğesi şablona dayalı** seçeneğini ve ardından **oluşturma**.

    Veya

    Tıklatın **Hayır** özelleştirilmiş değerlerinizi sağlamak istiyorsanız.

4. Uygun değerleri sağlayın **kişi türündeki**, **etkisi**, **aciliyet**, **kategori**, ve **alt kategori** metin kutuları ve ardından **oluşturma**.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Azure uyarıları ITSM iş öğeleri oluşturma
ITSM bağlayıcı şimdi Eylem grupları ile tümleşiktir. [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) Azure uyarılarınızı eylemleri tetikleyen, modüler ve yeniden kullanılabilir bir yolunu sağlar. Eylem grupları ITSM eylemde ITSM ürününüzü varolan ITSM bağlayıcı çözümünü kullanarak iş öğelerini oluşturur.

1. Azure portalında tıklayın **İzleyicisi**
2. Sol bölmede tıklayın **Eylem grupları**

    ![Eylem Grupları](media/log-analytics-itsmc/ActionGroups.png)

3. Sağlamak **adı** ve **kısaad** eylem grubunuz için. Seçin **kaynak grubu** ve **abonelik** oluşturulmasına eylem grubunuzun istediğiniz.

    ![Eylem grupları ayrıntısı](media/log-analytics-itsmc/ActionGroupsDetail.png)

4. Eylemler listesinde **ITSM** için açılan gelen **eylem türü**. Sağlayan bir **adı** 'ı tıklatın ve eylemi için **Düzenle ayrıntıları**.


5. Seçin **abonelik** günlük analizi çalışma alanınız bulunduğu. Seçin **bağlantı** yani Ardından, çalışma alanı adı, ITSM bağlayıcı adı. Örneğin, "MyITSMMConnector(MyWorkspace)."

    ![ITSM eylemi ayrıntıları](./media/log-analytics-itsmc/ITSMActionDetails.png)

6. Seçin **iş öğesi** açılan türünden.
7. ITSM ürününüzü tarafından gerekli alanları doldurun veya varolan bir şablonu kullanmak üzere seçin.
8. **Tamam**’a tıklayın.

Azure uyarı kuralı oluşturma/düzenleme yaparken ITSM eylemi olan bir eylem grubu, kullanın. Uyarıyı tetikleyen iş öğesi ITSM aracında oluşturulur. 

>[!NOTE]
>Şu anda yalnızca etkinlik günlüğü uyarıları ITSM eylemini desteklemektedir. Diğer Azure uyarılar için bu, Hayır op eylemdir.
>


## <a name="troubleshoot-itsm-connections-in-oms"></a>OMS ITSM bağlantı sorunlarını giderme
1.  Bağlantılı kaynağın kullanıcı Arabirimi ile gelen bağlantı başarısız olursa bir **bağlantı kaydetmede hata** iletisi, aşağıdaki adımları uygulayın:
 - ServiceNow, Cherwell ve Provance bağlantıları için
    - doğru kullanıcı adı, parola, istemci kimliği ve istemci parolası bağlantıların her biri için girdiğiniz emin olun.
    - bağlantıyı kurmak için karşılık gelen ITSM üründe yeterli ayrıcalıkları olup olmadığını denetleyin.
 - Service Manager bağlantılarında
     - Web uygulaması başarıyla dağıtılır ve karma bağlantı oluşturulan emin olun. Şirket içi Service Manager makineyle bağlantı kuran başarıyla doğrulamak için Web uygulaması URL'si yapma belgelerindeki ayrıntılı olarak ziyaret [karma bağlantı](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).
     
2.  ServiceNow verileri için günlük analizi eşitlenmedi, örneği değil uykuda ServiceNow emin olun. ServiceNow geliştirme örnekleri bazen boştayken uzun bir süre için uyku moduna gidin. Aksi takdirde, sorunu bildirin.
3.  OMS uyarıları yangın ancak iş öğeleri ITSM üründe oluşturulmamış veya yapılandırma öğeleri oluşturulan/iş öğeleri veya herhangi diğer genel bilgi için aşağıdaki konumlarda aramak için bağlı değil:
 -  **BT Hizmet Yönetimi Bağlayıcısı çözüm**: Çözüm bağlantıları/iş öğeleri/bilgisayarların vb. özetini gösterir. Döşeme gösteren tıklatın **bağlayıcı durumu**, size aldığı **günlük arama** ilgili sorgu. Günlük kayıtları LogType_S ile daha fazla bilgi için hata olarak arayın.
 - Hatalar ve ilgili bilgileri doğrudan görüntüleyin veya **günlük arama** sorgu kullanarak sayfa *türü ServiceDeskLog_CL =*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Service Manager Web uygulama dağıtım sorunlarını gider
1.  Web uygulama dağıtımı ile ilgili sorunları yüz kaynakları oluşturun/dağıtmak için belirtilen abonelik yeterli izinlere sahip olun.
2.  Alırsanız bir **"Nesne başvurusu ayarlı değil bir nesne örneğine"** çalıştırdığınızda hata [betik](log-analytics-itsmc-service-manager-script.md), altında geçerli değerler girdiğinizden emin olun **Kullanıcı Yapılandırması** bölümü .
3.  Service bus geçiş ad alanı oluşturma başarısız olursa, gereken kaynak sağlayıcısı abonelikte kayıtlı olduğundan emin olun. El ile kayıtlı değil, hizmet veri yolu geçişi ad Azure portalından oluşturun. Bunu sırasında da oluşturabilirsiniz [karma bağlantı oluşturma](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) Azure portalından.


## <a name="contact-us"></a>Bizimle iletişim kurun

Tüm sorgular veya BT Hizmet Yönetimi Bağlayıcısı geribildirim için adresinden bize başvurun [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Sonraki adımlar
[BT Hizmet Yönetimi Bağlayıcısı için ITSM ürünler/hizmetler eklemek](log-analytics-itsmc-connections.md).
