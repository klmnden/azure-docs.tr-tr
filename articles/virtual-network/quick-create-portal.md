---
title: "Bir Azure sanal ağı - Portal oluştur | Microsoft Docs"
description: "Hızlı bir şekilde Azure portalını kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ özel olarak birbirleriyle ve internet ile iletişim kurmak için sanal makineler gibi Azure kaynakları sağlar."
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
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c8f2cbe6b7377772e019a4ff90f91355ba0815ae
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ oluşturma

Bir sanal ağ sanal makineler (VM) gibi Azure kaynakları özel olarak birbirleriyle ve internet ile iletişim kurmasını sağlar. Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki VM sanal ağa dağıtın. Ardından internet'ten bir VM'ye bağlanın ve özel olarak iki VM iletişim.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin **+ kaynak oluşturma** üst üzerinde köşe Azure portalının sol.
2. Seçin **ağ**ve ardından **sanal ağ**.
3. Girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan değerleri kabul edin ve ardından **oluşturma**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVirtualNetwork|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| Seçin **Yeni Oluştur** ve girin *myResourceGroup*.|
    |Konum| Seçin **Doğu ABD**.|

    ![Sanal ağınız hakkında temel bilgileri girin](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

İki VM sanal ağ oluşturun:

### <a name="create-the-first-vm"></a>İlk VM oluşturma

1. Seçin **+ kaynak oluşturma** üst, sol üst köşesinde Azure portalı üzerinde bulunamadı.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin.
3. Girin veya seçin, aşağıdaki bilgileri, diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**:

    |Ayar|Değer|
    |---|---|
    |Ad|myVm1|
    |Kullanıcı adı| Seçtiğiniz kullanıcı adı girin.|
    |Parola| Seçtiğiniz bir parola girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| Seçin **var olanı kullan** seçip **myResourceGroup**.|
    |Konum| Seçin **Doğu ABD**|

    ![Sanal makine temelleri](./media/quick-create-portal/virtual-machine-basics.png)

4. VM için bir boyut seçin ve ardından **seçin**.
5. Altında **ayarları**, tüm Varsayılanları kabul edin ve ardından **Tamam**.

    ![Sanal makine ayarları](./media/quick-create-portal/virtual-machine-settings.png)

6. Altında **oluşturma** , **Özet**seçin **oluşturma** VM dağıtımı başlatmak için. VM dağıtmak için birkaç dakika sürer. 

### <a name="create-the-second-vm"></a>İkinci VM oluşturma

Tüm adımları 1-6 yeniden, ancak adım 3, VM adı *myVm2*.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir VM'ye bağlanın

1. Sonra *myVm1* olan bağlanmak için oluşturulmuş. Azure portalının en üstünde girin *myVm1*. Zaman **myVm1** arama sonuçlarında görünür. Seçin **Bağlan** düğmesi.

    ![Bir sanal makineye bağlanma](./media/quick-create-portal/connect-to-virtual-machine.png)

2. Seçtikten sonra **Bağlan** düğmesi, bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden.  
3. İndirilen rdp dosyasını açın. İstenirse, seçin **Bağlan**. Kullanıcı adı ve VM oluştururken belirttiğiniz parolayı girin. Seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, seçin **Evet** veya **devam**, bağlantı ile devam etmek için.

## <a name="communicate-privately-between-vms"></a>Özel olarak VM'ler arasında iletişim

1. PowerShell girin `ping myvm2`. Ping başarısız olursa, Internet Denetim İletisi Protokolü (ICMP) ve ICMP ping kullandığı için Windows Güvenlik Duvarı üzerinden varsayılan olarak izin verilmiyor.
2. İzin vermek için *myVm2* ping işlemi yapmak için *myVm1* bir sonraki adımda ICMP verir Powershell'den aşağıdaki komutu girin Windows Güvenlik Duvarı üzerinden gelen:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. Uzak Masaüstü Bağlantısı kapatmak *myVm1*. 

4. Bölümündeki adımları tamamlamanız [VM Bağlan internet'ten](#connect-to-a-vm-from-the-internet) yeniden, ancak bağlanmak *myVm2*. Bir komut isteminden girin `ping myvm1`.

    Yanıt aldığınız *myVm1*, üzerinde Windows Güvenlik Duvarı üzerinden ICMP izin *myVm1* bir önceki adımda VM.

5. Uzak Masaüstü Bağlantısı kapatmak *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kaynak grubu ve içerdiği kaynakların tümünü Sil:

1. Girin *myResourceGroup* içinde **arama** portalı üstündeki kutusu. Gördüğünüzde **myResourceGroup** arama sonuçlarında seçin.
2. **Kaynak grubunu sil**'i seçin.
3. Girin *myResourceGroup* için **türü kaynak grubu adı:** seçip **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ ve iki VM oluşturdunuz. Internet'ten bir VM'ye bağlı ve özel olarak VM ve başka bir VM iletişim. Sanal ağ ayarları hakkında daha fazla bilgi için bkz: [sanal ağ yönetme](manage-virtual-network.md).

Varsayılan olarak, Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar, ancak yalnızca gelen Uzak Masaüstü bağlantıları Windows VM'ler için Internet'ten izin verir. İzin vermek veya farklı türlerde ağ iletişimi için ve VM'ler kısıtlamak nasıl öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ trafiği filtreleme](virtual-networks-create-nsg-arm-pportal.md)