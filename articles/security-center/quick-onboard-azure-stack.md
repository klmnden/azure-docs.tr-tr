---
title: Azure Güvenlik Merkezi Hızlı Başlangıç - yerleşik güvenlik Merkezi'ne Azure Stack sanal makinelerinizi | Microsoft Docs
description: Bu hızlı başlangıçta bir Azure Stack sanal makinelerde Azure İzleyici, güncelleştirme ve yapılandırma yönetimi sanal makine uzantısı sağlamasını yapma gösterilmektedir.
services: security-center
documentationcenter: na
author: monhaber
manager: dsavage
editor: ''
ms.assetid: 8982348a-0624-40c7-8a1e-642a523c7f6b
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: monhaber
ms.openlocfilehash: 9efd6514b722168f8ecb1235159e7463ce318118
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904024"
---
# <a name="quickstart--onboard-your-azure-stack-virtual-machines-to-security-center"></a>Hızlı Başlangıç:  Yerleşik Güvenlik Merkezi, Azure Stack sanal makinelere
Sonra yerleşik Azure abonelik özelliklerini ekleyerek Azure Stack üzerinde çalıştırılan sanal makinelerinizi korumak için Güvenlik Merkezi'ni etkinleştirebilirsiniz **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** sanal makine uzantısını Azure Stack marketini.

Bu hızlı başlangıçta nasıl ekleneceği gösterilmektedir **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** bir sanal makinede sanal makine uzantısı (Linux ve Windows her ikisi de desteklenir) Azure Stack üzerinde çalışıyor.

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Güvenlik Merkezi'nin standart katmanında bir Azure aboneliğiniz varsa bu hızlı başlangıcı başlatmadan önce. Yükseltme yönergeleri için bkz. [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md). 30 gün boyunca Güvenlik Merkezi standart katmanı hiçbir ücret ödemeden deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="add-an-azure-stack-virtual-machine"></a>Bir Azure Stack sanal makine ekleyin

1. [Azure portal](https://azure.microsoft.com/features/azure-portal/) oturum açın.
2. **Microsoft Azure** menüsünde **Güvenlik Merkezi**’ni seçin. **Güvenlik Merkezi - Genel Bakış** açılır. 

   ![Güvenlik Merkezine genel bakış][2]

3. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
4. **Başlangıç** sekmesini seçin.

   ![başlarken][3]

5. **Yeni Azure dışı bilgisayarlar ekle** altında, **Yapılandır**’a tıklayın. Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanına veya Azure Stack VM için güvenlik verilerini raporlamaya istediğiniz başka bir çalışma alanı seçin.

    ![Azure olmayan bilgisayar ekleme](./media/quick-onboard-windows-computer/non-azure.png)

   **Doğrudan aracı** dikey penceresi, aracı yükleme bağlantısını içeren açılır ve anahtarları aracının yapılandırılmasında kullanılacak çalışma alanı Kimliğiniz için.

   >[!NOTE]
   > Aracıyı el ile karşıdan yüklemek gerekmez. Aracı, bir VM uzantısı'nda aşağıdaki adımları olarak yüklenir.

6. **Çalışma Alanı Kimliği**’nin sağ tarafında, kopyala simgesini seçin ve kimliği Not Defteri’ne yapıştırın.

7. **Birincil Anahtar**’ın sağ tarafında, kopyala simgesini seçin ve anahtarı Not Defteri’ne yapıştırın.

## <a name="add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines"></a>Mevcut Azure Stack sanal makinelerinizi sanal makine uzantısı Ekle
Artık eklemelisiniz **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** sanal makine uzantısı, Azure Stack üzerinde çalışan sanal makineleri için.

1. Yeni bir tarayıcı sekmesinde oturum açın, **Azure Stack** portalı.
2. Git **sanal makineler** sayfasında, Güvenlik Merkezi ile korumak istediğiniz sanal makineyi seçin. Azure Stack üzerinde sanal makine oluşturma hakkında daha fazla bilgi için bkz: [Windows sanal makineler için bu hızlı başlangıçta](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) veya [Bu hızlı başlangıçta Linux sanal makineleri için](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal).
3. **Uzantılar**'ı seçin. Bu sanal makinede yüklü sanal makine uzantıları listesi gösterilir.
4. Tıklayın **Ekle** sekmesi. **Yeni kaynak** menü dikey penceresi açılır ve kullanılabilir sanal makine uzantıları listesini gösterir. 
5. Seçin **Azure İzleyici, güncelleştirme ve yapılandırma yönetimi** uzantısı ve tıklatın **Oluştur**. **Uzantı yükleme** yapılandırma dikey penceresi açılır.
6. Üzerinde **uzantı yükleme** yapılandırma dikey penceresinde, Yapıştır **çalışma alanı kimliği** ve **çalışma alanı anahtarı (birincil anahtar)** önceki yordamda Not Defteri'ne kopyaladığınız.
7.  Gerekli yapılandırma ayarlarını sağlamayı bitiş olduğunda tıklayın **Tamam**.
8. Uzantı yükleme işlemi tamamlandıktan sonra durumu olarak görünür **sağlama başarılı**. Bu sanal makinenin Güvenlik Merkezi Portalı'nda görünmesi bir saate kadar sürebilir.

Yükleme ve Windows için aracı yapılandırma hakkında daha fazla bilgi için bkz. [bağlanmak Windows bilgisayarları](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Linux için aracı sorunları, sorun giderme bkz [Azure Log Analytics Linux Aracısı sorunlarını giderme](../azure-monitor/platform/agent-linux-troubleshoot.md).

Artık Azure VM’lerinizi ve Azure olmayan bilgisayarlarınızı tek bir yerde izleyebilirsiniz. Azure Portal'da Güvenlik Merkezi **işlem**, sahip olduğunuz tüm VM'ler ve bilgisayarlar, önerilerle birlikte genel bir bakış. Güvenlik Merkezi de güvenlik uyarılarında bu bilgisayarlar için herhangi bir algılama ortaya çıkarır.

  ![Bilgi İşlem dikey penceresi][6]

**Bilgi İşlem** dikey penceresinde gösterilen iki türlü simge vardır:

![simge1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Azure olmayan bilgisayar 

![simge2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM (Azure Stack sanal makineleri bu gruba gösterilir)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli olmadığında, Azure Stack portal aracılığıyla sanal makineden uzantıyı kaldırabilirsiniz.

Uzantıyı kaldırmak için:

1. Açık **Azure Stack portalı**.
2. Git **sanal makineler** sayfasında, uzantıyı kaldırmak istediğiniz sanal makineyi seçin.
3. Seçin **uzantıları**, uzantıyı seçin **Microsoft.EnterpriseCloud.Monitoring**.
4. Tıklayarak **kaldırma**ve tıklayarak seçimi onaylayın **Evet**.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Stack üzerinde çalışan bir sanal makinede Azure İzleyici, güncelleştirme ve yapılandırma yönetimi uzantısı sağlandı. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirme ile ilgili öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
