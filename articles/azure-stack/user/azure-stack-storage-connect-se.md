---
title: Bir Azure yığın abonelik veya depolama hesabı için Depolama Gezgini bağlanma | Microsoft Docs
description: Depolama Gezgini bir Azure yığın aboneliğine bağlanma öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/20/2018
ms.author: mattbriggs
ms.reviewer: xiaofmao
ms.openlocfilehash: 8b670ec7040aab7eca26d411c9e31a934052be19
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Depolama Gezgini Azure yığın abonelik veya depolama hesabı bağlayın

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Storage Gezgini Windows, macOS ve Linux Azure yığın depolama verilerle kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Azure yığın depolama gelen ve giden veri taşımak kullanılabilen çeşitli araçlar vardır. Daha fazla bilgi için bkz: [veri aktarımı için Azure yığın depolama Araçları](azure-stack-storage-transfer.md).

Bu makalede, Azure yığın abonelikleri ve Depolama Gezgini'ni kullanarak depolama hesapları için nasıl bağlayacağınızı öğrenin. 

Depolama Gezgini henüz yüklemediyseniz [karşıdan](http://www.storageexplorer.com/) ve yükleyin.

Bir Azure yığın abonelik veya bir depolama hesabı bağlandıktan sonra kullanabileceğiniz [Azure Storage Gezgini makaleleri](../../vs-azure-tools-storage-manage-with-storage-explorer.md) Azure yığın verilerle çalışmak için. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Azure yığınına bağlanmak için hazırlama

Azure yığın veya bir VPN bağlantısı için Depolama Gezgini Azure yığın abonelik erişmek için doğrudan erişim gerekir. Azure Stack ile VPN bağlantısı kurma hakkında bilgi edinmek için bkz. [VPN ile Azure Stack’e Bağlanma](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

Azure yığın Geliştirme Seti için Azure yığın yetkilisi kök sertifikasını dışarı aktarmanız gerekmez.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Dışarı aktarma ve Azure yığın sertifika içeri aktarma

1. Açık `mmc.exe` bir Azure yığın ana makine ya da Azure yığın VPN bağlantısı olan yerel makine üzerinde. 

2. İçinde **dosya**seçin **Ekle/Kaldır ek bileşenini**ve ardından ekleyin **sertifikaları** yönetmek için **kullanıcı hesabım**.

3. Altında **konsol Root\Certificated (yerel bilgisayar) \Trusted Root Certification Authorities\Certificates** Bul **AzureStackSelfSignedRootCert**.

    ![mmc.exe dosyası ile Azure Stack kök sertifikasını yükleme](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

4. Sertifikayı sağ tıklatın, seçin **tüm görevler** > **verme**ve ardından sertifikayı dışarı aktarmak için yönergeleri izleyin **Base-64 ile kodlanmış X.509 (. CER)**.  

    Dışarı aktarılan sertifika sonraki adımda kullanılır.

5. Depolama Gezgini'ni başlatın ve görürseniz **Azure Storage Bağlan** iletişim kutusunda, iptal edin.

6. Üzerinde **Düzenle** menüsündeki **SSL sertifikalarını**ve ardından **alma sertifikaları**. Dosya seçici iletişim kutusunu kullanarak, önceki adımda dışarı aktardığınız sertifikayı açın.

    İçeri aktardıktan sonra Depolama Gezgini'ni yeniden başlatmanız istenir.

    ![Sertifika depolama Explorer'a alma](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

7. Depolama Gezgini yeniden başlatıldıktan sonra Seç **Düzenle** menüsünde ve emin olun **hedef Azure yığın** seçilir. İşaretli değilse işaretleyin ve değişikliğin etkili olması için Depolama Gezgini’ni yeniden başlatın. Bu yapılandırma, Azure Stack ortamınıza uyum için gereklidir.

    ![Hedef Azure Stack’in seçili olduğundan emin olun](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription"></a>Bir Azure Stack aboneliğine bağlanma

Şimdi bir Azure yığın abonelik için Depolama Gezgini bağlanmak hazırsınız.

1. Depolama Gezgini sol bölmesinde seçin **hesaplarını yönetme**.  
    Açtığınız tüm Microsoft abonelik görüntülenir.

2. Azure yığın aboneliğine bağlanmayı seçin **Hesap Ekle**.

    ![Azure Stack hesabı ekleme](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. Azure Storage iletişim kutusu, Bağlan altında **Azure ortamı**seçin **Azure** veya **Azure Çin**, kullanıldığı Azure yığın hesabında bağlıdır ve ardından **oturum**. En az bir etkin Azure yığın aboneliği ile ilişkili Azure yığın hesapla oturum için.

    ![Azure depolama birimini bağlayın](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Bir Azure Stack hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure Stack abonelikleriyle doldurulur. Çalışmak istediğiniz Azure yığın abonelikleri seçin ve Uygula'yı seçin. (Seçerek veya tümü veya hiçbiri listelenen Azure yığın abonelikleri seçerek tüm abonelikleri onay kutusunu değiştirme düğmelerini temizleyerek.)

    ![Özel Bulut Ortamı iletişim kutusunu doldurduktan sonra Azure Stack aboneliklerini seçin](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)  

    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Azure Stack abonelik hesaplarını içeren depolama hesaplarının listesi](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Bir Azure yığın depolama hesabına bağlanma

Ayrıca, depolama hesabı adı ve anahtar çifti kullanarak Azure yığın depolama hesabına bağlanabilirsiniz.

1.  Depolama Gezgini sol bölmede hesapları Yönet'i seçin. Açtığınız tüm Microsoft hesapları görüntülenir.

    ![Hesap ekleme](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2.  Azure yığın aboneliğine bağlanmayı seçin **Hesap Ekle**.
 
    ![Hesap ekleme](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3.  Azure Storage iletişim kutusu Bağlan seçin **bir depolama hesabı adı ve anahtar kullanmak**.

4. Hesap adınızı girin **hesap adı**, hesap anahtarı içine yapıştırma **hesap anahtarı** metin kutusunda **diğer (aşağıda girin)** içinde **depolama uç noktaları etki alanı** ve Azure yığın uç noktası girin.  

    Bir Azure yığın uç nokta iki bölümleri içerir: bir bölge ve Azure yığın etki alanı adı. Azure yığın Development Kit'te varsayılan uç noktadır **local.azurestack.external**. Uç noktanız hakkında emin değilseniz, bulut yöneticinize başvurun.

    ![Ad ve anahtar ekleme](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5.  **Bağlan**’ı seçin.
6.  Depolama hesabı başarıyla eklendi sonra depolama hesabı ile görüntülenir (**dış, diğer**) adının eklenir.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini ile çalışmaya başlama](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure yığın depolama: farklar ve ilgili önemli noktalar](azure-stack-acs-differences.md)
* Azure Storage hakkında daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md)
