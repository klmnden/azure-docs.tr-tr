---
title: "Birden çok alt ağı ile - portalı bir Azure sanal ağ oluşturma | Microsoft Docs"
description: "Azure portalını kullanarak birden çok alt ağı ile bir sanal ağ oluşturmayı öğrenin."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 898bdef779282d7312c76696f744b97ec2dfcded
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-portal"></a>Azure portalını kullanarak birden çok alt ağı ile bir sanal ağ oluşturma

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Bir sanal ağ içinde birden fazla alt ağ oluşturmak, böylece filtre uygulayabilirsiniz ağınız segmentlere ayırmak veya denetim alt ağlar arasındaki trafik akışını sağlar. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Sanal ağ oluşturma
> * Alt ağ oluşturma
> * Sanal makineler arasındaki ağ iletişimi test

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Aşağıdaki resimde gösterildiği gibi girin *myVirtualNetwork* için **adı**, *10.0.0.0/16* için **adres alanı**,  **myResourceGroup** için **kaynak grubu**, *ortak* alt ağ için **adı**, alt ağ için 10.0.0.0/24 **adres aralığı**seçin bir **konumu** ve **abonelik**kalan Varsayılanları kabul edin ve ardından **oluşturma**:

    ![Sanal ağ oluşturma](./media/virtual-networks-create-vnet-arm-pportal/create-virtual-network.png)

    **Adres alanı** ve **adres aralığı** CIDR gösteriminde belirtilir. Belirtilen **adres alanı** IP adreslerini 10.0.0.0-10.0.255.254 içerir. **Adres aralığı** içinde bir alt ağ için belirtilen olmalıdır **adres alanı** sanal ağ için tanımlı. Azure DHCP bir alt ağda dağıtılan kaynak için bir alt ağ adres aralığından IP adresleri atar. Azure yalnızca atar adresleri 10.0.0.4-10.0.0.254 içinde dağıtılan kaynaklara **ortak** alt ağ, Azure ilk dört adresleri ayırdığından (Bu örnekte alt ağ için 10.0.0.0-10.0.0.3) ve son adres () Bu örnekte alt ağ için 10.0.0.255) her alt ağda.

## <a name="create-a-subnet"></a>Alt ağ oluşturma

1. İçinde **arama kaynakları, hizmetleri ve belgeleri** kutusunda portalın en üstünde, yazmaya başlayın *myVirtualNetwork*. Zaman **myVirtualNetwork** arama sonuçlarında görünür.
2. Seçin **alt ağlar** ve ardından **+ alt**, aşağıdaki resimde gösterildiği gibi:

     ![Bir alt ağ Ekle](./media/virtual-networks-create-vnet-arm-pportal/add-subnet.png)
     
3. İçinde **alt ağ Ekle** görünen kutusuna girin *özel* için **adı**, girin *10.0.1.0/24* için **adres aralığı**ve ardından **Tamam**.  Bir alt ağ adres aralığı, bir sanal ağ içindeki diğer alt ağlara adres aralıklarıyla çakışamaz. 

Azure sanal ağlar ve alt ağları üretim kullanımı için dağıtmadan önce kapsamlı bir adres alanıyla öğrenmeniz olduğunu önerilir [konuları](manage-virtual-network.md#create-a-virtual-network) ve [sanal ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Kaynakların alt dağıtıldığında, bazı sanal ağ ve adres aralıkları değiştirme gibi alt ağ değişiklikleri içindeki alt ağlara dağıtılan var olan Azure kaynak çözümünüzün yeniden dağıtımını gerektirebilir.

## <a name="test-network-communication"></a>Test ağ iletişimi

Bir sanal ağ Internet ile ve özel olarak birbirleriyle iletişim kurmak için Azure kaynaklarını çeşitli türleri sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. İki sanal makine sanal ağ oluşturun, bir sonraki adımda bunları ve Internet arasındaki ağ iletişimi test edebilirsiniz.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımları seçtiğiniz varsayın **Windows Server 2016 Datacenter**. 
3. İçin aşağıdaki bilgileri girin veya seçin **Temelleri**seçeneğini belirleyip **Tamam**:
    - **Name**: *myVmWeb*
    - **Kaynak grubu**: seçin **var olanı kullan** ve ardından *myResourceGroup*.
    - **Konum**: seçin *Doğu ABD*.

    **Kullanıcı adı** ve **parola** girdiğiniz bir sonraki adımda kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. **Konumu** ve **abonelik** seçili aynı sanal ağ olan abonelik ve konum olarak olması gerekir. Bu sanal ağ içinde oluşturuldu, ancak bu öğretici için aynı kaynak grubu seçili aynı kaynak grubunu seçin zorunlu değildir.
4. VM boyutu altında seçin **bir boyutu seçin**.
5. İçin aşağıdaki bilgileri girin veya seçin **ayarları**seçeneğini belirleyip **Tamam**:
    - **Sanal ağ**: emin **myVirtualNetwork** seçilir. Aksi takdirde, seçin **sanal ağ** ve ardından **myVirtualNetwork** altında **Seç sanal ağ**.
    - **Alt ağ**: emin **ortak** seçilir. Aksi takdirde, seçin **alt** ve ardından **ortak** altında **alt ağ seçin**, aşağıdaki resimde gösterildiği gibi:
    
        ![Sanal makine ayarları](./media/virtual-networks-create-vnet-arm-pportal/virtual-machine-settings.png)
 
6. Altında **oluşturma** içinde **Özet**seçin **oluşturma** sanal makine dağıtımı başlatmak için.
7. 1-6 yeniden adımları ancak girmeniz *myVmMgmt* için **adı** seçin ve sanal makine **özel** için **alt**.

Sanal makineler oluşturmak için birkaç dakika sürebilir. Her iki sanal makine oluşturulana kadar kalan adımlara devam etmeyin.

Bu makalede oluşturulan sanal makineler bir tane [ağ arabirimi](virtual-network-network-interface.md) dinamik olarak ağ arabirimine atanmış bir IP adresine sahip. VM dağıtıldıktan sonra şunları yapabilirsiniz [birden çok ortak ve özel IP adreslerini ekleyin ya da statik IP adresi ataması yöntemi Değiştir](virtual-network-network-interface-addresses.md#add-ip-addresses). Yapabilecekleriniz [ağ arabirimlerini ekleyin](virtual-network-network-interface-vm.md#vm-add-nic), desteklediği sınırına kadar [VM boyutu](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir sanal makine oluşturduğunuzda seçin. Ayrıca [tek köklü g/ç Sanallaştırması (SR-IOV) etkinleştir](create-vm-accelerated-networking-powershell.md) bir VM, ancak yalnızca özelliğini destekleyen bir VM boyutu ile bir VM oluşturun.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Sanal makineler arasında ve internet ile iletişim

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVmMgmt*. Zaman **myVmMgmt** arama sonuçlarında görünür.
2. Uzak Masaüstü bağlantı oluşturmak *myVmMgmt* seçerek sanal makine **Bağlan**, aşağıdaki resimde gösterildiği gibi:

    ![Sanal makineye bağlanma](./media/virtual-networks-create-vnet-arm-pportal/connect-to-virtual-machine.png)  

3. VM'e bağlanmak için indirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.
4. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, ne zaman girdiğiniz kimlik bilgileri belirtmek için sanal makine oluşturulan), ardından **Tamam**.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için.
6. Bir sonraki adımda ping ile iletişim kurmak için kullanılan *myVmMgmt* sanal makineden *myVmWeb* sanal makine. Varsayılan olarak Windows Güvenlik Duvarı üzerinden engellenir ICMP ping komutunu kullanır. ICMP, bir komut isteminden aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Bu makalede ping kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.
7. Güvenlik nedenleriyle, sanal makinelere uzaktan sanal bir ağa bağlanabilir sayısını sınırlamak için yaygın bir sorundur. Bu öğreticide *myVmMgmt* yönetmek için kullanılan sanal makine *myVmWeb* sanal ağdaki sanal makine. Uzak Masaüstü *myVmWeb* sanal makineden *myVmMgmt* sanal makine, bir komut isteminden aşağıdaki komutu girin:

    ``` 
    mstsc /v:myVmWeb
    ```
8. İçin iletişim kurmak için *myVmMgmt* sanal makineden *myVmWeb* sanal makine, bir komut isteminden aşağıdaki komutu girin:

    ```
    ping myvmmgmt
    ```

    Aşağıdaki örnek çıkış benzer bir çıktı alırsınız:
    
    ```
    Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
    Ping statistics for 10.0.1.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms
    ```
      
    Görebilirsiniz adresini *myVmMgmt* sanal makinedir 10.0.1.4. 10.0.1.4 adres aralığı içinde ilk kullanılabilir IP adresi olan *özel* dağıttığınız alt *myVmMgmt* sanal makineye bir önceki adımda.  Sanal makinenin tam etki alanı adı olup olmadığını *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Ancak *dar5p44cif3ulfq00wxznl3i3f* etki alanı adı bölümü, sanal makine için farklı olduğundan, etki alanı adını Kalan bölümleri aynıdır. 

    Varsayılan olarak, tüm Azure sanal makineleri varsayılan Azure DNS hizmeti kullanın. Bir sanal ağ içindeki tüm sanal makineleri Azure'nın varsayılan DNS hizmeti ile aynı sanal ağdaki diğer tüm sanal makinelerin adlarını çözümleyebilir. Azure'nın varsayılan DNS hizmeti kullanmak yerine, kendi DNS sunucusu veya Azure DNS hizmeti, özel etki alanı özelliği kullanabilirsiniz. Ayrıntılar için bkz [kendi DNS sunucu kullanılarak ad çözümleme](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) veya [kullanarak Azure DNS özel etki alanları için](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Windows Server için Internet Information Services (IIS) yüklemek için *myVmWeb* sanal makine, bir PowerShell oturumunda aşağıdaki komutu girin:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

10. IIS yüklemesi tamamlandıktan sonra bağlantı kesme *myVmWeb* size bırakır Uzak Masaüstü oturumu *myVmMgmt* Uzak Masaüstü oturumu. Bir web tarayıcısı açın ve http://myvmweb için göz atın. Karşılama sayfası IIS bakın.
11. Bağlantı kesme *myVmMgmt* Uzak Masaüstü oturumu.
12. Genel IP adresi Bul *myVmWeb* sanal makine. Azure oluşturduğunuzda *myVmWeb* sanal makine, bir ortak IP adresi kaynağı *myVmWeb* de oluşturulur ve sanal makineye atanmış. 52.170.5.92 için atandı görebilirsiniz **genel IP adresi** için *myVmMgmt* sanal makine içinde 2. adımda resim. Atanan genel IP adresini bulmak için *myVmWeb* sanal makine, arama *myVmWeb* arama sonuçlarında görüntülendiğinde, portal'ın arama kutusuna seçin.

    Bir sanal makine kendisine atanmış bir ortak IP adresi olması gerekli değildir ancak Azure varsayılan oluşturmak her bir sanal makine bir ortak IP adresi atar. Bir sanal makineye Internet üzerinden iletişim kurmak için bir ortak IP adresi sanal makineyi atanması gerekir. Bir ortak IP adresi sanal makineye atanmış olup olmadığına bakılmaksızın tüm sanal makinelerin giden Internet ile iletişim kurabilir. Azure'a giden Internet bağlantıları hakkında daha fazla bilgi için bkz: [azure'da giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
13. Genel IP adresi için kendi bilgisayarınızda Gözat *myVmWeb* sanal makine. Kendi bilgisayardan IIS Hoş Geldiniz sayfasını görüntülemek için denemesi başarısız olur. Sanal makineler dağıtıldığında, bu Azure varsayılan olarak her bir sanal makine için bir ağ güvenlik grubu oluşturduğundan denemesi başarısız olur. 

     Ağ güvenlik grubu izin veren veya reddeden IP adresi ve bağlantı noktasına gelen ve giden ağ trafiğinin güvenlik kuralları içerir. Oluşturulan Azure varsayılan ağ güvenlik grubu, aynı sanal ağdaki kaynakları arasındaki tüm bağlantı noktaları üzerinden iletişim sağlar. Windows sanal makineler için varsayılan ağ güvenlik grubu TCP bağlantı noktası 3389 (RDP) dışındaki tüm bağlantı noktaları üzerinden Internet'ten gelen tüm trafiği engeller. Sonuç olarak, varsayılan olarak, aynı zamanda RDP doğrudan yapabilecekleriniz *myVmWeb* sanal makine Internet'ten gelen değil isteyebilirsiniz olsa bile 3389 açık bir web sunucusuna bağlantı noktası. Bağlantı noktası 80 üzerinden trafiğe izin varsayılan ağ güvenlik grubu kural olduğundan iletişim bağlantı noktası 80 üzerinden iletişim kurar Web'e gözatma olduğundan, Internet'ten başarısız olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil: 

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağ birden fazla alt ağı dağıtmayı öğrendiniz. Ayrıca, Windows sanal makine oluşturduğunuzda, Azure'nın, sanal makineye ekler ve yalnızca trafiğe 3389, bağlantı noktası üzerinden Internet'ten izin veren bir ağ güvenlik grubu oluşturur bir ağ arabirimi oluşturur öğrendiniz. Ağ trafiği alt ağlara yerine tek tek sanal makineleri için filtre öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Alt ağ trafiği filtreleme](./virtual-networks-create-nsg-arm-pportal.md)
