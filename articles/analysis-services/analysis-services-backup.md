---
title: Azure Analysis Services veritabanı yedekleme ve geri yükleme | Microsoft Docs
description: Yedekleme ve geri yükleme bir Azure Analysis Services veritabanı açıklar.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: ec213d5c223180825ea0eabe95881002432b92e9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

Azure Analysis Services tablolu model veritabanlarını yedeklemek çok şirket içi Analysis Services aynıdır. Yedekleme dosyalarını depoladığınız birincil farktır. Yedek dosya kaydedildi, bir kapsayıcı bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md). Sunucunuz için depolama ayarlarını yapılandırırken oluşturulabilir veya zaten bir depolama hesabı ve kapsayıcı kullanabilirsiniz.

> [!NOTE]
> Bir depolama hesabı oluştururken bir ek hizmet ücretlerin alınmasına neden olabilir. Daha fazla bilgi için bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Yedeklemeler bir abf uzantısıyla kaydedilir. Bellek içi tablolu modeller için model verileri ve meta veriler depolanır. DirectQuery tablolu modeller için yalnızca model meta verilerini depolanır. Yedeklemeleri sıkıştırılmış ve, belirlediğiniz seçeneklere bağlı olarak şifrelenir. 



## <a name="configure-storage-settings"></a>Depolama ayarlarını yapılandırın
Yedekleme önce sunucunuzun depolama ayarlarını yapılandırmanız gerekir.


### <a name="to-configure-storage-settings"></a>Depolama ayarlarını yapılandırmak için
1.  Azure portalında > **ayarları**, tıklatın **yedekleme**.

    ![Yedekleme ayarları](./media/analysis-services-backup/aas-backup-backups.png)

2.  Tıklatın **etkin**, ardından **depolama ayarlarını**.

    ![Etkinleştirme](./media/analysis-services-backup/aas-backup-enable.png)

3. Depolama hesabınızı seçin veya yeni bir tane oluşturun.

4. Bir kapsayıcı seçin veya yeni bir tane oluşturun.

    ![Kapsayıcı seçin](./media/analysis-services-backup/aas-backup-container.png)

5. Yedekleme ayarlarınızı kaydedin.

    ![Yedekleme ayarlarını Kaydet](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Backup

### <a name="to-backup-by-using-ssms"></a>SSMS kullanarak yedekleme

1. SSMS, bir veritabanına sağ tıklayın > **yedekleme**.

2. İçinde **Backup Database** > **yedekleme dosyası**, tıklatın **Gözat**.

3. İçinde **Kaydet dosyası olarak** iletişim kutusunda, klasör yolunu doğrulayın ve ardından yedekleme dosyası için bir ad yazın. 

4. İçinde **Backup Database** iletişim, seçenekleri belirleyin.

    **Dosya izin üzerine** -yedekleme dosyalarının aynı ada sahip üzerine yazmak için bu seçeneği belirleyin. Bu seçenek seçilmezse kaydetmekte olduğunuz dosya aynı konumda zaten bir dosya olarak aynı ada sahip olamaz.

    **Sıkıştırma uygulamak** -yedekleme dosyası sıkıştırmak için bu seçeneği belirleyin. Sıkıştırılmış yedek dosyaları disk alanından tasarruf, ancak biraz daha yüksek CPU kullanımı gerektirir. 

    **Yedek dosyayı şifrelemek** -yedekleme dosyasını şifrelemek için bu seçeneği belirleyin. Bu seçenek, yedekleme dosyasını güvenli hale getirmek için bir kullanıcı tarafından sağlanan parola gerektirir. Parola, yedekleme verilerini başka bir yöntemle bir geri yükleme işlemi daha okuma önler. Yedeklemeleri şifrelemek seçerseniz, parolayı güvenli bir yerde saklayın.

5. Tıklatın **Tamam** oluşturma ve yedekleme dosyasını kaydedin.


### <a name="powershell"></a>PowerShell
Kullanım [yedekleme ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet'i.

## <a name="restore"></a>Geri Yükleme
Geri yüklerken, yedekleme dosyası, sunucunuz için yapılandırdığınız depolama hesabı olması gerekir. Bir yedekleme dosyası, depolama hesabınıza bir şirket içi konumundan taşımanız gerekirse, kullanın [Microsoft Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) veya [AzCopy](../storage/common/storage-use-azcopy.md) komut satırı yardımcı programı. 



> [!NOTE]
> Bir şirket içi sunucusundan geri yükleme, tüm etki alanı kullanıcıları modelinin rollerini kaldırın ve bunları geri rolleri Azure Active Directory Kullanıcıları olarak ekleyin.
> 
> 

### <a name="to-restore-by-using-ssms"></a>SSMS kullanarak geri yüklemek için

1. SSMS, bir veritabanına sağ tıklayın > **geri**.

2. İçinde **Backup Database** iletişim, **yedekleme dosyası**, tıklatın **Gözat**.

3. İçinde **veritabanı dosyalarını bulmak** iletişim kutusunda, geri yüklemek istediğiniz dosyayı seçin.

4. İçinde **geri yükleme veritabanı**, veritabanını seçin.

5. Seçeneklerini belirtin. Güvenlik seçenekleri yedekleme yapılırken kullanılan yedekleme seçeneklerini eşleşmesi gerekir.


### <a name="powershell"></a>PowerShell

Kullanım [geri yükleme ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet'i.


## <a name="related-information"></a>İlgili bilgiler

[Azure depolama hesapları](../storage/common/storage-create-storage-account.md)  
[Yüksek düzey kullanılabilirlik](analysis-services-bcdr.md)     
[Azure Analysis Services yönetme](analysis-services-manage.md)
