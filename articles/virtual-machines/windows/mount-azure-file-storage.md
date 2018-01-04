---
title: "Bağlama Azure dosya depolama biriminden bir Azure Windows VM | Microsoft Docs"
description: "Azure dosya depolama ile bulutta dosya depolamak ve bir Azure sanal makineden (VM) bulut dosya paylaşımına bağlayın."
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 8d537bdc882487784baef9f693e4677c76d3bd8d
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure dosya paylaşımları Windows VM ile birlikte kullanmak 

Azure dosya paylaşımları, depolama ve sanal makineden dosyalara erişmek için bir yol olarak kullanabilirsiniz. Örneğin, bir komut dosyası veya paylaşmak için tüm VM'ler istediğiniz uygulama yapılandırma dosyası depolayabilirsiniz. Bu makalede, nasıl oluşturulacağı ve bir Azure dosya paylaşımını bağlama ve dosyaları yükleme ve indirme nasıl gösteriyoruz.

## <a name="connect-to-a-file-share-from-a-vm"></a>Bir sanal makineden bir dosya paylaşımına bağlanmak

Bu bölümde, bağlanmak istediğiniz bir dosya paylaşımı zaten olduğunu varsayar. Bir oluşturmanız gerekiyorsa, bkz: [bir dosya paylaşımı oluşturmak](#create-a-file-share) bu makalenin ilerisinde yer.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasında **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin veya **+ dosya paylaşımı** kullanmak için yeni bir dosya paylaşımı oluşturmak için.
6. Tıklatın **Bağlan** Windows veya Linux dosya paylaşımına bağlanması için komut satırı sözdizimi gösterilmektedir sayfasını açın.
7. İçinde **sürücü harfi**, sürücü tanımlamak için kullanmak istediğiniz harfi seçin.
8. Hangi sözdizimini kullanın ve sağındaki panonuza kopyalamak için Kopyala düğmesine seçmek için seçin. Kolayca erişebilirsiniz bir yere bir yere yapıştırın. 
8. VM'nize bağlanmak ve bir komut istemi açın.
9. Düzenlenen bağlantı sözdiziminde yapıştırın ve isabet **Enter**.
10. Bağlantı oluşturulduğunda, ileti almak **komutu başarıyla tamamlandı.**
11. Bu sürücüye geçin ve yazın için sürücü harfini yazarak bağlantıyı denetleyin **dir** dosya paylaşımı içeriğini görmek için.



## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma 
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasında **Hizmetleri**seçin **dosyaları**.
5. Dosya hizmeti sayfasında tıklatın **+ dosya paylaşımı**.
6. Dosya Paylaşımı adı girin. Dosya Paylaşımı adları küçük harf, rakam ve tek tire kullanabilirsiniz. Adı bir tire ile başlayamaz ve birden çok kullanamazsınız art arda kısa çizgi. 
7. Ne kadar büyük olabilir, en fazla 5120 GB üzerinde bir sınır doldurun.
8. Tıklatın **Tamam** dosya paylaşımı oluşturmak için.
   
## <a name="upload-files"></a>Dosyaları karşıya yükleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasında **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin.
6. Tıklatın **karşıya** açmak için **dosyaları karşıya yükleme** sayfası.
7. Yerel dosya sisteminizi karşıya yüklenecek dosyayı göz atmak için klasör simgesine tıklayın.   
8. Tıklatın **karşıya** dosya dosya paylaşımına karşıya.

## <a name="download-files"></a>Dosyaları indirme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasında **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin.
6. Dosyayı sağ tıklayın ve seçin **karşıdan** yerel makinenize indirmek için.
   

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, oluşturun ve PowerShell kullanarak dosya paylaşımlarını yönetin. Daha fazla bilgi için bkz: [Windows Azure File storage ile çalışmaya başlama](../../storage/files/storage-dotnet-how-to-use-files.md).
