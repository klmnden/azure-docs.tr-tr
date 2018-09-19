---
title: Azure Traffic Manager'ı kullanarak alt ağ trafiği yönlendirme yöntemini yapılandırma | Microsoft Docs
description: Bu makalede, Traffic Manager, belirli alt ağlardan trafiği yönlendirmek için yapılandırma açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jpconnock
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: kumud
ms.openlocfilehash: 6e5e6008741306d322ebd07bcbf144133ca4e632
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131283"
---
# <a name="direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Traffic Manager'ı kullanarak kullanıcı alt ağa dayalı belirli Uç noktalara doğrudan trafik

Bu makalede, alt ağ trafik yönlendirme yöntemini yapılandırma açıklar. **Alt** trafik yönlendirme yöntemine özel uç noktaları için bir dizi IP adresi aralıklarını eşlemenizi sağlar ve Traffic Manager tarafından bir istek alındığında, isteğin kaynak IP inceler ve ilişkili uç noktasını döndürür. 

Alt ağ yönlendirme, kullanıcının sorgu bağlı olarak IP adresini kullanarak, bu makalede açıklanan senaryoda trafik ya da bir iç Web sitesine veya bir üretim Web sitesi için yönlendirilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Traffic Manager iş başında görmek için Bu öğretici aşağıdaki dağıttığınız gereklidir:
- iki temel Web siteleri çalışan farklı Azure bölgelerindeki - **Doğu ABD** (iç Web sitesi olarak hizmet eder) ve **Batı Avrupa** (üretim Web sitesi olarak görev yapar).
- Traffic Manager - bir VM ile test etmek için iki VM test **Doğu ABD** ve ikinci bir sanal makine içinde **Batı Avrupa**. 

Test sanal makineleri kullanıcı trafik Traffic Manager iç Web sitesine veya kullanıcı sorgusu geldiği alt temelinde üretim Web sitesi nasıl yönlendirdiğini göstermek için kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

### <a name="create-websites"></a>Web siteleri oluşturma

Bu bölümde, Traffic Manager profiline iki Azure bölgesi için iki hizmet uç noktaları sağlayan iki Web sitesi örnekleri oluşturun. İki Web sitesi oluşturmak için aşağıdaki adımları içerir:
1. Temel Web sitesi - birinde çalıştırmak için iki VM oluşturma **Doğu ABD**ve diğer **Batı Avrupa**.
2. Her sanal makinede IIS sunucusu yüklemek ve bir kullanıcının Web sitesini ziyaret ederken bağlı olduğu VM adı açıklayan varsayılan Web sayfası.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma
Bu bölümde, iki VM oluşturma *myEndpointVMEastUS* ve *myEndpointVMWEurope* içinde **Doğu ABD** ve **Batı Avrupa** Azure bölgeleri.

1. Üst üzerinde köşe seçin Azure portalının sol **kaynak Oluştur** > **işlem** > **Windows Server 2016 VM**.
2. **Temel Bilgiler** için aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myIISVMEastUS|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| Seçin **yeni** yazın *myResourceGroupTM1*.|
    |Konum| **Doğu ABD**’yi seçin.|
    |||
4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:
    
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet1*, alt ağ için girin  *mySubnet*.|
    |Ağ Güvenliği Grubu|Seçin **temel**hem de **seçin ortak gelen bağlantı noktası** açılan listesinde, select **HTTP** ve **RDP** |
    |Önyükleme tanılamaları|Seçin **devre dışı bırakılmış**.|
    |||
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. Aşağıdaki değişikliklerle birlikte 1.-6. adımları tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Kaynak grubu | **Yeni**'yi seçin ve *myResourceGroupTM2* yazın.|
    |Konum|Batı Avrupa|
    |VM Adı | myIISVMWEurope|
    |Sanal ağ | Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet2*, alt ağ için girin  *mySubnet*.|
    |||
8. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

   ![VM oluşturma](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iki Vm'lerde - IIS sunucusunu yükleme *myIISVMEastUS*  & *myIISVMWEurope*ve sonra varsayılan Web sitesi sayfası güncelleştirin. Özelleştirilmiş Web sayfası, bir web tarayıcısından Web sitesini ziyaret ettiğinizde, bağlandığınız ve VM adını gösterir.

1. Seçin **tüm kaynakları** tıklatın soldaki menüden ve ardından kaynak listesinden *myIISVMEastUS* bulunan *myResourceGroupTM1* kaynak grubu.
2. Üzerinde **genel bakış** sayfasında **Connect**ve ardından **sanal makineye bağlanma**seçin **indirme RDP dosyası**. 
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
7. Başlatma Windows PowerShell'i temel *myIISVMEastUS* ve aşağıdaki komutları kullanarak IIS sunucusu yüklemek ve varsayılan htm dosyasını güncelleştirin.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my test website server - " + $env:computername)
    ```
8. RDP bağlantısıyla kapatmak *myIISVMEastUS*.
9. VM ile RDP bağlantısı oluşturarak 1-6 adımlarını yineleyin *myIISVMWEurope* içinde *myResourceGroupTM2* IIS yüklemek ve varsayılan web sayfasını özelleştirmek için kaynak grubu.
10. Başlatma Windows PowerShell'i temel *myIISVMWEurope* ve aşağıdaki komutları kullanarak IIS sunucusu yüklemek ve varsayılan htm dosyasını güncelleştirin.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my production website server - " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'ler için DNS adlarını yapılandırın

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarını DNS adını temel alarak yönlendirir. Bu bölümde, IIS sunucuları - DNS adlarını yapılandırma *myIISVMEastUS* ve *myIISVMWEurope*.

1. Tıklayın **tüm kaynakları** sol taraftaki menüden, seçin ve sonra kaynak listesinden, *myIISVMEastUS* bulunan *myResourceGroupTM1* kaynak grubu.
2. Üzerinde **genel bakış** sayfasındaki **DNS adı**seçin **yapılandırma**.
3. Üzerinde **yapılandırma** sayfasında, DNS ad etiketi altında bir benzersiz ad ve ardından eklemek **Kaydet**.
4. 1-3 ' adlı VM için adımlarını *myIISVMWEurope* bulunan *myResourceGroupTM1* kaynak grubu.

### <a name="create-test-vms"></a>Oluşturma Vm'leri test edin

Bu bölümde, bir VM oluşturun (*mVMEastUS* ve *myVMWestEurope*) her bir Azure bölgesinde (**Doğu ABD** ve **Batı Avrupa**. Bu sanal makineler, Web sitesine göz attığınızda Traffic Manager en yakın IIS sunucusuna trafiği nasıl yönlendirdiğini test etmek için kullanır.

1. Üst üzerinde köşe seçin Azure portalının sol **kaynak Oluştur** > **işlem** > **Windows Server 2016 VM**.
2. **Temel Bilgiler** için aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVMEastUS|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Var olan**’ı seçin ve sonra *myResourceGroupTM1* öğesini seçin.|
    |||

4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet3*, alt ağ için girin  *mySubnet3*.|
    |Ağ Güvenliği Grubu|Seçin **temel**hem de **seçin ortak gelen bağlantı noktası** açılan listesinde, select **HTTP** ve **RDP** |
    |Önyükleme tanılamaları|Seçin **devre dışı bırakılmış**.|
    |||

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. 1-5 arası adımları aşağıdaki değişikliklerle birlikte tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |VM Adı | *myVMWEurope*|
    |Kaynak grubu | Seçin **varolan**, Anahtar'a tıklayın ve *myResourceGroupTM2*|
    |Sanal ağ | Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet4*, alt ağ için girin  *mySubnet4*.|
    |||

8. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
Özel uç noktaları isteğin kaynak IP tabanlı iade etmenize olanak tanıyan bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. **Traffic Manager profili oluştur** ekranında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**'u seçin:
    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ad                   | Bu adın trafficmanager.net bölge ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı sonuçlarında içinde benzersiz olması gerekir.                                   |
    | Yönlendirme yöntemi          | Seçin **alt** yönlendirme yöntemi.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | Seçin **varolan** girin *myResourceGroupTM1*. |
    | |                              |
    |
  
    ![Traffic Manager profili oluşturma](./media/traffic-manager-subnet-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

IIS çalıştıran iki sanal makine ekleme sunucuları - *myIISVMEastUS*  & *myIISVMWEurope* kullanıcının sorgu alt ağa dayalı kullanıcı trafiği yönlendirmek için.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Ad           | myTestWebSiteEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | **Bir genel IP adresi seçin** genel IP adresleri aynı abonelik altında kaynaklarla listesini göstermek için. İçinde **kaynak**, adlı ortak IP adresi seçin *myIISVMEastUS IP*. Bu VM Doğu ABD IIS sunucusunun genel IP adresidir.|
    |  Alt ağ yönlendirme ayarları    |   IP adresini ekleyin *myVMEastUS* VM test edin. Bu sanal makineden kaynaklanan herhangi bir kullanıcı sorgu yönlendirilirsiniz *myTestWebSiteEndpoint*.    |

4. 2 ve 3 adlı başka bir uç nokta ekleme *myProductionEndpoint* genel IP adresi için *myIISVMWEurope IP* IIS sunucusu adlı VM ile ilişkili *myIISVMWEurope* . İçin **alt ağ yönlendirme ayarları**, test sanal makinesi - IP adresini ekleyin *myVMWestEurope*. VM uç noktaya - yönlendirilecek bu test herhangi bir kullanıcı sorgu *myProductionWebsiteEndpoint*.
5.  Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

    ![Traffic Manager uç noktası ekleme](./media/traffic-manager-subnet-routing-method/customize-endpoint-with-subnet-routing-eastus.png)
  
## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme
Bu bölümde, kullanıcı trafiğinin belirli bir alt ağdan belirli bir uç nokta Traffic Manager tarafından nasıl yönlendirdiği test edin. Traffic Manager uygulamada görüntülemek için aşağıdaki adımları tamamlayın:
1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager eylemi gibi görüntüleyin:
    - VM testinden (*myVMEastUS*) bulunan **Doğu ABD** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.
    - VM testinden (*myVMEastUS*) bulunan **Batı Avrupa** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profili DNS adını belirleme
Bu öğreticide kolaylık olması için Traffic Manager profilini DNS adını Web sitelerini ziyaret etmek için kullanın. 

Traffic Manager profilini DNS adı şu şekilde belirleyebilirsiniz:

1.  Portalın arama çubuğunda önceki bölümde oluşturduğunuz **Traffic Manager profili** adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
1. **Genel Bakış**'a tıklayın.
2. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımında, bir DNS CNAME kaydı kullanılarak bir gösterim etki alanı adını Traffic Manager etki alanı adına işaret edecek şekilde yapılandırın.

   ![Traffic Manager DNS adı](./media/traffic-manager-subnet-routing-method/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
Bu bölümde, Traffic Manager eylemdir görebilirsiniz. 

1. Seçin **tüm kaynakları** tıklatın soldaki menüden ve ardından kaynak listesinden *myVMEastUS* bulunan *myResourceGroupTM1* kaynak grubu.
2. Üzerinde **genel bakış** sayfasında **Connect**ve ardından **sanal makineye bağlanma**seçin **indirme RDP dosyası**. 
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin. 
1. VM üzerinde bir web tarayıcısında *myVMEastUS*, Traffic Manager profilinizin, Web sitesini görüntülemek için DNS adını yazın. VM beri *myVMEastUS* uç noktası ile ilişkili IP adresi *myIISVMEastUS*, Test Web sunucusu - web tarayıcısını başlatır *myIISVMEastUS*.

   ![Traffic Manager profilini test etme](./media/traffic-manager-subnet-routing-method/test-traffic-manager.png)

2. Ardından, VM'ye bağlanmak *myVMWestEurope* bulunan **Batı Avrupa** adımları kullanarak 1-5 ve göz atma için Traffic Manager profili etki alanı adı bu VM'den. VM beri *myVMWestEurope* uç noktası ile ilişkili IP adresi *myIISVMEastUS*, Test Web sunucusu - web tarayıcısını başlatır *myIISVMWEurope*. 
  
## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini Sil
Artık gerekli olmadığında kaynak gruplarını silin (**ResourceGroupTM1** ve **ResourceGroupTM2**). Bunu yapmak için kaynak grubunu seçin (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [ağırlıklı trafik yönlendirme yöntemini](traffic-manager-configure-weighted-routing-method.md).
- Hakkında bilgi edinin [öncelikli yönlendirme yöntemini](traffic-manager-configure-priority-routing-method.md).
- Hakkında bilgi edinin [coğrafi yönlendirme yöntemini](traffic-manager-configure-geographic-routing-method.md).


