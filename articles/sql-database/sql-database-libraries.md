---
title: Bibliotecas de conexiones para SQL Database | Microsoft Docs
description: "Proporciona vínculos para obtener descargas de los módulos que permiten la conexión a SQL Server y a SQL Database desde una amplia variedad de lenguajes de programación de cliente."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: genemi
ms.assetid: 13d899d3-cf46-4e4d-8919-cf4b41ca836d
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: genemi
ms.openlocfilehash: bdf83fac9bd0ac6790062f802748a18045c7a171
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2017
---
# <a name="connectivity-libraries-and-frameworks-for-microsoft-sql-server"></a>Bibliotecas y entornos de conectividad para Microsoft SQL Server

Consulte nuestros [tutoriales de introducción](http://aka.ms/sqldev) para empezar a trabajar rápidamente con lenguajes de programación como C#, Java, Node.js, PHP y Python, y crear una aplicación mediante SQL Server en Linux o Windows o Docker en macOS.

La tabla siguiente enumera las bibliotecas de conectividad o los *controladores* que las aplicaciones cliente pueden utilizar desde diversos lenguajes para conectarse a instancias de Microsoft SQL Server que se ejecutan localmente o en la nube, en Linux, Windows o Docker, y también a Azure SQL Database y Azure SQL Warehouse. 

| Idioma | Plataforma | Recursos adicionales | Descargar | Introducción |
| :-- | :-- | :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Microsoft ADO.NET para SQL Server](https://docs.microsoft.com/sql/connect/ado-net/microsoft-ado-net-for-sql-server) | [Descargar](https://www.microsoft.com/net/download/) | [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/csharp/ubuntu)
| Java | Windows, Linux, macOS | [Controlador JDBC de Microsoft para SQL Server](http://msdn.microsoft.com/library/mt484311.aspx) | [Descargar](https://go.microsoft.com/fwlink/?linkid=852460) |  [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/java/ubuntu)
| PHP | Windows, Linux, macOS| [Controlador SQL de PHP para SQL Server](http://msdn.microsoft.com/library/dn865013.aspx) | Sistema operativo: <br/> \*[Windows](https://www.microsoft.com/download/details.aspx?id=20098) <br/> \*[Linux](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) <br/> \*[macOS](https://github.com/Microsoft/msphpsql/tree/dev#install-unix) |  [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/ubuntu)
| Node.js | Windows, Linux, macOS | [Controlador de Node.js para SQL Server](http://msdn.microsoft.com/library/mt652093.aspx) | [Instalación](https://msdn.microsoft.com/library/mt652094.aspx) |  [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/node/ubuntu)
| Python | Windows, Linux, macOS | [Controlador de Python para SQL Server](http://msdn.microsoft.com/library/mt652092.aspx) | Opciones de instalación: <br/> \*[pymssql](https://msdn.microsoft.com/library/mt694094.aspx) <br/> \*[pyodbc](http://msdn.microsoft.com/library/mt763257.aspx) |  [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/python/ubuntu)
| Ruby | Windows, Linux, macOS | [Controlador de Ruby para SQL Server](http://msdn.microsoft.com/library/mt691981.aspx) | [Instalación](https://msdn.microsoft.com/library/mt711041.aspx) | [Introducción](https://www.microsoft.com/en-us/sql-server/developer-get-started/ruby/ubuntu)
| C++ | Windows, Linux, macOS | [Microsoft ODBC Driver for SQL Server](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) | [Descargar](https://msdn.microsoft.com/en-us/library/mt654048(v=sql.1).aspx) |  

La tabla siguiente muestra algunos ejemplos de entornos y entornos web de asignación relacional de objetos (ORM) que las aplicaciones cliente pueden utilizar para conectarse a instancias de Microsoft SQL Server que se ejecutan localmente o en la nube, en Linux, Windows o Docker, y también a Azure SQL Database y Azure SQL Warehouse. 

| Idioma | Plataforma | ORM |
| :-- | :-- | :-- |
| C# | Windows, Linux, macOS | [Entity Framework](https://docs.microsoft.com/en-us/ef)<br>[Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/index) |
| Java | Windows, Linux, macOS |[Hibernate ORM](http://hibernate.org/orm)|
| PHP | Windows, Linux | [Laravel (Eloquent)](https://laravel.com/docs/5.0/eloquent) |
| Node.js | Windows, Linux, macOS | [Sequelize ORM](http://docs.sequelizejs.com) |
| Python | Windows, Linux, macOS |[Django](https://www.djangoproject.com/) |
| Ruby | Windows, Linux, macOS | [Ruby on Rails](http://rubyonrails.org/) |

## <a name="related-links"></a>Vínculos relacionados
- [Controladores de SQL Server](http://msdn.microsoft.com/library/mt654049.aspx) para conectarse desde aplicaciones cliente
- [Conexión a Base de datos SQL mediante .NET (C#)](sql-database-connect-query-dotnet.md)
- [Conexión a la base de datos SQL mediante PHP en Windows](sql-database-connect-query-php.md)
- [Conexión a Base de datos SQL mediante Node.js](sql-database-connect-query-nodejs.md)
- [Conexión a la base de datos SQL mediante Java con JDBC en Windows](sql-database-connect-query-java.md)
- [Conexión a Base de datos SQL mediante Python](sql-database-connect-query-python.md)
- [Conexión a Base de datos SQL mediante Ruby](sql-database-connect-query-ruby.md)
