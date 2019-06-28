---
title: Özel bulut ayrıcalıklar - CloudSimple tarafından Azure VMware çözümü
description: Yönetim vCenter işlevler için özel bulutunuzda ayrıcalıklar açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d11c88b91b13cca13120a9203e376fdc2c3d6d8d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332835"
---
# <a name="escalate-private-cloud-vcenter-privileges-from-the-cloudsimple-portal"></a>Özel bulut CloudSimple Portalı'ndan vCenter ayrıcalıklar 

Özel bulut vCenter'ınıza, yönetici erişimi için geçici olarak CloudSimple ayrıcalıklarınızı ilerletebilirler.  Yükseltilmiş ayrıcalıklar kullanarak, VMware çözümlerini yüklemek, kimlik kaynağı ekleme ve kullanıcıları yönetme.

Yeni kullanıcılar, vCenter SSO etki alanında oluşturulan ve vCenter erişim verilir.  Yeni kullanıcılar oluşturduğunda, bunları CloudSimple yerleşik gruplarına vCenter erişmek için ekleyin.  Daha fazla bilgi için [VMware vCenter'ın yeni bir izin modeli CloudSimple özel bulut](https://docs.azure.cloudsimple.com/learn-private-cloud-permissions/).

> [!CAUTION]
> Yönetim bileşenleri için yapılandırma değişiklikleri yapmayın. İlerletilen ayrıcalıklı durumu sırasında gerçekleştirilen eylemler sisteminizi olumsuz etkileyebilir veya sisteminizin kullanılamaz hale gelmesine neden olabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="escalate-privileges"></a>Ayrıcalıkları yükseltme

1. Erişim [CloudSimple portalı](access-cloudsimple-portal.md).

2. Açık **kaynakları** sayfasında, ayrıcalıkları yükseltmek istediğiniz özel Bulutu seçin.

3. Özet sayfasının altında altındaki **değiştirme vSphere ayrıcalıkları**, tıklayın **Escalate**.

    ![VSphere ayrıcalık değiştirme](media/escalate-private-cloud-privilege.png)

4. VSphere kullanıcı türünü seçin.  Yalnızca **CloudOwner@cloudsimple.local** yerel kullanıcı ilerletilmiş.

5. Açılan listeden escalate zaman aralığını seçin. Görevi tamamlamak sağlayacak en kısa süre seçin.

6. Risklerini anladığınızı onaylamanız için onay kutusunu seçin.

    ![Ayrıcalık iletişim İlerlet](media/escalate-private-cloud-privilege-dialog.png)

7. **Tamam**'ı tıklatın.

8. Yükseltme işlemi birkaç dakika sürebilir. İşlem tamamlandığında **Tamam**’a tıklayın.

Ayrıcalık yükseltme saldırısı başlar ve seçilen aralığın sonuna kadar sürer.  Özel bulut vCenter'ınıza yönetim görevlerini yapmak için oturum açabilir.

> [!IMPORTANT]
> Yalnızca bir kullanıcı ayrıcalıkları ilerletilmiş.  Başka bir kullanıcının ayrıcalıkları yükseltmek önce kullanıcının ayrıcalıkları ilerletebilir XML'deki gerekir.

## <a name="extend-privilege-escalation"></a>Ayrıcalık yükseltme saldırısı genişletme

Görevleri tamamlamak için ek süre gerektiriyorsa, ayrıcalık yükseltme dönemi genişletebilirsiniz.  Ek yönetim görevleri tamamlamak izin veren bir zaman aralığı İlerlet seçin.

1. Üzerinde **kaynakları** > **özel Bulutları** CloudSimple Portalı'nda, ayrıcalık yükseltme saldırısı genişletmek istediğiniz özel Bulutu seçin.

2. Özet sekmesi altına tıklayın **ayrıcalık yükseltme saldırısı genişletmek**.

    ![Ayrıcalık yükseltme saldırısı genişletme](media/de-escalate-private-cloud-privilege.png)

3. Açılan listeden bir escalate zaman aralığı seçin. Yeni yükseltme bitiş zamanı gözden geçirin.

4. Tıklayın **Kaydet** aralığını genişletmek için.

## <a name="de-escalate-privileges"></a>Seri durumdan ayrıcalıklarına İlerlet

Yönetim görevlerinizi tamamlandıktan sonra sizin ayrıcalıklarınıza XML'deki İlerlet.  

1. Üzerinde **kaynakları** > **özel Bulutları** CloudSimple Portalı'nda XML'deki ayrıcalıkları yükseltmek istediğiniz özel Bulutu seçin.

2. Tıklayın **XML'deki İlerlet**.

3. **Tamam**'ı tıklatın.

> [!IMPORTANT]
> Hataları önlemek için vCenter dışında oturum açın ve yeniden ayrıcalıkları XML'deki Etkinleºmesini sonra oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

* [Active Directory'yi kullanmak için vCenter kimlik kaynakları ayarlayın](https://docs.azure.cloudsimple.com/set-vcenter-identity/)
* Yedekleme çözümü yükleme [iş yükü sanal makineleri yedekleme](https://docs.azure.cloudsimple.com/backup-workloads-veeam/)