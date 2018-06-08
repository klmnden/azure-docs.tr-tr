---
title: URL yolu tabanlı yönlendirme kuralları ile - Azure portalında bir uygulama ağ geçidi oluşturma
description: URL yolu tabanlı yönlendirme kuralları bir uygulama ağ geçidi ve sanal makine ölçek Azure portalını kullanarak ayarlamak için oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: victorh
ms.openlocfilehash: 4ffaeedf125b6f74aeb88e22248040c6c3ef001c
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "34356180"
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Azure portalını kullanarak yol tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma

Azure Portalı'nı yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) oluştururken bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturun. Uygun sunucuları havuzlarındaki web trafiği ulaştığında emin olun yönlendirme kuralları oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyici oluşturun
> * Yol tabanlı yönlendirme kuralı oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[http://portal.azure.com](http://portal.azure.com) adresinden Azure portalında oturum açın

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

    - *myAppGateway* - Uygulama ağ geçidinin adı.
    - *myResourceGroupAG* - Yeni kaynak grubu.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - Sanal ağın adı.
    - *10.0.0.0/16* - Sanal ağın adres alanı.
    - *myAGSubnet* - Alt ağın adı.
    - *10.0.0.0/24* - Alt ağın adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarını ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte uygulama ağ geçidi için arka uç sunucuları olarak kullanılacak üç sanal makine oluşturun. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

1. **Yeni**’ye tıklayın.
2. Tıklatın **işlem** ve ardından **Windows Server 2016 Datacenter** öne çıkan listesinde.
3. Sanal makine için şu değerleri girin:

    - Sanal makinenin adı için *myVM1*.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam**’a tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. İki daha fazla sanal makine oluşturun ve yalnızca tamamlandı adımları kullanarak IIS yükleyin. Adlarını girin *myVM2* ve *myVM3* adlarını ve Set-AzureRmVMExtension VMName değerleri.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Sanal makineler ile arka uç havuzları oluşturma

1. Tıklatın **tüm kaynakları** ve ardından **myAppGateway**.
2. **Arka uç havuzları** öğesine tıklayın. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. **appGatewayBackendPool** öğesine tıklayın.
3. Tıklatın **Ekle hedef** eklemek için *myVM1* appGatewayBackendPool için.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

4. **Kaydet**’e tıklayın.
5. Tıklatın **arka uç havuzları** ve ardından **Ekle**.
6. Bir adı girin *imagesBackendPool* ve ekleme *myVM2* kullanarak **Ekle hedef**.
7. **Tamam**’a tıklayın.
8. Tıklatın **Ekle** başka bir arka uç havuzu adıyla birlikte yeniden eklemeyi *videoBackendPool* ve ekleme *myVM3* ona.

## <a name="create-a-backend-listener"></a>Arka uç dinleyici oluşturun

1. Tıklatın **dinleyicileri** ve **temel**.
2. Girin *myBackendListener* adı için *myFrontendPort* ön uç bağlantı noktası adını ve ardından *8080* dinleyicisinin bağlantı noktası olarak.
3. **Tamam**’a tıklayın.

## <a name="create-a-path-based-routing-rule"></a>Yol tabanlı yönlendirme kuralı oluşturma

1. Tıklatın **kuralları** ve ardından **yol tabanlı**.
2. Girin *Kural 2* adı.
3. Girin *görüntüleri* ilk yol adı. Girin */images/** yolu. Seçin **imagesBackendPool** arka uç havuzu için.
4. Girin *Video* ikinci yol adı. Girin */video/** yolu. Seçin **videoBackendPool** arka uç havuzu için.

    ![Yol tabanlı bir kural oluşturun](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. **Tamam**’a tıklayın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Tıklatın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin http://http://40.121.222.19.

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. URL'nin http:// değiştirme&lt;IP adresi&gt;: 8080/images/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. URL'nin http:// değiştirme&lt;IP adresi&gt;: 8080/video/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Video URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyici oluşturun
> * Yol tabanlı yönlendirme kuralı oluşturma

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.
