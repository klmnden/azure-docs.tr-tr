---
title: Azure Sanal WAN kullanarak Azure'a siteden siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a siteden siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: ea36a3d4a2471cee6a18d70275aaf2e83ffc6f39
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159660"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan-preview"></a>Öğretici: Azure Sanal WAN (Önizleme) kullanarak siteden siteye bağlantı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md)

> [!NOTE]
> Birden fazla siteniz varsa bu yapılandırmayı oluşturmak için [Sanal WAN iş ortağı](https://aka.ms/virtualwan) kullanmanız gerekir. Ancak ağ bağlantıları ve kendi VPN cihazınızı yapılandırma konularında deneyimliyseniz bu yapılandırmayı kendiniz oluşturabilirsiniz.
>

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Site oluşturma
> * Hub oluşturma
> * Bir hub'ı bir siteye bağlama
> * Bir sanal ağı bir hub'a bağlama
> * VPN cihazı yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

> [!IMPORTANT]
> Azure Sanal WAN şu anda yönetilen genel önizleme sürümündedir. Sanal WAN'yi kullanmak için [Önizlemeye kaydolmanız](#enroll) gerekir.
>
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* IKEv2 kullanabilen uyumlu bir rota temelli VPN cihazına sahip olduğunuzdan ve bu cihazı yapılandırabilecek birinin bulunduğundan emin olun. Sanal WAN iş ortağıyla çalışıyorsanız yapılandırma ayarları otomatik olarak oluşturulur ve cihazı el ile yapılandırma konusunda endişelenmenize gerek yoktur.
* VPN cihazınız için dışarıya dönük genel bir IPv4 adresi olduğunu doğrulayın. Bu IP adresi bir NAT’nin arkasında olamaz.
* Bağlanmak istediğiniz bir sanal ağınız varsa şirket içi ağınızdaki alt ağların bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Sanal ağınız için ağ geçidi alt ağına ihtiyaç yoktur ve sanal ağınızda sanal ağ geçidi bulunamaz. Sanal ağınız yoksa bu makaledeki adımları kullanarak oluşturabilirsiniz.
* Hub bölgenizden bir IP adresi aralığı edinin. Hub bir sanal ağdır ve hub bölgesi için belirttiğiniz adres aralığı, bağlandığınız var olan sanal ağlarla çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="enroll"></a>1. Önizlemeye kaydolma

Sanal WAN yapılandırabilmeniz için önce aboneliğinizi Önizleme'ye kaydetmeniz gerekir. Aksi halde portalda Sanal WAN ile çalışamazsınız. Kaydolmak için **azurevirtualwan@microsoft.com** adresine abonelik kimliğinizin bulunduğu bir e-posta gönderin. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız.

## <a name="vnet"></a>2. Sanal ağ oluşturma

Sanal ağınız yoksa PowerShell kullanarak hızlıca bir tane oluşturabilirsiniz. Azure portalı kullanarak da sanal ağ oluşturabilirsiniz.

* Oluşturduğunuz sanal ağın adres alanının bağlanmak istediğiniz diğer sanal ağların adres aralıklarıyla veya şirket içi ağ adres alanlarıyla çakışmadığından emin olun. 
* Sanal ağınız varsa gerekli ölçütleri karşıladığından ve sanal ağ geçidi bulunmadığından emin olun.

Bu makaledeki "Deneyin" düğmesine tıklayıp PowerShell konsolu açarak sanal ağınızı kolayca oluşturabilirsiniz. Değerleri ayarlayın ve ardından komutları kopyalayıp konsol penceresine yapıştırabilirsiniz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PowerShell komutlarını ayarlayın ve ardından bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Sanal ağ oluşturma

PowerShell komutlarını ayarlayarak ortamınızla uyumlu bir sanal ağ oluşturun.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

## <a name="wan"></a>3. Sanal WAN oluşturma

1. Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.
2. Bu noktada Sanal WAN'ı bulmak için **Tüm hizmetler**'e gidip Sanal WAN aratabilirsiniz. Alternatif olarak Azure portalın en üstündeki arama kutusuna Sanal WAN yazıp aratabilirsiniz. Sayfayı açmak için **Sanal WAN**'a tıklayın.
3. **Oluştur**'a tıklayarak **WAN Oluştur** sayfasını açın. Sayfanın kullanılabilir durumda olmaması bu Önizleme için henüz onaylanmadığınız anlamına gelir.

  ![WAN oluşturma](./media/virtual-wan-site-to-site-portal/createwan.png)
4. WAN Oluştur sayfasında aşağıdaki alanları doldurun.

  * **Ad**: WAN'ınıza vermek istediğiniz adı girin.
  * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
  * **Kaynak Grubu**: Yeni oluşturun veya var olanı kullanın.
  * **Kaynak Konumu**: Açılan menüden kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
5. WAN'ı oluşturmak için **Oluştur**’a tıklayın.

## <a name="site"></a>4. Site oluşturma

Fiziksel konumlarınıza karşılık gelecek sayıda site oluşturabilirsiniz. Örneğin İstanbul'da, Ankara'da ve İzmir'de birer şubeniz varsa üç ayrı site oluşturmanız gerekir. Bu siteler şirket içi VPN cihazı uç noktalarını içerir. Bu noktada siteniz için yalnızca bir özel adres alanı belirtebilirsiniz.

1. **Tüm kaynaklar**'a gidin.
2. Oluşturduğunuz sanal WAN'a tıklayın.
3. Sayfanın en üstündeki **+Site oluştur**'a tıklayarak **Site oluştur** sayfasını açın.

  ![yeni site](media/virtual-wan-site-to-site-portal/createsite.png)
4. **Site oluştur** sayfasında aşağıdaki alanları doldurun:

  *  **Ad**: Şirket içi sitenize vermek istediğiniz addır.
  *  **Genel IP adresi**: Şirket içi sitenizde yer alan VPN cihazının genel IP adresidir.
  *  **Özel adres alanı**: Şirket içi sitenizde yer alan IP adres alanıdır. Bu adres alanını hedefleyen trafik yerel sitenize yönlendirilir.
  *  **Abonelik**: Aboneliği doğrulayın.
  *  **Kaynak Grubu**: Kullanmak istediğiniz kaynak grubudur.
5. Ek ayarları görüntülemek için **Gelişmiş içeriği göster**'e tıklayın. **BGP'yi etkinleştir** (isteğe bağlı alan) seçeneğini belirlediğinizde bu işlev Azure'da bu site için oluşturulan tüm bağlantılarda etkinleştirilir. İsterseniz **Cihaz bilgileri** (isteğe bağlı alan) alanını da doldurabilirsiniz. Bu alan Azure Ekibinin ortamınızı daha iyi anlamasına ve gelecekte ek iyileştirme olanakları eklemesine veya sorun giderme aşamasında size destek olmasına yardımcı olabilir.

  ![BGP](media/virtual-wan-site-to-site-portal/sitebgp.png)
6. Siteyi oluşturmak için **Onayla**'ya tıklayın.
7. Bu adımları oluşturmak istediğiniz tüm siteler için tekrarlayın.

## <a name="hub"></a>5. Bir hub oluşturma ve siteleri bağlama

1. Sanal WAN'ınızın sayfasında **Siteler**'e tıklayın.
2. **İlişkilendirilmemiş siteler** bölümünde henüz bir hub'a bağlanmamış olan sitelerin listesini görürsünüz.
3. İlişkilendirmek istediğiniz siteyi seçin.
4. Açılan menüden hub'ınızın ilişkilendirileceği bölgeyi seçin. Hub'ınızı bağlanmak istediğiniz sanal ağların bulunduğu bölgeyle ilişkilendirmeniz gerekir.
5. **Onayla**'ya tıklayın. Bu bölgede henüz bir hub'ınız yoksa otomatik olarak bir sanal hub sanal ağı oluşturulur. Bu durumda **Bölgesel hub'lar oluşturun** sayfası açılır.
6. **Bölgesel hub'lar oluşturun** sayfasında hub sanal ağınızın adres aralığını girin. Bu sanal ağ, hub hizmetlerinizi içerecek olan sanal ağdır. Buraya girdiğiniz aralığın özel IP adresi aralığı olması ve şirket içi adres alanlarınızla veya sanal ağ adres alanlarınızla çakışmaması gerekir. Bu işlemin sonunda hub sanal ağında bir VPN uç noktası oluşturulacaktır. (Otomatik hub ve ağ geçidi oluşturma işlemleri yalnızca portaldan gerçekleştirilebilir.)
7. **Oluştur**’a tıklayın.

## <a name="vnet"></a>6. Sanal ağınızı bir hub'a bağlama

Bu adımda hub'ınızla bir sanal ağ arasında eşleme bağlantısı oluşturacaksınız. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantısı**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.

## <a name="device"></a>7. VPN yapılandırmasını indirme

Şirket içi VPN cihazınızı yapılandırmak için VPN cihazı yapılandırmasını kullanın.

1. Sanal WAN'ınızın sayfasında **Genel bakış**'a tıklayın.
2. Genel bakış sayfasının en üstünde **VPN yapılandırmasını indir**'e tıklayın. Azure, "microsoft-network-[konum]" kaynak grubunda bir depolama hesabı oluşturur ve burada konum, WAN'ın konumudur. Yapılandırmayı VPN cihazlarınıza uyguladıktan sonra bu depolama hesabını silebilirsiniz.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. Yapılandırmayı VPN cihazınıza uygulayın.

### <a name="understanding-the-vpn-device-configuration-file"></a>VPN cihazı yapılandırma dosyasını anlama

Cihaz yapılandırma dosyasında şirket içi VPN cihazınızı yapılandırırken kullanacağınız ayarlar bulunur. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanmış cihazın ayrıntıları yer alır. Dal cihazının adını ve genel IP adresini içerir.
* **vpnSiteConnections -** Bu bölümde aşağıdakilerle ilgili bilgiler yer alır:

    * Sanal hub sanal ağının **adres alanı**<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * Hub'a bağlı sanal ağların **adres alanı**<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * vpngateway sanal hub'ının **IP adresleri**. vpngateway, etkin-etkin yapılandırmada 2 tünel içeren bağlantılara sahip olduğundan bu dosyada iki taraftaki IP adreslerinin de listelendiğini göreceksiniz. Bu örnekte her site için "Instance0" ve "Instance1" örneklerini göreceksiniz.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar gibi **vpngateway bağlantısı yapılandırma ayrıntıları**. PSK, sizin için otomatik olarak oluşturulan önceden paylaşılan anahtardır. Dilediğiniz zaman genel bakış sayfasındaki bağlantıyı düzenleyerek özel bir PSK ekleyebilirsiniz.
  
### <a name="example-device-configuration-file"></a>Örnek cihaz yapılandırma dosyası

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>VPN cihazınızı yapılandırma

>[!NOTE]
> Sanal WAN iş ortağı çözümüyle çalışıyorsanız VPN cihazı yapılandırması otomatik olarak cihaz denetleyici yapılandırma dosyasını Azure'dan aldığında ve bunu Azure bağlantısı kurmak için cihaza uyguladığında gerçekleştirilir. Bu da VPN cihazını el ile yapılandırmayı bilmenize gerek olmadığı anlamına gelir.
>

Cihazınızı yapılandırma yönergelerine ihtiyaç duyarsanız [VPN cihazı yapılandırma betikleri sayfasındaki](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) yönergeleri aşağıdaki uyarılarla birlikte kullanabilirsiniz:

* VPN cihazları sayfasındaki yönergeler Sanal WAN için yazılmamıştır ancak yapılandırma dosyasındaki Sanal WAN değerlerini kullanarak VPN cihazınızı el ile yapılandırabilirsiniz. 
* Farklı yapılandırmaya sahip olduğundan VPN Gateway'e özgü indirilebilir cihaz yapılandırma betikleri Sanal WAN ile çalışmaz.
* Sanal WAN yalnızca IKEv2'yi kullanabilir, IKEv1'i kullanamaz.
* Sanal WAN yalnızca rota temelli cihazları ve cihaz yönergelerini kullanabilir.

## <a name="viewwan"></a>8. Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder. Hub sistem durumu özetini görüntülemek için noktalardan birinin üzerine gidin.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="viewhealth"></a>9. Kaynak durumunu görüntüleme

1. WAN'ınıza gidin.
2. WAN sayfanızın **Destek ve sorun giderme** bölümünde **Sistem durumu**'na tıklayın ve kaynağınızı görüntüleyin.

## <a name="connectmon"></a>10. Bir bağlantıyı izleme

Bir Azure sanal makinesi ile uzak site arasındaki iletişimi izlemek için bir bağlantı oluşturun. Bağlantı izleyici oluşturma hakkında bilgi almak için bkz. [Ağ iletişimini izleme](~/articles/network-watcher/connection-monitor.md). Kaynak alan Azure'daki sanal makinenin IP adresi, hedef IP ise Sitenin IP adresidir.

## <a name="cleanup"></a>11. Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="feedback"></a>Önizleme geri bildirimi

Geri bildirimleriniz bizim için önemlidir. Sanal WAN ile ilgili sorunları bildirmek veya geri bildirim (olumlu veya olumsuz) sağlamak için lütfen <azurevirtualwan@microsoft.com> adresine e-posta gönderin. Şirketinizin adını konu satırına “[ ]” içinde yazın. Sorun bildiriyorsanız abonelik kimliğinizi de eklemeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Site oluşturma
> * Hub oluşturma
> * Bir hub'ı bir siteye bağlama
> * Bir sanal ağı bir hub'a bağlama
> * VPN cihazı yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme
> * Kaynak durumunu görüntüleme
> * Bir bağlantıyı izleme

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
