---
title: Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynaklarına oluşturun | Microsoft Docs
description: Çoklu VM ortamları ve PaaS kaynaklarına Azure DevTest Labs'de bir Azure Resource Manager şablonu oluşturma hakkında bilgi edinin
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: v-craic
ms.openlocfilehash: 38e048e9ec4985d16632f8891e42c2b6394c83d6
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Çoklu VM ortamları ve PaaS kaynaklarına Azure Resource Manager şablonları ile oluşturma

[Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) kolayca sağlar [oluşturmak ve bir VM'yi laboratuvara ekleme](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Bu da aynı anda bir VM oluşturmak için çalışır. Ancak, ortamı birden çok VM içeriyorsa, her bir VM tek tek oluşturulmalıdır. Çok katmanlı Web uygulaması veya bir SharePoint grubu gibi senaryolar için bir mekanizma, tek bir adımda birden çok VM oluşturulmasına izin vermek için gereklidir. Azure Resource Manager şablonları kullanarak şimdi altyapısı ve Azure çözümünüzü yapılandırmasını tanımlayın ve sürekli olarak tutarlı bir durumda birden çok VM dağıtın. Bu özellik, aşağıdaki avantajları sağlar:

- Azure Resource Manager şablonları doğrudan, kaynak denetim deposundan (GitHub veya Team Services Git) yüklenir.
- Diğer türleri ile yaparken yapılandırdıktan sonra kullanıcılarınızın bir ortam Azure portalı, Azure Resource Manager şablonu seçerek oluşturabilirsiniz [VM tabanları](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS kaynaklarına Iaas Vm'leri yanı sıra bir Azure Resource Manager şablonundan bir ortamda sağlanabilir.
- Maliyeti ortamlar, tek tek sanal makineleri tabanları diğer türleri tarafından oluşturulan ek olarak laboratuvarda izlenebilir.
- PaaS kaynaklarına oluşturulur ve maliyet izlemesi görüntülenir; Ancak, VM otomatik kapatma PaaS kaynaklarına geçerli değildir.
- Tek Laboratuvar VM'ler için sahip oldukları gibi kullanıcıların ortamlar için aynı VM ilke denetimi vardır.

Çok hakkında daha fazla bilgi [Resource Manager şablonlarını kullanmanın yararları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) dağıtmak için güncelleştirme veya tüm Laboratuvar kaynaklarınızı tek bir işlemde silin.

> [!NOTE]
> Daha fazla laboratuvarı sanal makineleri oluşturmak için temel olarak bir Resource Manager şablonunu kullandığınızda, çoklu sanal makineleri veya tek sanal makineleri oluştururken dikkate alınması gereken bazı farklılıklar vardır. [Bir sanal makinenin Azure Resource Manager şablonu kullanmak](devtest-lab-use-resource-manager-template.md) bu farklar daha ayrıntılı açıklanmıştır.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Azure Resource Manager şablonu depoları yapılandırın

Altyapı olarak kodu ve kod olarak yapılandırma en iyi yöntemlerle biri olarak ortamı şablonları kaynak denetiminde yönetilmelidir. Azure DevTest Labs, bu yöntem izler ve tüm Azure Resource Manager şablonları doğrudan, GitHub veya VSTS Git havuzların yükler. Sonuç olarak, Resource Manager şablonları test ortamından üretim ortamına tüm yayın döngüsü boyunca kullanılabilir.

Birkaç Azure Resource Manager şablonlarınızı bir havuzda düzenlemek için izlemek için kurallar şunlardır:

- Ana şablon dosyası olarak adlandırılmalıdır `azuredeploy.json`. 

    ![Anahtar Azure Resource Manager şablon dosyaları](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Bir parametre dosyasında tanımlanan parametre değerlerini kullanmak istiyorsanız, parametre dosyası olarak adlandırılmalıdır `azuredeploy.parameters.json`.
- Parametrelerini kullanabilirsiniz `_artifactsLocation` ve `_artifactsLocationSasToken` parametersLink URI değeri oluşturmak için otomatik olarak iç içe geçmiş şablonları yönetmek DevTest Labs izin verme. Bkz: [nasıl Azure DevTest Labs yapar iç içe geçmiş Resource Manager şablonu dağıtımları test ortamlarında daha kolay](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) daha fazla bilgi için.
- Meta veri şablon görünen adı ve açıklaması belirtmek için tanımlanabilir. Bu meta veriler adındaki bir dosyada olmalıdır `metadata.json`. Aşağıdaki örnek meta veri dosyası, görünen ad ve açıklama belirtin verilmektedir: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of the template>"
 
}
```

Aşağıdaki adımlar bir depo Azure portalını kullanarak Laboratuvarınızı eklerken size kılavuzluk. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin.   
1. Laboratuvar 's üzerinde **genel bakış** bölmesinde, **yapılandırma ve ilkeleri**.

    ![Yapılandırma ve ilkeleri](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Gelen **yapılandırma ve ilkeleri** ayarları listesinde **depoları**. **Depoları** bölmesi Laboratuvar için eklenene depoları listeler. Adlı depo `Public Repo` tüm Laboratuvarları için otomatik olarak oluşturulur ve bağlandığı [DevTest Labs GitHub deposuna](https://github.com/Azure/azure-devtestlab) kullanımınız için birden fazla VM yapılar içerir.

    ![Ortak depodaki](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Seçin **Ekle +** Azure Resource Manager şablonu deponuz eklemek için.
1. Zaman ikinci **depoları** bölmesi açılır, gerekli bilgileri aşağıdaki gibi girin:
    - **Ad** -laboratuar ortamında kullanılan depo adını girin.
    - **Git kopyalama URL'si** -GitHub ya da Visual Studio Team Services GIT HTTPS kopya URL'si girin.  
    - **Şube** -Azure Resource Manager şablonu tanımlarınızı erişmek için şube adı girin. 
    - **Kişisel erişim belirteci** -kişisel erişim belirteci deponuz güvenli bir şekilde erişmek için kullanılır. Visual Studio Team Services belirtecinizi almak için seçin  **&lt;adınız >> Profilim > Güvenlik > Genel erişim belirteci**. Github'dan belirtecinizi almak için seçerek ve ardından, avatar seçin **ayarlar > Genel erişim belirteci**. 
    - **Klasör yolları** -iki giriş alanlarının birini kullanarak bir eğik çizgiyle - / - başlayan ve yapı tanımlarınızı Git kopyalama URI ya da görelidir klasör yolu girin (ilk giriş alanı) veya Azure Resource Manager şablonu tanımları .   
    
        ![Ortak depodaki](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Tüm gerekli alanların girilir ve geçemediğinden sonra seçeneğini **kaydetmek**.

Sonraki bölümde bir Azure Resource Manager şablonu ortamları oluşturmada size yol gösterir.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir Resource Manager şablonu bir ortam oluşturmak

Bir Azure Resource Manager şablonu deposu laboratuar ortamında yapılandırıldıktan sonra Laboratuvar kullanıcılarınızın Azure portalı ile aşağıdaki adımları kullanarak bir ortam oluşturabilirsiniz:

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
1. İstenen Laboratuvar labs listesinden seçin.   
1. Laboratuvar 's bölmesinde seçin **Ekle +**.
1. **Bir temel seçin** bölmesinde listelenen ilk Azure Resource Manager şablonları ile kullanabileceğiniz temel görüntüleri görüntüler. İstenen Azure Resource Manager şablonu seçin.

    ![Bir temel seçin](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Üzerinde **Ekle** bölmesinde girin **ortam adı** değeri. Ortam laboratuvara kullanıcılarınıza görüntülenen addır. Kalan giriş alanları Azure Resource Manager şablonunda tanımlanır. Varsayılan değerleri şablonda tanımlanmışsa veya `azuredeploy.parameter.json` dosya varsa, varsayılan değerleri, bu girdi alanlarında görüntülenir. Tür parametreleri için *güvenli dize*, laboratuar ortamında 's depolanan gizli kullanabilirsiniz [kişisel gizli depolama](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Bölmesi ekleme](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > -Belirtilmiş olsa bile - boş değerler olarak görüntülenen birkaç parametre değerleri vardır. Bu nedenle, kullanıcıların bir Azure Resource Manager şablonu parametreleri için değerler atarsanız, DevTest Labs değerleri görüntülemez. Bunun yerine boş giriş alanları burada Laboratuvar kullanıcılar bir değer ortamı oluştururken girmelisiniz gösterilir.
    > 
    > - GEN BENZERSİZ
    > - GEN - BENZERSİZ-[N]
    > - GEN SSH-PUB-ANAHTAR
    > - GEN-PAROLA 
 
1. Seçin **Ekle** ortamı oluşturmak için. Ortam başlatır hemen durumu görüntüleme ile sağlama **My sanal makineleri** listesi. Yeni bir kaynak grubu, Azure Resource Manager şablonunda tanımlanan tüm kaynakları sağlamak üzere Laboratuvar tarafından otomatik olarak oluşturulur.
1. Ortamı oluşturulduktan sonra ortamda seçin **My sanal makineleri** listesi kaynak grubu bölmesini açın ve ortamda sağlanan tüm kaynakları göz atın.
    
    ![Sanal makineler listem](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   Ortamda yalnızca sağlanan VM'ler listesini görüntülemek için ortamı genişletebilirsiniz.
    
    ![Sanal makineler listem](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Veri diskleri, değişen otomatik kapatma süresi ve daha fazlasını ekleme yapıları, uygulama gibi kullanılabilir eylemler - görüntülemek için ortamları birini tıklatın.

    ![Ortam Eylemler](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="deploy-a-resource-manager-template-to-create-a-vm"></a>Bir VM oluşturmak için Resource Manager şablonu dağıtma
Resource Manager şablonu kaydedilir ve gereksinimlerinize göre özelleştirilmiş sonra VM oluşturmayı otomatikleştirmek için kullanabilirsiniz. 
- [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) kaynaklarınızı Azure'a dağıtmak için Resource Manager şablonları ile Azure PowerShell kullanmayı açıklar. 
- [Resource Manager şablonları ve Azure CLI kaynaklarla dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) kaynaklarınızı Azure'a dağıtmak için Resource Manager şablonları ile Azure CLI kullanmayı açıklar.

> [!NOTE]
> Yalnızca Laboratuvar sahip izinlerine sahip bir kullanıcı Azure PowerShell kullanarak sanal makineleri bir Resource Manager şablonu oluşturabilirsiniz. Resource Manager şablonu kullanarak VM oluşturmayı otomatikleştirmek istediğiniz ve yalnızca kullanıcı izinlerine sahip, kullanabileceğiniz [ **az Laboratuvar vm oluşturma** CLI komutunu](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).

### <a name="general-limitations"></a>Genel sınırlamalar 

Bu sınırlamalara Resource Manager şablonu DevTest Labs'de kullanırken göz önünde bulundurun:

- Oluşturduğunuz herhangi bir Resource Manager şablonu mevcut kaynaklarla başvuramaz; yalnızca yeni kaynaklar için de başvurabilir. Ayrıca, genellikle DevTest Labs dışında dağıttığınız var olan bir Resource Manager şablonu varsa ve var olan kaynaklara başvurular içerir, bu laboratuar ortamında kullanılamaz.

   Tek özel durum olan, **yapabilirsiniz** varolan bir sanal ağı başvuru. 

- Formülleri Laboratuvar bir Resource Manager şablondan oluşturulan sanal makineleri gelen oluşturulamıyor. 

- Özel resimler Laboratuvar bir Resource Manager şablondan oluşturulan sanal makineleri gelen oluşturulamıyor. 

- Resource Manager şablonları dağıttığınızda çoğu ilkeleri değerlendirilmez.

   Örneğin, bir kullanıcı yalnızca beş VM'ler oluşturabilir belirten bir laboratuvar ilkesi olabilir. Kullanıcı VM'ler düzinelerce oluşturan bir Resource Manager şablonu dağıtır, ancak bu izin verilir. Değerlendirilmez ilkeler aşağıdakileri içerir:

   - Kullanıcı başına VM sayısı
   - Laboratuvar kullanıcı başına premium VM sayısı
   - Premium diskleri Laboratuvar kullanıcı başına sayısı


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Ortam kaynak grubuna erişim hakları Laboratuvar kullanıcılar için yapılandırma

Laboratuvar kullanıcılar bir Resource Manager şablonu dağıtabilirsiniz. Ancak, varsayılan olarak, bunlar bir ortam kaynak grubundaki kaynaklar değiştiremezler anlamına gelir okuyucu erişim haklarına sahip. Örneğin, bunlar durduramaz veya kaynaklarını başlatın.

Laboratuvar kullanıcılarınızın katkıda bulunan erişim hakkı vermek için aşağıdaki adımları izleyin. Resource Manager şablonu dağıttığınızda, daha sonra bu ortam kaynaklarında düzenlemek seçebilecekler. 


1. Laboratuvarınızı 's üzerinde **genel bakış** bölmesinde, **yapılandırma ve ilkeleri**.
1. Seçin **Laboratuvar ayarları**.
1. Laboratuvar ayarları bölmesinde seçin **katkıda bulunan** Laboratuvar kullanıcılara yazma izinleri vermek için.

    ![Laboratuvar kullanıcı erişim haklarını Yapılandır](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar
* Bir VM oluşturulduktan sonra seçerek VM'ye bağlanabilir **Bağlan** VM'ın yönetim bölmesinde.
* Kaynakları görüntülemek ve bir ortamda ortamda seçerek yönetmek **My sanal makineleri** Laboratuvarınızı listesinde. 
* Araştır [Azure Resource Manager şablonları Azure hızlı başlangıç Şablon Galerisinden](https://github.com/Azure/azure-quickstart-templates).
