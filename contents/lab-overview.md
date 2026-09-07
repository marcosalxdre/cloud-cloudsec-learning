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
<br>
Como pode ser visto no diagrama acima, a infraestrutura foi estruturada dentro de uma **AWS Region**, utilizando uma **VPC** distribuída em duas **Availability Zones (AZs)**. A VPC foi dividida em subnets com diferentes níveis de exposição e responsabilidades, separando a camada pública da camada de aplicação e da camada de banco de dados.

<br><br>
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
