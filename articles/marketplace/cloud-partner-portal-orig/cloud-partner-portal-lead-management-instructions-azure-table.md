---
title: Azure tablo | Microsoft Docs
description: Azure tablosu için sağlama yönetimi yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 2a8ae3ab71b258d92d9761cc813b168717e44d82
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61066632"
---
# <a name="lead-management-instructions-for-azure-table"></a>Azure tablosu için yönetim yönergeleri sağlama

Bu makalede, Azure tablo satışa depolamak için yapılandırma açıklanır. Azure tablo depolamak ve müşteri bilgileri özelleştirmenize olanak sağlar.

## <a name="to-configure-azure-table"></a>Azure tablo yapılandırmak için

1.  Bir Azure hesabınız yoksa, şunları yapabilirsiniz [ücretsiz bir deneme hesabı oluşturma](https://azure.microsoft.com/pricing/free-trial/).

2.  Azure hesabınız etkinleştikten sonra oturum [Azure portalında](https://portal.azure.com).
3.  Azure portalında bir depolama hesabı oluşturun. Sonraki ekran yakalama, bir depolama hesabı oluşturma işlemi gösterilmektedir. Depolama fiyatlandırması hakkında daha fazla bilgi için bkz. [depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

    ![Bir Azure depolama hesabı oluşturma adımları](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragecreate.png)

4.  Depolama hesabı bağlantı dizesi anahtarı kopyalayın ve yapıştırın **depolama hesabı bağlantı dizesi** bulut iş ortağı portalı ile sekmesindeki. Bir bağlantı dizesi örneğidir `DefaultEndpointsProtocol=https;AccountName=myAccountName;AccountKey=myAccountKey;EndpointSuffix=core.windows.net`
    
    ![Azure depolama anahtarı](./media/cloud-partner-portal-lead-management-instructions-azure-table/azurestoragekeys.png)

Kullanabileceğiniz [Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/) veya depolama Tablonuzdaki verileri görmek için başka bir araç. Ayrıca, Azure tablo verileri dışarı aktarabilirsiniz.
veriler.

## <a name="optional-use-microsoft-flow-with-an-azure-table"></a>**(İsteğe bağlı)**  Microsoft Flow kullanımı ile bir Azure tablosu

Kullanabileceğiniz [Microsoft Flow](https://docs.microsoft.com/flow/) Azure tablosunda her bir müşteri adayı eklendiğinde bildirim otomatik hale getirmek için. Aksi takdirde bir hesabınız yoksa, şunları yapabilirsiniz [ücretsiz bir hesap için kaydolun](https://flow.microsoft.com/).

### <a name="lead-notification-example"></a>Bildirim örneği sağlama

Bu örnek, bir Azure tablosu için yeni bir müşteri adayı eklendiğinde, otomatik olarak bir e-posta bildirimi gönderen basit bir akış oluşturmak için bir kılavuz olarak kullanın. Bu örnek, tablo depolama güncelleştirildiyse, müşteri adayı bilgilerini saatte göndermek için bir yineleme ayarlar.

1. Microsoft Flow hesabınızda oturum açın.
2. Sol gezinti çubuğunda **Akışlarım**.
3. Üst gezinti çubuğunda, seçin **+ yeni**.  
4. Aşağı açılan listesinde seçin **+ boş akış oluştur**
5. Boş akış Oluştur altında seçin **boş akış Oluştur**.

   ![Yeni bir akışı sıfırdan oluşturma](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-create-from-blank.png)

6. Bağlayıcılar ve tetikleyiciler arama sayfası seçin **Tetikleyicileri**.
7. Altında **Tetikleyicileri**seçin **yinelenme**.
8. İçinde **yinelenme** penceresinde varsayılan ayar 1 tutmak **aralığı**. Gelen **sıklığı** açılan listesinden **saat**.

   >[!NOTE] 
   >Bu örnek bir 1 saatlik zaman aralığında kullansa da, aralık ve, iş ihtiyaçlarınız için en iyi şekilde sıklığı seçebilirsiniz.

   ![Yineleme için 1 saat sıklığını ayarlayın](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-recurrence-dropdown.png)

9. Seçin **+ yeni adım**.
10. "Get geçmiş zamanı" için arama yapın ve ardından **geçmişteki saati Al** Eylemler altında. 

    ![Bulun ve seçin, eylem saati Al](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-search-getpasttime.png)

11. İçinde **geçmişteki saati Al** penceresinde **aralığı** 1.  Gelen **zaman birimi** açılan listesinden **saat**.
    >[!IMPORTANT] 
    >Bu aralık ve zaman birimi yineleme için yapılandırılan sıklık ve aralığı eşleştiğinden emin olun.

    ![Son zaman aralığı ayarla al](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getpast-time.png)

    >[!TIP] 
    >Akışınız her adımın doğru yapılandırıldığını doğrulamak için istediğiniz zaman kontrol edebilirsiniz. Akışınız denetlemek için **akış denetleyicisi** akış menü çubuğundan.

Sonraki adım kümesini, Azure tablonuza bağlanırsınız ve yeni işlemek için işleme mantığının ayarlama yol açar.

1. Adım Süresi geçmiş Get sonra seçin **+ yeni adım**ve ardından "Get varlıklar için" arayın.
2. Altında **eylemleri**seçin **alma varlıkları**ve ardından **Gelişmiş Seçenekleri Göster**.
3. İçinde **alma varlıkları** penceresinde, aşağıdaki alanlar için bilgileri sağlayın:

   - **Tablo** – Azure tablo Depolama'nızda adını girin. Bu örnekte "MarketPlaceLeads" girildiğinde sonraki ekran görüntüsü yakalamayı istemi gösterir. 

     ![Azure tablo adı için özel bir değer seçin](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-table-name.png)

   - **Filtre sorgusu** : Bu alana tıklayın ve açılan pencerede Get geçmiş zaman simgesi görüntülenir. Seçin **süresini geçen** bu sorguya filtre uygulamak için zaman damgası kullanmak için. Alternatif olarak, bu işlev alanına yapıştırabilirsiniz: `gt datetime'@{body('Get_past_time')}'`

     ![Filtre sorgu işlevi ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-getentities-filterquery.png)

4. Seçin **yeni adım** yeni müşteri adayları için Azure tablo taramak için bir koşul eklemek için.

   ![Azure tablo taramak için bir koşul eklemek için yeni adımı kullanın](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-add-filterquery-new-step.png)

5. İçinde **eylem seçin** penceresinde **eylemleri**ve ardından **koşul** denetimi.

     ![Koşul denetimi ekleme](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-action-condition-control.png)

6. İçinde **koşul** penceresinde **bir değer seçin** alan ve ardından **ifade** açılan penceresinde.
7. Yapıştırma `length(body('Get_entities')?['value'])` içine ***fx*** alan. Seçin **Tamam** bu işlevi eklemek için. Koşul ayarlamayı tamamlamak için:

   - "Açılır listeden büyüktür" seçin.
   - Değeri olarak 0 girin 

     ![Bir işlev koşula Ekle](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-fx0.png)

8. Koşulun sonucuna göre gerçekleştirilecek eylem ayarlama.

     ![Eylem koşulu sonuçlarına göre ayarlama](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-condition-pick-action.png)

9. Koşul halinde **hiçbir**, hiçbir şey yapmaz. 
10. Koşul halinde **Evet ise**, e-posta göndermek için Office 365 hesabınıza bağlayan eylem tetikler. Seçin **Eylem Ekle**.
11. Seçin **bir e-posta**. 
12. İçinde **bir e-posta** penceresinde, aşağıdaki alanlar için bilgileri sağlayın:

    - **İçin** -bu bildirim alacak herkes için bir e-posta adresi girin.
    - **Konu** – e-posta için bir konu belirtin. Örneğin: Yeni müşteri adayları!
    - **Gövde**:   Her e-posta (isteğe bağlı) içerir ve gövdesine yapıştırın istediğiniz metni Ekle `('Get_entities')?['value']` müşteri adayı bilgilerini eklemek için bir işlev olarak.

      >[!NOTE] 
      >Bu e-posta gövdesi için ek bir statik veya dinamik veri noktaları ekleyebilirsiniz.

       ![Sağlama bildirim e-posta](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-emailbody-fx.png)

13. Seçin **Kaydet** akışı kaydedin. Microsoft Flow, flow hataları için otomatik olarak test eder. Herhangi bir hata yoksa, akışınız kaydedildikten sonra çalışmaya başlar.

Sonraki ekran yakalama örneği son akışı nasıl görünmelidir gösterir.

 ![Son akış sırası](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-end-to-end.png)

### <a name="managing-your-flow"></a>Akışınız yönetme

Çalışmaya başladıktan sonra akışınızı yönetmek kolaydır.  Akışınızı üzerinde tam denetime sahip. Örneğin, durdurmak, düzenleme, çalıştırma geçmişini görebilir ve analizler elde. Sonraki ekran görüntüsü yakalamayı akış yönetmek kullanılabilir seçenekleri gösterir. 

 ![Bir akışı yönetme](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-manage-completed.png)

Akışı kullanarak durduruncaya kadar çalışmaya devam eder **kapatmak akış** seçeneği.

Herhangi bir müşteri adayı e-posta bildirim almıyorsanız, Azure tablosu için yeni müşteri adayları eklemediyseniz, anlamına gelir. Akış hatalarını varsa, sonraki ekran görüntüsünde örnekte olduğu gibi bir e-posta alırsınız.

 ![Akış hatası e-posta bildirimi](./media/cloud-partner-portal-lead-management-instructions-azure-table/msflow-failure-note.png)

## <a name="next-steps"></a>Sonraki adımlar

[Müşteri adaylarını yapılandırma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads)