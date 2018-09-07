---
title: Azure örnek meta veri hizmeti | Microsoft Docs
description: Windows sanal makinenin işlem, ağ ve yaklaşan bakımın olaylar hakkında bilgi almak için rESTful arabirimi.
services: virtual-machines-windows
documentationcenter: ''
author: harijayms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: ccaa6e79d9a24409b8c905561b265c70ea781dc2
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022584"
---
# <a name="azure-instance-metadata-service"></a>Azure örnek meta veri hizmeti


Azure örnek meta veri hizmeti yönetmek ve sanal makinelerinizi yapılandırmak için kullanılan sanal makine örneklerini çalıştırma hakkında bilgi sağlar.
Bu SKU, ağ yapılandırması ve yaklaşan Bakımı olayları gibi bilgileri içerir. Hangi tür bilgileri kullanılabilir daha fazla bilgi için bkz: [meta veri kategorileri](#instance-metadata-data-categories).

Azure'nın örnek meta veri hizmeti REST uç noktası aracılığıyla oluşturulan tüm Iaas Vm'leri için erişilebilir olan [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). İyi bilinen yönlendirilemeyen IP adresinde uç noktası kullanılabilir (`169.254.169.254`), erişilebilir yalnızca VM içinden.

> [!IMPORTANT]
> Bu hizmet **sunuldu** tüm Azure bölgelerinde.  Düzenli olarak, sanal makine örnekleri hakkında yeni bilgiler kullanıma sunmak için güncelleştirmeleri alır. Bu sayfa güncel yansıtır [veri kategorileri](#instance-metadata-data-categories) kullanılabilir.

## <a name="service-availability"></a>Hizmet kullanılabilirliği
Hizmet genel kullanıma sunulan Azure bölgelerinde kullanılabilir. Tüm API sürümü tüm Azure bölgelerinde kullanılabilir.

Bölgeler                                        | Kullanılabilirlik?                                 | Desteklenen Sürümler
-----------------------------------------------|-----------------------------------------------|-----------------
[Tüm genel kullanıma sunulan Global Azure bölgeleri](https://azure.microsoft.com/regions/)     | Genel Kullanıma Sunuldu   | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02
[Azure Devlet Kurumları](https://azure.microsoft.com/overview/clouds/government/)              | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01
[Azure Çin](https://www.azure.cn/)                                                           | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01
[Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/)                    | Genel Kullanıma Sunuldu | 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01

Bu tablo veya hizmet güncelleştirmeleri vardır ve yeni desteklenen sürümler kullanılabilir güncelleştiriliyor

Örnek meta veri hizmeti deneyebilmesi için bir VM oluşturma [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) veya [Azure portalında](http://portal.azure.com) yukarıdaki bölgelerde ve aşağıdaki örneklerde izleyin.

## <a name="usage"></a>Kullanım

### <a name="versioning"></a>Sürüm oluşturma
Örnek meta veri hizmeti sürümü. Sürümleri zorunludur ve genel Azure üzerinde geçerli sürümü `2018-04-02`. (2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02) geçerli desteklenen sürümler:

> [!NOTE] 
> Önceki Önizleme sürümlerinde zamanlanmış olaylar {son} api-version desteklenir. Bu biçim, artık desteklenmemektedir ve gelecekte kullanım dışı bırakılacaktır.

Daha yeni sürümler eklendikçe betiklerinizi özgü veri biçimleri üzerinde bağımlılıkları varsa, eski sürümleri yine de uyumluluk için erişilebilir. Ancak, önceki Önizleme sürümü (2017-03-01) hizmet genel olarak kullanılabilir olduğunda kullanılamayabilir.

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

Varsayılan olmayan yanıt biçimi erişmek için istenen biçimi istek sorgu dizesi parametresi olarak belirtin. Örneğin:

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

### <a name="security"></a>Güvenlik
Örnek meta veri Hizmeti uç noktası yalnızca yönlendirilemeyen bir IP adresi üzerinde çalışan sanal makine örneği içinde erişilebilir. Ayrıca, tüm ile istek bir `X-Forwarded-For` üst bilgi, hizmet tarafından reddedildi.
İstekleri de içermelidir bir `Metadata: true` başlığına gerçek isteği doğrudan amaçlayan ve yanlışlıkla yeniden yönlendirme bir parçası olduğundan emin olun. 

### <a name="error"></a>Hata
Bir veri öğesi bulunamadı ya da hatalı biçimlendirilmiş isteği ise örnek meta veri hizmeti standart HTTP hataları döndürür. Örneğin:

HTTP durum kodu | Neden
----------------|-------
200 TAMAM |
400 Hatalı istek | Eksik `Metadata: true` üstbilgisi
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
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-12-01"
```

**Yanıt**

> [!NOTE] 
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "compute": {
    "location": "westus",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
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
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-08-01 | select -ExpandProperty Content
```

Aracılığıyla veya `Invoke-RestMethod` PowerShell cmdlet:
    
```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-08-01 -Method get 
```

**Yanıt**

> [!NOTE] 
> Yanıt, bir JSON dizesi. Aşağıdaki örnek yanıt okunabilirlik açısından pretty yazdırılır.

```json
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
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
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Örnek meta veri kategorileri
Aşağıdaki veri kategorileri, örnek meta veri hizmeti kullanılabilir:

Veriler | Açıklama | Kullanıma sunulan sürümü 
-----|-------------|-----------------------
location | Azure bölgesi VM çalışıyor | 2017-04-02 
ad | VM adı | 2017-04-02
teklif | VM görüntüsü için bilgi sağlar. Bu değer, yalnızca Azure görüntü Galerisi'nden dağıtılan görüntülerin bulunur. | 2017-04-02
Yayımcı | VM görüntü yayımcısı | 2017-04-02
SKU | Belirli SKU için VM görüntüsü | 2017-04-02
sürüm | VM görüntüsü sürümü | 2017-04-02
osType | Linux veya Windows | 2017-04-02
platformUpdateDomain |  [Güncelleme etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
platformFaultDomain | [Hata etki alanı](manage-availability.md) VM'nin çalışır durumda | 2017-04-02
vmId | [Benzersiz tanımlayıcı](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) VM için | 2017-04-02
vmSize | [VM boyutu](sizes.md) | 2017-04-02
subscriptionId | Sanal makine için Azure aboneliği | 2017-08-01
etiketler | [Etiketleri](../../azure-resource-manager/resource-group-using-tags.md) sanal makineniz için  | 2017-08-01
resourceGroupName | [Kaynak grubu](../../azure-resource-manager/resource-group-overview.md) sanal makineniz için | 2017-08-01
placementGroupId | [Yerleştirme grubu](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) , sanal makine ölçek kümesi | 2017-08-01
plan | [Planı] (https://docs.microsoft.com/en-us/rest/api/compute/virtualmachines/createorupdate#plan) Azure Market görüntüsü içindeki bir VM için adı, ürün ve yayımcı içerir. | 2017-04-02
publicKeys | Ortak anahtarlar koleksiyonunu [https://docs.microsoft.com/en-us/rest/api/compute/virtualmachines/createorupdate#sshpublickey] VM ve yolları atanan | 2017-04-02
vmScaleSetName | [Sanal makine ölçek kümesi adı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) , sanal makine ölçek kümesi | 2017-12-01
bölge | [Kullanılabilirlik alanı](../../availability-zones/az-overview.md) sanal makinenizin | 2017-12-01 
IPv4/Privateıpaddress | Sanal makinenin yerel IPv4 adresi | 2017-04-02
IPv4/Publicıpaddress | Sanal makinenin genel IPv4 adresi | 2017-04-02
alt ağ/adresi | VM alt ağ adresi | 2017-04-02 
alt ağ/ön eki | Alt ağ ön eki, örnek 24 | 2017-04-02 
ipv6/ipAddress | Sanal makinenin yerel IPv6 adresi | 2017-04-02 
macAddress | VM mac adresi | 2017-04-02 
scheduledevents | Bkz: [zamanlanmış olaylar](scheduled-events.md) | 2017-08-01
identity | (Önizleme) Yönetilen hizmet kimliği. Bkz: [erişim belirteci alma](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01

## <a name="example-scenarios-for-usage"></a>Kullanım için örnek senaryolar  

### <a name="tracking-vm-running-on-azure"></a>Azure üzerinde çalışan VM izleme

Bir hizmet sağlayıcısı olarak, yazılım çalışan VM'lerin sayısını izlemek veya VM benzersizliğini izlemek için gereken aracıları gerektirebilir. Bir VM için benzersiz bir kimliği getirebilmesi için kullanın `vmId` alanını örnek meta veri hizmeti.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Yanıt**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Kapsayıcıları yerleştirilmesi, veri bölümlerine hata/güncelleştirme etki alanı tabanlı 

Belirli senaryolar farklı veri çoğaltmalarını yerleşimini prime çok önemlidir. Örneğin, [HDFS çoğaltma yerleştirme](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) veya kapsayıcı yerleştirme aracılığıyla bir [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) bilmek gerektirebilir `platformFaultDomain` ve `platformUpdateDomain` VM'nin çalışır durumda.
Ayrıca yararlanabileceğiniz [kullanılabilirlik](../../availability-zones/az-overview.md) bu kararları vermek örnekleri için.
Bu veriler doğrudan örnek meta veri hizmeti aracılığıyla sorgulayabilirsiniz.

**İstek**

```bash
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text" 
```

**Yanıt**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Destek olayı sırasında VM hakkında daha fazla bilgi edinme 

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


### <a name="getting-azure-environment-where-the-vm-is-running"></a>Azure VM çalıştığı ortamı alma 

Azure, çeşitli soverign Bulutları gibi sahiptir [Azure kamu](https://azure.microsoft.com/overview/clouds/government/) , bazen Azure ortamına bazı çalışma zamanı kararlar gerekir. Aşağıdaki örnek Bunu başarmak nasıl gösterir

**İstek**

```
  $metadataResponse = Invoke-WebRequest "http://169.254.169.254/metadata/instance/compute?api-version=2018-02-01" -H @{"Metadata"="true"} -UseBasicParsing
  $metadata = ConvertFrom-Json ($metadataResponse.Content)
 
  $endpointsResponse = Invoke-WebRequest "https://management.azure.com/metadata/endpoints?api-version=2017-12-01" -UseBasicParsing
  $endpoints = ConvertFrom-Json ($endpointsResponse.Content)
 
  foreach ($cloud in $endpoints.cloudEndpoint.PSObject.Properties) {
    $matchingLocation = $cloud.Value.locations | Where-Object {$_ -match $metadata.location}
    if ($matchingLocation) {
      $cloudName = $cloud.name
      break
    }
  }
 
  $environment = "Unknown"
  switch ($cloudName) {
    "public" { $environment = "AzureCloud"}
    "usGovCloud" { $environment = "AzureUSGovernment"}
    "chinaCloud" { $environment = "AzureChinaCloud"}
    "germanCloud" { $environment = "AzureGermanCloud"}
  }
 
  Write-Host $environment
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
1. Hata alıyorum `400 Bad Request, Required metadata header not specified`. Bu ne demektir?
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
   * Yorumlarınızı gönderme http://feedback.azure.com.
7. Bu sanal makine ölçek kümesi örneği için işe yarar?
   * Evet meta veri hizmetine ölçek kümesi örnekleri için kullanılabilir. 
8. Hizmet için nasıl destek alabilirim?
   * Hizmet için destek almak için uzun denemeden sonra meta veri yanıtını almanız mümkün olmadığı bir VM için Azure portalında bir destek sorunu oluşturun 
9. My çağrısı zaman aşımına uğradı isteği alabilirim hizmet?
   * Meta veri çağrıları VM'nin ağ kartına birincil IP adresinden yapılması gerekir, ayrıca, değişmesi durumunda yollarınızı var. ağ kartınıza dışında 169.254.0.0/16 adresi için bir yol olmalıdır.
10. Ben Etiketlerim sanal makine ölçek kümesindeki güncelleştirildi ancak Vm'leri farklı örneklerinde görünmüyor?
   * Şu anda ScaleSets için etiketleri yalnızca VM yeniden başlatma/yeniden görüntü oluşturma/bir disk örneği değiştirin ya da gösterilir. 

   ![Örnek meta veri desteği](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [zamanlanmış olaylar](scheduled-events.md)
