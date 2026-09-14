# Preparação

O que fiz em seguida foi definir algumas informações importantes e objetivos relacionados à infraestrutura a ser criada.

O objetivo é estabelecer desde já aspectos como região, Availability Zones, endereçamento IP, organização dos recursos, nomenclatura, tags, requisitos de segurança, conectividade e requisitos de funcionamento da workload, de forma que  decisões improvisadas durante a implementação sejam evitadas. 


Assim, ficou definido: 

### Região

A região escolhida foi: ca-central-1 (Canada Central). 

Para a escolha da região, no caso específico deste laboratório, foram avaliadas principalmente questões como o custo estimado dos serviços e a disponibilidade dos recursos necessários para utilização.

Como trata-se de um ambiente de laboratório, requisitos como a latência e residência de dados não são críticos neste cenário. A região escolhida oferece os serviços necessários para a infraestrutura proposta e apresenta custo adequado ao objetivo do laboratório.

### CIDR da VPC

A escolha do CIDR foi 10.0.0.0/16, faixa que fornece espaço suficiente para a infraestrutura atual e permite a criação de novas subnets caso o laboratório seja expandido posteriormente.

### CIDRs das subnets

Visto que o projeto propõe duas AZs, a VPC será subdividida em seis subnets, distribuídas entre duas Availability Zones. Cada camada da arquitetura terá uma subnet correspondente em cada AZ.

O tamanho /27 foi escolhido pela simplicidade de gerenciamento e pela quantidade de endereços disponível para o laboratório. Cada subnet possui 32 endereços IPv4, sendo 27 utilizáveis na AWS. O formato de divisão pode ser visto a seguir:

```text
    VPC
    10.0.0.0/16
    │
    ├── AZ-A
    │   ├── Public Subnet
    │   │   10.0.1.0/27
    │   │
    │   ├── Private App Subnet
    │   │   10.0.11.0/27
    │   │
    │   └── Private DB Subnet
    │       10.0.21.0/27
    │
    └── AZ-B
        ├── Public Subnet
         │   10.0.2.0/27
         │
        ├── Private App Subnet
         │   10.0.12.0/27
         │
        └── Private DB Subnet
            10.0.22.0/27
```

A divisão em subnets tem como objetivo separar componentes com diferentes funções e requisitos de exposição, fornecendo a cada camada certo nível de isolamento e permitindo aplicar políticas de roteamento e controles de tráfego distintos para cada camada.

### AZs

O objetivo a ser alcançado com a distribuição da infraestrutura em AZs diferentes e reduzir a dependência de uma única zona de disponibilidade e permitir uma arquitetura mais resiliente. Diante disso, neste laboratório, após verificar as AZs disponíveis, optou-se pelas AZs: 

ca-central-1
├── ?
└── ?

O que torna a lógica dessa escolha como a exibida a seguir: 

```text
┌───────────────────────────────┐
│            Region             │
│                               │
└───────────────┬───────────────┘
                │
        ┌───────┴───────┐
        ▼               ▼
      AZ-A             AZ-B
        │               │
   3 Subnets        3 Subnets
```

### Nome dos recursos

Mesmo lindando com uma infraestrutura relativamente simples, com o objetivo de facilitar a identificação dos recursos na infraestrutura, será utilizado certo padrão de nomenclatura para identificar recursos. O prefixo cloudlab-opssec identifica o projeto. Após o prefixo, será utilizada uma referência ao recurso e, quando necessário, à sua camada ou Availability Zone.

Exemplos:

cloudlab-opssec-vpc

cloudlab-opssec-public-a

cloudlab-opssec-app-a

cloudlab-opssec-db-a

cloudlab-opssec-public-b

cloudlab-opssec-app-b

cloudlab-opssec-db-b 



