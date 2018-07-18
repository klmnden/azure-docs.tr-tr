---
title: Azure SQL veritabanı yönetilen örnek sanal ağ yapılandırması | Microsoft Docs
description: Bu konuda, bir Azure SQL veritabanı yönetilen örneği ile bir sanal ağın (VNet) için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: dbd747fd3ec53b1221536609d6355ff5b4691977
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091613"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma

Azure SQL veritabanı yönetilen örneği (Önizleme) içinde bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak tanır: 
- Bir şirket içi ağdan doğrudan bir yönetilen örneğe bağlanma 
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket 
- Azure kaynakları için yönetilen örneğe bağlanma  

## <a name="plan"></a>Planlama

Bir yönetilen örneği'nde aşağıdaki sorulara verdiğiniz yanıtlar kullanarak sanal ağ dağıtımı planlama: 
- Tek veya birden çok yönetilen örnekler dağıtmayı planlıyor musunuz? 

  Yönetilen örnek sayısı minimum yönetilen örnekleriniz için ayrılacak alt ağ boyutunu belirler. Daha fazla bilgi için [yönetilen örneği için alt ağ boyutunu belirlemek](#create-a-new-virtual-network-for-managed-instances). 
- Yönetilen örneğiniz mevcut bir sanal ağa dağıtmak ihtiyacınız veya yeni bir ağ oluşturuyorsunuz? 

   Mevcut bir sanal ağı kullanmayı planlıyorsanız, yönetilen Örneğinize uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için [yönetilen örneği için var olan sanal ağı değiştirme](#modify-an-existing-virtual-network-for-managed-instances). 

   Yeni bir sanal ağ oluşturmayı planlıyorsanız bkz [yönetilen örneği için yeni sanal ağ oluştur](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Gereksinimler

Yönetilen örnek oluşturmak için aşağıdaki gereksinimlere uygun sanal ağ içindeki alt ağ ayırmanız:
- **Boş olması**: alt ağ ile ilişkili diğer bulut hizmeti içermemelidir ve ağ geçidi alt ağı olmamalıdır. Yönetilen örnek dışındaki kaynaklar içeren bir alt ağa yönetilen örneği oluşturma veya diğer kaynakları daha sonra alt ağ içinde eklemek mümkün olmayacaktır.
- **Hiçbir NSG**: alt ağ ile ilişkilendirilmiş bir ağ güvenlik grubu olmaması gerekir.
- **Belirli bir yol tablonuz**: alt ağı olarak atanmış tek yolu 0.0.0.0/0 sonraki atlama Internet ile kullanıcı rota tablosu (UDR) olması gerekir. Daha fazla bilgi için [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)
3. **İsteğe bağlı bir özel DNS**: Azure'nın yinelemeli Çözümleyicileri IP adresi (168.63.129.16 gibi) VNet üzerinde özel DNS belirtilirse, listeye eklenmelidir. Daha fazla bilgi için [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
4. **Uç nokta**: alt ağ ile ilişkili bir hizmet uç noktası (depolama veya Sql) olmamalıdır. Hizmet uç noktaları seçeneği sanal ağ oluştururken devre dışı emin olun.
5. **Yeterli IP adresi**: alt ağı en az 16 IP adresleri olması gerekir. Daha fazla bilgi için [yönetilen örnekler için alt ağ boyutunu belirler](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Hedef alt tüm önceki gereksinimleri ile uyumlu değilse, yeni yönetilen örneği dağıtmak mümkün olmayacaktır. Herhangi bir ihlali örneği hatalı durumuna girmek ve kullanılamaz duruma neden olabileceğinden ' % s'hedef sanal ağ ve alt ağı Bu yönetilen örneği gereksinimlerine uyacak şekilde (önce ve dağıtım sonrasında) tutulması gerekir. Gelen durumuna, uyumlu ağ ilkeleri ile bir sanal ağda yeni bir örneğini oluşturmak ihtiyaç duyduğu kurtarma, örnek düzeyi verileri yeniden oluşturmanız ve veritabanlarınızı geri yüklemek. Bu, uygulamalarınız için önemli kapalı kalma süresi sunar.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Yönetilen örnek için alt ağ boyutunu belirler

Yönetilen bir örneği oluşturduğunuzda, Azure sanal makineler sağlama sırasında seçtiğiniz katman boyutuna bağlı olarak bir dizi ayırır. Bu sanal makineler, alt ağ ile ilişkili olduğundan, bunlar IP adresi gerektirir. Normal işlemler ve hizmet bakım sırasında yüksek kullanılabilirlik sağlamak için Azure ek sanal makineler ayırabilir. Sonuç olarak, alt ağ gerekli IP adresi sayısı bu alt ağdaki yönetilen örnekler sayısından büyüktür. 

Tasarıma göre yönetilen örneğe en az 16 IP adresleri bir alt ağda olmalıdır ve en fazla 256 IP adresi kullanıyor olabilirsiniz. Sonuç olarak, alt ağ IP aralıkları tanımlarken, alt ağ maskeleri için/28'i /24 kullanabilirsiniz. 

Birden çok yönetilen örnek alt ağ içinde dağıtın ve alt ağı boyutuna göre en iyi duruma getirmeyi planlıyorsanız, bir hesaplama oluşturmak için şu parametreleri kullan: 

- Azure alt ağdaki beş IP adreslerini, kendi gereksinimleriniz için kullanır. 
- Genel amaçlı örneği her iki adres olması gerekir 
- Her bir iş açısından kritik örneği dört adres olması gerekir

**Örnek**: üç genel amaçlı ve iki iş açısından kritik yönetilen örneği planlama. 5 + 3 * 2 + 2 * 4 = 19 ihtiyacınız anlamına gelir IP adreslerini. IP aralıklarını 2'in gücünü tanımlanan 32 IP aralığı gerekir (2 ^ 5) IP adresi. Bu nedenle, / 27 alt ağ maskesine sahip bir alt ağı ayırmanız gerekir. 

## <a name="create-a-new-virtual-network-for-managed-instances"></a>Yönetilen örnek için yeni bir sanal ağ oluşturma 

Bir Azure sanal ağı oluşturma, bir yönetilen örnek oluşturmak için bir önkoşuldur. Azure portalını kullanabilir [PowerShell](../virtual-network/quick-create-powershell.md), veya [Azure CLI](../virtual-network/quick-create-cli.md). Aşağıdaki bölümde, Azure portalını kullanarak adımları gösterilmektedir. Burada tartışılan ayrıntıları bu yöntemlerin her biri için geçerlidir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Sanal Ağ**’ı bulup tıklayın, dağıtım modu olarak **Resource Manager**’ın seçili olduğunu doğrulayın ve ardından **Oluştur**’a tıklayın.

   ![sanal ağ oluşturma](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Sanal ağ formunu aşağıdaki ekran görüntüsüne benzer şekilde istenen bilgilerle doldurun:

   ![sanal ağ oluşturma formu](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. **Oluştur**’a tıklayın.

   Adres alanı ve alt ağ CIDR gösteriminde belirtilir. 

   > [!IMPORTANT]
   > Varsayılan değerleri alan tüm VNet adres alanı alt ağ oluşturun. Bu seçeneği tercih ederseniz yönetilen örnekten başka sanal ağ içindeki diğer tüm kaynakları oluşturulamıyor. 

   Önerilen yaklaşım, şunlar olur: 
   - Aşağıdaki alt ağ boyutunu hesaplamak [yönetilen örneği için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümü  
   - VNet geri kalanı için gereksinimleri değerlendirme 
   - VNet ve alt ağ adresi aralığı uygun şekilde doldurun 

   Hizmet uç noktaları kalır seçeneği emin **devre dışı bırakılmış**. 

   ![sanal ağ oluşturma formu](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

## <a name="create-the-required-route-table-and-associate-it"></a>Gerekli bir yol tablosu oluşturun ve ilişkilendirin

1. Azure portalında oturum açın  
2. **Yol tablosu**’nu bulup tıklayın ve ardından Yol tablosu sayfasında **Oluştur**’a tıklayın.

   ![yol tablosu oluşturma formu](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. 0.0.0.0/0 sonraki atlama Internet yolu, aşağıdaki ekran görüntüleri gibi bir şekilde oluşturun:

   ![yol tablosu ekleme](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![yol](./media/sql-database-managed-instance-tutorial/route.png)

4. Bu yol, yönetilen örnek, aşağıdaki ekran görüntüleri gibi bir şekilde için alt ağı ile ilişkilendirin:

    ![alt ağ](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![yol tablosu ayarlama](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![yol tablosu ayarlama-kaydetme](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Ağınız oluşturulduktan sonra yönetilen Örneğinize oluşturmaya hazırsınız.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Yönetilen örnek için var olan bir sanal ağı değiştirme 

Sorular ve cevaplar Bu bölümde bir yönetilen örnek mevcut bir sanal ağa ekleme işlemini göstermektedir. 

**Varolan bir sanal ağ için Klasik veya Resource Manager dağıtım modelini kullanıyor musunuz?** 

Bu gibi durumlarda, bir yönetilen örneği yalnızca Resource Manager sanal ağları oluşturabilirsiniz. 

**SQL yönetilen örneği için yeni bir alt ağ oluşturun veya var olan bir kullanmak ister misiniz?**

Yeni bir tane oluşturmak isterseniz: 

- Yönergeleri izleyerek alt ağ boyutunu hesaplamak [yönetilen örnekler için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümü.
- İzleyeceğiniz adımlar [ekleme, değiştirme veya silme bir sanal ağ alt](../virtual-network/virtual-network-manage-subnet.md). 
- Tek giriş içeren bir yol tablosu oluşturma **0.0.0.0/0**, sonraki atlama Internet ve yönetilen örneği için alt ağ ile ilişkilendirebilirsiniz.  

Var olan bir alt ağ içinde yönetilen bir örneğini oluşturmak istediğiniz durumlarda: 
- Ağ geçidi alt ağı dahil olmak üzere diğer kaynakları içeren bir alt ağda bir yönetilen örnek alt boşsa - oluşturulamıyor denetleyin 
- Yönergeleri izleyerek alt ağ boyutunu hesaplamak [yönetilen örnekler için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümünde ve uygun şekilde boyutlandırıldığından emin olun. 
- Hizmet uç noktaları alt ağda etkin olmadığından emin denetleyin.
- Alt ağ ile ilişkilendirilmiş ağ güvenlik grubu yok olduğundan emin olun 

**Yapılandırılmış özel DNS sunucusu gerekiyor?** 

Yanıt Evet ise bkz [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). 

- Gerekli bir yol tablosu oluşturun ve ilişkilendirin: bkz [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- VNet oluşturmak için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
