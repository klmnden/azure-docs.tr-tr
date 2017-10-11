## <a name="public-ip-address"></a>Genel IP adresi
Ya da IP adresi bir ayrılmış veya dinamik Internet'e yönelik genel bir IP adresi kaynağı sağlar. Tek başına nesne olarak genel bir IP adresi oluşturabilirsiniz, ancak gerçekte adresini kullanmak için başka bir nesneye ilişkilendirmeniz gerekir. Bir ortak IP adresi bir yük dengeleyici, uygulama ağ geçidi ya da bu kaynakları Internet erişimi sağlamak için bir NIC ilişkilendirebilirsiniz.  

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **Publicıpallocationmethod** |IP adresi ise tanımlar *statik* veya *dinamik*. |statik, dinamik |
| **idleTimeoutInMinutes** |4 dakikalık varsayılan bir değerle boşta kalma zaman aşımı tanımlar. Belirli bir oturum için daha fazla hiçbir paket alındığında bu süre içinde oturum sonlandırılır. |4 ile 30 arasında herhangi bir değer |
| **IP adresi** |Nesne için atanan IP adresi. Bu salt okunur bir özelliktir. |104.42.233.77 |

### <a name="dns-settings"></a>DNS ayarları
Genel IP adresine sahip adlı bir alt nesne **dnsSettings** aşağıdaki özellikleri içeren:

| Özellik | Açıklama | Örnek değerler |
| --- | --- | --- |
| **domainNameLabel** |Adlı ana bilgisayar adı çözümlemesi için kullanılır. |www, ftp, vm1 |
| **FQDN** |Genel IP için tam adı. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |IP adresine çözümler ve DNS PTR kaydı olarak kaydedilmiş tam etki alanı adı. |www.contoso.com. |

Örnek genel IP adresi JSON biçiminde:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Ek kaynaklar
* Hakkında daha fazla bilgi almak [genel IP adresleri](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* Hakkında bilgi edinin [örnek düzeyinde ortak IP adresleri](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Okuma [REST API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt163638.aspx) için ortak IP adresleri.

