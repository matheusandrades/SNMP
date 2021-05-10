# SNMP

Hoje iremos falar do SNMP oq é, e pra que ele serve.
O SNMP(Simple Network Management Protocol) é um protocolo padrão para monitoramento de dispositivos de rede. Ele pode ser usado tanto para fazer consultas quanto para inserir modificações.
Alem disso o SNMP ja faz parte de grandes softwares de monitoramento como ZABBIX, Naggios entre outros.
Nele existe as versões 1 2 e 3, vou citar abaixo a suas diferenças:

##SNMPv1##

 Utiliza o protocolo UDP para transmissão dos dados
 
 Agente escuta a porta 161
 
 Gerente escuta a porta 162 para receber traps
 
 Segurança fraca, baseada em comunidade (community string)
 
 Cada agente possui 3 comunidades: read-only, read-write e trap
 
 Por padrão os equipamentos usam “public” e “private” para community
 

 ##SNMPv2##
 Desenvolvido como uma solução intermediária
 
 Melhorias:
 
 Operação com outros protocolos além de UDP
 
 Suporte a comunicação gerente-gerente
 
 Novo formato de trap
 
 Forma criadas duas versões:
 
 SNMPv2p – baseado em parties (Complexo)
 
 SNMPv2c – baseado em comunidade (Padrão)

 SNMPv2u – baseado em usuários

 

 ##SNMPv3##
 
 Desenvolvido a partir de 1998
 
 Requisitos
 
 Manter compatibilidade com versões anteriores
 
 Resolver limitações das versões anteriores
 
 Arquitetura modular
 
 Manter o SNMP o mais simples possível
 
 USM (User-based Secutity Model)
 
 DES, MD5 e SHA1
 
 VACM (View-based Access Control Model)
 
 Controla quem pode e o que pode acessar
 
 Views – Grupos/Objetos que podem ser acessados

Para que a consulta se torne mais facil o SNMP conta com um dicionario chamado MIB(Management Information Base) em portugues Base de Informação de Gestão, nela contem todas informações que facilitam na hora de realizar uma consulta.
Dentro da MIB nos consenguimos obter os OID e qual é a sua respectiva informações, para simplica vou dar um exemplo abaixo: 

(LEMBRANDO QUE PARA EXECUTAR O COMANDO SNMP PRECISAMOS TER ELE INSTALADO EM TODOS MEUS EXEMPLOS ESTOU USANDO O SNMPv2)

Geralmente quando vamos realizar uma consulta executamos o seguinte comando:

SEM MIB:

snmpwalk -v2c -c COMMUNITY 192.168.0.1 1.2.3.4.5.6.1.7.2011.2.1.2.1234

Há matheus e onde a MIB entra nisso?!!
Vamos la... a MIB praticamente é como se fosse um DNS para o OID, nela nos temos a identificação de cada OID, vamos supor que o OID acima é o trafego de uma determinado interface, como ficaria a consulta tendo a MIB ?

COM MIB:

snmpwalk -v2c -c COMUNNITY 192.168.0.1 ifOutOctets.1234

Logo sabemos que ifOutOctets é todo trafego saindo daquela interface, bem mais facil não?!?

Legal, agora vamos ver como descobrir qual é a interface que esta saindo este trafego, para fazer isso iremos executar o seguinte comando:

snmpwalk -v2c -c COMUNNITY 192.168.0.1 ifName

Vamos obter um resultado semelhante a este:

IF-MIB::ifName.123 = STRING: INTERFACE ETH1

IF-MIB::ifName.1234 = STRING: INTERFACE ETH2

Agora que obtemos os nomes das interfaces daquele determinado host conseguimos verificar qual é o index do oid da interface que estamos monitorando, que no caso deste exemplo é a interface ETH2 onde o index é .1234 .
Pronto agora com o index podemos fazer buscas mais avançadas daquela interface, para facilitar vamos usar o comando grep para filtrar somente as informaçoes desta determinado interface, veja um exemplo abaixo:

snmpwalk -v2c -c COMUNNITY 192.168.0.1 | grep .1234

IF-MIB::ifName.1234 = STRING: INTERFACE ETH2

IF-MIB::ifIndex.1234 = INTEGER: -100663296

IF-MIB::ifMtu.1234 = INTEGER: 2000

IF-MIB::ifSpeed.1234 = Gauge32: 1000000000

IF-MIB::ifPhysAddress.1234 = STRING: 0:0:0:0:0:0

IF-MIB::ifAdminStatus.1234 = INTEGER: up(1)

IF-MIB::ifOperStatus.1234 = INTEGER: up(1)


Agora temos varias informaçoes da nosso interface ETH2, temos a velocidade que ela esta funcionando, o status administrativo e operacional e tambem o MTU..
Apartir dessas informações conseguimos utilizar elas atraves de softwares como Zabbix, Naggios e muitos outros para realizar um monitoramento pro-ativo da nossa infraestrutura. Lembrando que conseguimos usar tambem atraves de scripts como Python,PHP,JavaScript para obter essas informações atraves do SNMP.
Espero que tenham gostado deste conteudo e tenha lhe ajudado, forte abraço.
Se tiver qualquer duvida pode me encaminhar um email para admin@andradesolutions.com.br
