# sql-databases-project1
IF NOT EXISTS (SELECT * FROM sys.databases WHERE name = 'PR2')
BEGIN
CREATE DATABASE PR2;
END;
GO

USE PR2;
GO

IF OBJECT_ID('dbo.elev','U') IS NOT NULL
DROP TABLE dbo.elev;
GO

IF OBJECT_ID('dbo.scoala','U') IS NOT NULL
DROP TABLE dbo.scoala
GO

CREATE TABLE scoala (
ID_elev INT,
media DECIMAL (4,2),
disciplina_bac NVARCHAR (50),
absente INT,
PRIMARY KEY (ID_elev) 
);
GO

CREATE TABLE elev (
ID_elev INT,
Nume_Prenume NVARCHAR (50),
Sexul NVARCHAR (10),
CONSTRAINT FK_elev FOREIGN KEY (ID_elev) REFERENCES scoala(ID_elev)
);
GO

INSERT INTO scoala(ID_elev,media,disciplina_bac,absente) VALUES
(1,'8.92','Matematica',28),
(2,'6.2','Chimie',52),
(3,'9.71','Geografie',3),
(4,'7.78','Matematica',19),
(5,'8.34','Informatica',13)
GO

INSERT INTO elev(ID_elev,Nume_Prenume,Sexul) VALUES
(1,'Mariana Buburuza','F'),
(2,'Scott Mescudi','M'),
(3,'Don Toliver','M'),
(4,'Katy Pery','F'),
(5,'Kanye Omari West','M')
GO

SELECT s.media, s.absente, e.Nume_Prenume
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE media > '8.5';
GO

SELECT COUNT(*)  as Copii
FROM elev
where Sexul IN ('M','F')
GROUP BY Sexul;
GO

SELECT absente, AVG(media) AS Media
FROM scoala s
GROUP BY absente;

SELECT AVG(media) AS MediaBaietilor
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE Sexul = 'M';
GO

SELECT AVG(media) AS MediaFetelor
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE Sexul = 'F';
GO

SELECT SUM(absente) AS Absente_Fete
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE Sexul = 'F';
GO

SELECT SUM(absente) AS Absente_Baieti
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE Sexul = 'M'
GO

SELECT disciplina_bac, COUNT(ID_elev) AS Disciplini_Bac
FROM scoala s
GROUP BY disciplina_bac;
GO

SELECT e.Nume_Prenume, s.absente AS AbsentePutine
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
ORDER BY s.absente ASC;
GO

SELECT Nume_Prenume, MAX(absente) AS AbsenteMulte
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
GROUP BY Nume_Prenume;
GO

SELECT Nume_Prenume, MIN(media) AS CeaMaiMareMedie
FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
GROUP BY Nume_Prenume;
GO

SELECT * FROM elev 
WHERE Nume_Prenume LIKE 'K%';
GO

SELECT * FROM scoala
WHERE absente > 30;
GO

SELECT e.Nume_Prenume, s.media, s.absente
FROM elev e
FULL JOIN scoala s ON s.ID_elev = e.ID_elev;
GO

SELECT e.Nume_Prenume, s.disciplina_bac
FROM elev e
CROSS JOIN scoala s;
GO

SELECT DISTINCT disciplina_bac AS Discipline_alese_la_bac
FROM scoala;
GO

SELECT disciplina_bac FROM scoala
UNION
SELECT disciplina_bac FROM scoala;
GO

SELECT disciplina_bac 
FROM scoala
UNION ALL
SELECT disciplina_bac
FROM scoala;
GO

SELECT disciplina_bac, COUNT(*) AS repetitii
FROM scoala
GROUP BY disciplina_bac
HAVING COUNT(*) > 1;
GO

SELECT * FROM scoala
JOIN elev ON scoala.ID_elev = elev.ID_elev
WHERE media<9 AND absente > 25;
GO

SELECT * FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE media>8 OR absente<15;
GO

SELECT * FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE NOT disciplina_bac = 'Matematica';
GO

SELECT * FROM elev
WHERE Sexul NOT IN ('F');
GO

SELECT * FROM scoala s
JOIN elev e ON s.ID_elev = e.ID_elev
WHERE disciplina_bac IN ('Matematica','Informatica');
GO

SELECT ID_elev , absente + 10 AS Lectii_Scapate
FROM scoala;
GO

SELECT ID_elev, absente * 2 AS absente_in_total
FROM scoala;
GO

SELECT ID_elev, (media + 8)/ 2 AS Media_Finala
FROM scoala;
GO

SELECT ID_elev, absente - 7 AS cadou
FROM scoala;
GO
