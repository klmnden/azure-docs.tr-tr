---
author: WenJason
ms.service: sql-database
ms.topic: include
origin.date: 12/10/2018
ms.date: 01/14/2019
ms.author: v-jay
ms.openlocfilehash: e30651cb0ed7d74082163a92acbc428c21018255
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553221"
---
## <a name="c-program-example"></a>C#örnek program

Mevcut bu makalenin sonraki bölümlerinde bir C# Transact-SQL (T-SQL) deyimlerini SQL veritabanına göndermek için ADO.NET kullanan programı. C# Programı aşağıdaki eylemleri gösterir:

- [ADO.NET kullanarak SQL veritabanı'na bağlanma](#cs_1_connect)
- [T-SQL deyimleri döndüren yöntemler](#cs_2_return)
    - Tablo oluşturma
    - Tabloları verilerle doldurun
    - Güncelleştirme, silme ve verileri seçme
- [T-SQL veritabanına gönderme](#cs_3_submit)

### <a name="entity-relationship-diagram-erd"></a>Varlık ilişkisi şeması (ERD)

`CREATE TABLE` Deyimleri içeren **başvuruları** oluşturmak için anahtar sözcüğü bir *yabancı anahtar* iki tablo arasında ilişki (FK). Kullanıyorsanız *tempdb*, açıklama `--REFERENCES` önünde çizgiler çifti kullanarak anahtar sözcüğü.

ERD iki tablo arasındaki ilişkiyi gösterir. Değerler **tabEmployee.DepartmentCode** *alt* sütun sınırlı değerleri için **tabDepartment.DepartmentCode** *üst*sütun.

![ERD gösteren yabancı anahtar](./media/sql-database-csharp-adonet-create-query-2/erd-dept-empl-fky-2.png)

> [!NOTE]
> Bir satır eklemek için T-SQL düzenleme seçeneğiniz `#` tablo adlarına oluşturan bunları gibi geçici tablolarda *tempdb*. Test veritabanı kullanılabilir olduğunda bu gösterim amaçları için yararlıdır. Yabancı anahtarlar herhangi bir başvuru, bunların kullanılması sırasında zorlanmaz ve program çalışmayı tamamladıktan sonra bağlantıyı kapatır, geçici tabloları otomatik olarak silinir.

### <a name="to-compile-and-run"></a>Derlemek ve çalıştırmak için

C# Program mantıksal olarak bir .cs dosyası ve her blok anlamak daha kolay hale getirmek için bazı kod blokları şeklinde, fiziksel olarak ayrılmıştır. Derleme ve programı çalıştırmak için aşağıdaki adımları uygulayın:

1. Oluşturma bir C# Visual Studio'da proje. Proje türü olmalıdır bir *konsol*, altında bulunan **şablonları** > **Visual C#**   >  **WindowsMasaüstü**  >  **Konsol uygulaması (.NET Framework)**.

1. Dosyasındaki *Program.cs*, aşağıdaki adımlarla bulunan Başlangıç kod satırlarını değiştirin:

    1. Aşağıdaki kod blokları, bunların sunulur, aynı sırada Kopyala ve Yapıştır bkz [veritabanına bağlan](#cs_1_connect), [oluşturmak T-SQL](#cs_2_return), ve [veritabanı Gönder](#cs_3_submit).

    1. Aşağıdaki değerleri değiştirme `Main` yöntemi:

        - *Kal. Veri kaynağı*
        - *cb.UserID*
        - *Kal. Parola*
        - *Kal. InitialCatalog*

1. Derleme doğrulama *System.Data.dll* başvurulur. Doğrulamak için genişletme **başvuruları** düğümünde **Çözüm Gezgini** bölmesi.

1. Yapı ve Visual Studio'dan programı çalıştırmak için **Başlat** düğmesi. GUID değerleri, test çalışmaları arasında farklılık gösterir ancak rapor çıktısı bir program penceresinde görüntülenir.

    ```Output
    =================================
    T-SQL to 2 - Create-Tables...
    -1 = rows affected.

    =================================
    T-SQL to 3 - Inserts...
    8 = rows affected.

    =================================
    T-SQL to 4 - Update-Join...
    2 = rows affected.

    =================================
    T-SQL to 5 - Delete-Join...
    2 = rows affected.

    =================================
    Now, SelectEmployees (6)...
    8ddeb8f5-9584-4afe-b7ef-d6bdca02bd35 , Alison , 20 , acct , Accounting
    9ce11981-e674-42f7-928b-6cc004079b03 , Barbara , 17 , hres , Human Resources
    315f5230-ec94-4edd-9b1c-dd45fbb61ee7 , Carol , 22 , acct , Accounting
    fcf4840a-8be3-43f7-a319-52304bf0f48d , Elle , 15 , NULL , NULL
    View the report output here, then press any key to end the program...
    ```

<a name="cs_1_connect"/>

### <a name="connect-to-sql-database-using-adonet"></a>ADO.NET kullanarak SQL veritabanı'na bağlanma

```csharp
using System;
using System.Data.SqlClient;   // System.Data.dll
//using System.Data;           // For:  SqlDbType , ParameterDirection

namespace csharp_db_test
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                var cb = new SqlConnectionStringBuilder();
                cb.DataSource = "your_server.database.chinacloudapi.cn";
                cb.UserID = "your_user";
                cb.Password = "your_password";
                cb.InitialCatalog = "your_database";

                using (var connection = new SqlConnection(cb.ConnectionString))
                {
                    connection.Open();

                    Submit_Tsql_NonQuery(connection, "2 - Create-Tables", Build_2_Tsql_CreateTables());

                    Submit_Tsql_NonQuery(connection, "3 - Inserts", Build_3_Tsql_Inserts());

                    Submit_Tsql_NonQuery(connection, "4 - Update-Join", Build_4_Tsql_UpdateJoin(),
                        "@csharpParmDepartmentName", "Accounting");

                    Submit_Tsql_NonQuery(connection, "5 - Delete-Join", Build_5_Tsql_DeleteJoin(),
                        "@csharpParmDepartmentName", "Legal");

                    Submit_6_Tsql_SelectEmployees(connection);
                }
            }
            catch (SqlException e)
            {
                Console.WriteLine(e.ToString());
            }

            Console.WriteLine("View the report output here, then press any key to end the program...");
            Console.ReadKey();
        }
```

<a name="cs_2_return"/>

### <a name="methods-that-return-t-sql-statements"></a>T-SQL deyimleri döndüren yöntemler

```csharp
static string Build_2_Tsql_CreateTables()
{
    return @"
        DROP TABLE IF EXISTS tabEmployee;
        DROP TABLE IF EXISTS tabDepartment;  -- Drop parent table last.

        CREATE TABLE tabDepartment
        (
            DepartmentCode  nchar(4)          not null    PRIMARY KEY,
            DepartmentName  nvarchar(128)     not null
        );

        CREATE TABLE tabEmployee
        (
            EmployeeGuid    uniqueIdentifier  not null  default NewId()    PRIMARY KEY,
            EmployeeName    nvarchar(128)     not null,
            EmployeeLevel   int               not null,
            DepartmentCode  nchar(4)              null
            REFERENCES tabDepartment (DepartmentCode)  -- (REFERENCES would be disallowed on temporary tables.)
        );
    ";
}

static string Build_3_Tsql_Inserts()
{
    return @"
        -- The company has these departments.
        INSERT INTO tabDepartment (DepartmentCode, DepartmentName)
        VALUES
            ('acct', 'Accounting'),
            ('hres', 'Human Resources'),
            ('legl', 'Legal');

        -- The company has these employees, each in one department.
        INSERT INTO tabEmployee (EmployeeName, EmployeeLevel, DepartmentCode)
        VALUES
            ('Alison'  , 19, 'acct'),
            ('Barbara' , 17, 'hres'),
            ('Carol'   , 21, 'acct'),
            ('Deborah' , 24, 'legl'),
            ('Elle'    , 15, null);
    ";
}

static string Build_4_Tsql_UpdateJoin()
{
    return @"
        DECLARE @DName1  nvarchar(128) = @csharpParmDepartmentName;  --'Accounting';

        -- Promote everyone in one department (see @parm...).
        UPDATE empl
        SET
            empl.EmployeeLevel += 1
        FROM
            tabEmployee   as empl
        INNER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        WHERE
            dept.DepartmentName = @DName1;
    ";
}

static string Build_5_Tsql_DeleteJoin()
{
    return @"
        DECLARE @DName2  nvarchar(128);
        SET @DName2 = @csharpParmDepartmentName;  --'Legal';

        -- Right size the Legal department.
        DELETE empl
        FROM
            tabEmployee   as empl
        INNER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        WHERE
            dept.DepartmentName = @DName2

        -- Disband the Legal department.
        DELETE tabDepartment
            WHERE DepartmentName = @DName2;
    ";
}

static string Build_6_Tsql_SelectEmployees()
{
    return @"
        -- Look at all the final Employees.
        SELECT
            empl.EmployeeGuid,
            empl.EmployeeName,
            empl.EmployeeLevel,
            empl.DepartmentCode,
            dept.DepartmentName
        FROM
            tabEmployee   as empl
        LEFT OUTER JOIN
            tabDepartment as dept ON dept.DepartmentCode = empl.DepartmentCode
        ORDER BY
            EmployeeName;
    ";
}
```

<a name="cs_3_submit"/>

### <a name="submit-t-sql-to-the-database"></a>T-SQL veritabanına gönderme

```csharp
static void Submit_6_Tsql_SelectEmployees(SqlConnection connection)
{
    Console.WriteLine();
    Console.WriteLine("=================================");
    Console.WriteLine("Now, SelectEmployees (6)...");

    string tsql = Build_6_Tsql_SelectEmployees();

    using (var command = new SqlCommand(tsql, connection))
    {
        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                Console.WriteLine("{0} , {1} , {2} , {3} , {4}",
                    reader.GetGuid(0),
                    reader.GetString(1),
                    reader.GetInt32(2),
                    (reader.IsDBNull(3)) ? "NULL" : reader.GetString(3),
                    (reader.IsDBNull(4)) ? "NULL" : reader.GetString(4));
            }
        }
    }
}

static void Submit_Tsql_NonQuery(
    SqlConnection connection,
    string tsqlPurpose,
    string tsqlSourceCode,
    string parameterName = null,
    string parameterValue = null
    )
{
    Console.WriteLine();
    Console.WriteLine("=================================");
    Console.WriteLine("T-SQL to {0}...", tsqlPurpose);

    using (var command = new SqlCommand(tsqlSourceCode, connection))
    {
        if (parameterName != null)
        {
            command.Parameters.AddWithValue(  // Or, use SqlParameter class.
                parameterName,
                parameterValue);
        }
        int rowsAffected = command.ExecuteNonQuery();
        Console.WriteLine(rowsAffected + " = rows affected.");
    }
}
} // EndOfClass
}
```
