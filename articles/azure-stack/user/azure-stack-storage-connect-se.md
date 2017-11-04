---
title: "Depolama Gezgini bir Azure yığın aboneliğine bağlanma"
description: "Bir Azure yığın aboneliğine depolama Exporer bağlanma öğrenin"
services: azure-stack
documentationcenter: 
author: xiaofmao
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: xiaofmao
ms.openlocfilehash: c7e6d70148d39fd74f6409a0a239833f8e9f7614
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription"></a>Depolama Gezgini bir Azure yığın aboneliğine bağlanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Depolama Gezgini (Önizleme), Windows, macOS ve Linux Azure yığın depolama verilerle kolayca çalışmanızı sağlayan bir tek başına uygulamadır. Azure yığın depolama gelen ve giden veri taşımak için çeşitli araçlar kullanılabilir vardır. Daha fazla bilgi için bkz: [veri aktarımı için Azure yığın depolama Araçları](azure-stack-storage-transfer.md).

Bu makalede, Azure yığın depolama hesaplarınıza Depolama Gezgini'ni kullanarak bağlanmak nasıl öğrenin. 

Depolama Gezgini henüz yüklemediyseniz [karşıdan](http://www.storageexplorer.com/) ve yükleyin.

Azure yığın aboneliğinize bağlandıktan sonra kullanabileceğiniz [Azure Storage Gezgini makaleleri](../../vs-azure-tools-storage-manage-with-storage-explorer.md) Azure yığın verilerle çalışmak için. 

## <a name="prepare-an-azure-stack-subscription"></a>Bir Azure yığın abonelik hazırlama

Azure yığın ana makinenin Masaüstü veya bir VPN bağlantısı Azure yığın abonelik erişmek Depolama Gezgini için erişimin gerekir. Azure Stack ile VPN bağlantısı kurma hakkında bilgi edinmek için bkz. [VPN ile Azure Stack’e Bağlanma](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

Azure yığın Geliştirme Seti için Azure yığın yetkilisi kök sertifikasını dışarı aktarmanız gerekmez.

### <a name="to-export-and-then-import-the-azure-stack-certificate"></a>Azure yığın sertifika almak ve vermek için

1. Açık `mmc.exe` bir Azure yığın ana makine ya da Azure yığın VPN bağlantısı olan yerel makine üzerinde. 

2. İçinde **dosya**seçin **Ekle/Kaldır ek bileşenini**ve ardından ekleyin **sertifikaları** yönetmek için **kullanıcı hesabım**.



3. Altında **konsol Root\Certificated (yerel bilgisayar) \Trusted Root Certification Authorities\Certificates** Bul **AzureStackSelfSignedRootCert**.

    ![mmc.exe dosyası ile Azure Stack kök sertifikasını yükleme][25]

4. Sertifikayı sağ tıklatın, seçin **tüm görevler** > **verme**ve ardından sertifikayı dışarı aktarmak için yönergeleri izleyin **Base-64 ile kodlanmış X.509 (. CER)**.  

    Dışarı aktarılan sertifika sonraki adımda kullanılır.
5. Depolama Gezgini (Önizleme) başlatın ve görürseniz **Azure Storage Bağlan** iletişim kutusunda, iptal edin.

6. Üzerinde **Düzenle** menüsündeki **SSL sertifikalarını**ve ardından **alma sertifikaları**. Dosya seçici iletişim kutusunu kullanarak, önceki adımda dışarı aktardığınız sertifikayı açın.

    İçeri aktardıktan sonra Depolama Gezgini'ni yeniden başlatmanız istenir.

    ![Depolama Gezgini’ne (Önizleme) sertifika aktarma][27]

Şimdi bir Azure yığın abonelik için Depolama Gezgini bağlanmak hazırsınız.

### <a name="to-connect-an-azure-stack-subscription"></a>Bir Azure yığın abonelik bağlanmak için


1. Depolama Gezgini (Önizleme) yeniden başlatıldıktan sonra **Düzenle** menüsünü seçin ve **Hedef Azure Stack** seçeneğinin işaretli olduğundan emin olun. İşaretli değilse işaretleyin ve değişikliğin etkili olması için Depolama Gezgini’ni yeniden başlatın. Bu yapılandırma, Azure Stack ortamınıza uyum için gereklidir.

    ![Hedef Azure Stack’in seçili olduğundan emin olun][28]

7. Sol bölmede **Hesapları Yönet**’i seçin.  
    Oturum açtığınız tüm Microsoft hesapları görüntülenir.

8. Azure Stack hesabına bağlanmak için **Hesap ekle**’yi seçin.

    ![Azure Stack hesabı ekleme][29]

9. İçinde **Azure Storage Bağlan** iletişim kutusunda **Azure ortamı**seçin **kullanım Azure yığın ortamını**ve ardından **sonraki**.

10. En az bir etkin Azure yığın aboneliği ile ilişkili Azure yığın hesabınızla oturum açın doldurun **Azure yığın ortamına oturum** iletişim kutusu.  

    Her bir alanın ayrıntıları aşağıdaki gibidir:

    * **Ortam adı**: Bu alan kullanıcı tarafından özelleştirilebilir.
    * **ARM kaynak uç noktası**: Azure Resource Manager kaynak uç noktası örnekleri:

        * Bulut işleci için:<br> https://adminmanagement.Local.azurestack.external   
        * Kiracı için:<br> https://Management.Local.azurestack.external
 
    * **Kiracı kimliği**: isteğe bağlı. Değer yalnızca dizinin belirtilmesi zorunlu olduğunda verilir.

12. Bir Azure Stack hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure Stack abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini belirleyin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler** onay kutusunun işaretlenmesi veya temizlenmesi, listelenen Azure Stack aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Özel Bulut Ortamı iletişim kutusunu doldurduktan sonra Azure Stack aboneliklerini seçin][30]  
    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Azure Stack abonelik hesaplarını içeren depolama hesaplarının listesi][31]

## <a name="next-steps"></a>Sonraki adımlar
* [Depolama Gezgini (Önizleme) ile çalışmaya başlama](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure yığın depolama: farklar ve ilgili önemli noktalar](azure-stack-acs-differences.md)


* Azure Storage hakkında daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../../storage/common/storage-introduction.md)

[25]: ./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png
[26]: ./media/azure-stack-storage-connect-se/export-root-cert-azure-stack.png
[27]: ./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png
[28]: ./media/azure-stack-storage-connect-se/select-target-azure-stack.png
[29]: ./media/azure-stack-storage-connect-se/add-azure-stack-account.png
[30]: ./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png
[31]: ./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png
