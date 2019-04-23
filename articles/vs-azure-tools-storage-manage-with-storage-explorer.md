---
title: Depolama Gezgini ile çalışmaya başlama | Microsoft Docs
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetme
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2017
ms.author: cawa
ms.openlocfilehash: 38a857b1d309b92c48137a46655155e0e131908c
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60002689"
---
# <a name="get-started-with-storage-explorer"></a>Depolama Gezgini ile çalışmaya başlama

## <a name="overview"></a>Genel Bakış

Azure Depolama Gezgini Windows, macOS ve Linux'ta Azure depolama verileriyle kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Bu makalede, bağlama ve Azure depolama hesaplarınızı yönetme çeşitli yollarını öğrenin.

![Microsoft Azure Depolama Gezgini][0]

## <a name="prerequisites"></a>Önkoşullar

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Azure Depolama Gezgini, aşağıdaki Windows sürümlerinde desteklenir:

* Windows 10 (önerilir)
* Windows 8
* Windows 7

Windows, .NET Framework 4.6.2 tüm sürümleri için veya daha yükseği gerekli.

[Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Azure Depolama Gezgini, macOS üzerinde aşağıdaki sürümleri desteklenir:

* macOS 10.12 "Sierra" ve sonraki sürümler

[Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Azure Depolama Gezgini, Linux üzerinde aşağıdaki dağıtımlar desteklenir:

* Ubuntu 18.04 x64
* Ubuntu 16.04 x64
* Ubuntu 14.04 x64

Azure Depolama Gezgini diğer dağıtımlarında çalışabilir fakat yalnızca bunlardır yukarıda listelenen resmi olarak desteklenir.

Depolama Gezgini'ni yükleme Linux üzerinde daha fazla yardım için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/en-us/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

Azure Depolama Gezgini [sürüm notları](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) bazı dağıtımlar için belirli adımları içerir.

[Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Örneğin, şunları yapabilirsiniz:

* Azure aboneliğinizle ilişkili depolama hesaplarına bağlanın.
* Diğer Azure aboneliklerinden paylaşılan depolama hesaplarına ve hizmetlere bağlanın.
* Azure Depolama Öykünücüsü kullanarak yerel depolama alanınıza bağlanın ve yönetin.

Ayrıca global ve ulusal Azure'daki depolama hesaplarıyla çalışabilirsiniz:

* [Bir Azure aboneliğine bağlanma](#connect-to-an-azure-subscription): Azure aboneliğinize ait depolama kaynaklarını yönetin.
* [Yerel geliştirme deposu ile çalışma](#work-with-local-development-storage): Azure Storage öykünücüsü kullanarak yerel depolamayı yönetin.
* [Dış depolama birimine ekleme](#attach-or-detach-an-external-storage-account): Başka bir Azure aboneliğine ait olan veya Ulusal Azure bulutlarında bulunan depolama hesabının adını, anahtarını ve uç noktaları kullanarak olan depolama kaynaklarını yönetin.
* [SAS kullanarak depolama hesabı ekleme](#attach-a-storage-account-by-using-a-shared-access-signature-sas): Paylaşılan erişim imzası (SAS) kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetin.
* [SAS kullanarak hizmet ekleme](#attach-a-service-by-using-a-shared-access-signature-sas): SAS kullanarak başka bir Azure aboneliğine ait belirli bir depolama hizmetini (blob kapsayıcısı, kuyruk veya tablo) yönetin.
* [Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): Cosmos DB hesabı, bir bağlantı dizesi kullanarak yönetin.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma

> [!NOTE]
> Bir Azure hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Depolama Gezgini'nde seçin **hesaplarını yönetme** gitmek için **hesabı yönetim panelinde**.

    ![Hesapları Yönet][1]

2. Sol bölmede, artık için açtığınız tüm Azure hesaplarınıza görüntüler. Başka bir hesaba bağlanmak için **Hesap Ekle**

3. Ulusal bulut ya da bir Azure Stack oturum açmak istiyorsanız, tıklayarak **Azure ortamı** kullanmak istediğiniz hangi Azure bulut seçmek için açılır. Ortamınızı seçtikten sonra tıklayın **oturum açın...**  düğmesi. Azure Stack için imzalama olup [Depolama Gezgini'ni Azure Stack aboneliğine bağlanma](/azure-stack/user/azure-stack-storage-connect-se) daha fazla bilgi için.

    ![Seçeneğinde oturum][2]

4. Bir Azure hesabıyla oturum başarıyla oturum sonra hesabı ve bu hesapla ilişkili Azure abonelikleriyle sol bölmeye eklenir. Çalışmak ve sonra istediğiniz Azure aboneliklerini seçin **Uygula** (seçme **abonelikler:** veya listelenen Azure aboneliklerinin hiçbirinin seçilmemesini).

    ![Azure aboneliklerini seçme][3]

    Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

## <a name="work-with-local-development-storage"></a>Yerel geliştirme deposu ile çalışma

Depolama Gezgini ile bir öykünücüsü kullanarak yerel depolama ile çalışabilirsiniz. Bu yaklaşım, Azure'da dağıtılan bir depolama hesabı olmak zorunda kalmadan Azure Storage ile çalışma benzetimini sağlar.

Yerel depolama öykünücüsünde 1.1.0 sürümü ile başlayarak, tüm platformlarda desteklenir. Depolama Gezgini, kendi varsayılan yerel depolama uç noktaları için dinleme herhangi bir Öykünmüş hizmetine bağlanabilirsiniz.

> [!NOTE]
> Depolama Hizmetleri ve özellikleri için destek, yaygın olarak öykünücü ettiğiniz bağlı olarak değişiklik gösterebilir. Uygulamanızı öykünücü ile çalışmak için istediğinize özellikleri ve Hizmetleri desteklediğinden emin olun.

1. Uygulamanızı öykünücü kullanılmayan bir bağlantı noktasını dinlemek üzere tercih ettiğiniz hizmetlerini yapılandırın.

   Benzetilmiş hizmeti | Varsayılan uç nokta
   -----------------|-------------------------
   Bloblar            | `http://127.0.0.1:10000`
   Kuyruklar           | `http://127.0.0.1:10001`
   Tablolar           | `http://127.0.0.1:10002`

2. Öykünücüyü başlatın.
   > [!IMPORTANT]
   > Depolama Gezgini, öykünücüsü otomatik olarak başlamaz. Kendiniz başlatmanız gerekir.

3. Depolama Gezgini'nde tıklayın **hesabı Ekle** düğmesi. Seçin **eklemek için yerel bir öykünücü** tıklatıp **sonraki**.

4. (Bu hizmeti kullanmayı düşünmüyorsanız, boş bırakın) yapılandırılmış bir hizmeti için bağlantı noktası numaraları girin. Tıklayın **sonraki** ardından **Connect** bağlantı oluşturmak için.

5. Genişletin **yerel ve ekli** > **depolama hesapları** > düğümleri öykünücü bağlantınızı karşılık gelen düğümü altındaki hizmet düğümleri genişletin.

   Bu düğüm, oluşturmak ve yerel bloblar, kuyruklar ve tablolar ile çalışmak için kullanabilirsiniz. Her Depolama hesabı türü ile çalışma hakkında bilgi almak için aşağıdaki kılavuzlara bakın:

   * [Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
   * [Azure dosya depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-files.md)

## <a name="attach-or-detach-an-external-storage-account"></a>Harici bir depolama hesabı ekleme veya ayırma

Depolama Gezgini ile depolama hesaplarında bir kolayca paylaşılabilir böylece dış depolama hesaplarına ekleyebilirsiniz. Bu bölümde harici depolama hesaplarının nasıl ekleneceği (ve ayrılacağı) açıklanmaktadır.

### <a name="get-the-storage-account-credentials"></a>Depolama hesabının kimlik bilgilerini alma

Harici bir depolama hesabını paylaşmak için o hesabın sahibi ilk kimlik bilgilerini (hesap adı ve anahtarı) almalıdır ve sonra bilgi eklemek isteyen kişiyle hesabı belirtti paylaşın. Azure portal aracılığıyla depolama hesabı kimlik bilgileri aşağıdaki adımları uygulayarak elde edebilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Gözat**’ı seçin.

3. **Depolama Hesapları**’nı seçin.

4. Listesinde **depolama hesapları**, istenen depolama hesabını seçin.

5. **Ayarlar** altında **Erişim anahtarları**'nı seçin.

    ![Erişim Anahtarları seçeneği][7]

6. Kopyalama **depolama hesabı adı** ve **key1**.

    ![Erişim tuşları][8]

### <a name="attach-to-an-external-storage-account"></a>Harici bir depolama hesabı ekleme

Bir dış depolama hesabına bağlanmak için hesabın adı ve anahtarı gereklidir. “Depolama hesabı kimlik bilgilerini alma” bölümünde bu değerlerin Azure portalından nasıl alınacağı açıklanmaktadır. Ancak, portalda hesap anahtarı **key1** olarak adlandırılır. Bu nedenle, Depolama Gezgini, bir hesap anahtarı istediğinde, girdiğiniz **key1** değeri.

1. Depolama Gezgini'nde açın **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim**, seçin **bir depolama hesabı adı ve anahtarı kullan**

    ![Adı ve anahtarı seçeneği ile ekleme][10]

3. Hesap adınızı yapıştırın **hesap adı** metin kutusu ve hesap anahtarını yapıştırın ( **key1** Azure portalından değeri) içine **hesap anahtarı** metin kutusuna tıklayın ve ardından **Sonraki**.

    ![Ad ve anahtar sayfa][11]

    > [!NOTE]
    > Ulusal buluttan bir adı ve anahtarı kullanmak için **depolama uç noktaları etki alanı:** uygun uç noktaları etki alanı seçmek için açılır:
    >
    >

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin.

5. **Bağlan**’ı seçin.

6. Depolama hesabı başarıyla eklendikten sonra depolama hesabı ile görüntülenir **(Dış)** adının.

    ![Harici depolama hesabına bağlanma sonucu][12]

### <a name="detach-from-an-external-storage-account"></a>Harici bir depolama hesabı ayırma

1. Ayırmak istediğiniz dış depolama hesabına sağ tıklayın ve **Ayır**’ı seçin.

    ![Depolama alanından ayırma seçeneği][13]

2. Onay iletisinde, dış depolama hesabından ayırmayı onaylamak üzere **Evet**’i seçin.

## <a name="attach-a-storage-account-by-using-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) kullanarak bir depolama hesabı ekleme

Bir paylaşılan erişim imzası veya [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md), yönetici bir Azure aboneliğinin Azure aboneliği kimlik bilgilerini vermek zorunda kalmadan bir depolama hesabına geçici erişim izni sağlar.

Bu senaryoyu göstermek üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istediğini varsayalım:

1. Kullanıcıa belirli bir zaman dilimi ve istenilen izinlerle bir SAS bağlantı dizesi oluşturur.

2. Kullanıcıa SAS depolama hesabına erişim isteyen kişiyle (Bu örnekte Kullanıcıb) paylaşıyor.

3. Kullanıcıb, sağlanan SAS'ı kullanarak kullanıcıa'ya ait hesabı eklemek için Depolama Gezgini'ni kullanır.

### <a name="generate-a-sas-query-string-for-the-account-you-want-to-share"></a>Paylaşmak istediğiniz hesap için bir SAS sorgu dizesini oluştur

1. Depolama Gezgini'nde paylaşın ve ardından istediğiniz depolama hesabına sağ tıklayın **paylaşılan erişim imzası Al...** .

    ![SAS alma içerik menüsü seçeneği][14]

2. İçinde **paylaşılan erişim imzası oluşturma** iletişim kutusunda, bir zaman dilimi ve hesabınız için istediğiniz ve ardından izinleri belirtin **Oluştur** düğmesi.

    ![SAS alma iletişim kutusu][15]

3. Yanındaki **sorgu dizesi** metin kutusunda **kopyalama** panonuza kopyalayın ve ardından **Kapat**.

### <a name="attach-to-a-storage-account-by-using-a-sas-connection-string"></a>Depolama hesabı için bir SAS bağlantı dizesi kullanarak ekleme

1. Depolama Gezgini'nde açın **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim** iletişim kutusunda seçin **bağlantı dizesi veya paylaşılan erişim imzası URI'si kullanma** ve ardından **sonraki**.

    ![Azure depolamaya bağlan iletişim kutusu][16]

3. Seçin **bağlantı dizesini kullanmak** bağlantı dizenizi Yapıştır **bağlantı dizesi:** alan. Tıklayın **sonraki** düğmesi.

    ![Azure depolamaya bağlan iletişim kutusu][17]

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin.

5. **Bağlan**’ı seçin.

6. Depolama hesabı başarıyla eklendikten sonra depolama hesabı ile görüntülenir **(SAS)** adının.

    ![SAS kullanarak hesaba ekleme sonucu][18]

## <a name="attach-a-service-by-using-a-shared-access-signature-sas"></a>Paylaşılan erişim imzası (SAS) kullanarak bir hizmet ekleme

"SAS kullanarak depolama hesabı ekleme" bölümünde, Azure abonelik yöneticisinin oluşturma ve depolama hesabı için bir SAS paylaşarak nasıl geçici bir depolama hesabı erişim verebilirsiniz açıklanmaktadır. Benzer şekilde, belirli bir hizmet için (blob kapsayıcısı, kuyruk, tablo veya dosya paylaşımı) bir depolama hesabında bir SAS oluşturulabilir.

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Paylaşmak istediğiniz hizmet için SAS oluşturma

Bu bağlamda bir hizmet olması bir blob kapsayıcısı, kuyruk, tablo veya dosya paylaşımı. Listelenen bir hizmet için SAS oluşturmak istiyorsanız bkz.:

* [Blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

### <a name="attach-to-the-shared-account-service-by-using-a-sas-uri"></a>SAS URI'sini kullanarak paylaşılan hesap hizmetine ekleme

1. Depolama Gezgini'nde açın **Bağlan iletişim**.

    ![Azure Storage’a bağlanma seçeneği][9]

2. İçinde **Bağlan iletişim** iletişim kutusunda **bağlantı dizesi veya paylaşılan erişim imzası URI'si kullanma** ve ardından **sonraki**.

    ![Azure depolamaya bağlan iletişim kutusu][16]

3. Seçin **SAS URI'si kullanma** URI'niz içine yapıştırın **URI:** alan. Tıklayın **sonraki** düğmesi.

    ![Azure depolamaya bağlan iletişim kutusu][19]

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin.

5. **Bağlan**’ı seçin.

6. Hizmet başarıyla eklendikten sonra hizmetin altında görüntülenen **(SAS-Attached Hizmetleri)** düğümü.

    ![SAS kullanarak paylaşılan hizmete ekleme sonucu][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma

Seçeneklerinin yanı sıra Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetebilir, bir Azure Cosmos DB'ye bağlanmanın alternatif bir yolu, bir bağlantı dizesi kullanmaktır. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Azure Cosmos DB Hesapları**’na sağ tıklayıp **Azure Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    ![bağlantı dizesiyle Azure Cosmos DB'ye bağlanma][21]

2. Azure Cosmos DB API seçin, yapıştırma, **bağlantı dizesi**ve ardından **Tamam** Azure Cosmos DB hesabına bağlanın. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![bağlantı dizesi][22]

## <a name="connect-to-azure-data-lake-store-by-uri"></a>Azure Data Lake Store URI ile bağlanma

Aboneliğinizde olmayan kaynaklara erişmek istiyorsanız. Ancak diğer kullanıcılar kaynaklar için URI’yi alma izni veriyorsa. Bu durumda, oturum açtıktan sonra URI’yi kullanarak Data Lake Store’a bağlanabilirsiniz. Aşağıdaki adımlara bakın.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**’a sağ tıklayıp bağlam menüsünden **Data Lake Store’a bağlan...** seçeneğine tıklayın.

    ![Data Lake Store’a bağlanma bağlam menüsü](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Siz URI’yi girdikten sonra araç girdiğiniz URL’nin konumuna gider.

    ![Data Lake Store’a bağlanma bağlam iletişimi](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Data Lake Store’a bağlanma sonucu](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama

Depolama kaynağı bulmanız gerekiyorsa ve nerede olduğunu bilmiyorsanız, kaynağı aramak için sol bölmenin en üstünde arama kutusunu kullanabilirsiniz.

Arama kutusuna yazarken sol bölmede o noktaya kadar girdiğiniz arama değeriyle eşleşen tüm kaynakları görüntüler. Örneğin, bir arama **uç noktaları** aşağıdaki ekran görüntüsünde gösterilir:

![Depolama hesabı araması][23]

> [!NOTE]
> Kullanım **hesabı yönetim panelinde** aramanızın yürütme süresini kısaltmak için aradığınız öğeyi içermeyen tüm abonelikleri seçimini kaldırmak için. Ayrıca bir düğümüne sağ tıklayın ve seçin **gelen burada arama** belirli bir düğümden aramaya başlanacak.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile Azure Blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [(Önizleme) Azure depolama Gezgini'nde Azure Cosmos DB'yi yönetme](./cosmos-db/storage-explorer.md)
* [Depolama Gezgini ile Azure Data Lake Store kaynaklarını yönetme](./data-lake-store/data-lake-store-in-storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/Overview.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ManageAccounts.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog-SignInSelected.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/AccountPanel.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/SubscriptionNode.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/ConnectDialog.png
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
