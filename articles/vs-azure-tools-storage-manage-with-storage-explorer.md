<properties
    pageTitle="Depolama Gezgini ile çalışmaya başlama (Önizleme) | Microsoft Azure"
    description="Depolama Gezgini (Önizleme) ile Azure Storage kaynaklarını yönetme"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />


# Depolama Gezgini ile çalışmaya başlama (Önizleme)

## Genel Bakış 

Microsoft Azure Depolama Gezgini (Önizleme) Windows, OS X ve Linux’ta Azure Depolama ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Bu makalede Azure Storage hesaplarınızı bağlama ve yönetme ile ilgili çeşitli yöntemler öğreneceksiniz.

![Microsoft Azure Depolama Gezgini (Önizleme)][15]

## Ön koşullar

- [Depolama Gezgini (önizleme) indirip yüklemek](http://www.storageexplorer.com)

## Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini (Önizleme) depolama hesaplarına bağlamak için çok bir yol sağlar. Buna, Azure aboneliklerinizle ilişkilendirilen depolama hesaplarınızı bağlama, diğer Azure aboneliklerinden paylaşılan depolama hesapları ve hizmetlerine bağlanma ve hatta Azure Storage Öykünücüsü kullanarak yerel depolamaya bağlanma ve yönetme dahildir.

- [Bir Azure aboneliğine bağlanma](#connect-to-an-azure-subscription) - Azure aboneliğinize ait depolama kaynaklarını yönetin.
- [Yerel geliştirme deposu ile çalışma](#work-with-local-development-storage) -Azure Storage Öykünücüsü kullanarak yerel depolamayı yönetin. 
- [Dış depolama birimine ekleme](#attach-or-detach-an-external-storage-account) - Depolama hesabının hesap adı ve anahtarını kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetin.
- [SAS kullanarak depolama hesabı ekleme](#attach-storage-account-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetme.
- [SAS kullanarak hizmet ekleme](#attach-service-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait belirli bir depolama hizmetini (blob kapsayıcısı, kuyruk veya tablo) yönetme.

## Bir Azure aboneliğine Bağlanma

> [AZURE.NOTE] Bir Azure hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Storage Explorer’da (Önizleme) **Azure Hesap ayarları**’nı seçin. 

    ![Azure hesap ayarları][0]

1. Sol bölmede oturum açtığınız tüm Microsoft hesapları gösterilir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.

    ![Hesap ekleme][1]

1. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler**’in seçilmesi listelenen Azure aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Azure aboneliklerini seçme][3]

1. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

## Yerel geliştirme deposu ile çalışma

Depolama Gezgini (Önizleme) Azure Storage Öykünücüsü kullanarak yerel depolama karşı çalışmanıza olanak tanır. Bu, (depolama hesabı Azure Storage Öykünücüsü tarafından öykündüğü için) depolama hesabını Azure’ye dağıtmadan hesaba karşı kod yazıp test edebilmenizi sağlar.

>[AZURE.NOTE] Azure Storage Öykünücüsü şu anda yalnızca Windows için desteklenmektedir. 

1. Storage Explorer (Önizleme) sol bölmesinde **Yerel ve Bağlı** > **Depolama Hesapları** > **(Geliştirme)** düğümünü genişletin.

    ![Yerel geliştirme düğümü][21]

1. Azure Storage Öykünücüsünü henüz yüklemediyseniz, bir bilgi çubuğu ile yüklemeniz istenecektir. Bilgi çubuğu görüntülendiyse**En yeni sürümü indir**’i seçin ve öykünücüyü yükleyin. 

    ![Azure Storage Öykünücüsünü İndirme istemi][22]

1. Öykünücü yüklendikten sonra yerel bloblar, kuyruklar ve tablolar oluşturup bunlarla çalışabileceksiniz. Her depolama hesabı türü ile çalışmayı öğrenmek için aşağıdaki ilgili bağlantıyı seçin:

    - [Azure Blob Storage kaynaklarını yönetme](./vs-azure-tools-storage-explorer-blobs.md)
    - Azure dosya paylaşımı depolama kaynaklarını yönetme - *Çok yakında*
    - Azure kuyruk depolama kaynaklarını yönetmek - *Çok yakında*
    - Azure Table Storage kaynaklarını yönetmek - *Çok yakında*

## Harici bir depolama hesabı ekleme veya ayırma

Depolama Gezgini (Önizleme) depolama hesaplarının kolayca paylaşılabilmesi için harici depolama hesapları ekleme özelliği sağlar. Bu bölümde harici depolama hesaplarının nasıl ekleneceği (ve ayrılacağı) açıklanmaktadır.

### Depolama hesabının kimlik bilgilerini alma

Harici bir depolama hesabını paylaşmak için ilgili hesabın sahibi ilk olarak hesabın kimlik bilgilerini, yeni hesap adını ve anahtarını almalıdır, ardından bu bilgileri ilgili (harici) hesabı eklemek isteyen kişiyle paylaşmalıdır. Depolama hesabı kimlik bilgileri aşağıdaki adımları izleyerek Azure portal’ı üzerinden edinilebilir: 

1.  [Azure Portal](https://portal.azure.com)’ında oturum açın.
1.  **Gözat**’ı seçin.
1.  **Depolama Hesapları**’nı seçin.
1.  **Depolama Hesapları** dikey penceresinde istenen depolama hesabını seçin.
1.  Seçili depolama hesabının **Depolama** dikey penceresinde **Erişim tuşları**’nı seçin.

    ![Erişim Anahtarları seçeneği][5]
    
1.  **Erişim tuşları** dikey penceresinde depolama hesabını eklerken **DEPOLAMA HESABI ADI** ve **ANAHTAR 1** kullanım değerlerini kopyalayın. 

    ![Erişim tuşları][6]

### Harici bir depolama hesabı ekleme
Bir dış depolama hesabına bağlanmak için hesabın adı ve anahtarı gereklidir. *Depolama hesabı kimlik bilgilerini alma* bölümünde bu değerlerin Azure portalından nasıl alınacağı açıklanmaktadır. Ancak, portalda hesap anahtarı "anahtar 1" olarak adlandırılmaktadır; bu nedenle Storage Explorer (Önizleme) bir hesap anahtarı istediğinde "anahtar 1" değerini girmeniz (veya yapıştırmanız) gerekir. 
 
1.  Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

1.  **Azure Storage’a Bağlan** iletişim kutusunda hesap anahtarını (Azure portalındaki "anahtar 1" değeri) belirtin ve ardından **İleri**’yi seçin.

    ![Azure Storage’a bağlan iletişim kutusu][24] 

1.  **Harici Depolama Ekle** iletişim kutusunda **Hesap adı** kutusuna depolama hesabı adını girin, istediğiniz diğer ayarları belirtin ve işiniz bittiğinde **İleri**’yi seçin. 

    ![Harici depolama ekle iletişim kutusu][8]

1.  **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.

1.  Bağlandıktan sonra harici depolama hesabı depolama hesabı adına **(Harici)** metni eklenmiş olarak görüntülenecektir. 

    ![Harici depolama hesabına bağlanma sonucu][9]

### Harici bir depolama hesabı ayırma

1.  Ayırmak istediğiniz harici depolama hesabına sağ tıklayın ve içerik menüsünden **Ayır**’ı seçin.

    ![Depolama alanından ayırma seçeneği][10]

1.  Onaylama ileti kutusu göründüğünde harici depolama hesabından ayrılmayı onaylamak üzere **Evet**’i seçin.

## SAS kullanarak depolama hesabı ekleme

[SAS (Paylaşılan Erişim İmzası)](storage/storage-dotnet-shared-access-signature-part-1.md), Azure aboneliği yöneticilerine Azure abonelik kimlik bilgilerini girmeden geçici olarak bir depolama hesabına erişme olanağı sağlar. 

Bunu açıklamak üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim, ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istiyor:

1. KullanıcıA belirli bir zaman dilimi ve istenilen izinlerle (depolama hesabı için bağlantı dizesinden oluşan) bir SAS oluşturuyor.
1. KullanıcıA SAS’ı örneğimizde depolama hesabına erişmek isteyen kişiyle, KullanıcıB ile paylaşıyor.  
1. KullanıcıB, sağlanan SAS’ı kullanarak KullanıcıA’ya ait hesabı eklemek için Depolama Gezgini’ni (Önizleme) kullanır. 

### Paylaşmak istediğiniz hesap için bir SAS alma

1.  Storage Explorer’da (Önizleme) paylaşmak istediğiniz depolama hesabına sağ tıklayın ve içerik menüsünden **Paylaşılan Erişim İmzası Al**’ı seçin.

    ![SAS alma içerik menüsü seçeneği][13]

1. **Paylaşılan Erişim İmzası** iletişim kutusunda hesap ile ilgili istediğiniz zaman dilimi ve izinleri seçin, ardından **Oluştur**’u seçin.

    ![SAS alma iletişim kutusu][14]
 
1. SAS’ı görüntüleyen ikinci bir **Paylaşılan Erişim İmzası** iletişim kutusu görünecektir. Panoya kopyalamak için **Bağlantı Dizesi**’nin yanındaki **Kopyala**’yı seçin. İletişim kutusunu kapatmak için **Kapat**’ı seçin.

### SAS kullanarak paylaşılan hesaba ekleme

1.  Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

1.  **Azure Storage’a Bağlan** iletişim kutusunda bağlantı dizesini belirtin ve ardından **İleri**’yi seçin.

    ![Azure Storage’a bağlan iletişim kutusu][24] 

1.  **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.

1.  Ekledikten sonra, depolama hesabı sağladığınız hesap adının sonuna (SAS) metni eklenmiş olarak görüntülenecektir.

    ![SAS kullanarak hesaba ekleme sonucu][17]

## SAS kullanarak hizmet ekleme

[SAS kullanarak depolama hesabı ekleme](#attach-storage-account-using-sas) bölümünde Azure abonelik yöneticisinin depolama hesabı için bir SAS oluşturarak (ve paylaşarak) nasıl geçici erişim sağlayacağı açıklanır. Benzer şekilde bir depolama hesabının içinde belirli bir hizmet için (blob kapsayıcısı, kuyruk veya tablo) bir SAS oluşturulabilir.  

### Paylaşmak istediğiniz hizmet için bir SAS oluşturma

Bu bağlamda bir hizmet, bir blob kapsayıcısı, sıra veya tablo olabilir. Aşağıdaki bölümlerde listelenen hizmet için nasıl SAS oluşturulacağı açıklanmıştır:

- [Blob kapsayıcısı için SAS alma](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Dosya paylaşımı için SAS alma - *Çok yakında*
- Bir kuyruk için SAS alma - *Çok yakında*
- Bir tablo için SAS alma - *Çok yakında*

### SAS kullanarak paylaşılan hesap hizmetine ekleme

1.  Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

    ![Azure Storage’a bağlanma seçeneği][23]

1.  **Azure Storage’a Bağlan** iletişim kutusunda SAS URI’sini belirtin ve ardından **İleri**’yi seçin.

    ![Azure Storage’a bağlan iletişim kutusu][24] 

1.  **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.

1.  Eklendikten sonra yeni eklenen hizmet **(Hizmet SAS’ı)** düğümü altında görüntülenecektir.

    ![SAS kullanarak paylaşılan hizmete ekleme sonucu][20]

## Depolama hesapları arama

Depolama hesaplarından oluşan uzun bir listeniz varsa, sol bölmenin üzerindeki arama kutusunu kullanmak belirli bir depolama hesabını bulmak için hızlı bir yöntemdir. 

Arama kutusuna yazarken sol bölmede yalnızca o noktaya kadar girdiğiniz arama değeriyle eşleşen depolama hesapları görüntülenecektir. Aşağıdaki ekran görüntüsünde, aranan tüm depolama hesapları arasından “tarcher” metnini içeren depolama hesabı adlarının görüntülendiği bir örnek yer almaktadır.

![Depolama hesabı araması][11]
    
Aramayı temizlemek için arama kutusundaki **x** düğmesini seçin.

## Sonraki adımlar
- [Depolama Gezgini (Önizleme) ile Azure Blob Storage kaynaklarını yönetme](./vs-azure-tools-storage-explorer-blobs.md)

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



<!--HONumber=Sep16_HO3-->


