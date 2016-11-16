---
title: "İlk Windows VM’nizi oluşturma | Microsoft Belgeleri"
description: "Azure portalı kullanarak ilk Windows sanal makinenizi oluşturmayı öğrenin."
keywords: "Windows sanal makine,sanal makine oluşturma,sanal bilgisayar,sanal makine ayarlama"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 785e17eb-4a13-4f06-b70f-4bd496d0ec5d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: hero-article
ms.date: 09/06/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aaef478107d9c9771a1dc799a101ff9a41f821c6


---
# <a name="create-your-first-windows-virtual-machine-in-the-azure-portal"></a>Azure portalı kullanarak ilk Windows sanal makinenizi oluşturma
Bu öğretici, Azure Portal'ı kullanarak birkaç dakika içinde bir Windows sanal makinesi (VM) oluşturmanın ne kadar kolay olduğunu gösterir.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Marketten VM görüntüsünü seçin
Örnek olarak Windows Server 2012 R2 Datacenter görüntüsü kullanıyoruz, ancak bu Azure’un sunduğu birçok görüntüden sadece biridir. Görüntü seçenekleriniz aboneliğinize göre değişir. Örneğin, bazı masaüstü görüntüleri [MSDN aboneleri](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) tarafından kullanılabilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Hub menüsünde, **Yeni** > **Sanal Makineler** > **Windows Server 2012 R2 Datacenter**’a tıklayın.
   
    ![Portalda kullanılabilecek Azure VM görüntülerini gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)
3. **Windows Server 2012 R2 Datacenter** dikey penceresindeki **Dağıtım modeli seçin** menüsünde **Resource Manager**’ın seçili olduğunu doğrulayın. **Oluştur**’a tıklayın.
   
    ![VM için seçilecek dağıtım modelini gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## <a name="create-the-windows-virtual-machine"></a>Windows sanal makine oluşturma
Görüntüyü seçtikten sonra varsayılan ayarları kullanabilir ve hızlı bir şekilde sanal makine oluşturabilirsiniz.

1. **Temel Bilgiler** dikey penceresinde, sanal makine için **adı** girin. Ad, 1-15 karakter uzunluğunda olmalıdır ve özel karakterler içeremez.
2. VM’de yerel hesap oluşturmak için kullanılacak bir **Kullanıcı adı** ve güçlü bir **Parola** girin. Yerel hesap VM’de oturum açmak ve VM’yi yönetmek için kullanılır. 
   
    Parola 8-123 karakter uzunluğunda olmalıdır ve en az şu dört karmaşıklık gereksinimini karşılamalıdır: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. [Kullanıcı adı ve parola gereksinimleri](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm) hakkında daha fazla bilgi edinin.
3. Varolan [Kaynak grubunu](../azure-resource-manager/resource-group-overview.md#resource-groups) seçin veya yenisi için adı yazın. **Batı ABD** gibi, bir Azure veri merkezi **Konumu** yazın. 
4. İşiniz bittiğinde, sonraki bölüme geçmek için **Tamam**’a tıklayın. 
   
    ![Bir Azure VM’yi yapılandırmak için **Temel Bilgiler** dikey penceresindeki ayarları gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)
5. Bir VM [boyutu](virtual-machines-windows-sizes.md) seçin ve ardından devam etmek için **Seç**’e tıklayın. 
   
    ![Seçebileceğiniz Azure VM boyutlarını gösteren Boyut dikey penceresi ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/size-blade.png)
6. **Ayarlar** dikey penceresinde depolama ve ağ seçeneklerini değiştirebilirsiniz. Bu öğretici için varsayılan ayarları kabul edin. Bunu destekleyen bir sanal makine boyutu seçtiyseniz, **Disk türü** altında **Premium (SSD)**’yi seçerek Azure Premium Depolama’yı deneyebilirsiniz. Değişiklikleriniz bittiğinde **Tamam**’a tıklayın.
   
    ![Bir Azure VM için isteğe bağlı özellikleri yapılandırabileceğiniz Ayarlar dikey penceresi ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)
7. Seçimlerinizi gözden geçirmek için **Özet**’e tıklayın. **Doğrulama başarılı** iletisini gördüğünüzde **Tamam**’a tıklayın.
   
    ![Azure VM için yapılan yapılandırma seçimlerini gösteren Özet sayfası ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)
8. Azure sanal makineyi oluştururken, hub menüsündeki **Sanal Makineler** altında ilerleme durumunu izleyebilirsiniz. 

## <a name="connect-to-the-virtual-machine-and-sign-on"></a>Sanal makineye bağlanma ve oturum açma
1. Hub menüsünde **Sanal Makineler**’e tıklayın.
2. Listeden sanal makineyi seçin.
3. Sanal makine için dikey pencere üzerinde **Bağlan**’a tıklayın. Bu , makinenize bağlanmak için kısayola benzeyen bir Uzak Masaüstü Protokol dosyası (.rdp dosyası) oluşturur ve indirir. Kolay erişim için dosyayı masaüstünüze kaydetmek isteyebilirsiniz. VM'nize bağlanmak için bu dosyayı **Açın** .
   
    ![VM’nize nasıl bağlanacağınızı gösteren Azure portal ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/connect.png)
4. .rdp dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)
5. Windows Güvenlik penceresinde, VM oluşturduğunuz sırada oluşturduğunuz yerel hesabın kullanıcı adı ve parolasını yazın. Kullanıcı adı *vmname*&#92;*username* olarak girilir ve sonra **Tamam**’a tıklayın.
   
    ![VM adı, kullanıcı adı ve parolayı girme ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/credentials.png)
6. Sertifikanın doğrulanamadığına ilişkin uyarı alırsınız. Bu normaldir. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)

Bağlanmayı denediğinizde sorun yaşıyorsanız, bkz. [Windows tabanlı Azure Virtual Machine’e Uzak Masaüstü Bağlantı Sorunlarını Giderme](virtual-machines-windows-troubleshoot-rdp-connection.md).

Artık başka bir sunucuyla yaptığınız gibi sanal makine ile çalışabilirsiniz.

## <a name="optional-stop-the-vm"></a>İsteğe bağlı: VM’yi durdurma
Gerçekte kullanmadığınız durumda ücret ödememeniz için VM’yi durdurmak iyi bir fikirdir. **Durdur** ve ardından **Evet**’e tıklamanız yeterlidir.

![Bir VM’yi durdurmaya yönelik düğmeyi gösteren ekran görüntüsü](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)

Tekrar kullanmaya hazır olduğunuzda VM’yi yeniden başlatmak için **Başlat** düğmesine tıklamanız yeterlidir.

## <a name="next-steps"></a>Sonraki adımlar
* Yeni VM’nizi [IIS yükleyerek](virtual-machines-windows-hero-role.md) deneyebilirsiniz. Bu öğretici ayrıca bir ağ güvenlik grubu (NSG) kullanarak gelen web trafiğinde bağlantı noktası 80’i açmayı gösterir. 
* Ayrıca [Windows PowerShell kullanarak bir VM oluşturabilir](virtual-machines-windows-ps-create.md) veya Azure CLI kullanarak [Linux sanal makinesi oluşturabilirsiniz](virtual-machines-linux-quick-create-cli.md).
* Otomatik dağıtımlarla ilgileniyorsanız bkz. [Resource Manager şablonu kullanarak Windows sanal makine oluşturma](virtual-machines-windows-ps-template.md).




<!--HONumber=Nov16_HO2-->


