# LABORATÓRIO SOC 
### Implementei um SOC Home Lab utilizando Wazuh como SIEM/XDR e simulação de ataque de rede com Kali Linux e Hydra. O laboratório permite detecção em tempo real, alertas centralizados, resposta ativa automática a incidentes.

## **Arquitetura do Laboratório**
### Servidor SIEM: Ubuntu rodando Wazuh Manager
### Endpoint Monitorado: Windows 11 com Wazuh Agent
### Máquina Atacante: Kali Linux utilizando Hydra

![Arquitetura](Imagens/arquiteturaSOC.png)

## **Simulação de Ataque – Força Bruta via RDP**
### Para fins educacionais, foi realizada uma simulação de ataque de força bruta via RDP (porta 3389) utilizando o Kali Linux e a ferramenta Hydra, com o seguinte comando:

**hydra -l administrador -P /usr/share/wordlists/fasttrack.txt -t 4 (IP) rdp**

![alt text](<Imagens/hydra.png>)

## **Detecção pelo SIEM Wazuh**
### Durante a simulação, o SIEM Wazuh detectou o ataque em tempo real, através da análise dos logs de eventos de falha de login, gerando alertas associados ao evento 60122, demonstrando a eficácia do monitoramento e correlação de eventos.
![alt text](<Imagens/Wazuh.png>)

## **Resultado do Ataque**
### O Firewall do Windows, por padrão, bloqueia conexões de entrada que não estejam explicitamente autorizadas;

### O serviço de Área de Trabalho Remota (RDP) vem desabilitado por padrão no Windows; (versões recentes)

### Com a porta 3389 fechada, o Hydra recebeu a resposta “Connection Refused” ou " The connection failed to establish.", impedindo o acesso remoto.


## **Automação de Defesa (Active Response)** ⚡
### Foi configurado no Agent Server o Active Response do Wazuh (ossec.conf) para transformar o SIEM em uma defesa ativa. O sistema monitora falhas de login no Windows (evento 60122) e, ao identificar 4 ou mais tentativas falhas do mesmo IP em curto intervalo, executa automaticamente uma regra de firewall via netsh. Como resultado, o IP atacante é bloqueado por 10 minutos, impedindo novas conexões sem necessidade de intervenção humana.

### (Ubuntu agent server)
![alt text](<Imagens/ossec.conf.png>)
### (Testando ataque com hydra novamente é possível ver as tentativas de login param de receber respostas do servidor após 4 tentativas provando que o IP do atacante foi banido em tempo real pelo Firewall do Windows.")
![alt text](<Imagens/kali.png>)


