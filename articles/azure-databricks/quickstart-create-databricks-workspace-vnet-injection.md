---
title: Sanal ağ içinde bir Azure Databricks çalışma alanı oluşturma
description: Bu makalede, Azure Databricks, sanal ağınıza dağıtmayı açıklar.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 295b64b10f9f78ca6224d60fb84c6d1310aaa42e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770732"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-a-virtual-network"></a>Hızlı Başlangıç: Sanal ağ içinde bir Azure Databricks çalışma alanı oluşturma

Bu hızlı başlangıçta, bir sanal ağ içinde bir Azure Databricks çalışma alanı oluşturma gösterilmektedir. Ayrıca, bu çalışma alanı içindeki bir Apache Spark kümesi oluşturacaksınız.

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Azure portalında **kaynak Oluştur** > **ağ** > **sanal ağ**.

2. Altında **sanal ağ oluştur**, aşağıdaki ayarları uygulayın: 

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Ad|databricks hızlı başlangıç|Sanal ağınız için bir ad seçin.|
    |Adres alanı|10.1.0.0/16|CIDR gösterimiyle sanal ağın adres aralığı.|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks hızlı başlangıç|Seçin **Yeni Oluştur** ve hesabınız için yeni bir kaynak grubu adı girin.|
    |Location|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Burada, sanal ağınızı barındırabilirsiniz coğrafi bir konum seçin. Kullanıcılarınıza en yakın konumu kullanın.|
    |Alt ağ adı|default|Sanal ağınızda bulunan varsayılan alt ağ için bir ad seçin.|
    |Alt Ağ Adresi aralığı|10.1.0.0/24|CIDR gösteriminde alt ağın adres aralığı. Sanal ağın adres alanı tarafından bulunması gerekir. Kullanımda olan bir alt ağ adres aralığı düzenlenemez.|

    ![Azure portalında sanal ağ oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Dağıtım tamamlandıktan sonra sanal ağınıza gidin ve seçin **adres alanı** altında **ayarları**. İfadesini içeren kutuya *ek adres aralığı Ekle*, INSERT `10.179.0.0/16` seçip **Kaydet**.

    ![Azure sanal ağ adres alanı](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Azure Databricks çalışma alanı oluşturma

1. Azure portalında **kaynak Oluştur** > **Analytics** > **Databricks**.

2. Altında **Azure Databricks hizmeti**, aşağıdaki ayarları uygulayın:

    |Ayar|Önerilen değer|Açıklama|
    |-------|---------------|-----------|
    |Çalışma alanı adı|databricks hızlı başlangıç|Azure Databricks çalışma alanınız için bir ad seçin.|
    |Abonelik|\<Aboneliğiniz\>|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|databricks hızlı başlangıç|Sanal ağ için kullanılan aynı kaynak grubunu seçin.|
    |Location|\<Kullanıcılarınıza en yakın bölgeyi seçin\>|Sanal ağınızla aynı konumu seçin.|
    |Fiyatlandırma Katmanı|Standart veya Premium arasında seçim yapın.|Fiyatlandırma katmanları hakkında daha fazla bilgi için bkz: [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).|
    |Azure Databricks çalışma alanında, sanal ağ dağıtma|Evet|Bu ayar, sanal ağınızda bir Azure Databricks çalışma alanı dağıtmanıza olanak sağlar.|
    |Sanal Ağ|databricks hızlı başlangıç|Önceki bölümde oluşturduğunuz sanal ağı seçin.|
    |Ortak bir alt ağ adı|Ortak alt ağ|Varsayılan Genel alt ağ adı kullanın.|
    |Ortak alt ağ CIDR aralığı|10.179.64.0/18|Bu alt ağ için CIDR aralığı /26 /18 arasında olmalıdır.|
    |Özel alt ağ adı|Özel alt ağ|Varsayılan özel alt ağ adı kullanın.|
    |Özel alt ağ CIDR aralığı|10.179.0.0/18|Bu alt ağ için CIDR aralığı /26 /18 arasında olmalıdır.|

    ![Azure portalında bir Azure Databricks çalışma alanı oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Dağıtım tamamlandıktan sonra Azure Databricks kaynağına gidin. Sanal Ağ eşlemesi devre dışı bırakıldı dikkat edin. Ayrıca kaynak grubunu ve yönetilen kaynak grubuna genel bakış sayfasında dikkat edin. 

    ![Azure portalında Azure Databricks genel bakış](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    Yönetilen kaynak grubu, depolama hesabı (DBFS) çalışan-sg (ağ güvenlik grubu), çalışan sanal ağ (sanal ağ için) fiziksel konumunu içerir. Bu da sanal makineler, disk, IP adresi ve ağ arabirimi oluşturulacağı konumdur. Varsayılan olarak bu kaynak grubu kilitli; Ancak sanal ağ içinde bir küme başlatıldığında, bir ağ arabirimi yönetilen kaynak grubu içinde çalışan vnet ile "hub" sanal ağ arasında oluşturulur.

    ![Azure Databricks yönetilen kaynak grubu](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

## <a name="create-a-cluster"></a>Küme oluşturma

> [!NOTE]
> Azure Databricks kümesini oluşturmak için ücretsiz hesap oluşturmak istiyorsanız kümeyi oluşturmadan önce profilinize gidin ve aboneliğini **kullandıkça öde** modeline geçirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/).

1. Azure Databricks hizmetinize dönüp seçin **çalışma alanını Başlat** üzerinde **genel bakış** sayfası.

2. Seçin **kümeleri** > **+ küme oluştur**. Bir küme adı gibi oluşturup *databricks hızlı küme*, geri kalan varsayılan ayarları kabul edin. Seçin **küme oluşturma**.

    ![Azure Databricks kümesi oluşturma](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Küme çalışmaya başladıktan sonra Azure portalında yönetilen kaynak grubuna döndürür. Yeni sanal makineler, diskleri, IP adresi ve ağ arabirimleri dikkat edin. Bir ağ arabirimi IP adresleriyle ortak ve özel alt ağlar oluşturulur.  

    ![Küme oluşturulduktan sonra Azure Databricks yönetilen kaynak grubu](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Azure Databricks çalışma alanınıza dönmek ve oluşturduğunuz kümeyi seçin. Ardından gidin **yürütücüler** sekmesinde **Spark UI** sayfası. Özel alt ağ aralığında sürücü ve yürütücüler adresleri olduğuna dikkat edin. Bu örnekte, sürücü 10.179.0.6 ve yürütücüler 10.179.0.4 ve 10.179.0.5. IP adreslerinizi farklı olabilir.

    ![Azure Databricks Spark UI Yürütücü](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Makaleyi tamamladıktan sonra kümeyi sonlandırabilirsiniz. Bunu yapmak için Azure Databricks çalışma alanında sol bölmedeki **Kümeler**’i seçin. Sonlandırmak istediğiniz küme için imleci **Eylemler** sütunu altındaki üç noktanın üzerine taşıyın ve **Sonlandır** simgesini seçin. Bu, küme durdurulur.

El ile otomatik olarak durdurur küme sonlandırmazsanız, seçtiğiniz sağlanan **sonra Sonlandır \_ \_ yapılmadan geçecek dakika cinsinden** küme oluşturulurken onay kutusu. Böyle bir durumda, belirtilen süre boyunca etkin olmaması durumunda küme otomatik olarak durdurulur.

Küme yeniden kullanmak istemiyorsanız, Azure portalında oluşturduğunuz kaynak grubunu silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir sanal ağa dağıttığınız Azure databricks'te Spark kümesi oluşturulur. Bir Azure Databricks not defterinden JDBC kullanarak sanal ağa bir SQL Server Linux Docker kapsayıcısında sorgulama hakkında bilgi edinmek için sonraki makaleye ilerleyin.  

> [!div class="nextstepaction"]
>[Sorgu bir SQL Server Linux Docker kapsayıcısı içinde bir sanal ağdan bir Azure Databricks not defteri](vnet-injection-sql-server.md)
