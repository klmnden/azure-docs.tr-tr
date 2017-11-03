---
title: "BT Hizmet Yönetimi Bağlayıcısı bağlantılarla Azure günlük analizi desteklenen | Microsoft Docs"
description: "Merkezi olarak izlemek ve ITSM iş öğelerini yönetmek için Azure günlük analizi, BT Hizmet Yönetimi Bağlayıcısı ile ITSM ürünler/hizmetler bağlayın."
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
ms.date: 05/29/2017
ms.author: v-jysur
ms.openlocfilehash: bbec5773987b29eb62d10d17b88efcda29889612
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>ITSM ürünler/hizmetler ile BT Hizmet Yönetimi Bağlayıcısı (Önizleme) bağlanma
Bu makalede, BT Hizmet Yönetimi OMS Connector'daki ITSM Ürün/hizmet bağlanmak ve merkezi olarak, iş öğelerini yönetmek hakkında bilgi sağlar. BT Hizmet Yönetimi Bağlayıcısı hakkında daha fazla bilgi [genel bakış](log-analytics-itsmc-overview.md).

Aşağıdaki ürünler/hizmetler desteklenir:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-oms"></a>System Center Service Manager BT hizmetine bağlanmak OMS Yönetimi Bağlayıcısı

Aşağıdaki bölümlerde BT Hizmet Yönetimi Bağlayıcısı OMS, System Center Service Manager ürününüzü bağlanma hakkında ayrıntılar sağlanmaktadır.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki önkoşulları yerine olduğundan emin olun:

- BT Hizmet Yönetimi Bağlayıcısı yüklü.
Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Service Manager Web uygulaması (Web uygulaması) dağıtılır ve yapılandırılır. Web uygulaması hakkında bilgi [burada](#create-and-deploy-service-manager-web-app-service).
- Karma bağlantı oluşturulur ve yapılandırılır. Daha fazla bilgi: [karma bağlantı yapılandırma](#configure-the-hybrid-connection).
- Service Manager sürümleri desteklenir: 2012 R2 veya 2016.
- Kullanıcı rolü: [gelişmiş işleç](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Bağlantı yordamı

System Center Service Manager Örneğiniz için BT Hizmet Yönetimi Bağlayıcısı bağlanmak için aşağıdaki yordamı kullanın:

1. Git **OMS** >**ayarları** > **bağlı kaynakları**.
2. Seçin **ITSM bağlayıcı** tıklatın **yeni bağlantı Ekle**.

    ![Hizmet Yöneticisi ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **kaydetmek** bağlantı oluşturmak için:

> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Ad**   | BT Hizmet Yönetimi Bağlayıcısı ile bağlanmak istediğiniz System Center Service Manager örneği için bir ad yazın.  Bu örnekte iş öğelerini yapılandırma / ayrıntılı günlük analizi görünümü olduğunda, bu adı daha sonra kullanırsınız. |
| **Bağlantı türü seçin**   | Seçin **System Center Service Manager**. |
| **Sunucu URL'si**   | Service Manager Web uygulamasının URL'sini yazın. Service Manager Web uygulaması hakkında daha fazla bilgiyi [burada](#create-and-deploy-service-manager-web-app-service).
| **İstemci kimliği**   | Web uygulaması kimlik doğrulamasını (otomatik komut dosyası kullanarak) oluşturulan istemci kimliği yazın. Otomatik komut dosyası hakkında daha fazla bilgiyi [burada.](log-analytics-itsmc-service-manager-script.md)|
| **İstemci parolası**   | İstemci parolası yazın bu kimliği için oluşturulan   |
| **Veri Eşitleme kapsamı**   | BT Hizmet Yönetimi Bağlayıcısı üzerinden eşitlemek istediğiniz Service Manager iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır. **Seçenekler:** olaylar, değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, OMS etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |

Başarıyla bağlanıldı ve eşitlenen kullanıldığında:

- Seçili iş öğelerini Hizmet Yöneticisi'nden OMS aktarılır **günlük analizi.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.

- OMS olaylar OMS uyarıları veya günlük arama, bu Service Manager örneğini oluşturabilirsiniz.

Daha fazla bilgi: [OMS uyarılar için iş öğeleri oluşturma ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) ve [OMS günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Oluşturma ve Service Manager web uygulama hizmeti dağıtma

OMS BT Hizmet Yönetimi bağlayıcı ile şirket içi Hizmet Yöneticisi'ni bağlanmak için Microsoft Github'da Service Manager Web uygulaması oluşturmuştur.

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

Bir bağlantı ile BT Hizmet Yönetimi Bağlayıcısı oluşturduğunuzda değerleri kaydetmek, bunları kullanın.

**Web uygulaması yükleme denetimi**

1. Git **Azure portal** > **kaynakları**.
2. Web uygulaması seçin, **ayarları** > **uygulama ayarları**.
3. Komut dosyası aracılığıyla uygulama dağıtımı sırasındaki sağlanan Service Manager örneğiyle ilgili bilgiler onaylayın.

### <a name="configure-the-hybrid-connection"></a>Karma bağlantıyı yapılandırın

OMS BT Hizmet Yönetimi bağlayıcısıyla Service Manager örneğine bağlar karma bağlantıyı yapılandırmak için aşağıdaki yordamı kullanın.

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

4. **Kaydet** düğmesine tıklayın.

Karma bağlantı başarıyla bağlandı.

![başarılı karma bağlantı](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Sonra karma bağlantısı oluşturulduğunda, doğrulayın ve dağıtılan Service Manager Web uygulaması ziyaret ederek bağlantıyı sınayın. OMS BT Hizmet Yönetimi bağlayıcıda bağlanmaya çalışmadan önce bağlantı başarılı olduğundan emin olun.

Aşağıdaki resimde, başarılı bir bağlantı ayrıntılarını gösterir:

![Karma bağlantı testi](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-oms"></a>ServiceNow BT hizmetine bağlanmak OMS Yönetimi Bağlayıcısı

Aşağıdaki bölümlerde OMS BT Hizmet Yönetimi Connector ServiceNow ürününüzü bağlanma hakkında ayrıntılar sağlanmaktadır.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki önkoşulları yerine olduğundan emin olun:

- BT Hizmet Yönetimi Bağlayıcısı yüklü. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- ServiceNow sürümler – Fuji, Geneva, Helsinki desteklenir.

ServiceNow yöneticileri, kendi ServiceNow örneği aşağıdakileri yapmanız gerekir:
- İstemci kimliği ve ServiceNow ürün için istemci parolası oluşturur. İstemci kimliği ve parolası oluşturma hakkında daha fazla bilgi için bkz: [OAuth Kurulumu](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Microsoft OMS tümleştirme (ServiceNow uygulama) için kullanıcı uygulamasını yükleyin. [Daha fazla bilgi edinin](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Yüklenen kullanıcı uygulaması için tümleştirme kullanıcı rolü oluşturun. Tümleştirme kullanıcı rolü oluşturma konusunda bilgiler [burada](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Bağlantı yordamı**

ServiceNow bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Git **OMS** > **ayarları** > **bağlı kaynakları**.
2. Seçin **ITSM bağlayıcı** tıklatın **yeni bağlantı Ekle**.

    ![ServiceNow bağlantı](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **kaydetmek** bağlantı oluşturmak için:

> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Ad**   | BT Hizmet Yönetimi Bağlayıcısı ile bağlanmak istediğiniz ServiceNow örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü, OMS içinde daha sonra bu adı kullanın. |
| **Bağlantı türü seçin**   | Seçin **ServiceNow**. |
| **Kullanıcı Adı**   | BT Hizmet Yönetimi Bağlayıcısı bağlantıyı desteklemek üzere ServiceNow uygulamasında oluşturulan tümleştirme kullanıcı adı yazın. Daha fazla bilgi: [oluşturma ServiceNow uygulama kullanıcı rolü](#create-integration-user-role-in-servicenow-app).|
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not**: kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere OMS hizmet içinde depolanmaz.  |
| **Sunucu URL'si**   | BT Hizmet Yönetimi Bağlayıcısı için bağlanmak istediğiniz ServiceNow örneğinin URL'sini yazın. |
| **İstemci kimliği**   | Daha önce oluşturulan OAuth2 kimlik doğrulamasında kullanmak istediğiniz istemci kimliği yazın.  İstemci kimliği ve parolası oluşturma hakkında daha fazla bilgi: [OAuth Kurulumu](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **İstemci parolası**   | İstemci parolası yazın bu kimliği için oluşturulan   |
| **Veri Eşitleme kapsamı**   | BT Hizmet Yönetimi bağlayıcısı aracılığıyla OMS eşitlemek istediğiniz ServiceNow iş öğelerini seçin.  Seçilen değerleri günlük analizi alınır.   **Seçenekler:** olaylar ve değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, OMS etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |


Başarıyla bağlanıldı ve eşitlenen kullanıldığında:

- Seçili iş öğelerini ServiceNow bağlantısından OMS günlük analizi alınır.  Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.
- Bu ServiceNow örneğinin OMS uyarıları veya günlük aramadan olaylar, uyarılar ve olaylar oluşturabilirsiniz.  


Daha fazla bilgi: [OMS uyarılar için iş öğeleri oluşturma ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) ve [OMS günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>ServiceNow uygulamada tümleştirme kullanıcı rolü oluşturun

Aşağıdaki yordam kullanıcı:

1.  Ziyaret [ServiceNow deposunu](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) yükleyip **ServiceNow ve Microsoft OMS tümleştirme için kullanıcı uygulaması** ServiceNow örneğinin içine.
2.  Yükleme tamamlandıktan sonra sol gezinti çubuğunda ServiceNow örneğinin, arama ve select Microsoft OMS Tümleştirici ziyaret edin.  
3.  Tıklatın **yükleme Yapılacaklar listesi**.

    Durum görüntülenir **tamamlanmadı** oluşturulacak kullanıcı rolü henüz ise.

4.  Metin kutularındaki yanına **tümleştirme kullanıcı oluşturma**, BT Hizmet Yönetimi Bağlayıcısı OMS içinde bağlanıp kullanıcının kullanıcı adı girin.
5.  Bu kullanıcı için parola girin ve tıklayın **Tamam**.  

>[!NOTE]

> OMS içinde ServiceNow bağlantı kurmak için bu kimlik bilgileri kullanın.

Yeni oluşturulan kullanıcı atanmış varsayılan rolleriyle görüntülenir.

Varsayılan roller:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   ITIL
-   template_editor
-   view_changer

Kullanıcı başarıyla oluşturduktan sonra durumu **denetleyin yükleme Yapılacaklar listesi** oluşturulan uygulama için kullanıcı rolü ayrıntılarını listeleme tamamlandı olarak taşır.

> [!NOTE]

> Bir kullanıcı oluşturmak izin vermek için **uyarıları** ve **olayları** OMS ServiceNow içinde:

> - Olay Yönetimi Modülü yüklü ServiceNow örneğinde olduğundan emin olun.

> - Aşağıdaki roller için tümleştirme kullanıcı ekleyin:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-oms"></a>Provance BT hizmetine bağlanmak OMS Yönetimi Bağlayıcısı

Aşağıdaki bölümlerde OMS BT Hizmet Yönetimi Connector Provance ürününüzü bağlanma hakkında ayrıntılar sağlanmaktadır.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki önkoşulları yerine olduğundan emin olun:


- BT Hizmet Yönetimi Bağlayıcısı yüklü. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Azure AD ile - provance uygulama kaydedileceğini ve istemci kimliği kullanımına sunulur. Ayrıntılı bilgi için bkz: [active directory kimlik doğrulamasını yapılandırmak nasıl](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

- Kullanıcı rolü: yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Provance bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Git **OMS** > **ayarları** > **bağlı kaynakları**.
2. Seçin **ITSM bağlayıcı** tıklatın **yeni bağlantı Ekle**.  

    ![Provance bağlantı](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **kaydetmek** bağlantı oluşturmak için.

> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Ad**   | BT Hizmet Yönetimi Bağlayıcısı ile bağlanmak istediğiniz Provance örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü, OMS içinde daha sonra bu adı kullanın. |
| **Bağlantı türü seçin**   | Seçin **Provance**. |
| **Kullanıcı Adı**   | BT Hizmet Yönetimi Bağlayıcısı bağlanabilmesi için kullanıcı adını yazın.    |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere OMS hizmet içinde depolanmaz. _|
| **Sunucu URL'si**   | BT Hizmet Yönetimi Bağlayıcısı için bağlanmak istediğiniz Provance örneğinizi URL'sini yazın. |
| **İstemci kimliği**   | Provance örneğinde oluşturulan bu bağlantısının kimlik doğrulaması için istemci kimliği yazın.  İstemci kimliği hakkında daha fazla bilgi [active directory kimlik doğrulamasını yapılandırmak nasıl](../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Veri Eşitleme kapsamı**   | BT Hizmet Yönetimi bağlayıcısı aracılığıyla OMS eşitlemek istediğiniz Provance iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır.   **Seçenekler:** olaylar, değişiklik istekleri.|
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, OMS etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı.|

Başarıyla bağlanıldı ve eşitlenen kullanıldığında:

- Seçili iş öğelerini Provance bağlantısından OMS aktarılır **günlük analizi.**  Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.
- Olayları ve olayları OMS uyarıları veya günlük arama bu Provance örneğinde oluşturabilirsiniz.

Daha fazla bilgi: [OMS uyarılar için iş öğeleri oluşturma ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) ve [OMS günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-to-it-service-management-connector-in-oms"></a>Cherwell BT hizmetine bağlanmak OMS Yönetimi Bağlayıcısı

Aşağıdaki bölümlerde OMS BT Hizmet Yönetimi Connector Cherwell ürününüzü bağlanma hakkında ayrıntılar sağlanmaktadır.

### <a name="prerequisites"></a>Ön koşullar

Aşağıdaki önkoşulları yerine olduğundan emin olun:

- BT Hizmet Yönetimi Bağlayıcısı yüklü. Daha fazla bilgi: [BT Hizmet Yönetimi Bağlayıcısı çözümünü ekleme](log-analytics-itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Oluşturulan istemci kimliği. Daha fazla bilgi: [Cherwell için istemci kodu oluştur](#generate-client-id-for-cherwell).
- Kullanıcı rolü: yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Cherwell bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Git **OMS** >  **ayarları** > **bağlı kaynakları**.
2. Seçin **ITSM bağlayıcı** tıklatın **yeni bağlantı Ekle**.  

    ![Cherwell kullanıcı kimliği](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve tıklayın **kaydetmek** bağlantı oluşturmak için.

> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Ad**   | BT Hizmet Yönetimi Bağlayıcısı bağlanmak istediğiniz Cherwell örneği için bir ad yazın.  İş öğeleri bu ITSM yapılandırma / ayrıntılı günlük analizi görünümü, OMS içinde daha sonra bu adı kullanın. |
| **Bağlantı türü seçin**   | Seçin **Cherwell.** |
| **Kullanıcı Adı**   | BT Hizmet Yönetimi bağlayıcı bağlanabilir Cherwell kullanıcı adını yazın. |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve herhangi bir yere OMS hizmet içinde depolanmaz.|
| **Sunucu URL'si**   | BT Hizmet Yönetimi Bağlayıcısı için bağlanmak istediğiniz Cherwell örneğinizi URL'sini yazın. |
| **İstemci kimliği**   | Cherwell örneğinde oluşturulan bu bağlantısının kimlik doğrulaması için istemci kimliği yazın.   |
| **Veri Eşitleme kapsamı**   | BT Hizmet Yönetimi Bağlayıcısı üzerinden eşitlemek istediğiniz Cherwell iş öğelerini seçin.  Bu iş öğeleri günlük analizi aktarılır.   **Seçenekler:** olaylar, değişiklik istekleri. |
| **Eşitleme veri** | Verileri istediğiniz beri geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümde yeni yapılandırma öğesi oluşturma** | Yapılandırma öğeleri ITSM ürün oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, OMS etkilenen CI desteklenen ITSM sistem içinde yapılandırma öğeleri (Cı'ler var olmayan) durumunda oluşturur. **Varsayılan**: devre dışı. |

Başarıyla bağlanıldı ve eşitlenen kullanıldığında:

- Seçili iş öğeleri bu Cherwell bağlantısından OMS günlük analizi alınır. Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğu iş öğeleri.
- Olayları ve olayları bu OMS Cherwell örneğinden oluşturabilirsiniz. Daha fazla bilgi: Oluştur ITSM iş öğeleri OMS uyarılar ve ITSM oluşturmak için iş öğelerini OMS günlüklerinden.

Daha fazla bilgi: [OMS uyarılar için iş öğeleri oluşturma ITSM](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) ve [OMS günlükleri oluşturmak ITSM iş öğelerinden](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>İstemci kimliği için Cherwell oluşturun.

Cherwell için istemci kimliği/anahtar oluşturmak için aşağıdaki yordamı kullanın:

1. Cherwell Örneğiniz için yönetici olarak oturum açın
2. Tıklatın **güvenlik** > **Düzenle REST API İstemci Ayarları**.
3. Seçin **oluştur yeni istemci** > **gizli**.

    ![Cherwell kullanıcı kimliği](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Sonraki adımlar
 - [OMS uyarılar için ITSM iş öğeleri oluşturma](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [OMS günlüklerinden ITSM iş öğeleri oluşturma](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [Bağlantınız için Görünüm günlük analizi](log-analytics-itsmc-overview.md#using-the-solution)
