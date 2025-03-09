# Sistema de Gerenciamento de Oficina Mecânica

Este repositório contém a modelagem entidade-relacionamento (ER) para um sistema de controle e gerenciamento de ordens de serviço (OS) em uma oficina mecânica. A modelagem foi projetada para atender aos requisitos de um sistema que gerencia clientes, veículos, equipes de mecânicos, serviços, peças e ordens de serviço, permitindo o acompanhamento de revisões e reparos.

## Descrição da Modelagem

O modelo ER foi desenvolvido com base na seguinte narrativa:

- **Clientes** levam **veículos** à oficina para consertos ou revisões periódicas.
- Cada veículo é designado a uma **equipe de mecânicos**, que identifica os **serviços** a serem executados e preenche uma **ordem de serviço (OS)** com data de entrega.
- O valor de cada serviço é calculado consultando uma tabela de referência de mão de obra, e o valor das **peças** também é incluído na OS.
- O cliente autoriza a execução dos serviços, e a mesma equipe avalia e executa os trabalhos.
- Os **mecânicos** possuem código, nome, endereço e especialidade.
- Cada OS possui número, data de emissão, valor, status e data de conclusão.
- Uma OS pode incluir vários serviços, e um mesmo serviço pode estar em várias OS.
- Uma OS pode usar vários tipos de peças, e uma peça pode estar presente em várias OS.

### Estrutura do Modelo ER

#### Entidades e Atributos

1. **Cliente**
   - `idCliente` (PK): Identificador único do cliente.
   - `Nome`: Nome do cliente.
   - `Endereco`: Endereço do cliente.
   - `Telefone`: Contato do cliente.

2. **Veículo**
   - `idVeiculo` (PK): Identificador único do veículo.
   - `Cliente_idCliente` (FK): Referência ao cliente proprietário.
   - `Modelo`: Modelo do veículo.
   - `Placa`: Placa do veículo.
   - `Ano`: Ano de fabricação.

3. **Equipe**
   - `idEquipe` (PK): Identificador único da equipe.
   - `Nome_Equipe`: Nome ou descrição da equipe.

4. **Mecânico**
   - `idMecanico` (PK): Identificador único do mecânico.
   - `Nome`: Nome do mecânico.
   - `Endereco`: Endereço do mecânico.
   - `Especialidade`: Área de especialização do mecânico.
   - `Equipe_idEquipe` (FK): Referência à equipe à qual o mecânico pertence.

5. **Ordem_Servico (OS)**
   - `idOS` (PK): Número único da ordem de serviço.
   - `Veiculo_idVeiculo` (FK): Referência ao veículo associado.
   - `Equipe_idEquipe` (FK): Referência à equipe responsável.
   - `Data_Emissao`: Data de emissão da OS.
   - `Data_Conclusao`: Data prevista ou real de conclusão.
   - `Valor_Total`: Valor total da OS (calculado).
   - `Status`: Status da OS (e.g., Pendente, Em Andamento, Concluído).

6. **Servico**
   - `idServico` (PK): Identificador único do serviço.
   - `Descricao`: Descrição do serviço.
   - `Valor_Mao_Obra`: Valor de referência da mão de obra.

7. **OS_Servico** (Tabela de Associação)
   - `idOS_Servico` (PK): Identificador único do registro.
   - `OS_idOS` (FK): Referência à ordem de serviço.
   - `Servico_idServico` (FK): Referência ao serviço.
   - `Quantidade`: Quantidade de vezes que o serviço foi aplicado.
   - `Valor_Aplicado`: Valor específico aplicado na OS.

8. **Peca**
   - `idPeca` (PK): Identificador único da peça.
   - `Descricao`: Descrição da peça.
   - `Valor_Unitario`: Valor unitário da peça.

9. **OS_Peca** (Tabela de Associação)
   - `idOS_Peca` (PK): Identificador único do registro.
   - `OS_idOS` (FK): Referência à ordem de serviço.
   - `Peca_idPeca` (FK): Referência à peça.
   - `Quantidade`: Quantidade de peças utilizadas.
   - `Valor_Total_Peca`: Valor total das peças (Quantidade * Valor_Unitario).

#### Relacionamentos

- **Cliente - Veículo**: 1:N (um cliente pode ter vários veículos).
- **Veículo - Ordem_Servico**: 1:N (um veículo pode ter várias OS).
- **Equipe - Ordem_Servico**: 1:N (uma equipe pode gerenciar várias OS).
- **Equipe - Mecânico**: 1:N (uma equipe pode ter vários mecânicos).
- **Ordem_Servico - Servico**: N:N (via `OS_Servico`), pois uma OS pode conter vários serviços, e um serviço pode estar em várias OS.
- **Ordem_Servico - Peca**: N:N (via `OS_Peca`), pois uma OS pode usar várias peças, e uma peça pode estar em várias OS.
