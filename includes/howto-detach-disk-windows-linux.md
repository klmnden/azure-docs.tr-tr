---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/25/2018
ms.author: cynthn
ms.openlocfilehash: c97c94492417dcc87d34751908f1766393ad37ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61476262"
---
Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Ayırdığınız disk sanal makineden kaldırılır ancak Azure depolama hesabından silinmez.

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.  

> [!NOTE]
> Bir işletim sistemi diskini ayırmak için sanal makineyi silmeniz gerekir.
>

## <a name="find-the-disk"></a>Diski bulma
Diskin adını bilmiyorsanız veya ayırma işleminden önce doğrulamak istiyorsanız aşağıdaki adımları takip edin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Sanal Makineler**'e tıklayın ve ilgili VM'yi seçin.

3. Sanal makine panosunun sol kenarındaki **Ayarlar**'ın altından **Diskler**'e tıklayın.

 Sanal makine panosunda tüm ekli disklerin adı ve türü listelenir. Örneğin, bu ekran bir işletim sistemi (OS) diskine ve bir veri diskine sahip bir sanal makineyi göstermektedir:

    ![Veri diskini bulma](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a>Diski ayırma
1. Azure portalında **Sanal Makineler**'e ve ardından ayırmak istediğiniz veri diskinin bulunduğu sanal makinenin adına tıklayın.

2. Sanal makine panosunun sol kenarındaki **Ayarlar**'ın altından **Diskler**'e tıklayın.

3. Ayırmak istediğiniz diske tıklayın.

  ![Ayrılacak diski tanımlayın](./media/howto-detach-disk-windows-linux/disklist.png)

4. Komut çubuğundan **Ayır**'a tıklayın.

  ![Ayırma komutunu bulun](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. Diski ayırmak için onay penceresinde **Evet**'e tıklayın.

  ![Diski ayırmayı onaylayın](./media/howto-detach-disk-windows-linux/confirmdetach.png)

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.
