## <a name="azure-dns"></a>Azure DNS
Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir.

| Özellik | Açıklama | Örnek değer |
| --- | --- | --- |
| **DNSzones** |Belirli bir etki alanı ana bilgisayar DNS kayıtları etki alanı bölgesi bilgileri |/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com " |

### <a name="dns-record-sets"></a>DNS kayıt kümeleri
DNS bölgeleri kayıt kümesi adında bir alt nesne vardır. Kayıt kümeleri, ana bilgisayar kayıtları bir DNS bölgesi için türüne göre koleksiyonudur. Kayıt türleri: A, AAAA, CNAME, MX, NS, SOA, SRV ve TXT.

| Özellik | Açıklama | Örnek değer |
| --- | --- | --- |
| A |IPv4 kayıt türü |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/A/www |
| AAAA |IPv6 kayıt türü |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord |
| CNAME |kurallı ad kayıt türü <sup>1</sup> |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/CNAME/www |
| MX |posta kayıt türü |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/MX/Mail |
| NS |ad sunucusu kaydı türü |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/NS/ |
| SOA |Başlangıç yetkilisi kayıt türünün <sup>2</sup> |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SOA |
| SRV |Hizmet kayıt türü |/Subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com/SRV |

<sup>1</sup> yalnızca kayıt kümesi başına bir değer verir.

<sup>2</sup> yalnızca bir kayıt türü SOA DNS bölgesi başına izin verir. 

Json biçimindeki DNS bölgesini örneği:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a>Ek kaynaklar
Okuma [DNS bölgeleri için REST API belgeleri ](https://msdn.microsoft.com/library/azure/mt130626.aspx) daha fazla bilgi için.

Okuma [DNS kayıt kümelerini için REST API belgeleri](https://msdn.microsoft.com/library/azure/mt130627.aspx) daha fazla bilgi için.

