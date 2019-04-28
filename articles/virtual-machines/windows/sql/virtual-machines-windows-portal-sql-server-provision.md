---
title: Sağlama, Azure portalında Windows SQL Server Vm'leri için kılavuz | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, Azure portalında Windows SQL Server 2017 sanal makinelerine oluşturmaya yönelik seçeneklerinizi açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 05/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: bb051d37f3a1dd82d7d46bfe8b22c2ba1251be85
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62129898"
---
# <a name="how-to-provision-a-windows-sql-server-virtual-machine-in-the-azure-portal"></a>Azure portalında bir Windows SQL Server sanal makinesi sağlama

Azure portalında Windows SQL Server sanal makine oluşturduğunuzda, bu kılavuzda kullanılabilir farklı seçenekler hakkında ayrıntılar sağlar. Bu makalede daha fazla yapılandırma seçeneği ele alınmaktadır [SQL Server VM Hızlı Başlangıç](quickstart-sql-vm-create-portal.md), daha fazla ile bir olası görev sağlama gider. 

Kendi SQL Server VM oluşturmak için bu kılavuzu kullanın. Veya Azure portalında kullanılabilir seçenekler için referans olarak kullanın.

> [!TIP]
> SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a id="select"></a> SQL Server sanal makine galeri görüntüleri

Bir SQL Server sanal makine oluşturduğunuzda, sanal makine galerisinden birden fazla önceden yapılandırılmış görüntülerden birini seçebilirsiniz. Aşağıdaki adımlarda, SQL Server 2017 görüntülerinden birini seçme gösterilmektedir.

1. Hesabınızı kullanarak [Azure portal](https://portal.azure.com)da oturum açın.

1. Azure portalında **Kaynak oluştur**’a tıklayın. Portalda **Yeni** penceresi açılır.

1. **Yeni** penceresinde **İşlem**’e ve ardından **Tümünü gör**’e tıklayın.

1. Arama alanına **SQL Server 2017** yazın ve ENTER tuşuna basın.

1. Filtre açılan menülerde, seçin _Windows Server 2016_ için **işletim sistemi** seçip _Microsoft_ olarak **yayımcı**. 

     ![Yeni İşlem penceresi](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

1. Kullanılabilir SQL Server görüntülerini gözden geçirin. Her görüntü bir SQL Server sürümü ve işletim sistemi tanımlar.

1. Görüntüsünü seçin **ücretsiz SQL Server Lisansı: Windows Server 2016 üzerinde SQL Server 2017 Developer**.

   > [!TIP]
   > Geliştirme testi için bir tam özellikli ve ücretsiz SQL Server sürümü olduğundan bu izlenecek yolda Developer sürümü kullanılır. Yalnızca çalışan VM'ler için ücret ödersiniz. Ancak, bu izlenecek yolda kullanmak üzere istediğiniz görüntüyü birini seçebilirsiniz. Kullanılabilir görüntüleri açıklaması için bkz: [SQL Server Windows sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

   > [!TIP]
   > SQL Server Lisans maliyetlerini saniye başına fiyatına VM oluşturma ve değişir çekirdek sürümü ile birleştirilir. Geliştirme/test için (üretim için değil), SQL Express ise hafif iş yükleri (1 GB bellek, küçüktür 10 GB depolama alanı) için ancak SQL Server Developer sürümü ücretsizdir. Ayrıca--kendi-lisansını getir (KLG) ve yalnızca VM için ödeme. Bu görüntü adlarının başına {BYOL} ön eki getirilir. 
   >
   > Bu seçeneklerle ilgili daha fazla bilgi için bkz. [SQL Server Azure VM’leri için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Dağıtım modeli seçin** altında, **Resource Manager**’ın seçili olduğunu doğrulayın. Yeni sanal makineler için önerilen dağıtım modeli Resource Manager’dır. 

1. **Oluştur**’u seçin.


## <a id="configure"></a> Yapılandırma seçenekleri

Bir SQL Server sanal makineyi yapılandırmak için birden çok sekme bulunur. Bu kılavuzun amacı doğrultusunda, biz aşağıdakilere odaklanır: 

| Adım | Açıklama |
| --- | --- |
| **Temel Bilgiler** |[Temel ayarları yapılandırma](#1-configure-basic-settings) |
| **İsteğe bağlı özellikler** |[İsteğe bağlı özellikleri yapılandırma](#2-configure-optional-features) |
| **SQL Server ayarları** |[SQL Server ayarlarını yapılandırma](#3-configure-sql-server-settings) |
| **Gözden geçir + oluştur** | [Özeti gözden geçirme](#4-review--create) |

## <a name="1-configure-basic-settings"></a>1. Temel ayarları yapılandırma


Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri sağlayın:

* Altında **Project Details**, doğru aboneliğin seçildiğinden emin olun. 
*  İçinde **kaynak grubu** bölümü, seçin mevcut bir kaynak grubunda listeden veya seçin **Yeni Oluştur** yeni bir kaynak grubu oluşturmak için. Bir kaynak grubu, Azure’daki ilgili kaynakların bir koleksiyonudur (sanal makineler, depolama hesapları, sanal ağlar, vb.). 

    ![Abonelik](media/quickstart-sql-vm-create-portal/basics-project-details.png)

  > [!NOTE]
  > Yalnızca Azure’daki SQL Server dağıtımlarını test ediyor veya öğreniyorsanız, yeni bir kaynak grubu kullanmak faydalıdır. Test işleminizi tamamladıktan sonra, sanal makineyi ve bu kaynak grubu ile ilişkili tüm kaynakları otomatik olarak silmek için kaynak grubunu silin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a Genel Bakış](../../../azure-resource-manager/resource-group-overview.md).


* Altında **örnek ayrıntıları**:
    1. Benzersiz bir girin **sanal makine adı**.  
    1. İçin bir konum seçin, **bölge**. 
    1. Bu kılavuzun amacı doğrultusunda bırakın **kullanılabilirlik seçeneklerini** kümesine _gerekli altyapı artıklık_. Kullanılabilirlik seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Azure bölgeler ve kullanılabilirlik](../../windows/regions-and-availability.md). 
    1. İçinde **görüntü** listesinden _ücretsiz SQL Server Lisansı: Windows Server 2016 üzerinde SQL Server 2017 Developer_.  
    1. Tercih **değiştirme boyutu** için **boyutu** seçin ve sanal makine **A2 temel** teklifidir. Herhangi bir beklenmeyen maliyetleri önlemek için bunları ile işiniz bittiğinde, kaynakları temizlemek emin olun. Üretim iş yükleri için [Azure Virtual Machines'de SQL Server için en iyi performans uygulamaları](virtual-machines-windows-sql-performance.md)’nda önerilen makine boyutlarına ve yapılandırmalara bakın.

    ![Örnek ayrıntıları](media/quickstart-sql-vm-create-portal/basics-instance-details.png)

> [!IMPORTANT]
> **Boyut seçin** penceresinde gösterilen tahmini aylık maliyet, SQL Server lisans maliyetlerini içermez. Bu tahmin yalnızca VM'nin maliyetidir maliyetidir. SQL Server Express ve Developer sürümleri için bu, toplam tahmini maliyeti tahminidir. Diğer sürümler için [Windows Sanal Makineler fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) bakın ve hedef SQL Server sürümünüzü seçin. Ayrıca bkz: [SQL Server Azure Vm'leri için fiyatlandırma Kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md) ve [sanal makine boyutları](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Altında **yönetici hesabı**, bir kullanıcı adı ve parola sağlayın. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../../windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.

   ![Yönetici hesabı](media/quickstart-sql-vm-create-portal/basics-administrator-account.png)

* Altında **gelen bağlantı noktası kuralları**, seçin **Seçili bağlantı noktalarına izin** seçip **RDP (3389)** açılır listeden. 

   ![Gelen bağlantı noktası kuralları](media/quickstart-sql-vm-create-portal/basics-inbound-port-rules.png)


## <a name="2-configure-optional-features"></a>2. İsteğe bağlı özellikleri yapılandırma

### <a name="disks"></a>Diskler

Üzerinde **diskleri** sekmesinde, disk seçenekleri yapılandırın. 

* Altında **işletim sistemi disk türü**, açılır listeden işletim sisteminiz için istediğiniz disk türünü seçin. Premium, üretim sistemleri için önerilir, ancak temel bir sanal makine için kullanılabilir değil. Premium SSD yararlanmak için sanal makine boyutu değiştirin. 
* Altında **Gelişmiş**seçin **Evet** kullanım **yönetilen diskler**.

   > [!NOTE]
   > Microsoft, SQL Server için Yönetilen Diskleri önerir. Yönetilen Diskler, depolama alanını arka planda yönetir. Ayrıca, Yönetilen Disklere sahip sanal makineler aynı kullanılabilirlik kümesinde olduğunda Azure uygun artıklık düzeyini sağlamak için depolama kaynaklarını dağıtır. Daha fazla bilgi için bkz. [Azure yönetilen disklere genel bakış] [… / Yönetilen-disk-overview.md). Bir kullanılabilirlik kümesindeki yönetilen diskler hakkında daha fazla ayrıntı için bkz. [yönetilen diskleri kullan VM'ler için kullanılabilirlik kümesindeki] (.. /Manage-Availability.MD.

![SQL VM Disk ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-disks.png)
  
  
### <a name="networking"></a>Ağ

Üzerinde **ağ** sekmesinde, ağ seçenekleri yapılandırın. 

* Yeni bir **sanal ağ**, ya da mevcut bir vNet için SQL Server VM'nize kullanın. Belirlediğiniz bir **alt** de. 

* Altında **NIC güvenlik grubu**, temel güvenlik grubu veya Gelişmiş güvenlik grubunu seçin. Temel seçeneği, sağlar, SQL Server sanal makinesi için gelen bağlantı noktalarının seçilecek (yapılandırılmışsa aynı değerleri **temel** sekmesi). Gelişmiş seçeneğini belirleyerek, mevcut bir ağ güvenlik grubu seçin veya yeni bir tane oluşturmak sağlar. 

* Ağ ayarları başka değişiklikler yapmak veya varsayılan değerleri koruyun.

![SQL VM ağ ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-networking.png)

#### <a name="monitoring"></a>İzleme

Üzerinde **izleme** sekmesinde, izleme ve Otomatik kapatma yapılandırın. 

* Azure etkinleştirir **önyükleme izleme** VM için belirlenen aynı depolama hesabı ile varsayılan olarak. Bu ayarları burada yanı sıra etkinleştirme değiştirebilirsiniz **işletim sistemi Konuk tanılama**. 
* Etkinleştirebilirsiniz **sistem atanan yönetilen kimlik** ve **otomatik kapatma** de bu sekmedeki. 

![SQL VM yönetimi ayarları](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-management.png)


## <a name="3-configure-sql-server-settings"></a>3. SQL server ayarlarını yapılandırma

Üzerinde **SQL Server ayarları** sekmesi, belirli ayarları ve SQL Server iyileştirmelerini yapılandırın. SQL Server için yapılandırabileceğiniz ayarlar aşağıdakileri içerir:



| Ayar |
| --- |
| [Bağlantı](#connectivity) |
| [Kimlik doğrulaması](#authentication) |
| [Azure Anahtar Kasası Tümleştirmesi](#azure-key-vault-integration) |
| [Depolama yapılandırması](#storage-configuration) |
| [Otomatik Düzeltme Eki Uygulama](#automated-patching) |
| [Otomatik Yedekleme](#automated-backup) |
| [R Services (Gelişmiş analiz)](#r-services-advanced-analytics) |


### <a name="connectivity"></a>Bağlantı

**SQL bağlantısı** altında, bu VM’de SQL Server örneğini istediğiniz erişim türünü belirtin. Bu izlenecek yolda amacı doğrultusunda, seçin **genel (internet)** makineler ve hizmetlerden internet üzerindeki SQL Server bağlantılarına izin verecek şekilde. Bu seçenek ile Azure güvenlik duvarı ve ağ güvenlik grubu, seçilen bağlantı noktası üzerinde trafiğe izin verecek şekilde otomatik olarak yapılandırır.

> [!TIP]
> Varsayılan olarak, SQL Server **1433** gibi iyi bilinen bir bağlantı noktasını dinler. Daha yüksek güvenlik için önceki iletişim kutusunda dinleme bağlantı noktasını 1401 gibi varsayılan olmayan bir bağlantı noktasıyla değiştirin. Bağlantı noktasını değiştirirseniz, bu bağlantı noktasına SSMS gibi bir istemci aracı kullanarak bağlanmanız gerekir.

![SQL VM güvenliği](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-security.png)

İnternet üzerinden SQL Server'a bağlanmak için, sonraki bölümde açıklanan, SQL Server Kimlik Doğrulamasını etkinleştirmeniz gerekir.

İnternet üzerinden Veritabanı Altyapısı’na bağlantıları etkinleştirmek istemiyorsanız, aşağıdaki seçeneklerden birini seçin:

* SQL Server’a Yalnızca VM içinden gelen bağlantılara izin vermek için **Yerel (yalnızca VM dahilinde)** 
* SQL Server’a aynı sanal ağdaki makineler ve hizmetlerden gelen bağlantılara izin vermek için **Özel (Sanal Ağ dahilinde)** 

Genel olarak, senaryonuzun izin verdiği en kısıtlayıcı bağlantıyı seçerek güvenliği geliştirin. Ancak tüm seçenekler Ağ Güvenlik Grubu kuralları ve SQL/Windows Kimlik Doğrulaması üzerinden korumaya alınabilir. VM oluşturulduktan sonra Ağ Güvenlik Grubu’nu düzenleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](virtual-machines-windows-sql-security.md).



### <a name="authentication"></a>Kimlik Doğrulaması

SQL Server Kimlik Doğrulaması gerekiyorsa, **SQL kimlik doğrulaması** altında **Etkinleştir**’e tıklayın.

![SQL Server Kimlik Doğrulaması](./media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

> [!NOTE]
> SQL Server (genel bağlantı seçeneği) internet üzerinden erişmeyi planlıyorsanız, burada SQL kimlik doğrulamasını etkinleştirmeniz gerekir. SQL Server'a genel erişim SQL Kimlik Doğrulaması kullanılması gerektirir.

SQL Server Kimlik Doğrulamasını etkinleştirirseniz, bir **Oturum açma adı** ve **parola** belirtin. Bu oturum açma adı bir SQL Server kimlik doğrulaması oturum açma ve üyesi olarak yapılandırılmış **sysadmin** sabit sunucu rolü. Kimlik Doğrulama Modları hakkında daha fazla bilgi için bkz. [Kimlik Doğrulama Modu Seçme](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

SQL Server Kimlik Doğrulamasını etkinleştirmezseniz, SQL Server örneğine bağlanmak için VM’deki yerel Yönetici hesabını kullanabilirsiniz.

![SQL Server kimlik doğrulaması](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-authentication.png)

### <a name="azure-key-vault-integration"></a>Azure Anahtar Kasası tümleştirme

Şifreleme için güvenlik gizli anahtarlarını depolamak üzere, **Azure anahtar kasası tümleştirme**’ye ve **Etkinleştir**’e tıklayın.

![Azure Anahtar Kasası tümleştirme](media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Aşağıdaki tabloda Azure Anahtar Kasası Tümleştirmeyi yapılandırmak için gereken parametreler listelenmektedir.

| PARAMETRE | AÇIKLAMA | ÖRNEK |
| --- | --- | --- |
| **Anahtar Kasası URL'si** |Anahtar kasası konumu. |https:\//contosokeyvault.vault.azure.net/ |
| **Asıl ad** |Azure Active Directory hizmet asıl adı. Bu ad İstemci Kimliği olarak da bilinir. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Asıl parola** |Azure Active Directory hizmet asıl gizli anahtarı. Bu gizli anahtar İstemci Gizli Anahtarı olarak da bilinir. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Kimlik bilgisi adı** |**Kimlik bilgisi adı**: AKV tümleştirme VM'nin anahtar kasasına erişim sağlayan, SQL Server içinde bir kimlik bilgisi oluşturur. Bu kimlik bilgisi için bir ad seçin. |mycred1 |

Daha fazla bilgi için bkz. [Azure VM’lerde SQL Server için Azure Anahtar Kasası Tümleştirmeyi Yapılandırma](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="storage-configuration"></a>Depolama yapılandırması

Altında **depolama yapılandırması**seçin **Değiştir konfigürasyon** depolama gereksinimlerini belirlemek için.


> [!NOTE]
> VM’nizi standart depolama kullanmak için el ile yapılandırdıysanız, bu seçenek kullanılamaz. Otomatik depolama iyileştirmesi yalnızca Premium Storage için kullanılabilir.

> [!TIP]
> Durak sayısı ve her kaydırıcının üst sınırları seçtiğiniz VM boyutuna bağlıdır. Daha büyük ve daha güçlü bir VM, daha fazla ölçeklendirilebilir.

Gereksinimleri, saniye başına girdi/çıktı işlemleri (IOP), MB/saniyedeki iş ve toplam depolama boyutu olarak belirleyebilirsiniz. Hareketli ölçekleri kullanarak bu değerleri yapılandırın. İş yüküne göre bu depolama ayarlarını değiştirebilirsiniz. Portal, bu gereksinimler temelinde, eklenecek ve yapılandırılacak disk sayısını otomatik olarak hesaplar.

**Depolama iyileştirme seçimi** altında aşağıdaki seçeneklerden birini seçin:

* **Genel**, varsayılan ayardır ve çoğu iş yükünü destekler.
* **İşlem** işleme depolamayı geleneksel veritabanı OLTP iş yükleri için iyileştirir.
* **Veri depolama**, depolamayı çözümleme ve raporlama iş yükleri için iyileştirir.

![SQL VM depolama yapılandırması](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-storage-configuration.png)

### <a name="sql-server-license"></a>SQL Server Lisansı
Yazılım Güvencesi müşterisiyseniz kullanabilir [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) kaynaklardaki kaydedip kendi SQL Server lisansınızı getirebilirsiniz. 

![SQL VM lisansı](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-license.png)

### <a name="automated-patching"></a>Otomatik düzeltme eki uygulama

**Otomatik düzeltme eki uygulama** varsayılan olarak etkindir. Otomatik düzeltme eki uygulama Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını sağlar. Bakım penceresi için haftanın gününü, saati ve süreyi belirtin. Azure düzeltme eki uygulamayı bu bakım penceresinde gerçekleştirir. Bakım penceresi zamanlaması saat için VM yerel saatini kullanır. Azure’un SQL Server’a ve işletim sistemine otomatik olarak düzeltme eki uygulamasını istemiyorsanız tıklayın **Devre dışı**’na tıklayın.  

![SQL VM otomatik düzeltme eki uygulama](media/virtual-machines-windows-portal-sql-server-provision/azure-sqlvm-automated-patching.png)

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Düzeltme Eki Uygulama](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Otomatik yedekleme

**Otomatik yedekleme** altında, tüm veritabanları için otomatik veritabanı yedeklemeyi etkinleştirin. Otomatik yedekleme varsayılan olarak devre dışıdır.

SQL otomatik yedeklemeyi etkinleştirdiğinizde aşağıdaki ayarları yapılandırabilirsiniz:

* Yedeklemeler için elde tutma süresi (gün)
* Yedeklemeler için kullanılacak depolama hesabı
* Yedeklemeler için şifreleme seçeneği ve parola
* Backup sistem veritabanları
* Yedekleme zamanlamasını yapılandırma

Yedeklemeyi şifrelemek için **Etkinleştir**’e tıklayın. Ardından **Parola**’yı belirtin. Azure yedeklemeleri şifrelemek için bir sertifika oluşturur ve bu sertifikayı korumak için belirtilen parolayı kullanır.

Daha fazla bilgi için bkz. [Azure Virtual Machines’de SQL Server için Otomatik Yedekleme](virtual-machines-windows-sql-automated-backup.md).


### <a name="r-services-advanced-analytics"></a>R Services (Gelişmiş analiz)

Etkinleştirme seçeneğine sahip [SQL Server R Services (Gelişmiş analiz)](/sql/advanced-analytics/r/sql-server-r-services/). Bu seçenek, SQL Server 2017 ile Gelişmiş analizi kullanmanıza olanak sağlar. **SQL Server Ayarları** penceresinde **Etkinleştir** seçeneğini belirleyin.


## <a name="4-review--create"></a>4. Gözden geçirme + oluşturma

Üzerinde **gözden + Oluştur** sekmesinde, özeti gözden geçirin ve seçin **Oluştur** SQL Server, kaynak grubunu ve bu VM için belirtilen kaynakları oluşturmak için.

Azure portalından dağıtımı izleyebilirsiniz. Ekranın üst kısmındaki **Bildirimler** düğmesi dağıtımın temel durumunu gösterir.

> [!NOTE]
> Size dağıtım zamanları hakkında bir fikir vermek için,Doğu ABD bölgesinde, varsayılan ayarlarla bir SQL VM dağıttım. Bu test dağıtımının tamamlanması yaklaşık 12 dakika sürdü. Ancak bölgeniz ve seçili ayarlarınıza göre, daha hızlı veya daha yavaş dağıtım süresiyle karşılaşabilirsiniz.

## <a id="remotedesktop"></a> VM’yi Uzak Masaüstü ile açma

Uzak Masaüstü kullanarak SQL Server sanal makinesine bağlanmak için aşağıdaki adımları kullanın:

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

SQL Server sanal makineye bağlandıktan sonra, SQL Server Management Studio'yu başlatabilir ve yerel yönetici kimlik bilgilerinizi kullanarak Windows Kimlik Doğrulamasına bağlanabilirsiniz. SQL Server Kimlik Doğrulamasını etkinleştirdiyseniz, sağlama işlemi sırasında yapılandırdığınız SQL oturum açma adı ve parolasını kullanarak da SQL Kimlik Doğrulamasına bağlanabilirsiniz.

Makineye erişim, gereksinimlerinize göre makineyi ve SQL Server ayarlarını doğrudan değiştirmenize olanak tanır. Örneğin, güvenlik duvarı ayarlarını yapılandırabilir veya SQL Server yapılandırma ayarlarını değiştirebilirsiniz.

## <a id="connect"></a> SQL Server'a uzaktan bağlanma

Bu kılavuzda, seçtiğiniz **genel** sanal makine için erişim ve **SQL Server kimlik doğrulaması**. Bu ayarlar, İnternet üzerinden tüm istemcilerden gelen (doğru SQL oturum açma bilgilerine sahip oldukları varsayılarak) SQL Server bağlantılarına izin verecek şekilde sanal makineyi yapılandırdı.

> [!NOTE]
> Sağlama sırasında Genel’i seçmediyseniz, sağlamadan sonra SQL bağlantı ayarlarınızı portal üzerinden değiştirebilirsiniz. Daha fazla bilgi edinmek için bkz. [SQL bağlantı ayarlarınızı değiştirme](virtual-machines-windows-sql-connect.md#change).

Aşağıdaki bölümlerde, SQL Server sanal makine Örneğinize internet üzerinden bağlanmak gösterilmektedir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

  > [!NOTE]
  > Bu örnek, ortak bağlantı noktası 1433'ü kullanır. Ancak, bu değer, farklı bir bağlantı noktası (örneğin, 1401) SQL Server VM'SİNİN dağıtımı sırasında belirtilmişse değiştirilmesi gerekir. 


## <a name="next-steps"></a>Sonraki adımlar

Azure’da SQL Server'ı kullanma hakkında diğer bilgiler için bkz. [Azure Virtual Machines’de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) ve [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md).