---
title: "Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama | Microsoft Docs"
description: "Grupları kullanarak Azure Data Lake Store'da verilerin güvenliğini sağlamak öğrenin ve erişim denetim listeleri"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 70483cc7edf0aa9eaac03bbd0dc9b7e8b946a7ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama
Azure Data Lake Store'da verilerin güvenliğini sağlama üç adımlık bir yaklaşımdır.

1. Azure Active Directory (AAD içinde) güvenlik grupları oluşturarak başlayın. Bu güvenlik grupları, Azure portalında rol tabanlı erişim denetimi (RBAC) uygulamak için kullanılır. Daha fazla bilgi için bkz: [Microsoft Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).
2. AAD güvenlik gruplarının Azure Data Lake Store hesabına atayın. Bu erişim denetimleri Data Lake Store hesabından portalı veya API'ler portal ve yönetim işlemleri için.
3. AAD güvenlik gruplarının Data Lake Store dosya sistemi üzerinde denetim listeleri (ACL) erişimi atayın.
4. Ayrıca, Data Lake Store'da verilere erişebilir istemciler için bir IP adresi aralığı ayarlayabilirsiniz.

Bu makalede yukarıdaki görevleri gerçekleştirmek için Azure portalını kullanma hakkında yönergeler sağlar. Data Lake Store hesabı ve veri düzeyinde güvenlik nasıl uyguladığını hakkında ayrıntılı bilgi için bkz: [Azure Data Lake Store'da güvenlik](data-lake-store-security-overview.md). ACL'ler Azure Data Lake Store içinde nasıl uygulandığını derin Dalış hakkında bilgi için bkz: [Data Lake Store'da erişim denetimine genel bakış](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Azure Active Directory'de güvenlik grupları oluşturma
AAD güvenlik gruplarının nasıl oluşturulacağı ve gruba kullanıcı ekleme hakkında yönergeler için bkz: [Azure Active Directory'de güvenlik gruplarını yönetme](../active-directory/active-directory-accessmanagement-manage-groups.md).

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

1. Bir Azure Data Lake Store hesabını açın. Sol bölmeden tıklatın **Gözat**, tıklatın **Data Lake Store**ve ardından Data Lake Store dikey penceresinden bir kullanıcı veya güvenlik grubu atamak istediğiniz hesap adına tıklayın.

2. Data Lake Store hesabı ayarlar dikey penceresinde tıklayın **erişim denetimi (IAM)**. Varsayılan listeleri tarafından dikey **abonelik yöneticileri** Grup sahibi olarak.
   
    ![Azure Data Lake Store hesabına güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake Store hesabını güvenlik grubuna atayın")

    Bir grubu ekleyin ve ilgili rolleri atamak için iki yolu vardır.
   
    * Kullanıcı/grup hesabına eklemek ve bir role atayın veya
    * Bir rolü eklemek ve kullanıcıları/grupları rolüne atayın.
     
    Bu bölümde, biz ilk yaklaşım bir grup ekleme ve rol atama arayın. Önce bir rol seçin ve ardından grupları o role atamak için benzer adımları gerçekleştirebilirsiniz.
4. İçinde **kullanıcılar** dikey penceresinde tıklatın **Ekle** açmak için **erişim Ekle** dikey. İçinde **erişim Ekle** dikey penceresinde tıklatın **bir rol seçin**ve ardından kullanıcı/grup için bir rol seçin.
   
    ![Kullanıcı için bir rol ekleme](./media/data-lake-store-secure-data/adl.add.user.1.png "kullanıcı için bir rol Ekle")
   
    **Sahibi** ve **katkıda bulunan** rolü data lake hesabı yönetim işlevleri, çeşitli erişim sağlar. Bunları Ekle veri gölü içindeki veri etkileşim kuracağı kullanıcılar ** okuyucu ** rol. Bu rolleri kapsamını Azure Data Lake Store hesabına ilgili yönetim işlemleri sınırlıdır.
   
    Kullanıcıların ne işlemlerini tek tek dosya sistemi izinleri verileri tanımlamak. Bu nedenle, okuyucu rolüne sahip bir kullanıcı hesabıyla ilişkilendirilmiş yönetim ayarlarını yalnızca görüntüleme ancak potansiyel olarak okuyabilir ve dosya sistemi izinleri atanmış göre veri yazma. Data Lake Store dosya sistemi izinleri adresindeki açıklanmıştır [Ata güvenlik grubuna ACL'ler olarak Azure Data Lake Store dosya sistemi](#filepermissions).
5. İçinde **erişim Ekle** dikey penceresinde tıklatın **kullanıcıları eklemek** açmak için **kullanıcıları eklemek** dikey. Bu dikey pencerede, Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunun arayın. Gelen arama gruplarının çok varsa, metin kutusunun en üstünde grup adına filtrelemek için kullanın. **Seç**'e tıklayın.
   
    ![Güvenlik Grubu Ekle](./media/data-lake-store-secure-data/adl.add.user.2.png "güvenlik grubu Ekle")
   
    Listede olmayan bir grup/kullanıcı eklemek istiyorsanız, bunları kullanarak davet edebilirsiniz **davet** simgesi ve kullanıcı/grup için e-posta adresi belirtme.
6. **Tamam** düğmesine tıklayın. Aşağıda gösterildiği gibi eklenen güvenlik grubu görmeniz gerekir.
   
    ![Güvenlik grubuna eklenen](./media/data-lake-store-secure-data/adl.add.user.3.png "güvenlik grubuna eklendi")

7. Kullanıcı/güvenlik grubunun artık Azure Data Lake Store hesabına erişim izni vardır. Belirli kullanıcılara erişim sağlamak istiyorsanız, bunları güvenlik grubuna ekleyebilirsiniz. Benzer şekilde, bir kullanıcının erişimi iptal etmek istiyorsanız, bunları güvenlik grubundan kaldırabilirsiniz. Ayrıca, birden çok güvenlik grubu için bir hesap atayabilirsiniz. 

## <a name="filepermissions"></a>Kullanıcıların veya güvenlik grubu ACL'ler Azure Data Lake Store dosya sistemine atayın.
Azure Data Lake dosya sistemine kullanıcı/güvenlik gruplarına atayarak Azure Data Lake Store içinde depolanan veriler üzerinde erişim denetimi ayarlayın.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake Store hesabında dizin oluşturmak](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Data Lake hesabında dizinler oluşturma")
2. İçinde **Veri Gezgini** dikey penceresinde, dosya veya ACL yapılandırmak ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosyaya ACL atamak için tıklatmalısınız **erişim** gelen **Dosya Önizleme** dikey.
   
    ![Data Lake dosya sisteminde ACL'leri ayarlamak](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi üzerindeki ACL'lerin ayarlayın")
3. **Erişim** standart erişim ve kök atanmış özel erişim dikey penceresinde listelenir. Tıklatın **Ekle** Özel düzey ACL eklemek için simge.
   
    ![Liste standart ve özel erişim](./media/data-lake-store-secure-data/adl.acl.2.png "listesinde standart ve özel erişim")
   
   * **Standart erişim** UNIX stili erişim burada belirttiğiniz okuma, yazma, üç ayrı kullanıcı sınıfları (rwx) yürütme: sahibi, Grup ve diğerleri.
   * **Özel erişim** belirli adlandırılmış kullanıcıları veya grupları ve yalnızca dosyanın sahibi veya grup için izinleri ayarlamanızı sağlar POSIX ACL'leri karşılık gelir. 
     
     Daha fazla bilgi için bkz: [HDFS ACL'leri](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). ACL'ler Data Lake Store içinde nasıl uygulandığı hakkında daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](data-lake-store-access-control.md).
4. Tıklatın **Ekle** açmak için simgesini **eklemek özel erişim** dikey. Bu dikey pencerede tıklatın **kullanıcı veya Grup Seç**ve ardından **kullanıcı veya Grup Seç** dikey penceresinde, Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunun arayın. Gelen arama gruplarının çok varsa, metin kutusunun en üstünde grup adına filtrelemek için kullanın. Ekleyin ve ardından istediğiniz Grup tıklatın **seçin**.
   
    ![Grup ekleme](./media/data-lake-store-secure-data/adl.acl.3.png "grup ekleme")
5. Tıklatın **Select izinleri**, izinleri seçin ve varsayılan olarak ACL izinleri atamak istediğiniz olup olmadığını ACL ya da her ikisini de erişim. **Tamam** düğmesine tıklayın.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.4.png "gruplandırmak için izinler atama")
   
    Data Lake Store ve varsayılan/erişim ACL izinleri hakkında daha fazla bilgi için bkz: [Data Lake Store'da erişim denetimi](data-lake-store-access-control.md).
6. İçinde **eklemek özel erişim** dikey penceresinde tıklatın **Tamam**. Yeni eklenen grupla ilişkili izinlere şimdi listelenir **erişim** dikey.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.5.png "gruplandırmak için izinler atama")
   
   > [!IMPORTANT]
   > Geçerli sürümde altındaki 9 girişler yalnızca olabilir **özel erişim**. Birden fazla 9 kullanıcıları eklemek istiyorsanız, oluşturduğunuz güvenlik grupları, kullanıcı güvenlik gruplarına ekleme, eklemek için bu güvenlik grupları Data Lake Store hesabı için erişim sağlamak.
   > 
   > 
7. Grup ekledikten sonra gerekirse, erişim izinlerini değiştirebilirsiniz. Kaldırın veya bu izin güvenlik grubuna atamak isteyip istemediğinizi üzerinde temel her izin türü (okuma, yazma, yürütme) onay kutusunu seçin veya temizleyin. Tıklatın **kaydetmek** değişiklikleri kaydetmek için veya **atmak** değişiklikleri geri almak için.

## <a name="set-ip-address-range-for-data-access"></a>Veri erişimi için IP adres aralığını ayarlayın
Azure Data Lake Store, başka kilitleme veri deponuza ağ düzeyinde erişim sağlar. Güvenlik duvarını etkinleştir, bir IP adresi belirtin veya güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Etkinleştirildikten sonra IP adreslerini tanımlı aralığın içinde olan istemciler deposuna bağlanabilirsiniz.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-secure-data/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı için güvenlik grupları kaldırın
Azure Data Lake Store hesaplarını güvenlik grupları kaldırdığınızda, Azure portalı ve Azure Resource Manager API'leri kullanarak hesap yönetimi işlemleri için yalnızca değiştiriyorsunuz.

1. Data Lake Store hesabı dikey penceresinde tıklayın **ayarları**. Gelen **ayarları** dikey penceresinde tıklatın **kullanıcılar**.
   
    ![Azure Data Lake hesabına güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake hesabına Ata güvenlik grubu")
2. İçinde **kullanıcılar** dikey penceresinde, kaldırmak istediğiniz güvenlik grubunu'ı tıklatın.
   
    ![Güvenlik grubu kaldırmak için](./media/data-lake-store-secure-data/adl.add.user.3.png "kaldırmak için güvenlik grubu")
3. Güvenlik grubu için dikey penceresinde tıklayın **kaldırmak**.
   
    ![Güvenlik grubu kaldırıldı](./media/data-lake-store-secure-data/adl.remove.group.png "güvenlik grubu kaldırıldı")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Güvenlik grubu ACL'ler Azure Data Lake Store dosya sisteminden kaldırın
Güvenlik grupları ACL'ler Azure Data Lake Store dosya sisteminden kaldırdığınızda, Data Lake Store'da verilere erişimi değiştirme.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake hesabında dizin oluşturmak](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Data Lake hesabında dizinler oluşturma")
2. İçinde **Veri Gezgini** dikey penceresinde, dosya veya klasör ACL kaldırmak istediğiniz ve ardından hesap dikey penceresinde tıklatın, **erişim** simgesi. Bir dosya için ACL kaldırmak için tıklatmalısınız **erişim** gelen **Dosya Önizleme** dikey.
   
    ![Data Lake dosya sisteminde ACL'leri ayarlamak](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi üzerindeki ACL'lerin ayarlayın")
3. İçinde **erişim** dikey penceresinde, gelen **özel erişim** bölümünde, kaldırmak istediğiniz güvenlik grubunu tıklatın. İçinde **özel erişim** dikey penceresinde tıklatın **kaldırmak** ve ardından **Tamam**.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.remove.acl.png "gruplandırmak için izinler atama")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-powershell.md)
* [.NET SDK'yı kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store'a ilişkin tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md)

