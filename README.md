# ssh

Conexões SSH sem senha fácil

Utilizando chaves públicas e privadas é possível realizar conexões SSH sem o uso de senhas, é possível também a execução remota de comandos, o que facilita alguns processos de automatização. Vale lembrar que esta não é uma configuração totalmente segura, por isto devem ser adotadas medidas alternativas para melhorar a segurança.

Chaves públicas/privadas, o que são e para que servem?


São um par de chaves digitas que juntas fazem uma verificação de autenticidade das informações.

Para maiores informações visite o link:

    http://pt.wikipedia.org/wiki/Criptografia_de_chave_p%C3%BAblica 


O par de chaves é gerado no cliente.

A chave Privada não deve ser divulgada em hipótese nenhuma e a chave Pública pode e deve ser divulgada.

Nota: Este procedimento é voltado inteiramente para conexões SSH (existem outras aplicações de chaves Públicas/Privadas).

Gerando o par de chaves no cliente
No computador cliente execute o seguinte comando:

# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key
(/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in
/root/.ssh/id_rsa.
Your public key has been saved in
/root/.ssh/id_rsa.pub.
The key fingerprint is:
xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx

Quando perguntado para entrar com uma "passphrase", apenas tecle "Enter" duas vezes para que fique em branco, caso contrário esta frase precisará ser digitada posteriormente.

Agora as duas chaves foram geradas.

A chave Privada está em "/root/.ssh/id_rsa" e a Pública em "/root/.ssh/id_rsa.pub". 

Configurando o servidor


Para que seja possível uma conexão SSH sem senha entre cliente e servidor, é preciso divulgar para o servidor a chave pública gerada pelo cliente.

É possível fazer esta cópia remotamente executando os seguintes comandos:

# scp /root/.ssh/id_rsa.pub root@servidor:/root/
(digitar senha)
# ssh root@servidor
(digitar senha)
# cat /root/id_rsa.pub >> /root/.ssh/authorized_keys

No exemplo acima foi realizada a cópia do arquivo "id_rsa.pub" do cliente para o servidor, depois no servidor o conteúdo do arquivo "id_rsa.pub" foi inserido dentro do arquivo "authorized_keys".

Caso o diretório "/root/.ssh" não exista, é preciso criá-lo no servidor.

É importante que a chave pública (id_rsa.pub) seja adicionada (usando o sinal de maior maior ">>") dentro do arquivo "authorized_keys", pois podem haver outros clientes registrados neste mesmo arquivo.

Agora as conexões SSH iniciadas pelo cliente para o servidor serão efetuadas sem a necessidade de digitar a senha. 

Execução remota de comandos


Para executar os comandos remotamente basta executar o seguinte comando:

# ssh root@servidor '/etc/init.d/httpd restart'

Este é apenas um exemplo onde reiniciaria o servidor Web remotamente. Existem muitas possibilidades para a aplicação de execução de comandos remotos.

Execução remota de comandos usando PuTTY
No Windows a ferramenta de conexão SSH mais popular é o PuTTY. Ele disponibiliza diversas formas para conexões a consoles remotos como Telnet, Rlogin e SSH.

Uma das ferramentas "inclusas" é o Plink, que nada mais é que uma interface em linha de comando para as funcionalidades do PuTTY. Usando esta ferramenta e conexões SSH sem senha é possível executar comandos remotos dentro de scripts por exemplo.

O conceito de conexão SSH sem senha continua o mesmo, porém agora é preciso gerar uma chave pública/privada através do PuTTYgen, depois copiar a chave pública para o arquivo "authorized_keys" do servidor.

Nota: Existem formas diferentes de se realizar conexões sem senha usando o PuTTY, aqui mostro apenas uma delas. Para mais detalhes consulte a documentação (http://www.putty.nl/docs.html).

Vamos precisar de três arquivos do PuTTY encontrados neste endereço:

    http://www.putty.nl/download.html 


São eles:

    putty.exe
    plink.exe
    puttygen.exe 


Faça o download deles e salve no cliente.

Agora é preciso criar um par de chaves pública/privada, para isso execute o arquivo "puttygen.exe", depois clique no botão "Generate" e dê uma "brincadinha" com o mouse (essa é a parte que eu mais gosto! ;), pronto as chaves foram geradas. Para salvá-las, clique no botão "Save public key", escolha a pasta e dê um nome ao arquivo, faça o mesmo para a chave privada clicando no botão "Save private key" (manter a "passphrase" em branco).

A chave pública que deverá ser copiada para o servidor (em destaque na figura 1) está na janela principal do PuTTYgen no campo "Public key for pasting into OpenSSH authorized_keys file:", basta copiar este conteúdo e inserir dentro do arquivo "/root/.ssh/authorized_keys" (como nos exemplos anteriores) do servidor. Não entrarei em detalhes de como fazer esta cópia, pois existem muitas. 
