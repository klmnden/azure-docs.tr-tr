---
title: Azure DMZ örnek – derleme Nsg'ler ile basit DMZ | Microsoft Docs
description: Ağ güvenlik grupları (NSG) ile bir çevre ağı oluşturma
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
ms.openlocfilehash: 115a459c6a9e4ea96931c89272a49396f0656258
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362224"
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>Örnek 1 – derleme Nsg'leri kullanarak Klasik PowerShell ile basit DMZ
[Güvenlik sınırı en iyi yöntemler sayfasına geri dönün][HOME]

> [!div class="op_single_selector"]
> * [Resource Manager Şablonu](virtual-networks-dmz-nsg.md)
> * [Klasik - PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Bu örnekte, dört Windows sunucu ve ağ güvenlik grupları ile basit DMZ oluşturur. Bu örnek, her adımda daha derin bir anlayış sağlamak için ilgili PowerShell komutların her birini açıklar. Ayrıca bir ayrıntılı sağlamak için bir trafik senaryo bölümü yoktur adım adım dmz'deki savunma katmanları ile nasıl trafiği devam eder. Son olarak, test ve deneme ile çeşitli senaryolar için bu ortamı oluşturmak için yönerge ve tam kod başvuruları bölüm yöneliktir. 

![Gelen NSG ile DMZ][1]

## <a name="environment-description"></a>Ortam tanımı
Bu örnekte, bir aboneliği aşağıdaki kaynaklar içerir:

* İki bulut Hizmetleri: "FrontEnd001" ve "BackEnd001"
* "CorpNetwork"; iki alt ağa sahip bir sanal ağ "FrontEnd" ve "Arka uç"
* Her iki alt ağa uygulanan ağ güvenlik grubu
* Uygulama bir web sunucusu ("IIS01") temsil eden bir Windows Server
* Uygulama arka uç sunucuları ("AppVM01", "AppVM02") temsil eden iki windows sunucuları
* Bir DNS sunucusu ("DNS01") temsil eden bir Windows server

Başvuru bölümünde, bu örnekte açıklanan ortam çoğunu oluşturan bir PowerShell Betiği yoktur. VM'ler ve sanal ağlar oluşturma örnek komut dosyası tarafından yapılır ancak değil açıklanmıştır ayrıntılı bu belgedeki. 

Ortamı oluşturmak için;

1. References bölümünde dahil ağ yapılandırma xml dosyasını kaydedin (adları, konum ve IP adresleri verilen senaryo eşleşecek şekilde güncelleştirilmiş)
2. Kullanıcı değişkenleri betiğinde betiğidir (abonelik, hizmet adlarını, vb. karşı) çalıştırılacak ortam eşleşecek şekilde güncelleştirin
3. PowerShell Betiği yürütün

>[!Note]
>PowerShell betik miktarlara bölge ağ yapılandırma xml dosyasında miktarlara bölge eşleşmelidir.
>
>

Başarıyla ek komut dosyası çalıştırıldıktan sonra isteğe bağlı adımlar alınmış olabilir, bu DMZ yapılandırma ile test izin vermek için basit bir web uygulaması ile uygulama sunucusu ve web sunucusu kurmak için iki komut dosyası başvuruları bölümünde bulunur.

Aşağıdaki bölümlerde, ağ güvenlik grupları ve nasıl önemli satırlarını PowerShell Betiği walking tarafından bu örnek için işlev ayrıntılı bir açıklama sağlayın.

## <a name="network-security-groups-nsg"></a>Ağ Güvenlik Grupları (NSG)
Bu örnekte, bir NSG grubu oluşturulan ve daha sonra altı kuralları ile yüklendi. 

> [!TIP]
> Genel olarak bakıldığında, belirli "İzin ver" kurallarınızı önce oluşturmanız ve ardından daha genel "Reddet" kuralları son. Kurallar öncelik atanmış belirtir, ilk değerlendirilir. Trafiği belirli bir kuralın uygulanacağı bulunduktan sonra başka hiçbir kural değerlendirilir. NSG kuralları, gelen veya giden yönde (alt ağ perspektifinden) ya da uygulayabilirsiniz.
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

İnternet'e trafik çıkış izin veren varsayılan bir giden kuralı yok. Bu örnekte, biz giden trafiğe izin vermek ve herhangi bir giden kuralı değiştirme değil. Her iki yönde trafik kilitlemek için kullanıcı tanımlı yönlendirme gereklidir ve üzerinde "Örnek 3" araştırılan [güvenlik sınırı en iyi yöntemler sayfası][HOME].

Her kural aşağıdaki gibi daha ayrıntılı olarak ele (**Not**: aşağıdaki başlayarak listenin bir dolar işareti ile herhangi bir öğeyi (örneğin: $NSGName) bir kullanıcı tanımlı değişken başvuru bölümünde, bu belgenin bir betikten):

1. İlk ağ güvenlik grubu kuralları tutacak oluşturulması gerekir:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. Bu örnekte ilk kural, tüm iç ağ DNS sunucusuna arka uç alt ağı arasında DNS trafiğine izin verir. Kural, bazı önemli parametrelere sahiptir:
   
   * "Type" trafik akışını hangi yönde bu kural etkinleşir gösterir. Perspektifinden (bağlı olarak bu NSG yere bağlıdır) alt ağı veya sanal makine yönüdür. Bu nedenle "Giriş" türüdür ve trafiğin alt girdiğinden, kural uygular ve alt ağdan çıkan trafiğe etkilenmemiştir bu kural tarafından varsa.
   * "Öncelik" bir trafik akışı değerlendirilme sırasını ayarlar. Düşük sayı öncelik o kadar yüksektir. Belirli bir trafik akışı için bir kural uygulandığı zaman başka hiçbir kural işlenir. Böylece öncelik 1 ile bir kuralı, trafiğine izin verir ve trafiği, öncelik 2 ile bir kural engelleyen ve her iki kural'ı trafiğe uygulamak durumunda trafik akışına izin verilecek (Kural 1 daha yüksek bir önceliğe sahip olduğundan etkisi sürdü ve başka hiçbir kural uygulandı).
   * Bu kural tarafından etkilenen trafiği engellendi veya izin, "Action" belirtir.

     ```PowerShell    
     Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
     ```

3. Bu kural, herhangi bir sunucuda ilişkili alt ağ üzerinde RDP noktasına internet'ten akışı RDP trafiğine izin verir. Bu kural, adres ön ekleri iki özel tür kullanır; "Vırtual_network" ve "INTERNET." Bu etiketler, daha büyük bir kategorisini adres önekleri belirtmek için kolay bir yoludur.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Bu kural, web sunucusu isabet gelen internet trafiğini sağlar. Bu kural, yönlendirme davranışını değiştirmez. Kuralın yalnızca geçirmek için IIS01 hedefleyen trafiğe izin verir. Bu nedenle trafik Internet'ten bu kural izin ve daha fazla kural işlemeyi durdur hedef olarak web sunucusuna sahip. (Kuralda öncelikte 140 tüm diğer gelen internet trafiğini engellendi). Yalnızca HTTP trafiğini işlemek, yalnızca hedef 80 numaralı bağlantı noktasına izin vermek için bu kural daha kısıtlanmasını.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Bu kural, arka uç trafiği diğer ön uç AppVM01 sunucuya daha yeni bir kural bloklarını IIS01 sunucudan geçirilecek trafiğine izin verir. Bu kural, bağlantı noktasının eklenmesi gereken biliniyorsa geliştirmek için. IIS sunucusu yalnızca SQL Server üzerinde AppVM01 ulaşıyorsa, örneğin, hedef bağlantı noktası aralığı gelen değiştirilmelidir "*" (herhangi) için web uygulaması sürekli görmüş AppVM01 daha küçük bir gelen saldırı yüzeyinde böylece izin 1433'ü (SQL bağlantı noktası).

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Bu kural, ağ üzerindeki herhangi bir sunucuya internet'ten trafiği engeller. Öncelikle 110 ve 120 kurallarıyla etkisi yalnızca gelen internet trafiğini güvenlik duvarı ve RDP bağlantı noktaları sunucuları ve blokları için her şey izin vermektir. Bu kural, tüm beklenmeyen akışlar engellemek için bir "yedek" kuralıdır.
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
7. Son kuralı trafiği arka uç alt ağına ön uç alt ağından reddeder. Bu kural yalnızca bir gelen kuralı olduğundan, dönen trafik (alanından, ön uç için arka uç) izin verilir.

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
#### <a name="allowed-internet-to-web-server"></a>(*İzin verilen*) web sunucusuna Internet
1. İnternet kullanıcı FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) istekler bir HTTP sayfası
2. Bulut hizmet IIS01 doğrultusunda 80 numaralı bağlantı noktasında açık uç noktası aracılığıyla geçiş trafiği (web sunucusu)
3. Ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet'e IIS01) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafiği iç IP adresi (10.0.1.5'i) web sunucusunun IIS01 denk gelir.
5. IIS01 web trafiği, bu isteği alır ve isteği işlemeye başlar.
6. IIS01 SQL Server AppVM01 hakkında bilgi için sorar.
7. Ön uç alt ağda hiçbir giden kuralları olduğundan, trafiğe izin verilir
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
12. IIS sunucusu SQL yanıtı alır ve HTTP yanıtı tamamlandıktan ve istek sahibine gönderir
13. Giden kural olduğundan ön uç alt ağda yanıt izin verilir ve web sayfasının kullanıcı internet alır.

#### <a name="allowed-rdp-to-backend"></a>(*İzin verilen*) RDP için arka uç
1. İnternet üzerinde Sunucu Yöneticisi AppVM01 xxxxx RDP AppVM01 (atanan bağlantı noktası, Azure portalında veya PowerShell aracılığıyla bulunabilir) için rastgele atanan bağlantı noktası numarasını olduğu BackEnd001.CloudApp.Net:xxxxx üzerinde RDP oturumu isteği
2. Arka uç alt ağı gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) geçerli, trafiğin izin verilen, Dur kural işleme
3. Hiçbir giden kuralları, varsayılan kuralları uygulanır ve dönüş trafiğine izin verilir
4. RDP oturumu etkin
5. AppVM01 kullanıcı adı ve parola bilgilerini ister.

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

#### <a name="denied-web-to-backend-server"></a>(*Reddedildi*) arka uç sunucusuna Web
1. Bir Internet kullanıcı BackEnd001.CloudApp.Net hizmet aracılığıyla AppVM01 bir dosyaya erişmeye çalışır
2. Dosya Paylaşımı için açık uç nokta olduğundan bu trafiği bulut hizmeti geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. NSG kuralı 5 (Internet'e VNet) uç noktaları için herhangi bir nedenle açıksa, bu trafiği engeller

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Reddedildi*) DNS sunucusundaki Web DNS Arama
1. DNS01 BackEnd001.CloudApp.Net hizmeti aracılığıyla bir iç DNS kaydını aramak bir internet kullanıcı çalışır
2. DNS için açık uç nokta olduğundan bu trafiği bulut hizmeti geçecekse değil ve tarafından sunucuya ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, bu trafiğin NSG kuralı 5 (Internet'e VNet) engellenecekse (Not: Bu kural 1'in (DNS), iki nedenden dolayı uygulanamaz ilk kaynak adresi internet, bu kural yerel sanal ağ kaynağı olarak yalnızca uygular. hiçbir zaman trafiği reddetmeye Ayrıca bu kural bir izin verme kuralı olduğundan)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Reddedildi*) Web güvenlik duvarı üzerinden SQL erişimi
1. İnternet kullanıcı SQL veri FrontEnd001.CloudApp.Net (Internet'e yönelik bulut hizmeti) istekleri
2. SQL için açık uç nokta olduğundan bu trafiği bulut hizmeti geçecekse değil ve güvenlik duvarı ulaşmak mıydı
3. Uç noktaları için herhangi bir nedenle açıksa, ön uç alt ağını gelen kuralı işleme başlar:
   1. NSG kuralı 1 (DNS) değil, uygulamak için sonraki kural taşıma
   2. NSG kuralı 2 (RDP) değil, uygulamak için sonraki kural taşıma
   3. NSG kuralı 3 (Internet'e IIS01) geçerli, trafiğin izin verilen, Dur kural işleme
4. Trafik İsabetleri IIS01 iç IP adresi (10.0.1.5'i)
5. IIS01 dinleme bağlantı noktası 1433'ü, dolayısıyla isteğine yanıt olarak değil

## <a name="conclusion"></a>Sonuç
Bu örnekte, arka uç alt ağından gelen trafiği yalıtma, görece basit ve anlaşılır bir yoludur.

Daha fazla örnek ve ağ güvenlik sınırları genel bir bakış bulunabilir [burada][HOME].

## <a name="references"></a>Başvurular
### <a name="main-script-and-network-config"></a>Ana komut dosyası ve ağ yapılandırma
Bir PowerShell komut dosyasında tam komut dosyasını kaydedin. Ağ Yapılandırması "NetworkConf1.xml." adlı bir dosyaya kaydedin
Kullanıcı tanımlı değişkenleri gerektiği gibi değiştirin ve betiği çalıştırın.

#### <a name="full-script"></a>Tam betik
Bu betik, kullanıcı tarafından tanımlanan değişkenleri esas alarak olur;

1. Bir Azure aboneliğine Bağlanma
2. Depolama hesabı oluşturma
3. Bir sanal ağ ve ağ yapılandırma dosyasında tanımlanan iki alt ağ oluşturma
4. Dört windows server sanal makineleri oluşturma
5. NSG dahil olmak üzere yapılandırın:
   * Bir NSG oluşturma
   * Kuralları ile doldurma
   * NSG için uygun alt ağları bağlama

Bu PowerShell Betiği, bilgisayar veya sunucu bir İnternet'e bağlı yerel olarak çalıştırılmalıdır.

> [!IMPORTANT]
> Bu betik çalıştırıldığında, uyarı veya PowerShell'de pop diğer bilgilendirme iletileri olabilir. Yalnızca hata iletileri kırmızı endişeye neden olan.
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
Güncelleştirilmiş konumu ile bu xml dosyasını kaydedin ve önceki komut $NetworkConfigFile değişkeninde bu dosyaya bağlantı ekleyin.

```XML
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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
Bu ve diğer DMZ örnekleri için örnek uygulamayı yüklemek istiyorsanız, aşağıdaki bağlantıda bir sağlanmıştır: [Örnek uygulama betiği][SampleApp]

## <a name="next-steps"></a>Sonraki adımlar
* XML dosyasını kaydedin ve güncelleştirin
* Ortamı oluşturmak için PowerShell betiğini çalıştırma
* Örnek uygulamayı yüklemek
* Bu DMZ ile farklı trafik akışları test etme

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Gelen NSG ile DMZ"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

