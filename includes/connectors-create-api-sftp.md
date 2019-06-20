---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: d249a205c64f4e067f2d81c7e1068c8ad9756958
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67202759"
---
### <a name="prerequisites"></a>Önkoşullar
* Bir [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) hesabı  

SFTP hesabınıza bağlanmak için mantıksal uygulama, bir mantıksal uygulama çalıştırmasında SFTP hesabınıza kullanabilmeniz için önce yetkilendirmeniz gerekir. Neyse ki, Azure Portalı'nda mantıksal uygulama içinde bunu kolayca yapabilirsiniz.  

Mantıksal uygulamanızı SFTP hesabınıza bağlanmak için yetki vermek için adımlar şunlardır:  

1. Mantıksal Uygulama Tasarımcısı'nda, SFTP bağlantı oluşturmak için seçin **yönetilen API'leri Göster Microsoft** açılan liste enter *SFTP* arama kutusuna. Seçin **SFTP - bir dosya eklendiğinde veya değiştirildiğinde** tetikleyici:  
   ![SFTP bağlantı görüntü 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. SFTP önce herhangi bir bağlantı oluşturmadıysanız, SFTP kimlik bilgilerinizi sağlamanız girmem isteniyor. Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak için yetki vermek için kullanılan ve SFTP hesabınıza ait veri erişimi:  
   ![SFTP bağlantı resim 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Bağlantı oluşturuldu ve artık mantıksal uygulamanızdaki diğer adımlarla devam etmek ücretsiz, dikkat edin:   
   ![SFTP bağlantı görüntüsü 3](./media/connectors-create-api-sftp/sftp-3.png) 

