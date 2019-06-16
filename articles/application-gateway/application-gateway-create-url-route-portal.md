---
title: URL yolu tabanlı yönlendirme kurallarıyla - Azure portalında bir uygulama ağ geçidi oluşturma
description: URL yolu tabanlı yönlendirme kuralları bir uygulama ağ geçidi ve Azure portalını kullanarak sanal makine ölçek kümesi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: victorh
ms.openlocfilehash: 10bc4e4c440e5495afd820f588270b7990108b68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66135294"
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Azure portalını kullanarak yol tabanlı yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturma

Azure portalında yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturun. Daha sonra uygun sunucularla havuzlarındaki web trafiğini ulaştığında emin yönlendirme kuralları oluşturursunuz.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Bir arka uç dinleyici oluşturun
> * Yola dayalı kural oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

   - *myAppGateway* - Uygulama ağ geçidinin adı.
   - *myResourceGroupAG* - Yeni kaynak grubu.

     ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Tıklayın **genel bir IP adresi seçin**, tıklayın **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, application gateway için arka uç sunucular olarak kullanılacak üç sanal makine oluşturun. Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS de yükleyin.

1. **Yeni**’ye tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - Sanal makinenin adı için *myVM1*.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam** düğmesine tıklayın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName myVM1 `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. İki daha fazla sanal makine oluşturur ve yalnızca tamamlanmış adımları kullanarak IIS yükleyin. Adlarını girin *myVM2* ve *myVM3* adlarını ve değerlerini VMName AzVMExtension kümesi içinde.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Arka uç havuzları ile sanal makineleri oluşturma

1. Tıklayın **tüm kaynakları** ve ardından **myAppGateway**.
2. **Arka uç havuzları** öğesine tıklayın. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. **appGatewayBackendPool** öğesine tıklayın.
3. Tıklayın **Ekle hedef** eklemek için *myVM1* appGatewayBackendPool için.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-url-route-portal/application-gateway-backend.png)

4. **Kaydet**’e tıklayın.
5. Tıklayın **arka uç havuzları** ve ardından **Ekle**.
6. Bir ad girin *imagesBackendPool* ve ekleme *myVM2* kullanarak **Ekle hedef**.
7. **Tamam** düğmesine tıklayın.
8. Tıklayın **Ekle** başka bir arka uç havuzu adı ile tekrar eklemeyi *videoBackendPool* ve ekleme *myVM3* ona.

## <a name="create-a-backend-listener"></a>Bir arka uç dinleyici oluşturun

1. Tıklayın **dinleyicileri** tıklayarak **temel**.
2. Girin *myBackendListener* adı için *myFrontendPort* ön uç bağlantı noktası adını ve ardından *8080* dinleyicisinin bağlantı noktası.
3. **Tamam**'ı tıklatın.

## <a name="create-a-path-based-routing-rule"></a>Yola dayalı kural oluşturma

1. Tıklayın **kuralları** ve ardından **yol tabanlı**.
2. Girin *bağlanma2* adı.
3. Girin *görüntüleri* ilk yol adı. Girin */images/* \* yolu. Seçin **imagesBackendPool** arka uç havuzu için.
4. Girin *Video* ikinci yol adı. Girin */video/* \* yolu. Seçin **videoBackendPool** arka uç havuzu için.

    ![Yol tabanlı kural oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-route-rule.png)

5. **Tamam**'ı tıklatın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Tıklayın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Http gibi:\//40.121.222.19.

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. URL http:// değiştirme&lt;IP adresi&gt;: 8080/images/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir:

    ![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. URL http:// değiştirme&lt;IP adresi&gt;: 8080/video/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir:

    ![Video URL’sini uygulama ağ geçidinde test etme](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Bir arka uç dinleyici oluşturun
> * Yola dayalı kural oluşturma

Uygulama ağ geçitleri ve bunların ilişkili kaynakları hakkında daha fazla bilgi edinmek için nasıl yapılır makaleleriyle devam edin.
