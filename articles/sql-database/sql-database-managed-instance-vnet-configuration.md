---
title: Azure SQL veritabanı yönetilen örnek sanal ağ yapılandırması | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile bir sanal ağın (VNet) için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: b17749999f7903746651403c5948933332dbee5d
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047941"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma

Azure SQL veritabanı yönetilen örneği (Önizleme) içinde bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak tanır: 
- Bir şirket içi ağdan doğrudan bir yönetilen örneğe bağlanma 
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket 
- Azure kaynakları için yönetilen örneğe bağlanma  

## <a name="plan"></a>Planlama

Bir yönetilen örneği'nde aşağıdaki sorulara verdiğiniz yanıtlar kullanarak sanal ağ dağıtımı planlama: 
- Tek veya birden çok yönetilen örnekler dağıtmayı planlıyor musunuz? 

  Yönetilen örnek sayısı minimum yönetilen örnekleriniz için ayrılacak alt ağ boyutunu belirler. Daha fazla bilgi için [yönetilen örneği için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances). 
- Yönetilen örneğiniz mevcut bir sanal ağa dağıtmak ihtiyacınız veya yeni bir ağ oluşturuyorsunuz? 

   Mevcut bir sanal ağı kullanmayı planlıyorsanız, yönetilen Örneğinize uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için [yönetilen örneği için var olan sanal ağı değiştirme](#modify-an-existing-virtual-network-for-managed-instances). 

   Yeni bir sanal ağ oluşturmayı planlıyorsanız bkz [yönetilen örneği için yeni sanal ağ oluştur](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Gereksinimler

Yönetilen örnek oluşturmak için aşağıdaki gereksinimlere uygun sanal ağ içindeki bir alt ağı ayırmanız gerekir:
- **Ayrılmış alt**: alt ağ ile ilişkili diğer bulut hizmeti içermemelidir ve ağ geçidi alt ağı olmamalıdır. Yönetilen örnek dışındaki kaynaklar içeren bir alt ağa yönetilen örneği oluşturma veya diğer kaynakları daha sonra alt ağ içinde eklemek mümkün olmayacaktır.
- **Hiçbir NSG**: alt ağ ile ilişkilendirilmiş bir ağ güvenlik grubu olmaması gerekir. 
- **Belirli bir yol tablonuz**: alt ağı olarak atanmış tek yolu 0.0.0.0/0 sonraki atlama Internet ile kullanıcı rota tablosu (UDR) olması gerekir. Daha fazla bilgi için [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)
3. **İsteğe bağlı bir özel DNS**: Azure'nın yinelemeli Çözümleyicileri IP adresi (168.63.129.16 gibi) VNet üzerinde özel DNS belirtilirse, listeye eklenmelidir. Daha fazla bilgi için [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
4. **Hizmet uç noktası yok**: alt ağ ile ilişkili bir hizmet uç noktası olmaması gerekir. Hizmet uç noktaları seçeneğini sanal ağ oluştururken devre dışı emin olun.
5. **Yeterli IP adresi**: alt ağın en az 16 IP adresi olmalıdır (en az 32 IP adresleri önerilir). Daha fazla bilgi için [yönetilen örnekler için alt ağ boyutunu belirler](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Hedef alt tüm önceki gereksinimleri ile uyumlu değilse, yeni yönetilen örneği dağıtmak mümkün olmayacaktır. Herhangi bir ihlali örneği hatalı durumuna girmek ve kullanılamaz duruma neden olabileceğinden ' % s'hedef sanal ağ ve alt ağı Bu yönetilen örneği gereksinimlerine uyacak şekilde (önce ve dağıtım sonrasında) tutulması gerekir. Gelen durumuna, uyumlu ağ ilkeleri ile bir sanal ağda yeni bir örneğini oluşturmak ihtiyaç duyduğu kurtarma, örnek düzeyi verileri yeniden oluşturmanız ve veritabanlarınızı geri yüklemek. Bu, uygulamalarınız için önemli kapalı kalma süresi sunar.

Giriş ile _Ağ hedefi İlkesi_, yönetilen örneği oluşturulduktan sonra bir yönetilen örnek alt ağında bir ağ güvenlik grubu (NSG) ekleyebilirsiniz.

Bir NSG artık, uygulamaları sorgulayın ve kullanıcıları 1433 bağlantı noktasına giden ağ trafiğini filtreleme yaparak verileri yönetmek IP aralıklarını daraltmak için de kullanabilirsiniz. 

> [!IMPORTANT]
> 1433 numaralı bağlantı noktasına erişim restrain NSG kurallarını yapılandırırken, aynı zamanda gelen kuralları aşağıdaki tabloda görüntülenen en yüksek öncelikli eklemeniz gerekir. Aksi halde Ağ hedefi ilkesi değişikliği uyumlu olmayan olarak engeller.

| AD       |BAĞLANTI NOKTASI                        |PROTOKOLÜ|KAYNAK           |HEDEF|EYLEM|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|Herhangi biri     |Herhangi biri              |Herhangi biri        |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |Herhangi biri        |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Herhangi biri        |İzin Ver |

0.0.0.0/0 sonraki atlama türü yanı sıra Internet yolu, UDR, şirket içi özel IP aralıkları ile sanal ağ geçidi veya sanal ağ Gereci (NVA) doğrultusunda trafiği yönlendirmek için artık ekleyebilmeniz yönlendirme deneyimi de geliştirilmiştir.

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

## <a name="create-a-new-virtual-network-for-managed-instance-using-azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımını kullanarak yönetilen örneğe'için yeni bir sanal ağ oluşturma

Oluşturma ve sanal ağ yapılandırma en kolay yolu, Azure Resource Manager dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. Kullanım **azure'a Dağıt** düğmesi sanal ağı Azure bulutunda dağıtmak için:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Bu düğme, yönetilen örneği dağıtabileceğiniz ağ ortamını yapılandırmak için kullanabileceğiniz bir form açılır.

  > [!Note]
  > Bu Azure Resource Manager şablonu, sanal ağı iki alt ağ ile dağıtır. Adlı bir alt ağ **ManagedInstances** yönetilen örnekler için ayrılmıştır ve diğer alt ağı adlı sırada yol tablosu, önceden yapılandırılmış **varsayılan** yönetilen erişmeli diğer kaynaklar için kullanılır Örneği (örneğin, Azure sanal makineler). Kaldırabilirsiniz **varsayılan** ihtiyacınız yoksa alt ağ.

3. Ağ ortamı yapılandırın. Aşağıdaki formda ağ ortamınızın parametreleri yapılandırabilirsiniz:

![Azure ağı yapılandırma](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

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

- Gerekli bir yol tablosu oluşturun ve ilişkilendirin: bkz [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- VNet oluşturmak için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
