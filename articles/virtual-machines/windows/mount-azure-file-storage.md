---
title: Bağlama Azure dosya depolama'yı Azure Windows VM | Microsoft Docs
description: Azure dosya depolama ile bulutta dosya Store ve bir Azure sanal makineden (VM), bulut dosya paylaşımı bağlayın.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.component: files
ms.openlocfilehash: a2ddaa28f7fbcc6b363ce2c636521fe54ea752f7
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54478187"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure dosya paylaşımları ile Windows Vm'leri kullanın 

Azure dosya paylaşımları, VM'den dosyalara erişmek ve depolamak için bir yöntem olarak kullanabilirsiniz. Örneğin, bir komut dosyası veya paylaşmak için tüm Vm'leri istediğiniz uygulama yapılandırma dosyası depolayabilirsiniz. Bu makalede, oluşturma ve bir Azure dosya paylaşımını bağlama ve dosyaları yükleme ve indirme işlemini gösteriyoruz.

## <a name="connect-to-a-file-share-from-a-vm"></a>Bir VM'den bir dosya paylaşımına bağlanma

Bu bölümde, bağlanmak istediğiniz bir dosya paylaşımı zaten sahip olduğunuz varsayılır. Oluşturmak ihtiyacınız varsa bkz [dosya paylaşımı oluşturma](#create-a-file-share) bu makalenin ilerleyen bölümlerinde.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasındaki **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin veya **+ dosya paylaşımı** kullanmak için yeni bir dosya paylaşımı oluşturmak için.
6. Tıklayın **Connect** Windows veya Linux dosya paylaşımını bağlama için komut satırı sözdizimi gösteren bir sayfasını açın.
7. İçinde **sürücü harfi**, sürücü tanımlamak için kullanmak istediğiniz harfi seçin.
8. Panonuza kopyalamak için sağ taraftaki Kopyala düğmesini kullanın ve hangi söz dizimi seçin. Bazı yerde kolayca erişebilirsiniz bir yere yapıştırın. 
8. VM'nize bağlanmak ve bir komut istemi açın.
9. Düzenlenen bağlantı sözdiziminde yapıştırın ve isabet **Enter**.
10. Bağlantı oluşturulduğunda, iletinin alınması **komutu başarıyla tamamlandı.**
11. Bu sürücüsüne geçer ve ardından yazın sürücü harfini yazarak bağlantısını denetleyin **dir** dosya paylaşımının içeriğini görmek için.



## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma 
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasındaki **Hizmetleri**seçin **dosyaları**.
5. Dosya hizmeti sayfasında tıklatın **+ dosya paylaşımı**.
6. Dosya paylaşımı adını girin. Dosya Paylaşımı adları, küçük harf, sayı ve tek kısa çizgilerden kullanabilirsiniz. Adı bir tire ile başlayamaz ve birden çok kullanamazsınız art arda kısa çizgi. 
7. Bir sınır üzerinde ne kadar büyük olabilir, en fazla 5120 GB doldurun.
8. Tıklayın **Tamam** dosya paylaşımını oluşturmak için.
   
## <a name="upload-files"></a>Dosyaları karşıya yükleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasındaki **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin.
6. Tıklayın **karşıya** açmak için **dosyaları karşıya yükleme** sayfası.
7. Karşıya yüklenecek dosyayı için yerel dosya sisteminize göz atmak için klasör simgesine tıklayın.   
8. Tıklayın **karşıya** dosya paylaşımına dosyayı karşıya yüklemek için.

## <a name="download-files"></a>Dosyaları indirme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüsünde **depolama hesapları**.
3. Depolama hesabınızı seçin.
4. İçinde **genel bakış** sayfasındaki **Hizmetleri**seçin **dosyaları**.
5. Bir dosya paylaşımı seçin.
6. Dosyaya sağ tıklayıp seçin **indirme** yerel makinenize indirmek için.
   

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, oluşturun ve PowerShell kullanarak dosya paylaşımlarını yönetin. Daha fazla bilgi için [Windows üzerinde Azure dosya depolama ile çalışmaya başlama](../../storage/files/storage-dotnet-how-to-use-files.md).
