---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 394b242ab46da7821f77e8d008836753f4e358e2
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097734"
---
Bu adımda, el ile yük devretme kümesi Yöneticisi'ni ve SQL Server Management Studio kullanılabilirlik grubu dinleyicisi oluşturun.

1. Birincil çoğaltmasını barındıran düğümü yük devretme kümesi Yöneticisi'ni açın.

2. Seçin **ağları** düğüm ve Not küme ağ adı. Bu ad, PowerShell betik $ClusterNetworkName değişkeninde kullanılır.

3. Küme adını genişletin ve ardından **rolleri**.

4. İçinde **rolleri** bölmesinde, kullanılabilirlik grubunun adına sağ tıklayın ve ardından **kaynak Ekle** > **istemci erişim noktası**.

    ![Kullanılabilirlik grubu için istemci erişim noktası Ekle](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. İçinde **adı** kutusunda, bu yeni bir dinleyici için bir ad oluşturun, **sonraki** iki kez tıkladıktan sonra **son**.  
    Dinleyici veya kaynağı çevrimiçi bu noktada getirmek değil.

6. Tıklayın **kaynakları** sekmesine ve ardından yeni oluşturduğunuz istemci erişim noktası genişletin. 
    Kümenizdeki her bir küme ağı için IP adresi kaynağı görüntülenir. Bu yalnızca Azure çözümünü ise, yalnızca bir IP adresi kaynağı görüntülenir.

7. Aşağıdakilerden birini yapın:

   * Karma bir çözüm yapılandırmak için:

        a. Şirket içi alt ağa karşılık gelen IP adresi kaynağını sağ tıklayın ve ardından **özellikleri**. Ağ adı ve IP adresi adı not edin.

        b. Seçin **statik IP adresi**kullanılmayan IP adresi atayın ve ardından **Tamam**.

   * Yalnızca Azure çözümünü yapılandırmak için:

        a. Azure, alt ağa karşılık gelen IP adresi kaynağını sağ tıklayın ve ardından **özellikleri**.

       > [!NOTE]
       > Dinleyici çakışan bir IP adresi DHCP tarafından seçilen nedeniyle çevrimiçine alınamıyor. daha sonra başarısız olursa, bu Özellikler penceresinde geçerli bir statik IP adresi yapılandırabilirsiniz.
       > 
       > 

       b. Aynı **IP adresi** Özellikler penceresi, değişiklik **IP adresi adı**.  
        Bu ad, PowerShell betiğini $IPResourceName değişkeninde kullanılır. Çözümünüz birden fazla Azure sanal ağları kapsıyorsa, her IP kaynağı için bu adımı yineleyin.
        
<!-- Update_Description: update meta properties -->