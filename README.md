````sql
--parte2
--1. crear modelo en un base de datos llamada biblioteca
CREATE DATABASE "Biblioteca"
    WITH 
    OWNER = postgres
    ENCODING = 'UTF8'
    CONNECTION LIMIT = -1;

create table socios(
rut VARCHAR(20) primary key,
nombre_socio VARCHAR(20),
apellido_socio VARCHAR(20),
direccion VARCHAR(20),
telefono VARCHAR(60)
);

create table libro (
isbn VARCHAR(60) primary key,
titulo VARCHAR,
numero_paginas INT
);


create table autor (
codigo_autor INT primary key,
nombre_autor VARCHAR(60),
apellido_autor VARCHAR(60),
fecha_nacimiento INT,
fecha_autor INT,
tipo_de_autor VARCHAR(60)
);


create table autor_libro (
codigo_autor_fk INT references autor(codigo_autor),
isbn_libro_fk VARCHAR(60) references libro (isbn)
);

create table prestamo (
id_prestamo SERIAL primary key ,
fecha_inicio DATE,
fecha_esperada_devolucion DATE,
fecha_real_devolucion DATE,
socio_rut_fk VARCHAR(60) references socios(rut),
libro_isbn_fk VARCHAR(60) references libro(isbn)
);


--2.insertar registro en las tablas

insert into socios(rut,nombre_socio,apellido_socio,direccion,telefono)
values ('1111111-1', 'JUAN', 'SOTO' ,'AVENIDA 1, SANTIAGO', '911111111'),
('2222222-2', 'ANA', 'PÉREZ' ,'PASAJE 2, SANTIAGO' ,'922222222'),
('3333333-3', 'SANDRA', 'AGUILAR' ,'AVENIDA 2, SANTIAGO', '933333333'),
('4444444-4', 'ESTEBAN', 'JEREZ' ,'AVENIDA 3, SANTIAGO' ,'944444444'),
('5555555-5', 'SILVANA', 'MUÑOZ' ,'PASAJE 3, SANTIAGO' ,'955555555');

insert into libro(isbn,titulo,numero_paginas)
values
('111-111 1111-111','CUENTOS DE TERROR','344'),
('222-222 2222-222','POESÍAS CONTEMPORANEAS','167'),
('333-333 3333-333','HISTORIA DE ASIA','511'),
('444-444 4444-444','MANUAL DE MECÁNICA','298');

insert into autor (codigo_autor,nombre_autor,apellido_autor,fecha_nacimiento,fecha_muerte,tipo_de_autor)
values 
(3 ,'JOSE' ,'SALGADO', 1968,2020, 'PRINCIPAL'),
(4,'ANA', 'SALGADO', 1972,'COAUTOR'),
(1,'ANDRÉS', 'ULLOA', 1982, 'PRINCIPAL'),
( 2 ,'SERGIO', 'MARDONES', 1950,2012, 'PRINCIPAL'),
(5 ,'MARTIN', 'PORTA', 1976, 'PRINCIPAL');

insert into autor_libro(
codigo_autor_fk,isbn_libro_fk)
values(3,'111-111 1111-111'),
(4,'111-111 1111-111'),
(1,'222-222 2222-222'),
(2,'333-333 3333-333'),
(5,'444-444 4444-444');

insert into prestamo(fecha_inicio,fecha_esperada_devolucion,fecha_real_devolucion,socio_rut_fk,libro_isbn_fk)
values
('20-01-2020','27-01-2020','27-01-2020','1111111-1','111-111 1111-111'),
('20-01-2020','27-01-2020','30-01-2020','5555555-5','222-222 2222-222'),
(' 22-01-2020',' 29-01-2020','30-01-2020','3333333-3','333-333 3333-333'),
('23-01-2020','30-01-2020','30-01-2020','4444444-4','444-444 4444-444'),
('27-01-2020','03-02-2020','04-02-2020','2222222-2','111-111 1111-111'),
('31-01-2020','07-02-2020','12-02-2020','1111111-1','444-444 4444-444'),
('31-01-2020','07-02-2020','12-02-2020','3333333-3','222-222 2222-222');

--3. realizar consultas
--a.
select titulo
from libro as l
where l.numero_paginas < 300;

--b.
select a.nombre_autor,a.fecha_nacimiento
from autor as a
where a.fecha_nacimiento > 1970;

--c.
select count(p.libro_isbn_fk) as mas_solicitado, l.titulo
from  prestamo as p 
join libro as l
on p.libro_isbn_fk = l.isbn 
group by l.titulo 
limit 1;

--d.
select s.nombre_socio ,l.titulo, (p.fecha_real_devolucion -p.fecha_esperada_devolucion) as  dias_atraso,((p.fecha_real_devolucion -p.fecha_esperada_devolucion)*100) as multa 
from prestamo as p
join socios as s
on s.rut = p.socio_rut_fk
join libro as l
on l.isbn = p.libro_isbn_fk
group by l.titulo ,s.nombre_socio, dias_atraso, multa
order by dias_atraso desc;







````