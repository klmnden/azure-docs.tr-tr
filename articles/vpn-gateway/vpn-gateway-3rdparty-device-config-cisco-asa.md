---
title: "Cisco ASA cihazların Azure VPN ağ geçitleri için bağlamak için örnek yapılandırma | Microsoft Docs"
description: "Bu makale Azure VPN ağ geçitleri için Cisco ASA cihazları bağlamak için örnek bir yapılandırma sağlar."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: fbe22b70b4fe3463ffc7b0e9a7ebd683f681117d
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Örnek Yapılandırması: Cisco ASA aygıt (Ikev2/Hayır BGP)
Bu makale Azure VPN ağ geçitleri için bağlantı Cisco Uyarlamalı güvenlik Gereci (ASA) cihazlar için örnek yapılandırmalarını sağlar. Örnek, Ikev2 sınır ağ geçidi Protokolü (BGP) olmadan çalıştıran Cisco ASA cihazlar için geçerlidir. 

## <a name="device-at-a-glance"></a>Bir bakışta cihaz

|                        |                                   |
| ---                    | ---                               |
| Cihaz satıcısından          | Cisco                             |
| Cihaz modeli           | ASA                               |
| Hedef sürüm         | 8.4 ve üzeri                     |
| Sınanan modeli           | ASA 5505                          |
| Test edilen sürüm         | 9.2                               |
| IKE sürümü            | IKEv2                             |
| BGP                    | Hayır                                |
| Azure VPN ağ geçidi türü | Rota tabanlı VPN ağ geçidi           |
|                        |                                   |

> [!NOTE]
> Örnek bir yapılandırma bir Azure için Cisco ASA bağlanmasındakiyle **rota tabanlı** VPN ağ geçidi. Özel bir IPSec/IKE ilke ile bağlantı kullanıyor **UsePolicyBasedTrafficSelectors** açıklandığı gibi seçeneği [bu makalede](vpn-gateway-connect-multiple-policybased-rm-ps.md).
>
> ASA cihazları kullanan örnek gerektirir **Ikev2** erişim listesi temel yapılandırmalarla İlkesi VTI tabanlı değil. Doğrulamak için VPN cihaz Satıcı belirtimleri İlkesi şirket içi VPN cihazlarınızı üzerinde desteklenen Ikev2 başvurun.


## <a name="vpn-device-requirements"></a>VPN cihaz gereksinimleri
Azure VPN ağ geçitleri standart IPSec/IKE protokolü paketleri siteden siteye (S2S) VPN tünelleri oluşturmak için kullanın. Ayrıntılı IPSec/IKE protokolü parametreleri ve Azure VPN ağ geçitleri için varsayılan şifreleme algoritmaları için bkz: [VPN cihazları hakkında](vpn-gateway-about-vpn-devices.md).

> [!NOTE]
> Bölümünde açıklandığı gibi isteğe bağlı olarak şifreleme algoritmaları ve anahtar gücü belirli bir bağlantı için tam bir birleşimini belirtebilirsiniz [şifreleme gereksinimleri hakkında](vpn-gateway-about-compliance-crypto.md). Algoritmaları ve anahtar gücü tam bir birleşimini belirtirseniz, karşılık gelen belirtimleri VPN aygıtlarınızda kullandığınızdan emin olun.

## <a name="single-vpn-tunnel"></a>Tek VPN tüneli
Bu yapılandırma bir Azure VPN ağ geçidi ve bir şirket içi VPN cihazı arasında tek bir S2S VPN tüneli oluşur. İsteğe bağlı olarak yapılandırabilirsiniz [VPN tüneli üzerinden BGP](#bgp).

![Tek S2S VPN tüneli](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Azure yapılandırmalarını oluşturmak adım adım yönergeler için bkz: [tek VPN tüneli Kurulum](vpn-gateway-3rdparty-device-config-overview.md#singletunnel).

### <a name="virtual-network-and-vpn-gateway-information"></a>Sanal ağ ve VPN ağ geçidi bilgileri
Bu bölümde örnek parametreleri listeler.

| **Parametre**                | **Değer**                    |
| ---                          | ---                          |
| Sanal ağ adres önekleri        | 10.11.0.0/16<br>10.12.0.0/16 |
| Azure VPN ağ geçidi IP         | Azure_Gateway_Public_IP      |
| Şirket içi adres önekleri | 10.51.0.0/16<br>10.52.0.0/16 |
| Şirket içi VPN cihazının IP    | OnPrem_Device_Public_IP     |
| * Sanal ağ BGP ASN                | 65010                        |
| * Azure BGP eşdeğer IP           | 10.12.255.30                 |
| * Şirket içi BGP ASN         | 65050                        |
| * Şirket içi BGP eşdeğer IP     | 10.52.255.254                |
|                              |                              |

\*İsteğe bağlı parametresi BGP yalnızca.

### <a name="ipsecike-policy-and-parameters"></a>IPSec/IKE İlkesi ve parametreleri
Aşağıdaki tabloda IPSec/IKE algoritmalar ve örnekte kullanılan parametreleri listeler. VPN cihaz modelleri ve bellenim sürümleri için desteklenen algoritmalar doğrulamak için VPN aygıt belirtimlerine bakın.

| **IPsec/IKEv2**  | **Değer**                            |
| ---              | ---                                  |
| IKEv2 Şifrelemesi | AES256                               |
| IKEv2 Bütünlüğü  | SHA384                               |
| DH Grubu         | DHGroup24                            |
| * IPSec şifreleme | AES256                               |
| * IPSec bütünlük  | SHA1                                 |
| PFS Grubu        | PFS24                                |
| QM SA Yaşam Süresi   | 7200 saniye                         |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors $True |
| Önceden Paylaşılan Anahtar   | PreSharedKey                         |
|                  |                                      |

\*IPSec şifreleme algoritması AES-GCM olduğunda bazı cihazlarda IPSec bütünlüğünü boş bir değer olmalıdır.

### <a name="asa-device-support"></a>ASA cihaz desteği

* Ikev2 desteği ASA 8.4 ve sonraki sürümleri gerektirir.

* DH grubu ve grubun 5 ötesinde PFS grubu için destek gerektiren ASA sürüm 9.x.

* AES-GCM ile IPSec şifreleme ve SHA-256, SHA-384 ve SHA-512 ile IPSec bütünlük destek, ASA sürümünü gerektirir 9.x. Bu destek gereksinim yeni ASA cihazları için geçerlidir. Yayın zaman ASA modelleri 5505, 5510, 5520, 5540, 5550 ve 5580 Bu algoritmalar desteklemez. VPN cihaz modelleri ve bellenim sürümleri için desteklenen algoritmalar doğrulamak için VPN aygıt belirtimlerine bakın.


### <a name="sample-device-configuration"></a>Örnek aygıt yapılandırması
Betik önceki bölümlerde açıklanan parametreleri ve yapılandırmasını temel alan bir örnek sağlar. S2S VPN tüneli yapılandırma aşağıdaki bölümlerden oluşur:

1. Arabirimleri ve yolları
2. Erişim listeleri
3. IKE İlkesi ve parametreleri (1. Aşama ya da ana mod)
4. IPSec ilkesi ve parametreleri (2. Aşama ya da hızlı mod)
5. TCP MSS clamping gibi diğer parametreleri

> [!IMPORTANT]
> Örnek komut dosyasını kullanmadan önce aşağıdaki adımları tamamlayın. Komut dosyasında yer tutucu değerlerini yapılandırmanızı aygıt ayarlarını değiştirin.

* Her ikisi içinde ve dışında arabirimleri için arabirimi yapılandırması belirtin.
* İç/özel ve dış/genel ağlar için rotaları tanımlayın.
* Tüm adlarını ve ilke numaralarını aygıtınızda benzersiz olduğundan emin olun.
* Şifreleme algoritmaları aygıtınızda desteklendiğinden emin olun.
* Aşağıdaki değiştirin **yer tutucu değerlerini** yapılandırmanızı için gerçek değerlerle:
  - Arabirim adı dışında: **dışında**
  - **Azure_Gateway_Public_IP**
  - **OnPrem_Device_Public_IP**
  - IKE: **Pre_Shared_Key**
  - Sanal ağ ve yerel ağ geçidi adları: **vnetname adlı** ve **LNGName**
  - Sanal ağ ve şirket içi ağ adresi **önekleri**
  - Uygun **ağ maskeleri**

#### <a name="sample-script"></a>Örnek komut dosyası

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Basit hata ayıklama komutları

Hata ayıklama amacıyla aşağıdaki ASA komutları kullanın:

* IPSec veya IKE güvenlik ilişkisi (SA) göster:
    ```
    show crypto ipsec sa
    show crypto ikev2 sa
    ```

* Hata ayıklama modu girin:
    ```
    debug crypto ikev2 platform <level>
    debug crypto ikev2 protocol <level>
    ```
    `debug` Komutları konsolunda önemli çıkış üretebilir.

* Geçerli yapılandırmaları cihazda göster:
    ```
    show run
    ```
    Kullanım `show` listesi belirli bölümlerine aygıt yapılandırması örneğin subcommands:
    ```
    show run crypto
    show run access-list
    show run tunnel-group
    ```

## <a name="next-steps"></a>Sonraki adımlar
Etkin-Etkin şirket içi ve VNet-VNet bağlantıları yapılandırmak için bkz: [etkin-etkin VPN ağ geçitlerini yapılandırmak](vpn-gateway-activeactive-rm-powershell.md).
