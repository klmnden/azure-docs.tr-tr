---
title: "Rota ağ trafiğini - Azure portalı | Microsoft Docs"
description: "Azure Portalı'nı kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 4cc814e1fd3ee8979662695f9dec3dcce335dc2a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir güvenlik duvarı üzerinden alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Bir sanal ağ alt ağı için bir yol tablosu ilişkilendirme
> * Test yönlendirme
> * Yönlendirme sorunlarını giderme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

Varsayılan olarak bir sanal ağdaki tüm alt ağlar arasında trafiği Azure yollar. Azure'nın varsayılan yollar hakkında daha fazla bilgi için bkz: [sistem yolları](virtual-networks-udr-overview.md). Azure'nın varsayılan yönlendirme geçersiz kılmak için rotaları içeren bir yol tablosu oluşturmanız ve bir sanal ağ alt ağı için yol tablosu ilişkilendirebilirsiniz.

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **yol tablosu**.
3. Seçin, **abonelik** ve seçin veya aşağıdaki bilgileri girin ve ardından seçin **oluşturma**:
 
    ![Yol tablosu oluşturma](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Bir yol oluşturma

Bir rota tablosu sıfır veya daha fazla yol içerir. 

1. İçinde *arama kaynakları, hizmetleri ve belgeleri* kutusunda portalın en üstünde, yazmaya başlayın *myRouteTablePublic*. Zaman **myRouteTablePublic** arama sonuçlarında görünür.
2. Altında **ayarları**seçin **yollar** ve ardından **+ Ekle**, aşağıdaki resimde gösterildiği gibi:

    ![Yolu Ekle](./media/tutorial-create-route-table-portal/add-route.png) 
 
4. Altında **Add yolun**seçin veya aşağıdaki bilgileri girin ve ardından **Tamam**:
    - **Rota adı**: *ToPrivateSubnet*
    - **Adres ön eki**: 10.0.1.0/24
    - **Sonraki atlama türü**: seçin **sanal Gereci**.
    - **Sonraki atlama adresi**: 10.0.2.4

    Rota 10.0.2.4 IP adresiyle bir ağ sanal gereç aracılığıyla 10.0.1.0/24 adres ön eki giden tüm trafiği yönlendirir. Ağ sanal gereç ve alt ağ belirtilen adres ön eki ile daha sonraki adımlarda oluşturulur. Rota yönlendirme, doğrudan alt ağlar arasında trafiği yönlendirir Azure'nın varsayılan değerini geçersiz kılar. Her yol, bir sonraki atlama türü belirtir. Sonraki atlama türü trafiği yönlendirmek nasıl Azure bildirir. Bu örnekte, sonraki atlama türü olan *değerinin VirtualAppliance*. Kullanılabilir tüm sonraki atlama türlerini Azure ve bunların ne zaman kullanılacağı hakkında daha fazla bilgi için bkz: [türleri'sonraki atlama](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Bir alt ağ için bir yol tablosu ilişkilendirme

Bir alt ağ için bir yol tablosu ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir:

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Altında **sanal ağ oluştur**, seçin veya aşağıdaki bilgileri girin ve sonra seçin **oluşturma**:
    
    - **Ad**: *myVirtualNetwork*
    - **Adres alanı**: *10.0.0.0/16*
    - **Abonelik**: aboneliğinizi seçin.
    - **Kaynak grubu**: seçin **var olanı kullan** seçip **myResourceGroup**.
    - **Konum**: seçin *Doğu ABD*
    - **Alt ağ adı**: *ortak*
    - **Adres aralığı:** *10.0.0.0/24*

4. İçinde **arama kaynakları, hizmetleri ve belgeleri** kutusunda portalın en üstünde, yazmaya başlayın *myVirtualNetwork*. Zaman **myVirtualNetwork** arama sonuçlarında görünür.
5. İki ek alt ağlar sanal ağa ekleyin. Altında **ayarları**seçin **alt ağlar** ve ardından **+ alt**, aşağıdaki resimde gösterildiği gibi:

    ![Alt ağ ekleme](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Seçin veya aşağıdaki bilgileri girin ve ardından **Tamam**:
    - **Ad**: özel
    - **Adres alanı**: *10.0.1.0/24*

7. 5 ve 6 yeniden, aşağıdaki bilgileri sağlayarak adımları tamamlayın:
    - **Ad**: DMZ
    - **Adres alanı**: *10.0.2.0/24*

Bir yol tablosu sıfır veya daha fazla alt ağlara ilişkilendirebilirsiniz. Bir alt ağ için ilişkili sıfır veya bir yol tablosu olabilir. Bir alt ağdan giden trafik, Azure'nın varsayılan yollar ve bir yol tablosu bir alt ağa ilişkilendirmek için eklediğiniz tüm özel yollar göre yönlendirilir. İlişkilendirme *myRouteTablePublic* yol tablosuna *ortak* alt ağ:

1. **MyVirtualNetwork - alt ağlar** kutusu, önceki adımı tamamladıktan sonra görüntülenir. Altında **ayarları**seçin **alt ağlar** ve ardından **ortak**.
2. Aşağıdaki resimde gösterildiği gibi seçin **yol tablosu**seçeneğini belirleyip **MyRouteTablePublic**.

    ![Yol tablosu ilişkilendirme](./media/tutorial-create-route-table-portal/associate-route-table.png) 

3. **Kaydet**’i seçin.

## <a name="test-routing"></a>Test yönlendirme

Yönlendirme sınamak için önceki adımda oluşturduğunuz rota üzerinden yönlendiren ağ sanal gereç olarak hizmet veren bir sanal makine oluşturacaksınız. Ağ sanal gereç oluşturduktan sonra bir sanal makineye dağıtacaksınız *ortak* ve *özel* alt ağlar. Ardından gelen trafiği yönlendirmek *ortak* alt ağına *özel* alt ağ sanal gereç aracılığıyla.

### <a name="create-a-network-virtual-appliance"></a>Ağ sanal uygulaması oluşturma

Önceki adımda, bir ağ sanal gereç sonraki atlama türü belirtilmiş bir yol oluşturuldu. Bir ağ uygulaması çalıştıran bir sanal makine, genellikle bir ağ sanal gereç da adlandırılır. Üretim ortamlarında, dağıttığınız ağ sanal gereç önceden yapılandırılmış bir sanal makine görülür. Birçok ağ sanal Gereçleri edinilebilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Bu makalede, temel bir sanal makine oluşturulur.

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımları seçtiğiniz varsayın **Windows Server 2016 Datacenter**. 
3. İçin aşağıdaki bilgileri girin veya seçin **Temelleri**seçeneğini belirleyip **Tamam**:
    - **Name**: *myVmNva*
    - **Kaynak grubu**: seçin **var olanı kullan** ve ardından *myResourceGroup*.
    - **Konum**: seçin *Doğu ABD*.

    **Kullanıcı adı** ve **parola** girdiğiniz bir sonraki adımda kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. **Konumu** ve **abonelik** seçili aynı sanal ağ olan abonelik ve konum olarak olması gerekir. Bu sanal ağ içinde oluşturuldu, ancak bu öğretici için aynı kaynak grubu seçili aynı kaynak grubunu seçin zorunlu değildir.
4. VM boyutu altında seçin **bir boyutu seçin**.
5. İçin aşağıdaki bilgileri girin veya seçin **ayarları**seçeneğini belirleyip **Tamam**:
    - **Sanal ağ**: emin **myVirtualNetwork** seçilir. Aksi takdirde, seçin **sanal ağ**seçeneğini belirleyip **myVirtualNetwork** altında **Seç sanal ağ**.
    - **Alt ağ**: seçin **alt** ve ardından **DMZ** altında **alt ağ seçin**.
    - **Genel IP adresi**: seçin **genel IP adresi** seçip **hiçbiri** altında **genel IP adresi seçin**. Bunun için Internet'ten bağlanmayacaktır beri ortak IP adresi bu sanal makineye atanır.
6. Altında **oluşturma** içinde **Özet**seçin **oluşturma** sanal makine dağıtımı başlatmak için.

Sanal makine oluşturmak için birkaç dakika sürer. Azure sanal makine oluşturma tamamlandıktan ve sanal makine hakkında bilgi içeren bir kutu açar kadar sonraki adıma devam etmeyin.

Azure sanal makine oluşturduğunuzda, aynı zamanda oluşturulan bir [ağ arabirimi](virtual-network-network-interface.md) içinde *DMZ* alt ağ ve ağ arabirimin sanal makineye bağlı. Ağ arabirimine atanmış olmayan herhangi bir IP adresi için hedeflenen trafiğini her Azure ağ arabirimi için IP iletimini yeniden etkinleştirilmesi gerekir.

1. Bu, altında oluşturulduktan sonra sanal makine için açılan kutusunda **ayarları**seçin **ağ**ve ardından **myvmnva158** (ağ arabirimi Azure Sanal makineniz sonra farklı bir numara için oluşturulan **myvmnva**), aşağıdaki resimde gösterildiği gibi:

    ![Sanal makine ağı](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

    İçindeki ağ sanal gerecin oluşturduğunuzda *DMZ* alt ağ, Azure otomatik olarak oluşturulan ağ arabirimi, ağ arabirimin sanal makineye bağlı ve özel IP adresi atanmış  *10.0.2.4* ağ arabirimi. 

2. Altında **ayarları**seçin **IP yapılandırmaları**seçin **etkin** için **IP iletimini**ve ardından **Kaydet** , aşağıdaki resimde gösterildiği gibi:

    ![IP iletimini etkinleştirme](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu trafiğinden doğrulamak için iki sanal makine sanal ağ oluşturma *ortak* alt ağ için yönlendirilir *özel* sonraki adımda ağ sanal gereç aracılığıyla alt ağ.

1-6 adımlarını tamamlamanız [ağ sanal gereç oluşturma](#create-a-network-virtual-appliance). Adım 3 ve 5, aşağıdaki değişiklikler dışında aynı ayarları kullanır:

|Sanal makine   |Ad      |Alt ağ      | Genel IP adresi     |
|---------         |--------- | -----------|---------              |
|Sanal makine 1 | myVmWeb  | Genel     | Portal varsayılanı kabul edin |
|sanal makine 2 | myVmMgmt | Özel    | Portal varsayılanı kabul edin |

Oluşturabileceğiniz *myVmMgmt* sanal Azure oluşturduğu sırada makine *myVmWeb* sanal makine. Her iki sanal makine oluşturma Azure tamamlanana kadar aşağıdaki adımlarla devam etmeyin.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Bir ağ sanal gereç yoluyla trafiği yönlendirme

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVmMgmt*. Zaman **myVmMgmt** arama sonuçlarında görünür.
2. Uzak Masaüstü bağlantı oluşturmak *myVmMgmt* seçerek sanal makine **Bağlan**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal makineye bağlanma](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. VM'e bağlanmak için indirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.
4. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan), ardından **Tamam**.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için.
6. Bir sonraki adımda tracert.exe komut yönlendirme test etmek için kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedildi ICMP kullanır. ICMP, bir komut isteminden aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Bu makalede yönlendirme test etmek için tracert kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.
7. Sanal makinenin ağ arabirimi için IP iletimini Azure içindeki etkin [etkinleştirmek IP fowarding](#enable-ip-forwarding). Sanal makinede işletim sistemi ya da sanal makinede çalışan bir uygulama aynı zamanda ağ trafiğini iletebilir olması gerekir. Bir üretim ortamında Ağ sanal gereç dağıttığınızda, Gereci genellikle filtreleri, günlükleri veya trafiği iletmeden önce başka bir işlevi gerçekleştirir. Bu makalede ancak, işletim sisteminin yalnızca aldığı tüm trafiği iletir. İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva* yer alan aşağıdaki adımları tamamlayarak *myVmMgmt* sanal makine:

    Uzak Masaüstü ' *myVmNva* bir komut isteminden aşağıdaki komutu ile bir sanal makine:

    ``` 
    mstsc /v:myvmnva
    ```
    
    İşletim sistemi içinde iletme IP etkinleştirmek için PowerShell içinde aşağıdaki komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Ayrıca Uzak Masaüstü oturumu bağlantısını keser sanal makineyi yeniden başlatın.
8. Halen bağlı sırasında *myVmMgmt* sanal makine, sonra *myVmNva* sanal makine yeniden başlatıldığında, Uzak Masaüstü oturumu oluşturmak *myVmWeb* sanal makine aşağıdaki komutla:

    ``` 
    mstsc /v:myVmWeb
    ```
    
    ICMP, bir komut isteminden aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

9. Ağ trafiği yönlendirmesini test etmek için *myVmMgmt* sanal makineden *myVmWeb* sanal makine, bir komut isteminden aşağıdaki komutu girin:

    ```
    tracert myvmmgmt
    ```

    Yanıt aşağıdaki örneğe benzer:
    
    ```
    Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    İlk atlama ağ sanal gereç ait özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmMgmt* sanal makine. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.
10.  Uzak Masaüstü oturumu kapatmak *myVmWeb* , hala bağlı olarak bırakır, sanal makine *myVmMgmt* sanal makine.
11. Ağ trafiği yönlendirmesini test etmek için *myVmWeb* sanal makineden *myVmMgmt* sanal makine, bir komut isteminden aşağıdaki komutu girin:

    ```
    tracert myvmweb
    ```

    Yanıt aşağıdaki örneğe benzer:

    ```
    Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Trafik, doğrudan yönlendirilir görebilirsiniz *myVmMgmt* sanal makineye *myVmWeb* sanal makine. Varsayılan olarak, Azure yollar doğrudan alt ağlar arasında trafiği.
12. Uzak Masaüstü oturumu kapatmak *myVmMgmt* sanal makine.

## <a name="troubleshoot-routing"></a>Yönlendirme sorunlarını giderme

Önceki adımlarda öğrenilen Azure, isteğe bağlı olarak, kendi yollar geçersiz kılabilir varsayılan yolların geçerlidir. Bazı durumlarda, olmasını beklediğiniz gibi trafik yönlendirilemeyebilir. Kullanabileceğiniz [sonraki atlama](../network-watcher/network-watcher-check-next-hop-portal.md) Azure Ağ İzleyicisi yeteneğini Azure iki sanal makineler arasında trafiği yönlendirme nasıl belirlemek için. 

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *Ağ İzleyicisi*. Zaman **Ağ İzleyicisi** arama sonuçlarında görünür.
2. Altında **Ağ Tanılama Araçları**seçin **sonraki atlama**.
3. Gelen trafik yönlendirme test etmek için *myVmWeb* (10.0.0.4) sanal makineye *myVmMgmt* (10.0.1.4) sanal makine, aboneliğinizi seçin, aşağıdaki resimde gösterilen bilgileri girin (, **Ağ arabirimi** adı biraz farklıdır) ve ardından **sonraki atlama**:

    ![Sonraki atlama](./media/tutorial-create-route-table-portal/next-hop.png)  

    **Sonuç** çıkış bildirir, sonraki atlama IP adresi gelen trafiği için *myVmWeb* için *myVmMgmt* 10.0.2.4 olduğu ( *myVmNva* sanal makine) sonraki türü atlama, *değerinin VirtualAppliance*, ve yönlendirme neden rota tablosu olduğunu *myRouteTablePublic*.

Her ağ arabirimi için etkili rotaları, Azure'nın varsayılan yollar ve tanımladığınız yollar birleşimidir. Bir sanal makinede ağ arabirimi için geçerli olan tüm yolları görmek için aşağıdaki adımları tamamlayın:

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVmWeb*. Zaman **myVmWeb** arama sonuçlarında görünür.
2. Altında **ayarları**seçin **ağ**ve ardından **myvmweb369** (Azure sanal makineniz sonrafarklıbirnumaraiçinoluşturulanağarabirimi**myvmweb**).
3. Altında **destek + sorun giderme**seçin **etkili yollar**, aşağıdaki resimde gösterildiği gibi:

    ![Etkili yolları](./media/tutorial-create-route-table-portal/effective-routes.png) 

    Azure'nın varsayılan yolların ve eklediğiniz rota gördüğünüz *myRouteTablePublic* yol tablosu. Azure yol yollar listesinden nasıl seçtiği hakkında daha fazla bilgi için bkz: [Azure seçer bir rota](virtual-networks-udr-overview.md#how-azure-selects-a-route).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil: 

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş bir ağ sanal gereç oluşturuldu. Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](virtual-network-service-endpoints-configure.md#azure-portal)
