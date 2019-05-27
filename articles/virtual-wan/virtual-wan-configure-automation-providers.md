---
title: Azure sanal WAN iş ortakları | Microsoft Docs
description: Bu makale, Azure sanal WAN Otomasyon'u ayarlama iş ortakları yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: c007684f351e0980ff9840ac8950121f212eeb36
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66016085"
---
# <a name="virtual-wan-partners"></a>Sanal WAN iş ortakları

Bu makalede bağlanın ve bir dal cihaz (müşterinin şirket içi VPN cihazı veya SDWAN CPE) Azure sanal WAN için Otomasyon ortamın nasıl anlamanıza yardımcı olur. VPN bağlantısı IPSec/Ikev2 veya IPSec/Ikev1 üzerinden uyum dal cihazları sağlayan bir sağlayıcısıysanız, bu makalede, olur.

Bir dal cihaz (müşterinin şirket içi VPN cihazına veya SDWAN CPE) denetleyicisi/cihaz Panosu sağlanacak genellikle kullanır. SD-WAN çözüm yöneticiler genellikle ağa takılı önce bir cihazı önceden sağlamak için bir Yönetim Konsolu kullanabilirsiniz. Bu VPN uyumlu bir cihaz bir denetleyiciden denetim düzlemi mantığını alır. VPN cihazına veya SD-WAN denetleyicisi Azure API'leri, Azure sanal WAN bağlantısı otomatikleştirmek için kullanabilirsiniz. Bu tür bir bağlantı, kendisine atanmış dışarıya yönelik genel IP adresi sağlamak için şirket içi cihaz gerektirir.

## <a name ="before"></a>Otomatikleştirme başlamadan önce

* Cihazınızı IPSec Ikev1/IKEv2'yi desteklediğini doğrulayın. Bkz: [varsayılan ilkeler](#default).
* Bkz: [REST API'leri](https://docs.microsoft.com/rest/api/azure/) Azure sanal WAN bağlantısı otomatikleştirmek için kullanır.
* Azure sanal WAN'ın portal deneyimi görmek için test edin.
* Ardından, hangi kısmını bağlantı adımları otomatikleştirmek istediğiniz karar verin. En azından otomatikleştirme öneririz:

  * Erişim Denetimi
  * Dal aygıt bilgileri Azure sanal WAN'ın karşıya yükleme
  * Azure yapılandırma indiriliyor ve Azure sanal WAN içine dal CİHAZDAN bağlantısı ayarlama

* Azure sanal WAN ile birlikte beklenen müşteri deneyimini anlayın.

  1. Genellikle, bir sanal WAN kullanıcı, bir sanal WAN kaynak oluşturarak işlemini başlatacak.
  2. Kullanıcı, şirket içi sistem (, dal denetleyicisi veya VPN cihaz sağlama yazılımı) dal bilgilerini Azure sanal WAN yazmak için bir hizmet sorumlusu tabanlı bir kaynak grup erişimi ayarlayacaksınız.
  3. Kullanıcı, kullanıcı Arabiriminde oturum açın ve hizmet sorumlusu kimlik bilgilerini ayarla için şu anda karar verebilirsiniz. Tamamlandıktan, denetleyiciniz dal bilgilerini sağlayacağınız Otomasyonu ile yükleyebildiğini olmalıdır. El ile bu Azure tarafında oluşturma'Site ' eşdeğerdir.
  4. Site (dal cihaz) bilgilerini Azure'da kullanılabilir hale geldikten sonra kullanıcıya hub sitesine ilişkilendireceksiniz. Bir sanal hub'ı Microsoft tarafından yönetilen bir sanal ağ ' dir. Hub'da, şirket içi ağınızdan (vpnsite) gelen bağlantıyı etkinleştirmek için çeşitli hizmet uç noktaları bulunur. Hub, bir bölgedeki ağınızın merkezidir. Ayrıca Azure bölgesi başına tek bir hub yalnızca olabilir ve bu işlem sırasında içindeki vpn bitiş noktası (vpngateway) oluşturulur. VPN ağ geçidi gereksinimleri bant genişliği ve bağlantı uygun şekilde hangi boyutları göre ölçeklenebilir bir geçididir. Sanal hub'ı ve dal cihaz denetleyicisi panonuzdan vpngateway oluşturmayı otomatikleştirmek tercih edebilirsiniz.
  5. Sanal Hub sitesine ilişkili olduğunda, el ile yüklemek kullanıcı için bir yapılandırma dosyası oluşturulur. Burada, Otomasyon gelir ve kullanıcı deneyimini sorunsuz hale getirir budur. El ile indirin ve dal cihaz yapılandırmak zorunda kalmadan kullanıcı yerine Otomasyon ayarlayabilir ve böylece paylaşılan anahtarı uyumsuzluğu, IPSec parametresi gibi tipik bağlantı sorunlarını ortadan kaldırılmasına kullanıcı Arabirimi, tıklama minimal deneyimi sağlayın Okunabilirlik vb. uyuşmazlığı, yapılandırma dosyası.
  6. Çözümünüzdeki bu adımın sonunda, kullanıcının sanal hub'ı ve dal cihaz arasında sorunsuz bir siteden siteye bağlantı gerekir. Diğer hub'lar arasında ek bağlantıları da ayarlayabilirsiniz. Her bir aktif-aktif tüneli bağlantıdır. Müşteri, her biri için tünel bağlantıları için farklı bir ISS kullanmayı tercih edebilirsiniz.

## <a name ="understand"></a>Otomasyon ayrıntıları kavramanız gerekir


###  <a name="access"></a>Erişim denetimi

Müşteriler, uygun erişim denetimini cihazda kullanıcı Arabirimi için sanal WAN Ayarla mümkün olması gerekir. Bu işlem, bir Azure hizmet sorumlusu kullanılması önerilir. Hizmet sorumlusu tabanlı erişim dal bilgilerini karşıya yüklemek için cihaz denetleyicisi uygun kimlik doğrulaması sağlar. Daha fazla bilgi için [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application). Bu işlevselliği Azure sanal WAN teklifi dışında olsa da, sonra ilgili ayrıntılarını cihaz Yönetimi panoya girilen azure'da erişimi ayarlamak için tipik adımlar aşağıda biz listesi

* Şirket içi cihaz denetleyicisi için bir Azure Active Directory uygulaması oluşturun.
* Uygulama kimliği ve kimlik doğrulama anahtarını alma
* Kiracı kimliğini alma
* Uygulamaya "Katılımcı" rolü atama

###  <a name="branch"></a>Dal cihaz bilgilerini karşıya yükleme

Dal (şirket içi site) bilgilerini yüklemek için kullanıcı deneyimi tasarlayın. [REST API'leri](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) VPNSite site bilgilerini sanal WAN içinde oluşturmak için kullanılabilir. Tüm dal SDWAN/VPN cihazları sağlayın ya da cihaz özelleştirmeleri uygun olanını seçin.


### <a name="device"></a>Cihaz yapılandırma karşıdan yükleme ve bağlantı

Bu adım, Azure yapılandırması indiriliyor ve dal cihazında Azure sanal WAN bağlantısı ayarını içerir. Bu adımda, bir sağlayıcı kullanmayan bir müşteri el ile Azure yapılandırmayı indirmek ve bunu şirket içi SDWAN/VPN cihazını uygulayabilirsiniz. Bir sağlayıcı, bu adımı otomatikleştirin. Cihaz denetleyicisini genellikle şu dosyaya benzer görünecektir Azure yapılandırmayı indirmek için 'GetVpnConfiguration' REST API'si çağırabilirsiniz.

**Yapılandırma notları**

  * Azure sanal ağları sanal hub'ına takılı ise ConnectedSubnets görünürler.
  * VPN bağlantısı yapılandırma rota tabanlı ve Ikev2/Ikev1 kullanır.

#### <a name="understanding-the-device-configuration-file"></a>Cihaz yapılandırma dosyasını anlama

Cihaz yapılandırma dosyasında şirket içi VPN cihazınızı yapılandırırken kullanacağınız ayarlar bulunur. Bu dosyayı görüntülediğinizde aşağıdaki bilgilere dikkat edin:

* **vpnSiteConfiguration -** Bu bölümde sanal WAN'a bağlanan bir site olarak ayarlanmış cihazın ayrıntıları yer alır. Dal cihazının adını ve genel IP adresini içerir.
* **vpnSiteConnections -** Bu bölümde aşağıdakilerle ilgili bilgiler yer alır:

    * **Adres alanı** sanal hub(ları) VNet biri.<br>Örnek:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Adres alanı** hub'a bağlı sanal ağlar.<br>Örnek:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * vpngateway sanal hub'ının **IP adresleri**. vpngateway, etkin-etkin yapılandırmada 2 tünel içeren bağlantılara sahip olduğundan bu dosyada iki taraftaki IP adreslerinin de listelendiğini göreceksiniz. Bu örnekte her site için "Instance0" ve "Instance1" örneklerini göreceksiniz.<br>Örnek:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * BGP, önceden paylaşılan anahtar gibi **vpngateway bağlantısı yapılandırma ayrıntıları**. PSK, sizin için otomatik olarak oluşturulan önceden paylaşılan anahtardır. Dilediğiniz zaman genel bakış sayfasındaki bağlantıyı düzenleyerek özel bir PSK ekleyebilirsiniz.
  
#### <a name="example-device-configuration-file"></a>Örnek cihaz yapılandırma dosyası

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

## <a name="default"></a>IPSec bağlantısını için varsayılan ilkeler

### <a name="initiator"></a>Başlatıcı

Azure Başlatıcı tünel için olduğunda, aşağıdaki bölümlerde desteklenen ilke birleşimleri listeleyin.

**1. Aşama**

* AES_256 VE, SHA1, DH_GROUP_2
* AES_256 VE, SHA_256, DH_GROUP_2
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2

**Aşama 2**

* GCM_AES_256, GCM_AES_256, PFS_NONE
* AES_256 VE, SHA_1, PFS_NONE
* AES_256 VE, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE

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

Şirket içi SDWAN/VPN cihazınız veya cihaz yapılandırma SD-WAN ile aynı veya aşağıdaki algoritmaları ve Azure IPSec/IKE ilkesinde belirttiğiniz parametrelerini içerir.

* IKE şifreleme algoritması
* IKE bütünlük algoritması
* DH Grubu
* IPsec şifreleme algoritması
* IPsec bütünlük algoritması
* PFS Grubu

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz: [hakkında Azure sanal WAN](virtual-wan-about.md) ve [Azure sanal WAN SSS](virtual-wan-faq.md).

Herhangi bir ek bilgi için lütfen bir e-posta gönderin <azurevirtualwan@microsoft.com>. Şirketinizin adını konu satırına “[ ]” içinde yazın.
