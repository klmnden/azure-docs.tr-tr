<properties
    pageTitle="Bir SQL Server Sanal Makine Sağlama | Microsoft Azure"
    description="Portal kullanarak Azure’da bir SQL Server sanal makine oluşturun ve bağlanın Bu öğretici, Resource Manager modunu kullanır."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="05/24/2016"
    ms.author="jroth" />

# Azure Portal’da bir SQL Server sanal makine sağlama

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

Bu uçtan uca öğretici, SQL Server çalıştıran bir sanal makine sağlamak için Azure Portal’ın nasıl kullanılacağını gösterir.

Azure Virtual Machine (VM) galerisi, Microsoft SQL Server içeren birkaç görüntüyü içerir. Birkaç tıklatmayla, galerideki SQL VM görüntülerinden birini seçebilir ve bunu Azure ortamınızda sağlayabilirsiniz.

Bu öğreticide şunları yapacaksınız:

- [Galeriden bir SQL VM görüntüsü seçme](#select-a-sql-vm-image-from-the-gallery)
- [VM oluşturma ve yapılandırma](#configure-the-vm)
- [VM’yi Uzak Masaüstü ile açma](#open-the-vm-with-remote-desktop)
- [SQL Server'a uzaktan bağlanma](#connect-to-sql-server-remotely)

## Galeriden bir SQL VM görüntüsü seçme

1. Hesabınızı kullanarak [Azure portal](https://portal.azure.com)da oturum açın.

    >[AZURE.NOTE] Bir Azure hesabınız yoksa, [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)yi ziyaret edin.

1. Azure portalda **Yeni**’ye tıklayın. Portal **Yeni** dikey pencere açar. SQL Server VM kaynakları Marketin **Virtual Machines** grubundadır.

1. **Yeni** dikey penceresinde, **Virtual Machines**’e tıklayın.

1. Kullanılabilir tüm görüntüleri görmek için, **Virtual Machines** dikey penceresinde **Tümünü gör**’e tıklayın.

    ![Azure Virtual Machines Dikey Penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. **Veritabanı sunucuları** altında, **SQL Server**’a tıklayın. **Veritabanı sunucuları**nı bulmak için aşağı kaydırmanız gerekebilir. Kullanılabilir SQL Server şablonlarını gözden geçirin.

    ![Sanal Makine Galerisi SQL Görüntüleri](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Her şablon bir SQL Server sürümü ve işletim sistemi tanımlar. Listede bu görüntülerden birini seçin. Ardından sanal makine görüntüsünün bir açıklaması veren ayrıntılar dikey penceresini gözden geçirin.

1. **Dağıtım modeli seçin** altında, **Resource Manager**’ın seçili olduğunu doğrulayın ve **Oluştur**’a tıklayın.

    ![Resource Manager ile SQL VM oluşturma](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## VM yapılandırma
Bir SQL Server sanal makineyi yapılandırmak için beş dikey pencere vardır.

| Adım               | Açıklama                          |
|---------------------|-------------------------------|
| **Temel Bilgiler**              | [Temel ayarları yapılandırma](#1-configure-basic-settings)      |
| **Boyut**                | [Sanal makine boyutunu seçme](#2-choose-virtual-machine-size)   |
| **Ayarlar**            | [İsteğe bağlı özellikleri yapılandırma](#3-configure-optional-features)   |
| **SQL Server ayarları** | [SQL server ayarlarını yapılandırma](#4-configure-sql-server-settings) |
| **Özet**             | [Özeti gözden geçirme](#5-review-the-summary)            |

## 1. Temel ayarları yapılandırma
**Temel bilgiler** dikey penceresinde, aşağıdaki bilgileri sağlayın:

* Benzersiz bir sanal makine **adı** girin.
* VM’deki yerel yönetici hesabı için **Kullanıcı adı** belirtin. Bu hesap aynı zamanda SQL Server **sysadmin** sabit sunucu rolüne de eklenir.
* Güçlü bir **parola** girin.
* Birden fazla aboneliğiniz varsa, aboneliğin yeni VM için doğru olduğunu doğrulayın
* **Kaynak grubu** kutusuna, yeni kaynak grubu için bir ad yazın. Alternatif olarak, varolan bir kaynak grubunu kullanmak için tıklayın **Varolanı seç**’e tıklayın. Bir kaynak grubu, Azure’daki ilgili kaynakların bir koleksiyonudur (sanal makineler, depolama hesapları, sanal ağlar, vb.).

    >[AZURE.NOTE] Yalnızca Azure’daki SQL Server dağıtımlarını test ediyor veya öğreniyorsanız, yeni bir kaynak grubu kullanmak faydalıdır. Test işleminizi tamamladıktan sonra, sanal makineyi ve bu kaynak grubu ile ilişkili tüm kaynakları otomatik olarak silmek için kaynak grubunu silin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a Genel Bakış](../resource-group-overview.md).

* Bu dağıtım için bir **Konum** seçin.
* Ayarları kaydetmek için **Tamam**’a tıklayın.

    ![SQL Temel Bilgileri Dikey Penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## 2. Sanal makine boyutunu seçme
**Boyut** adımında, **Boyutu seç** dikey penceresinde bir sanal makine boyutunu seçin. Dikey pencere ilk başta seçtiğiniz şablona göre önerilen makine boyutlarını görüntüler. Ayrıca VM’yi çalıştırmak için aylık maliyeti de hesaplar.

![SQL VM Boyut Seçenekleri](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Üretim iş yükleri için, [Premium Storage](../storage/storage-premium-storage.md)’ı destekleyen bir sanal makine boyutu seçilmesini öneriyoruz. Bu düzeyde performans gerekli değilse, tüm makine boyutu seçeneklerini gösteren **Tümünü görüntüle** düğmesini kullanın. Örneğin, geliştirme veya test ortamı için daha küçük bir makine boyutu kullanabilirsiniz.

>[AZURE.NOTE] Sanal makine boyutları hakkında daha fazla bilgi için bkz. [Sanal makineler için boyutlar](virtual-machines-windows-sizes.md). SQL Server VM boyutları hakkında dikkat edilecek noktalar için bkz. [Azure Virtual Machines’de SQL Server için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

Makine boyutunuzu seçin ve ardından **Seç**’e tıklayın.

## 3. İsteğe bağlı özellikleri yapılandırma
**Ayarlar** dikey penceresinde, sanal makine için Azure Storage, ağ ve izlemeyi yapılandırın.

- **Depolama** altında, Standart veya Premium (SSD) olarak **Disk türü**nü belirtin. Premium Storage üretim iş yükleri için önerilir.

>[AZURE.NOTE] Premium Storage’ı desteklemeyen bir makine boyutu için Premium (SSD) seçerseniz, makine boyutunuz otomatik olarak değişir.  

- **Depolama hesabı** altında, otomatik olarak sağlanan depolama hesabı adını kabul edebilirsiniz. Ayrıca varolan bir hesabı seçmek için **Depolama hesabı**’na da tıklayabilir ve hesap türünü yapılandırabilirsiniz. Varsayılan olarak, Azure yerel olarak yedekli depolama ile yeni bir depolama hesabı oluşturur. Depolama seçenekleri hakkında daha fazla bilgi için bkz. [Azure Storage çoğaltma](../storage/storage-redundancy.md).

- **Ağ** altında, otomatik olarak doldurulan değerleri kabul edebilirsiniz. **Sanal ağ**, **Alt ağ**, **Genel IP adresi** ve **Ağ Güvenlik Grubu**’nu el ile yapılandırmak için her bir özelliğe de tıklayabilirsiniz.. Bu öğreticinin amaçları doğrultusunda, varsayılan değerleri koruyun.

- Azure varsayılan olarak, VM için belirlenen aynı depolama hesabıyla **İzleme**’yi etkinleştirir. Burada bu ayarları değiştirebilirsiniz.

- **Kullanılabilirlik kümesi** altında, bir kullanılabilirlik kümesi belirtin. Bu öğreticinin amaçları doğrultusunda, **yok**u seçebilirsiniz. SQL AlwaysOn Kullanılabilirlik Grupları kurulumunu planlıyorsanız, sanal makinenin yeniden oluşturulmasını önlemek için kullanılabilirliği yapılandırın.  Daha fazla bilgi için bkz. [Sanal Makinelerin Kullanılabilirliğini Yönetme](virtual-machines-windows-manage-availability.md).

Yapılandırma ayarlarını tamamladığınızda, **Tamam**’a tıklayın.

## 4. SQL server ayarlarını yapılandırma
**SQL Server ayarları** dikey penceresinde, belirli ayarları ve SQL Server iyileştirmelerini yapılandırın. SQL Server için yapılandırabileceğiniz ayarlar aşağıdakileri içerir.

| Ayar               |
|---------------------|
| [Bağlantı](#connectivity)              |
| [Kimlik Doğrulaması](#authentication)                |
| [Depolama yapılandırması](#storage-configuration)            |
| [Otomatik Düzeltme Eki Uygulama](#automated-patching) |
| [Otomatik Yedekleme](#automated-backup)             |
| [Azure Anahtar Kasası Tümleştirme](#azure-key-vault-integration)             |

### Bağlantı
**SQL bağlantısı** altında, bu VM’de SQL Server örneğini istediğiniz erişim türünü belirtin. Bu öğreticinin amacı doğrultusunda, SQL Server’a İnternet’teki makineler ve hizmetlerden bağlantılara izin vermek amacıyla **Genel (internet)**’i seçin. Bu seçenek seçildiğinde, Azure güvenlik duvarı ve ağ güvenlik grubunu bağlantı noktası 1433'te trafiğe izin verecek şekilde otomatik olarak yapılandırır.  

![SQL Bağlantı Seçenekleri](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

İnternet üzerinden SQL Server'a bağlanmak için, sonraki bölümde açıklanan, SQL Server Kimlik Doğrulamasını etkinleştirmeniz gerekir.

>[AZURE.NOTE] SQL Server VM’nize ağ iletişimleri için daha fazla kısıtlama eklemek mümkündür. Bunu VM oluşturulduktan sonra Ağ Güvenlik Grubu’nu düzenleyerek yapabilirsiniz. Daha fazla bilgi için bkz. [Ağ Güvenlik Grubu (NSG) nedir?](../virtual-network/virtual-networks-nsg.md)

İnternet üzerinden Veritabanı Altyapısı’na bağlantıları etkinleştirmek istemiyorsanız, aşağıdaki seçeneklerden birini seçin:

- SQL Server’a Yalnızca VM içinden gelen bağlantılara izin vermek için **Yerel (yalnızca VM dahilinde)** 
- SQL Server’a aynı sanal ağdaki makineler ve hizmetlerden gelen bağlantılara izin vermek için **Özel (Sanal Ağ dahilinde)** 

Genel olarak, senaryonuzun izin verdiği en kısıtlayıcı bağlantıyı seçerek güvenliği geliştirin. Ancak tüm seçenekler Ağ Güvenlik Grubu kuralları ve SQL/Windows Kimlik Doğrulaması üzerinden korumaya alınabilir.

**Bağlantı noktası** 1433 olarak varsayılan değer kılınır. Farklı bir bağlantı noktası belirtebilirsiniz.
Daha fazla bilgi için bkz. [Bir SQL Server Sanal Makinesine (Resource Manager) Bağlanma | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### Kimlik Doğrulaması
SQL Server Kimlik Doğrulaması gerekiyorsa, **SQL kimlik doğrulaması** altında **Etkinleştir**’e tıklayın.

![SQL Server Kimlik Doğrulaması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] SQL Server’a İnternet üzerinden erişmeyi planlıyorsanız (yani, Genel bağlantı seçeneği), burada SQL kimlik doğrulamasını etkinleştirmeniz gerekir. SQL Server'a genel erişim SQL Kimlik Doğrulaması kullanılması gerektirir.

SQL Server Kimlik Doğrulamasını etkinleştirirseniz, bir **Oturum açma adı** ve **parola** belirtin. Bu kullanıcı adı bir SQL Server Kimlik Doğrulaması oturum açma bilgisi ve **sysadmin** sabit sunucu rolü üyesi olarak yapılandırılır. Kimlik Doğrulama Modları hakkında daha fazla bilgi için bkz. [Kimlik Doğrulama Modu Seçme](http://msdn.microsoft.com/library/ms144284.aspx)

SQL Server Kimlik Doğrulamasını etkinleştirmezseniz, SQL Server örneğine bağlanmak için VM’deki yerel Yönetici hesabını kullanabilirsiniz.

### Depolama yapılandırması
Depolama gereksinimlerini belirlemek için **Depolama yapılandırması**na tıklayın. 

![SQL Storage Yapılandırması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Standart depolamayı seçerseniz, bu seçenek kullanılamaz. Otomatik depolama iyileştirmesi yalnızca Premium Storage için kullanılabilir.

Gereksinimleri, saniye başına girdi/çıktı işlemleri (IOP), MB/saniyedeki iş ve toplam depolama boyutu olarak belirleyebilirsiniz. Hareketli ölçekleri kullanarak bu değerleri yapılandırın. Portal, bu gereksinimler temelinde, disk sayısını otomatik olarak hesaplar.

Varsayılan olarak, Azure Storage’ı 5000 IOP 200 MB ve 1 TB depolama alanı için iyileştirir. İş yüküne göre bu depolama ayarlarını değiştirebilirsiniz. **Depolama iyileştirme seçimi** altında aşağıdaki seçeneklerden birini seçin:

- **Genel**, varsayılan ayardır ve çoğu iş yükünü destekler.
- **İşlem** işleme depolamayı geleneksel veritabanı OLTP iş yükleri için iyileştirir.
- **Veri depolama**, depolamayı çözümleme ve raporlama iş yükleri için iyileştirir.

>[AZURE.NOTE] Kaydırıcıların üst sınırları seçilen sanal makine boyutuna göre farklılık gösterir.

### Otomatik düzeltme eki uygulama
**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. Bakım penceresi için haftanın gününü, saati ve süreyi belirtin. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır. Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını istemiyorsanız tıklayın **Devre dışı**’na tıklayın.  

![SQL Otomatik Düzeltme Eki Uygulama](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-classic-sql-automated-patching.md).

### Otomatik yedekleme
**Otomatik yedekleme** altında, tüm veritabanları için otomatik veritabanı yedeklemeyi etkinleştirin. Otomatik yedekleme varsayılan olarak devre dışıdır.

SQL otomatik yedeklemeyi etkinleştirdiğinizde, aşağıdakileri yapılandırabilirsiniz:

- Yedeklemeler için elde tutma süresi (gün)
- Yedeklemeler için kullanılacak depolama hesabı
- Yedeklemeler için şifreleme seçeneği ve parola

Yedeklemeyi şifrelemek için **Etkinleştir**’e tıklayın. Ardından **Parola**’yı belirtin. Azure yedeklemeleri şifrelemek için bir sertifika oluşturur ve bu sertifikayı korumak için belirtilen parolayı kullanır.

![SQL Otomatik Yedekleme](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Yedekleme](virtual-machines-windows-classic-sql-automated-backup.md).

### Azure Anahtar Kasası tümleştirme
Şifreleme için güvenlik gizli anahtarlarını depolamak üzere, **Azure anahtar kasası tümleştirme**’ye ve **Etkinleştir**’e tıklayın.

![SQL Azure Anahtar Kasası Tümleştirme](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Aşağıdaki tabloda Azure Anahtar Kasası Tümleştirmeyi yapılandırmak için gereken parametreler listelenmektedir.

|PARAMETRE|AÇIKLAMA|ÖRNEK|
|----------|----------|-------|
|**Anahtar Kasası URL'si** |Anahtar kasası konumu.|https://contosokeyvault.vault.azure.net/ |
|**Asıl ad** |Azure Active Directory hizmet asıl adı. Bu ad İstemci Kimliği olarak da bilinir.  |fde2b411-33d5-4e11-af04eb07b669ccf2|
| **Asıl gizli anahtar**|Azure Active Directory hizmet asıl gizli anahtarı. Bu gizli anahtar İstemci Gizli Anahtarı olarak da bilinir. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM=|
|**Kimlik bilgisi adı**|**Kimlik bilgisi adı**: AKV Tümleştirme, VM’nin anahtar kasasına erişim sağlamasına izin vererek, SQL Server’da bir kimlik bilgisi oluşturur Bu kimlik bilgisi için bir ad seçin.| mycred1|

Daha fazla bilgi için bkz. [Azure VM’lerde SQL Server için Azure Anahtar Kasası Tümleştirmeyi Yapılandırma](virtual-machines-windows-classic-ps-sql-keyvault.md).

SQL Server ayarlarını yapılandırmayı bitirdiğinizde, **Tamam**’a tıklayın.

## 5. Özeti gözden geçirme
**Özet** dikey penceresinde, özeti gözden geçirin ve **Tamam**’a tıklayarak bu VM için belirtilen SQL Server, kaynak grubu ve kaynakları oluşturun.

Azure portaldan dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir.

>[AZURE.NOTE] Size dağıtım zamanları hakkında bir fikir vermek için,Doğu ABD bölgesinde, varsayılan ayarlarla bir SQL VM dağıttım. Bu test dağıtımının tamamlanması toplam 26 dakika sürdü. Ancak bölgeniz ve seçili ayarlarınıza göre, daha hızlı veya daha yavaş dağıtım süresiyle karşılaşabilirsiniz.

## VM’yi Uzak Masaüstü ile açma

Uzak Masaüstü kullanarak sanal makineye bağlanmak için aşağıdaki adımları kullanın:

1. Azure VM oluşturulduktan sonra, Azure panonuzda VM simgesi görünür. Bu, varolan sanal makinelerinize göz atarak da bulabilirsiniz. Yeni SQL sanal makinenize tıklayın. Bir **Sanal makine** dikey penceresi sanal makine ayrıntılarını görüntüler.
1. **Sanal makine** dikey penceresinin üst kısmında, **Bağlan**’a tıklayın.
1. Tarayıcı VM için RDP dosyasını indirir. RDP dosyasını açın.
    ![SQL VM’ye Uzak Masaüstü Bağlantısı](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Uzak Masaüstü Bağlantısı, bu uzak bağlantının yayımcısının tanımlanamadığı bildiriminde bulunur. Devam etmek için **Bağlan**’a tıklayın.
1. **Windows Güvenliği** iletişim kutusunda, **Başka bir hesap kullan**’a tıklayın.
1. **Kullanıcı adı** için, VM’yi yapılandırırken<user name> belirttiğiniz kullanıcı olan **\<kullanıcı adını>** yazın. Adın önüne bir başlangıç eğik çizgisi eklemelisiniz.
1. Bu VM için daha önce yapılandırdığınız **Parola**’yı yazın ve bağlanmak için **Tamam**’a tıklayın.
1. Başka bir **Uzak Masaüstü Bağlantısı** iletişim kutusu size bağlanıp bağlanmayı sorarsa **Evet**’e tıklayın.

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdeyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## SQL Server'a uzaktan bağlanma

Bu öğreticide,sanal makine için **Genel** erişimi ve **SQL Server Kimlik Doğrulaması**’nı seçtik. Bu ayarlar, İnternet üzerinden tüm istemcilerden gelen (doğru SQL oturum açma bilgilerine sahip oldukları varsayılarak) SQL Server bağlantılarına izin verecek şekilde sanal makineyi yapılandırdı.

>[AZURE.NOTE] Sağlama işlemi sırasında Genel seçeneğini seçmediyseniz, SQL Server örneğinize İnternet üzerinden erişmek için ek adımlar gereklidir. Daha fazla bilgi için bkz. [Bir SQL Server Sanal Makinesine Bağlanma](virtual-machines-windows-sql-connect.md).

Aşağıdaki bölümlerde, VM’nizdeki SQL Server örneğinize İnternet üzerinden farklı bir bilgisayarın nasıl bağlanacağı gösterilmektedir.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## Sonraki Adımlar
Azure’da SQL Server'ı kullanma hakkında diğer bilgiler için bkz. [Azure Virtual Machines’de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md).

Azure Virtual Machines’de SQL Server’a videolu genel bakış için [Azure VM, SQL Server 2016 için en iyi platformdur](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016) videosunu izleyin.



<!--HONumber=Jun16_HO2-->


