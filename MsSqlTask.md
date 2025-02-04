Таблица контейнеров:
CREATE TABLE Containers (
    ID UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
    Number INT NOT NULL,
    Type NVARCHAR(50) NOT NULL,
    Length DECIMAL(10, 2) NOT NULL,
    Width DECIMAL(10, 2) NOT NULL,
    Height DECIMAL(10, 2) NOT NULL,
    Weight DECIMAL(10, 2) NOT NULL,
    IsEmpty BIT NOT NULL,
    ArrivalDate DATETIME NOT NULL
);
Таблица операций:
CREATE TABLE Operations (
    ID UNIQUEIDENTIFIER PRIMARY KEY DEFAULT NEWID(),
    ContainerID UNIQUEIDENTIFIER NOT NULL,
    OperationStartDate DATETIME NOT NULL,
    OperationEndDate DATETIME NOT NULL,
    OperationType NVARCHAR(100) NOT NULL,
    OperatorName NVARCHAR(150) NOT NULL,
    InspectionLocation NVARCHAR(200) NOT NULL,
    FOREIGN KEY (ContainerID) REFERENCES Containers(ID)
);
Запрос для выборки данных из таблицы "Containers" в формате JSON без использования встроенной функции:
SELECT 
    '{' +
    '"ID": "' + CAST(ID AS NVARCHAR(36)) + '", ' +
    '"Number": ' + CAST(Number AS NVARCHAR) + ', ' +
    '"Type": "' + Type + '", ' +
    '"Length": ' + CAST(Length AS NVARCHAR) + ', ' +
    '"Width": ' + CAST(Width AS NVARCHAR) + ', ' +
    '"Height": ' + CAST(Height AS NVARCHAR) + ', ' +
    '"Weight": ' + CAST(Weight AS NVARCHAR) + ', ' +
    '"IsEmpty": ' + CASE WHEN IsEmpty = 1 THEN 'true' ELSE 'false' END + ', ' +
    '"ArrivalDate": "' + CONVERT(NVARCHAR, ArrivalDate, 120) + '"' +
    '}' AS JSONData
FROM 
    Containers;
Запрос для выборки данных из таблицы "Operations" для определенного контейнера в формате JSON без использования встроенной функции:
DECLARE @ContainerID UNIQUEIDENTIFIER = 'your id here';

SELECT 
    '{' +
    '"ID": "' + CAST(ID AS NVARCHAR(36)) + '", ' +
    '"ContainerID": "' + CAST(ID AS NVARCHAR(36)) + '", ' +
    '"OperationStartDate": "' + CONVERT(NVARCHAR, OperationStartDate, 120) + '", ' +
    '"OperationEndDate": "' + CONVERT(NVARCHAR, OperationEndDate, 120) + '", ' +
    '"OperationType": "' + OperationType + '", ' +
    '"OperatorName": "' + OperatorName + '", ' +
    '"InspectionLocation": "' + InspectionLocation + '"' +
    '}' AS JSONData
FROM 
    Operations
WHERE 
    ID = @ContainerID;
