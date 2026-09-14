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
