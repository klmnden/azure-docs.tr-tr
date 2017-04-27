---
title: "Depolama Gezgini ile çalışmaya başlama (Önizleme) | Microsoft Belgeleri"
description: "Depolama Gezgini (Önizleme) ile Azure Storage kaynaklarını yönetme"
services: storage
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 07b62cd6f6deb0cf3ff1c806204ebc26c773a164
ms.lasthandoff: 04/13/2017


---
# <a name="getting-started-with-storage-explorer-preview"></a>Depolama Gezgini ile çalışmaya başlama (Önizleme)
## <a name="overview"></a>Genel Bakış
Microsoft Azure Depolama Gezgini (Önizleme) Windows, macOS ve Linux’ta Azure Depolama ile kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Bu makalede Azure Depolama hesaplarınızı bağlama ve yönetme ile ilgili çeşitli yöntemler öğrenirsiniz.

![Microsoft Azure Depolama Gezgini (Önizleme)][15]

## <a name="prerequisites"></a>Önkoşullar
* [Depolama Gezgini (önizleme) indirip yükleme](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Bir depolama hesabı veya hizmetine bağlanmak
Depolama Gezgini (Önizleme) depolama hesaplarına bağlanmak için birkaç yol sağlar. Buna, Azure aboneliklerinizle ilişkilendirilen depolama hesaplarınızı bağlama, diğer Azure aboneliklerinden paylaşılan depolama hesapları ve hizmetlerine bağlanma ve hatta Azure Storage Öykünücüsü kullanarak yerel depolamaya bağlanma ve yönetme dahildir. Ayrıca global ve ulusal Azure'daki depolama hesaplarıyla çalışabilirsiniz:

* [Bir Azure aboneliğine bağlanma](#connect-to-an-azure-subscription) - Azure aboneliğinize ait depolama kaynaklarını yönetin.
* [Yerel geliştirme deposu ile çalışma](#work-with-local-development-storage) -Azure Storage Öykünücüsü kullanarak yerel depolamayı yönetin.
* [Dış depolama birimine ekleme](#attach-or-detach-an-external-storage-account) - Depolama hesabının adını, anahtarını ve uç noktalarını kullanarak başka bir Azure aboneliğine ait olan veya ulusal Azure bulutlarında bulunan depolama kaynaklarını yönetin.
* [SAS kullanarak depolama hesabı ekleme](#attach-storage-account-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait depolama kaynaklarını yönetme.
* [SAS kullanarak hizmet ekleme](#attach-service-using-sas) -SAS kullanarak başka bir Azure aboneliğine ait belirli bir depolama hizmetini (blob kapsayıcısı, kuyruk veya tablo) yönetme.

## <a name="connect-to-an-azure-subscription"></a>Bir Azure aboneliğine Bağlanma
> [!NOTE]
> Bir Azure hesabınız yoksa, [ücretsiz deneme için kaydolabilir](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) veya [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. Storage Explorer’da (Önizleme) **Azure Hesap ayarları**’nı seçin.

    ![Azure hesap ayarları][0]
2. Sol bölmede oturum açtığınız tüm Microsoft hesapları gösterilir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.
> [!NOTE]
>Oturum açarak Black Forest Azure, Fairfax Azure ve Mooncake Azure gibi ulusal Azure platformlarıyla bağlantı kurmak desteklenmez. Ulusal Azure depolama hesaplarına nasıl bağlanacağınızı öğrenmek için bkz. **Harici bir depolama hesabı ekleme veya ayırma**.

3. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler**’in seçilmesi listelenen Azure aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Azure aboneliklerini seçme][3]
4. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Seçili Azure abonelikleri][4]

## <a name="connect-to-an-azure-stack-subscription"></a>Bir Azure Stack aboneliğine bağlanma

1. Depolama Gezgini’nin Azure Stack aboneliğine uzaktan erişmesi için VPN bağlantısı gereklidir. Azure Stack ile VPN bağlantısı kurma hakkında bilgi edinmek için bkz. [VPN ile Azure Stack’e Bağlanma](azure-stack/azure-stack-connect-azure-stack.md#connect-with-vpn)

2. Azure Stack POC için Azure Stack yetkili kök sertifikasını dışarı aktarmanız gerekir. MAS-CON01, Azure Stack ana makinesi veya Azure Stack ile VPN bağlantısı olan yerel makinede `mmc.exe` menüsünü açın. **Dosya** menüsünde **Ek Bileşen Ekle/Kaldır**’ı seçin, **Yerel Bilgisayar**’ın **Bilgisayar hesabı**’nı yönetmek için **Sertifikalar** ekleyin.

   ![mmc.exe dosyası ile Azure Stack kök sertifikasını yükleme][25]   

   **Konsol Kökü\Sertifikalı (Yerel Bilgisayar)\Güvenilir Kök Sertifika Yetkilileri\Sertifikalar** altında **AzureStackCertificationAuthority** öğesini bulun. Öğeye sağ tıklayıp **Tüm Görevler -> Dışarı Aktar**’ı seçin. Ardından iletişim kutularını izleyerek sertifikayı **Base-64 kodlu X.509 (.CER)** ile dışarı aktarın. Dışarı aktarılan sertifika sonraki adımda kullanılır.   

   ![Kök Azure Stack yetkili kök sertifikasını dışarı aktarma][26]   

3. Depolama Gezgini’nde (Önizleme), **Düzenle** menüsünü, **SSL Sertifikaları** ve ardından **Sertifikaları İçeri Aktar**’ı seçin. Dosya seçici iletişim kutusunu kullanarak, önceki adımda bulduğunuz sertifikayı açın. İçeri aktardıktan sonra Depolama Gezgini'ni yeniden başlatmanız istenir.

   ![Depolama Gezgini’ne (Önizleme) sertifika aktarma][27]

4. Depolama Gezgini (Önizleme) yeniden başlatıldıktan sonra **Düzenle** menüsünü seçin ve **Hedef Azure Stack** seçeneğinin işaretli olduğundan emin olun. İşaretli değilse işaretleyin ve değişikliğin etkili olması için Depolama Gezgini’ni yeniden başlatın. Bu yapılandırma, Azure Stack ortamınıza uyum için gereklidir.

   ![Hedef Azure Stack’in seçili olduğundan emin olun][28]

5. Sol taraftaki çubukta **Hesapları Yönet**’i seçin. Oturum açtığınız tüm Microsoft hesapları sol bölmede gösterilir. Azure Stack hesabına bağlanmak için **Hesap ekle**’yi seçin.

   ![Azure stack hesabı ekleme][29]

6. **Yeni hesap ekle** iletişim kutusundaki **Azure ortamı** altında **Özel Ortam Seç** öğesini seçin ve ardından **İleri**’ye tıklayın.

7. Azure Stack ortamıyla ilgili tüm gerekli bilgileri girin ve ardından **Oturum Aç**’a tıklayın.  En az bir etkin Azure Stack aboneliğiyle ilişkili Azure Stack hesabıyla oturum açmak için **Özel Bulut ortamında oturum aç** iletişim kutusunu doldurun. İletişim kutusundaki her bir alanın ayrıntıları aşağıdaki gibidir:

    * **Ortam adı** – Alan kullanıcı tarafından özelleştirilebilir.
    * **Yetkili** – Değer https://login.windows.net olmalıdır. Azure Çin (Mooncake) için https://login.chinacloudapi.cn kullanılmalıdır.
    * **Oturum açma kaynak kimliği** – Aşağıdaki PowerShell komutunu yürüterek değeri alın:

    Bulut Yöneticisi iseniz:

    ```powershell
    PowerShell (Invoke-RestMethod -Uri https://adminmanagement.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
    ```

    Kiracıysanız:

    ```powershell
    PowerShell (Invoke-RestMethod -Uri https://management.local.azurestack.external/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]
    ```

    * **Graph uç noktası** – Değer https://graph.windows.net olmalıdır. Azure Çin (Mooncake) için https://graph.chinacloudapi.cn kullanılmalıdır.
    * **ARM kaynak kimliği** – Oturum açma kaynak kimliği ile aynı değeri kullanın.
    * **ARM kaynak uç noktası** – ARM kaynak uç noktası örnekleri:

    Bulut Yöneticisi için: https://adminmanagement.local.azurestack.external   
    Kiracı için: https://management.local.azurestack.external
 
    * **Kiracı Kimlikleri** – İsteğe bağlı. Değer yalnızca dizinin belirtilmesi zorunlu olduğunda verilir.

8. Bir Azure Stack hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure Stack abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini ve ardından **Uygula**’yı seçin. (**Tüm abonelikler**’in seçilmesi listelenen Azure Stack aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

   ![Özel Bulut Ortamı iletişim kutusunu doldurduktan sonra Azure Stack aboneliklerini seçin][30]

9. Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

   ![Azure Stack abonelik hesaplarını içeren depolama hesaplarının listesi][31]

## <a name="work-with-local-development-storage"></a>Yerel geliştirme deposu ile çalışma
Depolama Gezgini (Önizleme) Azure Storage Öykünücüsü kullanarak yerel depolama karşı çalışmanıza olanak tanır. Bu, (depolama hesabı Azure Storage Öykünücüsü tarafından öykündüğü için) depolama hesabını Azure’ye dağıtmadan hesaba karşı kod yazıp test edebilmenizi sağlar.

> [!NOTE]
> Azure Storage Öykünücüsü şu anda yalnızca Windows için desteklenmektedir.
>
>

1. Storage Explorer (Önizleme) sol bölmesinde **Yerel ve Bağlı** > **Depolama Hesapları** > **(Geliştirme)** düğümünü genişletin.

    ![Yerel geliştirme düğümü][21]
2. Azure Depolama Öykünücüsünü henüz yüklemediyseniz, bir bilgi çubuğu ile yüklemeniz istenir. Bilgi çubuğu görüntülendiyse**En yeni sürümü indir**’i seçin ve öykünücüyü yükleyin.

    ![Azure Storage Öykünücüsünü İndirme istemi][22]
3. Öykünücü yüklendikten sonra yerel bloblar, kuyruklar ve tablolar oluşturup bunlarla çalışabilirsiniz. Her depolama hesabı türü ile çalışma hakkında bilgi almak için aşağıdaki bağlantılardan birini seçin:

   * [Azure blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)
   * Azure dosya paylaşımı depolama kaynaklarını yönetme - *Çok yakında*
   * Azure kuyruk depolama kaynaklarını yönetmek - *Çok yakında*
   * Azure Table Storage kaynaklarını yönetmek - *Çok yakında*

## <a name="attach-or-detach-an-external-storage-account"></a>Harici bir depolama hesabı ekleme veya ayırma
Depolama Gezgini (Önizleme) depolama hesaplarının kolayca paylaşılabilmesi için harici depolama hesapları ekleme özelliği sağlar. Bu bölümde harici depolama hesaplarının nasıl ekleneceği (ve ayrılacağı) açıklanmaktadır.

### <a name="get-the-storage-account-credentials"></a>Depolama hesabının kimlik bilgilerini alma
Harici bir depolama hesabını paylaşmak için ilgili hesabın sahibi ilk olarak hesabın kimlik bilgilerini, yeni hesap adını ve anahtarını almalıdır, ardından bu bilgileri ilgili (harici) hesabı eklemek isteyen kişiyle paylaşmalıdır. Depolama hesabı kimlik bilgileri aşağıdaki adımları izleyerek Azure portal’ı üzerinden edinilebilir:

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. **Gözat**’ı seçin.
3. **Depolama Hesapları**’nı seçin.
4. **Depolama Hesapları** dikey penceresinde istenen depolama hesabını seçin.
5. Seçili depolama hesabının **Depolama** dikey penceresinde **Erişim tuşları**’nı seçin.

   ![Erişim Anahtarları seçeneği][5]
6. **Erişim tuşları** dikey penceresinde depolama hesabını eklerken **DEPOLAMA HESABI ADI** ve **ANAHTAR 1** kullanım değerlerini kopyalayın.

   ![Erişim tuşları][6]

### <a name="attach-to-an-external-storage-account"></a>Harici bir depolama hesabı ekleme
Bir dış depolama hesabına bağlanmak için hesabın adı ve anahtarı gereklidir. *Depolama hesabı kimlik bilgilerini alma* bölümünde bu değerlerin Azure portalından nasıl alınacağı açıklanmaktadır. Ancak, portalda hesap anahtarı "anahtar 1" olarak adlandırılmaktadır; bu nedenle Storage Explorer (Önizleme) bir hesap anahtarı istediğinde "anahtar 1" değerini girmeniz (veya yapıştırmanız) gerekir.

1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

   ![Azure Storage’a bağlanma seçeneği][23]
2. **Azure Storage’a Bağlan** iletişim kutusunda hesap anahtarını (Azure portalındaki "anahtar 1" değeri) belirtin ve ardından **İleri**’yi seçin.
> [!NOTE]
> Depolama bağlantısı dizesini ulusal Azure üzerindeki bir depolama hesabından girebilirsiniz. Örneğin, Azure Black Forest depolama hesaplarına bağlanmak için aşağıdakine benzer bağlantı dizeleri girin: DefaultEndpointsProtocol=https;AccountName=cawatest03;AccountKey=<storage_account_key>;EndpointSuffix=core.cloudapi.de; Bağlantı dizesini Azure portalından **Depolama hesabı kimlik bilgilerini alma** bölümünde anlatıldığı şekilde alabilirsiniz.

   ![Azure Storage’a bağlan iletişim kutusu][24]

3. **Harici Depolama Ekle** iletişim kutusunda **Hesap adı** kutusuna depolama hesabı adını girin, istediğiniz diğer ayarları belirtin ve işiniz bittiğinde **İleri**’yi seçin.

   ![Harici depolama ekle iletişim kutusu][8]
4. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.
5. Bağlandıktan sonra harici depolama hesabı depolama hesabı adına **(Harici)** metni eklenmiş olarak görüntülenir.

   ![Harici depolama hesabına bağlanma sonucu][9]

### <a name="detach-from-an-external-storage-account"></a>Harici bir depolama hesabı ayırma
1. Ayırmak istediğiniz harici depolama hesabına sağ tıklayın ve içerik menüsünden **Ayır**’ı seçin.

   ![Depolama alanından ayırma seçeneği][10]
2. Onaylama ileti kutusu göründüğünde harici depolama hesabından ayrılmayı onaylamak üzere **Evet**’i seçin.

## <a name="attach-storage-account-using-sas"></a>SAS kullanarak depolama hesabı ekleme
[SAS (Paylaşılan Erişim İmzası)](storage/storage-dotnet-shared-access-signature-part-1.md), Azure aboneliği yöneticilerine Azure abonelik kimlik bilgilerini girmeden geçici olarak bir depolama hesabına erişme olanağı sağlar.

Bunu açıklamak üzere; KullanıcıA’nın bir Azure aboneliği yöneticisi olduğunu düşünelim, ve KullanıcıA KullanıcıB’nin belirli bir süre çeşitli izinlerle bir depolama hesabına erişmesine izin vermek istiyor:

1. KullanıcıA belirli bir zaman dilimi ve istenilen izinlerle (depolama hesabı için bağlantı dizesinden oluşan) bir SAS oluşturuyor.
2. KullanıcıA SAS’ı örneğimizde depolama hesabına erişmek isteyen kişiyle, KullanıcıB ile paylaşıyor.  
3. KullanıcıB, sağlanan SAS’ı kullanarak KullanıcıA’ya ait hesabı eklemek için Depolama Gezgini’ni (Önizleme) kullanır.

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Paylaşmak istediğiniz hesap için bir SAS alma
1. Storage Explorer’da (Önizleme) paylaşmak istediğiniz depolama hesabına sağ tıklayın ve içerik menüsünden **Paylaşılan Erişim İmzası Al**’ı seçin.

   ![SAS alma içerik menüsü seçeneği][13]
2. **Paylaşılan Erişim İmzası** iletişim kutusunda hesap ile ilgili istediğiniz zaman dilimi ve izinleri seçin, ardından **Oluştur**’u seçin.

    ![SAS alma iletişim kutusu][14]
3. İkinci bir **Paylaşılan Erişim İmzası** iletişim kutusunda SAS gösterilir. Panoya kopyalamak için **Bağlantı Dizesi**’nin yanındaki **Kopyala**’yı seçin. İletişim kutusunu kapatmak için **Kapat**’ı seçin.

### <a name="attach-to-the-shared-account-using-the-sas"></a>SAS kullanarak paylaşılan hesaba ekleme
1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

   ![Azure Storage’a bağlanma seçeneği][23]
2. **Azure Storage’a Bağlan** iletişim kutusunda bağlantı dizesini belirtin ve ardından **İleri**’yi seçin.

   ![Azure Storage’a bağlan iletişim kutusu][24]
3. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.
4. Ekledikten sonra, depolama hesabı sağladığınız hesap adının sonuna (SAS) metni eklenmiş olarak görüntülenir.

   ![SAS kullanarak hesaba ekleme sonucu][17]

## <a name="attach-service-using-sas"></a>SAS kullanarak hizmet ekleme
[SAS kullanarak depolama hesabı ekleme](#attach-storage-account-using-sas) bölümünde Azure abonelik yöneticisinin depolama hesabı için bir SAS oluşturarak (ve paylaşarak) nasıl geçici erişim sağlayacağı açıklanır. Benzer şekilde bir depolama hesabının içinde belirli bir hizmet için (blob kapsayıcısı, kuyruk veya tablo) bir SAS oluşturulabilir.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Paylaşmak istediğiniz hizmet için bir SAS oluşturma
Bu bağlamda bir hizmet, bir blob kapsayıcısı, sıra veya tablo olabilir. Aşağıdaki bölümlerde listelenen hizmet için nasıl SAS oluşturulacağı açıklanmıştır:

* [Blob kapsayıcısı için SAS alma](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Dosya paylaşımı için SAS alma - *Çok yakında*
* Bir kuyruk için SAS alma - *Çok yakında*
* Bir tablo için SAS alma - *Çok yakında*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>SAS kullanarak paylaşılan hesap hizmetine ekleme
1. Storage Explorer’da (Önizleme) **Azure Storage’a bağlan**’ı seçin.

   ![Azure Storage’a bağlanma seçeneği][23]
2. **Azure Storage’a Bağlan** iletişim kutusunda SAS URI’sini belirtin ve ardından **İleri**’yi seçin.

   ![Azure Storage’a bağlan iletişim kutusu][24]
3. **Bağlantı Özeti** iletişim kutusundaki bilgileri doğrulayın. Herhangi bir ayarı değiştirmek isterseniz **Geri**’yi seçin ve istenilen ayarları yeniden girin. Tamamlandıktan sonra **Bağlan**’ı seçin.
4. Eklendikten sonra yeni eklenen hizmet **(Hizmet SAS’ı)** düğümü altında görüntülenir.

   ![SAS kullanarak paylaşılan hizmete ekleme sonucu][20]

## <a name="search-for-storage-accounts"></a>Depolama hesapları arama
Depolama hesaplarından oluşan uzun bir listeniz varsa, sol bölmenin üzerindeki arama kutusunu kullanmak belirli bir depolama hesabını bulmak için hızlı bir yöntemdir.

Arama kutusuna yazarken sol bölmede yalnızca o noktaya kadar girdiğiniz arama değeriyle eşleşen depolama hesapları görüntülenir. Aşağıdaki ekran görüntüsünde, aranan tüm depolama hesapları arasından “tarcher” metnini içeren depolama hesabı adlarının görüntülendiği bir örnek yer almaktadır.

![Depolama hesabı araması][11]

Aramayı temizlemek için arama kutusundaki **x** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini (Önizleme) ile Azure blob depolama kaynaklarını yönetme](vs-azure-tools-storage-explorer-blobs.md)

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

