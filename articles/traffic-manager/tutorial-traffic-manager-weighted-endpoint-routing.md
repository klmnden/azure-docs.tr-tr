---
title: Öğretici - Ağırlıklı Uç noktaları için trafiği yönlendirme - Azure Traffic Manager
description: Bu öğretici makalede, Traffic Manager'ı kullanarak trafiği ağırlıklı uç noktalara yönlendirme açıklanmaktadır.
services: traffic-manager
author: asudbring
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: allensu
ms.openlocfilehash: f9e2b6f6a45279c52e19a63509c57fb34e739330
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258381"
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

### <a name="sign-in-to-azure"></a>Oturum açın: Azure

[Azure Portal](https://portal.azure.com) oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesini oluşturmak için, aşağıdaki adımları tamamlayın:

1. Biri Doğu ABD diğeri Batı Avrupa bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'ye de IIS sunucusu yükleyin. Varsayılan web sayfasını, web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma

Bu bölümde, iki VM oluşturun (*myIISVMEastUS* ve *myIISVMWestEurope*) Doğu ABD ve Batı Avrupa Azure bölgelerinde.

1. Üst üzerinde köşe seçin Azure portalının sol **kaynak Oluştur** > **işlem** > **Windows Server 2019 Datacenter**.
2. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:

   - **Abonelik** > **kaynak grubu**: Seçin **Yeni Oluştur** yazın **myResourceGroupTM1**.
   - **Örnek ayrıntıları** > **sanal makine adı**: Tür *myIISVMEastUS*.
   - **Örnek ayrıntıları** > **bölge**:  **Doğu ABD**’yi seçin.
   - **Yönetici hesabı** > **kullanıcıadı**:  Seçtiğiniz bir kullanıcı adını girin.
   - **Yönetici hesabı** > **parola**:  Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Gelen bağlantı noktası kuralları** > **ortak gelen bağlantı noktası**: Seçin **Seçili bağlantı noktalarına izin**.
   - **Gelen bağlantı noktası kuralları** > **seçin gelen bağlantı noktalarının**: Seçin **RDP** ve **HTTP** çekme aşağı kutusu.

3. Seçin **Yönetim** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**, ardından **sonraki: Yönetim**. Altında **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**.
4. **İncele ve oluştur**’u seçin.
5. Ayarları gözden geçirin ve ardından **Oluştur**.  
6. Adlı ikinci bir VM oluşturmak için adımları *myIISVMWestEurope*, ile bir **kaynak grubu** adını *myResourceGroupTM2*, **konumu**, *Batı Avrupa*ve diğer ayarları aynı *myIISVMEastUS*.
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

![VM oluşturma](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-webpage"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iki VM myIISVMEastUS ve myIISVMWestEurope IIS sunucusu yükleyin ve sonra varsayılan Web sayfasını güncelleştirin. Özelleştirilmiş web sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

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
9. 1-8 arası adımları tekrarlayın. VM ile RDP bağlantısı oluşturma **myIISVMWestEurope** içinde **myResourceGroupTM2** kaynak grubu, IIS yükleyebilir ve kendi varsayılan Web sayfasını özelleştirme.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde, IIS sunucuları myIISVMEastUS ve myIISVMWestEurope DNS adlarını yapılandırın.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myIISVMEastUS** seçeneğini belirleyin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyin. Daha sonra **Kaydet**’e tıklayın.
4. Yineleme adlı VM için 1-3 adımları **myIISVMWestEurope** içinde **myResourceGroupTM2** kaynak grubu.

### <a name="create-a-test-vm"></a>Test VM’si oluşturma

Bu bölümde, bir VM oluşturun (*myVMEastUS* ve *myVMWestEurope*) her bir Azure bölgesinde (**Doğu ABD** ve **Batı Avrupa**). Traffic Manager trafiği yüksek ağırlık değerine sahip Web sitesi uç noktası nasıl yönlendirdiğini test etmek için bu sanal makineler kullanır.

1. Üst üzerinde köşe seçin Azure portalının sol **kaynak Oluştur** > **işlem** > **Windows Server 2019 Datacenter**.
2. İçinde **sanal makine oluşturma**yazın veya aşağıdaki değerleri seçin **Temelleri** sekmesinde:

   - **Abonelik** > **kaynak grubu**: Seçin **myResourceGroupTM1**.
   - **Örnek ayrıntıları** > **sanal makine adı**: Tür *myVMEastUS*.
   - **Örnek ayrıntıları** > **bölge**:  **Doğu ABD**’yi seçin.
   - **Yönetici hesabı** > **kullanıcıadı**:  Seçtiğiniz bir kullanıcı adını girin.
   - **Yönetici hesabı** > **parola**:  Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.
   - **Gelen bağlantı noktası kuralları** > **ortak gelen bağlantı noktası**: Seçin **Seçili bağlantı noktalarına izin**.
   - **Gelen bağlantı noktası kuralları** > **seçin gelen bağlantı noktalarının**: Seçin **RDP** çekme aşağı kutusu.

3. Seçin **Yönetim** sekmesinde veya seçin **sonraki: Diskleri**, ardından **sonraki: Ağ**, ardından **sonraki: Yönetim**. Altında **izleme**ayarlayın **önyükleme tanılaması** için **kapalı**.
4. **İncele ve oluştur**’u seçin.
5. Ayarları gözden geçirin ve ardından **Oluştur**.  
6. Adlı ikinci bir VM oluşturmak için adımları *myVMWestEurope*, ile bir **kaynak grubu** adını *myResourceGroupTM2*, **konumu** ' ın *Batı Avrupa*ve diğer ayarları aynı *myVMEastUS*.
7. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

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

IIS sunucuları myIISVMEastUS ve myIISVMWestEurope, kullanıcı trafiği yönlendirmek için bunları çalıştıran iki sanal makine ekleyin.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçlardan profili seçin.
2. **Traffic Manager profilinin** **Ayarlar** bölümünde **Uç noktalar** > **Ekle**'yi seçin.
3. Aşağıdaki bilgileri girin veya seçin. Diğer ayarlar için varsayılan değerleri kabul edin ve **Tamam**’ı seçin.

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Type                    | Azure uç noktasını girin.                                   |
    | Ad           | **myEastUSEndpoint** yazın.                                        |
    | Hedef kaynak türü           | **Genel IP adresi**'ni seçin.                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için genel IP adresi seçin. **Kaynak** bölümünde **myIISVMEastUS-ip** adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Ağırlık      | **100** değerini girin.        |
    |        |           |

4. 2 ve 3 adlı başka bir uç nokta ekleme **myWestEuropeEndpoint** genel IP adresi için **myIISVMWestEurope IP**. Bu IIS sunucusu myIISVMWestEurope adlı VM ile ilişkili adresidir. **Ağırlık** için **25** değerini girin.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte Traffic Manager profili bölümünde gösterilir.

## <a name="test-the-traffic-manager-profile"></a>Traffic Manager profilini test etme

Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görün.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme

Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
2. **Genel Bakış**’ı seçin.
3. Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden, **myResourceGroupTM1** kaynak grubundaki **myVMEastUS** seçeneğini belirleyin.
2. **Genel Bakış** sayfasında **Bağlan**'ı seçin. **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen .rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler** > **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Web sitesini görüntülemek için myVMEastUS adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını girin. Ağırlık değeri **100** ile daha yüksek olduğundan myIISVMEastUS üzerindeki IIS sunucusunda barındırılan web sitesine yönlendirilirsiniz. IIS server myIISVMWestEurope daha düşük bir uç nokta ağırlık değeri atanır **25**.

   ![Traffic Manager profilini test etme](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

7. VM myVMWestEurope ağırlıklı Web yanıtı görmek için 1-6 adımları tekrarlayın.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

Bu öğreticide oluşturduğunuz kaynak gruplarına ihtiyacınız kalmadığında onları silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kullanıcının coğrafi konumuna göre trafiği belirli uç noktalara yönlendirme](traffic-manager-configure-geographic-routing-method.md)
