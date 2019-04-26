---
title: Azure DMZ örnek – derleme Nsg'ler ile basit DMZ | Microsoft Docs
description: Ağ güvenlik grupları (NSG) ile bir çevre ağı oluşturma
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 68655ea03f53fe7100f67d111fcd3c8595bdf4c9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362173"
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Örnek 1 – derleme Nsg'leri kullanarak bir Azure Resource Manager şablonu ile basit DMZ
[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager Şablonu](virtual-networks-dmz-nsg.md)
> * [Klasik - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Bu örnekte, dört Windows sunucu ve ağ güvenlik grupları ile basit DMZ oluşturur. Bu örnek, her adımda daha derin bir anlayış sağlamak için ilgili şablonu bölümlerin her birinde açıklar. Trafik dmz'deki savunma katmanları ile nasıl devam eder, ayrıntılı bir adım adım bakış sağlamak için trafiği senaryo bölüm de mevcuttur. Son olarak, test ve deneme ile çeşitli senaryolar için bu ortamı oluşturmak için yönergeler ve tam veri şablonunun kod başvuruları bölüm yöneliktir. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Gelen NSG ile DMZ][1]

## <a name="environment-description"></a>Ortam tanımı
Bu örnekte, bir aboneliği aşağıdaki kaynaklar içerir:

* Tek bir kaynak grubu
* İki alt ağlı sanal ağ; "FrontEnd" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows Server
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server
* Uygulama web sunucusu ile ilişkili bir genel IP adresi

Başvuru bölümünde, bu örnekte açıklanan ortamı oluşturan bir Azure Resource Manager şablonu için bir bağlantı yoktur. VM'ler ve sanal ağlar oluşturma örnek şablon tarafından yapılan rağmen değil açıklanmıştır bu belgede ayrıntılı. 

**Bu ortamı oluşturmak için** (ayrıntılı yönergeler bu belgenin başvurular bölümünde olduğunda);

1. Azure Resource Manager şablonu dağıtın: [Azure hızlı başlangıç şablonları][Template]
2. Örnek uygulamayı yükleyin: [Örnek uygulama betiği][SampleApp]

>[!NOTE]
>Bu örnekte herhangi bir arka uç sunucularına RDP için IIS sunucusunda bir "Sıçrama kutusu." kullanılır. IIS sunucusuna ve ardından IIS sunucusu RDP arka uç sunucusu için ilk RDP. Alternatif olarak bir genel IP, her sunucu için daha kolay RDP NIC ile ilişkili olabilir.
> 
>

Aşağıdaki bölümlerde, ağ güvenlik grubu ve nasıl önemli satırlarını Azure Resource Manager şablonu walking tarafından bu örnek için işlev ayrıntılı bir açıklama sağlayın.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi. 

>[!TIP]
>Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız ve ardından daha genel "Reddet" kuralları son. Kurallar öncelik atanmış belirtir, ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları, gelen veya giden yönde (alt ağ perspektifinden) ya da uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kuralları için gelen trafiği oluşturulmakta:

1. İç DNS trafiği (bağlantı noktası 53) izin verilir
2. Herhangi bir VM için RDP trafiğinin (3389 bağlantı noktası) Internet'ten izin verilir
3. Web sunucusu (IIS01) İnternet'ten gelen HTTP trafiğine (bağlantı noktası 80) izin verilir
4. Tüm trafiğe (tüm bağlantı noktaları) IIS01 AppVM1 için izin verilir
5. Tüm trafiği (tüm bağlantı noktaları) İnternet'ten gelen tüm VNet (her iki alt ağ) reddedildi
6. Tüm trafik (tüm bağlantı noktaları) Frontend alt ağından arka uç alt ağına reddedildi

Bu kurallar ile hem kuralları 3 web sunucusuna Internet'ten gelen HTTP isteği ise, her alt ağa bağlı (izin ver) ve 5 (Reddet. geçerli), ancak yalnızca geçerli ve kural 5 oyuna değil gelir kural 3 daha yüksek bir önceliğe sahip olduğundan. Bu nedenle web sunucusuna HTTP isteği izin verilecek. Bu aynı trafiği DNS01 sunucunun erişmeye kural 5 (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmek için izin verilecek değil. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafik kuralları 1 ve 4) Konuşmayı gelen ön uç alt ağını engeller, bir saldırganın güvenlik ihlalleri saldırgan ön uç web uygulamasının sınırlı durumda bu kural kümesi arka uç ağını korur. arka uca "ağa (yalnızca AppVM01 sunucu üzerinde kullanıma sunulan kaynakları) korumalı erişim".

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik için güvenlik ilkesini uygulamak için kullanıcı tanımlı yönlendirme gereklidir ve üzerinde "Örnek 3" araştırılan [güvenlik sınırı en iyi yöntemler sayfası][HOME].

Her kural aşağıdaki gibi daha ayrıntılı olarak ele alınmıştır:

1. Bir ağ güvenlik grubu kaynak kuralları tutacak örneği gerekir:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. Bu örnekte ilk kural, tüm iç ağ DNS sunucusuna arka uç alt ağı arasında DNS trafiğine izin verir. Kural, bazı önemli parametrelere sahiptir:
   * DNS trafiği DNS sunucusu ulaşmasına izin verilir, böylece "destinationAddressPrefix" - "10.0.2.4" için hedef adres ön eki ayarlanır.
   * Trafik akışını hangi yönde bu kural etkinleşir "Yönü" belirtir. Perspektifinden (bağlı olarak bu NSG yere bağlıdır) alt ağı veya sanal makine yönüdür. Bu nedenle yönü "Giriş" ve trafiğin alt girdiğinden, kural uygular ve alt ağdan çıkan trafiğe değil etkilenecek bu kural tarafından varsa.
   * "Öncelik" bir trafik akışı değerlendirilme sırasını ayarlar. Düşük sayı öncelik o kadar yüksektir. Belirli bir trafik akışı için bir kural uygulandığı zaman başka hiçbir kural işlenir. Böylece öncelik 1 ile bir kuralı, trafiğine izin verir ve trafiği, öncelik 2 ile bir kural engelleyen ve her iki kural'ı trafiğe uygulamak durumunda trafik akışına izin verilecek (Kural 1 daha yüksek bir önceliğe sahip olduğundan etkisi sürdü ve başka hiçbir kural uygulandı).
   * "Erişim", bu kural tarafından etkilenen trafik engellenmiş ("Reddet") veya izin verilen ("izin ver") olduğunu gösterir.

     ```JSON
     "properties": {
     "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
     ```

3. Bu kural, herhangi bir sunucuda ilişkili alt ağ üzerinde RDP noktasına internet'ten akışı RDP trafiğine izin verir. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Bu kural, web sunucusu isabet gelen internet trafiğini sağlar. Bu kural, yönlendirme davranışını değiştirmez. Kuralın yalnızca geçirmek için IIS01 hedefleyen trafiğe izin verir. Bu nedenle trafik Internet'ten bu kural izin ve daha fazla kural işlemeyi durdur hedef olarak web sunucusuna sahip. (Kuralda öncelikte 140 tüm diğer gelen internet trafiğini engellendi). Yalnızca HTTP trafiğini işlemek, yalnızca hedef 80 numaralı bağlantı noktasına izin vermek için bu kural daha kısıtlanmasını.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet to [variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Bu kural, arka uç trafiği diğer ön uç AppVM01 sunucuya daha yeni bir kural bloklarını IIS01 sunucudan geçirilecek trafiğine izin verir. Bu kural, bağlantı noktasının eklenmesi gereken biliniyorsa geliştirmek için. IIS sunucusu yalnızca SQL Server üzerinde AppVM01 ulaşıyorsa, örneğin, hedef bağlantı noktası aralığı gelen değiştirilmelidir "*" (herhangi) için web uygulaması sürekli görmüş AppVM01 daha küçük bir gelen saldırı yüzeyinde böylece izin 1433'ü (SQL bağlantı noktası).

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] to [variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Adres ön eki bir "varsayılan etiket" adlı özel bir tür kuralları kullanabilir, bu etiketler daha büyük bir kategorisini adres önekleri belirtmek için kolay bir yol sağlayan sistem tarafından sağlanan tanımlayıcılardır. Bu kural varsayılan etiket hedef adres ön eki için "VirtualNetwork" sanal ağ içinde herhangi bir adresi belirtmek için kullanır. Internet ve AzureLoadBalancer olan diğer ön ek etiketleri. Bu kural, ağ üzerindeki herhangi bir sunucuya internet'ten trafiği engeller. Öncelikle 110 ve 120 kurallarıyla etkisi yalnızca gelen internet trafiğini güvenlik duvarı ve RDP bağlantı noktaları sunucuları ve blokları için her şey izin vermektir. Bu kural, tüm beklenmeyen akışlar engellemek için bir "yedek" kuralıdır.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate the [variables('VNetName')] VNet from the Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. Son kuralı trafiği arka uç alt ağına ön uç alt ağından reddeder. Bu kural yalnızca bir gelen kuralı olduğundan, dönen trafik (alanından, ön uç için arka uç) izin verilir.

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate the [variables('Subnet1Name')] subnet from the [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Trafik senaryoları
#### <a name="allowed-internet-to-web-server"></a>(*İzin verilen*) web sunucusuna Internet
1. NIC'nin IIS01 NIC ile ilişkili genel IP adresinden gelen HTTP sayfası Internet kullanıcı istekleri
2. Genel IP adresini IIS01 doğru sanal ağa trafiği geçirir (web sunucusu)
3. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet'e IIS01) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafiği iç IP adresi (10.0.1.5'i) web sunucusunun IIS01 denk gelir.
5. IIS01 web trafiği, bu isteği alır ve isteği işlemeye başlar.
6. IIS01 SQL Server AppVM01 hakkında bilgi için sorar.
7. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
8. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet için Güvenlik Duvarı) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak için trafiğe izin verilir, kuralı işlemeyi durdur
9. AppVM01 SQL sorgusunu alır ve bunlara yanıt verir
10. Arka uç alt ağda hiçbir giden kuralları olduğundan, yanıt izin verilir
11. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
    2. Alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı, trafiğe izin verilmesi bu trafiğe izin.
12. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlandıktan ve istemciye gönderir
13. Ön uç alt ağda hiçbir giden kuralları olduğundan, yanıt verilir ve istediğiniz web sayfası Internet kullanıcı alır.

#### <a name="allowed-rdp-to-iis-server"></a>(*İzin verilen*) RDP IIS sunucusuna
1. İnternet üzerindeki bir sunucu yöneticisi bir genel IP adresi (Bu genel IP adresi, portalı veya PowerShell bulunabilir) IIS01 NIC ile ilişkili NIC'nin IIS01 için RDP oturumu isteği
2. Genel IP adresini IIS01 doğru sanal ağa trafiği geçirir (web sunucusu)
3. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) geçerli, trafiğin izin verilen, Dur kural işleme
4. Hiçbir giden kuralları, varsayılan kuralları uygulanır ve dönüş trafiğine izin verilir
5. RDP oturumu etkin
6. IIS01 kullanıcı adı ve parola bilgilerini ister.

>[!NOTE]
>Bu örnekte herhangi bir arka uç sunucularına RDP için IIS sunucusunda bir "Sıçrama kutusu." kullanılır. IIS sunucusuna ve ardından IIS sunucusu RDP arka uç sunucusu için ilk RDP.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*İzin verilen*) DNS sunucusundaki Web sunucusu DNS Arama
1. Sunucu, IIS01 www.data.gov bir veri akışı gereksinimlerini, ancak adresini çözümlemek için gereksinimleri web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağında 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS01 için DNS isteği gönderir
3. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
4. Arka uç alt ağı gelen kuralı işleme başlar:
   * NSG kuralı 1 (DNS) geçerli, trafiğin izin verilen, Dur kural işleme
5. DNS sunucusu isteği alır
6. DNS sunucusu önbelleğe alınmış adresleriniz değil ve bir kök DNS sunucusu internet'te sorar
7. Hiçbir giden kuralları arka uç alt ağı üzerinde trafiğe izin verilir
8. Bu oturum dahili olarak başlatıldıktan sonra Internet DNS sunucusu yanıt, yanıt izin verilir
9. DNS sunucusu yanıtı önbelleğe alır ve geri IIS01 ilk isteğine yanıt verir
10. Hiçbir giden kuralları arka uç alt ağı üzerinde trafiğe izin verilir
11. Ön uç alt ağını gelen kuralı işleme başlar:
    1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
    2. Trafiğe izin verilmesi bu trafiğe izin alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı
12. IIS01 DNS01 yanıtı alır

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*İzin verilen*) AppVM01 üzerinde Web sunucusu erişim dosyası
1. Bir dosyanın AppVM01 IIS01 sorar
2. Ön uç alt ağı yok giden kuralları, trafiğe izin verilir
3. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet'e IIS01) değil, uygulamak için sonraki kural taşıma
   4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak için trafiğe izin verilir, kuralı işlemeyi durdur
4. AppVM01 isteği alır ve (erişim yetkisi varsayılarak) dosyası ile yanıt verir
5. Arka uç alt ağda hiçbir giden kuralları olduğundan, yanıt izin verilir
6. Ön uç alt ağını gelen kuralı işleme başlar:
   1. Gelen için uygulanan NSG kural yoktur hiçbir NSG kurallarını uygulamak için ön uç alt ağına arka uç alt ağından gelen trafiği
   2. Alt ağlar arasında trafiğe izin veren varsayılan sistem kuralı, trafiğe izin verilmesi bu trafiğe izin.
7. IIS sunucusu dosyayı alır

#### <a name="denied-rdp-to-backend"></a>(*Reddedildi*) RDP için arka uç
1. İnternet kullanıcı için RDP sunucusuna AppVM01 çalışır.
2. Bu sunucular NIC ile ilişkili genel IP adresi olduğundan, bu trafiğin VNet hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak mıydı
3. Bir genel IP adresi herhangi bir nedenden dolayı etkinleştirilirse, ancak NSG kuralı 2 (RDP) bu trafiğe izin verecek

>[!NOTE]
>Bu örnekte herhangi bir arka uç sunucularına RDP için IIS sunucusunda bir "Sıçrama kutusu." kullanılır. IIS sunucusuna ve ardından IIS sunucusu RDP arka uç sunucusu için ilk RDP.
>
>

#### <a name="denied-web-to-backend-server"></a>(*Reddedildi*) arka uç sunucusuna Web
1. Bir Internet kullanıcı AppVM01 bir dosyaya erişmeye çalışır
2. Bu sunucular NIC ile ilişkili genel IP adresi olduğundan, bu trafiğin VNet hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak mıydı
3. NSG kuralı 5 (Internet'e VNet) genel IP adresi herhangi bir nedenden dolayı etkinleştirilirse, bu trafiği engeller

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Reddedildi*) DNS sunucusundaki Web DNS Arama
1. Bir iç DNS kaydını DNS01 aramak bir internet kullanıcı çalışır
2. Bu sunucular NIC ile ilişkili genel IP adresi olduğundan, bu trafiğin VNet hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak mıydı
3. NSG kuralı 5 (Internet'e VNet) genel IP adresi herhangi bir nedenden dolayı etkinleştirilirse, bu trafiğin engellenecekse (Not: Kural 1 (DNS) istekleri kaynak adresi olduğundan uygulanamaz internet ve kural 1 yalnızca uygular yerel sanal ağ kaynağı olarak)

#### <a name="denied-sql-access-on-the-web-server"></a>(*Reddedildi*) web sunucusu üzerindeki SQL erişimi
1. İnternet kullanıcı SQL veri IIS01 istekleri
2. Bu sunucular NIC ile ilişkili genel IP adresi olduğundan, bu trafiğin VNet hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak mıydı
3. Bir genel IP adresi herhangi bir nedenden dolayı etkinleştirilirse, ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet'e IIS01) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafik İsabetleri IIS01 iç IP adresi (10.0.1.5'i)
5. IIS01 dinleme bağlantı noktası 1433'ü, dolayısıyla isteğine yanıt olarak değil

## <a name="conclusion"></a>Sonuç
Bu örnekte, arka uç alt ağından gelen trafiği yalıtma, görece basit ve anlaşılır bir yoludur.

Daha fazla örnek ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bu örnekte önceden tanımlanmış bir Azure Resource Manager şablonu Microsoft tarafından yönetilen bir GitHub deposu kullanır ve topluluğa açık. Bu şablon Github'dan sınırsız dağıtılabilir veya indirilen ve ihtiyaçlarınızı karşılayacak biçimde değiştirilebilir. 

"Azuredeploy.json." adlı dosyada ana şablonudur Bu şablon PowerShell veya CLI (ile ilişkili "azuredeploy.parameters.json" dosyası) bu şablonu dağıtmak için gönderilebilir. Github'da README.md sayfasında "Azure'a dağıtın" düğmesi kullanmak için en kolay yolu olan buluyorum.

Bu örnekte, GitHub ve Azure portalından oluşturan şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Bir tarayıcıdan gidin [şablonu][Template]
2. "Azure'a dağıtın" düğmesi (veya bu şablonu grafik gösterimi görmek için "Görselleştir" düğmesine) tıklayın.
3. Parametreler dikey penceresinde depolama hesabı, kullanıcı adı ve parola girin ve ardından tıklayın **Tamam**
5. Bu dağıtım için bir kaynak grubu oluşturun (mevcut bir kullanabilirsiniz, ancak en iyi sonuçlar için yeni bir önerim)
6. Gerekirse, abonelik ve konum ayarlarını değiştirin.
7. Tıklayın **yasal koşulları gözden geçir**, koşulları okuyun ve tıklayın **satın alma** kabul edin.
8. Tıklayın **Oluştur** Bu şablon dağıtımını başlatmak için.
9. Dağıtım başarıyla tamamlandıktan sonra içinde yapılandırılmış kaynakları görmek için kaynak bu dağıtım için oluşturulan grubuna gidin.

>[!NOTE]
>Bu şablon, yalnızca IIS01 sunucusuna RDP (Bul genel IP portalı IIS01 için) sağlar. Bu örnekte herhangi bir arka uç sunucularına RDP için IIS sunucusunda bir "Sıçrama kutusu." kullanılır. IIS sunucusuna ve ardından IIS sunucusu RDP arka uç sunucusu için ilk RDP.
>
>

Bu dağıtım kaldırmak için kaynak grubunu silin ve tüm alt kaynaklar da silinir.

#### <a name="sample-application-scripts"></a>Örnek uygulama komut dosyaları
Şablon başarıyla çalıştırıldıktan sonra web sunucusu ve uygulama sunucusu bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile ayarlayabilirsiniz. Bu ve diğer DMZ örnekleri için örnek uygulamayı yüklemek için bir aşağıdaki bağlantıda sağlanmıştır: [Örnek uygulama betiği][SampleApp]

## <a name="next-steps"></a>Sonraki adımlar

* Bu örnek dağıtma
* Örnek uygulaması oluşturma
* Bu DMZ ile farklı trafik akışları test etme

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Gelen NSG ile DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md
