<a id="is-custom-ipsecike-policy-supported-on-all-azure-vpn-gateway-skus" class="xliff"></a>

### Özel IPsec/IKE ilkesi tüm Azure VPN Gateway SKU’larında desteklenir mi?
Özel IPsec/IKE ilkesi, Azure **VpnGw1, VpnGw2, VpnGw3, Standard** ve **HighPerformance** VPN ağ geçitlerinde desteklenir. **Temel** SKU DESTEKLENMEZ.

<a id="how-many-policies-can-i-specify-on-a-connection" class="xliff"></a>

### Bir bağlantıda kaç tane ilke belirtebilirim?
Belirli bir bağlantı için yalnızca ***bir*** ilke birleşimi belirtebilirsiniz.

<a id="can-i-specify-a-partial-policy-on-a-connection-eg-only-ike-algorithms-but-not-ipsec" class="xliff"></a>

### Bir bağlantıda kısmi bir ilke belirtebilir miyim? (örneğin, IPsec olmadan yalnızca IKE algoritmaları)
Hayır, hem IKE (Ana Mod) hem de IPsec (Hızlı Mod) için tüm algoritmaları ve parametreleri belirtmeniz gerekir. Kısmi ilke belirtimine izin verilmez.

<a id="what-are-the-algorithms-and-key-strengths-supported-in-the-custom-policy" class="xliff"></a>

### Özel ilkede desteklenen algoritmalar ve anahtar güçleri nelerdir?
Aşağıdaki tabloda, müşteriler tarafından yapılandırılabilecek şifreleme algoritmaları ve anahtar güçleri listelenmiştir. Her alan için bir seçeneği belirlemeniz gerekir.

| **IPsec/IKEv2**  | **Seçenekler**                                                                 |
| ---              | ---                                                                         |
| IKEv2 Şifrelemesi | AES256, AES192, AES128, DES3, DES                                           |
| IKEv2 Bütünlüğü  | SHA384, SHA256, SHA1, MD5                                                   |
| DH Grubu         | ECP384, ECP256, DHGroup24, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, Hiçbiri |
| IPsec Şifrelemesi | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, None    |
| IPsec Bütünlüğü  | GCMAES256, GCMAES192, GCMAES128, SHA256, SHA1, MD5                          |
| PFS Grubu        | ECP384, ECP256, PFS24, PFS2048, PFS14, PFS2, PFS1, Hiçbiri                     |
| QM SA Yaşam Süresi*  | Saniye (tamsayı; **en az 300**) ve KBytes (tamsayı; **en az 1024**)                                      |
| Trafik Seçicisi | UsePolicyBasedTrafficSelectors** ($True/$False; varsayılan $False)                             |
|                  |                                                                             |

* (*) Azure VPN ağ geçitlerinde IKEv2 Ana Modu SA yaşam süresi 28.800 saniye olarak sabitlenmiştir
* (**) "UsePolicyBasedTrafficSelectors" için lütfen bir sonraki SSS maddesine bakın

<a id="does-everything-need-to-match-between-the-azure-vpn-gateway-policy-and-my-on-premises-vpn-device-configurations" class="xliff"></a>

### Azure VPN ağ geçidi ilkesi ile şirket içi VPN cihazı yapılandırmalarım arasında her şey eşleşmeli mi?
Şirket içi VPN cihazı yapılandırmanızın Azure IPsec/IKE ilkesinde belirttiğiniz şu algoritmalar ve parametrelerle eşleşmesi ya da bunları içermesi gerekir:

* IKE şifreleme algoritması
* IKE bütünlük algoritması
* DH Grubu
* IPsec şifreleme algoritması
* IPsec bütünlük algoritması
* PFS Grubu
* Trafik Seçicisi (*)

SA yaşam süreleri yalnızca yerel belirtimlerdir ve bunların eşleşmesi gerekmez.

**UsePolicyBasedTrafficSelectors** ilkesini etkinleştirirseniz, VPN cihazınızın herhangi bir noktadan herhangi bir noktaya bağlantı yerine şirket içi ağınızın (yerel ağ geçidi) tüm ön ekleri ile Azure sanal ağ ön ekleri arasındaki tüm birleşimlerle tanımlanmış, eşleşen trafik seçicilerine sahip olduğundan emin olmanız gerekir. Örneğin, şirket içi ağınızın ön ekleri 10.1.0.0/16 ve 10.2.0.0/16; sanal ağınızın ön ekleriyse 192.168.0.0/16 ve 172.16.0.0/16 şeklindeyse, aşağıdaki trafik seçicileri belirtmeniz gerekir:
* 10.1.0.0/16 <====> 192.168.0.0/16
* 10.1.0.0/16 <====> 172.16.0.0/16
* 10.2.0.0/16 <====> 192.168.0.0/16
* 10.2.0.0/16 <====> 172.16.0.0/16

Bu seçeneğin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için [Birden çok şirket içi ilke tabanlı VPN cihazına bağlanma](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

<a id="does-the-custom-policy-replace-the-default-ipsecike-policy-sets-for-azure-vpn-gateways" class="xliff"></a>

### Özel ilke, Azure VPN ağ geçitleri için IPsec/IKE ilke kümelerinin yerini mi alır?
Evet, bir bağlantıda özel bir ilke belirtildiğinde VPN ağ geçidi, bağlantıda hem IKE başlatıcı hem de IKE yanıtlayıcısı olarak yalnızca bu ilkeyi kullanır.

<a id="if-i-remove-a-custom-ipsecike-policy-does-the-connection-become-unprotected" class="xliff"></a>

### Özel bir IPsec/IKE ilkesini kaldırırsam bağlantı korumasız hale mi gelir?
Hayır, bağlantı IPsec/IKE tarafından korunmaya devam eder. Bir bağlantıdan özel bir ilkeyi kaldırdığınızda, Azure VPN ağ geçidi [varsayılan IPsec/IKE teklifleri listesine](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) döner ve şirket içi VPN cihazınızla IKE el sıkışmasını yeniden başlatır.

<a id="would-adding-or-updating-an-ipsecike-policy-disrupt-my-vpn-connection" class="xliff"></a>

### Bir IPsec/IKE ilkesinin eklenmesi veya güncelleştirilmesi, VPN bağlantımı kesintiye uğratır mı?
Evet, Azure VPN ağ geçidi tarafından mevcut bağlantı yıkılıp yeni şifreleme algoritmaları ve parametrelerle IPsec tünelini yeniden kurmak üzere IKE el sıkışması yeniden başlatılırken kısa bir kesinti (birkaç saniye) gerçekleşebilir. Kesinti süresini mümkün olduğunca azaltmak için lütfen şirket içi VPN cihazınızın da eşleşen algoritmalar ve anahtar güçleriyle yapılandırıldığından emin olun.

<a id="can-i-use-different-policies-on-different-connections" class="xliff"></a>

### Farklı bağlantılarda farklı ilkeler kullanabilir miyim?
Evet. Özel ilkeler, bağlantı başına bir ilke temelinde uygulanır. Farklı bağlantılarda farklı IPsec/IKE ilkeleri oluşturabilir ve uygulayabilirsiniz. Bağlantıların bir alt kümesinde özel ilkeler uygulamayı da tercih edebilirsiniz. Geriye kalan bağlantılar, Azure’daki varsayılan IPsec/IKE ilke kümelerin kullanır.

<a id="can-i-use-the-custom-policy-on-vnet-to-vnet-connection-as-well" class="xliff"></a>

### Özel ilkeyi VNet-VNet bağlantısında da kullanabilir miyim?
Evet, hem IPsec şirket içi ve dışı karışık bağlantılarda hem de Vnet-Vnet bağlantılarında özel ilke uygulayabilirsiniz.

<a id="do-i-need-to-specify-the-same-policy-on-both-vnet-to-vnet-connection-resources" class="xliff"></a>

### Her iki VNet-VNet bağlantı kaynağında aynı ilkeyi belirtmem mi gerekir?
Evet. VNet-VNet tüneli, her iki yön için birer tane olmak üzere Azure’daki iki bağlantı kaynağından oluşur. Her iki bağlantı kaynağının da aynı ilkeye sahip olduğundan emin olmanız gerekir, aksi takdirde VNet-VNet bağlantısı kurulamaz.

<a id="does-custom-ipsecike-policy-work-on-expressroute-connection" class="xliff"></a>

### Özel IPsec/IKE ilkesi ExpressRoute bağlantısında çalışır mı?
Hayır. IPsec/IKE ilkesi yalnızca Azure VPN ağ geçitleri aracılığıyla kurulan S2S VPN ve VNet-VNet bağlantılarında çalışır.
