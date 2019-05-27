---
title: Azure sanal ağlarına Azure Logic Apps'ten tümleştirme hizmeti ortamı (ISE) bağlanın.
description: Logic apps ve tümleştirme hesapları Azure sanal ağları (Vnet) erişebilmek için özel ve genel veya "Genel" azure'dan yalıtılmış kalsanız tümleştirme hizmeti ortamı (ISE) oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: b452485ccf235d1f245989e40840f2f0b3b2ae45
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544523"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Azure sanal ağlarına Azure Logic Apps'ten tümleştirme hizmeti ortamı (ISE) kullanarak bağlanma

Logic apps ve tümleştirme hesapları gereken yere erişimi senaryoları için bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md), oluşturun bir [ *tümleştirme hizmeti ortamı* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Bir işe ayrılmış depolama ve genel veya "Genel" Logic Apps hizmetinden ayrı tutulmasını diğer kaynakları kullanan özel ve ayrı bir ortamdır. Bu ayrım, diğer Azure kiracılarında, uygulamalarınızın performansını olabilir herhangi bir etkisi de azaltır. İŞE olan *eklenen* uygulamasına, Azure sanal ağına, daha sonra Logic Apps hizmetinin sanal ağınıza dağıtır. Bir mantıksal uygulama veya tümleştirme hesabı oluşturduğunuzda, bu işe kendi konum olarak seçin. Mantıksal uygulama veya tümleştirme hesabı, sanal makineleri (VM'ler), sunucular, sistemleri ve Hizmetleri, sanal ağınızda gibi kaynaklar doğrudan erişebilirsiniz.

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Bu makalede, bu görevleri tamamlamak gösterilmektedir:

* Tümleştirme hizmeti ortamı (ISE) üzerinden trafiği ilerleyebilir için sanal ağ içindeki alt ağlar arasındaki bağlantı noktaları Azure sanal ağınızda ayarlayın.

* Tümleştirme hizmeti ortamı (ISE) oluşturun.

* ISE'de çalıştırmak üzere bir mantıksal uygulama oluşturun.

* Tümleştirme hesabı için logic apps, ISE'de oluşturun.

Tümleştirme service ortamları hakkında daha fazla bilgi için bkz: [Azure Logic Apps için Azure sanal ağ kaynaklarına erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

  > [!IMPORTANT]
  > Logic apps, yerleşik tetikleyicileri, yerleşik Eylemler ve bağlayıcılar ISE kullanımınız fiyatlandırma planı tüketim tabanlı fiyatlandırma planından farklı çalıştır. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

* Bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir sanal ağınız yoksa, bilgi nasıl [bir Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md). 

  * Sanal ağınızı dört olmalıdır *boş* , ISE'de kaynakları oluşturma ve dağıtma için alt ağlar. Bu alt önceden oluşturabilirsiniz veya alt ağlar aynı anda oluşturabileceğiniz, işe oluşturana kadar bekleyebilirsiniz. Daha fazla bilgi edinin [alt ağ gereksinimleri](#create-subnet). 
  
    > [!NOTE]
    > Kullanırsanız [ExpressRoute](../expressroute/expressroute-introduction.md), Microsoft bulut hizmetlerine özel bir bağlantı sağlar, şunları yapmalısınız [yönlendirme tablosu oluşturma](../virtual-network/manage-route-table.md) aşağıdaki yönlendirmek ve bu tablo, işe tarafından kullanılan her alt ağ ile bağlantı vardır:
    > 
    > **Adı**: <*rota adı*><br>
    > **Adres ön eki**: 0.0.0.0/0<br>
    > **Sonraki atlama**: İnternet

  * Emin olun, sanal ağınızın [Bu bağlantı noktaları kullanılabilmesini](#ports) , işe düzgün şekilde çalışır ve erişilebilir kalır.

* Özel DNS sunucuları, Azure sanal ağı için kullanmak istiyorsanız [aşağıdaki adımları izleyerek bu sunucusu ayarlayabilir](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) sanal ağınıza, işe dağıtmadan önce. Aksi takdirde, DNS sunucunuzun değiştirdiğiniz her durumda, ayrıca, işe işe genel Önizleme sürümü ile kullanıma hazır bir özellik olan yeniden başlatmanız gerekir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Ağ bağlantı noktalarını ayarlayın

Erişilebilir kalmasını ve düzgün çalışması için tümleştirme hizmeti ortamı (ISE) belirli bağlantı noktalarını sanal ağınızda kullanılabilir olması gerekir. Aksi takdirde, bu bağlantı noktalarından birini kullanılamıyorsa, çalışmayı durdurabilir, işe için erişimi kaybedebilir. Bir sanal ağda bir işe kullandığınızda, engellenen bir veya daha fazla bağlantı noktaları ortak bir kurulum sorunu yaşıyor. İŞE hedef sistem arasındaki bağlantılar için kullandığınız bağlayıcı da kendi bağlantı noktası gereksinimleri olabilir. FTP Bağlayıcısı'nı kullanarak bir FTP sistemiyle iletişim kurmak, örneğin, üzerinde kullandığınız bağlantı noktası komutları göndermek için bağlantı noktası 21 gibi FTP sistemin kullanılabilir emin olun.

İŞE nerede dağıttığınız sanal ağın alt ağlar arasında trafiği denetlemek için ayarlayabileceğiniz [ağ güvenlik grupları](../virtual-network/security-overview.md) için bu alt ağları tarafından [alt ağlar arasında ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md). Bu tablo, işe kullanır ve bu bağlantı noktalarının kullanıldığı, sanal ağ bağlantı noktaları açıklar. [Resource Manager hizmet etiketleri](../virtual-network/security-overview.md#service-tags) güvenlik kuralı oluştururken karmaşıklığını en aza indirmenize yardımcı IP adresi ön eki grubunu temsil eder.

> [!IMPORTANT]
> Alt ağlarınızı içinde iç iletişimi için bu alt ağlardan içindeki tüm bağlantı noktaları açma ISE gerektirir.

| Amaç | Direction | Bağlantı Noktaları | Kaynak hizmeti etiketi | Hedef hizmeti etiketi | Notlar |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Azure Logic Apps gelen iletişimi | Giden | 80 & 443 | VirtualNetwork | İnternet | Bağlantı noktası ile iletişim kuran Logic Apps hizmetinin dış hizmete bağlıdır |
| Azure Active Directory | Giden | 80 & 443 | VirtualNetwork | AzureActiveDirectory | |
| Azure depolama bağımlılık | Giden | 80 & 443 | VirtualNetwork | Depolama | |
| İntersubnet iletişimi | Gelen ve giden | 80 & 443 | VirtualNetwork | VirtualNetwork | Alt ağlar arasındaki iletişim için |
| Azure Logic Apps ile iletişim | Gelen | 443 | İnternet  | VirtualNetwork | Herhangi bir istek tetikleyicisi veya mantıksal uygulamanızın mevcut Web kancası çağırır hizmet ve bilgisayar için IP adresi. Kapatma veya bu bağlantı noktası engelleyen istek Tetikleyicileri içeren mantıksal uygulamalar için HTTP çağrılarını engeller.  |
| Mantıksal uygulama çalıştırma geçmişi | Gelen | 443 | İnternet  | VirtualNetwork | Bilgisayar, mantıksal uygulamayı görüntülemek için IP adresi çalıştırma geçmişi. Kapatma ya da bu bağlantı noktası engellemelerini çalıştırma geçmişini görüntülemesini engellemez, ancak girişleri görüntüleyemezsiniz ve çıkışlar, her adımda için çalıştırma geçmişi. |
| Bağlantı Yönetimi | Giden | 443 | VirtualNetwork  | İnternet | |
| Tanılama günlükleri ve ölçümleri yayımlama | Giden | 443 | VirtualNetwork  | AzureMonitor | |
| Azure trafik Yöneticisi'nden iletişimi | Gelen | 443 | AzureTrafficManager | VirtualNetwork | |
| Logic Apps Tasarımcısı - dinamik özellikleri | Gelen | 454 | İnternet  | VirtualNetwork | İstekleri mantıksal uygulamalardan gelen [uç noktasına erişmek gelen IP adreslerini bu bölgede](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| App Service Management bağımlılık | Gelen | 454 & 455 | AppServiceManagement | VirtualNetwork | |
| Bağlayıcı dağıtımı | Gelen | 454 & 3443 | İnternet  | VirtualNetwork | Dağıtma ve bağlayıcıları güncelleştirme gerekli. Kapatma ya da bu bağlantı noktası engellemelerini ISE dağıtımları başarısız olmasına neden olur ve bağlayıcı güncelleştirmeler veya düzeltmeler önler. |
| Azure SQL bağımlılığı | Giden | 1433 | VirtualNetwork | SQL |
| Azure Kaynak Durumu | Giden | 1886 | VirtualNetwork | İnternet | Kaynak Durumu'nda sistem durumu yayımlamak için |
| API Yönetimi - yönetim uç noktası | Gelen | 3443 | APIManagement  | VirtualNetwork | |
| Olay hub'ı İlkesi ve İzleme Aracısı günlüğünden bağımlılığı | Giden | 5672 | VirtualNetwork  | EventHub | |
| Erişim Azure önbelleği için Redis örneği arasında rol örnekleri | Gelen <br>Giden | 6379-6383 | VirtualNetwork  | VirtualNetwork | Ayrıca, Redis için Azure önbellek ile çalışacak şekilde ISE için bunlar açmalısınız [Azure Cache Redis SSS açıklanan giden ve gelen bağlantı noktalarını](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Gelen | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE oluşturma

Tümleştirme hizmeti ortamı (ISE) oluşturmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **kaynak Oluştur**.
Arama kutusuna filtreniz olarak "tümleştirme hizmeti ortamı" girin.

   ![Yeni kaynak oluştur](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Tümleştirme hizmeti ortamı oluşturma bölmesinde **Oluştur**.

   !["Oluştur" öğesini seçin.](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları sağlayın ve ardından **gözden geçir + Oluştur**, örneğin:

   ![Ortam ayrıntılarını sağlayın](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Abonelik** | Evet | <*Azure-subscription-name*> | Ortamınız için kullanılacak Azure aboneliği |
   | **Kaynak grubu** | Evet | <*Azure kaynak grubu adı*> | Ortamınızı oluşturmak için istediğiniz Azure kaynak grubu |
   | **Tümleştirme hizmeti ortamı adı** | Evet | <*ortam adı*> | Ortamınızı verilecek ad |
   | **Konum** | Evet | <*Azure veri merkezi bölgesi*> | Azure veri merkezi bölgesini ortamınızı dağıtılacağı yeri |
   | **Ek kapasite** | Evet | 0 ile 10 | Bu işe kaynak için kullanılacak ek işleme birimi sayısı. Oluşturulduktan sonra Kapasite eklemek için bkz [ekleme ISE kapasite](#add-capacity). |
   | **Sanal ağ** | Evet | <*Azure sanal-ağ-adı*> | Mantıksal uygulamalar bu ortamda, sanal ağınızın erişebilmesi için ortamınızı eklemesine istediğiniz Azure sanal ağı. Bir ağ yoksa [öncelikle bir Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md). <p>**Önemli**: Yapabilecekleriniz *yalnızca* , işe oluşturduğunuzda bu ekleme gerçekleştirin. |
   | **Alt ağlar** | Evet | <*alt ağ kaynak listesi*> | Bir işe dört gerektirir *boş* ortamınızda kaynakları oluşturmak için alt ağlar. Her alt ağ oluşturmak için [bu tablonun altındaki adımları](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Alt ağ oluşturma**

   Ortamınızda kaynakları oluşturmak için işe dört gereken *boş* herhangi bir hizmeti temsilci olmayan alt ağlar. 
   *Olamaz* ortamınızı oluşturduktan sonra bu alt ağ adresleri değiştirin. Her alt ağ, şu ölçütleri karşılamalıdır:

   * Alfabetik bir karakter veya alt çizgi ile başlar ve şu karakterleri yok bir ada sahip: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Kullanan [sınıfsız etki alanları arası yönlendirme (CIDR) biçimi](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) ve sınıf B adres alanı.

   * En az bir kullanan `/27` adresini her alt ağa 32 adres olarak olması gerektiğinden boşluk *minimum*. Örneğin:

     * `10.0.0.0/27` 32 adres sahip 2<sup>(32-27)</sup> 2<sup>5</sup> veya 32.

     * `10.0.0.0/24` 256 adreslerine sahip 2<sup>(32-24)</sup> 2<sup>8</sup> veya 256.

     * `10.0.0.0/28` yalnızca 16 adresi vardır ve çok küçük olduğundan 2<sup>(32-28)</sup> 2<sup>4</sup> veya 16.

     Adresleri hesaplama hakkında daha fazla bilgi için bkz. [IPv4 CIDR blokları](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Kullanırsanız [ExpressRoute](../expressroute/expressroute-introduction.md), unutmayın [yönlendirme tablosu oluşturma](../virtual-network/manage-route-table.md) aşağıdaki yönlendirmek ve bu tablo, işe tarafından kullanılan her alt ağ ile bağlantı vardır:

     **Adı**: <*rota adı*><br>
     **Adres ön eki**: 0.0.0.0/0<br>
     **Sonraki atlama**: İnternet

   1. Altında **alt ağlar** listesinde **Yönet alt ağ yapılandırması**.

      ![Alt ağ yapılandırmasını yönet](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Üzerinde **alt ağlar** bölmesinde seçin **alt**.

      ![Alt ağ ekle](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Üzerinde **alt ağ Ekle** bölmesinde, bu bilgileri sağlayın.

      * **Ad**: Alt ağınız için bir ad
      * **Adres aralığı (CIDR bloğu)**: Sanal ağınızda bulunan ve CIDR biçimindeki alt ağın aralığı

      ![Alt ağ ayrıntıları ekleyin](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. İşiniz bittiğinde seçin **Tamam**.

   1. Üç daha fazla alt ağ için bu adımları yineleyin.

      > [!NOTE]
      > Alt ağlar oluşturmaya çalıştığınızda geçerli değilse, Azure portalında bir ileti gösterilir, ancak ilerleme durumunuzu engellemez.

1. Azure, işe bilgilerinizi başarıyla doğruladıktan sonra seçin **Oluştur**, örneğin:

   ![Doğrulama başarılı olduktan sonra "Oluştur" öğesini seçin.](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure başlar ancak bu işlem, ortamınızı dağıtma *olabilir* tamamlamadan önce iki saat sürebilir. 
   Azure, araç çubuğundaki dağıtım durumunu denetlemek için bildirimler bölmesi açılır bildirimler simgesini seçin.

   ![Dağıtım durumunu denetleyin](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla tamamlanırsa Azure bu bildirimi gösterilmektedir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   Aksi takdirde, dağıtım sorunlarını giderme için Azure portalı yönergeleri izleyin.

   > [!NOTE]
   > Dağıtım başarısız olursa veya size, işe silerseniz, Azure alabilir bir saat önce alt ağlarınızı serbest bırakma. Bu gecikme bu alt ağlardan başka bir işe yeniden kullanmadan önce beklemeniz gerekebilir anlamına gelir. 
   >
   > Sanal ağınızı silerseniz, Azure Genel alt ağlarınızı serbest önce iki saat sürer, ancak bu işlem uzun sürebilir. 
   > Sanal ağlar silerken kaynak hala bağlı olduğunuzdan emin olun. Bkz: [silme sanal ağ](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Ortamınızı görüntülemek için seçin **kaynağa Git** dağıtım tamamlandıktan sonra Azure ortamınıza otomatik olarak çıkmaz değil ise.  

Alt ağ oluşturma hakkında daha fazla bilgi için bkz. [bir sanal ağ alt ağı eklemek](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>ISE - mantıksal uygulama oluşturma

Tümleştirme service ortamı (ISE) içinde çalışan mantıksal uygulamalar oluşturmak için [her zamanki yolla logic apps oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) ayarladığınızda dışında **konumu** özelliği, ISE'den seçin  **Tümleştirme service ortamları** bölümünde, örneğin:

  ![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

Tetikleyiciler ve Eylemler iş ve genel Logic Apps hizmeti ile karşılaştırıldığında bir işe kullandığınızda nasıl etiketlendikten nasıl görürüm farklar için [karşı yalıtılmış işe genel bakış genel](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Tümleştirme hesabı - ISE oluşturma

Logic apps'te bir tümleştirme hizmeti ortamı (ISE) ile bir tümleştirme hesabı kullanmak istiyorsanız, bu tümleştirme hesabı kullanmalısınız *aynı ortam* mantıksal uygulamalar. Logic apps'te bir işe yalnızca tümleştirme hesapları aynı işe başvurabilirsiniz.

Bir işe kullandığı bir tümleştirme hesabı oluşturmak için [her zamanki yolla, tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ayarladığınızda dışında **konumu** özelliği, ISE'den seçin **tümleştirme Servis ortamları** bölümünde, örneğin:

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>ISE kapasite ekleyin

ISE temel birim kapasitesi, sabit daha fazla performans gerekiyorsa daha fazla ölçek birimi ekleyebilirsiniz. Performans ölçümleri temelinde veya ek işleme birimleri sayısına göre otomatik ölçeklendirme yapabilirsiniz. Ölçümlere göre otomatik ölçeklendirme seçerseniz, çeşitli ölçütler arasından seçim yapın ve bu ölçütlerine uyan eşiği koşulları belirtin.

1. Azure portalında, işe bulun.

1. Kullanım ve performans ölçümlerini, işe'nın ana menüsündeki, ISE için gözden geçirmek için seçin **genel bakış**.

   ![ISE için kullanımını görüntüleyin](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Otomatik ölçeklendirmeyi, altında ayarlanacak **ayarları**seçin **ölçeğini**. Üzerinde **yapılandırma** sekmesini, **etkinleştirmek otomatik ölçeklendirme**.

   ![Otomatik ölçeklendirme üzerinde Aç](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. İçin **otomatik ölçeklendirme ayarı adı**, ayarınız için bir ad sağlayın.

1. İçinde **varsayılan** bölümünde, ya da seçin **ölçek dayalı bir ölçüme göre** veya **belirli bir örnek sayısına ölçeklendirin**.

   * Örnek tabanlı seçerseniz, işleme birimlerinin sayısı 0 ile 10 arasında aralığında girin.

   * Ölçüm tabanlı seçerseniz, aşağıdaki adımları izleyin:

     1. İçinde **kuralları** bölümünde, seçin **alınabilecek**.

     1. Üzerinde **ölçek kuralı** bölmesinde, kural tetiklendiğinde gerçekleştirilecek, ölçütleri ve eylem ayarlayın.

     1. İşiniz bittiğinde seçin **Ekle**.

1. Otomatik ölçeklendirme ayarlarınızla tamamladığınızda değişikliklerinizi kaydedin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
