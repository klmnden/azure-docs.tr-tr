---
title: "Azure - Portal sanal ağ oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ türlerde özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 8b6a4abdb7677417462392feade0c7cfdf99246f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ oluşturma

Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki sanal makine sanal ağa dağıtın ve özel olarak aralarında ve internet ile iletişim kurar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Girin veya seçin, aşağıdaki bilgileri ve ardından **oluşturma**:
    - **Ad**: *myVirtualNetwork*
    - **Adres alanı**: varsayılanı kabul edin. Adres alanı CIDR gösteriminde belirtilir.
    - **Abonelik**: aboneliğinizi seçin.
    - **Kaynak grubu**: seçin **Yeni Oluştur** ve girin *myResourceGroup*.
    - **Konum**: seçin * Doğu ABD **.
    - **Alt ağ, adını**: varsayılan kabul edin.
    - **Alt ağ, adres aralığı**: varsayılan kabul edin.
    - **Hizmet uç noktaları**: varsayılan kabul edin.

    ![Sanal ağınız hakkında temel bilgileri girin](./media/quick-create-portal/virtual-network.png)

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal ağ çeşitli özel olarak birbirleriyle ve internet ile iletişim kurmak için Azure kaynaklarını sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir.

1. Seçin **+ kaynak oluşturma** üst, sol üst köşesinde Azure portalı üzerinde bulunamadı.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.
3. Girin ya da, aşağıdaki bilgileri seçin ve ardından **Tamam**:
    - **Ad**: *myVm1*
    - **Kullanıcı adı**: seçtiğiniz bir kullanıcı adı girin.
    - **Parola**: seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
    - **Abonelik**: aboneliğinizi seçin.
    - **Kaynak grubu**: seçin **var olanı kullan** seçip **myResourceGroup**.
    - **Konum**: seçin *Doğu ABD*.

    ![Bir sanal makine hakkında temel bilgileri girin](./media/quick-create-portal/virtual-machine-basics.png)
4. Sanal makine için bir boyut seçin ve ardından **seçin**.
5. Altında **ayarları**, *myVirtualNetwork* için zaten seçilmelidir **sanal ağ**, ancak değilse, seçin **sanal ağ** , ardından *myVirtualNetwork*. Bırakın *varsayılan* için seçilen **alt**ve ardından **Tamam**.

    ![Sanal ağ seçin](./media/quick-create-portal/virtual-machine-network-settings.png)
6. Üzerinde **Özet** sayfasında, **oluşturma** sanal makine dağıtımı başlatmak için. Sanal makine dağıtmak için birkaç dakika sürer. 
7. Tüm adımları 1-6 yeniden, ancak adım 3, sanal makine adı *myVm2*.

## <a name="connect-to-a-virtual-machine"></a>Bir sanal makineye bağlanma

1. Sonra *myVm1* uzaktan oluşturulur, bu kaynağa bağlanın. Azure portalının en üstünde girin *myVm1*. Zaman **myVm1** arama sonuçlarında görünür. Seçin **Bağlan** düğmesi.

    ![Sanal makineye genel bakış](./media/quick-create-portal/virtual-machine-overview.png)

2. Seçtikten sonra **Bağlan** düğmesi, bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden.  
3. İndirilen rdp dosyasını açın. İstenirse, seçin **Bağlan**. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan), ardından Tamam'ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** veya **devam** bağlantı ile devam etmek için.

## <a name="communicate-between-vms"></a>VM'ler arasında iletişim

1. Bir komut isteminden girin `ping myvm2`. Varsayılan olarak ICMP ve ICMP Windows aracılığıyla izin verilmeyen ping kullanır, güvenlik duvarı için Ping başarısız olur. İzin vermek için *myVm2* ping işlemi yapmak için *myVm1* bir sonraki adımda, bir komut isteminden aşağıdaki komutu girin:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

2. Uzak Masaüstü Bağlantısı kapatmak *myVm1*. 

3. Bölümündeki adımları tamamlamanız [bir sanal makineye Bağlan](#connect-to-a-virtual-machine), ancak bağlanmak *myVm2*. Bir komut isteminden girin `ping myvm1`.

    Başarılı bir şekilde ping mümkün *myVm1* sanal makineden *myVm2* sanal makine için:

    - üzerinde Windows Güvenlik Duvarı üzerinden ICMP izin *myVm1* sanal makine önceki adımda.
    - Varsayılan olarak, aynı sanal ağdaki kaynakları arasındaki tüm ağ trafiğini Azure sağlar.

## <a name="communicate-to-the-internet"></a>Internet ile iletişim kurmak

1. Halen bağlı sırasında *myVm2* bir komut isteminden sanal makine girin `ping bing.com`.

    Aratıp dört yanıt alırsınız. 

    Başarılı bir şekilde bir Internet kaynağından ping mümkün *myVm2* sanal makine herhangi bir sanal makine giden internet, varsayılan olarak iletişim kurabildiklerinden.

2. Uzak Masaüstü oturumu çıkın.

## <a name="communicate-from-the-internet"></a>İnternet'ten iletişim

1. Genel IP adresi al *myVm1* sanal makine. Altında gösterilen resim 1. adımda [bir sanal makineye Bağlan](#connect-to-a-virtual-machine), bir ortak IP adresini bakın. Aşağıdaki resimde adresidir *13.90.241.247*. Farklı sanal makineniz için adresidir. 

2. Genel IP adresi, bilgisayarınızdan ping, *myVm1* sanal makine. ICMP Windows Güvenlik Duvarı üzerinden açık olsa bile Ping başarısız olur.

    Windows sanal makineler, bağlantı noktası 3389, Uzak Masaüstü bağlantılarında dışındaki tüm trafik varsayılan olarak Azure tarafından engellendiği için Ping başarısız olur. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil:

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ bir alt ağ ile dağıtılabilir. Birden çok alt ağ ile özel bir sanal ağ oluşturmayı öğrenmek için özel bir sanal ağ oluşturmak için öğretici devam edin.

> [!div class="nextstepaction"]
> [Özel bir sanal ağ oluşturma](virtual-networks-create-vnet-arm-pportal.md)
