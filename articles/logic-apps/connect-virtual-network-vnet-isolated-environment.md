---
title: Azure Logic Apps'ten Azure sanal ağlarına bağlama
description: Azure sanal ağlar (Vnet'ler) Azure Logic Apps'ten erişmek için özel, adanmış ve yalıtılmış bir tümleştirme logic apps tutan service ortamları oluşturabilir ve diğer kaynaklara ayrı genel veya "Genel" Azure'dan
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: b1a75c140376c1e2e2fdfdcd1581978301ab32f1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46996477"
---
# <a name="create-isolated-environments-to-access-azure-virtual-networks-vnets-from-azure-logic-apps"></a>Azure sanal ağlar (Vnet'ler) Azure Logic Apps'ten erişmek için yalıtılmış ortamlar oluşturun

> [!NOTE]
> Bu özellik bulunduğu *özel Önizleme*. Erişim talep etmek [burada katılma isteğiniz oluşturma](https://aka.ms/iseprivatepreview).

Tümleştirme senaryolarını logic apps ve tümleştirme hesapları gereken yere erişimi için bir [Azure sanal ağı (VNET)](../virtual-network/virtual-networks-overview.md), oluşturabileceğiniz bir [ *tümleştirme hizmeti ortamı* (ISE) ](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) bağlantıları sanal AĞINIZA ve Logic Apps hizmetinin, VNET'te dağıtılır. Logic apps ve tümleştirme hesabı oluşturduğunuzda, bu işe kendi konum olarak seçin. Logic apps ve tümleştirme hesapları daha sonra doğrudan sanal makineleri (VM'ler), sunucular, sistemleri ve Hizmetleri, ağınızda gibi kaynakları erişebilirsiniz. 

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Ayrılmış depolama ve ayrı olarak genel kullanıma mevcut diğer kaynakları kullanan özel ve yalıtılmış bir ortam, işe olduğu veya *genel* Logic Apps hizmetinin. Bu ayrım, diğer Azure kiracılarında, uygulamalarınızın performansını olabilir herhangi etkiyi azaltmak da yardımcı olur. 

Bu makalede, bu görevlerin nasıl gerçekleştirileceğini gösterir:

* Özel Logic Apps örneği VNET'İNİZİ erişebilmesi için Azure sanal ağ üzerindeki izinleri ayarlayın.

* Tümleştirme hizmeti ortamı (ISE) oluşturun. 

* ISE'de çalıştırmak üzere bir mantıksal uygulama oluşturun. 

* Tümleştirme hesabı için logic apps, ISE'de oluşturun.

Tümleştirme service ortamları hakkında daha fazla bilgi için bkz: [yalıtılmış Azure Logic Apps Azure sanal ağ (VNET) kaynaklara erişim](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Bir Azure sanal ağı yoksa, bilgi nasıl [bir Azure sanal ağı oluşturma](../virtual-network/quick-create-portal.md). 

  > [!IMPORTANT]
  > Ortamınızı oluşturmak için bir Azure sanal ağı gerekmez ancak yapabilecekleriniz *yalnızca* o ortamı oluşturduğunuzda ortamınızın eş sanal ağ seçin. 

* Logic apps, Azure sanal ağa doğrudan erişim vermek için [rol tabanlı erişim denetimi (RBAC) izinleri ayarlama](#vnet-access) Logic Apps hizmetinin VNET'İNİZİ erişmek için izinlere sahiptir. 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="vnet-access"></a>

## <a name="set-up-vnet-permissions"></a>VNET izinleri ayarlama

Tümleştirme service ortamınızı oluşturduğunuzda, bir Azure sanal ağı (VNET) olarak seçebileceğiniz bir *eş* ortamınız için. Ancak, yalnızca bu adımı gerçekleştirebilir veya *eşlemesi*, ortamınızı oluşturduğunuzda. Bu ilişki, Logic Apps hizmetinin söz konusu sanal ağ içindeki kaynakları doğrudan bağlanmanızı sağlar ve bu kaynaklara, ortam erişim sağlar. 

Sanal AĞINIZA seçebilmeniz için AĞINIZDA rol tabanlı erişim denetimi (RBAC) izinlerini ayarlamanız gerekir. Bu görevi tamamlamak için Azure Logic Apps hizmetine belirli rol atamanız gerekir.

1. İçinde [Azure portalında](https://portal.azure.com)bulup, sanal ağ seçin. SANAL ağın menüsünde **erişim denetimi (IAM)**. 

1. Altında **erişim denetimi**seçin **rol ataması** zaten seçili değilse. Üzerinde **rol ataması** araç seçin **Ekle**. 

   ![Rol ataması Ekle](./media/connect-virtual-network-vnet-isolated-environment/set-up-role-based-access-control-vnet.png)

1. Üzerinde **izinleri eklemek** bölmesinde, her rol için Azure Logic Apps hizmetinin bu tablodaki ayarlayın. Seçtiğinizden emin olun **Kaydet** her rol tamamladıktan sonra:

   | Rol | Erişim ata: | Şunu seçin: | 
   |------|------------------|--------|
   | **Ağ Katılımcısı** | **Azure AD kullanıcı, Grup veya uygulama** | Girin **Azure Logic Apps**. Üye listesi göründükten sonra aynı değeri seçin. <p>**İpucu**: Bu hizmet bulamazsanız, Logic Apps hizmetin uygulama kimliği girin: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   | **Klasik Katılımcısı** | **Azure AD kullanıcı, Grup veya uygulama** | Girin **Azure Logic Apps**. Üye listesi göründükten sonra aynı değeri seçin. <p>**İpucu**: Bu hizmet bulamazsanız, Logic Apps hizmetin uygulama kimliği girin: `7cd684f4-8a78-49b0-91ec-6a35d38739ba` | 
   |||| 

   Örneğin:

   ![İzin ekleme](./media/connect-virtual-network-vnet-isolated-environment/add-contributor-roles.png)

Eşdüzey hizmet sağlama için gerekli rol izinleri hakkında daha fazla bilgi için bkz: [İzinler bölümünde oluşturma, değiştirme veya bir sanal ağ eşlemesini Sil](../virtual-network/virtual-network-manage-peering.md#permissions).

<a name="create-environment"></a>

## <a name="create-your-ise"></a>ISE oluşturma

Tümleştirme hizmeti ortamı (ISE) oluşturmak için aşağıdaki adımları izleyin:

1. İçinde [Azure portalında](https://portal.azure.com), ana Azure menüsünde **kaynak Oluştur**.

   ![Yeni kaynak oluştur](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. Arama kutusuna filtreniz olarak "tümleştirme hizmeti ortamı" girin.
Sonuçlar listesinden **tümleştirme hizmeti ortamı (Önizleme)** ve ardından **Oluştur**.

   !["Tümleştirme hizmeti ortamı" seçin](./media/connect-virtual-network-vnet-isolated-environment/select-integration-service-environment.png)

   !["Oluştur" öğesini seçin.](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Ortamınız için bu ayrıntıları sağlayın:

   ![Ortam ayrıntılarını sağlayın](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Ad** | Evet | <*ortam adı*> | Ortamınızı verilecek ad | 
   | **Abonelik** | Evet | <*Azure abonelik adı*> | Ortamınız için kullanılacak Azure aboneliği | 
   | **Kaynak grubu** | Evet | <*Azure kaynak grubu adı*> | Ortamınızı oluşturmak için istediğiniz Azure kaynak grubu |
   | **Konum** | Evet | <*Azure veri merkezi bölgesi*> | Azure veri merkezi bölgesini ortamınız hakkında bilgilerin depolanacağı |
   | **Eş VNET'in** | Hayır | <*Azure sanal ağ adı*> | Azure sanal ağı (VNET) ortamınızı ilişkilendirmek için bir *eş* mantıksal uygulamalar bu ortamda sanal AĞINIZA erişebilmek için. Bu ilişki oluşturabilmeniz için önce zaten emin olun [sanal ağınızdaki rol tabanlı erişim denetimi için Azure Logic Apps ayarlama](#vnet-access). <p>**Önemli**: bir VNET gerekli olmasa da, bir VNET seçin *yalnızca* ortamınızı oluşturduğunuzda. | 
   | **Eşleme adı** | Seçili sanal ağ ile Evet | <*eşleme adı*> | Eş ilişki verilecek ad | 
   | **Sanal ağ IP aralığı** | Seçili sanal ağ ile Evet | <*IP adresi aralığı*> | Ortamınızda kaynakları oluşturmak için kullanılacak IP adresi aralığı. Bu aralık kullanmalısınız [sınıfsız etki alanları arası yönlendirme (CIDR) biçimi](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing), örneğin, 10.0.0.1/16, sınıf B adres alanı gerekir. Seçili sanal ağ için adres alanı içinde aralık bulunmamalıdır **eş VNET** özelliği, ne de burada eş ağ bağlı eşlemesi veya ağ geçitleri aracılığıyla herhangi diğer özel IP adresleri içinde. <p><p>**Önemli**:, *değiştiremezsiniz* ortamınızı oluşturduktan sonra bu adres aralığı. |
   |||||
   
1. İşiniz bittiğinde **Oluştur**’u seçin. 

   Azure ortamınıza dağıtmaya başlar, ancak bu işlem sürebilir *iki saate kadar* tamamlamadan önce. 
   Azure, araç çubuğundaki dağıtım durumunu denetlemek için bildirimler bölmesi açılır bildirimler simgesini seçin.

   ![Dağıtım durumunu denetleyin](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Dağıtım başarıyla tamamlandığında, Azure bu bildirimi gösterilmektedir:

   ![Dağıtım başarılı oldu](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

1. Ortamınızı görüntülemek için seçin **kaynağa Git** dağıtım tamamlandıktan sonra Azure ortamınıza otomatik olarak çıkmaz değil ise.  

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>ISE - mantıksal uygulama oluşturma

Tümleştirme hizmeti ortamı (ISE) kullanan mantıksal uygulamalar oluşturmak için her zamanki adımları [bir mantıksal uygulama oluşturma işlemini](../logic-apps/quickstart-create-first-logic-app-workflow.md) ancak bu farklılıklar ve dikkat edilmesi gerekenler: 

* Mantıksal uygulamanızı oluşturduğunuzda **konumu** özellik listelerini, ISEs altında **tümleştirme service ortamları** kullanılabilir bölgelerin yanı sıra. Örneğin, işe yerine bir bölge seçin:

  ![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

* Üst mantıksal uygulama olarak aynı işe çalıştırılması aynı yerleşik olanları, HTTP tetikleyici veya eylemi gibi kullanabilirsiniz. Bağlayıcılarla **ISE** de aynı işe üst mantıksal uygulama çalıştırma etiketleyin. Bağlayıcılar olmadan **ISE** etiket genel Logic Apps hizmetinde çalıştırın.

  ![ISE bağlayıcıları seçme](./media/connect-virtual-network-vnet-isolated-environment/select-ise-connectors.png)

* Daha önce işe ile bir Azure sanal ağı bir eş olarak ayarlarsanız, logic apps, işe içinde doğrudan bu sanal ağ içindeki kaynaklara erişebilir. Bir işe için bağlı bir sanal ağ içindeki şirket içi sistemler için logic apps doğrudan bu sistemlerin bu öğelerden herhangi birini kullanarak erişebilir: 

  * Örneğin, SQL Server sistem için işe Bağlayıcısı

  * HTTP eylemi 

  * Özel bağlayıcı

  Sanal ağ içinde olmayan veya işe bağlayıcılar yoksa şirket içi sistemler için sonra bağlanmaya devam edebilirler [ayarlama ve şirket içi veri ağ geçidi kullanma](../logic-apps/logic-apps-gateway-install.md).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Tümleştirme hesabı - ISE oluşturma

Logic apps'te bir tümleştirme hizmeti ortamı (ISE) ile bir tümleştirme hesabı kullanmak için bu tümleştirme hesabı kullanmalısınız *aynı ortam* mantıksal uygulamalar. Logic apps'te bir işe yalnızca tümleştirme hesapları aynı işe başvurabilirsiniz. 

Bir işe kullanan bir tümleştirme hesabı oluşturmak için her zamanki adımları izleyin. [tümleştirme hesapları oluşturma işlemini](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dışında **konumu** artık, ISEs altında listeleyen özelliği  **Tümleştirme service ortamları** kullanılabilir bölgelerin yanı sıra. Örneğin, işe yerine bir bölge seçin:

![Tümleştirme hizmeti ortamı seçin](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

## <a name="get-support"></a>Destek alın

* Sorularınız için <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forumunu</a> ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için <a href="http://aka.ms/logicapps-wish" target="_blank">Logic Apps kullanıcı geri bildirimi sitesini</a> ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure sanal ağ](../virtual-network/virtual-networks-overview.md)
* Hakkında bilgi edinin [Azure Hizmetleri için sanal ağ tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md)
