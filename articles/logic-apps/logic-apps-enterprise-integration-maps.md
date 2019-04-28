---
title: XSLT eşlemeleri - Azure Logic Apps ile XML dönüştürme | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile XML'de dönüştürmek için XSLT eşlemeleri ekleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
manager: carmonm
ms.topic: article
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.date: 02/06/2019
ms.openlocfilehash: f6d778ddbce16c223945d4683bd7a950bd2a0cb0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61468015"
---
# <a name="transform-xml-with-maps-in-azure-logic-apps-with-enterprise-integration-pack"></a>Azure Logic Apps Enterprise Integration Pack ile maps'a ile XML dönüştürme

Azure Logic Apps Kurumsal tümleştirme senaryoları için biçimler arasında XML veri aktarmak için mantıksal uygulamanızı haritalar ya da daha açık belirtmek gerekirse Genişletilebilir Stil Sayfası Dil Dönüşümleri (XSLT) eşlemeleri kullanabilirsiniz. Bir harita verilerini bir XML belgesinden başka bir biçime dönüştürmek üzere açıklayan bir XML belgesidir. 

Örneğin, düzenli olarak B2B siparişlerini veya faturalarını YYYMMDD tarih biçimini kullanan bir müşteriden alma varsayalım. Ancak, kuruluşunuz MMDDYYY tarih biçimini kullanır. Tanımlayabilir ve sırası veya faturayı ayrıntıları müşteri etkinliği veritabanınızda depolamadan önce MMDDYYY biçimi YYYMMDD tarih biçimine dönüştürür bir haritayı kullanın.

Tümleştirme hesapları ve yapıtları eşlemeleri gibi ilgili limitleri için bkz. [limitler ve yapılandırma bilgilerini Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) depoladığınız etsek ve diğer yapıtlar için Kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümler.

* Haritanızda dış bütünleştirilmiş başvuruyorsa, karşıya yüklemeniz *hem derleme hem de eşleme* tümleştirme hesabınıza. Emin olun *derlemenizi önce karşıya*ve derlemeye başvuran harita yükleyin.

  Derlemenizi 2 MB veya daha küçük, derlemenizin tümleştirme hesabınıza ekleyebilirsiniz *doğrudan* Azure portalından. Ancak, derleme veya harita 2 MB değerinden büyük ancak daha büyük olup olmadığını [derlemeleri veya haritalar için boyut sınırı](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits), bu seçenekler vardır:

  * Derlemeler için bir Azure blob kapsayıcısı, derlemenizi ve, kapsayıcının konumunu burada karşıya yükleyebilirsiniz gerekir. Daha sonra tümleştirme hesabı için derleme eklediğinizde, bu şekilde, bu konuma sağlayabilir. 
  Bu görev için bu öğeler gerekir:

    | Öğe | Açıklama |
    |------|-------------|
    | [Azure depolama hesabı](../storage/common/storage-account-overview.md) | Bu hesapta, bir derleme için bir Azure blob kapsayıcısı oluşturun. Bilgi [bir depolama hesabının nasıl oluşturulacağını](../storage/common/storage-quickstart-create-account.md). |
    | BLOB kapsayıcısı | Bu kapsayıcıda derlemenizi yükleyebilirsiniz. Derleme, tümleştirme hesabına eklediğinizde bu kapsayıcının konumunu da gerekir. Bilgi edinmek için nasıl [bir blob kapsayıcısı oluşturursunuz](../storage/blobs/storage-quickstart-blobs-portal.md). |
    | [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) | Bu araç, daha fazla depolama hesaplarını yönetme ve blob kapsayıcıları kolayca yardımcı olur. Depolama Gezgini ya da kullanılacak [Azure Depolama Gezgini'ni indirip](https://www.storageexplorer.com/). Ardından, adımları izleyerek Depolama Gezgini'ni depolama hesabınıza bağlayın [Depolama Gezgini ile çalışmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md). Daha fazla bilgi için bkz: [hızlı başlangıç: Azure Depolama Gezgini ile nesne depolamada blob oluşturma](../storage/blobs/storage-quickstart-blobs-storage-explorer.md). <p>Veya, Azure portalında bulun ve depolama hesabınızı seçin. Depolama hesabı menüden **Depolama Gezgini**. |
    |||

  * Haritalar için şu anda daha büyük eşlemeleri kullanarak ekleyebileceğiniz [Azure Logic Apps REST API - eşler](https://docs.microsoft.com/rest/api/logic/maps/createorupdate).

Mantıksal uygulama oluştururken ve haritalar eklemek gerekmez. Ancak, bir harita kullanmak için mantıksal uygulamanızı eşlenen depoladığınız bir tümleştirme hesabı bağlama gerekir. Bilgi [tümleştirme hesapları için logic apps'i bağlama](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account). Mantıksal uygulama henüz sahip değilseniz, bilgi [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-referenced-assemblies"></a>Başvurulan derlemeleri ekleyin

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

1. Ana Azure menüsünden tümleştirme hesabınızı bulup seçin **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme hesabı" girin. 
   Seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Derlemenizi, örneğin eklemek istediğiniz tümleştirme hesabı seçin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **derlemeleri** Döşe.

   !["Derlemeleri" seçin](./media/logic-apps-enterprise-integration-maps/select-assemblies.png)

1. Sonra **derlemeleri** sayfasında açılır **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-maps/add-assembly.png)

Bütünleştirilmiş kod dosyanızın boyutuna bağlı olarak, ya da bir bütünleştirilmiş kod yükleme adımları [2 MB'a kadar](#smaller-assembly) veya [2 MB'tan fazla ancak yalnızca en çok 8 MB](#larger-assembly).
Tümleştirme hesabındaki bütünleştirilmiş kod miktarları üzerinde limitleri için bkz [limitler ve yapılandırma için Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits).

<a name="smaller-assembly"></a>

### <a name="add-assemblies-up-to-2-mb"></a>Derlemeleri ekleyin 2 MB'a kadar

1. Altında **derleme Ekle**, derlemenizin için bir ad girin. Tutun **küçük dosya** seçili. Yanındaki **derleme** kutusunda, klasör simgesini seçin. Bulup seçin, örneğin karşıya yüklemekte derleme:

   ![Daha küçük derleme karşıya yükleme](./media/logic-apps-enterprise-integration-maps/upload-assembly-file.png)

   İçinde **derleme adı** özelliği, derlemenin dosya adı görünür otomatik olarak derlemeyi seçin sonra.

1. Hazır olduğunuzda seçin **Tamam**.

   Derleme, derleme dosyasını karşıya yükleme tamamlandıktan sonra görünür **derlemeleri** listesi.

   ![Karşıya yüklenen derlemelerin listesi](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies-list.png)

   Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **derlemeleri** kutucuk artık sayısını gösterir karşıya yüklenen derlemeler, örneğin:

   ![Karşıya yüklenen derlemeler](./media/logic-apps-enterprise-integration-maps/uploaded-assemblies.png)

<a name="larger-assembly"></a>

### <a name="add-assemblies-more-than-2-mb"></a>2 MB'den fazla derlemeleri ekleyin

Daha büyük derlemeleri eklemek için Azure depolama hesabınızdaki bir Azure blob kapsayıcısına derlemenizi yükleyebilirsiniz. Derlemeleri eklemek için adımları farklıdır tabanlı blob kapsayıcınızın genel okuma erişimine sahip olup olmadığını. Bu nedenle ilk olarak, aşağıdaki adımları izleyerek blob kapsayıcınızın genel okuma erişimine sahip olup olmadığını denetleyin: [Blob kapsayıcısı genel erişim düzeyini ayarlayın](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcının erişim düzeyini denetleyin

1. Azure Depolama Gezgini'ni açın. Gezgini penceresinde, Azure aboneliğiniz zaten genişlettiyseniz genişletin.

1. Genişletin **depolama hesapları** > {*depolama hesabı your*} > **Blob kapsayıcıları**. Blob kapsayıcınızın seçin.

1. Blob kapsayıcının kısayol menüsünden seçin **genel erişim düzeyi ayarlamak**.

   * Blob kapsayıcınızın en az genel erişimi varsa, seçin **iptal**ve aşağıdaki adımları daha sonra bu sayfayı takip edin: [Genel erişim kapsayıcılarla yükleyin](#public-access-assemblies)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kapsayıcınızın genel erişime sahip değilse seçin **iptal**ve aşağıdaki adımları daha sonra bu sayfayı takip edin: [Kapsayıcı genel erişimini olmadan yükleme](#no-public-access-assemblies)

     ![Genel erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-assemblies"></a>

#### <a name="upload-to-containers-with-public-access"></a>Genel erişim kapsayıcılarla yükleyin

1. Derleme depolama hesabınıza yükleyin. 
   Sağ taraftaki penceresinde **karşıya**.

1. Karşıya yüklemeyi tamamladıktan sonra karşıya yüklenen derleme seçin. Araç çubuğunda **kopya URL** böylece derlemenin URL'yi kopyalayın.

1. Azure portalına dönün burada **derleme Ekle** bölmesini açın. 
   Derleme için bir ad girin. 
   Seçin **büyük dosya (2 MB'tan büyük)**.

   **İçerik URI** artık kutusu görüntülenirse, yerine **derleme** kutusu.

1. İçinde **içerik URI** kutusunda, bütünleştirilmiş kodun URL'yi yapıştırın. 
   Derlemenizi ekleme tamamlayın.

Şema karşıya yükleme, derleme tamamlandıktan sonra görünür **derlemeleri** listesi.
Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **derlemeleri** kutucuk artık karşıya yüklenen derlemelerin sayısını gösterir.

<a name="no-public-access-assemblies"></a>

#### <a name="upload-to-containers-without-public-access"></a>Kapsayıcı genel erişimini olmadan yükleme

1. Derleme depolama hesabınıza yükleyin. 
   Sağ taraftaki penceresinde **karşıya**.

1. Karşıya yüklemeyi tamamladıktan sonra derleme için paylaşılan erişim imzası (SAS) oluşturur. 
   Bütünleştirilmiş kodun kısayol menüsünden seçin **paylaşılan erişim imzası Al**.

1. İçinde **paylaşılan erişim imzası** bölmesinde **Oluştur kapsayıcı düzeyinde paylaşılan erişim imzası URI'si** > **Oluştur**. 
   SAS URL'si, yanındaki oluşturulan sonra **URL** kutusunda **kopyalama**.

1. Azure portalına dönün burada **derleme Ekle** bölmesini açın. 
   Derleme için bir ad girin. 
   Seçin **büyük dosya (2 MB'tan büyük)**.

   **İçerik URI** artık kutusu görüntülenirse, yerine **derleme** kutusu.

1. İçinde **içerik URI** kutusunda, daha önce oluşturulan SAS URI'sini yapıştırın. Derlemenizi ekleme tamamlayın.

Karşıya yükleme, derleme tamamlandıktan sonra derleme görünür **şemaları** listesi. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **derlemeleri** kutucuk artık karşıya yüklenen derlemelerin sayısını gösterir.

## <a name="create-maps"></a>MAPS'ı oluşturma

XSLT Belge Haritası olarak kullanabileceğiniz oluşturmak için Visual Studio 2015 kullanarak BizTalk tümleştirme projesi oluşturmak için kullanabilirsiniz [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md). Bu projede görsel öğeleri iki XML şema dosyaları arasında eşleme sağlayan bir tümleştirme haritası dosyası oluşturabilirsiniz. Bu projeyi derledikten sonra XSLT belge alın.
Tümleştirme hesabı eşlemesi miktarlar barındırabileceğiniz için bkz: [limitler ve yapılandırma için Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits). 

## <a name="add-maps"></a>Eşlemeler ekleme

Haritanızı başvurduğu tüm derlemeleri karşıya yüklenmesinin ardından, artık eşlemenizde karşıya yükleyebilirsiniz.

1. Zaten oturum açmadıysanız, oturum <a href="https://portal.azure.com" target="_blank">Azure portalında</a> Azure hesabı kimlik bilgilerinizle. 

1. Tümleştirme hesabı, ana Azure menüsünde açık değilse, seçin **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme hesabı" girin. 
   Seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-maps/find-integration-account.png)

1. Örneğin, harita eklemek istediğiniz tümleştirme hesabı seçin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-maps/select-integration-account.png)

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **haritalar** kutucuk.

   !["Haritalar" seçin](./media/logic-apps-enterprise-integration-maps/select-maps.png)

1. Sonra **haritalar** sayfasında açılır **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-maps/add-map.png)  

<a name="smaller-map"></a>

### <a name="add-maps-up-to-2-mb"></a>Eşlemeler ekleme 2 MB'a kadar

1. Altında **Eşlemesi Ekle**, haritanızı için bir ad girin. 

1. Altında **eşleme türü**, örneğin türünü seçin: **Liquid**, **XSLT**, **XSLT 2.0**, veya **XSLT 3.0**.

1. Tutun **küçük dosya** seçili. Yanındaki **harita** kutusunda, klasör simgesini seçin. Bulup seçin, örneğin karşıya yüklemekte eşlemesi:

   ![Harita karşıya yükleme](./media/logic-apps-enterprise-integration-maps/upload-map-file.png)

   Bırakılırsa **adı** özelliği boş, haritanın dosya adı otomatik olarak görünür özelliği otomatik olarak harita dosyasını seçtikten sonra. 
   Ancak, benzersiz bir ad kullanabilirsiniz.

1. Hazır olduğunuzda seçin **Tamam**. 
   Haritanın harita dosyanızı karşıya yükleme tamamlandıktan sonra görünür **haritalar** listesi.

   ![Karşıya yüklenen eşlemeleri listesi](./media/logic-apps-enterprise-integration-maps/uploaded-maps-list.png)

   Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **eşler** kutucuk artık sayısını gösterir karşıya yüklenen haritalar, örneğin:

   ![Karşıya yüklenen eşlemeleri](./media/logic-apps-enterprise-integration-maps/uploaded-maps.png)

<a name="larger-map"></a>

### <a name="add-maps-more-than-2-mb"></a>2 MB'den fazla eşlemeleri ekleme

Daha büyük eşlemeleri eklemek için şu anda kullanın [Azure Logic Apps REST API - eşler](https://docs.microsoft.com/rest/api/logic/maps/createorupdate).

<!--

To add larger maps, you can upload your map to 
an Azure blob container in your Azure storage account. 
Your steps for adding maps differ based whether your 
blob container has public read access. So first, check 
whether or not your blob container has public read 
access by following these steps: 
[Set public access level for blob container](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### Check container access level

1. Open Azure Storage Explorer. In the Explorer window, 
   expand your Azure subscription if not already expanded.

1. Expand **Storage Accounts** > {*your-storage-account*} > 
   **Blob Containers**. Select your blob container.

1. From your blob container's shortcut menu, 
   select **Set Public Access Level**.

   * If your blob container has at least public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers with public access](#public-access)

     ![Public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * If your blob container doesn't have public access, choose **Cancel**, 
   and follow these steps later on this page: 
   [Upload to containers without public access](#public-access)

     ![No public access](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access-maps"></a>

### Add maps to containers with public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**. 

1. After you finish uploading, select your 
   uploaded map. On the toolbar, choose **Copy URL** 
   so that you copy the map's URL.

1. Return to the Azure portal where the 
   **Add Map** pane is open. Choose **Large file**. 

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste your map's URL. 
   Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

<a name="no-public-access-maps"></a>

### Add maps to containers with no public access

1. Upload the map to your storage account. 
   In the right-hand window, choose **Upload**.

1. After you finish uploading, generate a 
   shared access signature (SAS) for your schema. 
   From your map's shortcut menu, 
   select **Get Shared Access Signature**.

1. In the **Shared Access Signature** pane, select 
   **Generate container-level shared access signature URI** > **Create**. 
   After the SAS URL gets generated, next to the **URL** box, choose **Copy**.

1. Return to the Azure portal where the 
   **Add Maps** pane is open. Choose **Large file**.

   The **Content URI** box now appears, 
   rather than the **Map** box.

1. In the **Content URI** box, paste the SAS URI 
   you previously generated. Finish adding your map.

After your map finishes uploading, 
the map appears in the **Maps** list.

-->

## <a name="edit-maps"></a>Haritalar Düzenle

Mevcut bir haritayı güncelleştirmek için istediğiniz değişiklikleri içeren yeni bir eşleme dosyasını karşıya yüklemek sahip. Ancak, ilk düzenlemek için varolan harita indirebilirsiniz.

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, bulun ve açın, tümleştirme hesabı, açık değilse.

1. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "tümleştirme hesabı" girin. Seçin **tümleştirme hesapları**.

1. Haritanızı güncelleştirmek istediğiniz tümleştirme hesabı'nı seçin.

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **haritalar** kutucuk.

1. Sonra **haritalar** haritanızı, sayfası açılır. 
   İndirin ve harita ilk düzenlemek için seçin **indirme**ve harita kaydedin.

1. Güncelleştirilmiş harita üzerinde yüklemeye hazır olduğunuzda **haritalar** sayfasında, güncelleştirmek ve istediğiniz harita **güncelleştirme**.

1. Bulun ve karşıya yüklemek istediğiniz güncelleştirilmiş harita seçin. 
   Harita dosyanızı karşıya yükleme tamamlandıktan sonra güncelleştirilmiş harita görünür **haritalar** listesi.

## <a name="delete-maps"></a>Haritalar Sil

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, bulun ve açın, tümleştirme hesabı, açık değilse.

1. Ana Azure menüsünde **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme hesabı" girin. 
   Seçin **tümleştirme hesapları**.

1. Eşlemeyi silmek istediğiniz tümleştirme hesabı'nı seçin.

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **haritalar** kutucuk.

1. Sonra **haritalar** seçin sayfası açılır ve haritanızı **Sil**.

1. Eşlemeyi silmek istediğinizi onaylamak için seçin **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md)  
* [Şemaları hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-schemas.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)