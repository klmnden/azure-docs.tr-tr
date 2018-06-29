---
title: SQL barındırma sunucuları Azure yığında | Microsoft Docs
description: SQL bağdaştırıcısı kaynak sağlayıcısı üzerinden sağlama SQL örnekleri ekleme.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeffgilb
ms.openlocfilehash: af820f90c5d8822dbdaa768b16360d534fd47828
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060051"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı için barındırma sunucuları ekleme

İçinde bir sanal makine (VM) SQL örneğinde barındırabilir [Azure yığın](azure-stack-poc.md), veya bir VM'de SQL kaynak sağlayıcısı örneğine bağlayabilirsiniz sürece, Azure yığın ortamınız dışında.

## <a name="overview"></a>Genel Bakış

Bir SQL server barındırma eklemeden önce aşağıdaki zorunlu ve genel gereksinimlerini gözden geçirin.

**Zorunlu gereksinimleri**

* SQL Server örneğinde SQL kimlik doğrulamasını etkinleştirin. SQL kaynak sağlayıcısı VM etki alanına katılmış olmadığından, yalnızca SQL kimlik doğrulaması'nı kullanarak bir barındırma sunucusuna bağlanabilir.
* SQL örnekleri için IP adreslerini genel olarak yapılandırın. Bu ağ üzerindeki bir SQL örneğine bağlanma gerekli olacak şekilde kaynak sağlayıcısı ve kullanıcılar, Web uygulamaları gibi kullanıcı ağ üzerinden iletişim kurar.

**Genel gereksinimler**

* SQL örneği kullanmak için kaynak sağlayıcısı ve kullanıcı iş yükleri tarafından atayın. Diğer bir tüketici tarafından kullanılan bir SQL örneğini kullanamazsınız. Bu kısıtlama, uygulama hizmetleri için de geçerlidir.
* Bir hesap ile uygun ayrıcalık düzeyi kaynak sağlayıcısı için yapılandırın.
* Olduğunuz SQL örnekleri ve konaklarının yönetmekten sorumludur.  Örneğin, kaynak sağlayıcısı olmayan güncelleştirmeleri uygulamak, tanıtıcı yedekleri veya işlemek döndürme kimlik bilgisi.

### <a name="sql-server-virtual-machine-images"></a>SQL Server sanal makine görüntüleri

SQL Iaas sanal makine görüntülerini Market yönetim özelliği kullanılarak kullanılabilir. Bu görüntüleri mevcut olan SQL VM'ler ile aynıdır.

Her zaman en son sürümünü indirme emin **SQL Iaas uzantısı** Market öğesi kullanarak bir SQL VM dağıtmadan önce. Iaas uzantısını ve karşılık gelen portal geliştirmeleri otomatik düzeltme eki uygulama gibi ek özellikler sağlar ve yedekleyin. Bu uzantı hakkında daha fazla bilgi için bkz: [SQL Server Aracısı uzantısına sahip Azure sanal makineler üzerinde yönetim görevlerini otomatikleştiren](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Şablonlarda dahil olmak üzere SQL VM dağıtma için diğer seçenekleri vardır [Azure yığın hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Bir çok düğümlü Azure yığında yüklü herhangi bir barındırma sunucuları bir kullanıcı abonelik oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kullanıcı Portalı'ndan veya uygun bir oturum açma ile bir PowerShell oturumundan oluşturulmalıdır. Tüm barındırma sunucuları Faturalanabilir VM'ler ve uygun SQL lisanslara sahip olması gerekir. Hizmet Yöneticisi _için_ bu aboneliğin sahibi olmalıdır.

### <a name="required-privileges"></a>Gerekli ayrıcalıklar

Bir SQL sysadmin'den daha düşük ayrıcalıklara sahip bir yönetici kullanıcı oluşturun. Kullanıcı yalnızca aşağıdaki işlemleri için izinler gerekir:

* Veritabanı: Oluşturma, Alter, kapsama (için her zaman açık yalnızca), bırakma, yedekleme
* Kullanılabilirlik Grubu: Alter, birleştirme, Ekle/Kaldır veritabanı
* Oturum açma: Oluşturma, seçin, Alter, Drop, iptal etme
* Select işlemleri: \[ana\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) birincilindeki (AlwaysOn), sys.databases, \[ana\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[ana\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>SQL server'ı barındıran bir tek başına bağlanarak kapasitesi sağlar

Tek başına kullanabilirsiniz (olmayan-HA) herhangi bir SQL Server 2014 veya SQL Server 2016 sürümünü kullanarak SQL sunucuları. Bir hesabın kimlik bilgilerini sysadmin ayrıcalıklarına sahip olduğundan emin olun.

Zaten ayarlanmış bir tek başına barındırma sunucusu eklemek için aşağıdaki adımları izleyin:

1. Azure yığın işleci portalına Hizmet Yöneticisi olarak oturum açın.

2. Seçin **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları**.

   ![SQL barındırma sunucuları](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   Altında **SQL barındırma sunucuları**, SQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet örneklerini SQL kaynak sağlayıcısı bağlanabilir.

   ![SQL bağdaştırıcısı Panosu](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Üzerinde **barındıran bir SQL Server ekleme**, SQL Server örneği için bağlantı ayrıntıları sağlayın.

   ![Bir SQL Server barındırma ekleme](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    İsteğe bağlı olarak, bir örnek adı sağlayın ve örneği için varsayılan bağlantı noktası 1433'ü atanmamış bir bağlantı noktası numarası belirtin.

   > [!NOTE]
   > SQL örneğinde yönetici Azure Resource Manager ve kullanıcı tarafından erişilen sürece, kaynak sağlayıcısı denetiminde yerleştirilebilir. SQL örneği __gerekir__ özel olarak kaynak sağlayıcısı ayrılamadı.

4. Sunucular eklediğinizde, varolan bir SKU'ya atayın veya yeni bir SKU oluşturun. Altında **barındıran bir sunucu eklemek**seçin **SKU'ları**.

   * Varolan bir SKU kullanmak için kullanılabilir bir SKU'ı seçin ve ardından **oluşturma**.
   * Bir SKU oluşturmak için seçin **+ yeni SKU oluşturma**. İçinde **oluşturma SKU**gerekli bilgileri girin ve ardından **Tamam**.

     > [!IMPORTANT]
     > Özel karakterler, boşluklar ve dönemleri dahil olmak üzere desteklenmeyen **adı** alan. Aşağıdaki ekran görüntüsünde değerlerini girmek için örneklerde **ailesi**, **katmanı**, ve **Edition** alanları.

     ![Bir SKU oluşturma](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

      SKU'ları portalında görünür olması için bir saat sürebilir. SKU tam olarak oluşturulana kadar kullanıcılar bir veritabanı oluşturulamıyor.

### <a name="sku-notes"></a>SKU notları

Hizmet teklifleri ayırt etmek için SKU'ları kullanabilirsiniz. Örneğin, aşağıdaki özelliklere sahip bir SQL Enterprise örneğine sahip olabilir:
  
* yüksek kapasiteli
* yüksek performans
* Yüksek kullanılabilirlik

Yüksek performanslı veritabanı gereken belirli gruplara erişimi sınırlayan bir SKU önceki örnek için oluşturabilirsiniz.

>[!TIP]
>Yansıtan bir SKU adı kullan kapasite ve performans gibi SKU sunucuların özelliklerini açıklar. Kullanıcıların kendi veritabanlarını uygun SKU dağıtmak yardımcı olmak için bir yardımcı programının adı görür.

En iyi uygulama, bir SKU tüm barındırma sunucuları aynı kaynak ve performans özelliklerini olması gerekir.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>SQL Always On kullanılabilirlik grupları kullanarak yüksek kullanılabilirlik sağlar

SQL Always On örnekleri yapılandırma ek adımlar gerektirir ve üç sanal makineleri (veya fiziksel makineler) gerektirir. Bu makalede, Always On kullanılabilirlik grupları düz bir anlayış zaten sahip olduğunuzu varsayar. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerde Tanıtımı](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Her zaman kullanılabilirlik grupları (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> SQL bağdaştırıcısı kaynak sağlayıcısı _yalnızca_ veya daha sonra örnekler için her zaman açık SQL 2016 SP1 Enterprise destekler. Bu bağdaştırıcı yapılandırma otomatik dengeli dağıtımı gibi yeni SQL özellikleri gerektirir.

Ayrıca, etkinleştirmelisiniz [otomatik dengeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) her kullanılabilirlik grubu SQL Server'ın her örneğinin üzerinde.

Tüm örneklerinde otomatik dengeli dağıtımı etkinleştirmek için düzenleyin ve ardından her örneği için aşağıdaki SQL komutunu çalıştırın:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

İkincil örneklerinde düzenleyin ve ardından her örneği için aşağıdaki SQL komutunu çalıştırın:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>SQL her zaman barındırma sunucuları eklemek için

1. Azure yığın yönetim portalında hizmet yönetici olarak oturum açın

2. Seçin **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **+ekleme**.

   Altında **SQL barındırma sunucuları**, SQL Server Kaynak sağlayıcısı SQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örneklerini bağlanabilir.

3. SQL Server örneği için bağlantı ayrıntılarla formu doldurun. Üzerinde her zaman dinleyicisi (ve isteğe bağlı bağlantı noktası numarası.) FQDN adresi kullandığınızdan emin olun Sysadmin ayrıcalıklarına sahip yapılandırılmış hesap için bilgi sağlar.

4. SQL her zaman üzerindeki kullanılabilirlik grubu örnekleri desteğini etkinleştirmek için her zaman üzerindeki kullanılabilirlik grubu onay kutusunu işaretleyin.

   ![Her zaman etkinleştirin](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. SQL Always On örnek bir SKU ekler.

   > [!IMPORTANT]
   > Aynı SKU durumlarda her zaman açık olan tek başına sunucular karıştıramazsınız. İlk barındırma sunucusu sonuçlar hata ekledikten sonra türlerini karma olarak çalışıyor.

## <a name="make-the-sql-databases-available-to-users"></a>SQL veritabanları kullanıcılar tarafından kullanılabilmesini sağlamak

Planları ve SQL veritabanları kullanıcılar için kullanılabilir hale getirmek için teklifleri oluşturun. Ekleme **Microsoft.SqlAdapter** hizmet planına ve varsayılan kota ekleyin veya yeni bir kota oluşturun.

![Planları ve veritabanlarını içerecek şekilde teklifleri oluştur](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="next-steps"></a>Sonraki adımlar

[Veritabanı ekleyin](azure-stack-sql-resource-provider-databases.md)
