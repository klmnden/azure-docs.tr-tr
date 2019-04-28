---
title: Azure Databricks, sanal ağınızın (Önizleme) dağıtma
description: Bu makalede, Azure Databricks, sanal ağ olarak da bilinen sanal ağa ekleme dağıtmayı açıklar.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 03/18/2019
ms.openlocfilehash: 2db588a0cf67d7826408139e8facb43a2e897951
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62126690"
---
# <a name="deploy-azure-databricks-in-your-virtual-network-preview"></a>Azure Databricks, sanal ağınızın (Önizleme) dağıtma

Azure Databricks varsayılan dağıtımını azure'da tam olarak yönetilen bir hizmettir: kilitli kaynak grubuna bir sanal ağın (VNet) dahil olmak üzere tüm veri düzlemi kaynaklar dağıtılır. Ağ özelleştirmesi gerekiyorsa, ancak Azure Databricks kaynakları kendi sanal ağında (Ayrıca adlı VNet ekleme), ne zaman dağıtabileceğiniz sağlar:

* Azure Databricks (Azure depolama gibi) diğer Azure Hizmetleri için hizmet uç noktaları kullanarak daha güvenli bir şekilde bağlanın.
* Şirket içi veri kaynakları kullanmak için kullanıcı tanımlı yollar yararlanarak Azure Databricks ile bağlanın.
* Tüm giden trafiği incelemek ve ayarına göre eylemleri için bir ağ sanal Gereci için Azure Databricks'e bağlayarak izin verme ve reddetme kuralları.
* Azure Databricks, özel DNS kullanacak şekilde yapılandırın.
* Çıkış trafiği kısıtlamaları belirtmek için ağ güvenlik grubu (NSG) kurallarını yapılandırın.
* Var olan sanal ağınızda Azure Databricks kümelerini dağıtın.

Azure Databricks kaynakları kendi sanal ağına da olanak tanır esnek CIDR aralıkları yararlanmanıza (/16-arasında herhangi bir / 24 için sanal ağ arasında /18-/ 26 alt ağlar için).

  > [!NOTE]
  > Sanal ağ için mevcut bir çalışma alanı değiştirilemiyor. Geçerli çalışma alanı gerekli etkin küme düğümleri sayısı sağlayamazsa, daha büyük bir sanal ağda başka bir çalışma alanı oluşturun. İzleyin [bu geçiş adımlarını ayrıntılı](howto-regional-disaster-recovery.md#detailed-migration-steps) kaynaklar (not defterleri, küme yapılandırmaları, işleri) eski sürümden yeni çalışma alanına kopyalamak için.

## <a name="virtual-network-requirements"></a>Sanal ağ gereksinimleri

Azure portalında Azure Databricks çalışma alanı dağıtımı arabirimi otomatik olarak bir sanal ağınız gerekli alt ağlar, ağ güvenlik grubu ve beyaz listeye ekleme ayarlarını yapılandırmak için kullanabileceğiniz veya Azure Resource Manager'ı kullanabilirsiniz Sanal ağınızı yapılandırmak ve çalışma alanınızı dağıtmak için şablonlar.

Azure Databricks çalışma alanınıza dağıttığınız sanal ağı aşağıdaki gereksinimleri karşılaması gerekir:

### <a name="location"></a>Location

Sanal ağ, Azure Databricks çalışma alanı ile aynı konumda bulunmalıdır.

### <a name="subnets"></a>Alt ağlar

Sanal ağ, Azure Databricks için ayrılmış iki alt ağa eklemeniz gerekir:

   1. Yapılandırılmış ağ güvenlik grubuyla iç küme iletişim kurmasına olanak tanıyan özel bir alt ağ

   2. Bir genel alt yapılandırılan ağ güvenlik grubuyla Azure Databricks denetim düzlemi ile iletişime olanak sağlar.

### <a name="address-space"></a>Adres alanı

CIDR bloğu /16 /24 sanal ağ için CIDR bloğu /18 /26 özel ve Genel alt ağlar için arasında arasındaki.

### <a name="whitelisting"></a>Güvenilenler listesine eklenme

Alt ağlar ve Azure Databricks denetim düzlemi arasındaki tüm giden ve gelen trafiğe izin verilenler listesinde olmalıdır.

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

Bu bölümde, Azure portalında bir Azure Databricks çalışma alanı oluşturun ve kendi mevcut bir sanal ağda dağıtma işlemleri açıklanmaktadır. Azure Databricks, iki yeni alt ağlar ve sizin tarafınızdan sağlanan CIDR aralıkları kullanarak ağ güvenlik grupları ile sanal ağ güncelleştirmeleri, beyaz gelen ve giden alt ağ trafiği ve sanal ağ güncelleştirildi çalışma dağıtır.

## <a name="prerequisites"></a>Önkoşullar

Azure Databricks çalışma dağıtacağınız bir sanal ağ olması gerekir. Mevcut bir sanal ağ'ı kullanın veya yeni bir tane oluşturun, ancak sanal ağ oluşturmayı planladığınız Azure Databricks çalışma alanı ile aynı bölgede olması gerekir. Sanal ağ için CIDR aralığı /16 /24 arasında gereklidir.

  > [!Warning]
  > Daha küçük sanal ağ ile bir çalışma alanı – diğer bir deyişle, bir alt CIDR aralığı – çalıştırılana IP adresleri (ağ alanı) dışında bir çalışma alanı daha büyük bir sanal ağ ile daha hızlı. Örneğin, bir çalışma alanı ile bir/24 sanal ağ ve /26 alt ağlar, en fazla 64 düğüm teker teker etkin olabilir, ancak bir çalışma alanı bir /20 ile sanal ağ ve /22 alt ağlar, en fazla 1024 düğümlerinin barındırmak.

  Çalışma alanınızı yapılandırın ve CIDR aralığı için alt ağ yapılandırması sırasında sağlamak için bir fırsat sunulur, alt ağları otomatik olarak oluşturulur.

## <a name="configure-the-virtual-network"></a>Sanal ağ yapılandırma

1. Azure portalında **+ kaynak Oluştur > Analytics > Azure Databricks** Azure Databricks hizmeti iletişim kutusunu açın.

2. 2. adımda açıklanan yapılandırma adımlarını izleyin: Kullanmaya başlama Kılavuzu'nda bir Azure Databricks çalışma alanı oluşturma ve dağıtma Azure Databricks çalışma alanı, sanal ağ seçeneğini belirleyin.

   ![Azure Databricks hizmeti oluşturma](./media/vnet-injection/create-databricks-service.png)

3. Kullanmak istediğiniz sanal ağı seçin.

   ![Sanal ağ seçenekleri](./media/vnet-injection/select-vnet.png)

4. CIDR aralıkları arasında /18 /26 için iki alt ağ, Azure Databricks için ayrılmış bir blok içinde sağlayın:

   * Ortak bir alt ağ ile Azure Databricks denetim düzlemi ile iletişim kurmasına olanak tanıyan bir ilişkili ağ güvenlik grubu oluşturulur.
   * Özel bir alt küme iç iletişime izin veren bir ilişkili ağ güvenlik grubu ile oluşturulur.

5. Tıklayın **Oluştur** Azure Databricks çalışma alanı sanal ağa dağıtma.

## <a name="advanced-resource-manager-configurations"></a>Gelişmiş kaynak yöneticisi yapılandırmaları

Sanal ağ yapılandırmanız üzerinde daha fazla denetim istiyorsanız-örneğin, var olan alt ağları kullanın, var olan ağ güvenlik grupları kullanın veya kendi güvenlik kuralları – oluşturmak istediğiniz yerine aşağıdaki Azure Resource Manager şablonlarını kullanabilirsiniz Portal sanal ağ yapılandırması ve çalışma alanı dağıtımı.

### <a name="all-in-one"></a>Hepsi bir arada

Bir sanal ağ, ağ güvenlik grupları ve Azure Databricks çalışma alanı hepsi bir arada oluşturmak için kullanın [Databricks VNet eklenen çalışma alanları için hepsi bir arada şablon](https://azure.microsoft.com/resources/templates/101-databricks-all-in-one-template-for-vnet-injection/).

Bu şablonu kullandığınızda, her alt ağ trafiği el ile beyaz listeye ekleme yapmak gerekmez.

### <a name="network-security-groups"></a>Ağ güvenlik grupları

Bir sanal ağınız için gerekli kurallar ile ağ güvenlik grupları oluşturmak için kullanın [ağ güvenlik grubu şablonu için Databricks VNet ekleme](https://azure.microsoft.com/resources/templates/101-databricks-nsg-for-vnet-injection).

Bu şablonu kullandığınızda, her alt ağ trafiği el ile beyaz listeye ekleme yapmak gerekmez.

### <a name="virtual-network"></a>Sanal ağ

Uygun genel ve özel alt ağa sahip bir sanal ağ oluşturmak için kullanın [sanal ağ şablonu için Databricks VNet ekleme](https://azure.microsoft.com/resources/templates/101-databricks-vnet-for-vnet-injection).

Bu şablon aynı zamanda ağ güvenlik grupları şablonunu kullanmadan kullanırsanız, sanal ağ ile kullandığınız ağ güvenlik grupları için beyaz listeye ekleme kuralları el ile eklemelisiniz.

### <a name="azure-databricks-workspace"></a>Azure Databricks çalışma alanı

Bir Azure Databricks çalışma alanı ortak ve özel alt ağları ve düzgün bir şekilde yapılandırılmış bir ağ güvenlik grupları önceden kurulmuş olan mevcut bir sanal ağı dağıtmak için kullanın [çalışma alanı şablonu için Databricks VNet ekleme](https://azure.microsoft.com/resources/templates/101-databricks-workspace-with-vnet-injection).

Bu şablon aynı zamanda ağ güvenlik grupları şablonunu kullanmadan kullanırsanız, sanal ağ ile kullandığınız ağ güvenlik grupları için beyaz listeye ekleme kuralları el ile eklemelisiniz.

## <a name="whitelisting-subnet-traffic"></a>Beyaz listeye ekleme alt ağ trafiği

Kullanmıyorsanız, [Azure portalında](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-portal) veya [Azure Resource Manager şablonları](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html#vnet-inject-advanced) , ağ güvenlik grupları oluşturmak için el ile aşağıdaki trafik beyaz liste, alt ağlarda gerekir.

|Direction|Protokol|Kaynak|Kaynak Bağlantı Noktası|Hedef|Hedef Bağlantı Noktası|
|---------|--------|------|-----------|-----------|----------------|
|Gelen|\*|VirtualNetwork|\*|\*|\*|
|Gelen|\*|Denetim düzlemi NAT IP|\*|\*|22|
|Gelen|\*|Denetim düzlemi NAT IP|\*|\*|5557|
|Giden|\*|\*|\*|Webapp IP|\*|
|Giden|\*|\*|\*|SQL (hizmet etiketi)|\*|
|Giden|\*|\*|\*|Depolama (hizmet etiketi)|\*|
|Giden|\*|\*|\*|VirtualNetwork|\*|

Beyaz liste alt ağ trafiği kullanarak aşağıdaki IP adresleri. SQL (meta veri deposu) ve depolama (Depolama) ve günlük yapıt için Sql ve Depolama'ya kullanmalısınız [hizmet etiketleri](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

|Azure Databricks bölgesi|Hizmet|Genel IP|
|-----------------------|-------|---------|
|Doğu ABD|Denetim düzlemi NAT </br></br>WebApp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Doğu ABD 2|Denetim düzlemi NAT </br></br>WebApp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Orta Kuzey ABD|Denetim düzlemi NAT </br></br>WebApp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Orta ABD|Denetim düzlemi NAT </br></br>WebApp|23.101.152.95/32 </br></br>40.70.58.221/32|
|Orta Güney ABD|Denetim düzlemi NAT </br></br>WebApp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Batı ABD|Denetim düzlemi NAT </br></br>WebApp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Batı ABD 2|Denetim düzlemi NAT </br></br>WebApp|40.83.178.242/32 </br></br>40.118.174.12/32|
|Orta Kanada|Denetim düzlemi NAT </br></br>WebApp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Doğu Kanada|Denetim düzlemi NAT </br></br>WebApp|40.85.223.25/32 </br></br>13.71.184.74/32|
|Birleşik Krallık Batı|Denetim düzlemi NAT </br></br>WebApp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Birleşik Krallık Güney|Denetim düzlemi NAT </br></br>WebApp|51.140.203.27/32 </br></br>51.140.204.4/32|
|Batı Avrupa|Denetim düzlemi NAT </br></br>WebApp|23.100.0.135/32 </br></br>52.232.19.246/32|
|Kuzey Avrupa|Denetim düzlemi NAT </br></br>WebApp|23.100.0.135/32 </br></br>52.232.19.246/32|
|Orta Hindistan|Denetim düzlemi NAT </br></br>WebApp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Güney Hindistan|Denetim düzlemi NAT </br></br>WebApp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Batı Hindistan|Denetim düzlemi NAT </br></br>WebApp|104.211.89.81/32 </br></br>104.211.101.14/32|
|Güneydoğu Asya|Denetim düzlemi NAT </br></br>WebApp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Doğu Asya|Denetim düzlemi NAT </br></br>WebApp|52.187.0.85/32 </br></br>52.187.145.107/32|
|Avustralya Doğu|Denetim düzlemi NAT </br></br>WebApp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Avustralya Güneydoğu|Denetim düzlemi NAT </br></br>WebApp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Avustralya Orta|Denetim düzlemi NAT </br></br>WebApp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Avustralya Orta 2|Denetim düzlemi NAT </br></br>WebApp|13.70.105.50/32 </br></br>13.75.218.172/32|
|Japonya Doğu|Denetim düzlemi NAT </br></br>WebApp|13.78.19.235/32 </br></br>52.246.160.72/32|
|Japonya Batı|Denetim düzlemi NAT </br></br>WebApp|13.78.19.235/32 </br></br>52.246.160.72/32|

## <a name="troubleshooting"></a>Sorun giderme

### <a name="workspace-launch-errors"></a>Çalışma alanı başlatma hataları

Azure Databricks üzerinde özel bir sanal ağdaki bir çalışma alanı başlatma oturum açma ekranı şu hatayla başarısız olur: **"Çalışma alanınızı oluştururken hatayla karşılaştık. Özel ağ yapılandırmasının doğru olduğundan emin olun ve yeniden deneyin."**

Gereksinimleri karşılamayan bir ağ yapılandırması tarafından bu hataya neden olur. Çalışma alanı oluşturduğunuzda bu konudaki yönergeleri izleyen onaylayın.

### <a name="cluster-creation-errors"></a>Küme oluşturma hataları

**Erişilemeyen örnekleri: Kaynakları SSH erişilebilir değil.**

Olası neden: çalışanları için Denetim düzlemi gelen trafik engellenir. Gelen güvenlik kuralları gereksinimlerini karşıladığını sağlayarak düzeltin. Şirket içi ağınıza bağlı olarak var olan bir sanal ağa dağıtıyorsanız, Azure Databricks çalışma alanınız şirket içi ağınıza bağlama içinde sağlanan bilgileri kullanarak ayarlarınızı gözden geçirin.

**Beklenmeyen bir başlatma hatası: Kümeyi oluşturan ayarlanırken beklenmeyen bir hatayla karşılaşıldı. Yeniden deneyin ve sorun devam ederse Azure Databricks ile iletişime geçin. İç hata iletisi: Düğüm yerleştirme sırasında zaman aşımı.**

Olası neden: Azure depolama uç noktaları çalışanları giden trafik engellenir. Giden güvenlik kuralları gereksinimlerini karşıladığını sağlayarak düzeltin. Ayrıca özel DNS sunucuları kullanıyorsanız, sanal Ağınızda DNS sunucularının durumunu denetleyin.

**Bulut sağlayıcısı başlatma hatası: Bir bulut Sağlayıcısı hatası küme ayarlarken hatayla karşılaşıldı. Daha fazla bilgi için Azure Databricks kılavuzuna bakın. Azure hata kodu: AuthorizationFailed/InvalidResourceReference.**

Olası neden: sanal ağ veya alt ağlara artık mevcut değil. Alt ağlar ve sanal ağın var olduğundan emin olun.

**Küme sonlandırıldı. Neden: Spark başlatma hatası: Spark başlangıç zamanı mümkün değildi. Bu sorun, düzgün çalışmayan bir Hive meta veri deposu, geçersiz Spark yapılandırmaları veya yapıyor init betikleri tarafından kaynaklanabilir. Bu sorunu gidermek için Spark sürücüsü günlüklere bakın ve sorun devam ederse, Databricks başvurun. İç hata iletisi: Spark başlatılamadı: Sürücü zamanında başlatılamadı.**

Olası neden: Kapsayıcı barındırma örneği veya DBFS depolama hesabına konuşamaz. Alt ağlara DBFS depolama hesabı için özel bir yolu olan Internet sonraki atlama ile ekleyerek düzeltin.

### <a name="notebook-command-errors"></a>Not defteri komut hataları

**Komut yanıt vermiyor**

Olası neden: çalışan alt iletişimi engellenir. Gelen güvenlik kuralları gereksinimlerini sağlayarak düzeltin.

**Not Defteri iş akışı şu özel durum ile başarısız oluyor: com.databricks.WorkflowException: org.apache.http.conn.ConnectTimeoutException**

Olası neden: Azure Databricks Webapp çalışanları gelen trafik engellenir. Giden güvenlik kuralları gereksinimlerini sağlayarak düzeltin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Databricks kullanarak verileri ayıklama, dönüştürme ve yükleme](databricks-extract-load-sql-data-warehouse.md)
