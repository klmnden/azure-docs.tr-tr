---
title: Rota ağ trafiğini - Azure portalı | Microsoft Docs
description: Azure Portalı'nı kullanarak bir yol tablosu ile ağ trafiğini yönlendirmek öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 980cf7b59ed16778bbb6cd1b657e3522407c79c9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir yol tablosu ile ağ trafiği yönlendirme

Azure otomatik olarak yollar varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında trafiği. Azure'nın geçersiz kılmak için kendi Rota oluşturabilmeniz için varsayılan yönlendirme. Örneğin, bir ağ sanal gereç (NVA) aracılığıyla alt ağlar arasında trafiği yönlendirmek istiyorsanız, özel yollar oluşturma olanağı yararlıdır. Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Rota tablosu oluşturma
> * Bir yol oluşturma
> * Birden çok alt ağı ile bir sanal ağ oluşturma
> * Bir alt ağ için bir yol tablosu ilişkilendirme
> * Trafiğini yönlendiren bir NVA oluşturma
> * Sanal makineler (VM) farklı alt dağıtma
> * Bir NVA aracılığıyla başka bir yolu trafiğini bir alt ağdan

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

Oturum açtığınızda Azure portalında http://portal.azure.com.

## <a name="create-a-route-table"></a>Rota tablosu oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **yol tablosu**.
3. Girin veya seçin, aşağıdaki bilgileri, kalan ayarı için varsayılan kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myRouteTablePublic|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu | Seçin **Yeni Oluştur** ve girin *myResourceGroup*.|
    |Konum|Doğu ABD|
 
    ![Yol tablosu oluşturma](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Bir yol oluşturma

1. İçinde *arama kaynakları, hizmetleri ve belgeleri* kutusunda portalın en üstünde, yazmaya başlayın *myRouteTablePublic*. Zaman **myRouteTablePublic** arama sonuçlarında görünür.
2. Altında **ayarları**seçin **yollar** ve ardından **+ Ekle**, aşağıdaki resimde gösterildiği gibi:

    ![Yolu Ekle](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Altında **Add yolun**girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Rota adı|ToPrivateSubnet|
    |Adres ön eki| 10.0.1.0/24|
    |Sonraki atlama türü | Seçin **sanal Gereci**.|
    |Sonraki atlama adresi| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Bir alt ağ için bir yol tablosu ilişkilendirme

Bir alt ağ için bir yol tablosu ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmak zorunda, sonra da bir alt ağ için yol tablosu ilişkilendirebilirsiniz:

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Altında **sanal ağ oluştur**girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork|
    |Adres alanı| 10.0.0.0/16|
    |Abonelik | Aboneliğinizi seçin.|
    |Kaynak grubu|Seçin **var olanı kullan** ve ardından **myResourceGroup**.|
    |Konum|Seçin *Doğu ABD*|
    |Alt ağ adı|Genel|
    |Adres aralığı|10.0.0.0/24|
    
4. İçinde **arama kaynakları, hizmetleri ve belgeleri** kutusunda portalın en üstünde, yazmaya başlayın *myVirtualNetwork*. Zaman **myVirtualNetwork** arama sonuçlarında görünür.
5. Altında **ayarları**seçin **alt ağlar** ve ardından **+ alt**, aşağıdaki resimde gösterildiği gibi:

    ![Alt ağ ekleme](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Seçin veya aşağıdaki bilgileri girin ve ardından **Tamam**:

    |Ayar|Değer|
    |---|---|
    |Ad|Özel|
    |Adres alanı| 10.0.1.0/24|

7. 5 ve 6 yeniden, aşağıdaki bilgileri sağlayarak adımları tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Ad|DMZ|
    |Adres alanı| 10.0.2.0/24|

8. **MyVirtualNetwork - alt ağlar** kutusu, önceki adımı tamamladıktan sonra görüntülenir. Altında **ayarları**seçin **alt ağlar** ve ardından **ortak**.
9. Aşağıdaki resimde gösterildiği gibi seçin **yol tablosu**seçin **MyRouteTablePublic**ve ardından **kaydetmek**:

    ![Yol tablosu ilişkilendirme](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Bir NVA oluşturma

Bir NVA yönlendirme, saldırısından veya WAN iyileştirmesi gibi bir ağ işlevi gerçekleştiren bir VM'dir.

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. Farklı bir işletim sistemi seçebilirsiniz, ancak geri kalan adımları seçtiğiniz varsayın **Windows Server 2016 Datacenter**. 
3. İçin aşağıdaki bilgileri girin veya seçin **Temelleri**seçeneğini belirleyip **Tamam**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVmNva|
    |Kullanıcı adı|Seçtiğiniz kullanıcı adı girin.|
    |Parola|Seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| Seçin **var olanı kullan** ve ardından *myResourceGroup*.|
    |Konum|Seçin **Doğu ABD**.|
4. VM boyutu altında seçin **bir boyutu seçin**.
5. İçin aşağıdaki bilgileri girin veya seçin **ayarları**seçeneğini belirleyip **Tamam**:

    |Ayar|Değer|
    |---|---|
    |Sanal ağ|myVirtualNetwork - seçilmezse, seçin **sanal ağ**seçeneğini belirleyip **myVirtualNetwork** altında **Seç sanal ağ**.|
    |Alt ağ|Seçin **alt** ve ardından **DMZ** altında **alt ağ seçin**. |
    |Genel IP adresi| Seçin **genel IP adresi** seçip **hiçbiri** altında **genel IP adresi seçin**. Bunun için Internet'ten bağlanmayacaktır beri ortak IP adresi bu VM atanır.
6. Altında **oluşturma** içinde **Özet**seçin **oluşturma** VM dağıtımı başlatmak için.

    VM oluşturmak için birkaç dakika sürer. Azure VM oluşturduktan ve VM hakkında bilgi içeren bir kutu açar kadar sonraki adıma devam etmeyin.

7. Altında oluşturulduktan sonra VM için açılan kutusunda **ayarları**seçin **ağ**ve ardından **myvmnva158** (Azure oluşturulan ağ arabirimi, VM, sonra farklı bir numara sahip **myvmnva**), aşağıdaki resimde gösterildiği gibi:

    ![VM ağı](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Kendi IP adresi için hedeflendiği değil, kendisine gönderilen ağ trafiği iletmek bir ağ arabirimi için IP iletme ağ arabirimi için etkinleştirilmesi gerekir. Altında **ayarları**seçin **IP yapılandırmaları**seçin **etkin** için **IP iletimini**ve ardından **Kaydet** , aşağıdaki resimde gösterildiği gibi:

    ![IP iletimini etkinleştirme](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

İki VM, sanal ağ oluşturun, böylece bu trafiğinden doğrulayabilirsiniz *ortak* alt ağ için yönlendirilir *özel* sonraki adımda nva'nın alt ağa. 1-6 adımlarını tamamlamanız [bir NVA oluşturma](#create-a-network-virtual-appliance). Adım 3 ve 5, aşağıdaki değişiklikler dışında aynı ayarları kullanır:

|Sanal makine adı      |Alt ağ      | Genel IP adresi     |
|--------- | -----------|---------              |
| myVmPublic  | Genel     | Portal varsayılanı kabul edin |
| myVmPrivate | Özel    | Portal varsayılanı kabul edin |

Oluşturabileceğiniz *myVmPrivate* Azure oluşturduğu sırada VM *myVmPublic* VM. Her iki VM oluşturma Azure tamamlanana kadar aşağıdaki adımlarla devam etmeyin.

## <a name="route-traffic-through-an-nva"></a>Bir NVA arasında trafiği yönlendirme

1. İçinde *arama* kutusunda portalın en üstünde, yazmaya başlayın *myVmPrivate*. Zaman **myVmPrivate** VM arama sonuçlarında görünür seçin.
2. Uzak Masaüstü bağlantı oluşturmak *myVmPrivate* seçerek VM **Bağlan**, aşağıdaki resimde gösterildiği gibi:

    ![VM’ye bağlanma ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. VM'e bağlanmak için indirilen RDP dosyasını açın. İstenirse, seçin **Bağlan**.
4. Kullanıcı adını ve VM oluştururken belirttiğiniz parolayı girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için), ardından **Tamam**.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Seçin **Evet** bağlantı ile devam etmek için.
6. Bir sonraki adımda tracert.exe komut yönlendirme test etmek için kullanılır. Tracert Windows Güvenlik Duvarı üzerinden reddedildi Internet Denetim İletisi Protokolü (ICMP) kullanır. ICMP Powershell'den aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Bu makalede yönlendirme test etmek için tracert kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.
7. VM'nin ağ arabirimi için IP iletimini Azure içindeki etkin [etkinleştirmek IP fowarding](#enable-ip-forwarding). VM dahilinde işletim sistemi ya da VM içinde çalışan bir uygulama aynı zamanda ağ trafiğini iletebilir olması gerekir. İşletim sistemi içinde IP iletimini etkinleştirmeniz *myVmNva* aşağıdaki tamamlayarak VM adımları *myVmPrivate* VM:

    Uzak Masaüstü ' *myVmNva* bir komut isteminden aşağıdaki komutu ile:

    ``` 
    mstsc /v:myvmnva
    ```
    
    İşletim sistemi içinde iletme IP etkinleştirmek için PowerShell içinde aşağıdaki komutu girin:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Ayrıca Uzak Masaüstü oturumu bağlantısını keser VM'yi yeniden başlatın.
8. Halen bağlı sırasında *myVmPrivate* VM, bir Uzak Masaüstü oturumu oluşturmak *myVmPublic* VM şu komutla sonra *myVmNva* VM'yi yeniden başlatır:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    ICMP Powershell'den aşağıdaki komutu girerek Windows Güvenlik Duvarı aracılığıyla etkinleştirin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Ağ trafiği yönlendirmesini test etmek için *myVmPrivate* VM'den *myVmPublic* VM Powershell'den aşağıdaki komutu girin:

    ```
    tracert myVmPrivate
    ```

    Yanıt aşağıdaki örneğe benzer:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    İlk atlama NVA'ın özel IP adresi 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama 10.0.1.4, özel IP adresi olan *myVmPrivate* VM. Eklenen rota *myRouteTablePublic* rota tablosu ve ilişkili *ortak* alt neden NVA aracılığıyla yerine doğrudan trafiği yönlendirmek Azure *özel* alt ağ.
10.  Uzak Masaüstü oturumu kapatmak *myVmPublic* , hala bağlı bırakır VM *myVmPrivate* VM.
11. Ağ trafiği yönlendirmesini test etmek için *myVmPublic* VM'den *myVmPrivate* VM, bir komut isteminden aşağıdaki komutu girin:

    ```
    tracert myVmPublic
    ```

    Yanıt aşağıdaki örneğe benzer:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Trafik, doğrudan yönlendirilir görebilirsiniz *myVmPrivate* VM *myVmPublic* VM. Varsayılan olarak, Azure yollar doğrudan alt ağlar arasında trafiği.
12. Uzak Masaüstü oturumu kapatmak *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği tüm kaynaklar Sil: 

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir yol tablosu oluşturulur ve bir alt ağa ilişkilendirilmiş. Ortak bir alt ağ trafiği için özel bir alt ağa yönlendirilmiş basit bir NVA oluşturuldu. Güvenlik Duvarı ve WAN iyileştirme dışında gibi ağ işlevleri gerçekleştirmek önceden yapılandırılmış NVAs çeşitli dağıtmak [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Yönlendirme tabloları üretim kullanımı için dağıtmadan önce baştan sona ile öğrenmeniz olduğunu önerilir [Azure'da yönlendirme](virtual-networks-udr-overview.md), [Yönet yol tablolarını](manage-route-table.md), ve [Azuresınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).


Bir sanal ağ içinde birçok Azure kaynakları dağıtabilirsiniz, ancak bazı Azure PaaS hizmetler için kaynaklar sanal bir ağa dağıtılamıyor. Hala erişimi bazı Azure PaaS Hizmetleri'nden trafik için yalnızca bir sanal ağ alt kaynaklara yine de kısıtlayabilirsiniz. Ağ erişimi Azure PaaS kaynaklarına erişimi kısıtlayabilir öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ erişimi PaaS kaynaklarına erişimi kısıtlayabilir](tutorial-restrict-network-access-to-resources.md)
