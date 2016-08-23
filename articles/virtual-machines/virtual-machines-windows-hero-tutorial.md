<properties
    pageTitle="İlk Windows VM’nizi oluşturma | Microsoft Azure"
    description="Azure portalı kullanarak ilk Windows sanal makinenizi oluşturmayı öğrenin."
    keywords="Windows sanal makine,sanal makine oluşturma,sanal bilgisayar,sanal makine ayarlama"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/07/2016"
    ms.author="cynthn"/>

# Azure portalı kullanarak ilk Windows sanal makinenizi oluşturma

Bu öğretici, Azure Portal'ı kullanarak birkaç dakika içinde bir Windows VM oluşturmanın ne kadar kolay olduğunu gösterir.  

Bir Azure aboneliğiniz yoksa, yalnızca birkaç dakika içinde [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturabilirsiniz.

Bu öğreticinin [videolu kılavuzunu](https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Create-A-Virtual-Machine-Running-Windows-In-The-Azure-Preview-Portal) burada bulabilirsiniz. 


## Marketten VM görüntüsünü seçin

Örnek olarak Windows Server 2012 R2 Datacenter görüntüsü kullanıyoruz, ancak bu Azure’un sunduğu birçok görüntüden sadece biridir. Görüntü seçenekleriniz aboneliğinize göre değişir. Örneğin, masaüstü görüntüleri [MSDN aboneleri](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) tarafından kullanılabilir.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. Hub menüsünde, **Yeni** > **Virtual Machines** > **Windows Server 2012 R2 Datacenter**’a tıklayın.

    ![Portalda kullanılabilecek Azure VM görüntülerini gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)


3. **Windows Server 2012 R2 Datacenter** sayfasında, **Dağıtım modeli seçin** altında, **Resource Manager**’ın seçili olduğunu doğrulayın. **Oluştur**’ tıklayın.

    ![VM için seçilecek dağıtım modelini gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## Windows sanal makine oluşturma

Görüntüyü seçtikten sonra, yapılandırmaların çoğu için Azure’un varsayılan ayarlarını kullanabilir ve hızlı bir şekilde sanal makine oluşturabilirsiniz.

1. **Temel Bilgiler** dikey penceresinde, sanal makine için **adı** girin. Ad, 1-15 karakter uzunluğunda olmalıdır ve özel karakterler içeremez.

2. VM’de yerel hesap oluşturmak için kullanılacak bir **Kullanıcı adı** ve güçlü bir **Parola** girin. Yerel hesap VM’de oturum açmak ve yönetmek için kullanılır. 

    Parola 12-123 karakter uzunluğunda olmalıdır ve en az şunları içermelidir: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. 


3. Varolan [Kaynak grubunu](../resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. **Batı ABD** gibi, bir Azure veri merkezi **Konumu** yazın. 

4. İşiniz bittiğinde, sonraki bölüme geçmek için **Tamam**’a tıklayın. 

    ![Bir Azure VM’yi yapılandırmak için Temel Bilgiler dikey penceresindeki ayarları gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)

    
5. Bir sanal makine [boyutu](virtual-machines-windows-sizes.md) seçin ve ardından devam etmek için **Seç**’e tıklayın. 

    ![Seçebileceğiniz Azure VM boyutlarını gösteren Boyut dikey penceresi ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/size-blade.png)

6. **Ayarlar** dikey penceresinde, depolama ve ağ seçeneklerini değiştirebilirsiniz. İlk sanal makine için, genellikle varsayılan ayarları kabul edebilirsiniz. Bunu destekleyen bir sanal makine boyutu seçtiyseniz, **Disk türü** altında **Premium (SSD)**’yi seçerek Premium Storage’ı deneyebilirsiniz. Değişiklikleriniz bittiğinde **Tamam**’a tıklayın.

    ![Bir Azure VM için isteğe bağlı özellikleri yapılandırabileceğiniz Ayarlar dikey penceresi ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)

7. Seçimlerinizi gözden geçirmek için **Özet**’e tıklayın. İşiniz bittiğinde, **Tamam**’a tıklayın.

    ![Azure VM için yapılan yapılandırma seçimlerini gösteren Özet sayfası ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)

8. Azure Virtual Machine oluştururken, hub menüsünde **Virtual Machines** altında ilerleme durumunu izleyebilirsiniz. 


## Sanal makineye bağlanma ve oturum açma

1.  Hub menüsünde, **Virtual Machines**’e tıklayın.

2.  Listeden sanal makineyi seçin.

3. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın. Bu , makinenize bağlanmak için kısayola benzeyen bir Uzak Masaüst Protokol dosyası (.rdp dosyası) oluşturur ve indirir. Kolay erişim için dosyayı masaüstünüze kaydetmek isteyebilirsiniz. VM'nize bağlanmak için bu dosyayı **Açın** .

    ![VM'nize nasıl bağlanacağınızı gösteren Azure portal ekran görüntüsü.](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. .rdp dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.

    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Windows Güvenlik penceresinde, VM oluşturduğunuz sırada oluşturduğunuz yerel hesabın kullanıcı adı ve parolasını yazın. Kullanıcı adı *vmname*&#92;*username* olarak girilir ve sonra **Tamam**’a tıklayın.

    ![VM adı, kullanıcı adı ve parolayı girme ekran görüntüsü.](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Sertifikanın doğrulanamadığına ilişkin uyarı alırsınız. Bu normaldir. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.

    ![VM kimliğini doğrulamaya ilişkin mesajı gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Bağlanmayı denediğinizde sorun yaşıyorsanız, bkz. [Windows tabanlı Azure Virtual Machine’e Uzak Masaüstü Bağlantı Sorunlarını Giderme](virtual-machines-windows-troubleshoot-rdp-connection.md).

Artık başka bir sunucuyla yaptığınız gibi sanal makine ile çalışabilirsiniz.

## VM’yi durdurma

Gerçekte kullanmadığınız durumda ücret ödememeniz için VM’yi durdurmak iyi bir fikirdir. **Durdur** düğmesine ve **Evet**’e tıklamanız yeterlidir.

![Bir VM’yi durdurmaya yönelik düğmeyi gösteren ekran görüntüsü.](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)
    
Tekrar kullanmaya hazır olduğunuzda, VM’yi yeniden başlatmak için **Başlat** düğmesine tıklamanız yeterlidir.


## Sonraki adımlar

* Sanal makinenize [bir veri diski eklemeyi](virtual-machines-windows-attach-disk-portal.md) de deneyimleyebilirsiniz. Veri diskleri sanal makineniz için daha fazla depolama alanı sağlar.

* Ayrıca [Windows Powershell kullanarak bir VM oluşturabilir](virtual-machines-windows-ps-create.md) veya Azure CLI kullanarak [Linux sanal makine oluşturabilirsiniz](virtual-machines-linux-quick-create-cli.md).



<!--HONumber=Aug16_HO1-->


