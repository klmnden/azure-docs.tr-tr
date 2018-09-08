---
title: Hizmet olarak Azure Stack doğrulama orijinal ekipman üreticisi (OEM) paketleri doğrulama | Microsoft Docs
description: Orijinal ekipman üreticisi (OEM) bir hizmet olarak doğrulama paketlerle denetleme hakkında bilgi edinin.
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
ms.openlocfilehash: cefa32c35df4e87d4d2b983ee8c4a16dc065e774
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160464"
---
# <a name="validate-oem-packages"></a>OEM paketleri doğrula

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bellenim veya sürücülerinin tamamlanan çözümü doğrulama için bir değişiklik olduğunda yeni bir OEM paketi test edebilirsiniz. Test paketiniz geçtiğinde, Microsoft tarafından imzalanır. Testiniz, Windows Server logo ve bilgisayarları testlerini geçtiğini bellenim ve sürücüleri ile güncelleştirilmiş OEM uzantı paketi içermelidir.

Tüm test sonucu ile 24 saat içinde son **başarılı**. Testin bir sonucunu varsa **başarısız**, hatanın dosya [Microsoft Collaborate](https://aka.ms/collaborate) ve e-posta göndererek Microsoft'a bildirmediğiniz [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

## <a name="get-your-oem-package-signed"></a>İmzalı, OEM paketini alma

1. Geçerli aylık güncelleştirmesinin uygulandığından emin olun. En son sürümde son sürümü için bkz [Azure Stack operatör belgeleri > genel bakış > Sürüm Notları](https://docs.microsoft.com/azure/azure-stack/) .

    Bir adlandırma kuralı kullanarak Microsoft Azure Stack yazılım güncelleştirmeleri belirlenmiş, örneğin, Mart 2018 için 1803 belirten güncelleştirme. Azure Stack güncelleştirme ilkesi hakkında daha fazla bilgi için kullanılabilen temposu ve sürüm notları, bakın [Azure Stack hizmet İlkesi](https://docs.microsoft.com/azure/azure-stack/azure-stack-servicing-policy).

1. Sistem sağlık durumu denetleyin **Test AzureStack** olarak açıklanan [Azure Stack için bir doğrulama testi Çalıştır. Herhangi bir test başlatmadan önce tüm uyarıları ve hataları düzeltin.

2. İçinde [doğrulama portalı](https://azurestackvalidation.com), varolan bir çözümü seçin. Çözümünüzü eklemediyseniz bkz [yeni çözüm ekleme](azure-stack-vaas-validate-solution-new.md#add-a-new-solution).

3. Seçin **Başlat** üzerinde **paket doğrulamaları** yeni bir iş akışını başlatmak için bir kutucuk.

    ![Paket doğrulamaları](media/image3.png)

4.  Bir tanılama bağlantı dizesini belirtin. Yönergeler için [bir depolama hesabı ayarlama](azure-stack-vaas-set-up-account.md).

    OEM uzantı paketinin her paket doğrulama çalışması için belirtilmiş olması gerekir. Çözümü Azure Stack dağıtım sırasında yüklenen OEM paketi belirtin. Yönergeler için [günlüklerini depolamak için bir Azure depolama blob oluşturma](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    Ortam değişkenlerini içeren bir JSON dosyası, giriş veri girişi hataları önlemek çalıştırma için gerekli alanlar için tamamlamak için kullanılmalıdır. Yönergeler için [Azure Stack dağıtım yapılandırma dosyası alma](azure-stack-vaas-parameters.md).

5. Testleri çalıştırın.

6. Tüm testler başarıyla tamamladıktan sonra çözüm ve paket doğrulama adını gönder [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com) paket imzalama istemek için.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi edinmek için [hizmet olarak Azure Stack doğrulama](https://docs.microsoft.com/azure/azure-stack/partner).
