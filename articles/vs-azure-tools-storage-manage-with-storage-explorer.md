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
ms.openlocfilehash: cccab530e86373fee8a78b42c8cba532b05c1bab
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="get-started-with-storage-explorer-preview"></a>Depolama Gezgini (Önizleme) ile çalışmaya başlama
## <a name="overview"></a>Genel Bakış
Azure Depolama Gezgini (Önizleme) Windows, macOS ve Linux’ta Azure Depolama ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Bu makalede Azure Depolama hesaplarınızı bağlama ve yönetme ile ilgili çeşitli yöntemler öğrenirsiniz.

![Microsoft Azure Depolama Gezgini (Önizleme)][15]

## <a name="prerequisites"></a>Önkoşullar
* [Depolama Gezgini (Önizleme) indirip yükleme](http://www.storageexplorer.com)

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

1. Storage Explorer’da (Önizleme) **Azure Hesap ayarları**’nı seçin.

    ![Azure hesap ayarları][0]

2. Sol bölmede oturum açtığınız tüm Microsoft hesapları gösterilir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için yönergeleri izleyin.

    >[!NOTE]
    >Ulusal Azure (oturum açarak Azure Almanya, Azure Kamu ve Azure China gibi) bulutları ile bağlantı şu anda desteklenmemektedir. Ulusal Azure depolama hesaplarına nasıl bağlanacağınızı öğrenmek için “Harici bir depolama hesabı ekleme veya ayırma” bölümüne bakın.

3. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler**’in seçilmesi listelenen Azure aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Azure aboneliklerini seçme][3]  
    Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Bir Azure Stack aboneliğine bağlanma

Azure Stack aboneliğine bağlanma hakkında bilgi için bkz. [Depolama Gezgini’ni Azure Stack aboneliğine bağlama](azure-stack/user/azure-stack-storage-connect-se.md).

## <a name="work-with-local-development-storage"></a>Yerel geliştirme deposu ile çalışma
Depolama Gezgini (Önizleme) ile Azure Depolama Öykünücüsü kullanarak yerel depolamaya karşı çalışabilirsiniz. Bu yaklaşım, (depolama hesabı Azure Depolama Öykünücüsü tarafından öykündüğü için) depolama hesabını Azure’a dağıtmadan hesaba karşı kod yazıp test edebilmenizi sağlar.

> [!NOTE]
> Azure Storage Öykünücüsü şu anda yalnızca Windows için desteklenmektedir.
>
>

1. Depolama Gezgini (Önizleme) sol bölmesinde **(Yerel ve Bağlı)** > **Depolama Hesapları** > **(Geliştirme)** düğümünü genişletin.

    ![Yerel geliştirme düğümü][21]

2. Azure Depolama Öykünücüsünü henüz yüklemediyseniz, bir bilgi çubuğu ile yüklemeniz istenir. Bilgi çubuğu görüntülendiyse **En yeni sürümü indir**’i seçin ve öykünücüyü yükleyin.

    ![Azure Storage Öykünücüsünü İndirme istemi][22]

3. Öykünücü yüklendikten sonra yerel bloblar, kuyruklar ve tablolar oluşturup bunlarla çalışabilirsiniz. Her depolama hesabı türü ile çalışma hakkında bilgi almak için aşağıdaki bağlantılardan birine bakın:

    * [Azure blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
    * Azure dosya paylaşımı depolama kaynaklarını yönetme: *Çok yakında*
    * Azure kuyruk depolama kaynaklarını yönetme: *Çok yakında*
    * Azure Tablo Depolama kaynaklarını yönetme - *Çok yakında*

## <a name="attach-or-detach-an-external-storage-account"></a>Harici bir depolama hesabı ekleme veya ayırma
Depolama Gezgini (Önizleme) ile depolama hesaplarının kolayca paylaşılabilmesi için dış depolama hesaplarına ekleyebilirsiniz. Bu bölümde harici depolama hesaplarının nasıl ekleneceği (ve ayrılacağı) açıklanmaktadır.

### <a name="get-the-storage-account-credentials"></a>Depolama hesabının kimlik bilgilerini alma
Harici bir depolama hesabını paylaşmak için ilgili hesabın sahibi ilk olarak hesabın kimlik bilgilerini (hesap adı ve anahtarı) almalıdır, ardından bu bilgileri ilgili (harici) hesabı eklemek isteyen kişiyle paylaşmalıdır. Depolama hesabı kimlik bilgilerini aşağıdaki adımları izleyerek Azure portalından alabilirsiniz:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Gözat**’ı seçin.

3. **Depolama Hesapları**’nı seçin.

4. **Depolama Hesapları** dikey penceresinde istenen depolama hesabını seçin.

5. Seçili depolama hesabının **Ayarlar** dikey penceresinde **Erişim anahtarları**’nı seçin.

    ![Erişim Anahtarları seçeneği][5]

6. **Erişim anahtarları** dikey penceresinde depolama hesabını eklerken kullanılacak **Depolama hesabı adı** ve **key1** değerlerini kopyalayın.

    ![Erişim tuşları][6]

### <a name="attach-to-an-external-storage-account"></a>Harici bir depolama hesabı ekleme
Bir dış depolama hesabına bağlanmak için hesabın adı ve anahtarı gereklidir. “Depolama hesabı kimlik bilgilerini alma” bölümünde bu değerlerin Azure portalından nasıl alınacağı açıklanmaktadır. Ancak, portalda hesap anahtarı **key1** olarak adlandırılır. Bu nedenle, Depolama Gezgini (Önizleme) bir hesap anahtarı istediğinde **key1** değerini girmeniz gerekir.

1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

2. **Azure Depolama’ya Bağlan** iletişim kutusunda hesap anahtarını (Azure portalındaki **key1** değeri) belirtin ve ardından **İleri**’yi seçin.

    > [!NOTE]
    > Depolama bağlantı dizesini, ulusal Azure bulutu üzerindeki bir depolama hesabından girebilirsiniz. Örneğin, Azure Almanya depolama hesaplarına bağlamak için aşağıdakine benzer bağlantı dizeleri girin: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<storage_account_key>
    >* EndpointSuffix=core.cloudapi.de
    
    >Bağlantı dizesini Azure portalından "Depolama hesabı kimlik bilgilerini alma" bölümünde açıklandığı gibi alabilirsiniz.

    ![Azure depolamaya bağlan iletişim kutusu][24]

3. **Dış Depolama Ekle** iletişim kutusundaki **Hesap adı** kutusuna depolama hesabının adını girin, istediğiniz diğer ayarları belirtin ve ardından **İleri**’yi seçin.

    ![Dış depolama ekle iletişim kutusu][8]

4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. 

5. **Bağlan**’ı seçin.

6. Başarıyla bağlandıktan sonra dış depolama hesabı, depolama hesabı adına **(External)** metni eklenmiş olarak görüntülenir.

    ![Harici depolama hesabına bağlanma sonucu][9]

### <a name="detach-from-an-external-storage-account"></a>Harici bir depolama hesabı ayırma
1. Ayırmak istediğiniz dış depolama hesabına sağ tıklayın ve **Ayır**’ı seçin.

    ![Depolama alanından ayırma seçeneği][10]

2. Onay iletisinde, dış depolama hesabından ayırmayı onaylamak üzere **Evet**’i seçin.

## <a name="attach-a-storage-account-by-using-an-sas"></a>SAS kullanarak depolama hesabı ekleme
[SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) bir Azure aboneliği yöneticisinin Azure aboneliği kimlik bilgilerini vermek zorunda kalmadan bir depolama hesabına geçici erişim izni vermesini sağlar.

Bu senaryoyu göstermek üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istediğini varsayalım:

1. KullanıcıA belirli bir zaman dilimi ve istenilen izinlerle (depolama hesabı için bağlantı dizesinden oluşan) bir SAS oluşturuyor.

2. KullanıcıA SAS’ı depolama hesabına erişmek isteyen kişiyle (bu örnekte KullanıcıB) paylaşıyor.  

3. KullanıcıB, sağlanan SAS’ı kullanarak KullanıcıA’ya ait hesabı eklemek için Depolama Gezgini’ni (Önizleme) kullanır.

### <a name="get-an-sas-for-the-account-you-want-to-share"></a>Paylaşmak istediğiniz hesap için bir SAS alma
1. Depolama Gezgini’nde (Önizleme) bağlam menüsünden paylaşmak istediğiniz depolama hesabına sağ tıklayın ve **Paylaşılan Erişim İmzası Al**’ı seçin.

    ![SAS alma içerik menüsü seçeneği][13]

2. **Paylaşılan Erişim İmzası** iletişim kutusunda hesap için istediğiniz zaman dilimi ve izinleri belirleyin, ardından **Oluştur**’u seçin.

    ![SAS alma iletişim kutusu][14]  
    **Paylaşılan Erişim İmzası** iletişim kutusu açılır ve SAS gösterilir.

3. **Bağlantı Dizesi**’nin yanındaki **Kopyala**’yı seçerek panoya kopyalayın ve **Kapat**’ı seçin.

### <a name="attach-to-the-shared-account-by-using-the-sas"></a>SAS kullanarak paylaşılan hesaba ekleme
1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

2. **Azure Depolamaya Bağlan** iletişim kutusunda bağlantı dizesini belirtin ve ardından **İleri**’yi seçin.

    ![Azure depolamaya bağlan iletişim kutusu][24]

3. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin. 

4. **Bağlan**’ı seçin.

5. Eklendikten sonra, depolama hesabı sağladığınız hesap adının sonuna **(SAS)** metni eklenmiş olarak gösterilir.

    ![SAS kullanarak hesaba ekleme sonucu][17]

## <a name="attach-a-service-by-using-an-sas"></a>SAS kullanarak hizmet ekleme
“SAS kullanarak depolama hesabı ekleme” bölümünde Azure abonelik yöneticisinin depolama hesabı için bir SAS oluşturup paylaşarak nasıl geçici erişim izni verebileceği açıklanmaktadır. Benzer şekilde bir depolama hesabının içinde belirli bir hizmet için (blob kapsayıcısı, kuyruk veya tablo) bir SAS oluşturulabilir.  

### <a name="generate-an-sas-for-the-service-that-you-want-to-share"></a>Paylaşmak istediğiniz hizmet için SAS oluşturma
Bu bağlamda bir hizmet, bir blob kapsayıcısı, sıra veya tablo olabilir. Listelenen bir hizmet için SAS oluşturmak istiyorsanız bkz.:

* [Blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Dosya paylaşımı için SAS alma: *Çok yakında*
* Bir kuyruk için SAS alma: *Çok yakında*
* Bir tablo için SAS alma: *Çok yakında*

### <a name="attach-to-the-shared-account-service-by-using-the-sas"></a>SAS kullanarak paylaşılan hesap hizmetine ekleme
1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

2. **Azure Storage’a Bağlan** iletişim kutusunda SAS URI’sini belirtin ve ardından **İleri**’yi seçin.

    ![Azure depolamaya bağlan iletişim kutusu][24]

3. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Değişiklik yapmak için **Geri**’yi seçin ve istediğiniz ayarları girin. 

4. **Bağlan**’ı seçin.

5. Eklendikten sonra yeni eklenen hizmet **(Hizmet SAS’ı)** düğümü altında görüntülenecektir.

    ![SAS kullanarak paylaşılan hizmete ekleme sonucu][20]

## <a name="connect-to-an-azure-cosmos-db-account-by-using-a-connection-string"></a>Bir bağlantı dizesi kullanarak bir Azure Cosmos DB hesabına bağlanma
Bir bağlantı dizesi kullanmak için bir Azure Cosmos DB bağlayan alternatif bir yolu, yanında Azure aboneliği ile Azure Cosmos DB hesaplarını yönetin. Bir bağlantı dizesi kullanarak bağlanmak için aşağıdaki adımları kullanın.

1. Bul **yerel ve iliştirildiği** sol ağacında sağ **Azure Cosmos DB hesapları**, seçin **Azure Cosmos DB Bağlan...**

    ![Bağlantı dizesi tarafından Azure Cosmos Veritabanına bağlanın][33]

2. Azure Cosmos DB API'ı seçin, yapıştırın, **bağlantı dizesi**ve ardından **Tamam** Azure Cosmos DB hesap bağlanmak için. Bağlantı dizesi alma hakkında daha fazla bilgi için bkz: [bağlantı dizesini almak](https://docs.microsoft.com/azure/cosmos-db/manage-account#get-the--connection-string).

    ![bağlantı dizesi][32]

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama
Depolama hesaplarından oluşan uzun bir listeniz varsa, sol bölmenin üzerindeki arama kutusunu kullanmak belirli bir depolama hesabını bulmak için hızlı bir yöntemdir.

Arama kutusuna yazarken sol bölmede o noktaya kadar girdiğiniz arama değeriyle eşleşen depolama hesapları görüntülenir. Örneğin, adında **tarcher** bulunan tüm depolama hesapları için yapılan bir arama aşağıdaki ekran görüntüsünde gösterilmektedir:

![Depolama hesabı araması][11]

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini (Önizleme) ile Azure Blob Depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
* [Azure Depolama Gezgini (Önizleme) Azure Cosmos DB yönetme](./cosmos-db/storage-explorer.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
[32]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connection-string.PNG
[33]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-db-by-connection-string.PNG
