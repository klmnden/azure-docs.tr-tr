---
title: Depolama Gezgini ile çalışmaya başlama | Microsoft Docs
description: Depolama Gezgini ile Azure depolama kaynaklarını yönetme
services: storage
author: cawaMS
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.date: 04/22/2019
ms.author: cawa
ms.openlocfilehash: f7dd6d3d30f34ba2c69b40111bb28d484ce572e7
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508745"
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

Depolama Gezgini'ni yükleme Linux üzerinde daha fazla yardım için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting#linux-dependencies).

Azure Depolama Gezgini [sürüm notları](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) bazı dağıtımlar için belirli adımları içerir.

[Depolama Gezgini’ni indirip yükleme](https://www.storageexplorer.com)

---

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini depolama hesaplarına bağlamak için birçok yol sağlar. Genel olarak şunlardan birini yapabilirsiniz:

* [Aboneliklerinizi ve kaynaklarına erişmek için Azure'da oturum açın](#sign-in-to-azure)
* [Belirli bir depolama veya CosmosDB kaynak ekleme](#attach-a-specific-resource)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

> [!NOTE]
> Tam olarak oturum açtıktan sonra kaynaklara erişmek için Depolama Gezgini'ni hem Yönetim (ARM) hem de veri katmanı izinleri gerektirir. Başka bir deyişle, erişim veren hesabındaki kapsayıcıları ve kapsayıcılar içindeki verileri depolama hesabınız, Azure AD izinleri gerekir. Veri katmanında yalnızca izinlere sahipseniz kullanmayı [Azure AD ile iliştirme](#add-a-resource-via-azure-ad). Depolama Gezgini'nin gerekir tam izinleri hakkında daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#role-based-access-control-permission-issues).
>
>

1. Depolama Gezgini'nde seçin **hesaplarını yönetme** gitmek için **hesabı yönetim panelinde**.

    ![Hesapları yönetme][1]

2. Sol bölmede, artık için açtığınız tüm Azure hesaplarınıza görüntüler. Başka bir hesaba bağlanmak için **Hesap Ekle**

3. Ulusal bulut ya da bir Azure Stack oturum açmak istiyorsanız, tıklayarak **Azure ortamı** kullanmak istediğiniz hangi Azure bulut seçmek için açılır. Ortamınızı seçtikten sonra tıklayın **oturum açın...**  düğmesi. Azure Stack için imzalama olup [Depolama Gezgini'ni Azure Stack aboneliğine bağlanma](/azure-stack/user/azure-stack-storage-connect-se) daha fazla bilgi için.

    ![Seçeneğinde oturum][2]

4. Bir Azure hesabıyla oturum başarıyla oturum sonra hesabı ve bu hesapla ilişkili Azure abonelikleriyle sol bölmeye eklenir. Çalışmak ve sonra istediğiniz Azure aboneliklerini seçin **Uygula** (seçme **abonelikler:** veya listelenen Azure aboneliklerinin hiçbirinin seçilmemesini).

    ![Azure aboneliklerini seçme][3]

    Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

### <a name="attach-a-specific-resource"></a>Belirli bir kaynak ekleyin
    
Çeşitli kaynak eklemek için Depolama Gezgini'ni seçenekleri vardır. Şunları yapabilirsiniz:

* [Azure AD ile bir kaynak ekleyin](#add-a-resource-via-azure-ad): Veri katmanında yalnızca izinlerine sahipseniz, bir Blob kapsayıcısı veya bir ADLS Gen2 Blob kapsayıcısı eklemek için bu seçeneği kullanabilirsiniz.
* [Bir bağlantı dizesi kullanın](#use-a-connection-string): Bir depolama hesabı bağlantı dizesi varsa. Depolama Gezgini, her iki anahtarı destekler ve [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) bağlantı dizeleri.
* [SAS URI'si kullanma](#use-a-sas-uri): Varsa bir [SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) bir Blob kapsayıcısını, dosya paylaşımı, kuyruk veya tablo için URI. SAS URI'si almak için kullanabilir [Depolama Gezgini](#generate-a-sas-in-storage-explorer) veya [Azure portalında](https://portal.azure.com).
* [Adı ve anahtarı kullan](#use-a-name-and-key): Ya da hesabı anahtarları, depolama hesabınıza biliyorsanız, hızlı bir şekilde bağlanmak için bu seçeneği kullanabilirsiniz. Depolama hesabınız için anahtarları depolama hesabında bulunan **erişim anahtarları** dikey penceresine [Azure portalında](https://portal.azure.com).
* [Ekleme için yerel bir öykünücü](#attach-to-a-local-emulator): Kullanılabilir Azure depolama öykünücüsünü birini kullanıyorsanız, öykünücüsüne kolayca bağlanmak için bu seçeneği kullanın.
* [Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma](#connect-to-an-azure-cosmos-db-account-by-using-a-connection-string): CosmosDB örneğine bir bağlantı dizesi varsa.
* [Azure Data Lake Store URI tarafından bağlanma](#connect-to-azure-data-lake-store-by-uri): Bir Azure Data Lake Store için bir URI varsa.

#### <a name="add-a-resource-via-azure-ad"></a>Azure AD ile bir kaynak ekleyin

1. Açık **Bağlan iletişim** tıklayarak **bağlanma düğmesi** sol taraftaki dikey araç çubuğu.

    ![Azure Storage’a bağlanma seçeneği][9]

2. Zaten yapmadıysanız kullanırsanız **Azure hesabı Ekle** seçeneği kaynağına erişimi olan bir Azure hesabı için oturum açın. İade için oturum açtıktan sonra **Bağlan iletişim**.

3. Seçin **Azure Active Directory (Azure AD) aracılığıyla bir kaynak ekleyin** seçeneğini ve tıklayın **sonraki**.

4. İliştirmek istediğiniz depolama kaynağı ve kaynak abonelik erişimi olan bir Azure hesabı seçin ve ardından **sonraki**.

5. Eklemek istediğiniz kaynak türünü seçin ve ardından bağlanmak için gereken bilgileri girin. Bu sayfadaki girişleri eklemekte olduğunuz kaynak türüne bağlı olarak değişir. Doğru kaynak türünü seçtiğinizden emin olun. Gerekli bilgileri tıklatın doldurduktan sonra **sonraki**.

6. Bağlantı özeti gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Tüm bilgiler doğru görünüyorsa ardından **Bağlan**, aksi takdirde önceki sayfalar geri dönüp **geri** yanlış herhangi bir bilgiyi düzeltmek için düğme.

Bağlantı başarıyla eklendikten sonra kaynak ağaç otomatik olarak bağlantıyı temsil eden düğümüne gidin. Herhangi bir nedenden dolayı altında görünüyor varsa **yerel ve ekli** → **depolama hesapları** → **(kapsayıcılar bağlı)** → **Blob kapsayıcıları** . Depolama Gezgini bağlantınızı ekleyemedi veya bağlantı başarıyla eklendikten sonra veri erişim sonra başvurun olamaz [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) Yardım.

#### <a name="use-a-connection-string"></a>Bağlantı dizesini kullanın

1. Açık **Bağlan iletişim** tıklayarak **bağlanma düğmesi** sol taraftaki dikey araç çubuğu.

    ![Azure Storage’a bağlanma seçeneği][9]

2. Seçin **bağlantı dizesini kullanmak** seçeneğini ve tıklayın **sonraki**.

3. Bağlantınız için bir görünen ad seçin ve bağlantı dizenizi girin. Ardından **İleri**'ye tıklayın.

4. Bağlantı özeti gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Tüm bilgiler doğru görünüyorsa ardından **Bağlan**, aksi takdirde önceki sayfalar geri dönüp **geri** yanlış herhangi bir bilgiyi düzeltmek için düğme.

Bağlantı başarıyla eklendikten sonra kaynak ağaç otomatik olarak bağlantıyı temsil eden düğümüne gidin. Herhangi bir nedenden dolayı altında görünüyor varsa **yerel ve ekli** → **depolama hesapları**. Depolama Gezgini bağlantınızı ekleyemedi veya bağlantı başarıyla eklendikten sonra veri erişim sonra başvurun olamaz [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) Yardım.

#### <a name="use-a-sas-uri"></a>SAS URI'si kullanma

1. Açık **Bağlan iletişim** tıklayarak **bağlanma düğmesi** sol taraftaki dikey araç çubuğu.

    ![Azure Storage’a bağlanma seçeneği][9]

2. Seçin **paylaşılan erişim imzası (SAS) URI kullanma** seçeneğini ve tıklayın **sonraki**.

3. Bağlantınız için bir görünen ad seçin ve içinde SAS URI'sini girin. Otomatik doldurma iliştirmekte olduğunuz kaynak türü için hizmet uç noktası olmalıdır. Özel bir uç noktasını kullanıyorsanız, ardından bunu yapamazsınız mümkündür. **İleri**’ye tıklayın.

4. Bağlantı özeti gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Tüm bilgiler doğru görünüyorsa ardından **Bağlan**, aksi takdirde önceki sayfalar geri dönüp **geri** yanlış herhangi bir bilgiyi düzeltmek için düğme.

Bağlantı başarıyla eklendikten sonra kaynak ağaç otomatik olarak bağlantıyı temsil eden düğümüne gidin. Herhangi bir nedenden dolayı altında görünüyor varsa **yerel ve ekli** → **depolama hesapları** → **(kapsayıcılar bağlı)** → **türü için hizmet düğümü kapsayıcı bağlı**. Depolama Gezgini bağlantınızı ekleyemedi veya bağlantı başarıyla eklendikten sonra veri erişim sonra başvurun olamaz [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) Yardım.

#### <a name="use-a-name-and-key"></a>Adı ve anahtarı kullan

1. Açık **Bağlan iletişim** tıklayarak **bağlanma düğmesi** sol taraftaki dikey araç çubuğu.

    ![Azure Storage’a bağlanma seçeneği][9]

2. Seçin **bir depolama hesabı adı ve anahtarı kullan** seçeneğini ve tıklayın **sonraki**.

3. Bağlantınız için bir görünen ad seçin.

4. Depolama hesabınızın adını ve erişim anahtarlarını alıp birini girin.

5. Seçin **depolama etki alanı** kullanın ve ardından **sonraki**.

4. Bağlantı özeti gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Tüm bilgiler doğru görünüyorsa ardından **Bağlan**, aksi takdirde önceki sayfalar geri dönüp **geri** yanlış herhangi bir bilgiyi düzeltmek için düğme.

Bağlantı başarıyla eklendikten sonra kaynak ağaç otomatik olarak bağlantıyı temsil eden düğümüne gidin. Herhangi bir nedenden dolayı altında görünüyor varsa **yerel ve ekli** → **depolama hesapları**. Depolama Gezgini bağlantınızı ekleyemedi veya bağlantı başarıyla eklendikten sonra veri erişim sonra başvurun olamaz [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) Yardım.

#### <a name="attach-to-a-local-emulator"></a>Yerel bir öykünücü için ekleme

Depolama Gezgini, tüm platformlarda öykünücüleri destekler. İki şu anda kullanılabilir resmi öykünücüleri şunlardır:
* [Azure depolama öykünücüsü](storage/common/storage-use-emulator.md) (yalnızca Windows)
* [Azurite](https://github.com/azure/azurite) (Windows, macOS veya Linux)

Varsayılan bağlantı noktalarını, öykünücünün çalıştığından, kullanabileceğiniz **öykünücüsü - varsayılan bağlantı noktalarını** her zaman altında bulunan düğüm **yerel ve ekli** → **depolama hesapları** , uygulamanızı öykünücü hızlıca erişmek için. Bağlantınız için farklı bir ad kullanın veya sizin öykünücü varsayılan bağlantı noktaları üzerinde çalışmıyor eğer istiyorsanız aşağıdaki adımları.

1. Öykünücüyü başlatın. Hangi bağlantı noktalarını not edin zaman öykünücü üzerinde her hizmet türü için dinliyor. Bu bilgiler daha sonra bilmeniz gerekir.

   > [!IMPORTANT]
   > Depolama Gezgini, öykünücüsü otomatik olarak başlamaz. Kendiniz başlatmanız gerekir.

2. Açık **Bağlan iletişim** tıklayarak **bağlanma düğmesi** sol taraftaki dikey araç çubuğu.

    ![Azure Storage’a bağlanma seçeneği][9]

3. Seçin **eklemek için yerel bir öykünücü** seçeneğini ve tıklayın **sonraki**.

4. Bağlantınız için bir görünen ad seçin ve her hizmet türü için bir öykünücü dinlediği bağlantı noktalarını girin. Varsayılan olarak, metin kutuları çoğu öykünücüleri için varsayılan bağlantı noktası değerlerini içerir. **Dosyaları bağlantı noktası** resmi öykünücüleri hiçbiri şu anda dosyaları hizmet desteği de varsayılan olarak boş bırakılır. Ardından kullanmakta olduğunuz öykünücüyü desteklemesini rağmen, kullanılmakta olan bağlantı noktası girin. **İleri**’ye tıklayın.

5. Bağlantı özeti gözden geçirin ve tüm bilgilerin doğru olduğundan emin olun. Tüm bilgiler doğru görünüyorsa ardından **Bağlan**, aksi takdirde önceki sayfalar geri dönüp **geri** yanlış herhangi bir bilgiyi düzeltmek için düğme.

Bağlantı başarıyla eklendikten sonra kaynak ağaç otomatik olarak bağlantıyı temsil eden düğümüne gidin. Herhangi bir nedenden dolayı altında görünüyor varsa **yerel ve ekli** → **depolama hesapları**. Depolama Gezgini bağlantınızı ekleyemedi veya bağlantı başarıyla eklendikten sonra veri erişim sonra başvurun olamaz [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) Yardım.

#### <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma

Seçeneklerinin yanı sıra Azure aboneliği aracılığıyla Azure Cosmos DB hesaplarını yönetebilir, bir Azure Cosmos DB'ye bağlanmanın alternatif bir yolu, bir bağlantı dizesi kullanmaktır. Bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Soldaki ağaçtan **Yerel ve Bağlı**’yı bulun ve **Azure Cosmos DB Hesapları**’na sağ tıklayıp **Azure Cosmos DB’ye bağlan...** seçeneğini belirleyin.

    ![bağlantı dizesiyle Azure Cosmos DB'ye bağlanma][21]

2. Azure Cosmos DB API seçin, yapıştırma, **bağlantı dizesi**ve ardından **Tamam** Azure Cosmos DB hesabına bağlanın. Bağlantı dizesini alma hakkında daha fazla bilgi için bkz. [Bağlantı dizelerini edinme](https://docs.microsoft.com/azure/cosmos-db/manage-account).

    ![bağlantı dizesi][22]

#### <a name="connect-to-azure-data-lake-store-by-uri"></a>Azure Data Lake Store URI ile bağlanma

Aboneliğinizde olmayan kaynaklara erişmek istiyorsanız. Ancak diğer kullanıcılar kaynaklar için URI’yi alma izni veriyorsa. Bu durumda, oturum açtıktan sonra URI’yi kullanarak Data Lake Store’a bağlanabilirsiniz. Aşağıdaki adımlara bakın.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, **Yerel ve Ekli** öğesini genişletin.
3. **Data Lake Store**’a sağ tıklayıp bağlam menüsünden **Data Lake Store’a bağlan...** seçeneğine tıklayın.

    ![Data Lake Store’a bağlanma bağlam menüsü](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach.png)

4. Siz URI’yi girdikten sonra araç girdiğiniz URL’nin konumuna gider.

    ![Data Lake Store’a bağlanma bağlam iletişimi](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-uri-attach-dialog.png)

    ![Data Lake Store’a bağlanma sonucu](./media/vs-azure-tools-storage-manage-with-storage-explorer/storageexplorer-adls-attach-finish.png)


## <a name="generate-a-sas-in-storage-explorer"></a>Depolama Gezgini'nde SAS oluşturma

### <a name="account-level-sas"></a>Hesap düzeyi SAS

1. Paylaşın ve ardından istediğiniz depolama hesabına sağ tıklayın **paylaşılan erişim imzası Al...** .

    ![SAS alma içerik menüsü seçeneği][14]

2. İçinde **paylaşılan erişim imzası oluşturma** iletişim kutusunda, bir zaman dilimi ve hesabınız için istediğiniz ve ardından izinleri belirtin **Oluştur** düğmesi.

    ![SAS alma iletişim kutusu][15]

3. Her iki kopya artık **bağlantı dizesi** veya ham **sorgu dizesi** panonuza.

### <a name="service-level-sas"></a>Hizmet düzeyi SAS

[Depolama Gezgini'nde bir blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)

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
