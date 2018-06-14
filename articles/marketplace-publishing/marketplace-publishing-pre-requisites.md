---
title: Azure Market teklifi oluşturmak için teknik olmayan önkoşulları | Microsoft Docs
description: Oluşturma ve bir teklifi Azure satın almak için Marketinde başkalarının dağıtma gereksinimlerini anlayın.
services: marketplace-publishing
documentationcenter: ''
author: msmbaldwin
manager: mbaldwin
editor: ''
ms.assetid: 3dae463b-8f48-4f52-8fa8-4e3975f09f43
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/18/2016
ms.author: mbaldwin
ms.openlocfilehash: 5c30e62bf345843fe83b3f17b728e1a937d19ce3
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
ms.locfileid: "29940050"
---
# <a name="general-prerequisites-for-creating-an-offer-for-the-azure-marketplace"></a>Azure Market teklifi oluşturmak için genel Önkoşullar
Teklif oluşturma işlemine ilerlemek için gereken genel, iş işlemi-merkezli Önkoşullar anlayın.

## <a name="ensure-that-you-are-registered-as-a-seller-with-microsoft"></a>Microsoft ile bir satıcı olarak kayıtlı olduğundan emin olun
Bir satıcı hesabı Microsoft ile kaydetme hakkında ayrıntılı yönergeler için Git [hesap oluşturma ve kayıt](marketplace-publishing-accounts-creation-registration.md).

* **Bir satıcı geliştirme Merkezi'ndeki olarak, şirketinizin zaten kayıtlı ve yeni bir teklif oluşturmak istiyorsanız** sonra oturum açma yayımlama için portal ile hangi Geliştirme Merkezi kayıt yapılır aynı e-posta kimliği. Geliştirici Merkezi ve yayımlama portal bağlı böylece birbirleri ile bu adım gereklidir.
* **Şirketiniz zaten bir satıcı geliştirme Merkezi'ndeki olarak kayıtlıysa ve mevcut bir teklif düzenlemek istediğiniz** sonra ya da oturum açma yayımlama yönetici hesabıyla veya yayımlama ortak yönetici olarak eklenen bir hesap portalı portal. Bir ortak yönetici hesabı eklemek için adımları aşağıda verilmiştir.

## <a name="steps-to-add-a-co-admin-in-the-publishing-portal"></a>Yayımlama portalında ortak yönetici ekleme adımları
Yöneticileri yayımlanmasıyla, yayımlama ortak yönetici olarak, uygulama üzerinde çalışan, diğer üyeleriyle Şirket portalı ekleyebilirsiniz portal. **Yönetici olduğu varsayımıyla** aşağıda verilen olan bir ortak yönetici ekleme adımları

> [!NOTE]
> Yeni kullanıcılar için önce eklediğiniz bir ortak yönetici yayımlama portal, en az bir uygulama yayımlama oluşturduğunuz olun portal. Bu olarak gereklidir **YAYIMCILAR** sekmesi görünür en az bir uygulama Yayımlama özelliği yalnızca oluşturduktan sonra portal.
> 
> 

1. Ortak yönetici e-posta kimliği Microsoft account(MSA) olduğundan emin olun. Değilse, bunu kullanarak MSA kaydetmek [bağlantı](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Ortak yönetici eklemek denemeden önce en az bir uygulama yönetici hesabı altında olduğundan emin olun
3. Yukarıdaki adımları yapılır sonra oturum açma yayımlama için portal ortak yönetici e-posta kimliği ve sonra oturumu kapatın.
4. Şimdi oturum açma yayımlama için portal yönetici e-posta kimliği.
5. Gidin yayımcılar -> hesabınıza -> seçin Yöneticiler -> Ekle ortak yönetici (ekran görüntüsü aşağıda verilen)
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgAddAdmin_05.png)
6. Yayımlama işlemi (örneğin Geliştirici portalını yayımlama Merkezi,) çeşitli aşamalarında sağlanan e-posta kimlikleri Microsoft tüm iletişimi için izlenir emin olun.
7. Geliştirici Merkezi kayıt için tek bir kişi ile ilişkili bir hesabı kullanmaktan kaçının. Bu, bağımlılık bir kişinin kaldırmak için önerilir.
8. Geliştirici Merkezi kayıt sorunlarla karşılaşırken, bunu kullanarak bir bilet Lütfen Yükselt [bağlantı](https://developer.microsoft.com/en-us/windows/support).

## <a name="steps-to-delete-a-co-admin-in-the-publishing-portal"></a>Ortak yönetici yayımlama silmek için adımları portalı
**Yönetici olduğu varsayımıyla** aşağıda verilen bir ortak yönetici silmek için adımlardır

1. Yayımlama için oturum açma portal yönetici e-posta kimliği.
2. Gidin **yayımcılar** hesabınızı -> Seç -> **Yöneticiler** -> **ortak yöneticileri**.
3. Tıklayın **X** Sigortalanan Top Sil (ekran görüntüsü aşağıda verilen) istediğiniz ortak yönetici yanındaki düğmesi.
   
    ![Çizim](media/marketplace-publishing-pre-requisites/imgDeleteAdmin_03.png)

> [!IMPORTANT]
> Yalnızca ücretsiz tekliflerini yayınlama (veya kendi lisansını Getir) planlıyorsanız, Şirket vergi ve banka bilgileri tamamlamak zorunda değildir.
> 
> Başlamak için şirket kayıt tamamlanması gerekir. Şirketiniz Microsoft Developer hesabı vergi ve banka bilgileri çalışırken, ancak, geliştiriciler, sanal makine görüntüsü oluşturma çalışmaya başlamadan [yayımlama portalında](https://publish.windowsazure.com), onu sertifikalı alma ve test etme Bunu Azure hazırlama ortamında. Teklifiniz Azure Marketinde yayımlama, yalnızca son adım için tam satıcı hesap onayı gerekir.
> 
> 

## <a name="acquire-an-azure-pay-as-you-go-subscription"></a>"Kullandıkça Öde" Azure aboneliği edinin
Bu VM görüntülerinizi oluşturmak ve görüntüleri üzerinden el için kullanacağınız aboneliğin olduğu [Azure Marketi](https://azure.microsoft.com/marketplace/). Mevcut bir aboneliğiniz yoksa, sonra lütfen oturum açın https://account.windowsazure.com/signup?offer=ms-azr-0003p.

## <a name="sell-from-countries"></a>"Satış Yeri" ülkelerde
> [!WARNING]
> Hizmetlerinizi Azure Market'te satmak için kayıtlı varlığınız onaylanan "Satış Yeri" ülkelerde birinden olduğundan emin olmanız gerekir. Ödeme ve vergi nedeniyle kısıtlamadır. Etkin olarak arıyoruz ülkelerin listesi yakın gelecekte genişletin, bu nedenle bizi izlemeye devam edin. Bölüm 1b, tam listesi için bkz: [Azure Marketi katılım ilkeleri](http://go.microsoft.com/fwlink/?LinkID=526833).
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Sonraki teknik olmayan ön koşullar karşılandıktan sonra teklif belirli teknik ön koşullar verilmiştir. Makale için Azure Marketi oluşturmak istediğinizi ilgili Teklif türü için bağlantısını tıklatın.

* [VM teknik ön koşullar](marketplace-publishing-vm-image-creation-prerequisites.md)
* [Çözüm şablonu teknik ön koşullar](marketplace-publishing-solution-template-creation-prerequisites.md)

## <a name="see-also"></a>Ayrıca bkz.
* [Başlarken: bir teklifi Azure Marketinde yayımlama](marketplace-publishing-getting-started.md)

