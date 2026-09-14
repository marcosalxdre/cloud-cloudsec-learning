# Preparação

O que fiz em seguida foi definir algumas informações importantes e objetivos relacionados à infraestrutura a ser criada.

O objetivo é estabelecer desde já aspectos como região, Availability Zones, endereçamento IP, organização dos recursos, nomenclatura, tags, requisitos de segurança, conectividade e requisitos de funcionamento da workload, de forma que  decisões improvisadas durante a implementação sejam evitadas. 

Assim, ficou definido: 
&nbsp;
### Região

A região escolhida foi: ca-central-1 (Canada Central). 

Para a escolha da região, no caso específico deste laboratório, foram avaliadas principalmente questões como o custo estimado dos serviços e a disponibilidade dos recursos necessários para utilização.

Como trata-se de um ambiente de laboratório, requisitos como a latência e residência de dados não são críticos neste cenário. A região escolhida oferece os serviços necessários para a infraestrutura proposta e apresenta custo adequado ao objetivo do laboratório.

&nbsp;
### CIDR da VPC

A escolha do CIDR foi 10.0.0.0/16, faixa que fornece espaço suficiente para a infraestrutura atual e permite a criação de novas subnets caso o laboratório seja expandido posteriormente.

&nbsp;
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

&nbsp;
### AZs

O objetivo a ser alcançado com a distribuição da infraestrutura em AZs diferentes e reduzir a dependência de uma única zona de disponibilidade e permitir uma arquitetura mais resiliente. Diante disso, neste laboratório, após verificar as AZs disponíveis, optou-se pelas AZs: 

```text
ca-central-1
├── ?
└── ?
```
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

&nbsp;
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

&nbsp;
### Tags

Com o objetivo de facilitar a organização, identificação e o controle de custos, defini uma padronização para os valores das tags, evitando variações de nomenclatura e mantendo a consistência entre os recursos.

Project     = cloudlab-opssec  
Environment = lab  
Owner       = Marcos  
ManagedBy   = manual  
Component   = network  
Tier = ?

&nbsp;
## Objetivos de segurança

Os objetivos de segurança do laboratório foram definidos com base em boas práticas de segurança em cloud computing e nos princípios de segurança recomendados por provedores de nuvem e organizações especializadas. Alguns destes documentos estão listados no primeiro material deste laboratório, no link: 

O objetivo, é claro, não é reproduzir uma arquitetura de produção completa, mas demonstrar, em um ambiente controlado, princípios fundamentais de segurança aplicados à infraestrutura em nuvem.

Assim, os principais objetivos de segurança a serem implementados nesse laboratório são:  

&nbsp;
### Segmentação da infraestrutura

A infraestrutura será dividida em camadas com diferentes funções e níveis de exposição. Recursos que não necessitam de acesso direto pela Internet serão mantidos em subnets privadas.

A segmentação terá como objetivo reduzir a superfície de ataque e limitar possíveis movimentos laterais em caso de comprometimento de um componente da infraestrutura.

A separação será complementada por controles de roteamento e regras de tráfego entre as diferentes camadas.

&nbsp;
### Minimizar ao máximo exposição à Internet

Os recursos serão privados por padrão, sendo disponibilizados publicamente somente quando houver uma necessidade arquitetural definida.

&nbsp;
### Aplicação do princípio do menor privilégio

O princípio do menor privilégio será aplicado principalmente ao gerenciamento de identidades e permissões, garantindo que cada identidade possua somente as permissões necessárias para executar suas funções.

&nbsp;
### Separação das funções dos componentes

Cada camada da infraestrutura terá uma responsabilidade específica:

**Public**  
↓  
Entrada de tráfego externo

**App**  
↓  
Processamento da aplicação

**DB**  
↓  
Persistência dos dados

O objetivo é facilitar a aplicação de controles de segurança específicos e limitar o impacto de um eventual comprometimento de uma camada.

&nbsp;
### Criação de uma arquitetura minimamente resiliente

A arquitetura será distribuída entre duas Availability Zones para reduzir a dependência de uma única zona e permitir a distribuição de componentes equivalentes entre diferentes zonas.

A utilização de duas AZs neste laboratório tem propósito principalmente demonstrativo, permitindo observar como uma arquitetura multi-AZ pode ser estruturada.

&nbsp;
### Criação de uma identidade administrativa adequada

Antes da criação dos recursos, será definida a forma de autenticação e autorização utilizada para as operações administrativas da conta AWS. O objetivo é separar as atividades administrativas cotidianas da identidade root e utilizar uma identidade apropriada para as operações do laboratório.

Por possuir possuir privilégios extremamente amplos, a conta root não deve ser tratada como a conta administrativa cotidiana, o que é um princípio básico em cibersegurança.

```text
AWS Account
│
├── Root User
│   └── Operações excepcionais que exigem especificamente o root
│
└── IAM Identity
    └── Operações administrativas do laboratório
            │
            ├── AWS Console
            ├── AWS CLI
            ├── SDKs
            └── APIs
```

Isso sera implementado seguindo os dois passos a seguir: 

#### Proteger o root com MFA

A conta root estará protegida com MFA e reservada para situações que realmente exigem essa identidade. Além disso, não será criada access keys para o root.

#### Criar a identidade administrativa para operações 

Simplesmente estabelecer uma identidade diferente do root para administrar o ambiente, tanto pelo console como via CLI, evitando a utilização cotidiana das credenciais do Root User. Preferencialmente, será utilizado o IAM Identity Center para fornecer acesso administrativo por meio de uma identidade humana e de um Permission Set apropriado.

