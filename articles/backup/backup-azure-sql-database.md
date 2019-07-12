---
title: SQL Server veritabanlarını azure'a yedekleyin | Microsoft Docs
description: Bu öğreticide, SQL Server'ı Azure'a yedekleme açıklanmaktadır. Makalede, SQL Server kurtarma de açıklanmaktadır.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: raynew
ms.openlocfilehash: 8e7e5d871fa1bb557de4e6fce22658115bf0fe94
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806987"
---
# <a name="about-sql-server-backup-in-azure-vms"></a>Azure VM'lerindeki SQL Server Backup hakkında

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleri değildir. Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedekleyebilirsiniz [Azure Backup](backup-overview.md).

## <a name="backup-process"></a>Yedekleme işlemi

Bu çözüm, SQL veritabanlarının yedeklemelerini almak için SQL yerel API'lerden yararlanır.

* Veritabanlarında korumak ve sorgulamak istediğiniz SQL Server VM belirttiğinizde, Azure Backup hizmeti bir iş yükü backup uzantısı VM'de adıyla yükleyecek `AzureBackupWindowsWorkload`  uzantısı.
* Bu uzantı, düzenleyici ve SQL eklentisi oluşur. Düzenleyici yedekleme, yedekleme ve geri yükleme yapılandırma gibi çeşitli işlemler için iş akışlarını tetiklemekten sorumlu olsa da, eklenti için gerçek veri akışı sorumludur.
* Bu VM'de veritabanlarını bulmak için Azure Backup, hesabı oluşturan `NT SERVICE\AzureWLBackupPluginSvc`. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir. Azure Backup yararlanır `NT AUTHORITY\SYSTEM` SQL ortak bir oturum açma olacak şekilde bu hesabınızın olması gerekir böylece veritabanı bulma/sorgulama için hesap. SQL Server VM Azure Market'te oluşturmadıysanız, bir hata alabilirsiniz **UserErrorSQLNoSysadminMembership**. Bu meydana gelirse [bu yönergeleri izleyin](backup-azure-sql-database.md).
* Seçili veritabanlarında koruma, tetikleyici yapılandırdıktan sonra backup hizmeti yedekleme zamanlamaları ve uzantı, sanal makinede yerel olarak önbelleğe alır diğer ilke ayrıntıları Düzenleyici ayarlar 
* Zamanlanan tarihte Düzenleyici eklenti ile iletişim kurar ve yedekleme verileri SQL Server'dan VDI kullanarak akış başlatır.  
* Eklenti, bu nedenle bir hazırlama konumu ihtiyacını doğrudan kurtarma Hizmetleri kasasına, verileri gönderir. Veriler şifrelenir ve depolama hesaplarında Azure Backup hizmeti tarafından depolanır.
* Veri aktarımı tamamlandığında, yedekleme hizmetiyle işleme Düzenleyicisi onaylar.

  ![SQL yedekleme mimarisi](./media/backup-azure-sql-database/backup-sql-overview.png)

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce doğrulama aşağıda:

1. Azure'da çalışan bir SQL Server örneğine sahip olduğunuzdan emin olun. Yapabilecekleriniz [kolayca bir SQL Server örneği oluşturma](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md) Market'te.
2. Gözden geçirme [göz önünde bulundurarak özellik](#feature-consideration-and-limitations) ve [senaryo Destek](#scenario-support).
3. [Sık sorulan soruları gözden](faq-backup-sql-server.md) bu senaryo hakkında.

## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen dağıtımlar** | SQL Marketplace Azure Vm'leri ve Market dışı (SQL Server el ile yüklenen) sanal makineleri desteklenir.
**Desteklenen coğrafi bölgeler** | Avustralya Güneydoğu (ASE), Doğu Avustralya (AE) <br> Brezilya Güney (BRS)<br> Kanada Orta (CNC), Kanada Doğu (CE)<br> Güney Doğu Asya (SEA), Doğu Asya (EA) <br> East US (EUS), East US 2 (EUS2), West Central US (WCUS), West US (WUS); West US 2 (WUS 2) North Central US (NCUS) Central US (CUS) South Central US (SCUS) <br> Hindistan Orta (Inc), Hindistan Güney (INS) <br> Japonya Doğu (JPE), Japonya Batı (JPW) <br> Kore Orta (KRC), Kore Güney (KRS) <br> Kuzey Avrupa (NE), Batı Avrupa <br> UK Güney (UKS) BK Batı (UKW)
**Desteklenen işletim sistemleri** | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012<br/><br/> Linux şu anda desteklenmemektedir.
**Desteklenen SQL Server sürümleri** | SQL Server 2017 ayrıntılı olarak [burada](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017), SQL Server 2016 ve ayrıntılı olarak Sp'ler [burada](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack), SQL Server 2014, SQL Server 2012.<br/><br/> Enterprise, Standard, Web, Developer, Express.
**Desteklenen .NET sürümleri** | .NET framework 4.5.2 ve üzeri VM'de yüklü

### <a name="support-for-sql-server-2008-and-sql-server-2008-r2"></a>SQL Server 2008 ve SQL Server 2008 R2 desteği

Azure yedekleme desteği kısa süre önce duyurulan [EOS SQL Server'lar](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support) -SQL Server 2008 ve SQL Server 2008 R2. Çözüm, şu an EOS SQL Server için Önizleme aşamasındadır ve aşağıdaki yapılandırmayı destekler:

1. SQL Server 2008 ve Windows 2008 R2 SP1'de çalışan SQL Server 2008 R2
2. .NET framework 4.5.2 ve üzeri VM'de yüklü gerekiyor
3. Durum, FCI ve yansıtılmış veritabanları için yedekleme desteklenmiyor

Kullanıcılar, bu özellik genel olarak kullanılabilir olduğu saat kasa için ücretlendirilmez. Diğer tüm [özellik önemli noktalar ve sınırlamalar](#feature-consideration-and-limitations) bu sürümler için geçerlidir. Genişliğinizin başvurmak [önkoşulları](backup-sql-server-database-azure-vms.md#prerequisites) SQL Server 2008 ve 2008 R2 ayarını dahil korumayı yapılandırmadan önce [kayıt defteri anahtarı](backup-sql-server-database-azure-vms.md#add-registry-key-to-enable-registration) (genellikle özelliğidir, bu adım gerekli olacaktır kullanılabilir).


## <a name="feature-consideration-and-limitations"></a>Özellik önemli noktalar ve sınırlamalar

- SQL Server Yedekleme, Azure portalında yapılandırılabilir veya **PowerShell**. CLI desteklemiyoruz.
- Çözüm üzerinde her iki türü desteklenir, [dağıtımları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) -Azure Resource Manager Vm'lerinde ve klasik VM'ler.
- SQL Server çalıştıran bir VM, Azure genel IP adresleri erişmek için Internet bağlantısı gerektirir.
- SQL Server **yük devretme kümesi örneği (FCI)** ve SQL Server her zaman yük devretme kümesi örneği üzerinde desteklenmez.
- Yedekleme ve geri yükleme işlemleri için yansıtma veritabanları ve veritabanı anlık görüntüleri desteklenmez.
- Tek başına SQL Server'ı yedeklemek için yedekleme çözümleri kullanarak birden fazla örneğini veya SQL Always on kullanılabilirlik grubu yedekleme hatasına neden olabilir; Bunun yapılması kaçının.
- Aynı veya farklı çözümler ile tek tek bir kullanılabilirlik grubunda iki düğüm yedekleme, yedekleme başarısız olmasına neden olabilir.
- Azure Backup'ın destekledikleri yalnızca tam ve yalnızca kopya tam yedekleme türleri için **salt okunur** veritabanları
- Veritabanları ile çok sayıda dosya korunamaz. Desteklenen en büyük sayı dosyalarının **~ 1000**.  
- En fazla yedekleyebilirsiniz **~ 2000** kasasındaki SQL Server veritabanları. Veritabanlarının daha büyük bir sayı varsa, birden çok kasa oluşturabilirsiniz.
- Yedekleme için en fazla yapılandırabilirsiniz **50** tek veritabanları gidin; bu kısıtlama, yedekleme yüklerini en iyi duruma yardımcı olur.
- Veritabanlarına varan destekliyoruz **2TB** performans sorunlarını boyutu; değerinden büyük boyutları için ortaya çıkabilir.
- Kaç tane veritabanları sunucu başına korunabilir dair bir fikir için bant genişliği, VM boyutu, yedekleme sıklığı, veritabanı boyutu, vb. gibi faktörleri dikkate alınması gereken ihtiyacımız var. Bu sayı, üzerinde kendi hesaplama yardımcı bir planner üzerinde çalışıyoruz. Biz kısa bir süre içinde yayımlama.
- Kullanılabilirlik Grupları'olması durumunda yedekleme birkaç faktöre bağlı olarak farklı bir düğümler alınmıştır. Kullanılabilirlik grubu için yedekleme davranış aşağıda özetlenmiştir.

### <a name="back-up-behavior-in-case-of-always-on-availability-groups"></a>Kullanılabilirlik grupları her zaman durumunda davranış yedekleyin

Yedekleme yalnızca tek bir ağ düğümünde yapılandırıldığını önerilir. Yedekleme, her zaman birincil düğüm ile aynı bölgede yapılandırılmalıdır. Diğer bir deyişle, her zaman birincil düğüm yedekleme yapılandırdığınız bölgede mevcut olması gerekir. AG tüm düğümlerin aynı bölgedeki yedekleme yapılandırıldığı, herhangi bir sorun yoktur.

**Bölgeler arası adlı AG için**
- Yedekleme tercihi ne olursa olsun yedeklemeleri yedeklemenin yapılandırıldığı aynı bölgede değil düğümlerden gerçekleşmez. Bölgeler arası yedeklemeler desteklenmez olmasıdır. Yalnızca 2 düğüm varsa ve ikincil düğümünde başka bir bölgede Bu durumda, yedeklemeler birincil düğümden olmasını devam eder (Yedekleme tercihinizi olmadığı sürece 'yalnızca ikincil').
- Yedeklemenin yapılandırıldığı hesaptan farklı bir bölgeye yük devretme durumda devredilen bölgede düğümlerinde yedeklemeler başarısız olur.

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

## <a name="set-vm-permissions"></a>VM izinleri ayarlama

  Azure Backup, SQL Server'da bulmayı çalıştırdığınızda, şunları yapar:

* AzureBackupWindowsWorkload uzantısı ekler.
* Sanal makine üzerindeki veritabanlarını bulmak için bir NT SERVICE\AzureWLBackupPluginSvc hesabı oluşturur. Bu hesap, yedekleme için kullanılır ve geri yükleme ve SQL sysadmin izinleri gerektirir.
* Bir VM üzerinde çalışan veritabanları bulur Azure Backup, NT AUTHORITY\SYSTEM hesabı kullanır. Bu hesabın bir genel oturum açma SQL olması gerekir.

SQL Server VM Azure Market'te oluşturmamışsınızdır veya SQL 2008 ve 2008 R2'de ise alabileceğiniz bir **UserErrorSQLNoSysadminMembership** hata.

Durumunda, izinleri vermek için **SQL 2008** ve **2008 R2** Windows 2008 R2'de çalışan, [burada](#give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2).

Diğer tüm sürümler için aşağıdaki adımlarla izinleri düzeltin:

  1. Bir hesap ile SQL Server sysadmin izinleri SQL Server Management Studio (SSMS) oturum açmak için kullanın. Özel izinler gerekmedikçe, Windows kimlik doğrulama çalışması gerekir.
  2. SQL Server'da açın **güvenlik/oturum açma bilgileri** klasör.

      ![Hesapları görmek için güvenlik/oturum açma bilgileri klasörünü açın](./media/backup-azure-sql-database/security-login-list.png)

  3. Sağ **oturumları** klasörü ve select **yeni oturum açma**. İçinde **oturum açma - yeni**seçin **arama**.

      ![Oturum Aç - yeni iletişim kutusu, Ara'yı seçin](./media/backup-azure-sql-database/new-login-search.png)

  4. Windows sanal hizmet hesabı **NT SERVICE\AzureWLBackupPluginSvc** SQL bulma aşamasından ve sanal makine kaydı sırasında oluşturuldu. Gösterildiği gibi hesap adını girin **Seçilecek nesne adını girin**. Seçin **Adları Denetle** adı çözümlenemedi. **Tamam**'ı tıklatın.

      ![Bilinmeyen hizmet adını çözümlemek için adları denetle seçin](./media/backup-azure-sql-database/check-name.png)

  5. İçinde **sunucu rolleri**, emin **sysadmin** rolü seçilir. **Tamam**'ı tıklatın. Gerekli izinleri olması gerekir.

      ![Sysadmin sunucu rolünün seçili olduğundan emin olun](./media/backup-azure-sql-database/sysadmin-server-role.png)

  6. Artık veritabanı kurtarma Hizmetleri kasası ile ilişkilendirin. Azure portalında, **korumalı sunucuların** listesinde, bir hata durumunda sunucusuna sağ tıklayın > **yeniden Bul DBs**.

      ![Sunucunun uygun izinlere sahip olun](./media/backup-azure-sql-database/check-erroneous-server.png)

  7. İlerleme iade **bildirimleri** alan. Seçili veritabanlarındaki bulunduğunda bir başarı iletisi görünür.

      ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

> [!NOTE]
> SQL Server SQL Server'ın yüklü birden çok örneğe sahip sonra sysadmin iznine eklemelisiniz **NT Service\AzureWLBackupPluginSvc** hesap için tüm SQL örnekleri.

### <a name="give-sql-sysadmin-permissions-for-sql-2008-and-sql-2008-r2"></a>SQL 2008 ve SQL 2008 R2 için SQL sysadmin izinleri verme

Ekleme **NT AUTHORITY\SYSTEM** ve **NT Service\AzureWLBackupPluginSvc** SQL Server örneği oturum açma:

1. Nesne Gezgini'nde SQL Server örneği gidin.
2. Güvenlik için oturum açma bilgileri ->
3. Oturum açma bilgilerini sağ tıklayarak *yeni oturum açma...*

    ![SSMS kullanarak yeni bir oturum açma](media/backup-azure-sql-database/sql-2k8-new-login-ssms.png)

4. Genel sekmesine gidip girmek **NT AUTHORITY\SYSTEM** oturum açma adı.

    ![SSMS için oturum açma adı](media/backup-azure-sql-database/sql-2k8-nt-authority-ssms.png)

5. Git *sunucu rolleri* ve *genel* ve *sysadmin* rolleri.

    ![SSMS'de rolleri seçme](media/backup-azure-sql-database/sql-2k8-server-roles-ssms.png)

6. Git *durumu*. *GRANT* veritabanı motoru ve oturum açma olarak bağlanmak için izni *etkin*.

    ![Ssms'de izin ver](media/backup-azure-sql-database/sql-2k8-grant-permission-ssms.png)

7. Tamam'a tıklayın.
8. Aynı adımlar (yukarıda, 1-7) dizisini NT Service\AzureWLBackupPluginSvc oturum açma SQL Server örneğine eklemek için yineleyin. Oturum açma zaten varsa, sysadmin sunucu rolüne sahip ve isteğe bağlı olarak durumu altında Veritabanı Altyapısı'na bağlanma izni ve etkin olarak oturum açma izni olduğundan emin olun.
9. İzin verme sonra **yeniden bulma DBs** Portalı'nda: Kasa **->** Yedekleme Altyapısı **->** iş yükü Azure sanal makinede:

    ![Azure portalında vt'leri](media/backup-azure-sql-database/sql-rediscover-dbs.png)

Alternatif olarak, Yönetici modunda aşağıdaki PowerShell komutlarını çalıştırarak izinleri vermeyi otomatik hale getirebilirsiniz. Örnek adı için MSSQLSERVER, varsayılan olarak ayarlanır. Örnek adı komut bağımsız değişkeni, bir değişiklik olması gerekir:

```powershell
param(
    [Parameter(Mandatory=$false)]
    [string] $InstanceName = "MSSQLSERVER"
)
if ($InstanceName -eq "MSSQLSERVER")
{
    $fullInstance = $env:COMPUTERNAME   # In case it is the default SQL Server Instance
}
else
{
    $fullInstance = $env:COMPUTERNAME + "\" + $InstanceName   # In case of named instance
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT Service\AzureWLBackupPluginSvc', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
try
{
    sqlcmd.exe -S $fullInstance -Q "sp_addsrvrolemember 'NT AUTHORITY\SYSTEM', 'sysadmin'" # Adds login with sysadmin permission if already not available
}
catch
{
    Write-Host "An error occurred:"
    Write-Host $_.Exception|format-list -force
}
```


## <a name="next-steps"></a>Sonraki adımlar

* [Hakkında bilgi edinin](backup-sql-server-database-azure-vms.md) SQL Server veritabanlarını yedekleme.
* [Hakkında bilgi edinin](restore-sql-database-azure-vm.md) yedeklenen SQL Server veritabanlarını geri yükleme.
* [Hakkında bilgi edinin](manage-monitor-sql-database-backup.md) yedeklenen SQL Server veritabanlarını yönetme.
