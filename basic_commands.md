# Guia rápido de mysql

**Considere a seguinte criação das tabelas Cliente e Pedido:**

Tabela Cliente:

		mysql> CREATE TABLE Cliente(
			-> id INTEGER PRIMARY KEY AUTO_INCREMENT,
			-> nome VARCHAR(255),
			-> sobrenome VARCHAR(255)
			-> );
		Query OK, 0 rows affected (0.37 sec)
		
		mysql> DESCRIBE Cliente;
		+-----------+--------------+------+-----+---------+----------------+
		| Field     | Type         | Null | Key | Default | Extra          |
		+-----------+--------------+------+-----+---------+----------------+
		| id        | int(11)      | NO   | PRI | NULL    | auto_increment |
		| nome      | varchar(255) | YES  |     | NULL    |                |
		| sobrenome | varchar(255) | YES  |     | NULL    |                |
		+-----------+--------------+------+-----+---------+----------------+
		3 rows in set (0.00 sec)

Tabela Pedidos:

		mysql> CREATE TABLE Pedidos(
			-> id INTEGER PRIMARY KEY AUTO_INCREMENT,
			-> cliente_id INT(5),
			-> produto VARCHAR(255),
			-> preco DECIMAL(5,2)
			-> );
		Query OK, 0 rows affected (0.26 sec)
		
		mysql> DESCRIBE Pedidos;
		+------------+--------------+------+-----+---------+----------------+
		| Field      | Type         | Null | Key | Default | Extra          |
		+------------+--------------+------+-----+---------+----------------+
		| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
		| cliente_id | int(5)       | YES  |     | NULL    |                |
		| produto    | varchar(255) | YES  |     | NULL    |                |
		| preco      | decimal(5,2) | YES  |     | NULL    |                |
		+------------+--------------+------+-----+---------+----------------+





**Baseado na criação das tabelas acima, responda com a declaração SQL correta para:**
A - Criar uma Coluna de email na tabela Cliente

		mysql> ALTER TABLE Cliente
			-> ADD COLUMN email VARCHAR(20) AFTER sobrenome;
		Query OK, 0 rows affected (0.41 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		
		mysql> DESCRIBE Cliente;
		+-----------+--------------+------+-----+---------+----------------+
		| Field     | Type         | Null | Key | Default | Extra          |
		+-----------+--------------+------+-----+---------+----------------+
		| id        | int(11)      | NO   | PRI | NULL    | auto_increment |
		| nome      | varchar(255) | YES  |     | NULL    |                |
		| sobrenome | varchar(255) | YES  |     | NULL    |                |
		| email     | varchar(20)  | YES  |     | NULL    |                |
		+-----------+--------------+------+-----+---------+----------------+
		4 rows in set (0.00 sec)

**B - Inserir, 3 clientes na tabela Cliente com os seguintes dados:**

- José Silva, email: jose@cb.com.br
- João Pedro, email: joao@pf.com.br
- Pedro Silva, email : pedro@ex.com.


		mysql> INSERT INTO Cliente(nome, sobrenome, email)
			-> VALUES('José', 'Silva', 'jose@cb.com.br');
		Query OK, 1 row affected (0.03 sec)
		
		
		mysql> INSERT INTO Cliente(nome, sobrenome, email) 
			-> VALUES('João','Pedro', 'joao@pf.com.br');
		Query OK, 1 row affected (0.03 sec)
		
		
		mysql> INSERT INTO Cliente(nome, sobrenome, email) 
			->VALUES('Pedro','Silva', 'pedro@ex.com');
		Query OK, 1 row affected (0.05 sec)
		
		
		mysql>
		mysql> SELECT * FROM Cliente;
		+----+-------+-----------+----------------+
		| id | nome  | sobrenome | email          |
		+----+-------+-----------+----------------+
		|  1 | José  | Silva     | jose@cb.com.br |
		|  2 | João  | Pedro     | joao@pf.com.br |
		|  3 | Pedro | Silva     | pedro@ex.com   |
		+----+-------+-----------+----------------+
		3 rows in set (0.00 sec)



**C – Inserir um pedido para cada cliente com os seguintes dados:**
- Para o cliente 1: Produto: Geladeira Brastemp, Preço: 1800,00
- Para o cliente 2: Produto: Fogão Consul, Preço 850,90
- Para o cliente 3: Produto: Celular Iphone XR, Preço 3399,00

		Change decimal to fit values xxxx.xx (6 digits and 2 are decimal)
		
		mysql> DESCRIBE Pedidos;
		+------------+--------------+------+-----+---------+----------------+
		| Field      | Type         | Null | Key | Default | Extra          |
		+------------+--------------+------+-----+---------+----------------+
		| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
		| cliente_id | int(5)       | YES  |     | NULL    |                |
		| produto    | varchar(255) | YES  |     | NULL    |                |
		| preco      | decimal(5,2) | YES  |     | NULL    |                |
		+------------+--------------+------+-----+---------+----------------+
		4 rows in set (0.00 sec)
		
		mysql> ALTER TABLE Pedidos MODIFY preco DECIMAL(6,2);
		Query OK, 0 rows affected (0.45 sec)
		Records: 0  Duplicates: 0  Warnings: 0
		
		mysql> DESCRIBE Pedidos;
		+------------+--------------+------+-----+---------+----------------+
		| Field      | Type         | Null | Key | Default | Extra          |
		+------------+--------------+------+-----+---------+----------------+
		| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
		| cliente_id | int(5)       | YES  |     | NULL    |                |
		| produto    | varchar(255) | YES  |     | NULL    |                |
		| preco      | decimal(6,2) | YES  |     | NULL    |                |
		+------------+--------------+------+-----+---------+----------------+
		4 rows in set (0.00 sec)


		mysql> INSERT INTO Pedidos (cliente_id, produto, preco)
			-> SELECT id, 'Geladeira Brastemp', 1800.00
			-> FROM Cliente
			-> WHERE nome = 'Jose' AND sobrenome = 'Silva';
		Query OK, 1 row affected (0.02 sec)
		Records: 1  Duplicates: 0  Warnings: 0
		
		mysql> INSERT INTO Pedidos (cliente_id, produto, preco) 
			-> SELECT id, 'Fogão Consul', 850.90 
			-> FROM Cliente 
			-> WHERE nome = 'João' AND sobrenome = 'Pedro';
		Query OK, 1 row affected (0.03 sec)
		Records: 1  Duplicates: 0  Warnings: 0
		
		mysql> INSERT INTO Pedidos (cliente_id, produto, preco) 
			-> SELECT id, 'Celular Iphone XR', 3399.00 
			-> FROM Cliente 
			-> WHERE nome = 'Pedro' AND sobrenome = 'Silva';
		Query OK, 1 row affected (0.04 sec)
		Records: 1  Duplicates: 0  Warnings: 0
		
		mysql> SELECT * from Pedidos;
		+----+------------+--------------------+---------+
		| id | cliente_id | produto            | preco   |
		+----+------------+--------------------+---------+
		|  1 |          1 | Geladeira Brastemp | 1800.00 |
		|  2 |          2 | Fogão Consul       |  850.90 |
		|  3 |          3 | Celular Iphone XR  | 3399.00 |
		+----+------------+--------------------+---------+


**D - Selecionar todos os pedidos de clientes com sobrenome Silva**

		mysql> SELECT * FROM Cliente;
		+----+-------+-----------+----------------+
		| id | nome  | sobrenome | email          |
		+----+-------+-----------+----------------+
		|  1 | José  | Silva     | jose@cb.com.br |
		|  2 | João  | Pedro     | joao@pf.com.br |
		|  3 | Pedro | Silva     | pedro@ex.com   |
		+----+-------+-----------+----------------+
		3 rows in set (0.00 sec)
		
		mysql> SELECT * FROM Pedidos;
		+----+------------+--------------------+---------+
		| id | cliente_id | produto            | preco   |
		+----+------------+--------------------+---------+
		|  1 |          1 | Geladeira Brastemp | 1800.00 |
		|  2 |          2 | Fogão Consul       |  850.90 |
		|  3 |          3 | Celular Iphone XR  | 3399.00 |
		+----+------------+--------------------+---------+
		3 rows in set (0.00 sec)
		
		mysql> SELECT cl.nome, cl.sobrenome, pd.produto, pd.preco
			-> FROM Cliente cl, Pedidos pd
			-> WHERE cl.id = pd.cliente_id AND cl.sobrenome = 'Silva';
		+-------+-----------+--------------------+---------+
		| nome  | sobrenome | produto            | preco   |
		+-------+-----------+--------------------+---------+
		| José  | Silva     | Geladeira Brastemp | 1800.00 |
		| Pedro | Silva     | Celular Iphone XR  | 3399.00 |
		+-------+-----------+--------------------+---------+
		2 rows in set (0.00 sec)


**E - Apagar a tabela Pedidos** 

		mysql> DROP TABLE Pedidos;
		Query OK, 0 rows affected (0.14 sec)
    
    

# Links
https://www.tutorialspoint.com/mysql/mysql-using-joins.htm
https://www.mysqltutorial.org/mysql-decimal/


