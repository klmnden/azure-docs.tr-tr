---
title: Klasik Azure VM oturumunu | Microsoft Docs
description: Klasik dağıtım modeli kullanılarak oluşturulmuş bir Windows sanal makine oturum açmak için Azure portalını kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 3c1239ed-07dc-48b8-8b3d-dc8c5f0ab20e
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 87ecc65d2d4802ae826f3260b66b26e0bbe414e6
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012368"
---
# <a name="log-on-to-a-windows-virtual-machine-using-the-azure-portal"></a>Azure Portal kullanarak bir Windows sanal makinesinde oturum açma
Azure portalında kullandığınız **Bağlan** bir Uzak Masaüstü oturumu ve bir Windows VM oturumunu Başlat düğmesi.

Bir Linux VM bağlanmak istiyor? Bkz: [Linux çalıştıran bir sanal makine için oturum açma](../../linux/mac-create-ssh-keys.md).

<!--
Deleting, but not 100% sure
Learn how to [perform these steps using new Azure portal](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager modelini kullanarak bir VM'de oturum açma hakkında daha fazla bilgi için bkz: [burada](../connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma
1. Azure Portal’da oturum açın.
2. Erişmek istediğiniz sanal makineye tıklayın. Ad içinde listelenen **tüm kaynakları** bölmesi.

    ![Sanal makine konumları](./media/connect-logon/azureportaldashboard.png)

1. Tıklatın **Bağlan** düğmesi sanal makine özellikleri sayfasında. 
2. İçinde **sanal makineye Bağlan** sayfasında uygun seçenekleri seçin tutmak ve tıklayın **karşıdan RDP dosyasını**.
2. İndirilen RDP dosyasını açın ve tıklatın **Bağlan** istendiğinde. 
2. `.rdp` dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/connect-logon/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinedeki bir hesabın kimlik bilgilerini yazın ve ardından **Tamam**'a tıklayın.
   
     **Yerel hesap** - bu genellikle, sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasıdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  
   
    **Etki alanına katılmış VM** - VM bir etki alanına aitse, kullanıcı adını *Etkialanı*& #92;*Kullanıcıadı* biçiminde girin. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.
   
    **Etki alanı denetleyicisi** - VM bir etki alanı denetleyicisiyse, etki alanı için etki alanının yönetici hesabına ait kullanıcı adını ve parolasını yazın.
4. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/connect-logon/cert-warning.png)

## <a name="next-steps"></a>Sonraki adımlar
* Varsa **Bağlan** düğmesi etkin değil veya Uzak Masaüstü Bağlantısı ile ilgili başka sorununuz, yapılandırmayı sıfırlamayı deneyin. tıklatın **sıfırlama uzaktan erişim** sanal makine panosundan.

    ![Sıfırlama uzaktan erişim](./media/connect-logon/virtualmachine_dashboard_reset_remote_access.png)

* Parolanızı sorunlar için onu sıfırlamayı deneyin. Tıklatın **parola sıfırlama** sol kenarına sanal makine panoyu altında **destek + sorun giderme**.

    ![Parola sıfırlama](./media/connect-logon/virtualmachine_dashboard_reset_password.png)

Bu ipuçları çalışmıyor veya ne bkz ihtiyacınız olmayan [sorun giderme Uzak Masaüstü bağlantıları için Windows tabanlı Azure sanal makine](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bu makale ortak sorunları tanılama ve çözme boyunca size yol gösterecektir.
