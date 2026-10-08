# Implementação do Laboratório Fase 2 (Route Tables/Security Groups/NACLs)

Após a implementação da VPC, das sub-redes e do Internet Gateway (IGW), partimos para as configurações de roteamento (Route Tables) e de controle de tráfego (Security Groups e NACLs).

&nbsp;

## Criação e Associação das Route Tables

As imagens a seguir demonstram a criação manual da tabela de rotas, especificamente a tabela de rotas pública, e sua posterior associação às sub-redes públicas da infraestrutura.

> **Nota:** Para não poluir esta parte do material com muitas imagens, registrei apenas a criação e associação da tabela de rotas pública. No total, foram criadas três tabelas de rotas: pública, aplicação e banco de dados, cada uma associada às suas respectivas sub-redes.
>

&nbsp;

![Criação e Associação Route Tables](../images/rt-1.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-2.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-3.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-4.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-5.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-6.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-7.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-8.png "Criação e Associação Route Tables")

![Criação e Associação Route Tables](../images/rt-9.png "Criação e Associação Route Tables")

&nbsp;

## Criação e Associação dos Security Groups

Em seguida, passei para a implementação dos Security Groups, um dos principais mecanismos de controle de tráfego utilizados na infraestrutura de rede da AWS.

Na infraestrutura proposta para o laboratório, até o momento, temos três grupos de segurança:

* **Security Group do Application Load Balancer (ALB)**

* **Security Group da camada de aplicação**

* **Security Group da camada de banco de dados**

&nbsp;

As imagens a seguir apresentam o processo de criação dos Security Groups e a configuração das respectivas regras de tráfego, incluindo as permissões de entrada e saída definidas para cada componente da arquitetura.

Essas regras foram configuradas utilizando referências aos Security Groups correspondentes, em vez de endereços IP ou sub-redes específicas, permitindo que o controle de acesso seja baseado diretamente na relação entre os componentes da arquitetura.

&nbsp;

![Criação e Configuração dos Security Groups](../images/sg-1.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-2.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-3.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-4.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-5.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-6.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-7.png "Criação e Configuração dos Security Groups")
![Criação e Configuração dos Security Groups](../images/sg-8.png "Criação e Configuração dos Security Groups")

&nbsp;

## Sobre as NACLs

Embora o uso das NACLs tenha sido previsto no planejamento, nesta fase optei por manter a NACL padrão, pois, até o momento, as tabelas de rotas e os Security Groups atendem bem aos controles de tráfego definidos para a arquitetura. Assim, decidi que não há necessidade, neste momento, de adicionar regras restritivas no nível das sub-redes.

A ideia é, em uma etapa futura, utilizar as NACLs em cenários práticos de estudo, como, por exemplo, adicionar uma EC2 com um Security Group mais permissivo à determinada sub-rede e analisar  como uma NACL pode atuar como uma camada adicional de controle de tráfego no nível da sub-rede.

As imagens a seguir apresentam a NACL padrão e suas respectivas regras de tráfego de entrada e saída.

&nbsp;

![NACL Padrão e Regras de Tráfego](../images/nacl-1.png "NACL Padrão e Regras de Tráfego")
![NACL Padrão e Regras de Tráfego](../images/nacl-2.png "NACL Padrão e Regras de Tráfego")









