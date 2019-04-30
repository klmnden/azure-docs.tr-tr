---
title: Configuration Manager'a bağlanmak için Azure İzleyici | Microsoft Docs
description: Bu makalede, Azure İzleyici'de çalışma alanına Configuration Manager'a bağlanmak ve verileri analiz etmeye başlamak için adımları gösterilmektedir.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: magoedte
ms.openlocfilehash: e5cf89b7fe01946de9944a7026ec448cd55dd6dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60741691"
---
# <a name="connect-configuration-manager-to-azure-monitor"></a>Configuration Manager'a bağlanmak için Azure İzleyici
System Center Configuration Manager ortamınızı Azure İzleyici eşitleme cihaz koleksiyonu verilere ve Azure İzleyici ve Azure Otomasyonu bu koleksiyonlara başvuru.  

## <a name="prerequisites"></a>Önkoşullar

Azure İzleyici, System Center Configuration Manager geçerli dal sürümü 1606 ve üstünü destekler.  

## <a name="configuration-overview"></a>Yapılandırmaya genel bakış
Aşağıdaki adımlar, Azure İzleyici ile Configuration Manager tümleştirmesini yapılandırma adımlarını özetler.  

1. Azure portalında bir Web uygulaması ve/veya Web API uygulaması olarak Configuration Manager kaydetmek ve istemci Kimliğini ve istemci gizli anahtarını kaydı Azure Active Directory'den olduğundan emin olun. Bkz: [Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](../../active-directory/develop/howto-create-service-principal-portal.md) bu adımı tamamlamak hakkında ayrıntılı bilgi için.
2. Azure portalında [Configuration Manager (kayıtlı web uygulaması) Azure İzleyici erişim izni vermek](#grant-configuration-manager-with-permissions-to-log-analytics).
3. Configuration Manager'da OMS Bağlantısı Ekleme Sihirbazı'nı kullanarak bir bağlantı ekleyin.
4. Parolası veya istemci gizli anahtarı hiç olmadığı kadar süresi veya kaybolursa, Yapılandırma Yöneticisi'nde, bağlantı özelliklerini güncelleştirir.
5. [Microsoft Monitoring Agent'ı yükleyip](#download-and-install-the-agent) bilgisayarda çalışan Configuration Manager hizmet bağlantı noktası site sistemi rolü. Aracı, Configuration Manager verilerini Azure İzleyici'de Log Analytics çalışma alanına gönderir.
6. Azure İzleyici'de [koleksiyonları Configuration Manager'dan içeri aktarma](#import-collections) bilgisayar grupları olarak.
7. Azure İzleyici'de verisinin Configuration Manager'dan olarak [bilgisayar grupları](computer-groups.md).

Daha fazla Azure İzleyici Configuration Manager bağlama hakkında [verileri Configuration Manager'dan Microsoft Log Analytics çalışma alanına Eşitle](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="grant-configuration-manager-with-permissions-to-log-analytics"></a>GRANT Configuration Manager'ı Log analytics'e izinlerle
Aşağıdaki yordamda vermesi *katkıda bulunan* Log Analytics çalışma alanınızda AD uygulaması ve hizmet sorumlusu, daha önce oluşturduğunuz Configuration Manager için rol.  Bir çalışma alanı zaten yoksa bkz [Azure İzleyici'de çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md) devam etmeden önce.  Bu, Configuration Manager'ın kimlik doğrulaması ve Log Analytics çalışma alanınıza bağlanmak için sağlar.  

> [!NOTE]
> Log Analytics çalışma alanında Configuration Manager için izinleri belirtmelisiniz. Aksi takdirde, Configuration Manager'da Yapılandırma Sihirbazı'nı kullandığınızda, bir hata iletisi alırsınız.
>

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics**’i seçin.
2. Log Analytics çalışma alanlarınızın listesinde değiştirmek için çalışma alanı seçin.
3. Sol bölmeden **erişim denetimi (IAM)**.
4. Erişim denetimi (IAM) sayfası, tıklayın **rol ataması Ekle** ve **rol ataması Ekle** bölmesi görünür.
5. İçinde **rol ataması Ekle** bölmesi altında **rol** açılan listesini seçin **katkıda bulunan** rol.  
6. Altında **erişim Ata** aşağı açılan listesinde, AD'de daha önce oluşturduğunuz Configuration Manager uygulaması'nı seçin ve ardından **Tamam**.  

## <a name="download-and-install-the-agent"></a>Aracısını indirme ve yükleme
Makalesini gözden geçirin [Azure İzleyici'de Azure Connect Windows bilgisayarlara](agent-windows.md) Configuration Manager hizmet bağlantı noktasını barındıran bilgisayarda Microsoft Monitoring Agent'ı yüklemek için kullanılabilen yöntemler anlamak için Site sistemi rolü.  

## <a name="add-a-log-analytics-connection-to-configuration-manager"></a>Configuration Manager için bir Log Analytics Bağlantısı Ekle
Log Analytics bağlantısı eklemek için Configuration Manager ortamınızı olmalıdır bir [hizmet bağlantı noktası](https://technet.microsoft.com/library/mt627781.aspx) çevrimiçi modu için yapılandırılmış.

1. İçinde **Yönetim** çalışma Configuration Manager, select **OMS Bağlayıcısı**. Bu açılır **Log Analytics Bağlantı Sihirbazı ekleme**. **İleri**’yi seçin.

   >[!NOTE]
   >OMS, artık Azure İzleyicisi'nin bir özelliği olan Log Analytics da adlandırılır.
   
2. Üzerinde **genel** aşağıdaki eylemleri yaptığınız her öğenin ayrıntılarını sahip ve ardından seçin ve onaylayın **sonraki**.

   1. Azure portalında bir Web uygulaması ve/veya Web API uygulaması olarak Configuration Manager kaydettiğinize göre ve sahip olduğunuz [kayıt istemci kimliği](../../active-directory/develop/quickstart-v1-add-azure-ad-app.md).
   2. Azure portalında Azure Active Directory'de kayıtlı uygulama için bir uygulama gizli anahtarı oluşturdunuz.  
   3. Azure portalında Azure İzleyici'de Log Analytics çalışma alanına erişmek için izne sahip kayıtlı web uygulaması sağladık.  
      ![Log Analytics Sihirbazı genel sayfası bağlantısı](./media/collect-sccm/sccm-console-general01.png)
3. Üzerinde **Azure Active Directory** ekranında, sağlayarak Log Analytics çalışma alanı bağlantı ayarlarını yapılandırın, **Kiracı**, **istemci kimliği**ve **İstemci gizli anahtarı**, ardından **sonraki**.  
   ![Log Analytics Sihirbazı Azure Active Directory sayfasına bağlantı](./media/collect-sccm/sccm-wizard-tenant-filled03.png)
4. Diğer tüm yordamları başarıyla sonra bilgileri üzerinde gerçekleştirilir, **OMS bağlantı yapılandırması** ekran otomatik olarak bu sayfada görüntülenir. Bağlantı ayarları için daha fazla bilgi için görünmelidir, **Azure aboneliği**, **Azure kaynak grubu**, ve **Operations Management Suite çalışma alanı**.  
   ![Log Analytics Sihirbazı Log Analytics bağlantısını sayfasına bağlantı](./media/collect-sccm/sccm-wizard-configure04.png)
5. Sihirbaz, giriş bilgileri kullanarak Log Analytics çalışma alanına bağlanır. Eşitleme hizmeti ile ve ardından istediğiniz cihaz koleksiyonları seçin **Ekle**.  
   ![Koleksiyonları seçin](./media/collect-sccm/sccm-wizard-add-collections05.png)
6. Bağlantı ayarlarınızı doğrulayın **özeti** ekran ve ardından **sonraki**. **İlerleme** ekran bağlantı durumunu gösterir, ardından gereken **tam**.

> [!NOTE]
> Azure İzleyicisi, hiyerarşinizdeki üst katman sitesine bağlamanız gerekir. Azure İzleyicisi ile tek başına bir birincil site bağlantı ve ortamınız için bir merkezi yönetim sitesine ekleyin, silin ve yeni bir hiyerarşi içinde bağlantıyı yeniden gerekir.
>
>

Azure İzleyici Configuration Manager bağlantılı sonra ekleyin veya koleksiyonları kaldırın ve bağlantı özelliklerini görüntüleyin.

## <a name="update-log-analytics-workspace-connection-properties"></a>Log Analytics çalışma alanı bağlantı özelliklerini güncelleştirin
Parolası veya istemci gizli anahtarı hiç olmadığı kadar süresi veya kaybolursa, Log Analytics bağlantı özelliklerini el ile güncelleştirmeniz gerekir.

1. Configuration Manager'da gidin **Cloud Services**, ardından **OMS Bağlayıcısı** açmak için **OMS bağlantısı özellikleri** sayfası.
2. Bu sayfada tıklayın **Azure Active Directory** görüntülemek için sekmesinde, **Kiracı**, **istemci kimliği**, **istemci gizli anahtarı süre sonu**. **Doğrulama** , **istemci gizli anahtarının** süresi dolmuşsa.

## <a name="import-collections"></a>Koleksiyonları İçeri Aktar
Configuration Manager için bir Log Analytics bağlantısı eklendi ve aracının yüklü bilgisayarda çalışan Configuration Manager hizmet bağlantı noktası site sistemi rolünün, sonraki adımda Azure Yapılandırma Yöneticisi'nden koleksiyonları içeri aktarmak için Bilgisayar grupları izleyin.

Cihaz koleksiyonları hiyerarşinizden içeri aktarmak için başlangıç yapılandırmasını tamamladıktan sonra koleksiyon üyelik bilgilerini üyelik güncel kalmasını sağlamak için 3 saatte alınır. Bu, istediğiniz zaman devre dışı seçebilirsiniz.

1. Azure portalının sol alt köşesinde bulunan **Tüm hizmetler**’e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Log Analytics çalışma alanları**.
2. Log Analytics çalışma alanlarınızın listesinde, Configuration Manager ile kayıtlı bir çalışma alanı seçin.  
3. **Gelişmiş ayarlar**’ı seçin.
4. Seçin **bilgisayar grupları** seçip **SCCM**.  
5. Seçin **alma Configuration Manager koleksiyon üyelikleri** ve ardından **Kaydet**.  
   ![Bilgisayar grupları - SCCM sekmesi](./media/collect-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Verileri Configuration Manager'dan görüntüle
Configuration Manager için bir Log Analytics bağlantısı eklendi ve Configuration Manager hizmet bağlantı noktası site sistem rolünü çalıştıran bilgisayarda aracının yüklü sonra Aracıdan verileri Log Analytics çalışma alanına Azure İzleyici'de gönderilir. Azure İzleyici'de, Configuration Manager koleksiyon olarak görünür. [bilgisayar grupları](../../azure-monitor/platform/computer-groups.md). Gruplardan görüntüleyebileceğiniz **Configuration Manager** altındaki **Settings\Computer grupları**.

Koleksiyonları içeri aktarıldıktan sonra kaç koleksiyon üyelikleri bilgisayarlarla algılanan görebilirsiniz. Aktarılan koleksiyonları sayısını da görebilirsiniz.

![Bilgisayar grupları - SCCM sekmesi](./media/collect-sccm/sccm-computer-groups02.png)

Tek tıkladığınızda, tüm içeri aktarılan gruplarının ya da her gruba ait tüm bilgisayarların görüntüleyen arama açılır. Kullanarak [günlük araması](../../azure-monitor/log-query/log-query-overview.md), Configuration Manager verilerini detaylı olarak çözümlenmesi başlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük araması](../../azure-monitor/log-query/log-query-overview.md) Configuration Manager verileriniz hakkında ayrıntılı bilgi görüntülemek için.
