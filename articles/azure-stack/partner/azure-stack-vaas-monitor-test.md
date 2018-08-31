---
title: Bir test bir hizmet olarak Azure Stack doğrulama ile izleme | Microsoft Docs
description: Hizmet olarak Azure Stack doğrulama ile bir test izleyin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 553d2a0e4bf6b23f5d8ab200f533d9245bf72d36
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286593"
---
# <a name="monitor-a-test-with-azure-stack-validation-as-a-service"></a>Bir test bir hizmet olarak Azure Stack doğrulama ile izleme

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bir testin yürütme görüntüleyerek izlenebilir **işlemleri** devam eden veya tamamlanmış test paketleri için sayfa. Bu sayfa, işlemlerini ve testin durumu ayrıntıları.

## <a name="monitor-a-test"></a>Bir test izleyin

1. Bir çözümü seçin.

2. Seçin **Yönet** herhangi bir iş akışı kutucuğa üzerinde.

3. Bir iş akışı kendi test Özet sayfasını açmak için tıklayın.

4. Bağlam menüsünü genişletin **[...]**  paketi örneği için test edin.

5. Seçin **işlemlerini görüntüleme**

![Alternatif metin](media\image4.png)

Çalıştırma tamamlandı testler için günlükleri test özeti sayfasından tıklayarak indirilebilir **indirme günlükleri** bağlam menüsündeki bir testin **[...]** . Azure Stack iş ortakları Bu günlükler, başarısız testlerde hata ayıklamak için kullanabilirsiniz.

## <a name="open-the-test-pass-summary"></a>Test geçişi özeti açın

1. Portalı'nı açın. 
2. Daha önce çalışma veya zamanlanmış testleri içeren varolan bir çözüm adını seçin.

    ![Test geçişleri yönetme](media/managetestpasses.png)

3. Seçin **Yönet** içinde **Test geçişleri** paneli.
4. Test geçiş testi geçişi özetini açmak için seçin. Test adı inceleyebilirsiniz oluşturma tarihi, çalıştırma, test ne kadar sürdüğünü ve sonucu (başarılı veya başarısız).
5. Seçin [ **.  .** ].

### <a name="test-pass-summary"></a>Test geçiş özeti

| Sütun | Açıklama |
| --- | --- |
| Test adı | Testin adı. Doğrulama numarası başvuruyor. |
| Oluşturulan | Test geçiş oluşturulduğu zaman. |
| Başlatıldı | Son test çalıştırdığınızda. |
| Süre | Test geçiş çalıştırmak için geçen süre sürenin uzunluğu. |
| Durum | Sonucu (başarılı veya başarısız) rest geçişi için. |
| Aracı Adı | Aracısı'nın tam etki alanı adı. |
| Toplam işlem | Test geçişi yapılmaya işlemlerinin toplam sayısı. |
| Başarılı işlemler | Test geçişi geçirilen işlemlerinin sayısı. |
|  Başarısız olan işlemler | Başarısız işlemlerin sayısı. |

### <a name="group-columns-in-the-test-pass-summary"></a>Grup sütunları test özeti geçirin

Seçin ve bir sütun üst bilgisi bir sütun değerine bir grup oluşturmak için sürükleyin.

## <a name="reschedule-a-test"></a>Bir test yeniden zamanlama

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **yeniden** test geçiş yeniden zamanlamak için.
3. Azure Stack örneğiniz bulut yönetici parolasını girin.
4. Hesabınızı tanımladığınız tanılama depolama bağlantı dizesi girin.
5. Test yeniden zamanlayın.

## <a name="cancel-a-test"></a>Bir testi iptal et

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **iptal**.

## <a name="get-test-information"></a>Test bilgileri Al

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **bilgilerini görüntüleyin** test geçiş yeniden zamanlamak için.

**Test bilgileri**

| Ad | Açıklama |
| -- | -- |
| Test adı | Örneğin, Azure Stack 1806 RC doğrulama OEM güncelleştirmeyi test adı. |
| Test sürümü | Örneğin, 5.1.4.0 test sürümü. |
| Yayımcı | Microsoft tarafından test yayımcı. |
| Kategori | Test kategorisi gibi **işlevsel** veya **güvenilirlik**. |
| Hedef Hizmetleri | Gibi VirtualMachines sınanan Hizmetleri |
| Açıklama | Test açıklaması. |
| Tahmini süre (dakika) | Testi çalıştırmak için geçen dakika cinsinden süre uzunluğu. |
| Bağlantılar | GitHub sorun İzleyicisi bağlantısı. |

## <a name="get-test-parameters"></a>Test parametreleri Al

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **görüntülemek parametreleri** test geçiş yeniden zamanlamak için.

**Parametreler**

| Ad | Açıklama |
| -- | -- |
| Test adı | Örneğin, oemupdate1806test test adı. |
| Test sürümü | Örneğin, 5.1.4.0 rest sürümü. |
| Test örneği kimliği | Test, örneğin, belirli örneğini tanımlayan bir GUID 20b20645-b400-4f0d-bf6f-1264d866ada9. |
| cloudAdminUser | Bulut Yöneticisi olarak, örneğin, kullanılan hesabın adını **cloudadmin**. |
| DiagnosticsContainerName | Tanılama kapsayıcı için kimliği 04dd3815-5f35-4158-92ea-698027693080. |

## <a name="get-test-operations"></a>Test işlemleri Al

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **görüntüleme işlemleri** test geçiş yeniden zamanlamak için. İşlemleri Özet bölmesi açılır.

## <a name="get-test-logs"></a>Test günlüklerini alma

1. [Test geçişi özeti açın](#open-the-test-pass-summary).
2. Seçin **indirme günlükleri** test geçiş yeniden zamanlamak için.  
    ReleaseYYYY-aa-DD.zip günlükleri indirmeleri içeren bir zip dosyası adı.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
