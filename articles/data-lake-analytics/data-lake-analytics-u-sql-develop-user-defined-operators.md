---
title: U-SQL kullanıcı tanımlı işleçler (Udo'lar) Azure Data Lake Analytics geliştirin
description: Kullanıcı tanımlı işleçler kullanılan ve Azure Data Lake Analytics işleri yeniden için geliştirmeyi öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 8d8937a6132f770e989d7595883b2c5cf804c44f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34623864"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>U-SQL kullanıcı tanımlı işleçler (Udo'lar) geliştirin
Bu makalede, U-SQL iş verileri işlemek için kullanıcı tanımlı işleçleri geliştirme açıklar.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Tanımlama ve U-SQL kullanıcı tanımlı bir işleç kullanma
**Oluşturmak ve U-SQL işi göndermek için**

1. Visual Studio seçin gelen **Dosya > Yeni > Proje > U-SQL projesi**.
2. **Tamam**’a tıklayın. Visual Studio Script.usql dosyası ile çözüm oluşturur.
3. Gelen **Çözüm Gezgini**Script.usql genişletin ve ardından **Script.usql.cs**.
4. Dosyasına aşağıdaki kodu yapıştırın:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Açık **Script.usql**, aşağıdaki U-SQL betiğini yapıştırın:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Data Lake Analytics hesabını, Veritabanı'nı ve Şema'yı belirtin.
8. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betik Oluştur**'a tıklayın.
9. **Çözüm Gezgini**'nden, **Script.usql** öğesine sağ tıklayın ve ardından **Betiği Gönder**'e tıklayın.
10. Azure aboneliğinize bağlanmadıysanız, Azure hesabı kimlik bilgilerinizi girmeniz istenir.
11. Tıklatın **gönderme**. Gönderim tamamlandığında Sonuçları penceresinde, gönderme işleminin sonuçları ve iş bağlantısı da kullanılabilir.
12. Tıklatın **yenileme** en son iş durumu ve ekranı yenilemek görmek için düğmesi.

**Çıkışı görmek için**

1. Gelen **Sunucu Gezgini**, genişletin **Azure**, genişletin **Data Lake Analytics**, Data Lake Analytics hesabınızı genişletin, **depolama hesapları**, varsayılan depolama sağ tıklayın ve ardından **Explorer**.
2. Örnekleri genişletin, çıkışları genişletin ve ardından **sürücüler.csv**.

## <a name="see-also"></a>Ayrıca bkz.
* [U-SQL kullanıcı kodu ifadelerle genişletme](https://msdn.microsoft.com/library/azure/mt621316.aspx)
* [U-SQL uygulamalarını geliştirmek için Visual Studio için Data Lake araçları kullanın](data-lake-analytics-data-lake-tools-get-started.md)
