---
title: Windows Server VM’ye bağlanma | Microsoft Belgeleri
description: Bağlanma ve Azure portalı ve Resource Manager dağıtım modelini kullanarak bir Windows VM için oturum açma hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 136b9141ccccfedf8d37fa0832b0673495d82417
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159998"
---
# <a name="how-to-connect-and-sign-on-to-an-azure-virtual-machine-running-windows"></a>Nasıl bağlanın ve bir Azure Windows çalıştıran sanal makine için oturum açın
Bir Windows masaüstü bilgisayarından Uzak Masaüstü (RDP) oturumu başlatmak için Azure portalında **Bağlan** düğmesini kullanırsınız. Önce sanal makineye bağlanın ve ardından oturum açın.

Bir Mac bilgisayardan bir Windows VM'ye bağlanmak için gibi Mac için bir RDP istemcisi yüklemeniz gerekecektir [Microsoft Uzak Masaüstü](https://aka.ms/rdmac).

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2. Sol menüden **sanal makineler**.
3. Listeden sanal makineyi seçin.
4. Sanal makine için sayfanın üst kısmındaki seçin **Connect**.
2. Üzerinde **sanal makineye bağlanma** sayfasında, uygun IP adresi ve bağlantı noktası seçin. Çoğu durumda, varsayılan IP adresini ve bağlantı noktası kullanılmalıdır. **RDP dosyasını indir**'i seçin. Just-ın-time ilke kümesini VM varsa, ilk seçmeniz gerekir **erişim isteği** RDP dosyasını yüklemeden önce erişim istemek için düğme. Tam zamanında İlkesi hakkında daha fazla bilgi için bkz: [yalnızca kullanarak sanal makine erişimini yönetme zamanında İlkesi](../../security-center/security-center-just-in-time.md).
2. İndirilen RDP dosyasını açın ve seçin **Connect** istendiğinde. 
2. Bir uyarı alırsınız, `.rdp` bilinmeyen bir yayımcıdan dosyasıdır. Bu beklenen bir durumdur. İçinde **Uzak Masaüstü Bağlantısı** penceresinde **Connect** devam etmek için.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/connect-logon/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinedeki bir hesabın kimlik bilgilerini girin ve ardından **Tamam**.
   
     **Yerel hesap**: Bu genellikle sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasıdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  
   
    **Etki alanına katılmış VM**: VM bir etki alanına aitse, kullanıcı adı biçiminde girin. *etki alanı*&#92;*kullanıcıadı*. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.
   
    **Etki alanı denetleyicisi**: VM bir etki alanı denetleyicisi ise, bu etki alanı için kullanıcı adı ve etki alanı yönetici hesabının parolasını girin.
4. Seçin **Evet** sanal makinenin kimliğini doğrulamak ve oturum açmayı tamamlayın.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Varsa **Connect** düğmesidir portalında grileştirilmiş ve aracılığıyla azure'a bağlanmayan bir [Express Route](../../expressroute/expressroute-introduction.md) veya [siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) bağlantısı oluşturmanız gerekir ve RDP kullanmadan önce sanal Makinenizin genel IP adresi atayın. Daha fazla bilgi için [Azure'da genel IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="connect-to-the-virtual-machine-using-powershell"></a>PowerShell kullanarak sanal makineye bağlanma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell kullanarak ve Azure PowerShell Modülü yüklü kullanarak bağlanabilir `Get-AzRemoteDesktopFile` cmdlet'ini aşağıda gösterildiği gibi.

Bu örnek, benzer istekleri aracılığıyla alma RDP bağlantı hemen başlatılır.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -Launch
```

Ayrıca, gelecekte kullanım için RDP dosyasını kaydedebilir.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -LocalPath "C:\Path\to\folder"
```

## <a name="next-steps"></a>Sonraki adımlar
Zorluk bağlanma varsa, bkz: [Uzak Masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

