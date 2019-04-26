---
title: Öğretici - Ağırlıklı Uç noktaları için trafiği yönlendirme - Azure Traffic Manager
description: Bu öğretici makalede, Traffic Manager'ı kullanarak trafiği ağırlıklı uç noktalara yönlendirme açıklanmaktadır.
services: traffic-manager
author: KumudD
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: kumud
ms.openlocfilehash: 50790e50602fbc8d302a67ea9963a4e492ce2f0b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60329687"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-by-using-traffic-manager"></a>Öğretici: Denetim trafik Traffic Manager'ı kullanarak ağırlıklı uç noktalar ile yönlendirme

Bu öğreticide, ağırlıklı yönlendirme yöntemini kullanarak kullanıcı trafiğini uç noktalar arasında yönlendirmeyi denetlemek için Azure Traffic Manager'ı kullanma adımları açıklanmaktadır. Bu yönlendirme yönteminde Traffic Manager profil yapılandırmasında her uç noktasına bir ağırlık değeri atarsınız. Kullanıcı trafiği, her bir uç noktaya atanan ağırlık değerine göre yönlendirilir. Ağırlık 1 ile 1.000 arasında bir tamsayıdır. Bir uç noktaya ne kadar yüksek bir ağırlık değeri atanırsa uç nokta o kadar yüksek önceliğe sahip olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma.
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma.
> * IIS çalıştıran VM'lerin DNS adını yapılandırma.
> * Traffic Manager profili oluşturma.
> * Traffic Manager profiline VM uç noktaları ekleme.
> * Traffic Manager'ın nasıl çalıştığını görün.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Traffic Manager'ın nasıl çalıştığını görmek için bu öğretici kapsamında aşağıdaki kaynakları dağıtın:
- Farklı Azure bölgelerinde çalıştırılan temel Web siteleri iki örneği: Doğu ABD ve Batı Avrupa.
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM Doğu ABD bölgesinde, diğeri ise Batı Avrupa bölgesinde olmalıdır. Traffic Manager’ın kullanıcı trafiğini uç noktasına daha yüksek bir ağırlık atanan bir web sitesine nasıl yönlendirdiğini göstermek için test VM’leri kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesini oluşturmak için, aşağıdaki adımları tamamlayın:
1. Biri Doğu ABD diğeri Batı Avrupa bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'ye de IIS sunucusu yükleyin. Varsayılan web sayfasını, web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma
Bu bölümde Doğu ABD ve Batı Avrupa Azure bölgelerinde *myIISVMEastUS* ve *myIISVMWEurope* adlı iki VM oluşturacaksınız.

1. Azure portalın sol üst köşesinde **Kaynak oluştur** > **İşlem** > **Windows Server 2016 VM**'yi seçin.
2. **Temel Bilgiler** bölümünde aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Oluştur**’u seçin.

    |Ayar|Değer|
    |---|---|
    |Ad|**myIISVMEastUS** yazın.|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Yeni**'yi seçin ve **myResourceGroupTM1** girin.|
    |Location| **Doğu ABD**’yi seçin.|
    |||

4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’ı belirleyin:
    
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| **Sanal ağ**'ı seçin. **Sanal ağ oluştur** sayfasında **Ad** alanına **myVNet1** girin. **Alt ağ** için **mySubnet** yazın.|
    |Ağ Güvenliği Grubu|**Temel**'i seçin. **Ortak gelen bağlantı noktası seçin** açılan menüsünde **HTTP** ve **RDP**'yi seçin. |
    |Önyükleme tanılamaları|**Devre dışı** girişini seçin.|
    |||

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. Aşağıdaki değişikliklerle birlikte 1.-6. adımları tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Kaynak grubu | **Yeni**'yi seçin ve **myResourceGroupTM2** girin.|
    |Location|**Batı Avrupa** yazın.|
    |VM Adı | **myIISVMWEurope** yazın.|
    |Sanal ağ | **Sanal ağ**'ı seçin. **Sanal ağ oluştur** sayfasında **Ad** alanına **myVNet2** girin. **Alt ağ** için **mySubnet** yazın.|
    |||

8. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki VM de oluşturulmadan diğer adımlara geçmeyin.

![VM oluşturma](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-webpage"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iki sanal makinelere IIS sunucusunu yükleme&mdash;myIISVMEastUS ve myIISVMWEurope&mdash;ve sonra varsayılan Web sayfasını güncelleştirin. Özelleştirilmiş web sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myIISVMEastUS** seçeneğini belirleyin.
2. **Genel Bakış** sayfasında **Bağlan**'ı seçin. **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen .rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. VM'yi oluştururken yapılandırdığınız kullanıcı adı ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler** > **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları** > **Sunucu Yöneticisi** bölümüne gidin.
7. VM1'de Windows PowerShell'i açın. IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.
    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default .htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom .htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

    ![IIS yükleme ve web sayfasını özelleştirme](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. **myIISVMEastUS** ile RDP bağlantısını kapatın.
9. 1-8 arası adımları tekrarlayın. **myResourceGroupTM2** kaynak grubu içindeki **myIISVMWEurope** adlı VM ile RDP bağlantısı kurarak IIS yükleyip varsayılan web sayfasını özelleştirin.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde myIISVMEastUS ve myIISVMWEurope adlı IIS sunucularının DNS adlarını yapılandıracaksınız.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myIISVMEastUS** seçeneğini belirleyin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyin. Daha sonra **Kaydet**’e tıklayın.
4. 1-3 arası adımları **myResourceGroupTM1** kaynak grubundaki **myIISVMWEurope** adlı VM için tekrarlayın.

### <a name="create-a-test-vm"></a>Test VM’si oluşturma

Bu bölümde, *mVMEastUS* adlı VM'yi oluşturacaksınız. Bu VM’yi Traffic Manager’ın trafiği daha yüksek ağırlık değerine sahip olan web sitesi uç noktasına nasıl yönlendirdiğini test etmek için kullanacaksınız.

1. Azure portalın sol üst köşesinde **Kaynak oluştur** > **İşlem** > **Windows Server 2016 VM**'yi seçin.
2. **Temel Bilgiler** bölümünde aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|**myVMEastUS** yazın.|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Var olanı kullan**’ı seçin ve sonra **myResourceGroupTM1** öğesini seçin.|
    |||

4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’ı belirleyin:

    |Ayar|Değer|
    |---|---|
    |Sanal ağ| **Sanal ağ**'ı seçin. **Sanal ağ oluştur** sayfasında **Ad** alanına **myVNet3** girin. Alt ağ için **mySubnet** yazın.|
    |Ağ Güvenliği Grubu|**Temel**'i seçin. **Ortak gelen bağlantı noktası seçin** açılan menüsünde **HTTP** ve **RDP**'yi seçin. |
    |Önyükleme tanılamaları|**Devre dışı** girişini seçin.|
    |||

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.
8. Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulmadan diğer adımlara geçmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
**Ağırlıklı** yönlendirme yöntemini temel alan bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. **Traffic Manager profili oluştur** bölümünde aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Oluştur**’u seçin.

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ad                   | trafficmanager.net bölgesi içinde benzersiz bir ad girin. Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adında bulunur.                                   |
    | Yönlendirme yöntemi          | **Ağırlıklı** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | **Var olanı kullan**’ı seçin ve sonra **myResourceGroupTM1** öğesini seçin. |
    |        |   |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

IIS sunucuları myIISVMEastUS ve myIISVMWEurope, kullanıcı trafiği yönlendirmek için bunları çalıştıran iki sanal makine ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçlardan profili seçin.
2. **Traffic Manager profilinin** **Ayarlar** bölümünde **Uç noktalar** > **Ekle**'yi seçin.
3. Aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Tamam**’ı seçin.

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktasını girin.                                   |
    | Ad           | **myEastUSEndpoint** yazın.                                        |
    | Hedef kaynak türü           | **Genel IP adresi**'ni seçin.                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için genel IP adresi seçin. **Kaynak** bölümünde **myIISVMEastUS-ip** adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Ağırlık      | **100** değerini girin.        |
    |        |           |

4. 2 ve 3 numaralı adımları tekrarlayarak **myIISVMWEurope-ip** genel IP adresi için **myWestEuropeEndpoint** adlı başka bir uç nokta ekleyin. Bu adres, myIISVMWEurope adlı IIS sunucusu VM'si ile ilişkilendirilir. **Ağırlık** için **25** değerini girin.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte Traffic Manager profili bölümünde gösterilir.

## <a name="test-the-traffic-manager-profile"></a>Traffic Manager profilini test etme
Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:
1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görün.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme
Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
1. **Genel Bakış**’ı seçin.
2. Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myVMEastUS** seçeneğini belirleyin.
2. **Genel Bakış** sayfasında **Bağlan**'ı seçin. **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen .rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler** > **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Web sitesini görüntülemek için myVMEastUS adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını girin. Ağırlık değeri **100** ile daha yüksek olduğundan myIISVMEastUS üzerindeki IIS sunucusunda barındırılan web sitesine yönlendirilirsiniz. myIISVMWEurope adlı IIS sunucusu, **25** ile daha düşük bir uç nokta ağırlık değerine sahiptir.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme
Bu öğreticide oluşturduğunuz kaynak gruplarına ihtiyacınız kalmadığında onları silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcının coğrafi konumuna göre trafiği belirli uç noktalara yönlendirme](traffic-manager-configure-geographic-routing-method.md)
