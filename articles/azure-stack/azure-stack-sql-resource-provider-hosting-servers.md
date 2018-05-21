---
title: SQL barındırma sunucuları Azure yığında | Microsoft Docs
description: SQL bağdaştırıcısı kaynak Sağlayıcısı sağlama SQL örnekleri ekleme
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
ms.date: 05/18/2018
ms.author: jeffgilb
ms.openlocfilehash: e08c0bfd3cbed64f5042e469801e20c913c2f70e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı için barındırma sunucuları ekleme
İçinde sanal makineleri üzerinde SQL örnekleri kullanabilirsiniz, [Azure yığın](azure-stack-poc.md), veya kaynak sağlayıcısı sağlanan Azure yığın ortamınızı dışında bir örneği için bağlanabilirsiniz. Genel gereksinimler vardır:

* SQL örneği RP ve kullanıcı iş yükleri tarafından kullanım için ayrılmış olmalıdır. Uygulama Hizmetleri dahil olmak üzere diğer herhangi bir tüketici tarafından kullanılan bir SQL örneğini kullanamazsınız.
* SQL kaynak sağlayıcısı VM etki alanına katılmamışsa ve yalnızca SQL kimlik doğrulaması kullanarak bağlanabilir.
* Kaynak sağlayıcısı tarafından kullanım için uygun ayrıcalıklara sahip bir hesap yapılandırmanız gerekir.
* Bu ağ üzerindeki bir SQL örneğine bağlanma gerekli olacak şekilde kaynak sağlayıcısı ve kullanıcılar, Web uygulamaları gibi kullanıcı ağ kullanın. Bu gereksinim genellikle IP SQL örnekleri için ortak bir ağda olması gerektiği anlamına gelir.
* SQL örnekleri ve konaklarının Yönetimi kadar; Kaynak sağlayıcısı düzeltme eki uygulama, Yedekleme gerçekleştirmek yok, döndürme, vb. kimlik bilgisi.
* SKU'ları, her zaman açık, vb. gibi performans, SQL yeteneklerini farklı sınıflar oluşturmak için kullanılabilir.

Bir dizi SQL Iaas sanal makine görüntülerini Market yönetim özelliği kullanılarak kullanılabilir. Her zaman en son sürümünü indirme emin **SQL Iaas uzantısı** Market öğesi kullanarak bir VM'i dağıtmadan önce. SQL görüntüleri mevcut olan SQL VM'ler ile aynıdır. SQL Iaas uzantısı bu görüntüleri kullanılarak oluşturulan ve portal geliştirmeleri karşılık gelen VM'ler için otomatik düzeltme eki uygulama ve yedekleme özellikleri gibi özellikler sağlar.

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

> [!IMPORTANT]
> Özel karakterler, boşluklar ve dönemleri dahil olmak üzere desteklenmiyor **ailesi** veya **katmanı** SQL ve MySQL kaynak sağlayıcıları için bir SKU oluşturduğunuzda adları.

Örnek:

![SKU'lar](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

>[!NOTE]
> SKU'ları portalında görünür olması için bir saat sürebilir. SKU tam olarak oluşturulana kadar kullanıcılar bir veritabanı oluşturulamıyor.

## <a name="provide-capacity-using-sql-always-on-availability-groups"></a>SQL Always On kullanılabilirlik grupları kullanarak kapasitesi sağlar
SQL Always On örnekleri yapılandırma ek adımlar gerektirir ve en az üç sanal makineleri (veya fiziksel makineler) içerir.

> [!NOTE]
> SQL bağdaştırıcısı RP _yalnızca_ otomatik dengeli dağıtımı gibi yeni SQL özellikleri gerektirdiği şekilde her zaman açık için SQL 2016 SP1 Enterprise veya sonraki örnekleri destekler. Önceki ortak gereksinimlerinin listesi ek olarak:

Özellikle, etkinleştirmeniz gerekir [otomatik dengeli](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) her kullanılabilirlik grubundaki her SQL Server örneği için:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

İkincil örnekleri bu SQL komutlarını kullanın:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

SQL Always On barındırma sunucuları eklemek için aşağıdaki adımları izleyin:

1. Azure yığın Yönetici portalı'ndaki bir Hizmet Yöneticisi olarak oturum açın

2. Tıklatın **Gözat** &gt; **yönetim KAYNAKLARININ** &gt; **SQL barındırma sunucuları** &gt; **+ekleme**.

    **SQL barındırma sunucuları** dikey olan SQL Server'ın kaynak sağlayıcısının arka ucu olarak hizmet gerçek örnekleri için SQL Server Kaynak sağlayıcısı burada bağlanabilir.

3. Form üzerinde her zaman dinleyicisi (ve isteğe bağlı bağlantı noktası numarası) FQDN adresi kullandığınızdan emin olmak için SQL Server örneği ile bağlantı ayrıntıları doldurun. Sistem yönetici ayrıcalıklarıyla yapılandırılmış hesap için hesap bilgilerini sağlayın.

4. SQL her zaman üzerindeki kullanılabilirlik grubu örnekleri desteğini etkinleştirmek için bu onay kutusunu işaretleyin.

    ![Barındırma sunucuları](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. SQL Always On örnek bir SKU ekler. 

> [!IMPORTANT]
> Aynı SKU durumlarda her zaman açık olan tek başına sunucular karıştıramazsınız. İlk barındırma sunucusu sonuçlar hata ekledikten sonra türlerini karma olarak çalışıyor.


## <a name="making-sql-databases-available-to-users"></a>SQL veritabanları kullanıcılar için kullanılabilir hale getirme

Planları ve SQL veritabanları kullanıcılar için kullanılabilir hale getirmek için teklifleri oluşturun. Microsoft.SqlAdapter hizmet planına ekleme ve var olan bir kota ekleyin veya yeni bir tane oluşturun. Bir kota oluşturursanız, izin vermek için kapasitesini belirtin.

![Planları ve veritabanlarını içerecek şekilde teklifleri oluştur](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)


## <a name="next-steps"></a>Sonraki adımlar

[Veritabanı ekleyin](azure-stack-sql-resource-provider-databases.md)
