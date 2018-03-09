---
title: "Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir Azure portalı | Microsoft Docs"
description: "Sanal Ağ eşlemesi ile sanal ağları bağlamayı öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ac0c1033546758a77b43298a5fa8cba5f5204650
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Azure Portalı'nı kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, oluşturma ve iki sanal ağlar arasındaki eşleme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İki sanal ağ oluşturma
> * Sanal ağlar arasında eşleme oluşturma
> * Test eşleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Aşağıdaki resimde gösterildiği gibi girin *myVirtualNetwork1* için **adı**, *10.0.0.0/16* için **adres alanı**,  **myResourceGroup** için **kaynak grubu**, *Subnet1* alt ağ için **adı**, alt ağ için 10.0.0.0/24 **adres aralığı** seçin bir **konumu** ve **abonelik**kalan Varsayılanları kabul edin ve ardından **oluşturma**:

    ![Sanal ağ oluşturma](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Adım 1-3 aşağıdaki değişikliklerle yeniden tamamlayın:
    - **Ad**: *myVirtualNetwork2*
    - **Kaynak grubu**: seçin **var olanı kullan** ve ardından **myResourceGroup**.
    - **Adres alanı**: *10.1.0.0/16*
    - **Alt ağ adres aralığı**: *10.1.0.0/24*

    Adres ön eki için *myVirtualNetwork2* sanal ağ adres alanı ile çakışmaması *myVirtualNetwork1* sanal ağ. Çakışan adres alanlarının ile sanal ağlar eş olamaz.

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

1. Azure portalı üstündeki arama kutusuna yazmaya başlayın *MyVirtualNetwork1*. Zaman **myVirtualNetwork1** arama sonuçlarında görünür.
2. Seçin **eşlemeler**altında **ayarları**ve ardından **+ Ekle**, aşağıdaki resimde gösterildiği gibi:

    ![Eşlemesi oluşturma](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Girin veya aşağıdaki resimde gösterilen bilgiler seçin ve ardından seçin **Tamam**. Seçilecek *myVirtualNetwork2* sanal ağ, select **sanal ağ**seçeneğini belirleyip *myVirtualNetwork2*.

    ![Eşleme ayarları](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    **EŞLİĞİ durumu** olan *başlatılan*, aşağıdaki resimde gösterildiği gibi:

    ![Eşleme durumu](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Durum görmüyorsanız, tarayıcınızı yenileyin.

4. Arama *myVirtualNetwork2* sanal ağ. Arama sonuçlarında döndürüldüğünde, onu seçin.
5. Adım 1-3 yeniden, aşağıdaki değişiklikleri tamamlayın ve ardından **Tamam**:
    - **Ad**: *myVirtualNetwork2 myVirtualNetwork1*
    - **Sanal ağ**: *myVirtualNetwork1*

    **EŞLİĞİ durumu** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork2 myVirtualNetwork1* gelen eşleme *başlatılan* için *bağlandı.* Sanal Ağ eşlemesi değil tam olarak kurulduğunda her iki sanal ağ eşleme durumu kadar *bağlandı.* 

Eşlemeler iki sanal ağ arasında ancak geçişli değil. Böylece, örneğin, ayrıca eş istemeniz durumunda *myVirtualNetwork2* için *myVirtualNetwork3*, ek sanal ağlar arasında eşlemeyi oluşturmanıza gerek *myVirtualNetwork2* ve *myVirtualNetwork3*. Olsa bile *myVirtualNetwork1* ile eşlenen *myVirtualNetwork2*, kaynakları içinde *myVirtualNetwork1* yalnızca kaynaklara erişim  *myVirtualNetwork3* varsa *myVirtualNetwork1* de ile eşlenen *myVirtualNetwork3*. 

Eşleme önce üretim sanal ağlar, baştan sona ile öğrenmeniz olduğunu önerilir [eşleme genel bakış](virtual-network-peering-overview.md), [eşliği yönetme](virtual-network-manage-peering.md), ve [sanal ağ sınırları ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Bu makalede bir iki sanal ağ aynı abonelik ve konum arasında eşleme gösterir ancak sanal ağlarda ayrıca eş [farklı bölgelerde](#register) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#portal). Ayrıca oluşturabilirsiniz [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) eşliği ile.

## <a name="test-peering"></a>Test eşleme

Bir eşleme aracılığıyla farklı sanal ağlardaki sanal makineler arasındaki ağ iletişimi sınamak için her alt ağda bir sanal makine dağıtın ve sanal makineler iletişim. 

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki adımda aralarında iletişim doğrulamak için bir sanal makinenin her sanal ağ oluşturun.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımları seçtiğiniz varsayın **Windows Server 2016 Datacenter**. 
3. İçin aşağıdaki bilgileri girin veya seçin **Temelleri**seçeneğini belirleyip **Tamam**:
    - **Ad**: *myVm1*
    - **Kaynak grubu**: seçin **var olanı kullan** ve ardından *myResourceGroup*.
    - **Konum**: seçin *Doğu ABD*.

    **Kullanıcı adı** ve **parola** girdiğiniz bir sonraki adımda kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. **Konumu** ve **abonelik** seçili aynı sanal ağ olan abonelik ve konum olarak olması gerekir. Bu sanal ağ içinde oluşturuldu, ancak bu makale için aynı kaynak grubu seçili aynı kaynak grubunu seçin zorunlu değildir.
4. VM boyutu altında seçin **bir boyutu seçin**.
5. İçin aşağıdaki bilgileri girin veya seçin **ayarları**seçeneğini belirleyip **Tamam**:
    - **Sanal ağ**: emin **myVirtualNetwork1** seçilir. Aksi takdirde, seçin **sanal ağ** ve ardından **myVirtualNetwork1** altında **Seç sanal ağ**.
    - **Alt ağ**: emin **Subnet1** seçilir. Aksi takdirde, seçin **alt** ve ardından **Subnet1** altında **alt ağ seçin**, aşağıdaki resimde gösterildiği gibi:
    
        ![Sanal makine ayarları](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Altında **oluşturma** içinde **Özet**seçin **oluşturma** sanal makine dağıtımı başlatmak için.
7. 1-6 yeniden adımları ancak girmeniz *myVm2* için **adı** seçin ve sanal makine *myVirtualNetwork2* için **sanal ağ**.

Atanan azure *10.0.0.4* özel IP adresi olarak *myVm1* sanal makine ve 10.1.0.4 için *myVm2* sanal makine, ilk kullanılabilir IP olduğundan adresleri *Subnet1* , *myVirtualNetwork1* ve *myVirtualNetwork2* sanal ağlar, sırasıyla.

Sanal makineler oluşturmak için birkaç dakika sürebilir. Her iki sanal makine oluşturulana kadar kalan adımlara devam etmeyin.

### <a name="test-virtual-machine-communication"></a>Test sanal makinesi iletişimi

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVm1*. Zaman **myVm1** arama sonuçlarında görünür.
2. Uzak Masaüstü bağlantı oluşturmak *myVm1* seçerek sanal makine **Bağlan**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal makineye bağlanma](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. VM'e bağlanmak için indirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.
4. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan), ardından **Tamam**.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için.
6. Bir sonraki adımda ping ile iletişim kurmak için kullanılan *myVm2* sanal makineden *myVmWeb* sanal makine. Varsayılan olarak Windows Güvenlik Duvarı üzerinden engellenir ICMP ping komutunu kullanır. ICMP, bir komut isteminden aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Ping test etmek için bu makaledeki kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

7. Bağlanmak için *myVm2* sanal makine, bir komut isteminden aşağıdaki komutu girin *myVm1* sanal makine:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Ping etkinleştirilmiş olduğundan *myVm1*, bu IP adresine göre şimdi ping:

    ```
    ping 10.0.0.4
    ```
    
    Dört yanıt alırsınız. Sanal makinenin adına göre ping (*myVm1*), IP adresini yerine ping, çünkü başarısız *myVm1* bir bilinmeyen ana bilgisayar adıdır. Azure'nın varsayılan ad çözümlemesi, aynı sanal ağdaki sanal makineler arasında ancak farklı sanal ağlardaki sanal makineler arasında değil çalışır. Sanal ağlar arasında adları çözümlemek için şunları yapmanız gerekir [kendi DNS sunucusu dağıtma](virtual-networks-name-resolution-for-vms-and-role-instances.md) veya [Azure DNS özel etki alanları](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Hem sizin RDP oturumların bağlantısını kesmek *myVm1* ve *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil: 

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

**<a name="register"></a>Genel sanal ağ eşleme Önizleme için kaydolun**

Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Sanal ağlar farklı bölgelerde şu anda önizlemede eşleme. Bkz: [sanal ağı güncelleştirmelerini](https://azure.microsoft.com/updates/?product=virtual-network) bölgeleri için kullanılabilir. Sanal ağlar bölgeler arasında eş için önce önizleme için kaydetmeniz gerekir. Portalı kullanarak kaydedilemiyor ancak kullanarak kaydolabilirsiniz [PowerShell](tutorial-connect-virtual-networks-powershell.md#register) veya [Azure CLI](tutorial-connect-virtual-networks-cli.md#register). Sanal ağlar farklı bölgelerde özellik kaydetmeden önce eş çalışırsanız, başarısız eşleme.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağlara bağlanmak nasıl öğrendiniz.

Kendi bilgisayarınızı VPN üzerinden sanal bir ağa bağlayın ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim devam edin.

> [!div class="nextstepaction"]
> [Bilgisayarınızı bir sanal ağa bağlan](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
