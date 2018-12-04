---
title: Azure SQL veritabanı yönetilen örnek sanal ağ yapılandırması | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile bir sanal ağın (VNet) için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: fcceecbd933980d0ab751fd5e377bbf810b9502e
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837639"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak tanır: 
- Bir şirket içi ağdan doğrudan bir yönetilen örneğe bağlanma 
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket 
- Azure kaynakları için yönetilen örneğe bağlanma  

## <a name="plan"></a>Planlama

Bir yönetilen örneği'nde aşağıdaki sorulara verdiğiniz yanıtlar kullanarak sanal ağ dağıtımı planlama: 
- Tek veya birden çok yönetilen örnekler dağıtmayı planlıyor musunuz? 

  Yönetilen örnek sayısı minimum yönetilen örnekleriniz için ayrılacak alt ağ boyutunu belirler. Daha fazla bilgi için [yönetilen örneği için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances). 
- Yönetilen örneğiniz mevcut bir sanal ağa dağıtmak ihtiyacınız veya yeni bir ağ oluşturuyorsunuz? 

   Mevcut bir sanal ağı kullanmayı planlıyorsanız, yönetilen Örneğinize uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için [yönetilen örneği için var olan sanal ağı değiştirme](#modify-an-existing-virtual-network-for-managed-instances). 

   Yeni bir sanal ağ oluşturmayı planlıyorsanız bkz [yönetilen örneği için yeni sanal ağ oluştur](#create-a-new-virtual-network-for-a-managed-instance).

## <a name="requirements"></a>Gereksinimler

Bir yönetilen örnek oluşturmak için aşağıdaki gereksinimlere uygun bir sanal ağ içinde ayrılmış bir alt ağ (yönetilen örnek alt) oluşturun:
- **Ayrılmış alt**: yönetilen örnek alt kendisiyle ilişkilendirilmiş diğer bulut hizmeti içermemelidir ve bir ağ geçidi alt ağı olmamalıdır. Yönetilen örnek haricinde kaynaklar içeren bir alt ağdaki bir yönetilen örnek oluşturma mümkün olmayacaktır ve daha sonra diğer kaynaklar alt ağdaki ekleyebilirsiniz değil.
- **Uyumlu ağ güvenlik grubu (NSG)**: bir yönetilen örnek alt ağ ile ilişkilendirilmiş bir NSG kuralları önüne başka kurallar aşağıdaki tablolarda (zorunlu gelen güvenlik kuralları ve zorunlu giden güvenlik kuralları) gösterilen içermesi gerekir. 1433 numaralı bağlantı noktasında trafiği filtreleyerek, tam olarak yönetilen örnek veri uç noktası erişimi denetlemek için bir NSG kullanabilirsiniz. 
- **Uyumlu kullanıcı tanımlı yol tablosu (UDR)**: yönetilen örnek alt, bir kullanıcı rota tablosuyla olmalıdır **0.0.0.0/0 sonraki atlama Internet** atanmış zorunlu UDR olarak. Ayrıca, bir hedef sanal ağ geçidi veya sanal ağ Gereci (NVA) üzerinden şirket içi özel IP aralıklarına sahip bu trafiği yönlendirir UDR ekleyebilirsiniz. 
- **İsteğe bağlı bir özel DNS**: sanal ağda özel DNS belirtilirse, Azure'nın yinelemeli çözümleyici IP adresi (örneğin, 168.63.129.16) listeye eklenmelidir. Daha fazla bilgi için [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). Özel DNS sunucusunun ana bilgisayar adlarını aşağıdaki etki alanları ve bunların alt çözümleyebilmesi gerekir: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, ve *microsoftonline-p.com*. 
- **Hizmet uç noktası yok**: yönetilen örnek alt ilişkili bir hizmet uç noktası değil olmalıdır. Hizmet uç noktaları seçeneğini sanal ağ oluştururken, devre dışı emin olun.
- **Yeterli IP adresi**: yönetilen örnek alt en az 16 IP adresi olmalıdır (en az 32 IP adresleri önerilir). Daha fazla bilgi için [yönetilen örnekler için alt ağ boyutunu belirler](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Hedef alt ağ, tüm bu gereksinimler ile uyumlu değilse, yeni bir yönetilen örnek dağıtmak mümkün olmayacaktır. Yönetilen bir örneği oluşturulduğunda bir *Ağ hedefi İlkesi* alt ağ yapılandırmasını uyumlu değişiklikleri önlemek için uygulanır. Son örnek bir alt ağdan kaldırıldıktan sonra *Ağ hedefi İlkesi* de kaldırılır

### <a name="mandatory-inbound-security-rules"></a>Zorunlu bir gelen güvenlik kuralları 

| AD       |BAĞLANTI NOKTASI                        |PROTOKOLÜ|KAYNAK           |HEDEF|EYLEM|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |Herhangi biri        |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |Herhangi biri        |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Herhangi biri        |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları 

| AD       |BAĞLANTI NOKTASI          |PROTOKOLÜ|KAYNAK           |HEDEF|EYLEM|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |Herhangi biri              |Herhangi biri        |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |Herhangi biri              |MI ALT AĞ  |İzin Ver |

  > [!Note]
  > Zorunlu bir gelen güvenlik kuralları, gelen trafiğe izin verse de _herhangi_ bağlantı noktalarını 9000, 9003, kaynak 1438, 1440, 1452 Bu bağlantı noktaları, yerleşik güvenlik duvarı tarafından korunur. Bu [makale](sql-database-managed-instance-management-endpoint.md) yönetim uç noktası IP adresi Bul ve güvenlik duvarı kurallarını doğrulayın nasıl gösterir. 

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Yönetilen örnek için alt ağ boyutunu belirler

Yönetilen bir örneği oluşturduğunuzda, Azure sanal makineler sağlama sırasında seçtiğiniz katmana bağlı olarak bir dizi ayırır. Bu sanal makineler, alt ağ ile ilişkili olduğundan, bunlar IP adresi gerektirir. Normal işlemler ve hizmet bakım sırasında yüksek kullanılabilirlik sağlamak için Azure ek sanal makineler ayırabilir. Sonuç olarak, alt ağ gerekli IP adresi sayısı bu alt ağdaki yönetilen örnekler sayısından büyüktür. 

Tasarıma göre yönetilen örneğe en az 16 IP adresleri bir alt ağda olmalıdır ve en fazla 256 IP adresi kullanıyor olabilirsiniz. Sonuç olarak, alt ağ IP aralıkları tanımlarken, alt ağ maskeleri için/28'i /24 kullanabilirsiniz. 

> [!IMPORTANT]
> Alt ağ boyutu 16 IP adresleriyle ile başka yönetilen örneğe ölçek genişletme için sınırlı olası tam düşük düzeyde grup üyeliğidir. Seçme alt ağ ön eki en az/27 veya altında önerilir. 

Birden çok yönetilen örnek alt ağ içinde dağıtın ve alt ağı boyutuna göre en iyi duruma getirmeyi planlıyorsanız, bir hesaplama oluşturmak için şu parametreleri kullan: 

- Azure alt ağdaki beş IP adreslerini, kendi gereksinimleriniz için kullanır. 
- Genel amaçlı örneği her iki adres olması gerekir 
- Her bir iş açısından kritik örneği dört adres olması gerekir

**Örnek**: üç genel amaçlı ve iki iş açısından kritik yönetilen örneği planlama. 5 + 3 * 2 + 2 * 4 = 19 ihtiyacınız anlamına gelir IP adreslerini. IP aralıklarını 2'in gücünü tanımlanan 32 IP aralığı gerekir (2 ^ 5) IP adresi. Bu nedenle, / 27 alt ağ maskesine sahip bir alt ağı ayırmanız gerekir. 

> [!IMPORTANT]
> Yukarıda gösterilen hesaplama geliştirmelerle daha eski hale gelir. 

## <a name="create-a-new-virtual-network-for-a-managed-instance"></a>Yeni bir sanal ağ için bir yönetilen örnek oluşturma

Oluşturma ve sanal ağ yapılandırma en kolay yolu, Azure Resource Manager dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. Kullanım **azure'a Dağıt** düğmesi sanal ağı Azure bulutunda dağıtmak için:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Bu düğme, yönetilen örneği dağıtabileceğiniz ağ ortamını yapılandırmak için kullanabileceğiniz bir form açılır.

  > [!Note]
  > Bu Azure Resource Manager şablonu, sanal ağı iki alt ağ ile dağıtır. Adlı bir alt ağ **ManagedInstances** yönetilen örnekler için ayrılmıştır ve diğer alt ağı adlı sırada yol tablosu, önceden yapılandırılmış **varsayılan** yönetilen erişmeli diğer kaynaklar için kullanılır Örneği (örneğin, Azure sanal makineler). Kaldırabilirsiniz **varsayılan** ihtiyacınız yoksa alt ağ.

3. Ağ ortamı yapılandırın. Aşağıdaki formda ağ ortamınızın parametreleri yapılandırabilirsiniz:

![Azure ağı yapılandırma](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

VNet ve alt ağlar adlarını değiştirme ve ağ kaynaklarınıza ilişkili IP aralıklarını ayarlama. "Satın Al" düğmesine basın, sonra bu form oluşturma ve ortamınızı yapılandırın. İki alt ağa ihtiyacınız yoksa, varsayılan silebilirsiniz. 

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Yönetilen örnek için var olan bir sanal ağı değiştirme 

Sorular ve cevaplar Bu bölümde bir yönetilen örnek mevcut bir sanal ağa ekleme işlemini göstermektedir. 

**Varolan bir sanal ağ için Klasik veya Resource Manager dağıtım modelini kullanıyor musunuz?** 

Bu gibi durumlarda, bir yönetilen örneği yalnızca Resource Manager sanal ağları oluşturabilirsiniz. 

**SQL yönetilen örneği için yeni bir alt ağ oluşturun veya var olan bir kullanmak ister misiniz?**

Yeni bir tane oluşturmak isterseniz: 

- Yönergeleri izleyerek alt ağ boyutunu hesaplamak [yönetilen örnekler için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümü.
- Bağlantısındaki [ekleme, değiştirme veya silme bir sanal ağ alt](../virtual-network/virtual-network-manage-subnet.md). 
- Tek giriş içeren bir yol tablosu oluşturma **0.0.0.0/0**, sonraki atlama Internet ve yönetilen örneği için alt ağ ile ilişkilendirebilirsiniz.  

Var olan bir alt ağ içinde bir yönetilen örnek oluşturmak istiyorsanız, alt hazırlamak için aşağıdaki PowerShell betiğini öneririz.
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Alt ağ hazırlama üç basit adımda gerçekleştirilir:

- Validate - seçili sanal: ağa bağlanmadı ve alt ağ için ağ gereksinimleri örneği yönetilen doğrulanır
- Onayla - kullanıcı bir alt ağ yönetilen örnek dağıtımına hazırlanmak için yapılan ve onaylarının sorulan gereken değişiklik kümesini gösterilir
- Sanal ağ ve alt düzgün şekilde yapılandırıldığından - hazırlama

**Yapılandırılmış özel DNS sunucusu gerekiyor?** 

Yanıt Evet ise bkz [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). 

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- VNet oluşturmak için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
