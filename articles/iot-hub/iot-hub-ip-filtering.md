---
title: Azure IOT hub'ı IP bağlantı filtreleri | Microsoft Docs
description: IP bloğu bağlantıları için Azure IOT hub'ınıza belirli IP adresleri için filtre kullanma Kişiye ait bağlantılar veya IP adresi aralıkları engelleyebilirsiniz.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: robinsh
ms.openlocfilehash: 82b079a7e826d870ed3e156b56921fc347a0fbd8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445538"
---
# <a name="use-ip-filters"></a>IP filtrelerini kullanma

Güvenlik, Azure IOT hub'ına bağlı tüm IOT çözümlerinin, önemli bir yönüdür. Bazen cihazlar bağlanabileceği IP adreslerini Güvenlik yapılandırmanızın bir parçası açıkça belirtmeniz gerekir. *IP Filtresi* özelliği belirli IPv4 adreslerinin gelen trafiği kabul etme veya reddetme kurallarını yapılandırmanıza olanak sağlar.

## <a name="when-to-use"></a>Kullanılması gereken durumlar

IOT Hub uç noktaları belirli IP adresleri için engellemek yararlıdır, iki belirli kullanım örnekleri vardır:

* IOT hub'ınıza, yalnızca belirtilen IP adreslerinden trafiğini almasına ve diğer her şeyi reddet. Örneğin, IOT hub'ınıza kullanarak [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) bir IOT hub'ı şirket içi altyapınız arasında özel bağlantılar oluşturmak için.

* IOT hub Yöneticisi tarafından kuşkulu olarak belirlenmiştir IP adreslerinden gelen trafiği reddetmek gerekir.

## <a name="how-filter-rules-are-applied"></a>Filtre kurallarının uygulanma yöntemi

IP Filtresi kurallarının, IOT Hub hizmet düzeyinde uygulanır. Bu nedenle IP Filtresi kurallarının, cihazları ve herhangi bir desteklenen protokolünü kullanarak arka uç uygulamaları tüm bağlantılara uygulanır.

IOT hub'ınızda bir rejecting IP kuralıyla eşleşen bir IP adresinden bağlantı girişimleri bir yetkisiz 401 durum kodu ve açıklama alır. Yanıt iletisi IP kuralı başvurmayacak.

## <a name="default-setting"></a>Varsayılan ayar

Varsayılan olarak, **IP Filtresi** portalında bir IOT hub'ı için kılavuz boştur. Bu varsayılan ayar, hub'ınıza herhangi bir IP adresinden gelen bağlantıları kabul etmesini anlamına gelir. Bu varsayılan ayarı 0.0.0.0/0 IP adresi aralığı kabul eden bir kural eşdeğerdir.

![IOT hub'ı varsayılan IP Filtresi Ayarları](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Bir IP filtresi kuralı ekleme veya düzenleme

Bir IP filtresi kuralı eklediğinizde, aşağıdaki değerleri istenir:

* Bir **IP filtresi kuralı adı** en fazla 128 karakter benzersiz, büyük/küçük harf, alfasayısal bir dize olmalıdır. Yalnızca ASCII 7 bit alfasayısal karakterler artı `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` kabul edilir.

* Seçin bir **Reddet** veya **kabul** olarak **eylem** IP filtresi kuralı için.

* Tek bir IPv4 adresi veya IP adresleri CIDR gösteriminde bloğu sağlar. Örneğin, CIDR gösterimi 192.168.100.0/22 1024 IPv4 adreslerini 192.168.100.0 ve 192.168.103.255 arasındaki.

![Bir IOT hub'ına bir IP filtresi kuralı Ekle](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Kural kaydettikten sonra güncelleştirme devam ediyor size bildiren bir uyarı görürsünüz.

![Bir IP filtresi kuralı kaydetme hakkında bildirim](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

**Ekle** en fazla 10 IP Filtresi kurallarının ulaştığında seçeneği devre dışıdır.

Mevcut bir kuralı, kuralı içeren satırı çift tıklayarak düzenleyebilirsiniz.

> [!NOTE]
> Reddetme IP adresleri IOT hub'ı ile etkileşim diğer Azure Hizmetleri (örneğin, Azure Stream Analytics, Azure sanal makineler veya Portalı'nda Device Explorer) engelleyebilirsiniz.

> [!WARNING]
> IP Filtresi etkinleştirilmiş bir IOT hub'ından iletileri okumak için Azure Stream Analytics (ASA) kullanırsanız, Event Hub ile uyumlu ada ve uç IOT hub'ınızın ASA bağlantı dizesini kullanın.

## <a name="delete-an-ip-filter-rule"></a>Bir IP filtresi kuralı Sil

Bir IP filtresi kuralı silmek için bir veya daha fazla kural kılavuz tıklatın seçin ve **Sil**.

![Bir IOT hub'ı IP filtresi kuralı Sil](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="retrieve-and-update-ip-filters-using-azure-cli"></a>Alma ve Azure CLI kullanarak IP filtreleri güncelleştirin

IOT Hub'ınızın IP filtreleri alınır ve aracılığıyla güncelleştirilmiş [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). 

IOT hub'ınızın geçerli IP filtreleri almak için şunu çalıştırın:

```azurecli-interactive
az resource show -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs
```

Burada, mevcut IP filtreleri altında listelenen bir JSON nesnesi döndürür `properties.ipFilterRules` anahtarı:

```json
{
...
    "properties": {
        "ipFilterRules": [
        {
            "action": "Reject",
            "filterName": "MaliciousIP",
            "ipMask": "6.6.6.6/6"
        },
        {
            "action": "Allow",
            "filterName": "GoodIP",
            "ipMask": "131.107.160.200"
        },
        ...
        ],
    },
...
}
```

IOT Hub'ınız için yeni IP filtresi eklemek için şunu çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules "{\"action\":\"Reject\",\"filterName\":\"MaliciousIP\",\"ipMask\":\"6.6.6.6/6\"}"
```

IOT hub'ına var olan bir IP filtresinin kaldırmak için şunu çalıştırın:

```azurecli-interactive
az resource update -n <iothubName> -g <resourceGroupName> --resource-type Microsoft.Devices/IotHubs --add properties.ipFilterRules <ipFilterIndexToRemove>
```

Unutmayın `<ipFilterIndexToRemove>` IOT Hub'ınızın IP filtreleri sıralama için eşleşmelidir `properties.ipFilterRules`.


## <a name="retrieve-and-update-ip-filters-using-azure-powershell"></a>Alma ve Azure PowerShell kullanarak IP filtreleri güncelleştirin

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

IOT Hub'ınızın IP filtreleri alınır ve aracılığıyla ayarlanan [Azure PowerShell](/powershell/azure/overview). 

```powershell
# Get your IoT Hub resource using its name and its resource group name
$iothubResource = Get-AzResource -ResourceGroupName <resourceGroupNmae> -ResourceName <iotHubName> -ExpandProperties

# Access existing IP filter rules
$iothubResource.Properties.ipFilterRules |% { Write-host $_ }

# Construct a new IP filter
$filter = @{'filterName'='MaliciousIP'; 'action'='Reject'; 'ipMask'='6.6.6.6/6'}

# Add your new IP filter rule
$iothubResource.Properties.ipFilterRules += $filter

# Remove an existing IP filter rule using its name, e.g., 'GoodIP'
$iothubResource.Properties.ipFilterRules = @($iothubResource.Properties.ipFilterRules | Where 'filterName' -ne 'GoodIP')

# Update your IoT Hub resource with your updated IP filters
$iothubResource | Set-AzResource -Force
```

## <a name="update-ip-filter-rules-using-rest"></a>REST kullanarak IP Filtresi kurallarının güncelleştir

Ayrıca, alma ve Azure kaynak sağlayıcının REST uç noktasını kullanarak IOT Hub'ınızın IP Filtresi değiştirme. Bkz: `properties.ipFilterRules` içinde [createorupdate yöntemini](https://docs.microsoft.com/rest/api/iothub/iothubresource/createorupdate).


## <a name="ip-filter-rule-evaluation"></a>IP filtresi kuralı değerlendirme

IP Filtresi kurallarının sırayla uygulanır ve IP adresi ile eşleşen ilk kural kabul etme veya reddetme eylemi belirler.

Örneğin, aralığı 192.168.100.0/22 adresleri kabul edin ve diğer her şeyi Reddet istiyorsanız, kılavuzdaki ilk kural adres aralığı 192.168.100.0/22 kabul etmelidir. Sonraki kural aralığı 0.0.0.0/0 kullanarak tüm adresleri reddedecek.

Üç dikey noktayı içeren bir satırın başında'ı tıklatarak ve sürükleyerek kullanarak kılavuzunda IP Filtresi kurallarının sırasını değiştirmek ve bırakın.

Yeni IP filtresi kuralı siparişinizi kaydetmek için tıklatın **Kaydet**.

![IOT hub'ı IP Filtresi kurallarının sırasını değiştirme](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [İşlemleri izleme](iot-hub-operations-monitoring.md)
* [IOT hub'ı ölçümleri](iot-hub-metrics.md)
