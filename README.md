
/* criação do banco */

create database hospital;
use hospital;

create table medico(
codigo_medico int primary key not null auto_increment,
nome_medico varchar(50)
);

create table paciente(
codigo_paciente int primary key not null auto_increment,
nome_paciente varchar(50),
CPF varchar (14),

unique key (CPF)
);

create table logies(
codlog int not null auto_increment primary key,
datacadastro date,
descricao varchar(255)
);

create table consulta(
id int primary key auto_increment,
codigo_medico int,
codigo_paciente int,
data_consulta date,
valor_consulta float,

foreign key(codigo_paciente) references paciente(codigo_paciente),
foreign key(codigo_medico) references medico(codigo_medico)
);


/* criação do banco */

create database hospital;
use hospital;

create table medico(
codigo_medico int primary key not null auto_increment,
nome_medico varchar(50)
);

create table paciente(
codigo_paciente int primary key not null auto_increment,
nome_paciente varchar(50),
CPF varchar (14),

unique key (CPF)
);

create table logies(
codlog int not null auto_increment primary key,
datacadastro date,
descricao varchar(255)
);

create table consulta(
id int primary key auto_increment,
codigo_medico int,
codigo_paciente int,
data_consulta date,
valor_consulta float,

foreign key(codigo_paciente) references paciente(codigo_paciente),
foreign key(codigo_medico) references medico(codigo_medico)
);

* inserção no banco */

INSERT INTO `hospital`.`paciente` (`nome_paciente`, `CPF`) VALUES ('paciente 1', '1111');
INSERT INTO `hospital`.`paciente` (`nome_paciente`, `CPF`) VALUES ('paciente 2', '2222');
INSERT INTO `hospital`.`paciente` (`nome_paciente`, `CPF`) VALUES ('paciente 3', '3333');
INSERT INTO `hospital`.`paciente` (`nome_paciente`, `CPF`) VALUES ('paciente 4', '4444');

INSERT INTO `hospital`.`medico` (`nome_medico`) VALUES ('medico 1');
INSERT INTO `hospital`.`medico` (`nome_medico`) VALUES ('medico 2');
INSERT INTO `hospital`.`medico` (`nome_medico`) VALUES ('medico 3');
INSERT INTO `hospital`.`medico` (`nome_medico`) VALUES ('medico 4');


INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('1', '1', '2023-03-01', '40');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('4', '1', '2023-03-01', '40');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('1', '2', '2023-02-03', '25');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('1', '3', '2023-01-05', '46');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('2', '4', '2023-02-11', '26');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('2', '1', '2023-02-09', '74');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('2', '2', '2023-01-03', '23');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('3', '3', '2023-01-10', '84');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('3', '4', '2023-01-10', '18');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('3', '1', '2023-02-21', '73');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('4', '2', '2023-01-11', '52');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('4', '3', '2023-02-09', '62');
INSERT INTO `hospital`.`consulta` (`codigo_medico`, `codigo_paciente`, `data_consulta`, `valor_consulta`) VALUES ('4', '4', '2023-02-10', '25');


/* Desenvolver trigger que adicione o cpf do paciente quando houver um update*/


drop trigger if exists tr_log ;
delimiter $
create trigger tr_log after update on paciente
for each row 
begin 
insert into logies (datacadastro, descricao) values (curdate(), 'Att na tabela log');
end$
delimiter ;



/* Função que retorne o nome e a soma das consultas de um determinado medico*/

drop function if exists fn_valorTotal;
create function fn_valorTotal( idMedico int)
returns varchar(255)
return (select concat ( nome_medico, '    R$',  sum(valor_consulta)) from medico, consulta where medico.codigo_medico = consulta.codigo_medico and consulta.codigo_medico = idMedico 
group by consulta.codigo_medico);

select fn_valorTotal(1) as Nome_e_Soma; /* 	é necessário passar o id do médico */




/* Função que retorne o nome do paciente com maior numero de consulta*/

drop function if exists fn_maiorPaciente;
create function fn_maiorPaciente()
returns varchar(255)
return (select paciente.nome_paciente from consulta, paciente where paciente.codigo_paciente = consulta.codigo_paciente  group by consulta.codigo_paciente order by count(nome_paciente) desc limit 1);

select fn_maiorPaciente() as pacieteMaisConsultas;

