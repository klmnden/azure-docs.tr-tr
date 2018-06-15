---
title: Visual Studio ile rolleri bir Azure bulut hizmeti için yapılandırma | Microsoft Docs
description: Ayarlama ve rolleri Visual Studio kullanarak Azure bulut Hizmetleri için yapılandırma hakkında bilgi edinin.
services: visual-studio-online
author: ghogen
manager: douge
assetId: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 09e6c3a9c27342ef27d49674d62ccf74d70d2e0f
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31798735"
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Azure bulut hizmeti rollerinizi ile Visual Studio'yu yapılandırma
Bir Azure bulut hizmeti bir veya daha fazla çalışan ya da web rolleri olabilir. Her rol için bu rolü nasıl ayarlanacağını tanımlayın ve ayrıca bu rolü nasıl çalışacağını yapılandırmak gerekir. Bulut Hizmetleri rolleri hakkında daha fazla bilgi edinmek için video bkz [Azure Cloud Services giriş](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

Bulut hizmetiniz için bilgileri aşağıdaki dosyalarda depolanır:

- **ServiceDefinition.csdef** -hizmet tanımı dosyası Bulut hizmeti hangi rollerin gerekli dahil olmak üzere, uç noktaları ve sanal makine boyutu için çalışma zamanı ayarları tanımlar. Depolanan verilerin hiçbiri `ServiceDefinition.csdef` rolünüze çalıştırırken değiştirilebilir.
- **ServiceConfiguration.cscfg** - hizmet yapılandırma dosyasının bir rolün kaç örneğini çalıştırma yapılandırır ve ayarlarının değerleri, bir rol için tanımlanan. Depolanan verileri `ServiceConfiguration.cscfg` rolünüze çalışırken değiştirilebilir.

Bir rolü nasıl çalıştığını denetleyen ayarlar için farklı değerler depolamak için birden çok hizmet yapılandırması tanımlayabilirsiniz. Her dağıtım ortamı için farklı hizmet yapılandırması kullanabilirsiniz. Örneğin, bir yerel hizmet yapılandırmasında yerel Azure storage öykünücüsünü kullanma ve Azure storage bulutta kullanılacak başka bir hizmet yapılandırması oluşturmak için depolama hesabı bağlantı dizenizi ayarlayabilirsiniz.

Visual Studio'da bir Azure bulut hizmeti oluşturduğunuzda, iki hizmet yapılandırması otomatik olarak oluşturulan ve Azure projenize eklenir:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Bir Azure bulut hizmeti yapılandırın
Visual Studio'da Çözüm Gezgini'nden bir Azure bulut hizmeti aşağıdaki adımlarda gösterildiği gibi yapılandırabilirsiniz:

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve bağlam menüsünden seçin **özellikleri**.
   
    ![Çözüm Gezgini proje bağlam menüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Proje özellikleri sayfasını seçin **geliştirme** sekmesi. 

    ![Proje Özellikleri sayfası - geliştirme sekmesi](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. İçinde **hizmet yapılandırmasını** listesinde, düzenlemek istediğiniz hizmet yapılandırmasının adı seçin. (Bu rol için tüm hizmet yapılandırması değişiklik yapmak isteyip istemediğinizi seçin **tüm yapılandırmaları**.)
   
    > [!IMPORTANT]
    > Belirli hizmet yapılandırmasını seçerseniz, tüm yapılandırmalar için yalnızca ayarlanamadığı için bazı özellikler devre dışı bırakılır. Bu özelliklerini düzenlemek için seçmelisiniz **tüm yapılandırmaları**.
    > 
    > 
   
    ![Bir Azure bulut hizmeti için hizmet yapılandırmasını listesi](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a>Rol örneklerinin sayısını değiştirme
Bulut hizmetinizin performansını artırmak için çalışan, kullanıcılar veya belirli bir rol için beklenen yükü sayısına dayalı bir rolün örnekleri sayısını değiştirebilirsiniz. Bulut hizmeti Azure içinde çalıştığında ayrı bir sanal makine rol her örneği için oluşturulur. Bu bulut hizmeti dağıtımının faturalama etkiler. Faturalama hakkında daha fazla bilgi için bkz: [faturanızı anlamak için Microsoft Azure](billing/billing-understand-your-bill.md).

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin. Altında **rolleri** düğümü, istediğiniz güncelleştirmek ve bağlam menüsünden seçmek için role sağ **özellikleri**.

    ![Çözüm Gezgini Azure rol bağlam menüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Seçin **yapılandırma** sekmesi.

    ![Yapılandırma sekmesi](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. İçinde **hizmet yapılandırmasını** listesinde, güncelleştirmek istediğiniz hizmet yapılandırmasını seçin.
   
    ![Hizmet yapılandırma listesi](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. İçinde **örnek sayısını** metin kutusuna, bu rol için başlatmak istediğiniz örneklerinin sayısını girin. Azure için bulut hizmeti yayımladığınızda, her örneği ayrı bir sanal makinede çalıştırır.

    ![Örnek sayısı güncelleştiriliyor](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. Visual Studio'dan araç, select **kaydetmek**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Depolama hesapları için bağlantı dizelerini Yönet
Ekleyin, kaldırın veya bağlantı dizeleri, hizmet yapılandırması için değiştirin. Örneğin, bir yerel bağlantı dizesi değerine sahip bir yerel hizmet yapılandırması için isteyebilirsiniz `UseDevelopmentStorage=true`. Azure'da bir depolama hesabını kullanan bir bulut hizmeti yapılandırması yapılandırmak isteyebilirsiniz.

> [!WARNING]
> Bir depolama hesabı bağlantı dizesi için Azure depolama hesabı anahtar bilgileri girdiğinizde, bu bilgileri yerel olarak hizmet yapılandırma dosyasında depolanır. Ancak, bu bilgileri şu anda şifreli metin olarak depolanmaz.
> 
> 

Her hizmet yapılandırması için farklı bir değer kullanarak, bulut hizmetiniz farklı bağlantı dizelerini kullanın veya Azure'da, bulut hizmetinizin yayımlarken kodunuzu değiştirmeniz gerekmez. Kodunuzdaki bağlantı dizesi için aynı adı kullanabilirsiniz ve bulut hizmetiniz derlerken veya yayımladığınızda seçin hizmet yapılandırmasını temel alarak, farklı değerdir.

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin. Altında **rolleri** düğümü, istediğiniz güncelleştirmek ve bağlam menüsünden seçmek için role sağ **özellikleri**.

    ![Çözüm Gezgini Azure rol bağlam menüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Seçin **ayarları** sekmesi.

    ![Ayarlar sekmesi](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. İçinde **hizmet yapılandırmasını** listesinde, güncelleştirmek istediğiniz hizmet yapılandırmasını seçin.

    ![Hizmet yapılandırması](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Bir bağlantı dizesi eklemek için seçin **ayar Ekle**.

    ![Bağlantı dizesi Ekle](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Yeni bir ayar listesine eklendikten sonra listedeki satır gerekli olan bilgileri güncelleştirin.

    ![Yeni bağlantı dizesi](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Ad** -bağlantı dizesi için kullanmak istediğiniz adı girin.
    - **Tür** - seçin **bağlantı dizesi** aşağı açılan listeden.
    - **Değer** -ya da doğrudan bağlantı dizesi girebilirsiniz **değeri** hücre ya da çalışmak için üç nokta (...) seçin **depolama bağlantı dizesi oluştur** iletişim.  

1. İçinde **depolama bağlantı dizesi oluştur** iletişim kutusunda bir seçenek için **bağlanırken**. Sonra hangi seçeneği için yönergeleri izleyin:

    - **Microsoft Azure storage öykünücüsü** -bu seçeneği seçerseniz, yalnızca Azure uygularken kalan Ayarları iletişim kutusunda devre dışı bırakılır. **Tamam**’ı seçin.
    - **Aboneliğinizi** - bu seçeneği seçerseniz seçin ve bir Microsoft hesabına oturum için açılır listeyi kullanın veya bir Microsoft hesabı ekleyin. Bir Azure aboneliği ve depolama hesabı seçin. **Tamam**’ı seçin.
    - **Kimlik bilgileri'el ile girilen** -birincil ya da ikinci anahtar ve depolama hesabı adı girin. Bir seçenek belirleyin **bağlantı** (HTTPS çoğu senaryolar için önerilir.) Seçin **Tamam**.

1. Bir bağlantı dizesi silmek için bağlantı dizesi seçin ve ardından **Kaldır ayarını**.

1. Visual Studio'dan araç, select **kaydetmek**.

## <a name="programmatically-access-a-connection-string"></a>Bir bağlantı dizesi programlamayla erişme

Aşağıdaki adımları program aracılığıyla C# kullanarak bir bağlantı dizesi nasıl erişeceğinizi gösterir.

1. Aşağıdaki yönergeleri nerede bulacağınızı ayarı kullanmak için bir C# dosya kullanarak:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Aşağıdaki kod, bir bağlantı dizesi erişmek nasıl bir örneği gösterir. Değiştir &lt;ConnectionStringName > yer tutucu uygun değere sahip. 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Azure bulut hizmetinde kullanmak için özel ayarları ekleme
Hizmet yapılandırma dosyasında özel ayarlar bir ad ve bir özel hizmet yapılandırması için bir dize değeri eklemenize olanak sağlar. Bir özellik ayarının değeri okuyarak ve kodunuzu mantık denetlemek için bu değeri kullanılarak bulut hizmetinizi yapılandırmak için bu ayarı kullanmayı seçebilirsiniz. Hizmet paketi veya Bulut hizmeti çalışmadığı zaman yeniden oluşturmak zorunda kalmadan, bu hizmet yapılandırma değerlerini değiştirebilirsiniz. Kodunuzu bildirimleri için bir ayar değiştiğinde kontrol edebilirsiniz. Bkz: [RoleEnvironment.Changing olay](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Ekleyin, kaldırın veya hizmet yapılandırmalarınızı için özel ayarları değiştirin. Bu dizeler için farklı hizmet yapılandırması için farklı değerler isteyebilirsiniz.

Her hizmet yapılandırması için farklı bir değer kullanarak, bulut hizmetiniz farklı dizeleri kullanın veya Azure'da, bulut hizmetinizin yayımlarken kodunuzu değiştirmeniz gerekmez. Kodunuzda dizesi için aynı adı kullanabilirsiniz ve bulut hizmetiniz derlerken veya yayımladığınızda seçin hizmet yapılandırmasını temel alarak, farklı değerdir.

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin. Altında **rolleri** düğümü, istediğiniz güncelleştirmek ve bağlam menüsünden seçmek için role sağ **özellikleri**.

    ![Çözüm Gezgini Azure rol bağlam menüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Seçin **ayarları** sekmesi.

    ![Ayarlar sekmesi](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. İçinde **hizmet yapılandırmasını** listesinde, güncelleştirmek istediğiniz hizmet yapılandırmasını seçin.

    ![Hizmet yapılandırma listesi](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Özel bir ayar eklemek için seçin **ayar Ekle**.

    ![Özel ayar Ekle](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Yeni bir ayar listesine eklendikten sonra listedeki satır gerekli olan bilgileri güncelleştirin.

    ![Yeni özel ayar](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Ad** -ayar adını girin.
    - **Tür** - seçin **dize** aşağı açılan listeden.
    - **Değer** -ayarın değerini girin. Değer doğrudan ya da girebilirsiniz **değeri** hücre ya da değer girmek için üç nokta (...) seçin **dize Düzenle** iletişim.  

1. Özel bir ayarı silmek için bir ayar seçin ve ardından **Kaldır ayarını**.

1. Visual Studio'dan araç, select **kaydetmek**.

## <a name="programmatically-access-a-custom-settings-value"></a>Özel bir ayarın değerini programlamayla erişme
 
Aşağıdaki adımları program aracılığıyla C# kullanarak özel bir ayar nasıl erişeceğinizi gösterir.

1. Aşağıdaki yönergeleri nerede bulacağınızı ayarı kullanmak için bir C# dosya kullanarak:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Aşağıdaki kod, özel bir ayar erişmek nasıl bir örneği gösterir. Değiştir &lt;SettingName > yer tutucu uygun değere sahip. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Her rol örneği için yerel depolama yönetin
Her bir rol örneği için yerel dosya sistemi depolama ekleyebilirsiniz. Depolama erişilebilir değil, verilerin depolandığı için rolünün diğer örnekler tarafından ya da diğer rolleri tarafından depolanan verileri.  

1. Oluşturun veya bir Azure bulut hizmeti projesini Visual Studio'da açın.

1. İçinde **Çözüm Gezgini**, proje düğümünü genişletin. Altında **rolleri** düğümü, istediğiniz güncelleştirmek ve bağlam menüsünden seçmek için role sağ **özellikleri**.

    ![Çözüm Gezgini Azure rol bağlam menüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Seçin **yerel depolama** sekmesi.

    ![Yerel depolama sekmesi](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. İçinde **hizmet yapılandırmasını** listesinde **tüm yapılandırmaları** yerel depolama ayarları uygulamak için tüm hizmet yapılandırması olarak seçilidir. Başka bir değer devre dışı bırakılıyor sayfasında tüm giriş alanları sonuçlanır. 

    ![Hizmet yapılandırma listesi](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. Yerel depolama girdisi eklemek için seçin **yerel depolama alanı Ekle**.

    ![Yerel depolama ekleyin](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Listeye yeni yerel depolama girdisi eklendikten sonra listedeki satır gerekli olan bilgileri güncelleştirin.

    ![Yeni yerel depolama girdisi](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Ad** -yeni yerel depolama için kullanmak istediğiniz adı girin.
    - **Boyut (MB)** -yeni yerel depolama için gereksinim duyduğunuz MB cinsinden boyutu girin.
    - **Rol geri dönüşüm üzerinde temiz** -sanal makine rolü için geri dönüştürüldüğünde verilerin yeni yerel depolama kaldırmak için bu seçeneği seçin.

1. Yerel depolama girdisi silmek için bir girişi seçin ve ardından **kaldırmak yerel depolama**.

1. Visual Studio'dan araç, select **kaydetmek**.

## <a name="programmatically-accessing-local-storage"></a>Program aracılığıyla yerel depolamaya erişme

Bu bölümde, C# test metin dosyası yazarak kullanarak yerel depolama programlı olarak erişmek verilmektedir `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-to-local-storage"></a>Yerel depolama alanına bir metin dosyasına yazma

Aşağıdaki kod, bir metin dosyası yerel depolama alanına yazmak nasıl bir örneği gösterir. Değiştir &lt;LocalStorageName > yer tutucu uygun değere sahip. 

    ```csharp
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-to-local-storage"></a>Yerel depolama alanına yazılmış bir dosya bulunamadı

Önceki bölümde kod tarafından oluşturulan dosyasını görüntülemek için aşağıdaki adımları izleyin:
    
1.  Windows bildirim alanındaki Azure simgesine sağ tıklayın ve bağlam menüsünden seçin **Göster işlem öykünücüsü kullanıcı Arabiriminde**. 

    ![Azure işlem öykünücüsünü Göster](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Web rolü seçin.

    ![Azure işlem öykünücüsü](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. Üzerinde **Microsoft Azure işlem öykünücüsü** menüsünde, select **Araçları** > **açık yerel deposu**.

    ![Açık yerel deposu menü öğesi](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Windows Gezgini penceresi açıldığında, girin ' MyLocalStorageTest.txt'' içine **arama** metin kutusu ve select **Enter** aramayı başlatmak için. 

## <a name="next-steps"></a>Sonraki adımlar
Visual Studio'da Azure projeler hakkında daha fazla bilgi okuyarak [bir Azure projesi yapılandırma](vs-azure-tools-configuring-an-azure-project.md). Bulut hizmeti şeması hakkında daha fazla bilgi okuyarak [şema başvurusu](https://msdn.microsoft.com/library/azure/dd179398).

