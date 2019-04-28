---
title: Azure bulut Hizmetleri NetworkConfiguration şeması | Microsoft Docs
ms.custom: ''
origin.date: 12/07/2016
ms.date: 11/06/2017
ms.prod: azure
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: c1b94a9e-46e8-4a18-ac99-343c94b1d4bd
caps.latest.revision: 28
author: thraka
ms.author: v-yiso
manager: timlt
ms.openlocfilehash: fb833904502c0c42b46201fd46a368de0376277c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130285"
---
# <a name="azure-cloud-services-config-networkconfiguration-schema"></a>Azure bulut Hizmetleri Yapılandırma NetworkConfiguration şeması

`NetworkConfiguration` Hizmet yapılandırma dosyası öğesi sanal ağ ve DNS değerleri belirtir. Bu ayarlar, bulut Hizmetleri için isteğe bağlıdır.

Sanal ağlar ve ilişkili şemaları hakkında daha fazla bilgi için aşağıdaki kaynak kullanabilirsiniz:

- [Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)
- [Bulut hizmeti (Klasik) tanım Şeması](schema-csdef-file.md)
- [Sanal ağ (Klasik) oluşturma](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)

## <a name="networkconfiguration-element"></a>NetworkConfiguration öğesi
Aşağıdaki örnekte gösterildiği `NetworkConfiguration` öğesi ve onun alt öğeleri.

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

Aşağıdaki tabloda, alt öğelerini açıklar `NetworkConfiguration` öğesi.

| Öğe       | Açıklama |
| ------------- | ----------- |
| AccessControl | İsteğe bağlı. Bir bulut Hizmeti uç noktalarına erişim için kuralları belirtir. Erişim denetimi adı için bir dize tarafından tanımlanan `name` özniteliği. `AccessControl` Öğesi içeren bir veya daha fazla `Rule` öğeleri. Birden fazla `AccessControl` öğesi tanımlanabilir.|
| Kural | İsteğe bağlı. Bir belirtilen alt ağ IP adresi aralığı için alınması gereken eylemi belirtir. Kural sırası için bir dize değeri tarafından tanımlanan `order` özniteliği. Düşük kuralı, daha yüksek öncelik numarası. Örneğin, kuralları 100, 200 ve 300 sıra numaraları ile belirtilebilir. Sipariş numarası 100 kurala sahip bir sipariş 200 kuralına göre önceliklidir.<br /><br /> Kural için bir eylem için bir dize tarafından tanımlanan `action` özniteliği. Olası değerler şunlardır:<br /><br /> -   `permit` – Yalnızca belirtilen alt ağ aralığı paketleri uç noktası ile iletişim kurabildiğini belirtir.<br />-   `deny` – Belirtilen alt ağ aralığında Uç noktalara erişimin reddedildiğini belirtir.<br /><br /> Kural tarafından etkilenen alt ağ IP adresi aralığı için bir dize tarafından tanımlanan `remoteSubnet` özniteliği. Kural açıklaması için bir dize tarafından tanımlanan `description` özniteliği.|
| EndpointAcl | İsteğe bağlı. Bir uç nokta erişim denetim kuralları atama belirtir. Uç nokta içeren rolün adı için bir dize tarafından tanımlanan `role` özniteliği. Uç nokta adı için bir dize tarafından tanımlanan `endpoint` özniteliği. Dizi adını `AccessControl` uç noktaya uygulanacak kuralları için bir dize içinde tanımlanmıştır `accessControl` özniteliği. Birden fazla `EndpointAcl` öğeleri tanımlanabilir.|
| DnsServer | İsteğe bağlı. Bir DNS sunucusu ayarlarını belirtir. DNS sunucuları olmadan bir sanal ağ ayarları belirtebilirsiniz. DNS sunucusu adı için bir dize tarafından tanımlanan `name` özniteliği. DNS sunucusunun IP adresi için bir dize tarafından tanımlanan `IPAddress` özniteliği. IP adresi geçerli bir IPv4 adresi olmalıdır.|
| VirtualNetworkSite | İsteğe bağlı. Bulut hizmetinizin dağıtmak istediğiniz sanal ağ alanı adını belirtir. Bu ayar, sanal bir ağ sitesi oluşturmaz. Sanal ağınız için ağ dosyasında önceden tanımlanmış bir site başvurduğu. Bir bulut hizmeti, yalnızca bir sanal ağ üyesi olabilir. Bu ayar belirtmezseniz, bulut hizmetine bir sanal ağa dağıtılmaz. Sanal ağ alanı adını bir dize tarafından tanımlanan `name` özniteliği.|
| InstanceAddress | İsteğe bağlı. İlişki rolü bir alt ağ veya alt ağ kümesi sanal ağ içinde belirtir. Bir rol adı için bir örnek adresi ile ilişkilendirdiğinizde, bu rol, ilişkili olmasını istediğiniz alt ağlar belirtebilirsiniz. `InstanceAddress` Bir alt öğe içeriyor. Alt ağlar ve alt ağ ile ilişkili rol adı için bir dize tarafından tanımlanan `roleName` özniteliği.|
| Alt ağ | İsteğe bağlı. Alt ağ adı ağ yapılandırma dosyasında karşılık gelen alt ağı belirtir. Alt ağ adı için bir dize tarafından tanımlanan `name` özniteliği.|
| ReservedIP | İsteğe bağlı. Dağıtım ile ilişkilendirilmesi gereken ayrılmış IP adresini belirtir. Ayrılmış IP adresi oluşturmak için ayrılmış IP adresi oluşturma kullanmanız gerekir. Her dağıtım bir bulut hizmetinde bir ayrılmış IP adresi ile ilişkilendirilebilir. Ayrılmış IP adresi adı için bir dize tarafından tanımlanan `name` özniteliği.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (Klasik) yapılandırma şeması](schema-cscfg-file.md)