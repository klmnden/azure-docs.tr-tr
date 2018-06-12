---
title: FTP sunucusu - Azure Logic Apps bağlanma | Microsoft Docs
description: Oluştur, izlemek ve Azure Logic Apps ile bir FTP sunucusunda dosyalarını yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/22/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 983e8f84e6e44bc9e5de5f4e7fff361b92b316c9
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295702"
---
# <a name="get-started-with-the-ftp-connector"></a>FTP Bağlayıcısı ile çalışmaya başlama
İzleme, yönetme ve FTP sunucusunda bulunan dosyaları oluşturmak için FTP Bağlayıcısı'nı kullanın. 

Kullanılacak [tüm bağlayıcıların](apis-list.md), ilk mantıksal uygulama oluşturmanız gerekir. Tarafından başlayabiliriz [şimdi mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-ftp"></a>FTP Bağlan
Mantıksal uygulamanızı herhangi bir hizmet erişebilmeniz için önce ilk önce oluşturmanız gerekir bir *bağlantı* hizmet. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

### <a name="create-a-connection-to-ftp"></a>FTP bağlantı oluşturun.
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>FTP tetikleyici kullanın
Bir tetikleyici bir mantıksal uygulama tanımlı iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!IMPORTANT]
> Internet'ten erişilebilen ve Pasif modu ile çalışmak için yapılandırılmış bir FTP sunucusu FTP Bağlayıcısı gerektirir. Ayrıca, FTP Bağlayıcıdır **örtük FTPS (FTP SSL üzerinden) ile uyumlu değil**. FTP Bağlayıcısı, yalnızca açık FTPS (FTP SSL üzerinden) destekler.  
> 
> 

Bu örnekte, ı size nasıl kullanılacağını gösterir **bir dosya eklendiğinde veya FTP -** bir dosya için eklendiğinde veya, bir FTP sunucusuna değiştiren bir mantık uygulama iş akışını başlatmak için tetikler. Bir kurumsal örnekte, bu tetikleyici müşterilerden siparişleri temsil eden yeni dosyalar için bir FTP klasörü izlemek için kullanabilirsiniz.  Bir FTP Bağlayıcısı eylem gibi sonra kullanabilir **alma dosya içeriğini** siparişleri veritabanınızda başka bir işleme ve depolama düzeni içeriğini almak için.

1. Girin *ftp* arama kutusuna logic apps tasarımcısında seçip **bir dosya eklendiğinde veya FTP -** tetikleyici   
   ![FTP tetikleyici görüntü 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   **Ne zaman bir dosya eklenen veya değiştirilen** denetim açılır  
   ![FTP tetikleyici görüntü 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Seçin **...**  denetimi sağ tarafta bulunur. Bu Klasör Seçici denetim açar  
   ![FTP tetikleyici görüntü 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Seçin **>** (sağ ok) için yeni veya değiştirilmiş dosyaları izlemek istediğiniz klasörü bulmak üzere göz atın. Klasör ve klasör gösterilen artık bildirim seçin **klasörü** denetim.  
   ![FTP tetikleyici görüntü 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Bu noktada, mantıksal uygulamanızı bir dosya değiştiren veya belirli FTP klasörde oluşturulan diğer tetikleyiciler ve Eylemler iş akışı bir farklı çalıştır başlayacak bir tetikleyici ile yapılandırıldı. 

> [!NOTE]
> Bir mantıksal uygulama işlevsel olması en az bir tetikleyici ve bir eylem içermelidir. Bir eylem eklemek için sonraki bölümdeki adımları izleyin.  
> 
> 

## <a name="use-a-ftp-action"></a>Bir FTP eylemi kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Eklediğiniz bir tetikleyici, tetikleyici tarafından bulunan yeni veya değiştirilmiş dosya içeriğini alacak bir eylem eklemek için aşağıdaki adımları izleyin.    

1. Seçin **+ yeni adım** eklemek için FTP sunucusunda dosyasının içeriğini almak için eylem  
2. Seçin **Eylem Ekle** bağlantı.  
   ![FTP eylem görüntü 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Girin *FTP* FTP ilgili tüm eylemleri arayın.
4. Seçin **FTP - Al dosya içeriğini** ne zaman gerçekleştirilecek eylemi yeni veya değiştirilmiş bir dosya FTP klasöründe bulunur.      
   ![FTP eylem görüntü 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   **Alma dosya içeriği** kontrol açar. **Not**: mantıksal uygulamanızı, bunu daha önceden yapmadıysanız, FTP sunucusu hesabınıza erişmeniz için yetkilendirmek istenir.  
   ![FTP eylem görüntüsü 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Seçin **dosya** denetimi (boşluk bulunan aşağıda ** dosya ***). Burada FTP sunucusunda bulunan yeni veya değiştirilmiş dosyasından çeşitli özelliklerinden herhangi birini kullanabilirsiniz.  
6. Seçin **dosya içeriği** seçeneği.  
   ![FTP eylem görüntüsü 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Gösteren denetimi güncelleştirilir **FTP - Al dosya içeriğini** eylem alırsınız *dosya içeriği* FTP sunucusunda yeni veya değiştirilmiş dosyasının.      
   ![FTP eylem görüntüsü 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Çalışmanızı kaydedin, sonra iş akışınızı test etmek için FTP klasörüne bir dosya ekleyin.    

Bu noktada, mantıksal uygulama, bir FTP sunucusundaki bir klasöre izlemek ve yeni bir dosya veya FTP sunucusunda değiştirilmiş bir dosya bulduğunda, iş akışını başlatmak için bir tetikleyici ile yapılandırıldı. 

Mantıksal uygulama de yeni veya değiştirilmiş dosyasının içeriğini almak için bir eylem ile yapılandırıldı.

Başka bir eylem gibi şimdi eklemek [SQL Server - Satır Ekle](connectors-create-api-sqlazure.md) yeni veya değiştirilmiş dosya içeriğini bir SQL veritabanı tablosuna eklemek için eylem.  

## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Sonraki Adımlar
[Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

