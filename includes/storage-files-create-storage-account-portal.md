---
title: storage-files-create-storage-account-portal
description: Azure Dosyaları için depolama hesabı oluşturma.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 82e141eb4941dbd58be95e19dae186acd18caf94
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
Depolama hesabı, Azure dosya paylaşımını veya bloblar veya sorgular gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzudur. Bir depolama hesabında sınırsız sayıda paylaşım olabilir; paylaşım da, depolama hesabının kapasite sınırları içinde sınırsız sayıda dosya depolayabilir.

Depolama hesabı oluşturmak için:

1. Sol menüden **+** seçeneğine tıklayarak bir kaynak oluşturun.
2. Arama kutusuna **depolama hesabı** yazın (1), **Depolama hesabı - blob, dosya, tablo, kuyruk** seçeneğini belirleyin (2) ve sonra **Oluştur**’a tıklayın.
    ![Kaynak araması iletişim kutusunda, depolama hesabı girişinin nasıl göründüğüne dair ekran görüntüsü](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. **Ad** alanına *mystorageacct* komutunu yazın ve benzersiz bir ad olduğunu belirten yeşil onay işareti görüntüleninceye kadar komutun sonuna birkaç rastgele sayı yazın. Depolama hesabı adı tamamen küçük harflerden oluşmalı ve genel olarak benzersiz olmalıdır. Daha sonra kullanacağınız için depolama hesabınızın adını not edin. 
4. **Dağıtım modeli** alanında varsayılan **Kaynak yöneticisi** değerini değiştirmeden bırakın. Azure Resource Manager ile klasik dağıtım arasındaki farklı hakkında daha fazla bilgi edinmek için bkz. [Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. **Hesap türü** bölümünde **StorageV2** seçeneğini belirleyin. Farklı türlerde depolama hesapları hakkında daha fazla bilgi edinmek için bkz. [Azure depolama hesaplarını anlama](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
6. **Performans** bölümünde, varsayılan **Standart depolama** değerini değiştirmeden bırakın. Premium depolamayı seçseniz de dosya paylaşımınız standart depolama alanında depolandığından Azure Dosyaları şu anda yalnızca standart depolama alanını destekler.
7. **Çoğaltma** bölümünde *Yerel olarak yedekli depolama (LRS)* seçeneğini belirleyin. 
8. **Güvenli aktarım gereklidir** bölümünde her zaman **Etkin** seçeneğini belirlemenizi öneririz. Bu seçenek hakkında daha fazla bilgi için bkz. [Aktarım sırasında şifrelemeyi anlama](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
9. **Abonelik** bölümünde, depolama hesabının oluşturulacağı aboneliği seçin. Yalnızca bir aboneliğiniz varsa bu, varsayılan değer olmalıdır.
10. **Kaynak grubu** bölümünde **Yeni oluştur**’u seçin ve ad olarak *myResourceGroup* yazın.
11. **Konum** bölümünde **Doğu ABD**’yi seçin.
12. **Sanal ağlar** bölümünde varsayılan seçeneği *Devre dışı* olarak değiştirmeden bırakın. 
13. Depolama hesabının kolayca bulunması için **Panoya sabitle**’yi seçin.
14. İşleminiz bittiğinde dağıtımı başlatmak için **Oluştur**’a tıklayın.