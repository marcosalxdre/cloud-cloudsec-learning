# Implementação do Laboratório Fase 2 (Route Tables/Security Groups/NACL)

Após a implementação da VPC, das sub-redes e do Internet Gateway (IGW), partimos para as configurações de roteamento (Route Tables) e de controle de tráfego (Security Groups e NACLs).

## Criação e Associação das Route Tables

As imagens a seguir demonstram a criação manual da tabela de rotas, especificamente a tabela de rotas pública, e sua posterior associação às sub-redes públicas da nossa infraestrutura.

> **Nota:** Para não poluir esta parte do material com muitas imagens, registrei apenas a criação e associação da tabela de rotas pública. No total, foram criadas três tabelas de rotas: pública, aplicação e banco de dados, cada uma associada às suas respectivas sub-redes.
> >
