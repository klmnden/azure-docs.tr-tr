---
title: "URL yolu tabanlı yönlendirme kuralları ile - Azure portalında bir uygulama ağ geçidi oluşturma | Microsoft Docs"
description: "URL yolu tabanlı yönlendirme kuralları bir uygulama ağ geçidi ve sanal makine ölçek Azure portalını kullanarak ayarlamak için oluşturmayı öğrenin."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: davidmu
ms.openlocfilehash: eb07b1811b017f71a003be26522e6b213a300321
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-application-gateway-with-path-based-routing-rules-using-the-azure-portal"></a>Azure portalını kullanarak yol tabanlı yönlendirme kuralları ile bir uygulama ağ geçidi oluşturma

Azure Portalı'nı yapılandırmak için kullanabileceğiniz [URL yolu tabanlı yönlendirme kuralları](application-gateway-url-route-overview.md) oluştururken bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, sanal makineleri kullanarak arka uç havuzları oluşturun. Uygun sunucuları havuzlarındaki web trafiği ulaştığında emin olun yönlendirme kuralları oluşturursunuz.

Bu makalede, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyici oluşturun
> * Yol tabanlı yönlendirme kuralı oluşturma

![URL yönlendirme örneği](./media/application-gateway-create-url-route-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Oturum açtığınızda Azure portalında [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte, iki alt ağ oluşturulur: bir uygulama ağ geçidi ve arka uç sunucular için diğer için. Uygulama ağ geçidi oluşturma aynı anda bir sanal ağ oluşturabilirsiniz.

1. Tıklatın **yeni** Azure portalında sol üst köşesinde bulundu.
2. Seçin **ağ** ve ardından **uygulama ağ geçidi** öne çıkan listesinde.
3. Uygulama ağ geçidi için bu değerleri girin:

    - *myAppGateway* - uygulama ağ geçidi adı.
    - *myResourceGroupAG* - yeni kaynak grubu için.

    ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-create.png)

4. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
5. Tıklatın **sanal ağ seçin**, tıklatın **Yeni Oluştur**ve ardından sanal ağ için bu değerleri girin:

    - *myVNet* - sanal ağın adı.
    - *10.0.0.0/16* - sanal ağ adres alanı.
    - *myAGSubnet* - alt ağ adı.
    - *10.0.0.0/24* - alt ağ adres alanı.

    ![Sanal ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-vnet.png)

6. Tıklatın **Tamam** sanal ağ ve alt ağ oluşturmak için.
7. Tıklatın **genel bir IP adresi seçin**, tıklatın **Yeni Oluştur**ve ortak IP adresini girin. Bu örnekte adlı ortak IP adresi *myAGPublicIPAddress*. Diğer ayarlar için varsayılan değerleri kabul edin ve ardından **Tamam**.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarını ve uygulama ağ geçidi oluşturmak için. Oluşturulması, dağıtımı, sonraki bölüme geçmeden önce başarıyla tamamlanana kadar bekleyin uygulama ağ geçidi için birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Bir alt ağ Ekle

1. Tıklatın **tüm kaynakları** sol taraftaki menüyü ve ardından **myVNet** kaynakları listesinden.
2. Tıklatın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-url-route-portal/application-gateway-subnet.png)

3. Girin *myBackendSubnet* 'ye tıklayın ve alt ağ adı için **Tamam**.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte uygulama ağ geçidi için arka uç sunucuları olarak kullanılacak üç sanal makine oluşturun. Ayrıca uygulama ağ geçidi başarıyla oluşturulduğunu doğrulamak için sanal makinelerde IIS yükleyin.

1. **Yeni**’ye tıklayın.
2. Tıklatın **işlem** ve ardından **Windows Server 2016 Datacenter** öne çıkan listesinde.
3. Sanal makine için bu değerleri girin:

    - *myVM1* - sanal makine adı için.
    - *azureuser* - yönetici kullanıcı adı.
    - *Azure123456!* parolası.
    - Seçin **var olanı kullan**ve ardından *myResourceGroupAG*.

4. **Tamam**’a tıklayın.
5. Seçin **DS1_V2** tıklatın ve sanal makine boyutu için **seçin**.
6. Olduğundan emin olun **myVNet** sanal ağ ve alt ağ için seçili olan **myBackendSubnet**. 
7. Tıklatın **devre dışı** önyükleme tanılaması devre dışı bırakmak için.
8. Tıklatın **Tamam**, Özet sayfasında ayarları gözden geçirin ve ardından **oluşturma**.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli Kabuğu'nu açın ve onu ayarlandığından emin olun **PowerShell**.

    ![Özel uzantısını yükleyin](./media/application-gateway-create-url-route-portal/application-gateway-extension.png)

2. IIS sanal makineye yüklemek için aşağıdaki komutu çalıştırın: 

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
2. Tıklatın **arka uç havuzları**. Varsayılan bir havuzu uygulama ağ geçidi ile otomatik olarak oluşturuldu. Tıklatın **appGateayBackendPool**.
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

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidi sınama

1. Tıklatın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Uygulama ağ geçidi genel IP adresi kaydı](./media/application-gateway-create-url-route-portal/application-gateway-record-ag-address.png)

2. Genel IP adresini kopyalayın ve ardından, tarayıcınızın adres çubuğuna yapıştırın. Örneğin, http://http: / / 40.121.222.19.

    ![Temel uygulama ağ geçidi URL'de test](./media/application-gateway-create-url-route-portal/application-gateway-iistest.png)

3. URL'nin http:// değiştirme&lt;IP adresi&gt;: 8080/video/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidi görüntüleri URL Sına](./media/application-gateway-create-url-route-portal/application-gateway-iistest-images.png)

4. URL'nin http:// değiştirme&lt;IP adresi&gt;: 8080/video/test.htm, değiştirerek &lt;IP adresi&gt; ile IP adresi ve aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidi olarak test video URL'si](./media/application-gateway-create-url-route-portal/application-gateway-iistest-video.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrenilen nasıl

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç sunucular ile arka uç havuzları oluşturma
> * Arka uç dinleyici oluşturun
> * Yol tabanlı yönlendirme kuralı oluşturma

Uygulama ağ geçitleri ile ilişkili kaynakları hakkında daha fazla bilgi için nasıl yapılır makaleleri devam edin.