---
title: "Azure DMZ örnek – yapı Nsg'ler ile basit DMZ | Microsoft Docs"
description: "Bir çevre ağ güvenlik grupları (NSG) ile derleme"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ec29e6b250f927a3a4a94ffdf83d6c7c0e325722
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Örnek 1 – Nsg'ler ile bir Azure Resource Manager şablonu kullanarak basit bir DMZ derleme
[Güvenlik sınırı en iyi yöntemler sayfasına dön][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager Şablonu](virtual-networks-dmz-nsg.md)
> * [Klasik - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Bu örnekte basit DMZ dört Windows sunucuları ve ağ güvenlik grupları oluşturulur. Bu örnekte her adımın daha derin bir anlayış sağlamak için ilgili şablonu bölümlerde açıklanmaktadır. Çevre savunma katmanları üzerinden trafik nasıl devam eder bir ayrıntılı adım adım bakış sağlamak için bir trafik senaryosu bölümü yoktur. Son olarak, içindeki başvuruların sınamak ve çeşitli senaryolarıyla denemeler için bu ortamı oluşturmak için yönergeleri ve tam şablonu kodu bölümüdür. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![NSG ile giriş DMZ][1]

## <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, bir abonelik aşağıdaki kaynaklar içeriyor:

* Tek bir kaynak grubu
* Sanal ağı iki alt ağ ile; "Ön uç" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu
* Uygulama web sunucusu ile ilişkili bir ortak IP adresi

References bölümünde bu örnekte açıklanan ortamı derlemeler bir Azure Resource Manager şablonu bağlantısı yoktur. VM'ler ve sanal ağlar, derleme örnek şablon tarafından yapılan rağmen değil açıklanmıştır bu belgede ayrıntılı. 

**Bu ortamı oluşturmak için** (ayrıntılı yönergeler bu belgenin references bölümünde olduğunda);

1. Konumundaki Azure Resource Manager şablonu dağıtma: [Azure hızlı başlangıç şablonları][Template]
2. Örnek uygulamayı yükleyin: [örnek uygulama betiği][SampleApp]

>[!NOTE]
>Bu örnekte tüm arka uç sunucularına RDP için IIS'nin bir "atlama kutusu." kullanılır İlk RDP IIS sunucusunu, ardından da arka uç sunucusuna IIS sunucu RDP. Alternatif olarak bir ortak IP her sunucu için daha kolay RDP NIC ile ilişkili olabilir.
> 
>

Aşağıdaki bölümlerde ağ güvenlik grubu ve nasıl Azure Resource Manager şablonu anahtar satırları arasında adım adım ilerlemenizi sağlayarak bu örnek için işlevleri ayrıntılı bir açıklama belirtin.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu yerleşik ve altı kurallarıyla yüklendi. 

>[!TIP]
>Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız gerekir ve daha genel "Deny" kuralları en son. Kurallar öncelik atanmış belirtir ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları her (alt ağ perspektifinden) gelen veya giden yönde uygulayabilirsiniz.
>
>

Bildirimli olarak, aşağıdaki kural gelen trafik için oluşturulmakta:

1. İç DNS trafiğinin (bağlantı noktası 53) izin verilir
2. Her VM için RDP trafiğinin (3389 numaralı bağlantı noktası) Internet'ten izin verilir
3. Web sunucusu (IIS01) için HTTP trafiğine (bağlantı noktası 80) Internet'ten izin verilir
4. Tüm trafiği (tüm bağlantı noktaları) IIS01 AppVM1 izin verilir
5. Tüm trafiği (tüm bağlantı noktaları) Internet'ten tüm VNet (her iki alt ağ) reddedildi
6. Tüm trafiği (tüm bağlantı noktaları) ön uç alt ağından arka uç alt ağa reddedildi

Bu kurallar ile bağlı her alt ağ için bir HTTP isteği hem kuralları 3, web sunucusu Internet'ten gelen ise (izin ver) ve 5 (uygulamak reddetme), ancak 3 kuralı, yalnızca geçerli olur ve kural 5 oyuna değil gelen daha yüksek öncelikli olduğundan. Bu nedenle web sunucusuna HTTP isteği izin verilir. Bu aynı trafiği DNS01 sunucunun erişmeye, kural 5 (Reddet) uygulamak için ilk olacaktır ve trafiği sunucuya geçirmeniz izin verilmiyor. Kural 6 (Reddet) arka uç alt ağına (dışında izin verilen trafiği kurallarında 1 ve 4) Konuşmayı gelen ön uç alt ağ blokları, bu kural kümesine durumda arka uç ağ saldırgan ön uç web uygulamasında arka uç "korumalı" ağa (yalnızca AppVM01 sunucuda kullanıma sunulan kaynakları) erişimi sınırlı bir saldırganın güvenlik ihlalleri korur.

İnternet giden trafiğe izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik için güvenlik ilkesini uygulamak için kullanıcı tanımlı yönlendirme gereklidir ve "Örnek 3" üzerinde incelediniz [güvenlik sınırı en iyi yöntemler sayfası][HOME].

Her kural aşağıdaki gibi daha ayrıntılı olarak ele alınmıştır:

1. Kuralları tutmak için ağ güvenlik grubu kaynak örneği gerekir:

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

2. Bu örnekte ilk kural DNS sunucusu arka uç alt ağdaki tüm iç ağ arasında DNS trafiğine izin verir. Kural bazı önemli parametreleri içerir:
  * "destinationAddressPrefix" - özel türde bir adres öneki "Varsayılan etiket" adlı kural kullanabilirsiniz, bu etiketler adres öneklerinin daha büyük bir kategori adres için kolay bir yol sağlayan sistem tarafından sağlanan tanımlayıcılardır. Bu kural varsayılan etiket "Internet" sanal ağ dışında herhangi bir adresini belirtmek için kullanır. Diğer önek VirtualNetwork ve AzureLoadBalancer etiketleridir.
  * "Yönünü" trafik akışı hangi yönde bu kural etkinleşir belirtir. Perspektifinden (bağlı olarak bu NSG burada bağlı) alt ağ ya da sanal makinenin yönü olur. Bu nedenle yönü "Giriş" ve trafik alt girerek, kural geçerli olur ve alt ağdan çıkan trafiğe etkilenmez bu kural tarafından durumunda.
  * "Öncelik" bir trafik akışı değerlendirileceğini sırasını belirler. Düşük sayı öncelik o kadar yüksektir. Belirli bir trafik akışı için bir kural uygulandığı zaman başka hiçbir kural işlenir. Bu nedenle bir kural öncelik 1 ile trafiğine izin verir ve trafiği, öncelik 2 olan bir kural reddeder ve her iki kural trafiğe uygulanır durumunda trafik akışı için izin verilir (Kural 1 yüksek önceliğe sahip olduğundan etkisi sürdü ve başka hiçbir kural uygulanan).
  * "Erişim" Bu kural tarafından etkilenen trafiği engellenen ("Deny") veya izin verilen ("izin ver") olup olmadığını belirtir.

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

3. Bu kural RDP bağlantı noktasına bağlı alt ağ üzerinde herhangi bir sunucuda internet'ten akış RDP trafiğine izin verir. 

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

4. Bu kural, web sunucusu isabet gelen internet trafiğine izin verir. Bu kural, yönlendirme davranışını değiştirmez. Kuralın yalnızca geçirmek için IIS01 giden trafiğe izin verir. Bu nedenle Internet trafiği bu kural izin ve daha fazla kural işlemeyi durdur hedef olarak web sunucusunun sahip. (Öncelikle kuralında 140 diğer tüm gelen Internet trafiğini engellendi). Yalnızca HTTP trafiğini işlemek, yalnızca hedef bağlantı noktası 80 izin vermek için bu kural daha fazla kısıtlanmasını.

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

5. Bu kural AppVM01 sunucuya daha yeni bir kural bloklarını arka uç trafiği diğer ön uç IIS01 sunucudan geçmesine trafiğine izin verir. Bu kural, bağlantı noktasının eklenmesi gereken biliniyorsa geliştirmek için. IIS sunucusu yalnızca SQL Server'da AppVM01 basarsa, örneğin, hedef bağlantı noktası aralığı değiştirildiği "*" (tüm) böylece web uygulaması hiç tehlikeye AppVM01 üzerinde daha küçük bir gelen saldırı yüzeyini sağlar 1433 (SQL bağlantı noktası) için.

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

6. Bu kural, ağ üzerindeki herhangi bir sunucuya internet'ten trafiği engeller. Öncelikle 110 ve 120 kurallarıyla etkisi yalnızca gelen Internet trafiği güvenlik duvarı ve RDP bağlantı noktalarını sunucularda ve blokları şey izin vermektir. Bu kural, tüm beklenmeyen akışlar engellemek için bir "yedek" kuralıdır.

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

7. Son bir kural arka uç alt ağına ön uç alt ağından trafiği engeller. Bu kural yalnızca bir gelen kuralı olduğundan, geriye doğru trafiğinin (Başlangıç ön uç için arka uç) izin verilir.

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
#### <a name="allowed-internet-to-web-server"></a>(*İzin verilen*) web sunucusu Internet'e
1. Bir Internet kullanıcı IIS01 NIC ile ilişkili NIC ortak IP adresinden gelen istekler bir HTTP sayfası
2. Genel IP adresi IIS01 doğru sanal ağa trafiği geçirir (web sunucusu)
3. Ön uç alt gelen kuralı işleme başlar:
  1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
  2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
  3. NSG kural 3 (IIS01 Internet'e) için geçerli, izin verilen, Dur kural işlenirken trafiğidir
4. İç IP adresi web sunucusunun IIS01 (10.0.1.5) trafiği isabetler
5. IIS01 web trafiği için dinleme, bu isteği alır ve isteği işlemeye başlıyor
6. IIS01 SQL Server AppVM01 hakkında bilgi için sorar
7. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
8. Arka uç alt gelen kuralı işleme başlar:
  1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
  2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
  3. NSG kural 3 (Internet güvenlik duvarı için) değil, uygulamak için sonraki kural taşıma
  4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak, trafiğine izin verilir, kural işleme Durdur
9. AppVM01 SQL sorgusu alır ve yanıtlar
10. Arka uç alt ağda hiçbir giden kuralları olduğundan, yanıt izin verilir
11. Ön uç alt gelen kuralı işleme başlar:
  1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
  2. Alt ağlar arasında trafiğe izin varsayılan sistem kuralı, trafiğine izin bu trafiği olanak tanır.
12. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlar ve istemciye gönderir
13. Ön uç alt ağda hiçbir giden kuralları olduğundan, yanıt izin verilir ve istenen web sayfasının Internet kullanıcı alır.

#### <a name="allowed-rdp-to-iis-server"></a>(*İzin verilen*) IIS sunucusuna RDP
1. İnternet üzerindeki bir sunucu yöneticisi IIS01 (Bu genel IP adresi, Portal veya PowerShell aracılığıyla bulunabilir) IIS01 NIC ile ilişkili NIC genel IP adresi üzerinde bir RDP oturumu istekleri
2. Genel IP adresi IIS01 doğru sanal ağa trafiği geçirir (web sunucusu)
3. Ön uç alt gelen kuralı işleme başlar:
  1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
  2. NSG kuralı 2 (RDP) uygulamak için izin verilen, Dur kural işlenirken trafiğidir
4. Hiçbir giden kuralları, varsayılan kuralları uygula ve dönüş trafiğine izin verilir
5. RDP oturumu etkin
6. Kullanıcı adı ve parola IIS01 isteyen

>[!NOTE]
>Bu örnekte tüm arka uç sunucularına RDP için IIS'nin bir "atlama kutusu." kullanılır İlk RDP IIS sunucusunu, ardından da arka uç sunucusuna IIS sunucu RDP.
>
>

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*İzin verilen*) Web sunucusu DNS araması DNS sunucusunda
1. Sunucu, IIS01, www.data.gov bir veri akışı gereksinimlerini ancak adresini çözümlemek için gereksinimlerini web.
2. Ağ yapılandırma için VNet listeleri DNS01 (arka uç alt ağda 10.0.2.4) birincil DNS sunucusu olarak IIS01 DNS isteği için DNS01 gönderir
3. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
4. Arka uç alt gelen kuralı işleme başlar:
  * NSG kural 1 (DNS) uygulamak için izin verilen, Dur kural işlenirken trafiğidir
5. DNS sunucusu isteği alır
6. DNS sunucusu önbelleğe adresi yoksa ve bir kök DNS sunucusu internet'te sorar
7. Hiçbir giden kuralları arka uç alt ağdaki trafiğe izin verilir
8. Bu oturumda dahili olarak başlatıldı beri Internet DNS sunucusu yanıt, yanıt izin verilir
9. DNS sunucusu yanıtı önbelleğe alır ve geri IIS01 ilk isteğine yanıt verir
10. Hiçbir giden kuralları arka uç alt ağdaki trafiğe izin verilir
11. Ön uç alt gelen kuralı işleme başlar:
  1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
  2. Trafiğe izin için alt ağlar arasında trafiğe izin varsayılan sistem kuralı bu trafiği olanak tanır
12. IIS01 DNS01 yanıtı alır

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*İzin verilen*) Web sunucusu erişimini dosyasını AppVM01 üzerinde
1. Bir dosyanın AppVM01 IIS01 sorar
2. Ön uç alt ağdaki hiçbir giden kuralları, trafiğe izin verilir
3. Arka uç alt gelen kuralı işleme başlar:
  1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
  2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
  3. NSG kural 3 (IIS01 Internet'e) değil, uygulamak için sonraki kural taşıma
  4. NSG kuralı 4 (IIS01 AppVM01 için) uygulamak, trafiğine izin verilir, kural işleme Durdur
4. AppVM01 isteği alır ve (erişim yetkisi varsayılarak) dosyası ile yanıt verir
5. Arka uç alt ağda hiçbir giden kuralları olduğundan, yanıt izin verilir
6. Ön uç alt gelen kuralı işleme başlar:
  1. Gelen için geçerli NSG kural yok NSG hiçbiri uygulama kuralları için ön uç alt ağa, arka uç alt ağından gelen trafik
  2. Alt ağlar arasında trafiğe izin varsayılan sistem kuralı, trafiğine izin bu trafiği olanak tanır.
7. IIS sunucu dosya alır

#### <a name="denied-rdp-to-backend"></a>(*Reddedildi*) arka uç için RDP
1. Bir Internet kullanıcı için RDP AppVM01 sunucuya çalışır.
2. Bu sunucular NIC ile ilişkili herhangi bir ortak IP adresi olduğundan, bu trafiği sanal ağ hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak olmayacaktır
3. Bir ortak IP adresi herhangi bir nedenden dolayı etkinleştirilirse, NSG kuralı 2 (RDP) bu trafiği ancak olanak tanır

>[!NOTE]
>Bu örnekte tüm arka uç sunucularına RDP için IIS'nin bir "atlama kutusu." kullanılır İlk RDP IIS sunucusunu, ardından da arka uç sunucusuna IIS sunucu RDP.
>
>

#### <a name="denied-web-to-backend-server"></a>(*Reddedildi*) arka uç sunucusuna Web
1. Bir Internet kullanıcı AppVM01 bir dosyaya erişmeyi dener
2. Bu sunucular NIC ile ilişkili herhangi bir ortak IP adresi olduğundan, bu trafiği sanal ağ hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak olmayacaktır
3. Bir ortak IP adresi herhangi bir nedenden dolayı etkinleştirilirse, NSG kural 5 (sanal ağa Internet) bu trafiği engeller

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Reddedildi*) DNS sunucusunda Web DNS araması
1. Bir iç DNS kaydını DNS01 aramak bir internet kullanıcı çalışır
2. Bu sunucular NIC ile ilişkili herhangi bir ortak IP adresi olduğundan, bu trafiği sanal ağ hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak olmayacaktır
3. Bir ortak IP adresi herhangi bir nedenden dolayı etkinleştirilirse, NSG kural 5 (sanal ağa Internet) bu trafiği engeller (Not: istekleri kaynak adresi için kural 1 (DNS) uygulanacak değil, internet ve kural 1 yalnızca geçerlidir kaynağı olarak yerel vnet'e)

#### <a name="denied-sql-access-on-the-web-server"></a>(*Reddedildi*) web sunucusu üzerinde SQL erişimi
1. Bir Internet kullanıcının SQL veri IIS01 istekleri
2. Bu sunucular NIC ile ilişkili herhangi bir ortak IP adresi olduğundan, bu trafiği sanal ağ hiçbir zaman girersiniz ve tarafından sunucuya ulaşmak olmayacaktır
3. Bir ortak IP adresi herhangi bir nedenden dolayı etkinleştirilirse, ön uç alt gelen kuralı işleme başlar:
  1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
  2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
  3. NSG kural 3 (IIS01 Internet'e) için geçerli, izin verilen, Dur kural işlenirken trafiğidir
4. Trafik isabetler IIS01 iç IP adresi (10.0.1.5)
5. IIS01 1433 numaralı bağlantı noktasını, dolayısıyla isteğine yanıt olarak dinlemede değil

## <a name="conclusion"></a>Sonuç
Bu örnek, arka uç alt ağından gelen trafiği yalıtma, nispeten basit ve düz ileriye doğru bir yoludur.

Daha fazla örnekler ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Bu örnek, önceden tanımlanmış bir Azure Resource Manager şablonu Microsoft tarafından yönetilen bir GitHub deposunu kullanır ve topluluğa açık. Bu şablon Github'dan sınırsız dağıtılabilir veya indirdiğiniz ve gereksinimlerinizi karşılayacak biçimde değiştirilebilir. 

"Azuredeploy.json." adlı dosyada ana şablon. Bu şablon PowerShell veya CLI (ile ilişkili "azuredeploy.parameters.json" dosyası) bu şablonu dağıtmak için gönderilebilir. Github'da README.md sayfasında "Azure dağıtma" düğmesi kullanmak için en kolay yoludur bulabilirim.

Bu örnek GitHub ve Azure portalını derlemeler şablonu dağıtmak için aşağıdaki adımları izleyin:

1. Tarayıcıdan gidin [şablonu][Template]
2. "Azure dağıtma" (veya bu şablonu grafik gösterimi görmek için "Görselleştir" düğmesini) tıklatın
3. Parametreler dikey penceresinde depolama hesabı, kullanıcı adı ve parola girin ve ardından **Tamam**
5. Bu dağıtım için bir kaynak grubu oluşturun (mevcut bir kullanabilirsiniz, ancak yeni bir en iyi sonuçlar için önerilir)
6. Gerekirse, abonelik ve konumda ayarlarını değiştirin.
7. Tıklatın **yasal koşulları gözden geçir**koşullarını okuyun ve tıklatın **satın alma** kabul edin.
8. Tıklatın **oluşturma** Bu şablon dağıtımını başlatmak için.
9. İçinde yapılandırılmış kaynakları görmek için kaynak bu dağıtım için oluşturulan grubuna dağıtım başarıyla tamamlandıktan sonra gidin.

>[!NOTE]
>Bu şablon RDP (Bul genel IP portalı IIS01 için) yalnızca IIS01 sunucuya sağlar. Bu örnekte tüm arka uç sunucularına RDP için IIS'nin bir "atlama kutusu." kullanılır İlk RDP IIS sunucusunu, ardından da arka uç sunucusuna IIS sunucu RDP.
>
>

Bu dağıtım kaldırmak için kaynak grubunu silin ve tüm alt kaynaklar da silinir.

#### <a name="sample-application-scripts"></a>Örnek uygulama komut dosyaları
Şablonu başarıyla çalıştıktan sonra web sunucusu ve uygulama sunucusu bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile ayarlayabilirsiniz. Bu ve diğer çevre örnekleri için örnek bir uygulama yüklemek için bir aşağıdaki bağlantıda sağlanmış: [örnek uygulama betiği][SampleApp]

## <a name="next-steps"></a>Sonraki adımlar

* Bu örnek dağıtma
* Örnek uygulaması oluşturma
* Bu DMZ aracılığıyla farklı trafik akışları test etme

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "NSG ile giriş DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md