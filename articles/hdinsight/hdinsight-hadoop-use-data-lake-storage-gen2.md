---
title: Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma
description: Azure Data Lake depolama Gen2, Azure HDInsight kümeleri kullanım oluşturmayı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: howto
ms.date: 01/10/2019
ms.author: hrasheed
ms.openlocfilehash: 1d43c7b6dd1bdec0a2507d8ce1a3883f5ce31a39
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54479566"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma

Azure Data Lake Storage (Data Lake depolama) 2. nesil, büyük veri analizi, Azure Blob Depolama alanında oluşturulmuş için ayrılmış özellikleri kümesidir. Data Lake depolama Gen2 Azure Blob Depolama ve Azure Data Lake depolama Gen1 yeteneklerini birleştirerek sonucudur. Sonucu gibi dosya sistemi sematiğini, dizin ve dosya düzeyinde güvenlik ve ölçek düşük maliyetli, katmanlı depolama yanı sıra yüksek kullanılabilirlik/olağanüstü durum kurtarma özelliklerini Azure Blob, Azure Data Lake depolama Gen1 ' özellikleri sunan bir hizmettir depolama alanı.

## <a name="data-lake-storage-gen2-availability"></a>Data Lake depolama Gen2 kullanılabilirlik

Azure Data Lake depolama Gen2, neredeyse tüm Azure HDInsight küme türleri için hem varsayılan hem de ek bir depolama hesabı olarak bir depolama seçeneği olarak kullanılabilir. HBase, ancak yalnızca tek bir Data Lake depolama Gen2 hesap olabilir.

> [!Note] 
> Data Lake depolama 2. nesil olarak seçtiğinizde, **birincil depolama türü**, ek depolama alanı olarak Data Lake depolama Gen1 hesabı seçemezsiniz.

## <a name="creating-an-hdinsight-cluster-with-data-lake-storage-gen2"></a>Data Lake depolama Gen2'ile bir HDInsight kümesi oluşturma

Data Lake depolama Gen2 için depolamayı kullanan bir HDInsight kümesi oluşturmak için doğru yapılandırılmış bir Data Lake depolama Gen2 hesabı oluşturmak için aşağıdaki adımları kullanın.

1. Bir kullanıcı tarafından atanan yönetilen kimlik, zaten yoksa, oluşturun. Bkz: [oluşturun, liste, delete veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik rol atama](/../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal#create-a-user-assigned-managed-identity.md).

    ![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/hdinsight-hadoop-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

1. Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturun. Emin **hiyerarşik dosya sistemi** seçeneği etkinleştirilir. Bkz: [hızlı başlangıç: Bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](/../storage/blobs/data-lake-storage-quickstart-create-account.md) daha fazla ayrıntı için.

    ![Azure portalında depolama hesabı oluşturmayı gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)
 
1. Yönetilen kimlik için Ata **depolama Blob verileri katkıda bulunan (Önizleme)** depolama hesabındaki rol. Bkz: [RBAC (Önizleme) ile Azure Blob ve kuyruk verilere erişim haklarını yönetme](/../storage/common/storage-auth-aad-rbac#assign-a-role-scoped-to-the-storage-account-in-the-azure-portal.md)

    1. İçinde [Azure portalında](https://portal.azure.com), depolama hesabınıza gidin.
    1. Depolama hesabınızı seçin ve ardından **erişim denetimi (IAM)** hesabı için erişim denetimi ayarlarını görüntülemek için. Seçin **rol atamaları** rol atamaları listesini görmek için sekmesinde.
    
        ![Depolama erişim denetimi ayarları gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
    1. Tıklayın **rol ataması Ekle** düğmesini yeni bir rolü ekleyin.
    1. İçinde **rol ataması Ekle** penceresinde **depolama Blob verileri katkıda bulunan (Önizleme)** rol. Daha sonra yönetilen bir kimlik ve depolama hesabını içeren aboneliği seçin. Ardından, kullanıcı tarafından atanan ve daha önce oluşturduğunuz yönetilen kimlik bulmak için arama yapın. Son olarak, yönetilen kimlik'i seçin ve altında listelenir **seçili üyeleri**.
    
        ![Bir RBAC rolü atayın gösteren ekran görüntüsü](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role2.png)
    
    1. **Kaydet**’e tıklayın. Seçtiğiniz kullanıcı tarafından atanan kimliği artık altında listelenen **katkıda bulunan** rol.

    1. Bu ilk Kurulum tamamlandıktan sonra portal üzerinden bir küme oluşturabilirsiniz. Küme, depolama hesabı aynı Azure bölgesinde olmalıdır. İçinde **depolama** bölümüne küme oluşturma menüsünden, aşağıdaki seçenekleri belirleyin:
        
        * İçin **birincil depolama türü**, tıklayın **Azure Data Lake depolama Gen2**.
        * Altında **bir depolama hesabı seçin**, arayın ve yeni oluşturduğunuz Data Lake depolama Gen2'ye depolama hesabını seçin.
        
            ![Data Lake depolama Gen2 Azure HDInsight ile kullanmak için depolama ayarları](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
        
        * Altında **kimlik** doğru aboneliği seçin ve yeni oluşturulan kullanıcı-yönetilen bir kimlik atanır.
        
            ![Kimlik ayarları, Azure HDInsight ile Data Lake depolama Gen2 kullanma](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Data Lake depolama 2. nesil'deki HDInsight için erişim denetimi

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Ne tür izinler, Data Lake depolama Gen2 destekliyor mu?

Azure Data Lake depolama 2. nesil hem Azure rol tabanlı erişim denetimi (RBAC) hem de benzer POSIX erişim denetim listeleri (ACL'ler) destekleyen bir erişim denetimi modeli kullanır. Verilere erişimi denetlemek için Data Lake yalnızca desteklenen depolama Gen1 erişim denetim listeleri.

Azure rol tabanlı erişim denetimi (RBAC), kullanıcılara, gruplara veya hizmet sorumlularının Azure kaynakları için izin kümelerini etkili bir şekilde uygulamak için rol atamalarını kullanır. Genellikle, bu Azure kaynakları en üst düzey kaynaklar (örneğin, Azure depolama hesabı) göre kısıtlanır. Azure depolama ve Azure Data Lake depolama Gen2 Ayrıca, bu mekanizma dosya sistemi kaynağa genişletilmiştir.

 RBAC ile dosya izinleri hakkında daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (RBAC)](/../storage/blobs/data-lake-storage-access-control#azure-role-based-access-control-rbac.md).

Dosya izinleri ACL'lerine sahip hakkında daha fazla bilgi için bkz. [erişim denetim listeleri dosyaların ve dizinlerin](/../storage/blobs/data-lake-storage-access-control#access-control-lists-on-files-and-directories.md).


### <a name="how-do-i-control-access-to-my-data-in-gen2"></a>Gen2 verilerimi için erişimi nasıl denetlerim?

Özelliği, HDInsight kümeniz, Data Lake depolama Gen2 dosyalara erişmek yönetilen kimlikleri denetlenir. Yönetilen bir kimlik, kimlik bilgileri Azure tarafından yönetilen Azure AD'de kayıtlı bir kimliktir. Hizmet sorumluları, Azure AD'ye kaydetme ve sertifikalar gibi kimlik bilgilerini korumak gerekmez.

Yönetilen kimliklerinin Azure Hizmetleri için iki tür vardır: sistem tarafından atanan ve kullanıcı tarafından atanan. Azure HDInsight, Azure Data Lake depolama Gen2'ye erişim kullanıcı tarafından atanan yönetilen kimlikleri kullanır. Kullanıcı tarafından atanan bir yönetilen kimlik, bir tek başına Azure kaynağı olarak oluşturulur. Bir oluşturma işlemi çerçevesinde, Azure kullanılan abonelik tarafından güvenilen Azure AD kiracısında bir kimlik oluşturur. Kimlik oluşturulduktan sonra, bir veya birden çok Azure hizmet örneğine atanabilir. Kullanıcı tarafından atanan kimliğin yaşam döngüsü, bu kimliğin atandığı Azure hizmet örneklerinin yaşam döngüsünden ayrı olarak yönetilir. Yönetilen kimlikleri hakkında daha fazla bilgi için bkz. [Azure kaynaklarını iş yönetilen biçimimi nasıl yaptığını](/../active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka.md).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-using-hive-or-other-services"></a>Hive veya diğer hizmetler kullanarak verileri sorgulamak, Data Lake depolama Gen2'ye Azure AD kullanıcıları için izinleri nasıl ayarlarım?

ACL'ler atanan sorumlu olarak Azure AD güvenlik grupları kullanın. Doğrudan bireysel kullanıcıları veya dosya erişim izinlerini ile hizmet sorumluları atamayın. İzinleri akışını denetlemek için AD güvenlik grupları'nı kullandığınızda, ekleyebilir ve kullanıcı veya hizmet sorumluları için tüm dizin yapısının ACL'leri yeniden uygulama olmadan kaldırın. Yalnızca eklemek veya kullanıcıların uygun kaldırmak kullandığınız Azure AD güvenlik grubu. ACL'ler devralınmaz ve bu nedenle ACL'leri yeniden uygulama her dosya ve alt noktasındaki ACL güncelleniyor gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake depolama Gen2 önizlemesi Azure HDInsight kümeleri ile kullanma](/../storage/blobs/data-lake-storage-use-hdi-cluster.md)
* [Azure HDInsight Tümleştirmesi ile Data Lake depolama Gen2 Önizleme - ACL ve güvenlik güncelleştirmesi](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Azure Data Lake depolama Gen2 Önizleme giriş](/../storage/blobs/data-lake-storage-introduction.md)