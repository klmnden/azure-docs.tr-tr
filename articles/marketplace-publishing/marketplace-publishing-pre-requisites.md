---
title: Azure Marketi için bir teklif oluşturma teknik olmayan önkoşulları | Microsoft Docs
description: Oluşturma ve bir teklif satın almak için Azure Marketi'ni için başkalarının dağıtma gereksinimleri öğrenin.
services: marketplace-publishing
documentationcenter: ''
author: HannibalSII
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
ms.openlocfilehash: ef19380372354b8f34343f9f94ebf6b384996f14
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261570"
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Azure Marketi için bir teklif oluşturmak için genel Önkoşullar
Teklif oluşturma işlemiyle birlikte ilerlemek için gereken genel, iş işlem-odaklı önkoşulları anlayın.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Microsoft ile bir satıcı olarak kayıtlı olduğundan emin olun.
Bir satıcı hesabı Microsoft ile kaydetme hakkında ayrıntılı yönergeler için Git [hesap oluşturma ve kayıt](marketplace-publishing-accounts-creation-registration.md).

* **Şirketinizin zaten bir satıcı geliştirme Merkezi'ndeki olarak kayıtlı ve yeni bir teklif oluşturmak istediğiniz** sonra oturum açma yayımlama için portal ile hangi Geliştirme Merkezi kayıt yapılır aynı e-posta kimliği. Geliştirme Merkezi ve Yayımlama Portalı bağlı olacak şekilde birbirleri ile bu adım gereklidir.
* **Şirketinizin zaten bir satıcı geliştirme Merkezi'ndeki olarak kayıtlı ve var olan bir teklif düzenlemek istediğiniz** ardından her iki oturum açma Yayımlama Portalı yönetici hesabıyla veya bir hesapla yayımlama ortak yönetici olarak eklendi portalı. Bir ortak yönetici hesabı eklemek için adımları aşağıda verilmiştir.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Yayımlama Portalı'nda ortak yönetici ekleme adımları
Yöneticiler yayımlanması, yayımlama ortak yönetici olarak, uygulama üzerinde çalışıyorsanız, diğer üyeleri, Şirket portalı ekleyebilirsiniz portalı. **Yönetici olduğu varsayılırsa,** aşağıda verilen olan ortak yönetici ekleme adımları

> [!NOTE]
> Yeni kullanıcılar için önce eklediğiniz bir ortak yönetici, Yayımlama Portalı, yayımlama, en az bir uygulaması oluşturduğunuzdan emin olun portalı. Bu olarak gereklidir **YAYIMCILAR** sekmesi görüntülenir, yayımlama yalnızca en az bir uygulama oluşturduktan sonra portal.
> 
> 

1. Ortak yönetici e-posta kimliği Microsoft account(MSA) olduğundan emin olun. Aksi takdirde, bunu kullanan bir MSA kaydetmek [bağlantı](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Ortak yönetici eklemek denemeden önce en az bir uygulama yönetici hesabı altında olduğundan emin olun
3. Yukarıdaki adımları yapılır sonra oturum açma Yayımlama Portalı ortak yönetici e-posta kimliği ve ardından oturumu kapatma.
4. Artık oturum açma Yayımlama Portalı yönetici e-posta kimliği.
5. Gidin yayımcıları -> hesabınızı seçin -> Yöneticiler -> ortak yönetici (ekran görüntüsü aşağıda verilen) Ekle
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Yayımlama işlemi (örneğin geliştirme Yayımlama Portalı Merkezi,) çeşitli aşamaları sırasında sağlanan e-posta kimlikleri Microsoft gelen tüm iletişimi için izlenen emin olun.
7. Geliştirme Merkezi kayıt için tek bir kişi ile ilişkili bir hesabı kullanmaktan kaçının. Bu, bir kişiden bağımlılık kaldırmak için önerilir.
8. Geliştirme Merkezi kaydı ile herhangi bir sorunla karşılaşırsanız, bunu kullanan bir bilet Lütfen raise [bağlantı](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Ortak yönetici yayımlama silmek için adımları portalı
**Yönetici olduğu varsayılırsa,** aşağıda verilen ortak yönetici silmek için adımlar yer almaktadır

1. Oturum açma Yayımlama Portalı yönetici e-posta kimliği.
2. Gidin **yayımcılar** -> hesabınızı seçin -> **Yöneticiler** -> **ortak Yöneticiler**.
3. Tıklayarak **X** tot Sil (ekran görüntüsü aşağıda verilen) istediğiniz ortak yönetici yanındaki düğmeyi.
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Yalnızca ücretsiz teklifleri yayımlama (veya kendi lisansını Getir) planlıyorsanız vergi ve banka şirket bilgilerini tamamlamak zorunda değildir.
> 
> Başlamak için şirket kaydı tamamlanmış olması gerekir. Şirketiniz Microsoft Developer hesabı vergi ve bank bilgileri çalışırken, ancak geliştiriciler içinde sanal makine görüntüsü oluşturma üzerinde çalışmaya başlayabilir [yayımlama portalı](https://publish.windowsazure.com), sertifika alma alma ve test etme Bunu Azure hazırlık ortamı. Teklifinizin Azure Market'te yayımlama, yalnızca son adım için tam satıcı hesabı onay gerekir.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>"Kullandıkça Öde" bir Azure aboneliği edinme
Bu VM görüntülerinizi oluşturmak ve görüntülerin üzerinden teslim için kullanacağınız aboneliği, [Azure Marketi](https://azure.microsoft.com/marketplace/). Mevcut bir aboneliğiniz yoksa, sonra lütfen adresinde kaydolun https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Satış yapan" ülkeler
> [!WARNING]
> Hizmetlerinizi Azure Marketi'nde satmak için kayıtlı varlık onaylı "satış yapan" ülkeleri birinden olduğundan emin olmanız gerekir. Bu kısıtlama ödeme ve vergi amaçlıdır. Etkin bir şekilde arıyoruz yakın gelecekte bu ülkelerde listesini genişletin, bu nedenle Takipte kalın. Bölüm 1b, tam listesi için bkz. [Azure Marketi katılım ilkeleri](https://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Teknik olmayan ön koşullar karşılandıktan sonra sonraki teklif belirli teknik ön koşullar verilmiştir. Makale için Azure Marketi için oluşturmak istediğiniz ilgili Teklif türü için bağlantıya tıklayın.

* [VM teknik ön koşullar](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Çözüm şablonu teknik ön koşullar](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: nasıl bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

