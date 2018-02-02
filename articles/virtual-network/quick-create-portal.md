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
ms.openlocfilehash: 264dc38383b9adad70325f7fb7802b1dcf2da1c0
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ oluşturma

Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki sanal makineye sanal ağa dağıtmak ve özel olarak aralarında iletişim.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Tıklatın **+ yeni** Azure portalının sol üst köşedeki üzerinde.

2. Seçin **ağ**ve ardından **sanal ağ**.

3. Aşağıdaki resimde gösterildiği gibi girin *myVirtualNetwork* için **adı**, *myResourceGroup* için **kaynak grubu**, bir seçin **Konum** ve **abonelik**, kalan Varsayılanları kabul edin ve ardından **oluşturma**. 

    ![Sanal ağınız hakkında temel bilgileri girin](./media/quick-create-portal/virtual-network.png)

    **Adres alanı** CIDR gösteriminde belirtilir. Bir sanal ağ sıfır veya daha fazla alt ağlar içeriyor. Varsayılan alt ağ **adres aralığı** varsayılan adres alanı ve aralığı'nı kullanarak sanal ağ içindeki başka bir alt ağ oluşturulamıyor 10.0.0.0/24 sanal ağ tüm adres aralığını kullanır. Belirtilen adres aralığı, IP adresleri 10.0.0.0-10.0.0.254 içerir. Yalnızca 10.0.0.4-10.0.0.254 kullanılabilir ancak Azure ilk dört adresler (0-3), her alt ağda son adresi ayırdığından. Kullanılabilir IP adreslerini dağıtılan bir sanal ağ içindeki kaynaklara atanır.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bir sanal ağ çeşitli özel olarak birbirleri ile iletişim kurmak için Azure kaynaklarını sağlar. Tek bir sanal ağa dağıttığınız kaynak türü, bir sanal makinedir. Doğrulayın ve daha sonraki bir adımda bir sanal ağdaki sanal makineler arasındaki iletişimi nasıl çalıştığını anlamak için iki sanal makineye sanal ağ oluşturun.

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.

3. Aşağıdaki resimde gösterilen sanal makine bilgilerini girin. **Kullanıcı adı** ve **parola** girdiğiniz bir sonraki adımda sanal makinede oturum açmak için kullanılır. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. Seçin, **abonelik**, varolan kullanmayı tercih *myResourceGroup* kaynak grubu ve emin **konumu** seçili oluşturduğunuz konumdur sanal ağ içinde. İşlem tamamlandığında **Tamam**’a tıklayın.

    ![Bir sanal makine hakkında temel bilgileri girin](./media/quick-create-portal/virtual-machine-basics.png)

4. Sanal makine için bir boyut seçin ve ardından **seçin**. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Sizin için görüntülenen boyutlar, aşağıdaki örnekte farklı olabilir: 

    ![Bir sanal makine için bir boyut seçin](./media/quick-create-portal/virtual-machine-size.png)

5. Altında **ayarları**, *myVirtualNetwork* için zaten seçilmelidir **sanal ağ**, ancak değilse, **sanal ağ**, ardından *myVirtualNetwork*. Bırakın *varsayılan* için seçilen **alt**ve ardından **Tamam**.

    ![Sanal ağ seçin](./media/quick-create-portal/virtual-machine-network-settings.png)

6. Üzerinde **Özet** sayfasında, **oluşturma** sanal makine dağıtımı başlatmak için. 

7. Sanal makine oluşturmak için birkaç dakika sürer. Oluşturma sonra sanal makineyi Azure portal panosuna sabitlenmiş ve sanal makine özeti otomatik olarak açılır. Tıklatın **ağ**.

    ![Sanal makine ağ bilgileri](./media/quick-create-portal/virtual-machine-networking.png)

    Gördüğünüz **özel IP** adresi *10.0.0.4*. Altında adım 5 **ayarları**, seçtiğiniz *myVirtualNetwork* sanal ağ ve adlı alt ağın kabul *varsayılan* için **alt**. Olduğunda, [sanal ağı oluşturan](#create-a-virtual-network), 10.0.0.0/24 alt ağ için varsayılan değerini kabul **adres aralığı**. Azure'nın DHCP sunucusu sanal makine için seçtiğiniz alt ağ için kullanılabilir ilk adrese atar. Her alt ağ ilk dört adresleri (0-3) Azure ayırır olduğundan, 10.0.0.4 ilk kullanılabilir IP adresi alt ağı için kullanılabilir kalır.

    **Genel IP** atanan adresi, sanal makineye atanan adresi farklı. Azure genel, bir Internet yönlendirilebilir IP adresi her bir sanal makine için varsayılan olarak atar. Genel IP adresi, sanal makineden atanmış bir [her bir Azure bölgesine atanan adresler havuzuna](https://www.microsoft.com/download/details.aspx?id=41653). Hangi ortak IP adresi bir sanal makineye atanan Azure bilir olsa da, bir sanal makinede çalışan işletim sistemi atanmış tüm ortak IP adresinin hiçbir şeyin sahiptir.

8. Tüm adımları 1-7 yeniden, ancak adım 3, sanal makine adı *myVm2*. 

9. Sanal makine oluşturulduktan sonra tıklatın **ağ**, 7. adımda gibi. Gördüğünüz **özel IP** adresi *10.0.0.5*. Azure ilk kullanılabilir adresini daha önce atanan beri *10.0.0.4* alt ağda *myVm1* sanal makine, kendisine atanmış *10.0.0.5* için  *myVm2* sanal makine, bir sonraki kullanılabilir adres alt ağda olduğundan.

## <a name="connect-to-a-virtual-machine"></a>Bir sanal makineye bağlanma

1. Uzaktan bağlanmak *myVm1* sanal makine. Azure portalının en üstünde girin *myVm1*. Zaman **myVm1** görünür arama sonuçlarında tıklatın. Tıklatın **Bağlan** düğmesi.

    ![Sanal makineye genel bakış](./media/quick-create-portal/virtual-machine-overview.png)


2. ' I tıklattıktan sonra **Bağlan** düğmesi, bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden.  
3. İndirilen rdp dosyasını açın. İstenirse, **Bağlan**’a tıklayın. Kullanıcı adı ve sanal makine oluştururken belirttiğiniz parolayı girin ve ardından **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

## <a name="validate-communication"></a>İletişim doğrula

Ping varsayılan olarak Windows Güvenlik Duvarı aracılığıyla izin verilmediğinden bir Windows sanal makine başarısız ping çalışılıyor. Ping işlemine izin vermek için *myVm1*, bir komut isteminden aşağıdaki komutu girin:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

İle iletişim doğrulamak için *myVm2*, bir komut isteminden aşağıdaki komutu girin *myVm1* sanal makine. Sanal makine oluştururken kullandığınız kimlik bilgilerini sağlayın ve ardından bağlantıyı tamamlayın:

```
mstsc /v:myVm2
```

Her iki sanal makine gelen atanan özel IP adresleri olduğundan Uzak Masaüstü bağlantısı başarılı *varsayılan* alt ağı ve Uzak Masaüstü varsayılan olarak Windows Güvenlik Duvarı üzerinden açık olduğundan. Bağlanabilmeleri için *myVm2* ana bilgisayar adı tarafından çünkü Azure otomatik olarak bir sanal ağ içindeki tüm ana bilgisayarlar için DNS ad çözümlemesi sağlar. Bir komut isteminden ping my *myVm1*, gelen *myVm2*.

```
ping myvm1
```

Ping işlemi başarılı, onu Windows Güvenlik Duvarı aracılığıyla izin *myVm1* bir önceki adımda sanal makine. İnternet giden iletişim onaylamak için aşağıdaki komutu girin:

```
ping bing.com
```

Aratıp dört yanıt alırsınız. Varsayılan olarak, herhangi bir sanal makine bir sanal ağdaki internet giden iletişim kurabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubunu ve tüm içeriğini silin. Azure portalının en üstünde girin *myResourceGroup*. Zaman **myResourceGroup** görünür arama sonuçlarında tıklatın. **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ bir alt ağı ve iki sanal makine ile dağıtılabilir. Birden çok alt ağ ile özel bir sanal ağ oluşturma ve temel yönetim görevlerini gerçekleştirme hakkında bilgi almak için özel bir sanal ağ oluşturma ve yönetmeyi öğretici devam edin.


> [!div class="nextstepaction"]
> [Özel bir sanal ağ oluşturma ve yönetme](virtual-networks-create-vnet-arm-pportal.md#portal)