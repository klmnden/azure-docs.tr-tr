<properties
    pageTitle="Depolama Gezgini ile çalışmaya başlama (Önizleme) | Microsoft Azure"
    description="Depolama Gezgini (Önizleme) ile Azure Storage kaynaklarını yönetme"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/05/2016"
    ms.author="tarcher" />

# Depolama Gezgini ile çalışmaya başlama (Önizleme)

## Genel Bakış 

Microsoft Azure Storage Gezgini (Önizleme) Windows, OSX ve Linux üzerinden Azure Storage ile kolayca çalışmanızı sağlayan bir tek başına programdır. Bu makalede Azure Storage hesaplarınızı bağlama ve yönetme ile ilgili çeşitli yöntemler öğreneceksiniz.

## Ön koşullar

- [Depolama Gezgini (önizleme) indirip yüklemek](http://go.microsoft.com/fwlink/?LinkId=708343)

## Bir depolama hesabı veya hizmetine bağlanmak

Depolama Gezgini (Önizleme) depolama hesaplarına bağlamak için çok bir yol sağlar. Buna, Azure aboneliklerinizle ilişkilendirilen depolama hesaplarınızı bağlama, diğer Azure aboneliklerinden paylaşılan depolama hesapları ve hizmetlerine bağlanma ve hatta Azure Storage Öykünücüsü kullanarak yerel depolamaya bağlanma ve yönetme dahildir.

- [Bir Azure aboneliğine bağlanma](#connect-to-an-azure-subscription) - Azure aboneliğinize ait depolama kaynaklarını yönetin.
- [Yerel depolamaya bağlanma](#connect-to-local-storage) -Azure Storage Öykünücüsü kullanarak yerel depolama yönetin. 
- [Dış depolama birimine ekleme](#attach-or-detach-an-external-storage-account) - Depolama hesabının hesap adı ve anahtarını kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetin.
- [SAS kullanarak hesap ekleme](#attach-account-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetme.
- [SAS kullanarak hizmet ekleme](#attach-service-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait belirli bir depolama hizmetini (blob kapsayıcısı, kuyruk veya tablo) yönetme.

## Bir Azure aboneliğine Bağlanma

> [AZURE.NOTE] Bir Azure hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Depolama Gezgini’ni (Önizleme) başlatın. 

1. Depolama Gezgini (Önizleme) ilk kez çalıştırıyorsanız, veya önce Depolama Gezgini (Önizleme) çalıştırdıysanız, ancak bir Azure hesabına bağlanmadıysanız, bir Azure hesabına bağlanmanızı sağlayacak bir bilgi çubuğu göreceksiniz.

    ![][0]
    
1. **Microsoft Azure’a Bağlan**’ı seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.

Bir Microsoft hesabıyla başarılı şekilde oturum açtıktan sonra Depolama Gezgini’nin (Önizleme) sol paneli Microsoft hesabı ile ilişkilendirilen tüm Azure abonelikleriyle ilişkilendirilen tüm depolama hesapları ile doldurulacaktır.

### Azure aboneliklerini filtreleme

Depolama Gezgini (Önizleme) sol panelde oturum açılan Microsoft hesabı veya hesaplarınızla ilişkilendirilen Azure aboneliklerinin hangilerinin depolama hesaplarının görüntüleneceğini filtrelemenizi sağlar.

1. **Ayarlar** (dişli) simgesini seçin.

    ![][1]   

1.  Sol bölmenin en üstünde oturum açtığınız tüm Microsoft hesaplarını içeren bir açılır liste göreceksiniz. 

    ![][3]
     
1.  Oturum açılan tüm Microsoft hesapları ile ek Microsoft hesaplarının eklenmesi (hesaplarda oturum açılması) ile ilgili bir bağlantı görmek üzere açılır listenin yanındaki aşağı yönlü oku seçin.

    ![][4]

1.  Açılır listeden istenilen Microsoft hesabını seçin.

1.  Sol bölmede seçili Microsoft hesabı ile ilişkilendirilen tüm Azure abonelikleri görüntülenecektir.
Her Azure aboneliğinin solundaki onay kutusu Depolama Gezgini’nin (Önizleme) ilgili Azure abonelikleriyle ilişkilendirilen tüm depolama hesaplarını görüntüleyip görüntülememeyi seçmenizi sağlar. **Tüm abonelikler**’in işaretlenmesi/işaretinin kaldırılması listelenen Azure aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.

    ![][2]  

1.  Yönetmek istediğiniz Azure aboneliklerini seçmeyi tamamladığınızda **Uygula**’yı seçin. Geçerli Microsoft hesabı için her seçili Azure aboneliğinin depolama hesabının listelenmesi için soldaki panel güncelleştirilecektir. 

### Ek Microsoft hesapları ekleme

Aşağıdaki adımlar, her hesabın Azure aboneliğini veya aboneliklerini ve depolama hesaplarını görüntülemek üzere ek Microsoft hesaplarını ekleme konusunda size kılavuzluk sağlayacaktır.

1.  **Ayarlar** (dişli) simgesini seçin.

    ![][1]   

1.  Sol bölmenin en üstünde mevcut bağlı tüm Microsoft hesaplarını içeren bir açılır liste göreceksiniz.

    ![][3]
     
1.  Oturum açılan tüm Microsoft hesapları ile ek Microsoft hesaplarının eklenmesi (hesaplarda oturum açılması) ile ilgili bir bağlantı görmek üzere açılır listenin yanındaki aşağı yönlü oku seçin.

    ![][4]

1.  En az bir etkin Azure aboneliği ile ilişkili bir hesapta oturum açmak için **Bir hesap ekle**’yi seçin ve iletişim kutularını izleyin.

1.  Gözatmak istediğiniz Azure aboneliklerinin onay kutularını seçin. 

    ![][2]  

1.  **Uygula**’yı seçin.

### Microsoft hesapları arasında geçiş

Birden çok Microsoft hesabına bağlanabilirsiniz, buna karşın soldaki bölmede yalnızca bir (geçerli) Microsoft hesabının abonelikleri ile ilgili depolama hesapları görüntülenir. Birden çok Microsoft hesabına bağlanırsanız, aşağıdaki adımları gerçekleştirerek hesaplar arasında geçiş yapabilirsiniz.

1.  **Ayarlar** (dişli) simgesini seçin.

    ![][1]   

1.  Sol bölmenin en üstünde mevcut bağlı tüm Microsoft hesaplarını içeren bir açılır liste göreceksiniz.

    ![][3]
     
1.  Oturum açılan tüm Microsoft hesapları ile ek Microsoft hesaplarının eklenmesi (hesaplarda oturum açılması) ile ilgili bir bağlantı görmek üzere açılır listenin yanındaki aşağı yönlü oku seçin.

    ![][4]

1.  İstenen Microsoft hesabını seçin.

1.  Gözatmak istediğiniz Azure aboneliklerinin onay kutularını seçin. 

    ![][2]  

1.  **Uygula**’yı seçin.
  
## Yerel depolamaya bağlanma

Depolama Gezgini (Önizleme) Azure Storage Öykünücüsü kullanarak yerel depolama karşı çalışmanıza olanak tanır. Bu, (depolama hesabı Azure Storage Öykünücüsü tarafından öykündüğü için) depolama hesabını Azure’ye dağıtmadan hesaba karşı kod yazıp test edebilmenizi sağlar.

>[AZURE.NOTE] Azure Storage Öykünücüsü şu anda yalnızca Windows için desteklenmektedir. 

1. Depolama Gezgini’ni (Önizleme) başlatın. 

1. Sol bölmedeki **(Geliştirme)** düğümünü genişletin.

    ![][21]

1. Azure Storage Öykünücüsünü henüz yüklemediyseniz, bir bilgi çubuğu ile yüklemeniz istenecektir. Bilgi çubuğu görüntülendiyse**En yeni sürümü indir**’i seçin ve öykünücüyü yükleyin. 

    ![][22]

1. Öykünücü yüklendikten sonra yerel bloblar, kuyruklar ve tablolar oluşturup bunlarla çalışabileceksiniz. Her depolama hesabı türü ile çalışmayı öğrenmek için aşağıdaki ilgili bağlantıyı seçin:

    - [Azure Blob Storage kaynaklarını yönetme](./vs-azure-tools-storage-explorer-blobs.md)
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

    ![][5]
    
1.  **Erişim tuşları** dikey penceresinde depolama hesabını eklerken **DEPOLAMA HESABI ADI** ve **ANAHTAR 1** kullanım değerlerini kopyalayın. 

    ![][6]

### Harici bir depolama hesabı ekleme

1.  Depolama Gezgini’nde (Önizleme) **Depolama Hesapları**’na sağ tıklayın ve içerik menüsünden **Harici Depolamayı Ekle**’yi seçin.

    ![][7]
    
1.  *Depolama hesabı bilgilerini al* bölümü depolama hesabı adı ve anahtar 1 değerlerinin nasıl alınacağını açıklamaktadır. Bu adımda, bu değerler kullanılır. **Harici Depolama Ekleme** iletişim kutusunda **Hesap adı** kutusuna depolama hesabı adını ve **Hesap anahtarı** kutusuna Anahtar 1 değerini girin. Tamamladığınızda **Tamam**’ı seçin. 

    ![][8]

    Bağlandıktan sonra harici depolama hesabı depolama hesabı adına **(Harici)** metni eklenmiş olarak görüntülenecektir. 

    ![][9]

### Harici bir depolama hesabı ayırma

1.  Ayırmak istediğiniz harici depolama hesabına sağ tıklayın ve içerik menüsünden **Ayır**’ı seçin.

    ![][10]

1.  Onaylama ileti kutusu göründüğünde harici depolama hesabından ayrılmayı onaylamak üzere **Evet**’i seçin.

    ![][12]

## SAS kullanarak hesap ekleme

Bir SAS (Paylaşılan Erişim İmzası) bir Azure aboneliği yöneticisine, Azure abonelik kimlik bilgilerini sağlamaya gerek kalmadan geçici olarak bir depolama hesabına erişme olanağı sağlar. 

Bunu açıklamak üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim, ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istiyor:

1. KullanıcıA belirli bir zaman dilimi ve istenilen izinlerle (depolama hesabı için bağlantı dizesinden oluşan) bir SAS oluşturuyor.
1. KullanıcıA SAS’ı örneğimizde depolama hesabına erişmek isteyen kişiyle, KullanıcıB ile paylaşıyor.  
1. KullanıcıB, sağlanan SAS’ı kullanarak KullanıcıA’ya ait hesabı eklemek için Depolama Gezgini’ni (Önizleme) kullanır. 

### Paylaşmak istediğiniz hesap için bir SAS alma

1.  Depolama Gezgini’ni (Önizleme) açın.
1.  Sol bölmede paylaşmak istediğiniz depolama hesabına sağ tıklayın ve içerik menüsünden **Paylaşılan Erişim İmzası Al**’ı seçin.

    ![][13]

1. **Paylaşılan Erişim İmzası** iletişim kutusunda hesap ile ilgili istediğiniz zaman dilimi ve izinleri seçin, ardından **Oluştur**’u seçin.

    ![][14]
 
1. SAS’ı görüntüleyen ikinci bir **Paylaşılan Erişim İmzası** iletişim kutusu görünecektir. Panoya kopyalamak için **Bağlantı Dizesi**’nin yanındaki **Kopyala**’yı seçin. İletişim kutusunu kapatmak için **Kapat**’ı seçin.

### SAS kullanarak paylaşılan hesaba ekleme

1.  Depolama Gezgini’ni (Önizleme) açın.
1.  Sol bölmede **Depolama Hesapları**’na sağ tıklayın ve içerik menüsünden **SAS kullanarak hesap ekle**’yi seçin.
    ![][15]

1. **SAS kullanarak Hesap Ekle** iletişim kutusunda:

    - **Hesap Adı** - Bu hesapla ilişkilendirmek istediğiniz adı girin. **Not:** hesap adının SAS’ın oluşturulduğu orijinal depolama hesabının adıyla aynı olması gerekli değildir. 
    - **Bağlantı Dizesi** - daha önce kopyaladığınız bağlantı dizesini yapıştırın.
    - Tamamladığınızda **Tamam**’ı seçin.
    
    ![][16]

Ekledikten sonra, depolama hesabı sağladığınız hesap adının sonuna (SAS) metni eklenmiş olarak görüntülenecektir.

![][17]

## SAS kullanarak hizmet ekleme

[SAS kullanarak hesap ekleme](#attach-account-using-sas) Azure abonelik yöneticisinin depolama hesabı için bir SAS oluşturarak (ve paylaşarak) nasıl geçici erişim sağladığını açıklamaktadır. Benzer şekilde bir depolama hesabının içinde belirli bir hizmet için (blob kapsayıcısı, kuyruk veya tablo) bir SAS oluşturulabilir.  

### Paylaşmak istediğiniz hizmet için bir SAS oluşturma

Bu bağlamda bir hizmet, bir blob kapsayıcısı, sıra veya tablo olabilir. Aşağıdaki bölümlerde listelenen hizmet için nasıl SAS oluşturulacağı açıklanmıştır:

- [Blob kapsayıcısı için SAS alma](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Bir kuyruk için SAS alma - *Çok yakında*
- Bir tablo için SAS alma - *Çok yakında*

### SAS kullanarak paylaşılan hesap hizmetine ekleme

1.  Depolama Gezgini’ni (Önizleme) açın.
1.  Sol bölmede **Depolama Hesapları**’na sağ tıklayın ve içerik menüsünden **SAS kullanarak hizmet ekle**’yi seçin.
    ![][18]

1. **SAS kullanarak Hesap Ekle** iletişim kutusunda daha önceden kopyaladığınız SAS URI’sini yapıştırın ve **Tamam**’ı seçin.

    ![][19]

Eklendikten sonra yeni eklenen hizmet **(Hizmet SAS’ı)** düğümü altında görüntülenecektir. 

![][20]

## Depolama hesapları arama

Depolama hesaplarından oluşan uzun bir listeniz varsa, sol bölmenin üzerindeki arama kutusunu kullanmak belirli bir depolama hesabını bulmak için hızlı bir yöntemdir. 

Arama kutusuna yazarken sol bölmede yalnızca o noktaya kadar girdiğiniz arama değeriyle eşleşen depolama hesapları görüntülenecektir. Aşağıdaki ekran görüntüsünde, aranan tüm depolama hesapları arasından “tarcher” metnini içeren depolama hesabı adlarının görüntülendiği bir örnek yer almaktadır.

![][11]
    
Aramayı temizlemek için arama kutusundaki **x** düğmesini seçin.

## Sonraki adımlar
- [Depolama Gezgini (Önizleme) ile Azure Blob Storage kaynaklarını yönetme](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-gear.png
[2]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-subscriptions.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/filter-accounts.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/accounts-drop-down.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[7]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-context-menu.png
[16]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-dlg.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[18]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-context-menu.png
[19]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-dlg.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png



<!--HONumber=Jun16_HO2-->


