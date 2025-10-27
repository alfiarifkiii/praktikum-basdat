# praktikum-basdat

CREATE TABLE produk_motor (
	id_motor SERIAL PRIMARY KEY,
	nama_motor VARCHAR (100),
	harga INT 
);

CREATE TABLE log_harga (
	id_log SERIAL PRIMARY KEY,
	id_motor INT REFERENCES produk_motor(id_motor),
	harga_lama INT,
	harga_baru INT,
	tgl_ubah DATE
); 

SELECT * FROM produk_motor;
SELECT * FROM log_harga;

INSERT INTO produk_motor VALUES
	(DEFAULT, 'Supra FIT', 5000000),
	(DEFAULT, 'Astrea', 3000000),
	(DEFAULT, 'Beat Karbu',7000000),
	(DEFAULT, 'Satria FU', 7000000);



BUAT FUNCTION
CREATE OR REPLACE FUNCTION func_log_harga()
		RETURNS TRIGGER LANGUAGE PLPGSQL 
		AS $$ BEGIN 
		INSERT INTO log_harga VALUES 
			(DEFAULT, OLD.id_motor, OLD.harga, NEW.harga, CURRENT_DATE);
		RETURN NEW;
	END;
	$$



BUAT TRIGGER
CREATE TRIGGER trig_log_harga
	BEFORE UPDATE ON produk_motor 
	FOR EACH ROW EXECUTE PROCEDURE func_log_harga();

UPDATE produk_motor SET harga = 10000000 
	WHERE id_motor = '3';

ALTER TRIGGER trig_log_harga ON produk_motor
	RENAME TO ini_trig_harga;

SELECT * FROM pg_trigger;

DROP TRIGGER ini_trig_harga ON produk_motor;



PERTAMBAHAN
CREATE OR REPLACE FUNCTION tambah(angka1 INT, angka2 INT)
	RETURNS INT LANGUAGE PLPGSQL 
	AS 
	$$
		BEGIN 
		RETURN angka1 + angka2;
	END;
	$$
SELECT tambah (10,2);


MENCARI >/<
CREATE OR REPLACE FUNCTION harga_aja(harga NUMERIC)
	RETURNS TEXT LANGUAGE PLPGSQL
	AS
	$$
		BEGIN
		IF harga > 100000 THEN 
			RETURN 'mahal ooy';
		ELSE 
			RETURN 'murah ooy';
		END IF;
	END;
	$$
SELECT harga_aja(harga) FROM produk_motor;		
	
