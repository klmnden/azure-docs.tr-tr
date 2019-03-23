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
ms.date: 03/12/2019
ms.openlocfilehash: 9cb3abff10482ec7e58b4b049f051e99178cb742
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371995"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Azure sanal ağlarına Azure Logic Apps'ten tümleştirme hizmeti ortamı (ISE) kullanarak bağlanma

> [!NOTE]
> Bu özellik bulunduğu [ *genel Önizleme*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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
  > Logic apps, yerleşik Eylemler ve bağlayıcılar, ISE'de çalıştıran farklı bir fiyatlandırma planı, değil tüketim tabanlı fiyatlandırma planı kullanın. Daha fazla bilgi için [Logic Apps fiyatlandırma](../logic-apps/logic-apps-pricing.md).

* Bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir sanal ağınız yoksa, bilgi nasıl [bir Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md). 

  * Sanal ağınızı dört olmalıdır *boş* , ISE'de kaynakları oluşturma ve dağıtma için alt ağlar. Bu alt önceden oluşturabilirsiniz veya alt ağlar aynı anda oluşturabileceğiniz, işe oluşturana kadar bekleyebilirsiniz. Daha fazla bilgi edinin [alt ağ gereksinimleri](#create-subnet). 
  
    > [!NOTE]
    > Kullanırsanız [ExpressRoute](../expressroute/expressroute-introduction.md), Microsoft bulut hizmetlerine özel bir bağlantı sağlar, şunları yapmalısınız [her alt ağa aşağıdaki yolu Ekle](../virtual-network/virtual-network-manage-subnet.md) , işe tarafından kullanılır. Alt ağa sahip bir yol tablosu kullanırsanız [yol tablonuz aşağıdaki yolu Ekle](../virtual-network/manage-route-table.md):
    > 
    > **Ad**: D3655BASE yönlendirme<br>
    > **Adres ön eki**: 0.0.0.0/0<br>
    > **Sonraki atlama**: Internet

  * Emin olun, sanal ağınızın [Bu bağlantı noktaları kullanılabilmesini](#ports) , işe düzgün şekilde çalışır ve erişilebilir kalır.

* Özel DNS sunucuları, Azure sanal ağı için kullanmak istiyorsanız [aşağıdaki adımları izleyerek bu sunucusu ayarlayabilir](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) sanal ağınıza, işe dağıtmadan önce. Aksi takdirde, DNS sunucunuzun değiştirdiğiniz her durumda, ayrıca, işe işe genel Önizleme sürümü ile kullanıma hazır bir özellik olan yeniden başlatmanız gerekir.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="set-up-network-ports"></a>Ağ bağlantı noktalarını ayarlayın

Erişilebilir kalmasını ve düzgün çalışması için tümleştirme hizmeti ortamı (ISE) belirli bağlantı noktalarını sanal ağınızda kullanılabilir olması gerekir. Aksi takdirde, bu bağlantı noktalarından birini kullanılamıyorsa, çalışmayı durdurabilir, işe için erişimi kaybedebilir. Bir sanal ağda bir işe kullandığınızda, engellenen bir veya daha fazla bağlantı noktaları ortak bir kurulum sorunu yaşıyor. İŞE hedef sistem arasındaki bağlantılar için kullandığınız bağlayıcı da kendi bağlantı noktası gereksinimleri olabilir. FTP Bağlayıcısı'nı kullanarak bir FTP sistemiyle iletişim kurmak, örneğin, üzerinde kullandığınız bağlantı noktası komutları göndermek için bağlantı noktası 21 gibi FTP sistemin kullanılabilir emin olun.

İŞE nerede dağıttığınız sanal ağın alt ağlar arasında trafiği denetlemek için ayarlayabileceğiniz [ağ güvenlik grupları](../virtual-network/security-overview.md) için bu alt ağları tarafından [alt ağlar arasında ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md). Bu tablo, işe kullanır ve bu bağlantı noktalarının kullanıldığı, sanal ağ bağlantı noktaları açıklar. [Hizmet etiketi](../virtual-network/security-overview.md#service-tags) güvenlik kuralı oluştururken karmaşıklığını en aza indirmenize yardımcı IP adresi ön eki grubunu temsil eder.

> [!IMPORTANT]
> Alt ağlarınızı içinde iç iletişimi için bu alt ağlardan içindeki tüm bağlantı noktaları açma ISE gerektirir.

| Amaç | Yön | Bağlantı Noktaları | Kaynak hizmeti etiketi | Hedef hizmet etiketi | Notlar |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Azure Logic Apps gelen iletişimi | Giden | 80 & 443 | VIRTUAL_NETWORK | INTERNET | Bağlantı noktası ile iletişim kuran Logic Apps hizmetinin dış hizmete bağlıdır |
| Azure Active Directory | Giden | 80 & 443 | VIRTUAL_NETWORK | AzureActiveDirectory | |
| Azure depolama bağımlılık | Giden | 80 & 443 | VIRTUAL_NETWORK | Depolama | |
| İntersubnet iletişimi | Gelen ve giden | 80 & 443 | VIRTUAL_NETWORK | VIRTUAL_NETWORK | Alt ağlar arasındaki iletişim için |
| Azure Logic Apps ile iletişim | Gelen | 443 | INTERNET  | VIRTUAL_NETWORK | Herhangi bir istek tetikleyicisi veya mantıksal uygulamanızın mevcut Web kancası çağırır hizmet ve bilgisayar için IP adresi. Kapatma veya bu bağlantı noktası engelleyen istek Tetikleyicileri içeren mantıksal uygulamalar için HTTP çağrılarını engeller.  |
| Mantıksal uygulama çalıştırma geçmişi | Gelen | 443 | INTERNET  | VIRTUAL_NETWORK | Bilgisayar, mantıksal uygulamayı görüntülemek için IP adresi çalıştırma geçmişi. Kapatma ya da bu bağlantı noktası engellemelerini çalıştırma geçmişini görüntülemesini engellemez, ancak girişleri görüntüleyemezsiniz ve çıkışlar, her adımda için çalıştırma geçmişi. |
| Bağlantı Yönetimi | Giden | 443 | VIRTUAL_NETWORK  | INTERNET | |
| Tanılama günlükleri ve ölçümleri yayımlama | Giden | 443 | VIRTUAL_NETWORK  | AzureMonitor | |
| Logic Apps Tasarımcısı - dinamik özellikleri | Gelen | 454 | INTERNET  | VIRTUAL_NETWORK | İstekleri mantıksal uygulamalardan gelen [uç noktasına erişmek gelen IP adreslerini bu bölgede](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| App Service Management bağımlılık | Gelen | 454 & 455 | AppServiceManagement | VIRTUAL_NETWORK | |
| Bağlayıcı dağıtımı | Gelen | 454 & 3443 | INTERNET  | VIRTUAL_NETWORK | Dağıtma ve bağlayıcıları güncelleştirme gerekli. Kapatma ya da bu bağlantı noktası engellemelerini ISE dağıtımları başarısız olmasına neden olur ve bağlayıcı güncelleştirmeler veya düzeltmeler önler. |
| Azure SQL bağımlılığı | Giden | 1433 | VIRTUAL_NETWORK | SQL |
| Azure Kaynak Durumu | Giden | 1886 | VIRTUAL_NETWORK | INTERNET | Kaynak Durumu'nda sistem durumu yayımlamak için |
| API Yönetimi - yönetim uç noktası | Gelen | 3443 | APIManagement  | VIRTUAL_NETWORK | |
| Olay hub'ı İlkesi ve İzleme Aracısı günlüğünden bağımlılığı | Giden | 5672 | VIRTUAL_NETWORK  | EventHub | |
| Erişim Azure önbelleği için Redis örneği arasında rol örnekleri | Gelen <br>Giden | 6379-6383 | VIRTUAL_NETWORK  | VIRTUAL_NETWORK | Ayrıca, Redis için Azure önbellek ile çalışacak şekilde ISE için bunlar açmalısınız [Azure Cache Redis SSS açıklanan giden ve gelen bağlantı noktalarını](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Gelen | * | AZURE_LOAD_BALANCER | VIRTUAL_NETWORK |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE oluşturma

Tümleştirme hizmeti ortamı (ISE) oluşturmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **kaynak Oluştur**.

   ![Yeni kaynak oluştur](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Arama kutusuna filtreniz olarak "tümleştirme hizmeti ortamı" girin.
Sonuçlar listesinden **tümleştirme hizmeti ortamı (Önizleme)** ve ardından **Oluştur**.

   !["Tümleştirme hizmeti ortamı" seçin](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   !["Oluştur" öğesini seçin.](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları sağlayın ve ardından **gözden geçir + Oluştur**, örneğin:

   ![Ortam ayrıntılarını sağlayın](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gereklidir | Value | Açıklama |
   |----------|----------|-------|-------------|
   | **Abonelik** | Evet | <*Azure-subscription-name*> | Ortamınız için kullanılacak Azure aboneliği |
   | **Kaynak grubu** | Evet | <*Azure kaynak grubu adı*> | Ortamınızı oluşturmak için istediğiniz Azure kaynak grubu |
   | **Tümleştirme hizmeti ortamı adı** | Evet | <*ortam adı*> | Ortamınızı verilecek ad |
   | **Konum** | Evet | <*Azure veri merkezi bölgesi*> | Azure veri merkezi bölgesini ortamınızı dağıtılacağı yeri |
   | **Ek kapasite** | Evet | 0, 1, 2, 3 | Bu işe kaynak için kullanılacak işleme birimi sayısı. Oluşturulduktan sonra Kapasite eklemek için bkz [Kapasite eklemek](#add-capacity). |
   | **Sanal ağ** | Evet | <*Azure sanal-ağ-adı*> | Mantıksal uygulamalar bu ortamda, sanal ağınızın erişebilmesi için ortamınızı eklemesine istediğiniz Azure sanal ağı. Bir ağ yoksa, bir oluşturabilirsiniz burada. <p>**Önemli**: Yapabilecekleriniz *yalnızca* , işe oluşturduğunuzda bu ekleme gerçekleştirin. Bu ilişki oluşturabilmeniz için önce ancak, zaten emin [sanal ağınızdaki rol tabanlı erişim denetimi için Azure Logic Apps ayarlama](#vnet-access). |
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

   * Kullanırsanız [ExpressRoute](../expressroute/expressroute-introduction.md), unutmayın [her alt ağa aşağıdaki yolu Ekle](../virtual-network/virtual-network-manage-subnet.md) , işe tarafından kullanılır. Alt ağa sahip bir yol tablosu kullanırsanız [bu yol tablosuna aşağıdaki yolu Ekle](../virtual-network/manage-route-table.md):

     **Ad**: D3655BASE yönlendirme<br>
     **Adres ön eki**: 0.0.0.0/0<br>
     **Sonraki atlama**: Internet

   1. Altında **alt ağlar** listesinde **Yönet alt ağ yapılandırması**.

      ![Alt ağ yapılandırmasını yönet](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. Üzerinde **alt ağlar** bölmesinde seçin **alt**.

      ![Alt ağ ekleme](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. Üzerinde **alt ağ Ekle** bölmesinde, bu bilgileri sağlayın.

      * **Ad**: Alt ağınız için bir ad
      * **Adres aralığı (CIDR bloğu)**: Sanal ağınızda bulunan ve CIDR biçimindeki alt ağın aralığı

      ![Alt ağ ayrıntıları ekleyin](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. İşiniz bittiğinde seçin **Tamam**.

   1. Üç daha fazla alt ağ için bu adımları yineleyin.

1. Azure, işe bilgilerinizi başarıyla doğruladıktan sonra seçin **Oluştur**, örneğin:

   ![Doğrulama başarılı olduktan sonra "Oluştur" öğesini seçin.](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure başlar ancak bu işlem, ortamınızı dağıtma *olabilir* tamamlamadan önce iki saat sürebilir. 
   Azure, araç çubuğundaki dağıtım durumunu denetlemek için bildirimler bölmesi açılır bildirimler simgesini seçin.

   ![Dağıtım durumunu denetleyin](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla tamamlanırsa Azure bu bildirimi gösterilmektedir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   > [!NOTE]
   > Dağıtım başarısız olursa veya, Azure, işe silerseniz *olabilir* alt ağlarınızı serbest önce bir saate kadar sürebilir. Bu nedenle, bu alt ağlardan başka bir işe yeniden kullanmadan önce beklemeniz gerekebilir.

1. Ortamınızı görüntülemek için seçin **kaynağa Git** dağıtım tamamlandıktan sonra Azure ortamınıza otomatik olarak çıkmaz değil ise.  

<a name="add-capacity"></a>

### <a name="add-capacity"></a>Kapasite ekleyin

ISE temel birim kapasitesi, sabit daha fazla performans gerekiyorsa daha fazla ölçek birimi ekleyebilirsiniz. Performans ölçümleri temelinde veya bir işleme birimi sayısına göre otomatik ölçeklendirme yapabilirsiniz. Ölçümlere göre otomatik ölçeklendirme seçerseniz, çeşitli ölçütler arasından seçim yapın ve bu ölçütlerine uyan eşiği koşulları belirtin.

1. Azure portalında, işe bulun.

1. Performans ölçümleri, işe'nın ana menüsündeki, işe görüntülemek için seçin **genel bakış**.

1. Otomatik ölçeklendirmeyi, altında ayarlanacak **ayarları**seçin **ölçeğini**. Üzerinde **yapılandırma** sekmesini, **etkinleştirmek otomatik ölçeklendirme**.

1. İçinde **varsayılan** bölümünde, ya da seçin **ölçek dayalı bir ölçüme göre** veya **belirli bir örnek sayısına ölçeklendirin**.

1. Örnek tabanlı seçerseniz, işleme birimlerinin sayısı 0 ile 3 arasında aralığında girin. Aksi durumda, ölçüm tabanlı izlemek için bu adımları:

   1. İçinde **varsayılan** bölümünde, seçin **alınabilecek**.

   1. Üzerinde **ölçek kuralı** bölmesinde, kural tetiklendiğinde gerçekleştirilecek, ölçütleri ve eylem ayarlayın.

   1. İşiniz bittiğinde seçin **Ekle**.

1. İşiniz bittiğinde, değişikliklerinizi kaydetmeyi unutmayın.

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>ISE - mantıksal uygulama oluşturma

Tümleştirme hizmeti ortamı (ISE) kullanan mantıksal uygulamalar oluşturmak için adımları [bir mantıksal uygulama oluşturma işlemini](../logic-apps/quickstart-create-first-logic-app-workflow.md) ancak bu farklılıkla: 

* Oluşturduğunuzda, mantıksal uygulamanızı altında **konumu** özelliği, ISE'den seçin **tümleştirme service ortamları** bölümünde, örneğin:

  ![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Aynı yerleşik tetikleyiciler ve Eylemler HTTP gibi mantıksal olarak aynı işe çalıştırılması kullanabilirsiniz. Bağlayıcılarla **ISE** ayrıca mantıksal olarak aynı işe çalışma etiketleyin. Bağlayıcılar olmadan **ISE** etiket genel Logic Apps hizmetinde çalıştırın.

  ![ISE bağlayıcıları seçme](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Bir Azure sanal ağa, işe ekleme sonra işe logic apps'te, sanal ağ içindeki kaynaklarla doğrudan erişebilirsiniz. Logic apps bu öğelerden herhangi birini kullanarak bu sistemlerin doğrudan erişerek bir sanal ağa bağlı şirket içi sistemler için bu ağa bir işe ekleme: 

  * Örneğin, SQL Server sistem için işe Bağlayıcısı
  
  * HTTP eylemi 
  
  * Özel bağlayıcı

  Bir sanal ağda olmayan veya işe bağlayıcıları, ilk yoksa şirket içi sistemler için [şirket içi veri ağ geçidi ayarlama](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Tümleştirme hesabı - ISE oluşturma

Logic apps'te bir tümleştirme hizmeti ortamı (ISE) ile bir tümleştirme hesabı kullanmak için bu tümleştirme hesabı kullanmalısınız *aynı ortam* mantıksal uygulamalar. Logic apps'te bir işe yalnızca tümleştirme hesapları aynı işe başvurabilirsiniz. 

Bir işe kullanan bir tümleştirme hesabı oluşturmak için adımları izleyin. [tümleştirme hesapları oluşturma işlemini](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dışında **konumu** özelliği burada **tümleştirme service ortamları**  bölümü görünür. Bunun yerine, bir bölgede yerine, işe örneğin seçin:

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="https://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
