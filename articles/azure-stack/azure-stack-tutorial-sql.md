---
title: Yüksek oranda kullanılabilir SQL veritabanları Azure Stack'te teklif | Microsoft Docs
description: Azure Stack ile bir SQL Server Kaynak sağlayıcısı ana bilgisayarı ve yüksek oranda kullanılabilir SQL AlwaysOn veritabanı oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/23/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/23/2018
ms.openlocfilehash: 27a1f385468c2fac59bdb2a5365e798e975b7c87
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252014"
---
# <a name="tutorial-offer-highly-available-sql-databases"></a>Öğretici: Yüksek oranda kullanılabilir SQL veritabanı teklifi

Azure Stack operatör, SQL Server veritabanlarını barındırmak için server Vm'leri yapılandırabilirsiniz. Sonra bir SQL barındırma sunucusu başarıyla, oluşturulan ve Azure Stack tarafından yönetilen SQL veritabanları SQL hizmetlerine abone olan kullanıcılar kolayca oluşturabilirsiniz.

Bu öğretici bir Azure Stack Hızlı Başlangıç şablonu oluşturmak için nasıl kullanılacağını gösterir. bir [SQL Server AlwaysOn Kullanılabilirlik grubu](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017), bir Azure Stack SQL barındırma sunucusu ekleyin ve ardından yüksek oranda kullanılabilir bir SQL veritabanı oluşturun.

Ne öğreneceksiniz:

> [!div class="checklist"]
> * Bir şablondan bir SQL Server AlwaysOn Kullanılabilirlik grubu oluşturun
> * Azure Stack SQL barındırma sunucusu oluşturma
> * Yüksek oranda kullanılabilir bir SQL veritabanı oluşturma

Bu öğreticide, bir iki VM SQL Server AlwaysOn Kullanılabilirlik grubu oluşturulur ve mevcut Azure Stack Market öğesi kullanılarak yapılandırılır. 

Bu öğreticide adımları uygulamaya başlamadan önce emin olun [SQL Server Kaynak sağlayıcısı](azure-stack-sql-resource-provider-deploy.md) başarıyla yüklendi ve Azure Stack Market'te kullanılabilir aşağıdaki öğeleri:

> [!IMPORTANT]
> Aşağıdakilerin tümü, kullanılacak Azure Stack Hızlı Başlangıç şablonu için gereklidir.

- [Windows Server 2016 Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer) Market görüntüsü.
- SQL Server 2016 SP1 veya SP2 (Enterprise, Standard veya Geliştirici) Windows Server 2016 server görüntü üzerinde. Bu öğreticide [Windows Server 2016 üzerinde SQL Server 2016 SP2 Enterprise](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.sqlserver2016sp2enterprisewindowsserver2016) Market görüntüsü.
- [SQL Server Iaas uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) 1.2.30 sürümü veya üzeri. SQL Iaas uzantısı Market SQL Server öğeleri tüm Windows sürümleri için gerekli olan gerekli bileşenleri yükler. Bu SQL sanal makinelerde yapılandırılacak SQL özgü ayarları sağlar. Yerel Market'te uzantı yüklü değilse SQL sağlama başarısız olur.
- [Windows için özel betik uzantısı](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.CustomScriptExtension) 1.9.1 sürümü veya üzeri. Özel betik uzantısı, dağıtım sonrası VM özelleştirme görevlerini otomatik başlatmak için kullanılan bir araçtır.
- [PowerShell Desired State Configuration (DSC)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.DSC-arm) 2.76.0.0 sürümü veya üzeri. DSC, dağıtma ve yazılım hizmetleri için yapılandırma verilerini yönetme ve bu hizmetlerin çalıştırıldığı ortam yönetimi etkinleştiren bir Windows PowerShell içinde bir yönetim platformudur.

Azure Stack marketini için öğe ekleme hakkında daha fazla bilgi için bkz: [Azure Stack Marketini genel bakış](azure-stack-marketplace.md).

## <a name="create-a-sql-server-alwayson-availability-group"></a>Bir SQL Server AlwaysOn Kullanılabilirlik grubu oluşturun
SQL Server AlwaysOn Kullanılabilirlik grubu kullanarak dağıtmak için bu bölümdeki adımları kullanın [sql 2016 alwayson Azure Stack Hızlı Başlangıç şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2016-alwayson). Bu şablon, bir Always On kullanılabilirlik grubu iki SQL Server Enterprise, Standard veya geliştirici örnekleri dağıtır. Aşağıdaki kaynakları oluşturur:

- Bir ağ güvenlik grubu
- Bir sanal ağ
- Dört depolama hesapları (biri için Active Directory (AD), SQL, dosya paylaşım tanığı için diğeri VM tanılama için)
- Dört genel IP adresleri (iki her bir SQL VM ve SQL AlwaysOn dinleyiciye bağlı herkese açık yük dengeleyici için bir AD için bir tane)
- SQL Vm'leri için SQL AlwaysOn dinleyicisinde bağlı genel IP için bir dış yük dengeleyici
- Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan bir VM (Windows Server 2016)
- SQL Server 2016 SP1 veya SP2 Enterprise, Standard veya Developer Edition ile yapılandırılmış ve kümelenmiş iki VM (Windows Server 2016). Bu Market görüntüleri olmalıdır.
- Küme için dosya paylaşım tanığı olarak yapılandırılan bir VM (Windows Server 2016)
- SQL ve dosya paylaşım tanığı Vm'leri içeren bir kullanılabilirlik kümesi  

1. 
[!INCLUDE [azs-admin-portal](../../includes/azs-admin-portal.md)]

2. Seçin **\+** **kaynak Oluştur** > **özel**, ardından **şablon dağıtımı**.

   ![Özel şablon dağıtımı](media/azure-stack-tutorial-sqlrp/1.png)


3. Üzerinde **özel dağıtım** dikey penceresinde **şablonu Düzen** > **Hızlı Başlangıç şablonu** ve sonra kullanılabilir özel şablonlar için aşağı açılan listesini kullanın seçin **sql 2016 alwayson** şablon tıklayın **Tamam**, ardından **Kaydet**.

   [![](media/azure-stack-tutorial-sqlrp/2-sm.PNG "Hızlı Başlangıç şablonu seçin")](media/azure-stack-tutorial-sqlrp/2-lg.PNG#lightbox)

4. Üzerinde **özel dağıtım** dikey penceresinde **parametreleri Düzenle** ve varsayılan değerleri gözden geçirin. Tüm gerekli parametre bilgilerini sağlayın ve ardından gerektikçe değerleri Değiştir **Tamam**.<br><br> En az:

    - Karmaşık parolalar ADMINPASSWORD SQLSERVERSERVICEACCOUNTPASSWORD ve SQLAUTHPASSWORD parametrelerini belirtin.
    - Tümü küçük harf DNSSUFFIX parametresi için geriye doğru arama için DNS sonekini girin (**azurestack.external** ASDK yüklemeleri için).
    
   [![](media/azure-stack-tutorial-sqlrp/3-sm.PNG "Özel dağıtım parametreleri Düzenle")](media/azure-stack-tutorial-sqlrp/3-lg.PNG#lightbox)

5. Üzerinde **özel dağıtım** dikey penceresinde, yeni bir kaynak grubu oluşturmak ve kullanmak için bir abonelik seçin veya özel dağıtımı için var olan bir kaynak grubunu seçin.<br><br> Ardından, kaynak grubu konumunu seçin (**yerel** ASDK yüklemeleri için) ve ardından **Oluştur**. Özel dağıtım ayarlarını doğrulanır ve ardından dağıtım başlar.

    [![](media/azure-stack-tutorial-sqlrp/4-sm.PNG "Özel dağıtım oluşturma")](media/azure-stack-tutorial-sqlrp/4-lg.PNG#lightbox)


6. Yönetim portalında **kaynak grupları** ve ardından kaynak grubunun adı için özel dağıtım oluşturduğunuz (**resource-group** Bu örnek için). Tüm dağıtımlar başarıyla tamamladınız emin olmak için dağıtım durumunu görüntüleyin.<br><br>Ardından, kaynak grubu öğeleri gözden geçirin ve seçin **SQLPIPsql\<kaynak grubu adı\>**  genel IP adresi öğesi. Yük dengeleyicinin genel IP tam FQDN'sini ve genel IP adresini kaydedin. Bu SQL AlwaysOn Kullanılabilirlik grubu yararlanarak SQL barındırma sunucusu oluşturmak için bu Azure Stack operatör sağlamanız gerekir.

   > [!NOTE]
   > Şablon dağıtımı tamamlanması birkaç saat sürebilir.

   ![Özel dağıtım tamamlandı](./media/azure-stack-tutorial-sqlrp/5.png)

### <a name="enable-automatic-seeding"></a>Otomatik dengeli dağıtımı etkinleştir
Şablon başarıyla dağıtılan ve SQL AlwaysON kullanılabilirlik grubu yapılandırılmış sonra etkinleştirmelisiniz [otomatik dengeli dağıtımı](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) her bir kullanılabilirlik grubundaki SQL Server örneği üzerinde. 

Bir kullanılabilirlik grubunda otomatik dengeli dağıtımı ile oluşturduğunuzda, SQL Server otomatik olarak her veritabanı için ikincil çoğaltma grubundaki herhangi bir el ile müdahale olmadan AlwaysOn veritabanlarını yüksek kullanılabilirliğini sağlamak gerekli oluşturur.

AlwaysOn Kullanılabilirlik grubu için otomatik dengeli dağıtımı yapılandırmak için aşağıdaki SQL komutlarını kullanın. Değiştirin \<InstanceName\> birincil ile AlwaysOn Kullanılabilirlik grubu adı gerekli olan SQL Server adını ve < availability_group_name > örneği. 

Birincil SQL örneği:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON '<InstanceName>'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

>  ![Birincil SQL örneği betiği](./media/azure-stack-tutorial-sqlrp/sql1.png)

İkincil SQL örnekleri:

  ```sql
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```
>  ![İkincil SQL örnek betiği](./media/azure-stack-tutorial-sqlrp/sql2.png)

### <a name="configure-contained-database-authentication"></a>Bağımsız veritabanı kimlik doğrulamasını yapılandırma
Bir kapsanan veritabanı bir kullanılabilirlik grubuna eklemeden önce bağımsız veritabanı kimlik doğrulaması sunucu seçeneği 1 kullanılabilirlik grubu için bir kullanılabilirlik çoğaltması barındıran her sunucu örneği olarak ayarlandığından emin olun. Daha fazla bilgi için [bağımsız veritabanı kimlik doğrulaması](https://docs.microsoft.com/sql/database-engine/configure-windows/contained-database-authentication-server-configuration-option?view=sql-server-2017).

Kullanılabilirlik grubundaki her bir SQL Server örneği için bağımsız veritabanı kimlik doğrulaması sunucu seçeneği ayarlamak için aşağıdaki komutları kullanın:

  ```sql
  EXEC sp_configure 'contained database authentication', 1
  GO
  RECONFIGURE
  GO
  ```
>  ![Bulunan veritabanı kimlik doğrulaması](./media/azure-stack-tutorial-sqlrp/sql3.png)

## <a name="create-an-azure-stack-sql-hosting-server"></a>Azure Stack SQL barındırma sunucusu oluşturma
SQL Server AlwayOn kullanılabilirlik grubu oluşturulduğundan ve düzgün yapılandırılmış sonra bir Azure Stack operatörü bir Azure Stack barındırma ek kapasite veritabanları oluşturmak için kullanıcıları için kullanılabilir hale getirmek için SQL Server oluşturmanız gerekir. 

SQL AlwaysOn Kullanılabilirlik grubunun kaynak grubunun oluşturulduğu SQL yük dengeleyicinin genel IP daha önce kaydedilen için genel IP veya tam FQDN değerini kullandığınızdan emin olun (**SQLPIPsql\<kaynak grubu adı\>** ). Ayrıca, SQL Server AlwaysOn Kullanılabilirlik grubunda SQL örneklerine erişmek için kimlik doğrulama kimlik bilgileri kullanılan bilmeniz gerekir.

> [!NOTE]
> Bu adım, Azure Stack Yönetim Portalı'ndan Azure Stack operatör tarafından çalıştırılmalıdır.

SQL AlwaysOn Kullanılabilirlik grubunun yük dengeleyici dinleyicisi ile genel IP ve SQL kimlik doğrulaması oturum açma bilgileri, bir Azure Stack operatörü artık [SQL AlwaysOn Kullanılabilirlik grubu kullanarak SQL barındırma sunucusu oluşturma](azure-stack-sql-resource-provider-hosting-servers.md#provide-high-availability-using-sql-always-on-availability-groups). 

Ayrıca planları oluşturduktan ve SQL AlwaysOn veritabanı oluşturma kullanıcılar için kullanılabilir hale getirmek sunar emin olun. İşleç eklemeniz gerekecektir **Microsoft.SqlAdapter** hizmeti bir plana ve yüksek oranda kullanılabilir veritabanları için özel bir yeni kota oluştur. Planları oluşturma hakkında daha fazla bilgi için bkz. [Plan, teklif, kota ve abonelik genel bakış](azure-stack-plan-offer-quota-overview.md).

> [!TIP]
> **Microsoft.SqlAdapter** hizmet planları kadar eklemek kullanılabilir olmayacak [SQL Server Kaynak sağlayıcısı dağıtılan](azure-stack-sql-resource-provider-deploy.md).

## <a name="create-a-highly-available-sql-database"></a>Yüksek oranda kullanılabilir bir SQL veritabanı oluşturma
Sonra SQL kullanılabilirlik grubu oluşturulduğundan, yapılandırılmış ve bir Azure Stack SQL barındırma sunucusu Azure Stack operatör tarafından eklenen AlwaysOn, SQL Server veritabanı özellikleri dahil olmak üzere bir aboneliğe sahip bir Kiracı Kullanıcı destekleyen SQL veritabanları oluşturabilirsiniz. Bu bölümdeki adımları izleyerek AlwaysOn işlevselliği. 

> [!NOTE]
> Bu adımları Azure Stack Kullanıcı Portalı'ndan (Microsoft.SQLAdapter hizmeti) SQL Server özellikleriyle bir abonelikle bir kiracı kullanıcı olarak çalıştırın.

1. 
[!INCLUDE [azs-user-portal](../../includes/azs-user-portal.md)]

2. Seçin **\+** **kaynak Oluştur** > **veri \+ depolama**, ardından **SQL veritabanı**.<br><br>Adı, harmanlama, en büyük boyutunu ve abonelik, kaynak grubunu ve konumu dağıtım için kullanmak da dahil olmak üzere gerekli veritabanı özellik bilgileri sağlar. 

   ![SQL veritabanı oluşturma](./media/azure-stack-tutorial-sqlrp/createdb1.png)

3. Seçin **SKU** uygun SQL barındırma sunucusu kullanmak için SKU seçin. Bu örnekte, Azure Stack operatörü oluşturduğu **Kurumsal-HA** SQL AlwaysOn Kullanılabilirlik grupları için yüksek kullanılabilirliği desteklemek için SKU.

   ![Select SKU](./media/azure-stack-tutorial-sqlrp/createdb2.png)

4. Seçin **oturum açma** > **yeni bir oturum açma oluşturma** ve ardından yeni veritabanı için kullanılacak SQL kimlik doğrulaması kimlik bilgilerini sağlayın. İşiniz bittiğinde tıklayın **Tamam** ardından **Oluştur** veritabanı dağıtım işlemini başlatmak için.

   ![Oturum açma oluşturma](./media/azure-stack-tutorial-sqlrp/createdb3.png)

5. SQL veritabanı dağıtımı başarıyla tamamlandığında, yeni yüksek oranda kullanılabilir veritabanına bağlanmak için kullanılacak bağlantı dizesi bulmak için veritabanı özellikleri gözden geçirin. 

   ![Bağlantı dizesini görüntüle](./media/azure-stack-tutorial-sqlrp/createdb4.png)




## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir şablondan bir SQL Server AlwaysOn Kullanılabilirlik grubu oluşturun
> * Azure Stack SQL barındırma sunucusu oluşturma
> * Yüksek oranda kullanılabilir bir SQL veritabanı oluşturma

Bilgi edinmek için sonraki öğreticiye ilerleyin nasıl yapılır:
> [!div class="nextstepaction"]
> [Yüksek oranda kullanılabilir olan MySQL veritabanları oluşturma](azure-stack-tutorial-mysql.md)