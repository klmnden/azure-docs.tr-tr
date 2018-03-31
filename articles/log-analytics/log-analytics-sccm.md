---
title: Configuration Manager günlük Analizi'ne bağlamak | Microsoft Docs
description: Bu makalede, Configuration Manager günlük Analizi'ne bağlayın ve veri çözümleme başlatmak için gereken adımları gösterir.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: magoedte
ms.openlocfilehash: 5ff0687fe99f0853e29e5f0d814a8555c367027c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Configuration Manager günlük Analizi'ne bağlayın
System Center Configuration Manager ortamınızı aygıt koleksiyonu veri eşitlemesine Azure günlük Analizi'ne bağlanmak ve bu koleksiyonları günlük analizi ve Azure Otomasyonu başvuru.  

## <a name="prerequisites"></a>Önkoşullar

Günlük analizi System Center Configuration Manager geçerli dalı, sürüm 1606 ve üstünü destekler.  

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış
Aşağıdaki adımlar Configuration Manager tümleştirmesi günlük analizi ile yapılandırmak için gereken adımları özetler.  

1. Azure portalında, Configuration Manager bir Web uygulaması ve/veya Web API uygulaması kaydetmek ve istemci Kimliğini ve istemci gizli anahtarı kayıt Azure Active Directory'den olduğundan emin olun. Bkz: [Active Directory kaynaklara erişebilir uygulama ve hizmet sorumlusu oluşturmak için kullanım portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) bu adımı gerçekleştirmek hakkında ayrıntılı bilgi.
2. Azure portalında [Configuration Manager (kayıtlı web uygulaması) günlük analizi erişim izni vermek](#grant-configuration-manager-with-permissions-to-log-analytics).
3. Yapılandırma Yöneticisi'nde [OMS Bağlantısı Ekleme Sihirbazı'nı kullanarak bir bağlantı ekleyin](#add-an-oms-connection-to-configuration-manager).
4. Yapılandırma Yöneticisi'nde [bağlantı özelliklerini güncelleştirmek](#update-oms-connection-properties) süresi veya kaybolursa şimdiye kadar parola veya istemci gizli anahtarı.
5. [Microsoft Monitoring Agent'ı yükleyip](#download-and-install-the-agent) bilgisayarda çalışan Configuration Manager hizmet bağlantı noktası site sistemi rolü. Aracı için günlük analizi çalışma alanındaki Configuration Manager veri gönderir.
6. Günlük analizi içinde [koleksiyonları Configuration Manager içinden içe](#import-collections) bilgisayar grupları olarak.
7. Günlük analizi içinde verisinin Yapılandırma Yöneticisi'nden olarak [bilgisayar grupları](log-analytics-computer-groups.md).

Daha fazla bilgiyi OMS Configuration Manager bağlanma hakkında [verilerini Yapılandırma Yöneticisi'nden Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>Günlük analizi izinlerine sahip GRANT Configuration Manager
Aşağıdaki yordamda, verdiğiniz *katkıda bulunan* AD uygulaması ve hizmet sorumlusu daha önce oluşturduğunuz Configuration Manager için günlük analizi çalışma alanınızdaki rol.  Bir çalışma alanı zaten yoksa bkz [Azure günlük analizi çalışma alanı oluşturma](log-analytics-quick-create-workspace.md) devam etmeden önce.  Bu, Configuration Manager'ın kimlik doğrulaması ve bağlanmak için günlük analizi çalışma alanınız sağlar.  

> [!NOTE]
> Günlük analizi Configuration Manager için izinleri belirtmeniz gerekir. Aksi takdirde, Configuration Manager'da Yapılandırma Sihirbazı'nı kullandığınızda, bir hata iletisi alırsınız.
>

1. Azure portalında tıklatın **tüm hizmetleri** sol üst köşedeki bulundu. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Günlük analizi çalışma alanları, listeden değiştirmek için çalışma alanını seçin.
3. Sol bölmeden seçin **erişim denetimi (IAM)**.
4. Erişim denetimi sayfasında tıklatın **Ekle** ve **izinleri eklemek** bölmesinde görünür.
5. İçinde **izinleri eklemek** bölmesi altında **rol** aşağı açılan listesinde seçin **katkıda bulunan** rol.  
6. Altında **atamak için erişim** aşağı açılan listesinde, AD içinde daha önce oluşturduğunuz Configuration Manager uygulaması'nı seçin ve ardından **Tamam**.  

## <a name="download-and-install-the-agent"></a>Aracıyı indirin ve yükleyin
Makaleyi gözden [Azure günlük analizi hizmeti bağlanmak Windows bilgisayarlara](log-analytics-agent-windows.md) Configuration Manager hizmeti barındıran bilgisayarda Microsoft izleme aracısı yüklemek için kullanılabilen yöntemler anlamak için bağlantı noktası site sistemi rolü.  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Configuration Manager'a bir OMS bağlantısı ekleme
Bir OMS bağlantısı eklemek için Configuration Manager ortamınızı içermelidir bir [hizmet bağlantı noktası](https://technet.microsoft.com/library/mt627781.aspx) çevrimiçi modu için yapılandırılmış.

1. İçinde **Yönetim** çalışma Configuration Manager, select **OMS bağlayıcı**. Bu açılır **OMS Bağlantısı Ekleme Sihirbazı'nı**. **İleri**’yi seçin.
2. Üzerinde **genel** aşağıdaki eylemleri yaptıktan ve her öğeye ilişkin ayrıntıları sahip sonra seçin onaylayın **sonraki**.

   1. Azure portalında bir Web uygulaması ve/veya Web API uygulaması Configuration Manager kaydınız ve sahip olduğunuz [kayıt istemci kimliği](../active-directory/active-directory-integrating-applications.md).
   2. Azure portalında Azure Active Directory'de kayıtlı uygulama için bir uygulama gizli anahtar oluşturduğunuzu düşünün.  
   3. Azure portalında OMS erişim izni ile kayıtlı web uygulaması sağladık.  
      ![OMS Sihirbazı genel sayfası bağlantı](./media/log-analytics-sccm/sccm-console-general01.png)
3. Üzerinde **Azure Active Directory** ekranında, sağlayarak günlük analizi için bağlantı ayarlarınızı yapılandırın, **Kiracı**, **istemci kimliği**, ve **istemci Gizli anahtar**seçeneğini belirleyip **sonraki**.  
   ![OMS Sihirbazı Azure Active Directory sayfası bağlantı](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Varsa, diğer yordamlar başarıyla, ardından bilgileri üzerinde gerçekleştirilen **OMS bağlantı yapılandırması** ekranı otomatik olarak bu sayfada görüntülenir. Bağlantı ayarlarının bilgileri için görünmelidir, **Azure aboneliği**, **Azure kaynak grubu**, ve **Operations Management Suite çalışma alanı**.  
   ![OMS Sihirbazı OMS bağlantısı sayfası bağlantısı](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Sihirbazın giriş bilgileri kullanarak günlük analizi hizmetine bağlanır. Hizmet ile eşitleme ve ardından istediğiniz cihaz koleksiyonları seçin **Ekle**.  
   ![Koleksiyonları seçin](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Bağlantı ayarlarınızı doğrulayın **Özet** ekran sonra seçin **sonraki**. **İlerleme** ekran bağlantı durumunu gösterir, ardından gereken **tam**.

> [!NOTE]
> Günlük analizi için hiyerarşinizdeki en üst katman sitesine bağlanmanız gerekir. Tek başına bir birincil site günlük Analizi'ne bağlayın ve sonra bir merkezi yönetim sitesi ortamınıza eklemek, bağlantı yeni hiyerarşi içinde silip gerekir.
>
>

Configuration Manager için günlük analizi bağladığınız sonra eklemek veya koleksiyonları kaldırın ve bağlantı özelliklerini görüntüleyin.

## <a name="update-log-analytics-connection-properties"></a>Günlük analizi bağlantı özelliklerini güncelleştirmek
Bir parola veya istemci gizli anahtarı hiç süresi veya kaybolursa, günlük analizi bağlantı özellikleri el ile güncelleştirmeniz gerekir.

1. Yapılandırma Yöneticisi'nde gidin **bulut Hizmetleri**seçeneğini belirleyip **OMS bağlayıcı** açmak için **OMS bağlantısı özellikleri** sayfası.
2. Bu sayfada tıklatın **Azure Active Directory** görüntülemek için **Kiracı**, **istemci kimliği**, **istemci gizli anahtarı sona erme**. **Doğrulama** , **istemci gizli anahtarı** süresi dolmuşsa.

## <a name="import-collections"></a>Koleksiyonları İçeri Aktar
Configuration Manager'a bir OMS bağlantısı eklendi ve aracının yüklü sonra Configuration Manager hizmeti bağlantısı çalıştıran bilgisayarda noktası site sistemi rolü, sonraki adım günlük analizi de Yapılandırma Yöneticisi'nden koleksiyonları içeri aktarmak için bilgisayar grupları.

Cihaz koleksiyonları, hiyerarşideki verileri içeri aktarmak için başlangıç yapılandırmasını tamamladıktan sonra koleksiyon üyelik bilgilerini üyelik güncel kalmasını sağlamak için 3 saatte alınır. Bu dilediğiniz zaman devre dışı bırakmayı seçebilirsiniz.

1. Azure portalında tıklatın **tüm hizmetleri** sol üst köşedeki bulundu. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Listenizde günlük analizi çalışma alanları, Configuration Manager ile kayıtlı çalışma alanını seçin.  
3. **Gelişmiş ayarlar**’ı seçin.<br><br> ![Log Analytics Gelişmiş Ayarlar](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Seçin **bilgisayar grupları** ve ardından **SCCM**.  
5. Seçin **alma Configuration Manager koleksiyon üyelikleri** ve ardından **kaydetmek**.  
   ![Bilgisayar grupları - SCCM sekmesi](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Yapılandırma Yöneticisi'nden verileri görüntüleme
Configuration Manager için bir OMS bağlantısı eklendi ve Configuration Manager hizmet bağlantı noktası site sistem rolünü çalıştıran bilgisayar üzerindeki aracının yüklü sonra Aracıdan verileri için günlük analizi gönderilir. Configuration Manager koleksiyonlarınızı günlük analizi içinde görünür [bilgisayar grupları](log-analytics-computer-groups.md). Gruplardan görüntüleyebilirsiniz **Configuration Manager** altında sayfa **Settings\Computer grupları**.

Koleksiyonları içeri aktarıldıktan sonra koleksiyon üyelikleri kaç bilgisayarlarla algılanan görebilirsiniz. İçe aktarılan koleksiyon sayısı de görebilirsiniz.

![Bilgisayar grupları - SCCM sekmesi](./media/log-analytics-sccm/sccm-computer-groups02.png)

Bunlardan herhangi birinin tıkladığınızda, tüm içeri aktarılan gruplarının ya da her gruba ait tüm bilgisayarları görüntüleyen arama açılır. Kullanarak [günlük arama](log-analytics-log-searches.md), Configuration Manager veri ayrıntılı analizini başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük arama](log-analytics-log-searches.md) Configuration Manager verileriniz hakkında ayrıntılı bilgi görüntülemek için.
