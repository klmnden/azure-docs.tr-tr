---
title: "Bir SQL Server Sanal Makine Sağlama | Microsoft Belgeleri"
description: "Portal kullanarak Azure’da bir SQL Server sanal makine oluşturun ve bağlanın Bu öğretici, Resource Manager modunu kullanır."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: jroth
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: c923f9aae4c7a1b8bd4f5760d0ec4f33923b9321
ms.contentlocale: tr-tr
ms.lasthandoff: 08/15/2017

---
# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Azure Portal’da bir SQL Server sanal makinesi sağlama
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

Bu uçtan uca öğretici, SQL Server çalıştıran bir sanal makine sağlamak için Azure Portal’ın nasıl kullanılacağını gösterir.

Azure Virtual Machine (VM) galerisi, Microsoft SQL Server içeren birkaç görüntüyü içerir. Birkaç tıklatmayla, galerideki SQL VM görüntülerinden birini seçebilir ve bunu Azure ortamınızda sağlayabilirsiniz.

Bu öğreticide şunları yapacaksınız:

* [Galeriden bir SQL VM görüntüsü seçme](#select-a-sql-vm-image-from-the-gallery)
* [VM oluşturma ve yapılandırma](#configure-the-vm)
* [VM'yi Uzak Masaüstü ile açma](#open-the-vm-with-remote-desktop)
* [SQL Server'a uzaktan bağlanma](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Galeriden bir SQL VM görüntüsü seçme

1. Hesabınızı kullanarak [Azure portal](https://portal.azure.com)da oturum açın.

   > [!NOTE]
   > Bir Azure hesabınız yoksa, [Azure ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/)yi ziyaret edin.

2. Azure portalda **Yeni**’ye tıklayın. Portalda **Yeni** penceresi açılır.

3. **Yeni** penceresinde **İşlem**’e ve ardından **Tümünü gör**’e tıklayın.

   ![Yeni İşlem penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

4. Arama alanına **SQL Server** yazın ve ENTER tuşuna basın.

5. Ardından **Filtre** simgesine tıklayın ve yayımcı olarak **Microsoft**’u seçin. Sonuçlarda Microsoft tarafından yayımlanan SQL Server görüntülerini filtrelemek için filtre penceresinde **Bitti**’ye tıklayın.

   ![Azure Sanal Makineleri penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Kullanılabilir SQL Server görüntülerini gözden geçirin. Her görüntü bir SQL Server sürümü ve işletim sistemi tanımlar.

6. **Ücretsiz Lisans: Windows Server 2016 üzerinde SQL Server 2016 SP1 Developer** görüntüsünü seçin.

   > [!TIP]
   > Geliştirme testi amacıyla kullanım için ücretsiz olan tam özellikli SQL Server sürümü olduğundan bu öğreticide Developer sürümü kullanılmıştır. Yalnızca çalışan VM'ler için ücret ödersiniz. Ancak, bu öğreticide kullanmak üzere istediğiniz görüntüyü seçebilirsiniz.

   > [!TIP]
   > SQL VM görüntüleri, oluşturduğunuz VM'nin dakika başına fiyatlandırmasına SQL Server lisans maliyetlerini ekler (Developer ve Express sürümleri hariç). SQL Server Developer geliştirme/test için (üretim için değil), SQL Express ise hafif iş yükleri (1 GB bellek ve 10 GB depolama alanı sınırının altı) için ücretsizdir. Diğer seçenek ise kendi lisansınızı getirmeniz (KLG) ve böylece yalnızca sanal makine için ödeme yapmanızdır. Bu görüntü adlarının başına {BYOL} ön eki getirilir. 
   >
   > Bu seçeneklerle ilgili daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

7. **Dağıtım modeli seçin** altında, **Resource Manager**’ın seçili olduğunu doğrulayın. Yeni sanal makineler için önerilen dağıtım modeli Resource Manager’dır. 

8. **Oluştur**’a tıklayın.

    ![Resource Manager ile SQL VM oluşturma](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>VM yapılandırma
Bir SQL Server sanal makinesini yapılandırmak için beş pencere vardır.

| Adım | Açıklama |
| --- | --- |
| **Temel Bilgiler** |[Temel ayarları yapılandırma](#1-configure-basic-settings) |
| **Boyut** |[Sanal makine boyutunu seçme](#2-choose-virtual-machine-size) |
| **Ayarlar** |[İsteğe bağlı özellikleri yapılandırma](#3-configure-optional-features) |
| **SQL Server ayarları** |[SQL Server ayarlarını yapılandırma](#4-configure-sql-server-settings) |
| **Özet** |[Özeti gözden geçirme](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Temel ayarları yapılandırma

**Temel bilgiler** penceresinde, aşağıdaki bilgileri sağlayın:

* Benzersiz bir sanal makine **adı** girin.

* En iyi performans için VM için disk türü olarak **SSD**’yi seçin.

* VM’deki yerel yönetici hesabı için **Kullanıcı adı** belirtin. Bu hesap aynı zamanda SQL Server **sysadmin** sabit sunucu rolüne de eklenir.

* Güçlü bir **parola** girin.

* Birden fazla aboneliğiniz varsa, aboneliğin yeni VM için doğru olduğunu doğrulayın

* **Kaynak grubu** kutusuna, yeni kaynak grubu için bir ad yazın. Alternatif olarak, varolan bir kaynak grubunu kullanmak için **Varolanı kullan**’a tıklayın. Bir kaynak grubu, Azure’daki ilgili kaynakların bir koleksiyonudur (sanal makineler, depolama hesapları, sanal ağlar, vb.).

  > [!NOTE]
  > Yalnızca Azure’daki SQL Server dağıtımlarını test ediyor veya öğreniyorsanız, yeni bir kaynak grubu kullanmak faydalıdır. Test işleminizi tamamladıktan sonra, sanal makineyi ve bu kaynak grubu ile ilişkili tüm kaynakları otomatik olarak silmek için kaynak grubunu silin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a Genel Bakış](../../../azure-resource-manager/resource-group-overview.md).

* Bu dağıtımı barındıracak Azure bölgesi olarak bir **Konum** seçin.

* Ayarları kaydetmek için **Tamam**’a tıklayın.

    ![SQL Temel Bilgileri penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Sanal makine boyutunu seçme

**Boyut** adımında, **Boyutu seç** penceresinde bir sanal makine boyutunu seçin. Pencere ilk başta seçtiğiniz görüntüye göre önerilen makine boyutlarını görüntüler.

> [!IMPORTANT]
> **Boyut seçin** penceresinde gösterilen tahmini aylık maliyet, SQL Server lisans maliyetlerini içermez. Bu maliyet tek başına VM içindir. SQL Server Express ve Developer sürümleri için bu maliyet, tahmin edilen toplam maliyettir. Diğer sürümler için [Windows Sanal Makineler fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) bakın ve hedef SQL Server sürümünüzü seçin. Ayrıca bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

![SQL VM Boyut Seçenekleri](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Üretim iş yükleri için [Azure Virtual Machines'de SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)’nda önerilen makine boyutlarına ve yapılandırmalara bakın. Listede olmayan bir makine boyutu gerekiyorsa **Tümünü görüntüle** düğmesine tıklayın.

> [!NOTE]
> Sanal makine boyutları hakkında daha fazla bilgi için bkz. [Sanal makineler için boyutlar](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Makine boyutunuzu seçin ve ardından **Seç**’e tıklayın.

## <a name="3-configure-optional-features"></a>3. İsteğe bağlı özellikleri yapılandırma

**Ayarlar** penceresinde, sanal makine için Azure Storage, ağ ve izlemeyi yapılandırın.

* **Depolama** altında, **Yönetilen Diskler** altındaki **Evet**’i seçin.

   > [!NOTE]
   > Microsoft, SQL Server için Yönetilen Diskleri önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure Yönetilen Disklere Genel Bakış](../../../storage/storage-managed-disks-overview.md). Bir kullanılabilirlik kümesindeki yönetilen diskler hakkında daha fazla ayrıntı için bkz. [Kullanılabilirlik kümesindeki VM’ler için yönetilen diskleri kullanma](../manage-availability.md).

* **Ağ** altında, otomatik olarak doldurulan değerleri kabul edebilirsiniz. <seg>
  **Sanal ağ**, **Alt ağ**, **Genel IP adresi** ve **Ağ Güvenlik Grubu**’nu el ile yapılandırmak için her bir özelliğe de tıklayabilirsiniz..</seg> Bu öğreticinin amaçları doğrultusunda, varsayılan değerleri koruyun.

* Azure varsayılan olarak, VM için belirlenen aynı depolama hesabıyla **İzleme**’yi etkinleştirir. Burada bu ayarları değiştirebilirsiniz.

* Bu öğreticide, **Kullanılabilirlik kümesi** için varsayılan olarak kullanılan **hiçbiri** değerini değiştirmeden bırakabilirsiniz. SQL AlwaysOn Kullanılabilirlik Grupları kurulumunu planlıyorsanız, sanal makinenin yeniden oluşturulmasını önlemek için kullanılabilirliği yapılandırın.  Daha fazla bilgi için bkz. [Sanal Makinelerin Kullanılabilirliğini Yönetme](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Yapılandırma ayarlarını tamamladığınızda, **Tamam**’a tıklayın.

## <a name="4-configure-sql-server-settings"></a>4. SQL server ayarlarını yapılandırma
**SQL Server ayarları** penceresinde, belirli ayarları ve SQL Server iyileştirmelerini yapılandırın. SQL Server için yapılandırabileceğiniz ayarlar aşağıdakileri içerir.

| Ayar |
| --- |
| [Bağlantı](#connectivity) |
| [Kimlik doğrulaması](#authentication) |
| [Depolama yapılandırması](#storage-configuration) |
| [Otomatik Düzeltme Eki Uygulama](#automated-patching) |
| [Otomatik Yedekleme](#automated-backup) |
| [Azure Anahtar Kasası Tümleştirmesi](#azure-key-vault-integration) |
| [R Hizmetleri](#r-services) |

### <a name="connectivity"></a>Bağlantı

**SQL bağlantısı** altında, bu VM’de SQL Server örneğini istediğiniz erişim türünü belirtin. Bu öğreticinin amacı doğrultusunda, SQL Server’a İnternet’teki makineler ve hizmetlerden bağlantılara izin vermek amacıyla **Genel (internet)**’i seçin. Bu seçenek seçildiğinde, Azure güvenlik duvarı ve ağ güvenlik grubunu bağlantı noktası 1433'te trafiğe izin verecek şekilde otomatik olarak yapılandırır.

![SQL Bağlantı Seçenekleri](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Varsayılan olarak, SQL Server **1433** gibi iyi bilinen bir bağlantı noktasını dinler. Daha yüksek güvenlik için önceki iletişim kutusunda dinleme bağlantı noktasını 1401 gibi varsayılan olmayan bir bağlantı noktasıyla değiştirin. Bunu yaparsanız, bu bağlantı noktasına SSMS gibi bir istemci aracı kullanarak bağlanmanız gerekir.

İnternet üzerinden SQL Server'a bağlanmak için, sonraki bölümde açıklanan, SQL Server Kimlik Doğrulamasını etkinleştirmeniz gerekir.

İnternet üzerinden Veritabanı Altyapısı’na bağlantıları etkinleştirmek istemiyorsanız, aşağıdaki seçeneklerden birini seçin:

* SQL Server’a Yalnızca VM içinden gelen bağlantılara izin vermek için **Yerel (yalnızca VM dahilinde)** 
* SQL Server’a aynı sanal ağdaki makineler ve hizmetlerden gelen bağlantılara izin vermek için **Özel (Sanal Ağ dahilinde)** 

Genel olarak, senaryonuzun izin verdiği en kısıtlayıcı bağlantıyı seçerek güvenliği geliştirin. Ancak tüm seçenekler Ağ Güvenlik Grubu kuralları ve SQL/Windows Kimlik Doğrulaması üzerinden korumaya alınabilir. VM oluşturulduktan sonra Ağ Güvenlik Grubu’nu düzenleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](virtual-machines-windows-sql-security.md).

> [!NOTE]
> SQL Server Express sürümü için sanal makine görüntüsü, TCP/IP protokolünü otomatik olarak etkinleştirmez. Bu durum Genel ve Özel bağlantı seçenekleri için de geçerlidir. Express sürümü için VM'yi oluşturduktan sonra [TCP/IP protokolünü el ile etkinleştirmek](#configure-sql-server-to-listen-on-the-tcp-protocol) üzere SQL Server Yapılandırma Yöneticisi'ni kullanmanız gerekir.

### <a name="authentication"></a>Kimlik Doğrulaması

SQL Server Kimlik Doğrulaması gerekiyorsa, **SQL kimlik doğrulaması** altında **Etkinleştir**’e tıklayın.

![SQL Server Kimlik Doğrulaması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> SQL Server’a İnternet üzerinden erişmeyi planlıyorsanız (yani, Genel bağlantı seçeneği), burada SQL kimlik doğrulamasını etkinleştirmeniz gerekir. SQL Server'a genel erişim SQL Kimlik Doğrulaması kullanılması gerektirir.
> 
> 

SQL Server Kimlik Doğrulamasını etkinleştirirseniz, bir **Oturum açma adı** ve **parola** belirtin. Bu kullanıcı adı bir SQL Server Kimlik Doğrulaması oturum açma bilgisi ve **sysadmin** sabit sunucu rolü üyesi olarak yapılandırılır. Kimlik Doğrulama Modları hakkında daha fazla bilgi için bkz. [Kimlik Doğrulama Modu Seçme](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode)

SQL Server Kimlik Doğrulamasını etkinleştirmezseniz, SQL Server örneğine bağlanmak için VM’deki yerel Yönetici hesabını kullanabilirsiniz.

### <a name="storage-configuration"></a>Depolama yapılandırması

Depolama gereksinimlerini belirlemek için **Depolama yapılandırması**na tıklayın. 

![SQL Storage Yapılandırması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> VM’nizi standart depolama kullanmak için el ile yapılandırdıysanız, bu seçenek kullanılamaz. Otomatik depolama iyileştirmesi yalnızca Premium Storage için kullanılabilir.

> [!TIP]
> Durak sayısı ve her kaydırıcının üst sınırları seçtiğiniz VM boyutuna bağlıdır. Daha büyük ve daha güçlü bir VM, daha fazla ölçeklendirilebilir.

Gereksinimleri, saniye başına girdi/çıktı işlemleri (IOP), MB/saniyedeki iş ve toplam depolama boyutu olarak belirleyebilirsiniz. Hareketli ölçekleri kullanarak bu değerleri yapılandırın. İş yüküne göre bu depolama ayarlarını değiştirebilirsiniz. Portal, bu gereksinimler temelinde, eklenecek ve yapılandırılacak disk sayısını otomatik olarak hesaplar.

**Depolama iyileştirme seçimi** altında aşağıdaki seçeneklerden birini seçin:

* **Genel**, varsayılan ayardır ve çoğu iş yükünü destekler.
* **İşlem** işleme depolamayı geleneksel veritabanı OLTP iş yükleri için iyileştirir.
* **Veri depolama**, depolamayı çözümleme ve raporlama iş yükleri için iyileştirir.

### <a name="automated-patching"></a>Otomatik düzeltme eki uygulama

**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. Bakım penceresi için haftanın gününü, saati ve süreyi belirtin. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır. Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını istemiyorsanız tıklayın **Devre dışı**’na tıklayın.  

![SQL Otomatik Düzeltme Eki Uygulama](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Otomatik yedekleme

**Otomatik yedekleme** altında, tüm veritabanları için otomatik veritabanı yedeklemeyi etkinleştirin. Otomatik yedekleme varsayılan olarak devre dışıdır.

SQL otomatik yedeklemeyi etkinleştirdiğinizde, aşağıdakileri yapılandırabilirsiniz:

* Yedeklemeler için elde tutma süresi (gün)
* Yedeklemeler için kullanılacak depolama hesabı
* Yedeklemeler için şifreleme seçeneği ve parola
* Backup sistem veritabanları
* Yedekleme zamanlamasını yapılandırma

Yedeklemeyi şifrelemek için **Etkinleştir**’e tıklayın. Ardından **Parola**’yı belirtin. Azure yedeklemeleri şifrelemek için bir sertifika oluşturur ve bu sertifikayı korumak için belirtilen parolayı kullanır.

![SQL Otomatik Yedekleme](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure Anahtar Kasası tümleştirme

Şifreleme için güvenlik gizli anahtarlarını depolamak üzere, **Azure anahtar kasası tümleştirme**’ye ve **Etkinleştir**’e tıklayın.

![SQL Azure Anahtar Kasası Tümleştirme](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Aşağıdaki tabloda Azure Anahtar Kasası Tümleştirmeyi yapılandırmak için gereken parametreler listelenmektedir.

| PARAMETRE | AÇIKLAMA | ÖRNEK |
| --- | --- | --- |
| **Anahtar Kasası URL'si** |Anahtar kasası konumu. |https://contosokeyvault.vault.azure.net/ |
| **Asıl ad** |Azure Active Directory hizmet asıl adı. Bu ad İstemci Kimliği olarak da bilinir. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Asıl parola** |Azure Active Directory hizmet asıl gizli anahtarı. Bu gizli anahtar İstemci Gizli Anahtarı olarak da bilinir. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Kimlik bilgisi adı** |**Kimlik bilgisi adı**: AKV Tümleştirme, VM’nin anahtar kasasına erişim sağlamasına izin vererek, SQL Server’da bir kimlik bilgisi oluşturur Bu kimlik bilgisi için bir ad seçin. |mycred1 |

Daha fazla bilgi için bkz. [Azure VM’lerde SQL Server için Azure Anahtar Kasası Tümleştirmeyi Yapılandırma](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="r-services"></a>R hizmetleri

[SQL Server R Hizmetleri](https://msdn.microsoft.com/library/mt604845.aspx) seçeneğini etkinleştirebilirsiniz. Bu özellik, SQL Server 2016 ile gelişmiş analizi kullanmanıza olanak sağlar. **SQL Server Ayarları** penceresinde **Etkinleştir** seçeneğini belirleyin.

> [!NOTE]
> SQL Server 2016 Developer sürümü için bu seçenek portal tarafından yanlış bir şekilde devre dışı bırakılmıştır. Developer sürümü için VM oluşturduktan sonra R Hizmetlerini elle etkinleştirmelisiniz.

![SQL Server R Hizmetlerini Etkinleştirme](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

SQL Server ayarlarını yapılandırmayı bitirdiğinizde, **Tamam**’a tıklayın.

## <a name="5-review-the-summary"></a>5. Özeti gözden geçirme

**Özet** penceresinde, özeti gözden geçirin ve **Satın al**’a tıklayarak bu VM için belirtilen SQL Server, kaynak grubu ve kaynakları oluşturun.

Azure portalından dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir.

> [!NOTE]
> Size dağıtım zamanları hakkında bir fikir vermek için,Doğu ABD bölgesinde, varsayılan ayarlarla bir SQL VM dağıttım. Bu test dağıtımının tamamlanması toplam 26 dakika sürdü. Ancak bölgeniz ve seçili ayarlarınıza göre, daha hızlı veya daha yavaş dağıtım süresiyle karşılaşabilirsiniz.

## <a name="open-the-vm-with-remote-desktop"></a>VM’yi Uzak Masaüstü ile açma

Uzak Masaüstü kullanarak SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdiyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## <a name="enable-tcpip-for-developer-and-express-editions"></a>Developer ve Express sürümleri için TCP/IP’yi etkinleştirme

Yeni bir SQL Server VM’si sağlanırken Azure, SQL Server Developer ve Express sürümleri için TCP/IP’yi otomatik olarak etkinleştirmez. Aşağıdaki adımlarda, uzaktan IP adresiyle bağlanabilmeniz için TCP/IP’yi el ile nasıl etkinleştirebileceğiniz açıklanmıştır.

Aşağıdaki adımlarda, SQL Server Developer ve Express sürümleri için TCP/IP protokolünü etkinleştirme yöntemi olarak **SQL Server Yapılandırma Yöneticisi** kullanılır.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-to-sql-server-remotely"></a>SQL Server'a uzaktan bağlanma

Bu öğreticide,sanal makine için **Genel** erişimi ve **SQL Server Kimlik Doğrulaması**’nı seçtik. Bu ayarlar, İnternet üzerinden tüm istemcilerden gelen (doğru SQL oturum açma bilgilerine sahip oldukları varsayılarak) SQL Server bağlantılarına izin verecek şekilde sanal makineyi yapılandırdı.

> [!NOTE]
> Sağlama sırasında Genel’i seçmediyseniz, sağlamadan sonra SQL bağlantı ayarlarınızı portal üzerinden değiştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [SQL bağlantı ayarlarınızı değiştirme](virtual-machines-windows-sql-connect.md#change).

Aşağıdaki bölümlerde, VM’nizdeki SQL Server örneğinize İnternet üzerinden farklı bir bilgisayarın nasıl bağlanacağı gösterilmektedir.

> [!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Azure’da SQL Server'ı kullanma hakkında diğer bilgiler için bkz. [Azure Virtual Machines’de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md).
