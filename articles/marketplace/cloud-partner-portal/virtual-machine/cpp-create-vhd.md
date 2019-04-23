---
title: Azure ile uyumlu bir VHD oluşturmak için Azure Market | Microsoft Docs
description: Azure Marketi'nde bir sanal makine teklifi için bir VHD oluşturma açıklanır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/27/2018
ms.author: pbutlerm
ms.openlocfilehash: 04a1741bbe4e60567a22445c5674ec03b232640c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59793085"
---
# <a name="create-an-azure-compatible-vhd"></a>Azure ile uyumlu bir VHD oluşturun

Bu makalede ayrıntıları Azure Marketi'nde bir sanal makine (VM) teklifi için bir sanal sabit disk (VHD) oluşturmak için adımları gereklidir.  Ayrıca, Uzak Masaüstü Protokolü (RDP) kullanarak, sanal makine için bir boyutunu seçme, en son Windows güncelleştirmelerini yükleme ve VHD görüntüsü genelleştiriliyor gibi çeşitli yönlerine, en iyi yöntemler içerir.  Aşağıdaki bölümlerde, çoğunlukla windows tabanlı VHD'lerde odaklanın; Linux tabanlı bir VHD oluşturma hakkında daha fazla bilgi için bkz. [tarafından Azure destekli dağıtımlarda Linux](../../../virtual-machines/linux/endorsed-distros.md). 

> [!WARNING]
> Önceden yapılandırılmış, desteklenen bir işletim sistemini içeren bir VM oluşturmak için Azure'ı kullanmak için bu konudaki yönergeleri izlemeniz önerilir.  Ardından bu çözümünüz ile uyumlu değilse, onaylanmış bir işletim sistemini kullanan bir şirket içi VM oluşturma ve yapılandırma mümkündür.  Yapılandırma ve açıklandığı gibi karşıya yükleme işlemine hazırlama [Windows VHD veya VHDX yüklemek için hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).


## <a name="select-an-approved-base"></a>Onaylı bir temel seçin
VM görüntünüzün işletim sistemi VHD'si, Windows Server veya SQL Server içeren bir Azure-onaylı bir temel görüntüye dayalı olması gerekir.
Başlamak için aşağıdaki görüntülerden birinden bir VM oluşturmak için Microsoft Azure Portal'da bulunan:

-   Windows Server ([2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016), [2012 R2 Datacenter](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012r2datacenter/), [2012 Datacenter](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2012datacenter/), [2008 R2 SP1](https://azure.microsoft.com/marketplace/partners/microsoft/windowsserver2008r2sp1/))
-   [SQL Server 2014](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)
-   [SQL Server 2012 SP2](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance) (Enterprise, Standard, Web)

> [!TIP]
> Geçerli Azure portalını veya PowerShell'i kullanıyorsanız, 8 Eylül 2014'te yayımlanan ve sonraki Windows Server görüntüleri onaylanır.

Alternatif olarak, Azure onaylı Linux dağıtımları sunmaktadır.  Geçerli bir listesi için bkz [tarafından Azure destekli dağıtımlarda Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).


## <a name="create-vm-in-the-azure-portal"></a>Azure portalında VM oluşturma 

Microsoft [Azure portalında](https://ms.portal.azure.com/), aşağıdaki adımları kullanarak temel bir görüntü oluşturun.

1. Portal, VM teklifi yayımlamak istediğiniz Azure aboneliği için Microsoft hesabı ile oturum açın.
2. Yeni bir kaynak grubu oluşturun ve belirtin, **kaynak grubu adı**, **abonelik**, ve **kaynak grubu konumu**.  Daha fazla rehberlik için bkz [kaynak gruplarını yönetme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
3. Tıklayarak **sanal makineler** içinde sanal makineler Ayrıntılar sayfasını görüntülemek için sol menü. 
4. Bu yeni sayfa tıklayarak **+ Ekle** görüntülenecek **işlem** dikey penceresi.  İlk ekranda VM türü görmüyorsanız, örneğin temel Makinenizin adını arayabilirsiniz:

    ![Yeni VM dikey penceresinde işlem](./media/publishvm_014.png)

5. Uygun sanal görüntüyü seçtikten sonra aşağıdaki değerleri sağlayın:
   * Üzerinde **Temelleri** dikey penceresinde girin bir **adı** alfasayısal karakterler 1-15 arasındaki sanal makine için. (Bu örnekte `DemoVm009`.)
   * Girin bir **kullanıcı adı** ve güçlü **parola**, VM'de yerel hesap oluşturmak için kullanılır.  (Burada `adminUser` kullanılır.)  Parola 8-123 karakter uzunluğunda olmalıdır ve en az şu dört karmaşıklık gereksinimini karşılamalıdır: bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter. Daha fazla bilgi için [kullanıcı adı ve parola gereksinimleri](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-faq#what-are-the-username-requirements-when-creating-a-vm).
   * Oluşturduğunuz kaynak grubunu seçin (burada `DemoResourceGroup`).
   * Bir Azure veri merkezi seçin **konumu** (burada `West US`).
   * Tıklayın **Tamam** bu değerleri kaydedin. 

6. Aşağıdaki öneriler kullanarak dağıtmak için VM boyutunu seçin:
   * VHD'yi şirket içinde geliştirmeyi planlıyorsanız boyut önemli değildir. En küçük VM'lerden birini kullanabilirsiniz.
   * Görüntüyü Azure'da geliştirmeyi planlıyorsanız, seçilen görüntü için önerilen VM boyutlarından birini kullanın.
   * Fiyatlandırma bilgileri için başvurmak **önerilen fiyatlandırma katmanları** portalda görüntülenen Seçici. Yayımcı tarafından sağlanan üç önerilen boyut görüntüler. (Burada, yayımcı Microsoft'tur.)

   ![Yeni sanal makinenin boyut dikey penceresi](./media/publishvm_015.png)

7. İçinde **ayarları** dikey penceresinde ayarlayın **yönetilen diski kullanma** seçeneğini **Hayır**.  Bu, el ile yeni VHD yönetmenize olanak sağlar. ( **Ayarları** dikey penceresinde de seçerek depolama ve ağ seçeneklerini, örneğin, diğer değişiklik değiştirmenizi sağlar **Premium (SSD)** içinde **Disk türü**.)  Tıklayın **Tamam** devam etmek için.

    ![Ayarlar dikey penceresinden yeni VM](./media/publishvm_016.png)

8. Seçimlerinizi gözden geçirmek için **Özet**’e tıklayın. **Doğrulama başarılı** iletisini gördüğünüzde **Tamam**’a tıklayın.

    ![Yeni VM özeti dikey penceresi](./media/publishvm_017.png)

Azure, belirttiğiniz sanal makinesini sağlama başlar.  Tıklayarak ilerleme durumunu izleyebilirsiniz **sanal makineler** sol sekmede.  Oluşturulduktan sonra durumu değişir **çalıştıran**.  Bu noktada, yapabilecekleriniz [sanal makineye bağlanma](./cpp-connect-vm.md).


## <a name="next-steps"></a>Sonraki adımlar

Yeni Azure tabanlı VHD oluşturma zorluk hatayla karşılaştıysanız bkz [VHD oluşturma işlemi sırasında sık karşılaşılan sorunlar](./cpp-common-vhd-creation-issues.md).  Sonraki gerekir, aksi takdirde, [Vm'lere bağlanma](./cpp-connect-vm.md) Azure üzerinde oluşturulur. 
