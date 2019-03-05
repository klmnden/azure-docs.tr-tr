---
title: İzleme ve Azure Stack VaaS portalında testleri yönetme | Microsoft Docs
description: İzleme ve Azure Stack VaaS portalında testleri yönetme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/04/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 8784acc0180be1c3e0ac277b7c2a21d422ebccd0
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57341060"
---
# <a name="monitor-and-manage-tests-in-the-vaas-portal"></a>İzleme ve testleri VaaS portalında yönetme

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack çözümünüzü karşı testleri zamanlama sonra test yürütme durumu raporlama doğrulama (VaaS) hizmet olarak başlar. Bu bilgiler, VaaS portalında kullanılabilirliğiyle ve testleri iptal etme gibi eylemler ile birlikte kullanılabilir.

## <a name="navigate-to-the-workflow-tests-summary-page"></a>İş akışı testleri Özet sayfasına gidin

1. Çözüm panosunda en az bir iş akışı olan varolan bir çözümü seçin.

    ![İş akışı kutucukları](media/tile_all-workflows.png)

1. Seçin **Yönet** iş akışı kutucuğundaki. Sonraki sayfanın seçili çözüm için oluşturulan iş akışları listeler.

1. Test özeti açmak için iş akışı adını seçin.

## <a name="change-workflow-parameters"></a>İş akışı parametreleri değiştirin

Her iş akışı türü düzenlemenize olanak sağlayan [Test parametreleri](azure-stack-vaas-parameters.md#test-parameters) iş akışı oluşturma sırasında belirtilebilen.

1. Testleri Özet sayfasında, seçin **Düzenle** düğmesi.

1. Şunlara göre yeni değerleri sağlayın [iş akışı ortak parametreleri için bir hizmet olarak Azure Stack doğrulama](azure-stack-vaas-parameters.md).

1. Seçin **Gönder** değerlerini kaydetmek için.

> [!NOTE]
> İçinde **Test geçiş** iş akışı, ihtiyaç duyacağınız test seçimi ve yeni parametre değerlerini kaydedebilmek için önce gözden geçirme sayfasına gidin.

### <a name="add-tests-test-pass-only"></a>Test (yalnızca Test geçiş) Ekle

İçinde **Test geçiş** iş akışları, her iki **ekleme testleri** ve **Düzenle** düğmeleri iş akışı içinde yeni testler zamanlamanıza olanak sağlar.

> [!TIP]
> Seçin **ekleme testleri** yalnızca yeni testleri zamanlayın istiyorsanız ve parametrelerini düzenlemek gerekmeyen bir **Test geçiş** iş akışı.

## <a name="managing-test-instances"></a>Test örneklerini yönetme

Terim ve kısaltmalarla çalıştırmalar için (yani, **Test geçiş** iş akışı), Azure Stack çözüm karşı zamanlanmış testleri testleri Özet sayfasında listelenir.

Resmi çalıştırmalar için (yani, **doğrulama** iş akışları), Azure Stack çözüm doğrulamasını tamamlamak için gereken testleri testleri Özet sayfasında listelenir. Doğrulama testleri bu sayfadan zamanlanmış.

Her bir zamanlanmış test örneği aşağıdaki bilgileri gösterir:

| Sütun | Açıklama |
| --- | --- |
| Test adı | Adı ve sürümü test. |
| Kategori | Test amaçlı. |
| Oluşturulan | Hangi test zamanlandığı saat. |
| Başlatıldı | Hangi test yürütme başlama zamanı. |
| Süre | Test çalıştırdığınızda uzunluğu. |
| Durum | Durum veya test sonucu. Yürütme öncesi veya devam eden durumlar şunlardır: `Pending`, `Running`. Terminal durumlar şunlardır: `Cancelled`, `Failed`, `Aborted`, `Succeeded`. |
| Aracı adı | Çalışan test aracısı'nın adı. |
| Toplam işlem | Test sırasında çalıştı işlemlerinin toplam sayısı. |
| Başarılı işlemler | Test sırasında başarılı işlem sayısı. |
|  Başarısız olan işlemler | Test sırasında başarısız olan işlemlerin sayısı. |

### <a name="actions"></a>Eylemler

Her bir test örneği kendi bağlam menüsünde tıkladığınızda gerçekleştirebileceğiniz eylemleri listeler **[...]**  test örnekleri tabloda.

#### <a name="view-information-about-the-test-definition"></a>Test tanımı hakkında bilgileri görüntüleyin

Seçin **bilgilerini görüntüleyin** test tanımı hakkında genel bilgileri görüntülemek için bağlam menüsünden. Bu, aynı ad ve sürümde test örneği tarafından paylaşılır.

| Test özelliği | Açıklama |
| -- | -- |
| Test adı | Testin adı. |
| Test sürümü | Test sürümü. |
| Yayımcı | Test yayımcı. |
| Kategori |  Test amaçlı. |
| Hedef Hizmetleri | Test edilen Azure Stack Hizmetleri. |
| Açıklama | Test açıklaması. |
| Tahmini süre (dakika) | Test beklenen çalışma zamanı. |
| Bağlantılar | Test ya da iletişim noktaları ilgili bilgileri. |

#### <a name="view-test-instance-parameters"></a>Görünümü test örneği parametreleri

Seçin **görüntülemek parametreleri** test örneği zamanlama sırasında sağlanan parametreleri görüntülemek için bağlam menüsünden. Parolalar gibi hassas dizeleri görüntülenmez. Bu eylem yalnızca zamanlanmış testleri için kullanılabilir.

Bu pencere, tüm test örnekleri için aşağıdaki meta verileri içerir:

| Test örnek özelliği | Açıklama |
| -- | -- |
| Test adı | Testin adı. |
| Test sürümü | Test sürümü. |
| Test örneği kimliği | Test belirli örneğini tanımlayan bir GUID. |

#### <a name="view-test-instance-operations"></a>Test örneği işlemlerini görüntüleme

Seçin **görüntüleme işlemleri** bağlamdan işlemleri ayrıntılı durumunu görüntülemek için menü, test sırasında gerçekleştirilen. Bu eylem yalnızca zamanlanmış testleri için kullanılabilir.

![işlemlerini görüntüleme](media/manage-test_context-menu-operations.png)

#### <a name="download-logs-for-a-completed-test-instance"></a>Tamamlanan test örneği için günlükleri indirin

Seçin **indirme günlükleri** indirmek için bağlam menüsünden bir `.zip` test yürütme sırasında günlükleri çıkış dosyası. Bu eylem, tamamlanan, yalnızca testler için bir test durumu ya da başka bir deyişle, kullanılabilir `Cancelled`, `Failed`, `Aborted`, veya `Succeeded`.

#### <a name="reschedule-a-test-instance-or-schedule-a-test"></a>Bir test örneği randevularını yeniden zamanlayabilir veya bir test zamanlama

Yönetim sayfasından testleri zamanlama testi altında çalışan iş akışı türü bağlıdır.

##### <a name="test-pass-workflow"></a>Test geçiş iş akışı

Test geçiş iş akışı **kullanılabilirliğiyle** bir test örneği aynı parametre kümesi özgün test örneği olarak kullanır ve *değiştirir* kendi günlükleri de dahil olmak üzere, özgün sonucu. Parolalar gibi hassas dizeleri, yeniden zamanladığınızda yeniden girmeniz gerekir.

1. Seçin **yeniden** bağlam menüsünden test örneği yeniden zamanlama için bir istem açın.

1. Herhangi bir geçerli parametre girin.

1. Seçin **Gönder** test örneği randevularını yeniden zamanlayabilir ve var olan bir örneğini değiştirin.

##### <a name="validation-workflows"></a>Doğrulama iş akışları

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

#### <a name="cancel-a-test-instance"></a>Bir test örneği iptal et

Zamanlanmış bir testi durumunu olması durumunda iptal edilebileceğini `Pending` veya `Running`.  

1. Seçin **iptal** bağlam menüsünden test örneği iptal etmek için bir istem açın.

1. Seçin **Gönder** test örneği iptal etmek için.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet olarak doğrulama sorunlarını giderme](azure-stack-vaas-troubleshoot.md)
