---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 842083f0f69552fb9076423386353dbb4dae73b5
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
1. Tıklatın **Bağlan** düğmesi sanal makine özellikleri sayfasında. 
2. İçinde **sanal makineye Bağlan** sayfasında uygun seçenekleri seçin tutmak ve tıklayın **karşıdan RDP dosyasını**.
2. İndirilen RDP dosyasını açın ve tıklatın **Bağlan** istendiğinde. 
2. `.rdp` dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.
   
    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinedeki bir hesabın kimlik bilgilerini yazın ve ardından **Tamam**'a tıklayın.
   
     **Yerel hesap** - bu genellikle, sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasıdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  
   
    **Etki alanına katılmış VM** - VM bir etki alanına aitse, kullanıcı adını *Etkialanı*& #92;*Kullanıcıadı* biçiminde girin. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.
   
    **Etki alanı denetleyicisi** - VM bir etki alanı denetleyicisiyse, etki alanı için etki alanının yönetici hesabına ait kullanıcı adını ve parolasını yazın.
4. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.
   
   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/virtual-machines-log-on-win-server/cert-warning.png)

