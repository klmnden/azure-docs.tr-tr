---
title: Azure Güvenlik Merkezi'nde Iaas için Gelişmiş veri güvenliği | Microsoft Docs
description: " Azure Güvenlik Merkezi'nde Iaas için Gelişmiş veri güvenliği etkinleştirme hakkında bilgi edinin. "
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: monhaber
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2019
ms.author: monhaber
ms.openlocfilehash: e7420adfe1608df39ef72124817f1d6dadf07db8
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66400152"
---
# <a name="advanced-data-security-for-sql-servers-on-iaas"></a>Iaas SQL Server'lar için Gelişmiş veri güvenliği
Azure Virtual Machines'de SQL sunucuları için Gelişmiş veri güvenliği, Gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir pakettir. Şu anda, görünmesini ve olası veritabanı güvenlik açıklarını azaltmaya ve veritabanınız için tehdit oluşturabilecek anormal etkinlikleri algılamaya yönelik işlevleri içerir. 

Bu güvenlik sunan Azure VM'lerin SQL sunucuları için kullanılan aynı temel teknoloji dayalı [Azure SQL veritabanı gelişmiş veri güvenliği paket](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="overview"></a>Genel Bakış

Gelişmiş SQL güvenlik özellikleri, güvenlik açığı değerlendirmesi ve Gelişmiş tehdit koruması oluşan bir dizi gelişmiş veri güvenliği sağlar.

* [Güvenlik Açığı değerlendirmesi](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) olduğu bir kolayca bulmak, izlemek ve yardımcı hizmetini yapılandırmak olası veritabanı güvenlik açıklarını düzeltin. Güvenlik durumunuzu görünürlük sağlar ve güvenlik sorunlarını çözün ve, veritabanı fortifications geliştirmek için gerekli adımları içerir.
* [Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) erişmek veya SQL server'ınızı yararlanmak için sıra dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Sürekli olarak veritabanınızı şüpheli etkinlikler için izler ve anormal veritabanı erişim modellerinin üzerinde eylem odaklı güvenlik uyarıları sağlar. Bu uyarılar, şüpheli etkinlik ayrıntılarını sağlayın ve önerilen araştırmak ve tehdidi azaltmak için Eylemler.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Azure vm'lerde SQL gelişmiş veri güvenliği ile çalışmaya başlama

Aşağıdaki adımlar Azure Vm'leri üzerinde SQL ile gelişmiş veri güvenliği başlamanıza yardımcı olmak.

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Azure vm'lerde SQL için Gelişmiş veri güvenliği ayarlayın

**Başlamadan önce**: Çözümlenen güvenlik günlükleri depolamak için bir Log Analytics çalışma alanı ihtiyacınız vardır. Elinizde değil sonra bir kolayca açıklandığı gibi oluşturabilirsiniz [Azure portalında Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

1. Log Analytics çalışma alanına SQL server'ı barındıran VM bağlanın. Yönergeler için [Azure İzleyici bağlanmak Windows bilgisayarlara](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).

1. Azure Marketi'nde Git [SQL gelişmiş veri güvenliği çözüm](https://ms.portal.azure.com/#create/Microsoft.SQLAdvancedDataSecurity).
(Bkz: aşağıdaki görüntüde olarak Market arama seçeneğini kullanarak bulabilirsiniz.) **SQL gelişmiş veri güvenliği** sayfası açılır.

    ![Iaas için Gelişmiş veri güvenliği](./media/security-center-advanced-iaas-data/sql-advanced-data-security.png)

1. **Oluştur**’a tıklayın. Çalışma görüntülenir.

    ![Gelişmiş veri güvenliği oluşturma](./media/security-center-advanced-iaas-data/sql-advanced-data-create.png)

1. ' E tıklayın çalışma alanını seçin **Oluştur**.

   ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/sql-workspace.png)

1. Yeniden [sanal makinenin SQL server](https://docs.microsoft.com/sql/database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services?view=sql-server-2017).


## <a name="explore-and-investigate-security-alerts"></a>Keşfedin ve güvenlik uyarılarını inceleyin

Görüntüleyebilir ve geçerli güvenlik uyarılarınızı yönetme.

1. Tıklayın **Güvenlik Merkezi** > **güvenlik uyarıları**ve bir uyarıya tıklayın.

    ![Uyarı bulunamadı](./media/security-center-advanced-iaas-data/find-alert.png)

1. Gelen **Saldırıya uğrayan kaynak** sütun, Saldırıya uğramış bir kaynağa tıklayın.

1. Uyarı ayrıntılarını ve geçerli tehdit araştırma ve gelecekteki tehditleri adresleme eylemleri görmek için kaydırmanız **genel bilgiler** sayfasında ve **düzeltme adımları** bölümünde, üzerinde tıklayın **Araştırma ADIMLARININ** bağlantı.

    ![Düzeltme adımları](./media/security-center-advanced-iaas-data/remediation-steps.png)

1. Uyarının tetiklenmesine ile ilişkili olan günlükleri görüntülemek için Git **günlük analizi çalışma alanı** ve aşağıdaki adımları uygulayın:

     > [!NOTE]
     > Varsa **günlük analizi çalışma alanı** sol menüde görünür değil'a tıklayın **tüm hizmetleri**, araması **günlük analizi çalışma alanı**.

    1. Sütunları görüntüleme mutlaka **fiyatlandırma katmanı** ve **Workspaceıd** sütunları. (**Günlük analizi çalışma alanı** > **sütunları Düzenle**, ekleme **fiyatlandırma katmanı** ve **Workspaceıd**.)

     ![Upravit Sloupce](./media/security-center-advanced-iaas-data/edit-columns.png)

    1. Uyarı günlükleri içeren çalışma alanını tıklatın.

    1. Altında **genel** menüsünü tıklatın **günlükleri**

    1. Göz tıklayın **SQLAdvancedThreatProtection** tablo. Günlükler listelenmektedir.

     ![Günlükleri görüntüle](./media/security-center-advanced-iaas-data/view-logs.png)

## <a name="set-up-email-notification-for-atp-alerts"></a>ATP uyarılar için e-posta bildirimlerini ayarlama 

ASC uyarılar oluşturulduğunda bir e-posta bildirimi almak için alıcıların listesi ayarlayabilirsiniz. E-posta, Azure Güvenlik Merkezi'nde uyarı ile ilgili tüm ayrıntılar için doğrudan bir bağlantı içerir. 

1. Git **Güvenlik Merkezi** > **Güvenlik İlkesi** ve ilgili aboneliği tıklatın satırında **ayarları düzenleyin >** .

    ![Abonelik ayarları](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. Gelen **ayarları** menüsünü tıklatın **e-posta bildirimleri**. 
1. İçinde **e-posta adresi** metin kutusunda, bildirim almak için e-posta adreslerini girin. E-posta adreslerini virgül (,) ile ayırarak birden fazla e-posta adresini girebilirsiniz.  Örneğin admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com

      ![E-posta Ayarları](./media/security-center-advanced-iaas-data/email-settings.png)

1. İçinde **e-posta bildirimi** ayarlar, aşağıdaki seçenekleri:
  
    * **Yüksek önem düzeyindeki uyarılar için e-posta bildirimi gönder**: Tüm uyarılar için e-posta göndermek yerine, yalnızca yüksek önem düzeyindeki uyarılar için gönderin.
    * **Ayrıca abonelik sahiplerine e-posta bildirimleri gönder**:  Bildirim abonelikleri sahipleri için çok gönderin.

1. Üst kısmındaki **e-posta bildirimleri** ekranında **Kaydet**.

  > [!NOTE]
  > Tıkladığınızdan emin olun **Kaydet** penceresi veya yeni kapatmadan önce **e-posta bildirimi** ayarlar kaydedilmeyecek.

## <a name="explore-vulnerability-assessment-reports"></a>Güvenlik Açığı değerlendirme raporları keşfedin

Güvenlik Açığı değerlendirmesi Pano tüm veritabanlarınız arasında değerlendirme sonuçlarınızı genel bir bakış sağlar. Veritabanlarını SQL Server sürümü, veritabanları ve denetimleri risk dağıtım göre başarısız olan genel bir özeti ve başarısız bir özetiyle birlikte göre dağılımını görüntüleyebilirsiniz.

Güvenlik Açığı değerlendirme sonuçlarını ve doğrudan Log Analytics raporları görüntüleyebilirsiniz.

1. Gelişmiş Veri güvenlik çözümü ile Log Analytics çalışma alanınıza gidin.
1. Gidin **çözümleri** seçip **SQL güvenlik açığı değerlendirmesi** çözüm.
1. İçinde **özeti** bölmesinde tıklayın **özetini görüntüle** ve seçin, **SQL güvenlik açığı değerlendirmesi raporu**.

    ![SQL değerlendirmesi raporu](./media/security-center-advanced-iaas-data/ads-sql-server-1.png)

    Rapor panosunu yükler. Zaman penceresi ayarlandığından emin olun en az **son 7 günde** 7 gün başına bir kez, sabit bir zamanlamada veritabanlarınızda güvenlik açığı değerlendirmesi tarama çalıştırıldığından.

    ![Son 7 gün ayarlayın](./media/security-center-advanced-iaas-data/ads-sql-server-2.png)

1. Daha fazla ayrıntı için detaya gitmek için herhangi bir Pano tıklayın. Örneğin:

   1. Bir güvenlik açığı iade tıklayın **başarısız denetler özeti** tüm veritabanlarında bu denetim için sonuçları içeren bir Log Analytics tablo görüntülemek üzere bölüm. Sonuçlara sahip olanları önce listelenir.

   1. Ardından, güvenlik açığı açıklaması ve etkisi, durumu, ilişkili riski ve bu veritabanında gerçek sonuçlar dahil olmak üzere her güvenlik açığı ayrıntılarını görüntülemek için tıklayın. Ayrıca, bu güvenlik açığını çözümlemek için bu onay ve düzeltme bilgilerini gerçekleştirmek için çalıştırılan gerçek sorgu görebilirsiniz.

    ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/ads-sql-server-3.png)

    ![Çalışma alanını seçme](./media/security-center-advanced-iaas-data/ads-sql-server-4.png)

1. Dilim ve verileri gereksinimlerinize göre nedensellik ilişkilerini, güvenlik açığı değerlendirmesi sonuçları verilerinizi Log Analytics sorguları çalıştırabilirsiniz.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Gelişmiş tehdit koruması için SQL Server Azure Vm'leri uyarılar
Erişmek veya exploit SQL sunucuları için olağan dışı ve zararlı olabilecek girişimleri tarafından uyarılar oluşturulur. Bu olaylar aşağıdaki uyarılar tetikleyebilirsiniz:

### <a name="anomalous-access-pattern-alerts"></a>Anormal erişim düzeni uyarıları

* **Olağan dışı bir konumdan erişim:** SQL Server, burada kişi SQL sunucusuna olağan dışı bir coğrafi konumdan oturum açmış olduğu erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Olası nedenler:
     * Bir saldırgan veya eski bir kötü amaçlı kullanan SQL Server'ınızı eriştiğini.
     * Bir kullanıcının gönderdiğini, yeni bir konumdan SQL Server'ınızı eriştiğini.
* **Zararlı olabilecek bir uygulamadan erişim**: zararlı bir uygulama veritabanına erişmek için kullanıldığında, bu uyarı tetiklenir. Olası nedenler:
     * Bir saldırganın yaygın saldırı araçlarının kullandığı SQL ihlal çalışılıyor.
     * Yasal bir güvenlik testlerini.
* **Erişim**: SQL Server, burada birisi bir sorumludan (SQL kullanıcısı) kullanarak SQL sunucusuna oturum açmış olduğu erişim deseninde değişiklik olduğunda bu uyarı tetiklenir. Olası nedenler:
     * Bir saldırgan veya eski bir kötü amaçlı kullanan SQL Server'ınızı eriştiğini. 
     * Bir kullanıcının gönderdiğini, SQL Server'dan yeni bir sorumlunun eriştiğini.
* **Deneme yanılma SQL kimlik**: Bu uyarı, bir olağandışı yüksek sayıda farklı kimlik bilgileri başarısız oturum açma denemesi olduğunda tetiklenir. Olası nedenler:
     * Deneme yanılma kullanarak SQL güvenliğini aşmak çalışan bir saldırgan.
     * Yasal bir güvenlik testlerini.

### <a name="potential-sql-injection-attacks-coming"></a>Olası SQL ekleme saldırıları (gelen)

* **SQL ekleme güvenlik açığı**: Bu uyarı, veritabanında bir uygulama hatalı SQL deyimi oluşturduğunda tetiklenir. Bu uyarı, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Olası nedenler:
     * Hatalı SQL deyimini oluşturan uygulama kodunda hata
     * Uygulama kodu veya depolanan yordamlar, hatalı SQL deyimi yapılandırılırken kullanıcı girişini temizlemez ve SQL Ekleme sırasında bu açıktan yararlanılabilir
* **Olası SQL eklemesi**: Bu uyarı, SQL ekleme bir tanımlı uygulama güvenlik açığına karşı etkin bir açıktan yararlanma meydana geldiğinde tetiklenir. Bu, saldırganın güvenlik açığına sahip uygulama kodu veya depolanan yordamları kullanan kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.
