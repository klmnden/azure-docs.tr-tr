---
title: Azure sanal WAN Otomasyonu - sanal WAN iş ortakları için yapılandırma | Microsoft Docs
description: Bu makale, yazılım tanımlı bağlantı çözüm iş ortakları Azure sanal WAN Otomasyon'u ayarlama yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: fc978c6ad9776271c790796f26912c63f9edcf74
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009388"
---
# <a name="configure-virtual-wan-automation---for-virtual-wan-partners-preview"></a>Sanal WAN Otomasyonu - sanal WAN iş ortakları (Önizleme) için yapılandırma

Bu makalede bağlanın ve bir dal cihaz (müşterinin şirket içi VPN cihazı veya SDWAN) Azure sanal WAN için Otomasyon envorionment kurma anlamanıza yardımcı olur. VPN bağlantısı IPSec/Ikev2 uyum dal cihazları sağlayan bir sağlayıcısıysanız, bu makale sizin içindir.

Yazılım tanımlı bir bağlantı çözümleri genellikle bir denetleyici veya bir cihaz Merkezi sağlama dal cihazlarını yönetmek için kullanın. Denetleyici, Azure API'leri, Azure sanal WAN bağlantısı otomatikleştirmek için kullanabilirsiniz. Bu tür bir bağlantı bir SDWAN veya VPN bulunan cihaz şirket içinde olan kendisine atanmış dışarıya yönelik genel IP adresi gerektirir.

##  <a name="access"></a>Erişim denetimi

Müşteriler, uygun erişim denetimini cihazda kullanıcı Arabirimi için sanal WAN Ayarla mümkün olması gerekir. Bu işlem, bir Azure hizmet sorumlusu kullanılması önerilir. Hizmet sorumlusu tabanlı erişim dal bilgilerini karşıya yüklemek için cihaz denetleyicisi uygun kimlik doğrulaması sağlar.

##  <a name="site"></a>Dal bilgilerini karşıya yüklemek

Dal (şirket içi site) bilgilerini yüklemek için kullanıcı deneyimi tasarlayın. [REST API'leri](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) için **VPNSite** site bilgilerini sanal WAN içinde oluşturmak için kullanılabilir. Tüm dal SDWAN/VPN cihazları sağlayın ya da cihaz özelleştirmeleri uygun olanını seçin.

##  <a name="hub"></a>Hub ve Hizmetleri

Dal cihazı Azure'a karşıya yüklendikten sonra müşteri Azure portalında bir dizi hub sanal ağ ve hub içinde VPN uç noktası oluşturma işlemlerini çağıran seçimler hub bölgesi ve/veya hizmetleri genellikle yapılır. VPN ağ geçidi gereksinimleri bant genişliği ve bağlantı uygun şekilde hangi boyutları göre ölçeklenebilir bir geçididir.

## <a name="device"></a>Cihaz yapılandırması

Bu adımda, bir sağlayıcı kullanmayan bir müşteri el ile Azure yapılandırmayı indirmek ve bunu şirket içi SDWAN/VPN cihazını uygulayabilirsiniz. Bir sağlayıcı, bu adımı otomatikleştirin. Denetleyici çağırabilirsiniz **GetVpnConfiguration** genellikle şu dosyaya benzer görünecektir Azure yapılandırmayı indirmek için REST API.

**Yapılandırma notları**

  * Azure sanal ağları sanal hub'ına takılı ise ConnectedSubnets görünürler.
  * VPN bağlantısı yapılandırma rota tabanlı ve Ikev2 kullanır.

### <a name="understanding-the-device-configuration-file"></a>Cihaz yapılandırma dosyasını anlama

Cihaz yapılandırma dosyası, şirket içi VPN Cihazınızı yapılandırırken kullanılacak ayarları içerir. Bu dosya görüntülediğinizde, aşağıdaki bilgileri dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'bağlanan bir site olarak ayarlanmış cihaz ayrıntılarını gösterir. Bu dal cihazın genel IP adresi ve adını içerir.
* **vpnSiteConnections -** Bu bölüm aşağıdakiler hakkında bilgi sağlar:

    * **Adres alanı** sanal hub(ları) VNet biri.<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adres alanı** hub'a bağlı sanal ağlar.<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **IP adresleri** sanal hub vpngateway biri. Vpngateway 2 Tünel etkin-etkin yapılandırmada her bağlantı kapsayan olduğundan, bu dosyada listelenen iki IP adresini görürsünüz. Bu örnekte, her site için "Instance0" ve "Örnek1" bakın.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Vpngateway bağlantısı yapılandırma ayrıntılarını** BGP gibi anahtar vb. önceden paylaşılan. PSK sizin için otomatik olarak oluşturulan önceden paylaşılan bir anahtardır. Genel bakış sayfasında bağlantı özel PSK için her zaman düzenleyebilirsiniz.
  
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

## <a name="default"></a>Varsayılan ilke

### <a name="initiator"></a>Başlatıcı

Azure Başlatıcı tünel için olduğunda, aşağıdaki bölümlerde desteklenen ilke birleşimleri listeleyin.

**1. Aşama**

* AES_256 VE, SHA1, DH_GROUP_2
* AES_256 VE, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Aşama 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256 VE, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256 VE, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE


### <a name="responder"></a>Yanıtlayıcı

Azure için tünel Yanıtlayıcı olduğunda aşağıdaki bölümlerde desteklenen ilke birleşimleri listelenmektedir.

**1. Aşama**

* AES_256 VE, SHA1, DH_GROUP_2
* AES_256 VE, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* 3DES, SHA1, DH_GROUP_2
* 3DES, SHA_256, DH_GROUP_2

**Aşama 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256 VE, SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256 VE, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE
* CBC_DES, SHA_1, PFS_NONE 
* AES_256 VE, SHA_1, PFS_1
* AES_256 VE, SHA_1, PFS_2
* AES_256 VE, SHA_1, PFS_14
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* CBC_3DES, SHA_1, PFS_1
* CBC_3DES, SHA_1, PFS_2
* CBC_3DES, SHA_256, PFS_2
* AES_256 VE, SHA_256, PFS_1
* AES_256 VE, SHA_256, PFS_2
* AES_256 VE, SHA_256, PFS_14
* AES_256 VE, SHA_1, PFS_24
* AES_256 VE, SHA_256, PFS_24
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14
* CBC_3DES, SHA_1, PFS_14

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>Her şeyi ve benim şirket içi SDWAN/VPN cihazına veya yapılandırma SD-WAN sanal hub'ı vpngateway İlkesi arasında eşleşmesi gerekiyor mu?

Şirket içi SDWAN/VPN cihazınız veya cihaz yapılandırma SD-WAN ile aynı veya aşağıdaki algoritmaları ve Azure IPSec/IKE ilkesinde belirttiğiniz parametrelerini içerir. SA yaşam süreleri yalnızca yerel belirtimlerdir olan ve eşleşmesi gerekmez.

* IKE şifreleme algoritması
* IKE bütünlük algoritması
* DH Grubu
* IPsec şifreleme algoritması
* IPsec bütünlük algoritması
* PFS Grubu

## <a name="feedback"></a>Önizleme geri bildirim

Görüşleriniz bizim için önemlidir. Lütfen bir e-posta gönderin <azurevirtualwan@microsoft.com> sorunları bildirmek veya geri bildirim (pozitif veya negatif) için sanal WAN sağlamak için. "[]" Şirket adınızı konu satırında içerir. Bir sorun bildiriyorsanız de abonelik Kimliğinizi içerir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz: [hakkında Azure sanal WAN](virtual-wan-about.md) ve [Azure sanal WAN SSS](virtual-wan-faq.md).