---
title: Azure Application Gateway bir özel ön uç IP adresiyle yapılandırın.
description: Bu makale, bir özel ön uç IP adresi ile uygulama ağ geçidi yapılandırma hakkında bilgi sağlar.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/26/2019
ms.author: absha
ms.openlocfilehash: cfc63349e20aa6dbef4e0d31e81842d325bd3ec6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66134599"
---
# <a name="configure-an-application-gateway-with-an-internal-load-balancer-ilb-endpoint"></a>Bir iç yük dengeleyici (ILB) uç noktası ile uygulama ağ geçidi yapılandırma

Azure Application Gateway, Internet'e yönelik VIP veya değil (ön uç IP adresi için bir özel IP kullanarak), Internet'e açık olan, iç yük dengeleyici (ILB) uç noktası da bilinen bir iç uç nokta ile yapılandırılabilir. Ön uç özel IP adresini kullanarak ağ geçidini yapılandırma Internet'e sunulmamış iç iş kolu satır uygulama için kullanışlıdır. Güvenlik sınırı içinde bulunan, İnternet’e sunulmamış ancak hala hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya Güvenli Yuva Katmanı (SLL) sonlandırması gerektiren çok katmanlı uygulamalar içindeki hizmetler ve katmanlar için de kullanışlıdır.

Bu makalede Azure Portalı'ndan ön uç özel IP adresine sahip bir uygulama ağ geçidi yapılandırma adımlarında size kılavuzluk eder.

Bu makalede, öğreneceksiniz nasıl yapılır:

- Özel ön uç IP yapılandırması için bir uygulama ağ geçidi oluşturma
- Özel ön uç IP yapılandırması ile bir uygulama ağ geçidi oluşturma


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Azure'da oturum açma

<https://portal.azure.com> adresinden Azure portalında oturum açın

## <a name="create-an-application-gateway"></a>Uygulama ağ geçidi oluşturma

Oluşturduğunuz kaynaklar arasında iletişim kurmak Azure için sanal ağ gerekir. Yeni bir sanal ağ oluşturma veya var olanı kullanın. Bu örnekte, yeni bir sanal ağ oluşturacağız. Uygulama ağ geçidini oluştururken aynı zamanda bir sanal makine oluşturabilirsiniz. Uygulama ağ geçidi örnekleri ayrı alt ağlara oluşturulur. Bu örnekte iki alt ağa oluşturursunuz: bir uygulama ağ geçidi ve diğeri arka uç sunucuları için.

1. Tıklayın **yeni** Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **Ağ** ve ardından Öne Çıkanlar listesinde **Application Gateway**’i seçin.
3. Girin *myAppGateway* uygulama ağ geçidinin adını ve *myResourceGroupAG* yeni kaynak grubu için.
4. Diğer ayarların varsayılan değerlerini kabul edin ve sonra **Tamam**’a tıklayın.
5. Tıklayın **bir sanal ağ seçin**, tıklayın **Yeni Oluştur**ve ardından sanal ağ için şu değerleri girin:
   - myVNet * - sanal ağ adı için.
   - 10.0.0.0/16* - sanal ağ adres alanı.
   - *myAGSubnet* - Alt ağın adı.
   - *10.0.0.0/24* - Alt ağın adres alanı.  
     ![Özel frontendip 1](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-1.png)
6. Sanal ağı ve alt ağı oluşturmak için **Tamam**’a tıklayın.
7. Ön uç IP yapılandırmasını özel olarak seçin ve isteğe bağlı olarak varsayılan olarak, dinamik bir IP adresi ataması olur. İlk kullanılabilir adresi seçilen alt ağ ön uç IP adresi olarak atanır.
8. Özel bir IP alt ağ adres aralığı (statik ayırma) seçmek istiyorsanız kutuyu tıklatın **belirli bir özel IP adresi seçin** ve IP adresini belirtin.
   > [!NOTE]
   > IP adresi türü (statik veya dinamik), daha sonra atandıktan sonra değiştirilemez.
9. Protokol ve bağlantı noktası, WAF yapılandırma (gerekirse) için dinleyici yapılandırmanızı seçin ve Tamam'a tıklayın.
    ![Özel frontendip 2](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-2.png)
10. Özet sayfasında ayarları gözden geçirin ve ardından **Tamam** ağ kaynaklarının ve uygulama ağ geçidi oluşturmak için. Bu, uygulama ağ geçidinin oluşturulması, sonraki bölüme geçmeden önce dağıtımın başarıyla tamamlanana kadar bekleyin birkaç dakika sürebilir.

## <a name="add-backend-pool"></a>Arka uç havuzu ekle

Arka uç havuzu, isteği sunan arka uç sunucuları istekleri yönlendirmek için kullanılır. Arka uç ağ, sanal makine ölçek kümeleri, genel IP'ler birleştirilebilir, iç IP'ler, tam etki alanı adlarını (FQDN) ve çok kiracılı arka-Azure App Service gibi biter. Bu örnekte, hedef arka uç olarak sanal makineler kullanacağız. Biz varolan sanal makineleri kullanabilir veya yeni bir tane oluşturun. Bu örnekte, Azure application gateway için arka uç sunucular olarak kullanan iki sanal makine oluşturacağız. Bunu yapmak için yapacağız:

1. 2 yeni VM oluşturma *myVM* ve *myVM2*, arka uç sunucular olarak kullanılacak.
2. Uygulama ağ geçidi başarıyla oluşturulduğunu doğrulamak için sanal makinelere IIS yüklersiniz.
3. Arka uç sunucularının arka uç havuzuna ekleyin.

### <a name="create-a-virtual-machine"></a>Bir sanal makine oluştur

1. **Yeni**’ye tıklayın.
2. Tıklayın **işlem** seçip **Windows Server 2016 Datacenter** Öne çıkanlar listesinde.
3. Sanal makine için şu değerleri girin:
   - *myVM* - Sanal makinenin adı.
   - Yönetici kullanıcı adı için *azureuser*.
   - *Azure123456!* Parola.
   - **Mevcut olanı kullan**’ı seçin ve *myResourceGroupAG* seçeneğini belirleyin.
4. **Tamam**'ı tıklatın.
5. Seçin **DS1_V2** tıklayın ve sanal makine boyutu için **seçin**.
6. Sanal ağ için **myVNet** öğesinin seçili olduğundan ve alt ağın **myBackendSubnet** olduğundan emin olun.
7. Önyükleme tanılamalarını devre dışı bırakmak için **Devre Dışı** seçeneğine tıklayın.
8. **Tamam**’a tıklayın, özet sayfasındaki ayarları gözden geçirin ve sonra **Oluştur**’a tıklayın.

### <a name="install-iis"></a>IIS yükleme

1. Etkileşimli kabuğu açın ve **PowerShell**’e ayarlandığından emin olun.
    ![Özel frontendip 3](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-3.png)
2. Sanal makineye IIS yüklemek için aşağıdaki komutu çalıştırın:

   ```azurepowershell
   Set-AzVMExtension `
   
     -ResourceGroupName myResourceGroupAG `
   
     -ExtensionName IIS `
   
     -VMName myVM `
   
     -Publisher Microsoft.Compute `
   
     -ExtensionType CustomScriptExtension `
   
     -TypeHandlerVersion 1.4 `
   
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' -Location EastUS  ```



3. Create a second virtual machine and install IIS using the steps that you just finished. Enter myVM2 for its name and for VMName in Set-AzVMExtension.

### Add backend servers to backend pool

1. Click **All resources**, and then click **myAppGateway**.
2. Click **Backend pools**. A default pool was automatically created with the application gateway. Click **appGatewayBackendPool**.
3. Click **Add target** to add each virtual machine that you created to the backend pool.
   ![private-frontendip-4](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-4.png)
4. Click **Save.**

## Test the application gateway

1. Check your frontend IP that got assigned by clicking the **Frontend IP Configurations** blade in the portal.
    ![private-frontendip-5](./media/configure-application-gateway-with-private-frontend-ip/private-frontendip-5.png)
2. Copy the private IP address, and then paste it into the address bar of your browser of a VM in the same VNet or on-premises which has connectivity to this VNet and try to access the Application Gateway.

## Next steps

In this tutorial, you learned how to:

- Create a private frontend IP configuration for an Application Gateway
- Create an application gateway with private frontend IP configuration

If you want to monitor the health of your backend, see [Application Gateway Diagnostics](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
