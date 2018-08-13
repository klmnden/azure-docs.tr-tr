---
title: Azure Marketi için bir sanal makine görüntüsü oluşturma | Microsoft Docs
description: Satın almak Azure Marketi başkaları için bir sanal makine görüntüsünün nasıl oluşturulacağı hakkında ayrıntılı yönergeler.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: hascipio
editor: ''
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: hascipio; v-divte
ms.openlocfilehash: 9199c9fc9a46e6b09eb066be5125c74420ad6cd6
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715344"
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Azure Market sanal makine görüntüsü oluşturma Kılavuzu
Bu makalede **2. adım**, sanal sabit Azure Marketi'nde dağıtacağınız diskleri (VHD) hazırlama konusunda size yol gösterir. Vhd'lerinizi sku'nuzun temelidir. İşlemi, bir Linux veya Windows tabanlı SKU kullanmanıza bağlı olarak farklılık gösterir. Bu makalede her iki senaryoyu da kapsamaktadır. Bu işlem ile paralel olarak gerçekleştirilebilir [hesap oluşturma ve kayıt][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Teklif ve SKU'ları tanımlama
Bu bölümde teklifler ile bunların ilişkili SKU'ları tanımlamayı öğrenin.

Teklif, tüm SKU'larının "üst öğesidir". Birden çok teklife sahip olabilirsiniz. Tekliflerinizi nasıl yapılandıracağınıza siz karar verirsiniz. Bir teklif, hazırlamaya gönderilirken tüm SKU'larıyla birlikte gönderilir. URL'de olacağından, SKU tanımlayıcıları dikkatlice düşünün:

* Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}
* Azure Önizleme portalı: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}  

Bir SKU VM görüntüsünün ticari unvanıdır. Bir işletim sistemi diski ile sıfır, bir VM görüntüsü içerir veya daha fazla veri diski. Özünde, bir sanal makinenin tam depolama profilidir. Disk başına bir VHD gereklidir. Oluşturulacak bir VHD bile boş veri diskleri gerektirir.

Kullandığınız işletim sisteminden bağımsız olarak yalnızca SKU için gereken en az sayıda veri diski ekleyin. Müşteriler dağıtım sırasında bir görüntünün parçası olan diskler kaldıramazsınız ancak gerekmesi durumunda her zaman diskleri süresince veya dağıtım sonrasında ekleyebilirsiniz.

> [!IMPORTANT]
> **Yeni bir görüntü sürümü sayısı disk değiştirmeyin.** Görüntüde veri diskleri yeniden yapılandırmanız gerekir, yeni bir SKU'ya tanımlayın. Farklı disk sayısı olan yeni bir görüntü sürüm yayımlama bozucu yeni dağıtımının olası çözümleri ARM şablonları ve diğer senaryolar ile otomatik ölçeklendirme, otomatik dağıtım durumlarda yeni görüntü sürümü temel.
>
>

### <a name="11-add-an-offer"></a>1.1 teklif ekleme
1. Oturum [yayımlama portalı] [ link-pubportal] satıcı hesabınız kullanarak.
2. Seçin **sanal makineler** Yayımlama Portalı'nın sekmesi. İstenen girdi alanına teklif adınızı girin. Teklif adı genellikle ürün ya da Azure Market'te satılsın planladığınız hizmet adıdır.
3. **Oluştur**’u seçin.

### <a name="12-define-a-sku"></a>1.2 bir SKU tanımlama
Bir teklif ekledikten sonra SKU'larınız tanımlayabilir ve gerekir. Birden çok Teklife sahip olabilirsiniz ve her bir teklifin birden çok SKU'ları altındaki olabilir. Bir teklif, hazırlamaya gönderilirken tüm SKU'larıyla birlikte gönderilir.

1. **Bir SKU ekleyin.** URL'de kullanılan tanımlayıcı, SKU gerektirir. Tanımlayıcı yayımlama profiliniz dahilinde benzersiz olmalıdır, ancak tanımlayıcı çakışması diğer yayımcılar olma riski yoktur.

   > [!NOTE]
   > Teklif ve SKU tanımlayıcıları Marketi'nde teklif URL görüntülenir.
   >
   >
2. **SKU'nuz için bir Özet açıklaması ekleyin.** Özet açıklamaları, müşterilere görünür olduğundan kolay okunabilir yapmanız. Bu bilgiler kadar "Hazırlamaya Gönder aşamasına" aşaması kilitlenmesi gerekmez. Bu aşamaya kadar bilgileri istediğiniz zaman düzenleyebilirsiniz.
3. Windows tabanlı SKU'lar kullanıyorsanız Windows Server'ın onaylanmış sürümlerini edinmek için önerilen bağlantıları izleyin.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. Azure ile uyumlu VHD'nizi (Linux tabanlı) oluşturma
Bu bölümde, Azure Market'te Linux tabanlı VM oluşturmaya yönelik en iyi uygulamalar ele alınmaktadır. Adım adım bir kılavuz için şu belgeye başvurun: [özel bir Linux VM görüntüsü oluşturma](../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. (Windows tabanlı), Azure ile uyumlu bir VHD oluşturun
Bu bölümde Azure Market'te Windows Server tabanlı bir SKU oluşturma adımları ele alınmaktadır.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1 doğru temel VHD'leri kullandığınızdan emin olun
VM görüntünüzün işletim sistemi VHD'si, Windows Server veya SQL Server içeren bir Azure-onaylı bir temel görüntüye dayalı olması gerekir.

Başlamak için aşağıdaki görüntülerden birinden bir VM oluşturma bulunan [Microsoft Azure Portal'da][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])
* SQL Server 2014 ([Kurumsal][link-sql-2014-ent], [standart][link-sql-2014-std], [Web] [ link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])

Bu bağlantılar SKU sayfasındaki Yayımlama Portalı'nda da bulunabilir.

> [!TIP]
> Geçerli Azure portalını veya PowerShell'i kullanıyorsanız, 8 Eylül 2014'te yayımlanan ve sonraki Windows Server görüntüleri onaylanır.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2 Windows tabanlı VM oluşturma
Microsoft Azure Portalı'ndan, yalnızca birkaç basit adımda onaylı bir temel görüntüye dayalı VM'nizi oluşturabilirsiniz. İşlemine genel bakış aşağıda verilmiştir:

1. Temel görüntü sayfasında seçin **sanal makine oluştur** yeni yönlendirilmesine [Microsoft Azure Portal'da][link-azure-portal].

    ![Çizim][img-acom-1]
2. Portal kullanmak istediğiniz Azure aboneliği için parola ve Microsoft hesabı ile oturum açın.
3. Seçtiğiniz temel görüntüyü kullanarak bir VM oluşturmak için istemleri izleyin. VM için bir konak adı (bilgisayarın adı), (yönetici olarak kaydedilen) kullanıcı adı ve parola sağlamanız gerekir.

    ![Çizim][img-portal-vm-create]
4. Dağıtılacak VM'nin boyutunu seçin:

    a.    VHD'yi şirket içinde geliştirmeyi planlıyorsanız boyut önemli değildir. En küçük VM'lerden birini kullanabilirsiniz.

    b.    Görüntüyü Azure'da geliştirmeyi planlıyorsanız, seçilen görüntü için önerilen VM boyutlarından birini kullanın.

    c.    Fiyatlandırma bilgileri için başvurmak **önerilen fiyatlandırma katmanları** portalda görüntülenen Seçici. Yayımcı tarafından sağlanan üç önerilen boyut burada belirtilmiştir. (Burada, yayımcı Microsoft'tur.)

    ![Çizim][img-portal-vm-size]
5. Özellikleri ayarlayın:

    a.    Hızlı dağıtım için bölümünde özellikler için varsayılan değerleri bırakın **isteğe bağlı yapılandırma** ve **kaynak grubu**.

    b.    Altında **depolama hesabı**, isteğe bağlı olarak, işletim sistemi VHD'si depolanacağı depolama hesabı seçebilirsiniz.

    c.    Altında **kaynak grubu**, isteğe bağlı olarak, VM'nin yerleştirileceği mantıksal grubu seçebilirsiniz.
6. Seçin **konumu** dağıtımı için:

    a.    VHD'yi şirket içinde geliştirmeyi planlıyorsanız, görüntüyü daha sonra Azure'a yükler için konum önemli değildir.

    b.    Görüntüyü Azure'da geliştirmeyi planlıyorsanız, başlangıçta ABD tabanlı Microsoft Azure bölgelerinden birini kullanın. Bu, görüntünüzü onaylanmak gönderdiğinizde, Microsoft sizin adınıza gerçekleştirdiği VHD kopyalama işlemini hızlandırır.

    ![Çizim][img-portal-vm-location]
7. **Oluştur**’a tıklayın. VM dağıtmaya başlar. Dağıtımınız birkaç dakika içinde başarıyla tamamlanır ve SKU'nuz için görüntü oluşturmaya başlayabilirsiniz.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3 VHD'nizi bulutta geliştirme
Uzak Masaüstü Protokolü (RDP) kullanarak VHD'nizi bulutta geliştirmeniz kesinlikle önerilir. RDP için kullanıcı adı ve parola sağlama sırasında belirtilen bağlanın.

> [!IMPORTANT]
> **Yönetilen diskler kullanmayın.** Bulut VHD'ye geliştirmek için kullanılan sanal makineyi bir görüntüden oluşturulması şu anda desteklemiyor gibi yönetilen disklerde esas gerekir.
> Yönetilen diskler için varsayılan isteğe bağlı özellik değişikliği sanal makine oluşturuluyor.

> (Bu önerilmez) şirket içinde VHD'nizi geliştirmeniz olup [şirket içi bir sanal makine görüntüsü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md). VHD'nizi indirmeniz bulutta geliştiriyorsanız gerekli değildir.
>
>

**Kullanarak RDP aracılığıyla bağlanma [Microsoft Azure portal][link-azure-portal]**

1. Seçin **tüm hizmetleri** > **Vm'leri**.
2. Sanal makineler dikey penceresi açılır. Bağlanmak istediğiniz VM'nin çalıştığından ve dağıtılan VM'ler listesinden seçin emin olun.
3. Seçilen VM'yi tanımlayan bir dikey pencere açılır. En üstünde tıklayın **Connect**.
4. Kullanıcı adı ve sağlama sırasında belirttiğiniz parolayı girmeniz istenir.

**PowerShell kullanarak RDP aracılığıyla bağlanma**

Bir yerel makineye Uzak Masaüstü dosyası indirmek için kullanın [Get-AzureRemoteDesktopFile cmdlet][link-technet-2]. Bu cmdlet'i kullanmak için VM adını ve hizmet adını bilmeniz gerekir. Sanal makineden oluşturduysanız [Microsoft Azure Portal'da][link-azure-portal], bu bilgiyi VM özellikleri bölümünde bulabilirsiniz:

1. Microsoft Azure portalında **tüm hizmetleri** > **Vm'leri**.
2. Sanal makineler dikey penceresi açılır. Dağıttığınız VM'yi seçin.
3. Seçilen VM'yi tanımlayan bir dikey pencere açılır.
4. **Özellikler**'e tıklayın.
5. Etki alanı adının ilk bölümü hizmet adıdır. Ana bilgisayar adı VM adıdır.

    ![Çizim][img-portal-vm-rdp]
6. Yöneticinin yerel masaüstüne VM'nin RDP dosyasını indirmek için cmdlet aşağıdaki gibidir.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

MSDN makalesinde RDP hakkında daha fazla bilgi bulunabilir [Azure VM'de RDP veya SSH ile bağlanma](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Bir VM yapılandırma ve NIZU oluşturma**

İşletim sistemi vhd'si oluşturulduktan sonra HyperV kullanın ve SKU'nuzu oluşturmaya başlamak için bir VM yapılandırın. Ayrıntılı adımlar şu TechNet bağlantısında bulunabilir bulunabilir: [HyperV yükleyin ve bir VM yapılandırma](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 doğru VHD boyutunu seçme
VM görüntünüzdeki Windows işletim sistemi VHD'si 128 GB sabit biçimli VHD oluşturulmalıdır.  

Fiziksel boyut 128 GB'den küçükse, VHD seyrek olmalıdır. Sağlanan temel Windows ve SQL Server görüntüleri, zaten bu gereksinimleri karşılayan, bu nedenle biçimi veya alınan VHD'nin boyutunu değiştirmeyin.  

Veri diskleri en fazla 1 TB büyüklüğünde olabilir. Disk boyutuna karar verirken, müşteriler dağıtım sırasında bir görüntü içindeki VHD'leri boyutlandıramayacağını unutmayın. Veri diski VHD'leri sabit biçimli VHD oluşturulmalıdır. Bunlar aynı zamanda seyrek olmalıdır. Veri diskleri boş olabilir veya veri içerebilir.

### <a name="35-install-the-latest-windows-patches"></a>3.5 en son Windows düzeltme eklerini yükleyin
Temel görüntüler, yayımlanma tarihlerine göre en son düzeltmeleri içerir. İşletim sistemi VHD'si, oluşturduğunuz yayımlamadan önce Windows Update'in çalıştırıldığından ve tüm kritik ve önemli güvenlik güncelleştirmeleri yüklediğinizden emin olun.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 gerektiği gibi ek yapılandırma ve zamanlama görevleri gerçekleştirme
Ek yapılandırma gerekirse, oluşturulduktan sonra VM'ye son değişiklikleri yapmak için başlangıçta çalışan bir zamanlanmış görev kullanarak göz önünde bulundurun:

* Görevin başarıyla yürütüldükten sonra kendini silmesi, kullanılabilecek en iyi yöntemdir.
* Bunlar her zaman mevcut için yalnızca iki sürücüleri olduğundan yapılandırma C ya da D, sürücüler dışında sürücü yararlanmalıdır. C sürücüsünün işletim sistemi diskidir ve D sürücüsünü geçici yerel disktir.

### <a name="37-generalize-the-image"></a>3.7 görüntüyü Genelleştirme
Azure Marketi'ndaki tüm görüntüler genel bir şekilde yeniden kullanılabilir olmalıdır. Diğer bir deyişle, işletim sistemi VHD'si genelleştirilmiş olmalıdır:

* Windows için görüntü "Sysprep uygulanmış" olmalı ve hiçbir yapılandırmaları desteklemeyen yapılmalıdır **sysprep** komutu.
* Dizin % windir%\System32\Sysprep aşağıdaki komutu çalıştırabilirsiniz.

        sysprep.exe /generalize /oobe /shutdown

  Sysprep için şu MSDN makalesine adımda işletim sistemini nasıl sağlandığını Kılavuzu: [oluşturup yükleme azure'a bir Windows Server VHD](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Vhd'lerinizden VM dağıtma
Vhd'lerinizi (genelleştirilmiş işletim sistemi VHD'si ve sıfır veya daha fazla veri diski) bir Azure depolama hesabına yükledikten sonra bir kullanıcı VM görüntüsü olarak kaydedebilirsiniz. Ardından bu görüntüyü test edebilirsiniz. İşletim sistemi VHD'si genelleştirilmiş olduğundan, doğrudan VM VHD URL'sini sağlayarak dağıtamayacağınızı unutmayın.

VM görüntüleri hakkında daha fazla bilgi edinmek için şu blog gönderilerini inceleyin:

* [VM görüntüsü](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [VM görüntüsü PowerShell nasıl yapılır](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Azure'da VM görüntüleri hakkında](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a>Gerekli araçlara, PowerShell ve Azure CLI'yı ayarlama
* [PowerShell ayarlama](/powershell/azure/overview)
* [Azure CLI'yı ayarlama](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 bir kullanıcı VM görüntüsü oluşturma
#### <a name="capture-vm"></a>VM yakalama
Lütfen API/PowerShell/Azure CLI kullanarak VM yakalama hakkında rehberlik için aşağıda verilen bağlantıları okuyun.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Görüntüyü Genelleştirme
Lütfen API/PowerShell/Azure CLI kullanarak VM yakalama hakkında rehberlik için aşağıda verilen bağlantıları okuyun.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 kullanıcı VM görüntüsünden VM dağıtma
Bir kullanıcı VM görüntüsünden VM dağıtmak için geçerli kullanabilirsiniz [Azure portalında](https://manage.windowsazure.com) veya PowerShell.

**Geçerli Azure portalından bir VM dağıtma**

1. Git **yeni** > **işlem** > **sanal makine** > **galerisinden**.

2. Git **görüntülerim**ve ardından sanal makine görüntüsünden VM dağıtmak için seçin:

   1. Hangi görüntüyü seçerseniz seçin, çünkü dikkat **görüntülerim** görünüm, işletim sistemi görüntüleri hem VM görüntüleri listeler.
   2. Disk sayısına bakmak hangi türde görüntü dağıttığınız, VM görüntülerinin çoğu birden fazla disk bulunduğundan'ı belirlemeye yardımcı olabilir. Ancak, ardından sahip yalnızca bir tek işletim sistemi diski, bir VM görüntüsü yine de mümkündür **disk sayısı** 1 olarak ayarlayın.

      ![Çizim][img-manage-vm-select]
3. Sanal Makine Oluşturma Sihirbazı'nı izleyin ve VM adı, VM boyutunu, konumu, kullanıcı adı ve parola belirtin.

**Powershell'den VM dağıtma**

Yeni oluşturulan genelleştirilmiş VM görüntüsünden büyük bir VM'yi dağıtmak için aşağıdaki cmdlet'leri kullanabilirsiniz.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Lütfen ek Yardım almak için [VHD oluşturma sırasında karşılaşılan yaygın sorunları giderme] bakın.
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. VM görüntünüz için sertifika alın
Azure Market, VM görüntüsü oluşturma sürecinde sonraki adım, sertifika alma olmasını sağlamaktır.

Bu işlem, özel bir sertifika aracı çalıştırma, doğrulama sonuçlarını Vhd'lerinizin bulunduğu Azure kapsayıcısında yükleme, teklif ekleme, SKU'nuzu tanımlama ve VM görüntünüzü sertifika için gönderme içerir.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 indirin ve Azure sertifikası için sertifika Test aracı çalıştırın
Sertifika aracı, VM görüntüsünün Microsoft Azure ile uyumlu olduğundan emin olmak için kullanıcı VM görüntüsünden çalışan bir VM'de üzerinde çalışır. Bu araç, VHD'nizi hazırlamaya yönelik yönerge ve gereksinimlerin karşılandığını doğrular. Aracın çıktısı istekte bulunan sertifika sırasında yayımlama portalında yüklenmelidir bir uyumluluk raporudur.

Sertifika aracı hem Windows hem de Linux Vm'leri ile kullanılabilir. Bu, Windows tabanlı Vm'lerine PowerShell aracılığıyla bağlanır ve Linux Vm'lerine SSH.Net aracılığıyla bağlanır:

1. Sertifika aracı indirmeniz [Microsoft İndirme sitesi][link-msft-download].
2. Sertifika Aracı'nı açın ve ardından **teni Test Başlat** düğmesi.
3. Gelen **Test bilgileri** ekranında, test çalıştırması için bir ad girin.
4. VM'nizin Linux üzerinde veya Windows üzerinde çalıştığına ilişkin seçim yapın. Seçiminize bağlı olarak sonraki seçenekleri belirleyin.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Sertifika Aracı'nı bir Linux VM görüntüsüne bağlama**
1. SSH kimlik doğrulama modunu seçin: Parola veya anahtar dosyası.
2. Parola tabanlı kimlik doğrulamasını kullanıyorsanız, etki alanı adı sistemi (DNS) adını, kullanıcı adı ve parola girin.
3. Anahtar dosyası kimlik doğrulama kullanılıyorsa DNS adını, kullanıcı adı ve özel anahtar konumunu girin.

   ![Linux VM görüntüsü parola kimlik doğrulaması][img-cert-vm-pswd-lnx]

   ![Linux VM görüntüsü anahtar dosyası kimlik doğrulaması][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Sertifika aracı bir Windows tabanlı VM görüntüsüne bağlama**
1. Tam VM DNS adını (örneğin, MyVMName.Cloudapp.net) girin.
2. Kullanıcı adını ve parolasını girin.

   ![Windows VM görüntüsünün parola kimlik doğrulaması][img-cert-vm-pswd-win]

Linux veya Windows tabanlı VM görüntünüz için doğru seçenekleri seçtikten sonra seçin **Test Bağlantısı** SSH.Net veya PowerShell'in test için geçerli bir bağlantıya sahip olmak. Bağlantı kurulduktan sonra seçip **sonraki** testi başlatmak için.

Test tamamlandığında her bir test öğesine yönelik sonuçları (Başarılı/Başarısız/Uyarı) alırsınız.

![Linux VM görüntüsü için test çalışmaları][img-cert-vm-test-lnx]

![Windows VM görüntüsü için test çalışmaları][img-cert-vm-test-win]

Testler başarısız olursa, görüntünüzü sertifikalı değil. Bu meydana gelirse, gereksinimleri gözden geçirin ve gerekli değişiklikleri yapın.

Otomatik test sonra VM görüntünüzdeki anketi ekran aracılığıyla ek giriş sağlamaları istenir.  Soruları tamamlayın ve ardından **sonraki**.

![Sertifika aracı anketi][img-cert-vm-questionnaire]

![Sertifika aracı anketi][img-cert-vm-questionnaire-2]

Anketi tamamladıktan sonra görüntü ve başarısız tüm değerlendirmesi için bir açıklama Linux VM için SSH erişimi bilgiler gibi ek bilgiler sağlayabilir. Yanıtlarınızı soru için ek günlük dosyaları yürütülen test çalışmalarını ve test sonuçlarını indirebilirsiniz. Vhd'lerinizi ile aynı kapsayıcıda sonuçları kaydedin.

![Sertifika, test sonuçları Kaydet][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 VM görüntüleriniz için paylaşılan erişim imzası URI'si Al
Yayımlama işlemi sırasında SKU'nuz için oluşturduğunuz VHD'leri her müşteri adayı Tekdüzen Kaynak Tanımlayıcıları (URI'lar) belirtin. Sertifika işlemi sırasında Microsoft'un bu VHD'lere erişmesi gerekir. Bu nedenle, her VHD için bir paylaşılan erişim imzası URI'si oluşturmanız gerekir. Bu, girilmesi URI'dir **görüntüleri** Yayımlama Portalı'nda sekmesi.

Paylaşılan erişim imzası URI'si oluşturulan aşağıdaki gereksinimlere uymalıdır:

Not: Aşağıdaki yönergelerde desteklenen tek tür olan yönetilmeyen diskler için geçerlidir.

* Paylaşılan erişim imzası, VHD için bir URI'leri oluştururken liste ve Okuma izinleri yeterli olur. Yazma veya Silme erişimi sağlamayın.
* Paylaşılan erişim imzası URI'si oluşturulduğunda erişim süresi üç (3) hafta arasından en az olmalıdır.
* UTC saati için korumak için geçerli tarihten bir gün seçin. Örneğin, geçerli tarihi 6 Ekim 2014 ise 10/5/2014'ı seçin.

SAS URL'si, Azure Market'te VHD'nizi paylaşmak için birden çok yolla oluşturulabilir.
3 önerilen araçlar şunlardır:

1.  Azure Depolama Gezgini
2.  Microsoft Depolama Gezgini
3.  Azure CLI

**Azure Depolama Gezgini'ni (Windows kullanıcıları için önerilir)**

Azure Depolama Gezgini'ni kullanarak SAS URL'si oluşturmak için adımları aşağıda verilmiştir

1. İndirme [Azure Depolama Gezgini 6 Preview 3](https://azurestorageexplorer.codeplex.com/) codeplex'ten. Git [Azure Depolama Gezgini 6 önizlemesi](https://azurestorageexplorer.codeplex.com/) tıklatıp **"İndirir"**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. İndirme [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) ve sıkıştırması açılıyor sonra yükleyin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Yüklendikten sonra uygulamayı açın.
4. Tıklayın **Hesap Ekle**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Depolama hesabı adı, depolama hesabı anahtarı ve depolama uç noktaları etki alanı belirtin. Burada Azure Portal'da VHD'nizi tutmuş depolama hesabı, Azure aboneliğinizde budur.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Azure Depolama Gezgini, belirli depolama hesabınıza bağlandıktan sonra tüm gösteren başlayacak içinde depolama hesabı içerir. İşletim sistemi disk VHD dosyası (senaryonuz için uygun olmaları durumunda da veri diskleri) kopyaladığınız kapsayıcıyı seçin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Blob kapsayıcısı seçtikten sonra Azure Depolama Gezgini kapsayıcı içindeki dosyaların gösteren başlatır. Gönderilmesi gereken görüntü dosyası (.vhd) seçin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Kapsayıcıda .vhd dosyasını seçtikten sonra **güvenlik** sekmesi.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  İçinde **Blob kapsayıcı güvenliği** iletişim kutusunda, varsayılan değerleri açık bırak **erişim düzeyi** sekmesine ve ardından **paylaşılan erişim imzaları** sekmesi.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. .Vhd görüntüsü için paylaşılan erişim imzası URI'si oluşturmak için aşağıdaki adımları izleyin:

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Erişime izin öğesinden:** UTC saati için korumak için geçerli tarihten bir gün seçin. Örneğin, geçerli tarihi 6 Ekim 2014 ise 10/5/2014'ı seçin.

    b. **Erişim izin verilir:** 3 hafta sonra en az bir tarihi seçin **erişimine izin gelen** tarih.

    c. **İzin verilen eylemleri:** seçin **listesi** ve **okuma** izinleri.

    d. .Vhd dosyanızı doğru seçtiğiniz sonra dosyanızı görünür **erişmek için Blob adı** .vhd uzantısı ile.

    e. Tıklayın **imzayı üretmek**.

    f. İçinde **oluşturulan paylaşılan erişim imzası URI'si bu kapsayıcının**, yukarıdaki onay vurgulanmış olarak aşağıdakiler:

       - Görüntü dosya adı olduğundan emin olun ve **".vhd"** URI'de olan.
       - İmza sonunda emin **"rl ="** görünür. Bu, okuma ve liste erişim başarıyla sağlandığını gösterir.
       - İmza ortadaki emin **"sr = c"** görünür. Bu kapsayıcı düzeyinde erişimi olduğunu gösterir.

11. Oluşturulan erişim imzası URI'si works paylaşılan emin olmak için tıklayın **tarayıcıda Test**. İndirme işlemini başlamalıdır.

12. Paylaşılan erişim imzası URI'si kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

13. Sku'daki her VHD için 6-10 adımları yineleyin.

**Microsoft Azure Depolama Gezgini'ni (Windows/MAC/Linux)**

Microsoft Azure Depolama Gezgini'ni kullanarak SAS URL'si oluşturmak için adımları aşağıda verilmiştir

1.  Microsoft Azure Depolama Gezgini form indirme [ http://storageexplorer.com/ ](http://storageexplorer.com/) Web sitesi. Git [Microsoft Azure Depolama Gezgini](http://storageexplorer.com/releasenotes.html) tıklatıp **"Windows için indirin"**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Yüklendikten sonra uygulamayı açın.

3.  Tıklayın **Hesap Ekle**.

4.  Microsoft Azure Depolama Gezgini'ni hesabınızda oturum açma aboneliğinize yapılandırın

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Depolama hesabına gidin ve kapsayıcısı seçin

6.  Seçin **"Paylaşım erişim imzası Al …"** Sağ tıklayarak kullanarak **kapsayıcı**

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Başlangıç zamanı, süre sonu ve izinler, aşağıdaki gibi başına güncelleştirme

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Başlangıç zamanı:** UTC saati için korumak için geçerli tarihten bir gün seçin. Örneğin, geçerli tarihi 6 Ekim 2014 ise 10/5/2014'ı seçin.

    b.  **Süre sonu:** 3 hafta sonra en az bir tarihi seçin **başlattığınızda** tarih.

    c.  **İzinler:** seçin **listesi** ve **okuma** izinleri

8.  Kapsayıcı paylaşılan erişim imzası URI'si kopyalayın

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Oluşturulan SAS URL'si için düzeyi kapsayıcıdır ve artık VHD adı eklemek gerekiyor.

    Kapsayıcı düzeyi SAS URL'SİNİN biçimi: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Aşağıda gösterildiği gibi SAS URL kapsayıcı adından sonra VHD adını Ekle `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Örnek:

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    VHD adı TestRGVM201631920152.vhd yapılır ve ardından VHD SAS URL'si olacaktır `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Görüntü dosya adı olduğundan emin olun ve **".vhd"** URI'de olan.
    - İmza ortadaki emin **"sp rl ="** görünür. Bu, okuma ve liste erişim başarıyla sağlandığını gösterir.
    - İmza ortadaki emin **"sr = c"** görünür. Bu kapsayıcı düzeyinde erişimi olduğunu gösterir.

9.  Oluşturulan erişim imzası URI'si works paylaşılan emin olmak için tarayıcıda test edin. Yükleme işlemini başlatmak

10. Paylaşılan erişim imzası URI'si kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

11. SKU'daki her VHD için bu adımları tekrarlayın.

**Azure CLI'yı (Windows dışı ve sürekli tümleştirme için önerilir)**

Azure CLI kullanarak bir SAS URL'si oluşturmak için adımları aşağıda verilmiştir

1.  Microsoft Azure CLI'dan indirme [burada](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). İçin farklı bağlantıları da bulabilirsiniz ** [Windows](http://aka.ms/webpi-azure-cli) ** ve ** [MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Lütfen indirme işlemi tamamlandığınızda yükleyin

3.  Oluşturma bir PowerShell (veya diğer komut dosyası yürütülebilir dosya) aşağıdaki kod ile dosya ve yerel olarak kaydedin

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Aşağıdaki parametrelerle güncelleştirme yukarıda

    a. **`<StorageAccountName>`**: Depolama hesabınızın adını verin.

    b. **`<Storage Account Key>`**: Depolama hesabı anahtarınızı verin

    c. **`<Permission Start Date>`**: UTC saati için korumak için geçerli tarihten bir gün seçin. Örneğin, geçerli tarihi 26 Ekim 2016'ya, sonra değeri olmalıdır 25/10/2016. Azure CLI 2.0 (az komutu) kullanıyorsanız, tarihi ve saati başlangıç ve bitiş tarihleri, örneğin sağlar: 10-25-2016T00:00:00Z.

    d. **`<Permission End Date>`**: 3 hafta sonra en az bir tarihi seçin **başlangıç tarihi**. Bu değer olmalıdır **02/11/2016**. Azure CLI 2.0 (az komutu) kullanıyorsanız, tarihi ve saati başlangıç ve bitiş tarihleri, örneğin sağlar: 11-02-2016T00:00:00Z.

    Doğru parametreleri güncelleştirdikten sonra kod örneği aşağıda verilmiştir

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=TIQE5QWMKHpT5q2VnF1bb+NUV7NVMY2xmzVx1rdgIVsw7h0pcI5nMM6+DVFO65i4bQevx21dmrflA91r0Vh2Yw=="
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  "Yönetici olarak çalıştır" moduyla PowerShell Düzenleyicisi'ni açın ve #3. adımda dosyasını açın. İşletim sistemlerinde kullanılabilir olan herhangi bir betik Düzenleyicisi kullanabilirsiniz.

5.  Betiği çalıştırmak ve bunu size SAS URL'si için kapsayıcı düzeyinde erişim sağlar.

    Aşağıdaki çıktı SAS imzası ve vurgulanan bölüm bir Not Defteri'ne kopyalayın.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Artık, kapsayıcı düzeyi SAS URL'si alırsınız ve VHD adı içinde eklemeniz gerekir.

    Kapsayıcı düzeyi SAS URL #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  Aşağıda gösterildiği gibi kapsayıcı adının SAS URL'si olarak VHD adını Ekle `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Örnek:

    VHD adı TestRGVM201631920152.vhd yapılır ve ardından VHD SAS URL'si olacaktır

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Görüntü dosyası adı ve ".vhd" URI'de olduğundan emin olun.
    -   İmza ortadaki emin olun "sp rl =" görünür. Bu, okuma ve liste erişim başarıyla sağlandığını gösterir.
    -   İmza ortadaki emin olun "sr = c" görünür. Bu kapsayıcı düzeyinde erişimi olduğunu gösterir.

8.  Oluşturulan erişim imzası URI'si works paylaşılan emin olmak için tarayıcıda test edin. Yükleme işlemini başlatmak

9.  Paylaşılan erişim imzası URI'si kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

10. SKU'daki her VHD için bu adımları tekrarlayın.


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 VM görüntüsüyle ilgili bilgileri sağlayın ve Yayımlama Portalı'nda sertifika isteme
Teklifinizi ve SKU'nuzu oluşturduktan sonra SKU ile ilişkili görüntü ayrıntılarını girmeniz gerekir:

1. Git [yayımlama portalı][link-pubportal]ve ardından satıcı hesabınızla oturum açın.
2. Seçin **VM görüntüleri** sekmesi.
3. Sayfanın en üstündeki listede teklif tanımlayıcısıdır ve SKU tanımlayıcısı değil tanımlayıcısıdır.
4. Altındaki özellikleri doldurun **SKU'ları** bölümü.
5. Altında **işletim sistemi ailesi**, işletim sistemi VHD'si ile ilişkili işletim sistemi türüne tıklayın.
6. İçinde **işletim sistemi** kutusunda, işletim sistemini açıklayın. İşletim sistemi ailesi, türü, sürümü ve güncelleştirmeleri gibi bir biçim kullanabilirsiniz. "Windows Server Datacenter 2014 R2." örneğidir
7. En fazla altı adet önerilen sanal makine boyutu seçin. Bu satın alma ve görüntünüzü dağıtmak karar müşterinin Azure Portalı'nda fiyatlandırma katmanı dikey penceresinde gösterilen önerilerdir. **Bunlar yalnızca öneridir. Müşterinin belirtilen diskler karşılar herhangi bir VM boyutunu seçebilir.**
8. Sürümü girin. Sürüm alanı ürün ve kendi güncelleştirmeleri belirlemek için bir semantik sürüm kapsar:
   * Sürümleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır.
   * Farklı sku'lardaki görüntüler, farklı bir birincil ve ikincil sürüme sahip olabilir.
   * Bir SKU içinde sürümleri yalnızca düzeltme eki sürümü (Z X.Y.Z gelen) artırmak artımlı değişiklikler olmalıdır.
9. İçinde **işletim sistemi VHD URL'si** kutusuna, paylaşılan erişim imzası URI'si, işletim sistemi VHD'si için oluşturulan girin.
10. Bu SKU ile ilişkili veri diskleri varsa, bu veri diskinin dağıtımdan sonra bağlanmasını istediğiniz mantıksal birim numarası (LUN) seçin.
11. İçinde **LUN X VHD URL'si** kutusuna, paylaşılan erişim imzası URI'si ilk veri VHD'si için oluşturulan girin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Ortak bir SAS URL'si sorunları ve düzeltmeleri

|Sorun|Hata iletisi|Düzelt|Belgeleri bağlantısı|
|---|---|---|---|
|Kopyalama hatası görüntüleri - "?" SAS url bulunamadı|Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.|Önerilen araçlar kullanarak SAS URL'sini güncelleştirme|[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri kopyalama hatası - SAS URL'si değil, "s" ve "se" parametreleri|Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.|Başlangıç ve bitiş tarihlerini üzerindeki ile SAS URL'sini güncelleştirme|[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri "sp rl SAS URL'si değil =" - kopyalama hatası|Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor|"Okuma" ve "liste ayarlanan izinler ile SAS URL'sini güncelleştirme|[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri - SAS URL'sini kopyalama hatası vhd adında boşluk olması|Hata: Görüntüleri kopyalanıyor. Blob SAS URI'sini sağlanan kullanarak indirmek karşılaştırılamıyor.|Beyaz boşluk olmadan SAS URL'sini güncelleştirme|[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri: SAS Url Yetkilendirme hatası kopyalama hatası|Hata: Görüntüleri kopyalanıyor. Yetkilendirme hatası nedeniyle blobu indirmek karşılaştırılamıyor|SAS URL'sini yeniden oluştur|[https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri – SAS URL'si "st" ve "se" parametreleri kopyalama hatası tam tarih-saat belirtimine sahip değil|Hata: Görüntüleri kopyalanıyor. Yanlış SAS URL'si nedeniyle blobu indirmek karşılaştırılamıyor |SAS Url başlangıç ve bitiş tarihi parametreleri ("s", "se") 11 gibi tam tarih-saat belirtimine sahip için gerekli-02-2017T00:00:00Z ve yalnızca tarih ve saat için kısaltılmış sürümleri. Azure CLI 2.0 (az komutu) kullanarak bu senaryoyu karşılaşmak mümkündür. Tam tarih-saat belirtimi sağlayın ve SAS URL'sini yeniden emin olun.|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|

## <a name="next-step"></a>Sonraki adım
SKU ayrıntılarla bitirdikten sonra İleri taşıyabilirsiniz [Azure Market pazarlama içerik Kılavuzu][link-pushstaging]. Yayımlama işleminin bu adımında pazarlama içeriği, fiyatlandırma ve öncesinde gerekli diğer bilgileri sağlayan **3. adım: Teklif hazırlamada VM'nizi test**, burada dağıtmadan önce çeşitli kullanım örneği senaryolarını test Genel görünürlük ve satın alma için Azure Marketi sunar.  

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

[img-acom-1]:media/marketplace-publishing-vm-image-creation/vm-image-acom-datacenter.png
[img-portal-vm-size]:media/marketplace-publishing-vm-image-creation/vm-image-portal-size.png
[img-portal-vm-create]:media/marketplace-publishing-vm-image-creation/vm-image-portal-create-vm.png
[img-portal-vm-location]:media/marketplace-publishing-vm-image-creation/vm-image-portal-location.png
[img-portal-vm-rdp]:media/marketplace-publishing-vm-image-creation/vm-image-portal-rdp.png
[img-azstg-add]:media/marketplace-publishing-vm-image-creation/vm-image-storage-add.png
[img-manage-vm-new]:media/marketplace-publishing-vm-image-creation/vm-image-manage-new.png
[img-manage-vm-select]:media/marketplace-publishing-vm-image-creation/vm-image-manage-select.png
[img-cert-vm-key-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-keyfile-linux.png
[img-cert-vm-pswd-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-linux.png
[img-cert-vm-pswd-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-password-win.png
[img-cert-vm-test-lnx]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-linux.png
[img-cert-vm-test-win]:media/marketplace-publishing-vm-image-creation/vm-image-certification-test-sample-win.png
[img-cert-vm-results]:media/marketplace-publishing-vm-image-creation/vm-image-certification-results.png
[img-cert-vm-questionnaire]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire.png
[img-cert-vm-questionnaire-2]:media/marketplace-publishing-vm-image-creation/vm-image-certification-questionnaire-2.png
[img-pubportal-vm-skus]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus.png
[img-pubportal-vm-skus-2]:media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-2.png

[link-pushstaging]:marketplace-publishing-push-to-staging.md
[link-github-waagent]:https://github.com/Azure/WALinuxAgent
[link-azure-codeplex]:https://azurestorageexplorer.codeplex.com/
[link-azure-2]:../storage/blobs/storage-dotnet-shared-access-signature-part-2.md
[link-azure-1]:../storage/common/storage-dotnet-shared-access-signature-part-1.md
[link-msft-download]:http://www.microsoft.com/download/details.aspx?id=44299
[link-technet-3]:https://technet.microsoft.com/library/hh846766.aspx
[link-technet-2]:https://msdn.microsoft.com/library/dn495261.aspx
[link-azure-portal]:https://portal.azure.com
[link-pubportal]:https://publish.windowsazure.com
[link-sql-2014-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/
[link-sql-2014-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/
[link-sql-2014-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/
[link-sql-2012-ent]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/
[link-sql-2012-std]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/
[link-sql-2012-web]:http://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/
[link-datactr-2012-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/
[link-datactr-2012]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/
[link-datactr-2008-r2]:http://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-technet-1]:https://technet.microsoft.com/library/hh848454.aspx
[link-azure-vm-2]:./virtual-machines-linux-agent-user-guide/
[link-openssl]:https://www.openssl.org/
[link-intsvc]:http://www.microsoft.com/download/details.aspx?id=41554
[link-python]:https://www.python.org/
