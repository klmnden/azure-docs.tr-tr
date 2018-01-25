---
title: "BT Hizmet Yönetimi Bağlayıcısı bağlantılarla Azure günlük analizi desteklenen | Microsoft Docs"
description: "Bu makale, merkezi olarak izlemek ve ITSM iş öğelerini yönetmek için OMS günlük analizi, BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile ITSM ürünler/hizmetler bağlanma hakkında bilgi sağlar."
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: a51ba4b45b7f6c72037d5c562a4ccd59e601cee4
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>ITSM ürünler/hizmetler ile BT Hizmet Yönetimi Bağlayıcısı Bağlan
Bu makalede, iş öğelerini merkezi olarak yönetmek için günlük analizi içinde ITSM Ürün/hizmet ve BT Hizmet Yönetimi Bağlayıcısı'nı (ITSMC) arasındaki bağlantı yapılandırma hakkında bilgi sağlar. ITSMC hakkında daha fazla bilgi için bkz: [genel bakış](log-analytics-itsmc-overview.md).

Aşağıdaki ITSM ürünler/hizmetler desteklenir. Ürünü için ITSMC ürün bağlanma hakkında ayrıntılı bilgileri görüntülemek için seçin.

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]

> ITSM bağlayıcı yalnızca bulut tabanlı ServiceNow örneklerine bağlanabilir. Şirket içi ServiceNow örneği şu anda desteklenmemektedir.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>System Center Service Manager BT hizmetine bağlanmak Azure Yönetimi Bağlayıcısı

Aşağıdaki bölümlerde Azure ITSMC System Center Service Manager ürününüzü bağlanmak nasıl kullanılacağı hakkındaki ayrıntıları belirtin.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- Yüklü ITSMC. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Service Manager Web uygulaması (Web uygulaması) dağıtılır ve yapılandırılır. Web uygulaması hakkında bilgi [burada](#create-and-deploy-service-manager-web-app-service).
- Karma bağlantı oluşturulur ve yapılandırılır. Daha fazla bilgi: [karma bağlantı yapılandırma](#configure-the-hybrid-connection).
- Service Manager sürümleri desteklenir: 2012 R2 veya 2016.
- Kullanıcı rolü: [gelişmiş işleç](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Bağlantı yordamı

System Center Service Manager Örneğiniz için ITSMC bağlanmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma veri kaynakları** tıklatın **ITSM bağlantıları**.

    ![Yeni bağlantı](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Sağ bölmede üstündeki **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]

> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz System Center Service Manager örneği için bir ad yazın.  Bu örnekte iş öğelerini yapılandırma / ayrıntılı günlük analizi görünümü olduğunda, bu adı daha sonra kullanırsınız. |
| **İş ortağı türü**   | Seçin **System Center Service Manager**. |
| **Sunucu URL'si**   | Service Manager Web uygulamasının URL'sini yazın. Service Manager Web uygulaması hakkında daha fazla bilgiyi [burada](#create-and-deploy-service-manager-web-app-service).
| **İstemci kimliği**   | Web uygulaması kimlik doğrulamasını (otomatik komut dosyası kullanarak) oluşturulan istemci kimliği yazın. Otomatik komut dosyası hakkında daha fazla bilgiyi [burada.](log-analytics-itsmc-service-manager-script.md)|
| **İstemci parolası**   | İstemci parolası yazın bu kimliği için oluşturulan   |
| **Veri Eşitleme kapsamı**   | ITSMC eşitlemek istediğiniz Service Manager iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır. **Seçenekler:** olaylar, değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, OMS etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |

![Service manager bağlantısı](./media/log-analytics-itsmc/service-manager-connection.png)

**Başarıyla bağlanıldı ve eşitlenen**:

- Seçili iş öğelerini Hizmet Yöneticisi'nden Azure aktarılır **günlük analizi.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.

- Günlük analizi uyarıları veya günlük kayıtları veya Azure Uyarıları'nda bu Service Manager örneğinin olaylar oluşturabilirsiniz.


Daha fazla bilgi edinin: [oluşturma ITSM iş öğeleri için günlük analizi uyarıları](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [günlük analizi günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) ve [Azure uyarıları oluşturma ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Oluşturma ve Service Manager web uygulama hizmeti dağıtma

ITSMC Azure ile şirket içi Hizmet Yöneticisi'ni bağlanmak için Microsoft Github'da Service Manager Web uygulaması oluşturmuştur.

Service Manager için ITSM Web uygulamasını kurup ayarlamak için aşağıdakileri yapın:

- **Web uygulaması dağıtma** – Web uygulaması dağıtma, özelliklerini ayarlamak ve Azure AD ile kimlik doğrulaması. Web uygulaması kullanarak dağıtabilirsiniz [betik otomatik](log-analytics-itsmc-service-manager-script.md) Microsoft, sağlamıştır.
- **Karma bağlantı yapılandırma** - [bu bağlantıyı yapılandırmak](#configure-the-hybrid-connection), el ile.

#### <a name="deploy-the-web-app"></a>Web uygulaması dağıtma
Otomatik kullanmak [betik](log-analytics-itsmc-service-manager-script.md) Web uygulamasını dağıtmak için özelliklerini ayarlamak ve Azure AD ile kimlik doğrulaması.

Aşağıdaki gerekli ayrıntıları sağlayarak komut dosyasını çalıştırın:

- Azure Abonelik Ayrıntıları
- Kaynak grubu adı
- Konum
- Service Manager sunucu ayrıntıları (sunucu adı, etki alanı, kullanıcı adı ve parola)
- Web uygulamanız için site adı öneki
- ServiceBus Namespace.

Komut dosyası (benzersiz hale getirmek amacıyla birkaç ek dizeleri birlikte) belirtilen adı kullanarak Web uygulaması oluşturur. Bunu oluşturan **Web uygulaması URL'si**, **istemci kimliği** ve **gizli**.

Bir bağlantı ile ITSMC oluşturduğunuzda değerleri kaydetmek, bunları kullanın.

**Web uygulaması yükleme denetimi**

1. Git **Azure portal** > **kaynakları**.
2. Web uygulaması seçin, **ayarları** > **uygulama ayarları**.
3. Komut dosyası aracılığıyla uygulama dağıtımı sırasındaki sağlanan Service Manager örneğiyle ilgili bilgiler onaylayın.

### <a name="configure-the-hybrid-connection"></a>Karma bağlantıyı yapılandırın

Azure'da ITSMC ile Service Manager örneğine bağlar karma bağlantıyı yapılandırmak için aşağıdaki yordamı kullanın.

1. Service Manager Web uygulamasının altında Bul **Azure kaynaklarını**.
2. Tıklatın **ayarları** > **ağ**.
3. Altında **karma bağlantılar**, tıklatın **karma bağlantı uç noktalarınızı yapılandırın**.

    ![Karma bağlantının ağ](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. İçinde **karma bağlantılar** dikey penceresinde tıklatın **karma Bağlantı Ekle**.

    ![Karma Bağlantısı Ekle](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. İçinde **eklemek karma bağlantılar** dikey penceresinde tıklatın **oluştur yeni karma bağlantı**.

    ![Yeni Karma bağlantı](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Aşağıdaki değerleri yazın:

    - **Uç nokta adı**: yeni bir karma bağlantı için bir ad belirtin.
    -  **Uç noktası ana bilgisayar**: Service Manager yönetim sunucusunun FQDN'si.
    - **Uç nokta bağlantı noktası**: 5724 yazın
    - **Servicebus ad alanı**: varolan bir servicebus ad kullanın veya yeni bir tane oluşturun.
    - **Konum**: konumu seçin.
    -  **Ad**:, oluşturuyorsanız servicebus için bir ad belirtin.

    ![Karma bağlantı değerleri](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Tıklatın **Tamam** kapatmak için **karma bağlantı oluşturmak** dikey penceresinde ve karma bağlantı oluşturma Başlat.

    Karma bağlantısı oluşturulduktan sonra dikey altında görüntülenir.

7. Karma bağlantısı oluşturulduktan sonra bağlantıyı seçin ve'ı tıklatın **Ekle seçili karma bağlantı**.

    ![Yeni Karma bağlantı](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Dinleyici Kurulumu Yapılandır

Karma bağlantının dinleyicisi Kur yapılandırmak için aşağıdaki yordamı kullanın.

1. İçinde **karma bağlantılar** dikey penceresinde tıklatın **Bağlantı Yöneticisi'ni indirin** ve System Center Service Manager örneğinin çalıştığı makineye yükleyin.

    Yükleme tamamlandıktan sonra **karma Bağlantı Yöneticisi kullanıcı Arabirimi** seçeneği altında kullanılabilir **Başlat** menüsü.

2. Tıklatın **karma Bağlantı Yöneticisi kullanıcı Arabirimi** , Azure kimlik bilgileriniz istenir.

3. Azure kimlik bilgilerinizle oturum açma ve karma bağlantı oluşturulduğu aboneliğinizi seçin.

4. **Kaydet**’e tıklayın.

Karma bağlantı başarıyla bağlandı.

![başarılı karma bağlantı](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Sonra karma bağlantısı oluşturulduğunda, doğrulayın ve dağıtılan Service Manager Web uygulaması ziyaret ederek bağlantıyı sınayın. Azure'da ITSMC bağlanmaya çalışmadan önce bağlantı başarılı olduğundan emin olun.

Aşağıdaki örnek görüntü başarılı bir bağlantı ayrıntılarını gösterir:

![Karma bağlantı testi](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>ServiceNow BT hizmetine bağlanmak Azure Yönetimi Bağlayıcısı

Aşağıdaki bölümler, azure'da ITSMC ServiceNow ürününüzü bağlanmak nasıl kullanılacağı hakkındaki ayrıntıları sağlar.

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların karşılandığından emin olun:
- Yüklü ITSMC. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- ServiceNow desteklenen sürümler: Cakarta, Istanbul, Helsinki, Geneva

**ServiceNow yöneticileri kendi ServiceNow örneği şunları yapması gerekir**:
- İstemci kimliği ve ServiceNow ürün için istemci parolası oluşturur. İstemci kimliği ve parolası oluşturma hakkında daha fazla bilgi için aşağıdaki bilgileri gerektiği gibi bakın:

    - [OAuth Cakarta için ayarlama](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Istanbul için ayarlama](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Helsinki için ayarlama](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Geneva için ayarlama](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)


- Microsoft OMS tümleştirme (ServiceNow uygulama) için kullanıcı uygulamasını yükleyin. [Daha fazla bilgi edinin](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
- Yüklenen kullanıcı uygulaması için tümleştirme kullanıcı rolü oluşturun. Tümleştirme kullanıcı rolü oluşturma konusunda bilgiler [burada](#create-integration-user-role-in-servicenow-app).

### <a name="connection-procedure"></a>**Bağlantı yordamı**
ServiceNow bağlantı oluşturmak için aşağıdaki yordamı kullanın:


1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma veri kaynakları** tıklatın **ITSM bağlantıları**.
    ![Yeni bağlantı](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Sağ bölmede üstündeki **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **Tamam** bağlantı oluşturmak için.


> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz ServiceNow örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü, OMS içinde daha sonra bu adı kullanın. |
| **İş ortağı türü**   | Seçin **ServiceNow**. |
| **Kullanıcı Adı**   | ServiceNow uygulama ITSMC bağlantıyı desteklemek için oluşturduğunuz tümleştirme kullanıcı adı yazın. Daha fazla bilgi: [oluşturma ServiceNow uygulama kullanıcı rolü](#create-integration-user-role-in-servicenow-app).|
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not**: kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere ITSMC hizmet içinde depolanmaz.  |
| **Sunucu URL'si**   | ServiceNow örneğinin ITSMC için bağlamak istediğiniz URL'yi yazın. |
| **İstemci kimliği**   | Daha önce oluşturulan OAuth2 kimlik doğrulamasında kullanmak istediğiniz istemci kimliği yazın.  İstemci kimliği ve parolası oluşturma hakkında daha fazla bilgi: [OAuth Kurulumu](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **İstemci parolası**   | İstemci parolası yazın bu kimliği için oluşturulan   |
| **Veri Eşitleme kapsamı**   | ITSMC aracılığıyla Azure günlük analizi için eşitlemek istediğiniz ServiceNow iş öğelerini seçin.  Seçilen değerleri günlük analizi alınır.   **Seçenekler:** olaylar ve değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |

![ServiceNow bağlantı](./media/log-analytics-itsmc/itsm-connection-servicenow-connection-latest.png)

**Başarıyla bağlanıldı ve eşitlenen**:

- ServiceNow örneğinin Seçili iş öğelerinden Azure'a aktarılır **günlük analizi.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.

- Günlük analizi uyarıları veya günlük kayıtları veya bu ServiceNow örneği Azure uyarıları olaylar oluşturabilirsiniz.

Daha fazla bilgi edinin: [oluşturma ITSM iş öğeleri için günlük analizi uyarıları](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [günlük analizi günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) ve [Azure uyarıları oluşturma ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>ServiceNow uygulamada tümleştirme kullanıcı rolü oluşturun

Aşağıdaki yordam kullanıcı:

1.  Ziyaret [ServiceNow deposunu](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) yükleyip **ServiceNow ve Microsoft OMS tümleştirme için kullanıcı uygulaması** ServiceNow örneğinin içine.
2.  Yükleme tamamlandıktan sonra sol gezinti çubuğunda ServiceNow örneğinin, arama ve select Microsoft OMS Tümleştirici ziyaret edin.  
3.  Tıklatın **yükleme Yapılacaklar listesi**.

    Durum görüntülenir **tamamlanmadı** oluşturulacak kullanıcı rolü henüz ise.

4.  Metin kutularındaki yanına **tümleştirme kullanıcı oluşturma**, ITSMC Azure içinde bağlanıp kullanıcının kullanıcı adı girin.
5.  Bu kullanıcı için parola girin ve tıklayın **Tamam**.  

>[!NOTE]

> Azure'da ServiceNow bağlantı kurmak için bu kimlik bilgileri kullanın.

Yeni oluşturulan kullanıcı atanmış varsayılan rolleriyle görüntülenir.

**Varsayılan rol**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   itil
-   template_editor
-   view_changer

Kullanıcı başarıyla oluşturduktan sonra durumu **denetleyin yükleme Yapılacaklar listesi** oluşturulan uygulama için kullanıcı rolü ayrıntılarını listeleme tamamlandı olarak taşır.

> [!NOTE]

> Bir kullanıcı oluşturmak izin vermek için **uyarıları** ve **olayları** azure'dan ServiceNow içinde:

> - Olay Yönetimi Modülü yüklü ServiceNow örneğinde olduğundan emin olun.

> - Aşağıdaki roller için tümleştirme kullanıcı ekleyin:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Provance BT hizmetine bağlanmak Azure Yönetimi Bağlayıcısı

Aşağıdaki bölümler, azure'da ITSMC Provance ürününüzü bağlanmak nasıl kullanılacağı hakkındaki ayrıntıları sağlar.


### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:


- Yüklü ITSMC. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Azure AD ile - provance uygulama kaydedileceğini ve istemci kimliği kullanımına sunulur. Ayrıntılı bilgi için bkz: [active directory kimlik doğrulamasını yapılandırmak nasıl](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

- Kullanıcı rolü: yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Provance bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma veri kaynakları** tıklatın **ITSM bağlantıları**.
    ![Yeni bağlantı](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Sağ bölmede üstündeki **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]

> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz Provance örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü olduğunda, bu adı daha sonra kullanırsınız. |
| **İş ortağı türü**   | Seçin **Provance**. |
| **Kullanıcı Adı**   | İçin ITSMC bağlanabilmesi için kullanıcı adını yazın.    |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere ITSMC hizmet içinde depolanmaz. _|
| **Sunucu URL'si**   | Provance Örneğiniz için ITSMC bağlamak istediğiniz URL'yi yazın. |
| **İstemci kimliği**   | Provance örneğinde oluşturulan bu bağlantısının kimlik doğrulaması için istemci kimliği yazın.  İstemci kimliği hakkında daha fazla bilgi [active directory kimlik doğrulamasını yapılandırmak nasıl](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Veri Eşitleme kapsamı**   | ITSMC aracılığıyla Azure günlük analizi için eşitlemek istediğiniz Provance iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır.   **Seçenekler:** olaylar, değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı.|

![Provance bağlantı](./media/log-analytics-itsmc/itsm-connections-provance-latest.png)

**Başarıyla bağlanıldı ve eşitlenen**:

- Seçili iş öğelerini bu Provance örneğinden Azure'a aktarılır **günlük analizi.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.

- Günlük analizi uyarıları veya günlük kayıtları veya Azure Uyarıları'nda bu Provance örnek olaylar oluşturabilirsiniz.

Daha fazla bilgi edinin: [oluşturma ITSM iş öğeleri için günlük analizi uyarıları](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [günlük analizi günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) ve [Azure uyarıları oluşturma ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Cherwell BT hizmetine bağlanmak Azure Yönetimi Bağlayıcısı

Aşağıdaki bölümler, azure'da ITSMC Cherwell ürününüzü bağlanmak nasıl kullanılacağı hakkındaki ayrıntıları sağlar.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- Yüklü ITSMC. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Oluşturulan istemci kimliği. Daha fazla bilgi: [Cherwell için istemci kodu oluştur](#generate-client-id-for-cherwell).
- Kullanıcı rolü: yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Provance bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma veri kaynakları** tıklatın **ITSM bağlantıları**.
    ![Yeni bağlantı](./media/log-analytics-itsmc/add-new-itsm-connection.png)

3. Sağ bölmede üstündeki **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]

> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | İçin ITSMC bağlanmak istediğiniz Cherwell örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü olduğunda, bu adı daha sonra kullanırsınız. |
| **İş ortağı türü**   | Seçin **Cherwell.** |
| **Kullanıcı Adı**   | ITSMC için bağlanabilir Cherwell kullanıcı adını yazın. |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere ITSMC hizmet içinde depolanmaz.|
| **Sunucu URL'si**   | Cherwell Örneğiniz için ITSMC bağlamak istediğiniz URL'yi yazın. |
| **İstemci kimliği**   | Cherwell örneğinde oluşturulan bu bağlantısının kimlik doğrulaması için istemci kimliği yazın.   |
| **Veri Eşitleme kapsamı**   | ITSMC eşitlemek istediğiniz Cherwell iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır.   **Seçenekler:** olaylar, değişiklik istekleri. |
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |


![Provance bağlantı](./media/log-analytics-itsmc/itsm-connections-cherwell-latest.png)

**Başarıyla bağlanıldı ve eşitlenen**:

- Seçili iş öğelerini bu Cherwell örneğinden Azure'a aktarılır **günlük analizi.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.

- Günlük analizi uyarıları veya günlük kayıtları veya Azure Uyarıları'nda bu Cherwell örnek olaylar oluşturabilirsiniz.

Daha fazla bilgi edinin: [oluşturma ITSM iş öğeleri için günlük analizi uyarıları](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts), [günlük analizi günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records) ve [Azure uyarıları oluşturma ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>İstemci kimliği için Cherwell oluşturun.

Cherwell için istemci kimliği/anahtar oluşturmak için aşağıdaki yordamı kullanın:

1. Cherwell Örneğiniz için yönetici olarak oturum açın
2. Tıklatın **güvenlik** > **Düzenle REST API İstemci Ayarları**.
3. Seçin **oluştur yeni istemci** > **gizli**.

    ![Cherwell kullanıcı kimliği](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Sonraki adımlar
 - [Günlük analizi uyarılar için ITSM iş öğeleri oluşturma](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-alerts)
 - [ITSM iş öğelerini günlük analizi günlüğünden kayıtları günlükleri oluşturabilir.](log-analytics-itsmc-overview.md#create-itsm-work-items-from-log-analytics-log-records)
 - [Azure uyarıları ITSM iş öğeleri oluşturma](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
