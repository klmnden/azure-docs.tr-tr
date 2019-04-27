---
title: include dosyası
description: include dosyası
services: virtual-machines
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 05/09/2018
ms.date: 06/04/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 9c4910d9ec1fdf651b1bc29955a4e1de2a775f7a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60576161"
---
1. Sanal makine özellikleri sayfasında **Bağlan** düğmesine tıklayın. 
2. İçinde **sanal makineye bağlanma** sayfasında uygun seçenekleri seçin tutun ve tıklayın **indirme RDP dosyası**.
2. İndirilen RDP dosyasını açın ve istendiğinde **Bağlan**’a tıklayın. 
2. `.rdp` dosyasının bilinmeyen bir yayımcıdan geldiğine ilişkin bir uyarı alırsınız. Bu normaldir. Uzak Masaüstü penceresinde, devam etmek için **Bağlan**’a tıklayın.

    ![Bilinmeyen yayımcıya ilişkin uyarı ekran görüntüsü](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. **Windows Güvenliği** penceresinde **Diğer seçenekler**'i ve ardından **Başka bir hesap kullanın**'ı seçin. Sanal makinedeki bir hesabın kimlik bilgilerini yazın ve ardından **Tamam**'a tıklayın.

     **Yerel hesap** - bu genellikle, sanal makineyi oluşturduğunuzda belirttiğiniz yerel hesap kullanıcı adı ve parolasıdır. Bu durumda, etki alanı sanal makinenin adıdır ve *vmadı*&#92;*kullanıcıadı* olarak girilir.  

    **Etki alanına katılmış VM** - VM bir etki alanına aitse, kullanıcı adını *Etkialanı*& #92;*Kullanıcıadı* biçiminde girin. Hesabın ayrıca, Yöneticiler grubunda olması ya da VM’ye uzaktan erişim ayrıcalıkları verilmiş olması gerekir.

    **Etki alanı denetleyicisi** - VM bir etki alanı denetleyicisiyse, etki alanı için etki alanının yönetici hesabına ait kullanıcı adını ve parolasını yazın.
4. Sanal makine kimliğini doğrulamak için **Evet**’e tıklayın ve oturum açmayı tamamlayın.

   ![VM kimliğini doğrulamaya ilişkin iletiyi gösteren ekran görüntüsü](./media/virtual-machines-log-on-win-server/cert-warning.png)
