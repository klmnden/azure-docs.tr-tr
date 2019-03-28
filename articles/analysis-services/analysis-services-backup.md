---
title: Azure Analysis Services veritabanını yedekleme ve geri yükleme | Microsoft Docs
description: Yedekleme ve bir Azure Analysis Services veritabanı geri yükleme işlemini açıklamaktadır.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 31e8e65b382a3a6bcad2998a0babdf9605dc4968
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539114"
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Analysis Services'da tablosal model veritabanlarını yedeklemeye kadar şirket içi Analysis Services ile aynıdır. Birincil, yedek dosyalarını depoladığınız farktır. Yedek dosya kaydedildi, bir kapsayıcıya bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md). Sunucunuzun depolama ayarlarını yapılandırırken oluşturulabilir veya bir depolama hesabı ve kapsayıcı zaten kullanabilirsiniz.

> [!NOTE]
> Bir depolama hesabı oluşturma, bir hizmet ücretlerinin alınmasına neden olabilir. Daha fazla bilgi için bkz. [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Yedeklemeleri .abf uzantısıyla kaydedilir. Bellek içi tablosal modeller model verileri hem meta veriler depolanır. DirectQuery için tablosal modeller, yalnızca model meta verilerini depolanır. Yedeklemeler, sıkıştırılır ve şifrelenir, belirlediğiniz seçeneklere bağlı olarak.


## <a name="configure-storage-settings"></a>Depolama ayarlarını yapılandırma
Yedekleme önce sunucunuzun depolama ayarlarını yapılandırmanız gerekir.


### <a name="to-configure-storage-settings"></a>Depolama ayarlarını yapılandırmak için
1.  Azure portalında > **ayarları**, tıklayın **yedekleme**.

    ![Yedekleme ayarları](./media/analysis-services-backup/aas-backup-backups.png)

2.  Tıklayın **etkin**, ardından **depolama ayarlarını**.

    ![Etkinleştirme](./media/analysis-services-backup/aas-backup-enable.png)

3. Depolama hesabınızı seçin veya yeni bir tane oluşturun.

4. Bir kapsayıcı seçin veya yeni bir tane oluşturun.

    ![Kapsayıcı seçme](./media/analysis-services-backup/aas-backup-container.png)

5. Yedekleme ayarlarınızı kaydedin.

    ![Yedekleme ayarlarını Kaydet](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Backup

### <a name="to-backup-by-using-ssms"></a>SSMS kullanarak yedekleme

1. SSMS'de, bir veritabanına sağ tıklayın > **yedekleme**.

2. İçinde **Backup Database** > **yedekleme dosyası**, tıklayın **Gözat**.

3. İçinde **kaydetme dosyası olarak** iletişim kutusunda, klasör yolunu doğrulayın ve ardından yedekleme dosyası için bir ad yazın. 

4. İçinde **Backup Database** iletişim kutusunda seçenekleri belirleyin.

    **Dosya izin üzerine** -yedekleme dosyalarının aynı ada sahip üzerine yazmak için bu seçeneği belirleyin. Bu seçenek seçilmezse kaydetmekte olduğunuz dosya aynı konumda zaten bir dosya olarak aynı ada sahip olamaz.

    **Sıkıştırma uygulamak** -yedekleme dosyasını sıkıştırmak için bu seçeneği belirleyin. Sıkıştırılmış yedekleme dosyalarını, disk alanından tasarruf, ancak biraz daha yüksek CPU kullanımı gerektirir. 

    **Yedekleme dosyasını şifrelemek** -yedekleme dosyasını şifrelemek için bu seçeneği belirleyin. Bu seçenek, yedekleme dosyasını güvenli hale getirmek için bir kullanıcı tarafından sağlanan parola gerektirir. Parola, yedekleme verilerini geri yükleme işlemi daha başka bir yolla okuma engeller. Yedeklemeleri şifrelemek isterseniz, parolayı güvenli bir konumda depolayın.

5. Tıklayın **Tamam** oluşturma ve yedekleme dosyasını kaydedin.


### <a name="powershell"></a>PowerShell
Kullanım [yedekleme ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet'i.

## <a name="restore"></a>Geri Yükleme
Geri yüklerken, yedekleme, dosya sunucunuz için yapılandırdığınız depolama hesabında olmalıdır. Bir yedekleme dosyası, depolama hesabınıza bir şirket içi konumdan taşımak ihtiyacınız varsa [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) veya [AzCopy](../storage/common/storage-use-azcopy.md) komut satırı yardımcı programı. 



> [!NOTE]
> Bir şirket içi sunucusunu geri yükleme, tüm etki alanı kullanıcıları modelin rollerini kaldırın ve bunları Azure Active Directory kullanıcı rollerine eklemeniz gerekir.
> 
> 

### <a name="to-restore-by-using-ssms"></a>SSMS kullanarak geri yüklemek için

1. SSMS'de, bir veritabanına sağ tıklayın > **geri**.

2. İçinde **Backup Database** iletişim, **yedekleme dosyası**, tıklayın **Gözat**.

3. İçinde **veritabanı dosyalarını Bul** iletişim kutusunda, geri yüklemek istediğiniz dosyayı seçin.

4. İçinde **geri yükleme veritabanı**, veritabanını seçin.

5. Seçeneklerini belirtin. Güvenlik seçenekleri yedeklerken kullanılan yedekleme seçeneklerini eşleşmesi gerekir.


### <a name="powershell"></a>PowerShell

Kullanım [geri yükleme-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet'i.


## <a name="related-information"></a>İlgili bilgiler

[Azure depolama hesapları](../storage/common/storage-create-storage-account.md)  
[Yüksek düzey kullanılabilirlik](analysis-services-bcdr.md)     
[Azure Analysis Services'ı yönetme](analysis-services-manage.md)
