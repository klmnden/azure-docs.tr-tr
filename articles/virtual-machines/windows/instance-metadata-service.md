---
title: Azure örnek meta veri hizmeti | Microsoft Docs
description: Windows sanal makinenin işlem, ağ ve yaklaşan bakımın olaylar hakkında bilgi almak için rESTful arabirimi.
services: virtual-machines-windows
documentationcenter: ''
author: KumariSupriya
manager: harijayms
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: sukumari
ms.reviewer: azmetadata
ms.openlocfilehash: 9097fef88a2c3c667416761c341a2e320c790121
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64919048"
---
# <a name="azure-instance-metadata-service"></a>Azure örnek meta veri hizmeti

Azure örnek meta veri hizmeti yönetmek ve sanal makinelerinizi yapılandırmak için kullanılan sanal makine örneklerini çalıştırma hakkında bilgi sağlar.
Bu SKU, ağ yapılandırması ve yaklaşan Bakımı olayları gibi bilgileri içerir. Hangi tür bilgileri kullanılabilir daha fazla bilgi için bkz: [API meta verileri](#metadata-apis).

Azure'nın örnek meta veri hizmeti REST uç noktası aracılığıyla oluşturulan tüm Iaas Vm'leri için erişilebilir olan [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/).
İyi bilinen yönlendirilemeyen IP adresinde uç noktası kullanılabilir (`169.254.169.254`), erişilebilir yalnızca VM içinden.

> [!IMPORTANT]
> Bu hizmet **sunuldu** tüm Azure bölgelerinde.  Düzenli olarak, sanal makine örnekleri hakkında yeni bilgiler kullanıma sunmak için güncelleştirmeleri alır. Bu sayfa güncel yansıtır [API meta verileri](#metadata-apis) kullanılabilir.

## <a name="service-availability"></a>Hizmet kullanılabilirliği

Hizmet genel kullanıma sunulan Azure bölgelerinde kullanılabilir. Tüm API sürümü tüm Azure bölgelerinde kullanılabilir.

Bölgeler                                        | Kullanılabilirlik?                                 | Desteklenen Sürümler
-----------------------------------------------|-----------------------------------------------|-----------------
[Tüm genel kullanıma sunulan Global Azure bölgeleri](https://azure.microsoft.com/regions/)     | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Çin](https://www.azure.cn/)                                                     | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01
[Genel Batı Orta ABD](https://azure.microsoft.com/regions/)                           | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01

Bu tablo, hizmet güncelleştirmeleri vardır ve ne zaman veya yeni desteklenen sürümler kullanılabilir güncelleştirilir.

> [!NOTE]
> 2019-02-01, şu anda kullanıma ve başka bölgelerde kısa bir süre sonra kullanıma sunulacaktır.

Örnek meta veri hizmeti deneyebilmesi için bir VM oluşturma [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) veya [Azure portalında](https://portal.azure.com) yukarıdaki bölgelerde ve aşağıdaki örneklerde izleyin.

## <a name="usage"></a>Kullanım

### <a name="versioning"></a>Sürüm oluşturma

Örnek meta veri hizmeti sürümü. Sürümleri zorunludur ve genel Azure üzerinde geçerli sürümü `2018-10-01`. Geçerli desteklenen (2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01) sürümleridir.

Daha yeni sürümler eklendikçe betiklerinizi özgü veri biçimleri üzerinde bağımlılıkları varsa, eski sürümleri yine de uyumluluk için erişilebilir.

Hiçbir sürüm belirtildiğinde, bir hata yeni Desteklenen sürümlerin bir listesi döndürülür.

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance"
```

**Yanıt**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

### <a name="using-headers"></a>Üst bilgileri kullanma

Örnek meta veri hizmeti sorguladığınızda, başlık sağlamalısınız `Metadata: true` istek istemeden yönlendirilmeyen emin olmak için.

### <a name="retrieving-metadata"></a>Meta veri alma

Örnek meta veri oluşturulan/yönetilen diskleri kullanarak VM'ler çalıştırmak için kullanılabilir [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Aşağıdaki isteği kullanarak bir sanal makine örneği için tüm veri kategorileri erişebilirsiniz:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

> [!NOTE]
> Tüm örnek meta veri sorguları büyük/küçük harfe duyarlıdır.

### <a name="data-output"></a>Veri çıkışı

Varsayılan olarak, örnek meta veri hizmeti verileri JSON biçiminde döndürür (`Content-Type: application/json`). Ancak, farklı bir API veri farklı biçimlerde istenmesi halinde döndürür.
Aşağıdaki tablo, diğer veri biçimlerini API'leri destekleyebilir bir başvurudur.

API | Varsayılan veri biçimi | Diğer biçimler
--------|---------------------|--------------
/instance | json | metin
/scheduledevents | json | yok
/ TPM'de | json | yok

Varsayılan olmayan yanıt biçimi erişmek için istenen biçimi isteğinde sorgu dizesi parametresi olarak belirtin. Örneğin:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> Yaprak düğümleri için `format=json` çalışmaz. Bu sorguları için `format=text` varsayılan biçimi json olup olmadığını açıkça belirtilmesi gerekir.

### <a name="security"></a>Güvenlik

Örnek meta veri Hizmeti uç noktası yalnızca yönlendirilemeyen bir IP adresi üzerinde çalışan sanal makine örneği içinde erişilebilir. Ayrıca, tüm ile istek bir `X-Forwarded-For` üst bilgi, hizmet tarafından reddedildi.
İstekleri de içermelidir bir `Metadata: true` başlığına gerçek isteği doğrudan amaçlayan ve yanlışlıkla yeniden yönlendirme bir parçası olduğundan emin olun.

### <a name="error"></a>Hata

Bir veri öğesi bulunamadı ya da hatalı biçimlendirilmiş isteği ise örnek meta veri hizmeti standart HTTP hataları döndürür. Örneğin:

HTTP durum kodu | Neden
----------------|-------
200 TAMAM |
400 Hatalı istek | Eksik `Metadata: true` üst bilgi veya bir yaprak düğüm sorgulanırken biçimi eksik
404 Bulunamadı | İstenen öğe yok
405 Yönteme izin verilmiyor | Yalnızca `GET` ve `POST` istekleri desteklenir
429 çok fazla istek | API şu anda en fazla saniye başına 5 sorguları destekler.
500 Hizmeti hatası     | Bir süre sonra yeniden deneyin

### <a name="examples"></a>Örnekler

> [!NOTE]
> Tüm API yanıtları JSON dizelerdir. Aşağıdaki örnek yanıtlar tüm okunabilirlik açısından pretty yazdırılır.

#### <a name="retrieving-network-information"></a>Ağ bilgileri alınıyor

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Genel IP adresi alınıyor

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Bir örneği için tüm meta verilerini alma

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2018-10-01"
```

**Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "jubilee",
    "offer": "Windows-10",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftWindowsDesktop",
    "resourceGroupName": "myrg",
    "sku": "rs4-pro",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Prod;Role:WorkerRole",
    "version": "17134.345.59",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_D1",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Windows sanal makine içindeki meta verilerini alma

**İstek**

Örnek meta veri Windows alınabilir `curl` programı:

```bash
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2018-10-01 | select -ExpandProperty Content
```

Aracılığıyla veya `Invoke-RestMethod` PowerShell cmdlet:

```powershell

Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2018-10-01 -Method get
```

**Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "compute": {
    "azEnvironment": "AZUREPUBLICCLOUD",
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "placementGroupId": "",
    "plan": {
        "name": "",
        "product": "",
        "publisher": ""
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "MicrosoftSQLServer",
    "resourceGroupName": "myrg",
    "sku": "Enterprise",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmScaleSetName": "",
    "vmSize": "Standard_DS2",
    "zone": ""
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="metadata-apis"></a>Meta veri API'leri

#### <a name="the-following-apis-are-available-through-the-metadata-endpoint"></a>Aşağıdaki API meta veri uç noktası aracılığıyla kullanılabilir:

Veriler | Açıklama | Kullanıma sunulan sürümü
-----|-------------|-----------------------
TPM'de | Bkz: [TPM'de veri](#attested-data) | 2018-10-01
identity | Azure kaynakları için yönetilen kimlikleri. Bkz: [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
örnek | Bkz: [API örneği](#instance-api) | 2017-04-02
scheduledevents | Bkz: [zamanlanmış olaylar](scheduled-events.md) | 2017-08-01

#### <a name="instance-api"></a>Örnek API
##### <a name="the-following-compute-categories-are-available-through-the-instance-api"></a>Aşağıdaki işlem kategorisinden örneği API aracılığıyla kullanılabilir:

> [!NOTE]
> Meta veri uç noktası Aşağıdaki kategorilerde örnek/işlem erişilir.

Veriler | Açıklama | Kullanıma sunulan sürümü
-----|-------------|-----------------------
azEnvironment | Azure ortamı burada VM çalışıyor | 2018-10-01
customData | Bkz: [özel veri](#custom-data) | 2019-02-01
location | Azure bölgesi VM çalışıyor | 2017-04-02
ad | VM adı | 2017-04-02
Teklif | VM görüntüsü için bilgi sağlar. Bu değer, yalnızca Azure görüntü Galerisi'nden dağıtılan görüntülerin bulunur. | 2017-04-02
osType | Linux veya Windows | 2017-04-02
placementGroupId | [Yerleştirme grubu](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) , sanal makine ölçek kümesi | 2017-08-01
planı | [Plan](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) bir VM için bir Azure Market görüntüsü adı, ürün ve yayımcı içerir | 2018-04-02
platformUpdateDomain |  [Güncelleme etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
platformFaultDomain | [Hata etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
sağlayıcı | Sanal makinenin sağlayıcısı | 2018-10-01
publicKeys | [Ortak anahtarlar koleksiyonunu](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#sshpublickey) VM ve yolları atanan | 2018-04-02
Yayımcı | VM görüntü yayımcısı | 2017-04-02
resourceGroupName | [Kaynak grubu](../../azure-resource-manager/resource-group-overview.md) sanal makineniz için | 2017-08-01
sku | Belirli SKU için VM görüntüsü | 2017-04-02
subscriptionId | Sanal makine için Azure aboneliği | 2017-08-01
etiketler | [Etiketleri](../../azure-resource-manager/resource-group-using-tags.md) sanal makineniz için  | 2017-08-01
version | VM görüntüsü sürümü | 2017-04-02
vmId | [Benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) VM için | 2017-04-02
vmScaleSetName | [Sanal makine ölçek kümesi adı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) , sanal makine ölçek kümesi | 2017-12-01
vmSize | [VM boyutu](sizes.md) | 2017-04-02
bölge | [Kullanılabilirlik alanı](../../availability-zones/az-overview.md) sanal makinenizin | 2017-12-01

##### <a name="the-following-network-categories-are-available-through-the-instance-api"></a>Aşağıdaki ağ kategorileri örneği API aracılığıyla kullanılabilir:

> [!NOTE]
> Meta veri uç noktası Aşağıdaki kategorilerde ağ/örnek/arabirimi üzerinden erişilir.

Veriler | Açıklama | Kullanıma sunulan sürümü
-----|-------------|-----------------------
ipv4/privateIpAddress | Sanal makinenin yerel IPv4 adresi | 2017-04-02
ipv4/publicIpAddress | Sanal makinenin genel IPv4 adresi | 2017-04-02
alt ağ/adresi | VM alt ağ adresi | 2017-04-02
alt ağ/ön eki | Alt ağ ön eki, örnek 24 | 2017-04-02
ipv6/ipAddress | Sanal makinenin yerel IPv6 adresi | 2017-04-02
macAddress | VM mac adresi | 2017-04-02

## <a name="attested-data"></a>TPM'de veri

Örnek meta veri 169.254.169.254 numaralı üzerinde http uç noktasında yanıt verir. Örnek meta veri hizmeti tarafından sunulan senaryonun parçası yanıt verileri Azure'dan geliyor garanti sağlamaktır. Market görüntülerini, Azure üzerinde çalışan kendi görüntüsü olduğundan emin olabilir, böylece Biz bu bilgileri bir parçası oturum açın.

### <a name="example-attested-data"></a>Örnek veri TPM'de

> [!NOTE]
> Tüm API yanıtları JSON dizelerdir. Aşağıdaki örnek yanıtlar okunabilirlik açısından pretty yazdırılır.

 **İstek**

 ```bash
curl -H Metadata:true "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"

```

API Sürüm zorunlu bir alan ve TPM'de verileri desteklenen sürümü 2018-10-01.
Nonce sağlanan bir isteğe bağlı 10 basamaklı dizedir. Nonce isteğin izlenmesi için kullanılabilir ve sağlanmazsa encoded yanıtı dize geçerli UTC zaman damgası döndürülür.

 **Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> İmza blobudur bir [pkcs7](https://aka.ms/pkcs7) belge sürümü imzalı. Oluşturma ve süre sonu belgenin ve görüntü ile ilgili plan bilgileri için VM ayrıntılarını Vmıd nonce, zaman damgası gibi imzalanması için kullanılan sertifika içeriyor. Plan bilgileri için Azure Market yerde görüntüleri yalnızca doldurulur. Sertifika gelen yanıt ayıklanır ve yanıt geçerli olduğunu ve Azure'dan gelen olduğunu doğrulamak için kullanılır.

#### <a name="retrieving-attested-metadata-in-windows-virtual-machine"></a>TPM'de meta verileri içinde Windows sanal makinesi alınıyor

 **İstek**

Örnek meta veri alınabilir Windows PowerShell yardımcı programı `curl`:

 ```bash
curl -H @{'Metadata'='true'} "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" | select -ExpandProperty Content
```

 Aracılığıyla veya `Invoke-RestMethod` cmdlet:

 ```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890" -Method get
```

API Sürüm zorunlu bir alan ve TPM'de verileri desteklenen sürümü 2018-10-01.
Nonce sağlanan bir isteğe bağlı 10 basamaklı dizedir. Nonce isteğin izlenmesi için kullanılabilir ve sağlanmazsa encoded yanıtı dize geçerli UTC zaman damgası döndürülür.

 **Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

 ```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

> İmza blobudur bir [pkcs7](https://aka.ms/pkcs7) belge sürümü imzalı. Oluşturma ve süre sonu belgenin ve görüntü ile ilgili plan bilgileri için VM ayrıntılarını Vmıd nonce, zaman damgası gibi imzalanması için kullanılan sertifika içeriyor. Plan bilgileri için Azure Market yerde görüntüleri yalnızca doldurulur. Sertifika gelen yanıt ayıklanır ve yanıt geçerli olduğunu ve Azure'dan gelen olduğunu doğrulamak için kullanılır.


## <a name="example-scenarios-for-usage"></a>Kullanım için örnek senaryolar  

### <a name="tracking-vm-running-on-azure"></a>Azure üzerinde çalıştırılan VM'yi izleme

Bir hizmet sağlayıcısı olarak, yazılım çalışan VM'lerin sayısını izlemek veya VM benzersizliğini izlemek için gereken aracıları gerektirebilir. Bir VM için benzersiz bir kimliği getirebilmesi için kullanın `vmId` alanını örnek meta veri hizmeti.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Yanıt**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Hata/güncelleştirme etki alanı temelinde kapsayıcıların, veri bölümlerinin yerleşimi 

Belirli senaryolar farklı veri çoğaltmalarını yerleşimini prime çok önemlidir. Örneğin, [HDFS çoğaltma yerleştirme](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya kapsayıcı yerleştirme aracılığıyla bir [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) bilmek gerektirebilir `platformFaultDomain` ve `platformUpdateDomain` VM'nin çalışır durumda.
Ayrıca [kullanılabilirlik](../../availability-zones/az-overview.md) bu kararları vermek örnekleri için.
Bu veriler doğrudan örnek meta veri hizmeti aracılığıyla sorgulayabilirsiniz.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Yanıt**

```text
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Destek olayı sırasında VM hakkında daha fazla bilgi alma

Bir hizmet sağlayıcısı olarak, VM hakkında daha fazla bilgi bilmek istediğiniz bir destek çağrısı alabilirsiniz. İşlem meta veri paylaşmak için müşteri isteyen azure'da VM türü hakkında bilgi edinmek için profesyonel destek için temel bilgiler sağlayabilir. 

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-08-01"
```

**Yanıt**

> [!NOTE]
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="getting-azure-environment-where-the-vm-is-running"></a>VM'nin çalıştırıldığı Azure Ortamını alma

Azure, çeşitli bağımsız bulutlarda gibi sahiptir [Azure kamu](https://azure.microsoft.com/overview/clouds/government/). Bazı durumlarda, bazı çalışma zamanı kararlar almak için Azure ortamı gerekir. Aşağıdaki örnek, bu davranışı nasıl elde edebileceğiniz gösterir.

**İstek**
```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Yanıt**
```bash
AZUREPUBLICCLOUD
```

### <a name="getting-the-tags-for-the-vm"></a>VM için etiketler alınıyor

Etiketleri bir taksonomi mantıksal olarak düzenlemek için Azure VM için uygulanmış. Aşağıdaki isteği'ni kullanarak bir sanal makineye atanan etiketleri alınabilir.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Yanıt**

```text
Department:IT;Environment:Test;Role:WebRole
```

> [!NOTE]
> Etiketleri noktalı virgülle ayrılmış olan. Etiketleri program aracılığıyla ayıklamak için bir ayrıştırıcı yazılmışsa, etiket adları ve değerleri sırayla düzgün çalışması ayrıştırıcı için noktalı virgül içermemelidir.

### <a name="validating-that-the-vm-is-running-in-azure"></a>VM'nin Azure'da çalıştırıldığını doğrulama

Market satıcı, yazılım'ın yalnızca Azure'da çalıştırılmak üzere lisanslanır sağlamak istiyorsunuz. Birisi VHD'yi kopyalayan, şirket içi, ardından bunların olan algılama özelliğine sahip olmalıdır. Örnek meta veri hizmeti tarafından çağırma, Market satıcılar yalnızca azure'dan yanıt garanti eden imzalı veri alabilirsiniz.

> [!NOTE]
> Jq yüklü olmasını gerektirir.

**İstek**

 ```bash
  # Get the signature
   curl  --silent -H Metadata:True http://169.254.169.254/metadata/attested/document?api-version=2018-10-01 | jq -r '.["signature"]' > signature
  # Decode the signature
  base64 -d signature > decodedsignature
  #Get PKCS7 format
  openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
  # Get Public key out of pkc7
  openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
  #Get the intermediate certificate
  wget -q -O intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
  openssl x509 -inform der -in intermediate.cer -out intermediate.pem
  #Verify the contents
  openssl smime -verify -in sign.pk7 -inform pem -noverify
 ```

 **Yanıt**

```json
Verification successful
{"nonce":"20181128-001617",
  "plan":
    {
     "name":"",
     "product":"",
     "publisher":""
    },
"timeStamp":
  {
    "createdOn":"11/28/18 00:16:17 -0000",
    "expiresOn":"11/28/18 06:16:17 -0000"
  },
"vmId":"d3e0e374-fda6-4649-bbc9-7f20dc379f34"
}
```

Veriler | Açıklama
-----|------------
nonce | Kullanıcı tarafından sağlanan isteği ile isteğe bağlı dize. Hiçbir nonce istekte belirtilirse, geçerli UTC zaman damgası döndürülür
planı | [Plan](https://docs.microsoft.com/rest/api/compute/virtualmachines/createorupdate#plan) bir VM'de bir Azure Market görüntüsü için adı, ürün ve yayımcı içerir.
zaman damgası/createdOn | İlk imzalı belge oluşturulduğu zaman damgası
zaman damgası/expiresOn | İmzalı belge süresinin dolma zaman damgası
vmId |  [Benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) VM için

#### <a name="verifying-the-signature"></a>İmza doğrulama

Yukarıdaki imza aldıktan sonra imza Microsoft'tan olduğunu doğrulayabilirsiniz. Ara Sertifika ve sertifika zincirini da doğrulayabilirsiniz.

> [!NOTE]
> Sertifika genel Bulut ve bağımsız bulut için farklı olacaktır.

 Bölgeler | Sertifika
---------|-----------------
[Tüm genel kullanıma sunulan Global Azure bölgeleri](https://azure.microsoft.com/regions/)     | Metadata.Azure.com
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | metadata.azure.us
[Azure Çin](https://www.azure.cn/)                                                           | Metadata.Azure.CN
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | metadata.microsoftazure.de

```bash

# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

### <a name="failover-clustering-in-windows-server"></a>Windows Server Yük devretme

Belirli senaryolar, örnek meta veri hizmeti Yük Devretme Kümelemesi ile sorgulanırken bir yolu yönlendirme tablosuna eklemek gereklidir.

1. Yönetici ayrıcalıklarıyla bir komut istemi açın.

2. Aşağıdaki komutu çalıştırın ve bir hedef ağ arabiriminin adresini not alın (`0.0.0.0`) IPv4 yönlendirme tablosuna.

```bat
route print
```

> [!NOTE] 
> Aşağıdaki örnek çıktıda etkin yük devretme kümesi ile bir Windows Server VM'den yalnızca IPv4 için rota tablosu Basitlik içerir.

```bat
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         10.0.1.10    266
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

1. Aşağıdaki komutu çalıştırın ve için hedef ağ arabiriminin adresini kullanın (`0.0.0.0`) olduğu (`10.0.1.10`) Bu örnekte.

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

### <a name="custom-data"></a>Özel Veriler
Örnek meta veri hizmeti VM'nin özel veri erişim olanağı sağlar. İkili verileri 64 KB'den az olmalıdır ve VM'ye base64 olarak kodlanmış biçimde sağlanır. Özel veriler içeren bir VM oluşturma hakkında daha fazla ayrıntı için bkz. [CustomData ile bir sanal makine dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).

#### <a name="retrieving-custom-data-in-virtual-machine"></a>Sanal makinede özel veri alma
Örnek meta veri hizmeti özel verileri base64 olarak kodlanmış biçimde VM sağlar. Aşağıdaki örnek, base64 kodlamalı dizenin kodunu çözer.

> [!NOTE]
> Bu örnekte özel verileri okuyan, "Süper gizli verilerimi." bir ASCII dizesi olarak yorumlanır.

**İstek**

```bash
curl -H "Metadata:true" "http://169.254.169.254/metadata/instance/compute/customData?api-version=2019-02-01&&format=text" | base64 --decode
```

**Yanıt**

```text
My super secret data.
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>VM içindeki farklı dilleri kullanarak meta verileri hizmete çağrı yapma örnekleri 

Dil | Örnek
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Başlayın  | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Python   | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
C++      | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
Perl       | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
Java       | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
Visual Basic | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb
Puppet | https://github.com/keirans/azuremetadata

## <a name="faq"></a>SSS

1. Hata alıyorum `400 Bad Request, Required metadata header not specified`. Bu ne anlama geliyor?
   * Örnek meta veri hizmeti üst bilgisi gerektiren `Metadata: true` istekte geçirilecek. Bu üst bilginin REST çağrısında geçirme örnek meta veri hizmetine erişim sağlar.
2. Neden sanal Makinem için işlem bilgileri almıyorum?
   * Örnek meta veri hizmeti şu anda yalnızca Azure Resource Manager ile oluşturulan örnekleri destekler. Bulut hizmeti sanal makineleri eklenebilir gelecekte desteği.
3. Azure Resource Manager aracılığıyla sanal makineme geri bir süre oluşturdum. Bkz: değil neden ben meta veri bilgilerini işlem?
   * Eylül 2016'dan sonra oluşturulan tüm vm'leri için bir [etiketi](../../azure-resource-manager/resource-group-using-tags.md) görmek için işlem meta verileri. Eski Vm'leri (Eylül 2016'dan önce oluşturulmuş) için ekleme/meta verilerini yenilemek için VM uzantıları veya veri diski kaldırma.
4. Yeni sürüm için doldurulmuş tüm veri görmüyorum
   * Eylül 2016'dan sonra oluşturulan tüm vm'leri için bir [etiketi](../../azure-resource-manager/resource-group-using-tags.md) görmek için işlem meta verileri. Eski Vm'leri (Eylül 2016'dan önce oluşturulmuş) için ekleme/meta verilerini yenilemek için VM uzantıları veya veri diski kaldırma.
5. Neden iletisi alıyorum hata `500 Internal Server Error`?
   * Üstel geri alma sistemi göre isteğinizi yeniden deneyin. Sorun devam ederse Azure desteğine başvurun.
6. Ek sorular/açıklamalar paylaşacağı?
   * Yorumlarınızı gönderme https://feedback.azure.com.
7. Bu sanal makine ölçek kümesi örneği için işe yarar?
   * Evet meta veri hizmetine ölçek kümesi örnekleri için kullanılabilir.
8. Hizmet için nasıl destek alabilirim?
   * Hizmet için destek almak için uzun denemeden sonra meta veri yanıtını almanız mümkün olmadığı bir VM için Azure portalında bir destek sorunu oluşturun.
9. My hizmet çağrısı zaman aşımına uğradı isteği alabilirim?
   * Meta veri çağrıları VM'nin ağ kartına birincil IP adresinden yapılması gerekir, ayrıca, değişmesi durumunda yollarınızı var. ağ kartınıza dışında 169.254.0.0/16 adresi için bir yol olmalıdır.
10. Ben Etiketlerim sanal makine ölçek kümesindeki güncelleştirildi ancak Vm'leri farklı örneklerinde görünmüyor?
    * Şu anda ScaleSets için etiketleri yalnızca VM yeniden başlatma/yeniden görüntü oluşturma/bir disk örneği değiştirin ya da gösterilir.

    ![Örnek meta veri desteği](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [zamanlanmış olaylar](scheduled-events.md)
