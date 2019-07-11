---
title: Öğretici - paylaşımı verileri müşteriler ve iş ortakları Azure veri paylaşımı Önizlemesi'ni kullanma
description: Öğretici - paylaşımı verileri müşteriler ve iş ortakları Azure veri paylaşımı Önizlemesi'ni kullanma
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: ebd60607f9a24074e0fa985973dfc35674f0d66c
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788230"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Öğretici: Azure veri paylaşımı Önizlemesi'ni kullanarak verilerinizi paylaşma

Bu öğreticide yeni bir Azure veri paylaşımı ayarlama ve müşteriler ve iş ortakları Azure kuruluşunuz dışındaki verilerinizi paylaşmaya başlayın öğreneceksiniz. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Veri paylaşımı oluşturun.
> * Veri kümeleri, veri paylaşımına ekleyin.
> * Bir eşitleme zamanlaması, veri paylaşımı için etkinleştirin. 
> * Alıcılar, veri paylaşımına ekleyin. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bir Azure depolama hesabı: Zaten yoksa, oluşturabileceğiniz bir [Azure depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-data-share-account"></a>Veri paylaşımı hesabı oluşturma

Bir Azure veri paylaşımı kaynak içinde bir Azure kaynak grubu oluşturun.

1. Portalın sol üst köşesinde bulunan **Kaynak oluştur** düğmesini (+) seçin.

1. Arama *veri paylaşımına*.

1. Veri paylaşımına (Önizleme) seçip seçin **Oluştur**.

1. Temel ayrıntıları Azure veri paylaşımı kaynağınızın aşağıdaki bilgilerle doldurun. 

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Ad | *datashareacount* | Veri paylaşımı hesabınız için bir ad belirtin. |
    | Subscription | Aboneliğiniz | Veri paylaşımı hesabınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Resource group | *test-resource-group* | Mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Location | *Doğu ABD 2* | Veri paylaşımı hesabınız için bir bölge seçin.
    | | |

1. Seçin **Oluştur** hesabınıza veri paylaşımı sağlayacak. Genellikle yeni bir veri paylaşımı hesabı sağlama alır yaklaşık 2 dakika veya daha az. 

1. Dağıtım tamamlandığında seçin **kaynağa Git**.

## <a name="create-a-data-share"></a>Veri paylaşımı oluşturma

1. Veri paylaşımına genel bakış sayfanıza gidin.

    ![Verilerinizi paylaşma](./media/share-receive-data.png "verilerinizi paylaşma") 

1. Seçin **verilerinizi paylaşmaya başlayın**.

1. **Oluştur**’u seçin.   

1. Veri paylaşımı için ayrıntıları doldurun. Paylaşım içeriğini açıklamasını ve (isteğe bağlı) kullanım koşullarını gibi bir ad belirtin. 

    ![EnterShareDetails](./media/enter-share-details.png "ayrıntıları girin Paylaş") 

1. Seçin **devam edin**

1. Veri kümeleri için veri paylaşımı eklemek için seçin **veri kümelerine ekleme**. 

    ![Veri kümeleri](./media/datasets.png "veri kümeleri")

1. Eklemek istediğiniz veri kümesi türü seçin. 

    ![AddDatasets](./media/add-datasets.png "veri kümeleri ekleme")    

1. Paylaşın ve 'Veri kümesi Ekle' seçmek için istediğiniz nesneye gidin. 

    ![SelectDatasets](./media/select-datasets.png "veri kümeleri seçin")    

1. Alıcılar sekmesini seçerek, veri tüketici e-posta adreslerini girin. ' + alıcısı Ekle '. 

    ![AddRecipients](./media/add-recipient.png "alıcıları ekleyin") 

1. Seçin **devam edin**

1. Verilerinizi artımlı güncelleştirmeleri almak için veri tüketici isterseniz, anlık görüntü zamanlama etkinleştirin. 

    ![EnableSnapshots](./media/enable-snapshots.png "anlık görüntüleri etkinleştir") 

1. Başlangıç saati ve yinelenme aralığını seçin. 

1. Seçin **devam edin**

1. İncelemede + sekme oluşturmak, paket içeriğini, ayarları, alıcıları ve eşitleme ayarlarını gözden geçirin. **Oluştur**’u seçin

Şimdi, Azure veri paylaşımı oluşturuldu ve alıcı, veri paylaşımı, daveti kabul etmek hazırdır. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri paylaşımı oluşturma ve davet alıcılar modellerin. Nasıl bir veri tüketici kabul edebilir ve veri paylaşım alma hakkında bilgi edinmek için devam [kabul edin ve veri alma](subscribe-to-data-share.md) öğretici. 