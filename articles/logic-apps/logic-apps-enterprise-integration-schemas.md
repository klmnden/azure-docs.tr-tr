---
title: XML şemaları - Azure Logic Apps ile doğrulama | Microsoft Docs
description: Azure Logic Apps Enterprise Integration Pack ile XML belgeleri doğrulamak için şemalar ekleme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.date: 02/06/2019
ms.openlocfilehash: 3cca995b353b88cc481cbda68df4211a724f7f09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60846387"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>XML şemaları Azure Logic Apps Enterprise Integration Pack ile doğrulama

Belge geçerli XML kullanın ve Azure Logic Apps Kurumsal tümleştirme senaryoları için önceden tanımlanmış biçiminde beklenen verileri sahip denetlemek için mantıksal uygulamanızı şemaları kullanabilirsiniz. Bir şema işletmeden işletmeye (B2B) senaryolarını logic apps exchange iletileri de doğrulayabilirsiniz.

Tümleştirme hesapları ve yapıtları şemalar gibi ilgili limitleri için bkz. [limitler ve yapılandırma bilgilerini Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) depoladığınız, şemalar ve diğer yapıtlar için Kurumsal tümleştirme ve işletmeden işletmeye (B2B) çözümler. 

  Şemanızı ise [2 MB veya daha küçük](#smaller-schema), doğrudan Azure portalından tümleştirme hesabınıza şemanızı ekleyebilirsiniz. Ancak, şemanızı 2 MB değerinden büyük ancak daha büyük ise [şema boyut sınırını](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits), bir Azure depolama hesabına şemanızı karşıya yükleyebilirsiniz. 
  Bu şema tümleştirme hesabınıza eklemek için ardından tümleştirme hesabınızdan depolama hesabınıza bağlayabilirsiniz. 
  Bu görev için gereksinim duyduğunuz öğeleri şunlardır: 

  * [Azure depolama hesabı](../storage/common/storage-account-overview.md) oluşturacağınız bir blob kapsayıcısı için şema. Bilgi edinmek için nasıl [depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md). 

  * Şemanızı depolamak için blob kapsayıcısı. Bilgi edinmek için nasıl [bir blob kapsayıcısı oluşturursunuz](../storage/blobs/storage-quickstart-blobs-portal.md). 
  Şemayı tümleştirme hesabınıza eklediğinizde, kapsayıcının içerik URI daha sonra gerekir.

  * [Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), depolama hesaplarını yönetmek için kullanın ve blob kapsayıcıları. 
  Depolama Gezgini'ni kullanmak için burada iki seçenekten birini seçin:
  
    * Azure portalında bulun ve depolama hesabınızı seçin. 
    Depolama hesabı menüden **Depolama Gezgini**.

    * Masaüstü sürümü için [Azure Depolama Gezgini'ni indirip](https://www.storageexplorer.com/). 
    Ardından, adımları izleyerek Depolama Gezgini'ni depolama hesabınıza bağlayın [Depolama Gezgini ile çalışmaya başlama](../vs-azure-tools-storage-manage-with-storage-explorer.md). 
    Daha fazla bilgi için bkz: [hızlı başlangıç: Azure Depolama Gezgini ile nesne depolamada blob oluşturma](../storage/blobs/storage-quickstart-blobs-storage-explorer.md).

Mantıksal uygulama oluşturma ve ekleme şemaları gerekmez. Ancak, bir şema kullanmak için mantıksal uygulamanız bu şema depoladığınız bir tümleştirme hesabı bağlama gerekir. Bilgi [tümleştirme hesapları için logic apps'i bağlama](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account). Mantıksal uygulama henüz sahip değilseniz, bilgi [mantıksal uygulamalar oluşturmak nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-schemas"></a>Şemalar ekleme

1. Azure hesabınızın kimlik bilgileriyle <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oturum açın.

1. Ana Azure menüsünden tümleştirme hesabınızı bulup seçin **tüm hizmetleri**. Arama kutusuna "tümleştirme hesabı" girin. Seçin **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-schemas/find-integration-account.png)

1. Örneğin, şemanızı eklemek istediğiniz tümleştirme hesabı seçin:

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-schemas/select-integration-account.png)

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **şemaları** Döşe.

   !["Şemaları" seçin](./media/logic-apps-enterprise-integration-schemas/select-schemas.png)

1. Sonra **şemaları** sayfasında açılır **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-schemas/add-schema.png)

Şema (.xsd) dosyanızın boyutuna bağlı olarak, herhangi bir şema karşıya yükleme adımlarını izleyin [2 MB'a kadar](#smaller-schema) veya [2 MB'tan fazla en fazla 8 MB](#larger-schema).

<a name="smaller-schema"></a>

### <a name="add-schemas-up-to-2-mb"></a>Şemalar ekleme 2 MB'a kadar

1. Altında **Şema Ekle**, şemanızı için bir ad girin. 
   Tutun **küçük dosya** seçili. Yanındaki **şema** kutusunda, klasör simgesini seçin. Bulup seçin, örneğin karşıya yüklemekte şema:

   ![Daha küçük bir şema yükleyin](./media/logic-apps-enterprise-integration-schemas/upload-smaller-schema-file.png)

1. Hazır olduğunuzda seçin **Tamam**.

   Şema şemanızı karşıya yükleme tamamlandıktan sonra görünür **şemaları** listesi.

<a name="larger-schema"></a>

### <a name="add-schemas-more-than-2-mb"></a>2 MB'den fazla şemalar ekleme

Daha büyük şemaları eklemek için Azure depolama hesabınızdaki bir Azure blob kapsayıcısına şemanızı yükleyebilirsiniz. Blob kapsayıcınızın genel okuma erişimine sahip olup olmadığını şemalar ekleme adımları uygulamanıza bağlı olarak farklılık gösterir. Bu nedenle ilk olarak, aşağıdaki adımları izleyerek blob kapsayıcınızın genel okuma erişimine sahip olup olmadığını denetleyin: [Blob kapsayıcısı genel erişim düzeyini ayarlayın](../vs-azure-tools-storage-explorer-blobs.md#set-the-public-access-level-for-a-blob-container)

#### <a name="check-container-access-level"></a>Kapsayıcının erişim düzeyini denetleyin

1. Azure Depolama Gezgini'ni açın. Gezgini penceresinde, Azure aboneliğiniz zaten genişlettiyseniz genişletin.

1. Genişletin **depolama hesapları** > {*depolama hesabı your*} > **Blob kapsayıcıları**. Blob kapsayıcınızın seçin.

1. Blob kapsayıcının kısayol menüsünden seçin **genel erişim düzeyi ayarlamak**.

   * Blob kapsayıcınızın en az genel erişimi varsa, seçin **iptal**ve aşağıdaki adımları daha sonra bu sayfayı takip edin: [Genel erişim kapsayıcılarla yükleyin](#public-access)

     ![Genel erişim](media/logic-apps-enterprise-integration-schemas/azure-blob-container-public-access.png)

   * Blob kapsayıcınızın genel erişime sahip değilse seçin **iptal**ve aşağıdaki adımları daha sonra bu sayfayı takip edin: [Kapsayıcı genel erişimini olmadan yükleme](#public-access)

     ![Genel erişim yok](media/logic-apps-enterprise-integration-schemas/azure-blob-container-no-public-access.png)

<a name="public-access"></a>

#### <a name="upload-to-containers-with-public-access"></a>Genel erişim kapsayıcılarla yükleyin

1. Şema, depolama hesabınıza yükleyin. 
   Sağ taraftaki penceresinde **karşıya**.

1. Karşıya yüklemeyi tamamladıktan sonra karşıya yüklenen şemanızı seçin. Araç çubuğunda **kopya URL** böylece şema URL'yi kopyalayın.

1. Azure portalına dönün burada **Şema Ekle** bölmesini açın. 
   Derleme için bir ad girin. 
   Seçin **büyük dosya (2 MB'tan büyük)** . 

   **İçerik URI** artık kutusu görüntülenirse, yerine **şema** kutusu.

1. İçinde **içerik URI** kutusunda, şemanızı ait URL'yi yapıştırın. 
   Şemanızı ekleme tamamlayın.

Şema şemanızı karşıya yükleme tamamlandıktan sonra görünür **şemaları** listesi. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **şemaları** kutucuk artık karşıya yüklenen şemaları sayısını gösterir.

<a name="no-public-access"></a>

#### <a name="upload-to-containers-without-public-access"></a>Kapsayıcı genel erişimini olmadan yükleme

1. Şema, depolama hesabınıza yükleyin. 
   Sağ taraftaki penceresinde **karşıya**.

1. Karşıya yüklemeyi tamamladıktan sonra şema için paylaşılan erişim imzası (SAS) oluşturur. 
   Şema kısayol menüsünden seçin **paylaşılan erişim imzası Al**.

1. İçinde **paylaşılan erişim imzası** bölmesinde **Oluştur kapsayıcı düzeyinde paylaşılan erişim imzası URI'si** > **Oluştur**. 
   SAS URL'si, yanındaki oluşturulan sonra **URL** kutusunda **kopyalama**.

1. Azure portalına dönün burada **Şema Ekle** bölmesini açın. Seçin **büyük dosya**.

   **İçerik URI** artık kutusu görüntülenirse, yerine **şema** kutusu.

1. İçinde **içerik URI** kutusunda, daha önce oluşturulan SAS URI'sini yapıştırın. Şemanızı ekleme tamamlayın.

Şema şemanızı karşıya yükleme tamamlandıktan sonra görünür **şemaları** listesi. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**, **şemaları** kutucuk artık karşıya yüklenen şemaları sayısını gösterir.

## <a name="edit-schemas"></a>Şemaları Düzenle

Var olan bir şema güncelleştirmek için istediğiniz değişiklikleri içeren yeni bir şema dosyası karşıya yüklemek sahip. Ancak, ilk düzenlemek için varolan şema indirebilirsiniz.

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, bulun ve açın, tümleştirme hesabı, açık değilse.

1. Ana Azure menüsünde **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme hesabı" girin. 
   Seçin **tümleştirme hesapları**.

1. Şemanızı güncelleştirmek istediğiniz tümleştirme hesabı'nı seçin.

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **şemaları** Döşe.

1. Sonra **şemaları** sayfası açıldıktan sonra şema seçin. 
   İndirin ve şemayı ilk düzenlemek için seçin **indirme**ve şema kaydedin.

1. Güncelleştirilmiş bir şema üzerinde yüklemeye hazır olduğunuzda **şemaları** sayfasında, güncelleştirmek ve istediğiniz şema **güncelleştirme**.

1. Bulun ve karşıya yüklemek istediğiniz güncelleştirilmiş şema seçin. 
   Şema dosyanızı karşıya yükleme tamamlandıktan sonra güncelleştirilmiş şema görünür **şemaları** listesi.

## <a name="delete-schemas"></a>Şemaları Sil

1. İçinde <a href="https://portal.azure.com" target="_blank">Azure portalında</a>, bulun ve açın, tümleştirme hesabı, açık değilse.

1. Ana Azure menüsünde **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme hesabı" girin. 
   Seçin **tümleştirme hesapları**.

1. Şemanızı silmek istediğiniz tümleştirme hesabı'nı seçin.

1. Tümleştirme hesabının üzerinde **genel bakış** sayfasındaki **bileşenleri**seçin **şemaları** Döşe.

1. Sonra **şemaları** seçin sayfası açılır ve şemanızı seçin **Sil**.

1. Şemayı silmek istediğinizi onaylamak için seçin **Evet**.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
* [Eşlemeleri hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md)
* [Dönüşümler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-transform.md)
