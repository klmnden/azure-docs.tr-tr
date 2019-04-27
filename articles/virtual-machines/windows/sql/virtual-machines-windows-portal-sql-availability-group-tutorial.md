---
title: SQL Server kullanılabilirlik gruplarını - Azure sanal makineler - öğretici | Microsoft Docs
description: Bu öğreticide, Azure sanal makineler üzerinde bir SQL Server Always On kullanılabilirlik grubu oluşturma işlemi gösterilmektedir.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
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
ms.date: 08/30/2018
ms.author: mikeray
ms.openlocfilehash: d86538fca907f7181bf58ff236bba8de186641fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60593794"
---
# <a name="tutorial-configure-always-on-availability-group-in-azure-vm-manually"></a>Öğretici: Yapılandırma Always On kullanılabilirlik grubu Azure VM'de el ile

Bu öğreticide, Azure sanal makineler üzerinde bir SQL Server Always On kullanılabilirlik grubu oluşturma işlemi gösterilmektedir. Tam öğretici, bir kullanılabilirlik grubunda iki SQL Server örneklerinde veritabanı çoğaltmasıyla oluşturur.

**Tahmini Süre**: Önkoşullar sağlandığında tamamlanması yaklaşık 30 dakika sürer.

Öğreticide yapı diyagramda gösterilmektedir.

![Kullanılabilirlik Grubu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, SQL Server Always On kullanılabilirlik grupları temel bir anlayışa sahip varsayar. Daha fazla bilgiye ihtiyacınız varsa bkz [genel bakış, Always On kullanılabilirlik grupları (SQL Server)](https://msdn.microsoft.com/library/ff877884.aspx).

Aşağıdaki tabloda, bu öğreticiye başlamadan önce tamamlanması gereken önkoşulları listeler:

|  |Gereksinim |Açıklama |
|----- |----- |----- |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png) | İki SQL sunucuları | -Azure kullanılabilirlik kümesinde <br/> -Tek bir etki alanı <br/> -Yük Devretme Kümelemesi özelliği yüklü olan |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)| Windows Server | Dosya Paylaşımı için küme tanığı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server hizmet hesabı | Etki alanı hesabı |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|SQL Server Aracısı hizmet hesabı | Etki alanı hesabı |  
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Güvenlik Duvarı bağlantı noktalarını açma | -SQL sunucusu: **1433** varsayılan örnek için <br/> -Veritabanı yansıtma uç noktası: **5022** veya tüm kullanılabilir bağlantı noktası <br/> -Kullanılabilirlik grubu yük dengeleyici IP adresi durum araştırması: **59999** veya tüm kullanılabilir bağlantı noktası <br/> -Küme Çekirdek yük dengeleyici IP adresi durum araştırması: **58888** veya tüm kullanılabilir bağlantı noktası |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Yük devretme kümesi özelliği Ekle | Bu özellik hem SQL sunucuları gerektirir |
|![Square](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/square.png)|Yükleme etki alanı hesabı | -Her bir SQL Server yerel yönetici <br/> -Her SQL Server örneği için SQL Server sysadmin sabit sunucu rolünün üyesi  |


Öğreticiye başlamadan önce yapmanız [Azure sanal makinelerinde Always On kullanılabilirlik grupları oluşturmak için gerekli önkoşulları tamamlayın](virtual-machines-windows-portal-sql-availability-group-prereq.md). Bu Önkoşullar zaten tamamlanmışsa, atlayabilirsiniz [küme oluşturma](#CreateCluster).

  >[!NOTE]
  > Bu öğreticide sağlanan adımları birçoğu ile artık otomatikleştirilebilir [Azure SQL VM CLI](virtual-machines-windows-sql-availability-group-cli.md) ve [Azure hızlı başlangıç şablonları](virtual-machines-windows-sql-availability-group-quickstart-template.md).


<!--**Procedure**: *This is the first “step”. Make titles H2’s and short and clear – H2’s appear in the right pane on the web page and are important for navigation.*-->

<a name="CreateCluster"></a>
## <a name="create-the-cluster"></a>Kümeyi oluşturma

Önkoşulları tamamladıktan sonra ilk adım, iki SQL Server'lar içeren Windows Server Yük devretme kümesi ve bir Tanık oluşturmaktır.

1. RDP için hem SQL sunucuları hem de Tanık sunucu yönetici olan bir etki alanı hesabı kullanarak ilk SQL Server.

   >[!TIP]
   >İzlediyseniz [Önkoşullar belgesi](virtual-machines-windows-portal-sql-availability-group-prereq.md), çağırılır firmanın oluşturduğunuz **CORP\Install**. Bu hesabı kullanın.

2. İçinde **Sunucu Yöneticisi'ni** panoyu seçin **Araçları**ve ardından **yük devretme kümesi Yöneticisi**.
3. Sol bölmede, sağ **yük devretme kümesi Yöneticisi**ve ardından **küme oluşturma**.
   ![Küme oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/40-createcluster.png)
4. Küme oluşturma Sihirbazı'nda, aşağıdaki tabloda yer alan ayarlarla sayfalarıyla adımla tek düğümlü bir küme oluşturun:

   | Sayfa | Ayarlar |
   | --- | --- |
   | Başlamadan önce |Varsayılanları kullan |
   | Sunucuları seçin |İlk SQL sunucusunun adını yazın **sunucu adını girin** tıklatıp **Ekle**. |
   | Doğrulama uyarısı |Seçin **ben bu küme için Microsoft'tan destek gerektirmez ve bu nedenle doğrulama testlerini çalıştırmak istemiyorsanız Hayır. Ben İleri'ye tıkladığınızda, küme oluşturma devam**. |
   | Kümeyi yönetmek için kullanılan erişim noktası |Bir küme adı yazın, örneğin **SQLAGCluster1** içinde **küme adı**.|
   | Onay |Depolama alanları'nı kullanmıyorsanız, varsayılan ayarları kullanın. Bu tablonun altındaki nota bakın. |

### <a name="set-the-windows-server-failover-cluster-ip-address"></a>Windows server yük devretme küme IP adresi ayarlama

1. İçinde **yük devretme kümesi Yöneticisi**, ekranı aşağı kaydırarak **küme çekirdek kaynakları** ve küme ayrıntıları genişletin. Her ikisi de görmelisiniz **adı** ve **IP adresi** kaynaklarında **başarısız** durumu. Kümenin aynı makine IP adresine atandığından IP adresi kaynağı çevrimiçi duruma getirilemiyor, bu nedenle bir yinelenen adres.

2. Başarısız sağ **IP adresi** kaynak ve ardından **özellikleri**.

   ![Küme Özellikleri](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/42_IPProperties.png)

3. Seçin **statik IP adresi** ve sanal makinelerinizi aynı alt ağdan kullanılabilir bir adres belirtin.

4. İçinde **küme çekirdek kaynakları** bölümünde, küme adını sağ tıklatıp **çevrimiçine**. Ardından, her iki kaynakları çevrimiçi olana kadar bekleyin. Küme Adı kaynağını çevrimiçi olduğunda, DC sunucusuna yeni bir AD bilgisayar hesabını güncelleştirir. Kullanılabilirlik grubu Küme hizmeti daha sonra çalıştırmak için bu AD hesabını kullanın.

### <a name="addNode"></a>Bir SQL Server kümesine ekleme

Bir SQL Server kümesine ekleyin.

1. Tarayıcı ağacında kümeye sağ tıklayıp **düğümü Ekle**.

    ![Düğümü kümeye Ekle](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/44-addnode.png)

1. İçinde **Düğüm Ekleme Sihirbazı'nı**, tıklayın **sonraki**. İçinde **sunucuları Seç** sayfasında, ikinci bir SQL Server ekleyin. Sunucu adı yazın **sunucu adını girin** ve ardından **Ekle**. İşiniz bittiğinde tıklayın **sonraki**.

1. İçinde **doğrulama uyarısı** sayfasında **Hayır** (bir üretim senaryosunda, doğrulama testleri gerçekleştirmeniz gerekir). Ardından **İleri**'ye tıklayın.

8. İçinde **onay** etiketli onay kutusunu temizleyin, depolama alanları'nı kullanıyorsanız sayfasında **tüm uygun Depolama alanlarını kümeye ekleyin.**

   ![Düğüm onayı ekleme](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/46-addnodeconfirmation.png)

    >[!WARNING]
   >Depolama alanları kullanarak ve değil işaretini kaldırın **tüm uygun Depolama alanlarını kümeye ekleyin**, Windows Kümeleme işlemi sırasında sanal diskleri ayırır. Sonuç olarak, depolama alanları küme kaldırılana kadar Disk Yöneticisi'nde veya Gezgini görünmez ve PowerShell kullanarak eklenemeyeceği. Depolama alanları, birden çok diskleri depolama havuzlarına gruplandırır. Daha fazla bilgi için [depolama alanları](https://technet.microsoft.com/library/hh831739).

1. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

   Yük Devretme Kümesi Yöneticisi, kümenizi yeni bir düğüm ve içinde listeler gösterir **düğümleri** kapsayıcı.

10. Uzak Masaüstü oturumunu kapatmak.

### <a name="add-a-cluster-quorum-file-share"></a>Küme Çekirdek dosya paylaşımı ekleme

Bu örnekte Windows Küme, Küme çekirdeğini oluşturmak için bir dosya paylaşımı kullanır. Bu öğreticide, bir düğüm ve dosya paylaşımı çoğunluğu çekirdeği kullanılır. Daha fazla bilgi için [bir yük devretme kümesinde Çekirdek Yapılandırmalarını Anlama](https://technet.microsoft.com/library/cc731739.aspx).

1. Uzak Masaüstü oturumu ile dosya paylaşımı tanığı üye sunucuya bağlanın.

1. Üzerinde **Sunucu Yöneticisi'ni**, tıklayın **Araçları**. Açık **Bilgisayar Yönetimi**.

1. Tıklayın **paylaşılan klasörleri**.

1. Sağ **paylaşımları**, tıklatıp **yeni paylaşım...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Kullanım **bir paylaşılan klasör oluşturma Sihirbazı** bir paylaşımı oluşturmak için.

1. Üzerinde **klasör yolu**, tıklayın **Gözat** bulun ve paylaşılan klasör için bir yol oluşturun. **İleri**’ye tıklayın.

1. İçinde **adı, açıklama ve ayarları** paylaşım adını ve yolunu doğrulayın. **İleri**’ye tıklayın.

1. Üzerinde **paylaşılan klasör izinleri** ayarlamak **izinleri Özelleştir**. Tıklayın **özel...** .

1. Üzerinde **izinleri Özelleştir**, tıklayın **Ekle...** .

1. Kümeyi oluşturmak için kullanılan hesap tam denetime sahip olduğundan emin olun.

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/50-filesharepermissions.png)

1. **Tamam** düğmesine tıklayın.

1. İçinde **paylaşılan klasör izinleri**, tıklayın **son**. Tıklayın **son** yeniden.  

1. Sunucu dışında oturum

### <a name="configure-cluster-quorum"></a>Küme çekirdeğini yapılandırın

Ardından, küme çekirdeği ayarlayın.

1. İlk küme düğümüne Uzak Masaüstü ile bağlanın.

1. İçinde **yük devretme kümesi Yöneticisi**, kümeye sağ tıklayın, fareyle **diğer eylemler**, tıklatıp **küme çekirdek ayarlarını yapılandır...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/52-configurequorum.png)

1. İçinde **küme çekirdeği Yapılandırma Sihirbazı**, tıklayın **sonraki**.

1. İçinde **çekirdek yapılandırma seçeneğini**, seçin **çekirdek tanığı Seç**, tıklatıp **sonraki**.

1. Üzerinde **çekirdek tanığı Seç**, tıklayın **dosya paylaşım tanığı yapılandırma**.

   >[!TIP]
   >Windows Server 2016'ya bir bulut tanığı destekler. Bu tür bir Tanık seçerseniz, bir dosya gerekmez Tanık paylaşın. Daha fazla bilgi için [bir yük devretme kümesi için bulut tanığı dağıtma](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness). Bu öğreticide, önceki işletim sistemleri tarafından desteklenen bir dosya paylaşımı tanığı kullanılır.

1. Üzerinde **dosya paylaşım Tanığını Yapılandır**, oluşturduğunuz paylaşım yolunu yazın. **İleri**’ye tıklayın.

1. Ayarları doğrulayın **onay**. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

Küme Çekirdek kaynakları ile bir dosya paylaşımı tanığı olarak yapılandırılır.

## <a name="enable-availability-groups"></a>Kullanılabilirlik gruplarını etkinleştir

Ardından, **AlwaysOn Kullanılabilirlik grupları** özelliği. Her iki SQL sunucularında bu adımları uygulayın.

1. Gelen **Başlat** ekranında, başlatma **SQL Server Configuration Manager**.
2. Tarayıcı ağacında **SQL Server Hizmetleri**, ardından sağ tıklayarak **SQL Server (MSSQLSERVER)** tıklayın ve hizmet **özellikleri**.
3. Tıklayın **AlwaysOn yüksek kullanılabilirlik** sekmesine ve ardından seçin **AlwaysOn Kullanılabilirlik gruplarını etkinleştir**gibi:

    ![AlwaysOn Kullanılabilirlik gruplarını etkinleştir](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/54-enableAlwaysOn.png)

4. **Uygula**'ya tıklayın. Tıklayın **Tamam** açılır iletişim kutusu.

5. SQL Server hizmetini yeniden başlatın.

Diğer SQL Server üzerinde bu adımları yineleyin.

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

1. RDP dosyası birinci SQL Server sysadmin sabit sunucu rolünün bir üyesi olan bir etki alanı hesabıyla başlatın.
1. SQL Server Management Studio'yu açın ve ilk SQL Server'a bağlanın.
7. İçinde **Nesne Gezgini**, sağ **veritabanları** tıklatıp **yeni veritabanı**.
8. İçinde **veritabanı adı**, türü **MyDB1**, ardından **Tamam**.

### <a name="backupshare"></a> Bir yedekleme paylaşımı oluşturun

1. İlk SQL sunucusuna **Sunucu Yöneticisi'ni**, tıklayın **Araçları**. Açık **Bilgisayar Yönetimi**.

1. Tıklayın **paylaşılan klasörleri**.

1. Sağ **paylaşımları**, tıklatıp **yeni paylaşım...** .

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/48-newshare.png)

   Kullanım **bir paylaşılan klasör oluşturma Sihirbazı** bir paylaşımı oluşturmak için.

1. Üzerinde **klasör yolu**, tıklayın **Gözat** bulun ve veritabanı yedekleme paylaşılan klasörü için bir yol oluşturun. **İleri**’ye tıklayın.

1. İçinde **adı, açıklama ve ayarları** paylaşım adını ve yolunu doğrulayın. **İleri**’ye tıklayın.

1. Üzerinde **paylaşılan klasör izinleri** ayarlamak **izinleri Özelleştir**. Tıklayın **özel...** .

1. Üzerinde **izinleri Özelleştir**, tıklayın **Ekle...** .

1. SQL Server ve SQL Server Aracısı hizmet hesapları için her iki sunucuyu tam denetime sahip olduğunuzdan emin olun.

   ![Yeni paylaşım](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/68-backupsharepermission.png)

1. **Tamam** düğmesine tıklayın.

1. İçinde **paylaşılan klasör izinleri**, tıklayın **son**. Tıklayın **son** yeniden.  

### <a name="take-a-full-backup-of-the-database"></a>Tam bir veritabanı yedek Al

Günlük zinciri başlatmak için yeni veritabanını yedeklemek gerekir. Yeni veritabanının bir yedeğini yapmazsanız, bir kullanılabilirlik grubuna eklenemez.

1. İçinde **Nesne Gezgini**, veritabanına sağ tıklayın, fareyle **görevler...** , tıklayın **yedekleme**.

1. Tıklayın **Tamam** varsayılan yedekleme konumuna tam yedek almak için.

## <a name="create-the-availability-group"></a>Kullanılabilirlik grubunu oluşturma
Aşağıdaki adımları kullanarak bir kullanılabilirlik grubunu yapılandırmak artık hazırsınız:

* İlk SQL Server'da bir veritabanı oluşturun.
* Tam yedekleme hem de bir veritabanının işlem günlüğü yedeklemesi alın
* Tam geri yükleme ve yedeklemeler ikinci bir SQL Server ile oturum **NORECOVERY** seçeneği
* Kullanılabilirlik grubunu oluşturma (**AG1**) zaman uyumlu işleme, otomatik yük devretme ve okunabilir ikincil çoğaltma

### <a name="create-the-availability-group"></a>Kullanılabilirlik grubu oluşturun:

1. İlk SQL Server için Uzak Masaüstü oturumu üzerinde. İçinde **Nesne Gezgini** SSMS'de sağ **AlwaysOn yüksek kullanılabilirlik** tıklatıp **yeni Kullanılabilirlik Grubu Sihirbazı'nı**.

    ![Yeni kullanılabilirlik grubu sihirbazını başlat](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/56-newagwiz.png)

2. İçinde **giriş** sayfasında **sonraki**. İçinde **kullanılabilirlik grubu adı belirtin** sayfasında, kullanılabilirlik grubu için bir ad yazın örneğin **AG1**, **kullanılabilirlik grup adının**. **İleri**’ye tıklayın.

    ![Yeni AG Sihirbazı, AG adını belirtin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/58-newagname.png)

3. İçinde **veritabanlarını seçin** sayfasında Veritabanınızı seçin ve tıklayın **sonraki**.

   >[!NOTE]
   >En az bir tam yedekleme hedeflenen birincil Çoğaltmada uyguladığınız için veritabanı kullanılabilirlik grubu için önkoşulları karşılar.

   ![Yeni AG Sihirbazı, veritabanlarını seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/60-newagselectdatabase.png)
4. İçinde **çoğaltmaları belirle** sayfasında **ekleme çoğaltma**.

   ![Yeni AG Sihirbazı, çoğaltmaları belirtin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/62-newagaddreplica.png)
5. **Sunucuya Bağlan** iletişim kutusu açılır. İkinci sunucunun adını yazın **sunucu adı**. **Bağlan**'a tıklayın.

   Geri **çoğaltmaları belirle** sayfasında, listelenen ikinci sunucuyu şimdi görmeniz **kullanılabilirlik çoğaltmalarının**. Çoğaltmaların aşağıdaki gibi yapılandırın.

   ![Yeni AG Sihirbazı, çoğaltmaları belirtin (tam)](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/64-newagreplica.png)

6. Tıklayın **uç noktaları** veritabanı yansıtma uç noktası bu kullanılabilirlik grubu için görmek için. Ayarlarken kullandığınız aynı bağlantı noktasını kullanacak [veritabanı yansıtma uç noktaları için güvenlik duvarı kuralı](virtual-machines-windows-portal-sql-availability-group-prereq.md#endpoint-firewall).

    ![Yeni AG Sihirbazı, ilk veri eşitlemeyi seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/66-endpoint.png)

8. İçinde **ilk veri eşitlemesini Seç** sayfasında **tam** ve paylaşılan bir ağ konumu belirtin. Konumu için [, oluşturduğunuz yedek paylaşımı](#backupshare). Olduğu, örnekte  **\\ \\ \<ilk SQL Server\>\Backup\\**. **İleri**’ye tıklayın.

   >[!NOTE]
   >Tam eşitleme, SQL Server'ın ilk örneğinde veritabanının tam yedekleme gerçekleştirir ve ikinci örneğine geri yükler. Büyük veritabanları için tam eşitleme önerilmez uzun sürebilir. El ile bir veritabanının yedeğini almak ve ile geri bu süreyi kısaltabilirsiniz `NO RECOVERY`. Veritabanı zaten ile geri `NO RECOVERY` ikinci SQL kullanılabilirlik grubunu yapılandırmadan önce sunucuda seçin **sadece Birleştir**. Kullanılabilirlik grubu yapılandırıldıktan sonra yedekleme almak istiyorsanız seçin **ilk veri eşitlemeyi atla**.

    ![Yeni AG Sihirbazı, ilk veri eşitlemeyi seçin](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/70-datasynchronization.png)

9. İçinde **doğrulama** sayfasında **sonraki**. Bu sayfa aşağıdaki görüntüye benzer olmalıdır:

    ![Yeni AG Sihirbazı, doğrulama](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/72-validation.png)

    >[!NOTE]
    >Kullanılabilirlik grubu dinleyicisi yapılandırılmadığı için dinleyici yapılandırması için bir uyarı yok. Azure sanal makinelerinde, dinleyici Azure Yük Dengeleyiciyi oluşturduktan sonra oluşturduğundan bu uyarıyı yoksayabilirsiniz.

10. İçinde **özeti** sayfasında **son**, yeni Kullanılabilirlik Grubu Sihirbazı'nı yapılandırır bekleyin. İçinde **ilerleme** sayfasında tıklayabilirsiniz **daha fazla ayrıntı** ayrıntılı ilerleme durumunu görüntülemek için. Sihirbaz tamamlandıktan sonra İnceleme **sonuçları** kullanılabilirlik grubunun başarıyla oluşturulduğunu doğrulamak için sayfa.

     ![Yeni AG Sihirbazı, sonuçlar](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/74-results.png)
11. Tıklayın **Kapat** sihirbazdan çıkmak için.

### <a name="check-the-availability-group"></a>Kullanılabilirlik grubunu denetleme

1. İçinde **Nesne Gezgini**, genişletme **AlwaysOn yüksek kullanılabilirlik**, ardından **kullanılabilirlik grupları**. Yeni kullanılabilirlik grubu bu kapsayıcısında görmelisiniz. Kullanılabilirlik grubunu sağ tıklatıp **Göster Pano**.

   ![AG panosunu Göster](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/76-showdashboard.png)

   **AlwaysOn panosunu** şuna benzer görünmelidir.

   ![AG Panosu](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/78-agdashboard.png)

   Çoğaltma, yük devretme modu her çoğaltma ve eşitleme durumu görebilirsiniz.

2. İçinde **yük devretme kümesi Yöneticisi**, kümenizi tıklayın. Seçin **rolleri**. Kullandığınız kullanılabilirlik grubu adı, küme üzerinde bir rolüdür. Bu kullanılabilirlik grubu dinleyicisi yapılandırmadınız çünkü istemci bağlantıları için bir IP adresi sahip değil. Bir Azure Yük Dengeleyiciyi oluşturduktan sonra dinleyiciyi yapılandırır.

   ![Yük Devretme Kümesi Yöneticisi'nde AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/80-clustermanager.png)

   > [!WARNING]
   > Kullanılabilirlik grubu yük devretme kümesi Yöneticisi'nden yük devretmek çalışmayın. Tüm yük devretme işlemleri içinden gerçekleştirilmelidir **AlwaysOn panosunu** ssms'de. Daha fazla bilgi için [kısıtlamaları üzerinde kullanarak yük devretme kümesi Yöneticisi kullanılabilirlik grupları ile](https://msdn.microsoft.com/library/ff929171.aspx).
    >

Bu noktada, bir kullanılabilirlik grubu çoğaltmalarda SQL Server'ın iki örneği ile var. Kullanılabilirlik grubu örnekleri arasında taşıyabilirsiniz. Kullanılabilirlik grubuna bir dinleyici olmadığı için henüz bağlanamazsınız. Azure sanal makineler'de dinleyicisi bir yük dengeleyici gerektirir. Sonraki adımda, Azure'da yük dengeleyici oluşturmaktır.

<a name="configure-internal-load-balancer"></a>

## <a name="create-an-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Azure sanal makinelerinde SQL Server kullanılabilirlik grubu yük dengeleyici gerektirir. Kullanılabilirlik grubu dinleyicisi ve Windows Server Yük devretme kümesi için yük dengeleyici IP adreslerini içerir. Bu bölümde, Azure portalında yük dengeleyici oluşturmak nasıl özetlenir.

Azure Load Balancer standart yük dengeleyici veya bir temel yük dengeleyici olabilir. Standart Load Balancer temel Load Balancer daha fazla özelliği vardır. Bir kullanılabilirlik grubu için standart Load Balancer (bir kullanılabilirlik kümesi) yerine bir kullanılabilirlik alanı kullanıyorsanız gereklidir. Yük Dengeleyici türleri arasındaki fark hakkında daha fazla bilgi için bkz: [Load Balancer SKU'su karşılaştırma](../../../load-balancer/load-balancer-overview.md#skus).

1. Azure portalında nerede, SQL sunucuları ve'a tıklayın kaynak grubuna gidin **+ Ekle**.
1. Arama **yük dengeleyici**. Microsoft tarafından yayımlanan Yük Dengeleyiciyi seçin.

   ![Yük Devretme Kümesi Yöneticisi'nde AG](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/82-azureloadbalancer.png)

1. **Oluştur**’a tıklayın.
1. Yük Dengeleyici için aşağıdaki parametreleri yapılandırın.

   | Ayar | Alan |
   | --- | --- |
   | **Ad** |Yük Dengeleyici için bir metin adı örneğin kullanın **sqlLB**. |
   | **Tür** |İç |
   | **Sanal ağ** |Azure sanal ağ adını kullanın. |
   | **Alt ağ** |Sanal makinenin bulunduğu alt ağ adını kullanın.  |
   | **IP adresi ataması** |Statik |
   | **IP adresi** |Alt ağdan kullanılabilir bir adres kullanın. Bu adres, kullanılabilirlik grubu dinleyicisi için kullanın. Bu, küme IP adresinden farklı olduğunu unutmayın.  |
   | **Abonelik** |Aynı abonelik, sanal makine olarak kullanın. |
   | **Konum** |Sanal makine olarak aynı konumu kullanın. |

   Azure portal dikey penceresinde, şu şekilde görünmelidir:

   ![Yük Dengeleyici oluşturma](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/84-createloadbalancer.png)

1. Tıklayın **Oluştur**, Yük Dengeleyiciyi oluşturun.

Yük Dengeleyici yapılandırmak için bir arka uç havuzu, bir yoklama oluşturma ve Yük Dengeleme kuralları gerekir. Azure portalında bunlar yapın.

### <a name="add-backend-pool-for-the-availability-group-listener"></a>Kullanılabilirlik grubu dinleyicisi için arka uç havuzu Ekle

1. Azure portalında, kullanılabilirlik grubuna gidin. Yeni oluşturulan yük dengeleyici görmek için görünümü yenilemeniz gerekebilir.

   ![Yük Dengeleyici kaynak grubunda bulabilirsiniz.](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/86-findloadbalancer.png)

1. Load balancer'ı tıklatın, **arka uç havuzları**, tıklatıp **+ Ekle**.

1. Arka uç havuzu için bir ad yazın.

1. Arka uç havuzu Vm'leri içeren kullanılabilirlik kümesi ile ilişkilendirin.

1. Altında **hedef ağ IP yapılandırmaları**, kontrol **sanal makine** ve sanal makinelerin kullanılabilirlik grubu çoğaltmaları barındıracak her ikisini de seçin. Dosya paylaşımı tanığı sunucusu içermez.

   >[!NOTE]
   >Her iki sanal makine belirtilmezse, bağlantıları yalnızca birincil çoğaltmaya başarılı olur.

1. Tıklayın **Tamam** arka uç havuzu oluşturun.

### <a name="set-the-probe"></a>Araştırma ayarlayın

1. Load balancer'ı tıklatın, **sistem durumu araştırmalarının**, tıklatıp **+ Ekle**.

1. Dinleyici durum araştırması aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Text | SQLAlwaysOnEndPointProbe |
   | **Protokol** | TCP seçin | TCP |
   | **Bağlantı Noktası** | Kullanılmayan herhangi bir bağlantı noktası | 59999 |
   | **Aralık**  | Saniye cinsinden araştırma girişimleri arasındaki süre |5 |
   | **Sağlıksız durum eşiği** | Bir sanal makinenin iyi durumda olmadığı kabul gerçekleşmesi gereken ardışık araştırma hatası sayısı  | 2 |

1. Tıklayın **Tamam** durum yoklaması ayarlamak için.

### <a name="set-the-load-balancing-rules"></a>Yük Dengeleme kuralları ayarlayın

1. Load balancer'ı tıklatın, **Yük Dengeleme kuralları**, tıklatıp **+ Ekle**.

1. Dinleyici Yük Dengeleme kuralları şu şekilde ayarlayın.

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Text | SQLAlwaysOnEndPointListener |
   | **Ön uç IP adresi** | Adres seçin |Yük Dengeleyici oluştururken oluşturduğunuz adresini kullanın. |
   | **Protokol** | TCP seçin |TCP |
   | **Bağlantı Noktası** | Kullanılabilirlik grubu dinleyicisinin bağlantı noktası kullan | 1433 |
   | **Arka uç bağlantı noktası** | Kayan IP için doğrudan sunucu dönüş ayarlandığında, bu alan kullanılmaz | 1433 |
   | **Araştırma** |Yoklama için belirtilen adı | SQLAlwaysOnEndPointProbe |
   | **Oturum kalıcılığı** | Açılan liste | **Yok.** |
   | **Boşta kalma zaman aşımı** | TCP bağlantısı açık tutmak için dakika | 4 |
   | **Kayan IP (doğrudan sunucu dönüşü)** | |Enabled |

   > [!WARNING]
   > Doğrudan sunucu dönüşü oluşturma sırasında ayarlanır. Bu değer değiştirilemez.

1. Tıklayın **Tamam** dinleyici Yük Dengeleme kuralları ayarlamak için.

### <a name="add-the-cluster-core-ip-address-for-the-windows-server-failover-cluster-wsfc"></a>Windows Server Yük devretme kümesi (WSFC) için küme çekirdek IP adresini ekleyin

WSFC IP adresi aynı zamanda yük dengeleyicide olması gerekir.

1. Aynı Azure load balancer, şirket Portalı'nda tıklatın **ön uç IP yapılandırması** tıklatıp **+ Ekle**. WSFC küme çekirdek kaynakları için yapılandırdığınız IP adresini kullanın. IP adresi, statik olarak ayarlayın.

1. Yük dengeleyicide tıklayın **sistem durumu araştırmalarının**, tıklatıp **+ Ekle**.

1. WSFC kümesi çekirdeği IP adresi durum araştırması aşağıdaki gibi ayarlayın:

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Text | WSFCEndPointProbe |
   | **Protokol** | TCP seçin | TCP |
   | **Bağlantı Noktası** | Kullanılmayan herhangi bir bağlantı noktası | 58888 |
   | **Aralık**  | Saniye cinsinden araştırma girişimleri arasındaki süre |5 |
   | **Sağlıksız durum eşiği** | Bir sanal makinenin iyi durumda olmadığı kabul gerçekleşmesi gereken ardışık araştırma hatası sayısı  | 2 |

1. Tıklayın **Tamam** durum yoklaması ayarlamak için.

1. Yük Dengeleme kuralları ayarlayın. Tıklayın **Yük Dengeleme kuralları**, tıklatıp **+ Ekle**.

1. Şu şekilde yük dengeleme kuralları küme çekirdek IP adresi ayarlayın.

   | Ayar | Açıklama | Örnek
   | --- | --- |---
   | **Ad** | Text | WSFCEndPoint |
   | **Ön uç IP adresi** | Adres seçin |WSFC IP adresini yapılandırıldığında oluşturduğunuz adresini kullanın. Bu dinleyici IP adresi farklıdır |
   | **Protokol** | TCP seçin |TCP |
   | **Bağlantı Noktası** | Bağlantı noktası için küme IP adresini kullanın. Bu dinleyici araştırma bağlantı noktası için kullanılmaz kullanılabilir bir bağlantı noktasıdır. | 58888 |
   | **Arka uç bağlantı noktası** | Kayan IP için doğrudan sunucu dönüş ayarlandığında, bu alan kullanılmaz | 58888 |
   | **Araştırma** |Yoklama için belirtilen adı | WSFCEndPointProbe |
   | **Oturum kalıcılığı** | Açılan liste | **Yok.** |
   | **Boşta kalma zaman aşımı** | TCP bağlantısı açık tutmak için dakika | 4 |
   | **Kayan IP (doğrudan sunucu dönüşü)** | |Enabled |

   > [!WARNING]
   > Doğrudan sunucu dönüşü oluşturma sırasında ayarlanır. Bu değer değiştirilemez.

1. Tıklayın **Tamam** Yük Dengeleme kuralları ayarlamak için.

## <a name="configure-listener"></a> Dinleyici yapılandırma

Bir sonraki şey yapmak için yük devretme kümesinde bir kullanılabilirlik grubu dinleyicisi yapılandırmaktır.

> [!NOTE]
> Bu öğreticide, tek bir dinleyiciniz - bir ILB IP adresiyle oluşturma işlemi gösterilmektedir. Bir veya daha fazla IP adresi kullanarak bir veya daha fazla dinleyici oluşturmak için bkz [Create Avaılabılıty Group dinleyici ve load balancer | Azure](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>

[!INCLUDE [ag-listener-configure](../../../../includes/virtual-machines-ag-listener-configure.md)]

## <a name="set-listener-port"></a>Dinleyici bağlantı noktasını ayarlayın

SQL Server Management Studio'da dinleyici bağlantı noktasını ayarlayın.

1. SQL Server Management Studio'yu başlatın ve birincil kopyaya bağlanın.

1. Gidin **AlwaysOn yüksek kullanılabilirlik** | **kullanılabilirlik grupları** | **kullanılabilirlik grubu dinleyicisi**.

1. Yük Devretme Kümesi Yöneticisi'nde, oluşturduğunuz adlı dinleyiciyi görmelisiniz. Dinleyici adına sağ tıklayıp **özellikleri**.

1. İçinde **bağlantı noktası** kutusunda için kullanılabilirlik grubu dinleyicisi bağlantı noktası numarasını belirtin. 1433 varsayılan değerdir, ardından **Tamam**.

Artık Resource Manager modunda çalıştıran Azure sanal makineler'de SQL Server kullanılabilirlik grubu vardır.

## <a name="test-connection-to-listener"></a>Dinleyici için bağlantıyı test edin

Bağlantıyı test etmek için:

1. RDP bir SQL Server'a aynı sanal ağda bulunan, ancak çoğaltma sahibi değildir. Bir SQL Server kümesinde kullanabilirsiniz.

1. Kullanım **sqlcmd** yardımcı programını kullanarak bağlantıyı test edin. Örneğin, aşağıdaki komut dosyası oluşturur bir **sqlcmd** Windows kimlik doğrulaması ile dinleyicisi aracılığıyla birincil kopyanın bağlantısı:

   ```cmd
   sqlcmd -S <listenerName> -E
   ```

   Dinleyici varsayılan dışında bir bağlantı noktası kullanıyorsa (1433) bağlantı noktası, bağlantı dizesinde bağlantı noktasını belirtin. Örneğin, aşağıdaki sqlcmd komutunu bir dinleyici bağlantı noktası 1435 bağlanır:

   ```cmd
   sqlcmd -S <listenerName>,1435 -E
   ```

Hangi SQL Server örneğini birincil çoğaltmayı barındıran için SQLCMD bağlantı otomatik olarak bağlanır.

> [!TIP]
> Belirttiğiniz bağlantı noktası hem de SQL Server güvenlik duvarının açık olduğundan emin olun. Her iki sunucuyu kullandığınız TCP bağlantı noktası için bir gelen kuralı gerektirir. Daha fazla bilgi için [Ekle veya güvenlik duvarı kuralını Düzenle](https://technet.microsoft.com/library/cc753558.aspx).

## <a name="next-steps"></a>Sonraki adımlar

- [İkinci bir kullanılabilirlik grubu için bir yük dengeleyici için bir IP adresi eklemek](virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md#Add-IP).
