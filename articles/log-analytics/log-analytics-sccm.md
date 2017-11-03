---
title: "Configuration Manager günlük Analizi'ne bağlamak | Microsoft Docs"
description: "Bu makalede, Configuration Manager günlük Analizi'ne bağlayın ve veri çözümleme başlatmak için gereken adımları gösterir."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: 62d31ed486458245156f7fc832294d662c62991e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Configuration Manager günlük Analizi'ne bağlayın
System Center Configuration Manager OMS günlük analizi aygıt koleksiyonu veri eşitlemesine bağlanabilir. Bu veri, Configuration Manager hiyerarşisinden OMS kullanılabilir hale getirir.

## <a name="prerequisites"></a>Ön koşullar

Günlük analizi System Center Configuration Manager geçerli dalı, sürüm 1606 ve üstünü destekler.  

## <a name="configuration-overview"></a>Yapılandırmasına genel bakış
Aşağıdaki adımlar Configuration Manager günlük Analizi'ne bağlanmak için işlem özetler.  

1. Azure Yönetim Portalı'nda Configuration Manager bir Web uygulaması ve/veya Web API uygulaması kaydetmek ve istemci Kimliğini ve istemci gizli anahtarı kayıt Azure Active Directory'den olduğundan emin olun. Bkz: [Active Directory kaynaklara erişebilir uygulama ve hizmet sorumlusu oluşturmak için kullanım portal](../azure-resource-manager/resource-group-create-service-principal-portal.md) hakkında ayrıntılı bilgi için bu adımı gerçekleştirmek.
2. Azure Yönetim Portalı'nda [OMS erişim izni olan Configuration Manager (kayıtlı web uygulaması) girin](#provide-configuration-manager-with-permissions-to-oms).
3. Yapılandırma Yöneticisi'nde [OMS Bağlantısı Ekleme Sihirbazı'nı kullanarak bir bağlantı ekleyin](#add-an-oms-connection-to-configuration-manager).
4. Yapılandırma Yöneticisi'nde [bağlantı özelliklerini güncelleştirmek](#update-oms-connection-properties) süresi veya kaybolursa şimdiye kadar parola veya istemci gizli anahtarı.
5. OMS Portalı'ndan bilgilerle [Microsoft Monitoring Agent'ı yükleyip](#download-and-install-the-agent) bilgisayarda çalışan Configuration Manager hizmet bağlantı noktası site sistemi rolü. Aracı için OMS Configuration Manager veri gönderir.
6. Günlük analizi içinde [koleksiyonları Configuration Manager içinden içe](#import-collections) bilgisayar grupları olarak.
7. Günlük analizi içinde verisinin Yapılandırma Yöneticisi'nden olarak [bilgisayar grupları](log-analytics-computer-groups.md).

Daha fazla bilgiyi OMS Configuration Manager bağlanma hakkında [verilerini Yapılandırma Yöneticisi'nden Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Configuration Manager için OMS izinlerle girin
Aşağıdaki yordam, OMS erişmek için gerekli izinlere sahip Azure Yönetim Portalı sağlar. Özellikle, vermelidir *katkıda bulunan rolü* için OMS Configuration Manager'a bağlanmak Azure Yönetim Portalı izin vermek üzere kaynak grubundaki kullanıcılar.

> [!NOTE]
> OMS Configuration Manager için izinleri belirtmeniz gerekir. Aksi takdirde, Configuration Manager'da Yapılandırma Sihirbazı'nı kullanırken bir hata iletisi alırsınız.
>
>

1. Açmak [Azure portal](https://portal.azure.com/) tıklatıp **Gözat** > **günlük analizi (OMS)** günlük analizi (OMS) dikey penceresini açın.  
2. Üzerinde **günlük analizi (OMS)** dikey penceresinde tıklatın **Ekle** açmak için **OMS çalışma** dikey.  
   ![OMS dikey penceresi](./media/log-analytics-sccm/sccm-azure01.png)
3. Üzerinde **OMS çalışma** dikey penceresinde, aşağıdaki bilgileri sağlayın ve ardından **Tamam**.

   * **OMS çalışma**
   * **Abonelik**
   * **Kaynak grubu**
   * **Konum**
   * **Fiyatlandırma katmanı**  
     ![OMS dikey penceresi](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > Yukarıdaki örnekte yeni bir kaynak grubu oluşturur. Kaynak grubu yalnızca Configuration Manager bu örnekte OMS çalışma alanı izinleri sağlamak için kullanılır.
     >
     >
4. Tıklatın **Gözat** > **kaynak grupları** açmak için **kaynak grupları** dikey.
5. İçinde **kaynak grupları** dikey penceresinde, kaynak grubu, açmak için yukarıda oluşturduğunuz tıklatarak &lt;kaynak grubu adı&gt; ayarlar dikey penceresi.  
   ![kaynak grubu ayarları dikey](./media/log-analytics-sccm/sccm-azure03.png)
6. İçinde &lt;kaynak grubu adı&gt; dikey penceresinde, erişim denetimi (IAM) açmak için tıklatın &lt;kaynak grubu adı&gt; kullanıcılar dikey penceresi.  
   ![kaynak grubu kullanıcılar dikey penceresi](./media/log-analytics-sccm/sccm-azure04.png)  
7. İçinde &lt;kaynak grubu adı&gt; kullanıcılar dikey penceresi, tıklatın **Ekle** açmak için **erişim Ekle** dikey.
8. İçinde **erişim Ekle** dikey penceresinde tıklatın **bir rol seçin**ve ardından **katkıda bulunan** rol.  
   ![bir rol seçin](./media/log-analytics-sccm/sccm-azure05.png)  
9. Tıklatın **kullanıcıları eklemek**, Configuration Manager kullanıcısını seçin, **seçin**ve ardından **Tamam**.  
   ![kullanıcıları ekleme](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Configuration Manager'a bir OMS bağlantısı ekleme
Bir OMS bağlantısı eklemek için Configuration Manager ortamınızı içermelidir bir [hizmet bağlantı noktası](https://technet.microsoft.com/library/mt627781.aspx) çevrimiçi modu için yapılandırılmış.

1. İçinde **Yönetim** çalışma Configuration Manager, select **OMS bağlayıcı**. Bu açılır **OMS Bağlantısı Ekleme Sihirbazı'nı**. Seçin **sonraki**.
2. Üzerinde **genel** aşağıdaki eylemleri yaptıktan ve her öğeye ilişkin ayrıntıları sahip sonra seçin onaylayın **sonraki**.

   1. Azure Yönetim Portalı'nda bir Web uygulaması ve/veya Web API uygulaması Configuration Manager kaydınız ve sahip olduğunuz [kayıt istemci kimliği](../active-directory/active-directory-integrating-applications.md).
   2. Azure Yönetim Portalı'nda bir uygulama gizli anahtarı Azure Active Directory'de kayıtlı uygulaması oluşturduğunuzu düşünün.  
   3. Azure Yönetim Portalı'nda OMS erişim izni ile kayıtlı web uygulaması sağladık.  
      ![OMS Sihirbazı genel sayfası bağlantı](./media/log-analytics-sccm/sccm-console-general01.png)
3. Üzerinde **Azure Active Directory** ekranında, sağlayarak OMS için bağlantı ayarlarınızı yapılandırın, **Kiracı** , **istemci kimliği** , ve **istemci gizli anahtar**  seçeneğini belirleyip **sonraki**.  
   ![OMS Sihirbazı Azure Active Directory sayfası bağlantı](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Varsa, diğer yordamlar başarıyla, ardından bilgileri üzerinde gerçekleştirilen **OMS bağlantı yapılandırması** ekranı otomatik olarak bu sayfada görüntülenir. Bağlantı ayarlarının bilgileri için görünmelidir, **Azure aboneliği** , **Azure kaynak grubu** , ve **Operations Management Suite çalışma alanı**.  
   ![OMS Sihirbazı OMS bağlantısı sayfası bağlantısı](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Sihirbazın giriş bilgileri kullanarak OMS hizmetine bağlanır. OMS ile eşitleme ve ardından istediğiniz cihaz koleksiyonları seçin **Ekle**.  
   ![Koleksiyonları seçin](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Bağlantı ayarlarınızı doğrulayın **Özet** ekran sonra seçin **sonraki**. **İlerleme** ekran bağlantı durumunu gösterir, ardından gereken **tam**.

> [!NOTE]
> OMS hiyerarşinizdeki en üst katman sitesine bağlanmanız gerekir. OMS bir tek başına birincil siteye bağlanmak ve sonra bir merkezi yönetim sitesi ortamınıza eklerseniz, yeni hiyerarşideki OMS bağlantısı silip gerekir.
>
>

Configuration Manager için OMS bağladığınız sonra eklemek veya koleksiyonları kaldırın ve OMS bağlantısı özelliklerini görüntüleyin.

## <a name="update-oms-connection-properties"></a>OMS bağlantısı özelliklerini güncelleştir
Bir parola veya istemci gizli anahtarı hiç süresi veya kaybolursa, OMS bağlantısı özellikleri el ile güncelleştirmeniz gerekir.

1. Yapılandırma Yöneticisi'nde gidin **bulut Hizmetleri** seçeneğini belirleyip **OMS bağlayıcı** açmak için **OMS bağlantısı özellikleri** sayfası.
2. Bu sayfada tıklatın **Azure Active Directory** görüntülemek için **Kiracı**, **istemci kimliği**, **istemci gizli anahtarı sona erme**. **Doğrulama** , **istemci gizli anahtarı** süresi dolmuşsa.

## <a name="download-and-install-the-agent"></a>Aracıyı indirin ve yükleyin
1. OMS portalında [OMS Aracısı kurulum dosyasını karşıdan](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Yükleme ve Configuration Manager hizmet bağlantı noktası site sistem rolünü çalıştıran bilgisayarda aracıyı yapılandırmak için aşağıdaki yöntemlerden birini kullanın:
   * [Kurulumu kullanarak aracı yükleme](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Komut satırını kullanarak aracı yükleme](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Azure Otomasyonu'nda DSC kullanarak aracı yükleme](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Koleksiyonları İçeri Aktar
Configuration Manager için bir OMS bağlantısı eklendi ve aracının yüklü sonra Configuration Manager hizmeti bağlantısı çalıştıran bilgisayarda noktası site sistemi rolü, sonraki adım Yapılandırma Yöneticisi'nden bilgisayarla OMS koleksiyonları içeri aktarmak için gruplar.

Başlangıcından etkinleştirildikten sonra koleksiyon üyelik bilgilerini koleksiyon üyelikleri güncel kalmasını sağlamak için 3 saatte alınır. Herhangi bir zamanda başlangıcından devre dışı bırakmayı seçebilirsiniz.

1. OMS Portalı'nda tıklatın **ayarları**.
2. Tıklatın **bilgisayar grupları** sekmesini ve sonra **SCCM** sekmesi.
3. Seçin **alma Configuration Manager koleksiyon üyelikleri** ve ardından **kaydetmek**.  
   ![Bilgisayar grupları - SCCM sekmesi](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Yapılandırma Yöneticisi'nden verileri görüntüleme
Configuration Manager için bir OMS bağlantısı eklendi ve Configuration Manager hizmet bağlantı noktası site sistem rolünü çalıştıran bilgisayar üzerindeki aracının yüklü sonra Aracıdan verileri için OMS gönderilir. Configuration Manager koleksiyonlarınızı OMS içinde görünür [bilgisayar grupları](log-analytics-computer-groups.md). Gruplardan görüntüleyebilirsiniz **Configuration Manager** altında sayfa **bilgisayar grupları** içinde **ayarları**.

Koleksiyonları içeri aktarıldıktan sonra koleksiyon üyelikleri kaç bilgisayarlarla algılanan görebilirsiniz. İçe aktarılan koleksiyon sayısı de görebilirsiniz.

![Bilgisayar grupları - SCCM sekmesi](./media/log-analytics-sccm/sccm-computer-groups02.png)

Bunlardan herhangi birinin tıkladığınızda, tüm içeri aktarılan gruplarının ya da her gruba ait tüm bilgisayarları görüntüleyen arama açılır. Kullanarak [günlük arama](log-analytics-log-searches.md), Configuration Manager veri ayrıntılı analizini başlatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* Kullanım [günlük arama](log-analytics-log-searches.md) Configuration Manager verileriniz hakkında ayrıntılı bilgi görüntülemek için.
