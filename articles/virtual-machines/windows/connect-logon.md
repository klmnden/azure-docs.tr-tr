---
title: Windows Server VM’ye bağlanma | Microsoft Belgeleri
description: Azure portal ve Resource Manager dağıtım yöneticisini kullanarak bir Windows VM’ye nasıl bağlanacağınızı ve oturum açacağınızı öğrenin.
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
ms.date: 04/11/2018
ms.author: cynthn
ms.openlocfilehash: 7c495a74ccbcad3ee18147726742ee59b65f1c0e
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Windows çalıştıran bir Azure Virtual Machine’e bağlanma ve oturum açma
Bir Windows masaüstü bilgisayarından Uzak Masaüstü (RDP) oturumu başlatmak için Azure portalında **Bağlan** düğmesini kullanırsınız. Önce sanal makineye bağlandıktan sonra oturum açın.

Bir Mac bilgisayardan bir Windows VM’e bağlanmaya çalışıyorsanız, Mac bilgisayar için [Microsoft Uzak Masaüstü](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417) gibi bir RDP istemcisi yüklemeniz gerekir.

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
1. Önceden yapmadıysanız, [Azure portal](https://portal.azure.com/)da oturum açın
2. Sol menüde **Sanal Makineler**'e tıklayın.
3. Listeden sanal makineyi seçin.
4. Sanal makine için sayfanın üst kısmında tıklatın ![Bağlan düğmesi görüntüsü.](./media/connect-logon/connect.png) tıklayın.
2. İçinde **sanal makineye Bağlan** sayfasında uygun seçenekleri belirtin ve tıklayın **karşıdan RDP dosyasını**.
2. İndirilen RDP dosyasını açın ve tıklatın **Bağlan** istendiğinde. 
2. `.rdp` dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/connect-logon/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinedeki bir hesabın kimlik bilgilerini yazın ve ardından **Tamam**'a tıklayın.
   
     **Yerel hesap** - bu genellikle, sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasıdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  
   
    **Etki alanına katılmış VM** - VM bir etki alanına aitse, kullanıcı adını *Etkialanı*& #92;*Kullanıcıadı* biçiminde girin. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.
   
    **Etki alanı denetleyicisi** - VM bir etki alanı denetleyicisiyse, etki alanı için etki alanının yönetici hesabına ait kullanıcı adını ve parolasını yazın.
4. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Portal'daki **Bağlan** düğmesi gri ise ve Azure’a bir [Express Route](../../expressroute/expressroute-introduction.md) veya [Siteden Siteye VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) aracılığıyla bağlı değilseniz RDP kullanmadan önce IP adresi oluşturmalı ve VM’nize atamalısınız. [Azure’da genel IP adresleri](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) hakkında daha fazlasını okuyabilirsiniz.
   > 
   > 


## <a name="next-steps"></a>Sonraki adımlar
Bağlanmayı denediğinizde sorun yaşıyorsanız, bkz. [Uzak Masaüstü bağlantı sorunlarını giderme](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu makale ortak sorunları tanılama ve çözme boyunca size yol gösterecektir.

