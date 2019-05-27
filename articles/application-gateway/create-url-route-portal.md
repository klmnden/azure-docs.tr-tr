---
title: Öğretici - URL yolu tabanlı yönlendirme kurallarıyla - Azure portalında bir uygulama ağ geçidi oluşturma
description: Bu öğreticide, URL yolu tabanlı yönlendirme kuralları bir uygulama ağ geçidi ve Azure portalını kullanarak sanal makine ölçek kümesi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/18/2019
ms.author: victorh
ms.openlocfilehash: 5307f7674635fd33241e1faba9bb0b7c0432d10b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134832"
---
# <a name="tutorial-create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak yol tabanlı yönlendirme kurallarıyla bir uygulama ağ geçidi oluşturma

Azure portalında yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](url-route-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](overview.md). Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturun. Daha sonra uygun sunucularla havuzlarındaki web trafiğini ulaştığında emin yönlendirme kuralları oluşturursunuz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Bir arka uç dinleyici oluşturun
> * Yola dayalı kural oluşturma

![URL yönlendirme örneği](./media/create-url-route-portal/scenario.png)

Tercih ederseniz Bu öğretici kullanarak tamamlayabilirsiniz [Azure CLI](tutorial-url-route-cli.md) veya [Azure PowerShell](tutorial-url-route-powershell.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Oturum açın: Azure

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Seçin **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

   - *myAppGateway* - Uygulama ağ geçidinin adı.
   - *myResourceGroupAG* - Yeni kaynak grubu.

     ![Yeni uygulama ağ geçidi oluşturma](./media/create-url-route-portal/application-gateway-create.png)

4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Seçin **bir sanal ağ seçin**seçin **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluştur](./media/create-url-route-portal/application-gateway-vnet.png)

6. Seçin **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Seçin **genel bir IP adresi seçin**seçin **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Seçin **tüm kaynakları** seçin ve soldaki menüden **myVNet** ve kaynak listesinden.
2. Seçin **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluştur](./media/create-url-route-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* seçin ve alt ağ adı için **Tamam**.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, application gateway için arka uç sunucular olarak kullanılacak üç sanal makine oluşturun. Ayrıca uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yüklersiniz.

1. **Yeni**'yi seçin.
2. **İşlem**’i ve sonra Öne Çıkanlar listesinde **Windows Server 2016 Datacenter**’ı seçin.
3. Sanal makine için şu değerleri girin:

    - Sanal makinenin adı için *myVM1*.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam**’ı seçin.
5. Seçin **DS1_V2** seçin ve sanal makine boyutu için **seçin**.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğini belirleyin.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’u seçin.

### <a name="install-iis"></a>IIS yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Etkileşimli bir kabuk açın ve olduğundan emin olun **PowerShell**.

    ![Özel uzantıyı yükleme](./media/create-url-route-portal/application-gateway-extension.png)

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

1. Seçin **tüm kaynakları** seçip **myAppGateway**.
2. Seçin **arka uç havuzları**. Uygulama ağ geçidi ile varsayılan bir havuz otomatik olarak oluşturulur. Seçin **appGatewayBackendPool**.
3. Seçin **Ekle hedef** eklemek için *myVM1* appGatewayBackendPool için.

    ![Arka uç sunucuları ekleme](./media/create-url-route-portal/application-gateway-backend.png)

4. **Kaydet**’i seçin.
5. Seçin **arka uç havuzları** seçip **Ekle**.
6. Bir ad girin *imagesBackendPool* ve ekleme *myVM2* kullanarak **Ekle hedef**.
7. **Tamam**’ı seçin.
8. Seçin **Ekle** başka bir arka uç havuzu adı ile tekrar eklemeyi *videoBackendPool* ve ekleme *myVM3* ona.

## <a name="create-a-backend-listener"></a>Bir arka uç dinleyici oluşturun

1. Seçin **dinleyicileri** ve select **temel**.
2. Girin *myBackendListener* adı için *myFrontendPort* ön uç bağlantı noktası adını ve ardından *8080* dinleyicisinin bağlantı noktası.
3. **Tamam**’ı seçin.

## <a name="create-a-path-based-routing-rule"></a>Yola dayalı kural oluşturma

1. Seçin **kuralları** seçip **yol tabanlı**.
2. Girin *bağlanma2* adı.
3. Girin *görüntüleri* ilk yol adı. Girin */images/* \* yolu. Seçin **imagesBackendPool** arka uç havuzu için.
4. Girin *Video* ikinci yol adı. Girin */video/* \* yolu. Seçin **videoBackendPool** arka uç havuzu için.

    ![Yol tabanlı kural oluşturma](./media/create-url-route-portal/application-gateway-route-rule.png)

5. **Tamam**’ı seçin.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Seçin **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresini kaydetme](./media/create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. Örneğin http://40.121.222.19.

    ![Temel URL’yi uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest.png)

3. URL http:// değiştirme&lt;IP adresi&gt;: 8080/images/test.htm, değiştirme &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir:

    ![Görüntü URL’sini uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest-images.png)

4. URL http:// değiştirme&lt;IP adresi&gt;: 8080/video/test.htm, değiştirme &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örnekte olduğu gibi bir şey görmeniz gerekir:

    ![Video URL’sini uygulama ağ geçidinde test etme](./media/create-url-route-portal/application-gateway-iistest-video.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Application gateway ile oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda, kaynak grubunu kaldırın. Kaynak grubu kaldırarak, ayrıca uygulama ağ geçidi ve tüm ilgili kaynakları kaldırın. 

Kaynak grubunu kaldırmak için:

1. Azure portal'ın sol menüsünde **kaynak grupları**.
2. Üzerinde **kaynak grupları** sayfasında, arama **myResourceGroupAG** listesinde seçin.
3. Üzerinde **kaynak grubu sayfasını**seçin **kaynak grubunu Sil**.
4. Girin *myResourceGroupAG* için **kaynak grubu adını yazın** seçip **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Application Gateway ile yapabilecekleriniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)
