<properties urlDisplayName="Create a virtual machine" pageTitle="Azure'de Windows çalıştıran bir sanal makine oluşturma" metaKeywords="Azure capture image vm, capturing vm" description="Learn to create Windows virtual machine (VM) .in Azure, then log on and attach a data disk" metaCanonical="" services="virtual-machines" documentationCenter="" title="" authors="kathydav, rasquill" solutions="" manager="timlt" editor="tysonn" />

<tags ms.service="virtual-machines" ms.workload="infrastructure-services" ms.tgt_pltfrm="vm-windows" ms.devlang="na" ms.topic="article" ms.date="09/12/2014" ms.author="kathydav" />



# Windows Çalıştıran Sanal Makine Oluşturma #

<div class="dev-center-tutorial-selector sublanding"><a href="/tr-tr/documentation/articles/virtual-machines-windows-tutorial/" title="Azure Portal" class="current">Azure Portalı</a><a href="/tr-tr/documentation/articles/virtual-machines-windows-tutorial-azure-preview/" title="Azure Preview Portal">Azure Önizleme Portalı</a></div>

Bu öğretici, Azure Yönetim Portalı'ndaki Görüntü Galerisi'nden bir Windows Server görüntüsünü örnek olarak kullanarak Windows çalıştıran bir Azure sanal makinesi (VM) oluşturmanın ne kadar kolay olduğunu gösterir. Görüntü Galerisi'nde Windows işletim sistemleri, Linux tabanlı işletim sistemleri ve uygulama görüntüleri dahil çeşitli görüntüler sunulur. 

> [WACOM.NOTE] Bu öğreticiyi tamamlamak için Azure VM'lerinde herhangi bir deneyiminiz olması gerekmez. Ancak, bir Azure hesabınız olması gereklidir. Yalnızca birkaç dakikada ücretsiz deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz. [Azure hesabı oluşturma](http://www.windowsazure.com/tr-tr/develop/php/tutorials/create-a-windows-azure-account/). 

Bu öğretici şunları gösterir:

- [Sanal makineyi oluşturma](#createvirtualmachine)
- [Sanal makineyi oluşturduktan sonra oturum açma](#logon)
- [Yeni sanal makineye veri diski ekleme](#attachdisk)

Daha fazla bilgi almak istiyorsanız bkz. [Sanal Makineler](http://go.microsoft.com/fwlink/p/?LinkID=271224).


## <a id="createvirtualmachine"></a>Sanal makineyi oluşturma ##

Bu bölümde, sanal makine oluşturmak için Yönetim Portalı'ndaki **Galeriden** seçeneğinin nasıl kullanılacağı gösterilmiştir. Bu seçenek, **Hızlı Oluştur** seçeneğinden daha fazla yapılandırma seçeneği sağlar. Örneğin, sanal makineyi bir sanal ağa eklemek istiyorsanız **Galeriden** seçeneğini kullanmanız gerekir.

> [WACOM.NOTE] Galeriden ne kadar ve ne tür görüntüler kullanılabileceği, sahip olduğunuz aboneliğin türüne bağlıdır. Bu öğreticide bir Windows Server görüntüsü kullanılmıştır, ancak bir MSDN aboneliğinde masaüstü görüntüleri dahil kullanabileceğiniz başka görüntüler olabilir. 

> Ayrıca sanal makine oluşturmak, çok makineli uygulama şablonlarının dağıtımını otomatikleştirmek, gelişmiş VM izleme ve tanılama özellikleri kullanmak ve daha fazlası için daha zengin ve özelleştirilebilen [Azure Önizleme Portalı](https://portal.azure.com)'nı deneyebilirsiniz. Bu iki Portal'daki kullanılabilen VM yapılandırma seçenekleri büyük ölçüde kesişir, ancak özdeş değildirler.  

[WACOM.INCLUDE [sanal-makineler-oluştur-WindowsVM](../includes/virtual-machines-create-WindowsVM.md)]

## <a id="logon"></a>Sanal makineyi oluşturduktan sonra oturum açma ##

Bu bölümde, sanal makinenin ayarlarını ve üzerinde çalıştıracağınız uygulamaları yönetebilmek için sanal makinede nasıl oturum açılacağı gösterilmiştir.

[WACOM.INCLUDE [sanal-makineler-oturum-açma-win-server](../includes/virtual-machines-log-on-win-server.md)]

## <a id="attachdisk"></a>Yeni sanal makineye veri diski ekleme ##

Bu bölümde, sanal makineye nasıl boş bir veri diski ekleneceği gösterilmiştir. Boş diskleri ekleme ve aynı zamanda mevcut disklerin nasıl iliştirileceği hakkında daha fazla bilgi için bkz. [Veri Diski Ekleme Öğreticisi](http://www.windowsazure.com/tr-tr/documentation/articles/storage-windows-attach-disk/).

1. Azure [Yönetim Portalı](http://manage.windowsazure.com)'nda oturum açın.

2. **Sanal Makineler**'e tıkladıktan sonra **MyTestVM** sanal makinesini seçin.

	![Select MyTestVM](./media/virtual-machines-windows-tutorial/selectvm.png)
	
3. Önce Hızlı Başlangıç sayfasına ulaşabilirsiniz. Bu durumda, en üstten **Pano**'yu seçin.

	![Select Dashboard](./media/virtual-machines-windows-tutorial/dashboard.png)

4. Komut çubuğunda, **Ekle**'ye tıkladıktan sonra açıldığında **Boş Disk Ekle**'ye tıklayın.

	![Select Attach from the command bar](./media/virtual-machines-windows-tutorial/commandbarattach.png)	

5. **Sanal Makine Adı**, **Depolama Konumu**, **Dosya Adı** ve **Ana Bilgisayar Önbellek Tercihi** sizin için önceden tanımlanmıştır. Tek yapmanız gereken diskin olmasını istediğiniz boyutunu girmektir. **Boyut** alanında **5** yazın. Sonra boş diski sanal makineye eklemek için onay işaretine tıklayın.

	>[WACOM.NOTE] Azure'de disk görüntülerinin Azure depolamada sayfa blob'ları olarak depolandığının bilinmesi önemlidir. Azure'un dışında, sanal sabit diskler VHD veya VHDX biçimlerinden birini kullanabilir. Bunlar sabit, dinamik olarak genişleyen veya fark kaydı yapan özelliklerde olabilir. Azure VHD biçimini, sabit diskleri destekler. Sabit biçim, mantıksal diski dosya içinde doğrusal olarak düzenlediğinden, disk farkı X'in içeriği blob farkı X konumunda depolanır. Blob'un sonundaki küçük bir alt bilgi VHD'nin özelliklerini tanımlar. Çoğunlukla, sabit biçim alanı israf eder, çünkü çoğu diskte kullanılmayan büyük aralıklar vardır. Ancak, Azure .vhd dosyalarını seyrek biçimde depoladığından aynı anda hem sabit hem de dinamik disklerin avantajlarından yararlanırsınız. Bu konu hakkında daha fazla bilgiyi [Azure'de VHD'ler hakkında](http://msdn.microsoft.com/tr-tr/library/azure/dn790344.aspx) konusunda okuyabilirsiniz 


	![Specify the size of the empty disk](./media/virtual-machines-windows-tutorial/emptydisksize.png)	
	
	>[WACOM.NOTE] Tüm diskler Windows Azure depolamadaki bir VHD dosyasından oluşturulur. **Dosya Adı** altında, depolamaya eklenen VHD dosyasının adını girebilirsiniz, ancak Azure diskin adını otomatik olarak oluşturur. 

6. Boş veri diskinin sanal makineye başarıyla eklendiğini doğrulamak için panoya geri dönün. İşletim sistemi Diski ile birlikte **Diskler** listesinde ikinci bir disk olarak listelenecektir.

	![Attach empty disk](./media/virtual-machines-windows-tutorial/disklistwithdatadisk.png)

	Veri diskini sanal makineye ekledikten sonra, disk çevrimdışı ve başlatılmamıştır. Veri depolamak amacıyla bu diski kullanabilmeniz için sanal makinede oturum açmanız ve diski başlatmanız gerekir.

7. [Sanal makineyi oluşturduktan sonra oturum açma] (#logon) başlıklı önceki bölümdeki adımları kullanarak sanal makineye bağlanın.

8. Sanal makinede oturum açtıktan sonra, **Sunucu Yöneticisi**'ni açın. Sol bölmede, **Dosya ve Depolama Hizmetleri**'ni seçin.

	![Expand File and Storage Services in Server Manager](./media/virtual-machines-windows-tutorial/fileandstorageservices.png)

9. Genişletilmiş menüden **Diskler**'i seçin.

	![Expand File and Storage Services in Server Manager](./media/virtual-machines-windows-tutorial/selectdisks.png)	
	
10. **Diskler** bölümünde, listede üç disk vardır: disk 0, disk 1 ve disk 2. Disk 0 işletim sistemi diskidir, disk 1 geçici kaynak diskidir (veri depolama için kullanılmamalıdır) ve disk 2 sanal makineye eklediğiniz veri diskidir. Veri diskinin daha önce belirtildiği gibi 5 GB kapasitesi olduğuna dikkat edin. Disk 2'ye sağ tıkladıktan sonra **Başlat**'ı seçin.

	![Start initialization](./media/virtual-machines-windows-tutorial/initializedisk.png)

11. Başlatma işlemini başlatmak için **Evet**'e tıklayın.

	![Continue initialization](./media/virtual-machines-windows-tutorial/yesinitialize.png)

12. Disk 2'ye yeniden sağ tıklayın ve **Yeni Birim**'i seçin. 

	![Create the volume](./media/virtual-machines-windows-tutorial/initializediskvolume.png)

13. Sağlanan varsayılan değerleri kullanarak sihirbazı tamamlayın. Sihirbaz bittiğinde, yeni birim **Birimler** bölümünde listelenir. 

	![Create the volume](./media/virtual-machines-windows-tutorial/newvolumecreated.png)

	Disk şimdi çevrimiçi ve yeni sürücü harfiyle kullanmaya hazırdır. 
	
## Sonraki Adımlar 

Azure'de Windows sanal makinelerini yapılandırma hakkında daha fazla bilgi edinmek için şu makalelere bakın:

[Bulut Hizmetinde Sanal Makinelere Bağlanma](http://www.windowsazure.com/tr-tr/documentation/articles/cloud-services-connect-virtual-machine/)

[Windows Server İşletim Sistemini İçeren Kendi Sanal Sabit Diskinizi Oluşturma ve Yükleme](http://www.windowsazure.com/tr-tr/documentation/articles/virtual-machines-create-upload-vhd-windows-server/)

[Sanal Makinelerin Kullanılabilirliğini Yönetme](http://www.windowsazure.com/tr-tr/documentation/articles/manage-availability-virtual-machines/)

[Azure VM yapılandırma ayarları hakkında](http://msdn.microsoft.com/library/azure/dn763935.aspx)

[VİDEO: VHD'leri Kullanmaya Başlama - Gerçekte Neler Oluyor](http://azure.microsoft.com/tr-tr/documentation/videos/getting-started-with-azure-virtual-machines)

[VİDEO: Mark Russinovich ile SSS - Windows Azure Windows'u Çalıştırıyor mu?](http://azure.microsoft.com/tr-tr/documentation/videos/mark-russinovich-windows-on-azure)

[VİDEO: Yeniden kullanılabilen görüntüler yaparak Web Grubuna yeni sanal makine ekleme ](http://azure.microsoft.com/tr-tr/documentation/videos/adding-virtual-machines-web-farm)

[VİDEO: Sanal Sabit Sürücüler, Depolama Hesapları Ekleme ve Sanal Makineleri Ölçeklendirme](http://azure.microsoft.com/tr-tr/documentation/videos/adding-drives-scaling-virtual-machines)

[VİDEO: Scott Guthrie Sanal Makineleri Başlatıyor](http://azure.microsoft.com/tr-tr/documentation/videos/virtual-machines-scottgu)

[VİDEO: Azure Sanal Makinelerinde Depolama ve Diskin Temelleri](http://azure.microsoft.com/tr-tr/documentation/videos/storage-and-disks-virtual-machines)



[About virtual machines in Azure]: #virtualmachine
[How to create the virtual machine]: #custommachine
[How to log on to the virtual machine after you create it]: #logon
[How to attach a data disk to the new virtual machine]: #attachdisk
[How to set up communication with		 the virtual machine]: #endpoints
