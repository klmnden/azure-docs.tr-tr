---
title: "SQL barındırma sunucuları Azure yığında | Microsoft Docs"
description: "SQL bağdaştırıcısı kaynak Sağlayıcısı sağlama SQL örnekleri ekleme"
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 0a29ef133a045b2828777050f2d7a204c0add4a8
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="add-hosting-servers-for-use-by-the-sql-adapter"></a>SQL bağdaştırıcısı tarafından kullanım için barındırma sunucuları ekleme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

İçinde sanal makineleri üzerinde SQL örnekleri kullanabilirsiniz, [Azure yığın](azure-stack-poc.md), veya kaynak sağlayıcısı sağlanan Azure yığın ortamınızı dışında bir örneği için bağlanabilirsiniz. Genel gereksinimler vardır:

* SQL örneği RP ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir SQL örneğini kullanamazsınız.
* RP bağdaştırıcısı etki alanına katılmamışsa ve yalnızca SQL kimlik doğrulaması kullanarak bağlanabilir.
* RP tarafından kullanım için uygun ayrıcalıklara sahip bir hesap yapılandırmanız gerekir.
* Bu ağ üzerindeki bir SQL örneğine bağlanma gerekli olacak şekilde RP ve kullanıcıların Web uygulamaları gibi kullanıcı ağ kullanın. Bu gereksinim genellikle IP SQL örnekleri için ortak bir ağda olması gerektiği anlamına gelir.
* SQL örnekleri ve konaklarının Yönetimi kadar; RP düzeltme eki uygulama, Yedekleme gerçekleştirmek yok, döndürme, vb. kimlik bilgisi.
* SKU'ları, her zaman açık, vb. gibi performans, SQL yeteneklerini farklı sınıflar oluşturmak için kullanılabilir.


Bir dizi SQL Iaas sanal makine görüntülerini Market yönetim özelliği kullanılarak kullanılabilir. Market öğesi kullanarak bir VM'i dağıtmadan önce her zaman SQL Iaas uzantısı'nın en son sürümü karşıdan emin olun. SQL görüntüleri mevcut olan SQL VM'ler ile aynıdır. SQL Iaas uzantısı bu görüntüleri kullanılarak oluşturulan ve portal geliştirmeleri karşılık gelen VM'ler için otomatik düzeltme eki uygulama ve yedekleme özellikleri gibi özellikler sağlar.

Şablonlarda dahil olmak üzere SQL VM dağıtma için diğer seçenekleri vardır [Azure yığın hızlı başlama Galerisi](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Bir çok düğümlü Azure yığında yüklü herhangi bir barındırma sunucuları bir kullanıcı abonelik oluşturulması gerekir. Varsayılan sağlayıcı abonelikten oluşturulamıyor. Bunlar, Kullanıcı Portalı'ndan veya uygun bir oturum açma ile bir PowerShell oturumundan oluşturulmalıdır. Tüm barındırma sunucuları ücrete tabi VM'ler ve uygun SQL lisanslara sahip olması gerekir. Hizmet Yöneticisi _için_ bu aboneliğin sahibi olmalıdır.


### <a name="required-privileges"></a>Gerekli ayrıcalıklar

Yeni yönetici kullanıcı değerinden tam sysadmin ayrıcalıklarına sahip oluşturabilirsiniz. İzin verilmesi için gereken belirli işlemler şunlardır:

- Veritabanı: Oluşturma, kapsama (her zaman açık yalnızca) ile açılan, yedekleme değiştirme,
- Kullanılabilirlik Grubu: Alter, birleştirme, Ekle/Kaldır veritabanı
- Oturum açma: Oluşturma, seçin, Alter, Drop, iptal etme
- Select işlemleri: \[ana\].\[ sys\].\[ availability_group_listeners\] (AlwaysOn) birincilindeki (AlwaysOn), sys.databases, \[ana\].\[ sys\].\[ dm_os_sys_memory\], SERVERPROPERTY, \[ana\].\[ sys\].\[ availability_groups\] (AlwaysOn) sys.master_files



## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>SQL server'ı barındıran bir tek başına bağlanarak kapasitesi sağlar
Tek başına kullanabilirsiniz (olmayan-HA) herhangi bir SQL Server 2014 veya SQL Server 2016 sürümünü kullanarak SQL sunucuları. Sistem Yöneticisi ayrıcalıklarına sahip bir hesabın kimlik bilgilerini sağlayın.

Barındırma zaten sağlandı sunucusu tek başına eklemek için aşağıdaki adımları izleyin:

1. Azure yığın yönetim portalında Hizmet Yöneticisi olarak oturum açın

2. Tıklatın **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları**.

  ![](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

  **SQL barındırma sunucuları** dikey olan SQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örnekleri için SQL Server Kaynak sağlayıcısı burada bağlanabilir.

  ![Barındırma sunucuları](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Form, SQL Server örneği ile bağlantı ayrıntıları doldurun.

  ![Yeni barındırma sunucusu](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    İsteğe bağlı olarak bir örnek adı içerebilir ve bir bağlantı noktası numarası örneği için varsayılan bağlantı noktası 1433'ü atanmamışsa sağlanabilir.

  > [!NOTE]
  > SQL örneğinde yönetici Azure Resource Manager ve kullanıcı tarafından erişilen sürece, kaynak sağlayıcısı denetiminde yerleştirilebilir. SQL örneği __gerekir__ özel olarak RP ayrılamadı.

4. Sunucular eklediğinizde, hizmet teklifleri ayırt etmek için bir yeni veya var olan SKU'ya atamanız gerekir. Örneğin, SQL Enterprise sağlayan örnek sahip olabilir:
  - Veritabanı kapasite
  - otomatik yedekleme
  - tek tek departmanlar için yüksek performanslı sunucuları
  - ve benzeri.

  Böylece kullanıcılar kendi veritabanlarını uygun şekilde yerleştirebilirsiniz SKU adı özelliklerini yansıtmalıdır. Bir SKU tüm barındırma sunucuları aynı olanaklara sahip olmalıdır.

    Örnek:

![SKU'lar](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU tam olarak oluşturulana kadar kullanıcılar bir veritabanı oluşturulamıyor.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>SQL Always On kullanılabilirlik grupları kullanarak kapasitesi sağlar
SQL Always On örnekleri yapılandırma ek adımlar gerektirir ve en az üç sanal makineleri (veya fiziksel makineler) içerir.

> [!NOTE]
> SQL bağdaştırıcısı RP _yalnızca_ otomatik dengeli dağıtımı gibi yeni SQL özellikleri gerektirdiği şekilde her zaman açık için SQL 2016 SP1 Enterprise veya sonraki örnekleri destekler. Önceki ortak gereksinimlerinin listesi ek olarak:

* Bir dosya sunucusu SQL Always On bilgisayarların yanı sıra sağlamanız gerekir. Var. bir [Azure yığın hızlı başlatma şablonunu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-ha) , oluşturabilirsiniz bu ortam için. Ayrıca, kendi örneği oluşturmak için bir kılavuz olarak görebilir.

* SQL sunucularını ayarlamanız gerekir. Özellikle, etkinleştirmeniz gerekir [otomatik dengeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) her kullanılabilirlik grubu SQL Server'ın her örneğinin üzerinde.

```
ALTER AVAILABILITY GROUP [<availability_group_name>]
    MODIFY REPLICA ON 'InstanceName'
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

İkincil örnekleri
```
ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
GO

```



SQL Always On barındırma sunucuları eklemek için aşağıdaki adımları izleyin:

1. Azure yığın Yönetici portalı'ndaki bir Hizmet Yöneticisi olarak oturum açın

2. Tıklatın **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **+ekleme**.

    **SQL barındırma sunucuları** dikey olan SQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örnekleri için SQL Server Kaynak sağlayıcısı burada bağlanabilir.


3. Form üzerinde her zaman dinleyicisi (ve isteğe bağlı bağlantı noktası numarası) FQDN veya IPv4 adresi kullandığınızdan emin olmak için SQL Server örneği ile bağlantı ayrıntıları doldurun. Sistem yönetici ayrıcalıklarıyla yapılandırılmış hesap için hesap bilgilerini sağlayın.

4. SQL her zaman üzerindeki kullanılabilirlik grubu örnekleri desteğini etkinleştirmek için bu onay kutusunu işaretleyin.

    ![Barındırma sunucuları](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. SQL Always On örnek bir SKU ekler. Aynı SKU durumlarda her zaman açık olan tek başına sunucular karıştıramazsınız. Bu ilk barındırma sunucusu eklerken belirlenir. Daha sonra türlerini karma olarak çalışırken bir hatayla sonuçlanır.


## <a name="making-sql-databases-available-to-users"></a>SQL veritabanları kullanıcılar için kullanılabilir hale getirme

Planları ve SQL veritabanları kullanıcılar için kullanılabilir hale getirmek için teklifleri oluşturun. Microsoft.SqlAdapter hizmet planına ekleme ve var olan bir kota ekleyin veya yeni bir tane oluşturun. Bir kota oluşturursanız, izin vermek için kapasitesini belirtin.

![Planları ve veritabanlarını içerecek şekilde teklifleri oluştur](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="maintenance-of-the-sql-adapter-rp"></a>SQL bağdaştırıcısının RP bakım

Bakım SQL örneği burada parola döndürme bilgileri dışında kapsamında değildir. Düzeltme eki uygulama ve yedekleme/kurtarma SQL bağdaştırıcısı ile kullanılan veritabanı sunucularının sorumlu.

### <a name="patching-and-updating"></a>Düzeltme eki uygulama ve güncelleştirme
 Bir eklenti bileşeni olduğu gibi SQL bağdaştırıcısı Azure yığın bir parçası olarak bakım değil. Microsoft Güncelleştirmeleri gerektiği gibi SQL bağdaştırıcıya sağlıyor olabilir. Üzerinde SQL bağdaştırıcısı örneği bir _kullanıcı_ varsayılan sağlayıcı abonelik altında sanal makine. Bu nedenle, Windows düzeltme ekleri, virüsten koruma imzaları vb. sağlamak gereklidir. Windows güncelleştirme, düzeltme ve güncelleştirme döngüsünün parçası Windows VM güncelleştirmeleri uygulamak için kullanılabilir olarak sağlanan paketleri. Güncelleştirilmiş bir bağdaştırıcı yayımlandığında, bir komut dosyası güncelleştirmeyi uygulamak için sağlanır. Bu komut dosyasını yeni bir RP VM oluşturur ve zaten herhangi bir durum geçirme.

 ### <a name="backuprestoredisaster-recovery"></a>Yedekleme/geri yükleme/olağanüstü durum kurtarma
 Bir eklenti bileşeni olduğu gibi SQL bağdaştırıcısı Azure yığın BC-DR işleminin bir parçası yedeklenmez. Komut dosyaları kolaylaştırmak için sağlanır:
- (Bir Azure yığın depolama hesabında depolanır) gerekli durum bilgisinin yedekleme
- Tam yığını kurtarma gerekli hale gelmesi durumunda RP geri yükleniyor.
Veritabanı sunucuları kurtarılması gereken ilk (gerekiyorsa), RP geri yüklenmeden önce.

### <a name="updating-sql-credentials"></a>SQL kimlik bilgileri güncelleştiriliyor

Oluşturma ve SQL Server sistem yönetici hesaplarında korumaya yönelik sorumluluğu size aittir. RP veritabanlarını kullanıcılar adına yönetmek için bu ayrıcalıklarına sahip bir hesap gerekir - bu veritabanları veri erişimi gerekmez. SQL Server sa parolalarını güncellemeniz gerekiyorsa, RP tarafından kullanılan depolanan parolayı değiştirmek için RP'ın yönetici arabirimi güncelleştirme özelliğini kullanabilirsiniz. Bu parolalar bir anahtar kasası, Azure yığın örneğinde depolanır.

Ayarları değiştirmek için tıklatın **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **SQL oturum açma bilgileri** ve bir oturum açma adı seçin. Değişiklik SQL örneğinde önce yapılmalıdır (ve gerekirse, tüm yinelemeleri). İçinde **ayarları** paneli, tıklayın **parola**.

![Yönetici parolasını güncelleştirin](./media/azure-stack-sql-rp-deploy/sqlrp-update-password.PNG)


## <a name="next-steps"></a>Sonraki adımlar

[Veritabanı ekleyin](azure-stack-sql-resource-provider-databases.md)
