---
title: 'Azure portal: SQL Yönetilen Örneği Oluşturma | Microsoft Docs'
description: Erişim için SQL Yönetilen Örneği, ağ ortamı ve istemci VM oluşturun.
keywords: sql veritabanı öğreticisi, sql yönetilen örneği oluşturma
services: sql-database
author: jovanpop-msft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: quickstart
ms.date: 08/13/2018
ms.author: jovanpop-msft
ms.openlocfilehash: cb378c2d2773096992ef688653fd77b2625f8754
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022714"
---
# <a name="create-an-azure-sql-managed-instance"></a>Azure SQL Yönetilen Örnek oluşturma

Bu hızlı başlangıçta Azure'da SQL Yönetilen Örneği oluşturma adımları gösterilmektedir. Azure SQL Veritabanı Yönetilen Örneği, Azure bulutunda yüksek oranda kullanılabilir SQL Server veritabanları çalıştırmanızı ve ölçeklendirmenizi sağlayan Hizmet Olarak Platform (PaaS) SQL Server Veritabanı Altyapısı Örneğidir. Bu hızlı başlangıçta, SQL veritabanı oluşturmaya başlama işlemleri gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="prepare-network-environment"></a>Ağ ortamını hazırlama

SQL Yönetilen Örneği, kendi Azure Sanal Ağınıza (VNet) yerleştirilen bir güvenli hizmettir. Yönetilen Örnek oluşturmak için aşağıdaki bileşenlerle Azure ağ ortamını hazırlamanız gerekir:
 - Yönetilen Örneğinizin yerleştirilecek Azure Sanal Ağı.
 - Yönetilen Örneklerin yerleştirileceği Azure Sanal Ağı alt ağı.
 - Yönetilen Örneğin, örneği denetleyen ve yöneten Azure hizmetleriyle iletişim kurmasını sağlayacak kullanıcı tanımlı rota.

Alt ağ, Yönetilen Örneklere ayrılmıştır ve bu alt ağda başka kaynak (Azure Sanal Makineleri gibi) oluşturamazsınız. Bu hızlı başlangıçta Azure Sanal Ağınızda iki alt ağ oluşturarak Yönetilen Örnekleri, Yönetilen Örnekler için ayrılmış alt ağa, diğer kaynakları da varsayılan alt ağa yerleştireceksiniz.

1. Aşağıdaki düğmeye tıklayarak Azure SQL Veritabanı Yönetilen Örneği için hazırlanmış olan Azure ağ ortamını dağıtabilirsiniz:

    <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener"> <img src="http://azuredeploy.net/deploybutton.png"> </a>

    Bu düğme Azure portalda dağıtım öncesinde ağ ortamınızı yapılandırabileceğiniz bir form açar.

2. İsteğe bağlı olarak sanal ağın ve alt ağların adını değiştirebilir ve ağ kaynaklarınızla ilişkili IP aralıklarını ayarlayabilirsiniz. Ardından "Satın al" düğmesine basarak ortamınızı oluşturabilir ve yapılandırabilirsiniz:

    ![yönetilen örnek ortamı oluşturma](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

    > [!Note]
    > Bu Azure Resource Manager dağıtımı, sanal ağda iki alt ağ oluşturur. **ManagedInstances** adlı alt ağ Yönetilen Örnekler için, **Default** adlı diğer ağ ise Yönetilen Örneğe bağlanmak için kullanılacak istemci sanal makinesi gibi diğer kaynaklar için kullanılır. İki alt ağa ihtiyacınız yoksa varsayılan alt ağı silebilirsiniz. Ancak bu durumda bu hızlı başlangıcın 3. adımındaki [istemci makinesini hazırlama](#prepare-client-machine) bölümünü tamamlayamazsınız.

    > [!Note]
    > Sanal ağın ve alt ağların adını değiştirirseniz yeni adları sonraki bölümlerde kullanmak üzere not etmeyi unutmayın. Öğreticinin geri kalan bölümünde **MyNewVNet** adlı bir sanal ağa, SQL Yönetilen Örnekleri için **ManagedInstances** adlı bir alt ağa ve sanal makinelerle diğer kaynaklar için **Default** adlı bir alt ağa sahip olduğunuz kabul edilecektir.

## <a name="create-a-managed-instance"></a>Yönetilen Örnek oluşturma

Aşağıdaki adımlar, önizlemeniz onaylandıktan sonra Yönetilen Örneğinizi oluşturma işlemini gösterir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yönetilen Örnek**’i bulup **Azure SQL Veritabanı Yönetilen Örneği (önizleme)** öğesini seçin.
3. **Oluştur**’a tıklayın.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

4. Aboneliğinizi seçin ve önizleme koşullarında **Kabul Edildi** ifadesinin gösterildiğini doğrulayın.

   ![yönetilen örnek önizlemesi kabul edildi](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Yönetilen Örnek formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyi rolü olduğundan "serveradmin" kullanmayın.| 
   |**Parola**|Geçerli bir parola|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Kaynak Grubu**|Önceden oluşturduğunuz kaynak grubu||
   |**Konum**|Daha önce seçtiğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Daha önce oluşturduğunuz sanal ağ| Önceki adımda adını değiştirmediyseniz **MyNewVNet/ManagedInstances** öğesini seçin. Aksi takdirde önceki bölümde girdiğiniz sanal ağ adını ve yönetilen örnek alt ağını seçin. **Yönetilen Örnekleri barındıracak şekilde yapılandırılmadığından varsayılan alt ağı kullanmayın**. |

   ![yönetilen örnek oluşturma formu](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. İşlem ve depolama kaynaklarını boyutlandırmaya ek olarak fiyatlandırma katmanı seçeneklerini gözden geçirmek için **Fiyatlandırma katmanı**’na tıklayın. Varsayılan olarak, örneğiniz ücretsiz 32 GB depolama alanı alır ancak **bu boyut uygulamalarınız için yeterli olmayabilir**.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. 
   ![yönetilen örnek fiyatlandırma katmanı](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Tamamlandığında, seçiminizi kaydetmek için **Uygula**’ya tıklayın.  
9. Yönetilen Örneği dağıtmak için **Oluştur**’a tıklayın.
10. Dağıtım durumunu görüntülemek için **Bildirimler** simgesine tıklayın.
 
   ![dağıtım ilerleme durumu](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Dağıtımın ilerleme durumunu daha ayrıntılı izlemek üzere Yönetilen Örnek penceresini açmak için **Dağıtım sürüyor**’a tıklayın.
 
   ![dağıtım ilerleme durumu 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Dağıtım gerçekleşirken sonraki yordama geçin.

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım süresi genellikle sonraki örneklerden çok daha uzundur, bazen tamamlanması 24 saatten uzun sürer. Dağıtım işlemini beklediğinizden daha uzun sürdüğü için iptal etmeyin. İlk örneğinizi dağıtma süresinin uzunluğu geçici bir durumdur. Genel önizlemenin başlamasından kısa süre sonra dağıtım süresinde önemli bir azalma olmasını bekleyebilirsiniz. Alt ağda ikinci Yönetilen Örneğin oluşturulması birkaç dakika sürer.

## <a name="prepare-client-machine"></a>İstemci makinesini hazırlama

SQL Yönetilen Örneği, özel Sanal Ağınızda bulunduğundan Yönetilen Örneğe bağlanmak ve sorgu yürütmek için SQL Server Management Studio veya SQL Operations Studio gibi SQL istemci araçlarının yüklü olduğu bir Azure sanal makinesi oluşturmanız gerekir.

> [!Note]
> İstemci Azure sanal makinesi yerine [Noktadan Siteye](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) bağlantı yapılandırabilir ve Yönetilen Örneğe yerel bilgisayarınızdan bağlanabilirsiniz.

Gerekli araçlara sahip bir istemci sanal makinesi oluşturmanın en kolay yolu, Azure Resource Manager şablonlarını kullanmaktır.

1. Aşağıdaki düğmeye tıklayın (başka bir tarayıcı sekmesinde Azure portal oturumu açtığınızdan emin olun):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Açılan forma sanal makinenin adını, yönetici kullanıcı adını ve VM bağlantısı için kullanacağınız parolayı girin.

    ![İstemci sanal makinesi oluşturma](./media/sql-database-managed-instance-get-started/create-client-sql-vm.png)

    Sanal ağ adını ve varsayılan alt ağı değiştirmediyseniz son iki parametreyi değiştirmeniz gerekmez. Aksi takdirde bu değerleri ağ ortamı kurulumu sırasında girdiğiniz değerlerle değiştirmeniz gerekir.

3. "Satın al" düğmesine bastığınızda Azure sanal makinesi hazırladığınız ağa dağıtılır.

4. Uzak Masaüstü bağlantısı kullanarak VM'nize bağlanın ve VM'ye otomatik olarak yüklenmiş olan SQL Server Management Studio veya SQL Operation Studio uygulamasını bulun.

5. SSMS uygulamasını açın ve **Sunucuya Bağlan** iletişim kutusundaki **Sunucu adı** kutusuna Yönetilen Örneğinizin **ana bilgisayar adını** girin, **SQL Server Kimlik Doğrulaması**'nı seçin, kullanıcı adınızı ve parolanızı girin ve **Bağlan**'a tıklayın.

    ![ssms bağlanma](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

 - [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
 - [Şirket içi veritabanlarınızı Yönetilen Örneğe geçirme](sql-database-managed-instance-migrate.md).


