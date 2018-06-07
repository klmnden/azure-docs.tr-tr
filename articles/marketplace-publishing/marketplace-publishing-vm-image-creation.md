---
title: Bir sanal makine görüntüsü için Azure Marketi oluşturma | Microsoft Docs
description: Satın almak Azure Marketi başkaları için bir sanal makine görüntüsünün nasıl oluşturulacağı hakkında ayrıntılı yönergeler.
services: Azure Marketplace
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: 5c937b8e-e28d-4007-9fef-624046bca2ae
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 01/05/2017
ms.author: mbaldwin
ms.openlocfilehash: ad6d48a03575e8fabd7eed2ebc1f7926ec4559d4
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808749"
---
# <a name="guide-to-create-a-virtual-machine-image-for-the-azure-marketplace"></a>Azure Market bir sanal makine görüntüsü oluşturmak için kılavuz
Bu makalede **2. adım**, sanal sabit Azure Marketi dağıtacağınız diskleri (VHD) hazırlama size yol gösterir. Vhd'lerinizi, sku'sunun temelidir. İşlem, bir Windows tabanlı veya Linux tabanlı SKU olup sağlanmaktadır bağlı olarak farklılık gösterir. Bu makalede her iki senaryoyu ele alınmaktadır. Bu işlem ile paralel olarak gerçekleştirilebilir [hesap oluşturma ve kayıt][link-acct-creation].

## <a name="1-define-offers-and-skus"></a>1. Teklifler ve SKU'ları tanımlayın
Bu bölümde, teklifleri ve ilişkili SKU'ları tanımlayın öğrenin.

Teklif, tüm SKU'larının "üst öğesidir". Birden çok teklife sahip olabilirsiniz. Tekliflerinizi nasıl yapılandıracağınıza siz karar verirsiniz. Bir teklif, hazırlamaya gönderilirken tüm SKU'larıyla birlikte gönderilir. URL'de görünür olacağından, SKU tanımlayıcıları dikkatlice düşünün:

* Azure.com: http://azure.microsoft.com/marketplace/partners/{PartnerNamespace}/{OfferIdentifier}-{SKUidentifier}
* Azure Önizleme portalı: https://portal.azure.com/#gallery/{PublisherNamespace}.{OfferIdentifier}{SKUIDdentifier}  

Bir SKU bir VM görüntüsü için ticari adıdır. Bir VM görüntüsü içeren bir işletim sistemi diski ve sıfır veya daha fazla veri diski. Özünde, bir sanal makinenin tam depolama profilidir. Bir VHD her disk için gereklidir. Bile boş veri diskleri oluşturulacak bir VHD gerektirir.

Kullandığınız işletim sisteminden bağımsız olarak yalnızca SKU için gereken en az sayıda veri diski ekleyin. Müşteriler görüntünün bir parçasını dağıtım zamanında disklerinin kaldırılamıyor ancak bunları ihtiyacınız varsa her zaman diskleri sırasında veya dağıtımdan sonra ekleyebilirsiniz.

> [!IMPORTANT]
> **Yeni bir görüntü sürümü disk sayısı değiştirmeyin.** Veri diskleri görüntüdeki yeniden yapılandırmanız gerekir, yeni bir SKU tanımlayın. Yeni bir görüntü sürümü farklı bir disk sayıları ile yayımlama sonu yeni dağıtım olasılığı yeni görüntü sürümünü durumlarda ARM şablonları ve diğer senaryolar aracılığıyla çözümlerini otomatik ölçeklendirme, otomatik dağıtımlarının temel.
>
>

### <a name="11-add-an-offer"></a>1.1 bir teklif ekleyin
1. Oturum [yayımlama portalında] [ link-pubportal] seller hesabınızı kullanarak.
2. Seçin **sanal makineleri** yayımlama portalında sekmesinde. İstendiğinde giriş alanına teklif adınızı girin. Teklif adı genellikle ürün veya Azure Marketi'nde satmak için planlama hizmetin adıdır.
3. **Oluştur**’u seçin.

### <a name="12-define-a-sku"></a>1.2 bir SKU tanımlayın
Bir teklif ekledikten sonra SKU'ları tanımlayabilir ve gerekir. Birden çok teklifleri olabilir ve her teklif birden çok SKU'ları altındaki olabilir. Bir teklif, hazırlamaya gönderilirken tüm SKU'larıyla birlikte gönderilir.

1. **Bir SKU ekleyin.** SKU URL'SİNDE kullanılan bir tanımlayıcı gerektirir. Tanımlayıcı yayımlama profili içinde benzersiz olmalıdır. ancak diğer yayımcı ile tanımlayıcı çakışma riski vardır.

   > [!NOTE]
   > Teklif ve SKU tanımlayıcıları Market teklifi URL'de görüntülenir.
   >
   >
2. **SKU için Özet açıklama ekleyin.** Özet açıklamaları, müşterilerin görebildiği olduğundan kolay okunabilir yapmanız. Bu bilgiler kadar "Hazırlama itme" aşamasında kilitli gerekmez. Bu aşamaya kadar bilgileri istediğiniz zaman düzenleyebilirsiniz.
3. Windows tabanlı SKU'lar kullanıyorsanız Windows Server'ın onaylanmış sürümlerini edinmek için önerilen bağlantıları izleyin.

## <a name="2-create-an-azure-compatible-vhd-linux-based"></a>2. (Linux tabanlı) bir Azure uyumlu VHD oluşturma
Bu bölümde Azure Market Linux tabanlı VM görüntüsü oluşturmak için en iyi uygulamalar odaklanır. Adım adım kılavuz için aşağıdaki belgelere bakın: [özel bir Linux VM görüntüsü oluşturma](../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="3-create-an-azure-compatible-vhd-windows-based"></a>3. (Windows tabanlı) bir Azure uyumlu VHD oluşturma
Bu bölüm, Windows Server için Azure Marketi tabanlı bir SKU oluşturmak için aşağıdaki adımları odaklanır.

### <a name="31-ensure-that-you-are-using-the-correct-base-vhds"></a>3.1 doğru temel VHD kullandığınızdan emin olun
İşletim sistemi VHD VM görüntüsü için Windows Server veya SQL Server içeren Azure onaylı temel görüntüde dayalı olması gerekir.

Başlamak için aşağıdaki görüntüleri birinden bir VM oluşturma bulunan [Microsoft Azure portal][link-azure-portal]:

* Windows Server ([2012 R2 Datacenter][link-datactr-2012-r2], [2012 Datacenter][link-datactr-2012], [2008 R2 SP1] [link-datactr-2008-r2])
* SQL Server 2014 ([Kurumsal][link-sql-2014-ent], [standart][link-sql-2014-std], [Web] [ link-sql-2014-web])
* SQL Server 2012 SP2 ([Enterprise][link-sql-2012-ent], [Standard][link-sql-2012-std], [Web][link-sql-2012-web])

Bu bağlantılar SKU sayfasındaki Yayımlama Portalı'nda da bulunabilir.

> [!TIP]
> Geçerli Azure portal veya PowerShell kullanıyorsanız, Windows Server görüntülerini 8 Eylül 2014 yayınlanan ve üzeri onaylanır.
>
>

### <a name="32-create-your-windows-based-vm"></a>3.2, Windows tabanlı VM oluşturma
Microsoft Azure Portalı'ndan yalnızca birkaç basit adımda onaylanmış bir temel görüntü göre VM oluşturabilirsiniz. İşlemine genel bakış verilmiştir:

1. Temel görüntü sayfasından seçin **sanal makine oluşturma** yeni yönlendirilmesine [Microsoft Azure portal][link-azure-portal].

    ![Çizim][img-acom-1]
2. Portal kullanmak istediğiniz Azure aboneliği için parola ve Microsoft hesabı ile oturum açın.
3. Seçtiğiniz temel görüntü kullanarak bir VM oluşturmak için istemleri izleyin. VM için bir ana bilgisayar adını (bilgisayar adı), (yönetici olarak kayıtlı) kullanıcı adı ve parolasını sağlamanız gerekir.

    ![Çizim][img-portal-vm-create]
4. Dağıtmak için VM boyutunu seçin:

    a.    VHD şirket içi geliştirme yapmayı planlıyorsanız, boyutu önemli değildir. En küçük VM'lerden birini kullanabilirsiniz.

    b.    Görüntüyü Azure'da geliştirmeyi planlıyorsanız, seçilen görüntü için önerilen VM boyutlarından birini kullanın.

    c.    Fiyatlandırma bilgileri için bkz **fiyatlandırma katmanlarına önerilen** portalında görüntülenen Seçici. Yayımcı tarafından sağlanan üç önerilen boyut burada belirtilmiştir. (Burada, yayımcı Microsoft'tur.)

    ![Çizim][img-portal-vm-size]
5. Özellikleri ayarlayın:

    a.    Hızlı dağıtım için altında özellikleri için varsayılan değer bırakabilirsiniz **isteğe bağlı yapılandırma** ve **kaynak grubu**.

    b.    Altında **depolama hesabı**, işletim sistemi VHD depolanacağı depolama hesabı isteğe bağlı olarak seçebilirsiniz.

    c.    Altında **kaynak grubu**, isteğe bağlı olarak VM yerleştirileceği mantıksal grup seçebilirsiniz.
6. Seçin **konumu** dağıtımı için:

    a.    VHD şirket içi geliştirme yapmayı planlıyorsanız, Azure için daha sonra görüntüyü yükler için konumun önemli değildir.

    b.    Görüntüyü Azure'da geliştirmeyi planlıyorsanız, başlangıçta ABD tabanlı Microsoft Azure bölgelerinden birini kullanın. Bu sertifika için görüntünüzü gönderdiğinizde, Microsoft sizin adınıza gerçekleştiren VHD kopyalama işlemini hızlandırır.

    ![Çizim][img-portal-vm-location]
7. **Oluştur**’a tıklayın. VM dağıtmak başlatır. Dağıtımınız birkaç dakika içinde başarıyla tamamlanır ve SKU'nuz için görüntü oluşturmaya başlayabilirsiniz.

### <a name="33-develop-your-vhd-in-the-cloud"></a>3.3, VHD bulutta geliştirin
Uzak Masaüstü Protokolü (RDP) kullanarak, VHD bulutta geliştirmek öneririz. Kullanıcı adı ve parolayla sağlama işlemi sırasında belirtilen için RDP bağlayın.

> [!IMPORTANT]
> **Yönetilen diskleri kullanmayın.** Bulut VHD'ye geliştirmek için kullanılan sanal makineyi bir görüntüden oluşturulması şu anda desteklemediğinden, yönetilen disklerde esas gerekir.
> İsteğe bağlı özellik değişikliği diskleri için varsayılan sanal makine oluşturma yönetilen.

> VHD geliştiriyorsanız (hangi önerilmez) şirket içi, bkz: [şirket içi bir sanal makine görüntüsü oluşturma](marketplace-publishing-vm-image-creation-on-premise.md). VHD indirme bulutta geliştiriyorsanız gerekli değildir.
>
>

**RDP aracılığıyla bağlanırken [Microsoft Azure portalı][link-azure-portal]**

1. Seçin **tüm hizmetleri** > **VM'ler**.
2. Sanal makineler dikey pencere açılır. Bağlanmak istediğiniz VM çalışıyorsa ve dağıtılan VM'ler listesinden seçin emin olun.
3. Seçilen VM açıklayan bir dikey pencere açılır. En üstte tıklatın **Bağlan**.
4. Kullanıcı adı ve sağlama işlemi sırasında belirtilen parola girmeniz istenir.

**PowerShell kullanarak RDP aracılığıyla bağlanma**

Uzak Masaüstü dosyası yerel makineye indirmek için kullanacağınız [Get-AzureRemoteDesktopFile cmdlet'i][link-technet-2]. Bu cmdlet kullanabilmeniz için VM adını ve hizmet adını bilmeniz gerekir. Sanal makineden oluşturduysanız [Microsoft Azure portal][link-azure-portal], bu bilgileri VM Özellikleri'nin altında bulabilirsiniz:

1. Microsoft Azure Portalı'nda seçin **tüm hizmetleri** > **VM'ler**.
2. Sanal makineler dikey pencere açılır. Dağıttığınız VM'yi seçin.
3. Seçilen VM açıklayan bir dikey pencere açılır.
4. **Özellikler**'e tıklayın.
5. Etki alanı adının ilk bölümü hizmet adıdır. Ana bilgisayar adı VM adıdır.

    ![Çizim][img-portal-vm-rdp]
6. Yöneticinin yerel Masaüstü için oluşturulan VM için RDP dosyasını karşıdan yüklemek için cmdlet'i aşağıdaki gibidir.

        Get‐AzureRemoteDesktopFile ‐ServiceName “baseimagevm‐6820cq00” ‐Name “BaseImageVM” –LocalPath “C:\Users\Administrator\Desktop\BaseImageVM.rdp”

RDP hakkında daha fazla bilgi MSDN makalesinde bulunabilir [RDP veya SSH ile bir Azure VM Bağlan](http://msdn.microsoft.com/library/azure/dn535788.aspx).

**Bir VM yapılandırın ve, SKU oluşturun**

VHD indirilir işletim sistemi sonra SKU oluşturmaya başlamak için bir VM yapılandırmak ve Hyper-v kullanın. Ayrıntılı adımlar aşağıdaki TechNet bağlantıda bulunabilir: [HyperV yükleme ve yapılandırma VM](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="34-choose-the-correct-vhd-size"></a>3.4 doğru VHD boyutu seçin
Windows işletim sisteminde VHD VM görüntüsü 128 GB sabit biçimli VHD oluşturulmalıdır.  

Fiziksel boyutu 128 GB daha az ise, VHD'yi seyrek olmalıdır. Sağlanan temel Windows ve SQL Server görüntüler zaten bu gereksinimleri karşılayan, bu nedenle biçimi veya elde edilen VHD öğesinin boyutuna değiştirmeyin.  

Veri diskleri 1 TB'ye kadar büyük olabilir. Disk boyutuna karar verirken, müşterilerin dağıtım zamanında içinde bir görüntü VHD'ler boyutlandırılamıyor unutmayın. Veri diski VHD'leri sabit biçimli VHD oluşturulmalıdır. Bunlar ayrıca seyrek olmalıdır. Veri diskleri boş olabilir veya veri içerebilir.

### <a name="35-install-the-latest-windows-patches"></a>3.5 en son Windows düzeltme eklerini yükleyin
Temel görüntüler, yayımlanma tarihlerine göre en son düzeltmeleri içerir. İşletim sistemi VHD yayımlamadan önce oluşturmuş olduğunuz, Windows Update'i çalıştırın ve tüm son kritik ve önemli güvenlik güncelleştirmelerini yüklediğinizden emin olun.

### <a name="36-perform-additional-configuration-and-schedule-tasks-as-necessary"></a>3.6 gerektiği gibi ek yapılandırma ve zamanlama görevleri gerçekleştirme
Ek yapılandırma gerekli olursa, onu dağıtıldıktan sonra VM son değişiklikler yapmak için başlangıçta çalışan zamanlanmış bir görev kullanmayı dikkate alın:

* Görevin başarıyla yürütüldükten sonra kendini silmesi, kullanılabilecek en iyi yöntemdir.
* Bunlar her zaman mevcut garanti yalnızca iki sürücüleri olduğundan herhangi bir yapılandırma C veya D sürücüsü dışındaki sürücülerde yararlanmalıdır. C sürücüsünün işletim sistemi diski ve sürücü D geçici yerel disk.

### <a name="37-generalize-the-image"></a>3.7 görüntü generalize
Tüm Azure Marketi görüntülerinde genel bir şekilde yeniden kullanılabilir olması gerekir. Diğer bir deyişle, işletim sistemi VHD genelleştirilmiş gerekir:

* Windows için "sysprepped" görüntü olmalıdır ve hiçbir yapılandırmaları desteklemeyen yapılmalıdır **sysprep** komutu.
* Dizin % windir%\System32\Sysprep aşağıdaki komutu çalıştırabilirsiniz.

        sysprep.exe /generalize /oobe /shutdown

  Sysprep için şu MSDN makalesine adımda işletim sistemini nasıl sağlandığını konusunda yönergeler: [Azure için Windows Server VHD oluşturun ve yükleyin](../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="4-deploy-a-vm-from-your-vhds"></a>4. Vhd'lerinizi VM'den dağıtma
Vhd'lerinizi (genelleştirilmiş bir işletim sistemi ve VHD disk sıfır veya daha fazla veri) için bir Azure depolama hesabı karşıya yükledikten sonra bunları bir kullanıcı VM görüntüsü olarak kaydedebilirsiniz. Ardından, görüntü test edebilirsiniz. İşletim sisteminizi VHD genelleştirilmiş olmadığından, doğrudan VM VHD URL'si sağlayarak dağıtamayacağınızı unutmayın.

VM görüntüleri hakkında daha fazla bilgi edinmek için aşağıdaki blog gönderileri gözden geçirin:

* [VM görüntüsü](https://azure.microsoft.com/blog/vm-image-blog-post/)
* [VM görüntüsü PowerShell nasıl yapılır](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)
* [Azure'da VM görüntüleri hakkında](https://msdn.microsoft.com/library/azure/dn790290.aspx)

### <a name="set-up-the-necessary-tools-powershell-and-azure-cli"></a>Gerekli araçlarını Kur, PowerShell ve Azure CLI
* [PowerShell ayarlama](/powershell/azure/overview)
* [Azure CLI ayarlama](../cli-install-nodejs.md)

### <a name="41-create-a-user-vm-image"></a>4.1 kullanıcı VM görüntüsü oluşturma
#### <a name="capture-vm"></a>VM yakalama
Lütfen PowerShell/API/Azure CLI kullanarak VM yakalama hakkında rehberlik için aşağıda verilen bağlantıları okuyun.

* [API](https://msdn.microsoft.com/library/mt163560.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="generalize-image"></a>Görüntü generalize
Lütfen PowerShell/API/Azure CLI kullanarak VM yakalama hakkında rehberlik için aşağıda verilen bağlantıları okuyun.

* [API](https://msdn.microsoft.com/library/mt269439.aspx)
* [PowerShell](../virtual-machines/windows/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Azure CLI](../virtual-machines/linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="42-deploy-a-vm-from-a-user-vm-image"></a>4.2 bir kullanıcı VM görüntüsü VM'den dağıtma
Bir kullanıcı VM görüntüsünden bir VM'yi dağıtmak için geçerli kullanabilirsiniz [Azure portal](https://manage.windowsazure.com) veya PowerShell.

**Geçerli Azure portalından bir VM'yi dağıtmak**

1. Git **yeni** > **işlem** > **sanal makine** > **galerisinden**.

2. Git **görüntülerim**ve ardından bir VM'yi dağıtmak VM görüntüsünü seçin:

   1. Hangi görüntüsünü seçin, çünkü dikkat **görüntülerim** işletim sistemi görüntüleri ve VM görüntüleri listeler.
   2. Disk sayısını arayan VM görüntüleri çoğunluğu olduğu için birden fazla disk görüntüsü hangi türü, dağıttığınız, belirlenmesine yardımcı olabilir. Ancak, bir VM görüntüsü sonra olması gereken bir tek işletim sistemi diskle sağlamak hala mümkündür **disk sayısı** 1 olarak ayarlayın.

      ![Çizim][img-manage-vm-select]
3. VM Oluşturma Sihirbazı'nı izleyin ve VM adı, VM boyutu, konum, kullanıcı adı ve parola belirtin.

**PowerShell VM'den dağıtma**

Yeni oluşturduğunuz genelleştirilmiş VM görüntüsü'dan büyük bir VM'yi dağıtmak için aşağıdaki cmdlet'leri kullanabilirsiniz.

    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot

> [!IMPORTANT]
> Ek Yardım için lütfen [VHD oluşturma sırasında karşılaşılan sorun giderme ortak sorunları] bakın.
>
>

## <a name="5-obtain-certification-for-your-vm-image"></a>5. VM görüntüsü için sertifika elde
VM görüntüsü için Azure Marketi hazırlama sonraki adım, sertifikalı sağlamaktır.

Bu işlem, özel sertifika aracını çalıştıran, doğrulama sonuçlarını Vhd'lerinizi bulunduğu Azure kapsayıcısına yüklenmesi, bir teklif ekleme, SKU tanımlama ve sertifika için VM görüntü gönderme içeriyor.

### <a name="51-download-and-run-the-certification-test-tool-for-azure-certified"></a>5.1 indirin ve Azure sertifikalı için sertifika Test aracı çalıştırın
VM görüntüsü Microsoft Azure ile uyumlu olduğundan emin olmak için kullanıcı VM görüntüden sağlanan çalışan bir VM, sertifika Aracı'nı çalıştırır. Bu araç, VHD'nizi hazırlamaya yönelik yönerge ve gereksinimlerin karşılandığını doğrular. Aracın çıktısının istekte bulunan sertifika sırasında yayımlama portalında yüklenmelidir uyumluluk rapordur.

Sertifika aracı, Windows ve Linux VM'ler ile kullanılabilir. Windows tabanlı VM'ler PowerShell aracılığıyla bağlanır ve Linux VM'ler SSH.Net üzerinden bağlanır:

1. İlk olarak, sertifika aracını indirin [Microsoft İndirme sitesi][link-msft-download].
2. Sertifika Aracı'nı açın ve ardından **yeni Testi Başlat** düğmesi.
3. Gelen **Test bilgi** ekranında, test çalışması için bir ad girin.
4. VM'nizin Linux üzerinde veya Windows üzerinde çalıştığına ilişkin seçim yapın. Seçiminize bağlı olarak sonraki seçenekleri belirleyin.

### <a name="connect-the-certification-tool-to-a-linux-vm-image"></a>**Bir Linux VM görüntüsü sertifika aracı Bağlan**
1. SSH kimlik doğrulama modunu seçin: Parola veya anahtar dosyası.
2. Parola tabanlı kimlik doğrulamasını kullanıyorsanız, etki alanı adı sistemi (DNS) adı, kullanıcı adı ve parola girin.
3. Anahtar dosyası kimlik doğrulamasını kullanıyorsanız, DNS adını, kullanıcı adı ve özel anahtar konumu girin.

   ![Linux VM görüntüsü parola kimlik doğrulaması][img-cert-vm-pswd-lnx]

   ![Linux VM görüntüsü anahtar dosyası kimlik doğrulaması][img-cert-vm-key-lnx]

### <a name="connect-the-certification-tool-to-a-windows-based-vm-image"></a>**Bir Windows tabanlı VM görüntüsü sertifika aracı Bağlan**
1. Tam VM DNS adını (örneğin, MyVMName.Cloudapp.net) girin.
2. Kullanıcı adı ve parola girin.

   ![Windows VM görüntüsünü parola kimlik doğrulaması][img-cert-vm-pswd-win]

Linux veya Windows tabanlı VM görüntü için doğru seçeneklerini seçtikten sonra Seç **Bağlantıyı Sına** SSH.Net veya PowerShell test etmek için geçerli bir bağlantı olduğundan emin olmak için. Bağlantı kurulduktan sonra Seç **sonraki** testi başlatmak için.

Test tamamlandığında her bir test öğesine yönelik sonuçları (Başarılı/Başarısız/Uyarı) alırsınız.

![Linux VM görüntüsü için test çalışmaları][img-cert-vm-test-lnx]

![Windows VM görüntüsü için test çalışmaları][img-cert-vm-test-win]

Tüm testler başarısız olursa, görüntünüzü uygunluğu onaylanmamıştır. Bu gerçekleşirse, gereksinimleri gözden geçirin ve gerekli değişiklikleri yapın.

Otomatikleştirilmiş test sonra VM görüntüsüne bir soru formu ekran aracılığıyla ek giriş vermeniz istenir.  Soruları tamamlayın ve ardından **sonraki**.

![Sertifika aracı anket][img-cert-vm-questionnaire]

![Sertifika aracı anket][img-cert-vm-questionnaire-2]

Soru tamamladıktan sonra görüntü ve başarısız tüm değerlendirmeleri için bir açıklama Linux VM için SSH erişim bilgileri gibi ek bilgiler sağlayabilir. Soru verdiğiniz yanıtlar yanı sıra test sonuçlarını ve yürütülen test çalışmaları için günlük dosyalarını indirebilirsiniz. Sonuçları aynı kapsayıcı Vhd'lerinizi olarak kaydedin.

![Test sonuçları sertifika Kaydet][img-cert-vm-results]

### <a name="52-get-the-shared-access-signature-uri-for-your-vm-images"></a>5.2 VM görüntülerinizi için URI paylaşılan erişim imzası Al
Yayımlama işlemi sırasında her bir SKU için oluşturduğunuz VHD'leri sağlama Tekdüzen Kaynak Tanımlayıcıları (URI'ler) belirtin. Sertifika işlemi sırasında Microsoft'un bu VHD'lere erişmesi gerekir. Bu nedenle, her VHD için bir paylaşılan erişim imzası URI oluşturmanız gerekir. İçinde girilmelidir URI budur **görüntüleri** yayımlama portalında sekmesindedir.

Oluşturulan URI paylaşılan erişim imzası aşağıdaki gereksinimlere uyması:

Not: Aşağıdaki yönergelerde desteklenen tek tür olan yönetilmeyen diskler için geçerlidir.

* Paylaşılan erişim imzası URI'ler Vhd'lerinizi için oluştururken, liste ve Okuma izinleri yeterlidir. Yazma veya Silme erişimi sağlamayın.
* Paylaşılan erişim imzası URI oluşturulduğunda süresi erişim için üç (3) hafta ila en az olmalıdır.
* UTC saati için korumak için geçerli tarihten önce gün seçin. Örneğin, geçerli tarihe 6 Ekim 2014 ise 5/10/2014 seçin.

SAS URL, VHD için Azure Marketi paylaşmak için birden çok şekilde oluşturulabilir.
3 önerilen araçlar şunlardır:

1.  Azure Depolama Gezgini
2.  Microsoft Storage Gezgini
3.  Azure CLI

**Azure Depolama Gezgini (Windows kullanıcıları için önerilir)**

Azure Storage Gezgini kullanarak SAS URL oluşturmak için adımlar aşağıda verilmiştir

1. Karşıdan [Azure Storage Gezgini 6 Önizleme 3](https://azurestorageexplorer.codeplex.com/) Codeplex. Git [Azure Storage Gezgini 6 Önizleme](https://azurestorageexplorer.codeplex.com/) tıklatıp **"İndirmeleri"**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_01.png)

2. Karşıdan [AzureStorageExplorer6Preview3.zip](https://azurestorageexplorer.codeplex.com/downloads/get/891668) ve unzipping sonra yükleyin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_02.png)

3. Yüklendikten sonra uygulamayı açın.
4. Tıklatın **hesabı eklemek**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_03.png)

5. Depolama hesabı adı, depolama hesabı anahtarı ve depolama uç noktaları etki alanını belirtin. Azure Portalı'nda, VHD'yi nerede tutulur Azure Aboneliğinize depolama hesabında budur.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_04.png)

6. Azure Storage Gezgini belirli depolama hesabınıza bağlandıktan sonra tüm gösteren başlayacak içinde depolama hesabını içerir. İşletim sistemi disk VHD dosyasını (senaryonuz için uygun olup olmadığını da veri diskleri) kopyaladığınız kapsayıcısı seçin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_05.png)

7. Blob kapsayıcısı seçtikten sonra Azure Storage Gezgini kapsayıcı içindeki dosyaların gösteren başlatır. Gönderilmesi gereken görüntü dosyası (.vhd) seçin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_06.png)

8.  Kapsayıcıda .vhd dosyası seçtikten sonra **güvenlik** sekmesi.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_07.png)

9.  İçinde **Blob kapsayıcı güvenlik** iletişim kutusunda, üzerinde Varsayılanları bırakabilir **erişim düzeyi** sekmesini ve ardından **paylaşılan erişim imzaları** sekmesi.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_08.png)

10. .Vhd görüntüsü için paylaşılan erişim imzası URI oluşturmak için aşağıdaki adımları izleyin:

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_09.png)

    a. **Erişime izin:** UTC saati için korumak için geçerli tarihten önce gününü seçin. Örneğin, geçerli tarihe 6 Ekim 2014 ise 5/10/2014 seçin.

    b. **Erişime izin:** 3 hafta sonra en az bir tarihi seçin **erişime izin** tarih.

    c. **İzin verilen eylemleri:** seçin **listesi** ve **okuma** izinleri.

    d. .Vhd dosyası doğru seçtiğiniz sonra dosyanızı görünür **erişmek için Blob adı** uzantısı .vhd ile.

    e. Tıklatın **imzayı üretmek**.

    f. İçinde **oluşturulan paylaşılan erişim imzası URI bu kapsayıcının**, yukarıdaki onay aşağıdaki vurgulanan için:

       - Görüntünüzü dosya adı olduğundan emin olun ve **".vhd"** URI'de şunlardır.
       - İmza sonunda olduğundan emin olun **"rl ="** görüntülenir. Bu, okuma ve liste erişim başarıyla sağlandı gösterir.
       - İmza ortadaki olduğundan emin olun **"sr c ="** görüntülenir. Bu kapsayıcı düzeyinde erişime sahip olması gösterir

11. Oluşturulan erişimin imza URI çalıştığı paylaşılan emin olmak için tıklatın **tarayıcısında Test**. İndirme işlemi başlamanız gerekir.

12. Paylaşılan erişim imzası URI kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

13. SKU'ndaki her VHD için 6-10 adımları yineleyin.

**Microsoft Azure Depolama Gezgini (MAC/Windows/Linux)**

Microsoft Azure Storage Gezgini kullanarak SAS URL oluşturmak için adımlar aşağıda verilmiştir

1.  Microsoft Azure Storage Gezgini form karşıdan [ http://storageexplorer.com/ ](http://storageexplorer.com/) Web sitesi. Git [Microsoft Azure Storage Gezgini](http://storageexplorer.com/releasenotes.html) tıklatıp **"Windows için karşıdan yükleme"**.

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_10.png)

2.  Yüklendikten sonra uygulamayı açın.

3.  Tıklatın **hesabı eklemek**.

4.  Microsoft Azure Storage Gezgini aboneliğinize hesabınızda oturum açma yapılandırın

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_11.png)

5.  Depolama hesabına gidin ve kapsayıcı seçin

6.  Seçin **"Paylaşımı erişim imzası Al …"** Sağ tıklatarak kullanarak **kapsayıcı**

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_12.png)

7.  Aşağıdaki gibi başına başlangıç zamanı, bitiş saati ve izinleri güncelleştirme

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_13.png)

    a.  **Başlangıç saati:** UTC saati için korumak için geçerli tarihten önce gününü seçin. Örneğin, geçerli tarihe 6 Ekim 2014 ise 5/10/2014 seçin.

    b.  **Sona erme saati:** 3 hafta sonra en az bir tarihi seçin **başlangıç saati** tarih.

    c.  **İzinleri:** seçin **listesi** ve **okuma** izinleri

8.  Kapsayıcı paylaşılan erişim imzası URI kopyalayın

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_14.png)

    Oluşturulan SAS URL için düzeyi kapsayıcıdır ve şimdi biz VHD ad içinde eklemeniz gerekir.

    Kapsayıcı düzeyi SAS URL biçimi: `https://testrg009.blob.core.windows.net/vhds?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    SAS URL kapsayıcı adlarında aşağıdaki şekilde sonra VHD adı Ekle `https://testrg009.blob.core.windows.net/vhds/<VHD NAME>?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    Örnek:

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_15.png)

    VHD adı TestRGVM201631920152.vhd yapılır ve ardından VHD SAS URL'si olacaktır `https://testrg009.blob.core.windows.net/vhds/TestRGVM201631920152.vhd?st=2016-04-22T23%3A05%3A00Z&se=2016-04-30T23%3A05%3A00Z&sp=rl&sv=2015-04-05&sr=c&sig=J3twCQZv4L4EurvugRW2klE2l2EFB9XyM6K9FkuVB58%3D`

    - Görüntünüzü dosya adı olduğundan emin olun ve **".vhd"** URI'de şunlardır.
    - İmza ortadaki olduğundan emin olun **"sp rl ="** görüntülenir. Bu, okuma ve liste erişim başarıyla sağlandı gösterir.
    - İmza ortadaki olduğundan emin olun **"sr c ="** görüntülenir. Bu kapsayıcı düzeyinde erişime sahip olması gösterir

9.  Oluşturulan erişimin imza URI çalıştığı paylaşılan emin olmak için tarayıcıda sınayın. Yükleme işlemini başlatmak

10. Paylaşılan erişim imzası URI kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

11. SKU'daki her VHD için bu adımları tekrarlayın.

**Azure CLI (Windows dışı & sürekli tümleştirme için önerilir)**

Azure CLI kullanarak SAS URL oluşturmak için adımlar aşağıda verilmiştir

1.  Microsoft Azure CLI üzerinden indirme [burada](https://azure.microsoft.com/en-in/documentation/articles/xplat-cli-install/). Farklı bağlantıları için bulabileceğiniz **[Windows](http://aka.ms/webpi-azure-cli)** ve  **[MAC OS](http://aka.ms/mac-azure-cli)**.

2.  Bir kez yüklenir, lütfen yükleyin

3.  Oluşturma bir PowerShell (veya başka bir komut dosyası yürütülebilir dosyayı) dosya aşağıdaki kod ile ve yerel olarak Kaydet

          $conn="DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<Storage Account Key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl <Permission End Date> -c $conn --start <Permission Start Date>  

    Aşağıdaki parametreleri güncelleştirin yukarıda

    a. **`<StorageAccountName>`**: Depolama hesabınızın adını verin

    b. **`<Storage Account Key>`**: Depolama hesabı anahtarınızı verin

    c. **`<Permission Start Date>`**: UTC saati için korumak için geçerli tarihten önce gün seçin. Örneğin, geçerli tarih 25 Ekim 2016 ise ardından değeri olmalıdır 25/10/2016. Azure CLI 2.0 (az komutu) kullanıyorsanız, tarih ve saati başlangıç ve bitiş tarihleri, örneğin sağlar: 10-25-2016T00:00:00Z.

    d. **`<Permission End Date>`**: En az 3 hafta sonra olan bir tarih seçin **başlangıç tarihi**. Bu değer olmalıdır **02/11/2016**. Azure CLI 2.0 (az komutu) kullanıyorsanız, tarih ve saati başlangıç ve bitiş tarihleri, örneğin sağlar: 11-02-2016T00:00:00Z.

    Uygun parametreleri güncelleştirdikten sonra örnek kod aşağıda verilmiştir

          $conn="DefaultEndpointsProtocol=https;AccountName=st20151;AccountKey=<account-key>"
          azure storage container list vhds -c $conn
          azure storage container sas create vhds rl 11/02/2016 -c $conn --start 10/25/2016  

4.  "Yönetici olarak çalıştır" moduyla PowerShell Düzenleyicisi'ni açın ve #3. adımda dosyasını açın. İşletim sisteminde kullanılabilir herhangi bir komut dosyası düzenleyicisi kullanabilirsiniz.

5.  Komut dosyasını çalıştırmak ve onu, SAS URL'si için kapsayıcı düzeyinde erişimi sağlar

    Aşağıdaki SAS imza çıktısını olmalı ve bir Not Defteri'nde vurgulanan bölümünü kopyalama

    ![Çizim](media/marketplace-publishing-vm-image-creation/img5.2_16.png)

6.  Kapsayıcı düzeyi SAS URL artık alacak ve VHD ad içinde eklemeniz gerekir.

    Kapsayıcı düzeyi SAS URL #

    `https://st20151.blob.core.windows.net/vhds?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

7.  Aşağıda gösterildiği gibi SAS URL kapsayıcı adlarında sonra VHD adı Ekle `https://st20151.blob.core.windows.net/vhds/<VHDName>?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    Örnek:

    VHD adı TestRGVM201631920152.vhd yapılır ve ardından VHD SAS URL'si olacaktır

    `https://st20151.blob.core.windows.net/vhds/ TestRGVM201631920152.vhd?st=2016-10-25T07%3A00%3A00Z&se=2016-11-02T07%3A00%3A00Z&sp=rl&sv=2015-12-11&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

    - Yansıma Dosya adı ve ".vhd" URI'de olduğundan emin olun.
    -   İmza ortadaki olduğundan emin olun "sp rl =" görüntülenir. Bu, okuma ve liste erişim başarıyla sağlandı gösterir.
    -   İmza ortadaki olduğundan emin olun "sr c =" görüntülenir. Bu kapsayıcı düzeyinde erişime sahip olması gösterir

8.  Oluşturulan erişimin imza URI çalıştığı paylaşılan emin olmak için tarayıcıda sınayın. Yükleme işlemini başlatmak

9.  Paylaşılan erişim imzası URI kopyalayın. Bu, Yayımlama Portalı'na yapıştırılacak olan URI'dir.

10. SKU'daki her VHD için bu adımları tekrarlayın.


### <a name="53-provide-information-about-the-vm-image-and-request-certification-in-the-publishing-portal"></a>5.3 VM görüntü ile ilgili bilgileri sağlayın ve yayımlama portalında sertifika isteği
Teklif ve SKU oluşturduktan sonra SKU ile ilişkili görüntü ayrıntıları girmeniz gerekir:

1. Git [yayımlama portalında][link-pubportal]ve satıcının hesabınızla oturum açın.
2. Seçin **VM görüntüleri** sekmesi.
3. Sayfanın başında listelenen tanımlayıcısı gerçekte teklif tanımlayıcısı ve SKU tanımlayıcısı değil.
4. Özellikleri altında doldurmak **SKU'ları** bölümü.
5. Altında **işletim sistemi ailesi**, işletim sistemi VHD ile ilişkili işletim sisteminin türünü tıklatın.
6. İçinde **işletim sistemi** kutusunda, işletim sistemi tanımlar. İşletim sistemi ailesi, türü, sürüm ve güncelleştirmeler gibi bir biçim göz önünde bulundurun. "Windows Server Datacenter 2014 R2." örneğidir
7. En fazla altı önerilen sanal makine boyutlarını seçin. Bu satın alıp, görüntüyü dağıtmak karar görüntülenen Azure Portalı'nda fiyatlandırma katmanı dikey penceresinde müşteriye önerilerdir. **Bunlar yalnızca önerilerdir. Müşteri yansımanıza belirtilen diskleri düzenler herhangi bir VM boyutu seçebilirsiniz.**
8. Sürümü girin. Sürüm alanı ürünü ve onun güncelleştirmeleri tanımlamak için bir anlamsal sürüm yalıtır:
   * Sürümleri X, Y ve Z tamsayılar nerede X.Y.Z biçiminde olmalıdır.
   * Farklı SKU'ları görüntülerinde farklı birincil ve ikincil sürümleri olabilir.
   * İçinde bir SKU sürümlerinde yalnızca düzeltme eki sürümü (Z X.Y.Z gelen) artırmak artımlı değişiklikler olmalıdır.
9. İçinde **OS VHD URL'si** kutusunda, işletim sistemi VHD için oluşturulan URI paylaşılan erişim imzası girin.
10. Bu SKU ile ilişkili veri disklerinin varsa, bu veri diski dağıtım takılı istediğiniz mantıksal birim numarası (LUN) seçin.
11. İçinde **LUN X VHD URL'si** kutusuna, ilk veri VHD için oluşturulan URI paylaşılan erişim imzası girin.

    ![Çizim](media/marketplace-publishing-vm-image-creation/vm-image-pubportal-skus-3.png)


## <a name="common-sas-url-issues--fixes"></a>Ortak SAS URL sorunları ve düzeltmeleri

|Sorun|Hata iletisi|Düzelt|Belge bağlantı|
|---|---|---|---|
|Kopyalama hatası görüntüleri - "?" SAS URL'si bulunamadı|Hata: Görüntüleri kopyalama. Blob SAS URI'sini sağlanan kullanarak indirmek erişilemiyor.|Önerilen SAS URL'yi kullanarak güncelleştirme araçları|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri kopyalama hatası - SAS URL'si değil, "st" ve "se" parametreleri|Hata: Görüntüleri kopyalama. Blob SAS URI'sini sağlanan kullanarak indirmek erişilemiyor.|Başlangıç ve bitiş tarihleri üzerindeki ile SAS Url güncelleştir|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri - "sp rl SAS url değil =" kopyalama hatası|Hata: Görüntüleri kopyalama. Blob SAS URI'sini sağlanan kullanarak indirmek için|SAS Url "listeyi ve"Okuma"ayarlanmış olan izinleri ile güncelleştirme|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri - SAS url kopyalama hatası vhd adlarında boşluk sahip|Hata: Görüntüleri kopyalama. Blob SAS URI'sini sağlanan kullanarak indirmek erişilemiyor.|Güncelleştirme boşluk olmadan SAS URL'si|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri – SAS Url yetkilendirmesi hata kopyalama hatası|Hata: Görüntüleri kopyalama. Yetkilendirme hatası nedeniyle blob indirmek için|SAS Url yeniden oluştur|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|
|Görüntüleri – SAS Url "st" ve "se" parametreleri kopyalama hatası tam tarih-saat belirtimi sahip değil|Hata: Görüntüleri kopyalama. Yanlış SAS Url nedeniyle blob indirmek için |SAS Url başlangıç ve bitiş tarihi parametreleri ("st", "se") 11 gibi tam tarih-saat belirtimi için gerekli-02-2017T00:00:00Z ve yalnızca tarih veya saat için kısaltılmış sürümleri. Azure CLI 2.0 (az komutu) kullanarak bu senaryoyu karşılaştığınız mümkündür. Tam tarih-saat belirtimi sağlamak ve SAS Url yeniden emin olun.|[https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)|

## <a name="next-step"></a>Sonraki adım
SKU ayrıntılarla tamamladıktan sonra İleri taşıyabilirsiniz [Azure Marketi pazarlama içerik Kılavuzu][link-pushstaging]. Yayımlama işleminin bu adımında pazarlama içeriği, fiyatlandırma ve öncesinde gerekli diğer bilgileri sağladığınız **adım 3: VM sınama teklif hazırlamada**, burada dağıtmadan önce çeşitli kullanım örneği senaryolarını test Genel görünürlük ve satın alma için Azure Marketi sunar.  

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

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
