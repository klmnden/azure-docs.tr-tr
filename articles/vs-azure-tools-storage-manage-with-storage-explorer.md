---
title: "Depolama Gezgini (Önizleme) ile çalışmaya başlama | Microsoft Docs"
description: "Depolama Gezgini (Önizleme) ile Azure Storage kaynaklarını yönetme"
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: 27b3775d81ec6dc093dae4ee46167c5d5a9c9e19
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-with-storage-explorer-preview"></a>Depolama Gezgini (Önizleme) ile çalışmaya başlama
## <a name="overview"></a>Genel Bakış
Azure Depolama Gezgini (Önizleme) Windows, macOS ve Linux’ta Azure Depolama ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Bu makalede, bağlanma ve Azure storage hesaplarınızı yönetmeye çeşitli yollarını öğrenin.

![Microsoft Azure Depolama Gezgini (Önizleme)][0]

## <a name="prerequisites"></a>Önkoşullar
* [Depolama Gezgini (Önizleme) indirip yükleme](http://www.storageexplorer.com)

> [!NOTE]
> Ubuntu 16.04 dışında Linux distro'lar için bazı bağımlılıklar el ile yüklemeniz gerekebilir. Genel olarak, aşağıdaki paketler gereklidir:
> * libgconf-2-4
> * libsecret
> * Güncel GCC
>
> Distro bağlı olarak yüklemek için gereken diğer paket olabilir. Depolama Gezgini [sürüm notları](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) bazı distro'lar için belirli adımlar içerir.
>
>

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak
Depolama Gezgini (Önizleme) depolama hesaplarına bağlamak için birçok yol sağlar. Örneğin, şunları yapabilirsiniz:
* Azure aboneliğinizle ilişkili depolama hesaplarına bağlanın.
* Diğer Azure aboneliklerinden paylaşılan depolama hesaplarına ve hizmetlere bağlanın.
* Azure Depolama Öykünücüsü kullanarak yerel depolama alanınıza bağlanın ve yönetin. 

Ayrıca global ve ulusal Azure'daki depolama hesaplarıyla çalışabilirsiniz:

* [Bir Azure aboneliğine bağlanma](#connect-to-an-azure-subscription): Azure aboneliğinize ait depolama kaynaklarını yönetin.
* [Yerel geliştirme deposu ile çalışma](#work-with-local-development-storage): Azure Depolama Öykünücüsü kullanarak yerel depolamayı yönetin.
* [Dış depolama birimine ekleme](#attach-or-detach-an-external-storage-account): Depolama hesabının adını, anahtarını ve uç noktalarını kullanarak başka bir Azure aboneliğine ait olan veya ulusal Azure bulutlarında bulunan depolama kaynaklarını yönetin.
* [SAS kullanarak depolama hesabı ekleme](#attach-storage-account-using-sas): Paylaşılan erişim imzası (SAS) kullanarak başka bir Azure aboneliğine ait olan depolama kaynaklarını yönetin.
* [SAS kullanarak hizmet ekleme](#attach-service-using-sas): SAS kullanarak başka bir Azure aboneliğine ait belirli bir depolama hizmetini (blob kapsayıcısı, kuyruk veya tablo) yönetin.
* [Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): bir bağlantı dizesi kullanarak yönetmek Cosmos DB hesabı.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma
> [!NOTE]
> Bir Azure hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Depolama Gezgini (Önizleme), seçin **hesaplarını yönetme** gitmek için **hesabı yönetim paneli**.

    ![Hesapları yönetme][1]

2. Sol bölmede, artık oturum açtığınız tüm Azure hesapları gösterilir. Başka bir hesapla bağlanmak için **Hesap Ekle**

3. Ulusal Bulut veya bir Azure yığın oturum istiyorsanız, tıklayın **Azure ortamı** kullanmak istediğiniz hangi Azure bulut seçmek için açılır. Ortamınızı seçtikten sonra tıklatın **oturum aç...**  düğmesi. Azure yığınına imzalama olup [Depolama Gezgini Azure yığın abonelik](azure-stack/user/azure-stack-storage-connect-se.md) daha fazla bilgi için.

    ![Seçeneğinde oturum][2]

3. Başarıyla bir Azure hesabı ile oturum sonra hesap ve bu hesapla ilişkili Azure abonelikleri sol bölmeye eklenir. Çalışmak ve ardından istediğiniz Azure aboneliklerini seçin **Uygula** (seçme **tüm abonelikleri:** veya listelenen Azure aboneliklerinin hiçbirinin seçerek geçiş yapar).

    ![Azure aboneliklerini seçme][3]

    Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

## <a name="work-with-local-development-storage"></a>Yerel geliştirme deposu ile çalışma
Depolama Gezgini (Önizleme) ile Azure Depolama Öykünücüsü kullanarak yerel depolamaya karşı çalışabilirsiniz. Bu yaklaşım, depolama hesabı Azure Storage öykünücüsü tarafından öykündüğü için mutlaka Azure üzerinde dağıtılan bir depolama hesabı gerek kalmadan Azure Storage ile çalışma benzetimini olanak tanır.

> [!NOTE]
> Azure Storage Öykünücüsü şu anda yalnızca Windows için desteklenmektedir.
>
>

> [!NOTE]
> Azure Storage öykünücüsü dosya paylaşımlarını desteklemez.
>
>

1. Depolama Gezgini (Önizleme) sol bölmesinde **(yerel ve iliştirildiği)** > **depolama hesapları** > **(Geliştirme)**  >  **Blob kapsayıcıları** düğümü.

    ![Yerel geliştirme düğümü][5]

2. Azure Storage öykünücüsünü henüz yüklü değilse, bunu bir bilgi çubuğu yapmak istenir. Bilgi çubuğu görüntülendiyse **En yeni sürümü indir**’i seçin ve öykünücüyü yükleyin.

    ![Azure Storage Öykünücüsünü İndirme istemi][6]

3. Öykünücü yüklendikten sonra yerel bloblar, kuyruklar ve tablolar oluşturup bunlarla çalışabilirsiniz. Her Depolama hesabı türü ile çalışmayı öğrenmek için aşağıdaki kılavuzlara bakın:

    * [Azure blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
    * Azure dosya paylaşımı depolama kaynaklarını yönetme: *Çok yakında*
    * Azure kuyruk depolama kaynaklarını yönetme: *Çok yakında*
    * Azure Tablo Depolama kaynaklarını yönetme - *Çok yakında*

## <a name="attach-or-detach-an-external-storage-account"></a>Harici bir depolama hesabı ekleme veya ayırma
Depolama Gezgini (Önizleme) ile depolama hesaplarının kolayca paylaşılabilmesi için dış depolama hesaplarına ekleyebilirsiniz. Bu bölümde harici depolama hesaplarının nasıl ekleneceği (ve ayrılacağı) açıklanmaktadır.

### <a name="get-the-storage-account-credentials"></a>Depolama hesabının kimlik bilgilerini alma
Harici bir depolama hesabı paylaşmak için ilgili hesabın sahibi ilk (hesap adı ve anahtar) kimlik bilgilerini hesap için almak ve bilgi eklemek isteyen kişiyle hesabı belirtti paylaşın. Aşağıdakileri yaparak Azure portalı üzerinden depolama hesabı kimlik bilgileri elde edebilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Gözat**’ı seçin.

3. **Depolama Hesapları**’nı seçin.

4. Listesinde **depolama hesapları**, istenen depolama hesabını seçin.

5. Altında **ayarları**seçin **erişim anahtarları**.

    ![Erişim Anahtarları seçeneği][7]

6. Kopya **depolama hesabı adı** ve **key1**.

    ![Erişim tuşları][8]

### <a name="attach-to-an-external-storage-account"></a>Harici bir depolama hesabı ekleme
Bir dış depolama hesabına bağlanmak için hesabın adı ve anahtarı gereklidir. “Depolama hesabı kimlik bilgilerini alma” bölümünde bu değerlerin Azure portalından nasıl alınacağı açıklanmaktadır. Ancak, portalda hesap anahtarı **key1** olarak adlandırılır. Bu nedenle, Depolama Gezgini (Önizleme) için bir hesap anahtarı istediğinde, girdiğiniz **key1** değeri.

1. Depolama Gezgini (Önizleme), açık **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim**, seçin **bir depolama hesabı adı ve anahtar kullanın**

    ![Ad ve anahtar seçeneği ile Ekle][10]

3. Hesap adınızı yapıştırın **hesap adı** metin kutusuna ve hesap anahtarınızın yapıştırın ( **key1** değeri Azure portalından) içine **hesap anahtarı** metin kutusuna ve ardından seçin **Sonraki**.

    ![Ad ve anahtar sayfası][11]

    > [!NOTE]
    > Bir ad ve anahtar Ulusal buluttan kullanmak için **depolama uç noktaları etki alanı:** uygun uç noktaları etki alanını seçmek için açılır: 
    >
    >

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. 

5. **Bağlan**’ı seçin.

6. Depolama hesabı başarıyla eklendi sonra depolama hesabı ile görüntülenir **(harici)** adının eklenir.

    ![Harici depolama hesabına bağlanma sonucu][12]

### <a name="detach-from-an-external-storage-account"></a>Harici bir depolama hesabı ayırma
1. Ayırmak istediğiniz dış depolama hesabına sağ tıklayın ve **Ayır**’ı seçin.

    ![Depolama alanından ayırma seçeneği][13]

2. Onay iletisinde, dış depolama hesabından ayırmayı onaylamak üzere **Evet**’i seçin.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) kullanarak bir depolama hesabı ekleme
Paylaşılan erişim imzası, a veya [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), geçici bir depolama hesabı için Azure aboneliği kimlik bilgilerini sağlamasına gerek kalmadan erişim yönetici bir Azure aboneliğinin olanak sağlar.

Bu senaryoyu göstermek üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istediğini varsayalım:

1. Kullanıcıa SAS bağlantı dizesi, belirli bir zaman dilimi ve istenilen izinlerle oluşturur.

2. Kullanıcıa SAS depolama hesabı erişim isteyen kişiyle (Bu örnekte, UserB) paylaşır.  

3. KullanıcıB, sağlanan SAS’ı kullanarak KullanıcıA’ya ait hesabı eklemek için Depolama Gezgini’ni (Önizleme) kullanır.

### <a name="generate-a-sas-connection-string-for-the-account-you-want-to-share"></a>Paylaşmak istediğiniz hesap için bir SAS bağlantı dizesi oluştur
1. Depolama Gezgini (Önizleme), paylaşımı ve ardından istediğiniz depolama hesabını sağ tıklatın **paylaşılan erişim imzası Al...** .

    ![SAS alma içerik menüsü seçeneği][14]

2. İçinde **paylaşılan erişim imzası oluşturmak** iletişim kutusunda, hesap için istediğiniz ve ardından izinleri ve zaman çerçevesi belirtin **oluşturma** düğmesi.

    ![Al SAS iletişim kutusu][15]  

3. Yanına **bağlantı dizesi** metin kutusunda **kopya** panonuza kopyalayın ve ardından **Kapat**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Bir SAS bağlantı dizesi kullanarak bir depolama hesabı ekleme
1. Depolama Gezgini (Önizleme), açık **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim** iletişim kutusunda, seçin **bir bağlantı dizesi veya paylaşılan erişim imzası URI kullanmak** ve ardından **sonraki**.

    ![Azure depolamaya bağlan iletişim kutusu][16]

3. Seçin **bir bağlantı dizesi kullanmak** bağlantı dizenizi Yapıştır **bağlantı dizesi:** alan. Tıklatın **sonraki** düğmesi.

    ![Azure depolamaya bağlan iletişim kutusu][17]

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin. 

5. **Bağlan**’ı seçin.

6. Depolama hesabı başarıyla eklendi sonra depolama hesabı ile görüntülenir **(SAS)** adının eklenir.

    ![SAS kullanarak hesaba ekleme sonucu][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) kullanarak bir hizmet ekleme
"Bir SAS kullanarak bir depolama hesabı ekleme" bölümünde, nasıl Azure abonelik yöneticisinin geçici bir depolama hesabı oluşturma ve depolama hesabı için bir SAS paylaşarak erişim sağlayabilirsiniz açıklanmaktadır. Benzer şekilde, belirli bir hizmet için (blob kapsayıcısı, kuyruk, tablo veya dosya paylaşımı) bir depolama hesabındaki bir SAS oluşturulabilir.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Paylaşmak istediğiniz hizmet için SAS oluşturma
Bu bağlamda bir hizmeti bir blob kapsayıcısı, kuyruk, tablo olabilir veya dosya paylaşımını. Listelenen bir hizmet için SAS oluşturmak istiyorsanız bkz.:

* [Blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Dosya paylaşımı için SAS alma: *Çok yakında*
* Bir kuyruk için SAS alma: *Çok yakında*
* Bir tablo için SAS alma: *Çok yakında*

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>SAS URI'sini kullanarak paylaşılan hesap hizmetine ekleme
1. Depolama Gezgini (Önizleme), açık **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim** iletişim kutusunda, seçin **bir bağlantı dizesi veya paylaşılan erişim imzası URI kullanmak** ve ardından **sonraki**.

    ![Azure depolamaya bağlan iletişim kutusu][16]

3. Seçin **SAS URI'sini kullanmak** , URI Yapıştır **URI:** alan. Tıklatın **sonraki** düğmesi.

    ![Azure depolamaya bağlan iletişim kutusu][19]

3. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin. 

4. **Bağlan**’ı seçin.

5. Hizmet başarıyla bağlandıktan sonra hizmet altında görüntülenen **(SAS-Attached Hizmetleri)** düğümü.

    ![SAS kullanarak paylaşılan hizmete ekleme sonucu][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma
Bir bağlantı dizesi kullanmak için bir Azure Cosmos DB bağlayan alternatif bir yolu, yanında Azure aboneliği ile Azure Cosmos DB hesaplarını yönetin. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Azure Cosmos DB Hesapları**’na sağ tıklayıp **Azure Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    ![Bağlantı dizesi tarafından Azure Cosmos Veritabanına bağlanın][21]

2. Azure Cosmos DB API'ı seçin, yapıştırın, **bağlantı dizesi**ve ardından **Tamam** Azure Cosmos DB hesap bağlanmak için. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![bağlantı dizesi][22]

 ## <a name="connect-to-azure-data-lake-store-by-uri"></a>Azure Data Lake Store URI tarafından bağlanın
Aboneliğinizde olmayan kaynaklara erişmek istiyorsanız. Ancak diğer kullanıcılar kaynaklar için URI’yi alma izni veriyorsa. Bu durumda, oturum açtıktan sonra URI’yi kullanarak Data Lake Store’a bağlanabilirsiniz. Aşağıdaki adımlara bakın.
1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**’a sağ tıklayıp bağlam menüsünden **Data Lake Store’a bağlan...** seçeneğine tıklayın.

    ![Data Lake Store’a bağlanma bağlam menüsü](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Siz URI’yi girdikten sonra araç girdiğiniz URL’nin konumuna gider.

    ![Data Lake Store’a bağlanma bağlam iletişimi](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Data Lake Store’a bağlanma sonucu](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama
Depolama kaynağı bulmanız gerekiyorsa ve olduğu bilmiyorsanız, kaynağı aramak için sol bölmenin en üstünde arama kutusunu kullanabilirsiniz.

Arama kutusuna yazarken sol bölmede, bu noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynakları görüntüler. Örneğin, bir Ara **uç noktaları** aşağıdaki ekran görüntüsünde gösterilen:

![Depolama hesabı araması][23]

> [!NOTE]
> Kullanım **hesabı yönetim paneli** aramanızı yürütme süresini artırmak için aradığınız öğe içermeyen herhangi bir aboneliğiniz seçimini kaldırmak için. Ayrıca bir düğümüne sağ tıklayın ve seçin **arama gelen burada** belirli bir düğümden aramaya başlamak için.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini (Önizleme) ile Azure Blob Depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Depolama Gezgini (Önizleme) Azure Cosmos DB yönetme](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini (Önizleme) ile Azure Data Lake Store kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/DevelopmentNode.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/EmulatorNotInstalled.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/PortalAccessKeys.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccessKeys.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-AddWithKeySelected.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-NameAndKeyPage.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithKeyAccount-Detach.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/GetSharedAccessSignature.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SharedAccessSignatureDialog.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-WithConnStringOrSASSelected.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-1.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AttachedWithSASAccount.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-ConnStringOrSASPage-2.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ServiceAttachedWithSAS.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Search.png
