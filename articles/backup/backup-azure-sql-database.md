---
title: SQL Server veritabanlarını azure'a yedekleyin | Microsoft Docs
description: Bu öğreticide, SQL Server'ı Azure'a yedekleme açıklanmaktadır. Makalede, SQL Server kurtarma de açıklanmaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: raynew
ms.openlocfilehash: d99a3d23959cfdd9bd068fbde3a882eb1bc9b4ae
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58847293"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Azure VM'lerindeki SQL Server Backup hakkında

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedekleyebilirsiniz [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Yedekleme işlemi

Bu çözüm, SQL veritabanlarının yedeklemelerini almak için SQL yerel API'lerden yararlanır.

* BT veritabanları için korumak ve sorgulamak istediğiniz SQL Server VM belirttiğinizde, Azure Backup hizmeti bir iş yükü backup uzantısı VM'de adıyla yükleyecek `AzureBackupWindowsWorkload`  uzantısı.
* Bu uzantı, düzenleyici ve SQL eklentisi oluşur. Düzenleyici yedekleme, yedekleme ve geri yükleme yapılandırma gibi çeşitli işlemler için iş akışlarını tetiklemekten sorumlu olsa da, eklenti için gerçek veri akışı sorumludur.
* Bu VM'de veritabanlarını bulmak için Azure Backup, hesabı oluşturan `NT SERVICE\AzureWLBackupPluginSvc`. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir. Azure Backup yararlanır `NT AUTHORITY\SYSTEM` SQL ortak bir oturum açma olacak şekilde bu hesabınızın olması gerekir böylece veritabanı bulma/sorgulama için hesap. SQL Server VM Azure Market'te oluşturmadıysanız, bir hata alabilirsiniz **UserErrorSQLNoSysadminMembership**. Bu meydana gelirse [bu yönergeleri izleyin](backup-azure-sql-database.md).
* Seçili veritabanlarında koruma, tetikleyici yapılandırdıktan sonra backup hizmeti yedekleme zamanlamaları ve uzantı, sanal makinede yerel olarak önbelleğe alır diğer ilke ayrıntıları Düzenleyici ayarlar 
* Zamanlanan tarihte Düzenleyici eklenti ile iletişim kurar ve yedekleme verileri SQL Server'dan VDI kullanarak akış başlatır.  
* Eklenti, bu nedenle bir hazırlama konumu ihtiyacını doğrudan kurtarma Hizmetleri kasasına, verileri gönderir. Veriler şifrelenir ve depolama hesaplarında Azure Backup hizmeti tarafından depolanır.
* Veri aktarımı tamamlandığında, yedekleme hizmetiyle işleme Düzenleyicisi onaylar.

  ![SQL yedekleme mimarisi](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce aşağıdakileri doğrulayın:

1. Azure'da çalışan bir SQL Server örneğine sahip olduğunuzdan emin olun. Yapabilecekleriniz [kolayca bir SQL Server örneği oluşturma](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) Market'te.
2. Gözden geçirme [göz önünde bulundurarak özellik](#feature-consideration-and-limitations) ve [senaryo Destek](#scenario-support).
3. [Sık sorulan soruları gözden](faq-backup-sql-server.md) bu senaryo hakkında.

## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen dağıtımlar** | SQL Marketplace Azure Vm'leri ve Market dışı (SQL Server el ile yüklenen) sanal makineleri desteklenir.
**Desteklenen coğrafi bölgeler** | Avustralya Güneydoğu (ASE), Doğu Avustralya (AE) <br> Brezilya Güney (BRS)<br> Kanada Orta (CNC), Kanada Doğu (CE)<br> Güney Doğu Asya (SEA), Doğu Asya (EA) <br> East US (EUS), East US 2 (EUS2), West Central US (WCUS), West US (WUS); West US 2 (WUS 2) North Central US (NCUS) Central US (CUS) South Central US (SCUS) <br> Hindistan Orta (Inc), Hindistan Güney (INS) <br> Japonya Doğu (JPE), Japonya Batı (JPW) <br> Kore Orta (KRC), Kore Güney (KRS) <br> Kuzey Avrupa (NE), Batı Avrupa <br> UK Güney (UKS) BK Batı (UKW)
**Desteklenen işletim sistemleri** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux şu anda desteklenmemektedir.
**Desteklenen SQL Server sürümleri** | SQL Server 2017 SQL Server 2016, SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.
**Desteklenen .NET sürümleri** | .NET framework 4.5.2 ve üzeri VM'de yüklü

## <a name="feature-consideration-and-limitations"></a>Özellik önemli noktalar ve sınırlamalar

- SQL Server Yedekleme, Azure portalında yapılandırılabilir veya **PowerShell**. CLI desteklemiyoruz.
- SQL Server çalıştıran bir VM, Azure genel IP adresleri erişmek için Internet bağlantısı gerektirir.
- SQL Server **yük devretme kümesi örneği (FCI)** ve SQL Server her zaman yük devretme kümesi örneği üzerinde desteklenmez.
- Yansıtma veritabanları ve veritabanı anlık görüntüleri için yedekleme ve geri yükleme işlemleri desteklenmez.
- Birden fazla yedekleme çözümleri, tek başına SQL Server Yedekleme kullanarak örneği veya SQL Always on kullanılabilirlik grubu yedekleme hatasına neden olabilir; Bunun yapılması kaçının.
- Aynı veya farklı çözümler ile tek tek bir kullanılabilirlik grubunda iki düğüm yedekleme, yedekleme başarısız olmasına neden olabilir. Azure Backup, algılamak ve kasa ile aynı bölgede bulunan tüm düğümleri koruyun. Birincil düğüm olan bölge yedekleme, SQL Server Always on kullanılabilirlik grubu birden çok Azure bölgesine yayılmış durumdaysa ayarlayın. Azure Backup, algılamak ve yedekleme tercihinize göre kullanılabilirlik grubundaki tüm veritabanlarını korumak.  
- Azure Backup'ın destekledikleri yalnızca tam ve yalnızca kopya tam yedekleme türleri için **salt okunur** veritabanları
- Veritabanları ile çok sayıda dosya korunamaz. Desteklenen en büyük sayı dosyalarının **~ 1000**.  
- En fazla yedekleyebilirsiniz **~ 2000** kasasındaki SQL Server veritabanları. Veritabanlarının daha büyük bir sayı varsa, birden çok kasa oluşturabilirsiniz.
- Yedekleme için en fazla yapılandırabilirsiniz **50** tek veritabanları gidin; bu kısıtlama, yedekleme yüklerini en iyi duruma yardımcı olur.
- Veritabanlarına varan destekliyoruz **2TB** performans sorunlarını boyutu; değerinden büyük boyutları için ortaya çıkabilir.
- Kaç tane veritabanları sunucu başına korunabilir dair bir fikir için bant genişliği, VM boyutu, yedekleme sıklığı, veritabanı boyutu, vb. gibi faktörleri dikkate alınması gereken ihtiyacımız var. Bu sayıyı siz kendi hesaplama yardımcı olacak bir planner üzerinde çalışıyoruz. Biz kısa bir süre içinde yayımlama.
- Kullanılabilirlik Grupları'olması durumunda yedekleme birkaç faktöre bağlı olarak farklı bir düğümler alınmıştır. Kullanılabilirlik grubu için yedekleme davranış aşağıda özetlenmiştir.

### <a name="backup-behavior-in-case-of-always-on-availability-groups"></a>Kullanılabilirlik grupları her zaman olması durumunda yedekleme davranışı

Yedekleme, yedekleme tercihi ve yedekleme türleri (tam/değişiklik/log/yalnızca kopya tam) bağlı olarak, belirli bir düğümden (birincil/ikincil) alınır.

- **Yedekleme tercihi: Primary**

**Yedekleme türü** | **Node**
    --- | ---
    Tam | Birincil
    Fark | Birincil
    Günlük |  Birincil
    Yalnızca kopya tam |  Birincil

- **Yedekleme tercihi: Yalnızca ikincil**

**Yedekleme türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca kopya tam |  İkincil

- **Yedekleme tercihi: İkincil**

**Yedekleme türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca kopya tam |  İkincil

- **Yedekleme tercih yok**

**Yedekleme türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca kopya tam |  İkincil

## <a name="fix-sql-sysadmin-permissions"></a>SQL sysadmin izinleri Düzelt

  İzinler nedeniyle hatayı düzeltmeniz gerekiyorsa bir **UserErrorSQLNoSysadminMembership** hata, aşağıdakileri yapın:

  1. Bir hesap ile SQL Server sysadmin izinleri SQL Server Management Studio (SSMS) oturum açmak için kullanın. Özel izinler gerekmedikçe, Windows kimlik doğrulama çalışması gerekir.
  2. SQL Server'da açın **güvenlik/oturum açma bilgileri** klasör.

      ![Hesapları görmek için güvenlik/oturum açma bilgileri klasörünü açın](./media/backup-azure-sql-database/security-login-list.png)

  3. Sağ **oturumları** klasörü ve select **yeni oturum açma**. İçinde **oturum açma - yeni**seçin **arama**.

      ![Oturum Aç - yeni iletişim kutusu, Ara'yı seçin](./media/backup-azure-sql-database/new-login-search.png)

  4. Windows sanal hizmet hesabı **NT SERVICE\AzureWLBackupPluginSvc** SQL bulma aşamasından ve sanal makine kaydı sırasında oluşturuldu. Gösterildiği gibi hesap adını girin **Seçilecek nesne adını girin**. Seçin **Adları Denetle** adı çözümlenemedi. **Tamam** düğmesine tıklayın.

      ![Bilinmeyen hizmet adını çözümlemek için adları denetle seçin](./media/backup-azure-sql-database/check-name.png)

  5. İçinde **sunucu rolleri**, emin **sysadmin** rolü seçilir. **Tamam** düğmesine tıklayın. Gerekli izinleri olması gerekir.

      ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Artık veritabanı kurtarma Hizmetleri kasası ile ilişkilendirin. Azure portalında, **korumalı sunucuların** listesinde, bir hata durumunda sunucusuna sağ tıklayın > **yeniden Bul DBs**.

      ![Sunucunun uygun izinlere sahip olun](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. İlerleme iade **bildirimleri** alan. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

      ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Hakkında bilgi edinin](backup-sql-server-database-azure-vms.md) SQL Server veritabanlarını yedekleme.
- [Hakkında bilgi edinin](restore-sql-database-azure-vm.md) yedeklenen SQL Server veritabanlarını geri yükleme.
- [Hakkında bilgi edinin](manage-monitor-sql-database-backup.md) yedeklenen SQL Server veritabanlarını yönetme.
