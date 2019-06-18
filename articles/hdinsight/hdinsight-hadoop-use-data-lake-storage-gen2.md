---
title: Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma
description: Azure HDInsight kümeleri ile Azure Data Lake depolama Gen2'ı kullanmayı öğrenin.
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: hrasheed
ms.openlocfilehash: f381090e663923ec9f45fba03d0688c9879ab173
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427393"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma

Azure Data Lake depolama Gen2'ye, büyük veri analizi, Azure Blob Depolama alanında oluşturulmuş adanmış bir bulut depolama hizmetidir. Data Lake depolama Gen2 Azure Blob Depolama ve Azure Data Lake depolama Gen1 özelliklerini bir araya getirir. Dosya sistemi sematiğini, dizin ve dosya düzeyinde güvenlik ve ölçeklenebilirlik, düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri birlikte gibi Azure Data Lake depolama Gen1 ' elde edilen hizmet özellikleri sunar Azure Blob depolama alanından.

## <a name="data-lake-storage-gen2-availability"></a>Data Lake depolama Gen2 kullanılabilirlik

Data Lake depolama Gen2, neredeyse tüm Azure HDInsight küme türleri için hem varsayılan hem de ek bir depolama hesabı olarak bir depolama seçeneği olarak kullanılabilir. HBase, ancak yalnızca bir Data Lake depolama Gen2 hesabı olabilir.

> [!Note]  
> Data Lake depolama 2. nesil olarak seçtikten sonra **birincil depolama türü**, ek depolama alanı olarak Data Lake depolama Gen1 hesabı seçemezsiniz.

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-portal"></a>Azure portalı üzerinden Data Lake depolama 2. nesil ile küme oluşturma

Depolama için Data Lake depolama Gen2 kullanan bir HDInsight kümesi oluşturmak için Data Lake depolama Gen2 hesabı yapılandırmak için aşağıdaki adımları izleyin.

### <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Bir kullanıcı tarafından atanan yönetilen kimlik, zaten yoksa, oluşturun. Bkz: [oluşturun, liste, delete veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik rol atama](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity). Azure HDInsight yönetilen kimlikleri çalışması hakkında daha fazla bilgi için bkz. [yönetilen Azure HDInsight kimliklerini](hdinsight-managed-identities.md).

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/hdinsight-hadoop-use-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

### <a name="create-a-data-lake-storage-gen2-account"></a>Bir Data Lake depolama Gen2 hesabı oluşturun

Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturun. Emin olun **hiyerarşik ad alanı** seçeneği etkinleştirilir. Daha fazla bilgi için [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](../storage/blobs/data-lake-storage-quickstart-create-account.md).

![Azure portalında depolama hesabı oluşturmayı gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)

### <a name="set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account"></a>Data Lake depolama Gen2 hesabındaki yönetilen kimlik için izinleri ayarla

Yönetilen kimlik için Ata **depolama Blob verileri sahibi** depolama hesabındaki rol. Daha fazla bilgi için [RBAC (Önizleme) ile Azure Blob ve kuyruk verilere erişim haklarını yönetme](../storage/common/storage-auth-aad-rbac.md).

1. İçinde [Azure portalında](https://portal.azure.com)depolama hesabınıza gidin.
1. Depolama hesabınızı seçin ve ardından **erişim denetimi (IAM)** hesabı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.
    
    ![Depolama erişim denetimi ayarları gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
1. Seçin **+ rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
1. İçinde **rol ataması Ekle** penceresinde **depolama Blob verileri sahibi** rol. Daha sonra yönetilen bir kimlik ve depolama hesabını içeren aboneliği seçin. Ardından, kullanıcı tarafından atanan ve daha önce oluşturduğunuz yönetilen kimlik bulmak için arama yapın. Son olarak, yönetilen kimlik'i seçin ve altında listelenir **seçili üyeleri**.
    
    ![Bir RBAC rolü atayın gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role3.png)
    
1. **Kaydet**’i seçin. Seçtiğiniz kullanıcı tarafından atanan kimliği artık seçili rolü altında listelenir.
1. Bu ilk Kurulum tamamlandıktan sonra portal üzerinden bir küme oluşturabilirsiniz. Küme, depolama hesabı aynı Azure bölgesinde olmalıdır. İçinde **depolama** bölümüne küme oluşturma menüsünden, aşağıdaki seçenekleri belirleyin:
        
    * İçin **birincil depolama türü**seçin **Azure Data Lake depolama Gen2**.
    * Altında **bir depolama hesabı seçin**, arayın ve yeni oluşturduğunuz Data Lake depolama Gen2'ye depolama hesabını seçin.
        
        ![Data Lake depolama Gen2 Azure HDInsight ile kullanmak için depolama ayarları](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
    
    * Altında **kimlik**, doğru aboneliği seçin ve yeni oluşturulan kullanıcı tarafından atanan yönetilen kimliği.
        
        ![Kimlik ayarları, Azure HDInsight ile Data Lake depolama Gen2 kullanma](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)
        
> [!Note]
> Depolama hesabı düzeyinde ikincil bir Data Lake depolama Gen2 hesabı eklemek için eklemek istediğiniz yeni Data Lake depolama Gen2'ye depolama hesabı için daha önce oluşturulan yönetilen kimlik yalnızca atayın. Lütfen HDInsight üzerinde "ek depolama hesapları" dikey penceresi aracılığıyla ikincil bir Data Lake depolama Gen2 hesap ekleme desteklenmiyor dikkat edin. 

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-cli"></a>Data Lake depolama Gen2 aracılığıyla Azure CLI ile küme oluşturma

Yapabilecekleriniz [örnek şablon dosya indirme](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) ve [örnek parametre dosyasını indirin](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json). Şablon kullanmadan önce dize değiştirin `<SUBSCRIPTION_ID>` gerçek Azure abonelik kimliğinizi Ayrıca, dize değiştirin `<PASSWORD>` hem kümenize oturum açma için kullanacağınız parola ve SSH parolasını ayarlamak için seçtiğiniz parolayla.

Aşağıdaki kod parçacığı ilk aşağıdakileri yapar:

1. Azure hesabınızda günlükleri.
1. Etkin aboneliği oluşturma işlemleri burada yapılır ayarlar.
1. Adlı yeni bir dağıtım etkinlikler için yeni bir kaynak grubu oluşturur `hdinsight-deployment-rg`.
1. Adlı bir kullanıcı tarafından atanan yönetilen kimliği oluşturan `test-hdinsight-msi`.
1. Data Lake depolama 2. nesil için özellikleri kullanmak için Azure CLI uzantısı ekler.
1. Adlı yeni bir Data Lake depolama Gen2 hesabı oluşturur `hdinsightadlsgen2`, kullanarak `--hierarchical-namespace true` bayrağı.

```azurecli
az login
az account set --subscription <subscription_id>

# Create resource group
az group create --name hdinsight-deployment-rg --location eastus

# Create managed identity
az identity create -g hdinsight-deployment-rg -n test-hdinsight-msi

az extension add --name storage-preview

az storage account create --name hdinsightadlsgen2 \
    --resource-group hdinsight-deployment-rg \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Ardından, portalda oturum açın. Eklemek için yeni kullanıcı tarafından atanan yönetilen kimlik **depolama Blob verileri katkıda bulunan** 3. adım altında açıklandığı gibi depolama hesabındaki rol [Azure portalını kullanarak](hdinsight-hadoop-use-data-lake-storage-gen2.md).

Rolü için kullanıcı tarafından atanan bir yönetilen kimlik atadıktan sonra aşağıdaki kod parçacığını kullanarak şablonu dağıtın.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group hdinsight-deployment-rg \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Data Lake depolama 2. nesil'deki HDInsight için erişim denetimi

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Ne tür izinler, Data Lake depolama Gen2 destekliyor mu?

Data Lake depolama Gen2, rol tabanlı erişim denetimi (RBAC) hem de benzer POSIX erişim denetim listeleri (ACL'ler) destekleyen bir erişim denetimi modeli kullanır. Data Lake depolama Gen1 destekler erişim denetimi listeleri'yalnızca veri erişimi denetlemek için.

RBAC, kullanıcılara, gruplara veya hizmet sorumlularının Azure kaynakları için izin kümelerini etkili bir şekilde uygulamak için rol atamalarını kullanır. Genellikle, bu Azure kaynaklarını en üst düzey kaynaklar (örneğin, Azure depolama hesapları) göre kısıtlanır. Azure depolama ve Data Lake depolama Gen2 Ayrıca, bu mekanizma dosya sistemi kaynağa genişletilmiştir.

 RBAC ile dosya izinleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Dosya izinleri ACL'lerine sahip hakkında daha fazla bilgi için bkz. [erişim denetim listeleri dosyaların ve dizinlerin](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="how-do-i-control-access-to-my-data-in-data-lake-storage-gen2"></a>Data Lake depolama Gen2 verilerimi için erişimi nasıl denetlerim?

Data Lake depolama Gen2 içindeki dosyalara erişme yeteneğini HDInsight kümenizin yönetilen kimlikleri denetlenir. Yönetilen bir kimlik, Azure Active Directory'de kimlik bilgileri, Azure tarafından yönetilir (Azure AD) kayıtlı bir kimliktir. Yönetilen kimliklerle hizmet sorumluları, Azure AD'ye kaydetme ve sertifikalar gibi kimlik bilgilerini güncelleştirmek gerekmez.

Azure hizmetlerine yönetilen kimlikleri iki tür vardır: sistem tarafından atanan ve kullanıcı tarafından atanan. HDInsight, Data Lake depolama Gen2'ye erişim kullanıcı tarafından atanan yönetilen kimlikleri kullanır. Kullanıcı tarafından atanan bir yönetilen kimlik, bir tek başına Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir.

Kullanıcı tarafından atanan kimliğin yaşam döngüsü, bu kimliğin atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynaklarını iş yönetilen biçimimi nasıl yapılacağı?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services"></a>Nasıl izinleri Azure AD kullanıcıları için Data Lake depolama Gen2 verilerde sorgu Hive veya diğer hizmetleri kullanarak ayarlayabilirim?

Verileri sorgulamak için kullanıcıların izinleri ayarlamak için ACL'leri atanan sorumlu olarak Azure AD güvenlik grupları kullanın. Dosya erişim izinlerini doğrudan bireysel kullanıcılar veya hizmet sorumlularına atamayın. İzinleri akışını denetlemek için Azure AD güvenlik gruplarını kullandığınızda, ekleme ve kullanıcı veya hizmet sorumluları için tüm dizin yapısının ACL'leri yeniden uygulama olmadan kaldırın. Yalnızca eklemek veya kullanıcıların uygun kaldırmak kullandığınız Azure AD güvenlik grubu. Her dosya ve alt noktasındaki ACL güncelleniyor gerekir ACL'leri yeniden uygulama için ACL'leri devralınmaz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight Tümleştirmesi ile Data Lake depolama Gen2 Önizleme - ACL ve güvenlik güncelleştirmesi](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Azure Data Lake depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
