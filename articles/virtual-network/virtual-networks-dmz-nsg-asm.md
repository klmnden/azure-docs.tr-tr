---
title: Azure DMZ örnek – yapı Nsg'ler ile basit DMZ | Microsoft Docs
description: Bir çevre ağ güvenlik grupları (NSG) ile derleme
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: ed172d552e1e4c9ee27c58abcd7ad2d98df21579
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "23928891"
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>Örnek 1 – Nsg'ler ile klasik PowerShell kullanarak basit bir DMZ derleme
[Güvenlik sınırı en iyi yöntemler sayfasına dön][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager Şablonu](virtual-networks-dmz-nsg.md)
> * [Klasik - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Bu örnekte basit DMZ dört Windows sunucuları ve ağ güvenlik grupları oluşturulur. Bu örnek, her bir adımın daha derin bir anlayış sağlamak için ilgili PowerShell komutların her birini açıklar. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryosu bölümü olan adım adım çevre savunma katmanlar arasında nasıl trafiği devam eder. Son olarak, içindeki başvuruların sınamak ve çeşitli senaryolarıyla denemeler için bu ortamı oluşturmak için yönerge ve tam bir kod bölümüdür. 

![NSG ile giriş DMZ][1]

## <a name="environment-description"></a>Ortam açıklaması
Bu örnekte, bir abonelik aşağıdaki kaynaklar içeriyor:

* İki bulut hizmetlerini: "FrontEnd001" ve "BackEnd001"
* Sanal ağ, "CorpNetwork" iki alt ağı; "Ön uç" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Uygulama web sunucusu ("IIS01") temsil eden bir Windows sunucusu
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows sunucusu

References bölümünde bu örnekte açıklanan ortamı çoğunu derlemeler bir PowerShell komut dosyası yok. VM'ler ve sanal ağlar, derleme örnek komut dosyası tarafından yapılır rağmen değil açıklanmıştır bu belgede ayrıntılı. 

Ortamı oluşturmak için;

1. References bölümünde bulunan ağ yapılandırma xml dosyasını kaydedin (adlar, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilir)
2. Kullanıcı değişkenleri (Abonelikleri, hizmet adlarını, vb. karşı) çalıştırılacak komut dosyasıdır ortamıyla eşleşecek şekilde güncelleştirin
3. PowerShell komut dosyası yürütme

>[!Note]
>PowerShell Betiği miktarlara bölge ağ yapılandırması xml dosyasında miktarlara bölge ile eşleşmelidir.
>
>

Komut dosyasını başarıyla ek çalıştırır sonra isteğe bağlı adımlar izlenebilir, web sunucusu ve uygulama sunucusu bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile ayarlamak için iki komut dosyası başvuruları bölümünde bulunur.

Aşağıdaki bölümlerde ağ güvenlik grupları ve bunların Bu örnek için PowerShell Betiği anahtar satırları arasında adım adım ilerlemenizi sağlayarak işlevleri ayrıntılı bir açıklama belirtin.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu yerleşik ve altı kurallarıyla yüklendi. 

> [!TIP]
> Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız gerekir ve daha genel "Deny" kuralları en son. Kurallar öncelik atanmış belirtir ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları her (alt ağ perspektifinden) gelen veya giden yönde uygulayabilirsiniz.
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

İnternet giden trafiğe izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir ve "Örnek 3" üzerinde incelediniz [güvenlik sınırı en iyi yöntemler sayfası][HOME].

Her kural aşağıdaki gibi daha ayrıntılı olarak ele (**Not**: dolar işareti aşağıdaki liste başlayarak herhangi bir öğeye (örneğin: $NSGName) kullanıcı tanımlı bir değişken başvurusu bölümünde bu belgenin betikten):

1. İlk ağ güvenlik grubu kuralları tutmak için yerleşik gerekir:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. Bu örnekte ilk kural DNS sunucusu arka uç alt ağdaki tüm iç ağ arasında DNS trafiğine izin verir. Kural bazı önemli parametreleri içerir:
   
   * "Tür" trafik akışı hangi yönde bu kural etkinleşir belirtir. Perspektifinden (bağlı olarak bu NSG burada bağlı) alt ağ ya da sanal makinenin yönü olur. Bu nedenle türüdür "Giriş" ve trafik alt girerek, kural geçerli olur ve alt ağdan çıkan trafiğe etkilenmemiştir bu kural tarafından varsa.
   * "Öncelik" bir trafik akışı değerlendirileceğini sırasını belirler. Düşük sayı öncelik o kadar yüksektir. Belirli bir trafik akışı için bir kural uygulandığı zaman başka hiçbir kural işlenir. Bu nedenle bir kural öncelik 1 ile trafiğine izin verir ve trafiği, öncelik 2 olan bir kural reddeder ve her iki kural trafiğe uygulanır durumunda trafik akışı için izin verilir (Kural 1 yüksek önceliğe sahip olduğundan etkisi sürdü ve başka hiçbir kural uygulanan).
   * Bu kural tarafından etkilenen trafiği engellendi veya izin verilen "Eylem" belirtir.

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. Bu kural RDP bağlantı noktasına bağlı alt ağ üzerinde herhangi bir sunucuda internet'ten akış RDP trafiğine izin verir. Bu kural adres öneklerini iki özel tür kullanır; "Vırtual_network" ve "INTERNET." Bu etiketler adres öneklerinin daha büyük bir kategori adres için kolay bir yoludur.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Bu kural, web sunucusu isabet gelen internet trafiğine izin verir. Bu kural, yönlendirme davranışını değiştirmez. Kuralın yalnızca geçirmek için IIS01 giden trafiğe izin verir. Bu nedenle Internet trafiği bu kural izin ve daha fazla kural işlemeyi durdur hedef olarak web sunucusunun sahip. (Öncelikle kuralında 140 diğer tüm gelen Internet trafiğini engellendi). Yalnızca HTTP trafiğini işlemek, yalnızca hedef bağlantı noktası 80 izin vermek için bu kural daha fazla kısıtlanmasını.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Bu kural AppVM01 sunucuya daha yeni bir kural bloklarını arka uç trafiği diğer ön uç IIS01 sunucudan geçmesine trafiğine izin verir. Bu kural, bağlantı noktasının eklenmesi gereken biliniyorsa geliştirmek için. IIS sunucusu yalnızca SQL Server'da AppVM01 basarsa, örneğin, hedef bağlantı noktası aralığı değiştirildiği "*" (tüm) böylece web uygulaması hiç tehlikeye AppVM01 üzerinde daha küçük bir gelen saldırı yüzeyini sağlar 1433 (SQL bağlantı noktası) için.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Bu kural, ağ üzerindeki herhangi bir sunucuya internet'ten trafiği engeller. Öncelikle 110 ve 120 kurallarıyla etkisi yalnızca gelen Internet trafiği güvenlik duvarı ve RDP bağlantı noktalarını sunucularda ve blokları şey izin vermektir. Bu kural, tüm beklenmeyen akışlar engellemek için bir "yedek" kuralıdır.
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $VNetName VNet from the Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. Son bir kural arka uç alt ağına ön uç alt ağından trafiği engeller. Bu kural yalnızca bir gelen kuralı olduğundan, geriye doğru trafiğinin (Başlangıç ön uç için arka uç) izin verilir.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a>Trafik senaryoları
#### <a name="allowed-internet-to-web-server"></a>(*İzin verilen*) web sunucusu Internet'e
1. Bir Internet kullanıcı FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) istekler bir HTTP sayfası
2. Bulut hizmeti geçişleri trafiğini açık uç noktasını IIS01 doğru bağlantı noktası 80 üzerinden (web sunucusu)
3. Ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kural 3 (IIS01 Internet'e) için geçerli, izin verilen, Dur kural işlenirken trafiğidir
4. İç IP adresi web sunucusunun IIS01 (10.0.1.5) trafiği isabetler
5. IIS01 web trafiği için dinleme, bu isteği alır ve isteği işlemeye başlıyor
6. IIS01 SQL Server AppVM01 hakkında bilgi için sorar
7. Ön uç alt ağda hiçbir giden kuralları olduğundan, trafiğe izin verilir
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
12. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlar ve istek sahibine gönderir
13. Hiçbir giden kuralları olduğundan ön uç alt ağda yanıt izin verilir ve internet kullanıcı istenen web sayfasının alır.

#### <a name="allowed-rdp-to-backend"></a>(*İzin verilen*) arka uç için RDP
1. Sunucu Yöneticisi internet üzerindeki AppVM01 xxxxx rastgele atanan bağlantı noktası numarası (atanan bağlantı noktası, Azure portal veya PowerShell aracılığıyla bulunabilir) AppVM01 için RDP olduğu BackEnd001.CloudApp.Net:xxxxx üzerinde RDP oturumu istekleri
2. Arka uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) uygulamak için izin verilen, Dur kural işlenirken trafiğidir
3. Hiçbir giden kuralları, varsayılan kuralları uygula ve dönüş trafiğine izin verilir
4. RDP oturumu etkin
5. Kullanıcı adı ve parola AppVM01 isteyen

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

#### <a name="denied-web-to-backend-server"></a>(*Reddedildi*) arka uç sunucusuna Web
1. Bir Internet kullanıcı BackEnd001.CloudApp.Net hizmeti aracılığıyla AppVM01 bir dosyaya erişmeyi dener
2. Dosya Paylaşımı için açık uç nokta yok olduğundan, bu trafiği bulut hizmeti geçirmek değil'yı ve tarafından sunucuya ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kural 5 (sanal ağa Internet) bu trafiği engeller

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Reddedildi*) DNS sunucusunda Web DNS araması
1. Bir iç DNS kaydında DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla aramak bir internet kullanıcı çalışır
2. DNS için açık uç nokta yok olduğundan, bu trafiği bulut hizmeti geçirmek değil'yı ve tarafından sunucuya ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, NSG kural 5 (sanal ağa Internet) bu trafiği engeller (Not: Bu kural 1 (DNS), iki nedenden dolayı geçerli değil, ilk kaynak adresi Internet, bu kural yalnızca kaynağı olarak yerel vnet'e uygulanır, hiçbir zaman trafiği reddetmeye böylece de bu kural bir izin verme kuralı)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Reddedildi*) Web güvenlik duvarı üzerinden SQL erişimi
1. Bir Internet kullanıcının SQL verileri FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) ' ister.
2. SQL için açık uç nokta yok olduğundan bu trafiği bulut hizmeti geçip geçmeyeceğini değil ve güvenlik duvarı ulaşmak olmayacaktır
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt gelen kuralı işleme başlar:
   1. NSG kural 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kural 3 (IIS01 Internet'e) için geçerli, izin verilen, Dur kural işlenirken trafiğidir
4. Trafik isabetler IIS01 iç IP adresi (10.0.1.5)
5. IIS01 1433 numaralı bağlantı noktasını, dolayısıyla isteğine yanıt olarak dinlemede değil

## <a name="conclusion"></a>Sonuç
Bu örnek, arka uç alt ağından gelen trafiği yalıtma, nispeten basit ve düz ileriye doğru bir yoludur.

Daha fazla örnekler ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırma
Tam komut dosyasını bir PowerShell komut dosyası kaydedin. Ağ Yapılandırma "NetworkConf1.xml." adlı bir dosyaya kaydet
Kullanıcı tanımlı değişkenleri gerektiği gibi değiştirin ve betiği çalıştırın.

#### <a name="full-script"></a>Tam betik
Bu komut dosyası, kullanıcı tanımlı değişkenlere bağlı olur;

1. Bir Azure aboneliğine Bağlanma
2. Depolama hesabı oluşturma
3. VNet ve ağ yapılandırma dosyasında tanımlanan iki alt ağ oluşturma
4. Dört windows server Vm'lerinin oluşturma
5. NSG dahil olmak üzere yapılandırın:
   * Bir NSG oluşturma
   * Kuralları ile doldurma
   * NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bir bilgisayar veya sunucu, Internet'e yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu komut dosyasını çalıştırdığınızda, uyarı veya PowerShell'de pop diğer bilgilendirici iletileri olabilir. Yalnızca hata iletileri kırmızı sorunu nedeni edilir.
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on the FrontEnd Subnet
   - Three Servers on the BackEnd Subnet
   - Network Security Groups to allow/deny traffic patterns as declared

  Before running script, ensure the network configuration file is created in
  the directory referenced by $NetworkConfigFile variable (or update the
  variable to reflect the path and file name of the config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  the appropriate layer(s) of protection. This script serves as an example of some
  of the techniques that can be used, but should not be used for all scenarios. You
  are responsible to assess your security needs and the appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

  BackEnd Service (BackEnd subnet 10.0.2.0/24)
   DNS01      - 10.0.2.4
   AppVM01    - 10.0.2.5
   AppVM02    - 10.0.2.6

#>

# Fixed Variables
    $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
    $VMName = @()
    $ServiceName = @()
    $VMFamily = @()
    $img = @()
    $size = @()
    $SubnetName = @()
    $VMIP = @()

# User-Defined Global Variables
  # These should be changes to reflect your subscription and services
  # Invalid options will fail in the validation section

  # Subscription Access Details
    $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

  # VM Account, Location, and Storage Details
    $LocalAdmin = "theAdmin"
    $DeploymentLocation = "Central US"
    $StorageAccountName = "vmstore02"

  # Service Details
    $FrontEndService = "FrontEnd001"
    $BackEndService = "BackEnd001"

  # Network Details
    $VNetName = "CorpNetwork"
    $FESubnet = "FrontEnd"
    $FEPrefix = "10.0.1.0/24"
    $BESubnet = "BackEnd"
    $BEPrefix = "10.0.2.0/24"
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: To ensure proper NSG Rule creation later in this script:
    #       - The Web Server must be VM 0
    #       - The AppVM1 Server must be VM 1
    #       - The DNS server must be VM 3
    #
    #       Otherwise the NSG rules in the last section of this
    #       script will need to be changed to match the modified
    #       VM array numbers ($i) so the NSG Rule IP addresses
    #       are aligned to the associated VM IP addresses.

    # VM 0 - The Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - The First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - The Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - The DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
        Return}
    Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
          New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

  # Update Subscription Pointer to New Storage Account
    Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
    Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

# Validation
$FatalError = $false

If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
     Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
     $FatalError = $true}

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "The network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation variable is correct and the network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see the above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}

# Create VNET
    Write-Host "Creating VNET" -ForegroundColor Cyan 
    Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

# Create Services
    Write-Host "Creating Services" -ForegroundColor Cyan
    New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
    New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

# Build VMs
    $i=0
    $VMName | Foreach {
        Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan

  # Build the NSG
    Write-Host "Building the NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign the NSG to the Subnets
        Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on the IIS Server)
  # Install Backend resource (Run Post-Build Script on the AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a>Ağ yapılandırma dosyası
Güncelleştirilmiş konumu ile bu xml dosyasını kaydedin ve bağlantıyı önceki komut $NetworkConfigFile değişkeninde bu dosyaya ekleyin.

```XML
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="DNS01" IPAddress="10.0.2.4" />
        <DnsServer name="Level3" IPAddress="209.244.0.3" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="CorpNetwork" Location="Central US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="FrontEnd">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="BackEnd">
            <AddressPrefix>10.0.2.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
        <DnsServersRef>
          <DnsServerRef name="DNS01" />
          <DnsServerRef name="Level3" />
        </DnsServersRef>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

#### <a name="sample-application-scripts"></a>Örnek uygulama komut dosyaları
Bu ve diğer çevre örnekleri için örnek bir uygulama yüklemek isterseniz, bir aşağıdaki bağlantıda sağlanmış: [örnek uygulama betiği][SampleApp]

## <a name="next-steps"></a>Sonraki adımlar
* Güncelleştirme ve XML dosyasını kaydedin
* Ortamı oluşturmak için PowerShell betiğini çalıştırın
* Örnek uygulamayı yükle
* Bu DMZ aracılığıyla farklı trafik akışları test etme

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "NSG ile giriş DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

