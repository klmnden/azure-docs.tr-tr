---
title: Öğretici - Azure Traffic Manager kullanarak trafiği ağırlıklı uç noktalara yönlendirme | Microsoft Docs
description: Bu öğretici makalede, Traffic Manager kullanarak trafiği ağırlıklı uç noktalara yönlendirme açıklanmaktadır.
services: traffic-manager
author: KumudD
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: kumud
ms.openlocfilehash: 171a9cfe5b5b63efde31c37cd49ef548f1e4ebcc
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649445"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-using-traffic-manager"></a>Öğretici: Traffic Manager kullanarak ağırlıklı uç noktalar ile trafiği denetleme 

Bu öğreticide, ağırlıklı yönlendirme yöntemini kullanarak kullanıcı trafiğini uç noktalar arasında yönlendirmeyi denetlemek için Traffic Manager kullanma açıklanmaktadır. Bu yönlendirme yönteminde, Traffic Manager profil yapılandırmasında her uç nokta için bir ağırlık atarsınız ve kullanıcı trafiği her uç noktaya atanan ağırlığa göre yönlendirilir. Ağırlık 1-1000 arasında bir tamsayıdır. Bir uç noktaya ne kadar yüksek bir ağırlık değeri atanırsa uç nokta o kadar yüksek önceliğe sahip olur.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma
> * IIS çalıştıran VM'lerin DNS adını yapılandırma
> * Traffic Manager profili oluşturma
> * Traffic Manager profiline VM uç noktaları ekleme
> * Traffic Manager'ın nasıl çalıştığını görün

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticide Traffic Manager'ın çalışmasını uygulamalı olarak görmek için şu sistemleri dağıtmanız gerekir:
- farklı Azure bölgelerinde (**Doğu ABD** ve **Batı Avrupa**) çalışan iki temel web sitesi örneği.
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM **Doğu ABD** bölgesinde, ikinci VM ise **Batı Avrupa** bölgesinde olmalıdır. Traffic Manager’ın kullanıcı trafiğini uç noktasına daha yüksek bir ağırlık atanan bir web sitesine nasıl yönlendirdiğini göstermek için test VM’leri kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesini oluşturmak için, aşağıdaki adımları tamamlayın:
1. Biri **Doğu ABD** diğeri **Batı Avrupa** bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'de de IIS sunucusu yükleyin ve varsayılan web sitesi sayfasını web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma
Bu bölümde **Doğu ABD** ve **Batı Avrupa** Azure bölgelerinde *myIISVMEastUS* ve *myIISVMWEurope* adlı iki VM oluşturacaksınız.

1. Azure portalın sol üst köşesinde **Kaynak oluştur** > **İşlem** > **Windows Server 2016 VM**'yi seçin.
2. **Temel Bilgiler** için aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myIISVMEastUS|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Yeni**'yi seçin ve *myResourceGroupTM1* yazın.|
    |Konum| **Doğu ABD**’yi seçin.|
    |||
4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:
    
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| **Sanal ağ**'ı seçin, **Sanal ağ oluştur** bölümündeki **Ad** alanına *myVNet1*, alt ağ alanına da *mySubnet* yazın.|
    |Ağ Güvenliği Grubu|**Temel**'i seçin ve **Ortak gelen bağlantı noktası seçin** açılan menüsünde **HTTP** ve **RDP**'yi seçin. |
    |Önyükleme tanılamaları|**Devre dışı** girişini seçin.|
    |||
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. Aşağıdaki değişikliklerle birlikte 1.-6. adımları tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Kaynak grubu | **Yeni**'yi seçin ve *myResourceGroupTM2* yazın.|
    |Konum|Batı Avrupa|
    |VM Adı | myIISVMWEurope|
    |Sanal ağ | **Sanal ağ**'ı seçin ve **Sanal ağ oluştur** bölümündeki **Ad** alanına *myVNet2*, alt ağ alanına da *mySubnet* yazın.|
    |||
8. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

![VM oluşturma](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde *myIISVMEastUS*  & *myIISVMWEurope* adlı iki VM'ye IIS sunucusunu yükleyecek ve varsayılan web sitesi sayfasını güncelleştireceksiniz. Özelleştirilmiş web sitesi sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin. 
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
7. VM1’de Windows PowerShell’i başlatın ve IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![IIS yükleme ve web sayfasını özelleştirme](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. *myIISVMEastUS* ile RDP bağlantısını kapatın.
9. 1-8 arası adımları tekrarlayın ve *myResourceGroupTM2* kaynak grubu içindeki *myIISVMWEurope* adlı VM ile RDP bağlantısı kurarak IIS yükleyip varsayılan web sayfasını özelleştirin.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde *myIISVMEastUS* ve *myIISVMWEurope* adlı IIS sunucularının DNS adlarını yapılandıracaksınız.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesini seçin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyip **Kaydet**'i seçin.
4. 1-3 arası adımları *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMWEurope* adlı VM için tekrarlayın.

### <a name="create-a-test-vm"></a>Test VM’si oluşturma

Bu bölümde, *mVMEastUS* adlı yeni bir VM oluşturursunuz. Bu VM’yi Traffic Manager’ın trafiği daha yüksek ağırlık değerine sahip olan web sitesi uç noktasına nasıl yönlendirdiğini test etmek için kullanacaksınız.

1. Azure portalın sol üst köşesinde **Kaynak oluştur** > **İşlem** > **Windows Server 2016 VM**'yi seçin.
2. **Temel Bilgiler** için aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVMEastUS|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Kaynak grubu| **Var olan**’ı seçin ve sonra *myResourceGroupTM1* öğesini seçin.|
    |||

4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| **Sanal ağ**'ı seçin ve **Sanal ağ oluştur** bölümündeki **Ad** alanına *myVNet3*, alt ağ alanına da *mySubnet* yazın.|
    |Ağ Güvenliği Grubu|**Temel**'i seçin ve **Ortak gelen bağlantı noktası seçin** açılan menüsünde **HTTP** ve **RDP**'yi seçin. |
    |Önyükleme tanılamaları|**Devre dışı** girişini seçin.|
    |||

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.
8. Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulana kadar geri kalan adımlara devam etmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
**Ağırlıklı** yönlendirme yöntemini temel alan bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. **Traffic Manager profili oluştur** ekranında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**'u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Adı                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | **Ağırlıklı** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | **Mevcut**’u seçin ve *myResourceGroupTM1* öğesini seçin. |
    |        |   |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

Kullanıcı trafiğini bunlara yönlendirmek için *myIISVMEastUS*  & *myIISVMWEurope* IIS sunucularını çalıştıran iki VM’yi ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Adı           | myEastUSEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için **Genel IP adresi seçin**. **Kaynak** bölümünde *myIISVMEastUS-ip* adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Ağırlık      | **100** değerini girin.        |
    |        |           |

4. 2 ve 3 numaralı adımları tekrarlayarak *myIISVMWEurope* adlı IIS sunucusu VM'si ile ilişkilendirilmiş olan genel IP adresi *myIISVMWEurope-ip* için *myWestEuropeEndpoint* adlı yeni bir uç nokta ekleyin ve **Ağırlık** için **25** değerini girin. 
5.  Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme
Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:
1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görün.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme
Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır. 

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1.  Portalın arama çubuğunda önceki bölümde oluşturduğunuz **Traffic Manager profili** adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
1. **Genel Bakış**'a tıklayın.
2. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz. 

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin. 
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin. 
6. Web sitesini görüntülemek için *myVMEastUS* adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını yazın. Daha düşük olan **25** uç nokta ağırlık değeri atanan *myIISVMWEurope* ISS sunucusuyla karşılaştırıldığında, *myIISVMEastUS* IIS sunucusuna daha yüksek olan **100** ağırlığı atandığından bu ISS sunucusu üzerinde barındırılan web sitesine yönlendirilirsiniz.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme
Artık ihtiyacınız olmadığında, bu öğreticide oluşturduğunuz kaynak gruplarını silin. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcının coğrafi konumuna göre trafiği belirli uç noktalara yönlendirme](traffic-manager-configure-geographic-routing-method.md)


