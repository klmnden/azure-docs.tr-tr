---
title: Azure Traffic Manager'ı kullanarak alt ağ trafiği yönlendirme yöntemini yapılandırma
description: Bu makalede Traffic Manager'ı kullanıcı alt ağlarından gelen trafiği belirli uç noktalara yönlendirecek şekilde yapılandırma adımları anlatılmaktadır.
services: traffic-manager
documentationcenter: ''
author: asudbring
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: allensu
ms.openlocfilehash: da2d4816f3f7a99ac2d213d72d7e801cf630e165
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304944"
---
# <a name="direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Traffic Manager'ı kullanarak trafiği kullanıcı alt ağına göre belirli uç noktalara yönlendirme

Bu makalede alt ağ trafiği yönlendirme yöntemini yapılandırma adımları anlatılmaktadır. **Alt ağ** trafik yönlendirme yöntemi, bir IP adresi kümesini belirli uç noktalarla eşlemenizi sağlar. Traffic Manager'a bir istek geldiğinde kaynak IP adresi incelenir ve bu adresle ilişkilendirilmiş uç nokta döndürülür.

Bu öğreticide alt ağ yönlendirme ile kullanıcı sorgusunun IP adresine bağlı olarak trafik iç web sitesine veya üretim web sitesine yönlendirilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IIS üzerinde basit bir web sitesi çalıştıran iki VM oluşturma
> * Traffic Manager'ı uygulamalı olarak görmek için iki test amaçlı VM oluşturma
> * IIS çalıştıran VM'lerin DNS adını yapılandırma
> * Trafiği kullanıcının alt ağına göre yönlendirmek için bir Traffic Manager profili oluşturma
> * Traffic Manager profiline VM uç noktaları ekleme
> * Traffic Manager'ın nasıl çalıştığını görün

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide Traffic Manager'ın çalışmasını uygulamalı olarak görmek için şu sistemleri dağıtmanız gerekir:

- farklı Azure bölgelerinde çalışan iki basit web sitesi: **Doğu ABD** (iç web sitesi olarak görev yapar) ve **Batı Avrupa** (üretim web sitesi olarak görev yapar).
- Traffic Manager'ı test etmek için iki test amaçlı VM: bir VM **Doğu ABD** bölgesinde, ikinci VM ise **Batı Avrupa** bölgesinde olmalıdır.

Test amaçlı VM'ler Traffic Manager'ın kullanıcı sorgusunun geldiği alt ağa göre kullanıcı trafiğini iç web sitesine veya üretim web sitesine nasıl yönlendirdiğini göstermek için kullanılır.

### <a name="sign-in-to-azure"></a>Oturum açın: Azure

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

### <a name="create-websites"></a>Web sitelerini oluşturma

Bu bölümde Traffic Manager profili için iki farklı Azure bölgesinde iki hizmet uç noktası sunan iki web sitesi örneği oluşturacaksınız. İki web sitesi oluşturmak için aşağıdaki adımları izleyin:

1. Biri **Doğu ABD** diğeri **Batı Avrupa** bölgesinde olmak üzere basit bir web sitesi çalıştıran iki VM oluşturun.
2. İki VM'de de IIS sunucusu yükleyin ve varsayılan web sitesi sayfasını web sitesini ziyaret eden kullanıcıların VM adını göreceği şekilde güncelleştirin.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma

Bu bölümde, iki VM oluşturma *myIISVMEastUS* ve *myIISVMWestEurope* içinde **Doğu ABD** ve **Batı Avrupa** Azure bölgeleri.

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

#### <a name="install-iis-and-customize-the-default-web-page"></a>IIS yükleme ve varsayılan web sayfasını özelleştirme

Bu bölümde, iki Vm'lerde - IIS sunucusunu yükleme *myIISVMEastUS* & *myIISVMWestEurope*ve sonra varsayılan Web sitesi sayfası güncelleştirin. Özelleştirilmiş web sitesi sayfası, web sitesini bir web tarayıcısından ziyaret ettiğinizde bağlandığınız VM'nin adını gösterecek.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Sunucu masaüstünde **Windows Yönetimsel Araçları**>**Sunucu Yöneticisi** bölümüne gidin.
7. VM başlatma Windows PowerShell *myIISVMEastUS*ve aşağıdaki komutları kullanarak IIS sunucusu yüklemek ve varsayılan htm dosyasını güncelleştirin.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

8. RDP bağlantısıyla kapatmak *myIISVMEastUS* VM.
9. VM ile RDP bağlantısı oluşturarak 1-6 adımlarını yineleyin *myIISVMWestEurope* içinde *myResourceGroupTM2* IIS yüklemek ve varsayılan web sayfasını özelleştirmek için kaynak grubu.
10. Başlatma Windows PowerShell'i temel *myIISVMWestEurope* VM ve aşağıdakileri kullanarak komutları IIS sunucusu yüklemek ve varsayılan htm dosyasını güncelleştirin.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>IIS çalıştıran VM'lerin DNS adlarını yapılandırma

Traffic Manager, kullanıcı trafiğini hizmet uç noktalarının DNS adına göre yönlendirir. Bu bölümde, IIS sunucuları - DNS adlarını yapılandırma *myIISVMEastUS* ve *myIISVMWestEurope*.

1. Sol menüden **Tüm kaynaklar**’a tıklayın ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myIISVMEastUS* öğesini seçin.
2. **Genel bakış** sayfasının **DNS adı** bölümünde **Yapılandır**'ı seçin.
3. **Yapılandır** sayfasının DNS adı bölümünde benzersiz bir ad ekleyip **Kaydet**'i seçin.
4. 1-3 ' adlı VM için adımlarını *myIISVMWestEurope* bulunan *myResourceGroupTM2* kaynak grubu.

### <a name="create-test-vms"></a>Test amaçlı VM'leri oluşturma

Bu bölümde, bir VM oluşturun (*myVMEastUS* ve *myVMWestEurope*) her bir Azure bölgesinde (**Doğu ABD** ve **Batı Avrupa**). Bu VM'ler, kullanıcı trafiğini kullanıcının sorgu alt ağda Traffic Manager tarafından nasıl yönlendirdiği test etmek için kullanır.

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

İsteğin kaynak IP adresine göre belirli uç noktalar döndürmenizi sağlayacak bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafından **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur**'u seçin.
2. **Traffic Manager profili oluştur** ekranında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**'u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ad                   | Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı ile sonuçlanması gerekir.                                   |
    | Yönlendirme yöntemi          | **Alt ağ** yönlendirme yöntemini seçin.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | **Var olan**’ı seçin ve sonra *myResourceGroupTM1* yazın. |
    | |                              |
    |

    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-subnet-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme

IIS çalıştıran iki sanal makine ekleme sunucuları - *myIISVMEastUS* & *myIISVMWestEurope* kullanıcının sorgu alt ağa dayalı kullanıcı trafiği yönlendirmek için.

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve görüntülenen sonuçların arasından bu profili seçin.
2. **Traffic Manager profili** sayfasının **Ayarlar** bölümünde **Uç noktalar**'a ve ardından **Ekle**'ye tıklayın.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Type                    | Azure uç noktası                                   |
    | Ad           | myInternalWebSiteEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | Genel IP adresine sahip kaynakların aynı abonelik altında listelenmesi için **Genel IP adresi seçin**. **Kaynak** bölümünde *myIISVMEastUS-ip* adlı genel IP adresini seçin. Bu, Doğu ABD bölgesindeki IIS sunucusu VM'sinin IP adresidir.|
    |  Alt ağ yönlendirme ayarları    |   IP adresini ekleyin *myVMEastUS* VM test edin. Bu sanal makineden kaynaklanan herhangi bir kullanıcı sorgu yönlendirilirsiniz *myInternalWebSiteEndpoint*.    |

4. 2 ve 3 adlı başka bir uç nokta ekleme *myProdWebsiteEndpoint* genel IP adresi için *myIISVMWestEurope IP* IIS sunucusu adlı VM ile ilişkili  *myIISVMWestEurope*. İçin **alt ağ yönlendirme ayarları**, test sanal makinesi - IP adresini ekleyin *myVMWestEurope*. Bu test amaçlı VM'den gelen kullanıcı sorguları *myProdWebsiteEndpoint* adlı uç noktaya yönlendirilir.
5. Her iki uç noktanın eklenmesi tamamlandığında, **Çevrimiçi** izleme durumuyla birlikte **Traffic Manager profili** bölümünde gösterilir.

## <a name="test-traffic-manager-profile"></a>Traffic Manager profilini test etme

Bu bölümde Traffic Manager'ın belirli bir alt ağdan gelen trafiği belirli bir uç noktaya nasıl yönlendirdiğini test edeceksiniz. Traffic Manager'ı uygulamalı olarak görmek için şu adımları gerçekleştirin:

1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager'ın nasıl çalıştığını görmek için şu adımları izleyin:
    - VM testinden (*myVMEastUS*) bulunan **Doğu ABD** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.
    - VM testinden (*myVMWestEurope*) bulunan **Batı Avrupa** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profilinin DNS adını belirleme

Bu öğreticide kolaylık olması açısından web sitelerini ziyaret etmek için Traffic Manager profilinin DNS adı kullanılmaktadır.

Traffic Manager profilinizin DNS adını belirlemek için şu adımları izleyin:

1. Portalın arama çubuğunda önceki bölümde oluşturduğunuz **Traffic Manager profili** adını arayın. Görüntülenen sonuçların arasından Traffic Manager profilini seçin.
2. **Genel Bakış**'a tıklayın.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Üretim dağıtımlarında bir gösterim etki alanı adını DNS CNAME kaydı kullanarak Traffic Manager etki alanı adına yönlendirirsiniz.

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün

Bu bölümde Traffic Manager'ın nasıl çalıştığını görebilirsiniz.

1. Sol menüden **Tüm kaynaklar**’ı seçin ve kaynak listesinden, *myResourceGroupTM1* kaynak grubunda bulunan *myVMEastUS* öğesine tıklayın.
2. **Genel Bakış** sayfasında **Bağlan**'a tıklayın ve **Sanal makineye bağlanma** bölümünde **RDP dosyasını indir**'i seçin.
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir.
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.
6. Web sitesini görüntülemek için *myVMEastUS* adlı VM'de bir web tarayıcısında Traffic Manager profilinizin DNS adını yazın. VM beri *myVMEastUS* uç noktası ile ilişkili IP adresi *myInternalWebsiteEndpoint*, Test Web sunucusu - web tarayıcısını başlatır *myIISVMEastUS*.

7. Ardından, VM'ye bağlanmak *myVMWestEurope* bulunan **Batı Avrupa** adımları kullanarak 1-5 ve göz atma için Traffic Manager profili etki alanı adı bu VM'den. VM beri *myVMWestEurope* uç noktası ile ilişkili IP adresi *myProductionWebsiteEndpoint*, Test Web sunucusu - web tarayıcısını başlatır *myIISVMWestEurope*.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini silme

İhtiyacınız kalmadığında kaynak gruplarını (**ResourceGroupTM1** ve **ResourceGroupTM2**) silebilirsiniz. Bunun için kaynak grubunu (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağırlıklı trafik yönlendirme yöntemi](traffic-manager-configure-weighted-routing-method.md) hakkında bilgi edinin.
- [Öncelik yönlendirme yöntemi](traffic-manager-configure-priority-routing-method.md) hakkında bilgi edinin.
- [Coğrafi yönlendirme yöntemi](traffic-manager-configure-geographic-routing-method.md) hakkında bilgi edinin.
