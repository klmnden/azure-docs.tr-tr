---
title: Azure Cloud Services NetworkConfiguration şema | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: c1b94a9e-46e8-4a18-ac99-343c94b1d4bd
caps.latest.revision: 28
author: thraka
ms.author: adegeo
manager: timlt
ms.openlocfilehash: ebe81b2e4dea347eb22b173ff1e9baf1ee6bb75d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359654"
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Config NetworkConfiguration şeması Azure bulut Hizmetleri

`NetworkConfiguration` Hizmet yapılandırma dosyası öğesi sanal ağ ve DNS değerleri belirtir. Bu ayarlar için bulut hizmetlerini isteğe bağlıdır.

Sanal ağlar ve ilgili şemalardan hakkında daha fazla bilgi için aşağıdaki kaynak kullanabilirsiniz:

- [Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)
- [Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)
- [Bir sanal ağ (Klasik) oluşturun](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>NetworkConfiguration öğesi
Aşağıdaki örnekte gösterildiği `NetworkConfiguration` öğesi ve kendi alt öğelerini.

```xml
<ServiceConfiguration>
  <NetworkConfiguration>
    <AccessControls>
      <AccessControl name="aclName1">
        <Rule order="<rule-order>" action="<rule-action>" remoteSubnet="<subnet-address>" description="rule-description"/>
      </AccessControl>
    </AccessControls>
    <EndpointAcls>
      <EndpointAcl role="<role-name>" endpoint="<endpoint-name>" accessControl="<acl-name>"/>
    </EndpointAcls>
    <Dns>
      <DnsServers>
        <DnsServer name="<server-name>" IPAddress="<server-address>" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSite name="<site-name>"/>
    <AddressAssignments>
      <InstanceAddress roleName="<role-name>">
        <Subnets>
          <Subnet name="<subnet-name>"/>
        </Subnets>
      </InstanceAddress>
      <ReservedIPs>
        <ReservedIP name="<reserved-ip-name>"/>
      </ReservedIPs>
    </AddressAssignments>
  </NetworkConfiguration>
</ServiceConfiguration>
```

Aşağıdaki tabloda alt öğeleri açıklanmıştır `NetworkConfiguration` öğesi.

| Öğe       | Açıklama |
| ------------- | ----------- |
| AccessControl | İsteğe bağlı. Uç noktalarına erişim için kuralları, bir bulut hizmetinde belirtir. Erişim denetimi adı için bir dize tarafından tanımlanan `name` özniteliği. `AccessControl` Öğesi içeren bir veya daha fazla `Rule` öğeleri. Birden fazla `AccessControl` öğesi tanımlanabilir.|
| Kural | İsteğe bağlı. Bir belirtilen alt ağ IP adresi aralığı için gerçekleştirilecek eylemi belirtir. Kural sırasını için dize değeri tarafından tanımlanan `order` özniteliği. Düşük kural numarası öncelik o kadar yüksektir. Örneğin, kuralları ile 100, 200 ve 300 sıra numaraları belirtilebilir. Sipariş numarası 100 kuralıyla bir sipariş 200 kuralının önceliklidir.<br /><br /> Kural için eylem için bir dize tarafından tanımlanan `action` özniteliği. Olası değerler şunlardır:<br /><br /> -   `permit` – Yalnızca belirtilen alt ağı aralığından paketleri uç noktasıyla iletişim kurabilir belirtir.<br />-   `deny` – Belirtilen alt ağ aralığında uç noktalarına erişimin reddedildiğini belirtir.<br /><br /> Kural tarafından etkilenen alt ağ IP adresi aralığı için bir dize tarafından tanımlanan `remoteSubnet` özniteliği. Kural açıklaması için bir dize tarafından tanımlanan `description` özniteliği.|
| EndpointAcl | İsteğe bağlı. Erişim denetimi kuralları için bir bitiş noktasına atanmasını belirtir. Uç nokta içeren rolün adı için bir dize tarafından tanımlanan `role` özniteliği. Uç noktanın adı için bir dize tarafından tanımlanan `endpoint` özniteliği. Kümesi adını `AccessControl` uç noktasına uygulanması gereken kuralları için bir dize tanımlanmış `accessControl` özniteliği. Birden fazla `EndpointAcl` öğeleri tanımlanabilir.|
| DnsServer | İsteğe bağlı. Bir DNS sunucusu ayarlarını belirtir. Sanal Ağ olmadan DNS sunucularının ayarlarını belirtebilirsiniz. DNS sunucusu adı için bir dize tarafından tanımlanan `name` özniteliği. DNS sunucusunun IP adresi için bir dize tarafından tanımlanan `IPAddress` özniteliği. IP adresi geçerli bir IPv4 adresi olmalıdır.|
| VirtualNetworkSite | İsteğe bağlı. Bulut hizmeti dağıtmak istediğiniz sanal ağ site adını belirtir. Bu ayar, bir sanal ağ Site oluşturmaz. Ağ dosyasında sanal ağınız için önceden tanımlanmış bir site başvurur. Bir bulut hizmeti, yalnızca bir sanal ağ üyesi olabilir. Bu ayar belirtmezseniz, bulut hizmeti bir sanal ağa dağıtılmaz. Sanal ağ sitesinin adı için bir dize tarafından tanımlanan `name` özniteliği.|
| InstanceAddress | İsteğe bağlı. Bir rol bir alt ağa ilişkilendirme veya alt ağ kümesi sanal ağ belirtir. Örnek adresi için bir rol adı ilişkilendirdiğinizde, bu rolün ilişkili olmasını istediğiniz alt ağlar belirtebilirsiniz. `InstanceAddress` Bir alt öğe içeriyor. Alt ağ veya alt ağları ile ilişkilendirilen rol adı için bir dize tarafından tanımlanan `roleName` özniteliği.|
| Alt ağ | İsteğe bağlı. Ağ yapılandırma dosyasında alt ağ adı için karşılık gelen alt ağını belirtir. Alt ağ adı için bir dize tarafından tanımlanan `name` özniteliği.|
| ReservedIP | İsteğe bağlı. Dağıtımı ile ilişkilendirilmesi gereken ayrılmış IP adresini belirtir. Ayrılmış IP adresi oluşturmak için ayrılmış IP adresi oluşturma kullanmanız gerekir. Her bir bulut hizmeti dağıtımında bir ayrılmış IP adresi ile ilişkilendirilebilir. Ayrılmış IP adresi adı için bir dize tarafından tanımlanan `name` özniteliği.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)