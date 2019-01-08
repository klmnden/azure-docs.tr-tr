---
title: Azure Marketi için bir teklif oluşturma teknik olmayan önkoşulları | Microsoft Docs
description: Oluşturma ve bir teklif satın almak için Azure Marketi'ni için başkalarının dağıtma gereksinimleri öğrenin.
services: marketplace-publishing
documentationcenter: ''
author: v-miclar
manager: hascipio
editor: ''
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: hascipio
ROBOTS: NOINDEX
ms.openlocfilehash: 4b925522186d2d9ae537431c1d96d39b107ad967
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073178"
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Azure Marketi için bir teklif oluşturmak için genel Önkoşullar
Teklif oluşturma işlemiyle birlikte ilerlemek için gereken genel, iş işlem-odaklı önkoşulları anlayın.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Microsoft ile bir satıcı olarak kayıtlı olduğundan emin olun.
Bir satıcı hesabı Microsoft ile kaydetme hakkında ayrıntılı yönergeler için Git [hesap oluşturma ve kayıt](marketplace-publishing-accounts-creation-registration.md).

* Şirketinizin zaten bir satıcı geliştirme Merkezi'ndeki olarak kayıtlı ve yeni bir teklif oluşturmak istediğiniz ardından yayımlama için oturum açın portal ile hangi Geliştirme Merkezi kayıt yapılır aynı e-posta kimliği. Geliştirme Merkezi ve Yayımlama Portalı bağlı olacak şekilde birbirleri ile bu adım gereklidir.
* Şirketiniz bir satıcı geliştirme Merkezi'ndeki olarak zaten kaydedildi ve var olan bir teklif düzenlemek istediğiniz durumunda yayımlama içine oturum yönetici hesabıyla veya bir coadmin yayımlama olarak eklenen bir hesapla portal portalı. Coadmin hesabı eklemek için adımları aşağıda verilmiştir.

## <a name="steps-to-add-a-coadmin-in-the-publishing-portal"></a>Yayımlama Portalı'nda bir coadmin ekleme adımları
Yöneticiler yayımlanması, yayımlama bir coadmin olarak, uygulama üzerinde çalışıyorsanız, diğer üyeleri, Şirket portalı ekleyebilirsiniz portalı. **Yönetici olduğu varsayılırsa,** aşağıda verilen bir coadmin eklemek için adımlar yer almaktadır.

> [!NOTE]
> Yeni kullanıcılar için önce eklediğiniz bir ortak yönetici, Yayımlama Portalı, yayımlama, en az bir uygulaması oluşturduğunuzdan emin olun portalı. Bu olarak gereklidir **YAYIMCILAR** sekmesi görüntülenir, yayımlama yalnızca en az bir uygulama oluşturduktan sonra portal.
> 
> 

1. Bir Microsoft account(MSA) coadmin e-posta kimliği olduğundan emin olun. Aksi takdirde, bunu kullanan bir MSA kaydetmek [bağlantı](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Bir coadmin eklemek denemeden önce en az bir uygulama yönetici hesabı altında olduğundan emin olun.
3. Yukarıdaki adımları tamamladıktan sonra oturum yayımlama coadmin portalıyla kimliği e-posta ve sonra oturumu kapatın.
4. Yayımlama için şimdi oturum yönetici e-posta kimliğiyle portal
5. Gidin yayımcıları -> hesabınızı seçin -> Yöneticiler, coadmin (ekran görüntüsü aşağıda verilen) Ekle ->
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Yayımlama işlemi (örneğin, geliştirme Yayımlama Portalı Merkezi,) çeşitli aşamaları sırasında sağlanan e-posta kimlikleri Microsoft gelen tüm iletişimi için izlenen emin olun.
7. Geliştirme Merkezi kayıt için tek bir kişi ile ilişkili bir hesabı kullanmaktan kaçının. Bu öneri, tek bağlılığı kaldırır.
8. Geliştirme Merkezi kaydı ile herhangi bir sorunla karşılaşırsanız, ardından bunu kullanarak bir bilet yükseltmek [bağlantı](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-coadmin-in-the-publishing-portal"></a>Bir coadmin yayımlama silmek için adımları portalı
**Yönetici olduğu varsayılırsa,** aşağıda verilen bir coadmin silmek için adımlar yer almaktadır.

1. Yayımlama için oturum yönetici e-posta kimliğiyle portal
2. Gidin **yayımcılar** -> hesabınızı seçin -> **Yöneticiler** -> **ortak Yöneticiler**.
3. Tıklayarak **X** tot Sil (ekran görüntüsü aşağıda verilen) istediğiniz coadmin yanındaki düğmeyi.
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Yalnızca ücretsiz teklifleri yayımlama (veya kendi lisansını Getir) planlıyorsanız vergi ve banka şirket bilgilerini tamamlamak zorunda değildir.
> 
> Başlamak için şirket kaydı tamamlanmış olması gerekir. Şirketiniz Microsoft Developer hesabı vergi ve bank bilgileri çalışırken, ancak geliştiriciler içinde sanal makine görüntüsü oluşturma üzerinde çalışmaya başlayabilir [yayımlama portalı](https://publish.windowsazure.com), sertifika alma alma ve test etme Bunu Azure hazırlık ortamı. Teklifinizin Azure Market'te yayımlama, yalnızca son adım için tam satıcı hesabı onay gerekir.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>"Kullandıkça Öde" bir Azure aboneliği edinme
Bu değer, VM görüntülerinizi oluşturmak ve görüntülerin üzerinden teslim için kullanacağınız aboneliği [Azure Marketi](https://azure.microsoft.com/marketplace/). Mevcut bir aboneliğiniz yoksa, ardından adresinde kaydolun https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Satış yapan" ülkeler
> [!WARNING]
> Hizmetlerinizi Azure Marketi'nde satmak için kayıtlı varlık onaylı "satış yapan" ülkeleri birinden olduğundan emin olmanız gerekir. Bu kısıtlama ödeme ve vergi amaçlıdır. Etkin bir şekilde arıyoruz yakın gelecekte bu ülkelerde listesini genişletin, bu nedenle Takipte kalın. Bölüm 1b, tam listesi için bkz. [Azure Marketi katılım ilkeleri](https://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Teknik olmayan ön koşullar karşılandıktan sonra sonraki teklif özel teknik ön koşullar verilmiştir. Makale için Azure Marketi için oluşturmak istediğiniz ilgili Teklif türü için bağlantıya tıklayın.

* [VM teknik ön koşullar](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Çözüm şablonu teknik ön koşullar](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: Nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

