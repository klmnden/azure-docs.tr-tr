---
title: Azure Stack aboneliğine veya bir depolama hesabı için Depolama Gezgini'ni bağlayın | Microsoft Docs
description: Depolama Gezgini, bir Azure Stack aboneliğine bağlanma hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/14/2019
ms.openlocfilehash: 314304e75ce0f2586f41b71a889fa0185501b845
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58622017"
---
# <a name="connect-storage-explorer-to-an-azure-stack-subscription-or-a-storage-account"></a>Depolama Gezgini'ni Azure Stack aboneliğine veya bir depolama hesabına bağlama

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makalede, Azure Stack aboneliklerini ve Depolama Gezgini'ni kullanarak depolama hesapları nasıl bağlayacağınızı öğreneceksiniz. Azure Depolama Gezgini Windows, macOS ve Linux'ta Azure Stack depolama verilerle kolayca çalışmanızı sağlayan bir tek başına uygulamadır.

> [!NOTE]  
> Ve Azure Stack depolama alanından verileri taşımak birkaç araç vardır. Daha fazla bilgi için [veri aktarımı için Azure Stack depolama Araçları](azure-stack-storage-transfer.md).

Depolama Gezgini'ni henüz yüklemediyseniz [Depolama Gezgini'ni indirin](https://www.storageexplorer.com/) ve yükleyin.

Azure Stack aboneliğine veya bir depolama hesabına bağlandıktan sonra kullanabileceğiniz [Azure Depolama Gezgini makaleleri](../../vs-azure-tools-storage-manage-with-storage-explorer.md) Azure Stack verilerinizle çalışmaya. 

## <a name="prepare-for-connecting-to-azure-stack"></a>Azure Stack'e bağlamak için hazırlama

Azure Stack veya bir VPN bağlantısı için Depolama Gezgini'ni Azure Stack aboneliğine erişmek doğrudan erişmeniz gerekir. Azure Stack ile VPN bağlantısı kurma hakkında bilgi edinmek için bkz. [VPN ile Azure Stack’e Bağlanma](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

Azure Stack geliştirme Seti'ni (ASDK için), Azure Stack yetkili kök sertifikasını dışarı aktarmanız gerekmez.

> [!Note]  
> VPN aracılığıyla, ASDK bağlanıyorsanız ASDK için VPN Kurulum işlemi sırasında oluşturulan kök sertifikayı (CA.cer) kullanmayın.  Bu DER kodlu bir sertifika olduğundan ve depolama Gezgini'nin Azure Stack aboneliklerinizi izin vermez. Depolama Gezgini ile kullanmak üzere bir Base-64 kodlamalı sertifika vermek için aşağıdaki adımları izleyin.

### <a name="export-and-then-import-the-azure-stack-certificate"></a>Dışarı aktarma ve ardından Azure Stack sertifikayı alın.

Dışarı aktarın ve sonra Azure Stack sertifika için ASDK alın. Tümleşik sistem için sertifika herkese açık şekilde imzalanmış. Bu nedenle, sistem tümleşik Azure Stack için Depolama Gezgini bağlantı kurma sırasında bu adım gerekli değildir.

1. Açık `mmc.exe` bir Azure Stack ana makinesi veya Azure Stack VPN bağlantısı olan yerel makine üzerinde. 

2. İçinde **dosya**seçin **Ekle/Kaldır ek bileşenini**. Seçin **sertifikaları** kullanılabilir ek bileşenler de. 

3. Seçin **bilgisayar hesabı**ve ardından **sonraki**. Seçin **yerel bilgisayar**ve ardından **son**.

4.  Altında **konsol kökü\sertifikalı (yerel bilgisayar) \Trusted kök sertifika Yetkilileri\Sertifikalar**. Bulma **AzureStackSelfSignedRootCert**.

    ![mmc.exe dosyası ile Azure Stack kök sertifikasını yükleme](./media/azure-stack-storage-connect-se/add-certificate-azure-stack.png)

5. Sertifikaya sağ tıklayın, **tüm görevler** > **dışarı**ve ardından olan sertifikayı dışarı aktarmak için yönergeleri izleyin **Base-64 ile kodlanmış X.509 (. CER)**.

    Dışarı aktarılan sertifika sonraki adımda kullanılır.

6. Depolama Gezgini'ni başlatın ve görürseniz **Azure Storage'a Bağlan** iletişim kutusunda, iptal edin.

7. Üzerinde **Düzenle** menüsünde **SSL sertifikaları**ve ardından **sertifikaları içeri aktar**. Dosya seçici iletişim kutusunu kullanarak, önceki adımda dışarı aktardığınız sertifikayı açın.

    Sertifikayı içeri aktardıktan sonra Depolama Gezgini'ni yeniden başlatmanız istenir.

    ![Depolama Gezgini'ne sertifikayı içe aktarın](./media/azure-stack-storage-connect-se/import-azure-stack-cert-storage-explorer.png)

8. Depolama Gezgini'ni yeniden başlatıldıktan sonra seçin **Düzenle** menü ve olmadığını görmek için onay **hedef Azure Stack API'leri** seçilir. Aksi takdirde seçin **hedef Azure Stack**ve etkili olması için Depolama Gezgini'ni yeniden başlatın. Bu yapılandırma, Azure Stack ortamınıza uyum için gereklidir.

    ![Hedef Azure Stack’in seçili olduğundan emin olun](./media/azure-stack-storage-connect-se/target-azure-stack.png)

## <a name="connect-to-an-azure-stack-subscription-with-azure-ad"></a>Azure AD ile bir Azure Stack aboneliğine bağlanma

Depolama Gezgini, bir Azure Active Directory (Azure AD) hesaba ait bir Azure Stack aboneliğine bağlanmak için aşağıdaki adımları kullanın.

1. Storage explorer'ın sol bölmesinde seçin **hesaplarını yönetme**. 
    Oturumunuz, tüm Microsoft aboneliği görüntülenir.

2. Azure Stack aboneliğine bağlamak için seçin **Hesap Ekle**.

    ![Azure Stack hesabı ekleme](./media/azure-stack-storage-connect-se/add-azure-stack-account.png)

3. Azure depolama iletişim kutusu, Bağlan altında **Azure ortamı**seçin **Azure**, **Azure Çin**, **Azure Almanya**,  **Azure ABD kamu**, veya **yeni ortam Ekle**, kullanılan Azure Stack hesabı bağlıdır. Seçin **oturum** en az bir etkin Azure Stack aboneliğiyle ilişkili Azure Stack hesabıyla oturum açmak için.

    ![Azure depolamaya bağlanma](./media/azure-stack-storage-connect-se/azure-stack-connect-to-storage.png)

4. Bir Azure Stack hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure Stack abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini belirleyin ve ardından **Uygula**’yı seçin. (**Tüm abonelikler** onay kutusunun işaretlenmesi veya temizlenmesi, listelenen Azure Stack aboneliklerinin tamamının seçilmesini veya hiçbirinin seçilmemesini sağlar.)

    ![Özel Bulut Ortamı iletişim kutusunu doldurduktan sonra Azure Stack aboneliklerini seçin](./media/azure-stack-storage-connect-se/select-accounts-azure-stack.png)

    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![Azure Stack abonelik hesaplarını içeren depolama hesaplarının listesi](./media/azure-stack-storage-connect-se/azure-stack-storage-account-list.png)

## <a name="connect-to-an-azure-stack-subscription-with-ad-fs-account"></a>AD FS hesapla Azure Stack aboneliğine bağlanma

> [!Note]  
> Azure hizmeti (AD FS) federe oturum açma deneyimi, Depolama Gezgini 1.2.0 veya Azure Stack 1804 ya da daha yeni bir güncelleştirme ile daha yeni sürümleri destekler.
Depolama Gezgini, bir AD FS hesabına ait olan bir Azure Stack aboneliğine bağlanmak için aşağıdaki adımları kullanın.

1. Seçin **hesaplarını yönetme**. Gezgini için imzalı Microsoft abonelikleri listeler.
2. Seçin **Hesap Ekle** Azure Stack aboneliğine bağlanma.

    ![Hesap ekleme](media/azure-stack-storage-connect-se/add-an-account.png)

3. **İleri**’yi seçin. Azure depolama iletişim kutusu, Bağlan altında **Azure ortamı**seçin **kullanım özel ortam**, ardından **sonraki**.

    ![Azure depolamaya Bağlan](media/azure-stack-storage-connect-se/connect-to-azure-storage.png)

4. Azure Stack özel ortamıyla, gerekli bilgileri girin. 

    | Alan | Notlar |
    | ---   | ---   |
    | Ortam adı | Alan kullanıcı tarafından özelleştirilebilir. |
    | Azure Resource Manager uç noktası | Azure Resource Manager kaynak uç noktaları Azure Stack geliştirme Seti'ni örnekleri.<br>İşleçler için: https://adminmanagement.local.azurestack.external <br> Kullanıcılar için: https://management.local.azurestack.external |

    Çalışıyorsanız üzerinde Azure Stack tümleşik sistemi ve olmayan yönetim uç noktanıza bilmeniz, operatörünüze başvurun.

    ![Hesap ekleme](./media/azure-stack-storage-connect-se/custom-environments.png)

5. Seçin **oturum**, en az bir etkin Azure Stack aboneliğiyle ilişkili Azure Stack hesabına bağlanmak için.



6. Birlikte çalışmak istediğiniz Azure Stack aboneliklerini seçin. **Uygula**’yı seçin.

    ![Hesap yönetimi](./media/azure-stack-storage-connect-se/account-management.png)

    Sol bölmede seçili Azure Stack abonelikleriyle ilişkili depolama hesapları gösterilir.

    ![İlişkili aboneliklerin listesi](./media/azure-stack-storage-connect-se/list-of-associated-subscriptions.png)

## <a name="connect-to-an-azure-stack-storage-account"></a>Bir Azure Stack depolama hesabına bağlama

Depolama hesabı adı ve anahtar çiftini kullanarak bir Azure Stack depolama hesabınıza da bağlanabilirsiniz.

1. Storage explorer'ın sol bölmesinde hesapları Yönet'i seçin. Oturumunuz, tüm Microsoft hesapları görüntülenir.

    ![Hesap ekleme](./media/azure-stack-storage-connect-se/azure-stack-sub-add-an-account.png)

2. Azure Stack aboneliğine bağlamak için seçin **Hesap Ekle**.

    ![Hesap ekleme](./media/azure-stack-storage-connect-se/azure-stack-use-a-storage-and-key.png)

3. Bağlan iletişim kutusunda Azure depolama, seçin **bir depolama hesabı adı ve anahtarı kullan**.

4. Hesap adınızı giriş **hesap adı**, hesap anahtarını yapıştırın **hesap anahtarı** metin kutusunda **diğer (aşağıda girin)** içinde **depolama uç noktaları etki alanı** ve Azure Stack uç noktası girin.

    Bir Azure Stack uç nokta iki bölümleri içerir: bir bölge ve Azure Stack etki alanı adı. Azure Stack geliştirme Seti'ni varsayılan uç noktadır **local.azurestack.external**. Uç noktanız hakkında emin değilseniz, bulut yöneticinize başvurun.

    ![Adı ve anahtarı ekleme](./media/azure-stack-storage-connect-se/azure-stack-attach-name-and-key.png)

5. **Bağlan**’ı seçin.
6. Depolama hesabı başarıyla bağlandıktan sonra depolama hesabı ile görüntülenir (**harici, diğer**) adının.

    ![VMWINDISK](./media/azure-stack-storage-connect-se/azure-stack-vmwindisk.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama Gezgini ile çalışmaya başlama](../../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Azure Stack Depolama: farklılıklar ve dikkat edilmesi gerekenler](azure-stack-acs-differences.md)
* Azure depolama hakkında daha fazla bilgi için bkz: [Microsoft Azure Depolama'ya giriş](../../storage/common/storage-introduction.md)
