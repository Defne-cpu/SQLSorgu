# SQLSorgu
Basit bir veritabanı örneği ve temel sorgularını içerir.

# Veritabanı Şeması Tasarımı ve Tabloların Oluşturulması 

Bu SQL kodu, bir üniversite veri tabanını tasarlamak için kullanılır. Tasarımda Bölümler, Öğrenciler, Dersler, ve Notlar olmak üzere dört temel tablo bulunur. Kod ayrıca bu tablolara veri ekler ve birkaç örnek sorgu içerir. 

Tabloların Oluşturulması 

Bölümler Tablosu 

Tablo Adı: Bölümler 

Amaç: Üniversitedeki bölümleri saklar. 

Alanlar: 

BolumID: Bölümün kimlik numarası (Primary Key). 

BolumAd: Bölümün adı. 

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Bölümler' AND xtype='U') 

CREATE TABLE Bölümler ( 

    BolumID INT PRIMARY KEY, 

    BolumAd VARCHAR(100) NOT NULL 

); 

2.Öğrenciler Tablosu 

Tablo Adı: Öğrenciler 

Amaç: Öğrencilerin temel bilgilerini saklar. 

Alanlar: 

OgrNo: Öğrenci numarası (Primary Key). 

Ad: Öğrencinin adı. 

Soyad: Öğrencinin soyadı. 

BolumID: Öğrencinin bağlı olduğu bölümün kimlik numarası (Foreign Key). 

 

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Öğrenciler' AND xtype='U') 

CREATE TABLE Öğrenciler ( 

    OgrNo INT PRIMARY KEY, 

    Ad VARCHAR(50) NOT NULL, 

    Soyad VARCHAR(50) NOT NULL, 

    BolumID INT, 

    FOREIGN KEY (BolumID) REFERENCES Bölümler(BolumID) 

); 

3.Dersler Tablosu 

Tablo Adı: Dersler 

Amaç: Üniversitedeki dersleri saklar. 

Alanlar: 

DersKod: Dersin kimlik numarası (Primary Key). 

DersAd: Dersin adı. 

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Dersler' AND xtype='U') 

CREATE TABLE Dersler ( 

    DersKod INT PRIMARY KEY, 

    DersAd VARCHAR(100) NOT NULL 

); 

4.Notlar Tablosu 

Tablo Adı: Notlar 

Amaç: Öğrencilerin aldığı derslere ait notları saklar. 

Alanlar: 

OgrNo: Öğrenci numarası (Primary Key ve Foreign Key). 

DersKod: Ders kodu (Primary Key ve Foreign Key). 

VizeNot: Vize notu. 

FinalNot: Final notu. 

İlişkiler: 

OgrNo, Öğrenciler tablosundaki OgrNo ile ilişkilendirilir. Öğrenci kaydı silindiğinde ilgili notlar da silinir (ON DELETE CASCADE). 

DersKod, Dersler tablosundaki DersKod ile ilişkilendirilir. 

IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='Notlar' AND xtype='U') 

CREATE TABLE Notlar ( 

    OgrNo INT, 

    DersKod INT, 

    VizeNot TINYINT, 

    FinalNot TINYINT, 

    PRIMARY KEY (OgrNo, DersKod), 

    FOREIGN KEY (OgrNo) REFERENCES Öğrenciler(OgrNo) ON DELETE CASCADE, 

    FOREIGN KEY (DersKod) REFERENCES Dersler(DersKod) 

); 

Verilerin Eklenmesi 

Bölümler Tablosu: 

Üç bölüm eklenir: Bilgisayar Mühendisliği, Makine Mühendisliği, Elektrik Elektronik Mühendisliği. 

INSERT INTO Bölümler (BolumID, BolumAd) VALUES (1, 'Bilgisayar Mühendisliği'); 

INSERT INTO Bölümler (BolumID, BolumAd) VALUES (2, 'Makine Mühendisliği'); 

INSERT INTO Bölümler (BolumID, BolumAd) VALUES (3, 'Elektrik Elektronik Mühendisliği'); 

Öğrenciler Tablosu: 

Üç öğrenci eklenir ve bunlar ilgili bölümlere atanır 

INSERT INTO Öğrenciler (OgrNo, Ad, Soyad, BolumID) VALUES (101, 'Ahmet', 'Yılmaz', 1); 

INSERT INTO Öğrenciler (OgrNo, Ad, Soyad, BolumID) VALUES (102, 'Ayşe', 'Demir', 2); 

INSERT INTO Öğrenciler (OgrNo, Ad, Soyad, BolumID) VALUES (103, 'Mehmet', 'Kara', 3); 

Dersler Tablosu: 

Üç ders eklenir: Matematik, Fizik, Programlama. 

INSERT INTO Dersler (DersKod, DersAd) VALUES (1, 'Matematik'); 

INSERT INTO Dersler (DersKod, DersAd) VALUES (2, 'Fizik'); 

INSERT INTO Dersler (DersKod, DersAd) VALUES (3, 'Programlama'); 

Notlar Tablosu: 

Üç öğrenciye ait ders notları eklenir. 

 

INSERT INTO Notlar (OgrNo, DersKod, VizeNot, FinalNot) VALUES (101, 1, 80, 90); 

INSERT INTO Notlar (OgrNo, DersKod, VizeNot, FinalNot) VALUES (102, 2, 70, 85); 

INSERT INTO Notlar (OgrNo, DersKod, VizeNot, FinalNot) VALUES (103, 3, 60, 75); 

Örnek Sorgular 

Tüm Öğrencileri Listeleme: 

Öğrenciler tablosundaki tüm öğrencileri listelemek için kullanılır. 

SELECT * FROM Öğrenciler; 

Öğrencilerin Aldığı Dersler ve Notlarını Sorgulama: 

Öğrencilerin aldıkları dersleri ve notlarını sorgulamak için kullanılır 

SELECT Öğrenciler.Ad, Öğrenciler.Soyad, Dersler.DersAd, Notlar.VizeNot, Notlar.FinalNot 

FROM Notlar 

JOIN Öğrenciler ON Notlar.OgrNo = Öğrenciler.OgrNo 

JOIN Dersler ON Notlar.DersKod = Dersler.DersKod; 

Belirli Bir Bölümdeki Öğrencileri Listeleme: 

Bilgisayar Mühendisliği bölümündeki öğrencileri listelemek için kullanılır. 

SELECT Ad, Soyad 

FROM Öğrenciler 

WHERE BolumID = 1; 

Öğrenci Bilgilerini Güncelleme: 

Öğrenci soyadını güncellemek için kullanılır. Örnekte öğrenci numarası 101 olan öğrenci güncellenmiştir. 

UPDATE Öğrenciler 

SET Soyad = 'Aksu' 

WHERE OgrNo = 101; 

Öğrenci Kayıtlarını Silme: 

Öğrenci numarası 102 olan öğrenciyi silmek için kullanılır. Bu işlem, ON DELETE CASCADE özelliği sayesinde ilişkili notları da otomatik olarak siler. 

DELETE FROM Öğrenciler 

WHERE OgrNo = 102; 

 

Bu dokümantasyon, SQL kodunun işlevini, tablolardaki veri ilişkilerini ve yapılan işlemleri açıklamaktadır. 
