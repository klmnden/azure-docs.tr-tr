---
title: Birden çok site barındırma ile - application gateway oluşturma Azure portalı | Microsoft Docs
description: Azure portalını kullanarak birden çok siteye barındıran bir uygulama ağ geçidi oluşturmayı öğrenin.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 85113a5007a171459b831684f584773ba4328b94
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62122429"
---
# <a name="create-an-application-gateway-with-multiple-site-hosting-using-the-azure-portal"></a>Bir uygulama ağ geçidi ile birden çok site barındırma Azure portalını kullanarak oluşturma

Azure portalında yapılandırmak için kullanabileceğiniz [birden çok web sitesi barındırma](application-gateway-multi-site-overview.md) oluşturduğunuzda bir [uygulama ağ geçidi](application-gateway-introduction.md). Bu öğreticide, sanal makine ölçek kümeleri kullanarak arka uç havuzları oluşturun. Ardından sahip olduğunuz dinleyicileri ve kuralları, web trafiğinin havuzlardaki uygun sunuculara ulaşması için yapılandırırsınız. Bu öğreticide, birden çok etki alanları ve kullandığı örnekleri olduğunuz varsayılır *www\.contoso.com* ve *www\.fabrikam.com*.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Dinleyici ve yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

![Çok siteli yönlendirme örneği](./media/application-gateway-create-multisite-portal/scenario.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir sanal ağ, oluşturduğunuz kaynakları arasındaki iletişim için gereklidir. Bu örnekte iki alt ağ oluşturulmuştur: biri uygulama ağ geçidi ve diğeri de arka uç sunucuları içindir. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Uygulama ağ geçidi için şu değerleri girin:

   - *myAppGateway* - Uygulama ağ geçidinin adı.
   - *myResourceGroupAG* - Yeni kaynak grubu.

     ![Yeni uygulama ağ geçidi oluşturma](./media/application-gateway-create-multisite-portal/application-gateway-create.png)

4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:

   - *myVNet* - Sanal ağın adı.
   - *10.0.0.0/16* - Sanal ağın adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.

     ![Sanal ağ oluşturma](./media/application-gateway-create-multisite-portal/application-gateway-vnet.png)

6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Tıklayın **genel bir IP adresi seçin**, tıklayın **Yeni Oluştur**ve ardından genel IP adresini adını girin. Bu örnekte genel IP adresinin adı *myAGPublicIPAddress* şeklindedir. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
8. Dinleyici yapılandırması için varsayılan değerleri kabul edin, Web uygulaması güvenlik duvarı devre dışı bırakın ve ardından **Tamam**.
9. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

### <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sol taraftaki menüde **Tüm kaynaklar**’a ve sonra kaynaklar listesinden **myVNet** öğesine tıklayın.
2. Tıklayın **alt ağlar**ve ardından **alt**.

    ![Alt ağ oluşturma](./media/application-gateway-create-multisite-portal/application-gateway-subnet.png)

3. Alt ağ adı için *myBackendSubnet* girin ve sonra **Tamam**’a tıklayın.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu örnekte, uygulama ağ geçidi için arka uç sunucular olarak kullanılacak iki sanal makine oluşturacaksınız. Ayrıca trafiği doğru yönlendirmeyi doğrulamak için sanal makinelere IIS yüklersiniz.

1. **Yeni**’ye tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:

    - *contosoVM* - sanal makinenin adı.
    - Yönetici kullanıcı adı için *azureuser*.
    - *Azure123456!* Parola.
    - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.

4. **Tamam**'ı tıklatın.
5. Sanal makinenin boyutu için **DS1_V2** seçeneğini belirleyin ve **Seç**’e tıklayın.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun. 
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.

    ![Özel uzantıyı yükleme](./media/application-gateway-create-multisite-portal/application-gateway-extension.png)

2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın: 

    ```azurepowershell-interactive
    $publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1");  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -Location eastus `
      -ExtensionName IIS `
      -VMName contosoVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -Settings $publicSettings
    ```

3. İkinci sanal makine oluşturun ve yalnızca tamamlanmış adımları kullanarak IIS yükleyin. Adlarını girin *fabrikamVM* adı ve Set-AzVMExtension içinde VMName değeri.

## <a name="create-backend-pools-with-the-virtual-machines"></a>Arka uç havuzları ile sanal makineleri oluşturma

1. Tıklayın **tüm kaynakları** ve ardından **myAppGateway**.
2. Tıklayın **arka uç havuzları**ve ardından **Ekle**.
3. Bir ad girin *contosoPool* ve ekleme *contosoVM* kullanarak **Ekle hedef**.

    ![Arka uç sunucuları ekleme](./media/application-gateway-create-multisite-portal/application-gateway-multisite-backendpool.png)

4. **Tamam** düğmesine tıklayın.
5. Tıklayın **arka uç havuzları** ve ardından **Ekle**.
6. Oluşturma *fabrikamPool* ile *fabrikamVM* yalnızca tamamlanmış adımları kullanarak.

## <a name="create-listeners-and-routing-rules"></a>Dinleyici ve yönlendirme kuralları oluşturma

1. Tıklayın **dinleyicileri** ve ardından **çok siteli**.
2. Dinleyici için şu değerleri girin:
    
   - *contosoListener* - dinleyicisinin adını.
   - *www\.contoso.com* -bu konak adı örneği, etki alanı adıyla değiştirin.

3. **Tamam**'ı tıklatın.
4. Adını kullanarak ikinci bir dinleyici oluşturun *fabrikamListener* ve ikinci etki alanı adınızı kullanın. Bu örnekte, *www\.fabrikam.com* kullanılır.

Kurallar listelendikleri sırayla işlenir ve trafik belirginlikten bağımsız olarak eşleşen ilk kural kullanarak yönlendirilir. Örneğin, aynı bağlantı noktasında temel bir dinleyici kullanan bir kuralınız ve çok siteli dinleyici kullanan bir kuralınız varsa çok siteli kuralın beklendiği gibi çalışması için çok siteli dinleyicinin kuralı temel dinleyici kuralından önce listelenmelidir. 

Bu örnekte, iki yeni kural oluşturursunuz ve uygulama ağ geçidini oluştururken oluşturduğunuz varsayılan kuralı silersiniz. 

1. Tıklayın **kuralları** ve ardından **temel**.
2. Girin *contosoRule* adı.
3. Seçin *contosoListener* dinleyici için.
4. Seçin *contosoPool* arka uç havuzu için.

    ![Yol tabanlı kural oluşturma](./media/application-gateway-create-multisite-portal/application-gateway-multisite-rule.png)

5. **Tamam**'ı tıklatın.
6. Adlarını kullanarak ikinci bir kural oluşturun *fabrikamRule*, *fabrikamListener*, ve *fabrikamPool*.
7. Adlı varsayılan kuralı *bağlanma1* tıklayarak ve ardından tıklatarak **Sil**.

## <a name="create-a-cname-record-in-your-domain"></a>Etki alanınızda bir CNAME kaydı oluşturma

Uygulama ağ geçidi genel IP adresiyle oluşturulduktan sonra DNS adresini alabilir ve etki alanınızda bir CNAME kaydı oluşturmak için kullanabilirsiniz. Uygulama ağ geçidi yeniden başlatıldığında VIP değişebileceğinden A kayıtlarının kullanımı önerilmez.

1. Tıklayın **tüm kaynakları**ve ardından **myAGPublicIPAddress**.

    ![Kayıt uygulama ağ geçidi DNS adresi](./media/application-gateway-create-multisite-portal/application-gateway-multisite-dns.png)

2. DNS adresi kopyalayın ve yeni bir CNAME kaydını etki alanınızda değeri olarak kullanın.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

1. Tarayıcınızın adres çubuğuna, etki alanı adınızı girin. Örneğin http://www.contoso.com.

    ![Uygulama ağ geçidinde contoso test etme](./media/application-gateway-create-multisite-portal/application-gateway-iistest.png)

2. Adresi diğer etki alanınızla değiştirin, aşağıdaki örneğe benzer bir şey görmeniz gerekir:

    ![Uygulama ağ geçidinde fabrikam sitesini test etme](./media/application-gateway-create-multisite-portal/application-gateway-iistest2.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Uygulama ağ geçidi oluşturma
> * Arka uç sunucuları için sanal makineler oluşturun
> * Arka uç havuzları ile arka uç sunucular oluşturma
> * Dinleyici ve yönlendirme kuralları oluşturma
> * Etki alanınızda bir CNAME kaydı oluşturma

> [!div class="nextstepaction"]
> [Uygulama ağ geçidi ile neler yapabileceğiniz hakkında daha fazla bilgi edinin](application-gateway-introduction.md)
