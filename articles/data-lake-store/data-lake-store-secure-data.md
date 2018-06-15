---
title: Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama | Microsoft Docs
description: Grupları kullanarak Azure Data Lake Store'da verilerin güvenliğini sağlamak öğrenin ve erişim denetim listeleri
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 5b83f02c55d0aa7b2e122d7fc8c9ef5734cdd924
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34197044"
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama
Azure Data Lake Store'da verilerin güvenliğini sağlama üç adımlık bir yaklaşımdır.  Her ikisi de rol tabanlı erişim denetimi (RBAC) ve tam olarak kullanıcılar ve güvenlik grupları için veri erişimini etkinleştirmek için erişim denetim listelerini (ACL'ler) ayarlamanız gerekir.

1. Azure Active Directory (AAD içinde) güvenlik grupları oluşturarak başlayın. Bu güvenlik grupları, Azure portalında rol tabanlı erişim denetimi (RBAC) uygulamak için kullanılır. Daha fazla bilgi için bkz: [Microsoft Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
2. AAD güvenlik gruplarının Azure Data Lake Store hesabına atayın. Bu erişim denetimleri Data Lake Store hesabından portalı veya API'ler portal ve yönetim işlemleri için.
3. AAD güvenlik gruplarının Data Lake Store dosya sistemi üzerinde denetim listeleri (ACL) erişimi atayın.
4. Ayrıca, Data Lake Store'da verilere erişebilir istemciler için bir IP adresi aralığı ayarlayabilirsiniz.

Bu makalede yukarıdaki görevleri gerçekleştirmek için Azure portalını kullanma hakkında yönergeler sağlar. Data Lake Store hesabı ve veri düzeyinde güvenlik nasıl uyguladığını hakkında ayrıntılı bilgi için bkz: [Azure Data Lake Store'da güvenlik](data-lake-store-security-overview.md). ACL'ler Azure Data Lake Store içinde nasıl uygulandığını derin Dalış hakkında bilgi için bkz: [Data Lake Store'da erişim denetimine genel bakış](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Azure Active Directory'de güvenlik grupları oluşturma
AAD güvenlik gruplarının nasıl oluşturulacağı ve gruba kullanıcı ekleme hakkında yönergeler için bkz: [Azure Active Directory'de güvenlik gruplarını yönetme](../active-directory/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Azure portalını kullanarak Azure AD'de bir gruba hem kullanıcı hem de diğer grupları ekleyebilirsiniz. Ancak, bir hizmet sorumlusu bir gruba eklemek için kullanmak [Azure AD PowerShell Modülü](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Kullanıcıların veya güvenlik gruplarının için Azure Data Lake Store hesaplarını atayın
Azure Data Lake Store hesapları için kullanıcıların veya güvenlik gruplarının atadığınızda, yönetim işlemlerinin Azure portalı ve Azure Resource Manager API'leri kullanılarak hesabındaki erişimi denetler. 

1. Bir Azure Data Lake Store hesabını açın. Sol bölmeden tıklatın **tüm kaynakları**ve ardından tüm kaynaklar dikey penceresinden bir kullanıcı veya güvenlik grubu atamak istediğiniz hesap adına tıklayın.

2. Data Lake Store hesabı dikey penceresinde tıklayın **erişim denetimi (IAM)**. Varsayılan dikey abonelik sahipleri sahibi olarak listeler.
   
    ![Azure Data Lake Store hesabına güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake Store hesabını güvenlik grubuna atayın")

3. İçinde **erişim denetimi (IAM)** dikey penceresinde tıklatın **Ekle** açmak için **izinleri eklemek** dikey. İçinde **izinleri eklemek** dikey penceresinde, select bir **rol** kullanıcı/grup için. Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunun arayın ve seçin. Kullanıcıların ve grupların gelen aramak için çok varsa, **seçin** grup adına filtrelemek için metin kutusu. 
   
    ![Kullanıcı için bir rol ekleme](./media/data-lake-store-secure-data/adl.add.user.1.png "kullanıcı için bir rol Ekle")
   
    **Sahibi** ve **katkıda bulunan** rolü data lake hesabı yönetim işlevleri, çeşitli erişim sağlar. Bunları Ekle veri gölü ancak hala hesap yönetim bilgilerini görüntülemek için gerek verilerde etkileşim kuracağı kullanıcılar için **okuyucu** rol. Bu rolleri kapsamını Azure Data Lake Store hesabına ilgili yönetim işlemleri sınırlıdır.
   
    Veri işlemleri için kullanıcıları yapabileceklerinizi tek tek dosya sistemi izinleri tanımlayın. Bu nedenle, okuyucu rolüne sahip bir kullanıcı hesabıyla ilişkilendirilmiş yönetim ayarlarını yalnızca görüntüleme ancak potansiyel olarak okuyabilir ve dosya sistemi izinleri atanmış göre veri yazma. Data Lake Store dosya sistemi izinleri adresindeki açıklanmıştır [Ata güvenlik grubuna ACL'ler olarak Azure Data Lake Store dosya sistemi](#filepermissions).

    > [!IMPORTANT]
    > Yalnızca **sahibi** rolü otomatik olarak dosya sistemi erişimini etkinleştirir. **Katkıda bulunan**, **okuyucu**, ve diğer tüm rolleri herhangi bir dosya ve klasörleri için erişim düzeyini etkinleştirmek ACL'leri gerektirir.  **Sahibi** rol süper kullanıcı dosya ve ACL geçersiz kılınamaz klasör izinleri sağlar. Veri erişimi RBAC ilkeleri nasıl eşleme ile ilgili daha fazla bilgi için bkz: [hesap yönetimi için RBAC](data-lake-store-security-overview.md#rbac-for-account-management).

4. Listede bir grubu/kullanıcısı eklemek isteyip istemediğinizi **izinleri eklemek** dikey penceresinde davet edebildiğiniz bunları kendi e-posta adresi yazarak **seçin** metin kutusuna ve ardından bunları listeden seçerek.
   
    ![Güvenlik Grubu Ekle](./media/data-lake-store-secure-data/adl.add.user.2.png "güvenlik grubu Ekle")
   
5. **Kaydet**’e tıklayın. Aşağıda gösterildiği gibi eklenen güvenlik grubu görmeniz gerekir.
   
    ![Güvenlik grubuna eklenen](./media/data-lake-store-secure-data/adl.add.user.3.png "güvenlik grubuna eklendi")

6. Kullanıcı/güvenlik grubunun artık Azure Data Lake Store hesabına erişim izni vardır. Belirli kullanıcılara erişim sağlamak istiyorsanız, bunları güvenlik grubuna ekleyebilirsiniz. Benzer şekilde, bir kullanıcının erişimi iptal etmek istiyorsanız, bunları güvenlik grubundan kaldırabilirsiniz. Ayrıca, birden çok güvenlik grubu için bir hesap atayabilirsiniz. 

## <a name="filepermissions"></a>Kullanıcıların veya güvenlik gruplarının ACL'ler Azure Data Lake Store dosya sistemine atayın.
Azure Data Lake dosya sistemine kullanıcı/güvenlik gruplarına atayarak Azure Data Lake Store içinde depolanan veriler üzerinde erişim denetimi ayarlayın.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Veri Gezgini üzerinden verileri görüntüleme](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Veri Gezgini üzerinden verileri görüntüleme")
2. İçinde **Veri Gezgini** dikey penceresinde, ACL yapılandırmak ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosyaya ACL'ler atamak için önce önizleme ve ardından dosyaya tıklatmalısınız **erişim** gelen **Dosya Önizleme** dikey.
   
    ![Data Lake dosya sisteminde ACL'leri ayarlamak](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi üzerindeki ACL'lerin ayarlayın")
3. **Erişim** dikey sahipleri listeler ve kök atanmış izinleri atanır. Tıklatın **Ekle** ek erişim ACL eklemek için simge.
    > [!IMPORTANT]
    > Tek bir dosya erişim izinlerini ayarlama mutlaka bir kullanıcı/grup bu dosyaya erişim izni yok. Dosya yoluna atanan kullanıcı/Grup erişilebilir olması gerekir. Daha fazla bilgi ve örnekler için bkz: [yaygın senaryolar ilgili izinleri](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Liste standart ve özel erişim](./media/data-lake-store-secure-data/adl.acl.2.png "listesinde standart ve özel erişim")
   
   * **Sahipleri** ve **diğerlerinin** UNIX stili erişim sağlayan, sizin belirlediğiniz okuma, yazma, üç ayrı kullanıcı sınıfları (rwx) yürütme: sahibi, Grup ve diğerleri.
   * **Atadığınız izinlerden** belirli adlandırılmış kullanıcılar veya gruplar dosyanın sahibi veya grup ötesinde için izinleri ayarlamanızı sağlar POSIX ACL'leri karşılık gelir. 
     
     Daha fazla bilgi için bkz: [HDFS ACL'leri](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). ACL'ler Data Lake Store içinde nasıl uygulandığı hakkında daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](data-lake-store-access-control.md).
4. Tıklatın **Ekle** açmak için simgesini **izinleri atamak** dikey. Bu dikey pencerede tıklatın **kullanıcı veya grup seçin**ve ardından **kullanıcı veya grup seçin** dikey penceresinde, Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunun arayın. Gelen arama gruplarının çok varsa, metin kutusunun en üstünde grup adına filtrelemek için kullanın. Ekleyin ve ardından istediğiniz Grup tıklatın **seçin**.
   
    ![Grup ekleme](./media/data-lake-store-secure-data/adl.acl.3.png "grup ekleme")
5. Tıklatın **seçin izinleri**seçin ACL veya her ikisi de varsayılan izinleri, izinleri yinelemeli olarak uygulanması gereken ve bir erişim, ACL izinleri atamak istediğiniz. **Tamam**’a tıklayın.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.4.png "gruplandırmak için izinler atama")
   
    Data Lake Store ve varsayılan/erişim ACL izinleri hakkında daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](data-lake-store-access-control.md).
6. ' I tıklattıktan sonra **Tamam** içinde **izinleri seçin** dikey penceresinde, yeni eklenen grup ve ilgili izinleri de artık listelenir **erişim** dikey.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.5.png "gruplandırmak için izinler atama")
   
   > [!IMPORTANT]
   > Geçerli sürümde, en fazla 28 girişleri altında bulunabilir **atanan izinlere**. Birden fazla 28 kullanıcılar eklemek istiyorsanız, oluşturduğunuz güvenlik grupları, kullanıcı güvenlik gruplarına ekleme, eklemek için bu güvenlik grupları Data Lake Store hesabı için erişim sağlamak.
   > 
   > 
7. Grup ekledikten sonra gerekirse, erişim izinlerini değiştirebilirsiniz. Kaldırın veya bu izin güvenlik grubuna atamak isteyip istemediğinizi üzerinde temel her izin türü (okuma, yazma, yürütme) onay kutusunu seçin veya temizleyin. Tıklatın **kaydetmek** değişiklikleri kaydetmek için veya **atmak** değişiklikleri geri almak için.

## <a name="set-ip-address-range-for-data-access"></a>Veri erişimi için IP adres aralığını ayarlayın
Azure Data Lake Store, başka kilitleme veri deponuza ağ düzeyinde erişim sağlar. Güvenlik duvarını etkinleştir, bir IP adresi belirtin veya güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Etkinleştirildikten sonra IP adreslerini tanımlı aralığın içinde olan istemciler deposuna bağlanabilirsiniz.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-secure-data/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı için güvenlik grupları kaldırın
Azure Data Lake Store hesaplarını güvenlik grupları kaldırdığınızda, Azure portalı ve Azure Resource Manager API'leri kullanarak hesap yönetimi işlemleri için yalnızca değiştiriyorsunuz.  

Veri erişimi değiştirilmemiştir ve hala ACL erişimi tarafından yönetilir.  Bunun özel durumu olan kullanıcıları/grupları sahipleri rolü.  Kullanıcıları/grupları sahipleri rolden Süper kullanıcılar artık olmayan ve bunların erişim erişim ACL ayarlarına geri döner. 

1. Data Lake Store hesabı dikey penceresinde tıklayın **erişim denetimi (IAM)**. 
   
    ![Azure Data Lake hesabına güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake hesabına Ata güvenlik grubu")
2. İçinde **erişim denetimi (IAM)** dikey penceresinde kaldırmak istediğiniz güvenlik gruplarını'ı tıklatın. Tıklatın **kaldırmak**.
   
    ![Güvenlik grubu kaldırıldı](./media/data-lake-store-secure-data/adl.remove.group.png "güvenlik grubu kaldırıldı")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Güvenlik grubu ACL'ler Azure Data Lake Store dosya sisteminden kaldırın
Güvenlik grubu ACL'ler Azure Data Lake Store dosya sisteminden kaldırdığınızda, Data Lake Store'da verilere erişimi değiştirme.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake hesabında dizin oluşturmak](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Data Lake hesabında dizinler oluşturma")
2. İçinde **Veri Gezgini** dikey penceresinde, ACL kaldırın ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosya için ACL'leri kaldırmak için ilk önizleme ve ardından dosyaya tıklatmalısınız **erişim** gelen **Dosya Önizleme** dikey. 
   
    ![Data Lake dosya sisteminde ACL'leri ayarlamak](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi üzerindeki ACL'lerin ayarlayın")
3. İçinde **erişim** dikey penceresinde kaldırmak istediğiniz güvenlik grubunu tıklatın. İçinde **erişim ayrıntıları** dikey penceresinde tıklatın **kaldırmak**.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.remove.acl.png "gruplandırmak için izinler atama")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store'a ilişkin tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md)

