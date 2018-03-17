---
title: "Azure SQL veritabanı örneği VNet yapılandırmasını yönetilen | Microsoft Docs"
description: "Bu konuda, yönetilen bir Azure SQL veritabanı örneği ile bir sanal ağ (VNet) için yapılandırma seçenekleri açıklanmaktadır."
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: e724a660f8ba2373cefdabe8595908b7bb42f4d6
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma

Azure SQL veritabanı örneği (Önizleme) yönetilen bir Azure içinde dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak sağlar: 
- Yönetilen bir örneğine doğrudan bağlanma formu bir şirket içi ağı 
- Bağlantılı sunucu veya başka bir yönetilen örneği bağlanma veri deposu şirket içi 
- Yönetilen bir örneğini Azure kaynaklarına bağlanma  

## <a name="plan"></a>Planlama

Yönetilen bir örneği aşağıdaki sorulara verdiğiniz yanıtlar kullanarak sanal ağ içinde dağıtımı planlama: 
- Tek veya birden çok yönetilen örnekleri dağıtmayı planlıyorsunuz? 

  Yönetilen örnek sayısı için yönetilen örneklerinizi ayırmak için alt minimum boyutunu belirler. Daha fazla bilgi için bkz: [yönetilen örneği için alt ağ boyutunu belirlemek](#create-a-new-virtual-network-for-managed-instances). 
- Yönetilen örneğinizi mevcut bir sanal ağı dağıtmak gerekiyor mu yoksa yeni bir ağ oluşturmak? 

   Varolan bir sanal ağı kullanmayı planlıyorsanız, yönetilen örneğinizi uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için bkz: [yönetilen örneği için var olan sanal ağı değiştirmek](#modify-an-existing-virtual-network-for-managed-instances). 

   Yeni sanal ağ oluşturmayı planlıyorsanız, bkz: [yönetilen örneği için yeni sanal ağ oluştur](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Gereksinimler

Yönetilen örneği oluşturmak için aşağıdaki gereksinimlere uygun sanal ağ içindeki alt ağ ayrılması:
- **Boş olması**: alt ağ için ilişkilendirilmiş diğer bulut hizmeti içermemelidir ve ağ geçidi alt ağı olmamalıdır. Yönetilen örneği yönetilen örneği dışında kaynakları içeren bir alt ağ oluşturun veya daha sonra alt ağ içindeki diğer kaynakların eklemek mümkün olmayacaktır.
- **Hiçbir NSG**: alt ağ ile ilişkilendirilmiş ağ güvenlik grubu olmaması gerekir.
- **Özel yol tablosunda**: alt ağ, kendisine atanmış yalnızca rota olarak bir kullanıcı rota tablosu (UDR) 0.0.0.0/0 sonraki atlama Internet ile olması gerekir. Daha fazla bilgi için bkz: [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)
3. **İsteğe bağlı özel DNS**: özel DNS VNet üzerinde belirtilirse, Azure'nın özyinelemeli çözümleyiciler IP adresi (örneğin, 168.63.129.16) listesine eklenmesi gerekir. Daha fazla bilgi için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
4. **Hiçbir hizmet uç noktası**: alt ağ için ilişkili bir hizmet uç noktası (depolama veya Sql) sahip olmamalıdır. Hizmet uç noktaları seçeneği VNet oluştururken devre dışı bırakıldığından emin olun.
5. **Yeterli IP adresine**: alt 16 IP adresini en az olmalıdır. Daha fazla bilgi için bkz: [yönetilen örnekleri için alt ağ boyutunu belirleme](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Hedef alt ağdaki tüm önceki gereksinimleri ile uyumlu değilse, yeni yönetilen örneğini dağıtmak mümkün olmayacaktır. Herhangi bir ihlali örneği hatalı durum girip kullanılamaz duruma neden olabileceğinden hedef Vnet ve alt ağı Bu yönetilen örneği gereksinimlerine uyacak şekilde (önce ve sonra dağıtım) tutulması gerekir. Gelen durumu ilkeleriyle uyumlu ağ sanal ağda yeni bir örneği oluşturmanızı gerektirir kurtarma, örnek düzeyinde veri yeniden oluşturun ve veritabanlarınızı geri yükleyin. Bu, uygulamalarınız için önemli kapalı kalma süresi sunar.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Yönetilen örnekleri için alt ağ boyutunu belirleme

Yönetilen bir örneği oluşturduğunuzda, Azure sanal makine sağlama sırasında seçtiğiniz katman boyutu bağlı olarak bir dizi ayırır. Bu sanal makine, alt ağ ile ilişkili olduğundan, bunlar IP adresi gerektirir. Normal işlemler ve hizmet bakım sırasında yüksek kullanılabilirlik sağlamak için Azure ek sanal makineler ayırabilir. Sonuç olarak, bir alt ağ gerekli IP adreslerini bu alt ağdaki yönetilen örnekleri sayısından büyük sayısıdır. 

Tasarıma göre yönetilen bir örneği 16 IP adresini bir alt ağda en az gerekir ve en fazla 256 IP adreslerini kullanabilir. Sonuç olarak, alt ağ IP aralıklarını tanımlarken, alt ağ maskelerinin /28 için /24 kullanabilirsiniz. 

Alt ağ içindeki birden çok yönetilen örnekleri dağıtma ve alt ağ boyutu en iyi duruma getirmek gereken planlıyorsanız, bir hesaplama oluşturmak için şu parametreleri kullan: 

- Azure beş IP adresi alt ağdaki kendi gereksinimleriniz için kullanır. 
- Her bir genel amaçlı örneği iki adres olması gerekir 

**Örnek**: sekiz yönetilen örnekleri sahip olmayı planladığınız. 5 + 8 * 2 = 21 ihtiyacınız anlamına gelir IP adresleri. IP aralıklarını 2 güç içinde tanımlanan 32 IP aralığını gerekir (2 ^ 5) IP adresi. Bu nedenle, alt ağ maskesini/27 ile ayırmanız gerekir. 

## <a name="create-a-new-virtual-network-for-managed-instances"></a>Yönetilen örnekleri için yeni bir sanal ağ oluşturma 

Bir Azure sanal ağı oluşturma, yönetilen bir örneğini oluşturmak için bir önkoşuldur. Azure portalını kullanabilirsiniz [PowerShell](../virtual-network/quick-create-powershell.md), veya [Azure CLI](../virtual-network/quick-create-cli.md). Aşağıdaki bölümde Azure Portalı'nı kullanarak adımları gösterir. Burada tartışılan ayrıntılar bu yöntemlerin her biri için geçerlidir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. Bulun ve ardından **sanal ağ**, doğrulama **Resource Manager** dağıtım modu olarak seçilir ve ardından **oluşturma**.

   ![sanal ağ oluşturma](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Aşağıdaki ekran görüntüsüne benzer şekilde istenen bilgilerle sanal ağ formu doldurun:

   ![sanal ağ oluşturma formu](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. **Oluştur**’a tıklayın.

   Adres alanı ve alt ağ CIDR gösteriminde belirtilir. 

   > [!IMPORTANT]
   > Varsayılan değerleri tüm VNet adres alanı alt ağ oluşturun. Bu seçeneği seçerseniz, yönetilen örneği dışındaki sanal ağ içindeki başka kaynağı oluşturulamıyor. 

   Önerilen yaklaşım şöyle olur: 
   - Alt ağ boyutunu izleyerek hesaplamak [yönetilen örneği için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümü  
   - VNet geri kalanı için gereksinimlerini değerlendirin 
   - VNet ve alt ağ adres aralıklarını uygun şekilde doldurun 

   Hizmet uç noktaları kalır seçeneği emin olun **devre dışı**. 

   ![sanal ağ oluşturma formu](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

## <a name="create-the-required-route-table-and-associate-it"></a>Gerekli yol tablosu oluşturun ve ilişkilendirin

1. Azure portalında oturum açın  
2. Bulun ve ardından **yol tablosu**ve ardından **oluşturma** rota tablosu sayfasında.

   ![yol tablosu formu oluşturma](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Aşağıdaki ekran görüntüleri gibi bir şekilde 0.0.0.0/0 sonraki atlama Internet rota oluşturun:

   ![yol tablosu ekleme](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![Rota](./media/sql-database-managed-instance-tutorial/route.png)

4. Bu yol, yönetilen Örneğin, aşağıdaki ekran görüntüleri gibi bir şekilde alt ağ ile ilişkilendirin:

    ![alt ağ](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![Rota tablo Ayarla](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![set rota tablosu-Kaydet](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Sanal ağınızı oluşturduktan sonra yönetilen örneğinizi oluşturmaya hazırsınız.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Yönetilen örnekleri için varolan bir sanal ağı değiştirme 

Sorular ve yanıtlar bu bölümde, varolan bir sanal ağa yönetilen bir örneğini ekleme gösterir. 

**Varolan bir sanal ağ için Klasik veya Resource Manager dağıtım modeli kullanıyor musunuz?** 

Bu gibi durumlarda, yönetilen bir örneği yalnızca Resource Manager sanal ağlarda oluşturabilirsiniz. 

**SQL yönetilen örneği için yeni bir alt ağ oluşturun veya varolan bir kullanmak ister misiniz?**

Yeni bir tane oluşturmak isterseniz: 

- Yönergeleri izleyerek alt ağ boyutu hesaplanamadı [yönetilen örnekleri için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümü.
- İzleyeceğiniz adımlar [ekleme, değiştirme veya silme bir sanal ağ alt](../virtual-network/virtual-network-manage-subnet.md). 
- Tek giriş içeren bir yol tablosu oluşturmanız **0.0.0.0/0**, sonraki atlama Internet ve yönetilen örneği için alt ağ ile ilişkilendirin.  

Yönetilen bir örneğinin var olan bir alt ağ içinde oluşturmak istediğiniz durumda: 
- Ağ geçidi alt ağı içeren diğer kaynakları içeren bir alt ağ alt boşsa - yönetilen bir örnek oluşturulamıyor denetleyin 
- Yönergeleri izleyerek alt ağ boyutu hesaplanamadı [yönetilen örnekleri için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) bölümünde ve uygun şekilde boyutlandırılmış doğrulayın. 
- Hizmet uç noktaları alt ağda etkin olmadığını denetleyin.
- Alt ağla ilişkili ağ güvenlik grubu yok emin olun 

**Yapılandırılmış özel DNS sunucusu var mı?** 

Yanıt Evet ise, bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). 

- Gerekli yol tablosu oluşturun ve ilişkilendirin: bkz [gerekli yol tablosu oluşturun ve ilişkilendirin](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Sonraki adımlar

- Genel bir bakış için bkz: [yönetilen örneği nedir](sql-database-managed-instance.md)
- Bir VNet oluşturun, yönetilen bir örneği oluşturun ve bir veritabanını bir veritabanı yedeklemeden geri yüklemek nasıl gösteren bir öğretici için bkz: [yönetilen bir Azure SQL veritabanı örneği oluşturmanızı](sql-database-managed-instance-tutorial-portal.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
