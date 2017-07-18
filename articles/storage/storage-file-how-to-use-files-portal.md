---
title: "Azure portalından Azure Dosya depolamayı yönetme | Microsoft Docs"
description: "Azure portalından Azure Dosya depolamayı yönetmeyi öğrenin."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 58353a8058c2722788a38d2ca596061d14b68caf
ms.contentlocale: tr-tr
ms.lasthandoff: 07/12/2017

---

# <a name="how-to-use-azure-file-storage-from-the-azure-portal"></a>Azure portalından Azure Dosya depolamayı yönetme
[Azure portalı](https://portal.azure.com), Azure Dosya paylaşımlarını yönetmek için bir kullanıcı arabirimi sunar. Web tarayıcınızdan aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Dosya Paylaşımı oluşturma
* Dosya paylaşımınızda dosyaları karşıya yükleme ve indirme.
* Her dosya paylaşımının gerçek kullanımını izleme.
* Dosya paylaşımının boyut kotasını ayarlama.
* Windows veya Linux istemcisinden dosya paylaşımınızı bağlamak için bağlama komutlarını kopyalayın.

## <a name="create-file-share"></a>Dosya paylaşımı oluşturma
1. Azure Portal’da oturum açın.
2. Gezinti menüsünde, **Depolama hesapları** veya **Depolama hesapları (klasik)** seçeneğine tıklayın.
    
    ![Portalda dosya paylaşımı oluşturmayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share1.png)

3. Depolama hesabınızı seçin.

    ![Portalda dosya paylaşımı oluşturmayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share2.png)

4. "Dosyalar" hizmetini seçin.

    ![Portalda dosya paylaşımı oluşturmayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share3.png)

5. "Dosya paylaşımları" seçeneğine tıklayın ve ilk dosya paylaşımınızı oluşturmak üzere bağlantıyı takip edin.

    ![Portalda dosya paylaşımı oluşturmayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share4.png)

6. İlk dosya paylaşımınızı oluşturmak üzere dosya paylaşımının adını ve boyutunu (en fazla 5120 GB) girin. Dosya paylaşımı oluşturulduktan sonra, SMB 2.1 veya SMB 3.0 destekleyen herhangi bir dosya sisteminden bağlayabilirsiniz. Dosya paylaşımında bulunan **Kota**’ya tıklayarak dosyanızın boyutunu en fazla 5120 GB’a kadar değiştirebilirsiniz. Azure Dosya depolama kullanıldığında depolamanın tahmini maliyetini hesaplamak lütfen [Azure Fiyatlandırma Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)’na bakın.

    ![Portalda dosya paylaşımı oluşturmayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-create-file-share5.png)

## <a name="upload-and-download-files"></a>Dosyaları yükleme ve indirme
1. Daha önce oluşturduğunuz bir dosya paylaşımını seçin.

    ![Portaldan dosya yükleme ve indirmeyi gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file1.png)

2. Yüklenen dosyalar için kullanıcı arabirimini açmak üzere **Yükle**’ye tıklayın.

    ![Portaldan dosya yüklemeyi gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-upload-file2.png)

## <a name="connect-to-file-share"></a>Dosya paylaşımına bağlanma
-  Dosya paylaşımını bağlamak üzere Windows ve Linux’tan komut satırı almak için **Bağlan**’a tıklayın. Linux kullanıcıları için, diğer Linux dağıtımlarına yönelik daha fazla bağlama yönergesini [Azure Dosya depolamayı Linux ile kullanma](storage-how-to-use-files-linux.md) bölümünde bulabilirsiniz.

    ![Dosya paylaşımını bağlamayı gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-connect.png)
-  Windows veya Linux’ta dosya paylaşımı bağlama ve Azure VM veya şirket içi makinenizde çalıştırma için komutları kopyalayabilirsiniz.

    ![Windows ve Linux için bağlama komutlarını gösteren ekran görüntüsü](media/storage-file-how-to-use-files-portal/use-files-portal-show-mount-commands.png)

**İpucu:**  
Bağlama için depolama hesabı erişim anahtarını, bağlan sayfasının en altındaki **Bu depolama hesabı için erişim anahtarlarını görüntüleyin** alanına tıklayarak bulabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.
Azure File Storage hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın.

* [SSS](storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-file-connection-problems.md)
