---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineleri - Öğreticisi | Microsoft Docs
description: Bu öğretici Azure sanal makineler üzerinde bir SQL Server her zaman üzerinde kullanılabilirlik grubu oluşturulacağını gösterir.
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 08a00342-fee2-4afe-8824-0db1ed4b8fca
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/09/2017
ms.author: mikeray
ms.openlocfilehash: a3bba4e8fd83b160472a2dc6a9425192b4bbd301
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114618"
---
# <a name="configure-always-on-availability-group-in-azure-vm-manually"></a>Yapılandırma her zaman üzerindeki kullanılabilirlik grubu Azure VM'de el ile

Bu öğretici Azure sanal makineler üzerinde bir SQL Server her zaman üzerinde kullanılabilirlik grubu oluşturulacağını gösterir. Tam öğretici iki SQL sunucusu üzerindeki veritabanı çoğaltmasıyla bir kullanılabilirlik grubu oluşturur.

**Zaman tahmin**: Önkoşullar sağlandığında tamamlanması yaklaşık 30 dakika sürer.

Aşağıdaki diyagramda, öğreticide yapı açıklanmıştır.

![Kullanılabilirlik grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Önkoşullar

Öğretici, SQL Server Always On kullanılabilirlik grupları temel bilgilere sahip varsayar. Daha fazla bilgi için bkz: [genel bakış, Always On kullanılabilirlik grupları (SQL Server)](http://msdn.microsoft.com/library/ff877884.aspx).

Aşağıdaki tabloda bu öğreticiye başlamadan önce tamamlamanız gereken önkoşulları listeler:

|  |Gereksinim |Açıklama |
|----- |----- |----- |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | İki SQL sunucuları | -Bir Azure kullanılabilirlik kümesine <br/> -Tek bir etki alanı <br/> -Yük Devretme Kümelemesi özelliği yüklü olan |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Küme Tanık dosya paylaşımı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server hizmet hesabı | Etki alanı hesabı |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Aracısı hizmet hesabı | Etki alanı hesabı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Güvenlik Duvarı bağlantı noktalarını açın | -SQL Server: **1433** varsayılan örnek için <br/> -Veritabanı yansıtma uç noktası: **5022** veya tüm kullanılabilir bağlantı noktası <br/> -Azure yük dengeleyici araştırmasını: **59999** veya tüm kullanılabilir bağlantı noktası |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Yük Devretme Kümelemesi özelliği Ekle | Bu özellik, her iki SQL sunucuları gerektirir |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Yükleme etki alanı hesabı | -Her bir SQL Server yerel yönetici <br/> -Her SQL Server örneği için SQL Server sysadmin sabit sunucu rolünün üyesi  |


Öğreticiye başlamadan önce şunları gerçekleştirmeniz [tamamlamak Azure sanal makinelerinde Always On kullanılabilirlik grupları oluşturmak için Önkoşullar](virtual-machines-windows-portal-sql-availability-group-prereq.md). Bu Önkoşullar zaten tamamladıysanız, atlayabilirsiniz [küme oluşturma](#CreateCluster).


<!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## <a name="create-the-cluster"></a>Kümeyi oluşturma

Önkoşullar tamamlandıktan sonra ilk adım iki SQL Server'lar içeren Windows Server Yük devretme kümesi ve bir Tanık oluşturmaktır.

1. RDP SQL sunucuları ve Tanık sunucu üzerindeki bir yönetici olan bir etki alanı hesabı kullanarak ilk SQL Server.

   >[!TIP]
   >İzlediyseniz [önkoşul belgesi](virtual-machines-windows-portal-sql-availability-group-prereq.md), adlı bir hesap oluşturulan **CORP\Install**. Bu hesabı kullanın.

2. İçinde **Sunucu Yöneticisi'ni** Pano, select **Araçları**ve ardından **yük devretme kümesi Yöneticisi**.
3. Sol bölmesinde, **yük devretme kümesi Yöneticisi**ve ardından **bir küme oluşturmak**.
   ![Küme oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Küme oluşturma Sihirbazı'nda, aşağıdaki tablodaki ayarları sayfalarında adımla tek düğümlü bir küme oluşturun:

   | Sayfa | Ayarlar |
   | --- | --- |
   | Başlamadan önce |Varsayılanları kullan |
   | Sunucuları seçin |İlk SQL Server adı yazın **sunucu adını girin** tıklatıp **Ekle**. |
   | Doğrulama uyarısı |Seçin **ı bu küme için Microsoft desteğine gereksiniminiz ve bu nedenle doğrulama testlerini çalıştırmak istemiyorsanız Hayır. Sonraki tıkladığınızda, kümeyi oluşturmaya devam**. |
   | Kümeyi yönetmek için erişim noktası |Bir küme adı yazın, örneğin **SQLAGCluster1** içinde **küme adı**.|
   | Onay |Depolama alanları kullanmadığınız sürece varsayılan ayarları kullanın. Bu tablonun altındaki nota bakın. |

### <a name="set-the-cluster-ip-address"></a>Küme IP adresi ayarlayın

1. İçinde **yük devretme kümesi Yöneticisi**, aşağı kaydırarak **küme çekirdek kaynakları** ve küme Ayrıntıları'nı genişletin. Her ikisi de görmelisiniz **adı** ve **IP adresi** kaynaklarında **başarısız** durumu. Küme makine aynı IP adresine atandığından IP adresi kaynağı çevrimiçi duruma getirilemiyor, bu nedenle, yinelenen bir adresi.

2. Başarısız sağ **IP adresi** kaynak ve ardından **özellikleri**.

   ![Küme Özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Seçin **statik IP adresi** ve sanal makinelerinizi aynı alt ağdaki kullanılabilir bir adres belirtin.

4. İçinde **küme çekirdek kaynakları** bölümünde, küme adını sağ tıklatın ve **çevrimiçine**. Daha sonra her iki kaynağın çevrimiçi olana kadar bekleyin. Küme Adı kaynağını çevrimiçi olduğunda, DC sunucusuna yeni bir AD bilgisayar hesabı ile güncelleştirir. Kullanılabilirlik grubu kümelenmiş hizmet daha sonra çalıştırmak için bu AD hesabı kullanın.

### <a name="addNode"></a>Kümeye bir SQL Server ekleyin

Bir SQL Server kümeye ekleyin.

1. Tarayıcı ağacında kümeye sağ tıklayın ve **düğüm Ekle**.

    ![Düğümü kümeye ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. İçinde **Düğüm Ekleme Sihirbazı'nı**, tıklatın **sonraki**. İçinde **sunucuları Seç** sayfasında, ikinci SQL Server ekleyin. Sunucu adı yazın **sunucu adını girin** ve ardından **Ekle**. İşiniz bittiğinde tıklatın **sonraki**.

1. İçinde **doğrulama uyarısı** sayfasında, **Hayır** (bir üretim senaryosunda, doğrulama testleri gerçekleştirmeniz gerekir). Ardından **İleri**'ye tıklayın.

8. İçinde **onay** depolama alanları kullanıyorsanız sayfasında, etiketli onay kutusunun işaretini **tüm uygun depolama kümeye ekleyin.**

   ![Düğüm onay Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Depolama alanları kullanma ve değil işaretini **tüm uygun depolamayı kümeye eklemek**, Windows Kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları kümeden kaldırılana kadar Disk Yöneticisi'nde veya Explorer görünmez ve PowerShell kullanarak yeniden. Depolama alanları, birden çok diskleri depolama havuzları için gruplar. Daha fazla bilgi için bkz: [depolama alanları](https://technet.microsoft.com/library/hh831739).

1. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

   Yük Devretme Kümesi Yöneticisi'ni gösterir kümenizi yeni bir düğüm ve içinde listeler **düğümleri** kapsayıcı.

10. Uzak Masaüstü oturumunu kapatmak.

### <a name="add-a-cluster-quorum-file-share"></a>Bir küme çekirdek dosya paylaşımı Ekle

Bu örnekte, Küme çekirdeğini oluşturmak için bir dosya paylaşımı Windows kümesi kullanır. Bu öğretici, bir düğüm ve dosya paylaşımı çoğunluğu çekirdek kullanır. Daha fazla bilgi için bkz: [yük devretme kümesindeki çekirdek yapılandırmalarını anlama](http://technet.microsoft.com/library/cc731739.aspx).

1. Uzak Masaüstü oturumu ile dosya paylaşımı tanığı üye sunucuya bağlanın.

1. Üzerinde **Sunucu Yöneticisi'ni**, tıklatın **Araçları**. Açık **Bilgisayar Yönetimi**.

1. Tıklatın **paylaşılan klasörleri**.

1. Sağ **paylaşımları**, tıklatıp **yeni paylaşım...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Kullanım **bir paylaşılan klasör oluşturma Sihirbazı** bir paylaşımı oluşturmak için.

1. Üzerinde **klasör yolu**, tıklatın **Gözat** bulun ve paylaşılan klasör için bir yol oluşturun. **İleri**’ye tıklayın.

1. İçinde **adı, açıklama ve ayarları** paylaşım adını ve yolunu doğrulayın. **İleri**’ye tıklayın.

1. Üzerinde **paylaşılan klasör izinlerini** ayarlamak **izinleri Özelleştir**. Tıklatın **özel...** .

1. Üzerinde **izinleri Özelleştir**, tıklatın **Ekle...** .

1. Kümeyi oluşturmak için kullanılan hesabın tam denetime sahip olduğundan emin olun.

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. **Tamam**’a tıklayın.

1. İçinde **paylaşılan klasör izinlerini**, tıklatın **son**. Tıklatın **son** yeniden.  

1. Sunucunun dışında oturum

### <a name="configure-cluster-quorum"></a>Küme çekirdeğini yapılandırın

Ardından, Küme çekirdeğini ayarlayın.

1. İlk küme düğümüne Uzak Masaüstü kullanarak bağlanın.

1. İçinde **yük devretme kümesi Yöneticisi**, kümeye sağ tıklayın, fareyle **diğer eylemler**, tıklatıp **küme çekirdek ayarlarını yapılandır...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. İçinde **küme çekirdeği Yapılandırma Sihirbazı**, tıklatın **sonraki**.

1. İçinde **çekirdek yapılandırma seçeneğini**, seçin **çekirdek tanığı Seç**, tıklatıp **sonraki**.

1. Üzerinde **çekirdek tanığı Seç**, tıklatın **bir dosya paylaşımı tanığı Yapılandır**.

   >[!TIP]
   >Windows Server 2016 bulut Tanık destekler. Bu tür bir Tanık seçerseniz, bir dosya gerekmez paylaşımı tanığını. Daha fazla bilgi için bkz: [bir yük devretme kümesi için bir bulut tanığı dağıtmak](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Bu öğretici, önceki işletim sistemleri tarafından desteklenen bir dosya paylaşımı tanığı kullanır.

1. Üzerinde **dosya paylaşımı Tanığını Yapılandır**, oluşturduğunuz paylaşımının yolu yazın. **İleri**’ye tıklayın.

1. Ayarları doğrulayın **onay**. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

Küme Çekirdek kaynakları dosya paylaşım tanığı ile yapılandırılır.

## <a name="enable-availability-groups"></a>Kullanılabilirlik gruplarını etkinleştir

Ardından, **AlwaysOn Kullanılabilirlik grupları** özelliği. Her iki SQL sunucularında bu adımları uygulayın.

1. Gelen **Başlat** ekranında, başlatma **SQL Server Configuration Manager**.
2. Tarayıcı ağacında tıklayın **SQL Server Hizmetleri**, sonra sağ **SQL Server (MSSQLSERVER)** 'ı tıklatın ve hizmeti **özellikleri**.
3. Tıklatın **AlwaysOn yüksek kullanılabilirlik** sekmesini ve ardından **etkinleştirmek AlwaysOn Kullanılabilirlik grupları**aşağıdaki gibi:

    ![AlwaysOn Kullanılabilirlik gruplarını etkinleştir](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. **Uygula**'ya tıklayın. Tıklatın **Tamam** açılan iletişim kutusunda.

5. SQL Server hizmetini yeniden başlatın.

Diğer SQL Server'da bu adımları yineleyin.

<!-----------------
## <a name="endpoint-firewall"></a>Open firewall for the database mirroring endpoint

Each instance of SQL Server that participates in an Availability Group requires a database mirroring endpoint. This endpoint is a TCP port for the instance of SQL Server that is used to synchronize the database replicas in the Availability Groups on that instance.

On both SQL Servers, open the firewall for the TCP port for the database mirroring endpoint.

1. On the first SQL Server **Start** screen, launch **Windows Firewall with Advanced Security**.
2. In the left pane, select **Inbound Rules**. On the right pane, click **New Rule**.
3. For **Rule Type**, choose **Port**.
1. For the port, specify TCP and choose an unused TCP port number. For example, type *5022* and click **Next**.

   >[!NOTE]
   >For this example, we're using TCP port 5022. You can use any available port.

5. In the **Action** page, keep **Allow the connection** selected and click **Next**.
6. In the **Profile** page, accept the default settings and click **Next**.
7. In the **Name** page, specify a rule name, such as **Default Instance Mirroring Endpoint** in the **Name** text box, then click **Finish**.

Repeat these steps on the second SQL Server.
-------------------------->

## <a name="create-a-database-on-the-first-sql-server"></a>İlk SQL Server'da bir veritabanı oluşturun

1. İlk SQL Server sysadmin sabit sunucu rolünün üyesi olan bir etki alanı hesabı ile için RDP dosyasını çalıştırın.
1. SQL Server Management Studio'yu açın ve ilk SQL sunucusuna bağlanın.
7. İçinde **Object Explorer**, sağ **veritabanları** tıklatıp **yeni veritabanı**.
8. İçinde **veritabanı adı**, türü **MyDB1**, ardından **Tamam**.

### <a name="backupshare"></a> Bir yedekleme paylaşımı oluşturun

1. İlk SQL sunucusuna **Sunucu Yöneticisi'ni**, tıklatın **Araçları**. Açık **Bilgisayar Yönetimi**.

1. Tıklatın **paylaşılan klasörleri**.

1. Sağ **paylaşımları**, tıklatıp **yeni paylaşım...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Kullanım **bir paylaşılan klasör oluşturma Sihirbazı** bir paylaşımı oluşturmak için.

1. Üzerinde **klasör yolu**, tıklatın **Gözat** bulun ve veritabanı yedekleme paylaşılan klasörü için bir yol oluşturun. **İleri**’ye tıklayın.

1. İçinde **adı, açıklama ve ayarları** paylaşım adını ve yolunu doğrulayın. **İleri**’ye tıklayın.

1. Üzerinde **paylaşılan klasör izinlerini** ayarlamak **izinleri Özelleştir**. Tıklatın **özel...** .

1. Üzerinde **izinleri Özelleştir**, tıklatın **Ekle...** .

1. SQL Server ve SQL Server Aracısı hizmet hesapları her iki sunucu için tam denetim olduğundan emin olun.

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. **Tamam**’a tıklayın.

1. İçinde **paylaşılan klasör izinlerini**, tıklatın **son**. Tıklatın **son** yeniden.  

### <a name="take-a-full-backup-of-the-database"></a>Tam veritabanı yedeklemesi alın

Günlük zinciri başlatmak için yeni veritabanını yedeklemek gerekir. Yeni veritabanının bir yedeğini almazsanız, bir kullanılabilirlik grubuna eklenemez.

1. İçinde **Object Explorer**, veritabanına sağ tıklayın, fareyle **görevleri...** , tıklatın **yedekleme**.

1. Tıklatın **Tamam** varsayılan yedekleme konumuna tam yedekleme olabilmesi için.

## <a name="create-the-availability-group"></a>Kullanılabilirlik grubu oluşturma
Artık aşağıdaki adımları kullanarak bir kullanılabilirlik grubu yapılandırmak hazırsınız:

* İlk SQL Server'da bir veritabanı oluşturun.
* Tam yedekleme ve veritabanının işlem günlüğü yedeklemesi gerçekleştirin
* Tam geri yükleme ve yedeklemeleri ikinci bir SQL Server ile oturum **NORECOVERY** seçeneği
* Kullanılabilirlik grubu oluşturun (**AG1**) zaman uyumlu tamamlama, otomatik yük devretme ve okunabilir ikincil çoğaltmalarda

### <a name="create-the-availability-group"></a>Kullanılabilirlik grubu oluşturun:

1. Uzak Masaüstü oturumu üzerinde ilk SQL Server. İçinde **Object Explorer** SSMS, sağ **AlwaysOn yüksek kullanılabilirlik** tıklatıp **yeni Kullanılabilirlik Grubu Sihirbazı'nı**.

    ![Yeni kullanılabilirlik grubu sihirbazını başlat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. İçinde **giriş** sayfasında, **sonraki**. İçinde **kullanılabilirlik grubu adı belirtin** sayfasında, kullanılabilirlik grubu için bir ad yazın, örneğin **AG1**, **kullanılabilirlik grubu adının**. **İleri**’ye tıklayın.

    ![Yeni AG Sihirbazı, AG adını belirtin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. İçinde **seçin veritabanları** sayfasında Veritabanınızı seçin ve tıklayın **sonraki**.

   >[!NOTE]
   >En az bir tam yedekleme hedeflenen birincil Çoğaltmada uyguladığınız için veritabanı bir kullanılabilirlik grubu için Önkoşullar karşılıyor.

   ![Yeni AG Sihirbazı, veritabanlarını seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. İçinde **çoğaltmaları belirle** sayfasında, **eklemek çoğaltma**.

   ![Yeni AG Sihirbazı, çoğaltmaları belirtin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. **Sunucuya Bağlan** iletişim kutusu açılır. İkinci sunucunun adını yazın **sunucu adı**. **Bağlan**'a tıklayın.

   Geri **çoğaltmaları belirle** sayfasında, listelenen ikinci sunucu artık görmelisiniz **kullanılabilirlik çoğaltmalarının**. Çoğaltmaların aşağıdaki gibi yapılandırın.

   ![Yeni AG Sihirbazı, çoğaltmaları belirtin (tam)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Tıklatın **uç noktaları** yansıtma uç noktası bu kullanılabilirlik grubu için veritabanı görmek için. Ayarlarken kullandığınız aynı bağlantı noktasını [veritabanı yansıtma uç noktaları için güvenlik duvarı kuralı](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Yeni AG Sihirbazı, ilk veri eşitlemeyi seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. İçinde **ilk veri eşitlemesi** sayfasında **tam** ve paylaşılan bir ağ konumu belirtin. Konum, [oluşturduğunuz yedekleme paylaşımı](#backupshare). Olduğu, örnekte **\\\\\<ilk SQL Server\>\Backup\**. **İleri**’ye tıklayın.

   >[!NOTE]
   >Tam eşitleme veritabanı SQL Server'ın ilk örneğinin üzerinde tam yedeğini alır ve ikinci örneğine geri yükler. Büyük veritabanları için tam eşitleme önerilmez uzun bir süre devam edebilir. El ile veritabanının bir yedeğini almak ve onunla geri yükleme bu kez azaltabilir `NO RECOVERY`. Veritabanı zaten ile geri yüklenirse `NO RECOVERY` ikinci SQL kullanılabilirlik grubu yapılandırmadan önce sunucuda seçin **yalnızca katmak**. Kullanılabilirlik grubu yapılandırdıktan sonra yedekleyin istiyorsanız seçin **ilk veri eşitlemeyi atla**.

    ![Yeni AG Sihirbazı, ilk veri eşitlemeyi seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. İçinde **doğrulama** sayfasında, **sonraki**. Bu sayfa aşağıdaki görüntüye benzer görünmelidir:

    ![Yeni AG Sihirbazı, doğrulama](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Bir kullanılabilirlik grubu dinleyicisi yapılandırılmadığı için dinleyici yapılandırması için bir uyarı yok. Azure sanal makinelerde, dinleyiciyi Azure yük dengeleyici oluşturduktan sonra oluşturduğundan bu uyarıyı yoksayabilirsiniz.

10. İçinde **Özet** sayfasında, **son**, ardından yeni Kullanılabilirlik Grubu Sihirbazı'nı yapılandırırken bekleyin. İçinde **ilerleme** tıklayabilirsiniz sayfasında **daha fazla ayrıntı** ayrıntılı ilerleme durumunu görüntülemek için. Sihirbaz tamamlandıktan sonra İnceleme **sonuçları** sayfa kullanılabilirlik grubu başarıyla oluşturulduğunu doğrulayın.

     ![Yeni AG Sihirbazı, sonuçlar](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Tıklatın **Kapat** sihirbazdan çıkmak için.

### <a name="check-the-availability-group"></a>Kullanılabilirlik grubu denetleyin

1. İçinde **Object Explorer**, genişletin **AlwaysOn yüksek kullanılabilirlik**, ardından **kullanılabilirlik grupları**. Bu kapsayıcıda yeni kullanılabilirlik grubu görmelisiniz. Kullanılabilirlik grubunu sağ tıklatın ve **Göster Pano**.

   ![AG panosunu Göster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   **AlwaysOn panosunu** şuna benzer görünmelidir.

   ![AG Panosu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Çoğaltmalar, yük devretme modu her çoğaltma ve eşitleme durumu görebilirsiniz.

2. İçinde **yük devretme kümesi Yöneticisi**, kümenizi'ı tıklatın. Seçin **rolleri**. Kullandığınız kullanılabilirlik grubu adı küme üzerinde bir rolüdür. Bu kullanılabilirlik grubu dinleyici yapılandırmadı olduğundan istemci bağlantıları için bir IP adresi yok. Bir Azure yük dengeleyici oluşturduktan sonra dinleyicisi yapılandırır.

   ![Yük Devretme Kümesi Yöneticisi'nde AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Kullanılabilirlik grubu yük devretme kümesi Yöneticisi'nden üzerinden vermesine çalışmayın. Tüm yük devretme işlemlerini içinden gerçekleştirilmelidir **AlwaysOn panosunu** SSMS içinde. Daha fazla bilgi için bkz: [kısıtlamaları üzerinde kullanarak yük devretme kümesi Yöneticisi kullanılabilirlik grupları ile](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Bu noktada, çoğaltmalar SQL Server'ın iki örneği üzerinde kullanılabilirlik grubuyla sahip. Kullanılabilirlik grubu örnekleri arasında taşıyabilirsiniz. Dinleyici olmadığı için kullanılabilirlik grubu için henüz bağlanamıyor. Azure sanal makinelerinde dinleyicisi bir yük dengeleyici gerektirir. Sonraki adım, Azure yük dengeleyici oluşturmaktır.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Azure sanal makinelerde SQL Server kullanılabilirlik grubu yük dengeleyici gerektirir. Yük Dengeleyici, kullanılabilirlik grubu dinleyicileri ve Windows Server Yük devretme için IP adreslerini tutar. Bu bölümde Azure portalında yük dengeleyici oluşturma özetlenmektedir.

1. Azure Portal'da, burada SQL sunucularınızı ve tıklatın Kaynak grubuna gidin **+ Ekle**.
2. Arama **yük dengeleyici**. Microsoft tarafından yayımlanan yük dengeleyici seçin.

   ![Yük Devretme Kümesi Yöneticisi'nde AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1.  **Oluştur**’a tıklayın.
3. Yük Dengeleyici için aşağıdaki parametreleri yapılandırın.

   | Ayar | Alan |
   | --- | --- |
   | **Ad** |Yük Dengeleyici için bir metin adı kullanın örneğin **sqlLB**. |
   | **Tür** |İç |
   | **Sanal ağ** |Azure sanal ağı adını kullanın. |
   | **Alt ağ** |Sanal makine alt ağ adını kullanın.  |
   | **IP adresi ataması** |Statik |
   | **IP adresi** |Kullanılabilir bir alt ağ adresi kullanın. Bu, küme IP adresinden farklı olduğuna dikkat edin |
   | **Abonelik** |Aynı abonelikte sanal makine olarak kullanın. |
   | **Konum** |Aynı konumda sanal makine olarak kullanın. |

   Azure portal dikey penceresinde aşağıdaki gibi görünmelidir:

   ![Yük Dengeleyici oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Tıklatın **oluşturma**, yük dengeleyici oluşturmak için.

Yük Dengeleyici yapılandırmak için bir arka uç havuzu, bir araştırma oluşturmak ve Yük Dengeleme kuralları ayarlamanız gerekir. Bunlar Azure portalında yapın.

### <a name="add-backend-pool-for-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi için arka uç havuzu ekleme

1. Azure portalında kullanılabilirlik grubuna gidin. Yeni oluşturulan yük dengeleyici görmek için görünümü yenilemeniz gerekebilir.

   ![Yük Dengeleyici kaynak grubunda bulunamadı](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Yük Dengeleyici tıklatın, **arka uç havuzları**, tıklatıp **+ Ekle**. 

1. Arka uç havuzu VM'ler içeren kullanılabilirlik kümesi ile ilişkilendirin.

1. Altında **hedef ağ IP yapılandırmaları**, denetleme **sanal makine** ve sanal makinelerin kullanılabilirlik grubu çoğaltmaları barındıracak her ikisini de seçin. Dosya paylaşımı tanığı sunucusu içermez.

   >[!NOTE]
   >Her iki sanal makine belirtilmezse, bağlantıları yalnızca birincil çoğaltma başarılı olur.

1. Tıklatın **Tamam** arka uç havuzu oluşturmak için.

### <a name="set-the-probe"></a>Araştırma ayarlayın

1. Yük Dengeleyici tıklatın, **sistem durumu araştırmalarının**, tıklatıp **+ Ekle**.

1. Sistem durumu araştırma aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Metin | SQLAlwaysOnEndPointProbe |
   | **Protokol** | TCP seçin | TCP |
   | **Bağlantı Noktası** | Kullanılmayan bir bağlantı noktası | 59999 |
   | **Aralık**  | Saniye cinsinden araştırma girişimleri arasındaki süre |5 |
   | **Sağlıksız durum eşiği.** | Sağlıksız olarak kabul edilmesi bir sanal makine için oluşması gereken arka arkaya araştırma hatası sayısı  | 2 |

1. Tıklatın **Tamam** durumu araştırması ayarlamak için.

### <a name="set-the-load-balancing-rules"></a>Yük Dengeleme kuralları ayarlama

1. Yük Dengeleyici tıklatın, **Yük Dengeleme kuralları**, tıklatıp **+ Ekle**.

1. Yük Dengeleme kuralları aşağıdaki gibi ayarlayın.
   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Metin | SQLAlwaysOnEndPointListener |
   | **Ön uç IP adresi** | Adres seçin |Yük Dengeleyici oluşturduğunuz sırada oluşturduğunuz adresi kullanın. |
   | **Protokol** | TCP seçin |TCP |
   | **Bağlantı Noktası** | Kullanılabilirlik grubu dinleyicisinin bağlantı noktası kullanma | 1435 |
   | **Arka uç bağlantı noktası** | Kayan IP için doğrudan sunucu dönüş ayarladığınızda, bu alan kullanılmıyor | 1435 |
   | **Araştırma** |Araştırması için belirtilen adı | SQLAlwaysOnEndPointProbe |
   | **Oturum kalıcılığı** | Açılan liste | **Yok** |
   | **Boşta kalma zaman aşımı** | Bir TCP bağlantısı açık tutmak için dakika | 4 |
   | **Kayan IP (doğrudan sunucu dönüşü)** | |Etkin |

   > [!WARNING]
   > Doğrudan sunucu dönüşü oluşturma sırasında ayarlanır. Bu değer değiştirilemez.

1. Tıklatın **Tamam** Yük Dengeleme kuralları ayarlamak için.

### <a name="add-the-front-end-ip-address-for-the-wsfc"></a>Ön uç IP adresi için WSFC Ekle

Ayrıca WSFC IP adresi yük dengeleyicide olması gerekir. 

1. Portalda, yeni bir ön uç IP yapılandırması için WSFC ekleyin. WSFC küme çekirdek kaynağı için yapılandırılan IP adresi kullanın. IP adresi statik olarak ayarlayın. 

1. Yük Dengeleyici tıklatın, **sistem durumu araştırmalarının**, tıklatıp **+ Ekle**.

1. Sistem durumu araştırma aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Metin | WSFCEndPointProbe |
   | **Protokol** | TCP seçin | TCP |
   | **Bağlantı Noktası** | Kullanılmayan bir bağlantı noktası | 58888 |
   | **Aralık**  | Saniye cinsinden araştırma girişimleri arasındaki süre |5 |
   | **Sağlıksız durum eşiği.** | Sağlıksız olarak kabul edilmesi bir sanal makine için oluşması gereken arka arkaya araştırma hatası sayısı  | 2 |

1. Tıklatın **Tamam** durumu araştırması ayarlamak için.

1. Yük Dengeleme kuralları ayarlayın. Tıklatın **Yük Dengeleme kuralları**, tıklatıp **+ Ekle**.

1. Yük Dengeleme kuralları aşağıdaki gibi ayarlayın.
   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Metin | WSFCPointListener |
   | **Ön uç IP adresi** | Adres seçin |WSFC IP adresi yapılandırıldığında oluşturduğunuz adresi kullanın. |
   | **Protokol** | TCP seçin |TCP |
   | **Bağlantı Noktası** | Kullanılabilirlik grubu dinleyicisinin bağlantı noktası kullanma | 58888 |
   | **Arka uç bağlantı noktası** | Kayan IP için doğrudan sunucu dönüş ayarladığınızda, bu alan kullanılmıyor | 58888 |
   | **Araştırma** |Araştırması için belirtilen adı | WSFCEndPointProbe |
   | **Oturum kalıcılığı** | Açılan liste | **Yok** |
   | **Boşta kalma zaman aşımı** | Bir TCP bağlantısı açık tutmak için dakika | 4 |
   | **Kayan IP (doğrudan sunucu dönüşü)** | |Etkin |

   > [!WARNING]
   > Doğrudan sunucu dönüşü oluşturma sırasında ayarlanır. Bu değer değiştirilemez.

1. Tıklatın **Tamam** Yük Dengeleme kuralları ayarlamak için.

## <a name="configure-listener"></a> Dinleyici yapılandırın

Sonraki bir şey yapmak için yük devretme kümesinde bir kullanılabilirlik grubu dinleyicisi yapılandırmaktır.

> [!NOTE]
> Bu öğretici bir ILB IP adresi ile tek bir dinleyicisi - oluşturulacağını gösterir. Bir veya daha fazla IP adresi kullanarak bir veya daha fazla dinleyicileri oluşturmak için bkz: [oluşturma kullanılabilirlik grubu dinleyici ve yük dengeleyici | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Set dinleyicisi bağlantı noktası

SQL Server Management Studio'da dinleyicisi bağlantı noktası ayarlayın.

1. SQL Server Management Studio'yu başlatın ve birincil kopyaya bağlanın.

1. Gidin **AlwaysOn yüksek kullanılabilirlik** | **kullanılabilirlik grupları** | **kullanılabilirlik grubu dinleyicileri**.

1. Yük Devretme Kümesi Yöneticisi'nde oluşturulan dinleyici adı görmelisiniz. Dinleyici adına sağ tıklatın ve **özellikleri**.

1. İçinde **bağlantı noktası** kutusunda, daha önce kullandığınız $EndpointPort kullanarak için kullanılabilirlik grubu dinleyici bağlantı noktası numarası belirtin (1433 olduğu varsayılan), ardından **Tamam**.

Artık Azure sanal makinelerde Kaynak Yöneticisi modunda çalışan bir SQL Server kullanılabilirlik grubu vardır.

## <a name="test-connection-to-listener"></a>Dinleyici bağlantıyı sınayın

Bağlantıyı sınamak için:

1. RDP bir SQL Server'a aynı sanal ağda bulunan, ancak çoğaltma kendisine ait değil. Bir SQL Server kümesinde kullanabilirsiniz.

1. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı sınayın. Örneğin, aşağıdaki komut dosyasını oluşturur. bir **sqlcmd** Windows kimlik doğrulaması dinleyicisiyle aracılığıyla birincil çoğaltma bağlantısı:

    ```
    sqlcmd -S <listenerName> -E
    ```

    Dinleyici varsayılan dışında bir bağlantı noktası kullanıyorsa (1433) bağlantı noktası, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komut bir dinleyici bağlantı noktası 1435 bağlanır:

    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

SQL Server'ın hangi örneğinin birincil çoğaltmayı barındıran için SQLCMD bağlantı otomatik olarak bağlanır.

> [!TIP]
> Belirttiğiniz bağlantı noktasının Güvenlik Duvarı'nı her iki SQL sunucularının açık olduğundan emin olun. Her iki sunucuyu kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Daha fazla bilgi için bkz: [Ekle veya Düzenle güvenlik duvarı kuralı](http://technet.microsoft.com/library/cc753558.aspx).
>
>



<!--**Notes**: *Notes provide just-in-time info: A Note is “by the way” info, an Important is info users need to complete a task, Tip is for shortcuts. Don’t overdo*.-->


<!--**Procedures**: *This is the second “step." They often include substeps. Again, use a short title that tells users what they’ll do*. *("Configure a new web project.")*-->

<!--**UI**: *Note the format for documenting the UI: bold for UI elements and arrow keys for sequence. (Ex. Click **File > New > Project**.)*-->

<!--**Screenshot**: *Screenshots really help users. But don’t include too many since they’re difficult to maintain. Highlight areas you are referring to in red.*-->

<!--**No. of steps**: *Make sure the number of steps within a procedure is 10 or fewer. Seven steps is ideal. Break up long procedure logically.*-->


<!--**Next steps**: *Reiterate what users have done, and give them interesting and useful next steps so they want to go on.*-->

## <a name="next-steps"></a>Sonraki adımlar

- [İkinci bir kullanılabilirlik grubu için yük dengeleyici için IP adresi eklemek](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
