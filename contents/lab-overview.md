Nesta parte, meu objetivo é apresentar a construção do laboratório, partindo dos conceitos estudados até sua aplicação prática na infraestrutura. A ideia fundamental é demonstrar como os diferentes conceitos se relacionam para possibilitar a construção de uma infraestrutura funcional, segura, resiliente e escalável.

O desenvolvimento será realizado de forma progressiva, organizando o laboratório em blocos e aumentando sua complexidade à medida que novos conceitos são introduzidos. Para cada etapa, serão apresentados os componentes envolvidos, suas respectivas funções e a forma como se relacionam.

Assim, será construída uma arquitetura inicial que servirá como base para os laboratórios seguintes. A partir dessa estrutura base, cada bloco acrescenta novos elementos, o que permite observar não apenas o funcionamento individual de cada componente, mas também seus impactos sobre a arquitetura como um todo.

O objetivo é estabelecer uma evolução gradual, onde a compreensão teórica sobre determinado conceito seja convertida em decisões arquiteturais e, então, em implementações práticas, criando uma relação contínua entre estudo, construção, experimentação e análise.

## Início (Estudo Teórico Base)

A primeira coisa que fiz foi dedicar um tempo aos estudos para compreender melhor alguns conceitos fundamentais de computação em nuvem, como, por exemplo:

- Region / Availability Zone;
- VPC / VNet;
- CIDR;
- Subnets;
- Private IP / Public IP;
- Route Tables;
- Security Group;
- Network ACL;
- Load Balancers (ALB / NLB);
- Internet Gateway;
- NAT Gateway;
- VPC / VNet Peering;
- VPN Gateway;
- DNS Privado (Route 53 Private Hosted Zones / Azure Private DNS);
- VPC Endpoints;
- Bastion Hosts.

Paralelamente, estudei a aplicação desses conceitos com foco em boas práticas de segurança, tomando como base materiais como os seguintes:

## Diagrama da Infraestrutura Base

Em seguida, estruturei um mapa mental para relacionar esses conceitos e desenhar uma arquitetura inicial simples utilizando o [Draw.io](https://www.drawio.com/).

O resultado é o diagrama da infraestrutura base representado abaixo:

<br>
<p align="center">
  <img src="https://github.com/marcosalxdre/cloud-cloudsec-learning/blob/eb08942d344309906bd203d2e3eaf79ee5c4029d/images/diagrama-baseline.png?raw=true" width="75%">
</p>

&nbsp;

Como pode ser visto no diagrama acima, a infraestrutura foi estruturada dentro de uma **AWS Region**, utilizando uma **VPC** distribuída em duas **Availability Zones (AZs)**. A VPC foi dividida em subnets com diferentes níveis de exposição e responsabilidades, separando a camada pública da camada de aplicação e da camada de banco de dados.

&nbsp;

```text
Region
└── VPC
    ├── AZ-A
    │   ├── Public Subnet A
    │   ├── Private App Subnet A
    │   └── Private DB Subnet A
    │
    └── AZ-B
        ├── Public Subnet B
        ├── Private App Subnet B
        └── Private DB Subnet B
```
&nbsp;

## Region

A **Region** representa uma localização geográfica da infraestrutura da AWS. Cada Region é formada por múltiplas **Availability Zones** fisicamente separadas entre si, permitindo distribuir recursos e reduzir a dependência de uma única zona.

Neste laboratório, a Region funciona como o limite geográfico dentro do qual a infraestrutura será construída.

A escolha da Region deve considerar fatores como custo, latência, disponibilidade dos serviços e requisitos relacionados à localização dos dados.
&nbsp;

## VPC

Dentro da Region é criada a **VPC (Virtual Private Cloud)**, que representa a rede virtual isolada onde os componentes do laboratório serão organizados.

A VPC define, entre outras coisas:

- o espaço de endereçamento IPv4 da rede;
- as subnets disponíveis;
- as regras de roteamento;
- os mecanismos de conectividade;
- os limites de comunicação entre os recursos.
&nbsp;

## Availability Zones

A VPC é distribuída entre duas **Availability Zones**:

```text
             VPC
              │
       ┌──────┴──────┐
       │             │
      AZ-A          AZ-B
```

Uma **Availability Zone** é composta por um ou mais datacenters dentro de uma Region, com infraestrutura projetada para oferecer independência em relação às outras AZs.

A utilização de duas Zonas de Disponibilidade (AZs) permite distribuir os componentes da aplicação entre locais fisicamente separados. Assim, caso uma AZ apresente falhas, a arquitetura pode ser projetada para continuar funcionando por meio dos recursos disponíveis na outra AZ.

> **Importante:** uma subnet pertence a uma única Availability Zone. Por isso, para distribuir a infraestrutura entre duas AZs, criamos subnets separadas em cada uma delas.
&nbsp;

## Subnets

Cada AZ possui três subnets, totalizando seis subnets:

```text
AZ-A
├── Public Subnet A
├── Private App Subnet A
└── Private DB Subnet A

AZ-B
├── Public Subnet B
├── Private App Subnet B
└── Private DB Subnet B
```

A separação não é apenas organizacional. Cada subnet poderá possuir rotas e controles de acesso diferentes, permitindo determinar quais recursos podem se comunicar com a Internet e quais devem permanecer restritos à rede privada.
&nbsp;

## Public Subnets

As **Public Subnets** são destinadas aos componentes que precisam possuir conectividade direta com a Internet por meio da infraestrutura de roteamento da VPC.

```text
              Internet
                  │
                  ▼
       ┌─────────────────────┐
       │                     │
Public Subnet A       Public Subnet B
```

Uma subnet é considerada pública não simplesmente porque contém um recurso público, mas porque sua tabela de rotas possui uma rota que permite alcançar um **Internet Gateway**, o que será configurado posteriormente através das **Route Tables**.
&nbsp;

## Private App e DB Subnets

As subnets privadas de aplicação e de banco de dados abrigam os recursos que processam a aplicação e armazenam suas informações. Como nenhum desses componentes necessita de endereços IP públicos nem de conexão direta com a Internet, estruturar a infraestrutura dessa forma reduz significativamente a superfície de ataque, isolando esses recursos do acesso externo.

Além disso, caso os servidores da aplicação precisem acessar serviços externos, é possível configurá-los por meio de um **NAT Gateway**, sem expor o ambiente diretamente à Internet.

## A separação em três camadas distribuídas em duas AZs junta **segurança e alta disponibilidade** no mesmo projeto.

A divisão em camadas públicas, de aplicação e de banco define quem acessa o quê: a Internet só chega ao ponto de entrada na camada pública, enquanto os servidores e o banco de dados ficam totalmente isolados na rede privada. Já a replicação em duas AZs duplica essa estrutura em locais físicos diferentes, garantindo que o sistema continue no ar se uma das zonas cair.
&nbsp;
&nbsp;

---
Em resumo, a infraestrutura base apresentada estabelece a estrutura de rede sobre a qual os demais componentes do laboratório serão posteriormente implantados.
