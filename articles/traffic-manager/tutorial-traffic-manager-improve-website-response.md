---
title: Öğretici - Azure Traffic Manager'ı kullanarak Web sitesi yanıt iyileştirmek için trafiği yönlendirme | Microsoft Docs
description: Bu öğretici makalesi, yüksek derecede yanıt veren bir Web sitesi oluşturmak için bir Traffic Manager profilinin nasıl oluşturulacağını açıklar.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: jeconnoc
editor: ''
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: kumud
ms.openlocfilehash: 89518d30b862e18fb7c989c95144ffa7f1c294fc
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40025179"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Öğretici: Traffic Manager'ı kullanarak Web sitesi yanıt geliştirin 

Bu öğreticide, Traffic Manager en düşük gecikme ile kullanıcı trafiğinin Web sitesine yönlendirerek yüksek derecede yanıt veren bir Web sitesi oluşturmak için kullanmayı açıklar. Genellikle, en düşük gecikme süresine sahip bir veri merkezinde coğrafi uzaklığı en yakın olan sertifikadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Temel bir Web sitesi IIS üzerinde çalışan iki VM oluşturma
> * İki test Traffic Manager uygulamada görüntülemek için VM oluşturma
> * IIS çalıştıran VM'ler için DNS adı yapılandırma
> * Geliştirilmiş Web sitesi performans için bir Traffic Manager profili oluşturun
> * VM uç noktaları Traffic Manager profiline Ekle
> * Görünüm Traffic Manager'ı iş başında

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Traffic Manager iş başında görmek için Bu öğretici aşağıdaki dağıttığınız gereklidir:
- temel Web siteleri çalıştıran iki örneğini farklı Azure bölgelerindeki - **Doğu ABD** ve **Batı Avrupa**.
- Traffic Manager - bir VM ile test etmek için iki VM test **Doğu ABD** ve ikinci bir sanal makine içinde **Batı Avrupa**. Test sanal makineleri, Traffic Manager kullanıcı trafiğinin gecikme süresi en düşük sağladığı gibi aynı bölgede çalıştırılan Web sitesine nasıl yönlendirdiğini göstermek için kullanılır.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

### <a name="create-websites"></a>Web siteleri oluşturma

Bu bölümde, Traffic Manager profiline iki Azure bölgesi için iki hizmet uç noktaları sağlayan iki Web sitesi örnekleri oluşturun. İki Web sitesi oluşturmak için aşağıdaki adımları içerir:
1. Temel Web sitesi - birinde çalıştırmak için iki VM oluşturma **Doğu ABD**ve diğer **Batı Avrupa**.
2. Her sanal makinede IIS sunucusu yüklemek ve bir kullanıcının Web sitesini ziyaret ederken bağlı olduğu VM adı açıklayan varsayılan Web sayfası.

#### <a name="create-vms-for-running-websites"></a>Web sitelerini çalıştırmak için VM oluşturma
Bu bölümde, iki VM oluşturma *myIISVMEastUS* ve *myIISVMWEurope* içinde **Doğu ABD** ve **Batı Avrupa** Azure bölgeleri.

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
    |Sanal ağ| Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet1*, alt ağ için girin * mySubnet*.|
    |Ağ Güvenliği Grubu|Seçin **temel**hem de **seçin ortak gelen bağlantı noktası** açılan listesinde, select **HTTP** ve **RDP** |
    |Önyükleme tanılamaları|Seçin **devre dışı bırakılmış**.|
    |||
6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. Aşağıdaki değişikliklerle birlikte 1.-6. adımları tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |Kaynak grubu | Seçin **yeni**, Anahtar'a tıklayın ve *myResourceGroupTM2*|
    |Konum|Batı Avrupa|
    |VM Adı | myIISVMWEurope|
    |Sanal ağ | Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet2*, alt ağ için girin * mySubnet*.|
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
7. VM1’de Windows PowerShell’i başlatın ve IIS sunucusunu yükleyip varsayılan htm dosyasını güncelleştirmek için aşağıdaki komutları kullanın.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![IIS'i çalıştırmanız ve web sayfasını özelleştirme](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. RDP bağlantısıyla kapatmak *myIISVMEastUS*.
9. VM ile RDP bağlantısı oluşturarak 1-8'le adımları yineleyin *myIISVMWEurope* içinde *myResourceGroupTM2* IIS yüklemek ve varsayılan web sayfasını özelleştirmek için kaynak grubu.

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
    |Kaynak grubu| Seçin **varolan** seçip *myResourceGroupTM1*.|
    |||

4. **Boyut seçin** bölümünden bir sanal makine boyutu seçin.
5. **Ayarlar** için aşağıdaki değerleri seçin ve **Tamam**’a tıklayın:
    |Ayar|Değer|
    |---|---|
    |Sanal ağ| Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet3*, alt ağ için girin * mySubnet*.|
    |Ağ Güvenliği Grubu|Seçin **temel**hem de **seçin ortak gelen bağlantı noktası** açılan listesinde, select **HTTP** ve **RDP** |
    |Önyükleme tanılamaları|Seçin **devre dışı bırakılmış**.|
    |||

6. **Özet**’in **Oluştur** bölümünde **Oluştur**’u seçerek sanal makine dağıtımını başlatın.

7. 1-5 arası adımları aşağıdaki değişikliklerle birlikte tekrar tamamlayın:

    |Ayar|Değer|
    |---|---|
    |VM Adı | *myVMWEurope*|
    |Kaynak grubu | Seçin **varolan**, Anahtar'a tıklayın ve *myResourceGroupTM2*|
    |Sanal ağ | Seçin **sanal ağ**, **sanal ağ oluştur**, için **adı**, girin *myVNet4*, alt ağ için girin * mySubnet*.|
    |||

8. Sanal makinelerin oluşturulması birkaç dakika sürebilir. Her iki sanal makine de oluşturulmadan kalan adımlara devam etmeyin.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma
En düşük gecikme süresine uç noktaya göndererek kullanıcı trafiği yönlendiren bir Traffic Manager profili oluşturun.

1. Ekranın sol üst tarafında seçin **kaynak Oluştur** > **ağ** > **Traffic Manager profili**  >  **Oluşturma**.
2. İçinde **Traffic Manager profili oluştur**girin veya seçin, aşağıdaki bilgileri, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **Oluştur**:
    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ad                   | Bu adın trafficmanager.net bölge ve Traffic Manager profilinize erişmek için kullanılan trafficmanager.net DNS adı sonuçlarında içinde benzersiz olması gerekir.                                   |
    | Yönlendirme yöntemi          | Seçin **öncelik** yönlendirme yöntemi.                                       |
    | Abonelik            | Aboneliğinizi seçin.                          |
    | Kaynak grubu          | Seçin **Yeni Oluştur** girin *myResourceGroupTM1*. |
    | Konum                | **Doğu ABD**’yi seçin.  Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.                              |
    |
  
    ![Traffic Manager profili oluşturma](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktaları ekleyin

IIS çalıştıran iki sanal makine ekleme sunucuları - *myIISVMEastUS*  & *myIISVMWEurope* kullanıcı trafiği yönlendirmek için kullanıcıya en yakın uç nokta.

1. Portalın arama çubuğunda, önceki bölümde oluşturduğunuz Traffic Manager profili adını arayın ve sonuçlarda profili seçin, görüntülenen.
2. İçinde **Traffic Manager profili**, **ayarları** bölümünde **uç noktaları**ve ardından **Ekle**.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Tür                    | Azure uç noktası                                   |
    | Ad           | myEastUSEndpoint                                        |
    | Hedef kaynak türü           | Genel IP Adresi                          |
    | Hedef kaynak          | **Bir genel IP adresi seçin** genel IP adresleri aynı abonelik altında kaynaklarla listesini göstermek için. İçinde **kaynak**, adlı ortak IP adresi seçin *myIISVMEastUS IP*. Bu VM Doğu ABD IIS sunucusunun genel IP adresidir.|
    |        |           |

4. 2 ve 3 adlı başka bir uç nokta ekleme *myWestEuropeEndpoint* genel IP adresi için *myIISVMWEurope IP* IIS sunucusu adlı VM ile ilişkili *myIISVMWEurope *.
5.  Her iki uç noktanın eklenmesi tamamlandığında, görüntülendikleri **Traffic Manager profili** izleme durumuyla birlikte **çevrimiçi**.

    ![Traffic Manager uç noktası ekleme](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)
  

## <a name="test-traffic-manager-profile"></a>Test Traffic Manager profili
Bu bölümde, Traffic Manager kullanıcı trafiğini en düşük gecikme süresi sağlamak için Web sitesi çalışan en yakın Vm'leri için nasıl yönlendirdiğini test edin. Traffic Manager uygulamada görüntülemek için aşağıdaki adımları tamamlayın:
1. Traffic Manager profilinizin DNS adını belirleyin.
2. Traffic Manager eylemi gibi görüntüleyin:
    - VM testinden (*myVMEastUS*) bulunan **Doğu ABD** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.
    - VM testinden (*myVMEastUS*) bulunan **Batı Avrupa** bölge, bir web tarayıcısında, Traffic Manager profilinizin DNS adına gidin.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Traffic Manager profili DNS adını belirleme
Bu öğreticide kolaylık olması için Traffic Manager profilini DNS adını Web sitelerini ziyaret etmek için kullanın. 

Traffic Manager profilini DNS adı şu şekilde belirleyebilirsiniz:

1.  Portalın arama çubuğunda arama **Traffic Manager profili** ve önceki bölümde oluşturduğunuz adı. Traffic manager profili görüntülenen sonuçlara tıklayın.
1. Tıklayın **genel bakış**.
2. **Traffic Manager profili** yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Üretim dağıtımında, bir DNS CNAME kaydı kullanılarak bir gösterim etki alanı adını Traffic Manager etki alanı adına işaret edecek şekilde yapılandırın.

   ![Traffic Manager DNS adı](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Görünüm Traffic Manager'ı iş başında
Bu bölümde, Traffic Manager eylemdir görebilirsiniz. 

1. Seçin **tüm kaynakları** tıklatın soldaki menüden ve ardından kaynak listesinden *myVMEastUS* bulunan *myResourceGroupTM1* kaynak grubu.
2. Üzerinde **genel bakış** sayfasında **Connect**ve ardından **sanal makineye bağlanma**seçin **indirme RDP dosyası**. 
3. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. 
4. **Tamam**’ı seçin.
5. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin. 
1. VM üzerinde bir web tarayıcısında *myVMEastUS*, Traffic Manager profilinizin, Web sitesini görüntülemek için DNS adını yazın. VM bulunan bu yana **Doğu ABD**, en yakın IIS sunucusunda barındırılan yakın Web sitesine yönlendirilir *myIISVMEastUS* bulunan **Doğu ABD**.

   ![Test Traffic Manager profili](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Ardından, VM'ye bağlanmak *myVMWestEurope* bulunan **Batı Avrupa** adımları kullanarak 1-5 ve göz atma için Traffic Manager profili etki alanı adı bu VM'den.  VM bulunan bu yana **Batı Avrupa**, artık IIS sunucusuna en yakın barındırılan Web sitesine yönlendirilir *myIISVMWEurope* bulunan **Batı Avrupa**. 

   ![Test Traffic Manager profili](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager profilini Sil
Artık gerekli olmadığında kaynak gruplarını silin (**ResourceGroupTM1** ve **ResourceGroupTM2**). Bunu yapmak için kaynak grubunu seçin (**ResourceGroupTM1** veya **ResourceGroupTM2**) ve ardından **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Trafiği bir uç nokta kümesine dağıtma](traffic-manager-configure-weighted-routing-method.md)


