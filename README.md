
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
