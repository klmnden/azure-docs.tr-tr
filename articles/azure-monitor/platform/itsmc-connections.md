---
title: Azure Log Analytics'te bağlantılar BT Hizmet Yönetimi Bağlayıcısı ile desteklenen | Microsoft Docs
description: Bu makalede, ITSM ürünler/hizmetler merkezi olarak izlemeye ve ITSM çalışma öğelerini yönetmek için Azure İzleyici'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile bağlanma hakkında bilgi sağlar.
documentationcenter: ''
author: jyothirmaisuri
manager: riyazp
editor: ''
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: v-jysur
ms.openlocfilehash: ffd9c4bfc934faff1664ff39c0e979a9d6c09487
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399788"
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector"></a>ITSM ürünler/hizmetler BT Hizmet Yönetimi Bağlayıcısı ile bağlanma
Bu makalede, iş öğeleri merkezi olarak yönetmek için Log Analytics'te ITSM ürününüz/hizmetiniz ve BT Hizmet Yönetimi Bağlayıcısı'nı (ITSMC) arasındaki bağlantıyı yapılandırmak hakkında bilgi sağlar. ITSMC hakkında daha fazla bilgi için bkz: [genel bakış](../../azure-monitor/platform/itsmc-overview.md).

Aşağıdaki ITSM ürünler/hizmetler desteklenir. Ürün için ITSMC bağlanma hakkında ayrıntılı bilgi görüntülemek için bir ürün seçin.

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](#connect-provance-to-it-service-management-connector-in-azure)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-azure)

> [!NOTE]
> 
> ITSM Bağlayıcısı, yalnızca bulut tabanlı ServiceNow örneklerine bağlanabilirsiniz. Şirket içi ServiceNow örneği şu anda desteklenmemektedir.

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-azure"></a>System Center Service Manager BT hizmetine bağlanmak azure'da Yönetimi Bağlayıcısı

Aşağıdaki bölümler, System Center Service Manager ürün azure'da ITSMC bağlanma hakkında ayrıntılı bilgi sağlar.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- ITSMC yüklü. Daha fazla bilgi: [Ekleme BT Hizmet Yönetimi bağlayıcı çözümü](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- (Web uygulaması) Service Manager Web uygulaması dağıtılıp yapılandırıldıktan. Web uygulaması hakkında bilgi [burada](#create-and-deploy-service-manager-web-app-service).
- Karma bağlantı oluşturulur ve yapılandırılır. Daha fazla bilgi: [Karma bağlantı yapılandırma](#configure-the-hybrid-connection).
- Desteklenen sürümler Service Manager'ın:  2012 R2 veya 2016.
- Kullanıcı rolü:  [Gelişmiş işleç](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Bağlantı yordamı

System Center Service Manager Örneğiniz için ITSMC bağlanmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma alanı veri kaynakları** tıklayın **ITSM bağlantıları**.

    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin en üstünde tıklayın **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri belirtin ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]
> 
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz System Center Service Manager örneği için bir ad yazın.  Bu örnekte iş öğelerini yapılandırma / ayrıntılı log analytics'e görüntülemek, bu adı daha sonra kullanın. |
| **İş ortağı türü**   | Seçin **System Center Service Manager**. |
| **Sunucu URL'si**   | Service Manager Web uygulamasının URL'sini yazın. Service Manager Web uygulaması hakkında daha fazla bilgiyi [burada](#create-and-deploy-service-manager-web-app-service).
| **İstemci kimliği**   | Web uygulaması kimlik doğrulaması için (otomatik komut dosyası kullanarak) oluşturulan istemci kimliği yazın. Otomatik komut dosyası hakkında daha fazla bilgiyi [burada.](../../azure-monitor/platform/itsmc-service-manager-script.md)|
| **İstemci gizli anahtarı**   | İstemci gizli anahtarını girin. Bu kimlik için oluşturulan   |
| **Veri Eşitleme**   | ITSMC eşitlemek istediğiniz Service Manager iş öğelerini seçin.  Bu iş öğeleri, Log Analytics'e aktarılır. **Seçenekler:**  Olaylar, değişiklik istekleri'ni tıklatın.|
| **Veri Eşitleme kapsamı** | Verilerden istediğiniz geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümüne yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, Log Analytics etkilenen CIS desteklenen ITSM sistemi içinde yapılandırma öğeleri (durumunda, mevcut olmayan CIS) oluşturur. **Varsayılan**: devre dışı. |

![Service manager bağlantısı](media/itsmc-connections/service-manager-connection.png)

**Başarıyla bağlandı ve eşitlenmiş**:

- Seçili iş öğelerini Service Manager'dan Azure'a aktarılır **Log Analytics.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğa iş öğeleri.

- Olaylar, Log Analytics uyarılarını veya günlük kayıtları veya bu Service Manager örneğindeki Azure uyarıları oluşturabilirsiniz.


Daha fazla bilgi edinin: [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Service Manager web uygulaması hizmeti oluşturma ve dağıtma

Şirket içi Service Manager ile azure'da ITSMC bağlanmak için Microsoft GitHub üzerinde bir Service Manager Web uygulaması oluşturdu.

Service Manager için ITSM Web uygulamasını ayarlama için aşağıdakileri yapın:

- **Web uygulaması dağıtma** – Web uygulaması dağıtma, özelliklerini ayarlayın ve Azure AD kimlik doğrulaması. Kullanarak web uygulaması dağıtabilirsiniz [betik otomatik](../../azure-monitor/platform/itsmc-service-manager-script.md) Microsoft, sağlamıştır.
- **Karma bağlantıyı yapılandırın** - [bu bağlantıyı yapılandırmak](#configure-the-hybrid-connection), el ile.

#### <a name="deploy-the-web-app"></a>Web uygulaması dağıtma
Otomatik kullanma [betik](../../azure-monitor/platform/itsmc-service-manager-script.md) Web uygulamasına dağıtmak için özellikleri ayarlayın ve Azure AD kimlik doğrulaması.

Aşağıdaki gerekli ayrıntıları sağlayarak betiği çalıştırın:

- Azure Abonelik Ayrıntıları
- Kaynak grubu adı
- Location
- Service Manager sunucusu ayrıntıları (sunucu adı, etki alanı, kullanıcı adı ve parola)
- Web uygulamanız için site adı öneki
- ServiceBus Namespace.

Betik (benzersiz hale getirmek amacıyla birkaç ek dize ile birlikte) belirtilen adı kullanarak Web uygulaması oluşturur. Ürettiği **Web uygulaması URL'si**, **istemci kimliği** ve **gizli**.

Bir bağlantı ile ITSMC oluşturduğunuzda değerleri kaydetmek, bunları kullanın.

**Web uygulaması yükleme denetimi**

1. Git **Azure portalında** > **kaynakları**.
2. Web uygulaması seçin, **ayarları** > **uygulama ayarları**.
3. Komut dosyası aracılığıyla uygulama dağıtımı sırasındaki sağlanan Service Manager örneğinin hakkında bilgiyi onaylamanız istenir.

### <a name="configure-the-hybrid-connection"></a>Karma bağlantı yapılandırma

Service Manager örneğinin azure'da ITSMC bağlanan karma bağlantıyı yapılandırmak için aşağıdaki yordamı kullanın.

1. Service Manager Web uygulamasının altında Bul **Azure kaynaklarını**.
2. Tıklayın **ayarları** > **ağ**.
3. Altında **karma bağlantılar**, tıklayın **karma bağlantı uç noktalarınızı yapılandırın**.

    ![Karma bağlantı ağ](media/itsmc-connections/itsmc-hybrid-connection-networking-and-end-points.png)
4. İçinde **karma bağlantılar** dikey penceresinde tıklayın **karma Bağlantı Ekle**.

    ![Karma Bağlantı Ekle](media/itsmc-connections/itsmc-new-hybrid-connection-add.png)

5. İçinde **karma bağlantılar ekleme** dikey penceresinde tıklayın **oluştur yeni karma bağlantı**.

    ![Yeni Karma bağlantı](media/itsmc-connections/itsmc-create-new-hybrid-connection.png)

6. Aşağıdaki değerleri yazın:

   - **Uç nokta adı**: Yeni Karma bağlantı için bir ad belirtin.
   - **Uç nokta konak**: Service Manager yönetim sunucusunun FQDN'si.
   - **Uç nokta bağlantı noktası**: Tür 5724
   - **Servicebus ad alanı**: Var olan bir Service Bus ad alanı kullanabilir veya yeni bir tane oluşturun.
   - **Konum**: konumu seçin.
   - **Ad**: Oluşturmakta olduğunuz servicebus için bir ad belirtin.

     ![Karma bağlantı değerleri](media/itsmc-connections/itsmc-new-hybrid-connection-values.png)
6. Tıklayın **Tamam** kapatmak için **karma bağlantı oluşturma** dikey penceresinde ve karma bağlantı oluşturmaya başlayabilirsiniz.

    Karma bağlantı oluşturulduktan sonra bu dikey pencerenin altında görüntülenir.

7. Karma bağlantı oluşturulduktan sonra bağlantıyı seçin ve tıklayın **seçili karma bağlantıyı Ekle**.

    ![Yeni Karma bağlantı](media/itsmc-connections/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Dinleyici Kurulumu yapılandırın

Karma bağlantı dinleyicisi Kurulumu yapılandırmak için aşağıdaki yordamı kullanın.

1. İçinde **karma bağlantılar** dikey penceresinde tıklayın **bağlantı yöneticisini indirin** ve System Center Service Manager örneğinin çalıştığı makineye yükleyin.

    Yükleme tamamlandıktan sonra **karma Bağlantı Yöneticisi kullanıcı Arabirimi** seçeneği altında kullanılabilir **Başlat** menüsü.

2. Tıklayın **karma Bağlantı Yöneticisi kullanıcı Arabirimi** , sizden Azure kimlik bilgileriniz istenir.

3. Azure kimlik bilgilerinizle oturum açma ve karma bağlantı oluşturulduğu aboneliğinizi seçin.

4. **Kaydet**’e tıklayın.

Karma bağlantı başarıyla bağlandı.

![başarılı bir karma bağlantı](media/itsmc-connections/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]
> 
> Sonra karma bağlantısı oluşturulur, doğrulayın ve dağıtılan Service Manager Web uygulamasını ziyaret ederek bağlantıyı test edin. Azure'da ITSMC bağlanmaya çalışmadan önce bağlantının başarılı olduğundan emin olun.

Aşağıdaki örnek resimde, başarılı bir bağlantı ayrıntılarını gösterir:

![Karma bağlantı testi](media/itsmc-connections/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-azure"></a>Servicenow'ı bağlama için BT Hizmet Yönetimi Bağlayıcısı Azure

Aşağıdaki bölümler, azure'da ITSMC ServiceNow ürün bağlanma hakkında ayrıntılı bilgi sağlar.

### <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların karşılandığından emin olun:
- ITSMC yüklü. Daha fazla bilgi: [Ekleme BT Hizmet Yönetimi bağlayıcı çözümü](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Servicenow'ı desteklenen sürümler: Madrid, Londra, Kingston, Cakarta, Istanbul, Helsinki, Cenevre.

**ServiceNow yöneticileri kendi ServiceNow örneği aşağıdakileri yapması gerekir**:
- İstemci Kimliğini ve istemci gizli anahtarı için ServiceNow ürün oluşturur. İstemci Kimliğini ve gizli anahtarı oluşturma hakkında daha fazla bilgi için aşağıdaki bilgileri gerektiği gibi bakın:

    - [OAuth Madrid için ayarlama](https://docs.servicenow.com/bundle/madrid-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Londra için ayarlama](https://docs.servicenow.com/bundle/london-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Kingston için ayarlama](https://docs.servicenow.com/bundle/kingston-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Cakarta için OAuth ayarlama](https://docs.servicenow.com/bundle/jakarta-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Istanbul için ayarlama](https://docs.servicenow.com/bundle/istanbul-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [OAuth Helsinki için ayarlama](https://docs.servicenow.com/bundle/helsinki-platform-administration/page/administer/security/task/t_SettingUpOAuth.html)
    - [Geneva için OAuth ayarlama](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/security/task/t_SettingUpOAuth.html)


- Microsoft Log Analytics tümleştirmesi (ServiceNow uygulama) için kullanıcı uygulamayı yükleyin. [Daha fazla bilgi edinin](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1 ).
- Kullanıcı uygulamasının yüklü olduğu için tümleştirme kullanıcı rolü oluşturun. Tümleştirme kullanıcı rolü oluşturma hakkında bilgi [burada](#create-integration-user-role-in-servicenow-app).

### <a name="connection-procedure"></a>**Bağlantı yordamı**
ServiceNow bağlantısı oluşturmak için aşağıdaki yordamı kullanın:


1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma alanı veri kaynakları** tıklayın **ITSM bağlantıları**.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin en üstünde tıklayın **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri belirtin ve tıklayın **Tamam** bağlantı oluşturmak için.


> [!NOTE]
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz ServiceNow örneği için bir ad yazın.  İçinde bu ITSM iş öğelerini yapılandırma / ayrıntılı log analytics'e görüntülemek, Log Analytics'te daha sonra bu adı kullanın. |
| **İş ortağı türü**   | Seçin **ServiceNow**. |
| **Kullanıcı Adı**   | ServiceNow uygulama ITSMC bağlantıyı desteklemek için oluşturduğunuz tümleştirme kullanıcı adını yazın. Daha fazla bilgi: [ServiceNow uygulama kullanıcı rolü oluşturma](#create-integration-user-role-in-servicenow-app).|
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not**: Kullanıcı adı ve parola kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ITSMC hizmetinde herhangi bir yerde depolanmaz.  |
| **Sunucu URL'si**   | ITSMC için bağlanmak istediğiniz ServiceNow örneğinin URL'sini yazın. |
| **İstemci kimliği**   | Daha önce oluşturulan OAuth2 kimlik doğrulamasında kullanmak istediğiniz istemci kimliği yazın.  İstemci kimliği ve gizli anahtarı oluşturma hakkında daha fazla bilgi:   [OAuth Kurulumu](https://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **İstemci gizli anahtarı**   | İstemci gizli anahtarını girin. Bu kimlik için oluşturulan   |
| **Veri Eşitleme kapsamı**   | Azure Log analytics'e ITSMC aracılığıyla eşitlemek istediğiniz ServiceNow çalışma öğelerini seçin.  Seçilen değerleri log analytics'e aktarılır.   **Seçenekler:**  Olaylar ve değişiklik istekleri.|
| **Veri Eşitleme** | Verilerden istediğiniz geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümüne yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CIS desteklenen ITSM sistemi içinde yapılandırma öğeleri (durumunda, mevcut olmayan CIS) oluşturur. **Varsayılan**: devre dışı. |

![ServiceNow bağlantısı](media/itsmc-connections/itsm-connection-servicenow-connection-latest.png)

**Başarıyla bağlandı ve eşitlenmiş**:

- ServiceNow örneğinin Seçili iş öğeleri, Azure'a aktarılır **Log Analytics.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğa iş öğeleri.

- Olaylar, Log Analytics uyarılarını veya günlük kayıtları veya bu ServiceNow örneğinin Azure uyarıları oluşturabilirsiniz.

Daha fazla bilgi edinin: [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="create-integration-user-role-in-servicenow-app"></a>ServiceNow uygulaması tümleştirme kullanıcı rolü oluşturun

Aşağıdaki yordam kullanıcı:

1. Ziyaret [ServiceNow deposu](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.1) yükleyip **servicenow'ı ve Microsoft OMS tümleştirmesi için kullanıcı uygulaması** ServiceNow örneğinizin içinde.
   
   >[!NOTE]
   >Azure İzleyici sürekli geçiş Microsoft Operations Management Suite (OMS) gelen bir parçası olarak, OMS artık Log Analytics adlandırılır.     
2. Yüklemeden sonra sol gezinti çubuğunda ServiceNow örneğinin, arama ve seçme Microsoft OMS entegratörü ziyaret edin.  
3. Tıklayın **yükleme Yapılacaklar listesi**.

   Durum olarak görüntülenen **tamamlanmadı** oluşturulacak kullanıcı rolünün henüz ise.

4. Metin kutularındaki yanındaki **tümleştirme kullanıcı oluşturma**, azure'da ITSMC bağlanıp kullanıcı kullanıcı adını girin.
5. Bu kullanıcı için parola girin ve tıklayın **Tamam**.  

> [!NOTE]
> 
> Azure'da ServiceNow bağlantı kurmak için bu kimlik bilgilerini kullanın.

Yeni oluşturulan kullanıcı atanmış varsayılan rolleri ile görüntülenir.

**Varsayılan rol**:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.user
-   ITIL
-   template_editor
-   view_changer

Kullanıcı başarıyla oluşturulduktan sonra durumunu **denetleyin yükleme Yapılacaklar listesi** uygulama için oluşturulan kullanıcı rolünün ayrıntılarını listeleme tamamlandı olarak taşır.

> [!NOTE]
> 
> ITSM Bağlayıcısı ServiceNow Örneğinizde yüklü modüllerin olmadan ServiceNow olayları gönderebilirsiniz. ServiceNow Örneğinizde EventManagement modülünü kullanıyorsanız ve olayları veya uyarıları bağlayıcı kullanarak ServiceNow içinde oluşturmak istediğiniz aşağıdaki rolleri için tümleştirme kullanıcısının ekleyin:
> 
>    - evt_mgmt_integration
>    - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-azure"></a>Provance bağlanmak için BT Hizmet Yönetimi Bağlayıcısı Azure

Aşağıdaki bölümler, Provance ürününüzü azure'da ITSMC bağlanma hakkında ayrıntılı bilgi sağlar.


### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:


- ITSMC yüklü. Daha fazla bilgi: [Ekleme BT Hizmet Yönetimi bağlayıcı çözümü](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Azure AD ile - provance uygulama kaydedilmelidir ve istemci kimliği kullanımına sunulur. Ayrıntılı bilgi için bkz. [active directory kimlik doğrulamasını yapılandırma](../../app-service/configure-authentication-provider-aad.md).

- Kullanıcı rolü:  Yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Provance bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma alanı veri kaynakları** tıklayın **ITSM bağlantıları**.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin en üstünde tıklayın **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri belirtin ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]
> 
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC ile bağlanmak istediğiniz Provance örneği için bir ad yazın.  İçinde bu ITSM iş öğelerini yapılandırma / ayrıntılı log analytics'e görüntülemek, bu adı daha sonra kullanın. |
| **İş ortağı türü**   | Seçin **Provance**. |
| **Kullanıcı Adı**   | İçin ITSMC bağlanan kullanıcı adını yazın.    |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve her yerde ITSMC hizmet içinde depolanmaz. _|
| **Sunucu URL'si**   | ITSMC için bağlanmak istediğiniz Provance örneğinizin URL'sini yazın. |
| **İstemci kimliği**   | Provance Örneğinizde oluşturulan Bu bağlantı kimliğini doğrulamak için istemci kimliği yazın.  İstemci kimliği, daha fazla bilgi bkz [active directory kimlik doğrulamasını yapılandırma](../../app-service/configure-authentication-provider-aad.md). |
| **Veri Eşitleme kapsamı**   | Azure Log analytics'e ITSMC aracılığıyla eşitlemek istediğiniz Provance çalışma öğelerini seçin.  Bu iş öğeleri, log analytics'e aktarılır.   **Seçenekler:**   Olaylar, değişiklik istekleri'ni tıklatın.|
| **Veri Eşitleme** | Verilerden istediğiniz geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümüne yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CIS desteklenen ITSM sistemi içinde yapılandırma öğeleri (durumunda, mevcut olmayan CIS) oluşturur. **Varsayılan**: devre dışı.|

![Provance bağlantı](media/itsmc-connections/itsm-connections-provance-latest.png)

**Başarıyla bağlandı ve eşitlenmiş**:

- Seçili iş öğelerini bu Provance örneğinden Azure'a aktarılır **Log Analytics.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğa iş öğeleri.

- Olaylar, Log Analytics uyarılarını veya günlük kayıtları veya bu Provance örnekte Azure uyarıları oluşturabilirsiniz.

Daha fazla bilgi edinin: [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

## <a name="connect-cherwell-to-it-service-management-connector-in-azure"></a>Cherwell bağlanmak için BT Hizmet Yönetimi Bağlayıcısı Azure

Aşağıdaki bölümler ITSMC azure'da Cherwell ürününüzü bağlanma hakkında ayrıntılı bilgi sağlar.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- ITSMC yüklü. Daha fazla bilgi: [Ekleme BT Hizmet Yönetimi bağlayıcı çözümü](../../azure-monitor/platform/itsmc-overview.md#adding-the-it-service-management-connector-solution).
- Oluşturulan istemci kimliği. Daha fazla bilgi: [İstemci kimliği oluşturmak için Cherwell](#generate-client-id-for-cherwell).
- Kullanıcı rolü:  Yönetici.

### <a name="connection-procedure"></a>Bağlantı yordamı

Provance bağlantı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portalında Git **tüm kaynakları** ve Ara **ServiceDesk(YourWorkspaceName)**

2.  Altında **çalışma alanı veri kaynakları** tıklayın **ITSM bağlantıları**.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin en üstünde tıklayın **Ekle**.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri belirtin ve tıklayın **Tamam** bağlantı oluşturmak için.

> [!NOTE]
> 
> Bu parametre zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı Adı**   | ITSMC için bağlanmak istediğiniz Cherwell örneği için bir ad yazın.  İçinde bu ITSM iş öğelerini yapılandırma / ayrıntılı log analytics'e görüntülemek, bu adı daha sonra kullanın. |
| **İş ortağı türü**   | Seçin **Cherwell.** |
| **Kullanıcı Adı**   | İçin ITSMC bağlanabilen Cherwell kullanıcı adını yazın. |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Not:** Kullanıcı adı ve parola kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ITSMC hizmetinde herhangi bir yerde depolanmaz.|
| **Sunucu URL'si**   | ITSMC için bağlanmak istediğiniz Cherwell örneğinizin URL'sini yazın. |
| **İstemci kimliği**   | Cherwell Örneğinizde oluşturulan Bu bağlantı kimliğini doğrulamak için istemci kimliği yazın.   |
| **Veri Eşitleme kapsamı**   | ITSMC eşitlemek istediğiniz Cherwell çalışma öğelerini seçin.  Bu iş öğeleri, log analytics'e aktarılır.   **Seçenekler:**  Olaylar, değişiklik istekleri'ni tıklatın. |
| **Veri Eşitleme** | Verilerden istediğiniz geçen gün sayısını yazın. **Üst sınır**: 120 gün. |
| **ITSM çözümüne yeni yapılandırma öğesi oluşturma** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçili olduğunda, ITSMC etkilenen CIS desteklenen ITSM sistemi içinde yapılandırma öğeleri (durumunda, mevcut olmayan CIS) oluşturur. **Varsayılan**: devre dışı. |


![Provance bağlantı](media/itsmc-connections/itsm-connections-cherwell-latest.png)

**Başarıyla bağlandı ve eşitlenmiş**:

- Seçili iş öğelerini bu Cherwell örneğinden Azure'a aktarılır **Log Analytics.** Bu bir özetini görüntüleyebilirsiniz BT Hizmet Yönetimi Bağlayıcısı kutucuğa iş öğeleri.

- Olaylar, Log Analytics uyarılarını veya günlük kayıtları veya bu Cherwell örnekte Azure uyarıları oluşturabilirsiniz.

Daha fazla bilgi edinin: [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts).

### <a name="generate-client-id-for-cherwell"></a>Cherwell için istemci kodu oluşturma

Cherwell için istemci kimliği/anahtarı oluşturmak için aşağıdaki yordamı kullanın:

1. Cherwell Örneğinize yönetici olarak oturum açın
2. Tıklayın **güvenlik** > **Düzenle REST API İstemci Ayarları**.
3. Seçin **oluştur yeni istemci** > **gizli**.

    ![Cherwell kullanıcı kimliği](media/itsmc-connections/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Sonraki adımlar
 - [Azure uyarılarından ITSM iş öğeleri oluşturma](../../azure-monitor/platform/itsmc-overview.md#create-itsm-work-items-from-azure-alerts)
