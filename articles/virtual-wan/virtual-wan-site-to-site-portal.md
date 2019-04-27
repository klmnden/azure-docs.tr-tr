---
title: Azure Sanal WAN kullanarak Azure'a siteden siteye bağlantı oluşturma | Microsoft Docs
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a siteden siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 02/26/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: 4b44eec5557d2083c38fe2714d93800f79b21b0f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60457934"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan"></a>Öğretici: Azure sanal WAN kullanarak siteden siteye bağlantı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv1 ve IKEv2) VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md)

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

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [Before you begin](../../includes/virtual-wan-tutorial-vwan-before-include.md)]

## <a name="vnet"></a>1. Sanal ağ oluşturma

[!INCLUDE [Create a virtual network](../../includes/virtual-wan-tutorial-vnet-include.md)]

## <a name="openvwan"></a>2. Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://aka.ms/azurevirtualwanpreviewfeatures) gidin ve Azure hesabınızla oturum açın.

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-tutorial-vwan-include.md)]

## <a name="site"></a>3. Site oluşturma

Fiziksel konumlarınıza karşılık gelecek sayıda site oluşturabilirsiniz. Örneğin İstanbul'da, Ankara'da ve İzmir'de birer şubeniz varsa üç ayrı site oluşturmanız gerekir. Bu siteler şirket içi VPN cihazı uç noktalarını içerir. Bu noktada siteniz için yalnızca bir özel adres alanı belirtebilirsiniz.

1. Oluşturduğunuz WAN'a tıklayın. VPN siteleri sayfasını açmak için WAN sayfasında **WAN Mimarisi** altındaki **VPN siteleri** seçeneğine tıklayın.
2. **VPN siteleri** sayfasında **+Site oluştur**’a tıklayın.
3. **Site oluştur** sayfasında aşağıdaki alanları doldurun:

   * **Ad**: Şirket içi sitenize vermek istediğiniz addır.
   * **Genel IP adresi**: Şirket içi sitenizde yer alan VPN cihazının genel IP adresidir.
   * **Özel adres alanı**: Şirket içi sitenizde yer alan IP adres alanıdır. Bu adres alanını hedefleyen trafik yerel sitenize yönlendirilir.
   * **Abonelik**: Aboneliği doğrulayın.
   * **Kaynak Grubu**: Kullanmak istediğiniz kaynak grubudur.
   * **Konum**.
4. Ek ayarları görüntülemek için **Gelişmiş içeriği göster**'e tıklayın. Etkinleştirmek için **BGP**’yi seçtiğinizde bu işlev Azure'da bu site için oluşturulan tüm bağlantılarda etkinleştirilir. İsterseniz **Cihaz bilgileri** (isteğe bağlı alanlar) alanını da doldurabilirsiniz. Bu alan Azure Ekibinin ortamınızı daha iyi anlamasına ve gelecekte ek iyileştirme olanakları eklemesine veya sorun giderme aşamasında size destek olmasına yardımcı olabilir.
5. **Onayla**'ya tıklayın.
6. **Onayla**’ya tıkladıktan sonra durumu VPN siteleri sayfasında görüntüleyin. Site **Hazırlanıyor**’dan **Hazırlandı** durumuna geçecektir.

## <a name="hub"></a>4. Hub oluşturma

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-hub-include.md)]

## <a name="associate"></a>5. Siteleri hub ile ilişkilendirme

Hub'lar genellikle VNet’in bulunduğu bölge ile aynı bölgedeki siteler ile ilişkilendirilmelidir.

1. **VPN siteleri** sayfasında hub ile ilişkilendirmek istediğiniz site veya siteleri seçin, ardından **+Yeni hub ilişkilendirmesi**’ne tıklayın.
2. **Bir veya birden fazla hub’ı olan ilişkili siteler** sayfasında aşağı açılan listeden bir hub seçin. **+İlişkilendirme ekle**’ye tıklayarak bir siteyi ek hub’lar ile ilişkilendirebilirsiniz.
3. Ayrıca burada belirli bir **PSK** ekleyebilir veya varsayılanı kullanabilirsiniz.
4. **Onayla**'ya tıklayın.
5. **VPN siteleri** sayfasında bağlantı durumunu görüntüleyebilirsiniz.

## <a name="vnet"></a>6. Sanal ağınızı bir hub'a bağlama

Bu adımda hub'ınızla bir sanal ağ arasında eşleme bağlantısı oluşturacaksınız. Bu adımları bağlanmak istediğiniz tüm sanal ağlar için tekrarlayın.

1. Sanal WAN'ınızın sayfasında **Sanal ağ bağlantıları**'na tıklayın.
2. Sanal ağ bağlantısı sayfasında **+Bağlantı ekle**'ye tıklayın.
3. **Bağlantı ekle** sayfasında aşağıdaki alanları doldurun:

    * **Bağlantı adı**: Bağlantınıza bir ad verin.
    * **Hub'lar**: Bu bağlantıyla ilişkilendirmek istediğiniz hub'ı seçin.
    * **Abonelik**: Aboneliği doğrulayın.
    * **Sanal ağ**: Bu hub'a bağlamak istediğiniz sanal ağı seçin. Sanal ağda önceden var olan bir sanal ağ geçidi bulunamaz.
4. Eşleme bağlantısını oluşturmak için **Tamam**’a tıklayın.

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
> Bir Sanal WAN iş ortağı çözümü ile çalışıyorsanız VPN cihazı yapılandırması otomatik olarak gerçekleşir. Cihaz denetleyicisi, Azure'dan yapılandırma dosyasını alarak Azure bağlantısını kurmak için cihaza uygular. Bu da VPN cihazını el ile yapılandırmayı bilmenize gerek olmadığı anlamına gelir.
>

Cihazınızı yapılandırma yönergelerine ihtiyaç duyarsanız [VPN cihazı yapılandırma betikleri sayfasındaki](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) yönergeleri aşağıdaki uyarılarla birlikte kullanabilirsiniz:

* VPN cihazları sayfasındaki yönergeler Sanal WAN için yazılmamıştır ancak yapılandırma dosyasındaki Sanal WAN değerlerini kullanarak VPN cihazınızı el ile yapılandırabilirsiniz. 
* Farklı yapılandırmaya sahip olduğundan VPN Gateway'e özgü indirilebilir cihaz yapılandırma betikleri Sanal WAN ile çalışmaz.
* Yeni bir Sanal WAN hem IKEv1 hem de IKEv2’yi destekleyebilir.
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

Bu kaynaklara artık ihtiyacınız olmadığında, kullanabileceğiniz [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

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
