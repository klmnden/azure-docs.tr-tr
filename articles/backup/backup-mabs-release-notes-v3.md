---
title: Microsoft Azure Backup sunucusu v3 için sürüm notları
description: Bu makalede, MABS v3 için geçici çözümler ve bilinen sorunlar hakkında bilgi sağlar.
services: backup
author: JYOTHIRMAISURI
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 11/22/2018
ms.author: v-jysur
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: d37245d7eed39ee9d219578db9e0a50d758ba9a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60499707"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Microsoft Azure Backup sunucusu için sürüm notları
Bu makalede, Microsoft Azure Backup sunucusu (MABS) V3 için bilinen sorunlar ve geçici çözümler sağlar.

##  <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Yedekleme ve kurtarma başarısız kümelenmiş iş yükleri için

**Açıklama:** Yedekleme/geri yükleme veritabanı kullanılabilirlik grubundaki (DAG) Hyper-V kümesine, SQL kümesi (SQL Always On) veya Exchange gibi kümelenmiş veri kaynakları için MABS V2 MABS V3 yükseltildikten sonra başarısız oldu.

**Geçici çözüm:** Bunu önlemek için SQL Server Management Studio (SSMS) açın) ve DPM DB'yi üzerinde aşağıdaki SQL betiğini çalıştırın:


    IF EXISTS (SELECT * FROM dbo.sysobjects
        WHERE id = OBJECT_ID(N'[dbo].[tbl_PRM_DatasourceLastActiveServerMap]')
        AND OBJECTPROPERTY(id, N'IsUserTable') = 1)
        DROP TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap]
        GO

        CREATE TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] (
            [DatasourceId]          [GUID]          NOT NULL,
            [ActiveNode]            [nvarchar](256) NULL,
            [IsGCed]                [bit]           NOT NULL
            ) ON [PRIMARY]
        GO

        ALTER TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] ADD
    CONSTRAINT [pk__tbl_PRM_DatasourceLastActiveServerMap__DatasourceId] PRIMARY KEY NONCLUSTERED
        (
            [DatasourceId]
        )  ON [PRIMARY],

    CONSTRAINT [DF_tbl_PRM_DatasourceLastActiveServerMap_IsGCed] DEFAULT
        (
            0
        ) FOR [IsGCed]
    GO


##  <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>Rusça yerel ayarda MABS V3 yükseltme başarısız

**Açıklama:** MABS V2 yükseltmeye MABS V3 Rusça yerel ayardaki bir hata koduyla başarısız **4387**.

**Geçici çözüm:** MABS V3 yükseltmek için aşağıdaki adımları uygulayın Rusça'ı kullanarak paketi yükleyin:

1.  [Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) SQL veritabanı ve MABS V2 kaldırırsanız (kaldırma sırasında korunan verileri tutmak seçin).
2.  SQL 2017'ye (Kurumsal) yükseltin ve yükseltmesinin bir parçası raporlama kaldırın.
3. [Yükleme](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Raporlama Hizmetleri (SSRS).
4.  [Yükleme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017#ssms-installation-tips-and-issues-ssms-1791) SQL Server Management Studio'yu (SSMS).
5.  Parametreleri açıklandığı gibi kullanarak raporlama yapılandırma [SQL 2017 ile SSRS yapılandırma](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs).
6.  [Yükleme](backup-azure-microsoft-azure-backup.md) MABS V3.
7. [Geri yükleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SSMS ve çalışma DPM eşitleme aracı açıklanan şekilde kullanarak SQL [burada](https://docs.microsoft.com/previous-versions/system-center/data-protection-manager-2010/ff634215(v=technet.10)).
8.  Aşağıdaki komutu kullanarak dbo.tbl_DLS_GlobalSetting tablo 'DataBaseVersion' özelliğinde güncelleştirin:

        UPDATE dbo.tbl_DLS_GlobalSetting
        set PropertyValue = '13.0.415.0'
        where PropertyName = 'DatabaseVersion'


9.  MSDPM hizmetini başlatın.


## <a name="next-steps"></a>Sonraki adımlar

[MABS V3 sürümünde yenilikler nelerdir?](backup-mabs-whats-new-mabs.md)
